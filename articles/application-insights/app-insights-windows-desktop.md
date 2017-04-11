<properties 
    pageTitle="Sledování použití a výkonu pro aplikace klasické pracovní plochy systému Windows" 
    description="Analýzu použití z desktopové aplikace Windows s HockeyApp a přehledech aplikace." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Sledování použití a výkonu v aplikacích pro počítač s Windows

*Přehledy aplikace je v náhledu.*

[Přehledy aplikace Visual Studio](app-insights-overview.md) a [HockeyApp](https://hockeyapp.net) umožňují sledovat nasazeném aplikace pro použití a výkonu.

> [AZURE.IMPORTANT] Doporučujeme [HockeyApp](https://hockeyapp.net) distribuce a sledování aplikací plochy a zařízení. S HockeyApp můžete můžete spravovat distribuční živou testování a názory uživatelů, jakož i sledovat sestav využití a dojde k chybě. Můžete taky [exportovat a dotaz telemetrie s analýzy](app-insights-hockeyapp-bridge-app.md).

> Telemetrie odesílat interpretace aplikace z desktopové aplikace, toto je však hlavně užitečné pro účely ladění a pokusné.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Posílání telemetrie interpretace aplikace z aplikace systému Windows

1. Na [portálu Azure](https://portal.azure.com), [Vytvořit přehledy aplikace zdroje](app-insights-create-new-resource.md). Typ aplikace zvolte ASP.NET aplikace.
2. Přepnout kopii klávesu přístrojového vybavení. Najdete klíč v Essentials rozevíracího seznamu nového prostředku, který jste právě vytvořili. 
3. Ve Visual Studiu upravte NuGet balíčky aplikace project a přidejte Microsoft.ApplicationInsights.WindowsServer. (Nebo vyberte Microsoft.ApplicationInsights, pokud chcete úplné rozhraní API bez moduly kolekce standardní telemetrie.)
4. Nastavte klíč přístrojového vybavení v kódu:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*kód*`";` 

    nebo v ApplicationInsights.config (Pokud jste si nainstalovali některou z balíčků standardní telemetrie):
 
    `<InstrumentationKey>`*kód*`</InstrumentationKey>` 

    Pokud používáte ApplicationInsights.config, ujistěte se, že vlastností v okně Průzkumník jsou nastavené **Akce sestavení = obsah, kopírovat do adresáře výstup = kopírovat**.
5. [Použití rozhraní API](app-insights-api-custom-events-metrics.md) pro odeslání telemetrie.
6. Spusťte aplikaci a najdete v článku telemetrie v zdroj, který jste vytvořili v portálu Azure.

## <a name="telemetry"></a>Příklad

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Další kroky

* [Vytvoření řídicího panelu](app-insights-dashboards.md)
* [Diagnostiky hledání](app-insights-diagnostic-search.md)
* [Prozkoumání metriky](app-insights-metrics-explorer.md)
* [Psaní analýzy dotazů](app-insights-analytics.md)
 
