<properties 
    pageTitle="Aplikace mapy v aplikaci přehledy | Microsoft Azure" 
    description="Vizuální prezentaci závislosti mezi součásti aplikace označené klíčových ukazatelů výkonu a upozornění." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Aplikace mapy v aplikaci přehledy

V [Přehledy aplikace Visual Studio](app-insights-overview.md)je aplikace mapy vizuálním rozložení vztahů závislosti součástí aplikace. Jednotlivé součásti zobrazují ukazatele KPI třeba načítání výkonu, chyb a upozornění, pomůže objevit všechny component způsobují problém s výkonem nebo selhání. Který vás přesměruje na prostřednictvím z kterékoli komponenty podrobnější diagnostiky z aplikace přehledy, a pokud aplikace používá Azure služby – Azure diagnostických nástrojů, jako je doporučení Advisor databáze SQL.

Je další grafy můžete připnout mapování aplikace Azure řídicího panelu, kde je plně funkční. 

## <a name="open-the-application-map"></a>Otevřete aplikaci mapy

Otevřete mapu z zásuvné přehled aplikace:

![Otevřete aplikaci mapy](./media/app-insights-app-map/01.png)

![aplikace mapy](./media/app-insights-app-map/02.png)

Mapy zobrazí:

* Dostupnost testů
* Součásti klientského počítače (sledovat pomocí JavaScript SDK)
* Součást straně serveru
* Závislosti součásti klienta a serveru

Můžete rozbalit nebo sbalit závislost propojit skupiny:

![Sbalit](./media/app-insights-app-map/03.png)
 
Pokud máte velký počet závislostí jednoho typu (SQL, HTTP atd.), se mohou objevit seskupené. 


![seskupené závislostí](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Místo, kde můžete problémy

Každý uzel má ukazatele výkonu důležité, například zatížení, výkon a selhání sazeb pro tuto složku. 

Varovné ikony zvýrazněna možných problémech. Oranžová upozornění znamená, že v žádosti o, závislost hovorů nebo zobrazení stránky jsou k chybám. Červené znamená výpadek vyšší než 5 %.


![Chyba při ikony](./media/app-insights-app-map/04.png)

 
Aktivní upozornění také zobrazit nahoru: 


![aktivní upozornění](./media/app-insights-app-map/05.png)
 
Pokud používáte SQL Azure, je ikona, která ukazuje, kdy jsou doporučení ohledně toho, jak můžete zlepšit výkonu. 


![Azure doporučení](./media/app-insights-app-map/06.png)

Klikněte na libovolnou ikonu získat další informace:


![Azure doporučení](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnostické klikněte na Procházet

Každý z uzlů na mapě nabízí cílových klikněte na Procházet diagnostických nástrojů. Možnosti se liší v závislosti na typu uzel.

![Možnosti serveru](./media/app-insights-app-map/09.png)

 
Součástí, které jsou hostované v Azure použijte možnosti přímé odkazy na ně.


## <a name="filters-and-time-range"></a>Filtry a časového rozsahu

Ve výchozím nastavení mapy je uveden souhrn všech dostupných údajů o zvoleného časového rozsahu. Ale můžete ho, aby obsahoval jenom určité operace názvy závislosti filtrovat.

* Název operace: Jedná se o zobrazení stránky a typů požadavků na straně serveru. Vyberete tuto možnost mapy zobrazuje klíčový ukazatel výkonu na uzel straně klienta a serveru pro s vybranými operacemi. Zobrazí v kontextu jednotlivých těchto akcí se označují jako závislostí.
* Závislost typu základní název: Tato volba zahrnuje závislosti straně prohlížeče AJAX a závislosti na straně serveru. Pokud sestava telemetrie vlastní závislost pomocí rozhraní API TrackDependency se zobrazí také tady. Můžete vybrat závislosti zobrazit na mapě. Upozorňujeme, že v současné době to nebude filtrovat požadavky na straně serveru nebo zobrazení stránky straně klienta.


![Nastavení filtrů](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Ukládání filtrů

Pokud chcete uložit filtry, které jste použili, připnete filtrovaného zobrazení na [řídicí panel](app-insights-dashboards.md).


![Připnout do řídicího panelu](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Zpětné vazby

Zadejte [svůj názor pomocí příkazu portálu svůj názor](app-insights-get-dev-support.md).


![Obrázek MapLink-1](./media/app-insights-app-map/13.png)


