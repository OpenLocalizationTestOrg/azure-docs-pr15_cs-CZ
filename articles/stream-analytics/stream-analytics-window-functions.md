<properties
    pageTitle="Úvod k funkcím toku analýzy okno | Microsoft Azure"
    description="Další informace o tři okno funkcích v toku analýzy (tumbling vše, klouzavé)."
    keywords="tumbling okno klouzavé okno vše okna"
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


# <a name="introduction-to-stream-analytics-window-functions"></a>Úvod k funkcím okno analýzy toku

V mnoha spuštěný streamování scénáře je nutné provádět operace pouze data obsažená v časový windows. Nativní podpora práce s okny funkce je klíčové funkce Azure toku analýzy, přesune jehly na produktivita při vytváření zpracování složitých toku úloh. Technologie pro analýzu toku umožňuje vývojářům použití funkcí windows [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx) [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) a [**posuvné**](https://msdn.microsoft.com/library/dn835051.aspx) časový operace v datových proudů data. Je vhodné poznamenat, že všechny operace v [okně](https://msdn.microsoft.com/library/dn835019.aspx) výstupu výsledky na **Konec** okna. Výstup okna budou jedné události podle agregační funkce používá. Událost bude obsahovat časové razítko na konec okna a všechny funkce okno jsou definované s pevnou délkou. Nakonec je důležité mít na paměti, že všechny funkce okno má být použita v klauzuli [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) .

![Okno analýzy toku funguje koncepty](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Okno tumbling

Tumbling okno, které funkce se používají segmentech toku dat do různých čas segmentů a spusťte funkci, jako je třeba v příkladu níže. Klíčové differentiators okna Tumbling se, že budou opakovat, nepřekrývaly a události nelze patří do více než jedno okno tumbling.

![Funkce analýzy okno toku tumbling Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Přepínání okna

Přepínání okno funkce směrování dopředu v čase podle pevné období. Je možné snadno představit je jako Tumbling okénka, která můžete překrývat, takže události můžete patří do více než jednu sadu výsledků okno Hopping. Vytvoření okna Hopping shodný Tumbling by okno jednu, stačí zadat velikost směrování stejný jako velikosti okna. 

![Funkce analýzy okno toku vše Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Posuvná okna

Posuvná okna funkcí, mezi které na rozdíl od Tumbling nebo Hopping windows plodiny výstup **pouze** při výskytu události. Všechna okna budou mít aspoň jednu událost a okna nepřetržitě slouží k přesunutí vpřed tak, že € (epsilon). Jako vše Windows události můžete do kterých patříte více klouzavé oken.

![Funkce analýzy okno toku klouzavé Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Získání nápovědy pomocí okna funkcí

Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
