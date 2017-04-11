<properties
 pageTitle="Zpracování dat senzor vozidla s Apache bouře na HDInsight | Microsoft Azure"
 description="Zjistěte, jak zpracuje vozidla senzor data z rozbočovače událostí pomocí Apache bouře na HDInsight. Přidání datového modelu z DocumentDB a ukládání výstupu do úložiště."
 services="hdinsight,documentdb,notification-hubs"
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
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Zpracování vozidla senzor dat z Azure události rozbočovače pomocí Apache bouře na HDInsight

Zjistěte, jak zpracuje vozidla senzor dat z Azure události rozbočovače pomocí Apache bouře na HDInsight. V tomto příkladu přečte senzor dat z Azure události rozbočovače obohacuje data podle odkazující na data uložená v Azure DocumentDB a nakonec ukládání dat do úložiště Azure pomocí soubor systému Hadoop (HDFS).

![HDInsight a diagram architektury Internet věcí (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Základní informace

Přidání senzory vozidla umožňuje předpovídání zařízení problémy podle trendů v datech historických, stejně jako vylepšování budoucí verze na základě vzorku analýzy využití. Zatímco tradiční MapReduce dávkové zpracování lze použít u této analýzy, musíte mít rychle a efektivně načtení dat z všechna vozidla do Hadoop před MapReduce zpracování může dojít. Kromě toho můžete chtít udělat analýza kritické chyby cesty (teploty engine, brakes (brzdy) atd.) v reálném čase.

Azure rozbočovače události se sestavují zpracovávání rozsáhlé objemu dat generovaných senzory a Apache bouře na HDInsight slouží k načtení a zpracování data před uložením do HDFS (podporovaným úložišti Azure) pro další zpracování MapReduce.

##<a name="solution"></a>Řešení

Telemetrickými daty pro teploty engine, teplota a rychlosti zaznamenal senzory odeslaný události rozbočovače spolu s autem vozidla identifikační číslo (VIN) a časové razítko. V ní bouře topologie spuštěna Apache bouře HDInsight clusteru načte data, zpracuje ji a uloží jej do HDFS.

Během zpracování vozidla slouží k načtení modelu informace z Azure DocumentDB. Tato funkce je přidána do datového proudu před uložením.

Komponenty použité v topologii bouře jsou:

* **EventHubSpout** - načte data z Azure rozbočovače události

* **TypeConversionBolt** - převede řetězec JSON z události rozbočovače řazené kolekce členů obsahující jednotlivé datové hodnoty teploty engine, teplota, rychlosti, VIN a časové razítko

* **DataReferencBolt** - vyhledá modelu vozidla z DocumentDB pomocí vozidla

* **WasbStoreBolt** - jsou uložená data, která chcete HDFS (úložišti Azure)

Následující obrázek je příklad některých dalších funkcí tato řešení:

![topologie bouře](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Toto je diagramu zjednodušené a jednotlivé součásti v řešení může mít více instancí. Příklad několika instancí jednotlivé součásti v topologii rozvržena uzlů v bouře clusteru HDInsight.

##<a name="implementation"></a>Implementace

Kompletní automatizované řešení pro tento scénář je k dispozici v rámci [HDInsight bouře příklady](https://github.com/hdinsight/hdinsight-storm-examples) úložiště na GitHub. Pokud chcete použít v tomto příkladu, postupujte podle [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Další kroky

Další příklad bouře topologií najdete v článku [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md).
