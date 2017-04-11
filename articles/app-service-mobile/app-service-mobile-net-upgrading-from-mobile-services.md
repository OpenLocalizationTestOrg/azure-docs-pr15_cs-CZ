<properties
    pageTitle="Upgrade z mobilních služeb Azure aplikace služby"
    description="Zjistěte, jak snadno upgradovat aplikaci služby Mobile do aplikace pro mobilní služba aplikace"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Upgrade existující .NET Azure Mobile Service pro aplikaci služby

Mobilní služba aplikace je nový způsob, jak vytvářet mobilní aplikace pomocí Microsoft Azure. Další informace najdete v tématu [jaké mobilní aplikace?].

Toto téma popisuje, jak upgradovat existující aplikaci back-end .NET Mobilní služby Azure nových aplikací Mobilní služba aplikace. Při tomto upgradu existující služby mobilní aplikace můžete dál pracovat.   Pokud je třeba upgradovat aplikaci back-end Node.js, v nápovědě k [upgradu služeb Mobile Node.js](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Při upgradu mobilní back-end aplikace služby Azure má přístup ke všem funkcím aplikace služby a účtovány podle předpisů rozhraní [aplikace služby ceny], ne mobilních služeb ceny.

##<a name="migrate-vs-upgrade"></a>Migrace a upgrade

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Doporučujeme tuto [migrace](app-service-mobile-migrating-from-mobile-services.md) před přechodem prostřednictvím upgrade. Tímto způsobem můžete umístit obě verze aplikace na stejné služby aplikace plánování a vzniknou žádné další náklady.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Vylepšení v mobilních aplikacích .NET server SDK

Upgrade na nové [Mobilní aplikace SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) poskytuje následující výhody:

- Větší flexibilitu o závislostech NuGet. Hostitelské prostředí již obsahuje vlastní verze NuGet balení, abyste mohli používat alternativní kompatibilní verze. Ale pokud nové kritické bugfixes nebo aktualizace zabezpečení, které SDK serveru mobilní telefon nebo závislosti, je nutné aktualizovat služby ručně.

- Větší flexibilitu při mobilní SDK. Explicitně můžete určit, které funkce a trasy nastavená, například ověřování, tabulka rozhraní API a koncový bod nabízených registrace. Další informace najdete v tématu [používání .NET server SDK Azure mobilních aplikací](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Podpora pro jiné typy projektů ASP.NET a trasy. Teď budete moct hostovat řadiče MVC a rozhraní API webových ve stejném projektu jako mobilní back-end projektu.

- Podpora pro nové funkce, ověřování aplikaci služby, které umožňují používat běžné konfigurace ověření přes svůj web a mobilní aplikace.

##<a name="overview"></a>Základní upgradu přehled

V mnoha případech upgradu se skládat například přepnout na novou server mobilní aplikace SDK a publikování kód do nové instance mobilní aplikaci. Existuje, ale některé scénáře, které budou vyžadovat další konfiguraci, třeba scénáře pokročilé ověřování a práci s naplánovaných úloh. Každý z nich jsou obsaženy v dalších částech.

>[AZURE.TIP] Doporučuje čtení a interpretaci zbývající části tohoto tématu úplně před spuštěním upgrade. Poznamenejte si funkce používáte vyznačenými níže.

Klient služby Mobile SDK jsou **není** kompatibilní s novým serverem mobilní aplikace SDK. Poskytování kontinuitu služby aplikace neměli publikování změn webů aktuálně obsluhuje publikované klienty. Místo toho měli vytvořit nové mobilní aplikace, které bude sloužit jako duplicitní. Tato aplikace můžete umístit na stejný plán služeb aplikací účtovány další finanční náklady.

Bude pak mít dvě verze aplikace: které zůstane stejná a slouží aplikace publikovaných v galerii přírody a druhé, které pak můžete upgradovat a cílové s novou verzi klienta. Přesunutí a testování kódu vašeho tempem, ale nezapomeňte, že všechny provedené opravy chyb získat použité pro obě. Jakmile máte pocit, že požadovaný počet klientské aplikace v přírody dokument aktualizují na nejnovější verzi, můžete odstranit původní migrované aplikace vyžadujete-li.

Úplné osnovy proces upgradu vypadá takto:

1. Vytvoření nové aplikace Mobile
2. Aktualizovat projekt použít nové SDK serveru
3. Nová verze vaší klientské aplikace
4. (Volitelné) Odstraňte původní migrované instanci

##<a name="mobile-app-version"></a>Vytvoření druhého instance aplikace
Cílem prvního kroku v upgradu je vytvořit mobilní aplikaci zdroje, které budou hostiteli novou verzi aplikace. Pokud už jste přešli stávající Mobilní služba, bude chcete vytvořit tuto verzi ve stejném hostingu plánu. Otevřete [Azure portál] a přejděte do aplikace migrované. Poznamenejte si plán služeb aplikací běží na.

Dále vytvořte druhý instance aplikace postupujte podle [pokynů vytváření back-end .NET](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Po zobrazení výzvy k vyberte plán služeb aplikací nebo "hostingu plánu" zvolte plán migrované aplikace.

Bude pravděpodobně chcete použít stejné databázi a oznámení centrální stejně jako při mobilních služeb. Zkopírujte tyto hodnoty otevřením [Azure portál] a přechod na původní aplikace a potom klikněte na **Nastavení** > **Nastavení aplikace**. V části **Připojovací řetězec**, zkopírujte `MS_NotificationHubConnectionString` a `MS_TableConnectionString`. Přejděte do nového upgradu webu a vložit do přepsání všech existujících hodnot. Tento postup opakujte pro další nastavení aplikace vašim potřebám aplikace. Pokud nepoužíváte migrované služby, si můžete přečíst připojovací řetězec a nastavení aplikace na kartě **Konfigurovat** oddílu Mobile služby [Azure klasické portálu].

Vytvořit kopii projektu ASP.NET aplikace a potom ji publikujte na nový web. Zkopírováním klientské aplikace aktualizován nová adresa URL, ověřte, že všechno funguje očekávaným způsobem.

## <a name="updating-the-server-project"></a>Aktualizace aplikace project serveru

Mobilní aplikace obsahuje nové [Mobilní aplikace Server SDK] , která obsahuje hodně stejné funkce jako modul runtime služby Mobile. Nejdřív byste měli odebrat všechny odkazy na balíčků mobilních služeb. Ve Správci balíčku NuGet vyhledejte `WindowsAzure.MobileServices.Backend`. Většina aplikace uvidí několik balíčků tady, včetně `WindowsAzure.MobileServices.Backend.Tables` a `WindowsAzure.MobileServices.Backend.Entity`. V takovém případě začínat balíček nejnižší ve stromové struktuře závislost jako `Entity`a odeberte ho. Po zobrazení výzvy neodebírat všechny balíčky závislé. Tento postup opakujte, dokud neodeberete `WindowsAzure.MobileServices.Backend` samotné.

V tomto okamžiku, máte projekt, který už odkazuje SDK služby Mobile.

Dále přidáte odkazy v mobilních aplikacích SDK. Pro tento upgrade Většina vývojářů bude chcete stáhnout a nainstalovat `Microsoft.Azure.Mobile.Server.Quickstart` balíček, jak je to získávat v celé požadovanou sadu.

Bude několik chyby kompilátoru vyplývající z rozdílů mezi SDK, ale jedná se snadno adresy, které jsou zahrnuty ve zbývající části v této části.

### <a name="base-configuration"></a>Základní konfigurace

Pak v WebApiConfig.cs, můžete nahradit:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

s

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Pokud chcete, můžete si další informace o nové .NET server SDK a jak si přidat či odebrat součásti z aplikace, najdete v tématu [jak používat .NET server SDK] .

Pokud je aplikace pomocí funkcí ověřování, bude potřeba zaregistrovat OWIN middleware. V tomto případě je možné přesunout výše konfigurační kód do nového třídy OWIN spuštění.

1. Přidání balíček NuGet `Microsoft.Owin.Host.SystemWeb` Pokud již není součástí projektu.
2. Ve Visual Studiu, klikněte pravým tlačítkem myši na projektu a vyberte **Přidat** -> **Nová položka**. Vyberte **Web** -> **Obecné** -> **OWIN spuštění předmětu**.
3. Přesunutí ve výše uvedeném kódu pro MobileAppConfiguration z `WebApiConfig.Register()` k `Configuration()` metoda třídy nové spuštění.

Přesvědčte se, zda `Configuration()` metoda končí:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Existují další změny souvisejících s ověřováním, které jsou uvedené v části úplný ověřování.

### <a name="working-with-data"></a>Práce s daty

V dialogovém okně mobilní služby název mobilní aplikace sloužila jako výchozí název schématu v nastavení Entity Framework.

Zajistit, že máte na stejné schéma odkazovaný jako předtím, použijte následující nastavení schématu v DbContext aplikace:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Ujistěte se, že máte MS_MobileServiceName nastavit když uděláte výše uvedených možností. Můžete také zadat jiný název schématu pokud dříve aplikace to přizpůsobili.

### <a name="system-properties"></a>Vlastnosti systému

#### <a name="naming"></a>Pojmenování

Do pole server Azure Mobile Services SDK vlastnosti systému vždy obsahovat dvojitého podtržení (`__`) předpona vlastnosti:

- __createdAt
- __updatedAt
- __deleted
- __version

Klient služby Mobile SDK mají zvláštní logiky pro analýzu vlastnosti systému v tomto formátu.

V Azure mobilní aplikace dialogovém okně Vlastnosti systému už máte vlastní formát a mají následující názvy:

- createdAt
- updatedAt
- Odstranit
- verze

Mobilní aplikace klient SDK použít nové názvy vlastnosti systému, žádné změny podporují kód klienta. Ale pokud vytváříte přímo ZBÝVAJÍCÍ volání ke službě pak změňte svoje dotazy příslušným způsobem.

#### <a name="local-store"></a>Místní úložiště přihlašovacích údajů

Změny názvů vlastnosti systému v úmyslu místní databázi offline synchronizace mobilních služeb není kompatibilní s aplikací Mobile. Pokud je to možné neměli byste upgradu klienta, které byly odeslány aplikací z mobilních služeb aplikací Mobile až po neuložené změny na server. Upgradovaná aplikace pak použijte nový název souboru databáze.

Pokud v klientské aplikaci z mobilních služeb upgradovaný na verzi aplikace Mobile i když jsou čekající offline změny ve frontě operace, musí databáze systému nastavit použít nové názvů sloupců. IOS může být dosahuje změna názvů sloupců pomocí lightweight migrace. Android a .NET spravovaných klienta byste měli zapsat vlastní SQL přejmenování sloupce tabulky dat objektu.

IOS je třeba změnit schématu základních dat pro vaše data entit takto. Všimněte si, že vlastnosti `createdAt`, `updatedAt` a `version` již `ms_` předponu:

| Atribut |  Typ   | Poznámka:                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | Řetězec označen jako požadovaný  | primární klíč v vzdálené úložiště         |
| createdAt | Datum    | (volitelné) mapy createdAt systém vlastnost         |
| updatedAt | Datum    | (volitelné) mapy updatedAt systém vlastnost         |
| verze   | Řetězec  | (volitelné) používat ke zjištění konflikty, mapy verzi |

#### <a name="querying-system-properties"></a>Dotazování vlastnosti systému

V dialogovém okně služby Azure Mobile vlastnosti systému neodesílají ve výchozím nastavení, ale pouze v případě, že se požadovaná pomocí řetězce dotazu `__systemProperties`. Naopak v Azure mobilní aplikace systému vlastnosti jsou **vždy zaškrtnuté** protože jsou součástí objektový model SDK serveru.

Tato změna hlavně ovlivňuje vlastní implementace správci domény, například rozšíření `MappedEntityDomainManager`. V dialogovém okně mobilní služby-li klienta požádáni o ukládání nemusíte vůbec žádné vlastnosti systému je možné použít `MappedEntityDomainManager` , který není mapování skutečně všechny vlastnosti. Však v mobilních aplikacích Azure, tyto nenamapovaná vlastnosti dojde k chybě v dotazech NAČÍST.

Nejjednodušší způsob, jak tento problém vyřešit, je upravit svůj DTOs tak, aby dědí od `ITableData` namísto `EntityData`. Potom přidejte `[NotMapped]` atribut pole, která by měla být vynechány.

Například následující definuje `TodoItem` bez vlastností systému:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Poznámka: Pokud se zobrazí chyby `NotMapped`, přidat odkaz na sestavení `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobilní služba obsaženy některé podpora CORS uživatelem obtékání ASP.NET CORS řešení. Tato vrstva obtékání byla odebrána dát vývojář mít větší kontrolu, můžete přímo využít [ASP.NET CORS podpory](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

Hlavní oblasti sledovanou používáte CORS si, že `eTag` a `Location` záhlaví musí mít za účelem pro klienta SDK fungovat správně.

### <a name="push-notifications"></a>Nabízená oznámení
U nabízených je hlavní položky, které můžete narazit na chybějící ze serveru SDK PushRegistrationHandler předmětu. Registrace fungují trochu jinak v mobilních aplikacích a ve výchozím nastavení jsou povoleny tagless registrace. Správa značky může provést pomocí vlastní rozhraní API. Najdete pokyny k [registraci značek](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) Další informace.

### <a name="scheduled-jobs"></a>Naplánovaných úloh
Naplánovaných úloh nejsou integrovaná mobilní aplikace, takže existující projekty, které máte v serverové části .NET bude potřeba upgradovaný jednotlivě. Jednou z možností je k vytvoření plánované [Web projektu] na webu kód mobilní aplikaci. Taky můžete nastavení řadiči, který obsahuje kód projektu a konfigurace [Azure Plánovač] strefíte tohoto koncového bodu na očekávané plán.

### <a name="miscellaneous-changes"></a>Různé změny
Všechny ApiControllers, které bude potřeba k jejich mobilních klientů teď musí mít `[MobileAppController]` atribut. To již standardně další ApiControllers přejdete ovlivněna mobilní formatters.

`ApiServices` Objekt již není součástí SDK. Přístup k nastavení mobilní aplikace, můžete provádět následující:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Podobně protokolování je teď dosáhnout pomocí standardní zápisu sledování ASP.NET:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Co byste měli zvážit ověřování

Součásti ověřování služby Mobile teď byl přesunut do funkce aplikace služby ověřování/se tak mohli ověřovat. Se dozvíte o tuto funkci povolit pro web o na téma [ověření přidat na svůj mobilní aplikace](app-service-mobile-ios-get-started-users.md) .

U některých poskytovatelů, například AAD, Facebook nebo Google je třeba možnost využít stávající registrace z aplikace kopírovat. Stačí přejít na portál zprostředkovatele identit a přidejte nové přesměrování adresy URL k registraci. Potom i konfiguraci aplikace služby ověřování/se tak mohli ověřovat pomocí ID klienta a tajná.

### <a name="controller-action-authorization"></a>Povolení akce řadiče domény
Všechny výskyty textu `[AuthorizeLevel(AuthorizationLevel.User)]` atributu musí být změnilo používat standardní ASP.NET `[Authorize]` atribut. Kromě toho řadiče jsou teď anonymní ve výchozím nastavení jako ostatní aplikace ASP.NET.
Pokud používáte některou z dalších AuthorizeLevel možností, jako je správce nebo aplikace, dejte pozor, toto jsou pryč. Místo toho můžete nastavit AuthorizationFilters pro sdílené tajemství nebo nakonfigurovat jistinu AAD služby umožňující volání do služby zabezpečené.

### <a name="getting-additional-user-information"></a>Získat další informace o uživatelích

Můžete získat další informace o uživatelích, včetně tokeny přístup prostřednictvím `GetAppServiceIdentityAsync()` metodu:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Navíc pokud aplikace, která bude závislostí na uživatelské ID, například ukládání do databáze, je důležité mít na paměti, že ID uživatelů mezi Mobile Services a aplikaci služby mobilní aplikace se liší. ID uživatele služeb Mobile, můžete pořád dostat přes. Všechny dílčí ProviderCredentials mají vlastnost ID uživatele. Z příkladu před pokračováním takto:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Pokud aplikace provést nějaké závislosti ID uživatele, je důležité, pokud je to možné využít stejnou registraci se zprostředkovatelem identit. ID uživatele jsou obvykle s rozsahem registrace aplikace, která byla použita tak úvodní informace o nové registrace bylo možné vytvořit problémy s odpovídajícími uživatelům svým datům.

### <a name="custom-authentication"></a>Vlastní ověřování

Pokud aplikace používá vlastní řešení ověřování, budete chtít, aby upgradovaném webu musí mít přístup k systému. Postupujte podle pokynů v nové vlastní ověřování v [.NET serveru SDK přehled] integrace vašeho řešení. Dejte pozor, aby součásti vlastní ověřování se stále nacházejí v náhledu.

##<a name="updating-clients"></a>Aktualizace klientů
Až budete mít provozní mobilní aplikaci back-end, můžete pracovat s novou verzí klientská aplikace, které využívá ho. Mobilní aplikace obsahuje taky novou verzi klienta SDK a podobná upgradu serveru nad, budete muset odstranit všechny odkazy SDK služby Mobile před instalací aplikací Mobile verze.

Jednou z hlavních změn mezi verzemi je konstruktorů už požadování klávesu application. Teď jednoduše předáte v adrese URL mobilní aplikaci. Například v klientech .NET `MobileServiceClient` konstruktor je nyní:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Další informace o instalaci nového SDK a práce s novou strukturu pomocí těchto odkazů:

- [iOS verze 3.0.0 nebo novější](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verze 2.0.0 nebo novější](app-service-mobile-dotnet-how-to-use-client-library.md)

Pokud vaše aplikace provede používat nabízená oznámení, poznamenejte si konkrétní registraci pokyny pro každou platformu, co byly některé změny stejně.

Když máte novou verzi klienta jste připravení vyzkoušejte proti upgradovaném serveru projektu. Po ověření, že funguje, můžete uvolnit nová verze aplikace pro zákazníky. Nakonec Jakmile se vaši zákazníci mohli dříve je moct přijímat tyto aktualizace, můžete odstranit služby mobilní verzi aplikace. V tomto okamžiku úplně upgradu na aplikaci služby mobilní aplikaci pomocí nejnovější serveru mobilní aplikace SDK.

<!-- URLs. -->

[Azure portálu]: https://portal.azure.com/
[Azure klasický portálu]: https://manage.windowsazure.com/
[Jaké mobilní aplikace?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobilní aplikace serveru SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Plánovač]: /en-us/documentation/services/scheduler/
[Web projektu]: ../app-service-web/websites-webjobs-resources.md
[Jak používat .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Aplikace služby ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Přehled SDK .NET serveru]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
