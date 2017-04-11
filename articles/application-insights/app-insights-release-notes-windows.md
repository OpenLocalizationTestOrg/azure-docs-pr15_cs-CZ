<properties 
    pageTitle="Poznámky k verzi pro přehledy aplikace pro systém Windows" 
    description="Nejnovější aktualizace Windows SDK úložiště přihlašovacích údajů." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Poznámky k verzi pro přehledy aplikace SDK pro Windows Phone a ukládání

Přehledy SDK aplikace odešle telemetrie o aplikace live pro [Přehledy aplikace](https://azure.microsoft.com/services/application-insights/), kde můžete analyzovat její využití a výkonu.


## <a name="version-111"></a>Verze 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Oprava zablokování během dojde k chybě při použití programu Silverlight SDK Windows Phone. Po provedení této změny budou všechny dojde k chybě, která se stane ~ 2 sekund po volání WindowsAppInitialier.InitializeAsync(...) zachován na disk a odešle dalšího spuštění aplikace. V případě zhroucení před ~ 2 sekund po hovor budou ignorovat.  
- Nastavení závislostí NuGet konkrétní verzi Core a Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Základní SDK

- Základní je spravovaný v github. Budoucí vydání poznámky Core SDK najdete [v github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Verze 1.1

### <a name="core-sdk"></a>Základní SDK

- SDK teď představuje nový typ telemetrie ```DependencyTelemetry``` který obsahuje informace o závislostech hovor z aplikace
- Nová metoda ```TelemetryClient.TrackDependency``` umožňuje odesílat informace o závislostech hovory z aplikace

## <a name="version-100"></a>Verze 1.0.0

### <a name="windows-app-sdk"></a>Aplikace Windows SDK

- Nové inicializace pro aplikace systému Windows. Nové `WindowsAppInitializer` třídy s `InitializeAsync()` metoda umožňuje zavádění inicializace SDK kolekce webů. Tato změna povolit přesněji a vylepšení výkonu inicializace významný aplikace nad předchozí postup ApplicationInsights.config.
- DeveloperMode se už automaticky nastaví. Postup změny chování DeveloperMode je nutné zadat kód.
- NuGet balíčku už vloží ApplicationInsights.config. Doporučujeme používat novou WindowsAppInitializer při přidání ručně NuGet balíčku.
- Pouze čtení ApplicationInsights.config `<InstrumentationKey>`, se v předvolby pro nastavení WindowsAppInitializer ignorují další nastavení.
- Úložiště Market bude automaticky shromážděná SDK.
- Velké množství opravy chyb, vylepšení stability a vylepšení výkonu.

### <a name="core-sdk"></a>Základní SDK

- Soubor ApplicationInsights.config je už requiered. A ne přidané balíčkem NuGet. Konfigurace může být plně zadaný v kódu.
- NuGet balíčku už přidá souboru cílů do vašeho řešení. To odebere automatické nastavení DeveloperMode během sestavení ladění. DeveloperMode je třeba ručně nastavit v kódu.

## <a name="version-017"></a>Verze 0.17

### <a name="windows-app-sdk"></a>Aplikace Windows SDK

- Windows SDK aplikaci teď podporuje univerzální aplikace Windows vytvořena proti Windows 10 technical preview a s RC a 2015.

### <a name="core-sdk"></a>Základní SDK

- Výchozí nastavení TelemetryClient s InMemoryChannel.
- Přidání nového rozhraní API `TelemetryClient.Flush()`. Tento způsob zarovnání spustí okamžité blokování nahrávání všech telemetrie přihlášení k lyncu pro klienta. Díky ruční spuštění nahrávání před ukončení procesu.
- Přidání NuGet balíčku .net 4.5 cíl. Tento cíl nemá žádné externí závislosti (odebrané BCL a ZDROJ_UDÁLOSTI závislosti).

## <a name="version-016"></a>Verze číslo 0,16 

Náhled 2015 04 28

- SDK nyní podporuje DNX cílové platformy a umožňují sledovat [.NET Core framework](http://www.dotnetfoundation.org/NETCore5) aplikací.
- Výskyty ```TelemetryClient``` Neukládat do mezipaměti přístrojového vybavení klíč už. Nyní pokud nebyla nastavena přístrojového vybavení klíč ```TelemetryClient``` explicitně ```InstrumentationKey``` bude vracet hodnotu null. Řešení problému, když nastavíte ```TelemetryConfiguration.Active.InstrumentationKey``` po již shromážděny některé telemetrie, moduly telemetrie jako závislostí kolekcí, web požadavky shromažďování a výkonu čítače shromažďování dat bude používat nový klíč přístrojového vybavení.

## <a name="version-015"></a>Verze 0,15

- Nová vlastnost ```Operation.SyntheticSource``` aktuálně dostupné na ```TelemetryContext```. Teď můžete označit položky telemetrie jako "není přenosy skutečného uživatele" a určit, jak byla vytvořena tato přenosy. Jako příklad tak, že nastavení této vlastnosti můžete přenosy odlišit od automatizaci test z zatížení zkušební provoz.
- Použití logických operátorů kanálu byl přesunut do samostatných NuGet s názvem Microsoft.ApplicationInsights.PersistenceChannel. Výchozí kanálu se teď nazývá InMemoryChannel
- Nová metoda ```TelemetryClient.Flush``` umožní synchronní vyprázdnění telemetrie položek z vyrovnávací paměť

## <a name="version-013"></a>Verze 0,13

Žádné poznámky k verzi pro starší verze k dispozici. 
