<properties 
    pageTitle="Přesuňte data z Teradata | Azure Data Factory" 
    description="Další informace o Teradata Connector pro službu Data Factory, která umožňuje přesuňte data z databáze Teradata" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Přesuňte data z Teradata pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přesuňte data z Teradata na jiný datový úložiště. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

Data factory podporuje připojení k místní zdroje Teradata prostřednictvím Brána pro správu dat. Najdete v článku [přesunutí dat mezi místního umístění a cloud](data-factory-move-data-between-onprem-and-cloud.md) se naučit používat Brána pro správu dat a podrobné pokyny k nastavení brány. 

> [AZURE.NOTE]
Bránu se i když Teradata je hostovaný ve OM IaaS Azure. Brány můžete nainstalovat na stejné OM IaaS jako úložiště dat nebo na různých OM, dokud brány můžete připojit k databázi. 

Data factory podporuje pouze přesunutí dat z Teradata další úložiště dat a ne z úložiště dalších dat do Teradata.

## <a name="installation"></a>Instalace 

U brány pro správu dat pro připojení k databázi Teradata budete potřebovat k instalaci [Zprostředkovatel dat .NET pro Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) ve stejném systému jako brána pro správu dat.

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z Teradata se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z Teradata k úložišti objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Ukázka: Zkopírování dat z Teradata objektů Blob Azure

Tento příklad ukazuje, jak ke kopírování dat z databáze Teradata k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku zkopíruje údaje z výsledek dotazu v databázi Teradata objektů blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

Jako první krok nastavení brány pro správu dat. Pokyny najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) .

**Teradata propojené služby:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
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


**Teradata vstupní datovou sadu:**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" Teradata a obsahuje sloupec s názvem "časové razítko" pro dat časové řady.

Nastavení "externí": PRAVDA informuje o tom, službu Data Factory, že v tabulce externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
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


**Objektů Blob Azure výstup datovou sadu:**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **RelationalSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **query** vybere data v poslední hodinu zkopírovat.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Vlastnosti Teradata propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu Teradata propojené. 

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Typ | Vlastnost typu musí být nastavena na: **OnPremisesTeradata** | Ano
Server | Název serveru Server Teradata. | Ano
authenticationType | Typ ověřování používá pro připojení k databázi Teradata. Možné hodnoty jsou: anonymní, Basic a Windows. | Ano
uživatelské jméno | Pokud používáte Basic a Windows ověřování, zadejte uživatelské jméno. | Ne 
heslo | Zadejte heslo k uživatelskému účtu, který jste definovali pro uživatelské jméno. | Ne 
názevbrány | Název brány, které služba Data Factory použít pro připojení k databázi Teradata místní. | Ano

Další informace o nastavení přihlašovacích údajů pro zdroj dat Teradata místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Vlastnosti typu datovou sadu Teradata

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady datovou sadu JSON podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V současné době nejsou žádné vlastnosti typ podporované pro datovou sadu Teradata. 


## <a name="teradata-copy-activity-type-properties"></a>Vlastnosti typ činnosti kopírovat Teradata

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Když je zdrojem typu **RelationalSource** (jejichž součástí Teradata) následující vlastnosti jsou k dispozici v části **typeProperties** :

Vlastnost | Popis | Povolené hodnoty | Povinné
-------- | ----------- | -------------- | --------
dotaz | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: vyberte * na tabulka. | Ano

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Typ mapování pro Teradata

Jak je uvedeno v následujícím článku [aktivity přesun dat](data-factory-data-movement-activities.md) , provede aktivity kopírovat převody typu Automatické z typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesouvání dat do Teradata, následující mapování lze z Teradata typu typ .NET.

Typ databáze Teradata | Typ .NET framework
----------------- | ---------------------------
Znak | Řetězec
Datový typ CLOB | Řetězec
Obrázek | Řetězec
Datová | Řetězec
VarGraphic | Řetězec
Objektů BLOB | Byte]
Bajt | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Decimal | Decimal
Dvojité | Dvojité
Celé číslo | Int32
Číslo | Dvojité
SmallInt | Int16
Datum | Data a času
Čas | Časového rozpětí
Dobu časové pásmo | Řetězec
Časové razítko | Data a času
Časové razítko s časové pásmo | DateTimeOffset
Interval den | Časového rozpětí
Interval dne na hodinu | Časového rozpětí
Interval den minuty. | Časového rozpětí
Interval den sekundu | Časového rozpětí
Interval hodinu | Časového rozpětí
Interval hodiny, minuty. | Časového rozpětí
Interval hodinu sekundu | Časového rozpětí
Interval minutu | Časového rozpětí
Interval minuty, sekundy. | Časového rozpětí
Interval druhé | Časového rozpětí
Interval rok | Řetězec
Interval rok, měsíc | Řetězec
Interval měsíc | Řetězec
Period(Date) | Řetězec
Period(Time) | Řetězec
Období (dobu časové pásmo) | Řetězec
Period(Timestamp) | Řetězec
Období (časového razítka s časové pásmo) | Řetězec
XML | Řetězec

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.