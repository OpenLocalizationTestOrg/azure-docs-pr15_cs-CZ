Pomocí postupu, která odpovídá typu projektu back-end&mdash; [back-end .NET](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>.NET back-end projektu

1. Ve Visual Studiu, klikněte pravým tlačítkem myši project server a klikněte na **Spravovat balíčků NuGet**, vyhledejte `Microsoft.Azure.NotificationHubs`, klikněte na tlačítko **nainstalovat**. Nainstalujete knihovnu oznámení rozbočovače klienta.

2. Ve složce řadiče otevřete TodoItemController.cs a přidejte následující `using` příkazy:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Nahrazení `PostTodoItem` metodu s Tento kód:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Publikujte projekt server.

### <a name="nodejs"></a>Node.js back-end projektu

1. Pokud jste to ještě neudělali, [Stáhněte si projektu rychlý úvod](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jinak [online editor Azure portálu](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
 
1. Nahraďte stávající kód v souboru todoitem.js takto:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    To pošle GCM oznámení, která obsahuje item.text po vložení nové položky úkolu. 

2. Při úpravě souboru v místním počítači, publikujte server project. 
