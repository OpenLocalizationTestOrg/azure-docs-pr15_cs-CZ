<properties 
    pageTitle="Jak vytvořit úloha zpracování analýzy dat pro analýzy toku | Microsoft Azure" 
    description="Vytvoření projektu zpracování analýzy dat pro analýzy toku | Přehled výukových segmentu cesty."
    keywords="zpracování analýzy dat"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Jak vytvořit úloha zpracování analýzy dat pro analýzy toku

Nejvyšší úrovně zdroje v Azure toku analýzy je úlohy analýzy proudu.  Je tvořen jeden či více zdrojů zadávání dat, dotazu vyjádření transformace dat a jeden nebo více výstup cílů, které jsou došlo k zápisu výsledky. Společná tyto povolit uživatelům provádění analýzy dat zpracování pro přenos dat scénáře.

Začít používat toku analýzy, začněte tak, že vytvoříte novou úlohu toku analýzy.  Všimněte si, že tato akce má žádné fakturační dopady až po spuštění projektu.

1.  Přihlaste se k online [Azure klasické portál](http://manage.windowsazure.com) nebo [Azure portálu](https://portal.azure.com/).
2.  Na portálu: **Klikněte na nový**, klikněte na **Datové služby** nebo **Technologie pro analýzu dat** v závislosti na portálu a potom klikněte na ** **Analýzy toku Azure** nebo toku technologie pro analýzu** a potom **Vytvořit**.

    ![Průvodce úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Vytvoření zpracování úlohy analýzy dat](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Zadejte požadovaná konfigurace pro danou úlohu toku analýzy.
    - Do pole **Název projektu** zadejte název k identifikaci úlohy toku analýzy. **Název úlohy** proběhne, zelená značka zaškrtnutí nabídne do pole název projektu. **Název úlohy** může obsahovat pouze alfanumerické znaky a "-" znaků a musí mít mezi 3 a 63 znaky.
    - Pomocí **oblasti** v Azure portál nebo **umístění** na portálu Azure určete zeměpisné polohy místo, kam chcete úlohu.
    - Pokud na portálu Azure, vyberte nebo vytvořte účet úložiště má být použit jako **Místního účtu sledování úložiště**. Tento účet úložiště se používá k ukládání monitorování dat pro všechny úlohami toku analýzy v této oblasti.
    - Pokud na portálu Azure, zadejte nový nebo existující **Pole Skupina zdroje** pro uložení související materiály pro aplikaci.

4.  Po konfiguraci nové možnosti analýzy toku úlohy klikněte na **Vytvořit úlohu analýzy proudu**. Může trvat několik minut úlohy toku analýzy vytvořit. Pokud chcete zkontrolovat stav, můžete sledovat průběh v centru oznámení.

    ![Rozbočovač notfications úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portálu analýzy dat zpracování úlohy vytvoření projektu](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Zobrazí se nová úloha se stavem **vytvořeno**. Všimněte si, že je zakázané tlačítko **Start** . Než začnete úlohy musíte nakonfigurovat úlohu vstupní, dotaz a výstup.

    ![Zpracování analýzy dat úloha stavu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portálu analýzy dat zpracování stavu úlohy projektu](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
