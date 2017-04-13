<properties
    pageTitle="Rozbočovače oznámení o nejnovějších příspěvků kurz – iOS"
    description="Naučte se používat rozbočovače oznámení Bus služby Azure o neodeslání oznámení nejnovějších příspěvků na zařízení s iOS."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Odeslání novinky pomocí rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat Azure oznámení rozbočovače vysílání oznámení nejnovější příspěvky do aplikace pro iOS. Až budete hotovi, bude moct zaregistrujte si nejnovější příspěvky kategorií, které vás zajímají a zobrazí jenom nabízených oznámení pro těchto kategorií. Tento scénář je běžné vzor pro mnoho aplikace, kde oznámení musí být odeslány do skupiny uživatelů, které jste dříve deklarované zájem o nich, například programu pro čtení RSS, aplikace pro hudbu ventilátory atd.

Vysílání scénáře jsou povoleny s využitím jednu nebo více _značek_ při vytváření registrace v centru oznámení. Když jsou odeslána oznámení následujícím uživatelům značku, všechna zařízení, které jste si zaregistrovali značku dostanou oznámení. Protože značek jsou jednoduše řetězce, nemají zřízení předem. Další informace o značky podívejte se do [směrování rozbočovače oznámení a výrazy značku](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto téma je založena na aplikaci vytvořené v části [Začínáme s oznámení rozbočovače][get-started]. Před zahájením tohoto kurzu, třeba již jste dokončili [začít pracovat s oznámení rozbočovače][get-started].

##<a name="add-category-selection-to-the-app"></a>Výběr kategorie přidejte do aplikace

Cílem prvního kroku je přidání prvků uživatelského rozhraní do vaší stávající scénáře, které umožňují uživatelům vybrat kategorie k registraci. Kategorie vybrané uživatelem jsou uložené na zařízení. Po spuštění aplikace, vytvoří se registrace zařízení jako značky ve rozbočovače oznámení s vybrané kategorie.

1. Ve vaší MainStoryboard_iPhone.storyboard přidejte následující součásti z knihovny objektu:
    + Popisek s textem "Novinkách."
    + Štítky s texty kategorie "Světa", "Politickém", "Business", "Technologie", "Jiné vědecké", "Sports"
    + Šest přepínače, jednu na kategorii, přepínač každý **stát** **vypnout** ve výchozím nastavení.
    + Tlačítko s označením "Přihlásit k odběru"

    Vaše scénáře by měl vypadat takto:

    ![][3]

2. V editoru Pomocník pro vytvoření zásuvek pro všechny přepínače a jim zavolá "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Vytvoření akce pro tlačítko s názvem "odběr". Vaše ViewController.h by měl obsahovat takto:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Vytvořte novou **Kakao dotykového ovládání třídy** s názvem `Notifications`. V části rozhraní souboru Notifications.h zkopírujte následující kód:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Přidejte následující směrnice import Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Zkopírujte následující kód v části implementaci souboru Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Tento třídy používá místní úložiště k ukládání a načítání kategorií příspěvků, které tato zařízení dostanou. Navíc obsahuje způsob, jak zaregistrovat těchto kategorií pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace.

7. V souboru AppDelegate.h pro Notifications.h přidáte příkaz import a přidejte vlastnost instance třídy oznámení:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. V metodě **didFinishLaunchingWithOptions** AppDelegate.m přidání kódu pro Inicializace instance oznámení na začátku metody.  
 
    `HUBNAME`a `HUBLISTENACCESS` (podle hubinfo.h) už měli `<hub name>` a `<connection string with listen access>` zástupné symboly nahrazeny názvu centrální oznámení a připojovací řetězec pro *DefaultListenSharedAccessSignature* , který jste získali dříve

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Protože přihlašovací údaje, které jsou rozdělení s klientské aplikace nejsou obecně zabezpečené, by měl distribuovat klávesu poslech přístup pomocí protokolu pouze s aplikací klienta. Příjem umožňuje přístup k aplikaci zaregistrovat k oznámení, ale existující registrace nelze upravit, a nemůže odeslána oznámení následujícím. Plný přístup používá se služby Zabezpečené back-end pro posílání oznámení a změna existující registrace.


9. V metodě **didRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m nahraďte kód v metodu následující kód k předání token zařízení třídy oznámení. Oznámení třídy provede registrace pro oznámení s kategoriemi. Pokud uživatel změní výběrů kategorie, název `subscribeWithCategories` metoda v odpovědi na tlačítko **Přihlásit odběr** k jejich aktualizaci.

    > [AZURE.NOTE] Protože tokenu zařízení přiřazené tak, že Apple nabízená oznámení služby (APNS) můžete kdykoli šanci, byste měli zaregistrovat oznámení často chcete-li předejít oznámení o selhání. V tomto příkladu zaregistruje pro oznámení o každém spuštění aplikace. K aplikacím, které často se mají spustit více než jednou za den, můžete pravděpodobně přeskočit registrace Chcete-li zachovat šířky pásma, pokud od předchozího registrace uplynul menší než jeden den.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Všimněte si, že v tomto okamžiku je třeba jiný kód v metodě **didRegisterForRemoteNotificationsWithDeviceToken** .

10. Následujících metod už by měly tvořit AppDelegate.m dokončení [začít pracovat s oznámení rozbočovače] [ get-started] kurz.  Pokud ne, je přidat.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Tento způsob zpracování oznámení při spuštění aplikace zobrazením simple **UIAlert**.

11. V ViewController.m přidejte příkaz import pro AppDelegate.h a zkopírujte následující kód do metody generované XCode **Přihlásit odběr** . Tento kód aktualizujete registraci oznamování používat novou kategorii značky, které uživatel zvolil v uživatelském rozhraní.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Tento způsob vytvoří **NSMutableArray** kategorií a používá třídu **oznámení** ukládat seznamu v místní úložiště a registruje odpovídající značky rozbočovače oznámení. Při změně kategorie, znovu pomocí nových kategorií vytvoří registrace.

12. V ViewController.m přidejte tento kód v metodě **viewDidLoad** nastavení uživatelského rozhraní založený na dříve uložené kategorie.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Aplikaci teď mohou být uloženy sadu kategorií v zařízení místní úložiště použili k registraci centru oznámení při každém spuštění aplikace.  Uživatele můžete změnit výběrem kategorie za běhu a klikněte na požadovanou metodu **přihlášení k odběru** aktualizaci registrace zařízení. Dále se aktualizovat aplikaci o neodeslání oznámení nejnovější příspěvky přímo přímo v aplikaci.


##<a name="optional-sending-tagged-notifications"></a>(volitelné) Odeslání příznakem oznámení

Pokud nemáte přístup ke Visual Studiu, můžete přeskočit dál a odeslat oznámení z samotnou aplikaci. Můžete odeslat taky oznámení správnou šablonu z [Portálu klasické Azure] pomocí karty ladění pro vaše Centrum oznámení. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(volitelné) Odeslání oznámení ze zařízení

Běžným způsobem bude službou back-end odesláno oznámení, ale můžete odeslat oznámení nejnovější příspěvky přímo v aplikaci. K tomuto budeme aktualizovat `SendNotificationRESTAPI` metodu, která byla definována v [začít pracovat s oznámení rozbočovače] [ get-started] kurz.


1. V aktualizaci ViewController.m `SendNotificationRESTAPI` metodu jako následuje tak, aby přijímá parametr značky kategorie a odešle oznámení správné [šablony](notification-hubs-templates-cross-platform-push-messages.md) .

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. V ViewController.m aktualizované akce **Odeslat oznámení** , jak je znázorněno kód, který následuje. Tak, aby vám pošle oznámení pomocí všech označení jednotlivě a odeslat více platformách.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Znovu vytvořte projekt a ujistěte se, že máte bezchybné Tvůrce dotazů.


##<a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat oznámení

1. Stiskněte tlačítko Spustit sestavovat projektu a spusťte aplikaci. Nastavit některé možnosti příspěvky jazycích přihlásit k odběru a klepněte na tlačítko **Přihlásit odběr** . Zobrazí dialogové okno označující, že oznámení přihlásili k odběru.

    ![][1]

    Při volbě **přihlásit k odběru**aplikace převede vybrané kategorie značky a požádá rozbočovači oznámení o nové registrace zařízení pro vybrané značky.

2. Napište zprávu odeslat jako novinky a pak stiskněte tlačítko **Odeslat oznámení** . Můžete taky aplikaci pro .NET konzoly Generovat oznámení.

    ![][2]


3. Každý zařízení, nemáte předplatné novinky budou upozornění jazycích příspěvky, které jste odeslali.



## <a name="next-steps"></a>Další kroky

V tomto kurzu budeme naučili vysílání novinky podle kategorie. Zkuste jednu z následujících výukové programy, které zvýrazňují další rozšířené scénáře rozbočovače oznámení o dokončení:

+ **[Použití oznámení rozbočovače vysílání lokalizované novinky]**

    Zjistěte, jak rozbalte aplikaci nejnovějších příspěvků povolit odesílání lokalizované upozornění.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Použití oznámení rozbočovače vysílání lokalizované novinky]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klasického portálu]: https://manage.windowsazure.com
