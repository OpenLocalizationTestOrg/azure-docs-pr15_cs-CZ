<properties 
    pageTitle="Připojení databáze Azure SQL k Azure vyhledávání pomocí indexování | Microsoft Azure | Indexování" 
    description="Zjistěte, jak můžete načítat data z databáze SQL Azure do indexu vyhledávání Azure pomocí indexování." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Připojení databáze SQL Azure pomocí indexování hledání Azure

Azure vyhledávací služby serveru je hostovanou cloudu vyhledávací služba, která usnadňuje prostředí skvělé vyhledávání. Před vyhledáváním, budete muset naplnění indexu vyhledávání Azure s daty. Pokud data jsou umístěná v databázi Azure SQL, nový **indexování hledání Azure databáze SQL Azure** (nebo **Azure SQL indexování** krátký) automatizovat indexování. To znamená, že máte menší část kódu k psaní a menší infrastrukturu záleží.

Tento článek popisuje mechanismy používání indexování, ale taky popisuje funkce, které jsou dostupné pouze s databáze Azure SQL (například integrované sledování změn). Azure hledání podporuje i jiných zdrojů dat, například Azure DocumentDB, úložiště objektů blob a úložiště tabulek. Pokud chcete zobrazit podporu další zdroje dat, uveďte na [fórum pro názory Azure vyhledávání](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexování a zdrojů dat

Můžete nastavit a konfigurace indexování Azure SQL pomocí: 

- Průvodce importem dat na [portálu Azure](https://portal.azure.com)
- Azure hledání [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure hledání [rozhraní REST API](http://go.microsoft.com/fwlink/p/?LinkID=528173)

V tomto článku používáme rozhraní REST API pro předvedení se vytváření a správa **zdrojů dat**a **indexování** . 

**Zdroj dat** Určuje, která data indexovat přihlašovací údaje pro přístup k datům a zásady identifikující efektivní změna dat (nové, změny nebo odstranění řádků). Je definován jako zdroj nezávislé tak, aby ji může používat víc indexování.

**Indexování** je zdroj, který připojí zdroje dat se cíl hledání indexy. Používá se indexování následujícími způsoby:
 
- Provedení jednorázové kopii dat k naplnění rejstříku.
- Aktualizujte rejstřík změnami ve zdroji dat podle plánu.
- Spusťte na vyžádání aktualizovat rejstřík podle potřeby. 

## <a name="when-to-use-azure-sql-indexer"></a>Kdy použít indexování Azure SQL

V závislosti na několika faktorech vztahující se k datům používá Azure SQL indexování může nebo nemusí být vhodné. Pokud vaše data vyhovuje následující požadavky, můžete použít indexování Azure SQL: 

- Všechna data pochází z jedné tabulky nebo zobrazení
    - Pokud data uloženy v několika tabulkách, můžete vytvořit zobrazení a zobrazení pomocí indexování. Ale pokud používáte zobrazení, nebude moct používat funkci rozpoznávání změny integrované serveru SQL Server. Další informace najdete v tématu [v této části](#CaptureChangedRows). 
- Datové typy používané ve zdroji dat jsou podporovány indexování. Většina, ale ne všechny typy SQL nejsou podporované. Podrobnosti najdete v tématu [mapování datových typů v Azure vyhledávání](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Není potřeba poblíž v reálném čase aktualizace indexovat při změně řádku. 
    - Indexování můžete přeindexování tabulky maximálně každých 5 minut. V případě data často mění a změny se projeví v indexu v sekundách nebo minutách jediné, doporučujeme používat [Rozhraní API indexu vyhledávání Azure](https://msdn.microsoft.com/library/azure/dn798930.aspx) přímo. 
- Pokud máte velký k sadám dat a plánování spustit indexování podle plánu, vaše schéma umožňuje efektivní identifikovat změnit (a odstranili, když to jde) řádků. Další podrobnosti najdete v tématu "Zachycení změnit a odstranění řádků" dole. 
- Velikost indexované pole v řádku nepřekročila maximální velikosti doručovaných Azure hledání indexování žádost, což je 16 MB. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Vytváření a používání indexování Azure SQL

Vytvoření zdroje dat: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Můžete získat připojovací řetězec z [Azure klasické portálu](https://portal.azure.com). použít `ADO.NET connection string` možnost.

Pokud už nemáte pak vytvořte index vyhledávání Azure cílové. Můžete vytvořit rejstřík [portál uživatelské rozhraní](https://portal.azure.com) nebo [Vytvoření rozhraní API Index](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zajistěte, aby byl schématu cílové index kompatibilní se schématem zdrojové tabulky – najdete v článku [mapování mezi datové typy SQL a Azure vyhledávání](#TypeMapping).

Nakonec vytvoříte indexování zadejte název a odkazování na index zdrojové a cílové dat:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indexování vytvořili tímto způsobem nemá plánu. Automaticky spustí po jeho vytvoření. Můžete ho spusťte kdykoli znovu pomocí žádost o **spuštění indexování** :

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Můžete přizpůsobit několika aspektů indexování chování, jako je velikost dávky a kolik dokumentů můžete vynechán, před spustitelný indexování se nezdaří. Další informace najdete v tématu [Vytvoření rozhraní API indexování](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Budete muset povolit Azure služby pro připojení k databázi. Další informace o tom, jak to udělat v tématu [Připojení z Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Sledování stavu indexování a spuštění historie (počet položek indexované, k chybám, atd.), můžete žádost **Stav indexování** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Odpověď na by měla vypadat přibližně takto: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Spuštění historie obsahuje až 50 naposledy dokončené spuštění, které jsou seřazené v obráceném chronologickém pořadí (tak, že nejnovější spuštění je v ní dřív v odpovědi). Další informace o odpověď můžete najít v [Získat stav indexování](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Spuštění indexování na plán

Můžete taky uspořádat indexování pravidelně spouštět podle plánu. K tomuto účelu přidáte vlastnost **plánu** při vytváření nebo aktualizaci indexování. Žádost o umístění k aktualizaci indexování ukazuje tento příklad:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Parametr **interval** je povinný. Interval odkazuje na časový interval mezi start dvě po sobě jdoucí indexování spuštění. Nejmenší povolené interval je 5 minut; nejdelší je jeden den. Musí být formátováno jako hodnotu "dayTimeDuration" XSD (s omezeným přístupem podmnožinu na hodnotu [doby trvání formátu ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Vzor pro tuto: `P(nD)(T(nH)(nM))`. Příklady: `PT15M` každých 15 minut `PT2H` pro každé 2 hodiny.

Volitelné **čas spuštění** označuje, kdy by měly začít plánované spuštění. Pokud není uveden, použije se aktuální čas UTC. Tentokrát může být v minulosti – naplánován případ první spuštění jako kdybyste indexování je spuštěn nepřetržitě od čas spuštění.  

Jedinou spuštění indexování mohlo by umožnit spuštění po druhém. Pokud indexování běží, kdy by měla proběhnout spuštění, spuštění odložit, dokud další plánovaná doba.

Zvažte příklad nastavte u této konkrétnější. Předpokládejme jsme následující hodinu nakonfigurované: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Tady najdete, co se stane: 

1. První indexování spuštění na nebo kolem 1 březen 2015 12:00 dop. UTC.
1. Předpokládejme, že tento spuštění trvá 20 minut (nebo kdykoli menší než 1 hodina).
1. Druhém spuštění na nebo kolem 1 březen 2015 1:00 dop. 
1. Předpokládejme, že tento spuštění zabírá víc než jednu hodinu – například 70 minut – tak, aby dokončením asi 2:10 dop.
1. Je nyní 2:00 dop., čas pro třetí spuštění začít. Ale protože druhý spuštění z 1 dop. pořád používá třetí spuštění vynechán. Třetí spuštění začíná hodnotou 3 hodiny.

Přidání, změna nebo odstranění kalendáře pro existující indexování pomocí žádost o **umístění indexování** . 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Zachycení nový, měnit a odstranění řádků

Pokud tabulka obsahuje počet řádků, byste měli použít zjišťování zásad dat změnit. Změna zjišťování umožňuje efektivně načítání pouze nové nebo změněné řádky, aniž byste museli přeindexování celou tabulku.

### <a name="sql-integrated-change-tracking-policy"></a>SQL integrované sledování zásad změn

Pokud databázi SQL podporuje [Sledování změn](https://msdn.microsoft.com/library/bb933875.aspx), doporučujeme používat **SQL integrované změnit způsob sledování**. Toto je nejefektivnější zásady. Kromě toho je možné hledat Azure k identifikaci odstraněné řádky aniž by bylo nutné přidat explicitní "měkké odstranit" sloupec do tabulky.

Integrované sledování změn je podporován od s následujícími verzemi databáze SQL serveru:
 
- SQL Server 2008 R2 nebo novější, pokud používáte na Azure VMs SQL serveru. 
- Azure SQL databáze V12, pokud používáte databázi SQL Azure.

Při použití zásad, sledování změn SQL integrované nezadáte zásadu samostatný datový odstranění duplicit – tuto zásadu má vestavěné podpory k identifikaci odstranit řádky.

Tuto zásadu lze použít pouze s tabulkami; nelze použít s zobrazení. Je třeba povolit na tabulku, kterou používáte, než budete moct použít tuto zásadu sledování změn. Pokyny najdete v článku [Povolení nebo zakázání sledování změn](https://msdn.microsoft.com/library/bb964713.aspx) . 

Pomocí této zásady, vytvoření nebo aktualizace zdroje dat takto:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Označení vysoká vodu změnit zjišťování zásad

Zatímco se doporučuje zásad integrované sledování změn SQL Server, lze použít pouze s tabulkami, ne zobrazení. Pokud používáte zobrazení, zvažte použití zásad označit vodu maximum. Tuto zásadu lze použít, pokud tabulky nebo zobrazení sloupcem, který splňuje následující kritéria:

- Všechny vloží zadejte hodnotu pro sloupec. 
- Všechny aktualizace k nějaké položce taky změnit hodnotu ve sloupci. 
- Hodnota v tomto sloupci závislá jednotlivé změny.
- Dotazy následujícím kde a klauzule ORDER by můžete provádět efektivní: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

Například sloupec indexované **rowversion** je ideální volbou pro označení vysoké vodu sloupec. Pomocí této zásady, vytvoření nebo aktualizace zdroje dat takto: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

> [AZURE.WARNING] Pokud zdrojová tabulka nemá indexu ve sloupci označení vysoká vodu, dotazů používaných indexování SQL vypršet časový limit. Zejména `ORDER BY [High Water Mark Column]` klauzule vyžaduje indexu Pokud tabulka obsahuje mnoho řádky efektivní spouštět. 

Pokud narazíte na chyby vypršení časového limitu, můžete `queryTimeout` nastavení konfigurace indexování nastavíte vypršení časového limitu dotazu na hodnotu vyšší než výchozí časový limit 5 minut. Pokud chcete nastavit časový limit 10 minut, například vytvořit nebo aktualizovat indexování pomocí následující konfigurace: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Můžete taky zakázat `ORDER BY [High Water Mark Column]` klauzule. Však to se nedoporučuje, protože pokud provádění indexování přeruší tak, že chybu, má indexování znovu zpracuje všechny řádky Pokud běží později -, i když indexování již zpracována skoro všechny řádky v době, kdy byla přerušena. Zákaz `ORDER BY` klauzule, použít `disableOrderByHighWaterMarkColumn` nastavení v definici indexování:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Měkké odstranit sloupec odstranění zjišťování zásad

Při odstranění řádků ze zdrojové tabulky, pravděpodobně chcete odstranit řádky z indexu vyhledávání i. Pokud používáte změnit SQL integrované sledování zásad, to se stará za vás. Však sledování zásad změn maximum vodu označit nemá vám pomohl odstraněné řádky. Co dělat? 

Pokud řádky fyzicky odeberete z tabulky, Azure hledání nijak odvodit stavu záznamy, které už existuje.  Postup "měkkých odstranit" však může použít logicky odstranit řádky bez odebrání z tabulky. Přidání sloupce na řádky tabulky nebo zobrazení a označit jako odstraněné pomocí tohoto sloupce.

Používáte-li odstranit měkké postup, můžete zadat měkké zásadu odstranit takto při vytváření nebo aktualizaci zdroje dat: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

**SoftDeleteMarkerValue** musí být řetězec – pomocí číselné řetězce skutečné hodnoty. Pokud máte sloupec celé číslo, kde jsou odstraněné řádky označené hodnota 1, pomocí například `"1"`. Pokud máte BIT sloupce, kde jsou odstraněné řádky označené logickou hodnotu true, `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapování mezi datové typy SQL a typy dat Azure vyhledávání

|Datový typ SQL. | Povolené cílové index typy polí |Poznámky 
|------|-----|----|
|Bit|Edm.Boolean Edm.String| |
|Funkce int, smallint, tinyint |Edm.String Edm.Int32, Edm.Int64,| |
| bigint | Edm.Int64 Edm.String | |
| skutečné, uvolnit |Edm.Double Edm.String | |
| Smallmoney desetinné místo číselné peníze | Edm.String| Azure hledání nepodporuje převod desetinné typy Edm.Double protože tím byste dojít ke ztrátě přesnosti |
| znak nchar, datová, nvarchar | Edm.String<br/>Collection(EDM.String)|Řetězec SQL mohou sloužit k naplnění pole Collection(Edm.String) Pokud řetězec představuje maticové JSON řetězců:`["red", "white", "blue"]` | 
|smalldatetime data a času, datetime2, datum, datetimeoffset |Edm.DateTimeOffset Edm.String| |
|uniqueidentifer | Edm.String | |
|zeměpisná oblast | Edm.GeographyPoint | Jsou podporované jenom geography výskyty typu bod s SRID 4326 (což je výchozí nastavení) | | 
|ROWVERSION| NENÍ K DISPOZICI |Sloupce řádku verzí nelze ukládat do indexu vyhledávání, ale budou se dá použít pro sledování změn | |
| čas, časový interval, binární, seskupit, obrázek, xml, geometrie, typy CLR | NENÍ K DISPOZICI |Nepodporovaná funkce |

## <a name="configuration-settings"></a>Konfigurace nastavení

SQL indexování zpřístupňuje několik nastavení: 

|Nastavení |Datový typ | Účel | Výchozí hodnota |
|--------|----------|---------|---------------|
| queryTimeout | řetězec | Nastaví časový limit pro spuštění dotazu SQL | 5 minut ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | Logická hodnota | Dotaz SQL použitý zásadami maximum vodu označit vynechat klauzule ORDER BY, bude se. V tématu [zásady označení vysoká vodu](#HighWaterMarkPolicy) | NEPRAVDA | 

Toto nastavení slouží v `parameters.configuration` objekt v definici indexování. Nastavit vypršení časového limitu dotazu na 10 minut, například vytvořit nebo aktualizovat indexování pomocí následující konfigurace: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Q:** Dá se systémem IaaS VMs v Azure SQL databáze pomocí indexování Azure SQL?

Odpověď: Ano. Potřebujete povolit vyhledávací služby pro připojení k databázi. Další informace najdete v tématu [Konfigurace připojení z indexování vyhledávání Azure SQL Server na OM Azure](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).


**Q:** Dá se systémem místní databáze SQL pomocí indexování Azure SQL? 

A: jsme nedoporučuje ani nepodporuje, protože tím byste vyžadují, abyste při otvírání databází na internetový provoz. 

**Q:** Můžete použít Azure SQL indexování s databázemi než SQL serveru ve IaaS na Azure? 

A: nepodporujeme tento scénář, protože jsme netestovali indexování s žádné databáze než SQL Server.  

**Q:** Můžete vytvořit více indexování spuštěných pro plán? 

Odpověď: Ano. Však jedinou indexování můžete používat v jednom uzlu najednou. Pokud potřebujete více indexování systém současně, zvažte škálování vyhledávací služby na víc než jednu jednotku vyhledávání. 

**Q:** Spuštění indexování vliv má pracovní zátěž můj dotaz? 

Odpověď: Ano. Indexování běží na jeden z uzlů v vyhledávací služby a materiály, které uzel jsou sdíleny indexování a sloužících přenosu dotazů a jiné požadavky rozhraní API. Pokud jste dostali náročné indexování a dotaz pracovního vytížení a dojde k vysoké rychlosti 503 chyby nebo rostoucí doby odezvy, zvažte škálování vyhledávací služby.
