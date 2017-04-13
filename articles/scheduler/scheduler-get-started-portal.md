<properties
 pageTitle="Začínáme s Azure Plánovač Azure portálu | Microsoft Azure"
 description="Začínáme s Azure Plánovač Azure portálu"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Začínáme s Azure Plánovač Azure portálu

Není těžké si vytvořit naplánovaných úloh v Azure plánovači. V tomto kurzu se dozvíte, jak vytvořit projekt. Také se dozvíte na Plánovač monitorování a správu možnosti.

## <a name="create-a-job"></a>Vytvoření projektu

1.  Přihlaste se k [portálu Azure](https://portal.azure.com/).  

2.  Klikněte na **+ Nový** > do vyhledávacího pole zadejte _Plánovač_ > Vybrat **Plánovač** ve výsledcích > klikněte na **vytvořit**.

     ![][marketplace-create]

3.  Vytvoření projektu, jednoduše narazí http://www.microsoft.com/ s požadavek GET. Na obrazovce **Plánovač úloh** zadejte následující informace:

    1.  **Název:**`getmicrosoft`  

    2.  **Předplatné:** Předplatné Azure   

    3.  **Úlohy kolekce:** Vyberte existující kolekce projektu, nebo klikněte na **Vytvořit nový** > zadejte název.

4.  Pak ve skupinovém rámečku **Nastavení akce**definujte následující hodnoty:

    1.  **Typ akce:**` HTTP`  

    2.  **Metoda:**`GET`  

    3.  **Adresy URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Nakonec Pojďme definovat plán. Projektu může být rozumí jednorázovou úlohu, ale chci vybrat plánu opakování:

    1. **Opakování**:`Recurring`

    2. **Spuštění**: dnešní datum

    3. **Opakování každé**:`12 Hours`

    4. **Konec**: je dva dny od dnešního data  

      ![][recurrence-schedule]

6.  Klikněte na tlačítko **vytvořit**

## <a name="manage-and-monitor-jobs"></a>Správa a sledování projektů

Po vytvoření úlohy se v hlavním Azure řídicího panelu. Klikněte na úkoly a otevře se nové okno s následujících kartách:

1.  Vlastnosti  

2.  Nastavení akce  

3.  Plán  

4.  Historie

5.  Uživatelé

    ![][job-overview]

### <a name="properties"></a>Vlastnosti

Tyto vlastnosti jen pro čtení popisují Správa metadat pro Plánovač úloh.

   ![][job-properties]


### <a name="action-settings"></a>Nastavení akce

Po kliknutí na projektu v okně **úlohy** umožňuje konfigurovat tuto úlohu. Toto oprávnění umožňuje Upřesnit nastavení, pokud je v nenastavili Průvodce vytvořením snadné.

Pro všechny typy akcí můžete změnit zásad opakování a akce chyby.

Pro typy akce úlohy HTTP a HTTPS může změnit způsob do libovolné povolené akce HTTP. Můžete taky přidat, odstranit nebo změnit záhlaví a informace o základní ověřování.

Pro typy akcí fronty úložiště můžete změnit účet úložiště, název fronty, token přidružení zabezpečení a textu.

Pro typy akcí bus služby můžete změnit obor názvů, cestu k tématu/frontě, nastavení ověřování, typ přenosu, vlastnosti zpráv a textu zprávy.

   ![][job-action-settings]

### <a name="schedule"></a>Plán

Toto oprávnění umožňuje překonfigurovat plánu, pokud chcete změnit plán vytvořený v Průvodce vytvořením snadné.

Toto je možnost vytvářet [složité plány a Upřesnit opakování v práci](scheduler-advanced-complexity.md)

Můžete změnit datum zahájení a čas, plán opakování a konce datum a čas (Pokud úkoly je zápisy z pravidelných.)

   ![][job-schedule]


### <a name="history"></a>Historie

Kartu **Historie** zobrazí vybrané metriky pro každé spuštění úloh systému pro vybrané úkoly. Tyto metriky zadat v reálném čase hodnoty týkající se stavu vaše plánovači:

1.  Stav  

2.  Podrobnosti  

3.  Novými pokusy o aktualizaci

4.  Výskyt: 1st, 2nd, 3, atd.

5.  Počáteční čas spuštění  

6.  Koncový čas spuštění

   ![][job-history]

Můžete kliknout na spustit a zobrazit její **Podrobnosti historie**, včetně celého odpověď pro každé spuštění. Toto dialogové okno taky vám umožní zkopírovat odpověď do schránky.

   ![][job-history-details]

### <a name="users"></a>Uživatelé

Azure na základě rolí přístup ovládacího prvku (RBAC) umožňuje správu jemně odstupňovaná přístupu pro Plánovač Azure. Další informace o používání karty uživatelů najdete v nápovědě k [Řízení přístupu Azure Role-Based](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Viz taky

 [Co je Plánovač?](scheduler-intro.md)

 [Plánovač koncepty, terminologie a entity hierarchie](scheduler-concepts-terms.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Jak vytvářet složité plány a Upřesnit opakování s Azure Plánovač](scheduler-advanced-complexity.md)

 [Plánovač rozhraní REST API odkaz](https://msdn.microsoft.com/library/mt629143)

 [Odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Plánovač vysoké dostupnosti a spolehlivosti](scheduler-high-availability-reliability.md)

 [Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Plánovač ověřování odchozích připojení](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
