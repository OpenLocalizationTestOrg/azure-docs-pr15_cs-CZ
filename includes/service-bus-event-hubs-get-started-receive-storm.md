## <a name="receive-messages-with-apache-storm"></a>Zprávy s Apache bouře

[**Apache bouře**](https://storm.incubator.apache.org) je systém distribuované v reálném čase výpočtu, který zjednodušuje spolehlivé zpracování neomezené datové proudy. Tato část popisuje události rozbočovače bouře hubičky používat k přijímání události z rozbočovače události. Pomocí Apache bouře, můžete rozdělit události více procesy hostované v různých uzlů. Události rozbočovače integrace s bouře zjednodušuje události spotřeba transparentně Kontrola průběhu pomocí prvku bouře Zookeeper instalace, Správa trvalý kontrolních bodů a současně přijímání rozbočovače události.

Další informace o události rozbočovače přijímání vzorků najdete v tématu [Přehled rozbočovače události][].

Tento kurz používá [HDInsight bouře][] instalace, která je součástí události rozbočovače hubičky již dostupné.

1. Postup [HDInsight bouře – Začínáme](../articles/hdinsight/hdinsight-storm-overview.md) pro vytvoření nového clusteru HDInsight a připojte k němu prostřednictvím vzdálené plochy.

2. Kopírovat `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` soubor, který chcete místní vývojové prostředí. Tato stránka obsahuje hubičky bouře události.

3. Pomocí následujícího příkazu nainstalovat do místní úložiště Maven. Umožňuje přidat ji jako odkaz v aplikaci project bouře později.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. V zatmění, vytvořte nový projekt Maven (klikněte na **soubor**, pak **Nový**a pak **projekt**).

    ![][12]

5. Vyberte **použít výchozí pracovní prostor umístění**a potom klikněte na tlačítko **Další**

6. Vyberte archetype **maven-archetype-rychlý úvod** a potom klikněte na tlačítko **Další**

7. Vložení **ID skupiny** a **ArtifactId**a potom klikněte na tlačítko **Dokončit**

8. V **pom.xml**, přidejte následující závislosti v `<dependency>` uzel.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Ve složce **src** vytvoření souboru s názvem **Config.properties** a zkopírujte následující obsah zaokrouhlením následující hodnoty:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Hodnota pro **eventhub.receiver.credits** Určuje, kolik události jsou dávce před uvolněním bouře kanálu. Tento příklad za účelem zjednodušení nastaví tuto hodnotu 10. Ve výrobním ho obvykle stanovit vyšší hodnoty; Příklad 1 024.

10. Vytvoření nové třídy s názvem **LoggerBolt** se tento kód:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Tento bouře blesku protokoly obsah přijaté události. Můžete to snadno rozšířit ukládat n-ticové v úložišti služby. [HDInsight senzor analýzy kurz] používá tento stejné přístup k ukládání dat do HBase.

11. Vytvoření třídy s názvem **LogTopology** se tento kód:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Tato třída vytvoří novou událost rozbočovače hubičky pomocí vlastností v konfiguraci zařaďte do instatiate. Je důležité mít na paměti, že v tomto příkladu vytvoří tolik spouts úkoly jako počet oddílů v centru události mohli použít maximální paralelismus povolené na základě této události centrální.

<!-- Links -->
[Přehled rozbočovače událostí]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight bouře]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight senzor analýzy kurz]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png