<properties
    pageTitle="Vývoj Java MapReduce programy pro na základě Linux HDInsight | Microsoft Azure"
    description="Zjistěte, jak k vytvoření Java MapReduce programy a nasadit na základě Linux HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Můžete vyvíjet Java MapReduce programy pro Hadoop na HDInsight Linux

Tento dokument vás provede pomocí Apache Maven vytvoření aplikace MapReduce pak nasadit a spustit na základě Linux Hadoop clusteru HDInsight.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 nebo novější (nebo rovnocenný, například OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Předplatné Azure**

- **Azure rozhraní příkazového řádku**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Konfigurace proměnné

Následující proměnné možná nastavil, když nainstalujete Java a JDK. Měli byste zkontrolovat, že existují a mohou obsahovat správné hodnoty systému.

* **JAVA_HOME** - by měl přejděte v adresáři, kde je nainstalovaný runtime prostředí Java (JRE). Například v systém OS X, Unix nebo Linux, měl by mít podobně jako hodnotu `/usr/lib/jvm/java-7-oracle`. V systému Windows má hodnotu podobně jako`c:\Program Files (x86)\Java\jre1.7`

* **Cesta** – by měl obsahovat následující cesty:

    * **JAVA_HOME** (nebo rovnocenný cesta)

    * **JAVA_HOME\bin** (nebo rovnocenný cesta)

    * V adresáři, kde je nainstalovaný Maven

##<a name="create-a-new-maven-project"></a>Vytvoření nového projektu Maven

1. Z terminálové relace nebo příkazového řádku v prostředí vývoj změňte adresáře do umístění, které chcete uložit tohoto projektu.

3. Příkazem __mvn__ , který se instaluje s Maven, generovat vygenerovaných projektu.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tím vytvoříte nový adresář v adresáři aktuální s názvem určené parametrem __artifactID__ (**wordcountjava** v tomto příkladu.) Tento adresář obsahuje následující položky:

    * __pom.xml__ - [Modelu objektu projektu (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , který obsahuje informace a konfiguraci podrobnosti slouží k vytváření projektu.

    * __src__ - adresář, který obsahuje adresáři __hlavní/java/organizační/apache/hadoop/příklady__ , kde se vytvářet aplikace.

3. Odstranění souboru __src/test/java/org/apache/hadoop/examples/apptest.java__ jako nebude v tomto příkladě použita.

##<a name="add-dependencies"></a>Přidání závislostí

1. Upravte soubor __pom.xml__ a přidejte následující text uvnitř `<dependencies>` oddíl:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Toto říká Maven projekt vyžaduje knihovny (uvedené v rámci &lt;artifactId\>) s určitou verzí (uvedené v rámci &lt;verze\>). V době kompilace to bude možné stáhnout z úložiště Maven výchozí. Chcete-li zobrazit více můžete [Maven úložiště vyhledávání](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) .

    `<scope>provided</scope>` Taky říká Maven, že tyto závislosti neměli součástí aplikací, jak vám poskytne clusteru HDInsight za běhu.

2. Přidejte následující text k souboru __pom.xml__ . Musí být uvnitř `<project>...</project>` značky v souboru. například mezi `</dependencies>` a `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
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
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    První modul plug-in nakonfiguruje [Modul plug-in Maven stín](http://maven.apache.org/plugins/maven-shade-plugin/), které se používá k vytváření uberjar (někdy se jí říká fatjar), který obsahuje závislosti vyžaduje aplikace. Také znemožňuje duplikace licence uvnitř balení sklenice, což může způsobit potíže v některých sítě.

    Druhý modul plug-in nakonfiguruje kompilátoru Maven, který slouží k nastavení verze Java vyžadované tuto aplikaci je verze použitá clusteru HDInsight.

3. Uložte soubor __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Vytvoření aplikace MapReduce

1. Přejděte do adresáře __wordcountjava/src/hlavní/java/organizační/apache/hadoop/příklady__ a přejmenujte soubor __App.java__ __WordCount.java__.

2. Otevřete soubor __WordCount.java__ v textovém editoru a nahraďte obsah takto:

        package org.apache.hadoop.examples;

        import java.io.IOException;
        import java.util.StringTokenizer;
        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.fs.Path;
        import org.apache.hadoop.io.IntWritable;
        import org.apache.hadoop.io.Text;
        import org.apache.hadoop.mapreduce.Job;
        import org.apache.hadoop.mapreduce.Mapper;
        import org.apache.hadoop.mapreduce.Reducer;
        import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
        import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class WordCount {

          public static class TokenizerMapper
               extends Mapper<Object, Text, Text, IntWritable>{

            private final static IntWritable one = new IntWritable(1);
            private Text word = new Text();

            public void map(Object key, Text value, Context context
                            ) throws IOException, InterruptedException {
              StringTokenizer itr = new StringTokenizer(value.toString());
              while (itr.hasMoreTokens()) {
                word.set(itr.nextToken());
                context.write(word, one);
              }
            }
          }

          public static class IntSumReducer
               extends Reducer<Text,IntWritable,Text,IntWritable> {
            private IntWritable result = new IntWritable();

            public void reduce(Text key, Iterable<IntWritable> values,
                               Context context
                               ) throws IOException, InterruptedException {
              int sum = 0;
              for (IntWritable val : values) {
                sum += val.get();
              }
              result.set(sum);
              context.write(key, result);
            }
          }

          public static void main(String[] args) throws Exception {
            Configuration conf = new Configuration();
            String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
            if (otherArgs.length != 2) {
              System.err.println("Usage: wordcount <in> <out>");
              System.exit(2);
            }
            Job job = new Job(conf, "word count");
            job.setJarByClass(WordCount.class);
            job.setMapperClass(TokenizerMapper.class);
            job.setCombinerClass(IntSumReducer.class);
            job.setReducerClass(IntSumReducer.class);
            job.setOutputKeyClass(Text.class);
            job.setOutputValueClass(IntWritable.class);
            FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
            FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
            System.exit(job.waitForCompletion(true) ? 0 : 1);
          }
        }

    Všimněte si název balíčku **org.apache.hadoop.examples** označeným je název třídy **WordCount**. Tyto názvy budou používat, když odešlete MapReduce zakázku.

3. Uložte soubor.

##<a name="build-the-application"></a>Vytvoření aplikace

1. Přejděte do __wordcountjava__ adresáře, pokud jste se už nezobrazují.

2. Umožňuje vytvořit soubor SKLENICE obsahující aplikace tento příkaz:

        mvn clean package

    To bude vyčistit všechny předchozí artefakty Tvůrce dotazů, stáhnout nějaké závislosti, které již byly nenainstalovali a vytvořit balíček aplikace.

3. Po dokončení příkazu bude obsahovat do souboru nazvaného __wordcountjava 1.0 SNAPSHOT.jar__ __wordcountjava/cílový__ adresář.

    > [AZURE.NOTE] Soubor __wordcountjava 1.0 SNAPSHOT.jar__ je uberjar, která obsahuje pouze WordCount úlohy, ale taky závislostí, které úkoly požaduje za běhu.


##<a id="upload"></a>Nahrání sklenice

K nahrání souboru sklenice na HDInsight headnode použijte tento příkaz:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Tím zkopírujete soubory z místního počítače do hlavního uzlu.

> [AZURE.NOTE] Pokud jste použili heslo k zabezpečení účtu SSH, zobrazí se výzva k zadání hesla. Pokud jste použili SSH klíč, bude pravděpodobně nutné použít `-i` parametry a cestu k privátním klíčem. Například `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Spuštění úlohy MapReduce

1. Připojení ke službě HDInsight pomocí SSH popsanou v těchto článcích:

    - [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Relace SSH můžete ke spuštění aplikace MapReduce tento příkaz:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    To umožňuje aplikaci WordCount MapReduce zjistit počet slov v souboru davinci.txt a uložení výsledků __wasbs: / / / Příklad/data/wordcountout__. Soubor vstupní a výstupní jsou uloženy k základnímu úložišti výchozí clusteru.

3. Po dokončení projektu, použijte následující aby se zobrazily výsledky:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Zobrazí seznam slov a počtu, s hodnotami podobně jako tento:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Další kroky

V tomto dokumentu můžete dozvěděli, jak se dají Java MapReduce projektu. Viz následující dokumentů pro další způsoby, jak pracovat s HDInsight.

- [Použití podregistru s HDInsight][hdinsight-use-hive]
- [Použití Prasátko s HDInsight][hdinsight-use-pig]
- [Použití MapReduce s HDInsight](hdinsight-use-mapreduce.md)

Další informace najdete v tématu taky [Středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

