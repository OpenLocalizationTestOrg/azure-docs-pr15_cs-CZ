<properties
    pageTitle="Začínáme s rozbočovače oznámení Azure pomocí Baidu | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak pomocí Azure oznámení rozbočovače nabízená oznámení na zařízeních s Androidem pomocí Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Začínáme s rozbočovače oznámení pomocí Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Baidu cloudu je čínštinu cloudové služby, které můžete použít k odeslání nabízená oznámení na mobilní zařízení. Tato služba je užitečné v Číně, kde poskytly nabízená oznámení Android není složité kvůli přítomnost ukládá jiné aplikace a služby nabízených kromě dostupnost zařízení s Androidem, které obvykle nejste připojení k GCM (Google Cloud zasílání zpráv).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ Android SDK (jsme se předpokládá, že používáte zatmění), kterou si můžete stáhnout z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webu Android</a>
+ [Android SDK mobilní služby]
+ [Android SDK Baidu připínáčku]

>[AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Vytvoření účtu Baidu

Pokud chcete používat Baidu, musíte mít účet Baidu. Pokud ještě nemáte, přihlaste se k [portálu Baidu] a přejděte k dalšímu kroku. V opačném naleznete v pokynech pod o tom, jak vytvořit účet Baidu.  

1. Přejděte na [portál Baidu] a klikněte na odkaz**登录**(**přihlášení**). Klikněte na tlačítko**立即注册**ke spuštění procesu registrace účtu.

    ![][1]

2. Zadejte požadované údaje – telefonní/e-mailovou adresu, heslo a ověření kód – a klikněte na **Registrace**.

    ![][2]

3. Můžete e-mailovou adresu jste zadali s odkazem aktivaci účtu Baidu odeslané e-mailu.

    ![][3]

4. Přihlaste se k e-mailovému účtu, otevřete e-mailu aktivace Baidu a klikněte na aktivační odkaz k aktivaci účtu Baidu.

    ![][4]

Až budete mít účet aktivované Baidu, přihlaste se k [portálu Baidu].

##<a name="register-as-a-baidu-developer"></a>Registrace Baidu vývojář

1. Jakmile jste přihlášení k [portálu Baidu], klikněte na**更多 >>** (**Další**).

    ![][5]

2. Přejděte dolů do části**站长与开发者服务 (pro správce webového serveru a služby Vývojář)** a klikněte na**百度开放云平台**(**Baidu otevřete cloudu platformy**).

    ![][6]

3. Na další stránce klikněte na**开发者服务**(**Developer Services**) v pravém horním rohu.

    ![][7]

4. V nabídce v pravém horním rohu na další stránce, klikněte na**注册开发者**(**Registrované vývojáře**).

    ![][8]

5. Zadejte název, popis a číslo mobilního telefonu pro přijímat textové zprávy ověření a klikněte na**送验证码**(**Odeslat ověřovací kód**). Všimněte si, že pro mezinárodní telefonní čísla, budete muset uzavřete kód země v závorkách. Například pro Spojené státy číslo, bude **(1) 1234567890**.

    ![][9]

6. Mají potom přijímat textové zprávy s počtem ověření, jak je vidět v následujícím příkladu:

    ![][10]

7. Zadejte nové číslo ověření ze zprávy v**验证码**(**potvrzovací kód**).

8. Nakonec dokončete registraci developer přijetí smlouva Baidu a klikněte na možnost**提交**(**Odeslat**). Zobrazí se na následující stránce úspěšné dokončení zápisu:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Vytvoření projektu Baidu cloudu připínáčku

Při vytváření projekt Baidu cloudu nabízených dostanete svoje ID aplikace rozhraní API spolu s tajné klíče.

1. Jakmile jste přihlášení k [portálu Baidu], klikněte na**更多 >>** (**Další**).

    ![][5]

2. Přejděte dolů do části**站长与开发者服务**(**pro správce webového serveru a služby vývojář**) a klikněte na**百度开放云平台**(**Baidu otevřete cloudu platformy**).

    ![][6]

3. Na další stránce klikněte na**开发者服务**(**Developer Services**) v pravém horním rohu.

    ![][7]

4. Na další stránce klikněte na**云推送**(**Cloud nabízená**) v oddílu**云服务**(**Cloudovým službám**).

    ![][12]

5. Jakmile se dostanete registrovaných developer, zobrazí se**管理控制台**(**Konzola pro správu**) v nabídce začátek. Klikněte na tlačítko**开发者服务管理**(**vývojářích služby Správa**).

    ![][13]

6. Na další stránce klikněte na**创建工程**(**Vytvořit projekt**).

    ![][14]

7. Zadejte název aplikace a klikněte na**创建**(**vytvořit**).

    ![][15]

8. Při úspěšné vytvoření projektu nabízených Baidu cloudu zobrazí se na stránku s **ID aplikace**, **Klíč rozhraní API**a **Klíčem tajná**. Poznamenejte si rozhraní API a skrytou klíč, které použijete později.

    ![][16]

9. Konfigurace project nabízených oznámení kliknutím**云推送**(**Cloud nabízených**) v levém podokně.

    ![][31]

10. Na další stránce klikněte na tlačítko**推送设置**(**nabízená nastavení**).

    ![][32]  

11. Na stránce konfigurace přidejte název balíčku, že se bude pomocí Android projektu v poli**应用包名**(**balíčku aplikace**) a klikněte na**保存设置**(**Uložit**).  

    ![][33]

Zobrazí se**保存成功!** (**úspěšně uložené!**) zprávy.

##<a name="configure-your-notification-hub"></a>Konfigurace rozbočovače oznámení

1. Přihlaste se k [Portálu klasické Azure]a potom klikněte na **+ Nový** v dolní části obrazovky.

2. Klikněte na **Aplikaci služby**klikněte na **Bus služby**, klikněte na **Centrum oznámení**a potom klikněte na **Vytvořit**.

3. Zadejte název pro **Centrální oznámení**, vyberte požadovanou **oblast** a **Namespace** kde centrální toto oznámení se vytvoří a potom klikněte na **vytvořit nový rozbočovač oznámení**.  

    ![][17]

4. Klikněte na obor názvů, ve které jste vytvořili rozbočovače oznámení a klikněte na **Oznámení rozbočovače** nahoře.

    ![][18]

5. Vyberte centru oznámení, kterou jste vytvořili a klikněte na **Konfigurovat** z nabídky nejvyšší.

    ![][19]

6. Přejděte dolů do části **baidu nastavení oznámení** a zadejte klíč rozhraní API a tajné klíč, který jste získali v konzole Baidu dříve Baidu cloudu nabízených projektu. Klikněte na **Uložit**.

    ![][20]

7. Klikněte na kartu **řídicího panelu** v horní rozbočovače oznámení a potom klikněte na **Zobrazení připojovací řetězec**.

    ![][21]

8. Poznamenejte si **DefaultListenSharedAccessSignature** a **DefaultFullSharedAccessSignature** v okně **informace o připojení aplikace Access** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Připojení aplikace k centru oznámení

1. V zatmění ADT, vytvoření nového projektu se systémem Android (**soubor** > **Nový** > **Android projekt aplikace**).

    ![][23]

2. Zadejte **Název aplikace** a ujistěte se, že verze **SDK povinné Minimum** je nastavena na **rozhraní API 16: Android 4.1**.

    ![][24]

3. Klikněte na tlačítko **Další** a pokračujte po průvodce, až se zobrazí okno **Vytvořit aktivity** . Ujistěte se, že je zaškrtnuto **Prázdné aktivity** a nakonec zvolte **Dokončit** pro vytvoření nové aplikace Android.

    ![][25]

4. Ujistěte se, že je správně nastavený **Vytvoření cílového projektu** .

    ![][26]

5. Oznámení o rozbočovače 0.4.jar moct soubor stáhněte z na kartu **soubory** [Oznámení-rozbočovače – Android – SDK na Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Přidání souboru do složky **knihoven** zatmění projektu a aktualizujte složce *knihoven* .

6. Stažení a unzip [Baidu nabízená Android SDK]a otevřete složku, do **knihoven** a potom zkopírujte soubor sklenice **pushservice x.y.z** a **armeabi** & **mips** složky ve složce **knihoven** aplikace Android.

7. Otevřete soubor **AndroidManifest.xml** Android projektu a přidejte oprávnění, která jsou vyžadované Baidu SDK.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. **Android: název** vlastnost přidáte do **aplikace** prvek v **AndroidManifest.xml**, nahrazení *yourprojectname* (například **com.example.BaiduTest**). Ujistěte se, že tento název projektu shoduje s některou, které jste nakonfigurovali v konzole Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Přidejte následující konfigurace v rámci prvek aplikace po **. MainActivity** aktivity elementu nahrazení *yourprojectname* (například **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Přidání nového třídy s názvem **ConfigurationSettings.java** do projektu.

    ![][28]

    ![][29]

10. Přidejte do něj následující kód:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Nastavte hodnotu **api_keya** s získána Baidu cloudu projektu dříve, **NotificationHubName** se svým názvem centrální oznámení z portálu Microsoft Azure klasické a **NotificationHubConnectionString** s DefaultListenSharedAccessSignature z portálu klasické Azure.

11. Přidání nového třídy s názvem **DemoApplication.java**a přidejte do něj následující kód:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Přidání jiného nové třídy s názvem **MyPushMessageReceiver.java**a přidejte do něj kód pod. Toto je třída zpracovávající nabízená oznámení, které jsou doručeny ze serveru nabízených Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Otevřete **MainActivity.java**a na požadovanou metodu **onCreate** přidejte následující text:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Otevřete následující příkazy pro import nahoře:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Odeslání oznámení pro aplikace


Můžete rychle otestovat dostávat oznámení ve formě do aplikace tak, že oznámení na [Portál Azure](https://portal.azure.com/) pomocí tlačítka **Testování odeslat** v centru oznámení, jak je vidět na následující obrazovce.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Nabízená oznámení jsou obvykle odesílány back-end služby, jako je mobilní služby nebo ASP.NET pomocí knihovně kompatibilní. Můžete taky rozhraní REST API přímo k odesílání zpráv oznámení, pokud není k dispozici pro back-end knihovny.

V tomto kurzu se v jednoduchosti je krása jsme jenom ukazují testování klientské aplikace tak, že oznámení pomocí .NET SDK pro oznámení rozbočovače aplikaci konzoly místo do back-end služby. Doporučujeme kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) jako další krok pro odeslání oznámení z back-end ASP.NET. Však následujících přístupů lze použít k odeslání oznámení:

* **Rozhraní REST**: podporují oznámení na libovolné back-end platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure oznámení rozbočovače .NET SDK**: V Nuget balíčku správce for Visual Studio spustit [Instalační balíček Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [používání rozbočovače oznámení z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobilní aplikace**: příklad o neodeslání oznámení z back Azure aplikace služby mobilní aplikace, která je integrována rozbočovače oznámení, najdete v článku [Přidání nabízená oznámení na mobilní aplikace](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: příklad toho, jak o neodeslání oznámení pomocí rozhraní REST API postup "pomocí rozbočovače oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Volitelné) Odešlete oznámení pomocí funkčně .NET konzoly aplikace.

V této části ukážeme odesílání oznámení pomocí aplikace konzoly .NET.

1. Vytvoření nové aplikace konzoly Visual C#:

    ![][30]

2. V okně Správce balíčků konzoly nastavit **výchozí projektu** nový projekt aplikace konzoly a v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Přidá odkaz na SDK rozbočovače oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification rozbočovače NuGet balíčku</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Otevřete soubor **Program.cs** a přidejte následující pomocí příkazu:

        using Microsoft.Azure.NotificationHubs;

4. Ve vaší `Program` třídy, přidejte následující metodu a *DefaultFullSharedAccessSignatureSASConnectionString* a *NotificationHubName* nahradit hodnoty, které máte.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Přidejte následující řádky v způsobu **hlavní** :

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Testování aplikace

Testování této aplikace s skutečné telefonní jenom připojte telefon k počítači pomocí kabelu USB. Načte aplikace na připojené telefonu.

Chcete-li otestovat tato aplikace pomocí emulátoru na horním panelu zatmění klikněte na **Spustit**a vyberte aplikace. To spustí emulátoru, načte a spustí aplikace.

Aplikace načte "ID uživatele" a "channelId" ze služby Baidu nabízených oznámení a registruje centru oznámení.

Odeslání oznámení test můžete použít kartu ladění portálu Classic Azure. Pokud integrované aplikace konzoly .NET for Visual Studio stačí stiskněte klávesu F5 ve Visual Studiu spustit aplikaci. Aplikace vám pošle oznámení, který bude zobrazen v horní oznamovací oblasti na zařízení nebo emulátor.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Android SDK mobilní služby]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Android SDK Baidu připínáčku]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure Classic portálu]: https://manage.windowsazure.com/
[Portál Baidu]: http://www.baidu.com/
