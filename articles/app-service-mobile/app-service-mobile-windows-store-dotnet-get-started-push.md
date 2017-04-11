<properties
    pageTitle="Přidání nabízená oznámení aplikací univerzální platformu systému Windows (UWP) | Azure mobilní aplikace"
    description="Naučte se používat Azure aplikace služby mobilní aplikace a Azure oznámení rozbočovače odešlete nabízená oznámení aplikací univerzální platformu systému Windows (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Přidání nabízených oznámení pro aplikace pro Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Základní informace

V tomto kurzu nabízená oznámení přidejte do projektu [start systému Windows snadné](app-service-mobile-windows-store-dotnet-get-started.md) tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Konfigurace rozbočovači oznámení

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Registrace aplikace nabízených oznámení

Potřebujete odeslat aplikace pro Windows Store a potom konfigurace serveru projektu integrovat s Windows oznámení služby (WNS) odeslat nabízených.

1. Ve Visual Studiu Průzkumníku klikněte pravým tlačítkem myši UWP aplikace project, klikněte na **Store** > **Přidružení App Store …**. 

    ![Přidružení aplikace pro Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. V průvodci klikněte na tlačítko **Další**, přihlaste se pomocí svého účtu Microsoft, do **rezervy nový název aplikace**zadejte název aplikace a potom klikněte na **Rezervovat**.

3. Po registraci aplikace úspěšně, vyberte nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přiřadit**. Tím přidáte do manifest aplikace požadované informace registrace pro Windows Store.  

7. Přejděte na [Centrum pro vývojáře Windows](https://dev.windows.com/en-us/overview)přihlásit pomocí svého účtu Microsoft, klikněte na novou registraci aplikace v **Moje aplikace**a potom rozbalte **služby** > **nabízená oznámení**. 

8. Na stránce **nabízená oznámení** na **server služby Live** v části **Microsoft Azure mobilní služby**.

9. Na stránce registrace si poznamenejte hodnotu v části **aplikace tajemství** a **ID balíčku zabezpečení**, které se vedle používá ke konfiguraci vašeho back-end mobilní aplikace. 

    ![Přidružení aplikace pro Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajná klienta a balíčku ID zabezpečení jsou důležité zabezpečovací přihlašovací údaje. Tyto hodnoty sdílet s kýmkoli nebo distribuce s aplikací. **Id aplikace** se používá s tajná konfigurace ověřování Account Microsoft.

##<a name="configure-the-backend-to-send-push-notifications"></a>Konfigurace back-end odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Aktualizace serveru odesílat nabízená oznámení

Pomocí následujícího postupu, která odpovídá typu projektu back-end&mdash; [back-end .NET](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>.NET back-end projektu

1. Ve Visual Studiu klikněte pravým tlačítkem myši project server a klikněte na **Spravovat balíčků NuGet**, vyhledejte Microsoft.Azure.NotificationHubs a potom klikněte na tlačítko **nainstalovat**. Nainstalujete knihovnu oznámení rozbočovače klienta.

2. Rozbalte položku **řadiče**, otevřete TodoItemController.cs a přidejte následující pomocí příkazů:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. V metodě **PostTodoItem** přidáte následující kód po volání **InsertAsync**:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Tento kód říká centru oznámení o odeslání nabízená oznámení nové položky po vložení.

4. Publikujte projekt server.

### <a name="nodejs"></a>Node.js back-end projektu

1. Pokud jste to ještě neudělali, [Stáhněte si projektu rychlý úvod](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jinak [online editor Azure portálu](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Nahraďte stávající kód v souboru todoitem.js takto:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    To pošle oznámení WNS zprávu, která obsahuje item.text po vložení nové položky úkolu.

2. Při úpravě souborů ve vašem počítači, publikujte server project.

##<a id="update-app"></a>Přidání nabízená oznámení pro aplikace

Potom aplikaci zaregistrujte nabízených oznámení při spuštění. Pokud už jste povolili ověřování, ujistěte se, že uživatel znaménka se změnami před pokusem o zaregistrujte nabízená oznámení.

1. Otevření souboru projektu **App.xaml.cs** a přidejte následující `using` příkazy:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Ve stejném souboru přidejte následující definice metody **InitNotificationsAsync** třídy **aplikace** :

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Tento kód načte ChannelURI aplikace z WNS a pak tuto ChannelURI zaregistruje s aplikací Mobilní služba aplikace.

3. V horní části obslužná **OnLaunched** v **App.xaml.cs**přidejte modifikátorem **asynchronní** definice metody a přidejte následující volání nová metoda **InitNotificationsAsync** jako v následujícím příkladu:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Zaručuje, že je krátkodobý ChannelURI registrovaná při každém spuštění aplikace.

4. Znovu vytvořte projekt aplikace UWP. Aplikace je nyní připravena k odběru informačního oznámení.

##<a id="test"></a>Test nabízená oznámení v aplikaci

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Další kroky

Další informace o nabízená oznámení:

* [Používání spravovaných klienta pro Azure mobilní aplikace](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Šablony pružnost posune různé platformy a lokalizované posune pošlete. Zjistěte, jak si zaregistrovat šablony.

* [Diagnostika problémů s nabízená oznámení](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Existuje různých důvodů, proč oznámení mohou získat textu nebo nekončí na zařízeních. Toto téma ukazuje, jak analyzovat a zjistit příčinou selhání nabízená oznámení. 

Zvažte, pokračovat k jedné z následujících výukové programy pro:

+ [Přidání ověřování aplikace](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Informace o ověřování uživatelů aplikace se zprostředkovatelem identit.

+ [Povolení offline synchronizace aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

