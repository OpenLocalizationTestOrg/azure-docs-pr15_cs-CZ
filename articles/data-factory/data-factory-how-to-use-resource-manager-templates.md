<properties 
    pageTitle="Správce prostředků použití šablony v Data Factory | Microsoft Azure" 
    description="Informace o vytváření a používání šablon správce prostředků Azure k vytváření entit Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Pomocí šablon můžete vytvořit entity Azure Data Factory

## <a name="overview"></a>Základní informace
Při používání Azure Data Factory pro vaše potřeby integrace dat můžete narazit na sami opětovné použití stejné vzorek v různých prostředích nebo provádění stejný úkol opakovaně ve stejném řešení. Šablony umožňují implementaci a správa podobnému sledu snadno způsobem. Šablony v Azure Data Factory jsou ideální pro scénáře, které se týkají opětovné použití a opakování.
 
Zvažte situaci, kde v organizaci 10 výrobní závody po celém světě. Protokoly z každého zařízení jsou uloženy v samostatném místní databázi SQL serveru. Chce vytvořit jeden datový sklad v cloudu pro ad-hoc analýzy. Také chce mít stejnou logiku ale různé konfigurace pro vývoj, test a výrobní prostředí. 

V tomto případě musí úkol opakovat v rámci stejné prostředí, ale s různými hodnotami přes továrny 10 dat pro každou výrobního podniku. **Opakování** vlastně prezentovat. Ukázka umožňuje odběru tohoto obecného tok (to znamená potrubí s stejnou činnost v jednotlivých dat závodech), ale používá samostatných parametr souboru pro každý výrobního podniku.

Kromě toho organizace chce nasazení tyto 10 dat továrny tisknutím v různých prostředích, šablony můžete využít tento **opětovné použití** využitím samostatné parametr souborů pro vývoj, test a výrobní prostředí.

## <a name="templating-with-azure-resource-manager"></a>Ukázka s Azure správce prostředků
[Správce prostředků Azure šablony](../azure-resource-manager/resource-group-overview.md#template-deployment) jsou skvělý způsob, jak dosáhnout ukázka v Azure Data Factory. Správce prostředků šablony definovat infrastrukturu a konfiguraci vašeho Azure řešení prostřednictvím souboru JSON. Protože správce prostředků Azure šablony pracovat s všechny/většina Azure služeb, ho široce lze snadno spravovat všechny zdroje Azure prostředky. V tématu [vytváření správce prostředků Azure šablon](../resource-group-authoring-templates.md) zobrazíte další informace o šablonách správce prostředků obecně. 

## <a name="tutorials"></a>Výukové programy
Viz následující kurzy podrobné pokyny k vytváření Data Factory entit pomocí Správce prostředků šablon:

- [Kurz: Vytváření kanálů ke kopírování dat pomocí šablony správce prostředků Azure](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Kurz: Vytvoření kanálu proces dat pomocí šablony správce prostředků Azure](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Šablony Factory dat na Github
Podívejte se na následující šablony Azure úvodní na Github: 

- [Vytvoření Data factory ke kopírování dat z úložiště objektů Blob Azure k databázi SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Vytvoření Data factory s aktivitou podregistru clusteru Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Vytvoření Data factory ke kopírování dat ze služby Salesforce objektů BLOB Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Neváhejte sdílení šablon Azure Data Factory v [Azure rychlý start](https://azure.microsoft.com/documentation/templates/). Při vývoji šablony, které mohou být sdíleny pomocí tohoto úložiště v nápovědě k [příspěvku Průvodce](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) . 

Následující části obsahují podrobné informace o definování zdroje dat Factory v šabloně správce prostředků. 

## <a name="defining-data-factory-resources-in-templates"></a>Definování zdroje dat Factory do šablony
Nejvyšší úrovně šablonu pro definování factory dat je:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Definování factory dat

Definovat factory dat v šabloně správce prostředků, jak je vidět v následujícím příkladu:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

DataFactoryName je definována v "proměnné" takto:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Definování propojené služeb 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Další informace o vlastnostech JSON konkrétní propojené službu, kterou chcete nasadit najdete v článku [Propojené služba úložiště](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [Výpočet propojené služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . "DependsOn" parametr určuje název odpovídající data factory. Příklad definování propojené služby Azure úložištěm se zobrazují v definici JSON následující:

### <a name="define-datasets"></a>Definice datové sady

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Podívejte se do [podporované úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) podrobné informace o vlastnostech JSON typu konkrétní datovou sadu, kterou chcete nasadit. Poznámka: "dependsOn" parametr určuje název odpovídající data factory a úložiště propojená služby. Příklad definování datovou sadu typu úložišti objektů blob Azure se zobrazují v definici JSON následující:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definování potrubí

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

V nápovědě k [Definování potrubí](data-factory-create-pipelines.md#pipeline-json) podrobné informace o vlastnostech JSON definování konkrétní kanálem k odesílání zpráv a aktivity, kterou chcete nasadit. Poznámka: "dependsOn" parametr určuje název factory dat a všechny odpovídající propojený služby nebo datové sady. V následující ukázce JSON je znázorněn příklad kanálu, který slouží ke kopírování dat z úložiště objektů Blob Azure k databázi SQL Azure:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parametrizování Data Factory šablony
Doporučené postupy na parametrizování, najdete v článku [Doporučené postupy pro vytváření šablon správce prostředků Azure](../resource-manager-template-best-practices.md#parameters) . Obecně použití parametru by měl být minimalizován, zejména pokud proměnné můžete místo toho použít. Jenom poskytnout parametrů v následujících situacích:

- Nastavení se liší podle prostředí (Příklad: vývoj, test a výrobní)
- Tajemství (například hesla)

Pokud potřebujete tajemství spotřebovávat [Azure klíč trezoru](../key-vault/key-vault-get-started.md) při nasazení Azure Data Factory entit pomocí šablony, zadejte **klíčové trezoru** a **tajné název** jak je vidět v následujícím příkladu:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Export šablon pro stávající data továrny není aktuálně podporován, je ale v programu works. 


