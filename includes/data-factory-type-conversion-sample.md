### <a name="type-conversion-sample"></a>Ukázka převodu typu
V následujícím příkladu je ke kopírování dat z objektů Blob Azure SQL s převody typu.

Předpokládejme datovou sadu objektů Blob jsou ve formátu CSV a obsahuje 3 sloupci. Jedna z nich je sloupec data a času pomocí formátu vlastní data a času pomocí zkrácený francouzsky názvy pro den v týdnu.

Definujete zdroj objektů Blob datovou sadu takto spolu s definice typů sloupců.

    {
        "name": "AzureBlobTypeSystemInput",
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

Zadaných tento kód SQL by typ .NET typ mapování tabulky nad můžete definovat v tabulce Azure SQL pomocí následující schéma.

| Název sloupce | Typ SQL |
| ----------- | -------- |
| ID uživatele | bigint |
| Jméno | text |
| lastlogindate | data a času |

Další takto definujete sady dat Azure SQL. Poznámka: Nepotřebujete k určení oddíl "strukturu" s typem informací od informace o typu již zadali v úložišti podkladová data.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

V tomto případě factory dat automaticky provede požadovaný typ převody včetně pole data a času s vlastní datetime naformátovat pomocí jazykovou verzi fr FR. při přesunutí dat z objektů Blob Azure SQL.


