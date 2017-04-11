<properties
    pageTitle="Azure funkce F # referenční informace pro vývojáře | Microsoft Azure"
    description="Pochopte, jak se dají funkce Azure pomocí F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure funkcí, funkce a zpracování webhooks, dynamické výpočetním, bez serveru architektura, F události#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Referenční informace pro vývojáře F # Azure funkcí

> [AZURE.SELECTOR]
- [C# skriptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skriptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # u funkcí Azure je řešení pro snadné spuštění malých částech kód nebo "funkcí, mezi které" v cloudu. Data se přesune do funkce F # prostřednictvím argumenty funkce. Názvy argumentů jsou v jazyku `function.json`, a nejsou předdefinovanými názvy pro přístup k například funkce protokolování a zrušení tokeny.

V tomto článku se předpokládá, že jste již přečetli [referenční informace pro vývojáře Azure funkcí](functions-reference.md).

## <a name="how-fsx-works"></a>Jak funguje .fsx

`.fsx` Je soubor skriptu F #. Je možné považovat za projektu F #, který je součástí tvořená jedním souborem. Soubor obsahuje kód pro váš program (v tomto případě funkce Azure) a pokyny pro správu závislosti.

Při použití `.fsx` pro funkci Azure běžně povinné sestavení jsou automaticky součástí vám umožňuje zaměření se na funkce spíše než "často používaný text" kód.

## <a name="binding-to-arguments"></a>Vytvoření vazby k argumenty

Každá vazba podporuje několik sadu argumenty, jak je uvedeno v [Azure funkce aktivačními událostmi a referenční informace pro vývojáře vazby](functions-triggers-bindings.md). Například vazeb argument, který podporuje aktivační objektů blob reprodukujte POCO, který může být vyjádřena pomocí záznamu F #. Příklad:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Funkce Azure F # bude trvat jeden nebo více argumentů. Když budeme argumenty funkce Azure, označovány argumenty _vstupní_ a _výstupní_ argumenty. Vstupní argument je přesně vypadá jako: při zadávání funkce Azure vaší F #. Je argumentem _výstupní_ proměnlivých dat nebo `byref<>` argumentů, které bude sloužit jako předávat data zpět _,_ funkce.

Ve výše uvedeném příkladu `blob` je argumentem zadávání a `output` je argumentem výstupní. Všimněte si, že jsme použili `byref<>` pro `output` (není potřeba přidat `[<Out>]` poznámky). Použití `byref<>` typ umožňuje funkce změnit které záznam a nebo objekt odkazuje argument.

Záznam F # použijete vstupní psaní, musí být definici záznam označeny `[<CLIMutable>]` aby rámec Azure funkcí pro nastavení polí řádně podporovat před odesláním záznam funkce. Rozšířená `[<CLIMutable>]` vygeneruje nastavení dialogovém okně Vlastnosti záznamu. Příklad:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Třídy F # lze také pro v i mimo argumenty. Pro blok předmětu vlastnosti obvykle potřebovat mechanismy získání a nastavení. Příklad:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Zapnout protokolování

Výstup přihlásit k vaší [streamování protokoly](../app-service-web/web-sites-streaming-logs-and-console.md) v F #, funkce brát jako argument typ `TraceWriter`. Konzistence, doporučujeme, abyste tento argument jmenuje `log`. Příklad:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchronní

`async` Pracovní postup můžete použít, ale výsledku, musí se vrátíte `Task`. To lze provést `Async.StartAsTask`, například:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Zrušení tokenu

Pokud potřebujete-li zpracovat vypnutí řádně funkce, můžete jí [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentů. To můžete kombinovat s `async`, například:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Import obory názvů

Obory názvů lze otevřít obvyklým způsobem:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Následující obory názvů jsou automaticky otevřené:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Odkazování na externí sestavení

Podobně sestavení framework bude přidáno odkazy `#r "AssemblyName"` směrnice.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Následující sestavení se automaticky přidají funkcemi Azure hostitelské prostředí:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Kromě toho je speciální následující sestavení notaci a mohou být tvořeny simplename (například `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Pokud potřebujete odkazovat na soukromé sestavení, můžete nahrát soubor sestavení do `bin` složky vzhledem k vaší funkce a funkce pro odkazy ho pomocí soubor pojmenujte (například  `#r "MyAssembly.dll"`). Informace o tom, jak nahrát soubory do složky funkce naleznete v části následující řízení balíčku.

## <a name="editor-prelude"></a>Editor Prelude

Editor, který podporuje F # kompilátoru služeb neprojeví obory názvů a sestavení, která automaticky obsahuje funkce Azure. Jako takové může být užitečné zahrnout prelude, která vám pomůže najít sestavení používaném editoru a explicitně otevřít obory názvů. Příklad:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Když funkce Azure provede kódu, zpracuje zdroje s `COMPILED` definována, takže editor prelude budou ignorovat.

## <a name="package-management"></a>Správa balíčku

Použití NuGet balíčků ve funkci F #, přidejte `project.json` soubor, který má funkce složce v systému souborů aplikace (funkce). Tady je příklad `project.json` soubor, který přidá odkaz balíčku NuGet `Microsoft.ProjectOxford.Face` verze 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Je podporována pouze 4.6 .NET Framework, proto se ujistěte, že vaše `project.json` soubor Určuje `net46` znázorněná na obrázku.

Když nahrajete `project.json` souboru, modul runtime získá balíčků a automaticky přidá odkazy na balíček. Není potřeba přidat `#r "AssemblyName"` směrnice. Přidejte požadované `open` příkazy, aby vaše `.fsx` soubor.

Můžete chtít vložit automaticky sestavení odkazy v editoru prelude, zlepšit jako editor interakce služby F # kompilaci.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Jak přidat `project.json` soubor funkce Azure

1. Počáteční zkontrolujte svoji aplikaci funkce běží, které můžete použít po otevření vaší funkce v portálu Azure. To také poskytuje přístup k streamování protokoly místo, kam se zobrazí výstup instalační balíček.

2. Pokud chcete odeslat `project.json` soubor, použijte jeden z způsobů popsaná v tématu [jak aktualizace souborů aplikace (funkce)](functions-reference.md#fileupdate). Pokud používáte [Nepřetržitý nasazení Azure funkcí](functions-continuous-deployment.md), můžete přidat `project.json` soubor, který chcete pracovní větev chcete vyzkoušet před přidáním do nasazení větev.

3. Po `project.json` soubor je přidán, zobrazí se výstup podobně jako v následujícím příkladu ve vaší funkci je přenos protokolu:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Proměnné

Proměnná prostředí nebo aplikace pro nastavení hodnoty, použijte `System.Environment.GetEnvironmentVariable`, například:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Opakované použití .fsx kód

Můžete použít kód z jiné `.fsx` souborů pomocí `#load` směrnice. Příklad:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Poskytuje cesty `#load` směrnice jsou relativní umístění svého `.fsx` soubor.

* `#load "logger.fsx"`Načtení souboru uloženého ve složce (funkce).

* `#load "package\logger.fsx"`Načtení souboru umístěného v `package` složky ve složce (funkce).

* `#load "..\shared\mylogger.fsx"`Načtení souboru umístěného v `shared` složku na stejné úrovni jako funkce složka, to znamená, přímo pod `wwwroot`.

`#load` Směrnice funguje jenom s `.fsx` soubory (F # skriptů) a not s operátorem `.fs` soubory.

## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících zdrojích:

* [F # Guide](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Referenční informace pro vývojáře Azure funkcí](functions-reference.md)
* [Azure funkce C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Referenční informace pro vývojáře Azure NodeJS funkce](functions-reference-node.md)
* [Azure aktivace funkcí a vazby](functions-triggers-bindings.md)
* [Testování funkcí Azure](functions-test-a-function.md)
* [Azure funkce měřítka](functions-scale.md)
