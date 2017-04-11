Existují určité datové limity počtu metriky a událostí za aplikace (to znamená pro klíč přístrojového vybavení). 

Omezení, závisí na [ceny osy](https://azure.microsoft.com/pricing/details/application-insights/) , které zvolíte.

**Zdroje** | **Výchozí Limit** | **Maximální Limit**
-------- | ------------- | -------------
Relace datových bodů<sup>1, 2</sup> za měsíc | neomezený | 
Celková datové body za měsíc pro žádost o událost, závislosti, sledování, výjimky a zobrazení stránky | miliónů 5 | 50 milionů<sup>3</sup>
[Sledování a protokolu](../articles/application-insights/app-insights-search-diagnostic-logs.md) rychlost dat | 200 dp/s | 500 dp/s
Rychlost [výjimky](../articles/application-insights/app-insights-asp-net-exceptions.md) dat | 50 dp/s | 50 dp/s
Součet dat sazbu žádost, události, závislost a telemetrie zobrazení stránky | 200 dp/s | 500 dp/s
Uchovávání informací neformátovaná data pro [hledání](../articles/application-insights/app-insights-diagnostic-search.md) a [technologie pro analýzu](../articles/application-insights/app-insights-analytics.md) | 7 dnů
Uchovávání informací souhrnná data pro [metriky Průzkumníka](../articles/application-insights/app-insights-metrics-explorer.md) | 90 dní
[Vlastnost](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) název počet | 100 |
Délka názvu vlastnost | 150 | 
Vlastnost délka | 8192 | 
Sledování a výjimce délka zprávy | 10000 |
Počet název [míru](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Délka názvu metrických |  150 | 
[Dostupnost testů](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> na datový bod je jednotlivé metrických hodnotu nebo událost, s připojeným vlastnosti a měření.

Relace A <sup>2</sup> datový bod protokoly zahájení nebo ukončení relace a protokoly identita uživatele.

<sup>3</sup> máte možnost si zakoupit další kapacitu za 50 milionů.
 
[O ceny a kvót v aplikaci přehledy](../articles/application-insights/app-insights-pricing.md)
