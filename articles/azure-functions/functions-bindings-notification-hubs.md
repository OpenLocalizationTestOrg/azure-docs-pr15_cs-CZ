<properties
    pageTitle="Azure vazby funkce oznámení centrální | Microsoft Azure"
    description="Pochopte, jak používat Azure oznámení centrální vazby ve funkcích Azure."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funguje, funkce a zpracování události, dynamické výpočetním bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure centrální oznámení funkce Výstupní vazba

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a kód Azure oznámení rozbočovači vazby ve funkcích Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Funkce Odeslat nabízená oznámení pomocí rozbočovači nakonfigurované Azure oznámení s několika řádky kódu. Však centrální oznámení Azure musí být nakonfigurované pro služby platformu oznámení (PNS) chcete použít. Další informace o konfiguraci rozbočovači Azure oznámení a vývoj klientské aplikace, které registrace pro příjem oznámení ve formě najdete v článku [Začínáme s rozbočovače oznámení](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a klikněte na svoji platformu cílového klienta nahoře.

Oznámení odeslané může být nativní upozornění nebo oznámení ve formě šablony. Nativní oznámení cílové platformy konkrétní oznámení, jak konfigurovat `platform` vlastnost Výstupní vazba. Oznámení šablony mohou sloužit k více platformu.   

## <a name="notification-hub-output-binding-properties"></a>Oznámení centrální výstup vazby vlastnosti

Soubor function.json obsahuje následující vlastnosti:

- `name`: Název proměnné používá k centrální oznámení v kódu funkce.
- `type`: musí být nastavený na *"notificationHub"*.
- `tagExpression`: Výrazy značku umožňují určit, že do sady zařízení, které jste si zaregistrovali pro příjem oznámení ve formě, které odpovídají výrazu označení dostávat oznámení.  Další informace najdete v tématu [výrazy směrování a značky](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Název oznámení Centrum zdrojů na portálu Azure.
- `connection`: Tento připojovací řetězec musí být nastavena na hodnotu *DefaultFullSharedAccessSignature* pro vaše Centrum oznámení **Nastavení aplikace** připojovací řetězec.
- `direction`: musí být nastavený na *","*. 
- `platform`: Vlastnost platformu označuje platformu oznámení oznámení cílů. Musí mít jednu z následujících hodnot:
    - `template`: To je platformu výchozí vlastnost platformy vynecháte z výstupu vazby. Oznámení ve formě šablony lze použít pro všechny platformu nakonfigurováno v centrální oznámení Azure. Další informace o použití šablon obecně o neodeslání oznámení křížového platformy s rozbočovači oznámení Azure najdete v článku [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Služba nabízených oznámení společnosti Apple. Další informace o konfiguraci centra oznámení pro APNS a přijímání oznámení v klientské aplikaci, najdete v článku [odesílání nabízená oznámení pro iOS rozbočovače oznámení Azure](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Zařízení Amazon zasílání zpráv](https://developer.amazon.com/device-messaging). Další informace o konfiguraci centra oznámení pro ADM a přijímání oznámení v aplikaci Kindle, najdete v článku [Začínáme s rozbočovače oznámení pro Kindle aplikace](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud zasílání zpráv](https://developers.google.com/cloud-messaging/). Firebase cloudu zasílání zpráv, který je nová verze GCM, podporuje i. Další informace o konfiguraci centra oznámení pro GCM/FCM a přijímání oznámení v aplikaci pro Android klienta najdete v článku [odesílání nabízených oznámení pro Android s rozbočovače oznámení Azure](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows nabízená oznámení služby](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) zacílení platformách Windows. WNS podporuje i Windows Phone 8.1 a novější. Další informace o konfiguraci centra oznámení pro WNS a přijímání oznámení v aplikaci univerzální platformu systému Windows (UWP), najdete v článku [Začínáme s oznámení rozbočovače pro univerzální platformy aplikace pro Windows](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Služby Microsoft nabízených oznámení](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Tuto platformu podporuje Windows Phone 8 a starší operační systémy Windows Phone. Další informace o konfiguraci centra oznámení pro MPNS a přijímání oznámení v aplikaci Windows Phone, najdete v článku [odesílání nabízená oznámení s rozbočovače oznámení Azure na Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Příklad function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Nastavení oznámení centrální připojení řetězce

Použití výstup rozbočovači oznámení vazba, musíte nakonfigurovat připojovací řetězec pro rozbočovači. Stačí na kartě *Integrace* výběrem rozbočovače oznámení nebo vytvoření nového. 

Můžete taky ručně přidat připojovací řetězec pro existující centrální přidáním připojovací řetězec pro *DefaultFullSharedAccessSignature* rozbočovače oznámení. Tento připojovací řetězec obsahuje vaše funkce přístupová oprávnění k odeslání oznámení. Hodnota *DefaultFullSharedAccessSignature* připojovacího řetězce můžete k nim získat přístup pod tlačítkem **klíčů** v hlavním zásuvné příslušného zdroje rozbočovači upozornění na portálu Azure. Ruční přidání připojovací řetězec pro vaše centrum, použijte následující kroky: 

1. Na zásuvné **funkce aplikace** portál Azure, klikněte na **Nastavení funkce aplikace > přejděte na nastavení aplikace služeb**.

2. V **Nastavení** zásuvné klikněte na **Nastavení aplikace**.

3. Přejděte dolů do části **připojovací řetězec** a přidejte pojmenované položky pro *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení. Změna typu na možnost **vlastní**.
4. Odkaz název připojovacího řetězce vazby výstupu. Podobně jako **MyHubConnectionString** použili v předchozím příkladu.

## <a name="native-notification-examples"></a>Příklady nativní oznámení

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APNS nativní oznámení s C# fronty aktivačních událostí

Tento příklad ukazuje, jak používat typy definice v [Microsoft Azure oznámení rozbočovače knihovny](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) k odeslání oznámení o nativní APNS. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM nativní oznámení s C# fronty aktivačních událostí

Tento příklad ukazuje, jak používat typy definice v [Microsoft Azure oznámení rozbočovače knihovny](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) k odeslání oznámení o nativní GCM. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS nativní oznámení s C# fronty aktivačních událostí

Tento příklad ukazuje, jak k poslání nativní WNS oznámením použít typy definice v [Microsoft Azure oznámení rozbočovače knihovny](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) . 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Příklady šablon oznámení

#### <a name="template-example-for-nodejs-timer-triggers"></a>Příklad šablony pro Node.js časovače aktivačních událostí 

V tomto příkladu pošle oznámení pro [registraci šablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , která obsahuje `location` a `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Příklad šablony pro F # časovače aktivačních událostí

V tomto příkladu pošle oznámení pro [registraci šablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , která obsahuje `location` a `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Příklad šablony pomocí parametr out 

V tomto příkladu pošle oznámení pro [registraci šablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , která obsahuje `message` zástupný symbol v šabloně.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Příklad šablony s asynchronních funkcí

Pokud používáte asynchronní kód, se parametry nejsou povolené. V tomto případě použití `IAsyncCollector` vrátíte šablony oznámení. Následující kód je příklad asynchronní výše uvedeného kódu. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Příklad šablony pomocí JSON 

V tomto příkladu pošle oznámení pro [registraci šablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , která obsahuje `message` zástupný symbol v šabloně pomocí platný řetězcový JSON.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Příklad šablony pomocí oznámení rozbočovače typy knihoven

Tento příklad ukazuje, jak používat typy definice v [Microsoft Azure oznámení rozbočovače knihovny](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
