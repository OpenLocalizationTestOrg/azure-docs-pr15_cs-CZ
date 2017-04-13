<properties 
    pageTitle="Univerzální aplikace Windows dosáhla integrace SDK" 
    description="Jak integrovat univerzální aplikace Windows Azure mobilní zapojení Reach"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Univerzální aplikace Windows dosáhla integrace SDK

Postup popsaná v [Systému Windows univerzální zapojení SDK integrace](mobile-engagement-windows-store-integrate-engagement.md) integrace před provedením této příručce.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Vložení SDK dosáhla zapojení do Windows univerzální projektu

Nemusíte nic přidat. `EngagementReach`odkazy a materiály již máte v projektu.

> [AZURE.TIP] Je možné upravit obrázky umístěné v `Resources` složky projektu, zejména ikonu značky (výchozí nastavení na ikonu zapojení). Na univerzální aplikace můžete přesunout také `Resources` složku na sdíleném projektu a jeho obsah mezi aplikací, ale můžete sdílet muset mít `Resources\EngagementConfiguration.xml` souboru na výchozí umístění beze platformy závislá.

## <a name="enable-the-windows-notification-service"></a>Povolení oznámení služby systému Windows

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x a Windows Phone 8.1 pouze

K použití **Služby Windows oznámení** (označované jako WNS) ve vaší `Package.appxmanifest` souborů v `Application UI` klikněte na `All Image Assets` do pole vlevo bot. V pravé části pole v `Notifications`, změnit `toast capable` z `(not set)` k `(Yes)`.

### <a name="all-platforms"></a>Všechny platformy

Potřebujete synchronizovat aplikace ke svému účtu Microsoft a platformu engagement. Pro tento budete muset vytvořit účet nebo se přihlaste [Centrum pro vývojáře windows](https://dev.windows.com). Až to vytvořit novou aplikaci a vyhledání ID zabezpečení a tajné klíče. Na frontend zapojení přejděte na nastavení aplikace v `native push` a vložte svoje přihlašovací údaje. Až to, klikněte pravým tlačítkem myši na projektu, vyberte `store` a `Associate App with the Store...`. Potřebujete vyberte aplikaci mít vytvoříte před jeho synchronizací.

## <a name="initialize-the-engagement-reach-sdk"></a>Inicializace Reach zapojení SDK

Změnit `App.xaml.cs`:

-   Vložení `EngagementReach.Instance.Init` hned za `EngagementAgent.Instance.Init` ve vaší `InitEngagement` metodu:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    `EngagementReach.Instance.Init` Běží ve vyhrazené vlákna. Nemusíte dělat sami.

> [AZURE.NOTE] Pokud používáte nabízená oznámení jinde v aplikaci budete muset [sdílet nabízených kanál](#push-channel-sharing) s dosáhla Engagement.

## <a name="integration"></a>Integrace

Zapojení dvěma způsoby přidání Reach v aplikaci plakáty a vkládaná zobrazení pro oznámení a hlasování v aplikaci: integrace překryvném a ruční integrace zobrazení webu. Neměli kombinovat obou přístup na stejné stránce.

Volba mezi dvěma integrace může shrnout tímto způsobem:

-   Pokud stránek už dědí od agenta můžete integrace překryvném `EngagementPage`, záleží jen jednoduše nahradit `EngagementPage` tak, že `EngagementPageOverlay` a `xmlns:engagement="using:Microsoft.Azure.Engagement"` tak, že `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stránkách.
-   Můžete rozhodnout zobrazení web ruční integrace Pokud chcete přesně umístit uživatelského rozhraní dosáhla v rámci stránky nebo pokud nechcete, aby chcete přidat další úroveň dědičnosti na stránky. 

### <a name="overlay-integration"></a>Integrace překrytí

Překryvné zobrazení zapojení dynamicky přidá prvky uživatelského rozhraní zobrazí Reach kampaní na stránce. Pokud překrytí nemá odpovídala rozložení měli byste zvážit zobrazení web ruční integrace místo.

V souboru změny .xaml `EngagementPage` odkaz na`EngagementPageOverlay`

-   Přidání názvů deklarací:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Nahrazení `engagement:EngagementPage` s `engagement:EngagementPageOverlay`:

**S EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**S EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

V souboru cs označení stránku v `EngagementPageOverlay` namísto `EngagementPage` a import `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Nahrazení `EngagementPage` s `EngagementPageOverlay`:

**S EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**S EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Přidá překryvném zapojení `Grid` prvek nad stránku skládající se ze rozložení a dva `WebView` prvků jeden nápisu a druhá vkládaná zobrazení.

Je možné upravit přímo v překryvném prvky `EngagementPageOverlay.cs` soubor.

### <a name="web-views-manual-integration"></a>Ruční integrace zobrazení webu

Reach bude prohledávat stránky pro dva `WebView` prvky odpovědná za zobrazování nápisu a vkládaná zobrazení. Jediné je potřeba udělat, je přidání těchto dvou `WebView` prvky někde na stránkách, tady je příklad:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


V tomto příkladu `WebView` prvky jsou roztažení jejich kontejner, který automaticky znovu s velikostí je v případě změny obrazovky otočení v prostoru nebo v okně velikost.

> [AZURE.WARNING] Je nutné zachovat stejnou naming `engagement_notification_content` a `engagement_announcement_content` pro `WebView` prvky. Reach je zjišťuje podle jejich jména. 

## <a name="handle-datapush-optional"></a>Úchyt datapush (volitelné)

Pokud chcete aplikaci moct přijímat posune Reach dat, máte implementace dvou událostí třídy EngagementReach:

V App.xaml.cs v konstruktoru App() přidáte:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Uvidíte, že zpětné obou metod vrátí logickou hodnotu. Zapojení rozešle názoru jeho back-end po odeslání nabízených data. Zpětné chybovou hodnotu NEPRAVDA, `exit` názory budou odeslat. V ostatních případech bude `action`. Pokud není zpětné je nastavena pro události, `drop` názory budou vráceny Engagement.

> [AZURE.WARNING] Zapojení není nedostáváte násobky názory pro data push. Pokud budete chtít nastavit několik obslužné rutiny události, mějte na paměti, že bude zpětnou vazbu k poslední odpovídat jeden odeslat. V tomto případě doporučujeme vždy vrátí stejnou hodnotu-li se vyhnout matoucí názory na front-end.

## <a name="customize-ui-optional"></a>Přizpůsobení uživatelského rozhraní (volitelné)

### <a name="first-step"></a>První krok

Jsme umožňuje přizpůsobit reach uživatelského rozhraní.

Postup, musíte vytvořit podtřídu `EngagementReachHandler` předmětu.

**Ukázkový kód:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Nastavte obsah `EngagementReach.Instance.Handler` pole s vlastní objektu ve vaší `App.xaml.cs` třídy v rámci `App()` metody.

**Ukázkový kód:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Ve výchozím nastavení používá zapojení vlastní provádění `EngagementReachHandler`.
> Nemusíte vytvářet vlastní a pokud uděláte, nemusíte přepsání všech metody. Výchozí chování je základní objekt Engagement.

### <a name="web-view"></a>Zobrazení webové stránky

Ve výchozím nastavení použije Reach vložené prostředky DLL zobrazíte oznámení a stránek.

Poskytnout úplné vlastní nastavení možností používáme pouze zobrazení webové stránky. Pokud chcete přizpůsobit rozložení, přepsat přímo souborů prostředků `EngagementAnnouncement.html` a `EngagementNotification.html`. Zapojení potřebuje všechny doručení s kódem v `<body></body>` správně spustit. Ale můžete přidat značku vnější `engagement_webview_area`.

Můžete však rozhodnete používat vlastní zdroje.

Je možné přepsat `EngagementReachHandler` metody v podtřídě zjistit zapojení používat rozložení, ale dávejte na vložené mechanismus můžete zapojit:

**Ukázkový kód:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Ve výchozím nastavení je AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Představuje soubor html, který je Navrhněte obsah zprávy nabízených (Text oznámení, Web anoucement a hlasování oznámení). Je AnnouncementName `engagement_announcement_content`. Je název návrhu webové zobrazení na stránce xaml.

Je NotfificationHTML `ms-appx-web:///Resources/EngagementNotification.html`. Představuje soubor html, který návrh oznámení o nabízených zprávy. Je NotfificationName `engagement_notification_content`. Je název návrhu webové zobrazení na stránce xaml.

### <a name="customization"></a>Vlastní nastavení

Můžete přizpůsobit oznámení a oznámení zobrazení webového obsahuje se má-li zachovat zapojení objektu. Prohlédněte péče této webové zobrazení objektu je popsán třikrát - poprvé ve vaší xaml, podruhé v souboru cs v metodu "setwebview()" a třetí čas v souboru ve formátu html.

-   Ve vaší xaml popisují aktuální součásti webové zobrazení grafického rozložení.
-   V souboru CS můžete definovat "setwebview()" nastavíte dimenzi dva webové zobrazení (oznámení, oznámení). Je velmi efektivní při změně velikosti aplikace.
-   V souboru html můžete zapojit popisu webové zobrazení obsahu, návrh a umístění prvků mezi sebou.

### <a name="launch-message"></a>Spuštění zprávy

Poté, co uživatel klikne na informační systému (oznámením), můžete zapojit otevře aplikace, načíst obsah zprávy nabízená a zobrazte stránku pro odpovídající kampaní.

Existuje zpoždění mezi spuštění aplikace a zobrazení stránky (v závislosti na rychlosti sítě).

Chcete-li uživateli načítání něco, by měl obsahovat vizuální informace, jako pruh průběh nebo indikátor průběhu. Zapojení nemůže zpracovat, ale obsahuje několik popisovače za vás.

Provádět zpětné App.xaml.cs v "Veřejné App() {}" přidejte:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Zpětné můžete nastavit v metodu "Veřejné App() {}" váš `App.xaml.cs` souboru nejlépe před `EngagementReach.Instance.Init()` volání.

> [AZURE.TIP] Každá obslužná rutina nazývá tak, že podprocesu uživatelského rozhraní. Nemáte starosti při použití MessageBox nebo něco souvisejících s uživatelského rozhraní.

##<a id="push-channel-sharing"></a>Nabízená sdílení kanálu

Pokud používáte nabízených oznámení pro jiné účely v aplikaci je nutné použít nabízená kanálu funkci SDK zapojení sdílení. Toto je chcete-li předejít zmeškané připínáčku.

- Můžete zadat vlastní nabízených kanálu, který chcete inicializace dosáhla Engagement. V SDK použije místo žádosti o novou.

Aktualizace inicializace dosáhla zapojení u svého kanálu nabízená v `InitEngagement` podle `App.xaml.cs` souboru:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Můžete taky Pokud chcete používat nabízená kanálu po inicializace Reach pak můžete nastavit zpětné zapojení dosáhla získat kanálu nabízených jednou je vytvořen tak, že v SDK.

Nastavte svůj zpětné na libovolné místo **Po** inicializační Reach:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Tip schéma

Připravili jsme pro použití vlastní schéma. Jiný typ URI můžete odeslat z frontend zapojení se nemusí používat v aplikaci engagement. Výchozí schéma jako `http, ftp, ...` jsou ovládat Windows, bude okno výzva Pokud jsou žádné výchozí aplikace na zařízení nainstalovaný. Můžete taky vytvořit vlastní schéma aplikace.

Jednoduchý způsob, jak nastavit vlastní schéma aplikace je otevřete svůj `Package.appxmanifest` přejděte v `Declarations` panelu. Vyberte `Protocol` v dostupné deklarace posun pole a přiřadit ji. Upravit `Name` pole s nový protokol požadovaný název.

Teď můžete tento protokol upravovat vaše `App.xaml.cs` s `OnActivated` metody a nezapomeňte také inicializace zapojení tady:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
