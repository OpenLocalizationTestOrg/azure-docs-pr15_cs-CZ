<properties
    pageTitle="Azure vazby funkce Twilio | Microsoft Azure"
    description="Pochopte, jak pomocí funkce Azure Twilio vazby."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Twilio funkce Výstupní vazba

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak konfigurovat a používat Twilio vazby s funkcemi Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure podporuje funkce Twilio výstupní vazby povolení funkcí pro odesílání textových zpráv SMS s několika řádky kódu a [Twilio](https://www.twilio.com/) účtu. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON Azure oznámení rozbočovače Výstupní vazba

Soubor function.json obsahuje následující vlastnosti:

- `name`: Proměnná Název použitý ve funkci kódu pro textové zprávy Twilio SMS.
- `type`: musí být nastavený na *"twilioSms"*.
- `accountSid`: Název nastavení aplikace, který obsahuje vaše ID zabezpečení účtu Twilio musíte nastavit tuto hodnotu.
- `authToken`: Tato hodnota musí nastavena na název aplikace nastavení, které obsahuje ověřovací token Twilio.
- `to`: Telefonní číslo, které se neodesílají text zprávy SMS nastavenou tuto hodnotu.
- `from`: Telefonní číslo odeslaném z textu zprávy SMS nastavenou tuto hodnotu.
- `direction`: musí být nastavený na *","*.
- `body`: Tuto hodnotu mohou sloužit k pevně kód textové zprávy SMS, pokud nemusíte dynamické nastavení v kódu pro vaše (funkce). 

 
Příklad function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Příklad C# fronty aktivační událost Twilio Výstupní vazba

#### <a name="synchronous"></a>Obrázek

Tento kód synchronní příklad pro aktivační události pro úložišti Azure fronty používá parametr out textovou zprávu odešlete zákazníkům, kteří objednávky.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchronní

Tento příklad asynchronní kód pro aktivační události pro úložišti Azure fronty pošle textovou zprávu zákazníkům, kteří objednávky.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Příklad Node.js fronty aktivační událost Twilio Výstupní vazba

V tomto příkladu Node.js pošle textovou zprávu zákazníkům, kteří objednávky.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
