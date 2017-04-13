<properties 
pageTitle="Operace indexování (Azure hledání služba REST API: 2015 02 28 náhled) | Náhled Azure hledání rozhraní API" 
description="Operace indexování (Azure hledání služba REST API: 2015 02 28 Preview)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Operace indexování (Azure hledání služba REST API: 2015 02 28 Preview)#

> [AZURE.NOTE] Tento článek popisuje indexování v rozhraní [REST API 2015 02 28 náhled](search-api-2015-02-28-preview.md). Tato verze rozhraní API přidá verze preview úložišti objektů Blob Azure indexování s extrahování dokumentu a úložiště tabulek Azure indexování plus další vylepšení. Rozhraní API taky podporuje obecně dostupných indexování (GA), včetně indexování pro databázi SQL Azure, SQL Server Azure VMs a Azure DocumentDB.

## <a name="overview"></a>Základní informace ##

Azure hledání můžete integrovat přímo některé běžné zdrojů dat, odebráním potřeba kódu indexovat data. Nastavit to nahoru, můžete volat Azure rozhraní API pro hledání můžete vytvořit a spravovat **zdroje dat**a **indexování** . 

**Indexování** je zdroj, který připojí zdroje dat se cíl hledání indexy. Používá se indexování následujícími způsoby: 

- Provedení jednorázové kopii dat k naplnění rejstříku.
- Synchronizujte indexu se změnami ve zdroji dat podle plánu. Plán je součástí definici indexování.
- Volat na vyžádání aktualizovat rejstřík podle potřeby. 

**Indexování** je užitečná, když se mají běžná aktualizace rejstříku. Můžete nastavit plánu vložené jako součást indexování definice, nebo ho spusťte jako služba pomocí [Spustit indexování](#RunIndexer). 

**Zdroj dat** Určuje, jaká data musí být indexované, pověření pro přístup k datům a zásady pro povolení Azure hledání účinně identifikaci změn dat (třeba upravit nebo odstranit řádky v tabulce databáze). Je definován jako zdroj nezávislé tak, aby ji může používat víc indexování.

V následujících zdrojích dat jsou aktuálně podporované:

- **Databáze Azure SQL** a **SQL Server na Azure VMs**. Cílových walk-through najdete v [tomto článku](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure DocumentDB**. Cílových walk-through najdete v [tomto článku](../documentdb/documentdb-search-indexer.md). 
- **Úložiště objektů Blob azure**včetně následující formáty dokumentů: PDF, Microsoft Office (DOCX/dokumentu XSLX/XLS, PPTX/PPT, MSG), HTML, XML, ZIP a ve formátu prostého textu soubory (včetně JSON). Cílových walk-through najdete v [tomto článku](search-howto-indexing-azure-blob-storage.md).
- **Úložiště tabulek azure**. Cílových walk-through najdete v [tomto článku](search-howto-indexing-azure-tables.md).
     
Jsme rozhodování přidání podpora pro další zdroje dat v budoucnu. Jak pomoct při určit jejich prioritu tyto rozhodnutí, zadejte svůj názor na [fórum pro názory Azure vyhledávání](http://feedback.azure.com/forums/263029-azure-search).

Maximální limity související s indexování a datových zdrojů zdroj naleznete v tématu [Limity služby](search-limits-quotas-capacity.md) .

## <a name="typical-usage-flow"></a>Typické použití toku ##

Můžete vytvářet a spravovat indexování a zdrojů dat prostřednictvím jednoduchých požadavků HTTP (příspěvek, GET, umístění, DELETE) proti dané `data source` nebo `indexer` zdroje.

Nastavení automatické indexování je obvykle zahrnuje čtyři kroky:

1. Určení zdroje dat, která obsahuje data, která chcete indexovat. Mějte na paměti, že hledání Azure nepodporuje všechny typy dat prezentovat ve zdroji dat: V seznamu najdete v článku [podporované datové typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) .

2. Vytvoření indexu vyhledávání Azure jehož schéma je kompatibilní se zdrojem dat.
  
3. Vytvoření zdroje dat Azure vyhledávání podle popisu v [Vytvořit zdroj dat](#CreateDataSource).
  
4. Vytvořte indexování hledání Azure jako popsáno [Vytváření rejstříku](#CreateIndexer).

Je třeba naplánovat týkající se vytváření jeden indexování pro každou kombinaci cílové indexu a datové zdroje. Máte víc indexování psaní do stejné index a můžete znovu použít stejný zdroj dat pro více indexování. Indexování však můžete používat jenom jeden zdroj dat najednou a můžete jenom psát do jednoho indexu. 

Po vytvoření indexování, můžete získat jeho stav spuštění myší [Získat stav indexování](#GetIndexerStatus) . Můžete taky spustit indexování kdykoli (místo něj nebo spolu pravidelně spuštěných pro plán) pomocí operace [Indexování spustit](#RunIndexer) .

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Vytvoření zdroje dat ##

V Azure vyhledávání zdroje dat se používá indexování poskytuje informace o připojení pro ně aktualizaci ad hoc nebo plánovaných dat z indexu založeného na cílové. Můžete vytvořit nový zdroj dat v rámci služby Azure vyhledávání pomocí žádost HTTP příspěvek.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Můžete taky můžete použít umístění a zadejte název zdroje dat na URI. Zdroje dat neexistuje, bude vytvořena.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Poznámka**: maximální počet zdrojů dat povolených se liší podle ceny osy. Bezplatné služby umožňuje až 3 zdroje dat. Standardní služba umožňuje 50 zdroje dat. Další informace najdete v článku [Omezení služeb](search-limits-quotas-capacity.md) .

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Žádost o **Vytvořit zdroj dat** můžete vytvořen metodou příspěvku nebo umístění. Pokud chcete použít příspěvek, je nutné zadat název zdroje dat v hlavním textu žádosti o spolu s definice zdroje dat. S umístění název je část adresy URL. Pokud neexistuje zdroje dat se vytvoří. Pokud již existuje, se aktualizuje na novou definici. 

Název zdroje dat musí být malá písmena, začínají na písmeno nebo číslo, bez lomítka nebo tečky a být menší než 128 znaků. Po název zdroje dat začínající na písmeno nebo číslo, můžete zahrnout zbytek název písmeno, číslo a typ čáry, dokud pomlčky nejsou po sobě jdoucí. Další informace najdete v tématu [zásady vytváření názvů pravidla](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

`api-version` Požaduje. Aktuální verze není `2015-02-28`.

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví. 

- `Content-Type`: Povinné. Nastavte`application/json`
- `api-key`: Povinné. `api-key` Slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. **Vytvoření zdroje dat** žádost musí obsahovat `api-key` záhlaví nastavit klíč správce (na rozdíl od dotazu klíče). 
 
Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby [Azure portálu](https://portal.azure.com/). V tématu [Vytvoření vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

<a name="CreateDataSourceRequestSyntax"></a>
**Žádost o syntaxi textu**

Textu žádosti o obsahuje definice zdroje dat, který obsahuje typ zdroje dat přihlašovací údaje pro číst data, jakož i volitelné dat změnit zjišťování a data odstranění zjišťování zásad, které slouží k identifikaci efektivní změní nebo odstraní data ve zdroji dat při použití s pravidelně plánované indexování. 

Syntaxe strukturování datové žádost je následujícím způsobem. Žádost o vzorku je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Žádost o obsahuje následující vlastnosti: 

- `name`: Povinné. Název zdroje dat Název zdroje dat musí pouze obsahují malá písmena, číslice pomlček nebo prázdných buněk, nemůžete začínat ani končit pomlčky a se omezí na 128 znaků.
- `description`: Volitelný popis. 
- `type`: Povinné. Musí být jeden zdroj podporované datové typy:
    - `azuresql`-Azure SQL databáze nebo SQL Server na Azure VMs
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Úložiště objektů Blob azure
    - `azuretable`-Azure Table Storage
- `credentials`:
    - Povinní `connectionString` vlastnost určuje připojovací řetězec pro zdroj dat. Formát připojovací řetězec závisí na typu zdroje dat: 
        - U SQL Azure je to obvykle připojovací řetězec SQL serveru. Pokud používáte portálu Azure k načtení připojovací řetězec, použijte `ADO.NET connection string` možnost.
        - Pro DocumentDB, musí být připojovací řetězec v v tomto formátu: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Všechny hodnoty jsou potřeba. Je můžete najít v [Azure portálu](https://portal.azure.com/).  
        - U objektů Blob Azure a úložiště tabulek je to připojovací řetězec účtu úložiště. Formát popsané [v tomto poli](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Protokol HTTPS koncový bod se vyžaduje.  
- `container`, požadované: Určuje data, která chcete indexovat pomocí `name` a `query` vlastnosti: 
    - `name`, vyžaduje:
        - Azure SQL: Určuje tabulky nebo zobrazení. Schéma kvalifikované názvy, můžete použít jako `[dbo].[mytable]`.
        - DocumentDB: Určuje kolekci. 
        - Úložiště objektů Blob Azure: Určuje kontejneru úložiště.
        - Úložiště tabulek Azure: název tabulky. 
    - `query`, volitelné:
        - DocumentDB: umožňuje zadat dotaz, který sloučí libovolného rozložení dokumentu JSON do ploché schéma, které můžete index vyhledávání Azure.  
        - Úložiště objektů Blob Azure: umožňuje určit virtuální složka v kontejneru objektů blob. Například cesta objektů blob `mycontainer/documents/blob.pdf`, `documents` mohou sloužit jako virtuální složka.
        - Úložiště tabulek Azure: umožňuje zadat dotaz, který filtruje sady řádků, které chcete importovat.
        - Azure SQL: query nepodporuje. V případě potřeby prvek pro tuhle funkci prosím Hlasujte pro [Tento návrh](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Volitelné `dataChangeDetectionPolicy` a `dataDeletionDetectionPolicy` vlastnosti jsou uvedeny níže.

<a name="DataChangeDetectionPolicies"></a>
**Data změny zjišťování zásad**

Má zjišťování zásad pro změnu dat účel efektivní určete položky s změněná data. Lišit v závislosti na typu zdroje dat podporované zásady. Následující oddíly popisují každého zásadu. 

***Horní mez změnit zjišťování zásad*** 

Použijte tuto zásadu, když zdroj dat obsahuje sloupce nebo vlastnost, která splňuje následující kritéria:
 
- Všechny vloží zadejte hodnotu pro sloupec. 
- Všechny aktualizace k nějaké položce taky změnit hodnotu ve sloupci. 
- Hodnota v tomto sloupci závislá jednotlivé změny.
- Dotazy, které používají klauzuli filtru podobná `WHERE [High Water Mark Column] > [Current High Water Mark Value]` můžete efektivně spouštět.

Třeba při použití zdroje dat Azure SQL, indexované `rowversion` sloupec je ideální volbou pro použití s zásadami označit vodu maximum. 

Této zásady může být zadán následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Pokud chcete použít DocumentDB zdroje dat, je nutné použít `_ts` vlastnost poskytovanou DocumentDB. 

> [AZURE.NOTE] Při použití zdrojů dat objektů Blob Azure, Azure hledání automaticky používá horní mez změna zjišťování zásad podle objektů blob datum poslední změny časového razítka; není potřeba zadat tyto zásady sami.   

***Změna zjišťování zásad integrované SQL***

Pokud databázi SQL podporuje [Sledování změn](https://msdn.microsoft.com/library/bb933875.aspx), doporučujeme používat SQL integrované změnit způsob sledování. Tuto zásadu umožňuje nejefektivnější sledování změn a umožňuje vyhledávání Azure k identifikaci odstraněné řádky aniž by bylo nutné mít explicitní "měkké odstranit" sloupec ve vaší schématu.

Integrované sledování změn je podporován od s následujícími verzemi databáze SQL serveru: 
- SQL Server 2008 R2, pokud používáte na Azure VMs SQL serveru.
- Azure SQL databáze V12, pokud používáte databázi SQL Azure.  

Při použití zásad integrované sledování změn SQL Server nezadáte zásadu samostatný datový odstranění rozpoznávání - tuto zásadu má vestavěné podporu k identifikaci odstranit řádky. 

Tuto zásadu lze použít pouze s tabulkami; nelze použít s zobrazení. Je třeba povolit na tabulku, kterou používáte, než budete moct použít tuto zásadu sledování změn. Pokyny najdete v článku [Povolení nebo zakázání sledování změn](https://msdn.microsoft.com/library/bb964713.aspx) .    
 
Při uspořádání žádost **Vytvořit zdroj dat** , zásady sledování změn SQL integrované lze zadat následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Data odstranění zjišťování zásad**

Má zjišťování zásad pro odstranění dat účel efektivní určete položky odstraněné data. V současné době podporované zásada pouze se `Soft Delete` zásad, které umožňuje identifikační odstraněné položky na základě hodnoty z `soft delete` sloupce nebo vlastnosti ve zdroji dat. Této zásady může být zadán následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Poznámka:** Podporuje pouze sloupce s řetězec, celé číslo nebo logické hodnoty. Hodnota použita jako `softDeleteMarkerValue` musí být řetězec, i když musí odpovídající sloupec obsahuje logické hodnoty nebo celá čísla. Pokud je argument hodnota, která se zobrazí ve zdroji dat 1, pomocí například `"1"` jako `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Příklady textu žádosti o**

Pokud chcete použít zdroje dat s indexování, která se spouští při plánování, tento příklad ukazuje, jak můžete určit, změna a odstranění zjišťování zásad: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Pokud chcete použít zdroje dat pro jednorázové zkopírování dat, nemusí provádět zásady:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odpověď**

Pro úspěšné žádost: 201 vytvořeno. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Aktualizace zdroje dat ##

Je možné aktualizovat existujícího zdroje dat pomocí žádost HTTP umístění. Zadejte název zdroje dat aktualizovat na vyžádání URI:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Požaduje. Aktuální verze není `2015-02-28`. [Správa verzí Azure vyhledávání](https://msdn.microsoft.com/library/azure/dn864560.aspx) obsahuje podrobnosti a další informace o alternativní verze.

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Žádost o**

Syntaxe textu žádosti o je stejné jako u [žádostí o vytvoření zdroje dat](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] V existujícím zdroji dat nelze aktualizovat některé vlastnosti. Typ existujícího zdroje dat například nelze změnit.  

> [AZURE.NOTE] Pokud nechcete změnit připojovací řetězec pro existujícího zdroje dat, můžete určit, literál `<unchanged>` pro připojovací řetězec. To je užitečné například v situacích, kdy potřebujete aktualizovat datového zdroje, ale nemáte pohodlný přístup k připojovací řetězec takové alternativní řešení je zabezpečené data.

**Odpověď**

Pro úspěšné žádost: 201 vytvořeno Pokud byl nový zdroj dat vytvořené a 204 bez obsahu Pokud byl aktualizován existujícího zdroje dat.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Zobrazit seznam zdrojů dat ##

**Zobrazit seznam zdrojů dat** operace vrátí seznam zdrojů dat v Azure vyhledávací služby. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

`api-version` Požaduje. Aktuální verze není `2015-02-28`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Pro úspěšné žádost: 200 OK.

Tady je text odpovědi příkladu:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Všimněte si, že je můžete vyfiltrovat odpověď dolů jenom vlastnosti, které vás zajímá. Pokud chcete jenom seznam názvů zdrojů dat, pomocí například OData `$select` dotazu možnost:

    GET /datasources?api-version=205-02-28&$select=name

V tomto případě odpověď z výše uvedený příklad vypadat takto: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Toto je užitečné postup uložit šířky pásma, pokud máte před sebou hodně zdrojů dat v vyhledávací služby.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Získání zdroje dat ##

**Získání zdroje dat** operace získá definice zdroje dat z Azure vyhledávání.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Požaduje. Aktuální verze není `2015-02-28`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Odpověď na je podobný příklady v [žádosti o příklad vytvořit zdroj dat](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Není nastavena `Accept` žádost o záhlaví `application/json;odata.metadata=none` při volání toto rozhraní API jako to bude mít za následek `@odata.type` atribut mají být vynechány odpověď a nebude možné rozlišit Změna dat a zásady rozpoznávání odstranění dat z různých typů. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Odstranění zdroje dat ##

**Odstranění zdroje dat** operace odeberete zdroj dat z Azure vyhledávací služby.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Pokud libovolný indexování odkazují na zdroj dat, který odstraňujete, se provede operace odstranění se nezobrazuje. Tyto indexování však přejde do stavu chyby při příštím spuštění.  

`api-version` Požaduje. Aktuální verze není `2015-02-28`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 204 bez obsahu budou vráceny úspěšné odpověď.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Vytvoření rejstříku ##

Můžete vytvořit nový indexování v rámci služby Azure vyhledávání pomocí žádost HTTP příspěvek.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Můžete taky můžete použít umístění a zadejte název zdroje dat na URI. Zdroje dat neexistuje, bude vytvořena.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Maximální počet indexování povoleno se liší podle ceny osy. Bezplatné služby umožňuje až 3 indexování. Standardní služba umožňuje 50 indexování. Další informace najdete v článku [Omezení služeb](search-limits-quotas-capacity.md) .

`api-version` Požaduje. Aktuální verze není `2015-02-28`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.


<a name="CreateIndexerRequestSyntax"></a>
**Žádost o textu syntaxi**

Textu žádosti o obsahuje definici indexování, která určuje zdroje dat a cílové index indexování, jakož i volitelný indexovací plán a parametry. 


Syntaxe strukturování datové žádost je následujícím způsobem. Žádost o vzorku je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indexování plánu**

Indexování můžete volitelně určete plán. Pokud je k dispozici plán, indexování se spustí pravidelně podle plánu. Plán obsahuje následující atributy:

- `interval`: Povinné. Doba trvání hodnota, která určuje intervalu nebo dobu indexování se spustí. Nejmenší povolené interval je 5 minut; nejdelší je jeden den. Musí být formátováno jako hodnotu "dayTimeDuration" XSD (s omezeným přístupem podmnožinu na hodnotu [doby trvání formátu ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Vzor pro tuto: `"P[nD][T[nH][nM]]"`. Příklady: `PT15M` každých 15 minut `PT2H` pro každé 2 hodiny. 

- `startTime`: Povinné. UTC datetime při indexování by měl spuštění. 

**Indexování parametry**

Indexování Volitelně můžete zadat několik parametry ovlivňující jeho chování. Všechny parametry jsou volitelné.  

- `maxFailedItems`: Počet položek, které může selhat indexovat před spustit indexování je považován za se nepovede. Výchozí hodnota je 0. Informace o nezdařeném uložení položek vám vrátil operaci [Získat stav indexování](#GetIndexerStatus) . 

- `maxFailedItemsPerBatch`: Počet položek, které může selhat indexovat v každé dávce před spustit indexování je považován za se nepovede. Výchozí hodnota je 0.

- `base64EncodeKeys`: Určí, zda dokument klíče bude základu-64. Azure hledání ukládá omezení znaků, které mohou být prezentovat dokument klíče. Hodnoty ve zdrojových datech však může obsahovat znaky, které jsou neplatné. Pokud je třeba indexovat tyto hodnoty jako dokument klíče, může být nastaven tento příznak true (pravda). Výchozí hodnota je `false`.

- `batchSize`: Určuje počet položek, které jsou číst ze zdroje dat a indexované jako jedné dávce aby se zvýšil výkon. Výchozí závisí na typu zdroje dat: je 1000 Azure SQL a DocumentDB a 10 k úložišti objektů Blob Azure.

**Mapování polí**

Mapování polí můžete použít namapujte název pole ve zdroji dat na jiný název pole v cílové index. Zvažte například ke zdrojové tabulce s polem `_id`. Azure hledání neumožňuje název pole začíná podtržítko, tak je pole přejmenovat. Lze to provést pomocí `fieldMappings` vlastnost indexování takto: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Je možné zadat více mapování polí: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Zdrojové a cílové názvy polí jsou malá a velká písmena.

<a name="FieldMappingFunctions"></a>
***Funkce mapování polí***

Mapování polí lze také transformace zdrojové hodnoty polí pomocí *funkce mapování*.

Aktuálně podporované jenom jedna tyto funkce: `jsonArrayToStringCollection`. Analyzuje pole, která obsahuje řetězec formátu JSON pole do pole Collection(Edm.String) v cílové index. To jsou určeny pro použití s Azure SQL indexování zejména od SQL nemá datový typ nativní kolekce. Lze následujícím způsobem: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Pokud pole zdroj obsahuje řetězec, například `["red", "white", "blue"]`, potom cílové pole typu `Collection(Edm.String)` bude naplněný tři hodnotami `"red"`, `"white"` a `"blue"`.

Všimněte si, že `targetFieldName` vlastnost je nepovinná; Pokud doleva, `sourceFieldName` bude použita hodnota. 

<a name="CreateIndexerRequestExamples"></a>
**Příklady textu žádosti o**

Následující příklad vytvoří indexování, který slouží ke kopírování dat z tabulky odkazuje `ordersds` zdroje dat `orders` index podle plánu, který začíná 1 ledna 2015 UTC a spustí každou hodinu. Každý indexování vyvolání bude úspěšná, v případě víc než 5 položek indexovat v každé dávce a víc než 10 položek nezdaří indexovat celkem. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odpověď**

201 vytvořeno pro úspěšné žádost.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Aktualizace rejstříku ##

Je možné aktualizovat existující indexování pomocí žádost HTTP umístění. Zadejte název aplikace indexování aktualizovat na vyžádání URI:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Požaduje. Aktuální verze není `2015-02-28`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Žádost o**

Syntaxe textu žádosti o je stejné jako u [žádostí o vytvoření indexování](#CreateIndexerRequestSyntax).

**Odpověď**

Pro úspěšné žádost: 201 vytvořeno Pokud byl nový indexování vytvořené a 204 bez obsahu Pokud aktualizovali stávající indexování.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Seznam indexování ##

Operace **Indexování seznamu** vrátí seznam indexování v Azure vyhledávací služby. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. [Správa verzí Azure vyhledávání](https://msdn.microsoft.com/library/azure/dn864560.aspx) obsahuje podrobnosti a další informace o alternativní verze.

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Pro úspěšné žádost: 200 OK.

Tady je text odpovědi příkladu:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Všimněte si, že je můžete vyfiltrovat odpověď dolů jenom vlastnosti, které vás zajímá. Pokud chcete jenom seznam jmen indexování pomocí například OData `$select` dotazu možnost:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

V tomto případě odpověď z výše uvedený příklad vypadat takto: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Toto je užitečné Postup uložení šířku pásma máte spoustu indexování v vyhledávací služby.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Získání indexování ##

Operace **Indexování získat** získá definici indexování z Azure vyhledávání.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Odpověď na je podobný příklady v [žádosti o vytvoření indexování příkladu](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Odstranění indexování ##

Operace **Indexování odstranit** odebere indexování z Azure vyhledávací služby.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Při odstranění indexování spuštění indexování probíhá v té době poběží do konce, ale žádné další spuštění naplánuje. Pokusy o použití neexistující indexování bude mít za následek HTTP stavový kód 404 Nenalezeno. 
 
`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 204 bez obsahu budou vráceny úspěšné odpověď.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Spuštění indexování ##

Kromě systém pravidelně podle plánu, indexování můžete taky uplatnit na vyžádání prostřednictvím operaci **Indexování spuštění** : 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 202 přijaté budou vráceny úspěšné odpověď.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Stav indexování ##

Operace **Získat stav indexování** načte aktuální stav a spuštění historie indexování: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 200 OK pro úspěšné odpověď.

Obsah odpovědí obsahuje informace o celkové stavu indexování, posledního indexování vyvolání i historii nedávných vyvolání indexování (pokud existuje). 

Text odpovědi ukázkové vypadat takto: 

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

**Stav indexování**

Stav indexování může být jeden z následujících hodnot:

- `running`Označuje, jestli je indexování spuštěný běžným způsobem. Všimněte si, že některé spuštění indexování pravděpodobně pořád nedaří, takže je vhodné ověřit `lastResult` vlastnost stejně. 

- `error`Označuje, že indexování došlo k chybě, který nelze opravit bez zásahu člověka. Například vypršela platnost přihlašovacích údajů pro zdroj dat nebo schématu zdroje dat nebo indexu cílové změnil v jazycích způsobem. 

**Indexování spuštění výsledek**

Výsledek spuštění indexování obsahuje informace o spuštění jednoho indexování. Nejnovější výsledek je vytažená jako `lastResult` vlastnost Stav indexování. Další poslední výsledky, pokud je k dispozici, jsou vráceny jako `executionHistory` vlastnost Stav indexování. 

Indexování spuštění výsledek obsahuje následující vlastnosti: 

- `status`: stav spustitelný. Podrobnosti najdete v části [Stav indexování spuštění](#IndexerExecutionStatus) níže. 

- `errorMessage`: chybová zpráva nezdařené spuštění. 

- `startTime`: času UTC při spuštění tohoto spuštění.

- `endTime`: času UTC při tomto spuštění ukončena. Tato hodnota není nastavena, pokud spuštění stále probíhá.

- `errors`: maticových úrovni položek chyby, pokud existuje. Každá položka obsahuje klíč dokumentu (`key` vlastnost) a chybová zpráva (`errorMessage` vlastnost). 

- `itemsProcessed`: počet datových zdrojů položky (například řádky tabulky), které chcete indexovat během tohoto spuštění pokus o službu indexování. 

- `itemsFailed`: počet položek, která způsobila nezdařenou tohoto spuštění.  
 
- `initialTrackingState`: vždy `null` pro první spuštění rejstříku, nebo pokud data změna zásad sledování není povolený na zdroj dat použít. Pokud je povoleno tyto zásady na pozdější spuštění tato hodnota označuje první (nejnižšímu) změna hodnota zpracovány službou tohoto spuštění sledování. 

- `finalTrackingState`: vždy `null` data změně zásad sledování není povolený na zdroj dat použít. V opačném označuje nejnovější (nejvyšší) sledování hodnotu úspěšně zpracovány službou tento provádění změn. 

<a name="IndexerExecutionStatus"></a>
**Spuštění stav indexování**

Stav indexování spuštění zaznamenává stav spuštění jednoho indexování. Můžete mít na tyto hodnoty:

- `success`Označuje, že byla úspěšně dokončena spuštění indexování.

- `inProgress`znamená, že spuštění indexování probíhá. 

- `transientFailure`Označuje, že se nezdařila spustitelný indexování. Viz `errorMessage` vlastnost podrobnosti. Selhání může nebo nemusí vyžadovat lidské zásah opravíte – například řešení schématu nekompatibilita mezi zdroje dat a index cílové vyžaduje akci uživatele během prostoje zdroj dočasná data nemá. Indexování vyvolání zůstanou podle plánu, pokud je k dispozici. 

- `persistentFailure`Označuje, že indexování selže způsobem, který vyžaduje zásahu člověka. Zastavit spuštění plánované indexování. Po vyřešení problému, můžete obnovit rozhraní API indexování restartujte plánované spuštění. 

- `reset`Označuje, že indexování resetování voláním obnovit rozhraní API indexování (viz níže). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Obnovení indexování ##

Operace **Indexování obnovit** obnoví stav indexování sledování změn. Můžete spustit ze začátku přeindexování (například pokud schématu zdroje dat se změnilo) nebo změnit zjišťování Změna údajů pro zdroj dat přidružený k indexování.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

`api-version` Požaduje. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být kód Product key pro správu (na rozdíl od dotazu klíče). Přečtěte si část ověření v [Hledání služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zobrazíte další informace o klíče. [Vytvořit vyhledávací služby na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíčové vlastnosti použité v pozvánce.

**Odpověď**

Stavový kód: 204 bez obsahu pro úspěšné odpověď.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapování mezi datové typy SQL a typy dat Azure vyhledávání ##

<table style="font-size:12">
<tr>
<td>Datový typ SQL.</td>  
<td>Povolené cílové index typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>Bit</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Funkce int, smallint, tinyint</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64 Edm.String</td>
<td></td>
</tr>
<tr>
<td>skutečné, uvolnit</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney peníze<br>Decimal<br>číselné
</td>
<td>Edm.String</td>
<td>Azure hledání nepodporuje převod desetinné typy Edm.Double protože tím byste dojít ke ztrátě přesnosti
</td>
</tr>
<tr>
<td>znak nchar, datová, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>V tomto dokumentu podrobné informace o tom, jak transformace sloupce řetězec do Collection(Edm.String) najdete v článku [Funkce mapování polí](#FieldMappingFunctions)</td>
</tr>
<tr>
<td>smalldatetime data a času, datetime2, datum, datetimeoffset</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>zeměpisná oblast</td>
<td>Edm.GeographyPoint</td>
<td>Jsou podporované jenom geography výskyty typu bod s SRID 4326 (což je výchozí nastavení)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>NENÍ K DISPOZICI</td>
<td>Sloupce řádku verzí nelze ukládat do indexu vyhledávání, ale budou se dá použít pro sledování změn</td>
</tr>
<tr>
<td>čas, časového rozpětí<br>binární, seskupit, obrázek,<br>XML, geometrie typy CLR</td>
<td>NENÍ K DISPOZICI</td>
<td>Nepodporovaná funkce</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datových typech a datových typů Azure hledání ##

<table style="font-size:12">
<tr>
<td>JSON datový typ</td> 
<td>Povolené cílové index typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>Logická hodnota</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Nedílnou čísel</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>Desetinná čísla</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>řetězec</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>pole Základní zadá, například ["a", "b", "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Řetězce, které vypadají jako kalendářní data</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON čárky objektů</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON body jsou JSON objekty v tomto formátu: {"typ": "Bod", "souřadnice": [long lat]} </td>
</tr>
<tr>
<td>Jiné objekty JSON</td>
<td>NENÍ K DISPOZICI</td>
<td>Nejsou podporovány. Azure hledání v současné době podporuje pouze základní typy a kolekcí řetězec</td>
</tr>
</table>