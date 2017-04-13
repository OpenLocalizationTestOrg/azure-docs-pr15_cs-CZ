<properties
    pageTitle="Zaregistrovat aktuálního uživatele k nabízená oznámení pomocí rozhraní API webových | Microsoft Azure"
    description="Zjistěte, jak požádat o registraci nabízená oznámení v aplikaci pro iOS s Azure oznámení rozbočovače při registeration provádí rozhraní API webových ASP.NET."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Registrace aktuálního uživatele k nabízená oznámení pomocí technologie ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak požádat o registraci nabízená oznámení s Azure oznámení rozbočovače při registraci provádí rozhraní API webových ASP.NET. Toto téma rozšiřuje kurz [upozornění uživatelé, kteří mají rozbočovače oznámení]. Musíte už dokončení požadované kroky v této kurz vytvoření ověřené mobilních služeb. Další informace o scénář uživatelé upozornit najdete v článku [upozornění uživatelé, kteří mají rozbočovače oznámení].

##<a name="update-your-app"></a>Aktualizace aplikace  

1. Ve vaší MainStoryboard_iPhone.storyboard přidejte následující součásti z knihovny objektu:

    + **Popisek**: "Použít pro uživatele s oznámení rozbočovače"
    + **Popisek**: "InstallationId"
    + **Popisek**: "Uživatel"
    + **Pole text**: "Uživatel"
    + **Popisek**: "Heslo"
    + **Pole text**: "Heslo"
    + **Tlačítko**: "Přihlášení"

    V tomto okamžiku vaše scénáře vypadá takto:

    ![][0]

2. V editoru Pomocník pro vytvoření zásuvek přepínané ovládacích prvků jim zavolá, spojení textových polí s řadiče zobrazení (delegátů) a vytvoření **Akce** pro tlačítko **přihlášení** .

    ![][1]

    Soubor BreakingNewsViewController.h by teď měla obsahovat následující kód:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Vytvoření třídy s názvem **DeviceInfo**a zkopírujte následující kód do části rozhraní souboru DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. V části implementaci DeviceInfo.m souborů zkopírujte následující kód:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. V PushToUserAppDelegate.h přidejte následující vlastnosti jednoznačné:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. V metodě **didFinishLaunchingWithOptions** PushToUserAppDelegate.m přidáte následující kód:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    První řádek inicializuje jednoznačné **DeviceInfo** . Na druhém řádku spustí registrace pro, které prezentovat oznámení, které už je, že už jste dokončili kurz [Seznámení s rozbočovače oznámení] .

9. V PushToUserAppDelegate.m implementovat metoda **didRegisterForRemoteNotificationsWithDeviceToken** ve vaší AppDelegate a přidejte následující kód:

        self.deviceInfo.deviceToken = deviceToken;

    Tím se nastaví token zařízení žádosti.

    > [AZURE.NOTE] V tomto okamžiku nesmí být jakýkoli kód v této metody. Pokud už máte volání metody **registerNativeWithDeviceToken** přidaný po dokončení kurz [Seznámení s rozbočovače oznámení](/manage/services/notification-hubs/get-started-notification-hubs-ios/) , je nutné škrtnutí komentáře nebo odebrat toto volání.

10. V souboru PushToUserAppDelegate.m přidáte metodu následující rutinu:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Tento způsob zobrazí upozornění v uživatelském rozhraní aplikace obdrží upozornění je spuštěná.

9. Otevřete soubor PushToUserViewController.m a vraťte se klávesnice k provedení těchto věcí:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. V metodě **viewDidLoad** v souboru PushToUserViewController.m inicializace nápisem installationId následujícím způsobem:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Přidejte následující vlastnosti v rozhraní v PushToUserViewController.m:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Potom přidejte následující implementaci:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Zkopírujte následující kód do vytvořil XCode rutina **přihlášení** :

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Tento způsob je načtena e ID instalace a kanálů pro nabízená oznámení a odešle, spolu s typy zařízení metodě ověřené rozhraní API webových vytvářející registrace v oznámení o rozbočovače. Tento rozhraní API webových byla definována v [oznámení uživatelé, kteří mají rozbočovače oznámení].

Teď aktualizoval klientské aplikaci vrátíte [upozornění uživatelé, kteří mají rozbočovače oznámení] a mobilní služba o neodeslání oznámení pomocí oznámení rozbočovače aktualizace.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Informujte uživatele s rozbočovače oznámení]: /manage/services/notification-hubs/notify-users-aspnet

[Začínáme s rozbočovače oznámení]: /manage/services/notification-hubs/get-started-notification-hubs-ios
