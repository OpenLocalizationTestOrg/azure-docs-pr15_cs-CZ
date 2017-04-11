<properties
    pageTitle="Vytvoření vlastní řídicího panelu v protokolu analýzy | Microsoft Azure"
    description="Tento průvodce pomůže pochopit, jak řídicí panely analýzy protokolu můžete vizualizace všechny uložený protokol vyhledávání, poskytuje jediné lens zobrazíte prostředí."
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
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Vytvoření vlastní řídicího panelu v protokolu analýzy

Tato příručka vám pomůže pochopit, jak můžete řídicí panely protokolu analýzy vizualizace všechny uložený protokol vyhledávání, poskytuje jediné lens zobrazíte prostředí.

![Příklad řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Všechny vlastní řídicí panely, které vytvoříte v portálu OMS jsou k dispozici v mobilní aplikaci OMS. Získáte další informace o aplikace na následujících stránkách.

- [Mobilní aplikace OMS z Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Mobilní aplikace OMS z Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobilní řídicího panelu](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Jak vytvořit řídicím panelu?

Chcete-li začít, přejděte na stránku přehled OMS. Zobrazí se na dlaždici **Vlastní řídicí panel** na levé straně. Klikněte na a procházení hierarchie do řídicího panelu.

![Základní informace](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Přidání dlaždici

V řídicích panelech dlaždice technologii hledání uložený protokol. OMS obsahuje mnoho předem udělali uložený protokol vyhledávání, abyste mohli začít. Pomocí následujících kroků, které popisují, jak začít.

V zobrazení vlastní řídicí panel, jednoduše klikněte na **vlastní** a zadejte vlastní nastavení režimu.

![Obrazové](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Panely, které se otevře v pravé části stránky zobrazuje veškerý uložený protokol hledání pracovního prostoru. Vizualizace hledání uložený protokol jako dlaždici, najeďte myší uloženého hledání a potom klikněte na **plus** symbol.

![Přidání dlaždic 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Klikněte na symbol **plus** novou dlaždici nabídne v zobrazení Můj řídicího panelu.

![Přidání dlaždic 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Úpravy dlaždice

V zobrazení vlastní řídicí panel, jednoduše klikněte na **vlastní** a zadejte vlastní nastavení režimu. Klikněte na dlaždici, kterou chcete upravit. Pravém panelu změny chcete upravit a poskytuje výběr možností:

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Dlaždice vizualizace#
Existují tři typy vizualizací dlaždice můžete vybírat:

|Typ grafu|co dělá|
|---|---|
|![Pruhový graf](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Pokud zobrazí se časovou osu s výsledky hledání uložený protokol jako pruhového grafu nebo seznamu výsledků podle pole v závislosti na hledání protokolu sloučí výsledky podle pole nebo ne.
|![metriky](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Zobrazí váš výsledků vyhledávání výsledek celkové protokolu jako číslo na dlaždici. Metriky dlaždice umožňují nastavit prahovou hodnotu, která zvýrazní dlaždici při dosažení mezní hodnota.|
|![řádek](./media/log-analytics-dashboards/oms-dashboards-line.png)|Slouží k zobrazení časové osy výsledek výsledků vyhledávání vaše uložené protokolu s hodnotami jako spojnicový graf.|

### <a name="threshold"></a>Mezní hodnota
Vytvoření prahovou hodnotu na dlaždici pomocí metrických vizualizace. Vyberte vytvořit mezní hodnota na dlaždici. Vyberte, jestli ke zvýraznění dlaždici nad nebo pod vybraným mezní hodnotu a pak nastavte hodnotu mezní hodnota.

## <a name="organizing-the-dashboard"></a>Uspořádání řídicího panelu
Uspořádání řídicího panelu, přejděte na Zobrazit Moje řídicího panelu a potom na Upravit **vlastní** a zadejte režimu. Kliknutím a tažením dlaždice, kterou chcete přesunout a přesunout na jiné místo na místo, kam chcete dlaždici, aby byl.

![Uspořádání řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Odebrat dlaždici
Odebrat dlaždici, přejděte na Zobrazit Moje řídicího panelu a klikněte na Upravit **vlastní** a zadejte režimu. Vyberte dlaždici, kterou chcete odebrat a potom na pravém panelu vyberte **Odebrat dlaždice**.

![Odebrat dlaždici](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Další kroky

- Vytvořte [upozornění](log-analytics-alerts.md) v protokolu analýzy Generovat oznámení a nápravě problémů.
