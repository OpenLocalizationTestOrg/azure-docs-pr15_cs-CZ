<properties 
    pageTitle="Aktivity podregistru" 
    description="Zjistěte, jak pomocí podregistru aktivity v Azure dat factory spouštění dotazů podregistru na služba/svůj vlastní clusteru HDInsight." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Aktivity podregistru
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

HDInsight podregistru aktivity v Data Factory [kanálem k odesílání zpráv](data-factory-create-pipelines.md) provede podregistru dotazy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight/Linux serveru s Windows. Tento článek je založena na článek [aktivity transformace dat](data-factory-data-transformation-activities.md) , který představuje obecné základní informace o transformaci dat a aktivity podporované transformace.

## <a name="syntax"></a>Syntaxe

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Syntaxe podrobnosti

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Jméno | Název aktivity | Ano
Popis | Text s popisem aktivity k čemu slouží | Ne
Typ | HDinsightHive | Ano
zadávání | Vstupů využívané podregistru aktivity | Ne
výstup | Výstupy vytvořené pomocí aktivitu podregistru | Ano 
linkedServiceName | Odkaz na obrázku HDInsight registrovaná jako propojené služby na Data Factory | Ano 
skript | Určení vložené podregistru skriptu | Ne
Cesta skriptu | Uložte skript podregistru do úložišti objektů blob Azure a zadejte cestu k souboru. Vlastnost "skript" nebo "scriptPath". Nejde kombinovat společně. Název souboru je velká a malá písmena. | Ne 
definuje | Zadejte parametry dvojic klíč/hodnota pro odkazování podregistru skriptu pomocí "hiveconf"  | Ne

## <a name="example"></a>Příklad

Zvažte příklad herní protokoly analýzy, ve které chcete určit dobu strávenou uživateli her spuštění ve vaší společnosti. 

Protokol následující je herní protokol vzorku, což je čárkou (`,`) oddělené a obsahuje následující pole – ID profilu SessionStart, doba trvání, SrcIPAddress a GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Podregistru skript** zpracuje tyto údaje:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

K provedení tohoto skriptu podregistru v kanálu Data Factory, budete muset takto

1. Vytvoření propojené služby registraci [vlastní HDInsight výpočet obrázku](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo konfigurace [na vyžádání HDInsight výpočet obrázku](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Pojďme volání propojené služba "HDInsightLinkedService".
2. Vytvořte [propojené služby](data-factory-azure-blob-connector.md) , který nakonfiguruje připojení k úložišti objektů Blob Azure hostingu data. Pojďme volání propojené služba "StorageLinkedService"
3. Vytvoření [datové sady](data-factory-create-datasets.md) přejdete na vstupní a výstupní data. Pojďme volání vstupní datovou sadu "HiveSampleIn" a "HiveSampleOut" datovou sadu výstup
4. Kopírovat podregistru dotazu jako soubor k úložišti objektů Blob Azure konfigurovat kroku #2. Pokud se úložiště pro hostování data jiná než ten, který je hostitelem tento soubor dotazu, vytvořit samostatné služba Azure úložiště propojený a odkažte činnosti. Pomocí **scriptPath **zadejte cestu k souboru dotazu podregistru a **scriptLinkedService** můžete určit Azure úložiště, která obsahuje požadovaný soubor skriptu. 

    > [AZURE.NOTE] Vložené skript podregistru v definici aktivity můžete také zadat pomocí **skriptu** vlastnosti. Nedoporučujeme tento přístup jako speciální znaky v skriptu v rámci dokumentu musí JSON předcházet jsme může způsobit problémy s ladění. Doporučený postup je podle pokynů v kroku #4.
5.  Vytvořte příležitost s HDInsightHive aktivity. Aktivity procesy a transformací data.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Nasazení kanálu. Článek najdete v článku [Vytvoření potrubí](data-factory-create-pipelines.md) podrobnosti. 
7.  Sledujte pomocí zobrazení dat factory monitorování a správu kanálu. V tématu [sledování a správa Data Factory potrubí](data-factory-monitor-manage-pipelines.md) článku podrobnosti. 


## <a name="specifying-parameters-for-a-hive-script"></a>Určení parametrů podregistru skriptu  
V tomto příkladu herní protokolech jsou denně požití do úložiště objektů Blob Azure a jsou uložené ve složce s datem a časem na oddíly. Chcete parametrizovat skript podregistru předat umístění vstupní složky dynamicky za běhu a také vytvořit výstup oddíly s datem a časem.

Pomocí skriptu parametry podregistru, postupujte takto

- Definujte parametry v **definuje**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- V části skript podregistru v nápovědě k parametrů pomocí **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Viz taky
- [Prasátko aktivity](data-factory-pig-activity.md)
- [Aktivity MapReduce](data-factory-map-reduce.md)
- [Aktivity streamování Hadoop](data-factory-hadoop-streaming-activity.md)
- [Vyvolání Spark programy](data-factory-spark.md)
- [Spustit skripty R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









