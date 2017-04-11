<properties
    pageTitle="Azure vazby funkce HTTP a webhook | Microsoft Azure"
    description="Pochopte, jak používat HTTP a webhook aktivačními událostmi a vazby ve funkcích Azure."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure funkce HTTP a webhook vazby

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak konfigurovat a kód HTTP a webhook aktivačními událostmi a vazby ve funkcích Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON pro HTTP a webhook vazby

Soubor *function.json* obsahuje vlastnosti, které se týkají žádostí a odpovědí.

Vlastnosti žádost HTTP:

- `name`: Proměnná Název použitý ve funkci kód pro objekt request (nebo žádost o textu v případě Node.js funkce).
- `type`: Musíte nastavit *httpTrigger*.
- `direction`: Musíte nastavit *v*. 
- `webHookType`: Pro WebHook aktivace platné hodnoty jsou *github*, *časová rezerva*a *genericJson*. Aktivační události HTTP, který není WebHook nastavte tuto vlastnost na prázdný řetězec. Další informace o WebHooks naleznete v části následující [WebHook aktivačních událostí](#webhook-triggers) .
- `authLevel`: Neplatí pro WebHook aktivačních událostí. Nastavte, aby "fungovat" vyžadovat klávesu rozhraní API pro rozevírací rozhraní API klíčové požadavek nebo "správce" vyžadovat hlavní klíč rozhraní API "anonymní". Další informace najdete v tématu [rozhraní API klíče](#apikeys) pod.

Vlastnosti pro odpověď HTTP:

- `name`: Proměnná Název použitý ve funkci kód objektu odpověď.
- `type`: Musí být nastavena na *http*.
- `direction`: Musíte nastavit *se*. 
 
Příklad *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>Aktivace WebHook

Aktivační WebHook je aktivační události pro HTTP, která má následující funkce určené pro WebHooks:

* Pro konkrétní poskytovatele WebHook (aktuálně GitHub a časová rezerva přidejte podporují) funkce runtime ověřuje poskytovatele podpis.
* Pro Node.js funkcí, mezi které modul runtime funkce poskytuje textu žádosti o místo na žádost o objekt. Vzhledem k tomu můžete určit, co je k dispozici zadáním parametru je žádné zvláštní zpracování C# funkcí, mezi které. Pokud zadáte `HttpRequestMessage` se zobrazí žádost o objekt. Pokud zadáte typu POCO, pokusí se za běhu funkce analyzovat JSON objekt v hlavním textu žádosti k naplnění vlastnosti objektu o.
* Aktivovat WebHook musí obsahovat funkce žádost HTTP rozhraní API klíče. Aktivačních událostí WebHook HTTP tohoto požadavku je nepovinný krok.

Další informace o tom, jak nastavit GitHub WebHook, najdete v článku [GitHub vývojář – vytvoření WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>Adresa URL aktivovat funkci

Spustit funkci, pošlete žádost HTTP adresu URL, která je tvořen kombinací adresa URL aplikace (funkce) a název funkce:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>Rozhraní API klíče

Ve výchozím nastavení musí být součástí žádost HTTP aktivovat funkci HTTP nebo WebHook rozhraní API klíče. Klíč může být součástí proměnnou řetězce dotazu s názvem `code`, nebo ho může být součástí `x-functions-key` HTTP záhlaví. Pro jiné WebHook funkcí, mezi které můžete určit, že rozhraní API klíč nepožaduje nastavením `authLevel` vlastnost na hodnotu "anonymní" v souboru *function.json* .

Rozhraní API klíčových hodnot můžete najít ve složce *D:\home\data\Functions\secrets* v systému souborů aplikace (funkce).  Hlavní klíč a funkční klávesa se nastavují v souboru *host.json* , jak je vidět v tomto příkladu. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Funkční klávesa z *host.json* lze použít k aktivaci všechny funkce ale zakázané funkce neaktivují. Hlavní klíč lze použít libovolnou funkci aktivovat a spustí funkci, i když je zakázán. Můžete nakonfigurovat funkci, kterou chcete vyžadovat hlavní klíč nastavením `authLevel` vlastnost na "správce". 

Obsahuje-li *tajemství* složka JSON soubor se stejným názvem jako funkci, `key` vlastnosti v tomto souboru lze také aktivovat funkci a tento klíč lze používat pouze funkce záznam odkazuje. Například klávesu rozhraní API pro funkci s názvem `HttpTrigger` podle *HttpTrigger.json* ve složce *tajemství* . Tady je příklad:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Při nastavování WebHook aktivační událost, nesdíleli hlavní klíč zprostředkovateli WebHook. Stisknutím klávesy, která budou fungovat jenom s funkcí zpracovávající WebHook.  Hlavní klíč můžete použít ke spuštění libovolnou funkci i zakázat funkce.

## <a name="example-c-code-for-an-http-trigger-function"></a>Příklad C# kódu pro funkci HTTP aktivační událost 

Příklad vyhledá `name` parametrů v řetězci dotazu nebo textu žádost HTTP.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Příklad F # kódu pro funkci HTTP aktivační událost

Příklad vyhledá `name` parametrů v řetězci dotazu nebo textu žádost HTTP.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Budete potřebovat `project.json` soubor, který používá NuGet neodkazuje `FSharp.Interop.Dynamic` a `Dynamitey` sestavení takto:

```json
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
```

Použije NuGet vzdáleně závislosti mezi úkoly a bude odkazovat na ně v skriptu.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Příklad Node.js kódu pro aktivační funkce HTTP 

Tento příklad vyhledá `name` parametrů v řetězci dotazu nebo textu žádost HTTP.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Příklad C# GitHub WebHook funkce 

Tento příklad protokoly GitHub problém komentáře.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Příklad F # GitHub WebHook funkce

Tento příklad protokoly GitHub problém komentáře.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Příklad Node.js GitHub WebHook funkce 

Tento příklad protokoly GitHub problém komentáře.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
