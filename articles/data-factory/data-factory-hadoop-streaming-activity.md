<properties 
    pageTitle="Aktivity streamování Hadoop" 
    description="Zjistěte, jak pomocí Hadoop datových proudů aktivity v Azure dat factory spustit přenos Hadoop programy na služba/svůj vlastní clusteru HDInsight." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Aktivity streamování Hadoop
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

Můžete použít aktivity HDInsightStreamingActivity vyvolat Hadoop streamování projektu z Azure Data Factory kanálu. Následující úryvek JSON ukazuje syntaxi pro použití HDInsightStreamingActivity v souboru JSON kanálem k odesílání zpráv. 

HDInsight datových proudů aktivity v Data Factory [kanálem k odesílání zpráv](data-factory-create-pipelines.md) je prováděn Hadoop streamování programy [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) vyžádání/Linux serveru s Windows HDInsight obrázku. Tento článek je založena na článek [aktivity transformace dat](data-factory-data-transformation-activities.md) , který představuje obecné základní informace o transformaci dat a aktivity podporované transformace.

## <a name="json-sample"></a>Ukázka JSON
HDInsight obrázku se automaticky naplní příklad programů (wc.exe a cat.exe) a dat (davinci.txt). Ve výchozím nastavení je název kontejneru, který používá clusteru HDInsight název samotného clusteru. Například pokud názvu clusteru je myhdicluster, název přidružené kontejneru objektů blob bude myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Mějte na paměti následující skutečnosti:

1. Nastavte **linkedServiceName** název propojené služby odkazující na svůj cluster HDInsight, který se spustí streamování úloha mapreduce.
2. Nastavení typu aktivity **HDInsightStreaming**.
3. Vlastnost **mapování** zadejte název mapování spustitelný. V tomto příkladu je cat.exe spustitelný mapování.
4. Vlastnost **reduktorem** zadejte název reduktorem spustitelný. V tomto příkladu je wc.exe spustitelný reduktorem.
5. Pro vlastnost **vstupní** typ zadejte vstupní souboru (včetně umístění) mapování. Příklad: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample je kontejner objektů blob, například/data/Gutenberg je složka, a davinci.txt objektů blob.
6. Pro vlastnost typu **výstupu** zadejte ve výstupním souboru (včetně umístění) reduktorem. Výstup projektu Hadoop přenos zapsán do zadaného umístění pro tuto vlastnost.
7. V části **filePaths** zadejte cesty pro mapování a reduktorem spustitelnými. Příklad: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob, například/aplikace je složka, a wc.exe spustitelný soubor.
8. Vlastnost **fileLinkedService** zadejte představující Azure úložiště, která obsahuje soubory uvedené v části filePaths služby Azure úložiště propojené.
9. Vlastnost **argumenty** zadejte argumenty streamování projektu.
10. Vlastnost **getDebugInfo** je volitelný prvek. Pokud je nastavena na selhání, protokoly stáhnou jenom o chybě. Pokud je nastavena pro všechny, stáhnou se bez ohledu na stav spuštění vždy protokoly.

> [AZURE.NOTE] Jak vidíte v příkladu, zadáte datovou sadu výstup pro Hadoop Streaming aktivity vlastnost **výstup** . Tento datovou sadu je právě formální datovou sadu, která je potřebná k řízení plánu kanálem k odesílání zpráv. Nemusíte zadejte vstupní sadu aktivity **vstupů** vlastnosti.  

    
## <a name="example"></a>Příklad
Kanálu v tomto návodu spustí program slov streamování mapy a zmenšení clusteru Azure HDInsight. 

### <a name="linked-services"></a>Propojené služby

#### <a name="azure-storage-linked-service"></a>Služba Azure úložiště propojené
Nejprve vytvořte propojené služby propojení úložišti Azure používanou Azure HDInsight clusteru factory Azure data. Pokud jste zkopírujte následující kód, nezapomeňte nahraďte název účtu a klíč účtu název a klíč úložišti Azure. 

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
Dále vytvořte propojené služby propojení svůj cluster Azure HDInsight factory Azure data. Pokud jste zkopírujte následující kód, název clusteru HDInsight nahraďte názvem svůj cluster HDInsight a změňte uživatelské jméno a heslo hodnoty. 
    
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
Kanálu v tomto příkladu nevyužívá nějaké vstupy. Zadáte datovou sadu výstup pro přenos aktivity HDInsight. Tento datovou sadu je právě formální datovou sadu, která je potřebná k řízení plánu kanálem k odesílání zpráv. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Kanálem k odesílání zpráv

Kanálu v tomto příkladu obsahuje pouze jeden činnost, která je tohoto typu: **HDInsightStreaming**. 

HDInsight obrázku se automaticky naplní příklad programů (wc.exe a cat.exe) a dat (davinci.txt). Ve výchozím nastavení je název kontejneru, který používá clusteru HDInsight název samotného clusteru. Například pokud názvu clusteru je myhdicluster, název přidružené kontejneru objektů blob bude myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Viz taky
- [Aktivity podregistru](data-factory-hive-activity.md)
- [Prasátko aktivity](data-factory-pig-activity.md)
- [Aktivity MapReduce](data-factory-map-reduce.md)
- [Vyvolání Spark programy](data-factory-spark.md)
- [Spustit skripty R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

