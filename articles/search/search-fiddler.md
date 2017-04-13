<properties
    pageTitle="Jak používat k vyhodnocování a testování Azure hledání REST API Fiddler | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Použití Fiddler kód bez přístupu k Kontrola dostupnosti Azure vyhledávání a vyzkoušet rozhraní REST API."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Použití Fiddler vyhodnocení a otestovat Azure hledání REST API
> [AZURE.SELECTOR]
- [Základní informace](search-query-overview.md)
- [Průzkumník hledání](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [ZBÝVAJÍCÍ](search-query-rest-api.md)

Tento článek vysvětluje, jak používat Fiddler [zdarma stáhnout z Telerik](http://www.telerik.com/fiddler), problém HTTP požadavky a odpovědi zobrazení pomocí Azure hledání rozhraní REST API, aniž byste museli psát jakýkoli kód. Azure Hledat je plně Správa přístupových práv a hostované cloudu vyhledávací služby na Microsoft Azure snadno programovatelnou prostřednictvím .NET a rozhraní REST API. Azure vyhledávací služba REST API jsou popsány na [webu MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

V následujících krocích budete vytvořit index, nahrajte dokumenty, dotaz rejstřík a dotaz systémové informace o službách.

Tyto kroky, budete potřebovat Azure vyhledávací služby a `api-key`. Další informace o tom, jak začít pracovat v tématu [Vytvoření Azure vyhledávací služby na portálu](search-create-service-portal.md) .

## <a name="create-an-index"></a>Vytvoření indexu

1. Spusťte Fiddler. V nabídce **soubor** vypněte **Zachytit přenosy** skrýt nadbytečné HTTP činnosti, které nesouvisí s aktuálního úkolu.

3. Na kartě **autora** budete formulace žádost, která vypadá jako následující obrazovka snímek.

    ![][1]

2. Vyberte **umístění**.

3. Zadejte adresu URL, která určuje adresy URL služby, atributy žádost a verze rozhraní api. Tipy na paměti:
   + Použijte HTTPS jako předponu.
   + Žádost o atribut je "/ indexy/hotely". Toto říká hledání při vytváření rejstříku s názvem "hotely".
   + Verze rozhraní API je malá písmena, zadali jako "? verze rozhraní api = 2015 02 28". Rozhraní API jsou důležité, protože Azure hledání nasadí aktualizace pravidelně. Výjimečně se může způsobovat aktualizace služby ovlivňující změna rozhraní API. Z tohoto důvodu Azure vyhledávání vyžaduje službu verze rozhraní api na každou žádost tak, aby se úplnou kontrolu nad tím, který z nich je používají.

    Úplnou adresu URL by měla vypadat podobně jako v následujícím příkladu.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Zadejte záhlaví žádost o nahrazení Host (hostitel) a rozhraní api klíč hodnoty, které jsou platné službě.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  V žádosti o textu vložte do pole, které tvoří definici indexu.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klikněte na **Spustit**.

Ve chvíli byste měli vidět odpověď HTTP 201 v seznamu relace, že index byl vytvořen úspěšně.

Pokud se zobrazí HTTP 504, ověřte, že adresa URL určuje HTTPS. Pokud se zobrazí HTTP 400 404, zkontrolujte textu žádosti o ověřil, jestli že nedošlo k chybám zkopírovat a vložit. HTTP 403 obvykle znamená problém s rozhraním api klíč (neplatný klíč nebo syntaxe problém s jak není zadán klávesu rozhraní api).

## <a name="load-documents"></a>Načtení dokumentů

Na kartě **autora** vaši žádost o dokumenty publikovat bude vypadat takto: Textu žádosti o obsahuje vyhledávání dat 4 hotely.

   ![][2]

1. Vyberte **Publikovat**.

2.  Zadejte adresu URL, která následuje začíná HTTPS, následovaný svoji adresu URL služby "/indexes/ < Název_indexu >/dokumenty/index? verze rozhraní api = 2015 02 28". Úplnou adresu URL by měla vypadat podobně jako v následujícím příkladu.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Žádost o záhlaví by měl být stejný jako před. Mějte na paměti, s hodnotami, které jsou platné službě nahradit Host (hostitel) a rozhraní api klíče.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Žádost o text obsahuje čtyři dokumentů a přidávají do indexu hotely.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klikněte na **Spustit**.

Ve chvíli byste měli vidět odpověď HTTP 200 v seznamu relace. To znamená, že byly dokumenty vytvořené úspěšně. Pokud se zobrazí 207, nejméně jeden dokument nepovedlo se odeslat. Pokud se zobrazí 404, máte syntaktickou chybu v záhlaví nebo textu žádosti o.

## <a name="query-the-index"></a>Dotaz rejstřík

Teď se nezavádějí rejstřík a dokumenty, můžete vydat dotazů je.  Na kartě **autora** bude vypadat podobně jako na následující snímek obrazovky příkaz **získat** dotazy služby.

   ![][3]

1.  Vyberte **získat**.

2.  Zadejte adresu URL, která následuje začíná HTTPS, následovaný svoji adresu URL služby "/indexes/ < Název_indexu > / dokumenty?", následovaný parametry dotazu. Jako příklad použijte následující adresu URL nahrazení název hostitele vzorku, který je platný službě.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Tento dotaz hledá termín "model" a obnovení kategorie podmínka pro hodnocení.

3.  Žádost o záhlaví by měl být stejný jako před. Mějte na paměti, s hodnotami, které jsou platné službě nahradit Host (hostitel) a rozhraní api klíče.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Kód odpovědi by měl být 200 a výstupu odpověď by měla vypadat podobně jako na následující obrazovce snímek.

   ![][4]

Dotaz v následujícím příkladu je z [indexu vyhledávání operací (hledání Azure rozhraní API)](http://msdn.microsoft.com/library/dn798927.aspx) na webu MSDN. Počet dotazů příklad v tomto tématu obsahovat mezery, které nejsou povoleny v Fiddler. Nahradit každý space s + znak před vložením do dotazu řetězec před pokusem o dotazu v Fiddler.

**Před mezery nahrazují:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Po mezery nahrazeny +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Dotaz systém

Můžete taky dotaz systému zobrazíte počty dokumentu a spotřeby úložiště. Na kartě **autora** bude vypadat podobně jako následující vaši žádost a odpověď vrátí celkový počet dokumentů a místo, které používají.

 ![][5]

1.  Vyberte **získat**.

2.  Zadejte adresu URL, která obsahuje váš adresy URL služby, následovaný "/ indexy/hotely/stat? verze rozhraní api = 2015 02 28":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Zadejte záhlaví žádost o nahrazení Host (hostitel) a rozhraní api klíč hodnoty, které jsou platné službě.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Nechte požadavku prázdné.

5.  Klikněte na **Spustit**. Měli byste vidět kód stavu HTTP 200 v seznamu relace. Vyberte položku k příkazu.

6.  Klikněte na kartu **kontroly** , klikněte na kartu **záhlaví** a pak vyberte formátu JSON. Měli byste vidět velikost dokumentu počet a úložiště (ve znalostní bázi Knowledge Base).

## <a name="next-steps"></a>Další kroky

Přečtěte si téma [Správa vyhledávací služby na Azure](search-manage.md) pro bez použití kódu přístup pro správu a pomocí vyhledávání Azure.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
