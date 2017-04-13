<properties
    pageTitle="Tipy pro ladění výkonu databáze SQL | Microsoft Azure"
    description="Tipy pro optimalizace v databázi SQL Azure pomocí hodnocení a zlepšení výkonu."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="Optimalizace výkonu databáze ladění výkonu sql optimalizace tipy najdete výkonu SQL ladění výkonu databáze sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>Tipy optimalizaci výkonu databáze SQL
Můžete změnit [vrstvy služeb](sql-database-service-tiers.md) z jedné databáze nebo zvětšit eDTUs fondu pružná databáze kdykoli zvýšit výkon, ale můžete určit příležitosti ke zlepšení a optimalizace výkonu dotazu nejdřív. Chybějící indexy a špatně optimalizované dotazů jsou běžné příčiny špatné databáze výkonu. Tento článek obsahuje pokyny k v databázi SQL ladění výkonu.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Postup při vyhodnocení a ladění výkonu databáze
1.  Na [Portálu Azure](https://portal.azure.com)klikněte **SQL databáze**, vyberte databázi a použijte sledování grafu můžete vyhledávat zdroje se přiblížíte k jejich maximum. DTU spotřeba se zobrazují ve výchozím nastavení. Klikněte na **Upravit** změňte časový rozsah a hodnoty zobrazené.
2.  Použijte [Přehled výkonu dotazu](sql-database-query-performance.md) k posouzení dotazů pomocí DTUs a zobrazení doporučení pro vytváření a rušení indexů parametrizování dotazů a řešení potíží schématu pomocí [Advisor databáze SQL](sql-database-advisor.md) .
3.  Chcete-li získat výkonu parametrů v reálném čase můžete použít Správa dynamických zobrazení (DMVs), rozšířené události (Xevents) a úložišti dotazu v SSMS. Naleznete v [tématu výkon pokyny](sql-database-performance-guidance.md) pro podrobné sledování a optimalizace tipy.


    > [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Postup pro zvýšení výkonu databáze s další materiály
1.  Pro jeden databáze můžete [změnit úrovně služby](sql-database-scale-up.md) na vyžádání ke zlepšení výkonu databáze.
2.  Více databázím zvažte použití [fondů pružná databáze](sql-database-elastic-pool-guidance.md) zobrazit zdroje automaticky.
