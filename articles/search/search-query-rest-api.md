<properties
    pageTitle="Dotaz vyhledávacím odkazu Azure pomocí rozhraní REST API | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Vytvoření vyhledávacího dotazu v Azure hledání a použití parametrů hledání k filtrování a řazení výsledků hledání."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Dotaz Azure vyhledávacím odkazu pomocí rozhraní REST API
> [AZURE.SELECTOR]
- [Základní informace](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [ZBÝVAJÍCÍ](search-query-rest-api.md)

Tento článek vám ukáže, jak k vytvoření dotazu rejstřík [Rozhraní REST API pro hledání Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Před zahájením tohoto návodu, byste měli mít již [vytvořené indexu vyhledávání Azure](search-what-is-an-index.md) a [vyplněné jej s daty](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Můžu. Identifikaci služby Azure vyhledávacího dotazu rozhraní api klíče
Hlavní součástí každého vyhledávací operace proti rozhraní REST API pro hledání Azure je *rozhraní api klíč* , který byl vytvořený pro službu, kterou jste zřízení. Máte platný klíč zavádí zabezpečení na základě na žádost o mezi aplikací odesláním žádosti a na službu, zpracuje ji.

1. K vyhledání rozhraní api klíče vaše služby musíte se přihlásit na [Portál Azure](https://portal.azure.com/)
2. Přejděte na zásuvné Azure vyhledávací služby
3. Klikněte na ikonu "Klávesy"

Služby bude mít *správy klíčů* a *dotazů*.

 - Primární a sekundární *Správce klíče* udělit plné právo všechny operace, včetně možnost spravovat službu, vytváření a odstraňování indexy, indexování a zdrojů dat. Existují dva klíče, takže můžete dál používat klávesu sekundární, pokud se rozhodnete obnovit primární klíč a naopak.
 - *Dotaz klíče* udělit jen pro čtení přístup k indexy a dokumenty a jsou obvykle úměrně klientské aplikace, které problémů žádosti o hledání.

Pro účely dotaz na rejstřík můžete použít jednu z dotazu klíče. Správu klíčů lze také pro dotazy, ale použijte klíč dotazu v kódu aplikace [Princip nejnižších možných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege)to lépe takto.

## <a name="ii-formulate-your-query"></a>II. Formulace dotazu
Existují dva způsoby, jak [Hledat index pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx). Jedním ze způsobů je o vystavení žádost HTTP POST kde bude je to možné definovat parametry dotazu v objektu JSON v hlavním textu žádosti o. Jiným způsobem je o vystavení žádost HTTP GET, kde bude je to možné definovat parametry dotazu v rámci požadavku na adresu URL. Všimněte si, že má příspěvek víc [zmírněny limity](https://msdn.microsoft.com/library/azure/dn798927.aspx) velikosti parametry dotazu než získat. Proto doporučujeme používat příspěvek pouze tehdy, je speciální okolností pomocí GET kde bude vhodnější.

PŘÍSPĚVEK a získat, budete muset zadat *název služby*, *index*a správné *verze rozhraní API* (aktuální verze rozhraní API `2015-02-28` při jeho publikování dokumentu) v požadavku na adresu URL. GET, *řetězce dotazu* na konci adresa URL bude místo, kam zadáte parametry dotazu. Formát adresy URL najdete níže:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Formát příspěvek je stejné, ale pomocí rozhraní api pouze verze v parametry řetězce dotazu.



#### <a name="example-queries"></a>Příklad dotazů

Tady je několik příklad dotazy na index s názvem "hotely". Tyto dotazy jsou zobrazeny ve formátu GET a publikovat.

Prohledávat celý index termínu "rozpočet" a vraťte se jenom `hotelName` pole:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Použití filtru indexu najít hotely levnější než 150 za noc a vraťte se `hotelId` a `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Prohledávat celý index order konkrétním poli (`lastRenovationDate`) v sestupném pořadí, prohlédněte výsledky dvě a zobrazit jenom `hotelName` a `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Odeslání žádosti o HTTP
Teď máte úpravě dotazu jako součást adresy URL žádost HTTP (pro získání) nebo textu (u příspěvku), můžete definovat žádost o záhlaví a odeslání dotazu.

#### <a name="request-and-request-headers"></a>Žádost a žádost o záhlaví
Dva záhlaví požadavků musíte definovat pro získání nebo tři příspěvku:
1. `api-key` Záhlaví musí být nastavený na klávesu dotazu jste našli v kroku můžu výše. Všimněte si, že můžete také kód Product key pro správu jako `api-key` záhlaví, ale doporučuje se, že používáte klíč dotazu jako výhradně zajišťuje přístup jen pro čtení na indexy a dokumenty.
2. `Accept` Záhlaví musí být nastavený na `application/json`.
3. Pouze příspěvku `Content-Type` záhlaví by měl nastavit taky `application/json`.

Žádost HTTP GET při vyhledávání v indexu "hotely" pomocí Azure hledání REST API pomocí jednoduchého dotazu, která vyhledává termín "model" najdete níže:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Tady je stejný dotaz příklad tentokrát pomocí HTTP příspěvku:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Žádost o úspěšný dotaz bude mít za následek stavový kód `200 OK` a výsledky jsou vráceny jako JSON v těle odpovědi. Tady je jaké výsledky nahoře query vypadají, za předpokladu, že index "hotely" vyplněné s ukázkovými daty v [Importu dat v Azure vyhledávání pomocí rozhraní REST API](search-import-data-rest-api.md) (Všimněte si, že ve formátu JSON byly naformátovány pro srozumitelnost).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Další informace, navštivte "Odpověď" část [Hledat dokumenty](https://msdn.microsoft.com/library/azure/dn798927.aspx). Další informace o kódech stavu jiných HTTP, které by mohly být vrácen když nepovede najdete v článku [kódy stav HTTP (Azure hledání)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
