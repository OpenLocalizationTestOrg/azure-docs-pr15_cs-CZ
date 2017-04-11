<properties
    pageTitle="Připojením DocumentDB pomocí vyhledávání Azure pomocí indexování | Microsoft Azure"
    description="Tento článek popisuje, jak pomocí indexování hledání Azure s DocumentDB jako zdroj dat."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Připojením DocumentDB pomocí vyhledávání Azure pomocí indexování

Pokud se chcete dozvědět implementovat prostředí skvělé hledání nad daty DocumentDB, díky indexování vyhledávání Azure pro DocumentDB. V tomto článku si ukážeme jak integrovat Azure DocumentDB pomocí funkce hledání Azure bez nutnosti napsat jakýkoli kód, abyste zachovali indexování infrastruktury!

Toto nastavení je nutné [nastavení účet Azure hledání](../search/search-create-service-portal.md) (nemusíte upgradovat na standardní hledání) a potom volání [Rozhraní REST API pro hledání Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx) k vytvoření DocumentDB **zdroje dat** a **indexování** pro tento zdroj dat..

V žádosti o Odeslat objednávku chcete provést interakci s rozhraní REST API můžete použít [pošťák](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)nebo jiný nástroj své preference.


##<a id="Concepts"></a>Azure koncepty indexování hledání

Vytváření a správa dat zdrojů (včetně DocumentDB) Azure podporuje vyhledávání a indexování, které pracují proti těmto zdrojům dat.

**Zdroj dat** Určuje, jaká data musí být indexované, pověření pro přístup k datům a zásady pro povolení Azure hledání efektivní identifikovat změny v okně data (například změnili nebo odstranili dokumenty uvnitř vaší kolekce). Zdroj dat je definována jako zdroj nezávislé tak, aby ji může používat víc indexování.

**Indexování** popisuje, jak přetékat data ze zdroje dat do indexu vyhledávání cílový. Je třeba naplánovat týkající se vytváření jeden indexování pro každou kombinaci cílové indexu a datové zdroje. I když můžete mít víc indexování psaní do stejné index, indexování můžete jenom psát do jednoho index. Indexování slouží:

- Provedení jednorázové kopii dat k naplnění rejstříku.
- Synchronizujte indexu se změnami ve zdroji dat podle plánu. Plán je součástí definici indexování.
- Vyvolání aktualizace na vyžádání index podle potřeby.

##<a id="CreateDataSource"></a>Krok 1: Vytvoření zdroje dat

Zadejte žádost HTTP POST vytvořit nový zdroj dat v Azure vyhledávací služby, včetně následujících žádost o záhlaví.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`api-version` Požaduje. Platné hodnoty jsou `2015-02-28` nebo jeho novější verzí. Navštivte [verzí rozhraní API v Azure hledání](../search/search-api-versions.md) zobrazíte všechny podporované verze rozhraní API pro hledání.

Textu žádosti o obsahuje definice zdroje dat, která by měl obsahovat následující pole:

- **název**: Zvolte kterýkoli název představující DocumentDB databáze.

- **Typ**: použití `documentdb`.

- **přihlašovací údaje**:

    - **connectionString**: povinné. Zadejte informace o připojení k databázi Azure DocumentDB ve formátu následující:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **kontejner**:

    - **název**: povinné. Zadejte id kolekci DocumentDB chcete indexovat.

    - **dotaz**: nepovinné. Můžete zadat dotaz: Pokud chcete sloučit libovolný dokument JSON do ploché schéma, které můžete index vyhledávání Azure.

- **dataChangeDetectionPolicy**: nepovinné. V části [dat můžete změnit zjišťování zásad](#DataChangeDetectionPolicy) dole.

- **dataDeletionDetectionPolicy**: nepovinné. V tématu [odstranění duplicit údajů](#DataDeletionDetectionPolicy) dole.

[Příklad textu žádosti o](#CreateDataSourceExample)najdete níže.

###<a id="DataChangeDetectionPolicy"></a>Zachycení změněné dokumenty

Má zjišťování zásad pro změnu dat účel efektivní určete položky s změněná data. V současné době podporované zásada pouze se `High Water Mark` zásady použití `_ts` datum poslední změny časového razítka vlastnost poskytovanou DocumentDB - která je určena následujícím způsobem:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Bude potřeba přidat `_ts` v promítání a `WHERE` klauzuli dotazu. Příklad:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Zachycení odstraněných dokumentů

Při odstranění řádků ze zdrojové tabulky, byste měli odstranit tyto řádky z indexu vyhledávání stejně. Má zjišťování zásad pro odstranění dat účel efektivní určete položky odstraněné data. V současnosti je jediný podporovaný zásad `Soft Delete` zásad (odstranění s označením příznaku určitý druh), která je určena následujícím způsobem:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Je třeba zahrnout vlastnost softDeleteColumnName v klauzuli SELECT, pokud používáte vlastní projekci.

###<a id="CreateDataSourceExample"></a>Příklad textu žádosti o

Následující příklad vytvoří zdroj dat pomocí vlastního dotazu a zásady tipy:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Odpověď

Pokud úspěšně vytvořili zdroje dat získáte odpověď HTTP 201 vytvořili.

##<a id="CreateIndex"></a>Krok 2: Vytvoření indexu

Vytvoření indexu založeného na cílové Azure vyhledávání, pokud už nemáte. Můžete to provést z [Portálu Azure uživatelské rozhraní](../search/search-create-index-portal.md) nebo pomocí rozhraní [API vytvořit Index](https://msdn.microsoft.com/library/azure/dn798941.aspx).

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Zajistěte, aby byl schématu cílové index kompatibilní s schématu zdrojové JSON dokumenty nebo výstup projekce svůj vlastní dotaz.

>[AZURE.NOTE] Rozdělený Collections výchozí dokument klíč je na DocumentDB `_rid` vlastnost, která se přejmenoval na `rid` v Azure vyhledávání. Navíc DocumentDB společnosti `_rid` hodnoty obsahují znaky, které jsou neplatné v Azure hledání klíčích; Proto `_rid` hodnoty jsou ve formátu Base 64 kódování.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Obrázek A: mapování mezi JSON datových typech a datových typů Azure hledání

| JSON DATOVÝ TYP|   TYPY POLÍ KOMPATIBILNÍ CÍLOVÉ INDEXU|
|---|---|
|Logická hodnota|Edm.Boolean Edm.String|
|Čísla, která vypadat celých čísel|Edm.String Edm.Int32, Edm.Int64,|
|Čísla, které vypadají plovoucí body|Edm.Double Edm.String|
|Řetězec|Edm.String|
|Pole základní typy například "a", "b", "c" |Collection(EDM.String)|
|Řetězce, které vypadají jako kalendářní data| Edm.DateTimeOffset Edm.String|
|GeoJSON objektů, například {"typ": "Bod", "souřadnice": [long lat]} | Edm.GeographyPoint |
|Jiné objekty JSON|NENÍ K DISPOZICI|

###<a id="CreateIndexExample"></a>Příklad textu žádosti o

Následující příklad vytvoří rejstřík s id a popis pole:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Odpověď

Obdržíte odpověď HTTP 201 vytvořili úspěšně vytvořen index.

##<a id="CreateIndexer"></a>Krok 3: Vytvoření indexování

Vytvořit nové indexování v rámci služby Azure vyhledávání pomocí žádost HTTP POST následující záhlaví.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Textu žádosti o obsahuje definici indexování, která by měl obsahovat následující pole:

- **název**: povinné. Název indexování.

- **dataSourceName**: povinné. Název existujícího zdroje dat.

- **targetIndexName**: povinné. Název existující index.

- **plán**: nepovinné. V tématu [indexování plánu](#IndexingSchedule) dole.

###<a id="IndexingSchedule"></a>Spuštění indexování na plán

Indexování můžete volitelně určete plán. Pokud je k dispozici plán, indexování se spustí pravidelně podle plánu. Plán obsahuje následující atributy:

- **interval**: povinné. Doba trvání hodnota, která určuje intervalu nebo dobu indexování spustí. Nejmenší povolené interval je 5 minut; nejdelší je jeden den. Musí být formátováno jako hodnotu "dayTimeDuration" XSD (s omezeným přístupem podmnožinu na hodnotu [doby trvání formátu ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Vzor pro tuto: `P(nD)(T(nH)(nM))`. Příklady: `PT15M` každých 15 minut `PT2H` pro každé 2 hodiny.

- **čas spuštění**: povinné. Datum a čas UTC, která určuje, kdy by měly začít indexování spuštěný.

###<a id="CreateIndexerExample"></a>Příklad textu žádosti o

Následující příklad vytvoří indexování, který slouží ke kopírování dat z kolekce odkazuje `myDocDbDataSource` zdroje dat `mySearchIndex` index podle plánu, který začíná 1 ledna 2015 UTC a spustí každou hodinu.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Odpověď

Obdržíte odpověď HTTP 201 vytvořili úspěšně vytvořili indexování.

##<a id="RunIndexer"></a>Krok 4: Spuštění indexování

Kromě systém pravidelně na plán, indexování lze také vyvolat jako služba zadáním následujících žádost HTTP příspěvku:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odpověď

Obdržíte HTTP 202 přijata odpověď indexování byla úspěšně vyvolat.

##<a name="GetIndexerStatus"></a>Krok 5: Získat stav indexování

Žádost HTTP GET k načtení aktuální stav a spuštění historie indexování můžete problému:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odpověď

Zobrazí se HTTP 200 OK odpovědi vrácené spolu s odpovědí textu, který obsahuje informace o celkové stavu indexování, posledního indexování vyvolání i historii nedávných vyvolání indexování (pokud existuje).

Odpověď na by měla vypadat přibližně takto:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Spuštění historie obsahuje až 50 posledních dokončené spuštění, které jsou seřazené v obráceném chronologickém pořadí (tak nejnovější spuštění je v ní dřív v odpovědi).

##<a name="NextSteps"></a>Další kroky

Blahopřejeme! Jste se naučili jenom jak integrovat Azure DocumentDB pomocí funkce hledání Azure pomocí indexování pro DocumentDB.

 - Další způsob informace o Azure DocumentDB, naleznete na [stránce service DocumentDB](https://azure.microsoft.com/services/documentdb/).

 - Další způsob Další informace o Azure hledání najdete v tématu [Vyhledávací služby stránku](https://azure.microsoft.com/services/search/).
