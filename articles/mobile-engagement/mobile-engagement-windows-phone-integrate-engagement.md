<properties 
    pageTitle="Windows Phone Silverlight zapojení SDK integrace" 
    description="Jak integrovat Azure mobilní zapojení s aplikacemi jiných Silverlight Windows Phone"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight zapojení SDK integrace

> [AZURE.SELECTOR] 
- [Univerzální systému Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Tento postup popisuje nejjednodušší způsob, jak aktivovat zapojení Mobile Azure technologie pro analýzu a sledování funkce v aplikaci Windows Phone Silverlight.

Tyto kroky jsou tak, abyste aktivovat sestav protokolů potřebné pro výpočet všech statistických údajů týkajících se uživatelů, relací, aktivity, dojde k chybě a Technicals. Sestava protokoly potřebné pro výpočet dalších statistiky jako události, chyby a úlohy musí udělat ručně pomocí rozhraní API zapojení (podívejte se, [jak používat rozšířené Mobile zapojení označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md) níže) od tyto statistiky jsou závislé na aplikace.

##<a name="supported-versions"></a>Podporované verze

Zapojení SDK Mobile pro Windows Silverlight lze integrovat jenom do aplikací:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Pokud jste směrujete Windows Phone 8.1 (bez Silverlight) v nápovědě k [Windows univerzální integrace postup](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Instalace programu Silverlight SDK mobilní zapojení

Zapojení SDK Mobile pro Windows Silverlight je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*. Můžete ji nainstalovat ze Visual Studio Nuget balíčku správce. 

##<a name="add-the-capabilities"></a>Přidání funkcí

Zapojení SDK musí některé funkce SDK Silverlight Windows Phone, abyste mohli pracovat správně.

Otevřete svůj `WMAppManifest.xml` souboru a ujistěte se, že jsou následující možnosti deklarovány v `Capabilities` panely:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Inicializace zapojení SDK

### <a name="engagement-configuration"></a>Zapojení konfigurace

Konfigurace zapojení centralizované v `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor:

-   Vaše aplikace připojovací řetězec mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete určit za běhu, můžete volat metodu před inicializace agent můžete zapojit:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí na portálu Classic Azure.

### <a name="engagement-initialization"></a>Zapojení inicializace

Při vytváření nového projektu `App.xaml.cs` vygeneruje soubor. Tato třída dědí od `Application` a obsahuje mnoho důležité metody. Také použije inicializace Engagement SDK.

Změnit `App.xaml.cs`:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Vložení `EngagementAgent.Instance.Init` v `Application_Launching` metodu:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Vložení `EngagementAgent.Instance.OnActivated` v `Application_Activated` metodu:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Jsme důrazně zamezilo přidáte inicializace zapojení na jiném místě aplikace. Mějte na paměti, která `EngagementAgent.Instance.Init` metoda spustí na vyhrazené vlákna a ne na vlákno uživatelského rozhraní.

##<a name="basic-reporting"></a>Základní vytváření sestav

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Doporučená metoda: přetížení vaše `PhoneApplicationPage` třídy

Pro aktivaci sestavy všechny požadované zapojení pro výpočet uživatelů, relací, aktivity, dojde k chybě a technických statistických protokoly, můžete jednoduše vytvoříte všechny vaše `PhoneApplicationPage` dílčí třídy dědí `EngagementPage` třídy.

Tady je příklad toho, jak to udělat pro stránku aplikace. Umí totéž na všech stránkách aplikace.

#### <a name="c-source-file"></a>C# zdrojového souboru

Upravte stránku `.xaml.cs` souboru:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Nahrazení `PhoneApplicationPage` s `EngagementPage` :

**Bez můžete zapojit:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**S můžete zapojit:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Pokud stránku dědí od `OnNavigatedTo` metody, dejte si pozor, aby `base.OnNavigatedTo(e)` volání. V opačném nebude vykázat aktivity. Nakonec `EngagementPage` volá `StartActivity` uvnitř `OnNavigatedTo` metody.

#### <a name="xaml-file"></a>Soubor XAML

Upravte stránku `.xaml` souboru:

-   Přidání názvů deklarací:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Nahrazení `phone:PhoneApplicationPage` s `engagement:EngagementPage` :

**Bez můžete zapojit:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**S můžete zapojit:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Změnit výchozí chování

Ve výchozím nastavení je název třídy stránky hlášené jako název aktivity s není extra. Pokud předmětu používá příponu "Stránka", zapojení se také ho odebrat.

Pokud chcete změnit výchozí chování na název, jednoduše přidáte do vašeho kódu:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Pokud chcete zprávu některé dodatečné informace s vaši činnost, můžete přidat do vašeho kódu:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Tyto metody nazývají v rámci `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metody: volání `StartActivity()` ručně

Pokud nemůžete ani nebudete chtít přetížení vaše `PhoneApplicationPage` třídy, můžete místo toho začít vašich aktivit najdete tak, že zavoláte `EngagementAgent` metody přímo.

Doporučujeme, volající `StartActivity` uvnitř vaší `OnNavigatedTo` metoda vaší PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Zajistěte, aby že správně ukončit relaci.
>
> V SDK automaticky volá `EndActivity` metody, když máte zavřený aplikace. Je tedy **DŮRAZNĚ** doporučujeme volání `StartActivity` metoda kdykoli aktivity uživatele změnit a **nikdy** volání `EndActivity` metody. Tento způsob odešle zprávu serveru můžete zapojit aktuálního uživatele opustil aplikace a to má vliv všechny protokoly aplikace.

##<a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Můžete také můžete chtít sestavy aplikace zvláštní události, chyby a úlohy, postup použít jiné metody najdete v `EngagementAgent` předmětu. Rozhraní API zapojení umožňuje používat všechny pokročilých funkcí na Engagement.

Další informace najdete v tématu [použití rozšířeného Mobile zapojení označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Upřesnit

### <a name="disable-automatic-crash-reporting"></a>Zákaz automatické pád vykazování

Můžete zakázat automatické pád vytváření sestav funkce Engagement. Potom, když dojde k neošetřené výjimce, zapojení nic neuděláte.

> [AZURE.WARNING] Pokud chcete tuto funkci zakázat, mějte na paměti, že pokud dojde ke zhroucení neošetřené aplikace, můžete zapojit neodešle pád **a** nezavře úlohy a relace.

Vypnout automatické pád sestav, přizpůsobení konfiguraci podle toho, tak, jak ho deklarované:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru

Nastavit sestavy pád `false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objekt za běhu

Nastavení sestavy pád NEPRAVDA pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Režim požadavků

Ve výchozím nastavení protokoly sestavy využití služeb v reálném čase. Pokud aplikace sestav protokolů často, je lepší vyrovnávací paměť protokoly a vykázat jejich v celém dokumentu pravidelných časových základní (říká se "požadavků režimu").

K tomu, zavolejte metodu:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je hodnota v **milisekundách**. Kdykoli Pokud chcete znovu aktivovat v reálném čase protokolování, stačí volejte metodu bez zadání parametru, nebo hodnotu 0.

Režim požadavků mírně zvětšit životnost battery ale má vliv na monitoru můžete zapojit: všechny relace a úlohy doba trvání bude zaokrouhleno na požadavků prahové hodnoty (tedy relace a úlohy kratší než mezní hodnota požadavků nemusí být viditelné). Doporučujeme používat prahové požadavků než 30000 (30s). Budete muset mějte na paměti uložené protokoly jsou omezené na 300 položek. Pokud odesílání je příliš dlouhá může dojít ke ztrátě některých protokoly.

> [AZURE.WARNING] Mezní hodnota požadavků se nedají konfigurovat období menší než jedna sekunda. Pokud se pokusíte tak SDK zobrazí sledování s chyby a bude automaticky obnovte na výchozí hodnotu, tedy nula sekund. To bude spouštět SDK vykazování protokoly v reálném čase.
 
