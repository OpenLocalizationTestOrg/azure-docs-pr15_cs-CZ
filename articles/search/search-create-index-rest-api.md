<properties
    pageTitle="Vytvoření indexu vyhledávání Azure pomocí rozhraní REST API | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Vytvoření indexu v kódu pomocí rozhraní REST API Azure hledání HTTP."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Vytvoření indexu vyhledávání Azure pomocí rozhraní REST API
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [ZBÝVAJÍCÍ](search-create-index-rest-api.md)


Tento článek vás provede procesem vytváření Azure hledání [indexu](https://msdn.microsoft.com/library/azure/dn798941.aspx) pomocí rozhraní REST API pro hledání Azure.

Před sledováním tohoto průvodce a vytváření rejstříku, měli byste mít již [vytvořili Azure vyhledávací služby](search-create-service-portal.md).

Vytvoření indexu vyhledávání Azure pomocí rozhraní REST API, vydá jednu žádost HTTP POST koncový bod adresy URL služby Azure vyhledávání. Definice indexu budou obsaženy v hlavním textu žádosti o jako ve správném formátu JSON obsah.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Můžu. Identifikaci Správce služby Azure hledání rozhraní api klíče
Teď máte zřízení služby Azure hledání, je vydat požadavků HTTP proti koncový bod adresu URL svého služby pomocí rozhraní REST API. Však *všechny* rozhraní API požadavky musí obsahovat rozhraní api klávesu vygenerovaný pro vyhledávací službu, kterou jste zřízení. Máte platný klíč zavádí zabezpečení na základě na žádost o mezi aplikací odesláním žádosti a na službu, zpracuje ji.

1. K vyhledání rozhraní api klíče vaše služby musíte se přihlásit na [Portál Azure](https://portal.azure.com/)
2. Přejděte na zásuvné Azure vyhledávací služby
3. Klikněte na ikonu "Klávesy"

Služby bude mít *správy klíčů* a *dotazů*.

 - Primární a sekundární *Správce klíče* udělit plné právo všechny operace, včetně možnost spravovat službu, vytváření a odstraňování indexy, indexování a zdrojů dat. Existují dva klíče, takže můžete dál používat klávesu sekundární, pokud se rozhodnete obnovit primární klíč a naopak.
 - *Dotaz klíče* udělit jen pro čtení přístup k indexy a dokumenty a jsou obvykle úměrně klientské aplikace, které problémů žádosti o hledání.

Pro účely vytvoření rejstříku, můžete použít buď kód primární a sekundární správce.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Definování Azure vyhledávacím odkazu pomocí ve správném formátu JSON
Žádost HTTP POST jednoho ke službě vytvoříte rejstřík. Textu vaši žádost HTTP POST bude obsahovat jediný JSON objekt, který definuje Azure vyhledávacím odkazu.

1. Na první vlastnost tento objekt JSON je název vaší indexu.
2. Druhá vlastnost tento objekt JSON je JSON pole s názvem `fields` , která obsahuje samostatný objekt JSON pro jednotlivá pole v rejstříku. Každý z těchto objektů JSON obsahovat víc dvojic název hodnota pro jednotlivá pole atributy včetně "název", "typ" atd.

Je důležité, aby byl vašich hledání uživatelské prostředí a obchodních potřeb myslet při návrhu index při každé pole, musí mít přiřazené [správné atributy](https://msdn.microsoft.com/library/azure/dn798941.aspx). Platí pro pole, která tyto atributy ovládací prvek, který hledání funkcí (filtrování, faceting řazení fulltextové vyhledávání, atd.). U všech atributů, které nezadáte výchozí bude povolit odpovídající funkce vyhledávání, pokud ho konkrétně zakázat.

Zpět k našemu příkladu jsme jste s názvem náš index "hotely" a naše polí definovaných následujícím způsobem:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Pečlivě zvolili atributy indexu u každého pole podle jak jsme myslíte, že se použije v aplikaci. Například `hotelId` je jedinečný klíč, aby uživatelé hledání hotely pravděpodobně nebudou vědět, abyste deaktivujeme, když fulltextové vyhledávání tohoto pole nastavením `searchable` k `false`, která ukládá místa v indexu.

Dejte pozor právě jeden pole ve odkazu typu `Edm.String` musí být označeny jako pole "klávesy".

Definici indexu pomocí analyzátoru vlastní jazyk pro `description_fr` polí, protože je určená pro uložení francouzsky textu. V tématu [jazyk podporují téma na webu MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) stejně jako odpovídající [příspěvek blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/) pro další informace o jazyce analyzátory.

## <a name="iii-issue-the-http-request"></a>III. Žádost o problém HTTP
1. Při použití definice indexu v těle zprávy žádost o vydat svoji adresu URL koncového bodu služby Azure hledání žádost HTTP příspěvek. Do pole Adresa URL, je potřeba použít vlastní název služby jako název hostitele a kde si ji správné `api-version` jako parametru řetězce dotazu (aktuální verze rozhraní API `2015-02-28` při jeho publikování dokumentu).
2. V žádosti o záhlaví, zadejte `Content-Type` jako `application/json`. Bude taky musíte zadat klíč správce vaší služby, který jste určili v kroku I v `api-key` záhlaví.


Bude nutné zadat vlastní služby název a rozhraní api klíč o vystavení žádost níže:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Pro úspěšné žádost o potvrzení byste měli vidět stavový kód 201 (vytvořeno). Další informace o vytváření rejstříku prostřednictvím rozhraní REST API navštivte rozhraní API odkaz na [webu MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx). Další informace o kódech stavu jiných HTTP, které by mohly být vrácen když nepovede najdete v článku [kódy stav HTTP (Azure hledání)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Až budete mít s rejstřík a chcete odstranit, stačí zadejte žádost HTTP odstranit. To je například jak chcete odstranit index "hotely":

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Další
Po vytvoření indexu vyhledávání Azure, bude připravená k [odeslání obsahu do indexu](search-what-is-data-import.md) tak hledat data.
