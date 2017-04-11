<properties 
    pageTitle="Přesuňte data z MySQL | Azure Data Factory" 
    description="Informace o tom, jak přesunout data z databáze MySQL pomocí Azure Data Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Přesunutí dat z MySQL pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesuňte data z MySQL na jiný datový úložiště. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který zobrazí obecný přehled přesun dat s kopírovat aktivity a podporovaný datový úložiště kombinace.

Datové služby Factory podporuje připojení k místním zdrojům MySQL pomocí Brána pro správu dat. Najdete v článku [přesunutí dat mezi místního umístění a cloud](data-factory-move-data-between-onprem-and-cloud.md) se naučit používat Brána pro správu dat a podrobné pokyny k nastavení brány. 

> [AZURE.NOTE] Bránu se i když databáze MySQL je hostovaný ve Azure IaaS virtuálního počítače (OM). Brány můžete nainstalovat na stejné OM jako úložiště dat nebo na různých OM, dokud brány můžete připojit k databázi.  

Data factory v současné době podporuje pouze přesunutí dat z MySQL k další úložiště dat, ale ne pro přesunutí dat z jiných úložiště dat na MySQL.

## <a name="installation"></a>Instalace 
U brány pro správu dat pro připojení k databázi MySQL budete potřebovat k instalaci [MySQL spojnice/čistého 6.6.5 pro systém Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) ve stejném systému jako brána pro správu dat.

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze MySQL se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které můžete použít k vytváření kanálů. Dokončení návodu s podrobnými pokyny najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) . Data můžete zkopírovat do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Ukázka: Zkopírování dat z MySQL objektů Blob Azure
Tento příklad ukazuje, jak ke kopírování dat z databáze MySQL místní k úložišti objektů Blob Azure. Data lze však zkopírovaného **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  

> [AZURE.IMPORTANT] V tomto příkladu obsahuje fragmenty JSON. Neobsahuje podrobné pokyny k vytváření data factory. Najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny. 
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku zkopíruje údaje z výsledek dotazu v databázi MySQL objektů blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky. 

Jako první krok nastavení brány pro správu dat. Pokyny najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) . 

**MySQL propojené služby**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**Zadávání datovou sadu MySQL**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" MySQL a obsahuje sloupec s názvem "timestampcolumn" pro dat časové řady.

Nastavení "externí": "true" informuje o službu Data Factory, že v tabulce externí stránku factory dat a není vytvořené pomocí aktivitu v data factory.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
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



**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Vlastnosti MySQL propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu MySQL propojené.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **OnPremisesMySql** | Ano |
| Server | Název serveru Server MySQL. | Ano |
| databáze | Název databáze MySQL. | Ano | 
| schéma  | Název schématu databáze. | Ne | 
| authenticationType | Typ ověřování používá pro připojení k databázi MySQL. Možné hodnoty jsou: anonymní, Basic a Windows. | Ano | 
| uživatelské jméno | Pokud používáte Basic a Windows ověřování, zadejte uživatelské jméno. | Ne | 
| heslo | Zadejte heslo k uživatelskému účtu, který jste definovali pro uživatelské jméno. | Ne | 
| názevbrány | Název brány, které služba Data Factory použít pro připojení k databázi MySQL místní. | Ano |

Další informace o nastavení přihlašovacích údajů pro zdroj dat MySQL místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>Vlastnosti typu datovou sadu MySQL

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu **RelationalTable** (který obsahuje sadu MySQL) obsahuje následující vlastnosti

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Název tabulky | Název tabulky databáze MySQL instance odkazující propojené služby. | Ne (Pokud není zadán **dotazu** **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>Vlastnosti typ činnosti kopírovat MySQL

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky jsou zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části **typeProperties** aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Když je zdroj v kopírování aktivitu typu **RelationalSource** (jejichž součástí MySQL) následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| dotaz | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: vyberte * z tabulka. | Ne (Pokud není zadán **tabulky** **datovou sadu** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Typ mapování MySQL

Uvedených v článku [dat pohyb aktivity](data-factory-data-movement-activities.md) kopírovat aktivity provádí automatické typ převody z typy zdrojů do jímky typů s následující postup dvoustupňového:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesouvání dat do MySQL, následující mapování lze z typů MySQL typy .NET.

| Typ databáze MySQL | Typ .NET framework |
| ------------------- | ------------------- | 
| bigint nepodepsaná | Decimal |
| bigint | Int64 |
| Bit | Decimal |
| objektů BLOB | Byte] |
| Logická hodnota |  Logická hodnota | 
| znak | Řetězec |
| Datum | Data a času |
| data a času | Data a času |
| Decimal | Decimal |
| dvojitou přesností | Dvojité |
| dvojité | Dvojité |
| výčet | Řetězec |
| uvolnit | Jeden |
| Funkce INT nepodepsaná | Int64 |
| Funkce INT | Int32 |
| celé číslo nepodepsaná | Int64 |
| celé číslo | Int32 | 
| dlouhé seskupit | Byte] |
| dlouhé datová | Řetězec |
| longblob | Byte] |
| LONGTEXT | Řetězec | 
| mediumblob |  Byte] | 
| mediumint nepodepsaná | Int64 |
| mediumint | Int32 | 
| mediumtext | Řetězec |
| číselné | Decimal |
| skutečné |  Dvojité |
| nastavení | Řetězec |
| smallint nepodepsaná | Int32 |
| smallint | Int16 |
| text | Řetězec |
| čas | Časového rozpětí |
| časové razítko | Data a času |
| tinyblob | Byte] |
| tinyint nepodepsaná |  Int16 | 
| tinyint | Int16 |
| tinytext | Řetězec | 
| Datová | Řetězec | 
| rok | Funkce INT | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

