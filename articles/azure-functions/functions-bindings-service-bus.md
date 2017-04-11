<properties
    pageTitle="Azure aktivace funkcí služby Bus a vazby | Microsoft Azure"
    description="Pochopte, jak pomocí aktivačních událostí Bus služby Azure a vazby ve funkcích Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
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
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure funkce služby Bus aktivačními událostmi a vazby pro fronty a témata

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a aktivace služby Azure Bus kód a vazby ve funkcích Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Služba Bus fronty nebo tématu aktivační událost

#### <a name="functionjson"></a>Function.JSON

Soubor *function.json* udává následující vlastnosti.

- `name`: Proměnná Název použitý v kódu funkce fronty nebo tématu nebo fronty nebo tématu zprávy. 
- `queueName`: Pro fronty pouze aktivace název fronty hlasování.
- `topicName`: K tématu aktivace pouze na název tématu hlasování.
- `subscriptionName`: K tématu aktivace pouze na název předplatného.
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec Bus služby. Připojovací řetězec musí být pro službu Bus názvů, mimo jiné konkrétní fronty nebo tématu. Pokud připojovací řetězec není Správa práv, nastavte `accessRights` vlastnost. Pokud necháte `connection` prázdná, aktivační událost nebo vazby fungovat s výchozí služby Bus připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsServiceBus nastavení aplikace.
- `accessRights`: Určuje přístupových práv k dispozici pro připojovací řetězec. Výchozí hodnota je `manage`. Nastavit `listen` při používání připojovací řetězec, který nenabízí možnost spravovat oprávnění. Jinak funkce runtime zkusit a selhání provádět operace, které vyžadují Správa přístupových práv.
- `type`: Musíte nastavit *serviceBusTrigger*.
- `direction`: Musíte nastavit *v*. 

Příklad *function.json* aktivační události fronty Bus služby:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# příkladu zpracovávající zprávu fronty Bus služby

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # příkladu zpracovávající zprávu fronty Bus služby

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Node.js příkladu zpracovávající zprávu fronty Bus služby

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Podporované typy

Zpráva fronty služby Bus může být rekonstruováno k některým z následujících typů:

* Objekt (JSON)
* řetězec
* pole bajtů 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>Chování PeekLock

Modul runtime funkce se zobrazuje zpráva v `PeekLock` režimu a volání `Complete` na zprávy v případě funkci dokončení úspěšně nebo volání `Abandon` selže funkci. Pokud funkce trvá déle, než `PeekLock` automaticky obnoven časový limit, zámek.

#### <a id="sbpoison"></a>Poškodit zpracování zpráv

Služba Bus znamená vlastní poškozená zpráva zpracování které nemůže ovládat nebo konfigurovat konfigurace Azure funkce nebo kód. 

#### <a id="sbsinglethread"></a>Zřetězení jednoduchým

Ve výchozím nastavení těchto funkcích zpracuje runtime současně více zpráv. Přímé modul runtime zpracuje pouze jedné fronty nebo tématu zprávy současně, nastavte u `serviceBus.maxConcurrrentCalls` 1 v souboru *host.json* . Informace o souboru *host.json* najdete v tématu [Struktura složek](functions-reference.md#folder-structure) v vývojář referenční článek a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) na wikiwebu WebJobs.Script úložiště.

## <a id="sboutput"></a>Služba Bus fronty nebo tématu Výstupní vazba

#### <a name="functionjson"></a>Function.JSON

Soubor *function.json* určuje následující vlastnosti.

- `name`: Proměnná Název použitý v kódu funkce fronty nebo fronty zprávy. 
- `queueName`: Pro fronty pouze aktivace název fronty hlasování.
- `topicName`: K tématu aktivace pouze na název tématu hlasování.
- `subscriptionName`: K tématu aktivace pouze na název předplatného.
- `connection`: Aktivace stejné jako u Bus služby.
- `accessRights`: Určuje přístupových práv k dispozici pro připojovací řetězec. Výchozí hodnota je `manage`. Nastavit `send` při používání připojovací řetězec, který nenabízí možnost spravovat oprávnění. Jinak funkce runtime zkusit a selhání provádět operace, které vyžadují Správa práv, například vytváření dotazů.
- `type`: Musíte nastavit *serviceBus*.
- `direction`: Musíte nastavit *se*. 

Příklad *function.json* pro pomocí aktivační timer psát služby Bus zpráv:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Podporované typy

Azure funkce můžete vytvořit zprávu fronty Bus služby v některé z následujících typů.

* Objekt (vždy vytvoří zprávu JSON, vytvoří zprávu s objekt null, pokud je argument hodnota null po ukončení funkce)
* řetězec (vytvoří zprávu, pokud je argument hodnota jinou hodnotu než null po ukončení funkce)
* pole bajtů (funguje řetězec) 
* `BrokeredMessage`(C#, funguje řetězec)

Vytvoření více zpráv ve funkci C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`. Zprávy se vytvoří při volání `Add` metody.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# příklady vytvořené služby Bus zpráv

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # příklad kódu, který vytvoří zprávu fronty Bus služby

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Příklad Node.js vytvoří zprávu fronty Bus služby

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
