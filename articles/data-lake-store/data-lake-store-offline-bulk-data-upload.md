<properties
   pageTitle="Odesílání velkých objemů dat do úložiště jezera dat pomocí offline metod | Microsoft Azure"
   description="Nástroj pro správu AdlCopy ke kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Import a Export službu Azure offline kopie dat za účelem jezera úložiště dat

V tomto článku se dozvíte o tom, jak zkopírovat velkých datových sad (> 200GB) do úložiště jezera dat Azure pomocí metody offline kopie jako [Služby Azure Import nebo Export](../storage/storage-import-export-service.md). Konkrétně soubor používá jako příklad v tomto článku je 339,420,860,416 bajtů znamená přibližně 319GB na disku. Pojďme volání 319GB.tsv tento soubor.

Služba Azure Import nebo Export umožňuje bezpečně přenášet velké objemy dat k úložišti objektů blob Azure odesíláním pevných discích Azure datacentrem.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Účet azure úložiště**.

- **Účet azure dat jezera analýzy (volitelné)** – najdete v článku [Začínáme s jezera analýzy dat Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Další informace o tom, jak vytvořit účet jezera úložiště.


## <a name="preparing-the-data"></a>Příprava dat

Před použitím služby Import nebo Export jsme musí přerušit na datový soubor jako přenesené **do kopií, které jsou menší než 200 GB** velikost. Důvodem je nástroj pro import nelze použít u souborů, vyšší než 200GB. Ke splnění tohoto v tomto kurzu budeme rozdělit soubor bloky 100GB bajtů. Lze provést jednoduše pomocí [softwaru Cygwin](https://cygwin.com/install.html). Softwaru Cygwin podporuje Linux příkaz, který umožňuje snadno. Pro našem případě používáme následující příkaz.

    split -b 100m 319GB.tsv

Operace rozdělit vytvoří soubory s názvy dole.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Příprava disků s daty

Postupujte podle pokynů u [Pomocí služby Azure Import a Export](../storage/storage-import-export-service.md) (v části **připravit vašich jednotkách** ) příprava pevných discích. Tady je celkový tok o tom, jak připravit jednotku.

1. Získávají pevného disku, který splňuje požadavky pro použití služby Auzre Import nebo Export.

2. Určení účet Azure úložiště kde data budou zkopírováním jednou si vyřízeny Azure datacentrem.

3. Použijte [Nástroj pro Import nebo Export Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), nástroj příkazového řádku. Tady je ukázka fragment o tom, jak používat nástroj.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Další ukázkové fragmenty o tom, jak používat **Nástroj pro Import nebo Export Azure**naleznete v tématu [Použití služby Azure Import nebo Export](../storage/storage-import-export-service.md) .

4. Výše uvedený příkaz vytvoří soubor deníku v zadaném umístění. Vytvoření úlohy importu z [Azure klasické portál](https://manage.windowsazure.com)použijete tento soubor deníku.

## <a name="create-an-import-job"></a>Vytvoření úlohy importu

Teď můžete vytvářet úlohy importu pomocí pokynů u [Pomocí služby Azure Import a Export](../storage/storage-import-export-service.md) (v části **vytvořit úlohy importu** ). Pro tento projekt importu se další informace najdete taky poskytují deníku soubor vytvořený při přípravě diskových jednotek.

## <a name="physically-ship-the-disks"></a>Fyzicky dodat disků

Můžete nyní fyzicky dodáte discích, které mají Azure datovém centru, které je zkopírována data přes pro objekty BLOB Azure úložiště jste zadali při vytváření úlohy importu. Také při vytváření projektu, pokud jste informace o sledování později, můžete teď můžete přejít zpátky do práce import a aktualizace číslem pro sledování.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Kopírování dat z Azure úložiště objektů BLOB Azure dat jezera Store

Jakmile se stav úlohy importu se zobrazí dokončené, můžete ověřit, jestli data je k dispozici v objektů BLOB Azure úložiště bylo zadané. Pak můžete různých způsobů přejděte tato data z objektů BLOB Azure úložiště úložiště jezera dat Azure. Všechny dostupné možnosti ukládání dat najdete v článku [Ingesting dat do úložiště jezera Data](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

V této části vám poskytneme definice JSON, které můžete použít k vytvoření kanálu Azure Data Factory ke kopírování dat. Můžete použít tyto JSON definice z [Azure portál](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) [Azure Powershellu](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Zdroj propojené služby (Azure úložiště objektů Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Propojené službu (Azure jezera úložišti)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Zadávání uvedenou množinu dat
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Výstup uvedenou množinu dat
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Kanálem k odesílání zpráv (Kopírovat aktivitu)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Další informace o používání Azure Data Factory k přesunutí dat z Azure úložiště objektů Blob a jezera dat Azure úložiště přihlašovacích údajů najdete v článku [přesunutí dat z Azure úložiště objektů Blob Azure dat jezera úložiště pomocí Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Obnovit datových souborů v úložišti jezera dat Azure

Můžeme začít soubor, který byl 319GB velikosti a došlo k poškození ho do soubory menší velikosti tak, aby ho může dojít ke ztrátě určitého pomocí služby Azure Import nebo Export. Teď se data v úložišti jezera Azure dat, můžeme reconstrcut souboru, který má původní velikosti. Následující cmldts Azure PowerShell můžete použít k tomu nevyzve.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
