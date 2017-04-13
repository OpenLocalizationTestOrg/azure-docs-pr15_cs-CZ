<properties 
    pageTitle="Windows Phone Silverlight Reach SDK integrace" 
    description="Jak integrovat Azure mobilní zapojení Reach s aplikacemi jiných Silverlight Windows Phone"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK integrace

Můžete třeba postupuje integrace ve [Windows Phone Silverlight zapojení SDK integrace](mobile-engagement-windows-phone-integrate-engagement.md) před provedením této příručce.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Vložení SDK dosáhla zapojení do projektu Silverlight Windows Phone

Nemusíte nic přidat. `EngagementReach`odkazy a materiály již máte v projektu.

> [AZURE.TIP]  Je možné upravit obrázky umístěné v `Resources` složky projektu, zejména ikonu značky (výchozí nastavení na ikonu zapojení).

##<a name="add-the-capabilities"></a>Přidání funkcí

Zapojení dosáhla SDK musí některé další možnosti.

Otevřete svůj `WMAppManifest.xml` souboru a ujistěte se, že jsou deklarovány následující možnosti:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

První z nich je umožňují službou MPNS zobrazení oznámením. Druhý slouží k vložení prohlížeče úkolu do SDK.

Upravit `WMAppManifest.xml` soubor a přidat do `<Capabilities />` značku:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Povolení služby Microsoft nabízených oznámení

K použití **Služby nabízená oznámení** (označované jako MPNS) vaší `WMAppManifest.xml` soubor musí mít `<App />` označení s `Publisher` atribut nastavte název projektu.

##<a name="initialize-the-engagement-reach-sdk"></a>Inicializace Reach zapojení SDK

### <a name="engagement-configuration"></a>Zapojení konfigurace

Konfigurace zapojení centralizované v `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor reach konfigurace:

-   *Volitelné*, zda aktivaci nativní nabízených (MPNS) nebo ne mezi `<enableNativePush>` a `</enableNativePush>` značky (`true` ve výchozím nastavení).
-   *Volitelné*, uveďte název kanálu nabízených mezi `<channelName>` a `</channelName>` značky, poskytnutí stejně, že aplikace může aktuálně používat nebo nechejte prázdné.

Pokud chcete určit za běhu, můžete volat metodu před inicializační agent můžete zapojit:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Můžete zadat název kanálu nabízených MPNS aplikace. Ve výchozím nastavení vytvoří zapojení název podle ID aplikace. Máte třeba zadat název sami, s výjimkou případů, pokud chcete používat nabízená kanálu mimo Engagement.

### <a name="engagement-initialization"></a>Zapojení inicializace

Změnit `App.xaml.cs`:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Vložení `EngagementReach.Instance.Init` hned za `EngagementAgent.Instance.Init` v `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Vložení `EngagementReach.Instance.OnActivated` v `Application_Activated` metodu:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] `EngagementReach.Instance.Init` Běží ve vyhrazené vlákna. Nemusíte dělat sami.

##<a name="app-store-submission-considerations"></a>App store odeslání co byste měli zvážit

Microsoft ukládá některá pravidla při použití nabízená oznámení:

Z Microsoft [Zásady použití] dokumentace oddílu 2.9:

1) Musíte požádejte uživatele, aby potvrďte příjem nabízených oznámení. V nastaveních, přidejte způsob, jak vypnout nabízená oznámení.

Objekt EngagementReach dvěma způsoby správy vyjádření výslovného nebo vyjádření výslovného rezervace, `EnableNativePush()` a `DisableNativePush()`. Můžete, například vytvořit jednu z možností v části nastavení s přepínač zakázání nebo povolení MPNS.

Můžete taky potřeba rozhodnout deaktivovat MPNS prostřednictvím konfigurace zapojení\<konfigurace systému windows – telefonu – sdk-reach\>.

> 2.9.1) aplikace musíte nejdřív popisují oznámení poskytované a **získat výslovného povolení uživatele (přihlášení)**a **třeba zadat mechanismus přes který uživatel může vyjádření výslovného nesouhlasu příjem nabízených oznámení**. Všechna oznámení, pokud tuto službu Microsoft nabízená oznámení se musí shodovat s popisem, k dispozici pro uživatele a musí splňovat všech platných [Zásady použití]  [ Content Policies] a [Další požadavky pro určité typy aplikací].

2) Nepoužívejte příliš mnoho nabízená oznámení. Zapojení zpracuje oznámení za vás.

> 2.9.2) aplikace a její používání služba nabízených oznámení společnosti Microsoft nesmí příliš pomocí síťové kapacity nebo šířku pásma služba nabízených oznámení společnosti Microsoft nebo jinak neoprávněně ovlivnit Windows Phone nebo jiné zařízení Microsoft nebo službu s nadbytečné nabízená oznámení, stanovené společností Microsoft v jeho rozumné rozhodnutí a musí poškodit nebo rušit žádné Microsoft sítě serverů nebo libovolnou třetích stran nebo sítí připojení ke službě Microsoft nabízená oznámení.

3) Nejsou založeny na MPNS a pošlete criticals informace. Zapojení využívá MPNS, a proto tohoto pravidla platí pro kampaně vytvořili do zaměstnání front-end.

> 2.9.3) nemusí být nabízená oznámení služby k odeslání oznámení, které jsou velice důležité nebo jinak může ovlivnit věcech životnost nebo úmrtí použili, zejména omezení důležitých oznámení týkající se zdravotnickou zařízení nebo podmínky. SPOLEČNOST MICROSOFT NEPOSKYTUJE VÝSLOVNĚ ZÁRUK POUŽÍVÁ MICROSOFT NABÍZENÁ OZNÁMENÍ SLUŽBY NEBO DORUČENÍ MICROSOFT NABÍZENÁ OZNÁMENÍ SLUŽBY OZNÁMENÍ NEPŘERUŠENÝ, BEZ CHYB, NEBO JINAK ZARUČENÉ PROBLÉMY NA ZÁKLADĚ V REÁLNÉM ČASE.

**Nemůžeme zaručit, že aplikace předá procesu ověření pokud neodpovídají těmito doporučeními.**

##<a name="handle-data-push-optional"></a>Úchyt pro data nabízená (volitelné)

Pokud chcete aplikaci moct přijímat posune Reach dat, máte implementace dvou událostí třídy EngagementReach:

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

##<a name="customize-ui-optional"></a>Přizpůsobení uživatelského rozhraní (volitelné)

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

Nastavte obsah `EngagementReach.Instance.Handler` pole s vlastní objektu ve vaší `App.xaml.cs` třídy v rámci `Application_Launching` metody.

**Ukázkový kód:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Ve výchozím nastavení používá zapojení samostatném provádění `EngagementReachHandler`. Nemusíte vytvářet vlastní a pokud uděláte, nemusíte přepsání všech metody. Výchozí chování je základní objekt Engagement.

### <a name="layouts"></a>Rozložení

Ve výchozím nastavení použije Reach vložené prostředky DLL zobrazíte oznámení a stránek.

Však můžete se rozhodnout, používat vlastními prostředky, aby odrážely značku v tyto součásti.

Je možné přepsat `EngagementReachHandler` metody v podtřídě zjistit zapojení použít rozložení:

**Ukázkový kód:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] `CreateNotification` Metody můžete vrátit hodnotu null. Nezobrazí se oznámení a kampaně reach dojde ke ztrátě.

Zjednodušit vaší implementací rozložení, nabízíme také naší xaml, který slouží jako základ pro váš kód. Se nacházejí v archivu můžete zapojit SDK (/ src/reach /).

> [AZURE.WARNING] Zdroje, které nabízíme jsou přesně stejné jako, které budeme používat. Takže pokud chcete upravovat přímo, nezapomeňte změnit obor názvů a název.

### <a name="notification-position"></a>Oznámení o umístění

Ve výchozím nastavení zobrazí se oznámení v aplikaci dole v levé části aplikace. Můžete změnit toto chování přepsáním `GetNotificationPosition` metodu `EngagementReachHandler` objektu.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

V současné době můžete vybrat mezi `BOTTOM` (výchozí) a `TOP` pozice.

### <a name="launch-message"></a>Spuštění zprávy

Poté, co uživatel klikne na informační systému (oznámením), můžete zapojit otevře aplikace, načíst obsah zprávy nabízená a zobrazte stránku pro odpovídající kampaní.

Existuje zpoždění mezi spuštění aplikace a zobrazení stránky (v závislosti na rychlosti sítě).

Chcete-li uživateli načítání něco, by měl obsahovat vizuální informace, jako pruh průběh nebo indikátor průběhu. Zapojení nemůže zpracovat, ale obsahuje několik popisovače za vás.

Provádět zpětné dělat:

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

Zpětné můžete nastavit v vaše `Application_Launching` metodu vaše `App.xaml.cs` souboru nejlépe před `EngagementReach.Instance.Init()` volání.

> [AZURE.TIP] Každá obslužná rutina nazývá tak, že podprocesu uživatelského rozhraní. Nemáte starosti při použití MessageBox nebo něco souvisejících s uživatelského rozhraní.

[Zásady použití]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Další požadavky pro určitou aplikaci typy]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
