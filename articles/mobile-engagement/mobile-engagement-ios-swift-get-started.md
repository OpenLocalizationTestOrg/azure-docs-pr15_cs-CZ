<properties
    pageTitle="Začínáme s Azure Mobile zapojení pro iOS v Swift | Microsoft Azure"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro iOS aplikace."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Začínáme s Azure Mobile zapojení pro iOS aplikace v Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu se dozvíte, jak používat Azure Mobile zapojení pomůže porozumět použití aplikace a odešlete Segmentovaný uživatelům aplikace iOS nabízená oznámení.
V tomto kurzu vytvoříte prázdné iOS aplikace, který shromažďuje základní data volání a přijímání nabízená oznámení pomocí Apple Push upozornění systému (APNS).

Tento kurz vyžaduje:

+ 8 XCode, které si můžete nainstalovat z App Store pro Mac.
+ [zapojení Mobile iOS SDK]
+ Nabízená oznámení certifikát (p12), kterou můžete získat v centru vývojářů Apple

> [AZURE.NOTE] Tento kurz používá Swift verze 3.0. 

Dokončení tohoto kurzu je požadována pro všechny ostatní Mobile zapojení výukové programy pro iOS aplikace.

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Nastavit mobilní zapojení pro aplikace pro iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integrace", což je sadu minimální potřebná k shromažďování dat a odeslání nabízená oznámení. Dokumentace úplné začlenění najdete v [mobilní zapojení iOS SDK integrace](mobile-engagement-ios-sdk-overview.md)

Vytvoříme základní aplikace s XCode prokázat integraci:

###<a name="create-a-new-ios-project"></a>Vytvoření nového projektu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Připojení k back-end zapojení mobilní aplikace

1. Stáhněte si [můžete zapojit Mobile iOS SDK]
2. Extrahování. tar.gz souboru do složky ve vašem počítači
3. Klikněte pravým tlačítkem myši projektu a vyberte možnost "Souborů, které chcete přidat..."

    ![][1]

4. Přejděte do složky, které jste extrahovali SDK a vyberte `EngagementSDK` složky Po klepnutí na tlačítko OK.

    ![][2]

5. Otevřít `Build Phases` kartu a `Link Binary With Libraries` nabídce Přidat předlohy, jak je ukázáno v následujícím příkladu:

    ![][3]

8. Vytvoření přemosťování záhlaví pro nebudou moct používat v SDK cíle C API pomocí příkazu Soubor > Nový > Soubor > iOS > zdroje > soubor záhlaví.

    ![][4]

9. Upravte přemostění soubor záhlaví vystavit Mobile zapojení cíle-C kód rychlé kódu, přidejte následující importy:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. V části nastavení sestavení zkontrolujte, že sestavení C cílů přemostění záhlaví nastavení v části Swift kompilátoru - generování kódu má cestu k toto záhlaví. Tady je příklad cesty: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (podle toho, cesta)**

    ![][6]

11. Vraťte se k portálu Azure na stránce vaše aplikace *Informace o připojení* a zkopírujte připojovací řetězec

    ![][5]

12. Vložit připojovací řetězec v `didFinishLaunchingWithOptions` delegáta

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Povolení sledování v reálném čase

Abyste mohli začít odesílání dat a zajistit, aby uživatelé jsou aktivní, musí aspoň jednu obrazovku (aktivita) poslat back-end Mobile Engagement.

1. Otevřete soubor **ViewController.swift** a nahraďte základní třídy **ViewController** být **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Povolení služby nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile vám umožní komunikovat a dosáhnout s jinými uživateli se nabízená oznámení a zprávy v aplikaci v souvislosti s kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech se instalační program aplikace k jejich odběru.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Povolení aplikace pro příjem pasivní nabízená oznámení

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Přidání Reach knihovny do projektu

1. Klikněte pravým tlačítkem myši do projektu
2. Vyberte`Add file to ...`
3. Přejděte do složky, které jste extrahovali SDK
4. Vyberte `EngagementReach` složky
5. Klikněte na Přidat
6. Upravte přemostění soubor záhlaví vystavit Mobile zapojení cíle-C dosáhla záhlaví a přidejte následující importy:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Úprava aplikace delegáta

1. V `didFinishLaunchingWithOptions` – vytvoření modulu reach a předejte mu existující čáru inicializace zapojení:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Povolení aplikace pro příjem APNS nabízená oznámení
1. Přidejte následující řádek `didFinishLaunchingWithOptions` metodu:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Přidejte `didRegisterForRemoteNotificationsWithDeviceToken` metoda takto:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Přidejte `didReceiveRemoteNotification:fetchCompletionHandler:` metoda takto:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Zapojení Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
