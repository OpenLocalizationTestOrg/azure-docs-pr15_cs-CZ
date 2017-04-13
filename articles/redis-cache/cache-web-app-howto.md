<properties 
    pageTitle="Jak vytvořit Web App s Redis mezipaměti | Microsoft Azure" 
    description="Naučte se vytvářet Web App s Redis mezipaměti" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Jak vytvořit Web App s Redis mezipaměti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [TECHNOLOGIE ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Tento kurz ukazuje, jak vytvářet a publikovat webové aplikace technologie ASP.NET pro web app v aplikaci služby Azure pomocí aplikace Visual Studio 2015. Ukázková aplikace zobrazí seznam s jednou variantou týmu z databáze a zobrazí různé způsoby použití Azure Redis mezipaměti pro ukládání a načtení dat z mezipaměti. Po dokončení kurzu budete mít do pracovního webových aplikací, který bude číst a zapíše do databáze, optimalizované s Azure Redis mezipaměti a hostovaný v Azure.

Se dozvíte:

-   Jak vytvořit webovou aplikaci ASP.NET MVC 5 ve Visual Studiu.
-   Získání přístupu k datům z databáze pomocí Entity Framework.
-   Jak zlepšit výkon dat a zmenšit ukládání a získávání dat pomocí Azure Redis mezipaměti zatížení databáze.
-   Jak používat Redis seřazené nastavit na stav načíst horní 5 týmy.
-   Jak zřídit Azure prostředky pro aplikaci použít šablonu? správce prostředků.
-   Jak publikovat aplikace Azure pomocí aplikace Visual Studio.

## <a name="prerequisites"></a>Zjistit předpoklady pro

K dokončení kurzu, musíte mít následující požadavky.

-   [Účet Azure](#azure-account)
-   [Visual Studio 2015 s Azure SDK pro .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Účet Azure

Musíte mít účet Azure k dokončení kurzu. Můžeš:

* [Otevřete účet Azure zdarma](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Potřebujete přeplatky, které lze použít k vyzkoušení placené služby Azure. I když přeplatky používají, můžete mít účet a použití funkcí a bezplatné služby Azure.
* [Aktivace Visual Studio účastnická výhod](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Vaše předplatné MSDN vám přeplatky každý měsíc, využívající služby placené Azure.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 s Azure SDK pro .NET

Kurz je ručně psaných pro Visual Studio 2015 s [Azure SDK pro .NET](../dotnet-sdk.md) 2.8.2 nebo novější. [Stáhněte si nejnovější SDK Azure pro Visual Studio 2015 tady](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio automaticky nainstalovaný s SDK Pokud ještě nemáte ho.

Pokud máte Visual Studio 2013, můžete [Stáhnout nejnovější SDK Azure Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Některé obrazovky může vypadat jinak, než ilustrace zobrazené v tomto kurzu.

>[AZURE.NOTE] Podle toho, kolik závislostí SDK již máte v počítači instalaci sady SDK může chvíli trvat, z několika minut zabere nejméně půl hodiny.

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

1. Otevřete aplikaci Visual Studio a klikněte na **soubor**, **Nový** **projekt**.

2. Rozbalte položku **Visual Basic** v seznamu **šablony** vyberte **cloudu**a klikněte na **Webová aplikace ASP.NET**. Ujistěte se, že je zaškrtnuto **.NET Framework 4.5.2** .  Do textového pole **název** zadejte **ContosoTeamStats** a klikněte na **OK**.
 
    ![Vytvoření projektu][cache-create-project]

3. Vyberte **MVC** jako typ projektu. Zrušte zaškrtnutí políčka **hostitele v cloudu** . [Zřízení Azure zdrojů](#provision-the-azure-resources) a [publikovat aplikace Azure](#publish-the-application-to-azure) v dalších částech se v tomto kurzu. Příklad zřizování webové aplikace pro aplikaci služby z aplikace Visual Studio necháte **hostitele v cloudu** zaškrtnuté najdete v článku [Začínáme s aplikací webové aplikace služby Azure pomocí technologie ASP.NET a Visual Studia](../app-service-web/web-sites-dotnet-get-started.md).

    ![Zvolte šablonu projektu][cache-select-template]

4. Klikněte na **OK** vytvořte projekt.

## <a name="create-the-aspnet-mvc-application"></a>Vytvoření aplikace ASP.NET MVC

V této části kurzu vytvoříte základní aplikace, která bude číst a zobrazí statistiku týmu z databáze.

-   [Přidat do modelu](#add-the-model)
-   [Přidání správce](#add-the-controller)
-   [Konfigurace zobrazení](#configure-the-views)

### <a name="add-the-model"></a>Přidat do modelu

1. Klikněte pravým tlačítkem myši **modely** v **Okně Průzkumník**a zvolte **Přidat** **předmětu**. 

    ![Přidání modelu][cache-model-add-class]

2. Zadejte `Team` předmětu název a klikněte na **Přidat**.

    ![Přidání modelu třídy][cache-model-add-class-dialog]

3. Nahrazení `using` příkazy v horní části `Team.cs` soubor s tímto pomocí příkazů.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Nahrazení definici `Team` třídy pomocí následující fragment kódu, který obsahuje aktualizovaný `Team` třídy definice, jakož i některé další třídy Entity Framework podpory. Další informace o kódu první přístup k Entity Framework, který slouží v tomto kurzu najdete v tématu [kód nejdřív pro novou databázi](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. V **Okně Průzkumník**poklepejte **web.config** a otevřete ho.

    ![Web.config][cache-web-config]

3.  Přidejte následující připojovací řetězec do `connectionStrings` oddílu. Název connection_string musí odpovídat název místní třídy Entity Framework databáze, která je `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Po přidání toho `connectionStrings` oddíl by měla vypadat podobně jako v následujícím příkladu.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Přidání správce

1. Stisknutím klávesy **F6** sestavení projektu. 
2. V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši na složku **řadiče** a zvolte **Přidat** **řadiče domény**.

    ![Přidání řadiče domény][cache-add-controller]

3. Zvolte **MVC 5 řadiče se zobrazeními pomocí Framework entita**a klikněte na **Přidat**. Pokud dojde k chybě po kliknutí na příkaz **Přidat**Ujistěte se, že jste vytvořili projektu nejdřív.

    ![Přidání řadiče třídy][cache-add-controller-class]

5. Vyberte **tým (ContosoTeamStats.Models)** z rozevíracího seznamu **modelu předmětu** . **TeamContext (ContosoTeamStats.Models)** vyberte z rozevíracího seznamu **třídy kontextu Data** . Typ `TeamsController` **řadiče** název do textového pole (Pokud není zadáno automaticky). Klikněte na **Přidat** k vytvoření třídy řadiče domény a přidání výchozí zobrazení.

    ![Konfigurace řadiče domény][cache-configure-controller]

4. V **Okně Průzkumník**rozbalte **Global.asax** a poklikejte na **Global.asax.cs** a otevřete ho.

    ![Global.asax.cs][cache-global-asax]

5. Přidání následujících dvou pomocí příkazů v horní části souboru v jiné pomocí příkazů.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Přidat následující kód na konci `Application_Start` metody.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. V **Okně Průzkumník**rozbalte `App_Start` a poklikejte na `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Nahrazení `controller = "Home"` následující kód v `RegisterRoutes` metodu s `controller = "Teams"` jak je vidět v následujícím příkladu.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Konfigurace zobrazení

1. V **Okně Průzkumník**rozbalte složku **zobrazení** a potom na **sdílenou** složku a poklikejte na **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Změnit obsah `title` prvek a nahraďte `My ASP.NET Application` s `Contoso Team Stats` jak je vidět v následujícím příkladu.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. V `body` oddílu, aktualizovat první `Html.ActionLink` údajů a nahradit `Application name` s `Contoso Team Stats` a nahradit `Home` s `Teams`.
    -   Před:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Po nastavení:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Změny kódu][cache-layout-cshtml-code]

4. Stisknutím **Kombinace kláves Ctrl + F5** k vytvoření a spuštění aplikace. Tato verze aplikace načítá výsledky přímo z databáze. Všimněte si **Vytvořit nový**, **Upravit**, **Podrobnosti**a **Odstranění** akce, které byly automaticky přidají i do aplikace tak, že scaffold **MVC 5 řadiče s zobrazeními pomocí Entity Framework** . V další části tohoto kurzu budete přidávat Redis mezipaměti optimalizovat přístup k datům a poskytují další funkce aplikace.

![Aplikace Starter][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Nakonfigurovat aplikaci pro použití Redis mezipaměti

V této části kurzu nakonfigurujete ukázková aplikace k ukládání a získání statistických údajů týmu Contoso z instance mezipaměti Redis Azure pomocí klienta [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) mezipaměti.

-   [Nakonfigurovat aplikaci pro použití StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [Aktualizovat třídu TeamsController tak, aby vracel výsledky z mezipaměti nebo databáze](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Aktualizovat způsoby vytvoření, úprava a odstranění pro práci s mezipaměti](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Aktualizovat rejstřík týmy zobrazení pro práci s mezipaměti](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Nakonfigurovat aplikaci pro použití StackExchange.Redis

1. Konfigurace klientské aplikace ve Visual Studiu použití balíčku StackExchange.Redis NuGet, klikněte pravým tlačítkem myši na projekt v **Okně Průzkumník** a zvolte **Spravovat balíčků NuGet**. 

    ![Správa NuGet balíčků][redis-cache-manage-nuget-menu]

2. Do textového pole Hledat zadejte **StackExchange.Redis** , vyberte požadovanou verzi z pole výsledky a klikněte na tlačítko **nainstalovat**.

    ![StackExchange.Redis NuGet balíčku][redis-cache-stack-exchange-nuget]

    Balíček NuGet a soubory ke stažení přidá požadované sestavení odkazy na klientské aplikace pro přístup k Azure Redis mezipaměti s klientem StackExchange.Redis mezipaměti. Pokud chcete použít silným názvem verzi knihovnu **StackExchange.Redis** klienta, zvolte **StackExchange.Redis.StrongName**; v opačném případě vyberte **StackExchange.Redis**.

3. V **Okně Průzkumník**rozbalte složku **řadiče** a poklikejte na **TeamsController.cs** a otevřete ho.

    ![Řadiče domény týmy][cache-teamscontroller]

4. Přidání následujících dvou pomocí příkazů na **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Přidejte následující dvě vlastnosti pro `TeamsController` předmětu.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Vytvoření souboru na počítači s názvem `WebAppPlusCacheAppSecrets.config` a jeho umístění v umístění, do kterého neproběhne pomocí zdrojového kódu aplikace ukázkové měli jste se rozhodli vrácení se změnami někde. V tomto příkladu `AppSettingsSecrets.config` je soubor umístěn v `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Upravit `WebAppPlusCacheAppSecrets.config` soubor a přidejte následující obsah. Pokud spustíte aplikaci místně tyto informace se používá pro připojení k instanci aplikace Azure Redis mezipaměti. Dále v tomto kurzu se zřízení instance Azure Redis mezipaměti a aktualizujte mezipaměti jméno a heslo. Pokud nemáte v plánu spustit aplikaci ukázkové místně můžete přejít vystavením tento soubor a další kroky, které odkazují na soubor vzhledem k tomu, že při nasazení Azure aplikace načte informace o připojení mezipaměti z aplikace nastavení pro Web App a ne z tohoto souboru. Protože `WebAppPlusCacheAppSecrets.config` není používaný k Azure s aplikací, nemusíte ho Pokud přecházíte místní spuštění aplikace.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. V **Okně Průzkumník**poklepejte **web.config** a otevřete ho.

    ![Web.config][cache-web-config]

3. Přidejte následující `file` atribut `appSettings` prvek. Pokud jste použili jiný název souboru či v jiném umístění, nahraďte tyto hodnoty z nich uvedeno v příkladu.
    -   Před:`<appSettings>`
    -   Po nastavení:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Modul runtime ASP.NET sloučí obsah souboru externí se značkami v `<appSettings>` prvek. Modul runtime ignoruje atribut souboru zadaný soubor nebyl nalezen. Tajemství (připojovací řetězec do mezipaměti) nejsou součástí zdrojového kódu pro aplikaci. Při nasazení webovou aplikaci Azure, což `WebAppPlusCacheAppSecrests.config` soubor nebude nasazena (to znamená, co potřebujete). Existuje několik způsobů, jak zadat tyto tajemství v Azure a v tomto kurzu jsou nakonfigurovány automaticky za vás při [zřizování Azure zdroje](#provision-the-azure-resources) na pozdější kurz krok. Podrobnosti o práci s tajemství v Azure najdete v článku [Doporučené postupy pro nasazení hesla a další citlivá data ASP.NET a služba Azure aplikací](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualizovat třídu TeamsController tak, aby vracel výsledky z mezipaměti nebo databáze

V tomto příkladu týmu statistiky načtená z databáze nebo z mezipaměti. Týmu statistiky jsou uložené v mezipaměti jako sériové `List<Team>`a také jako seřazené sadu pomocí Redis datové typy. Při načítání položek ze sady seřazené, můžete některé načíst nebo dotazu pro některé položky. V tomto příkladu se dotaz seřazené sadě pro začátek 5 týmy seřazené podle počtu wins.

>[AZURE.NOTE] Není potřeba ukládat statistiky týmu v několika formátů do mezipaměti mohli použít Azure Redis mezipaměti. Tento kurz pomocí různých formátech ukazují některé způsoby a různými typy dat, které vám pomohou data v mezipaměti.



1. Přidejte následující pomocí příkazů na `TeamsController.cs` soubor nahoře jiným pomocí příkazů.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Nahradit aktuální `public ActionResult Index()` metodu s následující implementaci.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Přidejte následující tři způsoby `TeamsController` třídy pro implementaci `playGames`, `clearCache`, a `rebuildDB` typů akcí z příkazu switch přidané v předchozí fragment kódu.

    `PlayGames` Metoda aktualizuje statistiku týmu pomocí simulace season her výsledky se uloží do databáze a vymaže teď zastaralá data z mezipaměti.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    `RebuildDB` Metoda k nové inicializaci databáze se výchozí sada týmy vygeneruje statistiky pro ně a vymaže teď zastaralá data z mezipaměti.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    `ClearCachedTeams` Metoda odebere všechny režim cached týmu statistiky z mezipaměti.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Přidejte následující čtyři metody `TeamsController` třídy provádět různé způsoby načítání statistiku týmu z mezipaměti a databáze. Každá z těchto metod vrátí `List<Team>` které se zobrazí v zobrazení.

    `GetFromDB` Metoda čte statistiku týmu z databáze.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    `GetFromList` Metoda čte statistiku týmu z mezipaměti jako sériové `List<Team>`. Pokud existuje paní mezipaměti, statistiku týmu číst z databáze a pak uložené v mezipaměti pro příště. V tomto příkladu jsme používáte JSON.NET serializace serializovat .NET objekty do a z mezipaměti. Další informace najdete v tématu [jak pracovat s .NET objektů v Azure Redis mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    `GetFromSortedSet` Metoda čte statistiku týmu z mezipaměti seřazené sadě. Pokud existuje paní mezipaměti, statistiku týmu číst z databáze a uložené v mezipaměti jako seřazené sady.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    `GetFromSortedSetTop5` Metoda čte týmy 5 hlavních ze sady seřazené v mezipaměti. Spuštění kontrolou mezipaměti existenci `teamsSortedSet` klíče. Pokud není k dispozici, tento klíč `GetFromSortedSet` metodu volat pro čtení statistiku týmu a byly ukládány v mezipaměti. Další režim cached seřazené sadě dotazovaném pro začátek 5 týmy, které se vracejí.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Aktualizovat způsoby vytvoření, úprava a odstranění pro práci s mezipaměti

Vygenerovaných kód, který byl vytvořen jako součást v tomto příkladu obsahuje metody přidávat, upravovat a odstraňovat týmy. Pokaždé, když týmu přidat, upravit nebo odebrat, bude zastaralá data v mezipaměti. V této části upravíte tyto tři způsoby vymazání mezipaměti týmy tak, aby mezipaměti nebudou se nesynchronizují s databází.

1. Přejděte `Create(Team team)` metoda `TeamsController` předmětu. Přidejte volání `ClearCachedTeams` způsob, jak je vidět v následujícím příkladu.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Přejděte `Edit(Team team)` metoda `TeamsController` předmětu. Přidejte volání `ClearCachedTeams` způsob, jak je vidět v následujícím příkladu.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Přejděte `DeleteConfirmed(int id)` metoda `TeamsController` předmětu. Přidejte volání `ClearCachedTeams` způsob, jak je vidět v následujícím příkladu.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualizace zobrazení týmů rejstříku pro práci s mezipaměti

1. V **Okně Průzkumník**složku **zobrazení** , klikněte na složku **týmy** a poklikejte na **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. V horní části souboru hledejte následující element odstavce.

    ![Tabulka akcí][cache-teams-index-table]

    Toto je odkaz na vytvořit nový týmový. Nahraďte element odstavce v následující tabulce. Tato tabulka obsahuje odkazy akcí pro vytvoření nového týmového, přehrávání nové season her, vymazání mezipaměti, načítání týmy z mezipaměti v různých formátech, načítání týmy z databáze a opětovné vytvoření databáze s svěže ukázkovými daty.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Přejděte na konec souboru **Index.cshtml** a přidejte následující `tr` prvek tak, aby je poslední řádek v tabulce poslední v souboru.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    V tomto řádku se zobrazí hodnoty `ViewBag.Msg` obsahující zprávy o stavu o aktuální operaci, který se nastaví, když klepnete na jeden z odkazů v části akce v předchozím kroku.   

    ![Stavová zpráva][cache-status-message]

4. Stisknutím klávesy **F6** sestavení projektu.

## <a name="provision-the-azure-resources"></a>Zřízení Azure zdroje

Hostovat aplikace v Azure, musíte nejdřív zřizujete Azure služby, které aplikace vyžaduje. Ukázková aplikace v tomto kurzu používá následující služby Azure.

-   Azure Redis mezipaměti
-   Aplikace služby Web Appu
-   Databáze SQL

Abyste mohli nasadit těchto služeb do nového nebo existujícího zdroje skupiny podle svého výběru, klikněte na toto tlačítko **instalovat na Azure** .

[! [Nasazení Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Kliknutím na toto tlačítko **instalovat na Azure** používá šablony [Vytvořit Web App plus Redis mezipaměti plus databáze SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure rychlý úvod](https://github.com/Azure/azure-quickstart-templates) zřízení těchto služeb a vyberte připojovací řetězec pro databázi SQL a nastavení aplikace pro připojovací řetězec Azure Redis mezipaměti.

>[AZURE.NOTE] Pokud nemáte účet Azure, můžete [Vytvořit bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) v jenom pár minut.

Klikněte na tlačítko **instalovat na Azure** přejdete na portál Azure a zahájí proces vytváření uvedené šablonou zdroje.

![Nasazení Azure][cache-deploy-to-azure-step-1]

1. Na zásuvné **nasazení vlastní** vyberte Azure předplatné použít a vyberte existující skupinu zdrojů nebo vytvořte nový účet a zadejte umístění skupina zdroje.
2. Na zásuvné **Parametry** zadejte název účtu správce (**ADMINISTRATORLOGIN** - nepoužívejte **Správce**), přihlašovacího hesla pro správce (**ADMINISTRATORLOGINPASSWORD**) a název databáze (**název databáze**). Další parametry nakonfigurovaná pro hostování plán a nižší náklady možnosti databáze a SQL Azure Redis mezipaměti, které nejsou součástí bezplatné osy bezplatnou aplikaci služby.
3. Nezměnili žádné další nastavení, pokud budete chtít, nebo ponechejte výchozí hodnoty a klikněte na **OK**.


![Nasazení Azure][cache-deploy-to-azure-step-2]

1. Klikněte na **Revize právní podmínky**.
2. Přečtěte si podmínky na zásuvné **nákupní** a klikněte na **koupit**.
3. Začněte zřizování zdrojů klikněte na **vytvořit** na zásuvné **Vlastní nasazení** .

Zobrazení průběhu nasazením, klikněte na ikonu oznámení a klepněte na **Začínáme nasazení**.

![Nasazení Začínáme][cache-deployment-started]

Zobrazení stavu vaší instalace na zásuvné **Microsoft.Template** .

![Nasazení Azure][cache-deploy-to-azure-step-3]

Po dokončení zřizování publikováním aplikaci Azure z aplikace Visual Studio.

>[AZURE.NOTE] Chybách, ke kterým může dojít během procesu zřizovací se zobrazí na zásuvné **Microsoft.Template** . Běžné chyby nejsou příliš mnoho servery SQL ani příliš mnoho bezplatnou aplikaci hostování služby plány jedno předplatné. Vyřešit chyby a znovu spustit proces kliknutím **přeinstalujte** na zásuvné **Microsoft.Template** nebo na tlačítko **instalovat na Azure** v tomto kurzu.

## <a name="publish-the-application-to-azure"></a>Publikování aplikace Azure

V tomto kroku kurzu budete publikovat aplikace Azure a spouštět v cloudu.

1. Klikněte pravým tlačítkem myši na **ContosoTeamStats** projekt ve Visual Studiu a vyberte **Publikovat**.

    ![Publikování][cache-publish-app]

2. Klikněte na **Microsoft Azure aplikaci služby**.

    ![Publikování][cache-publish-to-app-service]

3. Vyberte předplatné při vytváření Azure zdrojů, rozbalte skupina zdroje obsahující zdroje, vyberte požadovaný Web App a klikněte na **OK**. Pokud jste použili na tlačítko **instalovat na Azure** své jméno v prohlížeči začíná **webu** a za ním uveďte některé další znaky.

    ![Vyberte v prohlížeči][cache-select-web-app]

4. Kliknutím na **Ověřit připojení** zkontrolujte si nastavení a potom klikněte na **Publikovat**.

    ![Publikování][cache-publish]

    Za několik okamžiků bude dokončení procesu publikování a prohlížeči se spustí provozu vzorové aplikace. Pokud dojde k chybě DNS při ověřování nebo publikování a právě naposledy dokončení procesu zřizovací Azure zdroje pro aplikaci, chvíli počkejte a zkuste to znovu.

    ![Přidání mezipaměti][cache-added-to-application]

Následující tabulka popisuje jednotlivé odkazy akci z ukázkové aplikace.

| Akce                  | Popis                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vytvořit nový              | Vytvoření nového týmu.                                                                                                                                               |
| Přehrát období             | Přehrát season hry, aktualizovat stat týmu a zrušte zaškrtnutí zastaralé týmu dat z mezipaměti.                                                                          |
| Vymazat mezipaměť             | Zrušte zaškrtnutí políčka stat týmu z mezipaměti.                                                                                                                             |
| Seznam z mezipaměti         | Načtení stat týmu z mezipaměti. Pokud existuje paní mezipaměti, dejte stat z databáze a ukládání do mezipaměti pro příště.                                        |
| Seřazený k sadám z mezipaměti   | Načtení stat týmu z mezipaměti pomocí seřazené sady. Pokud existuje paní mezipaměti, dejte stat z databáze a ukládání do mezipaměti pomocí seřazené sadě.  |
| 5 hlavních týmy z mezipaměti  | Načtení horní 5 týmy z mezipaměti pomocí seřazené sady. Pokud existuje neúspěšných mezipaměti, dejte stat z databáze a ukládání do mezipaměti pomocí seřazené sadě. |
| Načtení z databáze            | Získání stat týmu z databáze.                                                                                                                       |
| Opětovné vytvoření databáze              | Opětovné vytvoření databáze a načíst s ukázkovými daty týmu.                                                                                                        |
| Úprava / podrobnosti a odstranění | Úprava týmu, zobrazte podrobnosti o týmu, tým odstranit.                                                                                                             |


Klikněte na některé akce a Experimentujte s načtení dat z různých zdrojů. Není rozdílech mezi časem bude trvat různých způsobů načtení dat z databáze a mezipaměti.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Odstranění zdroje po dokončení s aplikací

Po dokončení s ukázkovou výuková aplikaci můžete odstranit Azure zdrojů použitých k ušetřit náklady a prostředky. Pokud používáte na tlačítko **instalovat na Azure** v části [zřízení Azure zdroje](#provision-the-azure-resources) a všechny zdroje jsou obsaženy ve stejné skupině zdroje, můžete odstranit je společně v jednom kroku odstraněním skupina zdroje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com) a klikněte na **skupiny zdrojů**.
2. Do **Filtrovat položky** do textového pole zadejte název skupiny prostředků.
3. Kliknutím na **** napravo od skupina zdroje.
4. Klikněte na **Odstranit**.

    ![Odstranění][cache-delete-resource-group]

5. Zadejte název skupiny zdrojů a klikněte na **Odstranit**.

    ![Potvrzení odstranění][cache-delete-confirm]

Za několik okamžiků odstraňují skupina zdroje a všech jeho obsažené prostředků.

>[AZURE.IMPORTANT] Všimněte si, že odstranění skupina zdroje je nevratné a trvale odstraněna skupina zdroje a všechny zdroje v něm. Ujistěte se, že není odstraníte omylem nepovedlo skupina nebo zdrojů. Pokud jste vytvořili v tématech hostingu Tato ukázka do existující skupiny zdrojů, můžete odstranit jednotlivé zdroje jednotlivě z příslušné listy.

## <a name="run-the-sample-application-on-your-local-machine"></a>Spusťte aplikaci vzorku na místním počítači

Místní spuštění aplikace v počítači, je nutné instanci Azure Redis mezipaměti ve kterém mají vaše data v mezipaměti. 

-   Pokud jste publikovali aplikaci Azure podle popisu v předchozí části, můžete použít instanci Azure Redis mezipaměti, protože ji někdo zřídil v tomto kroku.
-   Pokud máte jiné existující instance Azure Redis mezipaměti, můžete, místně v tomto příkladu.
-   Pokud potřebujete k vytvoření instance Azure Redis mezipaměti, můžete postupujte podle kroků v tématu [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Jakmile jste vybrali nebo vytvořili mezipaměti používat, přejděte do mezipaměti v portálu Azure a načíst [název hostitele](cache-configure.md#properties) a [přístupových kláves z verze](cache-configure.md#access-keys) pro mezipaměť. Pokyny najdete v tématu [Konfigurace Redis nastavení mezipaměti](cache-configure.md#configure-redis-cache-settings).

1. Otevřít `WebAppPlusCacheAppSecrets.config` soubor, který jste vytvořili při kroku [Konfigurovat aplikaci pro použití Redis mezipaměti](#configure-the-application-to-use-redis-cache) tento kurz slouží editor jazyka podle svého výběru.

2. Upravit `value` atribut a nahradit `MyCache.redis.cache.windows.net` [název hostitele](cache-configure.md#properties) mezipaměť a určit buď na [primární a sekundární klíč](cache-configure.md#access-keys) mezipaměť jako heslo.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Stiskněte **Kombinaci kláves Ctrl + F5** spustit aplikaci.

>[AZURE.NOTE] Všimněte si, protože aplikace, včetně databáze, běží místně a mezipaměti Redis je hostovaný v Azure, mezipaměti se může zobrazit ve skupinovém rámečku provést databáze. Nejlepších výsledků dosáhnete klientské aplikace a instanci Azure Redis mezipaměti musí být ve stejném umístění. 

## <a name="next-steps"></a>Další kroky

-   Další informace o webu [ASP.NET](http://asp.net/) [Začínáte pracovat s ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) .
-   Další příklady vytváření webovou aplikaci ASP.NET v aplikaci služby naleznete v tématu [Vytvoření a nasazení aplikace ASP.NET webové aplikace služby Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) z připojit [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Rychlé další prohlídky z ukázku HealthClinic.biz najdete v článku [Rychlé Azure vývojář nástroje prohlídky](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Další informace o [kódu nejdřív pro novou databázi](https://msdn.microsoft.com/data/jj193542) přístup k Entity Framework, který slouží v tomto kurzu.
-   Další informace o [webových aplikacích aplikace služby Azure](../app-service-web/app-service-web-overview.md).
-   Zjistěte, jak [Sledování](cache-how-to-monitor.md) mezipaměť Azure portálu.

-   Prozkoumání Azure Redis mezipaměti prémiových funkcí
    -   [Postup při konfiguraci trvalé pro mezipaměť Redis Premium Azure](cache-how-to-premium-persistence.md)
    -   [Postup při konfiguraci clusterů pro mezipaměť Redis Premium Azure](cache-how-to-premium-clustering.md)
    -   [Postup při konfiguraci virtuální sítě podpory pro mezipaměť Redis Premium Azure](cache-how-to-premium-vnet.md)
    -   V tématu [Azure Redis mezipaměti nejčastější dotazy týkající se](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) Další informace o velikosti, výkon a šířky pásma s mezipamětí premium.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

