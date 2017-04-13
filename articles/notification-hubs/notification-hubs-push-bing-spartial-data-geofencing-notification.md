<properties
    pageTitle="Samostatně GEO nabízená oznámení s Azure oznámení rozbočovače a prostorových dat Bing | Microsoft Azure"
    description="V tomto kurzu se naučíte předvádění na základě umístění nabízená oznámení s Azure oznámení rozbočovače a prostorových dat Bing."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="nabízená oznámení, nabízená oznámení"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Samostatně GEO nabízená oznámení s Azure oznámení rozbočovače a Bing prostorových dat
 
 > [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

V tomto kurzu se naučíte předvádění na základě umístění nabízená oznámení s Azure oznámení rozbočovače a prostorových dat Bing využít z aplikace univerzální platformu Windows už.

##<a name="prerequisites"></a>Zjistit předpoklady pro
Především budete muset zkontrolujte, jestli všechny software a služby předpoklady:

* [Aktualizace 1 pro Visual Studio 2015](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) nebo novější ([Komunity Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) udělá taky). 
* Nejnovější verzi [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Centrum pro vývojáře mapy Bing účtu](https://www.bingmapsportal.com/) (můžete ho zdarma vytvořit a přidružit k účtu Microsoft). 

##<a name="getting-started"></a>Začínáme

Začneme tím, že vytvořením projektu. Ve Visual Studiu zahájení nového projektu typu **Prázdné App (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Po dokončení vytvářeného projektu, měli byste mít svazku samotnou aplikaci. Teď Pojďme nastavení všechno, co pro infrastrukturu geo oddělení. Protože jsme chodí použití služby Bing pro to, je veřejné koncového bodu rozhraní REST API, který umožňuje nám zjistit konkrétní umístění snímků:

    http://spatial.virtualearth.net/REST/v1/data/
    
Je třeba určit následující parametry správně fungoval:

* **ID zdroje dat** a **Název zdroje dat** – v rozhraní API aplikace mapy Bing, zdroje dat obsahovat různé bucketed metadat, třeba umístění a pracovní dobu operace. Další informace o těchto tady. 
* **Název entity** – entitu, kterou chcete použít jako odkaz bod pro oznámení. 
* **Klíč rozhraní API aplikace mapy Bing** – jedná se o klíč, který jste dříve získali při vytvoření účtu Centrum pro vývojáře služby Bing.
 
Pojďme se důkladné postupy při nastavení pro každou z výše uvedených prvky.

##<a name="setting-up-the-data-source"></a>Nastavení zdroje dat

Můžete to udělat v Centru pro vývojáře mapy Bing. Stačí kliknout na **zdroje dat** v horním navigačním panelu a vyberte **Spravovat zdroje dat**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Pokud jste s nepracovali rozhraní API aplikace mapy Bing před, pravděpodobně nebude možné všechny zdroje dat prezentovat, tak vytvoříte na novou kliknutím na nahrát dat ke zdroji dat. Zkontrolujte, jestli že vyplňte všechna povinná pole:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Se může být váhající – co je datový soubor a co by je třeba nahrávání? Pro účely tento test můžete jednoduše používáme ukázku založené na kanál, rámců určitá oblast waterfront Hradec Králové:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Výše uvedené představuje tuto entitu:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Jednoduše zkopírujte a vložte řetězec nad do nového souboru ho uložit jako **NotificationHubsGeofence.pipe**a nahrát v Centru pro vývojáře služby Bing.

>[AZURE.NOTE]Může být vyzváni k zadání nový klíč pro **hlavní klíč** , který se liší od **Klíč dotazu**. Stačí vytvořit nový klíč prostřednictvím řídicího panelu a aktualizace stránce nahrát zdroje dat.

Až nahrajete datového souboru, bude potřebujete udělat publikujte zdroje dat. 

Přejděte na **Spravovat zdroje dat**, stejně jako jsme provedli nad, najít zdroj dat v seznamu a klikněte na **Publikovat** ve sloupci **Akce** . Kromě trochu, měli byste vidět zdroji dat na kartě **Publikovaných zdrojů dat** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Pokud kliknete na tlačítko **Upravit**budete moct zobrazit na první pohled umístění, ve kterých jsme zavedený:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

V tomto okamžiku portálu nezobrazuje se hranice geofence jsme vytvořili – potřebujeme stačí potvrzení, že je umístěný zadané v blízkosti pravého.

Nyní máte k dispozici všechny požadavky na zdroj dat. Zobrazíte podrobnosti o požadavku na adresu URL pro volání rozhraní API v Centru pro vývojáře mapy Bing, klikněte na **zdroje dat** a vyberte **Informací o zdroji dat**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**Adresa URL dotazu** je jsme po tady. Toto je koncový bod proti němuž jsme spouštění dotazů a zkontrolovat, zda zařízení je aktuálně uvnitř hranic umístění nebo ne. Abyste mohli provést kontrola, jednoduše potřebujeme provést hovoru GET proti dotazu adresy URL, pomocí následujících parametrů přidaným:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Tímto způsobem zadáváte cíl ukazatel, které získáváme ze zařízení a mapy Bing bude automaticky provádět výpočty a zjistěte, jestli je v geofence. Po provedení požadavku prostřednictvím prohlížeče (nebo otočení) zobrazí standardní odpověď JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Tato odpověď se stane, pouze po místo ve skutečnosti v rámci určené omezení. Pokud není, zobrazí se vám bloku prázdné **výsledky** :

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Nastavení aplikace UWP

Teď, jsme připravení zdroje dat, můžeme začít pracovat UWP aplikaci, která jsme zavedeny dříve.

Především jsme musí povolit umístění služby pro naše aplikace. K tomuto účelu poklikejte na `Package.appxmanifest` soubor v **Průzkumníku řešení**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Na kartě Vlastnosti balíčku, který právě otevřeli klikněte na **Možnosti** a ujistěte se, že jste vybrali **umístění**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Deklarovaných umístění možnost vytvořit novou složku ve vašem řešení s názvem `Core`a přidejte nový soubor v něm s názvem `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

`LocationHelper` Samotné třídě je poměrně základní v tomto okamžiku – dělá stačí umožňují získat umístění uživatele pomocí rozhraní API systému:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Další informace o získání umístění uživatele v aplikacích UWP v [dokumentu MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Zjistit, že WIA umístění skutečně funguje otevřete straně kód z hlavní stránky (`MainPage.xaml.cs`). Vytvoření nové rutiny události `Loaded` události v aplikaci `MainPage` konstruktor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Obslužná rutina události je takto:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Všimněte si, že jsme obslužné rutiny deklarované jako asynchronní protože `GetCurrentLocation` je awaitable a proto vyžaduje, který bude proveden v kontextu asynchronní. Také protože za určitých okolností jsme pro vás nakonec znamenat null umístění (například umístění, do kterého jsou zakázány služby nebo aplikace byl odepřen oprávnění k přístupu umístění), potřebujeme abyste měli jistotu, že je správně zpracována null zaškrtnutý.

Spusťte aplikaci. Zkontrolujte, jestli že povolíte přístup umístění:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Jednou spustí aplikace se mají být k dispozici souřadnice v okně **výstupu** :

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Teď víte, že funguje pořízení umístění – neváhejte odebrat obslužné rutiny události test pro načítání, protože jsme nebudete používat ho už.

Dalším krokem je k zaznamenání umístění změny. K těmto přejděte zpět do `LocationHelper` třídy a přidání obslužné rutiny události pro `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Provedení zobrazí v okně **výstupu** souřadnice umístění:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Nastavení back-end

Stáhněte si [.NET back-end výběru GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Po dokončení stahování otevřete `NotifyUsers` složky, respektive – `NotifyUsers.sln` soubor.

Nastavit `AppBackend` project jako **Projekt při spuštění** a spustit.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekt už má nakonfigurovanou odešlete cílová zařízení nabízená oznámení, tak jsme muset udělat jenom dvě věci – zaměnit správné připojení řetězce pro centrální oznámení a přidat okraj identifikace k odeslání oznámení pouze v případě, že uživatel je v geofence.

Při konfiguraci připojovací řetězec v `Models` složku otevřít `Notifications.cs`. `NotificationHubClient.CreateClientFromConnectionString` Funkce musí obsahovat informace o rozbočovače oznámení, která může vstoupit [Azure portál](https://portal.azure.com) (vzhled uvnitř zásuvné **Zásady přístupu** v **dialogovém okně Nastavení**). Uložte aktualizovaný konfigurační soubor.

Teď potřebujeme vytvoření modelu pro výsledek rozhraní API aplikace mapy Bing. Nejjednodušší způsob, jak to udělat je klikněte pravým tlačítkem myši na `Models` složky, **Přidat** > **předmětu**. Pojmenujte ho `GeofenceBoundary.cs`. Po dokončení kopírovat ve formátu JSON rozhraní API odpověď, jsme popisované v části první a ve Visual Studiu použití **Upravit** > **Vložit jinak** > **JSON vložit jako třídy**. 

Tento způsob, jak můžeme zajistit, že objekt bude rekontrukci přesně tak, jak byla určena. Sady výsledné třídy by měl vypadat takto:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Potom otevřete `Controllers`  >  `NotificationsController.cs`. Potřebujeme doladit volání příspěvek účtu v cílové délky a šířky. K těmto jednoduše přidejte dva řetězce funkce podpis – `latitude` a `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Vytvoření nové třídy v rámci projektu s názvem `ApiHelper.cs` – jsme použijete k připojení do služby Bing ke kontrole přejděte hranice křižovatky. Implementace `IsPointWithinBounds` funkce, třeba takto:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Ujistěte se, že nahrazovat koncový bod rozhraní API s dotazu URL, kterou jste získali dříve z Centrum pro vývojáře služby Bing (totéž klávesu rozhraní API). 

Pokud jsou výsledky dotazu, která znamená, že na určité místo uvnitř hranic geofence, tak vrácených `true`. Pokud nejsou žádné výsledky, Bing je máme informace o tom, že bodu je mimo rámeček vyhledávání tak, aby vrácených `false`.

Po návratu do `NotificationsController.cs`, vytvářet přímo před příkaz switch kontrola:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Tímto způsobem oznámení pouze odeslaný po místo uvnitř hranic.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Testování nabízená oznámení v aplikaci UWP

Přechod zpět do aplikace UWP, jsme teď měli být schopní otestovat oznámení. V `LocationHelper` třídy, vytvořte novou funkci – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Záměna `POST_URL` umístění nasazeném webové aplikace, kterou jsme vytvořili v předchozí části. Nyní můžete spustit místně, ale když pracujete na nasazení veřejné verze, budete muset hostovat u poskytovatele externí.

Pojďme zkontrolovat jsme zaregistrovat aplikaci UWP nabízených oznámení. Ve Visual Studiu, klikněte na **projekt** > **ukládání** > **přidružení aplikace se obchodu**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Jakmile se přihlásit ke svému účtu vývojář, ujistěte se, vyberte existující aplikace nebo vytvořte nový účet a přidružit balíček. 

Přejděte na Centrum pro vývojáře a otevřete aplikaci, kterou jste právě vytvořili. Klikněte na **služby** > **Nabízená oznámení** > **služby Live webu**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Na webu všimnete si **Aplikaci tajná** a **ID balíčku zabezpečení**. Budete potřebovat jak na portálu Azure – otevřete rozbočovače oznámení, klikněte na možnost **Nastavení** > **Oznámení služby** > **Systému Windows (WNS)** a zadejte informace do požadovaná pole.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klikněte na **Uložit**.

Klikněte pravým tlačítkem na **odkazy** v **Průzkumníku řešení** a vyberte **Spravovat balíčků NuGet**. Bude potřeba přidat odkaz na **Microsoft Azure služby Bus Řízená knihovna** – jednoduše vyhledat `WindowsAzure.Messaging.Managed` a přiřadit ji někomu projektu.

![](./media/notification-hubs-geofence/vs-nuget.png)

Pro účely testování můžeme vytvořit `MainPage_Loaded` obslužná rutina události ještě jednou a přidejte do něj tento fragment kódu:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Výše uvedené registruje aplikace centrální oznámení. Jste připraveni přejít! 

V `LocationHelper`, vnitřní `Geolocator_PositionChanged` rutinu, můžete přidat část testovacího kódu, který bude vynutí umístění na místo v geofence:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Protože jsme nejsou předávání skutečné souřadnice (které nemusí být uvnitř hranic v okamžiku, kdy) a pomocí předdefinovaných zkušební hodnoty, vidíme oznámení zobrazují na aktualizace:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Co je další krok?

Existuje několik kroků, které možná budete muset postup kromě výše uvedené, abyste měli jistotu, že je řešení připravené na výroby.

Především můžete zkusit zajistit geofences dynamických. Abyste mohli moct nahrávat nové hranice v rámci existujícího zdroje dat to bude vyžadovat některé další práce pomocí rozhraní API Bing. Vyhledejte [Bing prostorové rozhraní API služeb dat si přečtěte následující dokumentaci](https://msdn.microsoft.com/library/ff701734.aspx) podrobné informace o předmět.

Za druhé při práci na zajistit, aby doručení správné účastníkům, můžete jej směrovat prostřednictvím [označování](notification-hubs-tags-segment-push-message.md).

Řešení uveden nad popisuje scénáře, ve které bude pravděpodobně širokou škálu cílové platformy, proto jsme není omezena geofencing k možnostem specifické pro systém. Který říká, univerzální platformu Windows už nabízí možnosti zjišťování [geofences správné mimo předdefinovaných](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Podrobné informace týkající se funkcí rozbočovače oznámení podívejte se na náš [portálem si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/notification-hubs/).
