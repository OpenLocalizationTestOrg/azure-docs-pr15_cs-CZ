<properties
   pageTitle="Jak používat Azure hledání z aplikace .NET | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
   description="Jak používat Azure hledání z aplikace pro .NET"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="10/06/2016"
   ms.author="brjohnst"/>

# <a name="how-to-use-azure-search-from-a-net-application"></a>Jak používat Azure hledání z aplikace pro .NET

Tento článek je návod pomůžou do začátků s [Azure hledání .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx). .NET SDK můžete provádět bohaté vyhledávání v aplikaci pomocí vyhledávání Azure.

## <a name="whats-in-the-azure-search-sdk"></a>Co je v Azure hledání SDK

V SDK se skládá z knihovny klienta `Microsoft.Azure.Search`. Umožňuje spravovat indexy, zdrojů dat a indexování, jakož i nahrát spravovat dokumenty a spouštění dotazů, aniž by bylo nutné zacházet s podrobnými HTTP a JSON.

Knihovna klienta definuje třídy jako `Index`, `Field`, a `Document`, stejně jako třeba operace `Indexes.Create` a `Documents.Search` na `SearchServiceClient` a `SearchIndexClient` třídy. Tyto třídy jsou uspořádány do následující obory názvů:

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

Aktuální verzi Azure hledání .NET SDK neexistuje teď obecně. Pokud chcete svůj názor nemůžeme začlenit do příští verzi, navštivte naše [stránka pro zadání názoru](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK podporuje verzi `2015-02-28` Azure hledání REST API uvedených na [webu MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx). Tato verze nyní podporuje Lucene syntaxi dotazu a analyzátory jazyka Microsoft. Nová funkce, které *není* součástí této verzi je třeba podpora `moreLikeThis` hledat parametr, jsou [preview](search-api-2015-02-28-preview.md) v dosud není k dispozici v sadě SDK. Můžete zkontrolovat, že znovu zapněte [Vyhledávací služby správy verzí](https://msdn.microsoft.com/library/azure/dn864560.aspx) pro aktualizací stavu na těchto funkcí.

Další funkce nejsou podporované v této SDK:

  - [Operace správy](https://msdn.microsoft.com/library/azure/dn832684.aspx). Správa operace patří zřízení služby Azure hledání a správy klíčů rozhraní API. Tyto se nebude podporovat v SDK .NET správy sady samostatném Azure vyhledávání v budoucnu.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Upgrade na nejnovější verzi sady SDK

Pokud už používáte starší verzi Azure hledání .NET SDK a chcete upgradovat na novou obecně dostupných verzí, [Tento článek](search-dotnet-sdk-migration.md) vysvětluje, jak.

## <a name="requirements-for-the-sdk"></a>Požadavky pro SDK

1. Visual Studio 2013 nebo Visual Studio 2015.

2. Vlastní Azure vyhledávací služby serveru. Abyste mohli používat v SDK, budete potřebovat název služby a jeden nebo více kláves rozhraní API. [Vytvoření služby na portálu](search-create-service-portal.md) vám pomůže použitím tohoto postupu.

3. Stáhněte Azure hledání .NET SDK [NuGet balíčku](http://www.nuget.org/packages/Microsoft.Azure.Search) pomocí "Správa NuGet balíčků" ve Visual Studiu. Jenom vyhledejte název balíčku `Microsoft.Azure.Search` na NuGet.org.

.NET SDK vyhledávací Azure podporuje aplikací pro .NET Framework 4.5, jakož i aplikace pro Windows Store zacílení Windows 8.1 a Windows Phone 8.1. Silverlight nepodporuje.

## <a name="core-scenarios"></a>Základní scénáře

Existuje několik věcí, které musíte udělat v aplikaci Vyhledávací služby. V tomto kurzu budeme se zabývat těmito oblastmi podobnému sledu core:

- Vytvoření rejstříku
- Vyplnění index s dokumenty
- Hledání dokumentů pomocí fulltextové vyhledávání a filtry

Ukázka kód, který následuje ukazuje každou z nich. Neváhejte fragmenty kódu použít vlastní aplikaci.

### <a name="overview"></a>Základní informace

Ukázková aplikace jsme budete prozkoumání vytvoří nový index s názvem "hotely" naplní několik dokumentů a pak provede některé vyhledávacích dotazů. Tady je hlavní program zobrazující celkový tok:

    // This sample shows how to delete, create, upload documents and query an index
    static void Main(string[] args)
    {
        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();
    }

Budete projdeme tento krok za krokem. Nejdřív potřeba vytvořit novou `SearchServiceClient`. Tento objekt umožňuje spravovat indexy. Za účelem vytvoření jednu, budete muset zadat název služby Azure vyhledávání, stejně jako správce rozhraní API klíče.

        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

> [AZURE.NOTE] Pokud zadáte nesprávné klávesy (například dotazu klíč kdy byla požadována klíč správce), `SearchServiceClient` vyvolají `CloudException` s chybou zpráva "Zakázáno" při prvním voláte metodu operace, například `Indexes.Create`. V takovém případě vám zkontrolujte naše rozhraní API klíče.

Další pár řádků volat metody při vytváření rejstříku s názvem "hotely", nejdřív jej odstraní, pokud již existuje. Bude projdeme těchto postupů trochu později.

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

Pak indexu musí žádné. K tomuto účelu potřebujeme `SearchIndexClient`. Získat dvěma způsoby: pomocí ho nebo tak, že zavoláte `Indexes.GetClient` na `SearchServiceClient`. Budeme používat ten pro usnadnění.

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

> [AZURE.NOTE] V aplikaci typické hledání je Správa indexu a populaci uskutečněných jednotlivými samostatnou komponentu z vyhledávacích dotazů. `Indexes.GetClient`je vhodné pro protože uloží potíže při poskytování jiný naplňovat indexu `SearchCredentials`. Je to předáním klávesu správce, který jste použili k vytvoření `SearchServiceClient` do nového `SearchIndexClient`. V části aplikace, které provede dotazy, je však lepší vytvořit `SearchIndexClient` přímo tak, aby předáte dotazu klíče místo klávesy správce. To je v souladu s Princip nejnižších možných oprávnění a pomůže aby byla aplikace líp zabezpečená. Můžete zjistit další informace o správy klíčů a dotaz [tady](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Teď, když máme `SearchIndexClient`, jsme naplnění index. Důvodem je jiným způsobem, který bude projdeme později.

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

Nakonec jsme provést několik vyhledávacích dotazů a zobrazení výsledků a znovu pomocí `SearchIndexClient`:

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();

Po spuštění této aplikace s platný název služby a rozhraní API klíč výstup by měl vypadat takto:

    Deleting index...

    Creating index...

    Uploading documents...

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []
    Complete.  Press any key to end application...

Úplné zdrojového kódu aplikace je k dispozici na konci tohoto článku.

Dále jsme se blíže podívejte na každý z metody volat `Main`.

### <a name="creating-an-index"></a>Vytvoření rejstříku

Po vytvoření `SearchServiceClient`, další možnost `Main` znamená je odstranit index "hotely", pokud již existuje. Který probíhá podle následujícího postupu:

    private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
    {
        if (serviceClient.Indexes.Exists("hotels"))
        {
            serviceClient.Indexes.Delete("hotels");
        }
    }

Tato metoda používá dané `SearchServiceClient` ke kontrole, pokud existuje index a pokud ano, odstraňte ji.

> [AZURE.NOTE] Příklad v tomto článku používá synchronní metody SDK Azure hledání .NET pro zjednodušení. Doporučujeme použít asynchronní metody svých aplikací ponechat stále scalable a citlivé. Například ve výše uvedené metody můžete použít `ExistsAsync` a `DeleteAsync` namísto `Exists` a `Delete`.

Další `Main` vytvoří nový index "hotely" tak, že zavoláte takto:

    private static void CreateHotelsIndex(SearchServiceClient serviceClient)
    {
        var definition = new Index()
        {
            Name = "hotels",
            Fields = new[]
            {
                new Field("hotelId", DataType.String)                       { IsKey = true },
                new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
            }
        };

        serviceClient.Indexes.Create(definition);
    }

Tento způsob vytvoří novou `Index` objekt s seznam `Field` objekty, které definuje schéma nový index. Každé pole má název, datový typ a několik atributy, které definují chování vyhledávání. Kromě toho pole můžete také přidat bodování profily, suggesters nebo CORS možnosti index (tyto vynechány ze vzorku neopakujeme). Další informace o objektu Index a jejích částí můžete najít SDK odkazu na [webu MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.index_members.aspx)i v [rozhraní REST API pro hledání Azure odkaz](https://msdn.microsoft.com/library/azure/dn798935.aspx).

### <a name="populating-the-index"></a>Vyplnění indexu

Další krok v `Main` je k naplnění index nově vytvořený. To je v následujícím způsobem:

    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var documents =
            new Hotel[]
            {
                new Hotel()
                {
                    HotelId = "1058-441",
                    HotelName = "Fancy Stay",
                    BaseRate = 199.0,
                    Category = "Luxury",
                    Tags = new[] { "pool", "view", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                    Rating = 5,
                    Location = GeographyPoint.Create(47.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "666-437",
                    HotelName = "Roach Motel",
                    BaseRate = 79.99,
                    Category = "Budget",
                    Tags = new[] { "motel", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                    Rating = 1,
                    Location = GeographyPoint.Create(49.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "970-501",
                    HotelName = "Econo-Stay",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "pool", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(46.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "956-532",
                    HotelName = "Express Rooms",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "wifi", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(48.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "566-518",
                    HotelName = "Surprisingly Expensive Suites",
                    BaseRate = 279.99,
                    Category = "Luxury",
                    ParkingIncluded = false
                }
            };

        try
        {
            var batch = IndexBatch.Upload(documents);
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

        // Wait a while for indexing to complete.
        Thread.Sleep(2000);
    }

Tento způsob zahrnuje čtyři části. První vytvoří matici `Hotel` objekty, které bude sloužit jako zadávání dat k nahrání na index. Tato data jsou pevně pro zjednodušení. Vlastní aplikace budou vaše data pravděpodobně pocházet z externího zdroje dat jako jsou databáze SQL.

Vytvoří druhé části `IndexBatch` obsahující dokumenty. Určení operace, které chcete použít s dávkou v době vytvoříte, v tomto případě tak, že zavoláte `IndexBatch.Upload`. Dávku potom nahrát do indexu vyhledávání Azure tak, že `Documents.Index` metody.

> [AZURE.NOTE] V tomto příkladu jsme jsou jenom odeslání dokumentů. Pokud byste chtěli proveďte sloučení změn do existující dokumenty a odstraňovat dokumenty, můžete vytvořit listy tak, že zavoláte `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, nebo `IndexBatch.Delete` místo. Je možné použít jiné operace v jedné dávce tak, že zavoláte `IndexBatch.New`, která trvá sbírka `IndexAction` objektů, z nichž každá říká Azure vyhledávání a pokusí se provedení operace konkrétní dokumentu. Můžete vytvořit každý `IndexAction` s vlastním operace voláním odpovídající metodu `IndexAction.Merge`, `IndexAction.Upload`a tak dál.

Třetí část tento způsob je skutečné blok, který pracuje s případu důležité chyby indexování. Pokud Azure vyhledávací služba přestane indexovat některé dokumenty v listu, `IndexBatchException` vyvolá `Documents.Index`. Tomu může dojít, pokud indexujete dokumenty při velkém zatížení služby. **Důrazně doporučujeme explicitně zpracování tento případ v kódu.** Zpoždění a opakujte indexování dokumenty, které se nepodařilo nebo můžete zaznamenat a pokračovat jako neobsahuje vzorku, nebo můžete udělat něco jiného v závislosti na požadavky konzistence dat aplikace.

Nakonec metoda zpoždění dobu 2 sekund. Indexování neděje asynchronní služby Azure hledání tak, aby aplikace ukázkové potřebuje chvíli k zajištění k dispozici pro hledání dokumentů Zpoždění takto splněny obvykle jenom v ukázky, testů a ukázkové aplikace.

#### <a name="how-the-net-sdk-handles-documents"></a>Zpracování dokumentů .NET SDK

Budete možná vás zajímá, jak se moci uložit instance uživatelem definovaných třídy jako .NET SDK vyhledávací Azure `Hotel` do indexu. Pokud chcete odpovědět na tuto otázku, Pojďme se podívat `Hotel` třídy:

    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }

Prvním krokem Všimněte si, je každý veřejné vlastnost `Hotel` odpovídá poli v definici index, ale s jednu zásadní rozdíl: název každého pole začíná malé písmeno ("velbloudí případ"), při název každého veřejné vlastnosti `Hotel` začíná velké písmeno ("Pascal písmeny na začátku"). Toto je běžné situace v aplikacích .NET provádějící vazby dat, kde je cílového schématu mimo ovládací prvek vývojář aplikace. Namísto porušovat .NET pojmenování pokyny tím, že vlastnost názvy velbloudů písmena, můžete to poznat SDK mapování názvů vlastnost k velbloudů písmena automaticky pomocí `[SerializePropertyNamesAsCamelCase]` atribut.

> [AZURE.NOTE] .NET SDK vyhledávací Azure používá knihovnu [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) serializovat a deserializovat objekty vlastní modelu do a z JSON. V případě potřeby můžete přizpůsobit tento serializace. Další podrobnosti najdete v tématu [Vlastní serializace s JSON.NET](#JsonDotNet).
 
Druhá důležité pokyny `Hotel` třídy jsou datové typy veřejné vlastnosti. Typy .NET tyto mapování vlastností a jejich typů odpovídající pole v definici indexu. Například `Category` řetězec vlastnost map `category` pole, která je typu `Edm.String`. Jsou podobné typ mapování mezi `bool?` a `Edm.Boolean`, `DateTimeOffset?` a `Edm.DateTimeOffset`atd. Zvláštní pravidla pro mapování typ jsou popsány s `Documents.Get` metoda na [webu MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Tato možnost používat vlastní třídy jako dokumentů funguje v obou směrech; Můžete taky výsledky hledání mají načítat a mít SDK automaticky rekonstruovat, aby typu podle svého výběru, jako jsme se zobrazí v další části.

> [AZURE.NOTE] .NET SDK vyhledávací Azure taky podporuje dynamicky zadali dokumentů pomocí `Document` třídy, která je klíč/hodnota mapování názvů polí do pole hodnoty. Toto je užitečný v případech, nevíte schématu indexu v době návrhu nebo pokud je nevhodné se vytvořit vazbu s třídy konkrétnímu modelu. Všechny metody v SDK, která zacházet s dokumenty mají přetížení, které spolupracují s `Document` předmětu, jakož i silného typu přetížení, které přebírají obecný typ parametrů. Pouze ten se používají v ukázkovém kódu v tomto kurzu. Můžete zjistit informace o `Document` třídy [tady](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Důležitá poznámka o datových typech**

Při návrhu vlastního modelu třídy namapujte do indexu vyhledávání Azure, doporučujeme, abyste deklarace vlastnosti hodnotou typů jako `bool` a `int` být s možnou hodnotou Null (například `bool?` namísto `bool`). Pokud používáte bez-s možnou hodnotou Null vlastnost, budete muset **zaručit** , že nejsou žádné dokumenty ve odkazu obsahovat hodnotu null pro odpovídající pole. V SDK ani Azure vyhledávací služba se můžete k jejímu vynucení to.

Toto není právě hypotetické problém: Představte si situace místo, kam přidat nové pole do existující index, který je typu `Edm.Int32`. Po aktualizaci definici indexu, budou všechny dokumenty obsahovat hodnotu null pro toto nové pole (protože se všechny typy s možnou hodnotou Null v Azure hledání). Pokud použijete třída modelu s jiných-s možnou hodnotou Null `int` vlastnost pro toto pole, zobrazí se vám `JsonSerializationException` při pokus o načtení vlastnosti dokumentů takto:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Proto doporučujeme použít s možnou hodnotou Null typy tříd modelu osvědčený.

<a name="JsonDotNet"></a>
#### <a name="custom-serialization-with-jsonnet"></a>Vlastní serializaci s JSON.NET

V SDK používá JSON.NET serializace a deserializace dokumenty. Můžete přizpůsobit serializace a deserializace v případě potřeby definovat vlastní `JsonConverter` nebo `IContractResolver` (viz [JSON.NET si přečtěte následující dokumentaci](http://www.newtonsoft.com/json/help/html/Introduction.htm) pro další podrobnosti). To může být užitečné, když budete chtít upravit existující třídy modelu z aplikace pro použití s Azure hledání a další pokročilejší scénáře. S vlastní serializaci můžete například:

 - Zahrnutí nebo vyloučení některé vlastnosti třídy modelu neukládaly jako pole dokumentu.
 - Namapujte mezi názvy vlastností v kódu a názvy polí ve odkazu.
 - Vytvoření vlastní atributy, které se dá použít pro mapování vlastností pole dokumentu jak vytváření odpovídající definice indexu.

Příklady implementace vlastního serializace v testy jednotek pro .NET SDK vyhledávací Azure na GitHub můžete najít. Vhodná výchozí hodnota je [Tato složka](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Obsahuje třídy, které využívají testy vlastní serializaci.

### <a name="searching-for-documents-in-the-index"></a>Hledání dokumentů v indexu

Poslední krok v ukázkové aplikaci je vyhledávat některé dokumenty v indexu. Podle pokynů pro dělá toto:

    private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
    {
        // Execute search based on search text and optional filter
        var sp = new SearchParameters();

        if (!String.IsNullOrEmpty(filter))
        {
            sp.Filter = filter;
        }

        DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
        foreach (SearchResult<Hotel> result in response.Results)
        {
            Console.WriteLine(result.Document);
        }
    }

Nejprve vytvoří novou tato metoda `SearchParameters` objektu. Slouží k určení další možnosti pro dotaz například řazení, filtrování, stránkování a faceting. V tomto příkladu jsme pouze stanovujete `Filter` vlastnost.

Dalším krokem je skutečně provést vyhledávacího dotazu. Důvodem je používat `Documents.Search` metody. V tomto případě jsme předat hledaný text jako řetězec plus parametrů hledání vytvořili použít. Také zadat `Hotel` jako parametr typ pro `Documents.Search`, která říká SDK deserializovat dokumenty ve výsledcích hledání na objekty typu `Hotel`.

Nakonec tato metoda prochází všechny shody ve výsledcích hledání tisk každý dokument konzoly.

Podívejme se podrobněji na tom, jak tento způsob je místo toho možnost:

    SearchDocuments(indexClient, searchText: "fancy wifi");

    SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

Při prvním hovoru jsme hledáte všechny dokumenty, které obsahují dotaz podmínek "efektních" nebo "Wi-Fi". Ve druhé volání hledaný text nastavena na "*", což znamená, že "najít všechno, co". Můžete najít další informace o syntaxe vyhledávacího dotazu výraz [tady](https://msdn.microsoft.com/library/azure/dn798920.aspx).

Druhé volání používá OData `$filter` výraz `category eq 'Luxury'`. Toto omezení hledání se jenom dokumenty místo, kam `category` pole přesně odpovídá řetězec "Luxusní". Můžete zjistit další informace o syntaxi OData, která podporuje Azure hledání [tady](https://msdn.microsoft.com/library/azure/dn798921.aspx).

Teď byste vědět, co dělat tyto dvě volání, by měl být čitelný proč jejich výstup vypadá takto:

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []

První vyhledávání vrátí dvou dokumentů. První má "Ozdobný" v názvu, zatímco druhý má "Wi-Fi" `tags` pole. Druhá vyhledávání vrátí dvou dokumentů, což se stát, aby jenom dokumenty v rejstříku, které mají `category` pole nastaveno na "Luxusní".

Dokončení tohoto kroku s kurzem, ale nechcete ukončit. **Další kroky** nabízí další zdroje informací pro získání dalších informací o hledání Azure.

## <a name="next-steps"></a>Další kroky

- Přejděte odkazy pro [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) a [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN.
- Prohloubit své znalosti prostřednictvím [videa a další ukázek a kurzů](search-video-demo-tutorial-list.md).
- Přečtěte si o funkcí a funkcí v této verzi Azure hledání SDK: [Přehled hledání Azure](https://msdn.microsoft.com/library/azure/dn798933.aspx)
- Prohlédněte si [konvence pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx) se dozvíte, pravidel pojmenování jednotlivé objekty.
- Prohlédněte si [podporované datové typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) v Azure vyhledávání.


## <a name="sample-application-source-code"></a>Ukázka použití zdrojového kódu

Tady je celé zdrojového kódu ukázkové aplikace použité v tomto procházení. Všimněte si, že byste potřebovali nahraďte název služby a rozhraní API klíčové zástupné symboly do Program.cs vlastní hodnoty Pokud chcete k vytvoření a spuštění vzorku.

Program.cs:

```csharp
using System;
using System.Configuration;
using System.Linq;
using System.Threading;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    class Program
    {
        // This sample shows how to delete, create, upload documents and query an index
        static void Main(string[] args)
        {
            // Put your search service name here. This is the hostname portion of your service URL.
            // For example, if your service URL is https://myservice.search.windows.net, then your
            // service name is myservice.
            string searchServiceName = "myservice";

            string apiKey = "Put your API admin key here.";

            SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

            Console.WriteLine("{0}", "Deleting index...\n");
            DeleteHotelsIndexIfExists(serviceClient);

            Console.WriteLine("{0}", "Creating index...\n");
            CreateHotelsIndex(serviceClient);

            ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

            Console.WriteLine("{0}", "Uploading documents...\n");
            UploadDocuments(indexClient);

            Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
            SearchDocuments(indexClient, searchText: "fancy wifi");

            Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
            SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

            Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("hotels"))
            {
                serviceClient.Indexes.Delete("hotels");
            }
        }

        private static void CreateHotelsIndex(SearchServiceClient serviceClient)
        {
            var definition = new Index()
            {
                Name = "hotels",
                Fields = new[]
                {
                    new Field("hotelId", DataType.String)                       { IsKey = true },
                    new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                    new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                    new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                    new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                    new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
                }
            };

            serviceClient.Indexes.Create(definition);
        }

        private static void UploadDocuments(ISearchIndexClient indexClient)
        {
            var documents =
                new Hotel[]
                {
                    new Hotel()
                    {
                        HotelId = "1058-441",
                        HotelName = "Fancy Stay",
                        BaseRate = 199.0,
                        Category = "Luxury",
                        Tags = new[] { "pool", "view", "concierge" },
                        ParkingIncluded = false,
                        LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                        Rating = 5,
                        Location = GeographyPoint.Create(47.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "666-437",
                        HotelName = "Roach Motel",
                        BaseRate = 79.99,
                        Category = "Budget",
                        Tags = new[] { "motel", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                        Rating = 1,
                        Location = GeographyPoint.Create(49.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "970-501",
                        HotelName = "Econo-Stay",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "pool", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(46.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "956-532",
                        HotelName = "Express Rooms",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "wifi", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(48.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "566-518",
                        HotelName = "Surprisingly Expensive Suites",
                        BaseRate = 279.99,
                        Category = "Luxury",
                        ParkingIncluded = false
                    }
                };

            try
            {
                var batch = IndexBatch.Upload(documents);
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

            // Wait a while for indexing to complete.
            Thread.Sleep(2000);
        }

        private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
        {
            // Execute search based on search text and optional filter
            var sp = new SearchParameters();

            if (!String.IsNullOrEmpty(filter))
            {
                sp.Filter = filter;
            }

            DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
            foreach (SearchResult<Hotel> result in response.Results)
            {
                Console.WriteLine(result.Document);
            }
        }
    }
}
```

Hotel.cs:

```csharp
using System;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }
}
```
