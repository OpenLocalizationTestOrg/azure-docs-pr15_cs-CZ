<properties
    pageTitle="Azure Mobile zapojení iOS SDK přehled | Microsoft Azure"
    description="Nejnovější aktualizace a postupy pro iOS SDK pro zapojení Mobile Azure"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK pro zapojení Mobile Azure

Chcete-li získat všechny podrobnosti o tom, jak integrovat Azure Mobile zapojení v iOS aplikace, začněte tady. Pokud chcete vyzkoušejte si to poprvé, zkontrolujte, jestli že se naše [kurz 15 minut](mobile-engagement-ios-get-started.md).

Kliknutím na tlačítko Zobrazit [Obsahu SDK](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integrace postupy
1. Začněte zde: [jak integrovat Mobile zapojení do aplikace pro iOS](mobile-engagement-ios-integrate-engagement.md)

2. Oznámení: [Jak integrovat Reach (oznámení) do aplikace pro iOS](mobile-engagement-ios-integrate-engagement-reach.md)

3. Označení plán implementaci: [používání Upřesnit Mobile zapojení přiřazování značek k rozhraní API v aplikace pro iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Poznámky k verzi

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Pevné oznámení o není actioned na zařízení s iOS 10.
-   Neschválení XCode 7.

Starší verze najdete v článku [dokončení poznámky](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už integrovali starší verzí systému zapojení do aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Možná bude potřeba postup několik Pokud zmeškané více verzích najdete v článku SDK dokončení [Upgradu postupy](mobile-engagement-ios-upgrade-procedure.md).

Pro každé nové verze SDK je třeba nejprve nahradit (odeberte a znovu importovat v xcode) EngagementSDK a EngagementReach složky.

###<a name="from-300-to-400"></a>Z 3.0.0 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 je povinný, počínaje 4.0.0 verzi SDK.

> [AZURE.NOTE] Pokud opravdu závisí na XCode 7 mohli byste použít [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh). Je známý problém při spuštění na zařízení s iOS 10 v modulu reach této předchozí verze: upozornění systému nejsou actioned. Vyřešíte to bude potřeba, abyste implementace zastaralé rozhraní API `application:didReceiveRemoteNotification:` v aplikaci delegovat následujícím způsobem:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nedoporučujeme toto alternativní řešení:** jako toto chování můžete změnit upgradu nadcházející (i dílčí) iOS verze, protože se nedá použít tento iOS rozhraní API. Můžete přepnout do XCode 8 co nejdříve.

#### <a name="usernotifications-framework"></a>UserNotifications framework
Budete muset přidat `UserNotifications` framework ve vaší sestavit fázích.

v podokně projektu otevřete podokno projektu a vyberte správný cíl. Potom otevřete kartu **"Sestavení fáze"** a v nabídce **"binární s knihovnami"** přidat framework `UserNotifications.framework` – nastavení na odkaz jako`Optional`

#### <a name="application-push-capability"></a>Aplikace nabízených možností
XCode 8 může obnovit aplikace nabízených možností, prosím dvojité vrácení se změnami `capability` kartu vybraného cíle.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Přidání nového registračního kódu oznámení o iOS 10
Starší fragment kódu pro registraci aplikace na oznámení stále funguje, ale používá zastaralé rozhraní API při spuštění v iOS 10. 

Import `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

V aplikaci delegátovi `application:didFinishLaunchingWithOptions` nahradit metodu:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

tak, že:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Pokud už máte svoji vlastní implementaci UNUserNotificationCenterDelegate

V SDK obsahuje vlastní provádění UNUserNotificationCenterDelegate protokolu. Tak, že v SDK slouží ke sledování životního cyklu zapojení oznámení na zařízeních s IOS 10 nebo novější. Pokud v SDK zjistí delegáta nepoužije vlastní implementaci, protože může být pouze jeden delegát UNUserNotificationCenter na aplikace. To znamená, že budete mít pro přidání logiky zapojení do vlastního delegáta.

Existují dva způsoby, jak dosáhnout.

Jednoduše tak, že přesměrování delegáta volá SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Nebo tak, že dědí od `AEUserNotificationHandler` třídy

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Můžete určit, jestli oznámení pochází z zapojení nebo není předáním jeho `userInfo` slovníku agenta `isEngagementPushPayload:` třídy metody.