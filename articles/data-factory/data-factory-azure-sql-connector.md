<properties 
    pageTitle="Přesunutí dat do/z databáze SQL Azure | Microsoft Azure" 
    description="Další informace o přesunutí dat z databáze SQL Azure pomocí Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Přesunutí dat do a z databáze SQL Azure pomocí Azure Data Factory
Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesun dat z databáze SQL Azure z nebo do jiného úložiště dat. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který zobrazí obecný přehled přesun dat s kopírovat aktivity a podporovaný datový úložiště kombinace. 

## <a name="supported-sources-and-sinks"></a>Podporované zdroje a propadů
[Podporované datový úložiště](data-factory-data-movement-activities.md#supported-data-stores-and-formats) najdete v tabulce seznam úložiště dat jako zdroje nebo propadů nepodporuje aktivity kopírovat. Data můžete přesunout z jakéhokoli podporované zdroje dat úložiště k databázi SQL Azure nebo z databáze SQL Azure do úložiště všechny podporované jímky.

## <a name="create-pipeline"></a>Vytvoření kanálu
Vytvořte příležitost aktivitu kopírování, přesouvání dat z databáze Azure SQL pomocí různých nástrojů a rozhraní API.  

- Kopírování Průvodce
- Azure portálu
- Visual Studio
- Azure Powershellu
- ROZHRANÍ API .NET
- ROZHRANÍ REST API

Podrobný postup pro různé způsoby vytváření kanálů aktivitu kopírovat naleznete v tématu [kurz aktivity kopírovat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .   

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze SQL Azure, je použít Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z databáze SQL Azure nebo úložišti objektů Blob Azure znázorňují. Data lze však zkopírovaný **přímo** z libovolných zdrojů k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Ukázka: Zkopírování dat z databáze SQL Azure objektů Blob Azure

Stejný definuje následujících entit Factory dat:

1. Propojené služba typu [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [SqlSource](#azure-sql-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat časové řady (každou hodinu, denních, atd.) z tabulky databáze Azure SQL na objekt blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky.  

**Služba propojené SQL Azure**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Naleznete v části [Propojené služby SQL Azure](#azure-sql-linked-service-properties) seznamu vlastností podporovaných tuto službu propojené. 

**Azure služba propojené úložiště objektů Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Naleznete v článku [Objektů Blob Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) seznamu vlastností podporovaných tuto službu propojené. 

**Zadávání sadu Azure SQL**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro dat časové řady. 

Nastavení "externí": "true" informuje o tom, služby Azure Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

Naleznete v části [Vlastnosti typu sady dat Azure SQL](#azure-sql-dataset-type-properties) seznamu vlastností podporované tímto typem datovou sadu.  

**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Naleznete v části [Vlastnosti typu datovou sadu objektů Blob Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) seznamu vlastností podporované tímto typem datovou sadu.  

**Příležitostí s aktivitou kopie**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **SqlSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **SqlReaderQuery** slouží k výběru data v poslední hodinu zkopírovat.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

V příkladu **sqlReaderQuery** určeným pro SqlSource. Aktivity kopírovat spustí dotaz zdroji databáze SQL Azure přístup k datům. Můžete taky můžete určit uložená procedura zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura trvá parametry).

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, v části strukturu datovou sadu JSON definované sloupce se používají k vytvoření dotazu na spustí hledání v databázi SQL Azure. Příklad: `select column1, column2 from mytable`. Pokud definici datovou sadu nemá strukturu, jsou vybrány všechny sloupce z tabulky. 


Seznam vlastností podporované SqlSource a BlobSink najdete v části [Zdroj Sql](#sqlsource) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) . 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Ukázka: Kopírování dat z objektů Blob Azure k databázi SQL Azure

Vzorku definuje následujících entit Factory dat:  

1.  Propojené služba typu [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) a [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Ukázka kopií časových řad dat (každou hodinu, denně, atd.) z Azure objektů blob do tabulky v Azure SQL databáze každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky. 


**Služba propojené SQL Azure**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Naleznete v části [Propojené služby SQL Azure](#azure-sql-linked-service-properties) seznamu vlastností podporovaných tuto službu propojené. 

**Azure služba propojené úložiště objektů Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Naleznete v článku [Objektů Blob Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) seznamu vlastností podporovaných tuto službu propojené.

**Azure vstupní datovou sadu objektů Blob**

Data je vybraných kontakty z nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Složka cesty a názvu souboru pro objektů blob jsou vyhodnoceny dynamicky podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc a den část čas zahájení a název souboru hodinu část počáteční čas. "externí": "true" nastavení informuje službu Data Factory, že v této tabulce externí stránku factory dat a není vytvořené pomocí aktivitu v data factory.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
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

Naleznete v části [Vlastnosti typu datovou sadu objektů Blob Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) seznamu vlastností podporované tímto typem datovou sadu.

**Sadu výstup Azure SQL**

Vzorku slouží ke kopírování dat na tabulku s názvem "Tabulka" v Azure SQL. Vytvoření tabulky v SQL Azure pomocí stejný počet sloupců podle očekávání soubor CSV objektů Blob má být. Nové řádky jsou přidané do tabulky každou hodinu. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Naleznete v části [Vlastnosti typu sady dat Azure SQL](#azure-sql-dataset-type-properties) seznamu vlastností podporované tímto typem datovou sadu.

**Příležitostí s aktivitou kopie**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **BlobSource** a **jímky** typ je nastavený na **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Seznam vlastností podporované SqlSink a BlobSource najdete v části [Jímky Sql](#sqlsink) a [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) . 


## <a name="azure-sql-linked-service-properties"></a>Vlastnosti služby Azure propojené SQL
Ve vzorcích použili jste propojené služba typu **AzureSqlDatabase** propojení databáze Azure SQL data factory. Následující tabulka obsahuje popis prvků JSON specifické pro službu propojené SQL Azure.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Typ | Vlastnost typu musí být nastavena na: **AzureSqlDatabase** | Ano |
| connectionString | Zadejte informace potřebné k připojení k databázi SQL Azure instanci vlastnost connectionString. | Ano |

> [AZURE.NOTE] Konfigurace [Brány Firewall databáze SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) databázový server [Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)služeb pro přístup k serveru. Navíc pokud k databázi SQL Azure se kopírují data z jiné než Azure včetně z místního zdroje dat s brána factory dat, nakonfigurujte odpovídající rozsah IP adres pro počítač, který je odesílání dat do databáze SQL Azure. 

## <a name="azure-sql-dataset-type-properties"></a>Vlastnosti typ sady dat Azure SQL
Ve vzorcích použili jste datovou sadu typu **AzureSqlTable** představující tabulky v databázi Azure SQL. 

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.). 

V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části **typeProperties** pro datovou sadu typ **AzureSqlTable** obsahuje následující vlastnosti:

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Název tabulky | Název tabulky v databázi SQL Azure instanci odkazující propojené služby. | Ano |

## <a name="azure-sql-copy-activity-type-properties"></a>Vlastnosti kopírovat typu aktivity Azure SQL
Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

> [AZURE.NOTE] Aktivity kopírovat pouze jeden vstup, která bude a jedinou výstup.

Vlastnosti dostupné v části **typeProperties** aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů. 

Pokud přesunujete dat z databáze Azure SQL, nastavíte typ zdroje v kopírování aktivitu **SqlSource**. Podobně při přesunutí dat do databáze Azure SQL nastavíte typ jímka v kopírování aktivitu **SqlSink**. Tato část obsahuje seznam vlastností podporované SqlSource a SqlSink. 

### <a name="sqlsource"></a>SqlSource

Kopírovat činnost Pokud je zdroj typu **SqlSource**následující vlastnosti jsou k dispozici **typeProperties** oddílu:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: `select * from MyTable`.  | Ne |
| sqlReaderStoredProcedureName | Název uložené procedury, která načte data ze zdrojové tabulky. | Název uložené procedury. | Ne |
| storedProcedureParameters | Parametrů pro uložené procedury. | Název/hodnota dvojice. Názvy a velikosti písmen parametrů musí shodovat s názvy a velikost písmen uložená procedura parametrů. | Ne |

Je-li **sqlReaderQuery** pro SqlSource, spustí aktivity Kopírovat tento dotaz zdroji databáze SQL Azure přístup k datům. Můžete taky můžete určit uložená procedura zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura trvá parametry). 

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, struktury bodu datovou sadu JSON sloupce se používají k vytvoření dotazu (`select column1, column2 from mytable`) spusťte databázi SQL Azure. Pokud definici datovou sadu nemá strukturu, jsou vybrány všechny sloupce z tabulky. 

> [AZURE.NOTE] Pokud používáte **sqlReaderStoredProcedureName**, potřebujete k zadání hodnoty pro vlastnost **název tabulky** v datové sadě JSON. Existuje bez ověření přes provedena v této tabulce. 

### <a name="sqlsource-example"></a>Příklad SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Definice uložené procedury:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** podporuje následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Počkejte, než času pro vložení dávku dokončete časový limit. | časového rozpětí<br/><br/> Příklad: "00: 30:00" (30 minut). | Ne | 
| writeBatchSize | Vloží data do tabulky SQL vyrovnávací paměť dosažení writeBatchSize. | Celé číslo (počet řádků)| Ne (výchozí: 10000)
| sqlWriterCleanupScript | Zadejte dotaz kopírovat aktivity provést tak, aby se data konkrétní výseče vyčistí. Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy). | Příkaz dotazu.  | Ne |
| sliceIdentifierColumnName | Zadejte do sloupce Název aktivity kopírovat vyplnit identifikátor automatické generování řez, který se používá vyčistit data konkrétní výseče při znovu spustit. Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy). | Sloupec název sloupce s datovým typem binary(32). | Ne |
| sqlWriterStoredProcedureName | Název uložené procedury upserts (aktualizace/vloží) data do cílové tabulky. | Název uložené procedury. | Ne |
| storedProcedureParameters | Parametrů pro uložené procedury. | Název/hodnota dvojice. Názvy a velikosti písmen parametrů musí shodovat s názvy a velikost písmen uložená procedura parametrů. | Ne | 
| sqlWriterTableType | Zadejte název tabulky typu se nemusí používat v uložená procedura. Kopírovat aktivity zpřístupní data, která je přesouvána temp tabulky pomocí tohoto typu tabulky. Uložená procedura kód pak sloučit data kopírovaný s existující data. | Zadejte název tabulky. | Ne |

#### <a name="sqlsink-example"></a>Příklad SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Sloupce identity v cílové databázi
Tato část obsahuje příklad ke kopírování dat z ke zdrojové tabulce bez sloupec identity do cílové tabulky se sloupcem identity. 

**Zdrojové tabulky:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Cílová tabulka:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Všimněte si, že cílovou tabulku má sloupec identity. 

**Definice JSON datovou sadu zdroje**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Určení datovou sadu JSON definice**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Všimněte si, že zdrojový a cílové tabulce mají různé schématu (cílový nemá nějaký další sloupec s identity). V tomto scénáři je třeba zadat vlastnost **strukturu** v definici datovou sadu cílové, která neobsahuje sloupec identity. 

Potom namapování sloupců z datovou sadu zdroje na sloupce v cílové datové sadě. Příklad jsou uvedeny v části [mapování sloupců vzorky](#column-mapping-samples) . 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Zadejte mapování pro SQL Server a databázi SQL Azure

Jak je uvedeno v aktivitu [aktivity přesun dat](data-factory-data-movement-activities.md) článek kopírování provádí automatické typ převody z typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesunutí dat a od SQL Azure, SQL server, Sybase následující mapování slouží z typ SQL typ .NET a naopak.

Mapování je stejný příkaz jako mapování SQL serveru datový typ pro ADO.NET.

| Typ SQL serveru databázový stroj | Typ .NET framework |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| binární. | Byte] |
| Bit | Logická hodnota |
| znak | Řetězec, znak] |
| Datum | Data a času |
| Data a času | Data a času |
| datetime2 | Data a času |
| DateTimeOffset | DateTimeOffset |
| Decimal | Decimal |
| Atribut FILESTREAM (varbinary(max)) | Byte] |
| Plovoucí | Dvojité |
| Obrázek | Byte] | 
| Funkce INT | Int32 | 
| peníze | Decimal |
| nchar | Řetězec, znak] |
| ntext | Řetězec, znak] |
| číselné | Decimal |
| nvarchar | Řetězec, znak] |
| skutečné | Jeden |
| ROWVERSION | Byte] |
| smalldatetime | Data a času |
| smallint | Int16 |
| Smallmoney | Decimal | 
| SQL_VARIANT | Objekt * |
| text | Řetězec, znak] |
| čas | Časového rozpětí |
| časové razítko | Byte] |
| tinyint | Bajt |
| UniqueIdentifier | Identifikátor GUID |
| Seskupit |  Byte] |
| Datová | Řetězec, znak] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.