<properties
    pageTitle="Nabízená oznámení Azure rozbočovače Rich"
    description="Zjistěte, jak chcete poslat formátovaný nabízená oznámení do aplikace pro iOS z Azure. Ukázky napsané v cíle-C a C#."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure oznámení rozbočovače RTF připínáčku


##<a name="overview"></a>Základní informace

Abyste mohli zapojit uživatelé, kteří mají rychlé bohaté obsah, možná budete chtít aplikace nabízená za prostý text. Toto oznámení propagace interakcí uživatele a prezentace obsahu, jako je adresy URL, zvuky, obrázky nebo kupóny nebo další. Tento kurz je založena na téma [Upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) a ukazuje, jak poslat nabízená oznámení, které obsahují datové části (například obrázek).


Tento kurz je kompatibilní s operačním systémem iOS 7 a 8.

  ![][IOS1]

Na vysoké úrovni:

1. Back-end aplikace:
    - Uloží propracovaných datové (v tomto případě obrázek) do back-end databáze nebo místní úložiště
    - Odešle ID tohoto bohaté oznámení na zařízení
2. Aplikace na zařízení:
    - Kontakty back-end požadující propracovaných datové s ID obdrží
    - Odešle oznámení uživatelů na zařízení při načítání dat dokončení a zobrazí datové hned po uživatelů klepněte na další informace


## <a name="webapi-project"></a>WebAPI projektu

1. Ve Visual Studiu otevřete **AppBackend** projekt, který jste vytvořili v kurzu [Upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .
2. Získáte obrázek, který chcete upozornit uživatele a umístí jej ve složce aplikace **img** v adresáři vašeho projektu.
3. Klikněte na **Zobrazit všechny soubory** v Průzkumníku řešení a klikněte pravým tlačítkem na složku, kterou chcete **Zahrnout do projektu**.
4. S vybraným obrázkem změňte jeho vytvoření akcí v okně vlastností **Vložený**zdroji.

    ![][IOS2]

5. V **Notifications.cs**, přidejte následující text pomocí příkazu:

        using System.Reflection;

6. Aktualizujte celé třídy **oznámení** o následující kód. Ujistěte se, že nahraďte zástupné symboly oznámení centrální pověření a název souboru obrázku.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (volitelné) Další informace o tom, jak přidat a získat projektové zdroje v nápovědě k [vložení a přístupu k prostředkům pomocí Visual Basic](http://support.microsoft.com/kb/319292) .

7. V **NotificationsController.cs**upravit **NotificationsController** s následující zlomky. Odešle první pasivní bohaté upozornění id zařízení a umožňuje klientských načítání obrázku:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Teď můžeme znovu nasadí tato aplikace na web Azure před uskutečněním přístupných osobám s postižením na všech zařízeních. Klikněte pravým tlačítkem myši na **AppBackend** projektu a vyberte **Publikovat**.

9. Vyberte Azure webu jako cíl publikování. Přihlaste se pomocí účtu Azure a vyberte web na existující nebo nové a poznamenejte si vlastnost **cílovou adresu URL** na kartě **připojení** . Tato adresa URL jako *koncový bod back-end* jsme bude odkazovat dál v tomto kurzu. Klikněte na **Publikovat**.

## <a name="modify-the-ios-project"></a>Upravit projekt iOS

Teď jste změnili back-end vaše aplikace odeslat jenom oznámení *id* , změní aplikace iOS zpracovat této id a načtení bohaté zprávy z vašeho back-end.

1. Otevřete projekt iOS a povolit vzdálené oznámení, že přejdete do cíle hlavní aplikace v části **cílů** .

2. Klikněte na **Možnosti**, zapněte **Režimy pozadí**a zaškrtněte políčko **Vzdálené oznámení** .

    ![][IOS3]

3. Přejděte na **Main.storyboard**a ujistěte se, že jste u řadiče zobrazení (uvedená jako Domů řadiče domény zobrazení v tomto kurzu) z kurzu [Upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .

4. Přidání **Řadiče domény navigace** do svého scénáře a ovládací prvek a přetáhněte do Domů zobrazení řadiče usnadnit **kořenové zobrazení** navigace. Zaškrtněte políčko **Je počáteční řadiče zobrazení** v atributy Kontrola metadat je pouze řadiče navigace.

5. Přidejte do **Zobrazení řadiče** scénáře a přidání **Obrázků zobrazení**. Tohle je stránka, která uživatelům se zobrazí po zvolte zobrazíte další kliknutím na notifiication. Vaše scénáře by měl vypadat takto:

    ![][IOS4]

6. Klikněte na **Domů zobrazení řadiče** ve scénáři a ujistěte se, že ho **homeViewController** jako své **Vlastní tříd** a **Scénáře ID** v části Kontrola Identity.

7. Stejně postupujte i u řadiče zobrazení obrázek jako **imageViewController**.

8. Vytvořte nový třídy řadiče zobrazení s názvem **imageViewController** zpracovat v uživatelském rozhraní jste právě vytvořili.

9. V **imageViewController.h**přidejte do následující řadiči rozhraní deklarace. Zkontrolujte, že ovládací prvek přetažením ze zobrazení obrázek scénáře můžete tyto vlastnosti chcete vytvořit odkaz dvou:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. V **imageViewController.m**přidejte na konci **viewDidload**následující:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. V **AppDelegate.m**importujte řadiče obrázek, který jste vytvořili:

        #import "imageViewController.h"

12. Přidání oddílu rozhraní následující deklarace with:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. V **AppDelegate**, zkontrolujte svoji aplikaci zaregistruje pasivní oznámení v **aplikace: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Nahraďte v následujících implementaci **aplikace: didRegisterForRemoteNotificationsWithDeviceToken** se scénáři uživatelského rozhraní změní účtu:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Potom přidejte následujících metod k **AppDelegate.m** k načtení obrázek z vašeho koncového bodu a odeslání místní oznámení po dokončení vyhledávání. Zkontrolujte, že nahraďte zástupný symbol `{backend endpoint}` s back-end koncový bod:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Zpracování místní oznámení nad otevřením řadiče zobrazení obrázků v **AppDelegate.m** pomocí následujících metod:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Spusťte aplikaci

1. V XCode spusťte aplikaci v zařízení fyzicky iOS (nabízená oznámení nebudou fungovat v simulator).

2. V aplikaci iOS uživatelského rozhraní zadejte uživatelské jméno a heslo stejné hodnoty pro ověřování a klikněte na **Log In**.

3. Klikněte na **Odeslat nabízených** a byste měli vidět oznámení v aplikaci. Když kliknete na **Další**, můžete být uvedena obrázek, který se rozhodnete přidat v serverové části aplikace.

4. Můžete taky kliknout na **odeslání nabízená** a okamžitě stiskněte tlačítko Domů zařízení. Ve chvíli zobrazí se nabízená oznámení. Pokud klepněte na něm nebo kliknutím na další můžete uvede do aplikace a obsah propracovaných obrázek.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
