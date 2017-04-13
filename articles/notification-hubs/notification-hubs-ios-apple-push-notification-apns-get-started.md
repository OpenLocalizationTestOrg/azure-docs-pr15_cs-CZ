<properties
    pageTitle="Odesílání nabízených oznámení pro iOS s Azure oznámení rozbočovače | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak používat Azure oznámení rozbočovače nabízená oznámení odešlete aplikace iOS."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="nabízená oznámení, nabízená oznámení ios nabízená oznámení"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Odesílání nabízených oznámení pro iOS rozbočovače oznámení Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače odešlete aplikace iOS nabízená oznámení. Vytvoříte prázdné iOS aplikace, která přijímá nabízená oznámení pomocí [služby nabízených oznámení společnosti Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Když jste hotoví, byste měli použít rozbočovače oznámení vysílání nabízená oznámení na zařízeních spuštění aplikace.

## <a name="before-you-begin"></a>Než začnete

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód pro účely tohoto návodu najdete [na GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ [Mobilní služba iOS SDK verze 1.2.4]
+ Nejnovější verzi [Xcode]
+ IOS 8 (nebo novější)-může zařízení
+ Členství v [Apple vývojář programu](https://developer.apple.com/programs/) .

   > [AZURE.NOTE] Z důvodu konfigurace požadavky pro nabízená oznámení musíte nasazení a otestujte nabízená oznámení na zařízení fyzicky iOS (iPhone nebo iPad) místo iOS Simulator.

Dokončení tohoto kurzu je požadována pro všechny ostatní oznámení rozbočovače výukové programy pro iOS aplikace.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurace rozbočovače oznámení pro iOS nabízená oznámení

V této části provede vás vytvořením nového centrální oznámení a konfigurace ověřování pomocí APNS pomocí certifikátu nabízených **p12** , který jste vytvořili. Pokud chcete použít rozbočovači oznámení, kterou jste vytvořili, můžete přejít ke kroku 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Kliknutím na tlačítko <b>Oznámení služby</b> v zásuvné <b>Nastavení</b> a potom vyberte <b>Apple (APNS)</b>. Klikněte na <b>Odeslat certifikát</b> a vyberte <b>p12</b> soubor, který jste exportovali dříve. Zkontrolujte, jestli že je také zadat správné heslo.</p>
<p>Zkontrolujte, že vyberte režim <b>izolovaného prostoru</b> , protože jde o vývojové. <b>Výrobní</b> použijte, pouze pokud chcete odeslat uživatelům, kteří si zakoupili aplikace z obchodu nabízená oznámení.</p>
</li>
</ol>
&emsp;&emsp;![Konfigurace APNS Azure portálu](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Konfigurace certifikátu APNS portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Vaše centrum oznámení je nyní nakonfigurován pro práci s APNS a máte řetězce připojení k registraci aplikace a odeslat nabízená oznámení.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Připojení k oznámení rozbočovače aplikace pro iOS

1. V Xcode vytvořte nový projekt iOS a vyberte šablonu **Jediný zobrazení aplikace** .

    ![Xcode - jednoduché zobrazení aplikace][8]

2. Nastavení možností pro nový projekt, nezapomeňte používat stejný **Název produktu** a **Identifikátor organizace** , který jste použili při jste nastavili na portálu pro vývojáře Apple ID skupiny.

    ![Xcode - možnosti aplikace project][11]

3. V části **cílů**klikněte na název projektu, klikněte na kartu **Vytvoření nastavení** a rozbalte **Identity podepsání kódu**a ve skupinovém rámečku **ladění**, nastavte vaši identitu podepsání kódu. Zapíná a vypíná **úrovně** na **základní** **všechny**a nastavte **Zřizování profilu** zřizovací profil, který jste vytvořili v předchozích krocích.

    Pokud nevidíte nové zřizovací profil, který jste vytvořili v Xcode, zkuste obnovit profilů u svého podpisu identity. **Xcode** řádku nabídek klikněte na, klikněte na **Předvolby**, klikněte na kartu **účet** , klikněte na tlačítko **Zobrazit podrobnosti** , klepněte podpisový identitu a klikněte na tlačítko Aktualizovat v pravém dolním.

    ![Xcode - zřizovací profilu][9]

4. Stáhněte si [mobilních služeb iOS SDK verze 1.2.4] a rozbalte soubor. V Xcode klikněte pravým tlačítkem myši projektu a klikněte na možnost **Přidat soubory do** **WindowsAzureMessaging.framework** složku přidat k Xcode projektu. **Kopírování položek v případě potřeby**vyberte a klikněte na tlačítko **Přidat**.

    >[AZURE.NOTE] Oznámení o rozbočovače SDK aktuálně nepodporuje bitcode na Xcode 7.  **Povolení Bitcode** musíte nastavit na hodnotu **Ne** v části **Možnosti vytváření** plánu projektu.

    ![Rozbalení Azure SDK][10]

5. Přidání nového souboru záhlaví projektu s názvem `HubInfo.h`. Tento soubor bude obsahovat konstanty pro vaše Centrum oznámení.  Přidejte následující definice a nahrazení zástupného literál typu řetězec s vaším *Centrální název* a *DefaultListenSharedAccessSignature* , který jste dříve nalezli.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Otevřete svůj `AppDelegate.h` soubor přidejte následující direktivy import:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Ve vaší `AppDelegate.m file`, přidejte následující kód v `didFinishLaunchingWithOptions` metoda podle si verzi iOS. Tento kód registruje zařízení úchyt APNs:

    Pro iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    IOS verze před 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Ve stejném souboru přidejte následující metody. Tento kód se připojí k centru oznámení pomocí informace o připojení, které jste zadali v HubInfo.h. Potom dá token zařízení k rozbočovači oznámení tak, aby centru oznámení můžete odeslat oznámení:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Ve stejném souboru přidejte následujícího postupu zobrazte **UIAlert** Pokud doručení během aplikace nebude aktivní:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Vytvoření a spuštění aplikace na svém zařízení a ověřte, že bez chyb.

## <a name="send-test-push-notifications"></a>Odeslání zkušební nabízená oznámení


Můžete otestovat dostávat oznámení ve formě do aplikace tak, že nabízená oznámení v [Azure portálu] prostřednictvím části **Poradce při potížích** v centrální zásuvné (použijte možnost *Testování odeslat* ).

![Azure portálu - Test odeslat][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Volitelné) Odeslání nabízená oznámení v aplikaci

>[AZURE.IMPORTANT] Tento příklad odeslat oznámení z klienta aplikace je vynechaný výukové pouze. Když to budou vyžadovat `DefaultFullSharedAccessSignature` jako prezentace v aplikaci klienta, poskytuje rozbočovače oznámení na riziko, že uživatel mohou získat přístup k neodeslání oznámení neoprávněným klienty.

Pokud chcete odeslat nabízená oznámení v aplikaci, tato část obsahuje příklady školá pomocí rozhraní REST.

1. V Xcode, otevřete `Main.storyboard` a přidejte následující prvky uživatelského rozhraní z knihovny objekt umožňuje uživateli odeslat nabízená oznámení v aplikaci:

    - Popisek s není text popisku. Použijí zprávy o chybách při odesílání oznámení. Vlastnost **řádky** měla být nastavena na **hodnotu 0** tak, aby ho automaticky velikost omezen na vpravo a levému okraji a horní části zobrazení.
    - Textové pole se **zástupným** textem nastavit **Zadejte oznámení**. Pole přímo pod nápisem omezíte, jak je ukázáno v následujícím příkladu. Nastavte zobrazení řadiče jako delegát výstupu.
    - Tlačítko s názvem **Odeslat oznámení** omezeny přímo pod textového pole a v centru vodorovné.

    Zobrazení by měl vypadat takto:

    ![Návrhář Xcode][32]


2. [Zásuvek přidat](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) popisek a textové pole připojení do zobrazení a aktualizace vaší `interface` definice podporuje `UITextFieldDelegate` a `NSXMLParserDelegate`. Přidáte tři vlastnosti deklarací ukázáno v následujícím příkladu podporu volání rozhraní REST API a analýze odpověď.

    Soubor ViewController.h by měl vypadat takto:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Otevřít `HubInfo.h` a přidejte následující konstant, které bude sloužit k odeslání oznámení rozbočovače. Nahraďte zástupný symbol řetězcová konstanta skutečné *DefaultFullSharedAccessSignature* připojovací řetězec.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Přidejte následující `#import` příkazy, aby vaše `ViewController.h` soubor.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. V `ViewController.m` implementace rozhraní přidat následující kód. Tento kód bude analyzovat *DefaultFullSharedAccessSignature* připojovací řetězec. Jak je uvedeno v [rozhraní REST API odkaz](http://msdn.microsoft.com/library/azure/dn495627.aspx), tyto analyzované informace se použijí k generovat token přidružení zabezpečení pro hlavičky **se tak mohli ověřovat** .

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. V `ViewController.m`, aktualizujte `viewDidLoad` způsob, jak analyzovat připojovací řetězec při načtení zobrazení. Také přidáte metody nástroj vidíte na obrázku, k provedení rozhraní.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. V `ViewController.m`, implementace rozhraní generovat token se tak mohli ověřovat přidružení zabezpečení, který se dozvíte v záhlaví **se tak mohli ověřovat** podle [REST API odkaz](http://msdn.microsoft.com/library/azure/dn495627.aspx)přidat následující kód.

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + přetáhněte na **Odeslat oznámení** tlačítko `ViewController.m` Chcete-li přidat akci s názvem **SendNotificationMessage** pro událost **Dotykového ovládání dolů** . Způsob aktualizace pomocí následující kód k odeslání oznámení pomocí rozhraní REST API.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. V `ViewController.m`, přidejte následující metodu delegáta k podpoře zavření klávesnice pro textové pole. CTRL + přetažení v textovém poli na ikonu řadiče domény zobrazení v Návrháři rozhraní nastavení zobrazení řadiče jako delegát výstupu.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. V `ViewController.m`, přidejte následující metody delegáta k podpoře analýza odpovědi pomocí `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Vytvoření projektu a ověřte, jestli jsou bez chyb.


> [AZURE.NOTE] Pokud dojde k chybě sestavení v Xcode7 o podpoře bitcode, změňte **Nastavení sestavení** > **Povolit Bitcode (ENABLE_BITCODE)** na hodnotu **Ne** v Xcode. V oznámení o rozbočovače SDK aktuálně nepodporuje bitcode. 

Všechny datové části možné oznámení můžete najít v Apple [místní a nabízená oznámení programového průvodce].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Kontrola, pokud nemáte aplikaci můžete dostávat nabízená oznámení

Testování nabízená oznámení v iOS, můžete na zařízení s iOS fyzické nasadit aplikace. Apple nabízená oznámení nelze odeslat pomocí iOS Simulator.

1. Spusťte aplikaci a ověřte, že se mu to registrace a potom klikněte na **OK**.

    ![iOS Test registrace aplikace nabízená oznámení][33]

2. Můžete odeslat nabízená oznámení test z [Portálu Azure], jak jsme je popsali výše. Pokud jste přidali kód pro odesílání nabízená oznámení v aplikaci, klepněte do textového pole pro zadání zprávy s upozorněním. Klepněte na tlačítko **Odeslat** na klávesnici nebo na tlačítko **Odeslat oznámení** v zobrazení, které chcete odeslat zprávu oznámení.

    ![iOS aplikace nabízená oznámení odeslat][34]

3. Nabízená oznámení odeslaný do všechna zařízení, které jsou registrované pro příjem oznámení z centra konkrétní oznámení.

    ![iOS aplikace nabízená oznámení přijímání Test][35]


##<a name="next-steps"></a>Další kroky

V tomto příkladu jednoduché vysílat nabízená oznámení na všech svých zařízeních registrovaných iOS. Měli byste jako dalším krokem při jste se naučili, pokračujte [Azure oznámení rozbočovače upozornit uživatele pro iOS back-end .NET] kurzu, který vás provede jednotlivými vytváření back-end odeslat nabízená oznámení pomocí značek. 

Pokud budete chtít segmentech uživatelů tak, že skupin úrok, můžete navíc přesunout kurz [Používání rozbočovače oznámení o odeslání novinky] . 

Obecné informace o oznámení rozbočovače najdete v tématu [Pokyny rozbočovače oznámení].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobilní služba iOS SDK verze 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Pokyny pro rozbočovače oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure oznámení rozbočovače upozornit uživatele pro iOS .NET back-end]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Odeslání novinky pomocí rozbočovače oznámení]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Místní a nabízená oznámení programování Průvodce]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure portálu]: https://portal.azure.com