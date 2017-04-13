<properties
    pageTitle="Nastavit upozornění u dotazů v toku analýzy | Microsoft Azure"
    description="Principy analýzy toku výstrahy."
    keywords="Nastavení upozornění"
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Nastavit upozornění u úlohy analýzy toku Azure

## <a name="introduction-monitor-page"></a>Úvod: Stránka sledování

Můžete nastavit upozornění pro výstrahu metriky dosažení zadané podmínky.

Například "při události výstup pro poslední 15 minut < 100, odeslání e-mailového oznámení id e-mailu: xyz@company.com”.

Pravidla se dá nastavit tak metrice prostřednictvím portálu nebo může být nakonfigurováno [programově](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) nad daty protokoly operací.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Nastavení upozornění prostřednictvím portálu klasické Azure

Nastavit upozornění na portálu Azure klasické dvěma způsoby:  

1.  Karta **Sledování** práce toku analýzy  
2.  Operace přihlášení správu přístupových  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Nastavit upozornění na kartě Monitor úlohy na portálu

1.  Výběr metriky na kartě monitor a klikněte na tlačítko **Přidat pravidlo** v dolní části řídicího panelu a nastavení pravidel.  

    ![Řídicí panel](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Definovat název a popis upozornění  

    ![Vytvoření pravidla](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Zadejte prahové hodnoty, okno Výstraha hodnocení a akcí pro upozornění  

    ![Definování podmínek](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Nastavení upozornění prostřednictvím protokoly operace

1.  Přejděte na kartu **výstrahy** v správu přístupových [Klasické portál Azure](https://manage.windowsazure.com).  
2.  Klikněte na **Přidat pravidlo**  

    ![Kritéria](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Definujte název a popis upozornění. Jako typ služby a název úlohy jako název služby vyberte toku analýzy.  

    ![Definování upozornění](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Nastavení upozornění v portálu Azure ##

Na portálu Azure vyhledejte toku analýzy úkoly, které vás zajímají upozorňování na a klikněte v části **Sledování** .  V zásuvné **míru** , která se otevře klikněte na příkaz **Přidat upozornění** .

  ![Nastavení Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Název pravidla výstrahy a zvolte popis, který se zobrazí v e-mailových oznámení.

Když vyberete metriky budete vyberte podmínku a mezní hodnota metriky.

  ![Azure portálu vyberte míru](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Podrobnější informace o konfiguraci upozornění na portálu Azure najdete v článku [přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
