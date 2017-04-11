<properties
    pageTitle="Referenční informace pro vývojáře Azure funkce | Microsoft Azure"
    description="Pochopte, jak se dají funkce Azure pomocí C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure funkce C# referenční informace pro vývojáře

> [AZURE.SELECTOR]
- [C# skriptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skriptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Možnosti C# Azure funkce vychází z Azure WebJobs SDK. Data se přesune do funkce C# prostřednictvím argumenty metody. Názvy argumentů jsou v jazyku `function.json`, a nejsou předdefinovanými názvy pro přístup k například funkce protokolování a zrušení tokeny.

V tomto článku se předpokládá, že jste již přečetli [referenční informace pro vývojáře Azure funkcí](functions-reference.md).

## <a name="how-csx-works"></a>Jak funguje .csx

`.csx` Formát umožňuje napsat méně často používaný "text" a se zaměřuje na psaní stejně C# funkci. Azure funkcí, které jenom obsahovat odkazy na sestavení a obory názvů horní, potřebujete si jako obvykle a místo obtékání všechno ve schůzce názvů a třídy, můžete jednoduše definovat vaše `Run` metody. Pokud potřebujete zahrnout všechny třídy, například definovat POCO objekty, můžete zadat třídu uvnitř stejného souboru.

## <a name="binding-to-arguments"></a>Vytvoření vazby k argumenty

Různé vazby je vázaný na funkci C# prostřednictvím `name` vlastnost v konfiguraci *function.json* . Každá vazba obsahuje vlastní podporovaných typů, které jsou uvedeny za vazba; řetězec, POCO nebo několik dalších typů, například podporují objektů blob aktivační událost. Typ, který se nejvíc hodí potřeby můžete použít. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Zapnout protokolování

Zaznamenat výstup do datových proudů protokoly v jazyce C#, můžete zadat `TraceWriter` zadali argumentů. Doporučujeme nazvěte ji `log`. Doporučujeme vyhnout se `Console.Write` ve funkcích Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchronní

Funkce asynchronní, použijte `async` klíčových slov a návrat `Task` objektu.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Zrušení Token

V některých případech může mít operace, které jsou citlivé vypnutím. Když je vždy nejlepší napsat kód, který můžete pracovat k selhání, v případech, kdy je potřeba zpracovávání vypnutí požaduje, definujete [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) zadali argument.  A `CancellationToken` se dozvíte, pokud se při spuštění aktivují vypnutí Host (hostitel). 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Import obory názvů

Pokud potřebujete importovat obory názvů, můžete to udělat tak jako obvykle, s `using` klauzule.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Následující obory názvů automaticky naimportují a proto jsou volitelné:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Odkazování na externí sestavení

Sestavení framework přidat odkazy pomocí `#r "AssemblyName"` směrnice.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Pokud potřebujete odkazovat na soukromé sestavení, můžete nahrát soubor sestavení do `bin` složky vzhledem k vaší funkce a funkce pro odkazy ho pomocí soubor pojmenujte (například  `#r "MyAssembly.dll"`). Informace o tom, jak nahrát soubory do složky funkce naleznete v části následující řízení balíčku.

## <a name="package-management"></a>Správa balíčku

Použití NuGet balíčků ve funkci C#, nahrát soubor *project.json* této funkci složky v systému souborů aplikace (funkce). Tady je příklad *project.json* souboru, který přidá odkaz na Microsoft.ProjectOxford.Face verze 1.1.0:

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

Je podporována pouze 4.6 .NET Framework, proto se ujistěte, že souboru *project.json* Určuje `net46` znázorněná na obrázku.

Po odeslání souboru *project.json* modul runtime získá balíčků a automaticky přidá odkazy na balíček. Není potřeba přidat `#r "AssemblyName"` směrnice. Přidejte požadované `using` příkazy k souboru *run.csx* použití podle balíčků NuGet typů.


### <a name="how-to-upload-a-projectjson-file"></a>Jak nahrát soubor project.json

1. Počáteční zkontrolujte svoji aplikaci funkce běží, které můžete použít po otevření vaší funkce v portálu Azure. 

    To také poskytuje přístup k streamování protokoly místo, kam se zobrazí výstup instalační balíček. 

2. K nahrání souboru project.json, použijte jeden z způsobů popsány v části **jak aktualizovat funkce aplikace soubory** v [tématu funkce Azure vývojář](functions-reference.md#fileupdate). 

3. Po uložení souboru *project.json* zobrazí výstup jako v následujícím příkladu ve vaší funkci je přenos protokolu:

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

Proměnná prostředí nebo aplikace pro nastavení hodnoty, použijte `System.Environment.GetEnvironmentVariable`, jak je ukázáno v následujícím příkladu:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Opakované použití .csx kód

Můžete použít třídy a metody definované v jiných souborech *.csx* v souboru *run.csx* . Aby je dostala, použijte `#load` směrnice v souboru *run.csx* , jak je vidět v následujícím příkladu.

Příklad *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Příklad *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Můžete použít relativní cestu s `#load` směrnice:

* `#load "mylogger.csx"`Načtení souboru uloženého ve složce (funkce).

* `#load "loadedfiles\mylogger.csx"`Načtení souboru uloženého ve složce ve složce (funkce).

* `#load "..\shared\mylogger.csx"`Načtení souboru uloženého ve složce na stejné úrovni jako funkce složka, to znamená, přímo pod *kořenových*.
 
`#load` Směrnice funguje pouze u souborů, *.csx* (C# skript), not s operátorem *cs* soubory. 

## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících zdrojích:

* [Referenční informace pro vývojáře Azure funkcí](functions-reference.md)
* [Azure funkce F # referenční informace pro vývojáře](functions-reference-fsharp.md)
* [Referenční informace pro vývojáře Azure NodeJS funkce](functions-reference-node.md)
* [Azure aktivace funkcí a vazby](functions-triggers-bindings.md)

