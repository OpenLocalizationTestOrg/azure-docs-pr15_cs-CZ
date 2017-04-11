<properties 
    pageTitle="Prasátko aktivity" 
    description="Zjistěte, jak pomocí aktivity Prasátko v Azure dat factory spouštěly skripty Prasátko na služba/svůj vlastní clusteru HDInsight." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Prasátko aktivity
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

Prasátko HDInsight aktivity v Data Factory [kanálem k odesílání zpráv](data-factory-create-pipelines.md) provede Prasátko dotazy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) vyžádání/Linux serveru s Windows HDInsight obrázku. Tento článek je založena na článek [aktivity transformace dat](data-factory-data-transformation-activities.md) , který představuje obecné základní informace o transformaci dat a aktivity podporované transformace.

## <a name="syntax"></a>Syntaxe

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Syntaxe podrobnosti

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Jméno | Název aktivity | Ano
Popis | Text s popisem aktivity k čemu slouží | Ne
Typ | HDinsightPig | Ano
zadávání | Nejméně jedna vstupů využívané Prasátko aktivity | Ne
výstup | Nejméně jedna výstupy vytvořené pomocí Prasátko aktivity | Ano
linkedServiceName | Odkaz na obrázku HDInsight registrovaná jako propojené služby na Data Factory | Ano
skript | Určení vložené Prasátko skriptu | Ne
Cesta skriptu | Ukládání skript Prasátko v úložišti objektů blob Azure a zadejte cestu k souboru. Vlastnost "skript" nebo "scriptPath". Nejde kombinovat společně. Název souboru je velká a malá písmena. | Ne
definuje | Zadejte parametry dvojic klíč/hodnota pro odkazování Prasátko skriptu | Ne

## <a name="example"></a>Příklad

Zvažte příklad herní protokoly analýzy, ve které chcete určit času stráveného přehrávače her spuštění ve vaší společnosti.
 
Následující ukázkový herní protokol je soubor oddělené středníkem (;). Obsahuje následující pole – ID profilu SessionStart, doba trvání, SrcIPAddress a GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Prasátko skript** zpracuje tyto údaje:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

K provedení tohoto skriptu Prasátko v kanálu Data Factory, postupujte takto:

1. Vytvoření propojené služby registraci [vlastní HDInsight výpočet obrázku](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo konfigurace [na vyžádání HDInsight výpočet obrázku](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Pojďme volání propojené služba **HDInsightLinkedService**.
2.  Vytvořte [propojené služby](data-factory-azure-blob-connector.md) , který nakonfiguruje připojení k úložišti objektů Blob Azure hostingu data. Pojďme volání propojené služba **StorageLinkedService**.
3.  Vytvoření [datové sady](data-factory-create-datasets.md) přejdete na vstupní a výstupní data. Pojďme volání vstupní datovou sadu **PigSampleIn** a datovou sadu výstup **PigSampleOut**.
4.  Zkopírujte tento dotaz Prasátko v souboru úložišti objektů Blob Azure nakonfigurovali v kroku #2. Pokud Azure úložiště, který je hostitelem data se liší od té, která hostuje soubor dotazu, vytvořte samostatné služba Azure úložiště propojené. V nápovědě ke službě propojené v konfiguraci aktivity. Umožňuje zadat cestu k souboru skriptu Prasátko a **scriptLinkedService** **scriptPath **. 
    
    > [AZURE.NOTE] Vložené skript Prasátko v definici aktivity můžete také zadat pomocí **skriptu** vlastnosti. Však nedoporučujeme tento přístup jako speciální znaky v skript třeba předcházet a může způsobit problémy s ladění. Doporučený postup je podle pokynů v kroku #4.
5. Vytvoření kanálu s HDInsightPig aktivity. Tato akce zpracuje zadávání dat spuštěním Prasátko skriptu clusteru HDInsight.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Nasazení kanálu. Článek najdete v článku [Vytvoření potrubí](data-factory-create-pipelines.md) podrobnosti. 
7. Sledujte pomocí zobrazení dat factory monitorování a správu kanálu. V tématu [sledování a správa Data Factory potrubí](data-factory-monitor-manage-pipelines.md) článku podrobnosti.

## <a name="specifying-parameters-for-a-pig-script"></a>Určení parametrů Prasátko skriptu 

Příklad: herní protokolech jsou požití denně do úložiště objektů Blob Azure a uložené ve složce oddílů podle data a času. Chcete parametrizovat skript Prasátko předat umístění vstupní složky dynamicky za běhu a také vytvořit výstup oddíly obsahující datum a čas.
 
Použití parametry Prasátko skript, postupujte takto:

- Definujte parametry v **definuje**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- V části skript Prasátko označovat parametrů pomocí "**$parameterName**", jak je ukázáno v následujícím příkladu:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Viz taky
- [Aktivity podregistru](data-factory-hive-activity.md)
- [Aktivity MapReduce](data-factory-map-reduce.md)
- [Aktivity streamování Hadoop](data-factory-hadoop-streaming-activity.md)
- [Vyvolání Spark programy](data-factory-spark.md)
- [Spustit skripty R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


