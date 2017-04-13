<properties
   pageTitle="Upgrade na Azure hledání .NET SDK verze 1.1 | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
   description="Upgrade na Azure hledání .NET SDK verze 1.1"
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
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Upgrade na Azure hledání .NET SDK verze 2.0 – náhled

Pokud používáte verze 1.1 nebo starší [Azure hledání .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), tento článek vám pomůže upgrade aplikace pomocí další hlavní verze 2.0 náhledu.

Obecnější návod SDK včetně příkladů naleznete v tématu [použití Azure hledání z aplikace .NET](search-howto-dotnet-sdk.md).

Verze 2.0 – náhled .NET SDK vyhledávací Azure obsahuje některé změny ze starších verzí. Toto jsou převážně menší, aby změna kódu potřeba povolit pouze minimální úsilí. Další informace o tom, jak změnit svůj kód použít novou verzi SDK najdete v článku [Postup upgradu](#UpgradeSteps) .

> [AZURE.NOTE] Pokud používáte verze preview 0,13 nebo starší, upgradujte verze 1.1 křestní jméno a potom upgradu 2.0 náhled. V tématu [dodatku: postup upgradu na verzi 1.1](#UpgradeStepsV1) pokyny.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Co je nového v verze 2.0 – preview

Verze 2.0 – preview je první verzi Azure hledání .NET SDK jej směrovat ukázkové verzi Azure hledání rozhraní REST API, konkrétně 2015-02-28náhled. Umožňuje používat řadu funkcí Náhled Azure hledání z aplikace .NET, včetně následujících:

- [Vlastní analyzátory](https://aka.ms/customanalyzers)
- [Úložiště objektů Blob Azure](search-howto-indexing-azure-blob-storage.md) a [Úložiště tabulek Azure](search-howto-indexing-azure-tables.md) indexování podpory
- Přizpůsobení indexování pomocí [mapování polí](search-indexer-field-mappings.md)
- Podpora ETags k povolení bezpečných souběžné aktualizace rejstříku definice, indexování a zdrojů dat
- Podpora pro .NET Core a .NET přenosná profil 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Postup upgradu

Nejdřív aktualizovat NuGet odkaz pro `Microsoft.Azure.Search` pomocí Správce balíčků NuGet konzoly nebo podle pravým tlačítkem myši na odkazy projektu a vyberete "Správa NuGet balíčků..." ve Visual Studiu. Ujistěte se, povolení předběžné balíčků tak, že vyberete "Zahrnout zkušební" v NuGet "Správa balíčků" okno ve Visual Studiu nebo pomocí `-IncludePrerelease` přepínač, pokud používáte konzoly NuGet balíčku správce.

Po stažení nové balíčky a jejich závislosti NuGet znovu vytvořte projekt. V závislosti na strukturování kódu ji může znovu úspěšně. Pokud ano, jste připravená k odeslání.

Pokud se vaše sestavení nezdaří, byste měli vidět chybu sestavení takto:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Dalším krokem je opravit tuto chybu Tvůrce dotazů. Podrobnosti o co způsobuje chybu a opravný nástroj fix it najdete v článku [zrušení změn v verze 2.0 – náhled](#ListOfChanges) .

Může se zobrazit další vytvořit upozornění související s zastaralé metody nebo vlastnosti. Upozornění bude obsahovat pokyny, co se má použít místo nepoužívané funkce. Například, pokud vaše aplikace používá `SearchRequestOptions.RequestId` vlastnosti by měly zobrazí se upozornění, že`"This property is deprecated. Please use ClientRequestId instead."`

Po neopravíte sestavení chyby, můžete aplikaci umožní využít výhod nových funkcí, pokud chcete dělat změny. Nové funkce v sadě SDK jsou uvedené v [Co je nového v verze 2.0 – náhled](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Zrušení změn ve verzi 2.0 preview

Existuje pouze jeden změnu nejnovější verze 2.0 – verze preview, může vyžadovat kód změny kromě opětovné vytvoření aplikace.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient vráceným typem

`Indexes.GetClient` Způsob zahrnuje nové vráceným typem. Dříve, je vrácena `SearchIndexClient`, ale takto změnila na `ISearchIndexClient` v verze 2.0 – náhledu. Toto je podporuje zákazníky, kteří chtějí model `GetClient` způsob testování vrácením mock provádění `ISearchIndexClient`.

#### <a name="example"></a>Příklad

Pokud váš kód vypadat takto:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Můžete jej změnit na roli a opravte případné sestavení chyby:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Uzavření
Pokud budete potřebovat další informace o použití .NET SDK vyhledávací Azure, najdete v článku naší nedávno aktualizované [postupy](search-howto-dotnet-sdk.md).

V SDK Vítáme svůj názor. Pokud narazíte na problémy, neváhejte nám žádali o pomoc na [fórum MSDN pro hledání Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Pokud se vám najít chybu, můžete poslat problém v [Azure .NET SDK GitHub úložiště](https://github.com/Azure/azure-sdk-for-net/issues). Zkontrolujte, že předpona název problému s "hledání SDK:".

Děkujeme za použití Azure hledání!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Dodatek: Kroky upgradovat na verze 1.1 

> [AZURE.NOTE] Tato část platí jenom pro uživatele verze Azure hledání .NET SDK 0,13 náhled a starší.

Nejdřív aktualizovat NuGet odkaz pro `Microsoft.Azure.Search` pomocí Správce balíčků NuGet konzoly nebo podle pravým tlačítkem myši na odkazy projektu a vyberete "Správa NuGet balíčků..." ve Visual Studiu.

Po stažení nové balíčky a jejich závislosti NuGet znovu vytvořte projekt.

Pokud jste dříve použili verze 1.0.0-preview, 1.0.1-preview nebo 1.0.2-preview, musí být sestavení úspěšné a budete chtít přejít!

Pokud jste dříve použili verze 0.13.0-preview nebo starší, byste měli vidět sestavit chyby jako následující:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Dalším krokem je oprava chyb sestavení jeden po druhém. Většina budou vyžadovat změnu některých třídy a metody názvy, které byly přejmenovat v SDK. [Seznam nejnovější změny ve verzi 1.1](#ListOfChangesV1) obsahuje seznam těchto změn názvů.

Pokud používáte vlastní třídy k modelování dokumentů a těchto tříd mají vlastnosti bez-s možnou hodnotou Null základní typy (například `int` nebo `bool` v jazyce C#), je chyba oprava 1.1 verzi SDK, které byste měli znát. Další informace najdete v článku [opravy chyb v verze 1.1](#BugFixesV1) .

Nakonec po neopravíte sestavení chyby, můžete dělat změny aplikaci umožní využít výhod nových funkcí, chcete-li.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Seznam nejnovější změny ve verze 1.1

Následující seznam podle pravděpodobnost, že změna ovlivní aplikaci kód.

#### <a name="indexbatch-and-indexaction-changes"></a>Změny IndexBatch a IndexAction

`IndexBatch.Create`přejmenoval na `IndexBatch.New` a už není `params` argumentů. Můžete použít `IndexBatch.New` pro listy, které kombinovat různé typy akcí (sloučení, odstraníte atd.). Kromě toho způsoby nové statické pro vytváření listů kde všechny akce jsou stejné: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.

`IndexAction`už má veřejné konstruktory a jeho vlastnosti jsou teď neměnný. Nové metody statické byste měli použít pro vytváření akcí pro jiné účely: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`. `IndexAction.Create`odebrali. Pokud jste použili přetížení, která přijímá jenom dokumentu, zkontrolujte, že použít `Upload` místo.

##### <a name="example"></a>Příklad

Pokud váš kód vypadat takto:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Můžete jej změnit na roli a opravte případné sestavení chyby:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Pokud chcete, můžete dál zjednodušit ho takto:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>Změny IndexBatchException

`IndexBatchException.IndexResponse` Vlastnost přejmenoval na `IndexingResults`, a jeho typ je teď `IList<IndexingResult>`.

##### <a name="example"></a>Příklad

Pokud váš kód vypadat takto:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Můžete jej změnit na roli a opravte případné sestavení chyby:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Operace způsob změny

Každou operaci v .NET SDK Azure hledání se zobrazí jako sady přetížení metody u synchronní a asynchronní volající. Podpisy a řešení Tato metoda přetížení změnil verze 1.1.

Třeba operace "Získat Index statistických údajů" ve starších verzích SDK zveřejněným tyto podpisy:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Podpisy metod pro stejnou operaci verze 1.1 vypadat takto:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Od verze 1.1 .NET SDK vyhledávací Azure uspořádává operace metody odlišným způsobem:

 - Volitelné parametry jsou teď modelovat jako výchozí parametry spíše než přetížení další metody. Sníží čísla metodu přetížení někdy výrazně.
 - Rozšíření metody teď skrýt spoustu nadbytečné podrobnosti HTTP z volající. Například starší verze aplikace v SDK vrátil odpověď objekt s kódem stav HTTP, které často neměli potřebujete zkontrolovat, protože operace metody vyvolat `CloudException` pro všechny stavový kód, který ukazuje o chybě. Nové způsoby rozšíření pouze vrátit objekty modelu, ušetříte potíže s rozbalit v kódu.
 - Základní naopak rozhraní teď vystavení metody, které umožňují větší kontrolu na úrovni HTTP pokud ho potřebujete. Teď můžete předat ve vlastních záhlaví HTTP mají být součástí žádosti a nové `AzureOperationResponse<T>` vráceným typem umožňuje přímý přístup k `HttpRequestMessage` a `HttpResponseMessage` pro operaci. `AzureOperationResponse`je definován v `Microsoft.Rest.Azure` názvů a nahradí `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>Změny ScoringParameters

Nové třídy s názvem `ScoringParameter` je přidán do nejnovější SDK usnadnit zadejte parametry bodování profily vyhledávacího dotazu. Dříve `ScoringProfiles` vlastnost `SearchParameters` třídy napsaný jako `IList<string>`; Teď je zadaná jako `IList<ScoringParameter>`.

##### <a name="example"></a>Příklad

Pokud váš kód vypadat takto:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Můžete jej změnit na roli a opravte případné sestavení chyby: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Změny třídy modelu

Z důvodu podpis se změní podle [Metoda operace změní](#OperationMethodChanges), mnoho tříd v `Microsoft.Azure.Search.Models` názvů byly přejmenovat nebo odebrat. Příklad:

 - `IndexDefinitionResponse`byla nahrazena`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`přejmenoval na`DocumentSearchResult`
 - `IndexResult`přejmenoval na`IndexingResult`
 - `Documents.Count()`nyní vrátí `long` s počet dokumentů místo`DocumentCountResponse`
 - `IndexGetStatisticsResponse`přejmenoval na`IndexGetStatisticsResult`
 - `IndexListResponse`přejmenoval na`IndexListResult`

Shrnutí, `OperationResponse`– odvozená třídy, které existoval jen zalomení mohly být odebrány modelu objektu. Zbývající třídy byly jejich přípony změnil z `Response` k `Result`.

##### <a name="example"></a>Příklad

Pokud váš kód vypadat takto:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Můžete jej změnit na roli a opravte případné sestavení chyby:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Odpověď tříd a IEnumerable

Další změny, které by mohly ovlivnit kódu je, že odpověď třídy, které obsahují kolekce už implementace `IEnumerable<T>`. Místo toho můžete využít vlastnost kolekce přímo. Pokud například kód vypadá takto:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Můžete jej změnit na roli a opravte případné sestavení chyby:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Důležité poznámky pro webové aplikace

Pokud jste ještě webovou aplikaci, která jsou `DocumentSearchResponse` přímo Pokud chcete poslat výsledky hledání v prohlížeči, budete muset změnit kód nebo výsledky nebudou serializovat správně. Pokud například kód vypadá takto:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Můžete změnit jeho začíná `.Results` vlastnost odpověď hledání řešení vykreslování výsledků hledání:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Budete muset vyhledejte těchto případů ve vašem kódu sami. **Kompilátoru nebude odpovíte** protože `JsonResult.Data` je typu `object`.

#### <a name="cloudexception-changes"></a>Změny CloudException

`CloudException` Třídy posunuly z `Hyak.Common` názvů `Microsoft.Rest.Azure` názvů. Také jeho `Error` vlastnost přejmenoval na `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Změny SearchServiceClient a SearchIndexClient

Typ `Credentials` vlastnost se změnil z `SearchCredentials` základní třídě `ServiceClientCredentials`. Pokud potřebujete pracovat `SearchCredentials` z `SearchIndexClient` nebo `SearchServiceClient`, stiskněte klávesovou zkratku nové `SearchCredentials` vlastnost.

Ve starších verzích SDK `SearchServiceClient` a `SearchIndexClient` měli konstruktory provedené `HttpClient` parametr. Tyto nahradily konstruktory, které přijímají `HttpClientHandler` a palety `DelegatingHandler` objekty. Usnadňuje nainstalovat vlastní rutiny předem zpracuje požadavků HTTP v případě potřeby.

Nakonec konstruktorů provedené `Uri` a `SearchCredentials` změnily. Pokud například máte kód, který bude vypadat takto:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Můžete jej změnit na roli a opravte případné sestavení chyby:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Všimněte si také, typ parametru přihlašovací údaje změnila tak, aby `ServiceClientCredentials`. Toto je pravděpodobně ovlivňovat kódu od `SearchCredentials` pochází z `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Předání žádosti o ID

Ve starších verzích SDK můžete nastavit ID požadavku na `SearchServiceClient` nebo `SearchIndexClient` a by být součástí každého požadavku na rozhraní REST API. To je užitečné pro odstraňování potíží s vyhledávací služby v případě potřeby kontaktovat podporu. Je ale zvýšíte jeho přínos pro nastavení jedinečných žádost ID pro všechny operace spíše než používat stejný ID pro všechny operace. Z tohoto důvodu `SetClientRequestId` metody `SearchServiceClient` a `SearchIndexClient` mohly být odebrány. Místo toho můžete předat ID požadavku obou metod operace prostřednictvím volitelné `SearchRequestOptions` parametr.

> [AZURE.NOTE] V budoucí verzi SDK přidáme nové mechanismus ID požadavku globálně nastavení v klientském počítači objektů, který je v souladu s přístupem používat jiné SDK Azure.

#### <a name="example"></a>Příklad

Pokud máte kód, který vypadá takto:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Můžete jej změnit na roli a opravte případné sestavení chyby:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Změny názvů rozhraní

Názvy skupin rozhraní operace byla změněna konzistentní s odpovídajícími názvy vlastnost:

 - Typ `ISearchServiceClient.Indexes` přejmenoval z `IIndexOperations` k `IIndexesOperations`.
 - Typ `ISearchServiceClient.Indexers` přejmenoval z `IIndexerOperations` k `IIndexersOperations`.
 - Typ `ISearchServiceClient.DataSources` přejmenoval z `IDataSourceOperations` k `IDataSourcesOperations`.
 - Typ `ISearchIndexClient.Documents` přejmenoval z `IDocumentOperations` k `IDocumentsOperations`.

Tato změna je pravděpodobně ovlivňovat kódu, pokud jste vytvořili mocks těchto rozhraní pro účely testování.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Opravy chyb v verze 1.1

Ve starších verzích .NET SDK vyhledávací Azure týkající se serializace třídy vlastní modelu jste udělali chybu. Chyby může dojít, pokud jste vytvořili vlastní modelu třídy vlastnost typu bez-s možnou hodnotou Null hodnoty.

#### <a name="steps-to-reproduce"></a>Postup pro zopakování

Vytvoření vlastní modelu třídy s vlastnost typu bez-s možnou hodnotou Null hodnotu. Například přidat veřejnou `UnitCount` vlastnost typu `int` namísto `int?`.

Pokud indexujete dokumentu s konci výchozí hodnotu tohoto typu (například 0 `int`), pole bude obsahovat hodnotu null v Azure vyhledávání. Pokud následně hledáte tento dokument `Search` volání vyvolají `JsonSerializationException` nesouhlasících, který nelze převést `null` k `int`.

Filtry také nefunguje očekávaným způsobem, protože hodnota null napsané indexovat místo určené hodnotu.

#### <a name="fix-details"></a>Oprava podrobnosti

Tento problém jsme mít pevných verze 1.1 SDK. Nyní, pokud máte třída modelu takto:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

a nastavíte `IntValue` 0, tato hodnota je teď správně serializován jako 0 na lince a uložených jako 0 v indexu. Kulaté vypínání taky funguje očekávaným způsobem.

Jeden potenciální problémy nějaká s Tento přístup: Pokud použijete typu model-nepovinných vlastnost, budete muset **zaručit** , že nejsou žádné dokumenty ve odkazu obsahovat hodnotu null pro odpovídající pole. V SDK ani rozhraní REST API pro hledání Azure se můžete k jejímu vynucení to.

Toto není právě hypotetické problém: Představte si situace místo, kam přidat nové pole do existující index, který je typu `Edm.Int32`. Po aktualizaci definici indexu, budou všechny dokumenty obsahovat hodnotu null pro toto nové pole (protože se všechny typy s možnou hodnotou Null v Azure hledání). Pokud použijete třída modelu s jiných-s možnou hodnotou Null `int` vlastnost pro toto pole, zobrazí se vám `JsonSerializationException` při pokus o načtení vlastnosti dokumentů takto:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Z tohoto důvodu pořád doporučujeme používat s možnou hodnotou Null typy tříd modelu osvědčený.

Další informace o této chybě a opravy najdete v článku [Tento problém na GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
