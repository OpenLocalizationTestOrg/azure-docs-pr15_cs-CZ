<properties 
    pageTitle="Vyvolání Spark programů Azure Data Factory" 
    description="Naučte se vyvolat Spark programů factory Azure dat pomocí MapReduce aktivity." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Vyvolání Spark programů Data Factory
## <a name="introduction"></a>Úvod
Můžete aktivitu MapReduce v kanálu Data Factory Spark programy spusťte svůj cluster HDInsight Spark. Článek [MapReduce aktivitu](data-factory-map-reduce.md) najdete v článku podrobné informace o použití aktivity před čtením tohoto článku. 

## <a name="spark-sample-on-github"></a>Ukázka Spark na GitHub
[Spark - ukázková Data Factory na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) ukazuje, jak používat MapReduce aktivitu vyvolat Spark programu. Spark program jenom slouží ke kopírování dat z jedné kontejneru objektů Blob Azure do jiného. 

## <a name="data-factory-entities"></a>Entit Factory dat
Složka **Spark-ADF/src/ADFJsons** obsahuje soubory Data Factory entit (propojené služeb, datové sady, kanálem k odesílání zpráv).  

V tomto příkladu jsou dvě **propojené služby** : Azure úložiště a dat Azure HDInsight. Zadejte název Azure úložiště a hodnoty klíče **StorageLinkedService.json** a clusterUri, uživatelské jméno a heslo v **HDInsightLinkedService.json**.

V tomto příkladu jsou dvě **datové sady** : **input.json** a **output.json**. Tyto soubory se nachází ve složce **datové sady** .  Tyto soubory představují vstupní a výstupní datové sady MapReduce aktivity

Ukázka potrubí umístění ve složce **ADFJsons/kanálem k odesílání zpráv** . Prohlédněte si kanálu pochopit, jak vyvolat programu Spark pomocí MapReduce aktivity. 

Aktivity MapReduce nakonfigurovaný tak, aby vyvolat **com.adf.sparklauncher.jar** kontejneru **adflibs** v úložišti Azure (zadané v StorageLinkedService.json). Zdrojový kód pro tento program je v Spark-ADF/src/hlavní/java/com/adf/složky a volá odeslání spark a spuštění Spark úloh. 

> [AZURE.IMPORTANT] 
> Přečíst [Soubor Readme pro. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) nejnovější informace o a další před použitím vzorku. 
>  
> Pomocí cluster HDInsight Spark tento přístup vyvolat Spark programů, které používají MapReduce aktivity. Není podporované použití clusteru HDInsight na vyžádání.   


## <a name="see-also"></a>Viz taky
- [Aktivity podregistru](data-factory-hive-activity.md)
- [Prasátko aktivity](data-factory-pig-activity.md)
- [Aktivity MapReduce](data-factory-map-reduce.md)
- [Aktivity streamování Hadoop](data-factory-hadoop-streaming-activity.md)
- [Spustit skripty R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
