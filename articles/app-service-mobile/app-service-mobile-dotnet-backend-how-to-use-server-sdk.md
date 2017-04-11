<properties
    pageTitle="Jak pracovat se serverem back-end .NET SDK k aplikacím Mobile | Azure aplikace služby"
    description="Informace o práci se serverem back-end .NET SDK Azure aplikace služby mobilních aplikací."
    keywords="aplikace služby azure aplikaci služby, mobilní aplikace, Mobilní služba nasazení aplikace nasazení, azure scalable, aplikace měřítko"
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

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Práce s back-end serveru .NET SDK k aplikacím Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

V tomto tématu se dozvíte, jak používat .NET back-end serveru SDK v klíčové scénáře Azure aplikace služby mobilní aplikace. SDK Azure mobilní aplikace umožňuje práci s mobilních klientů aplikace ASP.NET.

>[AZURE.TIP] [.NET server SDK k aplikacím Mobile Azure] [ 2] je otevřít zdroj na GitHub. Úložiště obsahuje všechny zdrojového kódu včetně celého serveru SDK Jednotková testovací sady a některé ukázkové projekty.

## <a name="reference-documentation"></a>Si přečtěte následující dokumentaci odkaz

Tady je umístěn odkaz si přečtěte následující dokumentaci pro server SDK: [Azure mobilní aplikace .NET odkaz][1].

## <a name="create-app"></a>Postup: vytvoření back-end .NET mobilní aplikaci

Pokud spouštíte nového projektu, můžete vytvořit aplikaci služby aplikace pomocí [Azure portál] nebo Visual Studio. Můžete spustit aplikaci služby aplikací místně nebo publikování projektu do cloudové služby aplikace mobilní aplikace pro.  

Chcete-li přidat mobilní funkce do existujícího projektu, naleznete v části [Stáhnout a inicializace SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Vytvoření .NET back-end pomocí portálu Azure

Aplikaci služby mobilní back-end buď vytvoříte [Rychlý úvod kurzu] [ 3] nebo postupujte podle těchto kroků:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Po návratu do zásuvné _Začínáme_ v části **vytvořit tabulku rozhraní API**zvolte **C#** jako **jazyk back-end**. Klikněte na tlačítko **Stáhnout**, extrahovat souborů komprimovaných projektu do místního počítače a otevřete řešení ve Visual Studiu.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Vytvoření .NET back-end pomocí aplikace Visual Studio 2013 a Visual Studio 2015

Instalace [Azure SDK pro .NET] [ 4] (verze 2.9.0 nebo novější) k vytvoření projektu Azure mobilních aplikací ve Visual Studiu. Po instalaci SDK vytvořte aplikaci ASP.NET pomocí následujících kroků:

1. Otevření dialogového okna **Nový projekt** (ze *souboru* > **Nový** > **projekt...**).
2. Rozbalení **šablony** > **Visual Basic**a vyberte **Web**.
3. Vyberte **webové aplikace**.
4. Zadejte název projektu. Klikněte na **OK**.
5. V části _ASP.NET 4.5.2 šablony_, vyberte **Azure mobilní aplikaci**. Zaškrtněte políčko **hostitele v cloudu** vytvořit mobilní back-end v cloudu, do kterého můžete publikovat tohoto projektu.
6. Klikněte na **OK**.

## <a name="install-sdk"></a>Postup: Stáhněte si a inicializace SDK

V SDK je dostupná na [NuGet.org]. Balíček obsahuje základní funkce povinné začít používat v SDK. Inicializace SDK, budete muset provádět akce na objekt **HttpConfiguration** .

### <a name="install-the-sdk"></a>Instalace SDK

Instalace SDK, klikněte pravým tlačítkem myši na project server ve Visual Studiu, vyberte **Správa balíčků NuGet**, vyhledejte balíček [Microsoft.Azure.Mobile.Server] , klikněte na **nainstalovat**.

###<a name="server-project-setup"></a>Inicializace project serveru

Project server back-end .NET smyčky podle podobně jako jiné projekty ASP.NET s využitím třídy spuštění OWIN. Ujistěte se, že máte odkazuje balíček NuGet `Microsoft.Owin.Host.SystemWeb`. Přidání této třídy ve Visual Studiu, klikněte pravým tlačítkem myši na server project a klikněte na **Přidat** > 
**Nová položka**a potom **Web** > **Obecné** > **OWIN spuštění předmětu**.  Blok předmětu vytvořený pomocí následující atribut:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

V `Configuration()` způsob svojí třídě spuštění OWIN objektu **HttpConfiguration** používá ke konfiguraci prostředí Azure mobilní aplikace.
Následující příklad inicializuje project serveru s přidanými funkcemi:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Povolit jednotlivých funkcí, musíte volat rozšíření metody **MobileAppConfiguration** objektu před voláním **ApplyTo**. Například následující kód přidá výchozí trasy všechny řadiče rozhraní API, které mají atribut `[MobileAppController]` během inicializace:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Rychlý úvod serveru z portálu Microsoft Azure hovorů **UseDefaultConfiguration()**. Tento odpovídá následující nastavení:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Rozšíření metod jsou:

* `AddMobileAppHomeController()`obsahuje výchozí Azure mobilní aplikace domovské stránky.
* `MapApiControllers()`poskytuje vlastní možnosti rozhraní API pro WebAPI řadiče ozdobenou s `[MobileAppController]` atribut.
* `AddTables()`poskytuje mapování `/tables` koncových bodů řadiče tabulky.
* `AddTablesWithEntityFramework()`je skladě krátký pro mapování `/tables` koncové body pomocí Entity Framework na základě řadiče.
* `AddPushNotifications()`poskytuje jednoduchý způsob registrace zařízení rozbočovače oznámení.
* `MapLegacyCrossDomainController()`poskytuje standardní CORS záhlaví pro místní vývoj.

### <a name="sdk-extensions"></a>Rozšíření SDK

Následující balíčky na základě NuGet rozšíření poskytují různé funkcí pro mobilní zařízení, které můžete použít v aplikaci. Povolit rozšíření během inicializace pomocí **MobileAppConfiguration** objektu.

- [Microsoft.Azure.Mobile.Server.Quickstart] podporuje základní nastavení mobilních aplikací. Ke konfiguraci přidány tak, že zavoláte metodu rozšíření **UseDefaultConfiguration** během inicializace. Tuto linku zahrnuje následující rozšíření: oznámení, ověřování, entitu, tabulek, doménami a balíčků Domů. Rychlý úvod mobilní aplikace dostupná na portálu Azure používá tento balíček.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   implementuje výchozí *Tento mobilní aplikace je do začátků stránky* ke kořenovému webu. Přidáte do konfigurace tak, že zavoláte metodu   **AddMobileAppHomeController** rozšíření.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   zahrnuje třídy pro práci s daty a nakreslenými sady datového kanálu. Přidáte do konfigurace tak, že zavoláte metodu **AddTables** rozšíření.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   umožňuje Entity Framework pro přístup k datům v databázi SQL. Přidáte do konfigurace tak, že zavoláte metodu **AddTablesWithEntityFramework** rozšíření.

- [Microsoft.Azure.Mobile.Server.Authentication] umožňuje ověřování a nakreslenými sady OWIN middleware používaný k ověření tokeny. Přidat do konfigurace tak, že zavoláte **AddAppServiceAuthentication**  
   a **IAppBuilder**. **UseAppServiceAuthentication** rozšíření metody.

- [Microsoft.Azure.Mobile.Server.Notifications] umožňuje nabízená oznámení a definuje koncový bod registrace připínáčku. Přidáte do konfigurace tak, že zavoláte metodu **AddPushNotifications** rozšíření.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   vytvoří zařízení, které bude sloužit dat starších webových prohlížečích v mobilní aplikaci. Přidáte do konfigurace tak, že zavoláte metodu   **MapLegacyCrossDomainController** rozšíření.

- [Microsoft.Azure.Mobile.Server.Login] poskytuje metodu AppServiceLoginHandler.CreateToken(), což je statické metoda používaný během scénáře vlastní ověřování.   

## <a name="publish-server-project"></a>Postup: publikování project serveru

Tato část popisuje projekt back-end .NET z aplikace Visual Studio publikovat. Můžete taky nasadit back-end projektu přes libovolná nebo na jakékoli jiné metody podle [si přečtěte následující dokumentaci nasazení služby Azure aplikace](../app-service-web/web-sites-deploy.md).

1. Ve Visual Studiu znovu vytvořte projekt obnovíte NuGet balíčků.

2. V okně Průzkumník projektu klikněte pravým tlačítkem myši, klikněte na **Publikovat**. Při prvním publikování, musíte definovat publikování profil. Pokud už máte profilu definované, můžete ho vyberte a klikněte na **Publikovat**.

2. Pokud se zobrazí výzva k výběru cíl publikování, klikněte na **Microsoft Azure aplikaci služby** > **Další**, a pak (v případě potřeby) Přihlaste se pomocí svých přihlašovacích údajů Azure. 
   Visual Studiu a soubory ke stažení bezpečně ukládá vaše nastavení přímo z Azure publikování.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Vyberte **předplatné**, vyberte **Typ zdroje** ze **zobrazení**, rozbalte **Mobilní aplikaci**a klikněte na vaše mobilní aplikaci back-end a potom na **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Ověření údajů o publikování profilu a klikněte na **Publikovat**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Po úspěšném publikování mobilní aplikaci backendovou uvidíte cílová stránka oznamující úspěch.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Postup: definování řadiči tabulky

Definování řadiči tabulky vystavit tabulky SQL pro mobilní klienty.  Konfigurace řadiči tabulky vyžaduje tři kroky:

1. Vytvoření třídy objekt přenos dat (DTO).
2. Konfigurace odkaz na tabulku v předmětu DbContext mobilní telefon.
3. Vytvoření tabulky řadiče.

Objekt pro přenos dat (DTO) je obyčejný C# objekt, který dědí od `EntityData`.  Příklad:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO slouží k definování tabulky v databázi SQL.  Pokud chcete vytvořit položku databáze, přidejte `DbSet<>` vlastnost DbContext používáte.  V projektu výchozí šablonu pro Azure mobilní aplikace DbContext nazývá `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Pokud máte nainstalovanou SDK Azure, teď můžete vytvářet řadiči šablony tabulky takto:

1. Klikněte pravým tlačítkem myši na složku řadiče a vyberte **Přidat** > **řadiče …**.
2. Vyberte možnost **Tabulka řadiče domény Azure mobilní aplikace** a potom klikněte na **Přidat**.
3. V dialogovém okně **Přidat řadiče** :
    * V rozevíracím seznamu **třídy modelu** vyberte nové DTO.
    * V rozevíracím seznamu **DbContext** vyberte Mobile Service DbContext předmětu.
    * Název řadiče se vytvoří.
4. Klikněte na **Přidat**.

Project server rychlý úvod obsahuje příklad jednoduchého **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Jak se: přizpůsobení stránkování velikosti tabulky

Ve výchozím nastavení aplikací Mobile Azure vrátí 50 záznamů na žádost o.  Stránkovací zaručuje, že klienta neblokuje jejich vlákna uživatelského rozhraní ani na serveru moc dlouho zajistí dobrý uživatelské prostředí. Pokud chcete změnit velikost tabulky stránkování, zvýšit na straně serveru "velikost dotazu oprávnění" a velikost stránky klienta na straně serveru "povolenou velikost dotazu" je upravit pomocí `EnableQuery` atribut:

    [EnableQuery(PageSize = 500)]

Zajištění PageSize stejné nebo větší než velikost požadované klientem.  Podívejte se do konkrétního klienta si přečtěte následující dokumentaci postupy podrobnosti o změně velikosti stránky klienta.

## <a name="how-to-define-a-custom-api-controller"></a>Postup: definování vlastního rozhraní API zařízení

Vlastní zařízení rozhraní API nabízí základní funkce pro mobilní aplikaci backendovou vystavením koncový bod. Můžete zaregistrovat řadiči rozhraní API specifických pro mobilní použití atribut [MobileAppController]. `MobileAppController` Atribut zaregistruje postupu nastaví serializer mobilní aplikace JSON a také bude zapnuta [Kontrola verzi klienta](app-service-mobile-client-and-server-versioning.md).

1. Ve Visual Studiu, klikněte pravým tlačítkem myši na složku řadiče a potom klikněte na tlačítko **Přidat** > domény**řadiče domény**, vyberte **webového rozhraní API 2 řadiče&mdash;prázdné** a klikněte na **Přidat**.

2. Zadat **název řadiče domény**, například `CustomController`a klikněte na **Přidat**.

3. V souboru třídy nové řadiče domény přidejte následující text pomocí příkazu:

        using Microsoft.Azure.Mobile.Server.Config;

4. Použití atribut **[MobileAppController]** rozhraní API řadiče definice třídy jako v následujícím příkladu:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. Ve zdrojovém souboru App_Start/Startup.MobileApp.cs přidejte volání metody **MapApiControllers** příponu, jako v následujícím příkladu:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Můžete taky použít `UseDefaultConfiguration()` rozšíření metoda namísto `MapApiControllers()`. Všechny řadiče domény, ve kterém není **MobileAppControllerAttribute** použité můžete pořád přístupné pro klienty, ale nemusí být zpracován správně klienty pomocí libovolného mobilní aplikaci klienta SDK.

## <a name="how-to-work-with-authentication"></a>Postup: práce s ověřování

Azure aplikace mobilní používá ověřování služby aplikace / se tak mohli ověřovat zabezpečit mobilní back-end.  V této části se dozvíte, jak provádět následující úkoly týkající se ověřování v projektu .NET back-end serveru:

+ [Postup: přidání ověřování serveru projektu](#add-auth)
+ [Postup: použít vlastní ověřování aplikace](#custom-auth)
+ [Postup: získání ověřit informace o uživatelích](#user-info)
+ [Postup: omezit přístup k datům oprávnění uživatelům](#authorize)

### <a name="add-auth"></a>Postup: přidání ověřování serveru projektu

Ověřování můžete přidat do projektu serveru rozšíření **MobileAppConfiguration** objektu a konfigurace OWIN middleware. Po instalaci balíček [Microsoft.Azure.Mobile.Server.Quickstart] a metodu rozšíření **UseDefaultConfiguration** volat, můžete přejít ke kroku 3.

1. Ve Visual Studiu nainstalujte balíček [Microsoft.Azure.Mobile.Server.Authentication] .

2. V souboru projektu Startup.cs přidáte následující kód na začátku metody **Konfigurace** :

        app.UseAppServiceAuthentication(config);

    Tato součást middleware OWIN ověří tokeny vydán přidružená brána aplikaci služby.

3. Přidejte `[Authorize]` atribut řadiče domény nebo metodu, která vyžaduje ověření. 

Další informace o ověřování klientům backendovou mobilních aplikací najdete v tématu [ověření přidat do aplikace](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Postup: použít vlastní ověřování aplikace

Pokud si nepřejete pomocí jednoho z poskytovatelů aplikace služby ověřování/se tak mohli ověřovat webu, můžete používat systému přihlášení. Nainstalujte balíček [Microsoft.Azure.Mobile.Server.Login] kvůli usnadnění tokenu generování ověřování.  Zadání vlastní kód pro ověření přihlašovací údaje uživatele. Může například kontrolu solené a hash hesla v databázi. V následujícím příkladu `isValidAssertion()` metoda (definované jinde) je zodpovědný za tyto kontroly.

Vlastní ověřování vystaven vytvořením ApiController a vystavení `register` a `login` akce. Klient by měl použít vlastní součásti uživatelského rozhraní shromažďování informací od uživatele.  Informace o potom odeslána do rozhraní API standardní HTTP POST hovoru. Jakmile serveru ověří výraz, token vystaven pomocí `AppServiceLoginHandler.CreateToken()` metody.  Použití **neměli** ApiController `[MobileAppController]` atribut. 

Příklad `login` akce:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

V předchozím příkladu LoginResult a LoginResultUser jsou serializovatelný objekty vystavení požadované vlastnosti. Klient očekává přihlášení odpovědi budou vráceny jako JSON objekty ve formuláři:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()` Metoda zahrnuje _cílové skupiny_ a _Vystavitel_ parametr. Oba tyto parametry jsou nastavená na adresa URL kořene aplikace pomocí schématu HTTPS. Podobně byste měli nastavit _secretKey_ je, že hodnota aplikace podpisu klíče. Jak mohou sloužit k Mátová klíče a zosobnění uživatelů není distribuce podpisový klíč v klientovi. Můžete získat podpisový klíč během hostitelem služby aplikace odkazování _webu\_AUTH\_podepisování\_klíč_ proměnné. V případě potřeby v rámci místní ladění, postupujte podle pokynů v části [místní ladění s ověřováním](#local-debug) načíst klíči a uložte ho jako nastavení aplikace.

Vydaný token mohou také obsahovat ostatní nároky a datum vypršení platnosti.  Vydaný token musí obsahovat minimálně, deklaraci předmět (**sub**).

Podpora standardní klienta `loginAsync()` způsobu přetížení směrování ověřování.  Pokud klient volá `client.loginAsync('custom');` mohli přihlásit, musí být váš směrování `/.auth/login/custom`.  Můžete nastavit postupu pro používání vlastní ověřování řadiče `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Použití `loginAsync()` přístup zaručuje, že ověřovací token je připojen k každé následující volání služby.

###<a name="user-info"></a>Postup: získání ověřit informace o uživatelích

Pokud ověření uživatele službou aplikace můžete využít přiřazené ID uživatele a další informace v kódu back-end .NET. Informace o uživateli lze použít při rozhodování se tak mohli ověřovat v back-end. Následující kód získá ID uživatele přidružené k žádost:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

ID zabezpečení je odvozeno z zprostředkovatele uživatelské ID a statická pro daný uživatel a zprostředkovatel přihlášení.  ID zabezpečení pro je null tokeny neplatné ověřování.

Aplikace služby vás také seznámí s požadavky konkrétní nároky na poskytovatele přihlášení. Každý zprostředkovatele identit můžete přidat další informace o používání zprostředkovatele identit SDK.  Můžete například rozhraní API Facebooku grafu přátel informace.  Nároky, které jsou požadovány můžete vyplnit zásuvné poskytovatele Azure portálu. Některé deklarací vyžadovat další konfiguraci pomocí poskytovatele identit.

Následující kód volá metodu **GetAppServiceIdentityAsync** rozšíření získat přihlašovací údaje, které zahrnují přístupový token potřebná k žádosti o proti rozhraní API Facebooku grafu:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Přidat knihovny pomocí příkazu pro `System.Security.Principal` poskytnout metodu **GetAppServiceIdentityAsync** rozšíření.

### <a name="authorize"></a>Postup: omezit přístup k datům oprávnění uživatelům

V předchozí části jsme ukázal, jak získat ID uživatele ověřeného uživatele. Omezit přístup k datům a další zdroje na základě této hodnoty. Jednoduchý způsob, jak omezit vrácená data jenom pro uživatelé je například přidání sloupce ID uživatele do tabulek a filtrování výsledků dotazu podle ID uživatele. Následující kód vrátí řádky dat pouze v případě, že ID zabezpečení použije hodnotu ve sloupci ID v tabulce TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()` Vrátí metoda `IQueryable` , můžete pracovat tak, že LINQ zpracovávání filtrování.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Postup: Přidání nabízených oznámení pro server project

Přidáním nabízených oznámení serveru projektu rozšíření **MobileAppConfiguration** objektu a vytváření klientských rozbočovače oznámení.

1. Ve Visual Studiu, klikněte pravým tlačítkem myši project server a klikněte na **Spravovat balíčků NuGet**, vyhledejte `Microsoft.Azure.Mobile.Server.Notifications`, klikněte na tlačítko **nainstalovat**. 

2. Opakujte tento krok nainstalovat `Microsoft.Azure.NotificationHubs` balíček, který obsahuje knihovnu oznámení rozbočovače klienta.

3. V App_Start/Startup.MobileApp.cs a přidejte volání metodu rozšíření **AddPushNotifications()** během inicializace:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Přidejte následující kód, který vytvoří klienta rozbočovače oznámení:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Teď můžete klientovi oznámení rozbočovače nabízená oznámení odešlete registrovaných zařízení. Další informace najdete v tématu [Přidat nabízených oznámení pro aplikace](app-service-mobile-ios-get-started-push.md). Další informace o oznámení rozbočovače najdete v tématu [Přehled rozbočovače oznámení](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Postup: Povolit určené nabízených pomocí značek

Oznámení rozbočovače můžete neodeslání oznámení cílových konkrétních registracích pomocí značek. Několik značek jsou vytvářeny automaticky:

* ID instalace identifikuje určitému zařízení.
* Id uživatele podle ověřené ID zabezpečení určuje určitým uživatelem.

ID instalace můžete k nim získat přístup z vlastnost **installationId** na **MobileServiceClient**.  Následující příklad ukazuje, jak pomocí ID instalace k instalaci specifické v oznámení rozbočovače můžete přidat značku:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Back-end ignoruje značky poskytnutých klienta při registraci nabízená oznámení při vytváření instalace. Povolit klienta k přidání značek k instalaci, musíte vytvořit vlastní rozhraní API, která přidá značky pomocí předchozího vzorce. 

Zobrazit [značky přidali klienta nabízená oznámení] [ 5] ve výběru aplikace služby mobilní aplikace dokončený rychlý úvod příklad.

##<a name="push-user"></a>Postup: odesílání nabízených oznámení pro ověřeného uživatele

Když ověřený uživatel zaregistruje nabízených oznámení, se automaticky přidají značku ID uživatele k registraci. Pomocí této značky můžete poslat nabízená oznámení na všech zařízeních registrovaných tuto osobu. Následující kód získá ID zabezpečení uživatele, který tvoří žádosti a odešle nabízená oznámení šablony každé registrace zařízení pro tento kontakt:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Při registraci nabízených oznámení z ověřené klienta, ujistěte se, že dokončením ověřování před pokusem o registraci. Další informace najdete v tématu [nabízená uživatelům] [ 6] ve výběru dokončené rychlý úvod aplikace služby mobilní aplikace pro .NET back-end.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Postup: ladění a řešení potíží s .NET serveru SDK

Azure aplikaci služby obsahuje několik ladění a řešení problémů s postupy pro ASP.NET aplikace:

- [Sledování aplikací služby Azure](../app-service-web/web-sites-monitor.md)
- [Povolit diagnostické protokolování v aplikaci služby Azure](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Poradce při potížích s služby Azure aplikace ve Visual Studiu](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Zapnout protokolování

Pomocí standardní zápisu sledování ASP.NET můžete zapisovat do protokoly pro diagnostiku aplikaci služby. Můžete uložit protokoly, musí být povolena Diagnostika v mobilní aplikaci backendovou.

Povolení diagnostických nástrojů a zapisovat do protokolů:

1. Postupujte podle kroků v tématu [jak povolit diagnostických nástrojů](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Přidejte následující pomocí příkazu v souboru kód:

        using System.Web.Http.Tracing;

3. Vytvoření sledování pokyn k zápisu dat ze back-end .NET pro protokolech diagnostiky následujícím způsobem:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Publikujte projekt serveru a přístup k mobilní aplikaci back-end provést kód cestu s protokolování.

5. Stažení a vyhodnotíte protokoly, jak je uvedeno v [jak: stažení protokoly](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Místní ladění s ověřováním

Můžete spustit aplikaci místně otestovat změny před publikováním do cloudu. Většina Azure mobilní aplikace back-end stiskněte klávesu *F5* ve Visual Studiu. Můžou ale nastat některé další aspekty při použití ověření.

Musí být mobilní aplikace s aplikací služby ověřování/se tak mohli ověřovat nakonfigurované cloudové a svému klientovi musí mít koncový bod cloudu zadaný jako host alternativních přihlášení. Najdete v dokumentaci pro svoji platformu klienta pro konkrétní kroky.

Zajistěte, aby měl mobilní back-end [Microsoft.Azure.Mobile.Server.Authentication] nainstalovaný. Potom ve třídě OWIN při spuštění aplikace, přidejte následující akci po `MobileAppConfiguration` byly použity pro vaše `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

V předchozím příkladu byste měli označit nastavení aplikace _authAudience_ a _authIssuer_ Web.config souboru pro každý adresu URL kořene aplikace pomocí schématu HTTPS. Podobně byste měli nastavit _authSigningKey_ je, že hodnota aplikace podpisu klíče. Postup získání podpisového klíče:

1. Přejděte do aplikace v [Azure portálu] 
2. Klikněte na **Nástroje** **Kudu** **Přejít**.
3. Na webu Kudu Správa klikněte na **prostředí**.
4. Nalezení hodnoty pro _webu\_AUTH\_podepisování\_klíč_. 

Pomocí klávesy podpisový parametru _authSigningKey_ v konfiguraci místní aplikace.  Mobilní back-end vybaven nyní ověřit tokeny při spuštění místně, které klient obdrží tokenu z cloudového koncového bodu.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portálu]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

