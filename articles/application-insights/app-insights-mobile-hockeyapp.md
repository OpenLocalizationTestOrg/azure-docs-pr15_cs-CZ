<properties
    pageTitle="Sledování výkonu pro mobilní webové aplikace pomocí analýzy vývojář | Microsoft Azure"
    description="Aplikace výkonu a využití sledování pro vývojáře mobilní aplikace. , plochy, webové služby a back-end aplikace s HockeyApp a přehledech aplikace."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobilní analýzy s HockeyApp a přehledech aplikace

Sledujte výkon a použití te000126961 beta testu a nasazené přenosných a stolních aplikací klasické [HockeyApp](https://hockeyapp.net/). Sledujte podpůrné webové služby back-end aplikace a s [Přehledy aplikace Visual Studio](app-insights-overview.md). Analýza dat z klienta a serveru aplikací v technologie pro analýzu a zobrazit grafy vedle sebe ve Azure řídicího panelu.

Microsoft Developer Analytics vám přináší dva řešení pro sledování výkonu aplikace:

* Aplikace **HockeyApp Mobile** a plochy.
 * Distribuce zkušební verze pro vybrané uživatele.
 * Docházet k chybám analýzy.
 * Vlastní telemetrie pro analýzy využití.
* **Aplikace přehledy pro weby** a služeb a back-end aplikace.
 * Metriky výkonu a využití a upozornění.
 * Vytváření sestav výjimky a diagnostické trasování.
 * Diagnostiky hledání s filtrování a související odkazy telemetrie.

Obou řešení nabízejí:

 * Výkonné **[analytické dotazovací jazyk](app-insights-analytics.md)** pro diagnostiku a analýzy.
 * **[Export dat](app-insights-export-telemetry.md)** do vlastního úložiště.
 * **Integrované řídicího panelu** zobrazení analytických grafů a tabulek.

## <a name="monitor-your-app-components"></a>Sledování součásti aplikace

Postupujte podle pokynů na těchto stránkách nainstalujete SDK v kódu a spusťte sledování aplikace.

### <a name="web-apps---application-insights"></a>Web apps – přehledy aplikace

* [V prohlížeči ASP.NET](app-insights-asp-net.md) 
* [Java v prohlížeči](app-insights-java-get-started.md)
* [Node.js web appu](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Služby Azure Cloud Services](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobilní aplikace - HockeyApp

* [aplikace iOS](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X aplikace](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Aplikace v androidu](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Aplikace univerzální Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Aplikace Windows Phone 8 nebo 8.1.](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows prezentaci Foundation aplikace](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Pro Windows aplikace klasické pracovní plochy doporučujeme HockeyApp. Ale můžete to udělat taky [Odeslat telemetrie z aplikace Windows interpretace aplikace](app-insights-windows-desktop.md). Můžete to udělat experiment s přehledy aplikace.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Technologie pro analýzu a Export pro HockeyApp telemetrie

Můžete prozkoumat HockeyApp vlastní a protokolování telemetrie pomocí technologie pro analýzu a nepřetržitý exportovat funkcí aplikace přehledy nastavením [Most](app-insights-hockeyapp-bridge-app.md).




