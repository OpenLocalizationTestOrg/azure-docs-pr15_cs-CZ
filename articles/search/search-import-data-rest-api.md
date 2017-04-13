<properties
    pageTitle="Nahrajte dat Azure vyhledávání pomocí rozhraní REST API | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Informace o odeslání dat do indexu vyhledávání Azure pomocí rozhraní REST API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Odeslání dat do Azure vyhledávání pomocí rozhraní REST API
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [ZBÝVAJÍCÍ](search-import-data-rest-api.md)

V tomto článku se dozvíte používání [Rozhraní REST API pro hledání Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx) k importu dat do indexu vyhledávání Azure.

Před zahájením tohoto návodu, by už jste [vytvořili indexu vyhledávání Azure](search-what-is-an-index.md).

Abyste mohli použít dokumenty do indexu pomocí rozhraní REST API, vydá žádost HTTP POST koncový bod adresu URL svého index. Text požadavek HTTP je objekt JSON obsahující dokumentů, které mají přidat, upravit nebo odstranit.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Můžu. Identifikaci Správce služby Azure hledání rozhraní api klíče
Při vydání požadavků HTTP proti služby pomocí rozhraní REST API, musí obsahovat *každý* rozhraní API požadavek rozhraní api klávesu vygenerovaný pro vyhledávací službu, kterou jste zřízení. Máte platný klíč vytváří vztah důvěryhodnosti na základě na žádost o mezi aplikací odesláním žádosti a na službu, zpracuje ji.

1. K vyhledání rozhraní api klíče vaše služby musíte se přihlásit na [Portál Azure](https://portal.azure.com/)
2. Přejděte na zásuvné Azure vyhledávací služby
3. Klikněte na ikonu "Klávesy"

Služby bude mít *správy klíčů* a *dotazů*.

  - Primární a sekundární *Správce klíče* udělit plné právo všechny operace, včetně možnost spravovat službu, vytváření a odstraňování indexy, indexování a zdrojů dat. Existují dva klíče, takže můžete dál používat klávesu sekundární, pokud se rozhodnete obnovit primární klíč a naopak.
  - *Dotaz klíče* udělit jen pro čtení přístup k indexy a dokumenty a jsou obvykle úměrně klientské aplikace, které vydávat žádosti o hledání.

Pro účely importu dat do rejstříku, můžete použít buď kód primární a sekundární správce.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Rozhodněte, které indexování použití akce
Při používání rozhraní REST API, budete vydávat požadavky HTTP POST s JSON žádost o orgány do indexu vyhledávání Azure adresa URL koncového bodu. JSON objektu ve vlastní obsah žádost HTTP bude obsahovat jedním maticovým JSON s názvem "hodnotu" obsahující JSON objekty, které představují dokumenty, které chcete přidat do indexu, aktualizace nebo odstranění.

Jednotlivé objekty JSON v poli "hodnotu" představuje dokument chcete indexovat. Každý z těchto objektů obsahuje klíč dokumentu a určuje požadovanou akci indexování (nahrát, hromadné korespondence, odstranit a další). Podle toho, které pod akce se rozhodnete, jen některé pole musí být zahrnuta pro každý dokument:

@search.action | Popis | Příslušná pole pro každý dokument | Poznámky
--- | --- | --- | ---
`upload` | `upload` Akce je podobná "upsert", kde vložen, pokud je nové a aktualizovat nebo jiný nahradil pokud existuje dokumentu. | klávesy a dalších polí, které chcete definovat | Při aktualizaci/nahrazení existujícího dokumentu libovolného pole, které není uvedené v žádosti o bude mít jeho je pole nastaveno na `null`. K tomu dojde, i když pole byla dříve nastavena na hodnotu než null.
`merge` | Aktualizuje existujícího dokumentu zadané pole. Pokud dokument v indexu neexistuje, se nepovede hromadné korespondence. | klávesy a dalších polí, které chcete definovat | Pole, které zadáte do sloučení nahradí existující pole v dokumentu. Jedná se o pole typu `Collection(Edm.String)`. Například, pokud dokument obsahuje pole `tags` s hodnotou `["budget"]` a nutná ke sloučení s hodnotou `["economy", "pool"]` pro `tags`, výsledná hodnota `tags` pole bude `["economy", "pool"]`. Nesmí být `["budget", "economy", "pool"]`.
`mergeOrUpload` | Tato akce se chová jako funkce `merge` Pokud dokumentu s konci daného klíč již existuje v indexu. Pokud dokument neexistuje, se chová jako `upload` s novým dokumentem. | klávesy a dalších polí, které chcete definovat | -
`delete` | Odebere zadaný dokument z indexu. | pouze klíč | Všechna pole zadáte jiný než pole klíče budou ignorovat. Pokud chcete z dokumentu odebrat jednotlivá pole, použijte `merge` místo a jednoduše pole explicitně nastavena na hodnotu null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Vytvoření žádosti o HTTP a požádat o textu
Jste shromáždili hodnot potřebné polí pro index akce, jste připraveni k vytvoření skutečnou žádost HTTP a textu žádosti o JSON naimportovat data z.

#### <a name="request-and-request-headers"></a>Žádost a žádost o záhlaví
Do pole Adresa URL, budete muset zadat název služby, název indexu ("hotely" v tomto případě), jakož i správné verze rozhraní API (aktuální verze rozhraní API `2015-02-28` při jeho publikování tohoto dokumentu). Potřebujete definovat `Content-Type` a `api-key` požádat o záhlaví. Pro ten použijte jeden z vaší služby správy klíčů.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Žádost o textu

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

V tomto případě používáme `upload`, `mergeOrUpload`, a `delete` jako naše akce hledání.

Se předpokládá, že tento index "hotely" Příklad už vyplněné počet dokumentů. Všimněte si, jak jsme není nutné zadávat všechna pole možné dokumentů při použití `mergeOrUpload` a jak jsme pouze klíč dokumentu (`hotelId`) při použití `delete`.

Všimněte si také, můžete zahrnout pouze až 1000 dokumentů (nebo 16 MB) v jednom indexování požadavku.

## <a name="iv-understand-your-http-response-code"></a>IV. Pochopení kódu odpověď HTTP
#### <a name="200"></a>200
Po odeslání žádosti o úspěšné indexování obdržíte odpověď HTTP s kódem stav `200 OK`. JSON textu odpověď HTTP bude mít takto:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Stavový kód `207` , bude vrácena při alespoň jednu položku nebyla úspěšně indexována. Obsah JSON odpovědi na HTTP bude obsahovat informace o neúspěšných dokumenty.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Často znamená to, že zatížení vyhledávací služba sahající bodu, kde indexování žádosti o začne se chcete vrátit `503` odpovědi. V tomto případě důrazně doporučujeme, kód klienta regrese a čekat, než. To vám umožní systému chvíli Pokud chcete obnovit, zvýšení pravděpodobnost, že budoucí požadavky proběhne úspěšně. Rychle opakování žádostech pouze prodloužit situace.

#### <a name="429"></a>429
Stavový kód `429` budou vráceny se překročení kvóty na počet dokumentů na index.

#### <a name="503"></a>503
Stavový kód `503` , bude vrácena pokud žádná z položky v žádosti o byly úspěšně indexovat. Tato chyba znamená, že je systém při velkém zatížení a v současné době nejde zpracovat žádosti o.

> [AZURE.NOTE] V tomto případě důrazně doporučujeme, kód klienta regrese a čekat, než. To vám umožní systému chvíli Pokud chcete obnovit, zvýšení pravděpodobnost, že budoucí požadavky proběhne úspěšně. Rychle opakování žádostech pouze prodloužit situace.

Další informace o dokumentu akce a odpovědi úspěch/chyby najdete v článku [Přidání, aktualizace nebo odstranění dokumentů](https://msdn.microsoft.com/library/azure/dn798930.aspx). Další informace o kódech stavu jiných HTTP, které by mohly být vrácen když nepovede najdete v článku [kódy stav HTTP (Azure hledání)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Další
Po naplnění Azure vyhledávacím odkazu, budou do ní začít vydání dotazy pro hledání dokumentů. Další informace najdete v článku [Indexu vyhledávání Azure svůj dotaz](search-query-overview.md) .
