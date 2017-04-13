<properties 
    pageTitle="Jak spustit přenos úlohy v toku analýzy | Microsoft Azure" 
    description="Spouštění streamování úlohy v Azure toku analýzy | Přehled výukových segmentu cesty."
    keywords="přenos úlohy"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Jak spustit streamování úlohy v Azure toku analýzy

Při úlohu vstupní, dotazu a výstup všech zadáno, že můžete začít úlohy toku analýzy.

Zahájení práce:

1.  Na portálu Azure klasické na řídicím panelu projektu klikněte na tlačítko **Start** v dolní části stránky.

    ![Zahájení práce tlačítko](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Na portálu Azure klikněte na tlačítko **Start** v horní části stránky projektu.

    ![Azure portálu zahájení práce tlačítko](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Zadejte hodnotu **Start výstup** zjistit, kdy tuto úlohu začnou vytváření výstupu. Ve výchozím nastavení úloh, které dřív byly nezačali je **Čas zahájení práce**, což znamená, že úloha okamžitě začít data zpracování. **Vlastní** čas můžete zadat také v minulosti (využívající historických data) nebo později (a zpoždění zpracování až na pozdější čas). Pro názvy případů při úlohy má dříve spustit a zastavit, možnost **Posledního přerušili** je k dispozici při obnovení úlohy z posledního výstup a vyhněte se ztrátou dat.  

    ![Spustit přenos pracovní čas](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portálu zahájení streamování úlohy čas](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Potvrďte výběr. Stav úlohy se změní na *začátek* a bude brzy přesuňte *spuštění* po spuštění úlohy. Můžete sledovat průběh **Spustit** operaci v **Centrální oznámení**:

    ![přenos průběhu projektu](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure portál streamování průběhu projektu](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
