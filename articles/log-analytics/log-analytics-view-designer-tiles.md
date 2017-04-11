<properties
    pageTitle="Protokolování analýzy zobrazení návrháře dlaždice odkaz | Microsoft Azure"
    description="Zobrazení návrhu v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Odkaz na zobrazení návrháře dlaždice protokolu analýzy
Zobrazení návrháře v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic.

Další články umožňující zobrazení návrhu jsou:

- [Zobrazení návrhu](log-analytics-view-designer.md) – základní informace o zobrazení návrhu a postupy pro vytváření a úpravy vlastní zobrazení.
- [Odkaz část vizualizace](log-analytics-view-designer-parts.md) – odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic. 


Následující tabulka uvádí různé typy k dispozici v Návrháři zobrazení vedle sebe.  Následující oddíly popisují každý typ dlaždice podrobných dat a jejich vlastnosti.

| Dlaždice | Popis |
|:--|:--|
| [Číslo](#number-tile) | Jedno číslo zobrazuje počet záznamů z dotazu. |
| [Dvě čísla.](#two-numbers-tile) | Dva jednoduché čísla zobrazující počty obsahující záznamy ze dvou různých dotazů. |
| [Prstencový](#donut-tile) | Prstencový graf na základě dotazu s souhrnné hodnoty v centru. |
| [Spojnicový graf a popisek](#line-chart-amp-callout-tile) | Spojnicový graf na základě dotazu a popisek s souhrnné hodnoty. |
| [Spojnicový graf](#line-chart-tile) | Spojnicový graf na základě dotazu. |
| [Dva časové osy](#two-timelines-tile) | Sloupcový graf s dvěma řadu, kterou každý na základě samostatné dotazu. |



## <a name="number-tile"></a>Číslo dlaždice

Dlaždice **číslo** zobrazí jedno číslo zobrazuje počet záznamů protokolu dotazu a popisku.

![Číslo dlaždice](media/log-analytics-view-designer/tile-number.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| **Dlaždice** |
| Pokud chcete legendu | Text, který se zobrazí pod hodnotu. |
| Dotaz | Spustit dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| **Rozšířené možnosti** |  **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |

## <a name="two-numbers-tile"></a>Dlaždice dvě čísla.

**Dva číslo** dlaždici se zobrazují dvě čísla zobrazuje počet obsahující záznamy ze dvou dotazů jiný protokol a popisku pro jednotlivá pole.

![Dlaždice dvě čísla.](media/log-analytics-view-designer/tile-two-numbers.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| **První dlaždice** |
| Pokud chcete legendu | Text, který se zobrazí pod hodnotu. |
| Dotaz | Spustit dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| **Druhá dlaždice** |
| Pokud chcete legendu | Text, který se zobrazí pod hodnotu. |
| Dotaz | Spustit dotaz.  Zobrazí se počet záznamů vracená dotazem. |
| **Rozšířené možnosti** | **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |

## <a name="donut-tile"></a>Dlaždice prstencový

Dlaždice **prstencový** zobrazí jedno číslo shrnuté ze hodnotu sloupce v dotazu protokolu.  Prstencový graficky zobrazuje výsledky horní tři záznamy.

![Dlaždice prstencový](media/log-analytics-view-designer/tile-donut.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| **Prstencový** |
| Dotaz | Dotazu pro prstencový.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky. |
| **Prstencový** | **> Centrum** |
| Text | Text, který se zobrazí pod hodnotu uvnitř prstencový. |
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota.<br><br>-Součet: Zadat hodnoty všechny záznamy s hodnotou vlastnosti.<br>– Procenta: Procento sečtené hodnoty ze záznamů s hodnotou vlastnosti ve srovnání s sečtené hodnoty všech záznamů. |
| Výsledných hodnot použito v operaci center | Volitelně klikněte na znaménko plus a přidejte jednu nebo více hodnot.  Výsledky dotazu budou omezeny na záznamy s vlastnostmi zadanými.  Pokud jste přidali žádné hodnoty, než všechny záznamy nacházejí v dotazu. |
| **Prstencový** | **> Další možnosti** |
| Barvy | Barvu, kterou chcete zobrazit u každé tři horní vlastnosti.  Pokud chcete určit alternativní barvy určitých nemovitostí s hodnotou, použijte rozšířené možnosti mapování barvu. |
| Mapování rozšířené barev | Zobrazí barvu pro konkrétní nemovitostí s hodnotou.  Pokud zadanou hodnotu v horním tři, se zobrazí alternativní barva místo standardní barvy.  Pokud je vlastnost není v horním tři barvy nezobrazuje. |
| **Rozšířené možnosti** | **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |

## <a name="line-chart-tile"></a>Dlaždice spojnicový graf

**Spojnicový graf** dlaždici se zobrazují spojnicového grafu s více řad z dotazu protokolu určitou dobu.  

![Spojnicový graf a popisek vedle sebe](media/log-analytics-view-designer/tile-line-chart.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| **Spojnicový graf** |  
| Dotaz | Dotaz spustit spojnicového grafu.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky.  Pokud dotaz používá klíčového slova **interval** osy x grafu použije tento časový interval.  Pokud dotaz neobsahuje klíčového slova **interval** hodinových intervalů se používají pro osy x. |
| **Spojnicový graf** | **> Osy Y** |
| Použití logaritmická stupnice | Vyberte pro účely logaritmická stupnice osu y. |
| Jednotky | Určení jednotek pro hodnoty vracená dotazem.  Tyto informace slouží k zobrazení popisky v grafu označující typy hodnot a volitelně pro převod hodnoty.  **Jednotka typ** Určuje kategorii jednotky a definuje **Aktuální jednotka typ** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** potom číselné hodnoty se převedou z **Aktuální jednotka** typ na **převést na** typ. |
| Vlastního štítku | Text zobrazený s osou Y vedle popisku pro typ jednotky.  Pokud není zadán žádný popisek, se zobrazí pouze typ jednotky. |
| **Rozšířené možnosti** | **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |


## <a name="line-chart--callout-tile"></a>Spojnicový graf a popisek dlaždice

**Spojnicový graf a popisek** dlaždici se zobrazují spojnicového grafu s více řad z dotazu protokolu přes čas a popisek s souhrnné hodnoty.  

![Spojnicový graf a popisek vedle sebe](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| **Spojnicový graf** |  
| Dotaz | Dotaz spustit spojnicového grafu.  Na první vlastnost by měl být textové hodnoty a vlastnost druhý číselnou hodnotu.  Toto je obvykle dotaz, který používá klíčové slovo **Míra** summarize výsledky.  Pokud dotaz používá klíčového slova **interval** osy x grafu použije tento časový interval.  Pokud dotaz neobsahuje klíčového slova **interval** hodinových intervalů se používají pro osy x. |
| **Spojnicový graf** | **> Popisek** |
| Popisek | Nadpis zobrazení nad hodnotu popisku. |
| Název řady | Hodnota vlastnosti pro řadu, kterou chcete použít pro hodnotu popisku.  Pokud žádná řada se používají všechny záznamy z dotazu. |
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota pro popisek.<br>-Průměrů: Hodnoty ze všech záznamů.<br><br>-Počet: Počet všech záznamů vracená dotazem.<br>-Poslední: Vyhovovat z interval mezi posledními zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu.<br>-Min: Minimální hodnota z intervalů zahrnuté v grafu.<br>-Součet: Součet hodnoty ze všech záznamů. |
| **Spojnicový graf** | **> Osy Y** |
| Použití logaritmická stupnice | Vyberte pro účely logaritmická stupnice osu y. |
| Jednotky | Určení jednotek pro hodnoty vracená dotazem.  Tyto informace slouží k zobrazení popisky v grafu označující typy hodnot a volitelně pro převod hodnoty.  **Jednotka typ** Určuje kategorii jednotky a definuje **Aktuální jednotka typ** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** potom číselné hodnoty se převedou z **Aktuální jednotka** typ na **převést na** typ. |
| Vlastního štítku | Text zobrazený s osou Y vedle popisku pro typ jednotky.  Pokud není zadán žádný popisek, se zobrazí pouze typ jednotky. |
| **Rozšířené možnosti** | **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |

## <a name="two-timelines-tile"></a>Dlaždice dvou časové osy

Dlaždici **dvou časové osy s obrázky** zobrazuje výsledky dvou dotazů protokolu v čase jako sloupcový graf.  Zobrazí se popisek pro každé řadě.  

![Dlaždice dvou časové osy](media/log-analytics-view-designer/tile-two-timelines.png)

| Nastavení | Popis |
|:--|:--|
| Jméno        | Text, který se zobrazí v horní části dlaždice. |
| Popis | Text, který se zobrazí pod název dlaždice.    |
| První graf   
| Pokud chcete legendu | Text, který se zobrazí pod popisek první řady.
| Barva | Barvu, kterou chcete použít pro sloupce v první řadě.
| Diagram dotazu | Dotazu pro první řadu.  Počet záznamů přes každý časový interval zobrazí představované sloupcům grafu.
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota pro popisek.<br><br>-Průměrů: Hodnoty ze všech záznamů.<br>-Počet: Počet všech záznamů vracená dotazem.<br>-Poslední: Vyhovovat z interval mezi posledními zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu.
| **Druhým grafem** |
| Pokud chcete legendu | Text, který se zobrazí pod popisek druhé řady.
| Barva | Barvu, kterou chcete použít pro sloupce ve druhé řady.
| Diagram dotazu | Spustit druhé řady dotaz.  Počet záznamů přes každý časový interval zobrazí představované sloupcům grafu.
| Operace | Operace provádět vlastnost hodnotu shrnutí je jedna hodnota pro popisek.<br><br>-Průměrů: Hodnoty ze všech záznamů.<br>-Počet: Počet všech záznamů vracená dotazem.<br>-Poslední: Vyhovovat z interval mezi posledními zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu. |
| **Rozšířené možnosti** | **> Ověření toku dat** |
| Povoleno | Vyberte, pokud ověření toku dat používat pro dlaždici.  To poskytuje alternativní zprávu, jestliže data nejsou k dispozici pro tuto dlaždici.  Tím se obvykle používá k poskytování zprávy dočasné období, kdy se instaluje zobrazení a data je k dispozici. |
| Dotaz | Dotaz při spuštění zaškrtněte, pokud je k dispozici pro zobrazení dat.  Pokud dotaz vrací žádné výsledky, se zobrazí zpráva místo hodnotu z hlavního dotazu. |
| Zpráva | Zpráva zobrazí, pokud ověření toku dat vracel žádná data.  Pokud zadaná žádná zpráva se zobrazí *Při provádění hodnocení* . |
| **Časový Interval** |
| Doba trvání | Doba trvání od aktuálního data pro účely časový interval dotazu.  Například pokud není zadán **7 dnů** , potom dotaz se omezí na záznamy vytvořené z 7 dny podle aktuálního data. |
| Posun konec dat | Volitelné posun od aktuálního data, která chcete použít pro časový interval hlavním dotazu.  Například pokud **-1 den** se používá pro **Koncové datum posun** a **7 dnů** použité dobu **trvání**, potom dotaz se omezí na záznamy vytvořené z 8 dny k včera. |

## <a name="next-steps"></a>Další kroky

- Informace o [protokolu hledání](log-analytics-log-searches.md) na podporu dotazy ve dlaždice.
- Přidání [Částí vizualizace](log-analytics-view-designer-parts.md) vlastní zobrazení.