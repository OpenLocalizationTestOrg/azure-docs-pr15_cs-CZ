<properties
   pageTitle="Jak opatřit zdroje dat | Microsoft Azure"
   description="Článek s postupy zvýraznění jak sledovat data prostředky v katalogu dat Azure, včetně popisných názvů, značky, popis a odborníky."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Jak opatřit zdroje dat

## <a name="introduction"></a>Úvod
**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. Jinými slovy katalog dat je celé o tom pomáhá lidé zjišťovat, pochopit a získání další hodnoty z existujících dat pomocí zdrojů dat a nápovědu organizace. Když zdroj dat máte zaregistrované v rámci katalogu dat, jeho metadata zkopírovali a indexované službou, ale textu není ukončit tam. Katalog dat umožňuje uživatelům zadání vlastní popisný metadat – například popisy a značky – doplnit metadata ze zdroje dat a aby zdroje dat srozumitelnější další lidi.

## <a name="annotation-and-crowdsourcing"></a>Pořizování poznámek a crowdsourcing
Všichni účastníci nemají k vyjádření. A to je dobré.
Katalog dat rozpozná, že jednotlivým uživatelům jiné perspektivy ve enterprise zdroje dat a aby každý z těchto perspektiv může být užitečné. Zvažte následující situaci:

* Správce systému zná smlouva o úrovni služeb pro servery nebo služby, které hostují zdroje dat.
* Správce databáze zná záložní plán pro každou databázi a povolené zpracování ETL windows.
* Vlastníka systému zná proces pro vyžádání přístupu ke zdroji dat.
* Správy hlavních dat ví, jak mapování majetek i atributy ve zdroji dat do datového modelu organizace.
* Analytik zná použití data v rámci obchodních procesů, které má podporuje.

Každý z těchto perspektiv je užitečná a katalog dat používá crowdsourcing přístup k metadata, která umožňuje každou z nich zachyceny a slouží k měli ucelený přehled zdrojů dat registrovaných. Na portálu katalogu dat, každý uživatel přidejte a upravte své poznámky a při tom mít možnost zobrazit poznámky poskytovanou ostatním uživatelům.

## <a name="different-types-of-annotations"></a>Různé druhy poznámky
Katalog dat podporuje následující typy poznámek:

| Pořizování poznámek     | Poznámky                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Popisný název  | Popisných názvů lze zadat na úrovni dat materiálů, aby data prostředky srozumitelnější. Popisných názvů jsou nejužitečnější po názvu podkladového objekt nejasná, zkrácený nebo jinak není smysluplné uživatelům.                                                                                                                            |
| Popis    | Popisy lze zadat data materiálů a atribut / sloupec úrovně. Popisy jsou volné krátký text poznámky, které popisují uživatele Perspektiva na materiálů dat nebo jeho použití.                                                                                                                                                              |
| Značky (značky uživatele)          | Značky lze zadat data materiálů a atribut / sloupec úrovně. Značky uživatele jsou definované uživatelem popisky, které mohou sloužit k zařadit do kategorií prostředky data nebo atributy.                                                                                                                                                                                                    |
| Značky (značky glosář)          | Značky lze zadat data materiálů a atribut / sloupec úrovně. Glosář značky jsou definovat centrálně glosář, které můžete použít k zařadit do kategorií prostředky data nebo atributy pomocí běžné obchodní taxonomie. Další informace najdete v článku [o vytvoření Glosář Business řídí značek](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Odborníci        | Odborníci lze zadat na úrovni materiálů data. Odborníci identifikovat uživatele nebo skupiny s expert perspektivami na datech a lze sloužit jako styčný bod pro uživatele, kteří zjišťovat zdroje registrovaných dat a, které nejsou zodpovězené existující poznámky.  |
| Vyžádání přístupu | Žádosti o přístup k informacím o lze zadat na úrovni materiálů data. Tyto informace jsou pro uživatele, kteří zjišťovat zdroje dat, který není zatím mají oprávnění k přístupu. Uživatele můžete zadat e-mailovou adresu uživatele nebo skupinu, která zajišťuje přístup, adresa URL obrázku nebo nástroje, které uživatelé potřebovat přístup, nebo můžete zadat samotný proces jako text. |
| Si přečtěte následující dokumentaci | Si přečtěte následující dokumentaci lze zadat na úrovni materiálů data. Si přečtěte následující dokumentaci materiálů je informace formátovaného textu, který obsahuje odkazů a obrázků, a poskytující všechny informace, není vyjádřené pomocí popisy a značky. |


## <a name="annotating-multiple-assets"></a>Přidání poznámek více prostředky
Při výběru více prostředky dat na portálu katalogu dat, uživatelé může pořizovat poznámky v všechny vybrané prostředky do jednoho operace. Poznámky bude platit pro všechny vybrané majetek, což usnadňuje vyberte a zadejte konzistentní popis a sady značky a odborníky majetku související data.

> [AZURE.NOTE] Značky a odborníky lze také zadat při registrace prostředky dat pomocí katalogu dat datového zdroje nástroj pro registraci.

Kdy výběr více tabulek a zobrazení, pouze sloupce všechny vybraná data, která prostředky máte společné bude zobrazen v portálu katalog dat. To umožňuje uživatelům poskytnout značky a popis všech sloupců se stejným názvem pro všechny vybrané prostředky.

## <a name="annotations-and-discovery"></a>Poznámky a zjišťování
Stejně jako metadata extrahuje ze zdroje dat při registraci se přidá do indexu vyhledávání katalogu dat, uživatel zadal metadat taky indexováno. To znamená, že nejen poznámky usnadněte tak uživatelům rychlé porozumění datům, který zjistí poznámky také usnadněte tak uživatelům Seznamte se s poznámkami dat prostředky hledáním pomocí podmínek, které vyhovují na ně.

## <a name="summary"></a>Souhrn
Registrace zdroje dat s katalogem dat zajišťuje tato data zjistitelný zkopírováním strukturální a popisný metadata ze zdroje dat do služby katalogu. Po zaregistroval zdroje dat může uživatelům poskytovat poznámky usnadnit zjišťovat a interpretaci z v katalogu dat portálu.

## <a name="see-also"></a>Viz taky
- [Začínáme s katalogem dat Azure](data-catalog-get-started.md) kurz podrobné informace o tom, jak sledovat zdroje dat
