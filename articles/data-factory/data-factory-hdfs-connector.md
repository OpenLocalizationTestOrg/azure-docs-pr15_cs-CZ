<properties 
    pageTitle="Přesuňte data z místního HDFS | Azure Data Factory" 
    description="Informace o tom, jak přesunout data z místního HDFS pomocí Azure Data Factory." 
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

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Přesuňte data z místního HDFS pomocí Azure Data Factory
Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory Pokud chcete přesunout data z místního HDFS k úložišti jiného. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který představuje obecné základní informace o přesun dat se kopie aktivity a podporovaný datový úložiště kombinace.

Data factory v současné době podporuje pouze přesunutí dat z místních HDFS další úložiště dat, ale ne pro přesunutí dat z jiných úložiště dat na místní HDFS.


## <a name="enabling-connectivity"></a>Povolení připojení
Datové služby Factory podporuje připojení k místní HDFS Brána pro správu dat pomocí. Najdete v článku [přesunutí dat mezi místního umístění a cloud](data-factory-move-data-between-onprem-and-cloud.md) se naučit používat Brána pro správu dat a podrobné pokyny k nastavení brány. Připojení k HDFS, i když je hostovaný ve OM IaaS Azure pomocí brány. 

Během instalace brány na stejném počítači místní nebo OM Azure jako HDFS, doporučujeme nainstalovat bránu na samostatném počítače/Azure IaaS OM. S brány na samostatném počítač snižuje konflikty prostředků a zlepšuje výkon. Při instalaci brány na samostatném počítač počítač soubory byste měli přístup k stroje s HDFS. 


## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanálu, který slouží ke kopírování dat z místních HDFS je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Znázorňují kopírování dat z místních HDFS k úložišti objektů Blob Azure. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Ukázka: Kopírovat data z místního HDFS objektů Blob Azure

Tento příklad ukazuje, jak chcete zkopírovat data z místního HDFS k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [sdílené položce](#hdfs-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [FileSystemSource](#hdfs-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat z místního HDFS k objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

Jako první krok nastavení brány pro správu dat. Pokyny v článku [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS propojené služby** Tento příklad používá ověřování systému Windows. Naleznete v části různých typů ověřování můžete použít [HDFS propojené služby](#hdfs-linked-service-properties) . 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS při zadávání datové sady** Tento datovou sadu odkazuje na složku HDFS DataTransfer/UnitTest /. Kanálu slouží ke kopírování všechny soubory v této složce do cíle. 

Nastavení "externí": "true" informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **FileSystemSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **query** vybere data v poslední hodinu zkopírovat.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Vlastnosti HDFS propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro HDFS propojená služby.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **Hdfs** | Ano | 
| Adresa URL | Adresa URL HDFS | Ano |
| encryptedCredential | [Nový AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup pověření přístup. | Ne |
| uživatelské jméno | Ověřování uživatelské jméno pro Windows. | Ano (ověřování systému Windows)
| heslo | Heslo pro ověřování systému Windows. | Ano (ověřování systému Windows)
| authenticationType | Systém Windows, nebo anonymní. | Ano |
| názevbrány | Název brány, které služba Data Factory použít pro připojení k HDFS. | Ano |   

Další informace o nastavení přihlašovacích údajů pro místní HDFS najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Použití anonymní přístup

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Pomocí ověřování Windows
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Vlastnosti typu datovou sadu HDFS

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady datovou sadu JSON podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. Oddíl typeProperties pro datovou sadu typu **sdílené položce** (který obsahuje sadu HDFS) obsahuje následující vlastnosti

Vlastnost | Popis | Povinné
-------- | ----------- | --------
cesta_ke_složce | Cestu ke složce. Příklad:`myfolder`<br/><br/>Používejte řídicí znak "\" pro speciální znaky v řetězci. Příklad: folder\subfolder, zadejte složku\\\\podsložky a d:\samplefolder, zadejte d:\\\\Příklad_složky.<br/><br/>Tato vlastnost můžete kombinovat s **partitionBy** mít složku cesty založené na výseč začátek/konec data a času. | Ano
Název souboru | Zadejte název souboru **cesta_ke_složce** Pokud tabulka by měla odkázat na konkrétní soubor ve složce. Pokud nezadáte každá hodnota parametru tato vlastnost tabulky odkazuje na všechny soubory ve složce.<br/><br/>Při zadání hodnoty název souboru není pro datovou sadu výstup název souboru vygenerovaných by měl být následující v tomto formátu: <br/><br/>Data. <Guid>txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Ne
partitionedBy | partitionedBy mohou sloužit k určení dynamické cesta_ke_složce název dat časové řady. Příklad: cesta_ke_složce s parametry pro každou hodinu data. | Ne
fileFilter | Určení filtru k slouží k výběru podmnožinu soubory v cesta_ke_složce, nikoli všechny soubory. <br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (znak).<br/><br/>Příklady 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Poznámka**: fileFilter platí pro zadávání datovou sadu sdílené položce | Ne
| Formát | Následující typy formátování podporované: **Formát textu** **AvroFormat**, **JsonFormat**, **OrcFormat**a **ParquetFormat**. Nastavte vlastnosti **Typ** v části formát na jednu z těchto hodnot. Naleznete v částech [Určující formát textu](#specifying-textformat), [Zadání AvroFormat](#specifying-avroformat), [Zadání JsonFormat](#specifying-jsonformat), [Určující OrcFormat](#specifying-orcformat)a [Určení ParquetFormat](#specifying-parquetformat) podrobnosti. Pokud chcete zkopírovat soubory jako-je mezi založené na souboru ukládá (binární kopii), můžete přejít v části formát v obou definice vstupní a výstupní datovou sadu. | Ne 
| komprese | Určete typ a stupeň komprese pro data. Jsou podporované typy: **GZip** **Deflate**a **BZip2** a podporované úrovně jsou: **Optimal** a **nejrychlejší**. Nastavení komprese nejsou v současné době podporované pro **AvroFormat** nebo **OrcFormat**daty. Další informace najdete v části [podporu komprese](#compression-support) .  | Ne |



> [AZURE.NOTE] Název souboru a fileFilter nejdou současně.


### <a name="using-partionedby-property"></a>Pomocí partionedBy vlastnosti

Jak je uvedeno v předchozí části, můžete určit dynamické cesta_ke_složce dat časové řady s partitionedBy název souboru. Lze provést pomocí Data Factory maker a systém proměnnou SliceStart, SliceEnd, které označují logické časové období pro dané výseč. 

Další informace o datových sad časové řady, plánování a výseče, najdete v tématu [Vytvoření datové sady](data-factory-create-datasets.md) [plánování a provádění](data-factory-scheduling-and-execution.md)a [Vytváření potrubí](data-factory-create-pipelines.md) články. 

#### <a name="sample-1"></a>Příklad 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

V tomto příkladu {výseč} nahrazuje s hodnotou Data Factory systém proměnné SliceStart ve formátu (YYYYMMDDHH). SliceStart odkazuje na začátek výseče. Cesta_ke_složce se liší pro každou výseč. Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Příklad 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

V tomto příkladu rok, měsíc, den a čas SliceStart extrahování do samostatných proměnných, které se používají vlastnostmi cesta_ke_složce a název souboru.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Vlastnosti typ HDFS kopírovat činnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Kopírovat aktivity po zdroj typu **FileSystemSource** následující vlastnosti jsou k dispozici v části typeProperties:

**FileSystemSource** podporuje následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| rekurzivní | Označuje, zda je dat přečíst zpětně ze složek sub nebo jenom ze zadané složky. | PRAVDA, NEPRAVDA (výchozí)| Ne |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

