<properties
    pageTitle="Začínáme s aplikacemi jiných Azure Mobile zapojení pro Windows Phone Silverlight"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízených oznámení pro Windows Phone Silverlight aplikace."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Začínáme s aplikacemi jiných Azure Mobile zapojení pro Windows Phone Silverlight

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu se dozvíte, jak používat Azure Mobile zapojení pomůže porozumět použití aplikace a odeslat nabízená oznámení Segmentovaný uživatelům aplikace Silverlight Windows Phone.
Tento kurz ukazuje jednoduchý scénář vysílání pomocí mobilního Engagement. Vytvořte z prázdné aplikace Silverlight Windows Phone, který shromažďuje základní data volání a přijímání nabízená oznámení pomocí Microsoft nabízená oznámení služby (MPNS).

> [AZURE.NOTE] Pokud směrujete Windows Phone 8.1 (bez Silverlight), podívejte se do [Windows univerzální kurz](mobile-engagement-windows-store-dotnet-get-started.md).

Tento kurz vyžaduje:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget balíčku

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Nastavení mobilních zapojení pro svůj Windows Phone aplikace

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integrace", což je sadu minimální potřebná k shromažďování dat a odeslání nabízená oznámení. Dokumentace dokončení integrace najdete v [mobilní zapojení Windows Phone SDK integrace](mobile-engagement-windows-phone-sdk-overview.md)

Pomocí aplikace Visual Studio prokázat integrace vytvoříme základní aplikace.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Vytvoření nového projektu Silverlight Windows Phone

Následující kroky předpokládají použití Visual Studio 2015 když kroky jsou podobné v dřívějších verzích aplikace Visual Studio. 

1. Spusťte aplikaci Visual Studio a na **úvodní** obrazovce vyberte **Nový projekt**.

2. V místní nabídce vyberte **Windows 8** -> **Windows Phone** -> **Prázdné aplikace (Windows Phone Silverlight)**. Vyplnění v aplikaci **název** a **název řešení**a klikněte na **OK**.

    ![][1]

3. Můžete ho směrovat **Windows Phone 8.0** nebo **Windows Phone 8.1**.

Teď jste vytvořili novou aplikaci Windows Phone Silverlight kam jsme začlenit Azure Mobile Engagement SDK.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Připojení k back-end zapojení mobilní aplikace

1. Nainstalujte balíček nuget [MicrosoftAzure.MobileEngagement] v projektu.

2. Otevřít `WMAppManifest.xml` (ve složce vlastnosti) a mít jistotu, deklarované následující (přidat je, pokud nejsou) v `<Capabilities />` značku:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Nyní vložte připojovací řetězec, který jste si zkopírovali zapojení mobilní aplikace a vložte je do `Resources\EngagementConfiguration.xml` zařadit mezi `<connectionString>` a `</connectionString>` značky:

    ![][3]

4. V `App.xaml.cs` souboru:

    na. Přidejte `using` údajů:

            using Microsoft.Azure.Engagement;

    b. Inicializace SDK v `Application_Launching` metodu:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Vložení následujících akcí v `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Povolit sledování v reálném čase

Abyste mohli začít odesílání dat a zajistit, aby uživatelé jsou aktivní, musí aspoň jednu obrazovku (aktivita) poslat back-end Mobile Engagement.

1. V MainPage.xaml.cs, přidejte `using` údajů:

        using Microsoft.Azure.Engagement;

2. Základní třídy **MainPage**, což je před **PhoneApplicationPage**, nahraďte **EngagementPage**.

        class MainPage : EngagementPage 
    
3. Ve vaší `MainPage.xml` souboru:

    na. Přidání názvů deklarací:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Nahrazení `phone:PhoneApplicationPage` v název značky XML s `engagement:EngagementPage`.

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile vám umožní komunikovat a dosáhnout vaši uživatelé s nabízená oznámení a zprávy v aplikaci v souvislosti s kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech nastavení aplikace k jejich odběru.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Povolení aplikace pro příjem MPNS nabízená oznámení

Přidání nové funkce pro váš `WMAppManifest.xml` souboru:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Inicializace REACH SDK

1. V `App.xaml.cs`, zavolejte `EngagementReach.Instance.Init();` ve funkci **Application_Launching** hned po inicializace agent:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. V `App.xaml.cs`, zavolejte `EngagementReach.Instance.OnActivated(e);` ve funkci **Application_Activated** hned po inicializace agent:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Máte všechno nastavené. Teď můžeme ověří, že budete mít správně báječné se tato základní integrace.

##<a id="send"></a>Odeslání oznámení pro aplikace

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Teď byste měli vidět oznámení na vašem zařízení, které se zobrazí jako oznámení v aplikaci Pokud aplikaci se otevře v opačném případě jako oznámením například následující: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
