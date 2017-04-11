<properties
    pageTitle="Přidat nabízená oznámení pro aplikace Xamarin.iOS službou Azure aplikace"
    description="Naučte se používat aplikaci služby Azure odešlete aplikace Xamarin.iOS nabízená oznámení"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Přidání aplikace Xamarin.iOS nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Základní informace

V tomto kurzu nabízená oznámení přidejte do projektu [Xamarin.iOS rychlý start](app-service-mobile-xamarin-ios-get-started.md) tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Proveďte [Rychlý úvod Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) kurz.

* Zařízení s iOS fyzické. Nabízená oznámení nepodporuje iOS simulator.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registrace aplikace pro nabízená oznámení na portálu pro vývojáře společnosti Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Konfigurace aplikace Mobile odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizace project serveru odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Konfigurace Xamarin.iOS projektu

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Přidání nabízená oznámení pro aplikace

1. V **QSTodoService**přidejte následující vlastnosti tak, aby **AppDelegate** získávat mobilního klienta:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Přidejte následující `using` údajů do horní části souboru **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. V **AppDelegate**přepište **FinishedLaunching** událostí:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Ve stejném souboru přepište **RegisteredForRemoteNotifications** událost. V tomto kódu registrujete pro oznámení jednoduché šablonu, kterou dostanou přes všechny podporované platformy serverem.

    Další informace o šablonách s rozbočovače oznámení najdete v článku [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Potom přepište událost **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Aplikace je teď aktualizovat, aby podporovaly nabízená oznámení.

## <a name="test"></a>Test nabízená oznámení v aplikaci

1. Stiskněte tlačítko **Spustit** sestavovat projektu a v může zařízení s iOS spusťte aplikaci a potom klikněte na **OK** přijmete nabízená oznámení.

    > [AZURE.NOTE] U aplikace musí explicitně přijmete nabízená oznámení. Tento požadavek vyskytuje při prvním spuštění aplikace.

2. V aplikaci zadejte úkolu a potom klepněte na znaménko plus (**+**) ikonu.

3. Ověřte, že je dostali oznámení a potom klikněte na **OK** zavřete oznámení.

4. Zopakujte krok 2 a okamžitě ukončete aplikaci a potom zkontrolujte, že se zobrazí oznámení.

Úspěšné dokončení tohoto kurzu.

<!-- Images. -->

<!-- URLs. -->



