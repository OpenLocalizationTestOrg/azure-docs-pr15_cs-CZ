<properties
    pageTitle="Práce s knihovnou spravovaných klientské aplikace služby mobilní aplikace (Windows | Xamarin) | Microsoft Azure"
    description="Naučte se používat klienta .NET pro Azure aplikace služby mobilní aplikace s Windows a Xamarin aplikace."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Používání spravovaných klienta pro Azure mobilní aplikace

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře používat knihovnu spravovaných klienta Azure aplikace služby mobilní aplikace pro Windows a aplikace Xamarin. Pokud začínáte mobilní aplikace, měli byste zvážit nejdřív dokončení [Rychlý úvod Azure mobilní aplikace] [ 1] kurz. V této příručce jsme zaměřit na spravované SDK klienta. Další informace o SDK serverovou k aplikacím Mobile, najdete v dokumentaci [.NET serveru SDK] [ 2] nebo [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Si přečtěte následující dokumentaci odkaz

Tady je umístěn odkaz si přečtěte následující dokumentaci pro klienta SDK: [Azure mobilní aplikace .NET klienta odkaz][4].
Několik ukázek klienta můžete zjistit taky v [Azure vzorky GitHub úložiště][5].

## <a name="supported-platforms"></a>Podporované platformy

Platformu .NET podporuje následující platformy:

* Xamarin Android aktualizací pro rozhraní API 19 až 24 (KitKat prostřednictvím Nougat)
* Xamarin iOS aktualizací pro iOS 8.0 nebo novější verze
* Univerzální Windows platforma
* Windows Phone 8.1
* Windows Phone 8.0 s výjimkou aplikace Silverlight

"Server toku" ověřování používá webového zobrazení pro prezentovaný uživatelského rozhraní.  Pokud zařízení není možné zobrazení webového zobrazení uživatelského rozhraní, je třeba jiné metody ověřování.  Proto není vhodná pro typ kukátka podobně zamčeném zařízení nebo tato SDK.

##<a name="setup"></a>Instalační program a požadavky

Budeme se předpokládá, že jste již vytvořili a publikování projektu back-end mobilní aplikaci, který obsahuje nejméně jednu tabulku.  V kód použité v tomto tématu je název tabulky `TodoItem` a obsahuje následující sloupce: `Id`, `Text`, a `Complete`. V této tabulce je stejné tabulky vytvořené po dokončení [rychlý úvod Azure mobilní aplikace].

Odpovídající zadaným typ klienta v jazyce C# je třída takto:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] slouží k definování *Položka Název vlastnosti* mapování mezi typ klienta a v tabulce.

Informace o vytváření tabulek v mobilních aplikací backendovou najdete v tématu informace v [tématu .NET serveru SDK] [ 7] nebo [tématu Node.js Server SDK][8]. Pokud jste vytvořili v portálu Azure pomocí rychlý úvod backendovou mobilní aplikaci, můžete také nastavení **snadno tabulek** [Azure portál].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Postup: instalace balíček SDK spravovaných klienta

K instalace balíček SDK spravovaných klienta mobilních aplikací pro z [NuGet]použijte jeden z těchto způsobů[9]:

+ **Visual Studio** Klikněte pravým tlačítkem na projektu, klikněte na **Spravovat balíčků NuGet**, vyhledejte `Microsoft.Azure.Mobile.Client` balíček a potom klikněte na **nainstalovat**.

+ **Xamarin Studio** Klikněte pravým tlačítkem na projektu, klikněte na tlačítko **Přidat** > **Přidání balíčků NuGet**, vyhledejte `Microsoft.Azure.Mobile.Client `balíček a potom klikněte na **Přidat balíček**.

V hlavním aktivity souboru nezapomeňte si přidat následující příkaz **pomocí** :

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Postup: práce s symboly ladění ve Visual Studiu

Symboly pro Microsoft.Azure.Mobile názvů jsou k dispozici v [SymbolSource][10].  Postupujte podle [pokynů SymbolSource] [ 11] SymbolSource integrovat Visual Studia do.

##<a name="create-client"></a>Vytvoření klienta mobilní aplikace

Následující kód vytvoří [MobileServiceClient] [ 12] objekt, který se používá pro přístup k vaší back-end mobilní aplikaci.

    var client = new MobileServiceClient("MOBILE_APP_URL");

V předchozím kódu nahraďte `MOBILE_APP_URL` s adresou URL back-end mobilní aplikace, které najdete v zásuvné pro mobilní aplikaci backendovou [Azure portálu]. Objekt MobileServiceClient by měl být jednoznačné.

## <a name="work-with-tables"></a>Práce s tabulkami

V následující části Podrobnosti o tom, jak vyhledat a vyhledání záznamů a upravovat data v tabulce.  Následující témata:

* [Vytvořit odkaz na tabulku](#instantiating)
* [Dotaz na data](#querying)
* [Filtrování vrácených dat.](#filtering)
* [Řazení vrátil data](#sorting)
* [Vrátí data na stránkách](#paging)
* [Vyberte konkrétní sloupce](#selecting)
* [Vyhledat záznam pomocí Id](#lookingup)
* [Práce s bez typu dotazy](#untypedqueries)
* [Vložení dat](#inserting)
* [Aktualizace dat](#updating)
* [Odstranění dat](#deleting)
* [Řešení konfliktů a optimistické souběžné](#optimisticconcurrency)
* [Vazbu s uživatelským rozhraním systému Windows](#binding)
* [Změna velikosti stránky](#pagesize)

###<a name="instantiating"></a>Postup: vytvoření odkaz na tabulku

Všechny kód, který má přístup k nebo mění data v tabulce back-end funkce vyzývá `MobileServiceTable` objektu. Získání odkazu do tabulky tak, že zavoláte metodu [Funkce GetTable] následujícím způsobem:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Vrácené objekt používá zadaný serializace modelu. Model bez typu serializace podporuje i. Následující příklad [vytvoří odkaz na tabulku bez typu]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

Bez typu dotazů je nutné zadat podkladového dotazu řetězec OData.

###<a name="querying"></a>Postup: dotazu data z aplikace Mobile

Tato část popisuje dotazy vydat back-end systému v mobilní aplikaci, která zahrnuje následující funkce:

- [Filtrování vrácených dat.](#filtering)
- [Řazení vrátil data](#sorting)
- [Vrátí data na stránkách](#paging)
- [Vyberte konkrétní sloupce](#selecting)
- [Vyhledání data podle ID](#lookingup)

>[AZURE.NOTE]Velikost stránky serveru řízené úsilím nevynucují zabráníte se vrací všechny řádky.  Stránkovací pořád žádosti o výchozí u velkých datových sad z negativně ovlivňující ochranu službu.  Pokud chcete vrátit víc než 50 řádků, použijte `Skip` a `Take` způsob, jak je uvedeno v [zpáteční dat na stránkách].

###<a name="filtering"></a>Postup: Filtr vrátil data

Následující kód ukazuje, jak k filtrování dat podle včetně `Where` klauzule v dotazu. Vrátí všechny položky z `todoTable` jehož `Complete` vlastnost je rovno `false`. Funkce [kde] platí řádku filtrování predikátu dotaz na tabulku.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Můžete zobrazit URI žádost odeslal back-end softwarem zprávy kontroly, například nástrojů pro vývojáře prohlížeče nebo [Fiddler]. Když se podíváte na žádost o URI, Všimněte si, že je upraven řetězce dotazu:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Požadavek OData je převést na dotazu SQL Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Funkce, které je do `Where` metoda může mít libovolný počet podmínky.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

V tomto příkladu by převést na dotaz SQL zobrazený pomocí serveru SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Tento dotaz taky jde rozdělit na víc klauzule:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Dva způsoby jsou ekvivalentní a mohou být použity jako synonyma.  Možnost bývalého&mdash;z zřetězení více predikáty v jednom dotazu&mdash;je kompaktnější a doporučené.

`Where` Klauzule podporuje operacích, které lze převést na podsady OData. Operace patří:

* Relační operátory (==,! =, <, <, >, > =),
* Aritmetické operátory (+, -, /, *, %),
* Číslo přesnost (Math.Floor, Math.Ceiling)
* Funkce String (délka, podřetězec, nahradit, IndexOf, StartsWith, EndsWith)
* Kalendářní data (roku, měsíce, den, hodiny, minuty, sekundy),
* Přístup k vlastnosti objektu a
* Výrazy kombinování některou z těchto akcí.

Při rozhodování, jaký Server SDK podporuje, můžete [si přečtěte následující dokumentaci OData v3].

###<a name="sorting"></a>Postup: řazení vrátil data

Následující kód ukazuje, jak řadit data podle včetně [Řadit podle] nebo [OrderByDescending] funkce v dotazu. Vrátí položky z `todoTable` seřazené vzestupně podle `Text` pole.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Postup: vrácení dat na stránkách

Ve výchozím nastavení vrátí back-end pouze prvních 50 řádků. Počet vrácených řádků můžete zvětšit tak, že zavoláte metoda [trvat] . Použití `Take` spolu s metody [vynechání] požádat o konkrétní "stránka" celkové datovou sadu vracená dotazem. Následující dotaz při spuštění vrátí prvních tří položek v tabulce.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Následující dotaz upravený přeskočí první tři výsledků a vrátí následující tři výsledky. Tento dotaz vytvoří druhá "stránka" data, kdy je tři položky velikost stránky.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Metoda [IncludeTotalCount] požaduje celkový počet různých hodnot pro _všechny_ záznamy, které by byly vráceny, ignorovat všechny stránkování/limit klauzule zadané:

    query = query.IncludeTotalCount();

V aplikaci skutečném světě můžete dotazů podobně jako v předchozím příkladu s srovnatelná uživatelské rozhraní nebo ovládací prvek stránkování navigace mezi stránkami.

>[AZURE.NOTE]Přepsat 50 řádkový limit v mobilní aplikaci back-end, musíte použít [EnableQueryAttribute] metodu veřejné GET a zadejte stránkování. Když použijete metodu, nastaví následující maximální vrácena 1 000 řádků:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Postup: vyberte konkrétní sloupce

Můžete určit, kterou sadu vlastností chcete zahrnout do výsledků přidáním klauzule [Select] do dotazu. Například následující kód ukazuje, jak vybrat jenom jedno pole a taky jak vybrání a formátování více polí:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Jsou všechny funkce zatím popsané doplňkové, proto jsme můžete ponechat zřetězení je. Každý zřetězené volání ovlivňuje z dotazu. Další příklad:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Postup: vyhledávat data podle ID

Funkce [LookupAsync] mohou sloužit k vyhledání objektů z databáze pomocí konkrétní ID.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Postup: spuštění bez typu dotazů

Při spuštění dotazu pomocí objektu bez typu tabulky, explicitně zadejte řetězec OData dotazu tak, že zavoláte [ReadAsync]jako v následujícím příkladu:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Můžete se vrátit JSON hodnoty, které můžete použít jako balíčku vlastností. Další informace o JToken a Newtonsoft Json.NET najdete v článku [Json.NET] webu.

### <a name="inserting"></a>Postup: vložte data do back-end mobilní aplikaci

Všechny typy klientů musí obsahovat člena název – **Id**, který je ve výchozím nastavení řetězec. Toto **Id** je potřeba k provedení operace CRUD a offline synchronizace. Následující kód ukazuje, jak používat metodu [InsertAsync] vložit nové řádky do tabulky. Parametr obsahuje data, která chcete vložit jako objekt .NET.

    await todoTable.InsertAsync(todoItem);

Pokud není součástí jedinečné vlastní hodnoty ID `todoItem` při vložení, identifikátor GUID generovaných serverem.
Načtení generovaného Id kontrolou objekt po vrátí hovor.

Pokud chcete vložit bez typu dat, může využít Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Tady je příklad použití e-mailovou adresu jako id jedinečné řetězce:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Práce s hodnotami ID

Mobilní aplikace podporuje jedinečný vlastní řetězec hodnoty pro sloupec **id** v tabulce. Hodnotu řetězce umožňuje aplikacím používat vlastní hodnoty, například e-mailové adresy nebo jména uživatelů ID.  Řetězec ID zadejte následující výhody:

* ID vygenerují kalendáře bez vytvoření doby odezvy v databázi.
* Záznamy budou snadněji sloučit z různých tabulek nebo databáze.
* Hodnoty ID můžete lepší integrace s logiky aplikace.

Pokud řetězcovou hodnotu ID není nastavena na vloženého záznamu, back-end mobilní aplikaci vygeneruje jedinečnou hodnotu ID. Použijte metodu [Guid.NewGuid] vygenerovat vlastní hodnoty ID v klientském počítači nebo v back-end.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Postup: Změna dat v mobilní aplikaci back-end

Následující kód ukazuje, jak lze pomocí metody [UpdateAsync] aktualizovat existující záznam se stejným ID novými informacemi. Parametr obsahuje data, která mají být aktualizovány jako objekt .NET.

    await todoTable.UpdateAsync(todoItem);

Aktualizace bez typu dat, se můžou využít [Json.NET] následujícím způsobem:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

`id` Je nutné zadat při aktualizaci. Použití back-end `id` pole k určení řádek, který má aktualizovat. `id` Pole lze získat výsledek `InsertAsync` volání. `ArgumentException` Dojde při pokusu o aktualizaci položky bez zadání `id` hodnotu.

###<a name="deleting"></a>Postup: odstranění dat v mobilní aplikaci back-end

Následující kód ukazuje, jak pomocí metody [DeleteAsync] odstranit existující instance. Je určen instance `id` sadě polí na `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Pokud chcete odstranit bez typu dat, může využít výhod Json.NET následujícím způsobem:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Když vyberete jednotlivé žádost o odstranění, musí být zadán ID. Další vlastnosti nejsou předaná ke službě nebo na službu, jsou ignorovány. Výsledek `DeleteAsync` volání je obvykle `null`. ID pro předávání v lze získat výsledek `InsertAsync` zavolat. A `MobileServiceInvalidOperationException` se vyvolá, když se pokusíte odstranit položku bez zadání `id` pole.

###<a name="optimisticconcurrency"></a>Postup: použití optimistické souběžné pro řešení konfliktů

Dva nebo více klientů může k stejnou položku zápisu změn ve stejnou dobu. Bez zjišťování konfliktů by poslední zapsat přepíše předchozí aktualizace. **Ovládací prvek optimistické souběžné** předpokládá, že každou transakci můžete potvrdit a tedy nepoužívá zamykání všechny zdroje.  Před použitím transakce, optimistické souběžné řízení zkontroluje, zda má žádná transakce změně data. Pokud byly změněny data, je spouštění transakce vrátit zpět.

Mobilní aplikace podporuje řízení optimistické souběžné tak, že každé položky bude používat sledování změn `version` sloupce vlastností systému, která je definovaná pro každou tabulku v mobilní aplikaci backendovou. Pokaždé, když se aktualizuje záznamu, nastaví mobilní aplikace `version` vlastnost tohoto záznamu na novou hodnotu. Při každé aktualizaci požadavku `version` vlastnosti záznamu zahrnutý v sadě žádost porovnáván vlastnosti jednoho záznamu na serveru. Pokud verze předané s žádost neodpovídá back-end a pak vyvolá knihovnu klienta `MobileServicePreconditionFailedException<T>` výjimku. Typ zahrnutý v sadě výjimku je záznam z back-end obsahující servery verze záznamu. Aplikace mohou použít tyto informace pak se rozhodnout, jestli provést žádost o aktualizaci znovu s správné `version` hodnotu z back-end potvrďte změny.

Definování sloupce v tabulce předmětu pro `version` systém vlastnost Povolit optimistické souběžné. Příklad:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Aplikace pomocí bez typu tabulek umožňují optimistické souběžné nastavením `Version` příznak na `SystemProperties` tabulky takto.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Kromě povolení optimistické souběžné, musíte taky zachytit `MobileServicePreconditionFailedException<T>` výjimce ve vašem kódu při volání [UpdateAsync].  Konflikt použitím správné `version` aktualizovaný záznam a volání [UpdateAsync] přeložena záznamu. Následující kód ukazuje, jak vyřešit konflikt zapsat jednou detekovaný:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Další informace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure] .

###<a name="binding"></a>Postup: vazbu mobilní aplikace data, která chcete uživatelské rozhraní systému Windows

Tato část popisuje objekty vrácená data pomocí prvkům uživatelského rozhraní v aplikaci Windows zobrazení.  Následující příklad vytvoří vazbu k tomuto zdroji seznamu pomocí dotazu pro neúplné položky. [MobileServiceCollection] vytvoří mobilní aplikace podporující kolekci vazby.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Některé ovládací prvky v spravovaných runtime podporují rozhraní nazvané [ISupportIncrementalLoading]. Toto rozhraní umožňuje ovládací prvky požádat o další data, když se uživatel přesune. Existuje integrovanou podporu pro rozhraní pro univerzální aplikace Windows prostřednictvím [MobileServiceIncrementalLoadingCollection], která automaticky zpracovává hovory z ovládacích prvků. Použití `MobileServiceIncrementalLoadingCollection` aplikace Windows takto:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Novou kolekci na Windows Phone 8 a "Silverlight" aplikace použijete `ToCollection` rozšíření metody `IMobileServiceTableQuery<T>` a `IMobileServiceTable<T>`. Načtení dat, zavolejte `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Když použijete kolekci vytvořili tak, že zavoláte `ToCollectionAsync` nebo `ToCollection`, dostanete kolekce svázané k ovládacím prvkům uživatelského rozhraní.  Tato kolekce známa stránkování –.  Protože kolekci načítání dat ze sítě, načítání někdy nezdaří. Provádět tyto chyby přepsat `OnException` metoda na `MobileServiceIncrementalLoadingCollection` zpracování výjimek výsledkem volání `LoadMoreItemsAsync`.

Zvažte Pokud tabulka obsahuje mnoho polí, ale chcete některé z nich zobrazit ve vašem ovládacím prvku. Mohli byste použít pokyny v předchozí části "[Vyberte konkrétní sloupce](#selecting)" Chcete-li vybrat konkrétní sloupce zobrazíte v uživatelském rozhraní.

###<a name="pagesize"></a>Změna velikosti stránky

Azure mobilní aplikace vrátí maximálně 50 položek na žádost ve výchozím nastavení.  Můžete změnit velikost stránkování zvýšením maximální velikost stránky na klienta a serveru.  Chcete-li zvětšit požadovanou stránku, zadejte `PullOptions` při použití `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Za předpokladu, že jste provedli `PageSize` rovna nebo větší než 100 serveru, žádost o vrátí až 100 položek.

##<a name="#offlinesync"></a>Práce s tabulkami v režimu Offline

Offline tabulkách se používá místní úložiště přihlašovacích údajů pro ukládání data SQLite pro použití v režimu offline.  Všechny tabulky, kterou operace dělají na místní SQLite obsahují místo úložišti vzdálený server.  Pokud chcete vytvořit offline tabulky, nejprve připravte projektu:

1. Ve Visual Studiu, klikněte pravým tlačítkem myši na řešení > **Spravovat NuGet balíčků řešení...**, a pak vyhledat a nainstalovat **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet pro všechny projekty na řešení.

2. (Volitelné) Podpora zařízení Windows, nainstalujte jednu z následujících balíčků runtime SQLite:

    * **Windows 8.1 Runtime:** Instalace [SQLite ve Windows 8.1][3].
    * **Windows Phone 8.1:** Instalace [SQLite pro Windows Phone 8.1][4].
    * **Univerzální Windows platformy** Instalace [SQLite univerzální univerzální Windows][5].

3. (Volitelné). Zařízení s Windows, klikněte na **Reference** > **Přidat odkaz**, rozbalte složku **Windows** > **rozšíření**a potom povolit příslušný **SQLite pro Windows** SDK spolu s **Vizuální C++ 2013 Runtime pro systém Windows** SDK.
    Názvy SQLite SDK se liší podle mírně každou platformu Windows.

Před vytvořením odkaz na tabulku, musíte počítat v místním úložišti:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Inicializace úložiště obvykle probíhá ihned po vytvoření klienta.  **OfflineDbPath** by měl být vhodné pro použití na platformách, které podporují název souboru.  Pokud je tato cesta úplnou cestu (to znamená, začíná lomítko), potom bude tato cesta použita.  Pokud cesta není plně kvalifikovaný, je soubor umístěn v umístění specifické pro platformu.

* Pro iOS a Android výchozí cesta je složce "Osobních souborů".
* Pro zařízení s Windows je tato cesta výchozí složky "AppData" specifické pro aplikaci.

Odkaz na tabulku lze získat `GetSyncTable<>` metodu:

    var table = client.GetSyncTable<TodoItem>();

Ověření používat offline tabulku nepotřebujete.  Potřebujete jenom ověření při komunikaci s službu back-end.

###<a name="syncoffline"></a>Synchronizace Offline tabulky

Offline tabulky nejsou synchronizovány s back-end ve výchozím nastavení.  Synchronizace je rozdělen do dvou částí.  Změny můžete nabízená samostatně stahování nové položky.  Tady je typické synchronizace metoda:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Pokud argument pole `PullAsync` Null, pak Přírůstková synchronizace nepoužívá.  Každý operace synchronizace vyhledá všechny záznamy.

V SDK provádí implicitního `PushAsync()` před zavedení záznamy.

Zpracování konfliktu se stane s `PullAsync()` metody.  Konflikty můžete řešit stejným způsobem jako tabulky online.  Konflikt pochází při `PullAsync()` nazývá namísto při vložení, aktualizace nebo odstranění. V případě více konfliktů, jsou seskupeny do jednoho MobileServicePushFailedException.  Úchyt pro každou selhání samostatně

##<a name="#customapi"></a>Práce s vlastní rozhraní API

Vlastní rozhraní API umožňuje definovat vlastní koncové body zaznamenávající serveru funkce, které nejsou mapování pro vložení, aktualizovat, odstranit nebo operace čtení. Pomocí vlastní rozhraní API můžete mít větší kontrolu nad zpráv, včetně čtení a nastavení protokolu HTTP záhlaví zpráv a definování formát textu zprávy než JSON.

Vlastní rozhraní API volání metod [InvokeApiAsync] zavoláním na straně klienta. Například následující kód odešle žádost o příspěvek **completeAll** rozhraní API na back-end:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Tento formulář jde o hovor zadaný metoda a vyžaduje, je vráceným typem **MarkAllResult** je definován. Klávesnice a bez typu metody jsou podporovány.

##<a name="authentication"></a>Ověřování uživatelů

Mobilní aplikace podporuje ověřování a ověřování uživatelů aplikace pomocí různých poskytovatelů identit externí: Facebook Google, Account Microsoft, Twitteru a Azure Active Directory. Nastavení oprávnění pro tabulky, které chcete omezit přístup pro konkrétní operace pouze pro ověřeného uživatele. Můžete taky identitu ověřených uživatelů implementaci ověřovacích pravidel v skripty serveru. Další informace najdete v tématu kurz [ověřování přidat do aplikace].

Dva toků ověřování podporují: toku _spravovanými klienta_ a _serveru Správa přístupových práv_ . Tok serveru Správa přístupových práv poskytuje možnosti nejjednodušší ověřování závisí na poskytovatele webového ověřování rozhraní. Závisí na zprostředkovatele specifické pro zařízení SDK toku klienta Správa přístupových práv umožňuje hlubší integrace s možnostmi specifické pro zařízení.

>[AZURE.NOTE] Doporučujeme používat toku klienta Správa přístupových práv v aplikacích výroby.

Nastavení ověřování, musíte zaregistrovat aplikace u jedné nebo více Zprostředkovatelé identit jiní.  Zprostředkovatele identit generuje ID klienta a tajná klientské aplikace.  Tyto hodnoty jsou ve vaší back-end vyberte položku Povolit aplikaci služby Azure ověřování/se tak mohli ověřovat.  Další informace postupujte podle pokynů podrobné v kurzu [ověřování přidat do aplikace].

V následujících tématech jsou uvedené v této části:

+ [Ověřování klienta Správa přístupových práv](#clientflow)
+ [Ověřování serveru Správa přístupových práv](#serverflow)
+ [Ukládání do mezipaměti token ověřování](#caching)

###<a name="clientflow"></a>Ověřování klienta Správa přístupových práv

Aplikace můžete nezávisle na sobě kontaktujte poskytovatele identity a potom zadat vrácené token při přihlášení se vaše back-end. Tok tohoto klienta umožňuje zajistit jednoho prostředí přihlašování pro uživatele nebo k načtení dat dalších uživatelů z zprostředkovatele identit. Ověření toku klienta je upřednostňovaný pomocí serveru toku zprostředkovatele identit SDK obsahuje více nativní chování činnosti koncového uživatele a umožňuje dalších úprav.

Příklady jsou k dispozici pro následující ověřování vzorků toku klienta:

+ [Služby Active Directory Authentication Library](#adal)
+ [Facebook nebo Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Ověření uživatelé, kteří mají Active Directory Authentication Library

Active Directory ověřování knihovny (ADAL) slouží k ověření zahájení uživatele z klienta pomocí ověřování služby Azure Active Directory.

1. Konfigurace back-end vaše mobilní aplikace pro AAD jednotného přihlášení pomocí následujících kurz [jak nakonfigurovat aplikaci služby Active Directory přihlášení] . Zkontrolujte, že uděláte volitelné registrace aplikace native client –.
2. Ve Visual Studiu nebo Xamarin Studio otevřete svůj projekt a přidat odkaz `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet balíčku. Při hledání zahrnout předběžné verze.
3. Přidání kódu pro následující aplikace podle platformy, které používáte. V každém zkontrolujte následující nahrazení:

    * **Vložení AUTORITA TADY** nahraďte názvem klienta, ve kterém zřízení aplikace. Formát by měl být https://login.windows.net/contoso.onmicrosoft.com. Tato hodnota můžete zkopírovali z karta domény v Azure Active Directory [Azure klasické portálu].
    * **Vložení zdroje ID TADY** nahraďte ID klienta pro mobilní aplikaci backendovou. Na kartě **Upřesnit** v části **Nastavení služby Azure Active Directory** v portálu můžete získat kód klienta.
    * Nahraďte **Vložení klienta ID TADY** jste zkopírovali z aplikace native client – ID klienta.
    * Nahraďte **Vložení PŘESMĚROVÁNÍ URI TADY** svého webu _/.auth/login/done_ koncový bod, pomocí schématu HTTPS. Tato hodnota by měl být stejný _https://contoso.azurewebsites.net/.auth/login/done_.

    Kód potřebný pro každou platformu takto:

    **Systém Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Jednotné přihlašování pomocí tokenu z Facebooku nebo Google

Tok klienta můžete použít, jak je uvedeno v této fragment kódu pro Facebook nebo Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Jeden přihlášení pomocí Account Microsoft Live SDK

Pro ověřování uživatelů, musíte zaregistrovat aplikace na účet Microsoft středisko pro vývojáře. Konfigurace podrobnosti registrace v mobilní aplikaci backendovou. Vytvořit registrace účtu Microsoft a připojte ji k vaší back-end mobilní aplikaci, postupujte podle pokynů v [rejstříku aplikaci používat přihlašování pomocí zvláštního účtu Microsoft]. Pokud máte pro Windows Store a Windows Phone 8/Silverlight verzích aplikace registraci nejdřív verze pro Windows Store.

Následující kód ověřuje pomocí Live SDK a pomocí vrácené token Přihlaste se k aplikaci Mobile back.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Další informace najdete v dokumentaci [Windows Live SDK] .

###<a name="serverflow"></a>Ověřování serveru Správa přístupových práv

Jakmile jste si zaregistrovali zprostředkovatele identit, zavolejte metodu [LoginAsync] na [MobileServiceClient] s hodnotou [MobileServiceAuthenticationProvider] svého poskytovatele. Například následující kód spustí na serveru toku přihlášení pomocí služby Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Pokud používáte zprostředkovatelem identit než Facebook, změňte hodnotu [MobileServiceAuthenticationProvider] na hodnotu pro vašeho poskytovatele.

V toku server Azure aplikaci služby má na starosti tok ověřování OAuth zobrazením přihlašovací stránce vybraného zprostředkovatele.  Jakmile vrátí zprostředkovatele identit, aplikaci služby Azure vygeneruje token ověřování aplikace služby. Metoda [LoginAsync] vrátí [MobileServiceUser], který obsahuje [ID uživatele] ověřeného uživatele i [MobileServiceAuthenticationToken]jako token web JSON (JWT). Tento token můžete cached a opětovné použití dokud vypršením jeho platnosti. Další informace najdete v tématu [ukládání do mezipaměti ověřovací token](#caching).

###<a name="caching"></a>Ukládání do mezipaměti token ověřování

V některých případech volání metodu login můžete zabránit po první úspěšném ověření ukládání ověřovací token od poskytovatele.  Aplikace pro Windows Store a UWP můžete použít [PasswordVault] do mezipaměti aktuální ověřovací token po úspěšně přihlásit, následujícím způsobem:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

ID uživatele hodnota je uložena jako uživatelské jméno pověření a token je uložená jako heslo. Na pozdější podniky můžete zkontrolovat, že **PasswordVault** k zadání přihlašovacích údajů uložených v mezipaměti. V následujícím příkladu režim cached přihlašovací údaje při jejich nacházejí a pokusí se v opačném případě znovu ověřit s back-end:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Při odhlášení uživatele, musíte taky odstranit uložené přihlašovací údaje, následujícím způsobem:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin aplikace umožňuje [Xamarin.Auth] rozhraní API bezpečné ukládání přihlašovacích údajů v objektu **účtu** . Příklad použití těchto rozhraní API najdete v článku souboru s kódem [AuthStore.cs] [vzorku pro sdílení fotografií ContosoMoments].

Když používáte klienta Správa přístupových práv ověřování, můžete taky mezipaměti přístupový token získáte od svého poskytovatele služeb, jako je Facebook nebo Twitter. Tento token lze zadat do požádat o nové ověřování token back-end, následujícím způsobem:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Nabízená oznámení

V následujících tématech průvodní nabízená oznámení:

* [Registrace k nabízená oznámení](#register-for-push)
* [Získat balíček pro Windows Store ID zabezpečení](#package-sid)
* [Zaregistrujte se šablonami a platformy](#register-xplat)

###<a name="register-for-push"></a>Postup: registrace k nabízená oznámení

Klient mobilní aplikace umožňuje registrovat nabízených oznámení s Azure oznámení rozbočovače. Při registraci, můžete získat ty, které jste získali od specifické pro platformu nabízená oznámení služby (PNS). Při vytváření registrace pak zadejte tuto hodnotu spolu s značky. Následující kód zaregistruje aplikace Windows nabízených oznámení s oznámení služby systému Windows (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Pokud jsou předání do WNS, pak je nutné [získat balíček pro Windows Store ID zabezpečení](#package-sid).  Další informace o aplikace Windows, včetně jak si zaregistrovat pro šablonu registrace najdete v článku [Přidání nabízených oznámení pro aplikace].

Požadování značky z klienta není podporovaná.  Požadavky na značku se nezobrazí tiše z zápisu.
Pokud se chcete zaregistrovat zařízení s klíčovými slovy, vytvořte vlastní rozhraní API, které používá rozhraní API rozbočovače oznámení provádět registrace vaším jménem.  [Volání rozhraní API vlastní](#customapi) místo `RegisterNativeAsync()` metody.

###<a name="package-sid"></a>Postup: získat balíček pro Windows Store ID zabezpečení

Balíček ID zabezpečení: není nutná pro povolení nabízená oznámení v aplikacích pro Windows Store.  Pro příjem balíčku ID zabezpečení, registrovat aplikaci Web Windows Store.

Postup získání tuto hodnotu:

1. Ve Visual Studiu Průzkumníku klikněte pravým tlačítkem myši na projekt aplikace pro Windows Store, klikněte na **Store** > **Přidružení App Store …**.
2. V průvodci klikněte na tlačítko **Další**, přihlaste se pomocí svého účtu Microsoft, do **rezervy nový název aplikace**zadejte název aplikace a potom klikněte na **Rezervovat**.
3. Po registraci aplikace úspěšně, vyberte název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přiřadit**.
4. Přihlaste se na [Centrum pro vývojáře Windows] pomocí Account Microsoft. V části **Moje aplikace**klikněte na registrace aplikace, kterou jste vytvořili.
5. Klikněte na položku **Správa aplikací** > **identit aplikace**a pak posuňte se dolů k vyhledání **ID balíčku zabezpečení**.

Mnoho použití balíčku ID zabezpečení takovým s ním zacházet jako identifikátor URI v takovém případě budete muset používat _ms aplikace: / /_ jako schéma. Poznamenejte si verzi balíčku ID zabezpečení tvořena zřetězením tuto hodnotu jako předponu.

Aplikace Xamarin vyžaduje některé další kód mají být k registraci aplikace systémem iOS nebo Android platformách. Další informace naleznete v tématu pro svoji platformu:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Postup: registrace nabízených šablony o neodeslání oznámení a platformy

Zaregistrovat šablon, můžete `RegisterAsync()` metoda šablonami takto:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Šablony by měl být `JObject` typů a může obsahovat více šablon ve formátu JSON takto:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Metoda **RegisterAsync()** zpracuje také sekundární dlaždice:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Všechny značky vynechány pryč při registraci cenného papíru. Přidání značky na zařízení nebo šablony v zařízení, najdete v článku [práce se serverem back-end .NET SDK k aplikacím Mobile Azure].

Pokud chcete odeslat oznámení o použití těchto registrovaných šablon, v nápovědě k rozhraní [API rozbočovače oznámení].

##<a name="misc"></a>Různé témata

###<a name="errors"></a>Postup: zpracování chyb

Když dojde k chybě v back-end, klienta SDK vyvolá `MobileServiceInvalidOperationException`.  Následující příklad ukazuje, jak pro zpracování, který vám vrátil back-end výjimky:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Jiný příklad použití se zabývá chybové podmínky najdete v [Ukázkové soubory mobilní aplikace]. Příklad [LoggingHandler] poskytuje rutinu delegáta protokolování (následující) k přihlášení žádosti do back-end.

###<a name="headers"></a>Jak se: přizpůsobení požádat o záhlaví

Pro podporu určitou aplikaci scénáře, můžete přizpůsobit komunikace s back-end mobilní aplikaci. Můžete například přidat vlastní záhlaví do každého odchozího požadavku nebo dokonce můžete změnit stavů odpovědi. Můžete použít vlastní [DelegatingHandler], jako v následujícím příkladu:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Přidání ověřování aplikace]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md
[Přidání nabízených oznámení pro aplikace]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Registrace aplikace pro použití přihlašování pomocí zvláštního účtu Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak nakonfigurovat aplikaci služby pro přihlášení k Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[Jít]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[Vytvoření odkazu na tabulku bez typu]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[Řadit podle]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Přijmout]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Vyberte]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Přeskočit]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[ID uživatele]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Kde]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portálu]: https://portal.azure.com/
[Azure klasické portálu]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Centrum pro vývojáře systému Windows]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Rozhraní API rozbočovače oznámení]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Ukázkové soubory mobilní aplikace]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[Přečtěte následující dokumentaci pro OData v3]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[Sdílení ukázkové fotografie ContosoMoments]: https://github.com/azure-appservice-samples/ContosoMoments