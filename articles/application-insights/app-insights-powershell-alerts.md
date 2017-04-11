<properties
    pageTitle="Nastavení upozornění v aplikaci přehledy pomocí prostředí Powershell"
    description="Automatické konfiguraci aplikace přehledy získat e-mailů o metrických změny."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Nastavení upozornění v aplikaci přehledy pomocí prostředí PowerShell

Konfigurace [upozornění](app-insights-alerts.md) v [Přehledy aplikace Visual Studio](app-insights-overview.md)můžete automatizovat.

Kromě toho můžete [nastavit webhooks k automatizaci svou odpověď na upozornění](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Jednorázové instalace

Pokud jste s předplatným Azure před nepoužili Powershellu:

Nainstalujte modul Azure Powershellu v počítači, ve které chcete spustit skripty.

 * Instalace [platformy Microsoft Web (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Umožňuje instalaci služeb Microsoft Azure Powershell


## <a name="connect-to-azure"></a>Připojení k Azure

Spusťte Azure PowerShell a [Připojit se ke svému předplatnému](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Výstrahy

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Přidání oznámení


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Příklad 1

Pokud na server odpovědí na žádosti o HTTP, průměr víc než 5 minut, jsou nižší než 1 druhé, e-mailové mě. Zdrojů přehledy aplikace se nazývá IceCreamWebApp a je ve skupině prostředků Fabrikam. Jste vlastník předplatného Azure.

Identifikátor GUID je ID předplatného (ne přístrojového vybavení klávesu aplikace).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Příklad 2

Mám aplikace použiji [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) vykazování metriky s názvem "salesPerHour." Odešlete že e-mail, aby kolegové při "salesPerHour" poklesu pod 100, průměr delší než 24 hodin.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Metriky vykázaného pomocí [parametrů měření](app-insights-api-custom-events-metrics.md#properties) hovoru jinou sledování TrackEvent ATP trackPageView lze použít stejné pravidlo.

## <a name="metric-names"></a>Metriky názvy

Metriky název | Zobrazované jméno | Popis
---|---|---
`basicExceptionBrowser.count`|Výjimky prohlížeče|Počet nezachycené výjimky v prohlížeči.
`basicExceptionServer.count`|Výjimky serveru|Počet neošetřené výjimky vyvolané aplikace
`clientPerformance.clientProcess.value`|Čas zpracování klienta|Čas mezi příjem poslední bajt dokumentu, dokud načtení modelu DOM. Může být pořád zpracování asynchronních.
`clientPerformance.networkConnection.value`|Času připojení sítě načtení stránky| Doba, prohlížeči se pro připojení k síti. Může být 0, pokud v mezipaměti.
`clientPerformance.receiveRequest.value`|Příjem doba odezvy| Čas mezi prohlížeče posílat žádosti o spuštění zaslat odpověď.
`clientPerformance.sendRequest.value`|Odeslání žádosti o čas| Doba prohlížečem odeslat požadavek.
`clientPerformance.total.value`|Čas zatížení Prohlížeč stránky|Čas požadavku uživatele, dokud načtení modelu DOM, pomocí šablony stylů, skripty a obrázky.
`performanceCounter.available_bytes.value`|Dostupnou pamětí|Pole fyzicky paměti okamžitě procesu nebo systému.
`performanceCounter.io_data_bytes_per_sec.value`|Proces vstupu a výstupu sazba|Celkový počet bajtů sekundu číst a zapisovat soubory, sítě a zařízení.
`performanceCounter.number_of_exceps_thrown_per_sec`|výjimky sazba|Výjimky vyvolání sekundu.
`performanceCounter.percentage_processor_time.value`|Proces procesoru|Procento uplynulý čas všechny podprocesy procesu používá procesoru spuštění pokyny pro proces aplikací.
`performanceCounter.percentage_processor_total.value`|Procesor pro platformu času|Procento doba, po kterou procesor ve vláknech nečinný.
`performanceCounter.process_private_bytes.value`|Soukromé bajtů obrázku|Paměti výhradně přiřazené k provedení sledovanou procesů.
`performanceCounter.request_execution_time.value`|Doba provádění požadavku ASP.NET|Spuštění čas poslední požadavek.
`performanceCounter.requests_in_application_queue.value`|ASP.NET požadavků ve frontě spuštění|Délka fronty žádosti o aplikaci.
`performanceCounter.requests_per_sec`|Četnost požadavků ASP.NET|Úroková sazba všechny žádosti o aplikaci sekundu služby ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Závislost typu chyby|Počet neúspěšných hovorů serverové aplikace na externí zdroje.
`request.duration`|Doba odezvy serveru|Čas mezi příjem žádost HTTP a dalších dokončovacích prací odesláním odpovědi.
`request.rate`|Žádost o sazba|Úroková sazba všechny žádosti o aplikaci sekundu.
`requestFailed.count`|Žádosti o nezdařeném uložení|Počet HTTP požadavky, které za následek kód odpovědi > = 400
`view.count`|Zobrazení stránky|Počet hodnot v klientské uživatele požadavky na webovou stránku. Syntetické přenosy odfiltrované.
{vlastní metrických názvu}|{Názvu metrických}|Hodnotu metriky vykázaného [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) nebo [parametr rozměry sledování hovoru](app-insights-api-custom-events-metrics.md#properties).

Metriky odesílá různých telemetrie moduly:

Metriky skupiny | Modul kolekcí
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>zobrazení | [Prohlížeče JavaScript](app-insights-javascript.md)
performanceCounter | [Výkon](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Počet závislostí](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
žádosti o<br/>requestFailed|[Žádost o serveru](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Můžete [automatizovat svou odpověď na upozornění](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure zavolá webovou adresu podle svého výběru při umocněno upozornění.

## <a name="see-also"></a>Viz taky


* [Skript ke konfiguraci aplikace přehledy](app-insights-powershell-script-create-resource.md)
* [Vytvoření aplikace přehledy a testovací prostředky webu ze šablony](app-insights-powershell.md)
* [Automatizace spojovacích diagnostických nástrojů Microsoft Azure interpretace aplikace](app-insights-powershell-azure-diagnostics.md)
* [Automatické odpovědi na upozornění](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
