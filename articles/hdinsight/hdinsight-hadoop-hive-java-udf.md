<properties
pageTitle="Použití funkce Java definované uživatelem (UDF) s podregistru v HDInsight | Microsoft Azure"
description="Naučte se vytvářet a používat funkci Java definované uživatelem (UDF) z podregistru v HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Použití Java UDF s podregistru v HDInsight

Je ideální pro práci s daty v HDInsight podregistru, ale někdy budete potřebovat obecnějším účel jazyk. Podregistru umožňuje vytvářet funkcí definovaných uživatelem (UDF) pomocí různé jazyky. V tomto dokumentu budou zjistěte, jak používat Java UDF z podregistru.

## <a name="requirements"></a>Požadavky

* Předplatné Azure

* HDInsight obrázku (Windows nebo na základě Linux)

    > [AZURE.NOTE] Většina kroků v tomto dokumentu budou pracovat na obou typů clusteru; postup slouží k nahrání zkompilované UDF clusteru a ho spusťte jsou však specifické pro clusterů na základě Linux. Jsou uvedeny odkazy na informace, které se dá používat se systémem Windows clusterů.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 nebo novější (nebo rovnocenný, například OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Do textového editoru nebo integrovaném vývojovém prostředí Java

    > [AZURE.IMPORTANT] Pokud používáte na základě Linux HDInsight server, ale vytváření souborů Python v klientském počítači Windows, je nutné použít v editoru používající LF jako poslední řádek. Pokud si nejste jisti, jestli editor používá LF nebo Line FEED, naleznete v části [Poradce při potížích](#troubleshooting) postup k odebrání znak CR pomocí nástrojů clusteru HDInsight.

## <a name="create-an-example-udf"></a>Vytvoření příklad UDF

1. Z příkazového řádku použijte následující postup pro vytvoření nového projektu Maven:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Pokud používáte Powershellu, musíte vložit text, který hledáte parametry. Například `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Tím vytvoříte nový adresář s názvem __exampleudf__, který bude obsahovat Maven projektu.

2. Po vytvoření projektu odstraňte __exampleudf nebo src/zkoušení__ adresář, který byl vytvořený v rámci projektu. nepoužije se v tomto příkladu.

3. Otevřete __exampleudf/pom.xml__a nahraďte stávající `<dependencies>` položka následující:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Tyto položky zadejte verzi systému Hadoop a podregistru zahrnutý v sadě HDInsight 3.3 3.4 clusterů. Můžete najít informace o verzích Hadoop a podregistru součástí HDInsight z dokumentu [HDInsight součást správy verzí](hdinsight-component-versioning.md) .

    Přidat `<build>` oddílu před `</project>` řádek na konci souboru. V této části by měl obsahovat takto:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Tyto položky definovat k vytváření projektu. Konkrétně verzi Java používající projektu a k vytváření uberjar pro nasazení clusteru.

    Uložte soubor po provedení změn.

4. Přejmenování __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__a otevřete soubor v editoru.

5. Obsah souboru __ExampleUDF.java__ nahraďte následujícím, pak uložte soubor.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    To používá uživatelem definovanou FUNKCI, která přijímá hodnotu řetězce a vrátí malá verze řetězce.

## <a name="build-and-install-the-udf"></a>Vytvoření a nainstalujte UDF

1. Pomocí následujícího příkazu kompilaci a balíček souboru UDF:

        mvn compile package

    To bude vytvářet a potom balíček souboru UDF do __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Použití `scp` příkaz zkopírovat soubor obrázku HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Nahraďte __myuser__ SSH uživatelský účet pro svůj cluster. __Clusteru__ nahraďte názvem obrázku. Pokud jste použili heslo zabezpečení účtu SSH se výzva k zadání hesla. Pokud jste použili certifikát, budete muset používat `-i` parametr určuje souboru privátní klíče.

3. Připojení k clusteru pomocí SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Další informace o použití SSH s Hdinsightu najdete v článku tyto dokumenty.

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Z SSH relace zkopírujte soubor sklenice k základnímu úložišti HDInsight.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Použití UDF z podregistru

1. Pomocí následujících od které začnete klientovi Beeline SSH relace.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Tento příkaz předpokládá, že jste použili výchozí __Správce__ pro přihlašovací účet pro svůj cluster.

2. Jakmile na přejdete `jdbc:hive2://localhost:10001/>` objeví se výzva, zadejte tento příkaz Přidat UDF podregistru a vystavit jako funkce.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Pomocí souboru UDF převod hodnoty načtené z tabulky na samá velká písmena řetězce.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    To bude vyberte zařízení platform (Android, Windows, iOS, atd.) z tabulky převést řetězci na malá písmena a potom je zobrazit. Výstup bude vypadat takto.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Další kroky

Další způsoby práce s podregistru najdete v článku [Použití podregistru s HDInsight](hdinsight-use-hive.md).

Další informace o funkcích Hive User-Defined, přečtěte si téma [podregistru operátory a funkce definované uživatelem](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) část podregistru wikiwebu na apache.org.
