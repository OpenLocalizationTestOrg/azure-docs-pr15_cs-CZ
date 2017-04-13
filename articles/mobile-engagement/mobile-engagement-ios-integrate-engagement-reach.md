<properties
    pageTitle="Azure Mobile zapojení iOS SDK dosáhla integrace | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Jak integrovat zapojení na iOS

Postup integrace postupu uvedeného v [tom, jak integrovat zapojení na iOS dokumentu](mobile-engagement-ios-integrate-engagement.md) před provedením této příručce.

Tento si přečtěte následující dokumentaci vyžaduje XCode 8. Pokud opravdu závisí na XCode 7 mohli byste použít [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh). Je známý problém v předchozí verzi při spuštění na zařízení s iOS 10: upozornění systému nejsou actioned. Vyřešíte to bude potřeba, abyste implementace zastaralé rozhraní API `application:didReceiveRemoteNotification:` v aplikaci delegovat následujícím způsobem:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nedoporučujeme toto alternativní řešení:** jako toto chování můžete změnit upgradu nadcházející (i dílčí) iOS verze, protože se nedá použít tento iOS rozhraní API. Můžete přepnout do XCode 8 co nejdříve.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Povolení aplikace pro příjem pasivní nabízená oznámení

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Kroky integrace

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Vložení SDK dosáhla zapojení do projektu iOS

-   Přidání Reach sdk Xcode projektu. V Xcode, přejděte na **projektu \> přidat do projektu** a zvolte `EngagementReach` složky.

### <a name="modify-your-application-delegate"></a>Úprava aplikace delegáta

-   V horní části souboru implementaci importujte modul dosáhla můžete zapojit:

        [...]
        #import "AEReachModule.h"

-   Uvnitř metody `applicationDidFinishLaunching:` nebo `application:didFinishLaunchingWithOptions:`, vytvořte modulu reach a předejte existující čáru inicializace můžete zapojit:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Úprava řetězec **"icon.png"** s názvem obrázek, který chcete jako oznamovací ikonu.
-   Pokud chcete použít možnost *aktualizace Odznáček hodnota* v Reach kampaní nebo pokud chcete používat nativní nabízených \<SaaS/Reach rozhraní API/kampaně formát/nativní nabízených\> kampaní, je nutné nechat Reach modul Správa Odznáček ikonu samotné (bude automaticky zrušte Odznáček aplikace a také obnovit hodnoty uložené ve zapojení pokaždé, když aplikace je Začínáme nebo foregrounded). Důvodem je přidáním následující řádek po inicializace modulu Reach:

        [reach setAutoBadgeEnabled:YES];

-   Pokud chcete zpracovat nabízených Reach data, je nutné nechat aplikace delegáta odpovídat `AEReachDataPushDelegate` protokol. Po inicializace modulu Reach přidejte následující řádek:

        [reach setDataPushDelegate:self];

-   Pak můžete provádět způsobů `onDataPushStringReceived:` a `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` v delegátovi aplikace:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategorie

Parametr kategorie je nepovinný při vytváření kampaně nabízená dat a umožňuje že k filtrování dat posune. To je užitečné, pokud chcete posunout různých druhů z `Base64` dat a chcete určit jejich typ před jejich analýzy.

**Aplikace je nyní připravena k přijímání a zobrazit obsah reach!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Jak přijímat oznámení a hlasování kdykoli

Zapojení můžete svým koncovým uživatelům poslat Reach oznámení kdykoli pomocí Apple nabízená oznámení služby.

Chcete-li tuto funkci, budete muset připravit aplikace pro Apple nabízená oznámení a změnit aplikace delegáta.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Příprava aplikace nabízených oznámení společnosti Apple

Postupujte podle průvodce: [jak připravit aplikace Apple nabízená oznámení](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Přidání kódu pro potřeby klienta

*V tomto okamžiku by měl mít vaše aplikace registrovaný certifikát Apple push v Engagement frontend.*

Pokud to není neučinili, musíte zaregistrovat aplikace pro příjem nabízených oznámení.

* Import `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Přidejte následující řádek při spuštění aplikace (obvykle v `application:didFinishLaunchingWithOptions:`):

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

Pak budete muset poskytovat zapojení token zařízení vrácené Apple servery. K tomu metodu s názvem `application:didRegisterForRemoteNotificationsWithDeviceToken:` v delegátovi aplikace:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Nakonec je potřeba informovat SDK zapojení, když aplikace obdrží vzdálené oznámení. Aby je dostala, zavolejte metodu `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` v delegátovi aplikace:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Výše uvedené metody zavedená v iOS 7. Pokud směrujete iOS < 7, zkontrolujte, že implementovat metodu `application:didReceiveRemoteNotification:` v aplikaci delegát a volání `applicationDidReceiveRemoteNotification` na EngagementAgent předáním nula místo `handler` argument:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Ve výchozím nastavení můžete zapojit dosáhla ovládací prvky completionHandler. Pokud chcete ručně Odpovědět `handler` blokovat ve vašem kódu, předáte nulu `handler` argument a ovládacím prvkem dokončení blokovat sami. Zobrazit `UIBackgroundFetchResult` typu seznam možných hodnot.


### <a name="full-example"></a>Kompletní příklad

Tady je příklad celou integrace:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Pokud máte svoji vlastní implementaci UNUserNotificationCenterDelegate

V SDK také obsahuje vlastní provádění UNUserNotificationCenterDelegate protokolu. Tak, že v SDK slouží ke sledování životního cyklu zapojení oznámení na zařízeních s IOS 10 nebo novější. Pokud v SDK zjistí delegáta nepoužije vlastní implementaci, protože může být pouze jeden delegát UNUserNotificationCenter na aplikace. To znamená, že budete mít pro přidání logiky zapojení do vlastního delegáta.

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

##<a name="how-to-customize-campaigns"></a>Jak přizpůsobit kampaně

### <a name="notifications"></a>Oznámení

Existují dva typy oznámení: oznámení o systému a v aplikaci.

Upozornění systému uskutečněných jednotlivými iOS a nelze upravovat.

Oznámení v aplikaci jsou určené zobrazení, který se dynamicky přidá aktuální okna aplikace. Je místo toho možnost překrytím oznámení. Oznámení překrytí jsou skvělé k rychlé integrace, protože se nevyžaduje upravte jakékoli zobrazení v aplikaci.

#### <a name="layout"></a>Rozložení

Chcete-li změnit vzhled vám oznámení v aplikaci, můžete jednoduše upravit soubor `AENotificationView.xib` vašim potřebám, dokud uchováváte značku hodnoty a typy existující dílčích zobrazení.

Ve výchozím nastavení jsou uvedeny v aplikaci oznámení v dolní části obrazovky. Pokud chcete, aby se zobrazily v horní části obrazovky, upravit poskytované `AENotificationView.xib` a změňte `AutoSizing` vlastnost hlavní zobrazení, můžete uchovávat v horní části jeho superview.

#### <a name="categories"></a>Kategorie

Při úpravě zadané rozložení změníte vzhled vám oznámení. Kategorie umožňují definovat různé vzhledy cílových (případně chování) pro oznámení. Kategorie lze zadat při vytváření Reach kampaní. Mějte na paměti, že kategorie také umožňují přizpůsobit oznámení a hlasování, které jsou popsané dál v tomto dokumentu.

Zaregistrovat rutinu kategorie pro vám oznámení, potřebujete přidat hovoru až modul reach inicializován.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`musí být instancí objekt, který odpovídá protokol `AENotifier`.

Implementace metody protokol sami nebo se rozhodnete přeimplementovat existující třídy `AEDefaultNotifier` která již provede většinu práce.

Pokud chcete znovu zobrazit oznámení pro určité kategorie, je třeba postupovat podle v tomto příkladu:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Tento jednoduchý příklad kategorie se předpokládá, že máte otevřený soubor s názvem `MyNotificationView.xib` v hlavní aplikaci příruček. Pokud metodu nedokáže najít odpovídající `.xib`, nezobrazí se oznámení a můžete zapojit vypíše zprávu v konzole.

Soubor ujednaných hrot psacího pera měli dodržovat následující pravidla:

-   By měl obsahovat pouze jeden zobrazení.
-   Dílčích zobrazení by měl být stejný typů jako souboru zadané hrot psacího pera s názvem`AENotificationView.xib`
-   Dílčích zobrazení by měla být stejné značky jako z nich uvnitř poskytované hrot psacího pera souboru nazvaného`AENotificationView.xib`

> [AZURE.TIP] Jenom zkopírujte soubor ujednaných hrot psacího pera s názvem `AENotificationView.xib`a začnete pracovat odtud. Dejte si pozor, ale zobrazení uvnitř tento soubor hrot psacího pera souvisí s předmětu `AENotificationView`. Tato třída znovu definovat metodu `layoutSubViews` přesunout a změnit velikost jeho dílčích zobrazení podle kontextu. Je vhodné nahradit `UIView` nebo vlastní zobrazení předmětu.

Pokud potřebujete hlubší přizpůsobení vám oznámení (Pokud například chcete načíst zobrazení přímo z kódu), doporučuje se podívejte se na zadané zdrojového kódu a třídy dokumentace `Protocol ReferencesDefaultNotifier` a `AENotifier`.

Všimněte si, že můžete použít stejné oznamovatel pro více kategoriemi.

Změnit jeho definici oznamovatel výchozí nějak takto:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Oznámení o zpracování

Pokud chcete použít výchozí kategorie, některé metody životního cyklu nazývají na `AEReachContent` objektu statistiky sestavy a aktualizovat stav kampaně:

-   Když oznámení zobrazená v aplikaci `displayNotification` způsob je místo toho možnost (který sestavy Statistika) tak, že `AEReachModule` Pokud `handleNotification:` vrátí `YES`.
-   Je-li zrušit oznámení `exitNotification` názvem metody statistický hlášení a další kampaní lze teď zpracovávat.
-   Po kliknutí na oznámení `actionNotification` je nazýván, vykázaného statistický a provedení akce přidružené.

Pokud vaše implementace `AENotifier` obchází výchozí chování, budete muset volat těchto postupů životního cyklu za vás. Následující příklady ilustrují některých případech, kdy je výchozí chování obejít:

-   Není rozšíření `AEDefaultNotifier`, například implementovaná kategorie zpracování od začátku.
-   Můžete overrode `prepareNotificationView:forContent:`, je potřeba namapovat aspoň `onNotificationActioned` nebo `onNotificationExited` jednu z vaší U.I ovládacích prvků.

> [AZURE.WARNING] Pokud `handleNotification:` vyvolá se odstraní výjimky obsahu a `drop` je s názvem, to je uveden ve Statistika a další kampaní lze teď zpracovávat.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Zahrnout oznámení jako součást existující zobrazení

Překrytí jsou skvělé k rychlé integrace, ale může být někdy není vhodné nebo můžete mít nežádoucí vedlejší efekty.

Pokud nejste spokojení se sadou překrytí v některé z vašich zobrazení, ho můžete přizpůsobit pro tyto zobrazení.

Můžete se rozhodnout, zahrnout naše oznámení rozložení existující zobrazení. Postup je dva implementaci styly:

1.  Přidání zobrazení oznámení pomocí rozhraní tvůrce

    -   Otevřít *rozhraní tvůrce*
    -   Umístěte 320 × 60 (nebo 768 × 60 Pokud jste na Ipadu) `UIView` místo, kam chcete oznámení zobrazit
    -   Nastavte hodnotu značky pro toto zobrazení: **36822491**

2.  Přidání zobrazení oznámení programově. Pokud zobrazení má aktualizována jenom přidáte následující kód:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Akce makra najdete v `AEDefaultNotifier.h`.

> [AZURE.NOTE] Výchozí oznamovatel automaticky zjistí, že rozložení oznámení je součástí toto zobrazení a nepřidá překrytí jej.

### <a name="announcements-and-polls"></a>Oznámení a hlasování

#### <a name="layouts"></a>Rozložení

Upravovat soubory `AEDefaultAnnouncementView.xib` a `AEDefaultPollView.xib` , dokud uchováváte značku hodnoty a typy existující dílčích zobrazení.

#### <a name="categories"></a>Kategorie

##### <a name="alternate-layouts"></a>Alternativní rozložení

Třeba oznámení kampaně kategorie lze mít alternativní rozložení pro oznámení a hlasování.

Vytvořit kategorie pro oznámení, musíte rozšíření **AEAnnouncementViewController** a zaregistrovat po aktualizována modulu reach:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Pokaždé, když uživatel bude klepnout na oznámení pro oznámení s kategorii "Moje\_kategorie", ovladači registrovaných zobrazení (v takovém případě `MyCustomAnnouncementViewController`) bude inicializován tak, že zavoláte metodu `initWithAnnouncement:` a zobrazení se přidají do okna aktuální aplikace.

Vaše implementace `AEAnnouncementViewController` třídy budete muset číst vlastnosti `announcement` inicializace vaší dílčích zobrazení. Zvažte níže uvedeném příkladu, kde dvě popisky jsou inicializována pomocí `title` a `body` vlastnosti `AEReachAnnouncement` třídy:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Pokud nechcete, aby načítání v zobrazeních za vás, ale chcete znovu použít výchozí rozložení zobrazení oznámení, můžete jednoduše provést ovladači vlastní zobrazení rozšiřuje zadané třídy `AEDefaultAnnouncementViewController`. V takovém případě duplikování souboru hrot psacího pera `AEDefaultAnnouncementView.xib` a přejmenujte ho, může být zavedená ovladači vlastní zobrazení (u řadiče s názvem `CustomAnnouncementViewController`, je potřeba zavolat na souboru hrot psacího pera `CustomAnnouncementView.xib`).

Nahrazení výchozí kategorie oznámení, jednoduše zaregistrovat ovladači vlastní zobrazení kategorie podle `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Hlasování můžete přizpůsobit stejným způsobem jako:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Tentokrát, poskytované `MyCustomPollViewController` musí rozšíření `AEPollViewController`. Nebo můžete zvolit rozšiřte z výchozího řadiče: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Nezapomeňte zavolejte `action` (`submitAnswers:` pro vlastní hlasování zobrazení řadiče) nebo `exit` metoda před zrušit řadiče zobrazení. V opačném statistických údajů se neodesílají (tedy bez technologie pro analýzu na kampaně) a další hlavně další kampaně upozorněni, dokud se nerestartuje procesu aplikace.

##### <a name="implementation-example"></a>Příklad implementace

V této implementaci zobrazení vlastní oznámení načíst z externích xib souboru.

Jako Upřesnit oznámení přizpůsobit, se doporučuje se podívat, zdrojového kódu standardní implementace.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
