<properties 
    pageTitle="Přesuňte data z Cassandra pomocí Data Factory | Microsoft Azure" 
    description="Informace o tom, jak přesunout data z místního Cassandra databáze pomocí Azure Data Factory." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Přesunutí dat z databáze Cassandra místní pomocí Azure Data Factory
Tento článek popisuje, jak můžete kopírovat aktivity v Azure dat factory ke kopírování dat z databáze Cassandra místní do jakéhokoli datový úložiště zařazený do kategorie jímky sloupce v části [podporované zdroje a propadů](data-factory-data-movement-activities.md#supported-data-stores) . Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který zobrazí obecný přehled přesun dat s kopírovat aktivity a podporovaný datový úložiště kombinace.

Data factory v současné době podporuje pouze přesunutí dat z databáze Cassandra se [Podpora jímky datový úložiště](data-factory-data-movement-activities.md#supported-data-stores), ale ne přesunutí dat z jiných úložiště dat do databáze Cassandra.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Pro službu Azure Data Factory mohli připojit k místní databázi Cassandra musíte nainstalovat takto: 

- Brána pro správu dat 2.0 nebo výše ve stejném počítači, který je hostitelem databáze nebo samostatné počítače, chcete-li předejít soutěží pro zdroje s databází. Brána pro správu dat je software, která bude propojena místních zdrojů dat ke cloudovým službám zabezpečené a spravovaných způsobem. Přečtěte si téma [Přesun dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) článek podrobnosti o bráně pro správu dat.
  
    Při instalaci brány nainstaluje automaticky ovladač Microsoft Cassandra ODBC používá pro připojení k databázi Cassandra. 

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z databáze Cassandra se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z databáze Cassandra k úložišti objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Ukázka: Zkopírování dat z Cassandra objektů Blob
Vzorku slouží ke kopírování dat z databáze Cassandra k objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsané v částech sledovat vzorky. Data můžete kopírovat přímo do jednotlivých propadů je uvedeno v článku [Aktivity pohyb dat](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory. 

- Propojené služba typu [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Zadávání [datovou sadu](data-factory-create-datasets.md) typu [CassandraTable](#cassandratable-properties).
- Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [CassandraSource](#cassandrasource-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Cassandra propojené služby**

Tento příklad používá službu **Cassandra** propojené. Naleznete v části vlastnosti podporované tuto službu propojené [Cassandra propojené služby](#onpremisescassandra-linked-service-properties) .  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
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

**Zadávání datovou sadu Cassandra**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
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

Nastavení **externí** na **hodnotu true** informoval službu Data Factory, že datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Příležitostí s aktivitou kopie**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **CassandraSource** a **jímky** typ je nastavený na **BlobSink**. 

Seznam vlastností nepodporuje RelationalSource naleznete v tématu [RelationalSource typ vlastnosti](#cassandrasource-type-properties) . 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Vlastnosti OnPremisesCassandra propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu Cassandra propojené.

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- | 
| Typ | Vlastnost typu musí být nastavena na: **OnPremisesCassandra** | Ano |
| Host (hostitel) | Jeden nebo více IP adresy nebo názvy hostitelů Cassandra serverů.<br/><br/>Zadejte seznam IP adresy nebo názvy hostitelů pro připojení do všech serverů současně. | Ano | 
| port | TCP port serveru Cassandra využívající poslouchat připojení klientů. | Ne, výchozí hodnota: 9042 |
| authenticationType | Základní nebo anonymní | Ano |
| uživatelské jméno |Zadejte uživatelské jméno pro uživatelský účet. | Ano, pokud je nastaveno authenticationType Basic. |
| heslo | Zadejte heslo k uživatelskému účtu.  | Ano, pokud je nastaveno authenticationType Basic. |
| názevbrány | Název brány, která se používá pro připojení k databázi Cassandra místní. | Ano |
| encryptedCredential | Přihlašovací údaje zašifrovaných brána. | Ne | 

## <a name="cassandratable-properties"></a>Vlastnosti CassandraTable

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Azure SQL Azure objektů blob Azure table, atd.).

V části **typeProperties** je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části typeProperties pro datovou sadu typ **CassandraTable** má následující vlastnosti

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| keyspace | Název keyspace nebo schématu databáze Cassandra. | Ano (Pokud není definované **dotazu** pro **CassandraSource** ). |
| Název tabulky | Název tabulky v databázi Cassandra. | Ano (Pokud není definované **dotazu** pro **CassandraSource** ). |


## <a name="cassandrasource-type-properties"></a>Vlastnosti CassandraSource typů
Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivitu na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

Pokud zdroj typu **CassandraSource**, následující vlastnosti jsou k dispozici v části typeProperties:

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------------- | -------- |
| dotaz | Vlastní dotaz umožňuje číst data. | Dotaz SQL-92 nebo CQL dotaz. Přečtěte si článek [CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Pokud chcete použít dotaz SQL zobrazený, zadejte **keyspace name.table název** pro tabulku, kterou chcete zadat dotaz. | Ne (je-li název tabulky a keyspace v sadě dat jsou definované).  |
| consistencyLevel | Úroveň konzistence Určuje, kolik repliky musí odpovědět na žádost o přečtení před vrácení dat ke klientské aplikaci. Cassandra zkontroluje zadaný počet repliky pro data, která splňují požadavek na čtení. | JEDNA, DVĚ, TŘI KVORA, VŠECHNY LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Další informace najdete v článku [konzistence dat konfigurace](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Ne. Výchozí hodnota je taková. |  


### <a name="type-mapping-for-cassandra"></a>Typ mapování pro Cassandra
Typ Cassandra | .NET na základě typ
--------------- | ---------------
ASCII | Řetězec 
BIGINT | Int64
OBJEKTŮ BLOB | Byte]
LOGICKÁ HODNOTA | Logická hodnota
DECIMAL | Decimal
DVOJITÉ | Dvojité
PLOVOUCÍ | Jeden
INERTNÍHO | Řetězec
FUNKCE INT | Int32
TEXT | Řetězec
ČASOVÉ RAZÍTKO | Data a času
TIMEUUID | Identifikátor GUID
UUID | Identifikátor GUID
DATOVÁ | Řetězec
VARINT | Decimal

> [AZURE.NOTE]  
> Kolekce typy (mapu, nastavení, seznam, atd.), naleznete v části [práce s typy kolekce Cassandra pomocí virtuální tabulky](#work-with-collections-using-virtual-table) . 
> 
> Typy definované uživatelem nejsou podporované.
> 
> Délka řetězce a binární sloupec délky nesmí být větší než 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Práce s kolekcí pomocí virtuální tabulky
Azure Data Factory používá pro připojení k a zkopírujte data z databáze Cassandra předdefinované ovladač ODBC. Pro typy kolekcí včetně map, nastavení a seznamů ovladač znovu sjednotí data na odpovídající virtuální tabulky. Pokud tabulka obsahuje všechny sloupce kolekce, konkrétně ovladač generuje v následujících tabulkách virtuální:

-   **Základní tabulka**, která obsahuje stejná data jako skutečného stolu s výjimkou sloupců kolekce. Základní tabulka používá stejný název jako skutečného stolu, který ho představuje.
-   **Virtuální tabulku** pro všechny kolekce sloupce, který rozšíří vnořených datových. Virtuální tabulky, které představují kolekce jsou s názvem nazvanou podle této skutečného stolu, oddělovač "_vt_" a název sloupce.

Virtuální tabulky odkazují na data v skutečného stolu povolení ovladač Nenormalizovaná data. Příklad v části Podrobnosti. Mít přístup k obsahu Cassandra kolekcích dotazování a spojování virtuální tabulek.

Využijte [Průvodce kopírováním](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitivně zobrazení seznamu tabulek v databázi Cassandra včetně virtuální tabulek a zobrazte náhled dat obsažených v. Můžete také vytvářet dotazu v Průvodce kopírováním a ověřte výsledky zobrazit.

### <a name="example"></a>Příklad
Například následující "ExampleTable" je Cassandra databázovou tabulku, která obsahuje celé číslo primární klíče sloupec s názvem "pk_int" textový sloupec s názvem hodnoty, sloupce seznamu, mapy sloupce a nastavit sloupec (pojmenovali "StringSet").

pk_int | Hodnota | Seznam | Mapy |   StringSet
------ | ----- | ---- | --- | --------
1 | "ukázkovou hodnotu 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "ukázkovou hodnotu 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Ovladač vygeneruje více virtuální tabulky představující jednu tabulku. Sloupce cizího klíče v tabulce virtuální odkazovat sloupce primárního klíče v skutečného stolu a označit skutečného stolu řádek, který odpovídá řádku virtuální tabulky. 

První virtuální tabulky je základní tabulku s názvem "ExampleTable" je zobrazen v následující tabulce. Základní tabulka obsahuje stejná data jako původní databázovou tabulku s výjimkou kolekce, které jsou vynechání z této tabulky a rozbalenými odpověďmi v jiných tabulkách virtuální.

pk_int | Hodnota
------ | -----
1 | "ukázkovou hodnotu 1"
3 | "ukázkovou hodnotu 3"

V následujících tabulkách zobrazit virtuální tabulky, které renormalize dat ze seznamu, mapy a StringSet sloupců. Sloupce s názvy, které končí "_index" nebo "_key" označuje umístění dat v rámci původní seznam nebo mapa. Sloupce s názvy, které končí "_value" s daty, rozbalené z kolekce.

#### <a name="table-exampletablevtlist"></a>Tabulka "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabulka "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabulka "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | PŘIROZENÉHO LOGARITMU E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.
