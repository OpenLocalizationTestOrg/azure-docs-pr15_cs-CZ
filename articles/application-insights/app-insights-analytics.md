<properties 
    pageTitle="Technologie pro analýzu – nástroj výkonné vyhledávání aplikace přehledy | Microsoft Azure" 
    description="Přehled analýzy, nástroj výkonné diagnostiky hledání přehledy aplikace. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Technologie pro analýzu v aplikaci přehledy


[Technologie pro analýzu](app-insights-analytics.md) je funkce výkonné vyhledávání [Aplikace přehledy](app-insights-overview.md). Tyto stránky popisují lanquage dotazu analýzy. 

* **[Podívejte se na úvodní video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Vyzkoušení analýzy na naše simulovaný data](https://analytics.applicationinsights.io/demo)** při aplikace není odesílání dat pro přehledy aplikaci ještě.

## <a name="queries-in-analytics"></a>Dotazy v analýzy
 
Typické dotaz je ke *zdrojové* tabulce následovaný řadu *operátory* odděleni `|`. 

Například Pojďme zjistit, jaké čas občanů Hyderabad Vyzkoušejte naše web app. A při tom, že tam, podíváme se, jaký výsledek kódy vracejí do jejich požadavků HTTP. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Jsme určení počtu jedinečných klienta IP adresy, je seskupíte podle hodin dne za posledních 7 dnů. 

Pojďme zobrazení výsledků během prezentace pruhového grafu výběru poskládat výsledky z různých odpovědí kódy:

![Vyberte pruhový graf, x a y osy a potom segmentace](./media/app-insights-analytics/020.png)

Vypadá to naše aplikace je nejoblíbenější lunchtime a rozložené čas v Hyderabad. (A jsme měli analýzu kódy těch 500)


Existují také výkonné statistické operace:

![](./media/app-insights-analytics/025.png)


Jazyk obsahuje mnoho atraktivní funkcí:

* [Filtr](app-insights-analytics-reference.md#where-operator) telemetrie nezpracovanými aplikace tak, že všechna pole, včetně vašich vlastních vlastností a metriky.
* [Připojit se ke](app-insights-analytics-reference.md#join-operator) více tabulek – correlate žádosti o zobrazení stránky, závislost hovory, výjimky a protokolu sleduje.
* Výkonné statistické funkce [agregace](app-insights-analytics-reference.md#aggregations).
* Stejně tak výkonné jako SQL, ale mnohem jednodušší složitých dotazů: místo vnořené příkazy kanálu dat z jedné základní operace na další.
* Okamžité a výkonných vizualizace.







## <a name="connect-to-your-application-insights-data"></a>Připojení k datům aplikace přehledy


Otevření analýzy z vaší aplikaci [zásuvné přehled](app-insights-dashboards.md) v aplikaci přehledy: 

![Otevřete portal.azure.com, otevřete aplikaci přehledy zdroje a klikněte na analýza.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Omezení

V současné době jsou omezené těsně nad týden minulých dat výsledků dotazu.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Další kroky


* Doporučujeme že začínat [prohlídky jazyk](app-insights-analytics-tour.md).