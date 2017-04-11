<properties 
    pageTitle="Přesuňte data z DB2 | Azure Data Factory" 
    description="Informace o přesunutí dat z databáze DB2 pomocí Azure Data Factory" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Přesunutí dat z DB2 pomocí Azure Data Factory
Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesuňte data z DB2 na jiný datový úložiště. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

Data factory podporuje připojení k místním zdrojům DB2 pomocí Brána pro správu dat. Najdete v článku [přesunutí dat mezi místního umístění a cloud](data-factory-move-data-between-onprem-and-cloud.md) se naučit používat Brána pro správu dat a podrobné pokyny k nastavení brány. 

> [AZURE.NOTE]
Připojení k DB2, i když je hostovaný v Azure IaaS VMs pomocí brány. Pokud se pokoušíte se připojit k instanci DB2 hostované v cloudu, můžete taky nainstalujete instance brány v OM IaaS.

Data factory v současné době podporuje pouze přesunutí dat z DB2 další úložiště dat a ne z úložiště dalších dat do DB2. 

## <a name="installation"></a>Instalace 

Brána pro správu dat nabízí předdefinované DB2 ovladač, který podporuje následující: 

- SQLAM 9 / 10 NEBO 11
- DB2 LUW (Linux, Unix Windows)
- DB2 z/OS a DB2 pro můžu (označovaná taky jako jako / 400)

Proto už nepotřebujete ruční instalace ovladačů při kopírování dat z DB2.

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze DB2 je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z databáze DB2 a úložišti objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Ukázka: Zkopírování dat z DB2 objektů Blob Azure

Tento příklad ukazuje, jak ke kopírování dat z databáze DB2 místní k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Vzorku zkopíruje údaje z výsledek dotazu v databázi DB2 objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

Jako první krok instalace a konfigurace brány pro správu dat. Pokyny najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) .

**DB2 propojené služby:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Úložiště objektů Blob Azure propojené služby:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**Zadávání sadu DB2:**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" DB2 a obsahuje sloupec s názvem "časové razítko" pro dat časové řady.

Nastavení "externí": PRAVDA informuje o tom, službu Data Factory, že tento datovou sadu externí stránku factory dat a není vytvořené pomocí aktivity v data factory. Všimněte si, jestli **Typ** je nastavený na **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Objektů Blob Azure výstup datovou sadu:**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **RelationalSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **query** slouží k výběru data z tabulky objednávky.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Vlastnosti DB2 propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu DB2 propojené. 

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **OnPremisesDB2** | Ano |
| Server | Název serveru DB2. | Ano |
| databáze | Název databáze DB2. | Ano |
| schéma | Název schématu databáze. Název schématu rozlišuje malá a velká písmena. | Ne |
| authenticationType | Typ ověřování používá pro připojení k databázi DB2. Možné hodnoty jsou: anonymní, Basic a Windows. | Ano |
| uživatelské jméno | Pokud používáte Basic a Windows ověřování, zadejte uživatelské jméno. | Ne |
| heslo | Zadejte heslo k uživatelskému účtu, který jste definovali pro uživatelské jméno. | Ne |
| názevbrány | Název brány, které služba Data Factory použít pro připojení k místní databázi DB2. | Ano |

Další informace o nastavení přihlašovacích údajů pro zdroj dat DB2 místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>Vlastnosti typu datovou sadu DB2

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady datovou sadu JSON podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu RelationalTable (který obsahuje sadu DB2) obsahuje následující vlastnosti.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Název tabulky | Název tabulky v databázi DB2 instanci odkazující propojené služby. Název tabulky rozlišuje malá a velká písmena. | Ne (Pokud není zadán **dotazu** **RelationalSource** ) |

## <a name="db2-copy-activity-type-properties"></a>Vlastnosti typ činnosti kopírovat DB2

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Kopírovat aktivity, pokud zdroj typu **RelationalSource** (jejichž součástí DB2) následující vlastnosti jsou k dispozici v části typeProperties:


| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------- | -------------- |
| dotaz | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: `"query": "select * from "MySchema"."MyTable""`. | Ne (Pokud není zadán **tabulky** **datovou sadu** )|

> [AZURE.NOTE] Schéma a tabulky názvy rozlišují malá a velká písmena. Uzavření názvů v "" (dvojité uvozovky) v dotazu.  

**Příklad:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Typ mapování pro DB2
Jak je uvedeno v následujícím článku [aktivity přesun dat](data-factory-data-movement-activities.md) , provede aktivity kopírovat převody typu Automatické z typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesouvání dat do DB2, následující mapování lze z DB2 typu typ .NET.

Typ DB2 databáze | Typ .NET framework 
----------------- | ------------------- 
SmallInt | Int16
Celé číslo | Int32
BigInt | Int64
Skutečné | Jeden
Dvojité | Dvojité
Plovoucí | Dvojité
Decimal | Decimal
DecimalFloat | Decimal
Číselné | Decimal
Datum | Data a času
Čas | Časového rozpětí
Časové razítko | Data a času
XML | Byte]
Znak | Řetězec
Datová | Řetězec
LongVarChar | Řetězec
DB2DynArray | Řetězec
Binární | Byte]
Seskupit | Byte]
LongVarBinary | Byte]
Obrázek | Řetězec
VarGraphic | Řetězec
LongVarGraphic | Řetězec
Datový typ CLOB | Řetězec
Objektů BLOB | Byte]
DbClob | Řetězec
SmallInt | Int16
Celé číslo | Int32
BigInt | Int64
Reálnou | Jeden
Dvojité | Dvojité
Plovoucí | Dvojité
Decimal | Decimal
DecimalFloat | Decimal
Číselné | Decimal
Datum | Data a času
Čas | Časového rozpětí
Časové razítko | Data a času
XML | Byte]
Znak | Řetězec


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.


