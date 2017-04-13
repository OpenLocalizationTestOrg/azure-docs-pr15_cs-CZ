<properties
    pageTitle="Začínáme s Azure mobilní zapojení pro Xamarin.iOS"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro aplikace Xamarin.iOS."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Začínáme s Azure mobilní zapojení Xamarin.iOS aplikací

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pomůže porozumět použití aplikace a odeslat nabízená oznámení Segmentovaný uživatelům v aplikaci Xamarin.iOS.
V tomto kurzu vytvoříte prázdné Xamarin.iOS aplikace, která shromažďuje základní data volání a přijímání nabízená oznámení pomocí Apple Push upozornění systému (APNS).

Tento kurz vyžaduje:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio můžete používat taky s Xamarin, ale tento kurz používá Xamarin Studio. Pokyny k instalaci najdete v článku [Nastavení a instalace for Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobilní zapojení Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Nastavit mobilní zapojení pro aplikace pro iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integrace" tedy s minimálně nastavte požadované shromažďování dat a odeslání nabízená oznámení.

Vytvoříme základní aplikace s Xamarin prokázat integrace:

###<a name="create-a-new-xamarinios-project"></a>Vytvoření nového projektu Xamarin.iOS

1. Spusťte Xamarin Studio. Přejděte na **soubor** -> **nové** -> **řešení** 

    ![][1]

2. Vyberte **Jednoduché zobrazení aplikace**, ujistěte se, že vybraném jazyce **C#** a pak klikněte na **Další**.

    ![][2]

3. Zadejte **Název aplikace** a **Identifikátor organizace** a potom na tlačítko **Další**. 

    ![][3]

    > [AZURE.IMPORTANT] Ujistěte se, že publikování profil, který se nasadit pomocí aplikace pro iOS používá ID aplikace, která odpovídá přesně identifikátorem příruček tady máte. 

4. V případě potřeby aktualizujte **Název projektu**, **Řešení název** a **umístění** a klikněte na **vytvořit**.

    ![][4]
 
Xamarin Studio vytvoří aplikaci ukázku ve kterém jsme začlenit Mobile Engagement. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Připojení k back-end zapojení mobilní aplikace

1. Klikněte pravým tlačítkem myši na složku **balíčků** ve windows řešení a vyberte **Přidat balíčků**

    ![][5]

2. Hledání v **Aplikaci Microsoft Azure Mobile zapojení Xamarin SDK** a přiřadit ji někomu řešení.  

    ![][6]
   
3. Otevřete **AppDelegate.cs** a přidejte následující pomocí příkazu:

        using Microsoft.Azure.Engagement.Xamarin;

4. V metodě **FinishedLaunching** přidejte následující inicializace připojení s back-end Mobile Engagement. Ujistěte se, že přidáte **ConnectionString**. Tento kód taky používá formální **NotificationIcon** , který je přidán SDK zapojení Mobile, který chcete nahradit. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Povolení sledování v reálném čase

Abyste mohli začít odesílání dat a zajistit, že jsou aktivní uživatelé, musí aspoň jednu obrazovku poslat back-end Mobile Engagement.

1. Otevřete **ViewController.cs** a přidejte následující pomocí příkazu:

        using Microsoft.Azure.Engagement.Xamarin;

2. Nahrazení třídy, ze které `ViewController` dědí od `UIViewController` k `EngagementViewController`. 

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile umožňuje pracovat s jinými uživateli a dosáhnout s nabízená oznámení a aplikace pro zasílání zpráv v rámci kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech nastavení aplikace k jejich odběru.

### <a name="modify-your-application-delegate"></a>Úprava aplikace delegáta

1. Otevřete **AppDelegate.cs** a přidejte následující pomocí příkazu:

        using System; 

2. Teď uvnitř `FinishedLaunching` metody, přidejte následující zaregistrovat k nabízených zprávy po`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Nakonec - aktualizovat nebo přidání těchto způsobů:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. V souboru **Info.plist** v řešení potvrďte, že **Příruček identifikátor** shoduje s **ID aplikace** máte ve svém zřizovací profilu v Centru pro vývojáře Apple. 

    ![][7]

5. Ve stejném souboru **Info.plist** Ujistěte se, že jste zaškrtli **Režimy povolení pozadí** a **Vzdálené oznámení**. 

    ![][8]

6. Spusťte aplikaci na zařízení, který máte přidružený k tomuto publikování profilu. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
