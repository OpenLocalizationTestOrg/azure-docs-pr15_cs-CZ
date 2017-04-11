<properties 
    pageTitle="Přesunutí dat do/z DocumentDB | Microsoft Azure" 
    description="Zjistěte, jak přesunout data do/z kolekce Azure DocumentDB pomocí Azure Data Factory" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Přesunutí dat a od DocumentDB pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesunout data Azure DocumentDB z jiného dat ukládat a přesuňte data z DocumentDB do jiného úložiště dat. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

Následující příklady ukazují, jak chcete data zkopírovat do a z Azure DocumentDB a úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** z libovolných zdrojů k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivitu v Azure Data Factory.  

> [AZURE.NOTE] Kopírování dat z dat IaaS o místní/Azure ukládá Azure DocumentDB a naopak jsou podporovány s Brána pro správu dat verze 2.1 a vyšší.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Ukázka: Zkopírování dat z DocumentDB objektů Blob Azure

Následující příklad ukazuje:

1. Propojené služba typu [DocumentDb](#azure-documentdb-linked-service-properties).
2. Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Zadávání [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat v Azure DocumentDB k objektů Blob Azure. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky.

**Azure DocumentDB propojené služby:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Úložiště objektů Blob Azure propojené služby:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure dokumentu DB vstupní datovou sadu:**

Vzorku předpokládá, že máte kolekci s názvem **osoby** v databázi aplikace Azure DocumentDB.
 
Nastavení "externí": "PRAVDA" a určení externalData informace o zásadách služby Azure Data Factory, že v tabulce externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Objektů Blob Azure výstup datovou sadu:**

Data zkopírována do nových objektů blob každou hodinu cesta objektů blob odrážející konkrétní datetime s granularity hodinu.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Ukázka JSON dokumentu v kolekci osoby v databázi DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB podporuje zjišťování dokumentů pomocí SQL jako syntaxe hierarchické JSON dokumenty. 

Příklad: Vyberte Person.PersonId, Person.Name.First jako jméno Person.Name.Middle jako MiddleName Person.Name.Last podle příjmení od osoby

Následující kanálem k odesílání zpráv slouží ke kopírování dat z kolekce osoby v databázi DocumentDB k objektů blob Azure. Jako součást aktivity kopírovat vstupní a výstupní byla specifikována datové sady.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Ukázka: Zkopírování dat z objektů Blob Azure Azure DocumentDB

Následující příklad ukazuje:

1. Propojené služba typu [DocumentDb](#azure-documentdb-linked-service-properties).
2. Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) a [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Vzorku slouží ke kopírování dat z Azure objektů blob Azure DocumentDB. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky.

**Úložiště objektů Blob Azure propojené služby:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB propojené služby:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure objektů Blob vstupní datovou sadu:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB výstup datovou sadu:**

Vzorku slouží ke kopírování dat do kolekce s názvem "Pracovníka".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Následující kanálem k odesílání zpráv slouží ke kopírování dat z objektů Blob Azure ke kolekci člověka v DocumentDB. Jako součást aktivity kopírovat vstupní a výstupní byla specifikována datové sady. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Pokud je ukázkový objektů blob vstup jako 

    1,John,,Doe

Výstup JSON v DocumentDB bude jako:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB je NoSQL úložiště pro dokumenty, JSON, kde jsou povoleny vnořené struktury. Azure Data Factory umožňuje uživateli označují hierarchie prostřednictvím **nestingSeparator**, což je "." v tomto příkladu. Oddělovače, kopírovat aktivity vygeneruje "Název" objekt s tři podřízené elementy nejdřív druhé jméno a příjmení, podle toho, "Name.First", "Name.Middle" a "Name.Last" v definice tabulky.

## <a name="azure-documentdb-linked-service-properties"></a>Vlastnosti DocumentDB propojené služby Azure

Následující tabulka obsahuje popis prvků JSON specifické pro službu Azure DocumentDB propojené. 

| **Vlastnost** | **Popis** | **Povinné** |
| -------- | ----------- | --------- |
| Typ | Vlastnost typu musí být nastavena na: **DocumentDb** | Ano |
| connectionString | Zadání informací potřebných pro připojení k databázi Azure DocumentDB. | Ano |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure vlastnosti datovou sadu DocumentDB typů

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly, například strukturu, dostupnost a zásady datovou sadu JSON se podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).
 
V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části typeProperties pro datovou sadu typ **DocumentDbCollection** má následující vlastnosti.

| **Vlastnost** | **Popis** | **Povinné** |
| -------- | ----------- | -------- |
| Název_kolekce | Název kolekce DocumentDB dokumentů. | Ano |


Příklad:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schéma tak, že Data Factory
Pro úložiště zdarma schématu dat například DocumentDB službu Data Factory odvozeny schématu v jednom z těchto způsobů:  

1.  Pokud zadáte struktury dat pomocí vlastnosti **strukturu** v definici datovou sadu, službu Data Factory respektuje tuto strukturu jako schéma. V tomto případě Pokud řádek neobsahuje hodnotu pro sloupec, hodnota null bude poskytována jej.
2.  Pokud nezadáte struktury dat pomocí vlastnosti **strukturu** v definici datovou sadu, službu Data Factory odvozeny schématu pomocí prvního řádku na data. V tomto případě pokud prvního řádku neobsahuje úplné schéma, některé sloupce chybět ve výsledku kopírování.

Pro zdroje dat uvolnit schématu doporučený postup tedy můžete určit, struktury dat pomocí vlastnosti **strukturu** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure vlastnosti typ DocumentDB kopírovat činnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.
 
**Poznámka:** Aktivity kopírovat pouze jeden vstup, která bude a jedinou výstup.

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší jednotlivé typy aktivit a v případě kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

V případě kopírovat aktivity po zdroj typu **DocumentDbCollectionSource** následující vlastnosti jsou k dispozici v části **typeProperties** :

| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Povinné** |
| ------------ | --------------- | ------------------ | ------------ |
| dotaz | Zadejte dotaz číst data. | Řetězec nepodporuje DocumentDB dotazu. <br/><br/>Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Ne <br/><br/>Pokud není zadán, který se spustí příkaz SQL:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Speciální znak označíte, že je vnořeno dokumentu | Jakýkoli znak. <br/><br/>DocumentDB je NoSQL úložiště pro dokumenty, JSON, kde jsou povoleny vnořené struktury. Azure Data Factory umožňuje uživateli označují hierarchie prostřednictvím nestingSeparator, což je "." v předchozích příkladech. Oddělovače, kopírovat aktivity vygeneruje "Název" objekt s tři podřízené elementy nejdřív druhé jméno a příjmení, podle toho, "Name.First", "Name.Middle" a "Name.Last" v definice tabulky. | Ne

**DocumentDbCollectionSink** podporuje následující vlastnosti:

| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Povinné** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Speciální znak v názvu zdrojového sloupce označíte vnořené dokumentu není potřeba. <br/><br/>Například nad: `Name.First` do výstupu tabulky vytvoří následující strukturu JSON DocumentDB dokumentu:<br/><br/>"Název": {<br/>  "První": "Petr"<br/>}, | Znak, který se používá k oddělení počet úrovní vnoření.<br/><br/>Výchozí hodnota je `.` (tečka). | Znak, který se používá k oddělení počet úrovní vnoření. <br/><br/>Výchozí hodnota je `.` (tečka). | Ne | 
| writeBatchSize | Počet paralelní žádosti o služby DocumentDB vytvářet dokumenty.<br/><br/>Můžete optimalizovat výkon při kopírování dat z DocumentDB pomocí této vlastnosti. Lepší výkon můžete očekávat, pokud zvětšíte writeBatchSize, protože se odešle žádost o další paralelní k DocumentDB. Přesto musíte se vyhnout omezení, která může vyvolat chybová zpráva: "Požadavku sazba velké".<br/><br/>Omezení je určeno mnoha faktorů, jako například velikosti dokumenty, počet podmínek v dokumentech, indexování zásad cílové kolekce atd. Při kopírování, je možné použít lepší kolekci (například S3) tím největší výkon dostupná (2 500 požádat o jednotky/druhé). | Celé číslo | Ne (výchozí: 10000) |
| writeBatchTimeout | Počkejte, než čas na dokončení časový limit operace. | časového rozpětí<br/><br/> Příklad: "00: 30:00" (30 minut). | Ne |
 
## <a name="appendix"></a>Dodatek
1. **Otázka:**  
    nemá aktualizaci podpory kopírovat aktivity stávajících záznamů?

    **Odpověď:**  
    ne.

2. **Otázka:**  
    jak už má opakovat kopii DocumentDB věcech zkopírovali záznamy?

    **Odpověď:**  
    Pokud záznamy obsahují pole "ID" a zkopírování pokusí vložte nový záznam se stejným ID, zkopírování vyvolá chybu.  
 
3. **Otázka:** 
    Data Factory podporuje [oblasti nebo dat na základě hash oddílů](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Odpověď:** 
    ne. 
4. **Otázka:** 
    můžete zadat víc než jednu kolekci DocumentDB pro tabulku?
    
    **Odpověď:** 
    ne. Jedinou kolekce může být zadán v tomto okamžiku.
     
## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.
