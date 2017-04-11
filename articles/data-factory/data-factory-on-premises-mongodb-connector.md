<properties 
    pageTitle="Přesuňte data z MongoDB pomocí Data Factory | Microsoft Azure" 
    description="Informace o tom, jak přesunout data z databáze MongoDB pomocí Azure Data Factory." 
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
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Přesunutí dat z MongoDB pomocí Azure Data Factory

Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory přejděte dat z databáze MongoDB v místním úložišti jiného. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivity a kombinace úložiště zdroj/jímky dat, které podporuje aktivity kopírovat.

Datové služby Factory podporuje připojení k místním zdrojům MongoDB pomocí Brána pro správu dat. [Brána pro správu dat](data-factory-data-management-gateway.md) článku se dozvíte o bráně pro správu dat a [přesuňte data z místního do cloudu](data-factory-move-data-between-onprem-and-cloud.md) v článku podrobné pokyny najdete na nastavení brány datový kanál chcete přesunout data. 

> [AZURE.NOTE] Budete muset používat bránu pro připojení k MongoDB i v případě, že je hostovaný v Azure IaaS VMs. Pokud se pokoušíte se připojit k instanci MongoDB hostované v cloudu, můžete taky nainstalovat instance brány v OM IaaS.

Data Factory v současné době podporuje pouze přesunutí dat z MongoDB k další úložiště dat, ale ne pro přesunutí dat z jiných úložiště dat na MongoDB.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Pro službu Azure Data Factory mohli připojit k místní databázi MongoDB musíte nainstalovat tyto prvky: 

- Brána pro správu dat 2.0 nebo výše ve stejném počítači, který je hostitelem databáze nebo samostatné počítače, chcete-li předejít soutěží pro zdroje s databází. Brána pro správu dat je software, která bude propojena místních zdrojů dat ke cloudovým službám zabezpečené a spravovaných způsobem. Viz článek [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o bráně pro správu dat.
  
    Při instalaci brány nainstaluje automaticky ovladač Microsoft MongoDB ODBC používá pro připojení k MongoDB. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze MongoDB se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z databáze MongoDB k úložišti objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>Ukázka: Zkopírování dat z MongoDB objektů Blob Azure
Tento příklad ukazuje, jak ke kopírování dat z databáze MongoDB místní k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesMongoDb](#linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [MongoDbCollection](#dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [MongoDbSource](#copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku zkopíruje údaje z výsledek dotazu v databázi MongoDB objektů blob každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

Jako první krok nastavení brány pro správu dat podle pokynů v článku [Brána pro správu dat](data-factory-data-management-gateway.md) . 

**MongoDB propojené služby**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
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

**Zadávání datovou sadu MongoDB** Nastavení "externí": "true" informuje o službu Data Factory, že v tabulce externí stránku factory dat a není vytvořené pomocí aktivity v data factory.
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
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
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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

Kanálu obsahuje aktivitu kopírovat, který je nakonfigurovaný na použití výše uvedených vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **MongoDbSource** a **jímky** typ je nastavený na **BlobSink**. Dotaz SQL zobrazený určeným pro vlastnost **query** vybere data v poslední hodinu zkopírovat.
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
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
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu **OnPremisesMongoDB** propojené.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **OnPremisesMongoDb** | Ano |
| Server | IP adresa nebo název hostitele MongoDB serveru. | Ano | 
| port | TCP portu, která používá MongoDB server poslouchat připojení klientů. | Volitelné, výchozí hodnota: 27017 |
| authenticationType | Základní nebo anonymní. | Ano | 
| uživatelské jméno | Uživatelský účet pro přístup k MongoDB. | Ano (je-li základní ověřování používá).|
| heslo | Heslo pro uživatele. |   Ano (je-li základní ověřování používá). | 
| authSource | Název MongoDB databáze, kterou chcete použít ke kontrole svoje přihlašovací údaje pro ověřování. | Volitelné (Pokud je použito základní ověřování). Výchozí: pomocí účtu správce a zadaná databáze pomocí vlastnosti název databáze. |  
| Název databáze | Název databáze MongoDB, která chcete mít přístup. | Ano |
| názevbrány | Název brány, který má přístup k úložišti. | Ano | 
| encryptedCredential | Přihlašovací údaje zašifrovaných brána. | Volitelné |


Další informace o nastavení přihlašovacích údajů pro zdroj dat MongoDB místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="dataset-type-properties"></a>Vlastnosti typu datové sady

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Oddíly například strukturu, dostupnost a zásady datovou sadu JSON podobají pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části typeProperties pro datovou sadu typ **MongoDbCollection** obsahuje následující vlastnosti:

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Název_kolekce | Název kolekce MongoDB databáze. | Ano | 

## <a name="copy-activity-type-properties"></a>Zkopírujte vlastnosti typ činnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části **typeProperties** aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Pokud je zdroj typu **MongoDbSource** následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| dotaz | Vlastní dotaz umožňuje číst data. | Dotaz SQL-92 řetězec. Příklad: vyberte * na tabulka. | Ne (Pokud není zadán **Název_kolekce** **datovou sadu** ) | 

## <a name="schema-by-data-factory"></a>Schéma tak, že Data Factory
Služby Azure Data Factory odvozeny schématu z kolekce MongoDB pomocí nejnovějších 100 dokumentů v kolekci. Pokud tyto 100 dokumenty neobsahují úplné schéma, může být některé sloupce ignorován během operace kopírování. 

## <a name="type-mapping-for-mongodb"></a>Typ mapování pro MongoDB

Uvedených v článku [aktivity přesun dat](data-factory-data-movement-activities.md) kopírovat aktivity provádí automatické typu Převod typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesouvání dat do MongoDB následující mapování lze z MongoDB typů typy .NET.

| Typ MongoDB | Typ .NET framework |
| ------------------- | ------------------- | 
| Binární | Byte] |
| Logická hodnota | Logická hodnota |
| Datum | Data a času |
| NumberDouble | Dvojité |
| NumberInt | Int32 |
| NumberLong | Int64 |
| Objektu | Řetězec |
| Řetězec | Řetězec |
| UUID | Identifikátor GUID | 
| Objekt | Renormalized do sloučit sloupce s "_" jako vnořené oddělovače |

> [AZURE.NOTE]  
> Informace o podpoře polí pomocí virtuálních tabulkách, najdete pod odkazy [podpory složité typů pomocí virtuálních tabulkách](#support-for-complex-types-using-virtual-tables) oddíl. 

V současné době nejsou podporovány následující datové typy MongoDB: DBPointer, JavaScript, maximum nebo minimum klávesu regulárních výrazů, Symbol, razítka, Nedefinováno

## <a name="support-for-complex-types-using-virtual-tables"></a>Podpora pro komplexní typy pomocí virtuální tabulek
Azure Data Factory používá pro připojení k a zkopírujte data z databáze MongoDB předdefinované ovladač ODBC. Pro komplexní typy například matice nebo objekty s různými typy ve dokumentech ovladač znovu sjednotí dat na odpovídající virtuální tabulky. Pokud tabulka obsahuje tyto sloupce, konkrétně ovladač generuje v následujících tabulkách virtuální:

-   **Základní tabulka**, která obsahuje stejná data jako skutečného stolu s výjimkou sloupců složité typu. Základní tabulka používá stejný název jako skutečného stolu, který ho představuje.
-   **Virtuální tabulku** pro všechny sloupce komplexní typ, který rozšíří vnořených datových. Virtuální tabulky mají název nazvanou podle této skutečného stolu a oddělovač "_" název pole nebo objektu.

Virtuální tabulky odkazují na data v skutečného stolu povolení ovladač Nenormalizovaná data. Příklad v části pod podrobnosti. Přístup k obsahu MongoDB matic dotazování a spojování virtuální tabulek.

Pomocí [Průvodce kopírováním](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitivně zobrazení seznamu tabulek v databázi MongoDB včetně virtuální tabulek a zobrazte náhled dat obsažených v. Můžete také vytvářet dotazu v Průvodce kopírováním a ověřte výsledky zobrazit.

### <a name="example"></a>Příklad

"ExampleTable" pod je například MongoDB tabulky obsahující jeden sloupec s polem objektů v každé buňce – faktury a jeden sloupec s polem skalární typů – hodnocení. 

_id | Jméno zákazníka | Faktury | Úrovně služeb | Hodnocení
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{invoice_id: "123", položky: "toaster", cena: "456", diskont_sazba: "0,2"}, {invoice_id: "124", položky: "sušárny", cena: "1235" slevy: "0,2"}] | Slovo stříbro | [5,6]
2222 | XYZ | [{invoice_id: "135", položky: "ledničky", cena: "12543", diskont_sazba: "0,0"}] | Gold | [1, 2]

Ovladač vygeneruje více virtuální tabulky představující jednu tabulku. Základní tabulku s názvem "ExampleTable" ukázáno v následujícím příkladu je první virtuální tabulka. Základní tabulka obsahuje všechna data z původní tabulky, ale data z pole byly vynechány a je rozbalenými odpověďmi v virtuálních tabulkách.

_id | Jméno zákazníka | Úrovně služeb
--- | ------------- | -------------
1111 | ABC | Slovo stříbro
2222 | XYZ | Gold

V následujících tabulkách zobrazit virtuální tabulky, které představují původní matice v příkladu. V této tabulce obsahují následující: 

- Odkaz zpátky na původní sloupce primárního klíče odpovídající na řádek pole původního (přes sloupec _id)
- Označení umístění dat v rámci původní určené 
- Rozbalené data pro každý prvek v matici

Tabulka "ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | položky | Price | Sleva
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | Toaster | 456 | 0,2
1111 | 1 | 124 | sušárny | 1235 | 0,2
2222 | 0 | 135 | ledničky | 12543 | 0,0

Tabulka "ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

## <a name="next-steps"></a>Další kroky
Najdete v tématu [přesouvání dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k vytváření datového kanálu přesune data z místního datový úložiště Azure úložiště. 

