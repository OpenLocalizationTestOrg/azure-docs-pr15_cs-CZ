<properties
    pageTitle="Oznámení o rozbočovače lokalizované nejnovější příspěvky kurz pro iOS"
    description="Naučte se používat rozbočovače oznámení Bus služby Azure o neodeslání oznámení příspěvky lokalizované jazycích (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Použití oznámení rozbočovače k odeslání lokalizované novinky zařízení s iOS

> [AZURE.SELECTOR]
- [Pro Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat funkci [šablony](notification-hubs-templates-cross-platform-push-messages.md) Azure oznámení rozbočovače vysílání jazycích příspěvky upozornění, která byla lokalizované jazyk a zařízení. V tomto kurzu spustíte aplikaci iOS vytvořené v [Rozbočovače použití oznámení o odeslání novinky]. Až budete hotovi, budete moct zaregistrujte kategorií, které vás zajímají, zadejte jazyk, ve kterém upozornění a přijímat jen nabízených oznámení pro vybrané kategorie v příslušném jazyce.


Tvoří dvě části situaci:

- aplikace iOS umožňuje klientského zařízení nastavit jazyk a přihlášení k odběru kategorií příspěvků různých jazycích.

- back-end vysílání oznámení pomocí **značek** a **šablon** feautres rozbočovače oznámení Azure.



##<a name="prerequisites"></a>Zjistit předpoklady pro

Musíte již jste dokončili kurz [Používání rozbočovače oznámení o odeslání novinky] a mít kód k dispozici, protože tento kurz vychází přímo z tento kód.

Visual Studio 2012 nebo novější je nepovinný krok.



##<a name="template-concepts"></a>Principy šablony

V [Oznámení rozbočovače použít k odeslání novinky] integrované aplikace, která používá **značky** odběry oznámení pro různé nové kategorie.
Mnoho aplikace však obrázku více trzích a vyžadovat lokalizace. To znamená, že obsah oznámení sami muset lokalizované a doručila ji do správnou sadu zařízení.
V tomto tématu ukážeme, jak můžete pomocí funkce **šablony** rozbočovače oznámení umožňuje snadno poskytovat lokalizované nejnovějších příspěvků oznámení.

Poznámka: jedním ze způsobů pošle lokalizované upozornění, že je vytvořit více verzí každou značku. Například pro podporu angličtinu, francouzštinu a Mandarínština, by potřebujeme třemi jiné značky světě informace: "world_en", "world_fr" a "world_ch". Potom nám odešlete lokalizované verzi novinky světa každý z těchto značky. V tomto tématu používáme šablony šíření značky a požadavku odeslání více zpráv.

Na vysoké úrovni jsou šablony způsob, jak určit, jak mají určitému zařízení dostávat oznámení. Šablona určuje formát přesné datové odkazem vlastnosti, které jsou součástí zprávy aplikace back-end. V našem případě pošleme bez ohledu na národním prostředí zprávu obsahující všechny podporované jazyky:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Potom jsme zajistí, že u šablony, které odkazuje na správnou vlastnost zaregistrovat zařízení. Aplikace pro iOS, který chce zaregistrovat francouzské informace zaregistruje například takto:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Šablony jsou velmi výkonných funkcí můžou dozvědět víc o v článku naší [šablony](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>Uživatelském rozhraní aplikace

Teď můžeme změní aplikaci novinkách, kterou jste vytvořili v tématu [Použití oznámení rozbočovače odeslat novinky] odeslat lokalizované novinky pomocí šablon.


Ve vaší MainStoryboard_iPhone.storyboard přidejte ovládací prvek Segmentovaný se třemi jazyky, které bude podporujeme: angličtinu, francouzštinu a Mandarínština.

![][13]

Zkontrolujte přidáte IBOutlet ve vaší ViewController.h, jak je ukázáno v následujícím příkladu:

![][14]

##<a name="building-the-ios-app"></a>Vytvoření aplikace IOS


1. Ve vaší Notification.h metodu *retrieveLocale* přidejte a upravte úložišti a přihlášení k odběru způsobů, jak je ukázáno v následujícím příkladu:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Ve vaší Notification.m upravte metodu *storeCategoriesAndSubscribe* přidáním parametr národní prostředí a ukládání do výchozí nastavení uživatele:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Změňte způsob *přihlášení k odběru* zahrnovala národní prostředí:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Všimněte si, jak teď používáme metoda *registerTemplateWithDeviceToken*místo *registerNativeWithDeviceToken*. Jsme zaregistrujte šablony máme poskytovat json šablony a taky název šablony (naše aplikace možná budete chtít zaregistrovat různých šablon). Zkontrolujte, že zaregistrovat kategorie jako značky, jak chceme, abyste měli jistotu přijímat notifciations tyto informace.

    Přidání metody k načtení národní prostředí z výchozího nastavení uživatele:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Teď jsme změnili Naše třída oznámení, máme Ujistěte se, že naše ViewController využívá nové UISegmentControl. Přidejte následující řádek v metodu *viewDidLoad* zobrazíte národní prostředí, které je aktuálně vybrán zajistit:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    V způsobu *přihlášení k odběru* změňte přepojíte hovor na *storeCategoriesAndSubscribe* následujícím způsobem:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Nakonec budete muset aktualizovat metodu *didRegisterForRemoteNotificationsWithDeviceToken* ve vaší AppDelegate.m tak, aby mohli správně aktualizujete registrace při spuštění aplikace. Změna hovoru na metodu *přihlášení k odběru* upozornění s následujícími:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(volitelné) Odešlete oznámení lokalizované šablony z aplikace konzoly .NET.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(volitelné) Odeslání oznámení lokalizované šablony ze zařízení

Pokud nechcete mít přístup k Visual Studio nebo chcete testovat odesílání oznámení lokalizované šablony přímo v aplikaci v zařízení.  Můžete to udělat jednoduché přidat parametry lokalizované šablony `SendNotificationRESTAPI` metoda definovaná v předchozím kurzu.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Další kroky

Další informace o použití šablon najdete v tématu:

- [Informujte uživatele s rozbočovače oznámení: ASP.NET]
- [Informujte uživatele s rozbočovače oznámení: mobilní služby]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Odeslání novinky pomocí rozbočovače oznámení]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Informujte uživatele s rozbočovače oznámení: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Informujte uživatele s rozbočovače oznámení: mobilní služby]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
