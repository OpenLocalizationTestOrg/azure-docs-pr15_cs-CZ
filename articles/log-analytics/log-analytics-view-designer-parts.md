<properties
    pageTitle="Protokolování analýzy zobrazení návrhu | Microsoft Azure"
    description="Zobrazení návrhu v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz na stránku nastavení pro jednotlivé části vizualizace k dispozici pro použití v vlastní zobrazení."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Odkazy na vizualizaci část protokol analýzy zobrazení návrhu
Zobrazení návrháře v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz na stránku nastavení pro jednotlivé části vizualizace k dispozici pro použití v vlastní zobrazení.

Další články umožňující zobrazení návrhu jsou:

- [Zobrazení návrhu](log-analytics-view-designer.md) – základní informace o zobrazení návrhu a postupy pro vytváření a úpravy vlastní zobrazení.
- [Dlaždice odkaz](log-analytics-view-designer-tiles.md) – odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic. 

Následující tabulka popisuje různé typy k dispozici v Návrháři zobrazení vedle sebe.  Následující oddíly popisují každý typ dlaždice podrobných dat a jejich vlastnosti.

| Typ zobrazení | Popis |
|:--|:--|
| [Seznam dotazů](#list-of-queries-part) | Zobrazuje seznam protokolu vyhledávacích dotazů.  Uživatele můžete klepnout na obou dotazů zobrazíte jeho výsledky.  |
| [Číslo a seznam](#number-amp-list-part) | Záhlaví obsahuje jeden číslo znázorňující počet záznamů z protokolu vyhledávacího dotazu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času. |
| [Dvěma čísly a seznam](#two-numbers-amp-list-part) | Záhlaví se dvěma čísly zobrazuje počet obsahující záznamy ze samostatného protokolu vyhledávacích dotazů.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času. |
| [Prstencový & seznam](#donut-amp-list-part) | Záhlaví zobrazí jedno číslo shrnuté ze hodnotu sloupce v dotazu protokolu.  Prstencový graficky zobrazuje výsledky horní tři záznamy. |
| [Dva časové osy a seznam](#two-timelines-amp-list-part) | Záhlaví zobrazí výsledky dvou dotazů protokolu v čase jako sloupcový graf s popiskem zobrazení jedno číslo shrnuté ze hodnotu sloupce v dotazu protokolu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času. |   
| [Informace](#information-part) | Záhlaví zobrazí statický text a volitelné odkaz.  Seznam jedné nebo více položek s statický text a názvem. |
| [Spojnicový graf, obrázek a seznam](#line-chart-callout-amp-list-part) | Záhlaví zobrazí spojnicového grafu s více řad z dotazu protokolu přes čas a popisek s souhrnné hodnoty.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času. |
| [Spojnicový graf a seznam](#line-chart-amp-list-part) | Záhlaví zobrazí spojnicového grafu s více řad z dotazu protokolu určitou dobu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času. |
| [Zásobníku části řádek grafy](#stack-of-line-charts-part) | Zobrazí tři samostatné spojnicové grafy s víc řad z dotazu protokolu určitou dobu. |


## <a name="list-of-queries-part"></a>Seznam části dotazů

Zobrazuje seznam protokolu vyhledávacích dotazů.  Uživatele můžete klepnout na obou dotazů zobrazíte jeho výsledky.  Zobrazení bude obsahovat jediného dotazu ve výchozím nastavení a klikněte na **+ dotazu** přidáte dalších dotazů.

![Seznam zobrazit dotazy](media/log-analytics-view-designer/view-list-queries.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název | Text, který se zobrazí v horní části zobrazení. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Předem vybraných filtrů | Čárkou oddělený seznam vlastností, které lze zahrnout v podokně vlevo filtru, když uživatel vybere dotazu. |
| V režimu | Počáteční zobrazení zobrazí, pokud je vybraný dotaz.  Po otevření dotazu může uživatel vybrat všechna dostupná zobrazení. |
| **Dotazy** |
| Vyhledávání | Spustit dotaz. |
| Popisný název | Popisný název dotazu zobrazíte uživateli. |


## <a name="number--list-part"></a>Část seznamu & čísla

Záhlaví obsahuje jeden číslo znázorňující počet záznamů z protokolu vyhledávacího dotazu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času.


![Seznam zobrazit dotazy](media/log-analytics-view-designer/view-number-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části zobrazení. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví.
| Použití ikony | Vyberte ikonu zobrazení. |
| **Název** |
| Pokud chcete legendu | Text, který se zobrazí v horní části záhlaví. |
| Dotaz | Spustit záhlaví dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se vlastnosti prvních dvou prvních deset záznamů ve výsledcích.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Pruhy jsou vytvořeny automaticky na základě relativní hodnoty ve sloupci číselné.<br><br>Řazení záznamů v seznamu pomocí příkazu řazení v dotazu.  Uživatele můžete kliknout na Zobrazit všechny spusťte dotaz a vrátíte se všechny záznamy. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Jméno a hodnota oddělovače | Znak oddělovače, pokud chcete analyzovat vlastnost text do více hodnot.  Podrobnosti najdete v části [Obecné nastavení](#name-value-separator) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |


## <a name="two-numbers--list-part"></a>Dvěma čísly a části Seznam

Záhlaví se dvěma čísly zobrazuje počet obsahující záznamy ze samostatného protokolu vyhledávacích dotazů.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času.

![Dvěma čísly a zobrazení seznamu](media/log-analytics-view-designer/view-two-numbers-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části zobrazení. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví.
| Použití ikony | Vyberte ikonu zobrazení. |
| **Název** |
| Pokud chcete legendu | Text, který se zobrazí v horní části záhlaví. |
| Dotaz | Spustit záhlaví dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se vlastnosti prvních dvou prvních deset záznamů ve výsledcích.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Pruhy jsou vytvořeny automaticky na základě relativní hodnoty ve sloupci číselné.<br><br>Řazení záznamů v seznamu pomocí příkazu řazení v dotazu.  Uživatele můžete kliknout na Zobrazit všechny spusťte dotaz a vrátíte se všechny záznamy. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Operace | Operace provádět minigraf.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Jméno a hodnota oddělovače | Znak oddělovače, pokud chcete analyzovat vlastnost text do více hodnot.  Podrobnosti najdete v části [Obecné nastavení](#name-value-separator) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |

## <a name="donut--list-part"></a>Část prstencový & seznam

Záhlaví zobrazí jedno číslo shrnuté ze hodnotu sloupce v dotazu protokolu.  Prstencový graficky zobrazuje výsledky horní tři záznamy.

![Prstencový & seznam zobrazení](media/log-analytics-view-designer/view-donut-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví. |
| Použití ikony | Vyberte ikonu zobrazení. |
| **Záhlaví** |
| Název | Text, který se zobrazí v horní části záhlaví.
| Titulků | Text, který se zobrazí pod záhlaví v horní části záhlaví.
| **Prstencový** |
| Dotaz | Dotazu pro prstencový.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu. |
| **Prstencový** |  **> Centrum** |
| Text | Text, který se zobrazí pod hodnotu uvnitř prstencový. |
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota.<br><br>-Součet: Zadat hodnoty všechny záznamy.<br>– Procenta: Procento záznamy vrácené hodnoty v **výsledných hodnot použito v operaci Centrum** pro celkový počet záznamů v dotazu. |
| Výsledných hodnot použito v operaci center | Volitelně klikněte na znaménko plus a přidejte jednu nebo více hodnot.  Výsledky dotazu budou omezeny na záznamy s vlastnostmi zadanými.  Pokud jste přidali žádné hodnoty, jsou zahrnuty všechny záznamy v dotazu. |
| **Další možnosti** | **> Barvy** |
| Barva 1<br>Barva 2<br>Barva 3 | Vyberte požadovanou barvu pro každý z hodnoty zobrazené v prstencový. |
| **Další možnosti** | **> Mapování Upřesnit barev** |
| Hodnota pole | Zadejte název pole Zobrazit jako jinou barvu, pokud je součástí prstencový. |
| Barva | Vyberte požadovanou barvu pro pole jedinečné. |
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Operace | Operace provádět minigraf.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Jméno a hodnota oddělovače | Znak oddělovače, pokud chcete analyzovat vlastnost text do více hodnot.  Podrobnosti najdete v části [Obecné nastavení](#name-value-separator) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |

## <a name="two-timelines--list-part"></a>Dvě části časové osy a seznam

Záhlaví zobrazí výsledky dvou dotazů protokolu v čase jako sloupcový graf s popiskem zobrazení jedno číslo shrnuté ze hodnotu sloupce v dotazu protokolu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času.

![Zobrazení dvou časové osy & seznamu](media/log-analytics-view-designer/view-two-timelines-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví. |
| Použití ikony | Vyberte ikonu zobrazení. |
| **Nejdřív graf<br>druhém grafu** |
| Pokud chcete legendu | Text, který se zobrazí pod popisek první řady. |
| Barva | Barvu, kterou chcete použít pro sloupce v řadě. |
| Dotaz | Dotazu pro první řadu.  Počet záznamů přes každý časový interval zobrazí představované sloupcům grafu. |
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota pro popisek.<br><br>-Součet: Součet hodnoty ze všech záznamů.<br>-Průměrů: Hodnoty ze všech záznamů.<br>-Poslední: Vyhovovat z interval mezi posledními zahrnuté v grafu.<br>– První: Hodnoty z prvního interval zahrnutý v grafu.<br>-Počet: Počet všech záznamů vracená dotazem.|
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Operace | Operace provádět minigraf.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |

## <a name="information-part"></a>Část s informacemi

Záhlaví zobrazí statický text a volitelné odkaz.  Seznam jedné nebo více položek s statický text a názvem.

![Zobrazení informací](media/log-analytics-view-designer/view-information.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Barva | Barva pozadí záhlaví. |
| **Záhlaví** |
| Obrázek | Obrázek souboru chcete zobrazit v záhlaví. |
| Popisek | Text zobrazit v záhlaví. |
| **Záhlaví** | **> Odkaz** |
| Popisek | Text odkazu. |
| Adresa URL | Adresy URL odkazu pro. |
| **Informace o položek** |
| Název | Text k zobrazení nadpisu každé položky. |
| Obsah | Text k zobrazení jednotlivých položek. |


## <a name="line-chart-callout--list-part"></a>Spojnicový graf, popisek a části Seznam

Záhlaví zobrazí spojnicového grafu s více řad z dotazu protokolu přes čas a popisek s souhrnné hodnoty.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času.

![Spojnicový graf, popisku a zobrazení seznamu](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví. |
| Použití ikony | Vyberte ikonu zobrazení. |
| **Záhlaví** |
| Název | Text, který se zobrazí v horní části záhlaví. |
| Titulků | Text, který se zobrazí pod záhlaví v horní části záhlaví. |
| **Spojnicový graf** |
| Dotaz | Dotaz spustit spojnicového grafu.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky.  Pokud dotaz používá klíčového slova **interval** osy x grafu použije tento časový interval.  Pokud dotaz neobsahuje klíčového slova **interval** hodinových intervalů se používají pro osy x. |
| **Spojnicový graf** | **> Popisek** |
| Název popisku | Text k zobrazení nad hodnotu popisku. |
| Název řady | Hodnota vlastnosti pro řadu, kterou chcete použít pro hodnotu popisku.  Pokud žádná řada se používají všechny záznamy z dotazu. |
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota pro popisek.<br><br>-Průměrů: Hodnoty ze všech záznamů.<br>-Počet počet všech záznamů vracená dotazem.<br>-Poslední: Vyhovovat z interval mezi posledními zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu.<br>-Min: Minimální hodnota z intervalů zahrnuté v grafu.<br>-Součet: Součet hodnoty ze všech záznamů. |
| **Spojnicový graf** | **> Osy Y** |
| Použití logaritmická stupnice | Vyberte pro účely logaritmická stupnice osu y. |
| Jednotky | Určení jednotek pro hodnoty vracená dotazem.  Tyto informace slouží k zobrazení popisky v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální jednotka typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnoty v převeďte a číselné hodnoty se převedou z aktuální jednotka typ na převést na zadejte. |
| Vlastního štítku | Text zobrazený s osou Y vedle popisku pro typ jednotky.  Pokud není zadán žádný popisek, se zobrazí pouze typ jednotky. |
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Operace | Operace provádět minigraf.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Jméno a hodnota oddělovače | Znak oddělovače, pokud chcete analyzovat vlastnost text do více hodnot.  Podrobnosti najdete v části [Obecné nastavení](#name-value-separator) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |

## <a name="line-chart--list-part"></a>Spojnicový graf a seznam část

Záhlaví zobrazí spojnicového grafu s více řad z dotazu protokolu určitou dobu.  Seznam zobrazuje horních deseti výsledky dotazu s graf označující relativní hodnotu číselné sloupce nebo jeho změn v průběhu času.

![Spojnicový graf a seznam zobrazení](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví. |
| Použití ikony | Vyberte ikonu zobrazení. |
| **Záhlaví** |
| Název | Text, který se zobrazí v horní části záhlaví. |
| Titulků | Text, který se zobrazí pod záhlaví v horní části záhlaví. |
| **Spojnicový graf** |
| Dotaz | Dotaz spustit spojnicového grafu.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky.  Pokud dotaz používá klíčového slova **interval** osy x grafu použije tento časový interval.  Pokud dotaz neobsahuje klíčového slova **interval** hodinových intervalů se používají pro osy x. |
| **Spojnicový graf** | **> Osy Y** |
| Použití logaritmická stupnice | Vyberte pro účely logaritmická stupnice osu y. |
| Jednotky | Určení jednotek pro hodnoty vracená dotazem.  Tyto informace slouží k zobrazení popisky v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální jednotka typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnoty v převeďte a číselné hodnoty se převedou z aktuální jednotka typ na převést na zadejte. |
| Vlastního štítku | Text zobrazený s osou Y vedle popisku pro typ jednotky.  Pokud není zadán žádný popisek, se zobrazí pouze typ jednotky. |
| **Seznam** |
| Dotaz | Spustit seznamu dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| Skrytí grafu | Vyberte zakázat grafu napravo od číselné sloupce. |
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Barva | Barvy pruhů nebo minigrafů. |
| Operace | Operace provádět minigraf.  Podrobnosti najdete v části [Obecné nastavení](#sparklines) . |
| Jméno a hodnota oddělovače | Znak oddělovače, pokud chcete analyzovat vlastnost text do více hodnot.  Podrobnosti najdete v části [Obecné nastavení](#name-value-separator) . |
| Navigace dotazu | Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Podrobnosti najdete v části [Obecné nastavení](#navigation-query) . |
| **Seznam** | **> Názvy sloupců** |
| Jméno | Text, který se zobrazí v horní první sloupec v seznamu. |
| Hodnota | Text, který se zobrazí v horní druhého sloupce v seznamu. |
| **Seznam** | **> Mezní hodnoty** |
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka povolíte mezní hodnoty.  Podrobnosti najdete v části [Obecné nastavení](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Zásobníku části řádek grafy

Zobrazí tři samostatné spojnicové grafy s víc řad z dotazu protokolu určitou dobu.

![Zásobníku spojnicový graf s dílčími pruhy](media/log-analytics-view-designer/view-stack-line-charts.png)

| Nastavení | Popis |
|:--|:--|
| **Obecné** |
| Název skupiny | Text, který se zobrazí v horní části dlaždice. |
| Novou skupinu | Vyberte k vytvoření nové skupiny v listu počínaje v aktuálním zobrazení. |
| Ikona | Soubor obrázku se zobrazí vedle výsledek v záhlaví. |
| **Graf 1<br>graf 2<br>graf 3** | **> Záhlaví** |
| Název | Text, který se zobrazí v horní části grafu. |
| Titulků | Text, který se zobrazí pod záhlaví v horní části grafu. |
| **Graf 1<br>graf 2<br>graf 3** | **Spojnicový graf** |
| Dotaz | Dotaz spustit spojnicového grafu.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky.  Pokud dotaz používá klíčového slova **interval** osy x grafu použije tento časový interval.  Pokud dotaz neobsahuje klíčového slova **interval** hodinových intervalů se používají pro osy x. |
| **Graf** | **> Osy Y** |
| Použití logaritmická stupnice | Vyberte pro účely logaritmická stupnice osu y. |
| Jednotky | Určení jednotek pro hodnoty vracená dotazem.  Tyto informace slouží k zobrazení popisky v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální jednotka typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnoty v převeďte a číselné hodnoty se převedou z aktuální jednotka typ na převést na zadejte. |
| Vlastního štítku | Text zobrazený s osou Y vedle popisku pro typ jednotky.  Pokud není zadán žádný popisek, se zobrazí pouze typ jednotky. |

## <a name="common-settings"></a>Běžné nastavení
Následující oddíly popisují nastavení společná pro několik částí vizualizace.

### <a name="name-value-separator">Jméno a hodnota oddělovače</a>
Znak oddělovače, pokud chcete analyzovat vlastnost text z dotazu seznamu do více hodnot.  Pokud zadáte oddělovače, zadejte jména pro každé pole oddělená oddělovačem stejné do pole název.

Zvažte například vlastnost s názvem *umístění* začleněná hodnoty, třeba *41 Redmond budovy* a *Pardubicích Building12*.  Můžete zadat – pro název a hodnotu oddělovač a *Město budování* název.  Každá hodnota to by analyzovat na dvě vlastnosti s názvem pole *Město* a *budovy*. 

### <a name="navigation-query">Navigace dotazu</a>
Dotaz, spustit, pokud uživatel vybere položku v seznamu.  Umožňuje přidat syntaxe pro položku, kterou uživatel vybral *{vybrané položky}* .

Pokud dotaz sloupec s názvem *počítače* a dotaz navigace je například *{vybrané položky}*dotazu jako *počítač = "Počítač"* by spustit-li vybrán uživatele do počítače.  Pokud dotaz navigace *Typ = události {vybrané položky}* poté dotaz *Typ = počítač událostí = "Počítač"* chcete spustit.

### <a name="sparklines">Minigrafy</a>
Minigraf je malé spojnicový graf, který bude ilustrovat hodnotu položky seznamu v čase.  Vizualizace částí se seznamem můžete vybrat, zda mají být zobrazena vodorovné ose pruhem, který relativní hodnoty číselné sloupce nebo minigrafu označující jeho hodnota v čase.

Následující tabulka popisuje nastavení minigrafů.

| Nastavení | Popis |
|:--|:--|
| Povolení minigrafy | Zaškrtnutím tohoto políčka zobrazíte minigrafu místo vodorovný pruh. |
| Operace | Pokud jsou povolené minigrafy, je to operaci provádět v jednotlivých vlastností v seznamu k výpočtu hodnot pro minigraf.<br><br>-Posledního vzorku: Poslední hodnotu řady během určitého časového intervalu.<br>-Max: Maximální hodnotu řady během určitého časového intervalu.<br>-Min: Minimální hodnotu řady během určitého časového intervalu.<br>-Suma: Součet hodnot v řadách během určitého časového intervalu.<br>– Shrnutí: Používá stejný příkaz míra jako dotaz v záhlaví. |

### <a name="thresholds">Mezní hodnoty</a>
Mezní hodnoty umožňují zobrazit barevné ikony vedle jednotlivých položek v seznamu, která nabízí rychlý vizuální indikátor položek, které překročí určitou hodnotu nebo spadají do určitého rozsahu.  Můžete třeba zobrazit zelenou ikonu položek s přijatelná hodnota, žlutá, pokud je argument hodnota v oblasti, které označuje upozornění a červené pokud překročí chybovou hodnotu.

Pokud povolíte prahových hodnot pro část, je nutné zadat jeden nebo více mezní hodnoty.  Pokud je argument hodnota položky větší než mezní hodnota a nižší než další mezní hodnota, daná barva použije.  Pokud položka je větší než potom nejvyšší mezní hodnota, je nastavení této barvy.   

Každá mezní obsahuje jeden mezní hodnota je **výchozí**.  Toto je sada překročení žádné hodnoty barev.  Můžete přidat nebo odebrat mezní hodnoty kliknutím **+** nebo její tlačítko **x** .

Následující tabulka popisuje nastavení Jakmile.

| Nastavení | Popis |
|:--|:--|
| Povolení mezní hodnoty | Zaškrtnutím tohoto políčka zobrazíte barevnou ikonu vlevo od každou hodnotu označující jeho stav relativní zadaných prahových hodnot. |
| Jméno | Název pro identifikaci mezní hodnota. |
| Mezní hodnota | Prahovou hodnotu.  Barvy nejvyšší mezní hodnota vyšší než hodnotou na položku nastavenou barvu stavu pro každou položku seznamu.  Existuje jednu výchozí prahovou hodnotu, která je barva, pokud žádná mezní hodnoty. |
| Barva | Barva pro mezní hodnota. |


## <a name="next-steps"></a>Další kroky

- Informace o [protokolu hledání](log-analytics-log-searches.md) na podporu dotazy v částech vizualizace.