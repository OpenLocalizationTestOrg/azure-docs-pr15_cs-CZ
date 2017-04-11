<properties
   pageTitle="Vývoj na základě Java topologie pro Apache bouře | Microsoft Azure"
   description="Naučte se vytvářet bouře topologií v Java tak, že vytvoříte topologii počet simple Wordu."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Vývoj na základě Java topologie pro aplikace základní slov s Apache bouře a Maven na HDInsight

Naučte se vytvořit na základě Java topologie pro Apache bouře na HDInsight pomocí Maven. Provede procesem vytvoření základní slov aplikace pomocí Maven a Java, kde je definována topologii v Java. Potom se naučíte definovat topologii pomocí rozhraní tok.

> [AZURE.NOTE] Tok framework je dostupná v bouře 0.10.0 nebo novější. Bouře 0.10.0 je dostupný u HDInsight 3.3 a 3.4.

Po dokončení kroků v tomto dokumentu, budete mít základní topologie, která můžete nasadit Apache bouře na HDInsight.

> [AZURE.NOTE] Je k dispozici v [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount)dokončené verze topologií vytvořené v tomto dokumentu.

##<a name="prerequisites"></a>Zjistit předpoklady pro

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java vývojář Kit (JDK) verze 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven je systém sestavení projektu pro projekty Java.

* Textovém editoru například programu Poznámkový blok <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime textu</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Nebo můžete použít integrované vývojové prostředí (integrovaném vývojovém prostředí) například <a href="https://eclipse.org/" target="_blank">zatmění</a> (verze vzhled Luna nebo novější).

    > [AZURE.NOTE] Vaše role redaktora nebo integrovaném vývojovém prostředí pravděpodobně určité funkce pro práci s Maven, který není adresovaný v tomto dokumentu. Informace o možnostech úpravy prostředí najdete v dokumentaci pro produkt, který používáte.

##<a name="configure-environment-variables"></a>Konfigurace proměnné

Následující proměnné možná nastavil, když nainstalujete Java a JDK. Měli byste zkontrolovat, že existují a mohou obsahovat správné hodnoty systému.

* **JAVA_HOME** - by měl přejděte v adresáři, kde je nainstalovaný runtime prostředí Java (JRE). Například v Unix nebo Linux rozdělení, měl by mít podobně jako hodnotu `/usr/lib/jvm/java-7-oracle`. V systému Windows má hodnotu podobně jako`c:\Program Files (x86)\Java\jre1.7`

* **Cesta** – by měl obsahovat následující cesty:

    * **JAVA_HOME** (nebo rovnocenný cesta)

    * **JAVA_HOME\bin** (nebo rovnocenný cesta)

    * V adresáři, kde je nainstalovaný Maven

##<a name="create-a-new-maven-project"></a>Vytvoření nového projektu Maven

Z příkazového řádku použijte následující kód pro vytvoření nového projektu Maven s názvem **WordCount**:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Tím vytvoříte nový adresář s názvem **WordCount** do aktuálního umístění, která obsahuje základní Maven projektu.

Adresář **WordCount** bude obsahovat následující položky:

* **pom.XML**: obsahuje nastavení pro Maven projektu.

* **src\main\java\com\microsoft\example**: obsahuje kód aplikace.

* **src\test\java\com\microsoft\example**: obsahuje testů aplikace. V tomto příkladu jsme nebude vytváření testů.

###<a name="remove-the-example-code"></a>Odebrání příklad

Protože jsme bude vytvářet naše aplikace, odstraňte vygenerovaných test a souborů aplikací:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Přidat vlastnosti

Maven umožňuje definovat hodnoty úrovni projektu s názvem vlastnosti. Přidejte následující po `<url>http://maven.apache.org</url>` řádek:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Můžete teď používáme tyto hodnoty v dalších částech. Třeba při zadávání verze součástí bouře, doporučujeme použít `${storm.version}` místo pevné kódování hodnotu.

##<a name="add-dependencies"></a>Přidání závislostí

Protože je to bouře topologie, musíte přidat závislost bouře součásti. Otevřete soubor **pom.xml** a přidejte následující kód v ** &lt;závislosti >** oddíl:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

V době kompilace Maven použije tyto informace chcete-li vyhledat **bouře core** v úložišti Maven. Nejdřív vypadá v úložišti ve vašem počítači. Nejsou-li soubory tam, bude stáhněte si je z veřejné Maven úložiště a ukládat v místním úložišti.

> [AZURE.NOTE] Upozornění `<scope>provided</scope>` řádek v části jsme přidali. Toto říká Maven vyloučit **bouře základní** z SKLENICE soubory, které jsme vytvořit, protože vám poskytne systému. Díky balíčky vytvoříte trochu menší, a zajistí, že budou využívat bitů **bouře základní** , které jsou součástí bouře clusteru HDInsight.

##<a name="build-configuration"></a>Vytvoření konfigurace

Moduly plug-in maven umožňuje přizpůsobit sestavení stadií projektu, například kompilaci projektu nebo jak zabalit do souboru SKLENICE. Otevřete soubor **pom.xml** a přidejte následující kód přímo nad `</project>` řádku.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

Tato část se použijí k přidání modulů plug-in, zdroje a další možnosti konfigurace Tvůrce dotazů. Úplné odkaz na soubor __pom.xml__ najdete v článku [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Přidat moduly plug-in

Pro bouře topologií <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Spouštění Maven modul plug-in</a> je užitečná, protože umožňuje snadné spuštění topologii místně vývojové prostředí. Přidání podle následujících pokynů `<plugins>` část souboru **pom.xml** zahrnout modul plug-in Maven spouštění:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Všimněte si, že `<mainClass>` položka používá `${storm.topology}`. Jsme neměli definovat to dříve v části Vlastnosti (ale může máme.) Místo toho jsme bude nastavte u této hodnoty z příkazového řádku při používání topologii s vývojové prostředí později.

Jiné užitečné modul plug-in je <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven kompilátoru modul plug-in</a>, které slouží ke změně možnosti kompilace. Primární důvod, Proč musím jsme to je změnit verze Java používající Maven zdrojové a cílové aplikace. Chceme verzi 1.7.

Přidejte následující v `<plugins>` část souboru **pom.xml** zahrnout Apache Maven kompilátoru modul plug-in a nastavit verze zdrojové a cílové 1.7.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Konfigurace zdroje

V části zdroje umožňuje zahrnout zdroje bez použití kódu, jako jsou soubory konfigurace potřeby tak, že komponent topologie. V tomto příkladu přidejte následující text v `<resources>` **pom.xml** souboru.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Tím přidáte adresáře prostředků v kořenovém projektu (`${basedir}`) jako umístění obsahuje zdroje, který obsahuje soubor s názvem __log4j2.xml__. Tento soubor se používá ke konfiguraci, jaké informace zaznamenané v topologii.

##<a name="create-the-topology"></a>Vytvoření topologii

Na základě Java bouře topologie je tvořen tří součástí, které musí vytvořit (nebo odkaz) jako závislostí.

* **Spouts**: načte data z externího zdroje a posílá datové proudy do topologii.

* **Bolts**: provede zpracování datových proudů které spouts nebo další prvky a posílá jeden nebo více datových proudů.

* **Topologie**: Určuje, jak jsou uspořádány spouts a prvky a poskytuje vstupní bod pro topologii.

###<a name="create-the-spout"></a>Vytvoření hubičky

Snížit požadavky pro nastavení externím zdrojům dat, jednoduše následující hubičky posílá náhodné věty. Je upravenou verzi hubičkou dodaný s [Příklady bouře Starter](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [AZURE.NOTE] Příklad hubičkou, který bude číst z externího zdroje dat najdete v následujících příkladech:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): hubičky příklad, který načte z Twitter
>
> * [Bouře Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): hubičkou, který bude číst z Kafka

Pro hubičky vytvoříte nový soubor s názvem **RandomSentenceSpout.java** v adresáři **src\main\java\com\microsoft\example** a použijte následující jako obsah:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Přečetli komentáře kódu pochopit, jak funguje tento hubičky chvíli trvat.

> [AZURE.NOTE] I když tento topologie používá jedinou hubičky, mají ostatní uživatelé mohou několik, které kanálu dat z různých zdrojů na topologii.

###<a name="create-the-bolts"></a>Vytvoření prvky

Prvky úchyt pro zpracování dat. Pro tento topologii máme dva prvky:

* **SplitSentence**: rozdělí vět vypuštění **RandomSentenceSpout** do jednotlivých slov.

* **WordCount**: Spočítá, kolikrát došlo k každého slova.

> [AZURE.NOTE] Prvky dělat doslova cokoliv, například výpočtu trvalé a mluvit externích součástí.

Vytvořte dva nové soubory, **SplitSentence.java** a **WordCount.Java** v adresáři **src\main\java\com\microsoft\example** . Použijte následující jako obsah pro soubory:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Číst pomocí komentářů kód pochopit, jak funguje každý blesku chvíli trvat.

###<a name="define-the-topology"></a>Definování topologii

Topologie spojuje spouts a bolts společně do grafu, který definuje, jak data přetékat mezi složkami. Poskytuje také paralelismus pokynů, které bouře používá při vytváření instancí součástí v rámci clusteru.

Následující obrázek je základní diagram grafu součásti pro tento topologii.

![Diagram znázorňující spouts a prvky uspořádání](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Provádět topologii nejradši s názvem **WordCountTopology.java** v adresáři **src\main\java\com\microsoft\example** . Použijte následující jako obsah souboru:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Přečetli komentáře kódu chcete porozumět tomu, jak je topologii definované a potom odeslat do clusteru chvíli trvat.

###<a name="configure-logging"></a>Konfigurace protokolování

Bouře používá Apache Log4j přihlásit informace. Pokud není konfigurace protokolování, topologii posílat spoustu diagnostické informace, které může být obtížné číst. Pokud chcete určit, co je přihlášení k lyncu, vytvořte do souboru nazvaného __log4j2.xml__ v adresáři __zdroje__ . Pomocí následujících jako obsah souboru.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

To nakonfiguruje nové protokolování pro třídy __com.microsoft.example__ , která obsahuje součásti v tomto příkladu topologii. Úroveň je nastavena na sledování pro tento protokolování, která zachytí všechny protokolované informace, které součástí tohoto topologie. Pokud si prošli zpět kód pro tento projekt, zjistíte, že pouze soubor WordCount.java implementuje protokolování; zaznamená počet každého slova.

`<Root level="error">` Oddíl nakonfiguruje kořenové úroveň protokolování (všechno, co ne v __com.microsoft.example__) pouze informace o chybě.

> [AZURE.IMPORTANT] Během značně sníží informace o přihlášení k lyncu při testování topologii ve vašem prostředí vývoj, neodebere všechny informace o ladění vyrobeno při výpočetnímu clusteru v výroby. Snížit tyto informace, musíte taky nastavíte ladění na hodnotu false v konfiguraci odeslané do clusteru. Kód WordCountTopology.java v tomto dokumentu příklad zobrazit. 

Další informace o konfiguraci protokolování pro Log4j najdete v tématu [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Verze bouře 0.10.0 používá Log4j 2.x. Starší verze bouře použít Log4j 1.x, které používá jiný formát pro konfiguraci protokolu. Další informace o konfiguraci starší najdete v článku [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testování topologii místně

Po uložení souborů pomocí příkazu následující testování topologii místně.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Při spuštění, zobrazí topologii úvodní informace. Potom začne zobrazíte řádky podobná vět jsou vyzařováno hubičky a zpracována prvky.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Pohledem na protokolování, které blesku WordCount jsme můžete zjistíte, že je "a" byla vyzařovaného 113 časy. Počet zůstanou přejít, dokud topologii nespustí, protože hubičky nepřetržitě posílá stejné věty.

Také bude 5 druhý interval mezi emise slov a počtu. K tomu dojde, protože komponentu __WordCount__ nakonfigurovaný tak, aby jenom posílat informace při příchodu řazené kolekce členů popisky a žádosti těchto záznamů jsou pouze doručení každých 5 sekund ve výchozím nastavení.

## <a name="convert-the-topology-to-flux"></a>Převedení topologii tok

Tok je k dispozici u bouře 0.10.0, který umožňuje oddělit konfigurace z implementace nové rámec. Komponenty (prvky a spouts,) jsou přesto podle jazyka Java, ale topologii je definována pomocí souboru YAML.

Soubor YAML definuje součásti pro účely topologii, jak data přetékat mezi nimi a hodnoty používat, když inicializace součásti. Můžete zahrnout YAML soubor jako součást sklenice soubor, ve kterém projektu, můžete ho nasadit nebo externího YAML souboru se dají používat při spuštění topologii.

1. Přesunutí souboru __WordCountTopology.java__ mimo projektu. Dříve to definované topologii, ale nemůžeme nebudete ji používat pro tok.

2. V adresáři __zdrojů__ vytvoření nového souboru s názvem __topology.yaml__. Pomocí následujících jako obsah tohoto souboru.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Čtení a interpretaci co dělá každý oddíl a jak souvisí na základě Java definice v souboru __WordCountTopology.java__ chvíli trvat.

3. Proveďte následující změny souboru __pom.xml__ .

    * Přidejte následující nové závislosti v `<dependencies>` oddíl:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Přidejte následující modul plug-in k `<plugins>` oddíl. Tento modul plug-in zpracovává vytvoření balíčku (sklenice soubor) projektu a platí některé transformace specifické pro tok při vytváření balíček.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
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

    * V __modulu plug-in spouštění maven__ `<configuration>` oddíl, změňte hodnotu pro `<mainClass>` k `org.apache.storm.flux.Flux`. Díky tok případě jsme ho spusťte místně ve vývoji topologii spuštěný.

    * V `<resources>` oddíl, přidejte podle následujících pokynů `<includes>`. Jedná se o YAML soubor, který definuje topologii v rámci projektu.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Otestujte Tok topologie místně

1. Pomocí následujících kompilaci a spouštět topologii tok pomocí Maven.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Pokud používáte Powershellu, použijte následující:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Pokud jste v systému Linux/Unix/OS X a máte [nainstalovaný bouře ve vývojovém prostředí](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), můžete použít následující příkazy místo:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    `--local` Parametr běží topologii místní režim vývojové prostředí. `-R /topology.yaml` Parametr používá `topology.yaml` prostředek souborů ze souboru sklenice definovat topologii.

    Při spuštění, zobrazí topologii úvodní informace. Potom začne zobrazíte řádky podobná vět jsou vyzařováno hubičky a zpracována prvky.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Bude 10 druhý zpoždění mezi listy protokolované informace, jako `topology.yaml` soubor překročí hodnotu `10` vytvoření komponentu WordCount. To nastaví interval zpoždění pro n-tici popisky na 10 sekund.

2.  Zkopírujte `topology.yaml` soubor z projektu. Volání nějak `newtopology.yaml`. V dialogu soubor najděte tento oddíl a změňte hodnotu `10` k `5`. Tím se změní na interval mezi vysílání listy s počtem slov z 10 sekund až 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Spustit topologii, použijte tento příkaz:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Nebo pokud máte bouře na Linux/Unix/OS X vývojové prostředí:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Změna `/path/to/newtopology.yaml` cestu k souboru newtopology.yaml jste vytvořili v předchozím kroku. Tento příkaz použije newtopology.yaml jako definici topologie. Protože jsme neměli zahrnout `compile` parametr, Maven opakovaně používat verzi projekt vytvořený v předchozích krocích.

    Po spuštění topologii by měl zjistíte, že časový interval mezi emitovaného listy změnila tak, aby odrážely v newtopology.yaml hodnotu. Abyste viděli, že můžete změnit konfiguraci prostřednictvím souboru YAML aniž byste museli překompilovat topologii.

Existuje několik dalších funkcí, že tok poskytuje, které nejsou popisované v tomto poli, jako například nahrazování proměnných v souboru YAML parametrů na předaných za běhu a od proměnné. Další informace o těchto a dalších funkcí framework tok najdete v článku [tok (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident je uceleném odběru poskytované bouře. Podporuje stavové zpracování. Má primární Trident tu výhodu, že ho zaručit, že všechny zprávy, které slouží k vložení topologii zpracována jenom jednou. Toto je obtížné dosáhnout nezpracovanými topologie Java, které zaručené společnosti, které zprávy budou zpracovány aspoň jednou. Existují také dalších rozdílů, jako jsou integrované součástí, které lze použít místo abyste vytvářeli prvky. Ve skutečnosti prvky úplně nahrazují méně obecný součásti, například filtry, průzkumy a funkcí.

Trident aplikace můžete vytvořený s využitím Maven projekty. Použít stejnou základní kroky uvedeného dříve v tomto článku – jen kód se liší. Trident taky (aktuálně) nejdou s rámcem tok.

Další informace o Trident najdete v tématu <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Přehled rozhraní API Trident</a>.

Příklad aplikace Trident najdete v článku [Twitter trendu témata s Apache bouře na HDInsight](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Další kroky

Se naučíte vytvořit topologii bouře pomocí jazyka Java. Teď zjistěte, jak:

* [Nasazením a správou Apache bouře topologií na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Můžete vyvíjet C# topologie pro Apache bouře na HDInsight pomocí aplikace Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Najdete další příklad bouře topologií navštivte [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md).
