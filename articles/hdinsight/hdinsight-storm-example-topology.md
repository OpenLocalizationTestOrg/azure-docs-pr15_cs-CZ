<properties
 pageTitle="Příklad Apache bouře topologií na HDInsight | Microsoft Azure"
 description="Seznam příklad bouře topologií vytvořili a testováno s Apache bouře na HDInsight včetně základní C# a Java topologií a pracovat s ním rozbočovače události."
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
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Příklad bouře toplogies a součásti pro Apache bouře na HDInsight

Následuje seznam příkladů vytvářejí a spravují společnost Microsoft pro použití s Apache bouře na HDInsight. Tyto příklady vysvětlovat různých tématech vytvářet základní C# a Java topologie pro práci s Azure služeb, jako je událost rozbočovače DocumentDB, Power BI, databáze SQL, HBase na HDInsight a úložišti Azure. Několik příkladů také ukazují, jak pracovat s technologiemi, není Azure nebo dokonce Microsoft, třeba SignalR a Socket.IO

| Popis                                                                                             | Ukazuje                                         | Jazyk/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Zapisovat do úložiště jezera dat Azure Apache bouře](hdinsight-storm-write-data-lake-store.md) | Psaní do úložiště jezera dat Azure | Java |
| [Spout centrální událostí a blesku zdroje](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Zdroje pro událost centrální hubičky a šroub | Java |
| [Vývoj na základě Java topologie pro Apache bouře na HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Můžete vyvíjet C# topologie pro Apache bouře na HDInsight pomocí aplikace Visual Studio][16fce2d1]                     | HDInsight Tools for Visual Studio                    | C# Java                   |
| [Vytvoření více datových proudů v jazyce C# bouře topologii][ec5a4064]                                         | Více datových proudů                                     | C#                         |
| [Určení Twitter trendu témata s bouře na HDInsight][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Proces události z Azure události rozbočovače s bouře na HDInsight (C#)][844d1d81]                                | Rozbočovače události                                           | C# a Java                |
| [Proces události z Azure události rozbočovače s bouře na HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Rozbočovače události | Java |
| [Vizualizace dat z topologie bouře pomocí Power Bi][94d15238]                              | Power BI                                             | C#                         |
| [Analýza dat senzor s bouře a HBase v HDInsight][ab894747]                                     | Událost rozbočovače, HBase, Socket.IO Web řídicího panelu          | C# Java, JavaScript, HTML |
| [Zpracování vozidla senzor dat z rozbočovače událostí pomocí bouře na HDInsight][246ee964]                        | Událost rozbočovače, DocumentDb, Azure úložiště objektů Blob (WASB)    | C# Java                   |
| [Extrahovat, transformace a načíst (ETL) z Azure události rozbočovače k HBase pomocí bouře na HDInsight][b4b68194] | Událost rozbočovače HBase                                    | C#                         |
| [Šablona C# bouře topologie projektu pro práci s Azure společnosti bouře na HDInsight][ce0c02a2]  | Událost rozbočovače, DocumentDb, SQL databáze, HBase, SignalR | C# Java                   |
| [Škálovatelnost ukazatele pro čtení z rozbočovače události Azure HDInsight pomocí bouře][d6c540e3]           | Výkon zprávy, události rozbočovače databáze SQL         | C# Java                   |
| [Sladit událostí použití bouře a HBase na HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Použití Python s bouře na HDInsight](hdinsight-storm-develop-python-topology.md) | Komponenty Python s Java a Clojure založeny bouře topologií | Python |

## <a name="next-steps"></a>Další kroky

* [Začínáme s Apache bouře na HDInsight][2b8c3488]

* [Zjistěte, jak nasazením a správou bouře topologií s bouře na HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Zjistěte, jak bouře HDInsight clusteru Kam zmizely řídicího panelu bouře nasazení topologií příklad."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Naučte se nasazovat a spravovat pomocí řídicí panel bouře založené na webu a bouře uživatelské rozhraní nebo nástroje HDInsight for Visual Studio topologií."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Naučte se vytvářet C# bouře topologií pomocí nástroje HDInsight for Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Naučte se vytvářet bouře topologií v Java pomocí Maven, tak, že vytvoříte topologii základní wordcount."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Ukazuje, jak k zápisu dat Power BI z topologie C# a pak vytvoření řídicího panelu a od data."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Ukazuje základní topologie bouře, která provádí wordcount implementovaná v jazyce C#. Tento taky příklad ukazuje, jak vytvořit více datových proudů v rámci topologie C#."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Informace o čtení a zápis dat z Azure události rozbočovače s bouře na HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Naučte se používat Apache bouře na HDInsight zpracování senzor dat z Azure události rozbočovače, vizualizovat pomocí D3.js a (volitelně), uložte ho HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Naučte se používat Trident vytvořit topologie bouře, která určuje trendu témata (podle hashtags,) na Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Naučte se používat topologie bouře čtení zpráv z Azure události rozbočovače číst dokumenty z Azure DocumentDB pro odkazující na data a uložení dat k základnímu úložišti Azure."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Několik topologií prokázat výkon při čtení z Azure události rozbočovače a ukládání k SQL databázi pomocí Apache bouře na HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Zjistěte, jak k načtení dat z Azure události rozbočovače souhrnný a transformace dat a pak uložte ho do HBase na HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Tento projekt obsahuje šablony pro spouts, prvky a topologie chcete provést interakci s různými Azure služby, jako třeba rozbočovače události, DocumentDB a databáze SQL."
 
