<properties
    pageTitle="Nabízená oznámení Azure rozbočovače zabezpečené"
    description="Zjistěte, jak posílání zabezpečené nabízená oznámení pro aplikace pro iOS z Azure. Ukázky napsané v cíle-C a C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Nabízená oznámení Azure rozbočovače zabezpečené

> [AZURE.SELECTOR]
- [Univerzální systému Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Základní informace

Podpora nabízených oznámení v Microsoft Azure umožňuje přístup k snadno použitelné s více platformami, diagramů s měřítky mimo nabízených infrastruktury, což výrazně zjednodušuje provádění nabízených oznámení pro aplikace příjemce a enterprise pro mobilní platformy.

Z důvodu zákonné nebo omezení zabezpečení někdy aplikace může být vhodné zahrnout něco upozornění, které nelze předávají infrastruktury standardní nabízená oznámení. Tento kurz popisuje, jak dosáhnout stejné prostředí tak, že citlivé informace pomocí funkce zabezpečené ověřené připojení mezi klientského zařízení a back-end aplikace.

Na vysoké úrovni, řízení toku, je takto:

1. Aplikace back-end:
    - Úložiště zabezpečené datové v back-end databázi.
    - ID toto oznámení odešle zařízení (se neodesílají žádné informace zabezpečené).
2. Aplikaci v zařízení pro příjem oznámení:
    - Zařízení kontaktů back-end požaduje zabezpečené datové.
    - Aplikaci můžete zobrazit datové jako upozornění na zařízení.

Je důležité mít na paměti, že v předchozím tok (a v tomto kurzu) jsme se předpokládá, že zařízení ukládá token ověřování místní úložiště, po přihlášení. Úplně bezproblémovou práci, zajistíte tak jako zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na upozornění. Pokud aplikace neukládá tokeny ověřování na zařízení nebo můžete platnost těchto tokenů, aplikaci zařízení po přijetí oznámení by měl zobrazit obecné oznámení dotázání uživatele k spuštění aplikace. Aplikace potom ověřuje uživatele a zobrazí datové oznámení.

Tento kurz zabezpečené nabízených ukazuje, jak bezpečně odeslání nabízená oznámení. Kurz je založena na kurz [Upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tak, aby měli dokončení kroků v tomto kurzu nejdřív.

> [AZURE.NOTE] Tento kurz se předpokládá, že jste vytvořili a nakonfigurovali oznámení rozbočovači podle popisu v [Příručce Začínáme s rozbočovače oznámení (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Upravit projekt iOS

Teď jste změnili vaše aplikace serverové odeslat jenom oznámení *id* , budete muset změnit iOS aplikace zpracovat oznámení a zavoláte zpět vaše serverové k načtení zabezpečené zprávy zobrazovat.

K tomuto účelu máme zápis logiky k načtení zabezpečený obsah z aplikace back-end.

1. Ujistěte se, registry aplikace pro pasivní oznámení, zpracuje id oznámení odeslané z back-end v **AppDelegate.m**. Možnost **UIRemoteNotificationTypeNewsstandContentAvailability** doplněk didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. V **AppDelegate.m** přidáte oddíl implementaci nahoře následující deklarace with:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. V části implementaci zadejte následující kód nahrazení zástupného symbolu `{back-end endpoint}` s koncový bod pro vaše serverové získané dříve:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Teď máme zpracovat příchozí oznámení o a použijte metodu výše k načtení obsah, který chcete zobrazit. Nejdřív máme povolení aplikace iOS běží na pozadí příjem nabízených oznámení. V **XCode**vyberte projekt aplikace v levém panelu klikněte na cílovém hlavní aplikace v části **cíle** z podokna centrální.

5. Potom klikněte na kartu **Možnosti** v horní části středovém podokně a zaškrtněte políčko **Vzdálené oznámení** .

    ![][IOS1]


6. V **AppDelegate.m** přidejte následující metodu ke zpracování nabízená oznámení:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Všimněte si, že je vhodnější případů chybějící vlastnosti záhlaví ověřování nebo zamítnutí zpracovávat back-end. Specifické zpracování takovýchto případech závisí převážně na cílovém uživatelské prostředí. Jednou z možností se zobrazí oznámení s obecný výzva pro uživatele k ověření k načtení skutečné oznámení.

## <a name="run-the-application"></a>Spusťte aplikaci

Pokud chcete spustit aplikaci, postupujte takto:

1. V XCode spusťte aplikaci v zařízení fyzicky iOS (nabízená oznámení nebudou fungovat v simulator).

2. V aplikaci iOS uživatelského rozhraní zadejte uživatelské jméno a heslo. Tyto může být jakýkoli řetězec, ale musí mít stejnou hodnotu.

3. V aplikaci iOS uživatelského rozhraní klikněte na **přihlásit**. Klepněte na tlačítko **Odeslat připínáčku**. Měli byste vidět zabezpečené oznámení zobrazení v Centru pro oznámení.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
