## <a name="column-mapping-with-translator-rules"></a>Mapování sloupce s pravidly Minipřekladač
Mapování sloupce lze určit, jak sloupce zadané struktury"" zdrojové tabulky mapy na sloupce podle "struktuře" jímky tabulky. Vlastnost **mapování sloupců** je k dispozici v části **typeProperties** aktivity kopírovat.

Mapování sloupce podporuje následujících situacích:

- Všechny sloupce v tabulce zdroj "strukturu" jsou namapované na všech sloupců v tabulce jímky "strukturu".
- Podmnožinu sloupců v tabulce zdroj "strukturu" jsou namapované na všech sloupců v tabulce jímky "strukturu".

Následující chybové podmínky a budou výsledkem výjimku:

- Méně sloupců nebo více sloupců, "strukturu" jímky tabulku než podle mapování.
- Duplicitní mapování.
- Výsledek dotazu SQL nemá název sloupce, který není zadán v mapování.

## <a name="column-mapping-samples"></a>Sloupec mapování vzorky
> [AZURE.NOTE] Následující ukázky Azure SQL a objektů Blob Azure ale jsou k dispozici žádné úložiště dat, který podporuje obdélníkový datové sady. Budete muset upravit sady dat a definic propojené služeb v příkladech pod tak, aby ukazovaly na data ve zdroji relevantních dat. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Příklad 1 – sloupce mapování z Azure SQL objektů blob Azure
V tomto příkladu vstupní tabulka obsahuje strukturu a odkazy na tabulky SQL databáze Azure SQL.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

V tomto příkladu dosadit má strukturu a ukazoval objektů blob v úložišti objektů blob Azure.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
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
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

JSON aktivity jsou uvedeny níže. Sloupců ze zdroje namapovaných na sloupce v jímky (**columnMappings**) pomocí vlastnosti **Minipřekladač** .

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Tok mapování sloupců:**

![Tok mapování sloupce](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Příklad 2 – sloupce mapování u SQL dotazu v Azure SQL objektů blob Azure
V tomto příkladu dotaz SQL zobrazený slouží k extrahování dat z Azure SQL místo stačí zadat název tabulky a názvy sloupců v části "strukturu". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

V tomto případě výsledků dotazu jsou namapované nejdřív sloupců zadaných struktury"" zdroje. Pak sloupců ze zdroje "strukturu" jsou namapované na sloupce v jímky "strukturu" s pravidly podle columnMappings.  Předpokládejme, že dotaz vrátí 5 sloupců, dalších dvou sloupců jsou uvedeny v "struktuře" zdroje.

**Tok mapování sloupce**

![Mapování sloupce toku-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







