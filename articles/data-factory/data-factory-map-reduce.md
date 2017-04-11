<properties 
    pageTitle="Spustit Program MapReduce z Azure Data Factory" 
    description="Naučte se zpracování dat spuštěním MapReduce programy Azure HDInsight clusteru z Azure dat factory." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Vyvolání MapReduce programů Data Factory
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

HDInsight MapReduce aktivitu v Data Factory [kanálem k odesílání zpráv](data-factory-create-pipelines.md) je prováděn MapReduce programy [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) vyžádání/Linux serveru s Windows HDInsight obrázku. Tento článek je založena na článek [aktivity transformace dat](data-factory-data-transformation-activities.md) , který představuje obecné základní informace o transformaci dat a aktivity podporované transformace.

## <a name="introduction"></a>Úvod 
Kanálů v Azure dat factory zpracovává data v propojených úložiště služby podnikové propojené výpočetním. Obsahuje řadu aktivity místo, kam jednotlivé aktivity provádí operace konkrétní zpracování. Tento článek popisuje použití aktivity MapReduce HDInsight.
 
Další informace o spouštění skriptů Prasátko/podregistru clusteru/Linux serveru s Windows HDInsight z potrubí pomocí HDInsight Prasátko a podregistru aktivity v tématu [Prasátko](data-factory-pig-activity.md) a [podregistru](data-factory-hive-activity.md) . 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON HDInsight MapReduce aktivity 

V definici JSON HDInsight aktivity: 
 
1. Nastavte **Typ** **aktivitu** na **HDInsight**.
3. Zadejte název třídy vlastnost **název třídy** .
4. Zadejte cestu k souboru SKLENICE včetně názvu souboru **jarFilePath** vlastnosti.
5. Určení propojené služba, která se vztahuje k základnímu úložišti objektů Blob Azure, která obsahuje soubor SKLENICE **jarLinkedService** vlastnosti.   
6. V části **argumenty** určete argumentů MapReduce programu. Za běhu, uvidíte několik dalších argumenty (například: mapreduce.job.tags) v rámci MapReduce. K odlišení argumenty s argumenty MapReduce, zvažte možnost a hodnotu jako argumenty jak je vidět v následujícím příkladu (- ne, – vstupní, – výstup atd., způsoby těsně následovaný jejich hodnoty).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Aktivity MapReduce HDInsight slouží ke spuštění libovolný soubor sklenice MapReduce clusteru HDInsight. Následující ukázkový JSON definici potrubí aktivity HDInsight je nakonfigurován souboru Mahout JAR.

## <a name="sample-on-github"></a>Ukázka na GitHub
Můžete si stáhnout ukázkový pro používání aktivity MapReduce HDInsight z: [Factory údajům na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Spuštění programu slov
Kanálu v tomto příkladu spustí program slov mapy a zmenšení clusteru Azure HDInsight.   

### <a name="linked-services"></a>Propojené služby
Nejprve vytvořte propojené služby propojení úložišti Azure používanou Azure HDInsight clusteru factory Azure data. Pokud jste zkopírujte následující kód, nezapomeňte nahraďte **název účtu** a **klíč účtu** název a klíč úložišti Azure. 

#### <a name="azure-storage-linked-service"></a>Služba Azure úložiště propojené

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight propojené služby
Dále vytvořte propojené služby propojení svůj cluster Azure HDInsight factory Azure data. Pokud jste zkopírujte následující kód, **název clusteru HDInsight** nahraďte názvem svůj cluster HDInsight a změňte uživatelské jméno a heslo hodnoty.   

    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Datové sady

#### <a name="output-dataset"></a>Výstup datové sady
Kanálu v tomto příkladu nevyužívá nějaké vstupy. Zadáte datovou sadu výstup pro aktivity MapReduce HDInsight. Tento datovou sadu je právě formální datovou sadu, která je potřebná k řízení plánu kanálem k odesílání zpráv.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Kanálem k odesílání zpráv
Kanálu v tomto příkladu obsahuje pouze jeden činnost, která je tohoto typu: HDInsightMapReduce. Některé důležité vlastnosti v ve formátu JSON jsou: 

Vlastnost | Poznámky
:-------- | :-----
Typ | Typ musíte nastavit **HDInsightMapReduce**. 
Název třídy | Název třídy: **wordcount**
jarFilePath | Cesta k souboru sklenice obsahující předmětu. Pokud jste zkopírujte následující kód, nezapomeňte si změnit název clusteru. 
jarLinkedService | Azure služba úložiště propojené se souborem sklenice. Tuto službu propojené odkazuje k základnímu úložišti, který bude přidružený clusteru HDInsight. 
argumenty | Wordcount program má dva argumenty, vstup a výstup. Vstupní soubor je soubor davinci.txt.
četnost/interval | Výstup datovou sadu se shodují hodnoty pro tyto vlastnosti. 
linkedServiceName | odkazuje na HDInsight propojené službu, kterou jste dříve vytvořili.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Spuštění Spark programy
Aktivity MapReduce umožňuje Spark programy spusťte svůj cluster HDInsight Spark. Další informace najdete v článku [vyvolat Spark aplikací z Azure Data Factory](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Viz taky
- [Aktivity podregistru](data-factory-hive-activity.md)
- [Prasátko aktivity](data-factory-pig-activity.md)
- [Aktivity streamování Hadoop](data-factory-hadoop-streaming-activity.md)
- [Vyvolání Spark programy](data-factory-spark.md)
- [Spustit skripty R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
