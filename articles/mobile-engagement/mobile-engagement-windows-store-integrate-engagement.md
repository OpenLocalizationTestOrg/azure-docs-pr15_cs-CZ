<properties 
    pageTitle="Integrace s Windows univerzální aplikace zapojení SDK" 
    description="Jak integrovat univerzální aplikace Windows Azure mobilní zapojení do"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Integrace s Windows univerzální aplikace zapojení SDK

> [AZURE.SELECTOR] 
- [Univerzální Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Tento postup popisuje nejjednodušší způsob, jak aktivovat společnosti zapojení technologie pro analýzu a sledování funkce v aplikaci Windows univerzální.

Tyto kroky jsou tak, abyste aktivovat sestav protokolů potřebné pro výpočet všech statistických údajů týkajících se uživatelů, relací, aktivity, dojde k chybě a Technicals. Sestava protokoly potřebné pro výpočet dalších statistiky jako události, chyby a úlohy musí udělat ručně pomocí rozhraní API zapojení (postup [pomocí rozšířeného Mobile zapojení označování rozhraní API v aplikaci Windows univerzální](mobile-engagement-windows-store-use-engagement-api.md) od tyto statistiky jsou závisí na aplikaci.

## <a name="supported-versions"></a>Podporované verze

Mobilní zapojení SDK pro Windows univerzální aplikace lze integrovat jenom do Windows Runtime a aplikací univerzální platformu Windows už:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (stolní počítače a mobilní rodiny)

> [AZURE.NOTE] Pokud směrujete Silverlight Windows Phone a postupujte podle [postupu integrace Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Instalace mobilních zapojení univerzální aplikace SDK

### <a name="all-platforms"></a>Všechny platformy

Zapojení SDK pro Windows univerzální aplikaci Mobile je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*. Můžete ji nainstalovat ze Visual Studio Nuget balíčku správce.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x a Windows Phone 8.1

NuGet automaticky nasadí SDK zdrojů v `Resources` složky v kořenovém adresáři aplikace project.

### <a name="windows-10-universal-windows-platform-applications"></a>Aplikací univerzální platformu Windows Windows 10

NuGet automaticky nasazení SDK zdrojům v aplikaci UWP ještě. Je potřeba udělat ji ručně, až vrátí nasazení prostředků v NuGet:

1.  Otevřete Průzkumníka souborů.
2.  Přejděte do umístění, následující (**x.x.x** je verze zapojení instalujete): *uživatelského profilu %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Přetažení složky **zdrojů** z Průzkumníka souborů do kořenové projektu ve Visual Studiu.
4.  Ve Visual Studiu vyberte projektu a na ikonu **Zobrazit všechny soubory v** horní části **Průzkumníku řešení**aktivovat.
5.  Některé soubory nejsou součástí projektu. K importu je najednou klikněte pravým tlačítkem na složku **prostředky** **vyloučit z projektu** jiného klikněte pravým tlačítkem myši na složku **prostředky** , **Zahrnout v projektu** znovu zahrnout celou složku a pak. Všechny soubory ve složce **zdroje** jsou teď součástí projektu.

## <a name="add-the-capabilities"></a>Přidání funkcí

Zapojení SDK potřebuje některé funkce sady Windows SDK k fungovat správně.

Otevřete svůj `Package.appxmanifest` souboru a ujistěte se, že jsou deklarované následující možnosti:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Inicializace zapojení SDK

### <a name="engagement-configuration"></a>Zapojení konfigurace

Konfigurace zapojení centralizované v `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor:

-   Vaše aplikace připojovací řetězec mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete určit za běhu, můžete volat metodu před inicializace agent můžete zapojit:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí na portálu klasické Azure.

### <a name="engagement-initialization"></a>Zapojení inicializace

Při vytváření nového projektu `App.xaml.cs` vygeneruje soubor. Tato třída dědí od `Application` a obsahuje mnoho důležité metody. Také použije inicializace Engagement SDK.

Změnit `App.xaml.cs`:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Určení metody sdílení inicializace zapojení jednou pro všechny hovory:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Volání `InitEngagement` v `OnLaunched` metodu:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Pokud je aplikace spuštěna pomocí vlastní schéma, jiné aplikace nebo příkazového řádku pak bude `OnActivated` s názvem metody. Budete potřebovat zahajte SDK zapojení při aktivaci aplikace. Postup přepsat `OnActivated` metodu:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Jsme důrazně zamezilo přidáte inicializační zapojení na jiném místě aplikace.

## <a name="basic-reporting"></a>Základní vytváření sestav

### <a name="recommended-method-overload-your-page-classes"></a>Doporučená metoda: přetížení vaše `Page` třídy

Pro aktivaci sestavy všechny požadované zapojení pro výpočet uživatelů, relací, aktivity, dojde k chybě a technických statistických protokoly, můžete jednoduše vytvoříte všechny vaše `Page` dílčí třídy dědí `EngagementPage` třídy.

Tady je příklad toho, jak to udělat pro stránku aplikace. Umí totéž na všech stránkách aplikace.

#### <a name="c-source-file"></a>C# zdrojového souboru

Úprava stránky `.xaml.cs` souboru:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Nahrazení `Page` s `EngagementPage`:

**Bez můžete zapojit:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**S můžete zapojit:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Pokud stránku přepíše `OnNavigatedTo` metody, je potřeba zavolat `base.OnNavigatedTo(e)`. V opačném nebude možné vykázat aktivity ( `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).

#### <a name="xaml-file"></a>Soubor XAML

Upravte stránku `.xaml` souboru:

-   Přidání názvů deklarací:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Nahrazení `Page` s `engagement:EngagementPage`:

**Bez zapojení:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**S můžete zapojit:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Přepsat výchozí chování

Ve výchozím nastavení je název třídy stránce hlášené jako název aktivity s není extra. Pokud předmětu používá příponu "Stránka", zapojení se také ho odebrat.

Pokud chcete přepsat chování výchozí název, jednoduše přidáte do vašeho kódu:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Pokud chcete zprávu některé další údaje s vaši činnost, můžete přidat do vašeho kódu:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Tyto metody nazývají v rámci `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metody: volání `StartActivity()` ručně

Pokud nemůžete ani nebudete chtít přetížení vaše `Page` třídy, můžete místo toho začít vašich aktivit najdete tak, že zavoláte `EngagementAgent` metody přímo.

Doporučujeme volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránku.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Zajistěte, aby že správně ukončit relaci.
> 
> Windows SDK univerzální automaticky volá `EndActivity` metody, když máte zavřený aplikace. Je tedy **DŮRAZNĚ** doporučujeme volání `StartActivity` metoda kdykoli aktivity uživatele změnit a **nikdy** volání `EndActivity` metody, tato metoda odesílá na server zapojení aktuálního uživatele má opustit aplikaci, bude ovlivní všechny protokoly aplikace.

## <a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Můžete také můžete chtít hlásit aplikace zvláštní události, chyby a úlohy, postup použít jiné metody najdete v `EngagementAgent` předmětu. Rozhraní API zapojení umožňuje používat všechny pokročilých funkcí na Engagement.

Další informace najdete v tématu [použití rozšířeného Mobile zapojení označování rozhraní API v aplikaci Windows univerzální](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Upřesnit

### <a name="disable-automatic-crash-reporting"></a>Zákaz automatické pád vykazování

Můžete zakázat automatické pád vytváření sestav funkce Engagement. Potom, když dojde k neošetřené výjimce, zapojení nic neuděláte.

> [AZURE.WARNING] Pokud chcete tuto funkci zakázat, mějte na paměti, že pokud dojde ke zhroucení neošetřené aplikace, můžete zapojit posílat že pád **a** nezavře úlohy a relace.

Zákaz automatické pád sestav, přizpůsobení konfiguraci v závislosti na způsobu jeho deklarované:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru

Nastavit sestavy pád `false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objekt za běhu

Nastavení sestavy pád NEPRAVDA pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Režim požadavků

Ve výchozím nastavení protokoly sestavy využití služeb v reálném čase. Pokud aplikace sestav protokolů často, je lepší vyrovnávací paměť protokoly a vykazování v celém dokumentu na pravidelných základní časových (říká se "požadavků režimu").

K tomu, zavolejte metodu:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je hodnota v **milisekundách**. Kdykoli Pokud chcete znovu aktivovat v reálném čase protokolování, stačí volejte metodu bez zadání parametru, nebo hodnotu 0.

Režim požadavků mírně zvětšit života battery, ale bude mít vliv na monitoru zapojení: všechny relace a úlohy doba trvání bude zaokrouhleno na požadavků prahové hodnoty (tedy relace a úlohy kratší než mezní hodnota požadavků nemusí být vidět). Doporučujeme používat mezní požadavků než 30000 (30s). Budete muset mějte na paměti uložené protokoly jsou omezené na 300 položek. Pokud odesílání je příliš dlouhá může dojít ke ztrátě některých protokoly.

> [AZURE.WARNING] Mezní hodnota požadavků se nedají konfigurovat období menší než hodnotami 1. Pokud se pokusíte to udělat, SDK zobrazí sledování s chybou a automaticky obnoví výchozí hodnotu, tedy 0s. To bude spouštět SDK vykazování protokoly v reálném čase.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
