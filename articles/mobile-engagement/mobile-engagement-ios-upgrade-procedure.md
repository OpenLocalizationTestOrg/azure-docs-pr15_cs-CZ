<properties
    pageTitle="Azure Mobile zapojení iOS postup upgradu SDK | Microsoft Azure"
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

#<a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už integrovali starší verzí systému zapojení do aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Pro každé nové verze SDK je třeba nejprve nahradit (odeberte a znovu importovat v xcode) EngagementSDK a EngagementReach složky.

##<a name="from-300-to-400"></a>Z 3.0.0 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 je povinný, počínaje 4.0.0 verzi SDK.

> [AZURE.NOTE] Pokud opravdu závisí na XCode 7 mohli byste použít [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh). Je známý problém při spuštění na zařízení s iOS 10 v modulu reach této předchozí verze: upozornění systému nejsou actioned. Vyřešíte to bude potřeba, abyste implementace zastaralé rozhraní API `application:didReceiveRemoteNotification:` v aplikaci delegovat následujícím způsobem:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nedoporučujeme toto alternativní řešení:** jako toto chování můžete změnit upgradu nadcházející (i dílčí) iOS verze, protože se nedá použít tento iOS rozhraní API. Můžete přepnout do XCode 8 co nejdříve.

### <a name="usernotifications-framework"></a>UserNotifications framework
Budete muset přidat `UserNotifications` framework ve vaší sestavit fázích.

v podokně projektu otevřete podokno projektu a vyberte správný cíl. Potom otevřete kartu **"Sestavení fáze"** a v nabídce **"binární s knihovnami"** přidat framework `UserNotifications.framework` – nastavení na odkaz jako`Optional`

### <a name="application-push-capability"></a>Aplikace nabízených možností
XCode 8 může obnovit aplikace nabízených možností, prosím dvojité vrácení se změnami `capability` kartu vybraného cíle.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Přidání nového registračního kódu oznámení o iOS 10
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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Pokud už máte svoji vlastní implementaci UNUserNotificationCenterDelegate

V SDK má i svůj vlastní provádění UNUserNotificationCenterDelegate protokolu. Tak, že v SDK slouží ke sledování životního cyklu zapojení oznámení na zařízeních s IOS 10 nebo novější. Pokud v SDK zjistí delegáta nepoužije vlastní implementaci, protože může být pouze jeden delegát UNUserNotificationCenter na aplikace. To znamená, že budete mít pro přidání logiky zapojení do vlastního delegáta.

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

##<a name="from-200-to-300"></a>Z 2.0.0 3.0.0
Podpora pro iOS nezobrazí 4.X. Spuštění z této verze cíl nasazení aplikace musí být alespoň iOS 6.

Pokud používáte Reach v aplikaci, musíte přidat `remote-notification` hodnotu `UIBackgroundModes` matice v souboru Info.plist k vzdálené oznámení.

Metoda `application:didReceiveRemoteNotification:` musí být nahrazuje `application:didReceiveRemoteNotification:fetchCompletionHandler:` v aplikaci delegátovi.

"AEPushDelegate.h" se nedá použít rozhraní a budete muset uživateli odebrat všechny odkazy. Platí to i pro odebrání `[[EngagementAgent shared] setPushDelegate:self]` a metody delegáta z aplikace delegáta:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Z 1.16.0 2.0.0
Popisují následující kroky migrace SDK integrace služby Capptain zaměstnanecké přidružení Capptain zabezpečení, které do aplikace technologii Azure Mobile Engagement.
Při migraci ze starší verze, obraťte se na webové stránce Capptain migrovat do 1.16 nejdřív pak použijte následující postup.

>[AZURE.IMPORTANT] Capptain a Mobile využití nejsou stejné služby a postupu uvedeného dole zvýrazní jenom jak migrovat aplikaci klienta. Migrace SDK v aplikaci se nemigrují dat z servery Capptain serverům zapojení Mobile

### <a name="agent"></a>Agent

Metoda `registerApp:` nahradily nová metoda `init:`. Aplikace delegát musí být aktualizovány příslušným způsobem a použijte připojovací řetězec:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Sledování SmartAd byla odebrána z SDK potřebujete jenom odstranit všechny výskyty `AETrackModule` třídy

### <a name="class-name-changes"></a>Změny názvů třídy

V rámci rebranding existuje několik souboru třídy/názvy, které je potřeba změnit.

Všechny třídy předponou "CP" jsou přejmenované předponou "AE".

Příklad:

-   `CPModule.h`Přejmenování k `AEModule.h`.

Všechny třídy předponou "Capptain" jsou přejmenované předponou "Využití".

Příklady:

-   Třídy `CapptainAgent` přejmenovat na `EngagementAgent`.
-   Třídy `CapptainTableViewController` přejmenovat na `EngagementTableViewController`.
-   Třídy `CapptainUtils` přejmenovat na `EngagementUtils`.
-   Třídy `CapptainViewController` přejmenovat na `EngagementViewController`.
