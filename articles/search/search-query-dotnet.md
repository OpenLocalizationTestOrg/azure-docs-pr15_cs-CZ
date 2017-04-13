<properties
    pageTitle="Dotaz vyhledávacím odkazu Azure pomocí .NET SDK | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Vytvoření vyhledávacího dotazu v Azure hledání a použití parametrů hledání k filtrování a řazení výsledků hledání."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Dotaz Azure vyhledávacím odkazu pomocí .NET SDK
> [AZURE.SELECTOR]
- [Základní informace](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [ZBÝVAJÍCÍ](search-query-rest-api.md)

Tento článek vám ukáže, jak k vytvoření dotazu rejstřík [Azure hledání .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Před zahájením tohoto návodu, byste měli mít již [vytvořené indexu vyhledávání Azure](search-what-is-an-index.md) a [vyplněné jej s daty](search-what-is-data-import.md).

Všimněte si, že všechny ukázkový kód v tomto článku je napsaný v jazyce C#. Můžete najít celou zdroj kód [na GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Můžu. Identifikaci služby Azure vyhledávacího dotazu rozhraní api klíče
Teď, když jste vytvořili indexu vyhledávání Azure, jste připraveni téměř o vystavení dotazů pomocí .NET SDK. Nejdřív bude muset pořídit jednu z dotazu rozhraní api klávesy generovaných pro vyhledávací službu, kterou jste zřízení. .NET SDK odešle tento klíč rozhraní API aplikace na všechny žádosti o služby. Máte platný klíč zavádí zabezpečení na základě na žádost o mezi aplikací odesláním žádosti a na službu, zpracuje ji.

1. K vyhledání rozhraní api klíče vaše služby musíte se přihlásit na [Portál Azure](https://portal.azure.com/)
2. Přejděte na zásuvné Azure vyhledávací služby
3. Klikněte na ikonu "Klávesy"

Služby bude mít *správy klíčů* a *dotazů*.

  - Primární a sekundární *Správce klíče* udělit plné právo všechny operace, včetně možnost spravovat službu, vytváření a odstraňování indexy, indexování a zdrojů dat. Existují dva klíče, takže můžete dál používat klávesu sekundární, pokud se rozhodnete obnovit primární klíč a naopak.
  - *Dotaz klíče* udělit jen pro čtení přístup k indexy a dokumenty a jsou obvykle úměrně klientské aplikace, které problémů žádosti o hledání.

Pro účely dotaz na rejstřík můžete použít jednu z dotazu klíče. Správu klíčů lze také pro dotazy, ale použijte klíč dotazu v kódu aplikace [Princip nejnižších možných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege)to lépe takto.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Vytvoření instance třídy SearchIndexClient
O vystavení dotazů s .NET SDK vyhledávací Azure, musíte se vytvořit instance `SearchIndexClient` předmětu. Tento předmětu má několik konstruktory. Požadovaný trvá vyhledávací služby název, název index a `SearchCredentials` objekt jako parametry. `SearchCredentials`zalamuje kód rozhraní api.

Následující kód vytvoří novou `SearchIndexClient` "hotely" index (vytvořený v tématu [Vytvoření indexu vyhledávání Azure pomocí .NET SDK](search-create-index-dotnet.md)) pomocí hodnoty pro vyhledávací služby název a rozhraní api klávesami uložených v souboru konfigurace aplikace (`app.config` nebo `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`má `Documents` vlastnost. Tato vlastnost poskytuje všechny metody potřebujete k vytvoření dotazu Azure vyhledávací indexy.

## <a name="iii-query-your-index"></a>III. Dotaz index
Hledání pomocí .NET SDK je jednoduchá – stačí volání `Documents.Search` metoda na vaše `SearchIndexClient`. Tento způsob má několik parametry, včetně hledaný text spolu s `SearchParameters` objekt, který můžete použít kterými dále upřesníte dotaz.

#### <a name="types-of-queries"></a>Typy dotazů
Jsou dva hlavní [typy dotazů](search-query-overview.md#types-of-queries) použijete `search` a `filter`. A `search` dotazu vyhledávání pro jedno nebo více podmínek ve všech vrácených polích _vyhledávání_ v indexu. A `filter` dotaz vyhodnotí logický výraz přes všechno _filtrovatelné_ polí v indexu.

Hledání a filtry provádí pomocí `Documents.Search` metody. Můžete předaný vyhledávacího dotazu `searchText` parametr, zatímco výrazu filtru můžete předaný `Filter` vlastnost `SearchParameters` předmětu. Pokud chcete filtrovat bez hledání, přetaženy `"*"` pro `searchText` parametr. Pokud chcete hledat bez filtrování, nechejte `Filter` vlastnost zrušit, nebo není v `SearchParameters` instance vůbec.

#### <a name="example-queries"></a>Příklad dotazů

Následující ukázkový kód zobrazuje několika různými způsoby k vytvoření dotazu index "hotely" definovaný v tématu [Vytvoření indexu vyhledávání Azure pomocí .NET SDK](search-create-index-dotnet.md#DefineIndex). Všimněte si, že vrácených výsledků hledání dokumentů výskyty `Hotel` třídy, která byla definována v [Importu dat v Azure vyhledávání pomocí .NET SDK](search-import-data-dotnet.md#HotelClass). Ukázkový kód využívá `WriteDocuments` metoda výstup výsledků hledání do konzoly. Tento způsob je popsaná v následující části.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Zpracování výsledků hledání
`Documents.Search` Vrátí metoda `DocumentSearchResult` objekt, který obsahuje výsledků dotazu. Příklad v předchozí části používanou metodou s názvem `WriteDocuments` výstup výsledků hledání do konzoly:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Tady je výsledek vypadat takto dotazy v předchozí části, za předpokladu, že index "hotely" vyplněné se vzorovými daty v [Importu dat v Azure vyhledávání pomocí .NET SDK](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Kód ukázkové používá konzole výstup výsledků hledání. Podobně bude nutné zobrazit výsledky hledání ve vlastní aplikaci. Naleznete [v tomto příkladu na GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) příklad toho, jak se vykreslují pomocí výsledky hledání na základě ASP.NET MVC webové aplikace.
