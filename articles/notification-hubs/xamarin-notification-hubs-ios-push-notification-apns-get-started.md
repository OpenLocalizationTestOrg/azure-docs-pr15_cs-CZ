<properties
    pageTitle="iOS nabízená oznámení s rozbočovače oznámení pro aplikace Xamarin | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak používat Azure oznámení rozbočovače odešlete aplikace iOS Xamarin nabízená oznámení."
    services="notification-hubs"
    keywords="IOS nabízená oznámení, nabízených zprávy nabízená oznámení, nabízená zprávy"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS nabízená oznámení s rozbočovače oznámení pro aplikace Xamarin

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace
> [AZURE.IMPORTANT] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače odešlete aplikace iOS nabízená oznámení.
Vytvoříte prázdné Xamarin.iOS aplikace, která přijímá nabízená oznámení pomocí [Služby nabízených oznámení společnosti Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Když jste hotoví, byste měli použít rozbočovače oznámení vysílání nabízená oznámení na zařízeních spuštění aplikace. Dokončení kód je dostupná v [NotificationHubs app] [ GitHub] vzorku.

Tento kurz ukazuje na jednoduché nabízených vysílání scénář zprávy s rozbočovače oznámení.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ [Xcode 6.0][Install Xcode]
+ IOS 7.0 (nebo novější) kompatibilní zařízení
+ iOS členství v programu Developer
+ [Xamarin Studio]

   > [AZURE.NOTE] Z důvodu konfigurace požadavky pro iOS nabízená oznámení, musíte nasazení a otestujte ukázkové aplikaci v zařízení fyzicky iOS (iPhone nebo iPad) místo v simulator.

Dokončení tohoto kurzu je požadována pro všechny ostatní oznámení rozbočovače výukové programy pro Xamarin iOS aplikace.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Konfigurace rozbočovače oznámení

V této části provede vás vytvořením nového centrální oznámení a konfigurace ověřování pomocí APNS pomocí certifikátu nabízených **p12** , který jste vytvořili. Pokud chcete použít rozbočovači oznámení, kterou jste vytvořili, můžete přejít ke kroku 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Chceme, který nakonfiguruje připojení APNS na portálu Azure, otevřete si nastavení oznámení centrální, ande klepněte na <b>Oznámení služby</b>a potom na položku <b>Apple (APNS)</b> v seznamu. Po dokončení klikněte na <b>Odeslat certifikát</b> a vyberte <b>p12</b> certifikát, který jste exportovali dříve, a heslo pro potvrzení.</p>
<p>Zkontrolujte, že vyberte režim <b>izolovaného prostoru</b> , protože bude odesílání zpráv nabízená v vývojové prostředí. Pokud chcete odeslat nabízených oznámení pro uživatele, kteří už jste si koupili aplikace z obchodu pouze pomocí nastavení <b>výrobní</b> .</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Vaše centrum oznámení je nyní nakonfigurován pro práci s APNS a máte řetězce připojení k registraci aplikace a odeslat nabízená oznámení.


##<a name="connect-your-app-to-the-notification-hub"></a>Připojení aplikace k centru oznámení

#### <a name="create-a-new-project"></a>Vytvoření nového projektu

1. V Xamarin studiu vytvořte nový projekt iOS a vyberte **Unified rozhraní API** > **Jedné aplikace zobrazení** šablony.

    ![Xamarin Studio – typ výběr aplikace][31]

2. Přidání odkazu do komponentu Azure zasílání zpráv. V zobrazení řešení pravým tlačítkem myši složku **součástí** projektu a zvolte **Získat další součásti**. Vyhledejte komponentu **Azure zasílání zpráv** a přidejte součást do projektu.

3. V **AppDelegate.cs**, přidejte následující text pomocí příkazu:

        using WindowsAzure.Messaging;

4. Deklarujte instanci **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Vytvoření třídy **Constants.cs** s následující proměnné:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. V **AppDelegate.cs**aktualizované **FinishedLaunching()** podle těchto věcí:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Přepište metodu **RegisteredForRemoteNotifications()** v **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Přepište metodu **ReceivedRemoteNotification()** v **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Vytvoření metodu **ProcessNotification()** v **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Je možné přepsat **FailedToRegisterForRemoteNotifications()** zvládnout situace například žádné síťové připojení. To je zvlášť důležité, kde uživatel může spusťte aplikaci v režimu offline (například letadla ve) a chcete zpracovat nabízených zprávy scénáře specifické pro aplikaci.


10. Spusťte aplikaci na vašem zařízení.


## <a name="sending-push-notifications"></a>Odesílání nabízená oznámení


Můžete otestovat, jak je vidět na následující obrazovce příjem nabízených oznámení v aplikaci tak, že oznámení [Azure portálu] prostřednictvím **Testování odeslat** možnost sada nástrojů **Poradce při potížích** , přímo na stránce Centrum oznámení.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Nabízená oznámení jsou obvykle posílané back-end služby, jako je mobilní služby nebo ASP.NET pomocí knihovně kompatibilní. Můžete taky rozhraní REST API přímo k odesílání zpráv nabízených Pokud knihovně není k dispozici nefunguje. 

V tomto kurzu se v jednoduchosti je krása jsme jenom ukazují testování klientské aplikace tak, že oznámení pomocí .NET SDK pro oznámení rozbočovače aplikaci konzoly místo do back-end služby. Doporučujeme kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) jako další krok pro odeslání oznámení z back-end ASP.NET. Však následujících přístupů lze použít k odeslání oznámení:

* **Rozhraní REST**: podpora nabízených oznámení na libovolné back-end platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure oznámení rozbočovače .NET SDK**: V Nuget balíčku správce for Visual Studio spustit [Instalační balíček Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [používání rozbočovače oznámení z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobilní aplikace**: příklad o neodeslání oznámení z back Azure aplikace služby mobilní aplikace, která je integrována rozbočovače oznámení, najdete v článku [Přidání nabízená oznámení na mobilní aplikace](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: například o tom, jak poslat nabízená oznámení pomocí rozhraní REST API postup "pomocí rozbočovače oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Volitelné) Odeslání nabízená oznámení pomocí funkčně .NET konzoly aplikace

V této části pošleme nabízená oznámení pomocí jednoduchého aplikace konzoly .NET. Pro účely v tomto příkladu jsme přepněte do vývojové prostředí Windows, který má Visual Studio už nainstalovaný.

1. Ve Visual Studiu vytvořte novou aplikaci konzoly Visual C#:

    ![Visual Studio: vytvoření nové aplikace konzoly][213]

2. Ve Visual Studiu klikněte na **Nástroje**, klikněte na **Správce balíčků NuGet**a klikněte na **Správce balíčků konzoly**.

    Konzole balíčku Správce by se měly ukotvený k dolní části pracovního prostoru aplikace Visual Studio.

3. V okně Správce balíčků konzoly nastavit **výchozí projektu** nový projekt aplikace konzoly a v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Přidá odkaz na SDK rozbočovače oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification rozbočovače NuGet balíčku</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otevřít `Program.cs` soubor a přidejte následující `using` údajů, zajistit, že můžete používáme Azure tříd a funkcí v rámci hlavní třídy:

        using Microsoft.Azure.NotificationHubs;

3. Ve vaší `Program` třídy, přidejte následující metodu (Nezapomeňte nahradit **připojovacího řetězce** a **Centrální název**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Přidejte následující řádky ve vaší `Main` metodu:

         SendNotificationAsync();
         Console.ReadLine();

5. Stisknutím klávesy F5 ke spuštění aplikace. Vyvolané byste měli vidět nabízená oznámení zobrazí ve vašem zařízení. Jestli používáte Wi-Fi nebo mobilní datové síti, zkontrolujte, jestli aktivní připojení k Internetu v zařízení.

Všechny možné datové části můžete najít v Apple [místní a nabízená oznámení programového průvodce].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Volitelné) Odeslat oznámení z mobilních služeb

V této části pošleme nabízená oznámení pomocí mobilní služby prostřednictvím skriptu uzel.

Odeslání oznámení pomocí mobilních služeb, postupujte podle [Začínáme se službami Mobile]a potom:

1. Přihlaste se k [Portálu Classic Azure]a vyberte mobilních služeb.

2. Vyberte kartu **Plánovač** nahoře.

    ![Azure klasické portálu - Plánovač][215]

3. Vytvoření nové úlohy, vložte název a vyberte **jako služba**.

    ![Azure Classic portál – vytvoření nového projektu][216]

4. Vytvoření projektu, klikněte na název projektu. Klikněte na kartu **skriptu** na horním panelu.

5. Vložte tento skript uvnitř funkce Plánovač. Zkontrolujte, že nahraďte zástupný text názvu centrální oznámení a připojovací řetězec pro *DefaultFullSharedAccessSignature* , který jste získali dříve. Klikněte na **Uložit**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Klikněte na **Spustit** na dolním panelu. Mají dostávat upozornění na vašem zařízení.

##<a name="next-steps"></a>Další kroky

V tomto příkladu jednoduché vysílat nabízených oznámení pro všechny vaše zařízení s iOS. S cílem zacílit určité uživatele, podívejte se do kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele]. Pokud budete chtít segmentech uživatelů tak, že skupin úrok, si můžete přečíst [Rozbočovače použití oznámení o odeslání novinky]. Další informace o používání upozornění rozbočovače [oznámení rozbočovače] pokyny a v [Oznámení rozbočovače postupy pro iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Začínáme se službami Mobile]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure Classic portálu]: https://manage.windowsazure.com/
[Pokyny pro rozbočovače oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Oznámení o rozbočovače postupy pro iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Pomocí oznámení rozbočovače nabízených oznámení pro uživatele]: /manage/services/notification-hubs/notify-users-aspnet
[Odeslání novinky pomocí rozbočovače oznámení]: /manage/services/notification-hubs/breaking-news-dotnet

[Místní a nabízená oznámení programování příručku]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure portálu]: https://portal.azure.com
