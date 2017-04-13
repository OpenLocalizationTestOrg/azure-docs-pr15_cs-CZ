<properties 
    pageTitle="Ladění myší a služby protokoly do analýzy toku | Microsoft Azure" 
    description="Postupy použití analýzy toku operace protokoly" 
    keywords="protokoly služby"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Ladění toku analýzy úlohy pomocí služby a operace protokoly

Všechny služby Azure zadejte zprávy provozní protokolování pro uživatele k zaznamenání podrobností související s operacemi správy. V Azure toku analýzy tyto informace se dá použít pro účely například zobrazení stavu úlohy, průběhu projektu ladění a zpráv o chybách ke sledování průběhu projektu v průběhu času od zahájení do zpracování výstupu.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Najít operace protokoly do portálu pro správu Azure

Protokoly operací přístupné dvěma způsoby:  

- Řídicí panel úlohy toku analýzy  
- Správa služby Azure klasické portálu  

## <a name="dashboard-of-the-stream-analytics-job"></a>Řídicí panel úlohy toku analýzy

Odkaz na odpovídající protokoly toku analýzy úlohy se zobrazí na karta řídicí panel v projektu. Když kliknete na odkaz, nastaví filtry tak, že uvedený nejnovější protokoly pro konkrétní úlohu.

  ![Vyberte Správa přístupových protokoly](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Správa přístupových

Přejděte ručně operace protokoly pro toku technologie pro analýzu a další služby Azure klasické portálu:

1.  Klikněte na **Správa služby** [Azure klasické portálu](https://manage.windowsazure.com).
2.  Vyberte **Toku Analytics** pro **Typ** a název projektu pro **Název služby**.  

  ![Vyberte toku analýzy](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Vyhledání protokolů auditování na portálu Azure ##

Najít provozní protokoly toku analýzy práce na portálu Azure, klikněte na **Procházet** a vyberte **protokoly auditování**.

  ![Azure portál vyberte analýzy toku](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Otevře se zásuvné zobrazující události z za posledních 7 dnů pro všechny zdroje předplatné.  Můžete filtrovat, zobrazí se události zadat typu nebo časové rozmezí: Kliknutím na příkaz **Filtr** .

  ![Azure portál vyberte analýzy toku](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Zobrazení podrobností protokolu

Můžete filtrovat podle časového rozsahu a stav na Zobrazit protokoly pro danou úlohu.

Na portálu Správa Azure klikněte na tlačítko **podrobností** v dolní části okna zobrazíte další informace o událost nastavit jako vybrané. 

  ![Zaškrtněte políčko Podrobnosti](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Na portálu Azure klikněte na položku protokolu zobrazíte podrobné informace o událostech uvnitř.

  ![Azure portál vyberte podrobnosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Odtud můžete otevřít zásuvné **podrobností** kliknutím na událost.

  ![Azure portál vyberte podrobnosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Ladění nezdařeném uložení projektu

Na portálu Správa Azure klikněte na ikonu hledání zadejte "se nezdařila". To vám výsledek všechny protokoly s k chybám. 

  ![Ladění nezdařeném uložení projektu](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Na portálu Azure můžete filtrovat podle úrovně zprávy zobrazíte **důležité** události.

  ![Azure portálu ladění](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Vyberte kterýkoliv z chyby a klikněte na **Podrobnosti o** Další informace o chybě.  Některé chybové zprávy taky obsahují informace o tom, jak zmírnění problém. 

  ![Podrobnosti operace](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

V případě, když potřebujete kontaktovat [podporu](https://azure.microsoft.com/support/options/) nebo zadejte informace do týmu prostřednictvím na [fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), dejte pozor podrobnosti operace konkrétně **ID korelace**. 

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
