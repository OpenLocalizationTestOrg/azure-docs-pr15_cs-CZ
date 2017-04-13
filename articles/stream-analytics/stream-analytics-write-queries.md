<properties 
    pageTitle="Jak psát dotazy v toku analýzy | Microsoft Azure" 
    description="Psát dotazy v toku technologie pro analýzu a data dotazu | Přehled výukových segmentu cesty."
    keywords="jak psát dotazy, dotaz dat, vytvořit dotaz, psaní dotazů"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Jak psát dotazy v toku analýzy

Psaní dotazy týkající se zpracování logiky v Azure toku analýzy toku prováděna jako "postavení dotaz", který je definován před úlohy spustí a provést na datech, protože nedosáhnete projektu. Transformace dat je vyjádřený v jazyce SQL profesionálové dotazu, který je převážně podmnožinu T-SQL s některá rozšíření přidaný jazyk jako [práce s okny](https://msdn.microsoft.com/library/azure/dn835019.aspx) použitý pro vyjádření časový sémantiku.

## <a name="writing-queries"></a>Psaní dotazů: ##

1. V toku analýzy práce na portálu Správa Azure klikněte na **dotaz**.

    ![Výběrový dotaz](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Na portálu Azure klikněte na **dotaz**.

    ![Vyberte náhledu dotazu](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Nové úlohy mají šablony dotazu, které vám pomůžou začít pracovat. Šablona dotazu provede "předávací" dotaz, který projekty všechna pole ze vstupní události do výstupu.  

    - Pokud jste definovali alespoň jeden vstupní a výstupní práce, nahraďte zástupný symbol "[YourOutputAlias]" a "[YourInputAlias]" pole s aliasy vstupní a výstupní, který chcete použít jako první. Kromě toho můžete pořád vytváření a testování dotazu na portálu klasické Azure bez definující vstupů a výstupů na projektu.
    - Pokud chcete provést další zpracování než jednoduchý předávací, můžete upravovat definice dotazu. Začít s vytváření dotazů, podívejte se na některé běžné dotazu vzorků ukládány [tady](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Dotaz na data okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Pro ověření dat query funguje: ##

Můžete otestovat, aby dotazu se chová podle očekávání spuštěním v prohlížeči přes jeden nebo víc místní JSON souborů obsahující testovací data. To nebude zahájení projektu nebo mají vliv na všechny fakturace.

> [AZURE.NOTE] Aktuálně testování dotazu v prohlížeči nepodporuje na portálu Azure.  

1.  Zkontrolujte, že dotazu (v opačném případě tlačítko testovacího bude zakázán) jsou bez chyb a potom klikněte na tlačítko Testovat.  

    ![Dotaz na data Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Zobrazí se výzva k zadání souborů pro jednotlivá pole v dotazu odkazuje vstupů. V tomto příkladu šablony dotazu zůstane jako-je, aby se dialogové okno dotazování na vstup s názvem "yourinputalias".  

    ![Testovat dotaz dat](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Přejděte na testovací soubor. Několik ukázkové soubory jsou dostupné na [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) a můžete taky ukázková data načtete z vlastní vstupů toku dat pomocí funkce ukázkových dat na kartě vstupů.  

    ![Vstupní dotazu](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Po zavření dialogového okna, spustí dotaz přes testovací data a uvidíte výsledky v dolní části stránky dotazu.  

    ![Souhrn dotazu](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
