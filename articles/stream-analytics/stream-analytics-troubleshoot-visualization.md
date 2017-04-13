<properties
    pageTitle="Vizualizace a řešení potíží s toku analýzy úlohy | Microsoft Azure"
    description="Naučte se vizualizace příležitosti úlohy toku Analytics pro samoobslužné Poradce při potížích s pomocí funkce diagramu diagnostických nástrojů."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Vizualizace a řešení potíží s úlohy toku analýzy

V toku analýzy s jinými technologiemi cloudové řešení potíží s je někdy potřeba o proč úlohy nevytvoří očekávaného výstupu (nebo všechny výstup pro věci). S tímto nezapomeňte toku analýzy poskytuje možnost pro vizualizaci streamování projektu. To je také užitečné jako prostředek modelování a má výhodu straně pro tyto vyžadující přečtěte následující dokumentaci pro své práce.

V panelu vizualizace jsou viditelné a je spuštěný dotaz a potom všechny výstupy nakonfigurované vstupní hodnoty. Problémy s připojením či konfigurace, může být lépe poznat a mohou být také užitečné zobrazíte vizuální znázornění konfiguraci.

## <a name="using-the-diagnosis-diagram-tool"></a>Pomocí nástroje pro diagnostiku diagramu

Přístup k této visualizer, jednoduše klikněte na tlačítko "Diagnostiky diagramu" v "Nastavení" zásuvné z úlohy toku analýzy.

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Každý vstupní a výstupní je barevně označíte aktuální stav této součásti, jak je ukázáno v následujícím příkladu.

![Stream-Analytics-troubleshoot-Visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Pokud chce uživatel podívejte se na kroky intermediate dotazu pochopit vzorky toku dat do projektu, nástroj vizualizaci poskytuje zobrazení hierarchické dotaz do jeho součásti kroky a toku pořadí. Po kliknutí na každý krok dotazu se zobrazí v odpovídající části dotazu úpravy podokno jak je znázorněno. 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
