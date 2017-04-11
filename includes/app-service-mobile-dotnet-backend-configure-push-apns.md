
+ **.NET back-end (C#)**:    
    1. Ve Visual Studiu, klikněte pravým tlačítkem myši project server a klikněte na **Spravovat balíčků NuGet**, vyhledejte `Microsoft.Azure.NotificationHubs`, klikněte na tlačítko **nainstalovat**. Nainstalujete knihovnu rozbočovače oznámení pro odesílání oznámení z vaší back-end.

    2. V back-end Visual Studio projektu otevřete **řadiče** > **TodoItemController.cs**. V horní části souboru, přidejte následující text `using` údajů:

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
    
                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";
    
                try
                {
                    // Send the push notification and log the results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);
    
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

+ **Back-end Node.js** : 
   
    1. Pokud jste to ještě neudělali, [Stáhněte si projektu rychlý úvod](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jinak [online editor Azure portálu](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor). 
    
    2. Nahraďte skript tabulky todoitem.js tento kód:


            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');
            
            var table = azureMobileApps.table();
            
            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of the Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');
                
                // Create a payload that contains the new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";
                
                // Execute the insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do the push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });
            
            module.exports = table;

    2. Při úpravě souborů ve vašem počítači, publikujte server project.
