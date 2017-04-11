<properties
    pageTitle="Azure aktivace funkcí a vazby | Microsoft Azure"
    description="Pochopte, jak pomocí aktivačních událostí a vazby ve funkcích Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure funkce aktivačními událostmi a vazby referenční informace pro vývojáře


Toto téma obsahuje obecné odkaz pro aktivačních událostí a vazby. Obsahují některé pokročilé funkcí a syntaxe nepodporuje všechny typy vazeb.  

Pokud hledáte podrobné informace kolem konfigurace a kódování určitý typ aktivační událost nebo vazbu, je vhodné klikněte na jednu ze aktivační událost nebo vazby uvedeny níže:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Tyto články se předpokládá, že jste si přečetli [referenční informace pro vývojáře Azure funkcí](functions-reference.md)a [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)nebo [Node.js](functions-reference-node.md) články odkaz vývojář.



## <a name="overview"></a>Základní informace

Aktivace jsou události odpovědi použít ke spuštění vlastního kódu. Umožňují reagovat na události přes platformu Azure nebo místní. Vazby představují potřebné metadata připojuje kódu požadované aktivační událost nebo přidružené vstupní a výstupní data.

Získat lepší představu o různých vazby, které můžete integrovat s aplikací funkce Azure, najdete pod odkazy v následující tabulce.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Pochopíte lépe spustí a vazby obecně chtít, aby měli k provedení některých kódu procesu nezobrazí nové položky do fronty Azure úložiště. Azure funkcí obsahuje aktivační události pro Azure fronty podporuje to. Které jsou potřeba, tyto informace ke sledování fronty:
 
- Úložiště účet, pokud existuje fronta.
- Název fronty.
- Proměnná název, který byste použili kód odkázat na nové položky, které bylo zrušeno do fronty.  
 
Aktivační fronty vazba obsahuje tyto informace o funkci Azure. Soubor *function.json* jednotlivých funkcí obsahuje všechny související vazby.  Tady je příklad *function.json* obsahující fronty aktivace vazby. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Kód může odeslat různé typy výstupu v závislosti na způsobu zpracování nová položka fronty. Můžete třeba psaní nového záznamu do tabulky Azure úložiště.  K tomu můžete nastavit výstupní vazba na tabulku Azure úložiště. Tady je příklad *function.json* obsahující úložiště výstup Vazba tabulky, které lze použít s fronty aktivační událost. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Následující funkce C# odpoví na nové položky se nezobrazí do fronty a zapíše nový uživatelský záznam do tabulky Azure úložiště.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Další příklady a podrobnější informace o typech Azure úložiště, které jsou podporovány najdete v tématu [Aktivace Azure funkcí a vazby úložištěm Azure](functions-bindings-storage.md).


Použít pokročilejších funkcí vazby v portálu Azure, vyberte možnost **Rozšířený editor** na kartě **Integrace** funkce. Rozšířený editor umožňuje upravovat *function.json* přímo na portálu.

## <a name="dynamic-parameter-binding"></a>Dynamické parametr vazba 

Místo statickou konfiguraci nastavení vlastnosti vazby výstup můžete nakonfigurovat nastavení Svázat dynamicky se data, která je součástí vašeho aktivační událost Vstupní vazba. Zvažte situace, kde nové objednávky zpracovávají fronty Azure úložiště. Každou novou položku fronty je JSON řetězec obsahující alespoň následující vlastnosti:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Můžete odeslat textová zpráva pomocí svého účtu Twilio jako aktualizace, zda byla přijata pořadí.  Můžete nakonfigurovat `body` a `to` pole vaše Twilio Výstupní vazba dynamicky vázat `name` a `mobileNumber` , které byly část vstupní následujícím způsobem.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Funkce kód teď bude inicializace výstupní parametr následujícím způsobem. Během provádění bude vlastnosti výstup vázaný na požadované vstupní data.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Náhodné GUID

Azure funkce poskytuje syntaxi pro generování náhodného GUID s vazby. Následující syntaxe vazby zapíše výstup do nových objektů BLOB jedinečný název v kontejneru úložišti Azure: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Vrácení jeden výstup

V případech, kde kód funkce vrátí jednu výstup, můžete použít výstup vazbu s názvem `$return` uchovávání přirozenější podpis funkce v kódu. Můžete použít jen s jazyků, které podporují vrácená žádná hodnota (C# Node.js, F #). Vazba bude podobný následující objektů blob Výstupní vazba, který se používá s fronty aktivační událost.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Následující kód C# vrátí výstup snadnější bez použití `out` parametrů v podpisu (funkce).


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Tento stejný postup je znázorněn s Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # příkladu jsou uvedeny níže.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Tento můžete také použít s několika výstupních parametrů jmenovat jeden výstup s `$return`.


## <a name="route-support"></a>Podpora směrování

Ve výchozím nastavení při vytváření funkce HTTP aktivační událost nebo WebHook, funkce je s možností zadání s směrování formuláře:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Je možné upravit tento postup pomocí volitelné `route` vlastnost na aktivační událost protokolu HTTP vstupního vazbu. Jako příklad následující soubor *function.json* definuje `route` vlastnost aktivační události pro HTTP:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Pomocí této konfiguraci, tato funkce je teď s možností zadání s následujícím postupu místo původního směrování.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Díky kód funkce pro podporu dvěma parametry v adrese, `category` a `id`. Použít jakékoli [Webového rozhraní API směrování omezení](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry. Následující kód funkce C# využívá obou parametrů.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Tady je Node.js funkce kód použít stejné směrování parametry.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Ve výchozím nastavení jsou všechny trasy funkce předponou *rozhraní api*. Můžete také upravit nebo odebrat předponu pomocí `http.routePrefix` vlastnost v souboru *host.json* . Následující příklad odebere předpona trasy *rozhraní api* pomocí prázdný řetězec předpony v souboru *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Podrobné informace o tom, jak aktualizovat soubor *host.json* vaše funkcí naleznete v tématu [jak se aktualizovat aplikaci funkce soubory](functions-reference.md#fileupdate). 

Přidáním této konfiguraci je teď funkci s možností zadání s následující postup:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Další informace o dalších vlastností, které můžete konfigurovat v souboru *host.json* najdete v článku Principy [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících zdrojích:

* [Testování funkcí](functions-test-a-function.md)
* [Změna velikosti funkce](functions-scale.md)
