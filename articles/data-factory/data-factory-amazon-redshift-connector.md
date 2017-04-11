<properties 
    pageTitle="Přesuňte data z Amazon Redshift pomocí Data Factory | Microsoft Azure" 
    description="Informace o tom, jak přesunout data z Amazon Redshift pomocí Azure Data Factory." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Přesunutí dat z Amazon Redshift pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesuňte data z Amazon Redshift na jiný datový úložiště. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který představuje obecné základní informace o přesun dat se kopie aktivity a seznamem úložišť zdrojový/jímky dat.  

Data factory v současné době podporuje pouze přesunutí dat z Amazon Redshift další úložiště dat, ale ne pro přesunutí dat z jiných úložiště dat na Amazon Redshift.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Při přesunutí dat do úložiště dat místní udělte přístup k obrázku Amazon Redshift Brána pro správu dat (použít IP adresu počítače). Pokyny najdete v článku [Povolení přístupu k clusteru](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Při přesunutí dat do úložiště Azure dat naleznete v tématu [Dat Azure Centrum rozsahy IP adres](https://www.microsoft.com/download/details.aspx?id=41653) výpočet rozsahy IP adres (včetně oblastí SQL) používané datacentrech Microsoft Azure. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z Amazon Redshift je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak chcete zkopírovat data z Amazon Redshift k úložišti objektů Blob Azure. Data můžete však zkopírována do jednotlivých propadů uvedená [v tomto poli](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Ukázka: Zkopírování dat z Amazon Redshift objektů Blob Azure
Tento příklad ukazuje, jak ke kopírování dat z databáze Amazon Redshift k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

- Propojené služba typu [AmazonRedshift](#linked-service-properties).
- Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Zadávání [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-type-properties).
- Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](#copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku zkopíruje údaje z výsledek dotazu v Amazon Redshift objektů blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

**Amazon Redshift propojené služby**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Zadávání datovou sadu Amazon Redshift**

Nastavení **"externí": PRAVDA** informoval službu Data Factory, že datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory. Nastavte tuto vlastnost na hodnotu true na vstupní sady dat, která není vytvořené pomocí aktivita v kanálu.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **RelationalSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **query** vybere data v poslední hodinu zkopírovat.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu Amazon Redshift propojené.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **AmazonRedshift**. | Ano |
| Server | IP adresa nebo název hostitele Amazon Redshift serveru. | Ano |
| port | Číslo portu TCP, který používá server Amazon Redshift poslouchat připojení klientů. | Ne, výchozí hodnota: 5439 |
| databáze | Název databáze Amazon Redshift. | Ano |
| uživatelské jméno | Jméno uživatele, kteří mají přístup k databázi.| Ano |
| heslo | Heslo k uživatelskému účtu.| Ano |


## <a name="dataset-type-properties"></a>Vlastnosti typu datové sady

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady se podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu **RelationalTable** (který obsahuje sadu Amazon Redshift) obsahuje následující vlastnosti

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Název tabulky | Název tabulky v databázi Amazon Redshift odkazující propojené služby. | Ne (Pokud není zadán **dotazu** **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Zkopírujte vlastnosti typ činnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části **typeProperties** aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Po zdrojem kopírovat aktivity typu **RelationalSource** (jejichž součástí Amazon Redshift) následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| dotaz | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: vyberte * na tabulka. | Ne (Pokud není zadán **tabulky** **datovou sadu** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Typ mapování pro Amazon Redshift

Uvedených v článku [dat pohyb aktivity](data-factory-data-movement-activities.md) kopírovat aktivity provádí automatické typ převody z typy zdrojů do jímky typů s následující postup dvoustupňového:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesunutí dat do Amazon Redshift, použije se následující mapování z Amazon Redshift typů na typy .NET.

Amazon Redshift typ | .NET na základě typ
-------------------- | ---------------
SMALLINT | Int16
CELÉ ČÍSLO | Int32
BIGINT | Int64
DECIMAL | Decimal
SKUTEČNÉ | Jeden
DVOJITOU PŘESNOSTÍ | Dvojité
LOGICKÁ HODNOTA | Řetězec
ZNAK | Řetězec
DATOVÁ | Řetězec
DATUM | Data a času
ČASOVÉ RAZÍTKO | Data a času
TEXT | Řetězec



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

## <a name="next-steps"></a>Další kroky
Naleznete v následujících článcích: 
- [Kopírovat aktivity kurz](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytváření kanálů aktivitu kopírovat. 