<properties
    pageTitle="Azure vazby funkce DocumentDB | Microsoft Azure"
    description="Pochopte, jak používat Azure DocumentDB vazby ve funkcích Azure."
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

# <a name="azure-functions-documentdb-bindings"></a>Azure funkce DocumentDB vazby

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a kód Azure DocumentDB vazby ve funkcích Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB vstupní vazby

Vstupní vazby můžete načíst dokument z kolekce DocumentDB a předejte přímo do vaší vazby. Id dokumentu můžou být závisí na aktivační událost, kterou zavolat a funkce. V jazyce C# funkci všechny změny provedené záznam automaticky odešle zpět do kolekce při ukončení funkce úspěšně.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON DocumentDB Vstupní vazba

Soubor *function.json* obsahuje následující vlastnosti:

- `name`: Proměnná Název použitý ve funkci kódu pro dokument.
- `type`: musí být nastavený na "documentdb".
- `databaseName`: Databázi obsahující dokument.
- `collectionName`: Kolekci obsahující dokument.
- `id`: Id dokumentu k načítání. Tato vlastnost podporuje vazby podobně jako "{queueTrigger}", které použije hodnotu řetězce fronty zprávy jako dokumentu Id.
- `connection`: Tento řetězec musí být aplikace nastavení koncový bod pro váš účet DocumentDB. Pokud se rozhodnete účtu na kartě Integrace, nové nastavení aplikace se vytvoří pro vás s názvem, který má následující podobu yourAccount_DOCUMENTDB. Pokud je potřeba ručně vytvořit nastavení aplikace, připojovací řetězec musí následující podobu, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "směr: musí být nastavený na *"v"*.

Příklad *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB zadávání kódu příklad v jazyce C# fronty aktivační událost
 
Použití výše uvedených příklad function.json, bude vstupní vazba DocumentDB načíst dokument s id, které odpovídá řetězce zprávy fronty a předat parametr "dokument". Pokud tento dokument, bude mít parametr "dokument" null. Dokument bude pak aktualizována pomocí nového textovou hodnotu při ukončení funkce.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Příklad zadávání kódu Azure DocumentDB pro F # fronty aktivační události pro

Použití výše uvedených příklad function.json, bude vstupní vazba DocumentDB načíst dokument s id, které odpovídá řetězce zprávy fronty a předat parametr "dokument". Pokud tento dokument, bude mít parametr "dokument" null. Dokument je potom aktualizovány nová hodnota text při ukončení funkce.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Budete potřebovat `project.json` soubor, který používá NuGet k určení `FSharp.Interop.Dynamic` a `Dynamitey` balíčků jako závislostí balíčku, takto:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Použije NuGet vzdáleně závislosti mezi úkoly a bude odkazovat na ně v skriptu.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB vstupní příklad aktivační události Node.js fronty
 
Pomocí výše uvedených příklad function.json, bude vstupní vazba DocumentDB načítat dokument s id, které odpovídá řetězce zprávy fronty a předejte jej `documentIn` vlastnost vazba. Ve funkcích Node.js aktualizované dokumenty nejsou odesílány kolekci. Však můžete předat vstupní vazba přímo do výstupu DocumentDB vazby pojmenované `documentOut` podporuje aktualizace. Tento příklad kódu aktualizuje vlastnost text vstupní dokumentu a nastavíte jako výstupního dokumentu.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB výstup vazby

Funkce můžete napsat JSON dokumenty k databázi Azure DocumentDB pomocí **Azure DocumentDB dokumentu** Výstupní vazba. Další informace o Azure DocumentDB: kontrolujte potvrzení [Úvod k DocumentDB](../documentdb/documentdb-introduction.md) a [Začínáme kurz](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON DocumentDB Výstupní vazba

Soubor function.json obsahuje následující vlastnosti:

- `name`: Proměnná Název použitý ve funkci kódu pro nový dokument.
- `type`: musí být nastavený na *"documentdb"*.
- `databaseName`: Databázi obsahující kolekci tam, kde bude vytvořili nový dokument.
- `collectionName`: Kolekci tam, kde bude vytvořili nový dokument.
- `createIfNotExists`: Toto je logická hodnota označíte, zda bude vytvořena kolekci, pokud není k dispozici. Výchozí hodnota je *false*. Důvod, proč se nové kolekce se vytvářejí rezervovaná výkon, tato role má ceny důsledky. Další informace naleznete v [ceny stránky](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: **Nastavení aplikace** nastavit koncový bod pro váš účet DocumentDB musí být tento řetězec. Pokud se rozhodnete účtu na kartě **Integrace** , nové nastavení aplikace se vytvoří se s názvem, který má následující podobu `yourAccount_DOCUMENTDB`. Pokud je potřeba ručně vytvořit nastavení aplikace, připojovací řetězec musí následující podobu, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: musí být nastavený na *","*. 
 
Příklad function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB výstup příklad aktivační události Node.js fronty

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Výstupní dokument:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB výstup příklad pro F # fronty aktivační události pro

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB výstup příklad pro C# fronty aktivační událost


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB výstup kód příklad nastavení názvu souboru

Pokud chcete nastavit název dokumentu ve funkci, jednoduše nastavte `id` hodnotu.  Například ztracené JSON obsahu pro zaměstnance byl je do fronty podobná této:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Ve funkci aktivační událost fronty můžete použít následující kód C#: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Nebo rovnocenný F # kód:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Příklad výstupu:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
