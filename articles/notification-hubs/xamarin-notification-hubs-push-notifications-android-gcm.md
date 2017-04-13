<properties
    pageTitle="Začínáme s rozbočovače oznámení pro aplikace Xamarin.Android | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak používat Azure oznámení rozbočovače odešlete aplikace Xamarin Android nabízená oznámení."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Začínáme s rozbočovače oznámení s Xamarin pro Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače odešlete aplikace Xamarin.Android nabízená oznámení.
Vytvoříte prázdné Xamarin.Android aplikace, která přijímá nabízená oznámení pomocí zasílání zpráv Cloud Google (GCM). Až budete hotovi, budete moct používat rozbočovače oznámení vysílání nabízená oznámení na zařízeních s aplikací. Dokončení kód je dostupná v [NotificationHubs app] [ GitHub] vzorku.

Tento kurz ukazuje jednoduchý scénář vysílání při používání rozbočovače oznámení.


## <a name="before-you-begin"></a>Než začnete

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód pro tento kurz se nachází na GitHub [tady](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ Na [Nastavení a instalace for Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx)je Visual Studio s Xamarin v systému Windows nebo Xamarin Studio v Mac OS X. úplné pokyny k instalaci.
+ Aktivní účet Google
+ [Zasílání zpráv součásti Azure]
+ [Součást klienta Messaging Google Cloud]

Dokončení tohoto kurzu je požadována pro všechny ostatní oznámení rozbočovače výukové programy pro Xamarin.Android aplikace.

> [AZURE.IMPORTANT] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Povolení Cloud Google zasílání zpráv

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Konfigurace rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Klikněte na kartu <b>Konfigurovat</b> nahoře, zadejte hodnotu <b>Klíč rozhraní API</b> , kterou jste získali v předchozí části a klikněte na tlačítko <b>Uložit</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Vaše centrum oznámení je nyní nakonfigurován pro práci s GCM a máte řetězce připojení k obojímu zaregistrovat aplikace pro příjem oznámení ve formě a odešlete nabízená oznámení.

##<a name="connect-your-app-to-the-notification-hub"></a>Připojení aplikace k centru oznámení

###<a name="create-a-new-project"></a>Vytvoření nového projektu

1. V Xamarin Studio klikněte na **Nové řešení**a **Aplikace v Androidu**klepněte na tlačítko **Další**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Zadejte **Název aplikace** a **identifikátor URI**. Klikněte na **Cílovém Plaforms** chcete podporu a potom klikněte na **Další** a **vytvořit**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Tím vytvoříte nový projekt Android.

2. Otevřete dialogové okno Vlastnosti projektu tak, že kliknete pravým tlačítkem myši nového projektu v zobrazení řešení a kliknutím na **Možnosti**. Vyberte položku **Android aplikace** v části **vytvořit** .

    Ujistěte se, že první písmeno **názvu balíčku** malá písmena.

    > [AZURE.IMPORTANT] První písmeno názvu balíčku musí být malá písmena. V opačném aplikace seznamu chybové zprávy při registraci **BroadcastReceiver** a **IntentFilter** nabízených oznámení dole.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Volitelně můžete nastavte na **Android minimální verze** na jinou rozhraní API úroveň.

4. Volitelně můžete nastavit **cíl Android verze** na jinou verzi rozhraní API, který chcete obrázku (musí být rozhraní API úrovni 8 nebo vyšší).

Klikněte na tlačítko **OK** a zavřete dialogové okno Možnosti aplikace Project.


###<a name="add-the-required-components-to-your-project"></a>Přidejte požadované součásti do projektu

Google Cloud zpráv klientovi dostupné úložiště součástí Xamarin zjednodušuje podpora nabízených oznámení v Xamarin.Android.

1. Klikněte pravým tlačítkem na složku součásti aplikace Xamarin.Android a zvolte **Získat další součásti**.

2. Vyhledejte komponentu **Azure zasílání zpráv** a přidejte do projektu.

3. Vyhledejte komponentu **Google Cloud zasílání zpráv** a přidejte do projektu.


###<a name="set-up-notification-hubs-in-your-project"></a>Nastavení oznámení rozbočovače v projektu

1. Shromážděte pro Android centrální aplikace a oznámení o následující informace:

    - **GoogleProjectNumber**: získání tuto hodnotu čísla projektu z základní informace o aplikaci na portálu pro vývojáře Google. Poznámku tuto hodnotu dříve volání, když jste aplikaci vytvořili na portálu.
    - **Poslech připojovací řetězec**: na řídicí panel na [Klasické portál Azure]klikněte na **zobrazení připojovací řetězec**. Zkopírujte připojovací řetězec *DefaultListenSharedAccessSignature* pro tuto hodnotu.
    - **Centrální název**: to je název rozbočovače z [Azure klasické portálu]. Například *mynotificationhub2*.

    Vytvořte třída **Constants.cs** Xamarin projektu a určete následující konstanty v předmětu. Nahraďte zástupný text požadované hodnoty.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Přidejte následující pomocí příkazů na **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Přidání proměnné instance k `MainActivity` třídy, která se použije k zobrazení upozornění dialogu při spuštění aplikace:

        public static MainActivity instance;


3. V předmětu **MainActivity** vytvořte následujícím způsobem:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. V `OnCreate` inicializace metoda **MainActivity.cs** `instance` proměnnou a přidejte volání `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Vytvoření nové třídy **MyBroadcastReceiver**.

    > [AZURE.NOTE] Bude projdeme vytvoření třídy **BroadcastReceiver** od začátku dole. Rychlé alternativy ručně vytváření **MyBroadcastReceiver.cs** je však v nápovědě k souboru **GcmService.cs** součástí projektu Xamarin.Android vzorku zahrnutý v sadě [NotificationHubs vzorky][GitHub]. Duplikování **GcmService.cs** a změny názvy tříd může být skvělé místo, kde začít stejně.

5. Přidejte následující pomocí příkazů na **MyBroadcastReceiver.cs** (odkazující na součástí a montáží, který jste přidali výše):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. V **MyBroadcastReceiver.cs**přidejte následující požadavky na oprávnění mezi příkazy **pomocí** a deklaraci **názvů** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. V **MyBroadcastReceiver.cs**změňte třídě **MyBroadcastReceiver** podle těchto věcí:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Přidání jiné třídy v **MyBroadcastReceiver.cs** s názvem **PushHandlerService**, která je odvozena z **GcmServiceBase**. Zkontrolujte, že vyrovnat předmětu atribut **služby** :

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** používá metody **OnRegistered()** **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**a **OnError()**. Náš třídy implementaci **PushHandlerService** musí přepsat těchto postupů a těchto postupů bude platit odpověď interakce s centru oznámení.


9. Přepište metodu **OnRegistered()** v **PushHandlerService** tento kód:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] V kódu **OnRegistered()** výše je třeba si uvědomit možnost určit značky k registraci pro konkrétní zpráv kanály.

10. Přepište metodu **OnMessage** v **PushHandlerService** tento kód:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Přidání metody následující **createNotification** a **dialogNotify** **PushHandlerService** pro upozornění uživatelů po přijetí upozornění.

    >[AZURE.NOTE] Oznámení o návrháři v Android verze 5.0 nebo novější představuje významné odeslání z předchozích verzí. Pokud jste to vyzkoušet na Android 5.0 nebo novější, bude nutné aplikaci běžet pro příjem oznámení. Další informace najdete v tématu [Android oznámení](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Přepište abstraktní členy **OnUnRegistered()** **OnRecoverableError()**a **OnError()** tak, aby kompilaci kódu:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Spuštění aplikace v emulátor

Při spuštění této aplikace v emulátor, ujistěte se, použijte Android virtuální zařízení (AVD), který podporuje rozhraní API Google.

> [AZURE.IMPORTANT] Pro příjem nabízených oznámení, musíte nastavit účet Google na zařízení s Androidem virtuální. (V emulátoru, přejděte na **Nastavení** a klikněte na **Přidat účet**) Nezapomeňte, že emulátor připojení k Internetu.

>[AZURE.NOTE] Oznámení o návrháři v Android verze 5.0 nebo novější představuje významné odeslání z předchozích verzí. Další informace najdete v tématu [Androidem oznámení](http://go.microsoft.com/fwlink/?LinkId=615880).


1. Z **Nástroje**klikněte na tlačítko **Otevřít Android emulátoru správce**, vyberte své zařízení a klikněte na tlačítko **Upravit**.

    ![][18]

2. Vyberte **Google rozhraní API** v **cílové**a pak klikněte na **OK**.

    ![][19]

3. Na horním panelu nástrojů klikněte na **Spustit**a vyberte aplikace. Spustí emulátor a spustí aplikace.

  Aplikace načte *atributy registrationId* z GCM a registruje centru oznámení.

##<a name="send-notifications-from-your-backend"></a>Odeslat oznámení z vaší back-end


Můžete otestovat, jak je vidět na následující obrazovce zobrazovat oznámení v aplikaci tak, že oznámení v [Azure klasické portálu] prostřednictvím kartě ladění v centru oznámení.

![][30]


Nabízená oznámení jsou obvykle odesílány back-end služby jako je třeba mobilní služby nebo ASP.NET prostřednictvím kompatibilní knihovny. Můžete taky rozhraní REST API přímo k odesílání zpráv oznámení, pokud není k dispozici pro vaše back-end knihovny.

Tady je seznam některé kurzy, které budete chtít zkontrolovat pro zasílání oznámení ve formě:

- Technologie ASP.NET: Najdete v článku [Použití oznámení rozbočovače nabízených oznámení pro uživatele].
- Azure Java rozbočovače oznámení SDK: vás [naučí používat rozbočovače oznámení z Java](notification-hubs-java-push-notification-tutorial.md) pro odesílání oznámení z Java. To otestování v zatmění pro Android vývoj.
- PHP: Vás [naučí používat rozbočovače oznámení z PHP](notification-hubs-php-push-notification-tutorial.md).


V dalším pododdílu kurzu odeslat oznámení pomocí aplikace konzoly .NET a pomocí mobilní služby prostřednictvím skriptu uzlů.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Volitelné) Odeslání oznámení pomocí aplikace .NET

V této části pošleme oznámení pomocí aplikace konzoly .NET

1. Vytvoření nové aplikace konzoly Visual C#:

    ![][20]

2. Ve Visual Studiu klikněte na **Nástroje**, klikněte na **Správce balíčků NuGet**a klikněte na **Správce balíčků konzoly**.

    Zobrazí se konzole Správce balíčků ve Visual Studiu.

3. V okně Správce balíčků konzoly nastavit **výchozí projektu** nový projekt aplikace konzoly a v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Přidá odkaz na SDK rozbočovače oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification rozbočovače NuGet balíčku</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Otevřete soubor Program.cs a přidejte následující `using` údajů:

        using Microsoft.Azure.NotificationHubs;

5. Ve vaší `Program` třídy, přidejte následujícího postupu. Aktualizujte zástupného textu *DefaultFullSharedAccessSignature* připojení řetězce a centrální názvu z [Azure klasického portálu].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Přidejte následující řádky v způsobu **hlavní** :

         SendNotificationAsync();
         Console.ReadLine();

7. Stisknutím klávesy F5 ke spuštění aplikace. V aplikaci mají dostávat oznámení.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Volitelné) Odeslání oznámení pomocí mobilní služby

1. Postupujte podle [Začínáme se službami Mobile].

1. Přihlaste se k [Portálu klasické Azure]a vyberte mobilních služeb.

2. Vyberte kartu **Plánovač** nahoře.

    ![][22]

3. Vytvoření nové úlohy, vložte název a vyberte **jako služba**.

    ![][23]

4. Vytvoření projektu, klikněte na název projektu. Klikněte na kartu **skriptu** na horním panelu.

5. Vložte tento skript uvnitř funkce Plánovač. Zkontrolujte, že nahraďte zástupný text názvu centrální oznámení a připojovací řetězec pro *DefaultFullSharedAccessSignature* , který jste získali dříve. Klikněte na **Uložit**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klikněte na **Spustit** na dolním panelu. Dostáváte informačního oznámení.

##<a name="next-steps"></a>Další kroky

V tomto příkladu jednoduché vysílat oznámení pro všechny vaše zařízení s Androidem. S cílem zacílit určité uživatele, podívejte se do kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele]. Pokud budete chtít segmentech uživatelů tak, že skupin úrok, si můžete přečíst [Rozbočovače použití oznámení o odeslání novinky]. Další informace o používání upozornění rozbočovače [oznámení rozbočovače] pokyny a v [Oznámení rozbočovače postupy pro Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Začínáme s mobilní služby]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure klasického portálu]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Pokyny pro rozbočovače oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Oznámení o rozbočovače postupy pro Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Pomocí oznámení rozbočovače nabízených oznámení pro uživatele]: /manage/services/notification-hubs/notify-users-aspnet
[Odeslání novinky pomocí rozbočovače oznámení]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Součást klienta Messaging Google Cloud]: http://components.xamarin.com/view/GCMClient/
[Zasílání zpráv součásti Azure]: http://components.xamarin.com/view/azure-messaging
