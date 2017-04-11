<properties
    pageTitle="Vytvoření první dat factory (Správce prostředků šablona) | Microsoft Azure"
    description="V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí šablony správce prostředků Azure."
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Kurz: Vytvoření výrobce první Azure dat pomocí šablony správce prostředků Azure
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
- [Azure portálu](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Správce prostředků šablony](data-factory-build-your-first-pipeline-using-arm.md)
- [ROZHRANÍ REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto článku použijete šablonu správce prostředků Azure k vytvoření prvního factory Azure data.

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Prostudujte si článek [Přehled](data-factory-build-your-first-pipeline.md) a postupujte podle pokynů **předpoklad** .
- Postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) a nainstalujte nejnovější verzi Azure PowerShell ve vašem počítači.
- V tématu [Vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md) na základní informace o šablonách správce prostředků Azure. 

## <a name="in-this-tutorial"></a>V tomto kurzu
Entita | Popis  
------ | ----------- 
Služba Azure úložiště propojené | Odkazy na váš účet Azure úložiště do továrny data. Účet Azure úložiště obsahuje vstupní a výstupní data profilace v tomto příkladu. 
HDInsight na vyžádání propojené služby| Odkazy na vyžádání HDInsight clusteru do továrny data. Obrázku se automaticky vytvoří na zpracování dat a odstraní se po dokončení zpracování.
Azure vstupní datovou sadu objektů Blob | Odkazuje na službu Azure úložiště propojené. Propojené služby odkazuje účet Azure úložiště a datovou sadu objektů Blob Azure určuje kontejneru, složky a název souboru v úložišti obsahující vstupní data. 
Výstup datovou sadu objektů Blob Azure | Odkazuje na službu Azure úložiště propojené. Propojené služby odkazuje účet Azure úložiště a datovou sadu objektů Blob Azure určuje kontejneru, složky a název souboru v úložišti obsahující výstupní data. 
Datového kanálu | Kanálu nějaký použitý aktivitu typu HDInsightHive spotřebovává vstupní datovou sadu a vytvoří datovou sadu výstupu.   

Factory dat můžete mít minimálně jednu potrubí. Potrubí může obsahovat jedno nebo více aktivitami ho. Existují dva typy činností: [data pohyb aktivity](data-factory-data-movement-activities.md) a [aktivity transformace dat](data-factory-data-transformation-activities.md). V tomto kurzu vytvoříte potrubí s jednu aktivitu (kopírovat aktivity).

Následující části jsou uvedeny šabloně dokončení správce prostředků pro definování entity Data Factory, aby mohli rychle projít kurzu a otestovat šablonu. Pokud chcete zjistit, jak se jednotlivé Data Factory entity definované, naleznete v části [Data Factory entity v šabloně](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Šablona Factory JSON dat
Nejvyšší úrovně šablonu správce prostředků pro definování factory dat je: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Vytvoření souboru JSON s názvem **ADFTutorialARM.json** ve složce **C:\ADFGetStarted** pomocí následující obsah:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    }
                }
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Jiný příklad použití Správce prostředků šablony můžete najít při vytváření factory Azure dat na [kurz: vytvoření příležitosti s aktivity kopírovat pomocí šablony pro správce prostředků Azure](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parametry JSON 
Vytvoření souboru JSON s názvem **ADFTutorialARM Parameters.json** obsahující parametry správce prostředků Azure šablony.  

> [AZURE.IMPORTANT] Zadejte název a klíč účtu úložiště Azure **storageAccountName** a **storageAccountKey** parametrů v tomto souboru parametr. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Můžete mít samostatný parametr JSON souborů pro vývoj testování a provozním prostředí, které můžete použít stejné šablonou Data Factory JSON. Pomocí skriptu prostředí PowerShell můžete automatizovat nasazení Data Factory entity v každém z těchto prostředí. 

## <a name="create-data-factory"></a>Vytvoření factory dat

1. Spusťte **Azure PowerShell** a spusťte tento příkaz: 
    - Spuštění `Login-AzureRmAccount` a zadejte uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.  
    - Spuštění `Get-AzureRmSubscription` zobrazíte všechna předplatná pro tento účet.
    - Spuštění `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` vyberte předplatné, ke kterým chcete pracovat s. Toto předplatné by měl být stejný jako ten, který jste použili v portálu Azure.
1. Spusťte tento příkaz nasazení Data Factory entity pomocí Správce prostředků šablony, že kterou jste vytvořili v kroku 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Sledování kanálem k odesílání zpráv
 
1.  Po přihlášení k [portálu Azure](https://portal.azure.com/), klikněte na **Procházet** a vyberte **továrny Data**.
        ![Procházet -> zdroje dat](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  V zásuvné **Zdroje dat** klikněte na factory dat (**TutorialFactoryARM**), které jste vytvořili.   
2.  V zásuvné **Data Factory** pro vaše data factory klikněte na **Diagram**.
        ![Dlaždice diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  V **Zobrazení diagramu**najdete v článku Přehled kanály a datové sady použita v tomto kurzu.
    
    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. V zobrazení diagramu poklikejte na datovou sadu **AzureBlobOutput**. Můžete vidět, že řez, které aktuálně zpracovávání.

    ![Datové sady](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Po dokončení zpracování uvidíte výseč **připravena** . Vytvoření obrázku HDInsight na vyžádání zabere někdy (přibližně 20 minut). Proto očekávat potrubí zpracování trvat **zhruba 30 minut** výseče.

    ![Datové sady](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Pokud řez je **připravena** , zkontrolujte **partitioneddata** složku v kontejneru **adfgetstarted** v úložišti objektů blob pro výstupní data.  

Pokyny pro sledování kanálem k odesílání zpráv a datových sad pomocí Azure portálu listy, že které jste vytvořili v tomto kurzu najdete v článku [kanálem k odesílání zpráv a sledování datové sady](data-factory-monitor-manage-pipelines.md) .

Můžete také Monitor a spravovat aplikace pro sledování datové kanály. V tématu [sledování a správa Azure Data Factory potrubí sledování aplikací](data-factory-monitor-manage-app.md) podrobné informace o používání aplikace. 

> [AZURE.IMPORTANT] Vstupní soubor se odstraní, jakmile řez úspěšně zpracovat. Proto pokud chcete znovu spustit řez nebo znovu provést kurzu, vstupní soubor (input.log) na nahrajte inputdata složek kontejneru adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Entit Factory dat v šabloně
### <a name="define-data-factory"></a>Definování factory dat
Definovat factory dat v šabloně správce prostředků, jak je vidět v následujícím příkladu:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName je definován takto: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Jedná se o jedinečný řetězec podle ID zdroje skupiny.  

### <a name="defining-data-factory-entities"></a>Definování entity Data Factory
Následující Entity Data Factory jsou definované v šabloně JSON: 

- [Služba Azure úložiště propojené](#azure-storage-linked-service)
- [HDInsight na vyžádání propojené služby](#hdinsight-on-demand-linked-service)
- [Zadávání datovou sadu objektů blob Azure](#azure-blob-input-dataset)
- [Výstup sadu objektů blob Azure](#azure-blob-output-dataset)
- [Datového kanálu aktivitu kopie](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Služba Azure úložiště propojené
Zadejte název a klíč účtu Azure úložiště v této části. Další informace o vlastnostech JSON slouží k definování služby Azure úložiště propojené najdete v článku [úložišti Azure propojené služby](data-factory-azure-blob-connector.md#azure-storage-linked-service) . 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

**ConnectionString** používá parametry storageAccountName a storageAccountKey. Hodnoty pro tyto parametrech pomocí konfiguračního souboru. Definice taky používá proměnné: azureStroageLinkedService a dataFactoryName definované v šabloně. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight na vyžádání propojené služby
Článek [Výpočet propojené služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) najdete v článku podrobné informace o vlastnostech JSON slouží k definování služby propojený na vyžádání HDInsight.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Mějte na paměti následující skutečnosti: 

- Data Factory vytvoří HDInsight clusteru **serveru s Windows** s nad JSON. Může taky ji máte vytvoření HDInsight clusteru **na základě Linux** . Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Místo použití clusteru HDInsight na vyžádání můžete použít **HDInsight cluster** . Podrobnosti najdete v části [Propojené služby HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight clusteru vytvoří **výchozí kontejner** v úložišti objektů blob, které jste zadali v JSON (**linkedServiceName**). HDInsight nedojde k odstranění tento kontejner při odstranění clusteru. Toto chování se o záměr. Se službou HDInsight propojený na vyžádání HDInsight clusteru je vytvořen pokaždé, když řez musí být zpracovány, pokud je existující live obrázku (**timeToLive**) a odstraní se po dokončení zpracování.

    Jak se zpracovávají další výseče, vidíte mnoho kontejnery v úložišti objektů blob Azure. Pokud pro řešení problémů s projekty je nepotřebujete, můžete odstranit snížit náklady úložiště. Názvy těchto kontejnerů platí vzorek: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Odstranění kontejnery v úložišti objektů blob Azure pomocí nástrojů, jako jsou [Průzkumníka úložiště](http://storageexplorer.com/) .

Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Zadávání datovou sadu objektů blob Azure
Zadejte názvy objektů blob kontejneru, složky a soubor, který obsahuje vstupní data. Další informace o vlastnostech JSON slouží k definování datovou sadu objektů Blob Azure najdete v článku [objektů Blob Azure datovou sadu vlastností](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Tato definice pomocí následujících parametrů v šabloně parametr definovány: blobContainer, inputBlobFolder a inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Výstup datovou sadu objektů Blob Azure
Zadejte názvy objektů blob kontejner a složky, která obsahuje výstupní data. Další informace o vlastnostech JSON slouží k definování datovou sadu objektů Blob Azure najdete v článku [objektů Blob Azure datovou sadu vlastností](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Tato definice pomocí následujících parametrů v šabloně parametr definované: blobContainer a outputBlobFolder. 

#### <a name="data-pipeline"></a>Datového kanálu
Definování kanálů, což transformace dat spuštěním podregistru skriptu clusteru Azure HDInsight na vyžádání. Popis prvků JSON slouží k definování kanálů v tomto příkladu najdete v článku [JSON kanálem k odesílání zpráv](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Opakované použití šablony 
V tomto kurzu jste vytvořili šablonu pro definování Data Factory entity a šablony pro předávání hodnot parametrů. Používat stejnou šablonu pro nasazení Data Factory entity různých prostředích, vytvoříte soubor parametr pro každé prostředí a používá při nasazení prostředí.     

Příklad:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Všimněte si, že prvního příkazu používá soubor parametrů pro vývojové prostředí, druhý pro testovacím prostředí a třetí jeden pro provozním prostředí.  

Můžete taky použít šablonu pro opakované úkoly. Například je potřeba vytvořit mnoho továrny dat s jeden nebo více kanály, které implementace stejné logiky ale každý factory dat používá jiný Azure úložiště a databáze SQL Azure účty. V tomto scénáři používáte stejnou šablonu ve stejné prostředí (odchylka, test nebo výrobní) s jinou parametr soubory k vytvoření zdroje dat. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Správce prostředků šablony pro vytvoření brány
Tady je Ukázková šablona správce prostředků pro vytváření logické brány na pozadí. Instalace brány na místním počítači nebo Azure IaaS OM a bránu zaregistrujte se službou Data Factory pomocí klíče. Další informace najdete v článku [Přesun dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Tato šablona vytvoří factory dat s názvem GatewayUsingArmDF s názvem brány: GatewayUsingARM. 

## <a name="see-also"></a>Viz taky
| Téma | Popis |
| :---- | :---- |
| [Činnost transformace dat](data-factory-data-transformation-activities.md) | Tento článek obsahuje seznam aktivit transformace dat (třeba jste použili v tomto kurzu transformace HDInsight podregistru) nepodporuje Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje plánování a provádění aspekty model aplikace Azure Data Factory. |
| [Potrubí](data-factory-create-pipelines.md) | Tento článek vám pomůže porozumět kanály a aktivity v Azure Data Factory a jak je lze používat k vytvoření začátku do konce postupů založených na datech pro scénář nebo business. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady v Azure Data Factory.
| [Sledování a správa potrubí pomocí sledování aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak sledovat, Správa a ladění potrubí pomocí příkazu sledování a Správa aplikací. 

  

