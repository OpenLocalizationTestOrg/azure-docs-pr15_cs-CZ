<properties 
    pageTitle="Přesuňte data z OData zdrojů | Azure Data Factory" 
    description="Informace o tom, jak přesunout data ze zdrojů OData pomocí Azure Data Factory." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Přesunutí dat z OData zdroj pomocí Azure Data Factory
Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory Pokud chcete přesunout data ze zdroje OData k úložišti jiného. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

> [AZURE.NOTE] Tuto podporu konektor OData kopírování dat z obou cloudu OData a OData zdrojů na pracovišti. K nim budete potřebovat k instalaci Brána pro správu dat. Přečtěte si téma [Přesun dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) článek podrobnosti o bráně pro správu dat.

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z OData zdroje je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z OData zdroje k úložišti objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Ukázka: Kopírování dat ze zdroje OData do objektů Blob Azure

Tento příklad ukazuje, jak chcete zkopírovat data ze zdroje OData k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [v tomto poli](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivitu v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OData](#odata-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [ODataResource](#odata-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [RelationalSource](#odata-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat z dotazování zdroj OData na objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky. 

**OData propojené služby** Tento příklad používá základní ověřování. [OData propojené služby](#odata-linked-service-properties) v části různých typů ověřování můžete použít. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**Zadávání datovou sadu OData**

Nastavení "externí": "true" informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivitu v data factory.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

Zadání **cesty** v datové sadě definice vynechán. 


**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **RelationalSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **dotazu** vybere nejnovější (nejnovější) data ze zdroje OData.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


Zadání **dotazu** v definici kanálem k odesílání zpráv je nepovinný krok. **Adresa URL** , kterou službu Data Factory používá k načtení dat je: adresa URL zadaná v propojených služby (povinné) + cesta podle datovou sadu (volitelné) + dotazu v kanálu (volitelné). 

## <a name="odata-linked-service-properties"></a>OData propojené vlastnosti služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu OData propojené.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **OData** | Ano |
| Adresa URL| Adresa URL služby OData. | Ano |
| authenticationType | Typ ověřování používá pro připojení ke zdroji OData. <br/><br/> Možné hodnoty pro cloudu OData, jsou anonymní a základní. Pro místní OData možné hodnoty jsou anonymní, Basic a Windows. | Ano | 
| uživatelské jméno | Pokud používáte základní ověřování, zadejte uživatelské jméno. | Ano (jenom v případě, že používáte základní ověřování) | 
| heslo | Zadejte heslo k uživatelskému účtu, který jste definovali pro uživatelské jméno. | Ano (jenom v případě, že používáte základní ověřování) | 
| názevbrány | Název brány, který službu Data Factory má použít pro přístup ke službě OData místní. Určete, pokud se kopírují data z místní OData zdroje. | Ne |

### <a name="using-basic-authentication"></a>Základní ověřování

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Použití anonymní přístup
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Pomocí ověřování Windows přístup k místnímu zdroji OData

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>Vlastnosti typu datovou sadu OData

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu **ODataResource** (který obsahuje sadu OData) obsahuje následující vlastnosti

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Cesta | Cesta k tomuto zdroji OData | Ne | 

## <a name="odata-copy-activity-type-properties"></a>Vlastnosti typ činnosti kopírovat OData

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy činností. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Když je zdrojem typu **RelationalSource** (jejichž součástí OData) následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Příklad | Povinné |
| -------- | ----------- | -------------- | -------- |
| dotaz | Vlastní dotaz umožňuje číst data. | "? $select = název, popis a $top = 5" | Ne | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Typ mapování pro OData

Uvedených v článku [aktivity přesun dat](data-factory-data-movement-activities.md) kopírovat aktivity provádí automatické typu Převod typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesunutí dat z datového OData ukládá, jsou namapované datové typy OData typy .NET.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

