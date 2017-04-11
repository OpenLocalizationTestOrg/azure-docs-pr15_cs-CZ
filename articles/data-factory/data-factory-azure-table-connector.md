<properties 
    pageTitle="Přesunutí dat do/z tabulky Azure | Microsoft Azure" 
    description="Zjistěte, jak chcete přesunout data z úložiště tabulek Azure pomocí Azure Data Factory." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Přesunutí dat do a z tabulky Azure pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory Pokud chcete přesunout data z tabulky Azure z/k úložišti jiného. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který zobrazí obecný přehled přesun dat a podporovaný datový úložiště kombinací s kopírovat aktivitu.

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z úložiště tabulek Azure je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z úložiště tabulek Azure a databázových objektů Blob Azure a znázorňují. Data lze však zkopírovaný **přímo** z libovolných zdrojů k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivitu v Azure Data Factory.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Ukázka: Zkopírování dat z Azure tabulky objektů Blob Azure

Následující příklad ukazuje:

1.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (slouží k tabulce a objektů blob).
2.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [Azure](#azure-table-dataset-type-properties).
3.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [AzureTableSource](#azure-table-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Vzorku slouží ke kopírování dat patřící do výchozího oddílu v tabulce Azure na objekt blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky.

**Služba Azure úložiště propojené:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory podporuje dva typy služby Azure úložiště propojené: **AzureStorage** a **AzureStorageSas**. Pro první z nich zadat připojovací řetězec, který obsahuje klíč účtu a pro pozdější skupinu, zadejte identifikátor Uri sdílené přidružení (zabezpečení aplikace Access podpis). [Propojené služby](#linked-services) v části Podrobnosti.  

**Azure tabulky vstupní datovou sadu:**

Vzorku předpokládá, že jste vytvořili tabulky "Tabulka" v tabulce Azure.
 
Nastavení "externí": "true" informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Objektů Blob Azure výstup datovou sadu:**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **AzureTableSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený zadanými pomocí vlastnosti **AzureTableSourceQuery** slouží k výběru data z oddílu výchozí každou hodinu zkopírovat.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Ukázka: Zkopírování dat z objektů Blob Azure tabulky Azure

Následující příklad ukazuje:

1.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (slouží k blob & tabulky)
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [Azure](#azure-table-dataset-type-properties). 
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) a [AzureTableSink](#azure-table-copy-activity-type-properties). 


Ukázka kopií časových řad dat z Azure objektů blob do Azure tabulky každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky.

**Služba Azure úložiště (u tabulky Azure a objektů Blob) propojené:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory podporuje dva typy služby Azure úložiště propojené: **AzureStorage** a **AzureStorageSas**. Pro první z nich zadat připojovací řetězec, který obsahuje klíč účtu a pro pozdější skupinu, zadejte identifikátor Uri sdílené přidružení (zabezpečení aplikace Access podpis). [Propojené služby](#linked-services) v části Podrobnosti. 

**Azure objektů Blob vstupní datovou sadu:**

Data je vybraných kontakty z nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Složky cesty a názvu souboru pro objekt blob jsou vyhodnoceny dynamicky podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc a den část čas zahájení a název souboru hodinu část počáteční čas. "externí": "true" nastavení informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azure Table výstup datovou sadu:**

Vzorku slouží ke kopírování dat na tabulku s názvem "Tabulka" v tabulce Azure. Vytvoření Azure tabulky mají stejný počet sloupců podle očekávání soubor CSV objektů Blob má být. Nové řádky jsou přidané do tabulky každou hodinu. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **BlobSource** a **jímky** typ je nastavený na **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Propojené služby
Existují dva typy propojené služby, které můžete použít k propojení úložišti objektů blob Azure factory Azure data. Jsou: **AzureStorage** propojené služeb a **AzureStorageSas** propojené. Služba úložiště Azure propojené poskytuje factory dat s globální přístup k základnímu úložišti Azure. Vzhledem k tomu, propojené přidružení Azure úložiště zabezpečení (sdílené přístup podpis) služba poskytuje factory dat s omezení/čas mez přístup k základnímu úložišti Azure. Existuje bez dalších rozdílů mezi tyto dvě propojené služby. Zvolte propojené služba, která vyhovuje vašim potřebám. V následujících částech jsou podrobněji na tyto dvě propojené služby.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure vlastnosti typu datovou sadu tabulky

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady datovou sadu JSON podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části **typeProperties** pro datovou sadu typ **Azure** má následující vlastnosti.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Název tabulky | Název tabulky v databázi tabulky Azure instanci odkazující propojené služby. | Ano. Pokud název tabulky není zadán bez azureTableSourceQuery, všechny záznamy z tabulky budou zkopírovány do cíle. V případě azureTableSourceQuery také záznamy z tabulky, která splňuje dotaz se zkopírují do cíle. |

### <a name="schema-by-data-factory"></a>Schéma tak, že Data Factory
Pro úložiště zdarma schématu dat například tabulky Azure Data Factory služby odvozeny schématu v jednom z těchto způsobů:

1.  Pokud zadáte struktury dat pomocí vlastnosti **strukturu** v definici datovou sadu, službu Data Factory respektuje tuto strukturu jako schéma. V tomto případě Pokud řádek neobsahuje hodnotu pro sloupec, hodnota null je vynechaný ho.
2. Pokud nezadáte struktury dat pomocí vlastnosti **strukturu** v definici datovou sadu, Data Factory odvozeny schématu pomocí prvního řádku na data. V tomto případě pokud prvního řádku neobsahuje úplné schéma, jsou ve výsledku kopírování zmeškané některé sloupce.

Pro zdroje dat uvolnit schématu doporučený postup tedy můžete určit, struktury dat pomocí vlastnosti **strukturu** .

## <a name="azure-table-copy-activity-type-properties"></a>Azure vlastnosti typ činnosti kopírovat tabulku

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní datových sad a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

V části typeProperties **AzureTableSource** podporuje následující vlastnosti:

Vlastnost | Popis | Povolené hodnoty | Povinné
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Vlastní dotaz umožňuje číst data. | Řetězec tabulek Azure dotazu. Příklady v následující části. | Ne. Pokud název tabulky není zadán bez azureTableSourceQuery, všechny záznamy z tabulky budou zkopírovány do cíle. V případě azureTableSourceQuery také záznamy z tabulky, která splňuje dotaz se zkopírují do cíle.
azureTableSourceIgnoreTableNotFound | Označuje, zda swallow výjimky obsahu není k dispozici. | PRAVDA<br/>NEPRAVDA | Ne |

### <a name="azuretablesourcequery-examples"></a>Příklady azureTableSourceQuery

Pokud je sloupec tabulky Azure typu řetězec: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Pokud sloupec tabulky Azure typu datum a čas: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


V části typeProperties **AzureTableSink** podporuje následující vlastnosti:


Vlastnost | Popis | Povolené hodnoty | Povinné  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Výchozí oddíl hodnoty klíče, mohou sloužit jímkou. | Hodnotu řetězce. | Ne 
azureTablePartitionKeyName | Zadejte název sloupce, jehož hodnoty jsou používána jako zkratky oddíl. Pokud není zadán, AzureTableDefaultPartitionKeyValue slouží jako klíč oddílu. | Název sloupce. | Ne |
azureTableRowKeyName | Zadejte název sloupce, jehož hodnoty sloupců slouží jako klíč řádku. Pokud není zadán, použijte identifikátor GUID pro každý řádek. | Název sloupce. | Ne  
azureTableInsertType | Režim vložte data do tabulek Azure.<br/><br/>Tato vlastnost určuje, jestli máte existujících řádků v tabulce výstup s odpovídajícími oddíl řádku klávesám jejich hodnoty nahrazeny nebo sloučit. <br/><br/>Další informace o fungování tato nastavení (sloučit a nahradit) najdete v tématech [vložení či entitu hromadné korespondence](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [Insert nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> Toto nastavení se použije na úrovni řádku, ne úrovni tabulky a ani odstraní řádků v tabulce výstup, které nejsou ve vstupním. | sloučení (výchozí)<br/>nahrazení | Ne 
writeBatchSize | Vloží data do tabulek Azure při zásahu writeBatchSize nebo writeBatchTimeout. | Celé číslo (počet řádků)| Ne (výchozí: 10000) 
writeBatchTimeout | Vloží data do tabulek Azure při přístupů writeBatchSize nebo writeBatchTimeout | časového rozpětí<br/><br/>Příklad: "00:20:00" (20 minut) | Ne (výchozí limit úložiště klienta výchozí hodnota 90 sec)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Zdrojový sloupec namapujte na cílovém sloupci pomocí Minipřekladač vlastnost JSON, než budete moct použít cílovém sloupci jako azureTablePartitionKeyName.

V následujícím příkladu je zdrojový sloupec DivisionID namapovat sloupec cíl: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

DivisionID není zadán jako klíč oddílu. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Typ mapování tabulek Azure

Uvedených v článku [dat pohyb aktivity](data-factory-data-movement-activities.md) kopírovat aktivity provádí automatické typ převody z typy zdrojů do jímky typů s následující postup dvoustupňového.

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesunutí dat do a z tabulky Azure, následující [mapování definované službou Azure tabulky](https://msdn.microsoft.com/library/azure/dd179338.aspx) se používají z Azure tabulky OData typy .NET typ a naopak. 

| OData datový typ | Typ .NET | Podrobnosti |
| --------------- | --------- | ------- |
| Edm.Binary | Byte] | Pole bajtů až 64 KB. |
| Edm.Boolean | Logická hodnota | Logická hodnota. |
| Edm.DateTime | Data a času | Hodnota 64-bit vyjádřený jako koordinovaný univerzální čas (UTC). Podporované oblasti data a času počínaje 12:00 hodin, 1, 1601. ledna (DOBĚ) UTC. Oblast končí 31 prosinec 9999. |
| Edm.Double | dvojité | 64-bit plovoucí čárky hodnotu. |
| Edm.Guid | Identifikátor GUID | 128bitové globálně jedinečný identifikátor. |
| Edm.Int32 | Int32 nebo int | 32bitovou celočíselnou. |
| Edm.Int64 | Int64 nebo dlouhá citace | 64bitové celé číslo. |
| Edm.String | Řetězec | Hodnota kódování UTF-16. Řetězcové hodnoty může být až 64 KB. |

### <a name="type-conversion-sample"></a>Ukázka převodu typu

V následujícím příkladu je ke kopírování dat z objektů Blob Azure do tabulky Azure s převody typu. 

Předpokládejme datovou sadu objektů Blob jsou ve formátu CSV a obsahuje tři sloupce. Jedna z nich je sloupec data a času pomocí formátu vlastní data a času pomocí zkrácený francouzsky názvy pro den v týdnu. 

Definování datovou sadu objektů Blob zdroj takto spolu s definice typů sloupců.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Věnovat mapování typ z Azure tabulky OData typu typ .NET, definujete tabulky v tabulce Azure pomocí následující schéma. 

**Azure schématu tabulky:**

Název sloupce | Typ
----------- | --------
ID uživatele | Edm.Int64
Jméno | Edm.String 
lastlogindate | Edm.DateTime

Pak definujte sady dat Azure tabulky takto. Nemusíte zadejte "strukturu" oddílu s typem informací od informace o typu již zadali v úložišti podkladová data.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

V tomto případě Data Factory automaticky při převodech typů včetně pole data a času s formátem vlastní data a času pomocí jazykové verze "fr-fr" při přesunutí dat z objektů Blob Azure tabulky.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
Další informace o klíčových faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho, najdete v článku [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md).







