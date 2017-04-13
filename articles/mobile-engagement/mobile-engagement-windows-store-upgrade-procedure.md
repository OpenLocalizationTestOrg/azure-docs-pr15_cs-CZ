<properties 
    pageTitle="Univerzální aplikace Windows SDK Upgrade postupy" 
    description="Univerzální aplikace Windows SDK Upgrade postupy pro Azure mobilní zasunutí"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Univerzální aplikace Windows SDK Upgrade postupy

Pokud jste už integrovali starší verzí systému zapojení do aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Možná bude potřeba postup několik Pokud zmeškané více verzích SDK. Pokud například můžete migrovat z 0.10.1 do 0.11.0 budete muset nejdřív použijte postup "z 0.9.0 0.10.1" pak "z 0.10.1 0.11.0" postup.

##<a name="from-330-to-340"></a>Z 3.3.0 3.4.0

### <a name="test-logs"></a>Testování protokoly

Protokoly konzoly vytvořené pomocí SDK můžete by se povolené/vypnutí/filtrování. Chcete-li přizpůsobit, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu hodnotu poskytuje společnost `EngagementTestLogLevel` výčet, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Zdroje informací

Překryvné zobrazení Reach jsme vylepšili. Je součástí SDK NuGet balíčku zdrojů.

Při upgradu na novou verzi SDK můžete zvolit, zda chcete zachovat stávající soubory ve složce překryvném prostředků nebo ne:

* Pokud předchozí překryvném pracuje za vás nebo integrace `WebView` prvky ručně pak můžete se rozhodnout, pokud chcete zachovat stávající soubory, bude dál pracovat. 
* Pokud chcete aktualizovat na nové překrytí, jednoduše nahradit celé `overlay` složku, ze svých prostředcích novým z balíčku SDK (UWP aplikace: po upgradu, dostanete se nová složka překryvném z uživatelského profilu %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Pomocí nové překryvném přepíše veškeré vlastní nastavení na předchozí verze.

##<a name="from-320-to-330"></a>Z 3.2.0 3.3.0

### <a name="resources"></a>Zdroje informací
Tento krok se týká jenom přizpůsobený zdroje. Pokud jste přizpůsobili prostředky poskytované SDK (html, obrázky, překryvném) budete muset zálohovat před upgradem a opakované použití vaše přizpůsobení v upgradovaném zdroje.

##<a name="from-310-to-320"></a>Z 3.1.0 3.2.0

### <a name="resources"></a>Zdroje informací
Tento krok se týká jenom přizpůsobený zdroje. Pokud jste přizpůsobili prostředky poskytované SDK (html, obrázky, překryvném) budete muset zálohovat před upgradem a opakované použití vaše přizpůsobení v upgradovaném zdroje.

### <a name="webview-integration"></a>Integrace webové zobrazení
Některé vylepšení podle jiné zařízení velikostech byly vydané v této verzi. Ujistěte se, že vaše integrace webové zobrazení odpovídá takto:

Ve vaší XAML stránku ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

I v souboru přidružená CS:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Z 2.0.0 3.0.0

### <a name="resources"></a>Zdroje informací
Tento krok se týká jenom přizpůsobený zdroje. Pokud jste přizpůsobili prostředkům poskytovaným SDK (html, obrázky, překryvném) budete muset zálohovat před upgradem a opakované použití vaše přizpůsobení v upgradovaném zdroje.

##<a name="from-111-to-200"></a>Z 1.1.1 2.0.0

Popisují následující kroky migrace SDK integrace služby Capptain zaměstnanecké přidružení Capptain zabezpečení, které do aplikace technologii Azure Mobile Engagement. 

> [Azure.IMPORTANT] Capptain a Mobile využití nejsou stejné služby a postupu uvedeného dole zvýrazní jenom jak migrovat aplikaci klienta. Migrace SDK v aplikaci se nemigrují dat z servery Capptain serverům zapojení Mobile

Při migraci ze starší verze, obraťte se na Capptain webu migrovat do 1.1.1 nejdřív a pak použijte následující postup

### <a name="nuget-package"></a>Nuget balíčku

**Capptain.WindowsPhone** nahrazení balíčkem Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Použití mobilního zapojení

V SDK používá termín `Engagement`. Budete muset aktualizovat projekt podle tuto změnu.

Budete muset odinstalovat aktuální balíčku nuget Capptain. Zvažte možnost, budou odebrány všechny změny ve složce Capptain zdroje. Pokud chcete zachovat soubory a pak vytvořit kopii.

Až to nainstalujte nový balíček nuget Microsoft Azure Engagement na projektu. Můžete najít ho přímo na [nuget webu]. nebo sem index. Tato akce nahradí všechny soubory zdrojů používaných zapojení a přidá nový DLL zapojení do odkazy projektu.

Budete muset vyčistit odkazy projektu odstraněním Capptain DLL odkazy. Pokud není uděláte toto, způsobí konflikt verzi Capptain a chyby dojde.

Pokud jste přizpůsobili Capptain zdrojů, zkopírujte staré obsahu soubory a vložte je do nové soubory můžete zapojit. Upozorňujeme, že soubory xaml a cs mají být aktualizovány.

Po provedení těchto kroků musíte pouze nahradit staré Capptain odkazy na nový odkaz Engagement.

1. Všechny obory názvů Capptain mají být aktualizovány.

    Před migrací:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Po migraci:
    
        using Microsoft.Azure.Engagement;

2. Všechny tříd Capptain, které obsahují "Capptain" by měl obsahovat "Využití".

    Před migrací:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Po migraci:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Pro soubory xaml Capptain názvů a atributy taky změnit.

    Před migrací:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Po migraci:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Překryvné zobrazení stránky se přizpůsobuje
    > [AZURE.IMPORTANT] Překryvné zobrazení změníte. Nový obor názvů je `Microsoft.Azure.Engagement.Overlay`. Má se nemusí používat v soubory xaml a cs. Kromě `CapptainGrid` má být s názvem `EngagementGrid`, `capptain_notification_content` a `capptain_announcement_content` jsou s názvem `engagement_notification_content` a `engagement_announcement_content`.
    
    Pro překrytí:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Z něj stal:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Pro dalších zdrojů, jako jsou Capptain obrázků a souborů ve formátu HTML Všimněte si, že jsou také mít přejmenování používat "Využití".

### <a name="project-declaration"></a>Deklarace projektu

Na Package.appxmanifest `File Type Associations` byl aktualizovaný z:

 -   capptain\_dosáhla\_obsahu pro zapojení\_dosáhla\_obsahu
 -   capptain\_protokolu\_soubor, který chcete zapojení\_protokolu\_souboru

### <a name="application-id--sdk-key"></a>ID aplikace / klíč SDK

Zapojení používá připojovací řetězec. Nemusíte zadat ID aplikace a klávesu se SDK zapojení Mobile, stačí zadat připojovací řetězec. Ho můžete nastavit v souboru EngagementConfiguration.

Konfigurace zapojení se dá nastavit vaší `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor:

-   Vaše aplikace připojovací řetězec mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete určit za běhu, můžete volat metodu před inicializace agent můžete zapojit:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí na portálu Classic Azure.

### <a name="items-name-change"></a>Změna názvu položky

Všechny položky s názvem *capptain* mít název *engagement*. Podobně jako pro *Capptain* k *Engagement*.

Příklady běžně používaných Capptain položek:

-   CapptainConfiguration teď s názvem EngagementConfiguration
-   CapptainAgent teď s názvem EngagementAgent
-   CapptainReach teď s názvem EngagementReach
-   CapptainHttpConfig teď s názvem EngagementHttpConfig
-   GetCapptainPageName teď s názvem GetEngagementPageName

Všimněte si, že přejmenovat také ovlivňuje přepsat metody.

 
