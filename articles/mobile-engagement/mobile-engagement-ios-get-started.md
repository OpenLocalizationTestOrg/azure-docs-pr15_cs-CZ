<properties
    pageTitle="Začínáme s Azure Mobile zapojení pro iOS v cíle C | Microsoft Azure"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízených oznámení pro aplikace iOS."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Začínáme s Azure Mobile zapojení pro iOS aplikace do cíle C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pomůže porozumět použití aplikace a odeslat nabízená oznámení Segmentovaný uživatelům aplikace iOS.
V tomto kurzu vytvoříte prázdné iOS aplikace, který shromažďuje základní data volání a přijímání nabízená oznámení pomocí Apple Push upozornění systému (APNS).

Tento kurz vyžaduje:

+ 8 XCode, které si můžete nainstalovat z App Store pro Mac.
+ [zapojení Mobile iOS SDK]

Dokončení tohoto kurzu je požadována pro všechny ostatní Mobile zapojení výukové programy pro iOS aplikace.

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Nastavit mobilní zapojení pro aplikace pro iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integraci", což je sadu minimální potřebná k shromažďování dat a odeslání nabízená oznámení. Dokumentace dokončení integrace najdete v [mobilní zapojení iOS SDK integrace](mobile-engagement-ios-sdk-overview.md)

Vytvoříme základní aplikace s XCode prokázat integrace.

###<a name="create-a-new-ios-project"></a>Vytvoření nového projektu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Připojení k back-end zapojení mobilní aplikace

1. Stáhněte si [můžete zapojit Mobile iOS SDK].
2. Extrahování. tar.gz souboru do složky ve vašem počítači.
3. Klikněte pravým tlačítkem myši projektu a potom vyberte **Přidat soubory do**.

    ![][1]

4. Přejděte do složky, které jste extrahovali SDK, vyberte `EngagementSDK` složky a stiskněte **OK**.

    ![][2]

5. Otevřete kartu **Vytvoření fáze** a v nabídce **Binární s knihovnami** přidat předlohy, jak je ukázáno v následujícím příkladu:

    ![][3]

6. Vraťte se k portálu Azure na stránce vaše aplikace **Informace o připojení** a zkopírujte připojovací řetězec.

    ![][4]

7. Přidejte následující řádek kódu v souboru **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Vložit připojovací řetězec v `didFinishLaunchingWithOptions` delegáta.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`je nepovinné údajů, což umožňuje SDK protokoly pro identifikaci problémy. 

##<a id="monitor"></a>Povolit sledování v reálném čase

Abyste mohli začít odesílání dat a zajistit, aby uživatelé jsou aktivní, musí aspoň jednu obrazovku (aktivita) poslat back-end Mobile Engagement.

1. Otevřete soubor **ViewController.h** a import **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Teď nahradit super třídy rozhraní **ViewController** tak, že `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile umožňuje pracovat s jinými uživateli a dosáhnout s nabízená oznámení a aplikace pro zasílání zpráv v rámci kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech nastavení aplikace k jejich odběru.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Povolení aplikace pro příjem pasivní nabízená oznámení

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Přidání Reach knihovny do projektu

1. Klikněte pravým tlačítkem myši do projektu.
2. Vyberte **soubor, který chcete přidat**.
3. Přejděte do složky, které jste extrahovali SDK.
4. Vyberte `EngagementReach` složky.
5. Klikněte na **Přidat**.

### <a name="modify-your-application-delegate"></a>Úprava aplikace delegáta

1. Zpátky v souboru **AppDeletegate.m** importujte modul dosáhla Engagement.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Uvnitř `application:didFinishLaunchingWithOptions` metody, vytvořte modul Reach a předejte mu existující čáru inicializace můžete zapojit:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Povolení aplikace pro příjem APNS nabízená oznámení

1. Přidejte následující řádek `application:didFinishLaunchingWithOptions` metodu:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Přidejte `application:didRegisterForRemoteNotificationsWithDeviceToken` metoda takto:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Přidejte `didFailToRegisterForRemoteNotificationsWithError` metoda takto:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Přidejte `didReceiveRemoteNotification:fetchCompletionHandler` metoda takto:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Zapojení Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

