V této části kód aktualizujete existujícího projektu back-end mobilní aplikace odeslání nabízená oznámení pokaždé, když se přidá nová položka. To je technologii funkce [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) oznámení rozbočovače, povolení a platformy posune. Různých klientů jsou registrované pro nabízená oznámení pomocí šablon a jeden univerzální nabízených dostali na všechny platformy klienta.

Zvolte následujícího postupu, která odpovídá typu projektu back-end&mdash; [back-end .NET](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>.NET back-end projektu
1. Ve Visual Studiu, klikněte pravým tlačítkem myši project server a klikněte na **Spravovat balíčků NuGet**, vyhledejte `Microsoft.Azure.NotificationHubs`, klikněte na tlačítko **nainstalovat**. Nainstalujete knihovnu rozbočovače oznámení pro odesílání oznámení z vaší back-end.

3. V aplikaci project serveru otevřete **zařízení** > **TodoItemController.cs**a přidejte následující pomocí příkazů:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. V metodě **PostTodoItem** přidáte následující kód po volání **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    To pošle oznámení šablony obsahující položku. Text při vložení nové položky.

4. Publikujte projekt server. 

### <a name="nodejs"></a>Node.js back-end projektu

1. Pokud jste to ještě neudělali, [Stáhněte projektu back-end rychlý úvod](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jinak [online editor Azure portálu](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Nahraďte stávající kód v todoitem.js takto:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    To pošle oznámení šablonu, která obsahuje item.text po vložení nové položky.

2. Při úpravě souborů ve vašem počítači, publikujte server project.
