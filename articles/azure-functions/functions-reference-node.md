<properties
    pageTitle="Referenční informace pro vývojáře Azure funkce NodeJS | Microsoft Azure"
    description="Pochopte, jak se dají funkce Azure pomocí NodeJS."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Referenční informace pro vývojáře Azure NodeJS funkce

> [AZURE.SELECTOR]
- [C# skriptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skriptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Možnosti uzel/JavaScript Azure funkce usnadňuje export funkci, která je `context` objektu pro komunikaci s modul runtime a přijímání a odesílání dat pomocí vazby.

V tomto článku se předpokládá, že jste již přečetli [referenční informace pro vývojáře Azure funkcí](functions-reference.md).

## <a name="exporting-a-function"></a>Export funkce

Všechny funkce JavaScript třeba exportovat typu single `function` prostřednictvím `module.exports` modulu runtime najít funkci a ho spusťte. Tato funkce musí vždy obsahovat `context` objektu.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Vazby `direction === "in"` předávají podél jako argumenty funkce, což znamená, můžete použít [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamicky zpracovávání nové vstupy (například pomocí `arguments.length` iterace všechny vstupy). Tato funkce je velmi užitečné, pokud máte jenom aktivační událost žádné další vstupy, jak můžete předvídatelností přístup ke svým datům aktivační událost bez odkazování na vaše `context` objektu.

Argumenty jsou vždy předán podél funkci v pořadí, v jakém že se objevují ve *function.json*, i když je v exporty údajů nezadáte. Pokud máte například `function(context, a, b)` a změňte ho na `function(context, a)`, pořád dostanou přínosu `b` v kódu funkce odkazem `arguments[3]`.

Všechny vazby, bez ohledu na směr, taky předávají se podél `context` objektu (viz níže). 

## <a name="context-object"></a>objekt kontextu

Modul runtime používá `context` objekt k předání dat do a z funkce a umožňují komunikovat s modul runtime.

Kontext objektu je vždy první parametr funkce a vždy je zahrnuta protože má metody jako `context.done` a `context.log` které jsou vyžadovány správně používat modul runtime. Je můžete zadat název objektu něco jiného, co chcete (tedy `ctx` nebo `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

`context.bindings` Objekt shromažďuje všechny vstupní a výstupní data. Data se přidá do `context.bindings` objekt prostřednictvím `name` vlastnosti vazby. Například následující definice vazby podle *function.json*, se dostanete obsah fronty prostřednictvím `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

`context.done` Funkce říká modul runtime, že budete mít spuštěný. Co je důležité volání až to budete mít s funkcí; Pokud nechcete, modul runtime pořád nikdy vědět, že funkce dokončilo. 

`context.done` Funkce umožňuje předat zpět chybu uživatelem definovaných modul runtime, jakož i balík vlastností vlastnosti, který přepíše vlastnosti `context.bindings` objektu.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

`context.log` Metoda umožňuje výstup protokolu příkazy, jež jsou společná pro účely protokolování. Pokud používáte `console.log`, zprávy se zobrazí pouze pro úroveň protokolování procesů, které není jako užitečné.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

`context.log` Metoda podporuje stejný formát parametr, který podporuje uzel [util.format metody](https://nodejs.org/api/util.html#util_util_format_format) . Ano například kód takto:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Můžete se měly zapisovat takto:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>Aktivace HTTP: context.req a context.res

V případě HTTP aktivačních událostí protože je běžné vzorek používat `req` a `res` HTTP objektů žádostí a odpovědí Rozhodli jsme usnadňují přístup počítačů v kontextu objekt místo vynucení použít úplné `context.bindings.name` vzorku.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Verze uzel a Správa balíčku

Verze uzel je uzamčen na `5.9.1`. Jsme způsoby přidání podpora pro další verze a nastavení, která dokáže nahradit.

Můžete zahrnout balíčků ve vaší funkci uložením souboru *package.json* do složky vaší funkce v systému souborů aplikace (funkce). Soubor nahrát pokyny najdete v tématu **jak aktualizovat soubory aplikace funkce** část [Karta Vývojář v tématu funkce Azure](functions-reference.md#fileupdate). 

Můžete taky použít `npm install` v rozhraní aplikací funkce Správce služeb (Kudu) příkazového řádku:

1. Přejděte na: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klikněte na **ladění konzoly > CMD**.

3. Přejděte na `D:\home\site\wwwroot\<function_name>`.

4. Spuštění `npm install`.

Po instalaci balíčků potřebujete importu svého funkce obvyklým způsobem (tedy prostřednictvím `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Proměnné

Proměnná prostředí nebo aplikace pro nastavení hodnoty, použijte `process.env`, jak je ukázáno v následujícím příkladu:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Podpora stroji/CoffeeScript

Není k dispozici, ale přímé podpory pro automatické kompilace stroji/CoffeeScript prostřednictvím modul runtime, které byste měli všechny muset zachází mimo modul runtime při zavedení. 

## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících zdrojích:

* [Referenční informace pro vývojáře Azure funkcí](functions-reference.md)
* [Azure funkce C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](functions-reference-fsharp.md)
* [Azure aktivace funkcí a vazby](functions-triggers-bindings.md)
