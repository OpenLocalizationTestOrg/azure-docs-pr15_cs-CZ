<properties
    pageTitle="Analýza využití dat v protokolu analýzy | Microsoft Azure"
    description="Použití stránky můžete v protokolu analýzy zobrazíte množství zpracovávaných dat je odesílaného ke službě OMS."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analýza využití dat v protokolu analýzy

Technologie pro analýzu protokolu v sadě operace Management (OMS) shromažďuje údaje a pravidelně odešle ke službě OMS.  Na stránce **použití** zobrazíte množství zpracovávaných dat je odesílaného ke službě OMS. Na stránce **použití** taky zobrazuje množství zpracovávaných dat se právě odesílá denně řešení a jak často serverech posílají data.

>[AZURE.NOTE] Pokud máte bezplatného účtu vytvořené pomocí [OMS webu](http://www.microsoft.com/oms)jste omezené odesílání 500 MB dat ve službě OMS denně. Pokud dostanete denní limit, analýza dat zastavte a obnovit na začátku následujícího dne. Taky musíte odeslání všechna data, která nebyla zdroji přijaté nebo zpracovány službou OMS.

Používání můžete zobrazit pomocí dlaždici **použití** na řídicí panel **Přehled** v OMS.

![použití dlaždic](./media/log-analytics-usage/usage-tile.png)

Pokud překročíte limit pro vaši denní využití nebo pokud jste v blízkosti limit pro vaši, můžete volitelně odebrat řešení snížit množství dat, které odesíláte ke službě OMS. Další informace o odebrání řešení najdete v článku [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md).

![Používání řídicích panelů](./media/log-analytics-usage/usage-dashboard.png)

Na stránce **použití** se zobrazí následující informace:

- Průměrná použití denně
- Použití dat pro každou řešení během posledních 30 dní
- Kolik datové servery v prostředí odesíláte ke službě OMS během posledních 30 dní
- Váš datový tarif ceny osy a odhad nákladů
- Informace o vaší smlouva o úrovni služeb (SLA), včetně, jak dlouho trvá OMS zpracuje dat

## <a name="to-work-with-usage-data"></a>Práce s použití zásad správy informací

1. Na stránce **Přehled** klikněte na dlaždici **použití** .
2. Na stránce **použití** zobrazení použití kategorií, které ukazují oblastí, že jste nezáleží.
3. Pokud máte řešení, které využívá příliš mnoho denní kvóty nahrát, zvažte odebrání řešení.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Chcete-li zobrazit náklady a další informace
1. Na stránce **Přehled** klikněte na dlaždici **použití** .
2. Na stránce **použití** v části **použití**kliknutím na dvojitou šipku (**>**) vedle **Odhad nákladů**.
3. V rozšířené **datový tarif** podrobnosti, zobrazí se vaše odhadovaná měsíční nákladů.  
    ![Datový tarif](./media/log-analytics-usage/usage-data-plan.png)
4. Pokud chcete zobrazit vašich fakturačních údajů, klikněte na **zobrazení faktury** k zobrazení informací o vašem předplatném.
    - Na stránce předplatná klikněte na předplatné zobrazíte podrobnosti a řádkové položky seznamu použití.  
        ![předplatné](./media/log-analytics-usage/usage-sub01.png)
    - Na souhrnné stránce pro vaše předplatné můžete provést řadu úkolů pro správu a zobrazit další podrobnosti předplatného.  
        ![Podrobnosti předplatného](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Chcete-li zobrazit listy data pro svůj SLA
1. Na stránce **Přehled** klikněte na dlaždici **použití** .
2. V části **Smlouvách úroveň**klikněte na **Stáhnout SLA podrobnosti**.
3. Stažení souboru aplikace Excel XLSX ke kontrole.  
    ![Podrobnosti o SLA](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Další kroky

- Viz [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
