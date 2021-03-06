<properties
    pageTitle="Odesílání nabízených oznámení pro Android s Azure oznámení rozbočovače | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak pomocí Azure oznámení rozbočovače nabízená oznámení na zařízeních s Androidem."
    services="notification-hubs"
    documentationCenter="android"
    keywords="nabízená oznámení, nabízená oznámení, android nabízená oznámení"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Odesílání nabízených oznámení pro Android s rozbočovače oznámení Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

> [AZURE.IMPORTANT] Toto téma ukazuje nabízená oznámení s zpráv Cloud Google (GCM). Pokud používáte společnosti Google Firebase cloudu zasílání zpráv (FCM), přečtěte si článek [odeslání nabízených oznámení pro Android s Azure oznámení rozbočovače a FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače nabízená oznámení odešlete aplikace Android.
Vytvoříte prázdné Android aplikace, která přijímá nabízená oznámení pomocí zasílání zpráv Cloud Google (GCM).

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód pro účely tohoto návodu si můžete stáhnout z GitHub [tady](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Zjistit předpoklady pro

> [AZURE.IMPORTANT] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Kromě Azure active účet výše uvedené tento kurz vyžaduje jenom nejnovější verzi [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Dokončení tohoto kurzu je požadována pro všechny ostatní oznámení rozbočovače výukové programy pro Android aplikace.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Vytvoření projektu, který podporuje zasílání zpráv Cloud Google

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Konfigurace rozbočovači nového oznámení


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. v zásuvné **Nastavení** vyberte **Oznámení služby** a **Google (GCM)**. Zadejte klávesu rozhraní API a klikněte na **Uložit**.

&emsp;&emsp;![Azure oznámení rozbočovače – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Vaše centrum oznámení je nyní nakonfigurován pro práci s GCM a máte řetězce připojení k obojímu zaregistrovat aplikace přijímání a odesílání nabízená oznámení.

##<a id="connecting-app"></a>Připojení aplikace k centru oznámení

###<a name="create-a-new-android-project"></a>Vytvoření nového projektu se systémem Android

1. V Android Studio zahájení nového projektu Android Studio.

    ![Android Studio - nového projektu][13]

2. Zvolte koeficient z **telefonu a tabletu** a **Minimální SDK** , která chcete podporu. Klepněte na tlačítko **Další**.

    ![Android Studio – postup vytvoření projektu][14]

3. Zvolte **Prázdné aktivity** hlavní aktivity, klikněte na tlačítko **Další**a pak klikněte na **Dokončit**.

###<a name="add-google-play-services-to-the-project"></a>Přidání služby Google Play do projektu

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Přidání knihoven rozbočovače oznámení Azure


1. V `Build.Gradle` souboru pro **aplikaci**, přidejte následující řádky v části **závislosti** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Přidejte následující úložiště pod oddílem **závislosti** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Aktualizace AndroidManifest.xml.


1. K podpoře GCM, jsme v naší kód, který se používá k [získání registrace tokeny](https://developers.google.com/cloud-messaging/android/client#sample-register) pomocí [Rozhraní API ID Instance společnosti Google](https://developers.google.com/instance-id/)implementovat službu posluchače Instance ID. V tomto kurzu budeme název třídy `MyInstanceIDService`. 
 
    Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř `<application>` značku. Nahrazení `<your package>` zástupný symbol vaše jméno skutečné balíčku filtr v horní části `AndroidManifest.xml` soubor.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Jakmile obdrželi jsme naše GCM registrační token z rozhraní API ID Instance, použijeme k [registraci centru Azure oznámení](notification-hubs-push-notification-registration-management.md). Bude podporujeme tuto registraci do pozadí pomocí `IntentService` s názvem `RegistrationIntentService`. Tuto službu budou povinnost [aktualizace náš GCM registrační token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
 
    Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř `<application>` značku. Nahrazení `<your package>` zástupný symbol vaše jméno skutečné balíčku filtr v horní části `AndroidManifest.xml` soubor. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. Také zadá sluchátko pro příjem oznámení ve formě. Přidejte následující definici sluchátko k souboru AndroidManifest.xml uvnitř `<application>` značku. Nahrazení `<your package>` zástupný symbol vaše jméno skutečné balíčku filtr v horní části `AndroidManifest.xml` soubor.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Přidejte následující potřebné GCM týkající se oprávnění dole `</application>` značku. Ujistěte se, pokud chcete nahradit `<your package>` s názvem balíčku filtr v horní části `AndroidManifest.xml` soubor.

    Další informace o oprávněních najdete v tématu [Nastavení GCM klientské aplikace pro Android](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Přidání kódu


1. V části zobrazení projektu rozbalte **aplikace** > **src** > **hlavní** > **java**. Klikněte pravým tlačítkem myši do balíčku složky ve skupinovém rámečku **java**, klikněte na **Nový**a klikněte na **Tříd jazyka Java**. Přidání nového třídy s názvem `NotificationSettings`. 

    ![Android Studio – nové tříd jazyka Java][6]

    Aktualizujte tyto tři zástupné symboly v následující kód pro `NotificationSettings` třídy:
    * **Identifikátor SenderId**: číslo projektu jste získali dříve v [Konzole Cloud Google](http://cloud.google.com/console).
    * **HubListenConnectionString**: **DefaultListenAccessSignature** připojovací řetězec pro vaše centrum. Zkopírujte tento připojovací řetězec kliknutím **Zásady přístupu** na zásuvné **Nastavení** rozbočovače na [Portál Azure].
    * **HubName**: použijte název rozbočovač oznámení, která se zobrazí v centrální zásuvné [Azure portálu].

    `NotificationSettings`kód:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Podle kroků nahoře, přidat další nové třídu s názvem `MyInstanceIDService`. To bude naše implementaci Instance ID posluchače služby.

    Kód pro tento třídy vyzvedne naše `IntentService` [GCM token aktualizace](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) na pozadí.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Přidání jiného nové třídy projektu s názvem, `RegistrationIntentService`. To bude implementaci pro naše `IntentService` , zpracuje [aktualizace GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [zaregistrovali centru oznámení](notification-hubs-push-notification-registration-management.md).

    Použijte následující kód pro tento předmětu.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. Ve vaší `MainActivity` třídy, přidejte následující `import` příkazů nad deklarací třídy.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Přidejte následující soukromých členů v horní části předmětu. Použijeme tyto [Kontrola dostupnosti služby Google Play doporučené Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Ve vaší `MainActivity` třídy, přidejte následující metodu dostupnosti z Google Play Services. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. Ve vaší `MainActivity` třídy, přidat následující kód, který bude vyhledávat Google Play Services před voláním vaše `IntentService` získat registrace GCM token a zaregistrovat se vaše Centrum oznámení.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. V `OnCreate` metodu `MainActivity` třídy, přidat následující kód pro zahájení procesu registrace po aktivity.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Přidejte tyto další metody `MainActivity` k ověření aplikace stavu a sestavy stavu aplikace.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. `ToastNotify` Používá na *"Ahoj světe"* `TextView` zprávy o stavu a oznámení trvale v aplikaci ovládacího prvku. V rozložení activity_main.xml přidejte následující id pro ovládací prvek.

        android:id="@+id/text_hello"

11. Další přidáme podtřídou pro naše sluchátko, která byla definována v AndroidManifest.xml. Přidání jiného nové třídy do projektu s názvem `MyHandler`.

12. Přidejte následující příkazy importu v horní části `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Přidejte následující kód pro `MyHandler` předmětu a podtřídu `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Tento kód přepíše `OnReceive` metody, takže obslužné ohlásí oznámení, které jsou doručeny. Obslužná rutina také odesílá nabízená oznámení správce Android oznámení s použitím `sendNotification()` metody. `sendNotification()` Metodu by měl provést, když neběží v aplikaci a doručení.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Android Studio na řádku nabídek klikněte na **vytvořit** > **Znovu vytvořit projekt** a ujistěte se, zda jsou k dispozici v kódu chyby.

##<a name="sending-push-notifications"></a>Odesílání nabízená oznámení

Můžete otestovat, jak je ukázáno v následujícím příkladu příjem nabízených oznámení v aplikaci odesláním prostřednictvím [Portálu Azure] – hledejte části **Poradce při potížích** v centrální zásuvné.

![Odeslání Azure oznámení rozbočovače - Test](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Volitelné) Odeslání nabízená oznámení přímo v aplikaci

Za normálních okolností by odeslat oznámení pomocí back-end serveru. U některých případech můžete odeslat nabízená oznámení přímo z aplikace klienta. Tento oddíl vysvětluje, jak o neodeslání oznámení z klienta pomocí [Azure oznámení centrální REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. V zobrazení projektu Android Studio rozbalte **aplikace** > **src** > **hlavní** > **rozlišení** > **rozložení**. Otevřít `activity_main.xml` soubor rozložení a klikněte na **textu** tab na aktualizaci obsahu textový soubor. Aktualizovat s kódem níže, která přidá nový `Button` a `EditText` ovládací prvky pro odesílání nabízená oznámení k rozbočovači oznámení. Přidat tento kód dole, těsně před `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. V zobrazení projektu Android Studio rozbalte **aplikace** > **src** > **hlavní** > **rozlišení** > **hodnoty**. Otevřít `strings.xml` soubor a přidat řetězcové hodnoty, které odkazuje nové `Button` a `EditText` ovládací prvky. Přidání na konci souboru, těsně před `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Ve vaší `NotificationSetting.java` souboru, přidejte následující nastavení `NotificationSettings` předmětu.

    Aktualizace `HubFullAccess` s **DefaultFullSharedAccessSignature** připojovací řetězec pro vaše centrum. Tento připojovací řetězec můžete zkopírovali z [Portálu Azure] kliknutím **Zásady přístupu** na zásuvné **Nastavení** pro vaše Centrum oznámení.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. Ve vaší `MainActivity.java` soubor, přidejte následující `import` příkazy výše uvedených `MainActivity` předmětu.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. V vaše `MainActivity.java` souboru, přidejte následující členů v horní části `MainActivity` předmětu.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Je třeba vytvořit token Software přístup podpisu (přidružení zabezpečení) ověření žádost příspěvek odešlete vaše oznámení Centrum zpráv. Důvodem je analýzu klíčových data z připojovací řetězec a pak vytvořením token přidružení zabezpečení, jak je uvedeno v rozhraní REST API odkaz [Běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx) . Následující kód je příklad implementace.

    V `MainActivity.java`, přidat metodu `MainActivity` třídy analyzovat připojovací řetězec.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. V `MainActivity.java`, přidat metodu `MainActivity` třídy k vytvoření přidružení zabezpečení ověřovací token.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. V `MainActivity.java`, přidat metodu `MainActivity` třídy ke zpracování klikněte na tlačítko **Odeslat oznámení** a do zprávy nabízená oznámení o Centru pomocí předdefinovaných rozhraní REST API.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Testování aplikace

####<a name="push-notifications-in-the-emulator"></a>Nabízená oznámení v emulátor

Pokud chcete otestovat nabízená oznámení uvnitř emulátoru, ujistěte se, že obrázek emulátoru podporuje rozhraní API Google úroveň, kterou jste zvolili aplikace. Pokud obrázek, které nejsou podporovány nativní rozhraní API Google, můžete se nakonec znamenat **služby\_není\_dostupné** výjimku.

Kromě výše uvedené, ujistěte se, že přidáte svůj účet Google pracovního emulátoru klikněte v části **Nastavení** > **účty**. V opačném pokusu o registraci GCM, můžete mít **ověřování\_VADNÝ** výjimku.

####<a name="running-the-application"></a>Spuštění aplikace

1. Spusťte aplikaci a Všimněte si, že je ID registrace vykázaného za úspěšné registraci.

    ![Testování na Android – registrace kanálu][18]

2. Zadejte zprávy s upozorněním na všech zařízeních s Androidem, které mají registrovaným v centru odeslána.

    ![Testování na Android – odesláním každé zprávy][19]

3. Stisknutím klávesy **Odeslat oznámení**. Se zobrazí všechna zařízení aplikaci spuštěna `AlertDialog` instance zprávou nabízená oznámení. Zařízení, které nemají aplikaci spuštěná, ale byly dříve registrované pro nabízená oznámení, dostanou oznámení v okně Správce oznámení Android. Můžou být je možné zobrazit potažením prstem dolů v levém horním rohu.

    ![Testování na Android – oznámení][21]

##<a name="next-steps"></a>Další kroky

Doporučujeme kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele] jako dalším krokem. To vám ukáže, jak o neodeslání oznámení z ASP.NET back-end pomocí značek jej směrovat určitým uživatelům.

Pokud budete chtít segmentech uživatelů tak, že skupin zájem, podívejte se na [Použití oznámení rozbočovače odeslat novinky] kurz.

Další obecné informace o rozbočovače oznámení, najdete v článku naše [Oznámení rozbočovače pokyny].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Pokyny pro rozbočovače oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Pomocí oznámení rozbočovače nabízených oznámení pro uživatele]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Odeslání novinky pomocí rozbočovače oznámení]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure portálu]: https://portal.azure.com
