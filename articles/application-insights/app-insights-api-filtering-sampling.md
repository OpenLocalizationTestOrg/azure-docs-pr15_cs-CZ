<properties 
    pageTitle="Filtrování a předzpracování v aplikaci přehledy SDK | Microsoft Azure" 
    description="Napište Telemetrie procesorů a Telemetrie Incializátory SDK nějak filtrovat nebo přidat vlastnosti k datům na portál přehledy aplikace se odesílá telemetrie." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtrování a předzpracování telemetrie v aplikaci přehledy SDK

*Přehledy aplikace je v náhledu.*

Můžete napsat a konfigurovat přehledy SDK aplikaci můžete přizpůsobit způsob zachyceny a zpracování před odesláním ke službě aplikace přehledy telemetrie moduly plug-in. 

Tyto funkce jsou aktuálně k dispozici pro ASP.NET SDK.

* [Odběr](app-insights-sampling.md) snižuje objemu telemetrie beze změny statistiky. Uloží společně související datové body tak, aby můžete procházet mezi nimi při diagnostikovat potíže. Na portálu jsou celkový počet vynásobená pro odběr.
* [Filtrování pomocí Telemetrie procesorů](#filtering) umožňuje vybrat nebo změnit telemetrie v SDK odesílá na server. Například může omezení počtu telemetrie vyloučením žádosti z robots. Ale filtrování jednodušší přístup ke snížení přenosy než odběr. Umožňuje větší kontrolu nad co jsou přenášena, musíte ale mějte na paměti, že ovlivňuje statistiky – například pokud odfiltrovat všechny žádosti o úspěšně.
* Všechny telemetrie odesílaným z aplikace, včetně telemetrie z standardní moduly [telemetrie inicializátory přidat vlastnosti](#add-properties) . Můžete například přidat počítané hodnoty. nebo čísla verze prodloužením doby k filtrování dat na portálu.
* [Rozhraním API SDK](app-insights-api-custom-events-metrics.md) slouží k odesílání vlastní události a metriky.


Než začnete:

* Instalace [aplikace přehledy SDK ASP.NET v2](app-insights-asp-net.md) v aplikaci. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtrování: ITelemetryProcessor

Tento postup vám větší kontrolu nad co je součástí nebo vyloučené ze telemetrie proudu. Můžete ve spojení s vzorků nebo samostatně.

Filtrování telemetrie, psaní telemetrie procesoru a zaregistrovat se SDK. Všechny telemetrie prochází procesoru a můžete přetáhnout z proudu nebo můžete přidat vlastnosti. Platí to i telemetrie z standardní moduly například kolekcí žádost HTTP a závislost kolekcí i telemetrie, který jste sami napsali. Můžete, například odfiltrovat telemetrie o žádosti o robots ani úspěšné závislost hovory. 

> [AZURE.WARNING] Filtrování telemetrie odesílaným z SDK použití procesorů můžete zkosení statistiky, která se zobrazí na portálu a ztížit ke sledování související položky.
> 
> Místo toho zvažte použití [odběr](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Vytvoření telemetrie procesor

1. Zkontrolujte, že verze 2.0.0 SDK přehledy aplikace v projektu nebo později. Klikněte pravým tlačítkem na projektu v okně Průzkumník řešení Visual Studia a zvolte spravovat balíčků NuGet. Ve Správci balíčku NuGet zaškrtněte Microsoft.ApplicationInsights.Web.

1. Vytvoření filtru, implementace ITelemetryProcessor. Toto je jiný bod rozšiřitelnost například telemetrie modul telemetrie inicializační a telemetrie kanálu. 

    Všimněte si, že Telemetrie procesorů vytvářet řetěz zpracování. Když instanci telemetrie procesor předáte odkaz na další procesor v řetězci. Když datový bod telemetrie předán metodu proces, nemá svou práci a pak volá další procesor Telemetrie v řetězci.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Vkládaný znak v ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Toto je stejné části kde inicializace filtr odběr.)

Předat řetězcové hodnoty ze souboru config zadáním veřejné pojmenované vlastnosti ve svojí třídě. 

> [AZURE.WARNING] Dávejte podle názvu typu a názvy vlastnost v souboru config názvy tříd a vlastnosti v kódu. Pokud soubor config odkazuje na neexistující typ nebo vlastnost, SDK tiše nemusí odeslat všechny telemetrie.

 
**Můžete taky** inicializace filtru v kódu. Vhodné inicializace třídy – například AppStart v Global.asax.cs - procesor do vkládat řetězec:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients vytvořili od tohoto okamžiku použije procesorů.

### <a name="example-filters"></a>Příklad filtry

#### <a name="synthetic-requests"></a>Syntetické požadavky

Odfiltrovat roboti a web testů. Přestože metriky Explorer nabízí možnost odfiltrovat syntetické zdrojů, tato možnost snižuje přenosy pomocí filtrování na stránce SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Neúspěšné ověření

Odfiltrovat požadavky s "401" odpověď. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Odfiltrovat rychlé vzdálené závislost volání

Pokud chcete Diagnostika hovory, které jsou pomalé, odfiltrovat rychlé z nich. 

> [AZURE.NOTE] To bude zkosení statistických údajů, které se zobrazí na portálu. Závislost typu grafu budou vypadat stejně, jako kdyby hovory závislost všechny chyby.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Diagnostika problémů s závislostí

[Tento blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) popisuje projektu Diagnostika problémů s závislost automaticky odesláním běžná příkazu ping závislostí.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Přidání vlastnosti: ITelemetryInitializer

Umožňuje definovat globální vlastnosti, které jsou odeslané s všechny telemetrie; telemetrie inicializátory a přepsání vybraných chování modulů standardní telemetrie. 

Třeba přehledy aplikace pro analýza balíčku webového shromažďuje telemetrie o požadavků HTTP. Ve výchozím nastavení se označuje jako neúspěšný jakékoliv žádosti o s kódem odezvy > = 400. Ale pokud chcete 400 považovat za úspěch, můžete zadat telemetrie inicializační, která nastavuje vlastnosti úspěch.

Pokud zadáte inicializační telemetrie, je označená jako kdykoli metod Track*() je místo toho možnost. Jedná se o metody s názvem standardní telemetrie moduly. Podle názvů moduly nenastaví jakoukoliv vlastnost, která už nastavil inicializační. 

**Definování vaší inicializační**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Načtení vaší inicializační**

V ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Případně* můžete vytvořit instanci inicializační v kódu, například v Global.aspx.cs:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Zobrazit větší část v tomto příkladu.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>Inicializátory telemetrie JavaScript

*JavaScript*

Vložení telemetrie inicializační bezprostředně za inicializace kód, který jste dostali od portálu: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Souhrn k dispozici v telemetryItem není vlastní vlastnosti najdete v článku [datového modelu](app-insights-export-data-model.md#lttelemetrytypegt).

Můžete přidat tolik inicializátory vám zachce. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor a ITelemetryInitializer

Jaký je rozdíl mezi telemetrie procesorech a telemetrie inicializátory?

* Existuje několik překrývá v jaké máte možnosti s nimi: obojí mohou sloužit k přidání vlastnosti telemetrie.
* Vždy spustit před TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors umožňují úplně nahradit nebo odstranit položku telemetrie.
* TelemetryProcessors není zpracovat telemetrie čítač výkonu.



## <a name="persistence-channel"></a>Trvalé kanálu 

Pokud aplikace spustí, kde připojení k Internetu není vždy dostupná nebo pomalé, zvažte použití kanálu trvalé místo výchozího v paměti kanálu. 

Výchozí v paměti kanál ztratí telemetrie, který nebyla odeslána podle času slouží k zavření aplikace. Přestože lze použít `Flush()` pokus o odeslání všechna data v vyrovnávací paměť zbývá ho pořád dojde ke ztrátě dat pokud existuje bez připojení k Internetu nebo pokud aplikaci vypne před dokončením přenos.

Naopak kanálu trvalé vyrovnávací paměti telemetrie v souboru, před odesláním k portálu. `Flush()`zajišťuje, že data se ukládají v souboru. Pokud žádná data neodešle podle času slouží k zavření aplikace, zůstane v souboru. Po restartování aplikace data odešle pak při připojení k Internetu. Data sečteny v souboru po dobu, je nutné, dokud nebude k dispozici připojení. 

### <a name="to-use-the-persistence-channel"></a>Použití trvalé kanálu

1. Importujte balíček NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Zahrnout tento kód vaši aplikaci distribuovali vhodné inicializace umístění:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Použití `telemetryClient.Flush()` před aplikace zavře, abyste měli jistotu odeslané na portálu nebo do souboru uloženy dat.

    Všimněte si, že Flush() synchronní trvalé kanálu, ale asynchronní jiných kanálů.

 
Trvalé kanál je optimalizována pro zařízení scénáře, kde počet událostí vytvořené pomocí aplikace je relativně malým a připojení se často nedůvěryhodných. Tento kanál zapsat události na disk do důvěryhodného úložiště nejdřív a pokuste se odeslat. 

#### <a name="example"></a>Příklad

Řekněme, že chcete sledovat neošetřené výjimky. Přihlášení k odběru `UnhandledException` události. V zpětné zahrnout volání zarovnání a ujistěte se, že telemetrie zachován.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Když aplikaci vypne, uvidíte souboru v `%LocalAppData%\Microsoft\ApplicationInsights\`, která obsahuje mu tuhle zkomprimovanou události. 
 
Při příštím spuštění této aplikace kanál bude vystopovat tento soubor a předvádění telemetrie interpretace aplikace, pokud je to možné.

#### <a name="test-example"></a>Příklad test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Kód kanálu trvalé je [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Odkaz dokumenty

* [Přehled rozhraní API](app-insights-api-custom-events-metrics.md)

* [Přehled technologie ASP.NET](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>Kód SDK

* [Základní ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Další kroky


* [Hledání událostí a protokolování][diagnostic]
* [Analytický nástroj vzorkování](app-insights-sampling.md)
* [Řešení potíží][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
