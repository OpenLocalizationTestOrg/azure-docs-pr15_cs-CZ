<properties 
    pageTitle="Přesuňte data z Sybase | Azure Data Factory" 
    description="Informace o tom, jak přesunout data z databáze Sybase pomocí Azure Data Factory." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Přesuňte data z Sybase pomocí Azure Data Factory 

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory Pokud chcete přesunout data z Sybase k úložišti jiného. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

Datové služby Factory podporuje připojení k Sybase zdrojů na pracovišti Brána pro správu dat pomocí. Najdete v článku [přesunutí dat mezi místního umístění a cloud](data-factory-move-data-between-onprem-and-cloud.md) se naučit používat Brána pro správu dat a podrobné pokyny k nastavení brány. 

> [AZURE.NOTE]
> Bránu se i když databáze Sybase je hostovaný ve OM IaaS Azure. Brány můžete nainstalovat na stejné OM IaaS jako úložiště dat nebo na různých OM, dokud brány můžete připojit k databázi. 

Data factory v současné době podporuje pouze přesunutí dat z Sybase další úložiště dat a ne z jiných úložiště dat k Sybase.

## <a name="installation"></a>Instalace

U brány pro správu dat pro připojení k databázi Sybase budete potřebovat k instalaci [Zprostředkovatel dat pro Sybase](http://go.microsoft.com/fwlink/?linkid=324846) ve stejném systému jako brána pro správu dat.

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze Sybase se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Znázorňují kopírování dat z databáze Sybase k úložišti objektů Blob Azure. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Ukázka: Zkopírování dat z Sybase objektů Blob Azure
Tento příklad ukazuje, jak ke kopírování dat z databáze Sybase k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [v tomto poli](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivitu v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Liked služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku zkopíruje údaje z výsledek dotazu v databázi Sybase objektů blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky. 

Jako první krok nastavení brány pro správu dat. Pokyny najdete v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) .

**Sybase propojené služby:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sada Sybase zadávání dat:**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" Sybase a obsahuje sloupec s názvem "časové razítko" pro dat časové řady.

Nastavení "externí": PRAVDA informuje o tom, službu Data Factory, že tento datovou sadu externí stránku factory dat a není vytvořené pomocí aktivity v data factory. Všimněte si, že **typu** propojené služby je nastavený na: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **RelationalSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **dotazu** slouží k výběru data ze správce databáze. Objednávky pro tabulky v databázi.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Vlastnosti Sybase propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu Sybase propojené.

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Typ | Vlastnost typu musí být nastavena na: **OnPremisesSybase** | Ano
Server | Název serveru Sybase. | Ano
databáze | Název databáze Sybase. | Ano 
schéma  | Název schématu databáze. | Ne
authenticationType | Typ ověřování používá pro připojení k databázi Sybase. Možné hodnoty jsou: anonymní, Basic a Windows. | Ano
uživatelské jméno | Pokud používáte Basic a Windows ověřování, zadejte uživatelské jméno. | Ne
heslo | Zadejte heslo k uživatelskému účtu, který jste definovali pro uživatelské jméno. |  Ne
názevbrány | Název brány, které služba Data Factory použít pro připojení k databázi Sybase místní. | Ano 

Další informace o nastavení přihlašovacích údajů pro zdroj dat Sybase místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Vlastnosti typu datovou sadu Sybase

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části **typeProperties** pro datovou sadu typ **RelationalTable** (který obsahuje sadu Sybase) obsahuje následující vlastnosti:

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Název tabulky | Název tabulky databáze Sybase instance odkazující propojené služby. | Ne (Pokud není zadán **dotazu** **RelationalSource** )

## <a name="sybase-copy-activity-type-properties"></a>Vlastnosti typ činnosti kopírovat Sybase 

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity, přečtěte si téma [Vytvoření potrubí](data-factory-create-pipelines.md) článek. Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy činností. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Když je zdrojem typu **RelationalSource** (jejichž součástí Sybase) následující vlastnosti jsou k dispozici v části **typeProperties** :

Vlastnost | Popis | Povolené hodnoty | Povinné
-------- | ----------- | -------------- | --------
dotaz | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: vyberte * z tabulka. | Ne (Pokud není zadán **tabulky** **datovou sadu** )

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Typ mapování Sybase

Uvedených v článku [Dat pohyb aktivity](data-factory-data-movement-activities.md) aktivitu kopírovat provádí automatické typ převody z typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Sybase podporuje T-SQL a typy T-SQL. Mapování tabulku z sql typy .NET typ, najdete v článku [Spojnice SQL Azure](data-factory-azure-sql-connector.md) článek.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.