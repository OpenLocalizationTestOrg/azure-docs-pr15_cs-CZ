<properties
   pageTitle="Načtení dat s Azure Data Factory | Microsoft Azure"
   description="Zjistěte, jak pomocí načítání dat s Azure Data Factory"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Načtení dat s Azure Data Factory 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Tento kurz se dozvíte, jak vytvořit kanálů v Azure Data Factory k přesunutí dat z Azure úložiště objektů Blob Azure SQL datový sklad. Pomocí následujícího postupu udělejte toto:

+ Nastavte si ukázková data v úložišti objektů Blob Azure.
+ Připojení zdroje k Azure Data Factory.
+ Vytvoření kanálu chcete přesunout data z úložiště objektů BLOB k SQL datový sklad.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Než začnete

Seznámení s Azure Data Factory, najdete v článku [Úvod do továrny dat Azure][].

### <a name="create-or-identify-resources"></a>Vytvoření nebo identifikaci zdroje

Před zahájením tohoto kurzu, musíte mít v následujících zdrojích:

   + **Azure úložiště objektů Blob**: Tento kurz používá Azure úložiště objektů Blob jako zdroj dat Azure Data Factory profilace a tak musíte mít jednu k dispozici pro ukládání ukázková data. Pokud nemáte už, přečtěte si, jak [vytvořit účet úložiště][].

   + **Datový sklad SQL**: Tento kurz slouží k přesunutí dat z Azure úložiště objektů Blob SQL datový sklad a Ano potřebují mít datový sklad online, která se načte s ukázkovými daty AdventureWorksDW. Pokud už nemáte datový sklad, přečtěte si, jak [vytvořit jednu][Create a SQL Data Warehouse]. Pokud máte datový sklad, ale neměli zřízení s ukázkovými daty, můžete to udělat [načíst ručně][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory dokončí aktuální zatížení a proto je potřeba mít, který můžete použít k vytvoření kanálu přesun dat. Pokud nemáte už, přečtěte si, jak a vytvořte si ho v kroku 1 [začít pracovat s Azure Data Factory (Editor Factory dat)][].

   + **AZCopy**: je třeba AZCopy zkopírujte ukázková data z místního klienta do vaší objektů Blob Azure úložiště. Pokyny k instalaci najdete v [dokumentaci AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Krok 1: Zkopírujte ukázková data Azure úložiště objektů Blob

Až budete mít všechny části připravená, jste připraveni zkopírujte ukázková data do svého objektů Blob Azure úložiště.

1. [Stáhněte si ukázková data][]. Tato data přidá další třech letech data o prodeji AdventureWorksDW ukázková data.

2. Pomocí tohoto příkazu AZCopy zkopírujte třech letech dat do vašeho objektů Blob Azure úložiště.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Krok 2: Připojení zdroje dat Factory Azure

Teď jsou data na místě vytvoříme Azure Data Factory kanálu se přesuňte data z úložiště objektů blob Azure do SQL datový sklad.

Nejdřív otevřete [Azure portál][] a v levé nabídce vyberte data factory.

### <a name="step-21-create-linked-service"></a>Krok 2.1: Vytvoření propojených služby

Propojit váš účet Azure úložiště a SQL datový sklad data factory.  

1. Nejdřív spusťte proces registrace kliknutím na "Propojené službami" část factory dat a potom klikněte na "nová data obsahují." Vyberte název k registraci úložišti azure v části vyberte úložišti Azure jako typ a poté zadejte název účtu a klíč účtu.

2. Zaregistrujte SQL datový sklad přejděte "Autor a nasazení" části zaškrtněte úložišti nový a potom "Azure SQL datový sklad". Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétních informací.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Krok 2.2: Definice datové sady

Po vytvoření propojených služby, budou máme definovat výše uvedené množiny dat.  Tady to znamená, že definování struktury dat, který je přesouvána z úložiště datový sklad.  Další informace o vytváření

1. Tento proces spusťte tak, že přejdete do oddílu "Autor a nasazení" factory dat.

2. Klepněte na "Novou datovou sadu" a "Úložiště objektů Blob Azure" propojíte úložišti data factory.  Můžete použít pod skript k definování datům v úložišti objektů Blob Azure:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Teď můžeme naše datovou sadu pro definovat SQL datový sklad. Začneme stejným způsobem, kliknutím na "Novou datovou sadu" a "Azure SQL datový sklad".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Krok 3: Vytvoření a spuštění svého kanálu

Nakonec nastavit a spustit kanálu v Azure Data Factory.  Toto je operaci, která provede přesun skutečnými daty.  Můžete najít zobrazení na celé operace, které lze provádět s SQL datový sklad a Azure Data Factory [tady][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

V části "Autor a nasazení" klikněte na další příkazy a potom "Nové kanálem k odesílání zpráv".  Po vytvoření kanálu, můžete použít pod kód přenášet data datový sklad:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Další kroky

Další informace, začněte tím, zobrazení:

- [Cesta výuky azure Data Factory][].
- [Azure SQL datový sklad spojnice][]. Toto je základní tématu o pomocí datový sklad Azure SQL Azure Data Factory.


Tato témata poskytují podrobné informace o Azure Data Factory. Budou diskutovat o různých databáze SQL Azure nebo HDInsight, ale informace se týkají taky datový sklad SQL Azure.

- [Kurz: Začínáme s Azure Data Factory][] Toto je základní kurz pro zpracování dat s Azure Data Factory. V tomto kurzu vytvoříte vaše první kanálem k odesílání zpráv, který používá HDInsight transformace a analyzovat web protokoly měsíčně. Všimněte si není kopie v tomto kurzu.
- [Kurz: kopírování dat z Azure úložiště objektů Blob k databázi SQL Azure][]. V tomto kurzu vytvoříte kanálu v Azure Data Factory ke kopírování dat z Azure úložiště objektů Blob k databázi SQL Azure.

<!--Image references-->

<!--Article references-->
[Si přečtěte následující dokumentaci AZCopy]: ../storage/storage-use-azcopy.md
[Azure SQL datový sklad konektor]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Vytvoření účtu úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Začínáme s Azure Data Factory (Editor Factory dat)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Úvod k Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Kurz: Kopírování dat z Azure úložiště objektů Blob k databázi SQL Azure]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Kurz: Začínáme s Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory naučná stezka]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portálu]: https://portal.azure.com
[Stažení ukázkových dat]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
