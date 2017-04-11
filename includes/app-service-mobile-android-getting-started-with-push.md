1. Otevřete soubor v **aplikaci** projektu, `AndroidManifest.xml`. V kódu v dalších dvou krocích nahraďte _`**my_app_package**`_ s názvem balíčku aplikace pro váš projekt, který je hodnota `package` atribut `manifest` značku.

2. Přidejte nová oprávnění za stávající `uses-permission` prvek:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Přidejte následující kód po `application` otevření značku:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Otevřete soubor *ToDoActivity.java*a přidejte do import tento příkaz:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Přidejte následující soukromé proměnné do třídy: nahrazení _`<PROJECT_NUMBER>`_ s projektem číslo přiřazené Google do aplikace v předchozím postupu:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Změňte definici *MobileServiceClient* **soukromé** do **Veřejné statické**, abyste teď vypadá takto:

        public static MobileServiceClient mClient;

7. Další potřebujeme pro přidání nového třídy zpracování oznámení. V Průzkumník projektu otevřete **src** => **hlavní** => **java** uzly a klikněte pravým tlačítkem myši na uzel název balíčku: klikněte na **Nový**a potom klikněte na **Tříd jazyka Java**.

8. V **názvu** typu `MyHandler`, klikněte na tlačítko **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. V souboru MyHandler nahraďte deklaraci třídy s

        public class MyHandler extends NotificationsHandler {


10. Přidejte následující příkazy pro import `MyHandler` třídy:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Dále přidejte tento členům `MyHandler` třídy:

        public static final int NOTIFICATION_ID = 1;


12. V `MyHandler` třídy, přidejte následující kód přepsat metodu **onRegistered** , který registruje zařízení ke službě mobilní centrální oznámení.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. V `MyHandler` třídy, přidejte následující kód přepsat metodu **onReceive** , který způsobuje oznámení zobrazíte dala po přijetí.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Zpátky v souboru TodoActivity.java aktualizované metodu **onCreate** třídy *ToDoActivity* zaregistrovat třídu rutinu oznámení. Zkontrolujte, že po vytvořena *MobileServiceClient* přidat tento kód.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Aplikace je teď aktualizovat, aby podporovaly nabízená oznámení.
