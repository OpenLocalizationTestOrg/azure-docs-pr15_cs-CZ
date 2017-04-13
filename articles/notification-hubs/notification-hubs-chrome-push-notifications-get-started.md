<properties
    pageTitle="Nabízená oznámení odešlete aplikací Chrome s Azure oznámení rozbočovače | Microsoft Azure"
    description="Naučte se používat Azure oznámení rozbočovače odešlete nabízená oznámení aplikací Chrome."
    services="notification-hubs"
    keywords="nabízená oznámení na mobil, nabízená oznámení nabízená oznámení, chromu nabízená oznámení"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Odešlete aplikací Chrome s Azure oznámení rozbočovače nabízená oznámení

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Toto téma ukazuje, jak používat Azure oznámení rozbočovače odešlete nabízená oznámení aplikací Chrome, která se zobrazí v rámci prohlížeč Google Chrome. V tomto kurzu budeme vytvářet stylu webové části aplikace, která přijímá nabízená oznámení pomocí [Zasílání zpráv Cloud Google (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Kurz vás provede těchto základních kroků, které umožňují nabízená oznámení:

* [Povolení Cloud Google zasílání zpráv](#register)
* [Konfigurace rozbočovače oznámení](#configure-hub)
* [Připojení stylu webové části aplikace k centru oznámení](#connect-app)
* [Odešlete aplikací Chrome nabízená oznámení](#send)
* [Další funkce a funkce](#next-steps)

>[AZURE.NOTE] Nabízená oznámení aplikací Chrome nejsou obecný oznámení v prohlížeči – jsou specifické pro rozšíření prohlížeče modelu (podrobnosti najdete v článku [Přehled aplikací Chrome] ). Kromě plochy prohlížeče Chrome aplikace projít Mobile (Android a iOS) Apache Cordova. V tématu [Chrome aplikací na mobilní telefon] Další informace.

Konfigurace GCM a Azure oznámení rozbočovače je shodný s konfigurace pro Android, protože [Zasílání zpráv Cloud Google Chrome] se nepoužívá a stejné GCM nyní podporuje zařízení s Androidem a Chrome instance.

##<a id="register"></a>Povolení Cloud Google zasílání zpráv

1. Přejděte na web [Služby Google Cloud konzoly] , přihlaste se pomocí svých přihlašovacích údajů účtu Google a potom klikněte na tlačítko **Vytvořit projekt** . Zadejte vhodný **Název projektu**a potom klikněte na tlačítko **vytvořit** .

    ![Google Cloud konzoly – vytvoření projektu][1]

2. Poznamenejte si **Číslo projektu** na stránce **projektů** pro projekt, který jste právě vytvořili. Použijete tento jako **ID GCM odesílatele** v seznamu aplikací Chrome k registraci GCM.

    ![Google Cloud konzoly - číslo projektu][2]

3. V levém podokně klikněte na **rozhraní API & auth**a potom přejděte dolů a klikněte na přepínač Povolit **Google Cloud zpráv pro Android**. Nemusíte povolit **Zasílání zpráv Cloud Google Chrome**.

    ![Google Cloud konzoly - Server klíč][3]

4. V levém podokně klikněte na **pověření** > **Vytvořit nový klíč** > **Klíč serveru** > **vytvořit**.

    ![Konzola Google Cloud – přihlašovací údaje][4]

5. Poznamenejte si serveru **Rozhraní API klíče**. Nakonfigurujete takto v centrální oznámení pak umožňuje odešlete GCM nabízená oznámení.

    ![Konzola Google Cloud – rozhraní API klíč][5]

##<a id="configure-hub"></a>Konfigurace rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. v zásuvné **Nastavení** vyberte **Oznámení služby** a **Google (GCM)**. Zadejte klávesu rozhraní API a uložte.

&emsp;&emsp;![Azure oznámení rozbočovače – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Připojení stylu webové části aplikace k centru oznámení

Vaše centrum oznámení je nyní nakonfigurován pro práci s GCM a máte řetězce připojení k registraci aplikace jak přijímání a odesílání nabízená oznámení. LK

###<a name="create-a-new-chrome-app"></a>Vytvoření nové aplikace chromu

Následující ukázce se podle [Ukázkové GCM aplikací Chrome] a používá doporučené postupy při vytváření aplikace Chrome. Jsme zvýrazní kroky související s Azure oznámení rozbočovače. 

>[AZURE.NOTE] Doporučujeme stáhnout zdroje pro tuto aplikaci Chrome z [Chrome aplikace oznámení centrální vzorku].

Aplikaci Chrome se vytvoří pomocí skriptu JavaScript a můžete používáte některou z editory upřednostňované word pro jeho vytvoření. Níže je tato aplikace Chrome bude vypadat takto.

![Aplikace Google Chrome][15]

1. Vytvořte složku a pojmenujte ho `ChromePushApp`. Samozřejmě, je v poli název libovolného: Pokud zadáte název je něco jiného, zkontrolujte, jestli že nahraďte cestu v segmentech požadovaný kód.

2. Stáhněte si [požadavkům js knihovna] ve složce, kterou jste vytvořili v druhém kroku. Tato složka knihovny bude obsahovat dva podsložky: `components` a `rollups`.

3. Vytvoření `manifest.json` soubor. Všech aplikací Chrome jsou podporovaným seznamu soubor, který obsahuje metadata aplikace a většina je důležité, všechna oprávnění udělená při instalaci uživatelem aplikace.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Upozornění `permissions` prvek, který určuje, že tato aplikace Chrome bude moct příjem nabízených oznámení z GCM. Třeba také zadat URI rozbočovače oznámení Azure kde aplikací Chrome budou volání ZBÝVAJÍCÍ k registraci.
    Naše ukázkové aplikace taky používá soubor ikony `gcm_128.png`, zobrazený ve zdroji opětovné použití z původního GCM vzorku. Můžete nahradit ho nějaký obrázek, která [ikonu kritéria](https://developer.chrome.com/apps/manifest/icons).

4. Vytvoření souboru s názvem `background.js` s kódem takto:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Toto je soubor, který překryvné okno aplikací Chrome HTML (**register.html**) a také definuje rutiny **messageReceived** zpracovávání příchozích nabízená oznámení.

5. Vytvoření souboru s názvem `register.html` – tato možnost definuje v uživatelském rozhraní aplikace pro Chrome. 

   >[AZURE.NOTE] V tomto příkladu **CryptoJS v3.1.2**. Pokud jste si stáhli jinou verzi knihovnu, ujistěte se, nahradíte správně verze v `src` cestu.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Vytvoření souboru s názvem `register.js` s kódem dole. Tento soubor Určuje skript za `register.html`. Aplikací Chrome Nepovolit spuštění vloženého, musíte vytvořit samostatné zálohování skriptu pro vaše uživatelské rozhraní.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Výše uvedené skript obsahuje následující klíče parametry:
    - **window.OnLoad** definuje události klikněte na tlačítko dvou tlačítek v uživatelském rozhraní. Jednu registruje GCM a další možnosti využití ID registrace vrácená po registraci s GCM k registraci rozbočovače oznámení Azure.
    - **updateLog** je funkce, která umožňuje zpracovat možnosti jednoduché protokolování.
    - **registerWithGCM** je prvním kliknutí na tlačítko obslužné, takže `chrome.gcm.register` volat GCM zaregistrovat aktuální instance aplikace Chrome.
    - **registerCallback** je zpětné funkci, která se nazývá při registraci volání GCM vrátí.
    - **registerWithNH** je druhá rutiny klikněte na tlačítko, který registruje rozbočovače oznámení. Nenarazí `hubName` a `connectionString` (který uživatel zadal) a crafts oznámení rozbočovače registrace REST API volání.
    - **splitConnectionString** a **generateSaSToken** jsou uživatelé, které představují JavaScript provádění procesu tokenu vytvoření přidružení zabezpečení použitého v všechna volání rozhraní REST API. Další informace najdete v tématu [Běžné koncepty](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** je funkce, která umožňuje Azure oznámení rozbočovače HTTP ZBÝVAJÍCÍ volání.
    - **registrationPayload** definuje datové XML registrace. Další informace najdete v tématu [Vytvoření registrace NH REST API]. S co jsme dostali od GCM aktualizujeme ID registrace v nich.
    - **Klient** je instanci **XMLHttpRequest** využívající aby žádost HTTP příspěvek. Všimněte si, že aktualizujeme `Authorization` záhlaví s `sasToken`. Úspěšné dokončení tohoto volání zaregistruje tuto instanci aplikace Chrome s Azure oznámení rozbočovače.


Celková struktura složek pro tento projekt by měl vypadat takto:     ![aplikací Google Chrome – struktura složek][21]

###<a name="set-up-and-test-your-chrome-app"></a>Nastavení a otestování Chrome aplikace

1. Otevřete prohlížeč Chrome. Otevřete **Chrome rozšíření** a povolení **režimu Vývojář**.

    ![Google Chrome – povolení režimu Vývojář][16]

2. Klikněte na **načíst rozbalené rozšíření** a přejděte do složky, které jste vytvořili soubory. Můžete taky volitelně skupině **aplikací Chrome a rozšíření Developer nástroj**. Tento nástroj je aplikací Chrome sám o sobě (nainstalovali z obchodu Chrome Web) a poskytuje pokročilé možnosti ladění pro vývoj aplikací Chrome.

    ![Google Chrome – načíst rozbalené rozšíření][17]

3. Pokud je aplikace Chrome vytvořená bez chyb, uvidíte aplikací Chrome neprojevila.

    ![Google Chrome – zobrazení aplikací Chrome][18]

4. Zadejte **Číslo projektu** , kterou jste získali dříve z **Konzoly Cloud Google** jako ID odesílatele a klikněte na **Zaregistrovat se GCM**. Musí se zobrazit zpráva **registrace s GCM proběhla úspěšně.**

    ![Google Chrome – přizpůsobení aplikací Chrome][19]

5. Zadejte svoje **Jméno centrální oznámení** a **DefaultListenSharedAccessSignature** , který jste získali dříve z portálu Microsoft a klikněte na **Zaregistrovat se centrální oznámení Azure**. Musí se zobrazit zpráva **Registrace centrální upozornění na úspěšné!** a podrobnosti o registraci odpověď, která obsahuje registrace Azure oznámení rozbočovače ID.

    ![Google Chrome - určete podrobnosti centrální oznámení][20]  

##<a name="send"></a>Odeslat oznámení aplikací Chrome

Pro účely testování, doporučujeme vám pošle Chrome nabízená oznámení pomocí .NET konzoly aplikace. 

>[AZURE.NOTE] Nabízená oznámení s rozbočovače oznámení můžete odeslat z libovolné back-end prostřednictvím naše veřejné <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a>. Podívejte se na náš [portálem si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/notification-hubs/) Další příklady a platformy.

1. Ve Visual Studiu v nabídce **soubor** vyberte **Nový** a potom **projekt**. V části **Visual Basic**klikněte na **Windows** a **Aplikace konzoly**a potom klikněte na **OK**.  Tím vytvoříte nový projekt aplikace konzoly.

2. V nabídce **Nástroje** klikněte na **Správce balíčku knihovny** a potom **Konzoly balíčku správce**. Zobrazí se konzole balíčku správce.

3. V okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Přidá odkaz na SDK Bus služby Azure <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet balíčku</a>.

4. Otevřít `Program.cs` a přidejte následující `using` údajů:

        using Microsoft.Azure.NotificationHubs;

5. V `Program` třídy, přidejte následujícím způsobem:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Ujistěte se, pokud chcete nahradit `<hub name>` zástupný symbol s názvem centrální oznámení, která se zobrazí na [portál](https://portal.azure.com) v centrální oznámení zásuvné. Zástupný symbol řetězec připojení taky nahradit připojovací řetězec s názvem `DefaultFullSharedAccessSignature` , který jste získali v části Oznámení centrální konfigurace.

    >[AZURE.NOTE] Ujistěte se, používají připojovací řetězec s **úplným** přístupem, ne **Poslech** přístup. **Poslech** přístup připojovací řetězec není udělit oprávnění Odeslat nabízená oznámení.

5. Přidejte následující volání v `Main` metodu:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Ujistěte se, jestli je spuštěný Chrome a spusťte aplikaci konzoly.

7. Měli byste vidět tohle upozornění překryvné na ploše.

    ![Google Chrome – oznámení][13]

8. Můžete pomocí okna Chrome upozornění na hlavním panelu (v systému Windows) zobrazit všechny oznámení při spuštění Chrome.

    ![Google Chrome – seznam oznámení][14]

>[AZURE.NOTE] Nemusíte mít spouštění aplikací Chrome nebo otevření v prohlížeči (i když sám chromu musí běžet). V okně chromu oznámení taky získáte konsolidované zobrazení vám oznámení.

## <a name="next-steps"> </a>Další kroky

Další informace o rozbočovače upozornění v [Oznamovací rozbočovače přehledu].

Jej směrovat určité uživatele, podívejte se do kurz [Azure oznámení rozbočovače upozornit uživatele] . 

Segmentech uživatelů tak, že skupin úrok postupujte kurz [Rozbočovače oznámení Azure novinky] .

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome aplikace oznámení centrální ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud konzoly]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Přehled rozbočovače oznámení]: notification-hubs-push-notification-overview.md
[Přehled aplikací Chrome]: https://developer.chrome.com/apps/about_apps
[Ukázka GCM aplikací Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome aplikací na mobilní telefon]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Vytvoření registrace NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Knihovna požadavkům js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Zasílání zpráv pro Chrome Cloud Google]: https://developer.chrome.com/apps/cloudMessagingV1
[Informujte uživatele, rozbočovače Azure oznámení]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure oznámení rozbočovače novinky]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
