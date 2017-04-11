<properties 
    pageTitle="Konfigurace aplikace přehledy SDK s ApplicationInsights.config nebo XML" 
    description="Povolení nebo zakázání moduly shromažďování dat a přidání výkonnosti a ostatní parametry" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurace aplikace přehledy SDK s ApplicationInsights.config nebo XML

V aplikaci přehledy .NET SDK se skládá z počtu NuGet balíčků. [Základní balíček](http://www.nuget.org/packages/Microsoft.ApplicationInsights) poskytuje rozhraní API pro odesílání telemetrie interpretace aplikace. [Další balíčky](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) stanovit telemetrie _moduly_ a _inicializátory_ automaticky sledovat telemetrie z aplikace a její kontext. Úpravou konfiguračního souboru můžete povolit nebo zakázat telemetrie moduly a inicializátory a nastavte parametry pro některé z nich.

Konfigurační soubor jmenuje `ApplicationInsights.config` nebo `ApplicationInsights.xml`v závislosti na typu aplikace. Se automaticky přidají do vašeho projektu při [instalaci Většina verzí SDK][start]. Je také přidán k web appu tak, že [Sledování stavu na serveru IIS][redfield], nebo když vyberete Appplication přehledy [v článku rozšíření OM nebo Azure webu](app-insights-azure-web-apps.md).

Není k dispozici odpovídající soubor řídit [SDK na webové stránce][client].

Tento dokument popisuje oddílů, které se zobrazí v konfiguraci souboru, jak budou určit komponent SDK, a které NuGet balíčky načíst tyto součásti.

## <a name="telemetry-modules-aspnet"></a>Moduly telemetrie (ASP.NET)

Každý modul telemetrie shromažďuje určitý typ dat a používá základní rozhraní API odešlete data. Moduly vám asi nainstaluje různých NuGet balíčky, které také přidat do souboru config požadované řádky.

Podívejte se na uzel v konfiguračního souboru pro každý modul. Pokud chcete zakázat modulu, odstranit uzel nebo komentář ji.



### <a name="dependency-tracking"></a>Závislost typu sledování

[Závislost typu sledování](app-insights-dependencies.md) shromažďuje telemetrie o hovory, které aplikace umožňuje a externí služby i databází. Pokud chcete povolit tento modul práce v serveru IIS, budete potřebovat k [instalaci sledování stavu][redfield]. Můžete ho v Azure webových aplikací nebo VMs, [Vyberte rozšíření přehledy aplikace](app-insights-azure-web-apps.md).

Můžete také napsat vlastní závislost kód pomocí rozhraní [TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)sledování.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet balíček.

### <a name="performance-collector"></a>Kolekcí výkonu

[Shromažďuje výkonnosti systému](app-insights-performance-counters.md) třeba procesoru a paměti sítě načíst ze služby IIS zařízení. Můžete zadat které čítače shromažďování, včetně výkonnosti, kterou jste vytvořili sami.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet balíček.


### <a name="application-insights-diagnostics-telemetry"></a>Aplikace přehledy diagnostiky Telemetrie

`DiagnosticsTelemetryModule` Ohlásí chyby v samotném kódu přístrojového vybavení přehledy aplikace. Například pokud kód nemáte přístup ke výkonnosti nebo `ITelemetryInitializer` výjimku. Do pole [Diagnostiky hledání]se zobrazí sledování telemetrie sledována tento modul[diagnostic]. Odešle dat diagnostiky dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet balíček. Pokud nainstalujete pouze tento balíček, není automaticky vytvoří soubor ApplicationInsights.config. 

### <a name="developer-mode"></a>Režimu Vývojář

`DeveloperModeWithDebuggerAttachedTelemetryModule`Vynutí přehledy aplikace `TelemetryChannel` odeslat data okamžitě, jeden telemetrie položku vždy, když je ladění připojen k procesu aplikace. To zkracuje dobu mezi okamžiku aplikace sleduje telemetrie a pokud se zobrazí na portálu přehledy aplikace. Způsobí, že významné režijních v procesoru a síťové šířky pásma.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Aplikace přehledy systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet balíčku

### <a name="web-request-tracking"></a>Žádost o web sledování

Sestavy [kód čas a výsledek odpovědi](app-insights-asp-net.md) požadavků HTTP. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet balíčku

### <a name="exception-tracking"></a>Výjimky sledování

`ExceptionTrackingTelemetryModule`skladeb neošetřené výjimky ve web appu. Přečtěte si téma [chyby a výjimky][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet balíčku


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-sleduje [výjimky zmeškané úkolu](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-sleduje neošetřené výjimky pro pracovníka role, služby systému windows a aplikace konzoly.
* [Aplikace přehledy systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet balíček.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Balíček Microsoft.ApplicationInsights obsahuje [základní rozhraní API](https://msdn.microsoft.com/library/mt420197.aspx) v SDK. Pomocí tohoto příkazu jiné telemetrie moduly a můžete taky [použít definovat vlastní telemetrie](app-insights-api-custom-events-metrics.md).

* Žádné položky v ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet balíček. Pokud instalujete jenom tento NuGet, vygeneruje se žádné config soubor.

## <a name="telemetry-channel"></a>Telemetrie kanálu

Kanál telemetrie spravuje vyrovnávací paměť a předávání telemetrie ke službě přehledy aplikace. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`je výchozí kanálu služby. Ho vyrovnávací paměti dat v paměti.
* `Microsoft.ApplicationInsights.PersistenceChannel`je alternativy pro konzoly aplikace. Všechna data, unflushed ho můžete uložit do trvalé úložiště až aplikace zavře a odešle se při spuštění aplikace znovu.


## <a name="telemetry-initializers-aspnet"></a>Telemetrie inicializátory (ASP.NET)

Telemetrie inicializátory nastavit vlastnosti kontext, které jsou odesílány spolu s jednotlivé položky telemetrie. 

Můžete [napsat vlastní inicializátory](app-insights-api-filtering-sampling.md#add-properties) nastavení vlastností pro kontextu.

Standardní inicializátory jsou všechno nastavené buď Web nebo WindowsServer NuGet balení:


* `AccountIdTelemetryInitializer`Nastaví vlastnost AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Nastaví vlastnost AuthenticatedUserId jako sami JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`aktualizace `RoleName` a `RoleInstance` vlastnosti `Device` kontext pro všechny položky telemetrie s informacemi o extrahovaných z Azure runtime prostředí.
* `BuildInfoConfigComponentVersionTelemetryInitializer`aktualizace `Version` vlastnost `Component` kontext pro všechny položky telemetrie s hodnotou extrahovaných z `BuildInfo.config` soubor vytvořené pomocí MS Build.
* `ClientIpHeaderTelemetryInitializer`aktualizace `Ip` vlastnost `Location` podle kontextu všechny položky telemetrie `X-Forwarded-For` záhlaví HTTP žádosti o.
* `DeviceTelemetryInitializer`Následující vlastnosti se aktualizuje `Device` kontext pro všechny položky telemetrie.
 - `Type`je nastavený na "PC"
 - `Id`nastavenou na název domény počítače, na kterém běží webové aplikace.
 - `OemName`je nastavena na hodnotu extrahovaných z `Win32_ComputerSystem.Manufacturer` polí pomocí WMI.
 - `Model`je nastavena na hodnotu extrahovaných z `Win32_ComputerSystem.Model` polí pomocí WMI.
 - `NetworkType`je nastavena na hodnotu extrahovaných z `NetworkInterface`.
 - `Language`je nastavený na název `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`aktualizace `RoleInstance` vlastnost `Device` kontext pro všechny položky telemetrie s názvem domény počítače, na kterém běží webové aplikace.
* `OperationNameTelemetryInitializer`aktualizace `Name` vlastnost `RequestTelemetry` a `Name` vlastnost `Operation` kontextu všechny položky telemetrie podle metoda HTTP, jakož i názvy řadiče domény ASP.NET MVC a akce zavolat a zpracování požadavku.
* `OperationIdTelemetryInitializer`nebo `OperationCorrelationTelemetryInitializer` aktualizace `Operation.Id` vlastnost kontextu všech položek telemetrie sledovány při zpracování žádost o se automaticky generované `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`aktualizace `Id` vlastnost `Session` kontext pro všechny položky telemetrie s hodnotou extrahovaných z `ai_session` souborů cookie generované kódem přístrojového vybavení ApplicationInsights JavaScript v prohlížeči uživatele. 
* `SyntheticTelemetryInitializer`nebo `SyntheticUserAgentTelemetryInitializer` aktualizace `User`, `Session` a `Operation` kontexty vlastnosti všechny položky telemetrie sledovány při zpracování žádost z syntetické zdroje, jako jsou dostupné testovat nebo hledání engine bot. Ve výchozím nastavení nezobrazuje [Metriky Explorer](app-insights-metrics-explorer.md) syntetické telemetrie. 

    `<Filters>` Nastavit identifikační vlastnosti žádosti.
* `UserAgentTelemetryInitializer`aktualizace `UserAgent` vlastnost `User` podle kontextu všechny položky telemetrie `User-Agent` záhlaví HTTP žádosti o.
* `UserTelemetryInitializer`aktualizace `Id` a `AcquisitionDate` vlastnosti `User` kontext pro všechny položky telemetrie s hodnotami extrahovaných z `ai_user` souborů cookie generované kódem přístrojového vybavení JavaScript přehledy aplikace spuštěné v prohlížeči uživatele.
* `WebTestTelemetryInitializer`Nastaví id uživatele, id relace a vlastnosti syntetické zdroje pro požadavků HTTP pocházejících z [dostupnost testů](app-insights-monitor-web-app-availability.md).
`<Filters>` Nastavit identifikační vlastnosti žádosti.

## <a name="telemetry-processors-aspnet"></a>Procesory telemetrie (ASP.NET)

Procesory telemetrie můžete filtrovat a změnit všechny požadované položky telemetrie těsně před odesláním ze sady SDK k portálu.

Můžete [napsat vlastní telemetrie procesorů](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Procesor telemetrie adaptivní odběru (z 2.0.0-beta3)

To je standardně. Pokud aplikace odešle spoustu telemetrie, odebere procesoru některých textů.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametr obsahuje cílem algoritmu se snaží dosáhnout. Každý výskyt SDK funguje nezávisle, pokud je server clusteru několika počítačích, skutečného objemu telemetrie se vynásobí příslušným způsobem.

[Další informace o odběr](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Procesor telemetrie odběr pevnou sazba (z 2.0.0-beta1)

Existuje standardní [Analytický nástroj vzorkování telemetrie procesor](app-insights-api-filtering-sampling.md#sampling) (z 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Parametry kanálu (Java)

Tyto parametry ovlivňuje Java SDK mají ukládat a vyprázdněte telemetrickými daty, která shromažďuje.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Počet telemetrie položky, které mohou být uloženy v sadě SDK v paměti úložiště. Při toto číslo, vyprázdnění vyrovnávací paměť telemetrie – tedy položky telemetrie posílají serveru přehledy aplikace.

-   Min: 1
-   Maximální: 1 000
-   Výchozí: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Určuje, jak často daty uloženými v úložišti v paměti by měl vyprázdnit (odeslaných interpretace aplikace).

-   Min: 1
-   Maximální: 300
-   Výchozí: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Určuje maximální velikost v Megabajtech vyhrazený k základnímu úložišti trvalý na místním disku. Toto úložiště položka se používá pro zachování telemetrie neúspěšných předávají koncový bod přehledy aplikace. Při splnění velikosti úložiště nové položky telemetrie budou ztraceny.

-   Min: 1
-   Maximální: 100
-   Výchozí: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Tato možnost určuje aplikace přehledy zdroje, ve kterém se zobrazí data. Obvykle vytvoříte samostatný zdroje s zvláštní klíč pro každou z aplikací.

Pokud chcete nastavit klíč dynamicky – například pokud chcete poslat výsledky z různých zdrojů – aplikace můžete vynechat klávesu z konfiguračního souboru a nastavení v kódu.

Pokud chcete nastavit klíč pro všechny výskyty TelemetryClient, včetně standardní telemetrie moduly v nastavit klíč TelemetryConfiguration.Active. Akce v metodu inicializace, například global.aspx.cs služby ASP.NET

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Pokud chcete odeslat určité skupiny události jinému zdroji, můžete nastavit klíč pro konkrétní TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Další informace o rozhraní API][api].

Chcete-li získat nový klíč, [Vytvoření nového zdroje na portálu aplikace přehledy][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

