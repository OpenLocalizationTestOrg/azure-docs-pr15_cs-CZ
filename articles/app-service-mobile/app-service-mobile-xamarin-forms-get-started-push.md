<properties
    pageTitle="Přidat nabízená oznámení pro aplikace Xamarin.Forms | Microsoft Azure"
    description="Informace o použití služby Azure odeslat více nabízená oznámení ke svým aplikacím Xamarin.Forms."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Přidání aplikace Xamarin.Forms nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Základní informace

V tomto kurzu přidáte nabízených, které oznámení pro všechny projekty výsledkem [Xamarin.Forms rychlý start](app-service-mobile-xamarin-forms-get-started.md) tak, aby nabízená oznámení se neodesílají všechny klienty a platformy pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Pro iOS, budete potřebovat [Program vývojář Apple členství](https://developer.apple.com/programs/ios/) a zařízení fyzicky iOS protože [iOS simulator nepodporuje nabízená oznámení](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Konfigurace rozbočovači oznámení

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizace project serveru odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Volitelné) Konfiguraci a spuštění Android projektu

Dokončení této části můžete povolit nabízených oznámení pro projekt Xamarin.Forms Droid pro Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Povolení Firebase cloudu zasílání zpráv (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Konfigurace k odeslání žádosti o nabízených pomocí FCM back-end mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Přidání nabízená oznámení Android projektu

S back-end nakonfigurována FCM jsme můžete přidat součásti kódy ke klientovi pro registraci s FCM, zaregistrujte nabízená oznámení s Azure oznámení rozbočovače prostřednictvím mobilní aplikace back-end a zasílat oznámení ve formě.

1. V projektu **Droid** klikněte pravým tlačítkem na složku **součásti** , klikněte na **Získat víc součástí...**, vyhledejte komponentu **Google Cloud zasílání zpráv** a přidejte do projektu. Tato součást podporuje nabízených oznámení pro Xamarin Android projektu.


2. Otevření souboru projektu MainActivity.cs a přidejte následující pomocí příkazu v horní části souboru:

        using Gcm.Client;

3. Přidejte následující kód metody **OnCreate** po volání **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Přidejte nová metoda helper **CreateAndShowDialog** takto:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Přidejte následující kód třídy **MainActivity** :

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    To zpřístupňuje aktuální instance **MainActivity** proto lze provést v hlavním uživatelském rozhraní podprocesu.

6. Inicializace `instance`, proměnné na začátku metodu **OnCreate** takto.

        // Set the current instance of MainActivity.
        instance = this;

2. Přidání nového souboru třídy **Droid** projektu s názvem `GcmService.cs`a zkontrolujte, že následující příkazy **pomocí** jsou prezentovat v horní části souboru:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Přidejte následující požadavky na oprávnění v horní části souboru, po **pomocí** příkazů a před deklaraci **názvů** .

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Přidejte následující definici třídy na obor. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Nahraďte **< PROJECT_NUMBER >** číslo svého projektu, které bylo uvedeno dříve.   

11. Nahraďte prázdné třídy **GcmService** následující kód, který používá nové vysílání příjemce:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Přidejte následující kód **GcmService** třídy, která přepsala obslužné rutiny události **OnRegistered** a používá způsob **zaregistrovat** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Přidejte následující kód implementující **OnMessage**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Úchyty pro upozornění na příchozí a jejich odeslání vedoucímu oznámení zobrazovat.

14. **GcmServiceBase** také vyžaduje implementovat **OnUnRegistered** a **PřiChybě** rutiny metody, které můžete udělat takto:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Teď jste připravení test nabízená oznámení v seznamu aplikací na zařízení s Androidem emulátor.

###<a name="test-push-notifications-in-your-android-app"></a>Test nabízená oznámení v aplikace pro Android

První dva kroky jsou požadovány pouze při testování emulátoru.

1. Ujistěte se, že při nasazení nebo ladění na virtuální zařízení, které má rozhraní API Google nastavit jako cíle, jak je znázorněno níže ve Správci zařízení Android virtuální (AVD).

2. Přidejte účet Google do zařízení s Androidem kliknutím **aplikace** > **Nastavení** > **Přidat účet**a potom sledovat pomocí pokynů přidejte stávající účet Google do zařízení s vytvořte nový účet.

1. Ve Visual Studiu nebo Xamarin Studio klikněte pravým tlačítkem myši na **Droid** projekt a klikněte na **nastavit jako projekt při spuštění**.

2. Tlačítko **Spustit** sestavovat projektu a spusťte aplikaci na zařízení s Androidem nebo emulátoru.

3. V aplikaci zadejte úkolu a potom klepněte na znaménko plus (**+**) ikonu.

4. Ověřte, že doručení po přidání se položky.


##<a name="optional-configure-and-run-the-ios-project"></a>(Volitelné) Konfiguraci a spuštění projektu iOS

Tato část je určená pro systém iOS projektu Xamarin pro zařízení s iOS. Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Konfigurace centra oznámení pro APNS

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Pak nakonfigurujete nastavení iOS projektu v Xamarin Studio nebo Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Přidání nabízených oznámení pro aplikace pro iOS

1. Otevřete AppDelegate.cs v **iOS** projektu, přidejte tento příkaz **pomocí** do horní části souboru kódu.

        using Newtonsoft.Json.Linq;

4. Ve třídě **AppDelegate** přidejte přepsání pro událost **RegisteredForRemoteNotifications** zaregistrovat k oznámení:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. V **AppDelegate**také přidáte následující přepsání obslužné rutiny události **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Tento způsob zpracování upozornění na příchozí je spuštěná aplikace.

2. Ve třídě **AppDelegate** přidáte následující kód metody **FinishedLaunching** : 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Díky Podpora vzdálené oznámení a žádosti o nabízená registrace.

Aplikace je teď aktualizovat, aby podporovaly nabízená oznámení.

####<a name="test-push-notifications-in-your-ios-app"></a>Test nabízená oznámení v aplikace pro iOS

1. Klikněte pravým tlačítkem myši na iOS projekt a klikněte na **nastavit jako StartPp projekt**.

2. Stiskněte tlačítko **Spustit** nebo **F5** ve Visual Studiu vytvořte projekt a spusťte aplikaci v zařízení s iOS a potom klikněte na **OK** přijmete nabízená oznámení.

    > [AZURE.NOTE] U aplikace musí explicitně přijmete nabízená oznámení. Tento požadavek vyskytuje při prvním spuštění aplikace.

3. V aplikaci zadejte úkolu a potom klepněte na znaménko plus (**+**) ikonu.

4. Ověřte, že je dostali oznámení a potom klikněte na **OK** zavřete oznámení.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Volitelné) Konfigurace a spuštění Windows projekty

Tento oddíl je pro spuštění Xamarin.Forms WinApp a WinPhone81 projektů pro zařízení s Windows. Tento postup také podporují projekty univerzální platformu systému Windows (UWP). Pokud nepracujete s zařízení s Windows, můžete tuto část přeskočit.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrace aplikace Windows nabízených oznámení s WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Konfigurace centra oznámení pro WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Přidání nabízených oznámení pro aplikace pro Windows

1. Ve Visual Studiu otevřete **App.xaml.cs** v systému Windows projektu a přidejte následující příkazy **pomocí** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Nahrazení `<your_TodoItemManager_portable_class_namespace>` s obor názvů přenosná projektu, který obsahuje `TodoItemManager` předmětu.
 

2. V App.xaml.cs přidejte **InitNotificationsAsync** takto: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Tento způsob získá kanál nabízená oznámení a zaregistruje šablonou pro zasílat oznámení ve formě šablony z rozbočovače upozornění. Do tohoto klienta budete dostávat oznámení šablonu, která podporuje *messageParam* .

3. V App.xaml.cs, definici **OnLaunched** obslužné rutiny události metody můžete aktualizovat přidáním `async` modifikátor, přidat následující kód na konci metodu: 

        await InitNotificationsAsync();

    Zajistíte tím, že registrace nabízená oznámení vytvořila se nebo aktualizovat při každém spuštění aplikace. Je důležité tento postup zaručit kanálu nabízených WNS je vždy aktivní.  

4. V Průzkumníku for Visual Studio otevřete soubor **Package.appxmanifest** a **Uvidí může** na hodnotu **Ano** v části **oznámení**.

5. Vytvářejte aplikace a ověřte, že máte bez chyb.  Klient aplikace by nyní zaregistrujte šablony sdělení back-end mobilní aplikaci. Opakujte tento oddíl pro každý Windows projekt ve vašem řešení.


####<a name="test-push-notifications-in-your-windows-app"></a>Test nabízená oznámení v aplikaci Windows

1. Ve Visual Studiu klikněte pravým tlačítkem myši projektu Windows a klikněte na **nastavit jako po spuštění projektu**.

2. Tlačítko **Spustit** sestavovat projektu a spusťte aplikaci.

3. V aplikaci, zadejte název nového todoitem a potom klepněte na znaménko plus (**+**) ho přidejte na ikonu.

4. Ověřte, že doručení po přidání se položky.

##<a name="next-steps"></a>Další kroky

Další informace o nabízená oznámení:

* [Diagnostika problémů s nabízená oznámení](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Existuje různých důvodů, proč oznámení mohou získat textu nebo nekončí na zařízeních. Toto téma ukazuje, jak analyzovat a zjistit příčinou selhání nabízená oznámení. 

Zvažte, pokračovat k jedné z následujících výukové programy pro:

* [Přidání ověřování aplikace](app-service-mobile-xamarin-forms-get-started-users.md)  
Informace o ověřování uživatelů aplikace se zprostředkovatelem identit.

* [Povolení offline synchronizace aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

