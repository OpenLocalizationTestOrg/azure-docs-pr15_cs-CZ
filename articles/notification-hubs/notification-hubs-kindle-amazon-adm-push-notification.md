<properties
    pageTitle="Začínáme s Azure oznámení rozbočovače pro Kindle aplikace | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak používat Azure oznámení rozbočovače odešlete do aplikací pro Kindle nabízená oznámení."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Začínáme s rozbočovače oznámení pro Kindle aplikace

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače odešlete do aplikací pro Kindle nabízená oznámení.
Vytvoříte z prázdné aplikace Kindle, která přijímá nabízená oznámení pomocí zasílání zpráv zařízení Amazon (ADM).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ Získávání Android SDK (jsme se předpokládá, že používáte zatmění) z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webu</a>.
+ Postupujte podle pokynů v tématu <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Nastavení si svůj vývojové prostředí:</a> nastavit vývojové prostředí pro Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Přidání nové aplikace do portálu pro vývojáře

1. Nejprve vytvořte aplikace [portál developer Amazon].

    ![][0]

2. Zkopírujte **klávesu Application**.

    ![][1]

3. Na portálu klikněte na název aplikace a potom klikněte na kartu **Zařízení zasílání zpráv** .

    ![][2]

4. Klikněte na **vytvořit nový profil zabezpečení**a potom vytvořit nový profil zabezpečení (například **TestAdm zabezpečovací profil**). Klepněte na tlačítko **Uložit**.

    ![][3]

5. Klikněte na **Profily zabezpečení** zobrazíte zabezpečovací profil, který jste právě vytvořili. Zkopírujte hodnoty **ID klienta** a **Tajná klienta** pro pozdější použití.

    ![][4]

## <a name="create-an-api-key"></a>Vytvoření rozhraní API klíče

1. Otevřete příkazový řádek s oprávněními správce.
2. Přejděte do složky, Android SDK.
3. Zadejte tento příkaz:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  **Keystore** heslo zadejte **android**.

5.  Zkopírujte otisku **MD5** .
6.  Zpátky v portálu pro vývojáře, na kartě **zasílání zpráv** klikněte na **Android/Kindle** a zadejte název balíčku aplikace (například **com.sample.notificationhubtest**) a hodnotu **MD5** a klikněte na **Generovat klíč rozhraní API**.

## <a name="add-credentials-to-the-hub"></a>Přidání přihlašovací údaje k rozbočovači

Na portálu dodejte tajná klienta a ID klienta kartě **Konfigurovat** rozbočovače oznámení.

## <a name="set-up-your-application"></a>Nastavení aplikace

> [AZURE.NOTE] Když vytváříte aplikace, použijte aspoň rozhraní API úroveň 17.

Přidání knihoven ADM do zatmění projektu:

1. Získání knihovnu ADM [Stáhnout SDK]. Soubor zip SDK extrahujte.
2. V zatmění klikněte pravým tlačítkem myši projektu a potom klikněte na **Vlastnosti**. Vyberte **Java vytvořit cestu** na levé straně a potom na kartu **knihoven **nahoře. Klikněte na **Přidat externí Jar**a vyberte požadovaný soubor `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` z adresáře, do které jste extrahovali Amazon SDK.
3. Stáhněte si Android SDK NotificationHubs (propojení).
4. Rozbalení balíčku a potom přetáhněte soubor `notification-hubs-sdk.jar` do `libs` složky v zatmění.

Úprava manifestu aplikace podporuje ADM:

1. Přidání názvů Amazon v kořenovém prvku seznamu:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Přidáte oprávnění k jako první prvek v prvku seznamu. Nahraďte balíček, který jste použili k vytvoření aplikace pro **[Vaše jméno balíčku]** .

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Vložte následující prvek jako první podřízený prvek aplikace. Nezapomeňte nahraďte název svého ADM procesu zpracování zpráv, kterou vytvoříte v následující části (včetně balíčku) **[Vaše jméno služby]** a **[Vaše jméno balíčku]** nahraďte název balíčku, se kterým jste vytvořili aplikace.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Vytvoření zpracování zpráv ADM

1. Vytvoření nového předmětu, které dědí z `com.amazon.device.messaging.ADMMessageHandlerBase` a nazvěte ji `MyADMMessageHandler`, jak je znázorněno na následujícím obrázku:

    ![][6]

2. Přidejte následující `import` příkazy:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Přidejte následující kód určitého předmětu, který jste vytvořili. Mějte na paměti nahrazovat rozbočovači název a připojovací řetězec (poslech):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Přidat následující kód, který `OnMessage()` metodu:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Přidat následující kód, který `OnRegistered` metodu:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Přidat následující kód, který `OnUnregistered` metodu:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. V `MainActivity` metody, přidejte následující příkaz import:

        import com.amazon.device.messaging.ADM;

8. Přidejte následující kód na konci `OnCreate` metodu:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Přidání klíče rozhraní API do aplikace

1. V zatmění vytvoření nového souboru s názvem **api_key.txt** v adresáři prostředky projektu.
2. Otevřete soubor a zkopírujte klávesu rozhraní API, které vygenerovalo v portálu pro vývojáře Amazon.

## <a name="run-the-app"></a>Spusťte aplikaci

1. Spusťte emulátor.
2. V emulátoru potáhněte prstem od horního a klikněte na **Nastavení**a potom klikněte na **Můj účet** a zaregistrovat pod svým účtem platné Amazon.
3. V zatmění spusťte aplikaci.

> [AZURE.NOTE] Pokud dojde k potížím, zkontrolujte čas emulátoru (nebo zařízení). Požadovanou časovou hodnotu musí být přesné. Změna doby emulátoru Kindle, můžete z adresáře platformy nástroje Android SDK spusťte tento příkaz:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Odeslání zprávy

Odeslání zprávy pomocí .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Portál pro vývojáře Amazon]: https://developer.amazon.com/home.html
[Stáhnout sadu SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
