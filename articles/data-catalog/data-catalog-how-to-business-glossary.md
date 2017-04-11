<properties
    pageTitle="Jak nastavit Glosář Business pro řídit označování | Microsoft Azure"
    description="Článek s postupy zvýraznění Glosář business v katalogu dat Azure pro definice a používání běžné obchodní slovník označit registrované dat prostředky."
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

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Jak nastavit Glosář firmy řídit značek

## <a name="introduction"></a>Úvod

Azure katalog dat poskytuje možnosti pro zjišťování zdroje dat, umožňuje uživatelům umožňuje snadné zjišťování a vysvětlení zdroje dat, které budou muset analýza a přijímání rozhodnutí. Při uživatelů můžete najít a vysvětlení nejrozmanitější dostupné zdroje dat provést tyto funkce zjišťování největší vliv.

Označování jeden funkce katalogu dat, která podporuje větší Principy dat prostředky. Označování umožňuje uživatelům přidružit aktivum nebo sloupec, který zase usnadňuje Seznamte se s materiálů pomocí vyhledávání a procházení, klíčových slov a umožní uživatelům snadněji pochopit kontext a záměr majetku.

Však označování někdy může způsobit problémy vlastní. Některé problémy, které mohou pocházet označování příklady:

1.  Uživatelé pomocí zkratek u některých prostředky a rozšířený text na ostatní při označování. Této nekonzistence omezuje zjišťování prostředky, i když záměr byl označení aktiva s stejnou značku.
2.  Značky, které znamenat různé věci v různých kontextech. Například značku s názvem "Výnos" se sadou dat zákazníků může nechtěli příjmů podle zákazníka, ale na stejnou značku čtvrtletní prodeje datovou sadu znamenat Čtvrtletní výnosy společnosti.  

Katalog dat pomohou při řešení těchto i jiných podobné úkoly, obsahuje glosář Business.

Glosář firmy katalogu dat umožňuje organizacím dokumentu klíčovými podnikovými podmínky a jejich vysvětlení vytvořit běžné obchodní slovníku. Této zásady správného řízení umožňuje konzistence použití dat v rámci celé organizace. Jakmile termínů v Glosář firmy, můžete přidělovat majetku dat v katalogu používají stejný přístup značek a umožňuje _řídit značky_.

> [AZURE.NOTE] Funkce popisované v tomto článku jsou k dispozici pouze v standardní vydání z Azure katalogu dat. Bezplatné Edition neposkytuje funkcí pro upraveny označování nebo Glosář business.

## <a name="glossary-availability-and-privileges"></a>Glosář dostupnost a oprávnění

*Glosář firmy je dostupné ve standardní vydání z Azure katalogu dat. Bezplatné Edition z katalogu dat nezahrnuje Glosář.*

Glosář firmy můžete k nim získat přístup prostřednictvím možnost "Glosář" v portálu katalog dat navigační nabídku.  

![Přístup k Glosář business](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Správci katalogu dat a členy této role správců Glosář můžete vytvářet, upravovat a odstraňovat Glosář termínů v Glosář business. Všichni uživatelé katalogu dat můžete zobrazit definici termínů a můžete označit aktiv pomocí Glosář.

![Přidání nového Glosář termínů](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Vytváření Glosář

Správci katalogu dat a glosář můžete vytvořit nový Glosář kliknutím na nový termín "tlačítko Vytvořit glosář s následující pole:

* Definice firmy termínu
* Popis, který zaznamenává zamýšlen nebo podniková pravidla materiálů a sloupci
* Seznam účastníků, kteří nejvíce vědět termín
* Nadřazený termín, který definuje hierarchie, ve kterém jsou uspořádána termín


## <a name="glossary-term-hierarchies"></a>Glosář termínů hierarchie

Glosář firmy katalog dat umožňuje popisují firmy slovníku jako hierarchie termínů. To umožňuje organizacím vytvoření klasifikace termínů lépe představující své firmy taxonomie.

Název termínu musí být jedinečný dané úrovně hierarchie – nejsou povolené názvy duplicitních. Není omezena na požadovaný počet úrovní v hierarchii, ale hierarchie je často srozumitelnější Pokud existují tři úrovně nebo méně.

Použití hierarchií v Glosář firmy je nepovinný krok. Nechte nadřazeného termínu pole prázdné Glosář termínů vytvoří seznam ploché (jiných než hierarchických) termínů v Glosář.  

## <a name="tagging-assets-with-glossary-terms"></a>Označování aktiv pomocí Glosář

Po definování Glosář termínů v rámci katalogu experience značení prostředky optimalizováno pro hledání Glosář během zadávání znaků uživatelem jejich značky. Na portálu katalog dat zobrazí seznam odpovídajících Glosář pro uživatele můžete vybírat. Pokud uživatel vybere Glosář termínů v seznamu pole bude přidáno do majetku jako značka (také Glosář značka). Uživatele můžete taky zvolit vytvořit novou tak, že zadáte výraz, který není v Glosář (také značka uživatele).

![Data materiálů označený značku jednomu uživateli a dvě Glosář značky](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Značky uživatele jsou pouze typu značky podporované v bezplatné Edition dat katalogu.

### <a name="hover-behavior-on-tags"></a>Umístěte ukazatel chování značek
Na portálu katalogu dat se vizuálně liší, pomocí různých hover chování dva typy značky. Když uživatel něj myší značku uživatel vidí text značky a uživatele nebo uživatelů, kteří přidali značku. Když uživatel něj myší Glosář značku, vidí taky definici Glosář termínů a odkaz otevřete Glosář firmy zobrazení úplné definice termín.

### <a name="search-filters-for-tags"></a>Filtry hledání značek
Glosář značky a značky uživatele jsou vyhledávání a můžete použijí jako filtry v hledání.

## <a name="summary"></a>Souhrn
Glosář business v katalogu dat Azure a upraveny značky, které umožňuje, počkejte, až prostředky dat identifikovat, spravovat, a zjistil konzistentní. Glosář firmy můžete zvýšit výukové slovník business mezi uživatelům v organizaci a podporuje smysluplné metadata zachycení, díky materiálů zjišťování Principy uloženy.

## <a name="see-also"></a>Viz taky

- [Rozhraní REST API si přečtěte následující dokumentaci pro Glosář podnikání](https://msdn.microsoft.com/library/mt708855.aspx)
