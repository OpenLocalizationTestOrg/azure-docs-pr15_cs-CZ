<properties
    pageTitle="Azure vazby funkce události centrální | Microsoft Azure"
    description="Pochopte, jak používat Azure události centrální vazby ve funkcích Azure."
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure funkce události centrální vazby

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a kód [Azure události centrální](../event-hubs/event-hubs-overview.md) vazby u funkcí Azure. Azure funkce podpory vazby aktivační události a výstup pro rozbočovače události Azure.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Pokud začínáte rozbočovače události Azure, najdete v článku [Přehled centrální Azure události](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure rozbočovači události aktivace vazby

Aktivační události pro centrální události Azure mohou sloužit k reagovat na události odeslané události rozbočovači události proudu. Musíte mít přístup pro čtení k centru událost nastavit vazbu aktivační událost.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON události rozbočovače aktivace vazby

Soubor *function.json* pro aktivační události pro centrální události Azure určuje následující vlastnosti:

- `type`: Musíte nastavit *eventHubTrigger*.
- `name`: Název proměnné používá k centrální zpráva o události v kódu funkce. 
- `direction`: Musíte nastavit *v*. 
- `path`: Název centru události.
- `consumerGroup`: Toto je nepovinný vlastnost slouží k nastavení [skupiny spotř](../event-hubs-overview.md#consumer-groups) použitá k přihlášení k odběru událostí v centru. Pokud není zadáno, `$Default` spotř skupina se používá. 
- `connection`: Název aplikace nastavení, které obsahuje připojovací řetězec na obor názvů umístěnou v centru události. Zkopírujte připojovací řetězec kliknutím na tlačítko **Informace o připojení** pro názvů, není událost centru navigace samotné.  Tento připojovací řetězec musí mít aspoň oprávnění ke čtení aktivovat aktivační událost.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Příklad aktivační událost C# Azure centrální události
 
Pomocí výše uvedených příklad function.json textu zprávy události se Zaprotokolují pomocí C# funkce následující kód:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Příklad aktivační událost F # Azure centrální události

Pomocí výše uvedených příklad function.json textu zprávy události se Zaprotokolují pomocí F # funkce následující kód:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Aktivační událost Azure události centrální Node.js příklad
 
Pomocí výše uvedených příklad function.json textu zprávy události se Zaprotokolují pomocí funkce Node.js následující kód:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure události Centrální výstupní vazba

Centrální události Azure Výstupní vazba slouží k zápisu událostí událost centrální události proudu. Musíte mít oprávnění Odeslat k rozbočovači události zápisu události. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON události rozbočovače Výstupní vazba

Soubor *function.json* rozbočovači události Azure výstup vazby určuje následující vlastnosti:

- `type`: Musíte nastavit *eventHub*.
- `name`: Proměnná Název použitý v kódu funkce zprávy rozbočovači události. 
- `path`: Název centru události.
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec na obor názvů umístěnou v centru události. Zkopírujte připojovací řetězec kliknutím na tlačítko **Informace o připojení** pro názvů, není událost centru navigace samotné.  Tento připojovací řetězec musíte mít oprávnění Odeslat zprávu odešlete události centrální proudu.
- `direction`: Musíte nastavit *se*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure události rozbočovači C# příklad pro vazbu výstup
 
Následující C# funkce příklad ukazuje, psaní události toku události centrální události. Tento příklad představuje centrální události Výstupní vazba uveden nad použité u C# časovače aktivační události.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure události centrální F # příklad pro vazbu výstup

Následující F # funkce příklad ukazuje, psaní události toku události centrální události. Tento příklad představuje centrální události Výstupní vazba uveden nad použité u C# časovač aktivační události.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure příklad Node.js centrální událostí pro vazbu výstup
 
Následující Node.js funkce příklad ukazuje, psaní události toku události rozbočovači události. Tento příklad představuje centrální události Výstupní vazba uveden nad použité u aktivační časovače Node.js.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
