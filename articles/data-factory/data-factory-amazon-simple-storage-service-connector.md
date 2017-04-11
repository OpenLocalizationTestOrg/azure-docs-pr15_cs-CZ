<properties 
    pageTitle="Přesuňte data z Amazon jednoduché úložiště služby pomocí Data Factory | Microsoft Azure" 
    description="Informace o tom, jak přesunout data z Amazon jednoduché úložiště služby (S3) pomocí Azure Data Factory." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Přesunutí dat z Amazon jednoduché úložiště služby pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivitu v Azure dat factory přesuňte data z Amazon jednoduché úložiště služby (S3) jiné data uložená. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který představuje obecné základní informace o přesun dat a seznam podporovaných zdroj/jímky úložiště dat s aktivitou kopírovat.  

Data factory v současné době podporuje pouze přesunutí dat z Amazon S3 další úložiště dat, ale ne pro přesunutí dat z jiných úložiště dat na Amazon S3.

## <a name="required-permissions"></a>Požadované oprávnění

Kopírování dat z Amazon S3, zkontrolujte, jestli že jste obdrželi následující oprávnění:

- **S3:GetObject** a **s3:GetObjectVersion** pro Amazon S3 objekt operace
- **S3:ListBucket** a **s3:ListAllMyBuckets** (používané v Průvodci kopírovat pouze) pro Amazon S3 bloku operace 

Úplný seznam Amazon S3 permisions s podrobností můžete najít z [Určující oprávnění v zásadu](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z Amazon S3 je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak chcete zkopírovat data z Amazon S3 k úložišti objektů Blob Azure. Data můžete však zkopírována do jednotlivých propadů uvedená [v tomto poli](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Ukázka: Zkopírování dat z Amazon S3 objektů Blob Azure
Tento příklad ukazuje, jak chcete zkopírovat data z Amazon S3 k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

- Propojené služba typu [AwsAccessKey](#linked-service-properties).
- Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AmazonS3](#dataset-type-properties).
- Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [FileSystemSource](#copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat z Amazon S3 k objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

**Amazon S3 propojené služby**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
            }
        }
    }

**Služba Azure úložiště propojené**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Zadávání datovou sadu Amazon S3**

Nastavení **"externí": PRAVDA** informoval službu Data Factory, že datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory. Nastavte tuto vlastnost na hodnotu true na vstupní sady dat, která není vytvořené pomocí aktivita v kanálu.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Příležitostí s aktivitou kopie**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **FileSystemSource** a **jímky** typ je nastavený na **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Následující tabulka obsahuje popis JSON elementy specifické pro Amazon S3 (**AwsAccessKey**) propojené služby.

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID tajné přístupové klávesy. | řetězec | Ano |
| secretAccessKey | Řeknu přístupová klávesa samotné. | Šifrované tajné řetězec | Ano | 


## <a name="dataset-type-properties"></a>Vlastnosti typu datové sady

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady se podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu **AmazonS3** (který obsahuje sadu Amazon S3) obsahuje následující vlastnosti

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------- | ------ | 
| bucketName | Název bloku S3. | Řetězec | Ano |
| klíč | Klíč S3 objektu. | Řetězec | Ne | 
| Předpona | Předpona pro klávesu S3 objektu. Tím se vyberou objekty jejíž klíče začít tuto předponu. Platí jenom klíč po prázdné. | Řetězec | Ne | 
| verze | Verze objektu S3, pokud je povolena Správa verzí S3. | Řetězec | Ne |  
| Formát | Následující typy formátování podporované: **Formát textu** **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Nastavte vlastnosti **Typ** v části formát na jednu z těchto hodnot. Naleznete v částech [Určující formát textu](#specifying-textformat), [Zadání AvroFormat](#specifying-avroformat), [Zadání JsonFormat](#specifying-jsonformat), [Určující OrcFormat](#specifying-orcformat)a [Určení ParquetFormat](#specifying-parquetformat) podrobnosti. Pokud chcete zkopírovat soubory jako-je mezi založené na souboru ukládá (binární kopii), můžete přejít v části formát v obou definice vstupní a výstupní datovou sadu.| Ne
| komprese | Určete typ a stupeň komprese pro data. Jsou podporované typy: **GZip** **Deflate**a **BZip2** a podporované úrovně jsou: **Optimal** a **nejrychlejší**. Nastavení komprese nejsou v současné době podporované pro **AvroFormat** nebo **OrcFormat**daty. Další informace najdete v části [podporu komprese](#compression-support) .  | Ne |

> [AZURE.NOTE] bucketName + klávesa Určuje umístění objektu S3 kde blok je kontejner kořenové pro S3 objekty a klíč je úplnou cestu k S3 objektu.

### <a name="sample-dataset-with-prefix"></a>Ukázkové datové sady s předponou

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Ukázka uvedenou množinu dat (s verze)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamické cesty pro S3

Ve vzorku používáme pevných hodnot pro spolu s bucketName vlastností v datové sadě Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Můžete mít Data Factory výpočet klávesu spolu s bucketName dynamicky za běhu pomocí proměnných systému třeba SliceStart.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Můžete udělat stejný pro vlastnost předponu datovou sadu Amazon S3. Seznam podporovaných funkcí a proměnných naleznete v tématu [funkce Factory dat a systémových proměnných](data-factory-functions-variables.md) . 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Zkopírujte vlastnosti typ činnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části **typeProperties** aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Po zdroje v kopírování aktivitu typu **FileSystemSource** (jejichž součástí Amazon S3) následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- | 
| rekurzivní | Určuje, zda chcete zpětně seznamu S3 objektů v adresáři. | True nebo false | Ne | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

## <a name="next-steps"></a>Další kroky
Naleznete v následujících článcích: 
- [Kopírovat aktivity kurz](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytváření kanálů aktivitu kopírovat. 
