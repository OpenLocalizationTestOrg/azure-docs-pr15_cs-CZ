<properties
    pageTitle="Kurz: Vytváření kanálů pomocí šablony správce prostředků | Microsoft Azure"
    description="V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí Správce prostředků Azure šablony."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Kurz: Vytváření kanálů s aktivitou kopírovat pomocí šablony správce prostředků Azure
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopírování Průvodce](data-factory-copy-data-wizard-tutorial.md)
- [Azure portálu](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure správce prostředků šablony](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [ROZHRANÍ REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [ROZHRANÍ API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tento kurz se dozvíte, jak vytvářet a sledovat factory Azure dat pomocí šablony správce prostředků Azure. Kanálu v factory dat slouží ke kopírování dat z úložiště objektů Blob Azure k databázi SQL Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Projděte si [Přehled a požadavcích](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a postupujte podle pokynů **předpoklad** .
- Postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) a nainstalujte nejnovější verzi Azure PowerShell ve vašem počítači. V tomto kurzu pomocí prostředí PowerShell nasazení Data Factory entity. 
- (volitelné) V tématu [Vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md) na základní informace o šablonách správce prostředků Azure.


## <a name="in-this-tutorial"></a>V tomto kurzu

V tomto kurzu vytvoříte pomocí následujících entit Data Factory factory dat:

Entita | Popis  
------ | ----------- 
Služba Azure úložiště propojené | Odkazy na váš účet Azure úložiště do továrny data. Azure úložiště se obchodu zdroje dat a databáze Azure SQL je úložišti jímky kopírovat aktivity v tomto kurzu. Určuje úložiště účet, který obsahuje zadávání dat aktivity kopírovat. 
Služby Azure SQL databáze propojená| Propojení databáze Azure SQL data factory. Určuje obsahující výstupní data aktivity kopii databáze Azure SQL. 
Azure vstupní datovou sadu objektů Blob | Odkazuje na službu Azure úložiště propojené. Propojené služby odkazuje účet Azure úložiště a datovou sadu objektů Blob Azure určuje kontejneru, složky a název souboru v úložišti obsahující vstupní data. 
Sadu výstup Azure SQL | Odkazuje na službu propojené SQL Azure. Služba SQL Azure propojené odkazuje na serveru Azure SQL a sady dat Azure SQL určuje název tabulky obsahující výstupní data. 
Datového kanálu | Kanálu obsahuje jednu aktivitu typu kopírovat, kterým datovou sadu objektů blob Azure jako vstup a sady dat Azure SQL jako výstup. Kopírovat aktivity slouží ke kopírování dat z Azure objektů blob do tabulky v databázi Azure SQL.  

Factory dat můžete mít minimálně jednu potrubí. Potrubí může obsahovat jedno nebo více aktivitami ho. Existují dva typy činností: [data pohyb aktivity](data-factory-data-movement-activities.md) a [aktivity transformace dat](data-factory-data-transformation-activities.md). V tomto kurzu vytvoříte potrubí s jednu aktivitu (kopírovat aktivity).

![Kopírování objektů Blob Azure k databázi Azure SQL](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Vytvoření souboru JSON s názvem **ADFCopyTutorialARM.json** ve složce **C:\ADFGetStarted** pomocí následující obsah:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parametry JSON 
Vytvoření souboru JSON s názvem **ADFCopyTutorialARM Parameters.json** obsahující parametry správce prostředků Azure šablony. 

> [AZURE.IMPORTANT] Zadejte název a klíč účtu úložiště Azure **storageAccountName** a **storageAccountKey** parametry.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Můžete mít samostatný parametr JSON souborů pro vývoj testování a provozním prostředí, které můžete použít stejné šablonou Data Factory JSON. Pomocí skriptu prostředí PowerShell můžete automatizovat nasazení Data Factory entity v každém z těchto prostředí.  

## <a name="create-data-factory"></a>Vytvoření factory dat
1. Spusťte **Azure PowerShell** a spusťte tento příkaz:
    - Spuštění `Login-AzureRmAccount` a zadejte uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.  
    - Spuštění `Get-AzureRmSubscription` zobrazíte všechna předplatná pro tento účet.
    - Spuštění `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` vyberte předplatné, ke kterým chcete pracovat s. 
2. Spusťte tento příkaz nasazení Data Factory entity pomocí Správce prostředků šablony, že kterou jste vytvořili v kroku 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Sledování kanálem k odesílání zpráv
1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu Azure.
2. **Zdroje dat** v nabídce klikněte na levé (nebo) klikněte na **Další služby** a klikněte na **zdroje dat** v rámci **MĚŘÍTKA + analýzy** kategorie.

    ![Nabídka továrny data](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Na stránce **zdroje dat** Hledat a najít data factory. 

    ![Hledání dat factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klikněte na výrobce Azure data. Vidíte domovské stránce data factory.

    ![Domovská stránka pro factory dat](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klikněte na dlaždici **diagramu** zobrazíte zobrazení diagramu data factory.

    ![Zobrazení diagramu factory dat](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. V zobrazení diagramu poklikejte na datovou sadu **SQLOutputDataset**. Zobrazí stav výseče. Po dokončení operace kopírování stav nastavíte **připravení**.

    ![Výstup výseč připravena](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Když řez **připravena** , ověřte, že data zkopírována **emp** tabulky v databázi Azure SQL.

Pokyny pro sledování kanálem k odesílání zpráv a datových sad pomocí Azure portálu listy, že které jste vytvořili v tomto kurzu najdete v článku [kanálem k odesílání zpráv a sledování datové sady](data-factory-monitor-manage-pipelines.md) .

Můžete také Monitor a spravovat aplikace pro sledování datové kanály. V tématu [sledování a správa Azure Data Factory potrubí sledování aplikací](data-factory-monitor-manage-app.md) podrobné informace o používání aplikace.


## <a name="data-factory-entities-in-the-template"></a>Entit Factory dat v šabloně

### <a name="define-data-factory"></a>Definování factory dat
Definování factory dat v šabloně správce zdrojů, jak je vidět v následujícím příkladu:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName je definován takto: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Je jedinečný řetězec podle ID zdroje skupiny.  

### <a name="defining-data-factory-entities"></a>Definování entity Data Factory
Následující Entity Data Factory jsou definované v šabloně JSON: 

1. [Služba Azure úložiště propojené](#azure-storage-linked-service)
2. [Služba propojené SQL Azure](#azure-sql-database-linked-service)
3. [Datovou sadu objektů blob Azure](#azure-blob-dataset)
4. [Sady dat Azure SQL](#azure-sql-dataset)
5. [Datového kanálu aktivitu kopie](#data-pipeline)

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

ConnectionString používá parametry storageAccountName a storageAccountKey. Hodnoty pro tyto parametrech pomocí konfiguračního souboru. Definice taky používá proměnné: azureStroageLinkedService a dataFactoryName definované v šabloně. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL databáze propojené služby
Zadejte název serveru Azure SQL, název databáze, uživatelské jméno a heslo uživatele v této části. Další informace o vlastnostech JSON slouží k definování služby SQL Azure propojené najdete v článku [Azure SQL propojené služby](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

ConnectionString používá sqlServerName, název databáze, sqlServerUserName a sqlServerPassword parametrech jehož hodnoty jsou pomocí konfiguračního souboru. Definice taky používá následující proměnné ze šablony: azureSqlLinkedServiceName dataFactoryName.

#### <a name="azure-blob-dataset"></a>Datovou sadu objektů blob Azure
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Sady dat Azure SQL
Zadejte název tabulky v databázi Azure SQL, která obsahuje zkopírovaná data z úložiště objektů Blob Azure. Podrobnosti o vlastnostech JSON slouží k definování sady dat Azure SQL najdete v části [Vlastnosti sady dat Azure SQL](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Datového kanálu
Definování kanálu, který slouží ke kopírování dat z datovou sadu objektů blob Azure do sady dat Azure SQL. Popis prvků JSON slouží k definování kanálů v tomto příkladu najdete v článku [JSON kanálem k odesílání zpráv](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Opakované použití šablony 
V tomto kurzu jste vytvořili šablonu pro definování Data Factory entity a šablony pro předávání hodnot parametrů. Kanálu slouží ke kopírování dat z Azure úložiště účtu k databázi Azure SQL zadanými pomocí parametrů. Používat stejnou šablonu pro nasazení Data Factory entity různých prostředích, vytvoříte soubor parametr pro každé prostředí a používá při nasazení prostředí.     

Příklad:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Všimněte si, že prvního příkazu používá soubor parametrů pro vývojové prostředí, druhý pro testovacím prostředí a třetí jeden pro provozním prostředí.  

Můžete taky použít šablonu pro opakované úkoly. Například je potřeba vytvořit mnoho továrny dat s jeden nebo více kanály, které implementace stejné logiky ale každý factory dat používá jiný Azure úložiště a databáze SQL Azure účty. V tomto scénáři používáte stejnou šablonu ve stejné prostředí (odchylka, test nebo výrobní) s jinou parametr soubory k vytvoření zdroje dat.   

