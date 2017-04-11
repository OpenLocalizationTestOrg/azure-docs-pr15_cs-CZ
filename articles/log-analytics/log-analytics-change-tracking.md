<properties
    pageTitle="Sledování řešení v protokolu analýzy změn | Microsoft Azure"
    description="Můžete sledování změn konfigurace řešení v protokolu analýzy umožňují snadno identifikovat software a služby změny, které nastanou v prostředí – identifikační tyto změny konfigurace vám můžou pomoct zdůraznit provozní problémy."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Změna řešení sledování v protokolu analýzy


Můžete sledování změn konfigurace řešení v protokolu analýzy umožňují snadno identifikovat software a služby změny a Linux démon probíhajících v prostředí – identifikační tyto změny konfigurace vám můžou pomoct zdůraznit provozní problémy.

Nainstalovat řešení aktualizovat typ je agent, kterého jste nainstalovali. Změn nainstalovaná software a služby na serverech sledované se budou číst a odeslaný data služby protokolu analýzy v cloudu pro zpracování. Použití logických operátorů je použité pro přijaté data a cloudovou službu záznamů data. Při nalezení změny se servery se změnami opravách na řídicím panelu sledování změn Pomocí informací na řídicím panelu sledování změn můžete snadno uvidíte změny provedené v infrastruktury serverů.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Operations Manager je potřebný pro sledování změn řešení.
- Windows nebo Operations Manager agent musí mít na každém počítači, ve které chcete sledovat změny.
- Přidání sledování změn řešení OMS pracovního prostoru pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.


## <a name="change-tracking-data-collection-details"></a>Změnit podrobnosti kolekce dat sledování

Sledování změn shromažďuje software zásob a metadata služby Windows používání agentů, které jste zapnuli automatický přístup.

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje pro sledování změn.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ano](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ano](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ne](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Ne](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Ano](./media/log-analytics-change-tracking/oms-bullet-green.png)| každou hodinu|

## <a name="use-change-tracking"></a>Pomocí sledování změn

Po instalaci řešení uvidí pomocí dlaždici **Sledování změn** na stránce **Přehled** v OMS souhrnné informace o změnách sledované serverů.

![Obrázek rohu dlaždice sledování změn](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Můžete zobrazit změny infrastruktury a potom přejít k podrobnostem podrobnosti o těchto kategorií:

- Změny podle typu konfigurace pro software a služby systému Windows
- Software se změní na aplikace a aktualizace pro jednotlivé servery
- Celkový počet změny software pro jednotlivé aplikace
- Změny pro jednotlivé servery služby systému Windows

![Obrázek s řídicím panelem sledování změn](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Obrázek s řídicím panelem sledování změn](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Chcete-li zobrazit změny u všech změnit typ

1. Na stránce **Přehled** klikněte na dlaždici **Sledování změn** .
2. Na řídicím panelu **Změnit sledování** zkontrolujte souhrnné informace v jednom z změnit typ listy a potom klikněte na jednu zobrazíte podrobné informace o ho na stránce **log vyhledávání** .
3. Na jakékoli stránce protokolu hledání se výsledky zobrazují čas, podrobné výsledky a historie hledání protokolu. Můžete taky filtrovat podle fasetami k upřesnění výsledků.

## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data sledování změn.
