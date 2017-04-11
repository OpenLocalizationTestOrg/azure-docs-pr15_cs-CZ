<properties
 pageTitle="Vývoj napařování MapReduce úlohy s Maven | Microsoft Azure"
 description="Zjistěte, jak používat Maven k vytvoření napařování MapReduce úlohy, a pak nasazení a spuštění úlohy na Hadoop clusteru HDInsight."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Můžete vyvíjet napařování MapReduce úlohy s Apache Hadoop na HDInsight

Napařování je knihovna Scala, díky kterému je snadno vytvářet Hadoop MapReduce úlohy. Nabízí stručné syntaxe, jakož i integraci s Scala.

V tomto dokumentu Naučte se používat Maven k vytvoření základní Wordu počet MapReduce úlohy napsané v Scalding. Potom se informace o nasazení a spuštění této úlohy clusteru HDInsight.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windows nebo Linux pomocí Hadoop clusteru HDInsight**. Další informace najdete v článku [na základě poskytování Linux Hadoop na HDInsight](hdinsight-hadoop-provision-linux-clusters.md) nebo [serveru s Windows by Hadoop na HDInsight](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Platformu Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější**

## <a name="create-and-build-the-project"></a>Vytvoření a vytvoření projektu

1. Umožňuje vytvořit nový projekt Maven tento příkaz:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Tento příkaz vytvoříte nový adresář s názvem **scaldingwordcount**a vytvoříte generování uživatelského rozhraní aplikace Scala.

2. V adresáři **scaldingwordcount** otevřete soubor **pom.xml** a nahraďte obsah takto:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
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
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Tento soubor popisuje projektu, závislosti a moduly plug-in. Tady je důležité položky:

    * **maven.Compiler.Source** a **maven.compiler.target**: Nastaví verze Java pro tento projekt

    * **úložištích**: databází, které obsahují závislost soubory používané tohoto projektu

    * **napařování core_2.11** a **hadoop základní**: Tento projekt vyžaduje ke Scalding a Hadoop balíčků základní

    * **modul plug-in maven scala**: modul plug-in pro kompilaci scala

    * **modul plug-in maven stín**: modul plug-in pro vytvoření stínované sklenic po g (fat). Tento modul plug-in platí filtrů a transformací; specificially:

        * **filtry**: použitými filtry změňte informace metaznaček s klíčovými zahrnutý v souboru sklenice. Nechcete, aby podepisování výjimky za běhu, nevztahuje se na různé podpis soubory, které mohou být součástí závislosti.

        * **spuštění**: Konfigurace spuštění balíčku fáze určuje třídě **com.twitter.scalding.Tool** jako hlavní třída balíčku. Bez toho by bylo nutné zadat com.twitter.scalding.Tool třídy, která obsahuje logiku aplikace při spuštění úlohy pomocí příkazu hadoop.

3. Odstraňte adresáři **src nebo zkoušení** nebude vytváření testů v tomto příkladu.

4. Otevřete soubor **src/main/scala/com/microsoft/example/App.scala** a nahraďte obsah takto:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    To používá úlohy počet základní Wordu.

5. Uložte a zavřete soubory.

6. Zadejte následující příkaz z adresáře **scaldingwordcount** sestavovat a balíček aplikace:

        mvn package

    Po dokončení této úlohy balíček obsahující WordCount aplikaci najdete v **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Spuštění úlohy na základě Linux obrázku

> [AZURE.NOTE] Následující postup používá SSH a příkaz Hadoop. Další metody spuštěním MapReduce úloh najdete v článku [Použití MapReduce v Hadoop na HDInsight](hdinsight-use-mapreduce.md).

1. K nahrání balíčku na svůj cluster HDInsight použijte tento příkaz:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Tím zkopírujete soubory z místního počítače do hlavního uzlu.

    > [AZURE.NOTE] Pokud jste použili heslo k zabezpečení SSH účtu, zobrazí se výzva k zadání hesla. Pokud jste použili SSH klíč, bude pravděpodobně nutné použít `-i` parametry a cestu k privátním klíčem. Například`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Umožňuje připojit k hlavy clusteru tento příkaz:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Pokud jste použili heslo k zabezpečení SSH účtu, zobrazí se výzva k zadání hesla. Pokud jste použili SSH klíč, bude pravděpodobně nutné použít `-i` parametry a cestu k privátním klíčem. Například`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Po připojení k hlavního uzlu, pomocí následujícího příkazu spuštění úlohy počet aplikace word

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Tím spustíte WordCount předmětu, které jste dříve implementovaná. `--hdfs`Nastaví úlohy používat HDFS. `--input`Určuje vstupní textový soubor, zatímco `--output` Určuje umístění výstupu.

4. Až se dokončí úloha, použijte následující zobrazit výstup.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Zobrazí se informace podobná této:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Spuštění úlohy clusteru serveru s Windows

Následující postup používá prostředí Windows PowerShell. Další metody spuštěním MapReduce úloh najdete v článku [Použití MapReduce v Hadoop na HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Spusťte Azure PowerShell a přihlášena k účtu Azure. Po zadání přihlašovacích údajů, příkaz vrátí informace o účtu.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Pokud máte víc předplatných, zadejte id předplatného, které chcete použít pro nasazení.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Můžete použít `Get-AzureRMSubscription` zobrazte seznam všech předplatných přidruženého k vašemu účtu, které zahrnují předplatné Id pro každou z nich.

4. Použijte tento skript nahrávat a úlohu WordCount. Nahrazení `CLUSTERNAME` s názvem vaší HDInsight clusteru a ujistěte se, že `$fileToUpload` je správné cesta k souboru __scaldingwordcount 1.0 SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Když spustíte skript, se výzva k zadání správce uživatelské jméno a heslo pro svůj cluster HDInsight. Všechny chyby, ke kterým dochází je spuštěná úloha se Zaprotokolují do konzoly.
     
6. Po dokončení projektu výstup stahuje do souboru __výstup.txt__ aktuálního adresáře. Pomocí následujícího příkazu zobrazte výsledky.

        cat output.txt

    Soubor musí obsahovat hodnoty podobná této:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili použití Scalding k vytvoření MapReduce úlohy pro HDInsight, použijte následující odkazy, které můžete prozkoumat další způsoby, jak pracovat s Azure HDInsight.

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce úlohy s HDInsight](hdinsight-use-mapreduce.md)
