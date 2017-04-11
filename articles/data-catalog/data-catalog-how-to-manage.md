<properties
   pageTitle="Správa aktiv dat | Microsoft Azure"
   description="Článek s postupy zvýraznění jak určit viditelnost a vlastnictví dat aktiv registraci v katalogu dat Azure."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>Správa aktiv dat

## <a name="introduction"></a>Úvod

**Katalog dat Azure** poskytuje možnosti pro zjišťování zdroje dat, umožňuje uživatelům umožňuje snadné zjišťování a porozumět zdroje dat, které budou muset analýza a přijímání rozhodnutí. Při všem uživatelům můžete najít a pochopit nejrozmanitější dostupné zdroje dat, provádět tyto funkce zjišťování největší vliv. S tímto nezapomeňte katalogu dat se ve výchozím nastavení pro všechny zdroje dat registrované budou viditelné – a zjistitelný tak, že – všechny katalogu uživatelů.

Katalog dat není uživatelům přístup k datům samotné. Přístup k datům řídí vlastníka zdroje dat. Katalog dat umožňuje uživatelům zjišťovat zdroje dat a zobrazit metadata související s zdroje registrované v katalogu.

Může nastat situace, ale místo, kam zdroje dat by měl být pouze určitým uživatelům nebo členům skupiny konkrétní viditelný. Podobnému sledu katalog dat umožňuje uživatelům převzetí vlastnictví majetku registrovaná data v rámci katalogu a pak ovládací prvek viditelnost aktiva vlastní.

> [AZURE.NOTE] Funkce popisované v tomto článku jsou k dispozici pouze v standardní vydání z Azure katalogu dat. Bezplatné Edition neposkytuje vlastnictví a omezení viditelnost materiálů dat v rámci možnosti.

## <a name="managing-ownership-of-data-assets"></a>Správa vlastnictví dat majetku
Ve výchozím nastavení jsou data prostředky registraci v katalogu dat bez přiřazeného vlastnictví; všichni uživatelé s oprávněním přístup ke katalogu můžete prozkoumat a pořizovat poznámky v těchto prostředky. Uživatele můžete využít vlastnictví aktiv bez přiřazeného vlastnictví dat a můžete omezit viditelnost prostředky, které vlastní.

Když vlastní materiálů dat v katalogu dat pouze uživatelé povoleno vlastníky můžete Seznamte se s majetku zobrazit jeho metadata a pouze vlastníci můžete odstranit majetku z katalogu.

> [AZURE.NOTE] Vlastnictví v katalogu dat ovlivní pouze metadata uložená v katalogu. Nezaručuje žádné oprávnění v podkladovém zdroji dat.

### <a name="taking-ownership"></a>Převzetí vlastnictví
Uživatelů může trvat vlastnictví dat aktiv tak, že vyberete možnost "Převzít vlastnictví" v katalogu dat portálu. Žádné zvláštní oprávnění jsou požadované převzetí vlastnictví aktiva bez přiřazeného vlastnictví dat; Každý uživatel může trvat vlastnictví aktiva bez přiřazeného vlastnictví data.

### <a name="adding-owners-and-co-owners"></a>Přidání vlastníkům a dalších vlastníkům
Pokud už vlastní dat materiálů, uživatelé nelze provést jednoduše vlastnictví – musí být přidat jako spolu vlastníci existující vlastníka. Všechny vlastník můžete přidat další uživatele nebo skupiny zabezpečení jako spolu vlastníci.

> [AZURE.NOTE] Je doporučený postup mít aspoň dva osobám jako vlastníci pro všechny vlastnictví dat materiálů.

### <a name="removing-owners"></a>Odebrání vlastníků
Stejně jako jakékoli materiálů vlastník můžete přidat spolu vlastníci, všechny materiály vlastník můžete odebrat všechny spoluvlastníka.

Pokud některé vlastníky majetku odebere sám jako vlastníka, mu majetku už spravovat. Pokud některé vlastníky majetku: Odstraní sám jako některé vlastníky a jsou bez dalších vlastníci, majetku se vrátit k stavu bez přiřazeného vlastnictví.

## <a name="visibility"></a>Viditelnost
Vlastníci materiálů dat můžete ovládat viditelnost majetek dat, který vlastní. Pro omezení viditelnost výchozí – kde všichni uživatelé katalogu dat můžete prozkoumat a zobrazení dat majetku – vlastníka majetku můžete zapíná a vypíná nastavení viditelnosti z "Všichni" na "Vlastníci a tyto uživatele" ve vlastnostech majetku. Vlastníci potom můžete přidat určití uživatelé a skupiny zabezpečení.

> [AZURE.NOTE] Kdykoli je to možné, je vhodné majetku vlastnictví a viditelnost oprávnění přiřadit skupinám zabezpečení a jednotlivé uživatele.

## <a name="catalog-administrators"></a>Správci katalogu
Správci katalogu dat jsou implicitně vedlejšího vlastníky všechny vybavení katalogu. Vlastníci materiálů nemůžete odebrat viditelnost z katalogu správci a správci můžou spravovat vlastnictví a viditelnosti pro všechna data aktiva v katalogu.

## <a name="summary"></a>Souhrn
Katalog dat crowdsourcing model metadata a data zjišťování materiálů všem uživatelům katalogu přispívání a seznamte se s. Standardní vydání z katalogu dat poskytuje možnosti pro vlastnictví a správu omezit viditelnost a použití aktiv určitá data.
