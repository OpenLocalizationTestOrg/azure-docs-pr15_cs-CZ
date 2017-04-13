<properties
    pageTitle="Nahrajte dat Azure vyhledávání pomocí .NET SDK | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Informace o odeslání dat do indexu vyhledávání Azure pomocí .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Odeslání dat do Azure vyhledávání pomocí .NET SDK
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [ZBÝVAJÍCÍ](search-import-data-rest-api.md)

V tomto článku se dozvíte používání [Azure hledání .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) k importu dat do indexu vyhledávání Azure.

Před zahájením tohoto návodu, by už jste [vytvořili indexu vyhledávání Azure](search-what-is-an-index.md). Tento článek také předpokládá, že jste již vytvořili `SearchServiceClient` objektu, jak je vidět v tématu [Vytvoření indexu vyhledávání Azure pomocí .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).

Všimněte si, že všechny ukázkový kód v tomto článku je napsaný v jazyce C#. Můžete najít celou zdroj kód [na GitHub](http://aka.ms/search-dotnet-howto).

Abyste mohli použít dokumenty do indexu pomocí .NET SDK, musíte:

  1. Vytvoření `SearchIndexClient` objekt, který chcete připojit k vyhledávacím odkazu.
  2. Vytvoření `IndexBatch` obsahující dokumenty, které budou přidány, upravit nebo odstranit.
  3. Volání `Documents.Index` metodu vaše `SearchIndexClient` odeslat `IndexBatch` do indexu vyhledávání.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Můžu. Vytvoření instance třídy SearchIndexClient
K importu dat do indexu pomocí .NET SDK vyhledávací Azure, musíte se vytvořit instance `SearchIndexClient` předmětu. Můžete vytvořit tuto instanci sami, ale jeho snadněji Pokud už máte `SearchServiceClient` instance volání jeho `Indexes.GetClient` metody. Tady je například jak by získáváte `SearchIndexClient` indexu s názvem "hotely" z `SearchServiceClient` s názvem `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] V aplikaci typické hledání je Správa indexu a populaci uskutečněných jednotlivými samostatnou komponentu z vyhledávacích dotazů. `Indexes.GetClient`je vhodné pro protože uloží potíže při poskytování jiný naplňovat indexu `SearchCredentials`. Je to předáním klávesu správce, který jste použili k vytvoření `SearchServiceClient` do nového `SearchIndexClient`. V části aplikace, které provede dotazy, je však lepší vytvořit `SearchIndexClient` přímo tak, aby předáte dotazu klíče místo klávesy správce. To je v souladu s [Princip nejnižších možných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) a pomůže aby byla aplikace líp zabezpečená. Můžete zjistit víc informací o správy klíčů a dotaz v [rozhraní REST API pro hledání Azure odkaz na webu MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`má `Documents` vlastnost. Tato vlastnost poskytuje všechny metody, pomocí kterých budete muset přidat, upravit, odstranění nebo dotaz dokumenty ve odkazu.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Rozhodněte, které indexování použití akce
K importu dat pomocí .NET SDK, budete muset zabalit dat do `IndexBatch` objektu. `IndexBatch` Zapouzdřuje kolekci `IndexAction` objektů, z nichž každá obsahuje dokument a vlastnost, která sděluje Azure vyhledávání, jaké akce můžete provádět na tento dokument (nahrát, hromadné korespondence, odstranit a další). Podle toho, které pod akce se rozhodnete, jen některé pole musí být zahrnuta pro každý dokument:

Akce | Popis | Příslušná pole pro každý dokument | Poznámky
--- | --- | --- | ---
`Upload` | `Upload` Akce je podobná "upsert", kde vložen, pokud je nová a aktualizovat nebo jiný nahradil pokud existuje dokumentu. | klávesy a dalších polí, které chcete definovat | Při aktualizaci/nahrazení existujícího dokumentu libovolného pole, které není uvedené v žádosti o bude mít jeho je pole nastaveno na `null`. K tomu dojde, i když pole byla dříve nastavena na hodnotu než null.
`Merge` | Aktualizuje existujícího dokumentu zadané pole. Pokud dokument v indexu neexistuje, se nepovede hromadné korespondence. | klávesy a dalších polí, které chcete definovat | Pole, které zadáte do sloučení nahradí existující pole v dokumentu. Jedná se o pole typu `DataType.Collection(DataType.String)`. Například, pokud dokument obsahuje pole `tags` s hodnotou `["budget"]` a nutná ke sloučení s hodnotou `["economy", "pool"]` pro `tags`, výsledná hodnota `tags` pole bude `["economy", "pool"]`. Nesmí být `["budget", "economy", "pool"]`.
`MergeOrUpload` | Tato akce se chová jako funkce `Merge` Pokud dokumentu s konci daného klíč již existuje v indexu. Pokud dokument neexistuje, se chová jako `Upload` s novým dokumentem. | klávesy a dalších polí, které chcete definovat | -
`Delete` | Odebere zadaný dokument z indexu. | pouze klíč | Všechna pole zadáte jiný než pole klíče budou ignorovat. Pokud chcete z dokumentu odebrat jednotlivá pole, použijte `Merge` místo a jednoduše pole explicitně nastavena na hodnotu null.

Můžete určit, jakou akci, kterou chcete použít s různými metodami statické `IndexBatch` a `IndexAction` třídy, jak je uvedeno v následující části.

## <a name="iii-construct-your-indexbatch"></a>III. Vytvářet svůj IndexBatch
Nyní již znáte akcí, které provádět v dokumentech, jste připraveni k vytvoření `IndexBatch`. Následující příklad ukazuje, jak vytvoříte list s několika různé akce. Všimněte si, že náš příklad používá vlastní třídy nazvané `Hotel` , která odpovídá dokumentu na rejstřík "hotely".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

V tomto případě používáme `Upload`, `MergeOrUpload`, a `Delete` jako naše akce hledání podle metody s názvem na `IndexAction` předmětu.

Se předpokládá, že tento index "hotely" Příklad už vyplněné počet dokumentů. Všimněte si, jak jsme není nutné zadávat všechna pole možné dokumentů při použití `MergeOrUpload` a jak jsme pouze klíč dokumentu (`HotelId`) při použití `Delete`.

Všimněte si také, můžete zahrnout pouze až 1000 dokumenty v jednom indexování požadavku.

> [AZURE.NOTE] V tomto příkladu jsme do jiných dokumentů použít různé akce. Pokud byste chtěli provádět stejné akce ve všech dokumentech dávku místo volání `IndexBatch.New`, můžete použít jiné statické metody `IndexBatch`. Můžete například vytvořit listy tak, že zavoláte `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, nebo `IndexBatch.Delete`. Tyto metody trvat sbírky sledovaných dokumentů (objekty typu `Hotel` v tomto příkladu) místo `IndexAction` objekty.

## <a name="iv-import-data-to-the-index"></a>IV. Import dat do indexu
Teď, když máte spuštění `IndexBatch` objektu, můžete ji odešlete index tak, že zavoláte `Documents.Index` na vaše `SearchIndexClient` objektu. Následující příklad ukazuje, jak volat `Index`, stejně jako některé dodatečné kroky budete muset provést:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Poznámka `try` / `catch` kolem volání `Index` metody. Blokování skutečné zpracovává případu důležité chyby indexování. Pokud Azure vyhledávací služba přestane indexovat některé dokumenty v listu, `IndexBatchException` vyvolá `Documents.Index`. Tomu může dojít, pokud indexujete dokumenty při velkém zatížení služby. **Důrazně doporučujeme explicitně zpracování tento případ v kódu.** Zpoždění a opakujte indexování dokumenty, které se nepodařilo nebo můžete zaznamenat a pokračovat jako neobsahuje vzorku, nebo můžete udělat něco jiného v závislosti na požadavky konzistence dat aplikace.

Nakonec kód v příkladu výše zpoždění dobu 2 sekund. Indexování neděje asynchronní služby Azure hledání tak, aby aplikace ukázkové potřebuje chvíli k zajištění k dispozici pro hledání dokumentů Zpoždění takto splněny obvykle jenom v ukázky, testů a ukázkové aplikace.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Zpracování dokumentů .NET SDK

Budete možná vás zajímá, jak se moci uložit instance uživatelem definovaných třídy jako .NET SDK vyhledávací Azure `Hotel` do indexu. Pokud chcete odpovědět na tuto otázku, Pojďme se podívat `Hotel` třídy, která odpovídá schématu index definované v tématu [Vytvoření indexu vyhledávání Azure pomocí .NET SDK](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Prvním krokem Všimněte si, je každý veřejné vlastnost `Hotel` odpovídá poli v definici index, ale s jednu zásadní rozdíl: název každého pole začíná malé písmeno ("velbloudí případ"), při název každého veřejné vlastnosti `Hotel` začíná velké písmeno ("Pascal písmeny na začátku"). Toto je běžné situace v aplikacích .NET provádějící vazby dat, kde je cílového schématu mimo ovládací prvek vývojář aplikace. Namísto porušovat .NET pojmenování pokyny tím, že vlastnost názvy velbloudů písmena, můžete to poznat SDK mapování názvů vlastnost k velbloudů písmena automaticky pomocí `[SerializePropertyNamesAsCamelCase]` atribut.

> [AZURE.NOTE] .NET SDK vyhledávací Azure používá knihovnu [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) serializovat a deserializovat objekty vlastní modelu do a z JSON. V případě potřeby můžete přizpůsobit tento serializace. Další informace najdete v [inovace Azure hledání .NET SDK verze 1.1](search-dotnet-sdk-migration.md#WhatsNew). Příkladem to je použití `[JsonProperty]` atributů na `DescriptionFr` vlastnost ve výše uvedené ukázkové kódu.

Druhá důležité pokyny `Hotel` třídy jsou datové typy veřejné vlastnosti. Typy .NET tyto mapování vlastností a jejich typů odpovídající pole v definici indexu. Například `Category` řetězec vlastnost map `category` pole, která je typu `DataType.String`. Jsou podobné typ mapování mezi `bool?` a `DataType.Boolean`, `DateTimeOffset?` a `DataType.DateTimeOffset`atd. Zvláštní pravidla pro mapování typ jsou popsány s `Documents.Get` metoda na [webu MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Tato možnost používat vlastní třídy jako dokumentů funguje v obou směrech; Můžete taky výsledky hledání mají načítat a nechat SDK automaticky rekonstruovat, aby typu podle svého výběru, jak je vidět v následujícím [článku další](search-query-dotnet.md).

> [AZURE.NOTE] .NET SDK vyhledávací Azure taky podporuje dynamicky zadali dokumentů pomocí `Document` třídy, která je klíč/hodnota mapování názvů polí do pole hodnoty. Toto je užitečný v případech, nevíte schématu indexu v době návrhu nebo pokud je nevhodné se vytvořit vazbu s třídy konkrétnímu modelu. Všechny metody v SDK, která zacházet s dokumenty mají přetížení, které spolupracují s `Document` předmětu, jakož i silného typu přetížení, které přebírají obecný typ parametrů. Pouze ten se používají v ukázkovém kódu v tomto článku. Můžete zjistit informace o `Document` třídy [na webu MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Důležitá poznámka o datových typech**

Při návrhu vlastního modelu třídy mapovat do indexu vyhledávání Azure, doporučujeme, abyste deklarace vlastnosti hodnotou typů jako `bool` a `int` být s možnou hodnotou Null (například `bool?` namísto `bool`). Pokud používáte bez-s možnou hodnotou Null vlastnost, budete muset **zaručit** , že nejsou žádné dokumenty ve odkazu obsahovat hodnotu null pro odpovídající pole. V SDK ani Azure vyhledávací služba se můžete k jejímu vynucení to.

Toto není právě hypotetické problém: Představte si situace místo, kam přidat nové pole do existující index, který je typu `DataType.Int32`. Po aktualizaci definici indexu, budou všechny dokumenty obsahovat hodnotu null pro toto nové pole (protože se všechny typy s možnou hodnotou Null v Azure hledání). Pokud použijete třída modelu s jiných-s možnou hodnotou Null `int` vlastnost pro toto pole, zobrazí se vám `JsonSerializationException` při pokus o načtení vlastnosti dokumentů takto:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Proto doporučujeme použít s možnou hodnotou Null typy tříd modelu osvědčený.

## <a name="next"></a>Další
Po naplnění Azure vyhledávacím odkazu, budou do ní začít vydání dotazy pro hledání dokumentů. Další informace najdete v článku [Indexu vyhledávání Azure svůj dotaz](search-query-overview.md) .
