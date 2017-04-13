<properties 
    pageTitle="Principy datového proudu analýzy úlohy sledování | Microsoft Azure" 
    description="Principy datového proudu analýzy úlohy sledování" 
    keywords="sledování dotazu"
    services="stream-analytics" 
    documentationCenter="" 
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

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Princip monitorování projektu toku analýzy a sledování dotazů

## <a name="introduction-the-monitor-page"></a>Úvod: Stránka sledování

Na portálu Správa Azure a Azure portál ukazovat metriky klíčové výkonu, které slouží ke sledování a odstraňování případných problémů výkonu dotazu a úlohy. 

Na portálu Správa Azure klikněte na kartě **Monitor** spuštěná úloha toku analýzy zobrazíte tyto metriky. Dojde ke zpoždění nejvíce 1 minuty vyjádřené pomocí měřítka neprojevují na stránce sledování.  

  ![Monitorování projektu řídicího panelu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Na portálu Azure procházením toku analýzy projektu zajímají vidět metriky pro a získáte v části **Sledování** .  

  ![Azure portál monitorování projektu řídicího panelu](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Při prvním vytvoření toku analýzy úlohy v oblasti, musíte se ke konfiguraci diagnostických nástrojů pro danou oblast. K tomuto účelu se zobrazí klikněte na libovolné místo v části **Sledování** a zásuvné **diagnostických nástrojů** . Tady můžete povolit diagnostických nástrojů a zadejte účet úložiště pro sledování data.  

  ![Azure portál konfigurace dotazu diagnostických nástrojů](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metriky umožňující toku analýzy


| Metriky | Definice |
|--------|-------------|
| Využití % SH | Využití jednotek streamování přiřazené k projektu na kartě Měřítko úlohy. Tento indikátor dosažení 80 % nebo nad, je vysoká pravděpodobnost, že zpracování události může být zpožděno nebo zastavení vytváření průběh. |
| Zadávání události | Množství dat dostali toku analýzy projektu v počet případů. Mohou sloužit k ověření, že události jsou odesílány ke zdroji zadávání. |
| Výstup události | Velikost dat odeslaných úlohy toku analýzy na cílový výstup v počet případů. |
| Události mimo pořadí | Počet soutěží dostali mimo pořadí, které byly nezobrazí nebo zadaných upravená časové razítko na základě zásad řazení události. To může dojít k ovlivnění konfigurací nastavení okna odchylky nepřítomnosti v pořádku. |
| Chybám v převodu | Počet chybám v převodu vzniklé úlohy toku analýzy. |
| Chyby za běhu | Číslo chyby, ke kterým dojde při provádění analýzy toku úlohy. |
| Nejpozději možné vstupní události | Počet soutěží příchozí nejpozději možné ze zdroje, které mají buď nezobrazí nebo jejich časové razítko byl upraven podle konfigurace zásad pořadí událostí nastavení zpoždění doručení odchylky okna. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Přizpůsobení sledování na portálu Správa Azure ##

Až 6 metriky mohou být zobrazena na graf.

Pokud chcete přepnout mezi zobrazením relativních hodnot (konečná hodnota jenom pro každou míru) a absolutní hodnoty (osy Y zobrazí), vyberte relativní a absolutní v horní části grafu.

  ![Relativní absolutní monitor dotazu](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Metriky je možné zobrazit v grafu monitoru v agregacích 1 hodinu, 12 hodin, 24 hodin nebo 7 dní.

Chcete-li změnit časový rozsah graf zobrazuje metriky vyberte hodnotu 1 hodina, 24 hodin nebo 7 dní v horní části grafu.

  ![Sledování dotazu časového měřítka](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Můžete nastavit pravidla, která může upozornění e-mailem v případě, že úlohy ve výkresu protínají definovaný mezní hodnota. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Přizpůsobení sledování na portálu Azure ##

Můžete upravit typ grafu, metriky uveden a načasovat oblasti v nastavení upravte grafu. Další informace najdete v tématu [přizpůsobení sledování](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure portálu dotazu Monitor časového měřítka](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Stav úlohy

Stav toku analýzy úloh můžete zobrazit na portálu klasické Azure, kde se zobrazí seznam úloh. Klepnutím na ikonu toku analýzy na portálu klasické Azure uvidíte seznam úloh.

| Stav | Definice |
|--------|------------|
| Vytvoření | Projekt byl vytvořen, ale nebyla spuštěna. |
| Spuštění | Uživatel klikl při spuštění úlohy a zahájení projektu |
| Spuštění | Úlohy přiřazen zpracování vstupní nebo čeká na zpracování vstupní. Pokud úkoly zobrazí stav spuštění bez vytváření výstup, je pravděpodobné, že zpracování dat časového intervalu velká nebo jestli logika dotazu je složité. Může být jiného důvodu, že momentálně není k dispozici žádná data odesílaného do projektu. |
| Zastavení | Uživatel klikl na ukončit úlohu a ukončení projektu. |
| Přerušili | Úkoly se zastavil. |
| Sníženou kvalitu. | Tento stav označuje, že úlohy toku analýzy vzniku přechodná chyb (pro ex. Vstupní a výstupní chyby, zpracování chyb, chyby při převodu atd.). Úlohy spuštěné, avšak existují spoustu chyby generovaná. Tento úkol vyžaduje pozornost zákazníka a zákazníka uvidíte operace protokoly chyb. |
| Se nezdařila. | To znamená, že úlohy selže kvůli chyby a zpracování přestal. Zákazník musí pohled na protokoly operace k ladění chyb. |
| Odstranění | Tento údaj označuje, že Probíhá odstranění úlohy. |

## <a name="diagnosis"></a>Diagnostika

Na portálu Správa Azure řídicím panelu úlohy obsahuje informace o potřebujete-li vyhledat diagnostiky, tedy vstupy, uloží a/nebo operace přihlásit. Kliknete na odkaz přejdete správného umístění visiové diagnostiky.

  ![Sledování dotazu chyby](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Po kliknutí na zdroj vstupní a výstupní obsahuje podrobné diagnostické informace. To je aktualizované nejnovější informace o diagnostice je spuštěná úloha.

  ![Diagnostika dotazu](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
