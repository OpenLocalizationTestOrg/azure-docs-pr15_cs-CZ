<properties
    pageTitle="Přidání nabízená oznámení aplikace Cordova Apache Azure mobilních aplikacích | Azure aplikace služby"
    description="Informace o použití aplikace Mobile Azure nabízená oznámení odešlete Apache Cordova aplikace."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Přidat do aplikace Apache Cordova nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Základní informace

V tomto kurzu nabízená oznámení přidejte do projektu [Apache Cordova rychlý start] tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz zahrnuje aplikace Apache Cordova vyvinuté pomocí aplikace Visual Studio 2015, která poběží na Android emulátoru Google, zařízení se systémem Android, zařízení s Windows a zařízení s iOS.

K provedení tohoto kurzu, budete potřebovat:

* Počítač s [Visual Studio komunity 2015] nebo novější verze.
* [Visual Studio Tools for Apache Cordova].
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/).
* Dokončeného projektu [Apache Cordova rychlý start] .
* (Android) [Účet Google] s ověřenou e-mailovou adresu.
* (iOS) Apple vývojář aplikace členství a zařízení s iOS (iOS Simulator podporuje nabízených).
* (Windows) Účet systému Windows Store vývojář a zařízení s Windows 10.

##<a name="configure-hub"></a>Konfigurace rozbočovači oznámení

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Podívejte se na video znázorňující postup v této části](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizace project serveru odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Úprava Cordova aplikace pro příjem nabízených oznámení

Musíte zajistíte, že projekt aplikace Apache Cordova je připravená k nabízená oznámení zpracovávat instalaci modulu plug-in nabízených Cordova plus všech specifické pro platformu nabízených služeb.

#### <a name="update-the-cordova-version-in-your-project"></a>Aktualizace verze Cordova v projektu.

Doporučujeme aktualizovat projekt klienta Cordova 6.1.1 Pokud projektu se konfigurují starší verze. Aktualizovat projekt, klikněte pravým tlačítkem myši config.xml otevřete návrháře konfigurace. Vyberte kartu platformách a zvolte 6.1.1 v textovém poli **Cordova rozhraní příkazového řádku** .

Zvolte **vytvořit**a pak na **Sestavit řešení** aktualizovat projekt.

#### <a name="install-the-push-plugin"></a>Instalace modulu plug-in připínáčku

Aplikace Apache Cordova neukládají nativně funkce zařízením nebo se sítí.  Tyto funkce jsou dostupné ve moduly, které jsou publikované na [npm](https://www.npmjs.com/) nebo na GitHub.  `phonegap-plugin-push` Modul plug-in slouží k ovládání sítě nabízená oznámení.

Nainstalujte modul plug-in nabízená v jednom z těchto způsobů:

**Z příkazového řádku:**

Spusťte následující příkaz:

    cordova plugin add phonegap-plugin-push

**Z aplikace Visual Studio:**

1.  V Průzkumníku otevřete `config.xml` souboru klikněte na **moduly plug-in** > **vlastní**zaškrtněte **Libovolná** jako zdroj instalace, pak zadejte `https://github.com/phonegap/phonegap-plugin-push` jako zdroj.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klikněte na šipku vedle položky zdroj instalace.

3. V **SENDER_ID**Pokud už máte ID číselné projektu Google vývojář konzoly projektu, můžete přidat ho tady. Jinak, zadejte zástupného symbolu, například hodnota 777777, a Pokud směrujete Android můžete aktualizovat tuto hodnotu v config.xml později.

4. Klikněte na **Přidat**.

Teď je nainstalovaný modul plug-in nabízených.

####<a name="install-the-device-plugin"></a>Instalace modulu plug-in zařízení

Použijte stejný postup, který jste použili k instalaci modulu plug-in nabízená, ale zjistíte modul plug-in zařízení v seznamu moduly plug-in pro základní (klikněte na **moduly plug-in** > **základní** najít). Potřebujete tento modul plug-in k získání názvu platformu (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Registrace zařízení nabízených při spuštění

Nejprve jsme bude obsahovat některé minimální kódu pro Android. Později jsme budou měnit některé malé běžet iOS nebo Windows 10.

1. Přidání hovoru do **registerForPushNotifications** během zpětné proces přihlášení, nebo na konci metodu **onDeviceReady** :

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Tento příklad ukazuje volání **registerForPushNotifications** po úspěšném ověření, které se doporučuje při použití nabízená oznámení a ověřování v aplikaci.

2. Přidejte nová metoda **registerForPushNotifications** takto:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Ve výše uvedeném kódu nahradit `Your_Project_ID` pomocí numerické v aplikaci project ID pro aplikaci z [Konzoly vývojář Google].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Volitelné) Nakonfigurovat a spustit aplikaci na Androidu

Dokončení této části můžete povolit nabízených oznámení pro Android.

####<a name="enable-gcm"></a>Povolení Firebase cloudu zasílání zpráv

Protože jsme směrujete platformu Google Android poprvé, musíte povolit Firebase cloudu zpráv. Podobně pokud byla zaměřené na zařízení Microsoft Windows, by povolíte WNS podporu.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Konfigurace k odeslání žádosti o nabízených pomocí FCM back-end mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Konfigurace Cordova aplikace pro Android

V aplikaci Cordova otevřete config.xml a nahraďte `Your_Project_ID` pomocí numerické v aplikaci project ID pro aplikaci z [Konzoly vývojář Google].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Otevřete index.js a aktualizujte kód použít ID číselné projektu.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Konfigurace pro USB ladění zařízení s Androidem

Před nasazením aplikace do zařízení s Androidem, musíte povolit ladění USB.  Na telefonu s Androidem proveďte následující kroky:

1. Přejděte na **Nastavení** > **o telefon**a pak klepněte na **číslo sestavení** do režimu Vývojář aktivované (asi 7 časy).

2. Po návratu do **Nastavení** > **Vývojář možnosti** povolit **ladění USB**, potom je připojíte svůj telefon s Androidem vývoj počítači pomocí kabelu USB.

Testování to použít nexus Google – 5 X zařízení s Androidem 6.0 (meloun).  Postupy jsou však běžné přes libovolná moderní Android verze.

#### <a name="install-google-play-services"></a>Nainstalujte Google Play Services

Modul plug-in nabízených závisí na Android Google Play služby nabízených oznámení.  

1.  Ve **Visual Studiu**, klikněte na **Nástroje** > **Android** > **Android SDK správce**, rozbalte složku **Extra** a zaškrtněte políčko, abyste měli jistotu, že každé z následujících SDK nainstalovaný.
    * Zařízení s androidem 2.3 nebo novější
    * Revize Google úložiště 27 nebo vyšší
    * Google Play Services 9.0.2 nebo vyšší

2.  Klikněte na **Nainstalovat balíčků** a počkejte na dokončení instalace.

Aktuální požadovaná knihoven, najdete v [dokumentaci k instalaci phonegap modul plug-in nabízených].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Test nabízená oznámení v aplikaci na Androidu

Spuštění aplikace a vkládání položek v tabulce TodoItem můžete nyní test nabízená oznámení. Můžete to uděláte z stejné zařízení nebo z druhé zařízení, když používáte stejné back-end. Otestujte Cordova aplikace na Android platformě v jednom z těchto způsobů:

- **Pole fyzicky zařízení:**  
Připojte zařízení s Androidem vývojovém počítači pomocí kabelu USB.  Místo toho **Android emulátoru Google**zvolte **zařízení**. Visual Studio nasazení aplikace do zařízení a ho spusťte.  Pak můžete pracovat s aplikací na zařízení.  
Zlepšete prostředí a vývoj.  Sdílení aplikací, jako je [Mobizen] obrazovky vám může pomoci při vytváření aplikace Android tak, že projekci Android obrazovky k webovém prohlížeči na počítači.

- **Na Android emulátoru:**  
Existují další konfiguraci kroky při používání s emulátoru.

    Ujistěte se, že při nasazení nebo ladění na virtuální zařízení, které má rozhraní API Google nastavit jako cíle, jak je znázorněno níže ve Správci zařízení Android virtuální (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Pokud chcete použít rychlejší x86 emulátoru, [nainstalujte ovladač HAXM](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) a konfigurace emulátor používá.

    Přidejte účet Google do zařízení s Androidem kliknutím **aplikace** > **Nastavení** > **Přidat účet**a potom postupujte podle pokynů přidejte stávající Google účet pro zařízení (doporučujeme pomocí existujícího účtu nevytvářejte nový).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Aplikaci pro seznam úkolů jako před a vložte nového položky úkolu. Tentokrát, ikona upozornění se zobrazí v oznamovací oblasti. Můžete otevřít okraj oznámení zobrazit celý text oznámení.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Volitelné) Konfigurace a systémem iOS

Tento oddíl je pro spuštění aplikace project Cordova na zařízení s iOS. Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Instalace a spuštění iOS remotebuild agent na Macu nebo cloudové službě

Před spuštěním Cordova aplikace v iOS pomocí aplikace Visual Studio projděte kroky v [iOS spustíte Průvodce nastavením](http://taco.visualstudio.com/en-us/docs/ios-guide/) k instalaci a spuštění agenta remotebuild.

Zkontrolujte, jestli že je možné vytvářet aplikace pro iOS. Vytvořit pro iOS z aplikace Visual Studio musí kroků v průvodce nastavením. Pokud nemáte Mac, je možné vytvářet pro iOS pomocí agenta remotebuild na služby, jako je MacInCloud. Další informace najdete v tématu [spuštění aplikace pro iOS v cloudu](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] Použití modulu plug-in nabízených IOS vyžaduje XCode 7 nebo vyšší.

####<a name="find-the-id-to-use-as-your-app-id"></a>Nalezení ID používat jako ID aplikace

Před registrací aplikace pro nabízená oznámení, otevřít config.xml v aplikaci Cordova najít `id` atribut hodnoty v ovládacím prvku prvku a jeho zkopírování pro pozdější použití. V následujícím XML ID je `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Při vytváření ID aplikace na portálu pro vývojáře společnosti Apple později, použijte tento identifikátor. (Je-li vytvořit jiné ID aplikace na portálu pro vývojáře a chcete použít, musíte udělat několik kroků navíc dál v tomto kurzu změnit toto ID v config.xml. ID v elementu widgety musí odpovídat ID aplikace na portálu pro vývojáře.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registrace aplikace pro nabízená oznámení na portálu pro vývojáře společnosti Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Podívejte se na video zobrazující podobné kroky](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Ověřte, zda vaše ID aplikace odpovídá Cordova aplikace

Pokud jste vytvořili už ve vašem účtu vývojář Apple ID aplikace odpovídá ID elementu widgety v config.xml, můžete tento krok přeskočit. Ale pokud ID neodpovídají, Uděláte to takto:

1. Odstraňte složku platformách z vašeho projektu.

2. Odstraňte složku moduly plug-in z vašeho projektu.

3. Odstraňte složku node_modules z vašeho projektu.

4. Aktualizujte atribut id elementu widgety v config.xml používat, který jste vytvořili ve vašem účtu vývojář Apple ID aplikace.

5. Opětovné sestavení projektu.

#####<a name="test-push-notifications-in-your-ios-app"></a>Test nabízená oznámení v aplikace pro iOS

1. Ve Visual Studiu Ujistěte se, že tento **iOS** je zúžený na cíl nasazení a vyberte **zařízení** spustit na zařízení s iOS připojení.

    Je možné spouštět na zařízení s iOS připojené k počítači pomocí iTunes. IOS Simulator nepodporuje nabízená oznámení.

2. Stiskněte tlačítko **Spustit** nebo **F5** ve Visual Studiu vytvořte projekt a spusťte aplikaci v zařízení s iOS a potom klikněte na **OK** přijmete nabízená oznámení.

    >[AZURE.NOTE] U aplikace musí explicitně přijmete nabízená oznámení. Tento požadavek vyskytuje při prvním spuštění aplikace.

3. V aplikaci zadejte úkolu a potom klepněte na znaménko plus (+) ikonu.

4. Ověřte, že je dostali oznámení a potom klikněte na OK zavřete oznámení.

##<a name="optional-configure-and-run-on-windows"></a>(Volitelné) Konfigurace a operačním systému Windows

Tato část je určená pro spuštění aplikace project Apache Cordova na zařízeních s Windows 10 (modul plug-in nabízených PhoneGap je podporována ve Windows 10). Pokud nepracujete s zařízení s Windows, můžete tuto část přeskočit.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrace aplikace Windows nabízených oznámení s WNS

Pomocí možností úložiště ve Visual Studiu, vyberte cílovou Windows ze seznamu řešení platformách jako **Windows x64** nebo **Windows x86** (vyhnout **Windows AnyCPU** nabízených oznámení).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Podívejte se na video zobrazující podobné kroky](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Konfigurace centra oznámení pro WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Konfigurace aplikace Cordova podporuje Windows nabízená oznámení

Otevřete návrháře konfigurace (klikněte pravým tlačítkem myši na config.xml a vyberte **Zobrazení návrhu**), vyberte kartu **Windows** a zvolte **Windows 10** klikněte v části **Cílová verze Windows**.

>[AZURE.NOTE] Pokud používáte verze Cordova před Cordova 5.1.1 (6.1.1 doporučeno), musí nastavit příznak oznámením může config.xml true (pravda).

K podpoře nabízená oznámení ve výchozím (ladění) je založena, build.json otevřít soubor. Zkopírujte konfiguraci "verze" Konfigurace ladění.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Po instalaci aktualizace předchozí kód by měl vypadat takto.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Vytvářejte aplikace a zkontrolujte, jestli máte bez chyb. Klient aplikace by nyní zaregistrujte sdělení back-end mobilní aplikaci. Opakujte tento oddíl pro každý Windows projekt ve vašem řešení.

####<a name="test-push-notifications-in-your-windows-app"></a>Test nabízená oznámení v aplikaci Windows

Ve Visual Studiu Ujistěte se, že platformu Windows už je zúžený na cíl nasazení, jako je **Windows x64** nebo **Windows x86**. Pokud chcete spustit aplikaci na počítači s Windows 10, který je hostitelem Visual Studio, zvolte **Místního počítače**.

Stiskněte tlačítko Spustit sestavovat projektu a spusťte aplikaci.

V aplikaci, zadejte název nového todoitem a potom klepněte na znaménko plus (+) ho přidejte na ikonu.

Ověřte, že doručení po přidání se položky.

##<a name="next-steps"></a>Další kroky

* Přečtěte si o [Oznámení rozbočovače] se naučit používat nabízená oznámení.
* Pokud jste to ještě neudělali, pokračujte kurzu tak, že [Přidání ověřování] , který Apache Cordova aplikace.

Naučte se používat SDK.

* [Apache Cordova SDK]
* [Serverového SDK]
* [Node.js Server SDK]

<!-- URLs -->
[Přidání ověřování]: app-service-mobile-cordova-get-started-users.md
[Představení aplikace Apache Cordova]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Účet Google]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Karta Vývojář v Google konzoly]: https://console.developers.google.com/home/dashboard
[instalace phonegap modul plug-in připínáčku si přečtěte následující dokumentaci]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Komunita aplikace Visual Studio 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Oznámení rozbočovače]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Serverového SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
