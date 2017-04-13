<properties
   pageTitle="Zobrazení SQL datový sklad | Microsoft Azure"
   description="Tipy pro použití zobrazení jazyce Transact-SQL v Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Zobrazení SQL datový sklad

Zobrazení jsou užitečné zejména v SQL datový sklad. Použitím několika různými způsoby zlepšit kvalitu řešení.  Tento článek popisuje pár příklady obohacení řešení se zobrazení, jakož i omezení, které je potřeba považovat za.

> [AZURE.NOTE] Syntaxe `CREATE VIEW` není popisované v tomto článku. Získáte v článku [Vytvoření zobrazení][] na webu MSDN referenční informace.

## <a name="architectural-abstraction"></a>Architektonické odběru
Velmi běžné vzorek aplikace je k opětovnému vytvoření tabulky pomocí vytvoření tabulky jako vyberte (CTAS) a za ním uveďte objektu přejmenování vzorku, zatímco načítání dat.

V příkladu níže přidá nové datum záznamy dimenze datum. Poznámka týkající se nové tabble DimDate_New, vytvoření a potom přejmenovat nahrazení původní verze tabulky.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Tento postup však může způsobit tabulek zobrazená a ztrácejí ze zobrazení uživatele, stejně jako "tabulky neexistuje" chybové zprávy. Zobrazení můžete použít k uživatelům poskytnout vrstvě konzistentní prezentace zatímco jsou přejmenované objekty. Tím, že uživatelům přístup k datům až zobrazení, znamená, že uživatelé nemusí používat viditelnost podkladové tabulky. To poskytuje konzistentní uživatelského prostředí a zajistit, že data sklad Návrháři můžete vyvíjet datového modelu zvýšení výkonu pomocí CTAS během načítání proces dat.    

## <a name="performance-optimization"></a>Optimalizace výkonu
Zobrazení můžete taky využít k jejímu vynucení výkonu optimalizované spojení mezi tabulkami. Zobrazení můžete například zahrnutí nadbytečné distribuční klíč jako součást spojování kritéria minimalizovat přesun dat.  Další výhodou zobrazení může být vynutit konkrétní dotazu nebo spojování tip. Použití zobrazení tímto způsobem zaručuje, že vždy spojení optimální způsobem, že uživatelé zapamatovatelné správné konstrukce pro jejich spojení.

## <a name="limitations"></a>Omezení
Zobrazení v SQL datový sklad jsou pouze metadata.  Proto nejsou k dispozici následující možnosti:

-   Žádným způsobem schématu vazby
-   Základní tabulky nelze aktualizovat prostřednictvím zobrazení
-   Zobrazení nelze vytvořit přes dočasné tabulky
-   Žádná podpora pro ROZBALENÍ či NOEXPAND tipy
-   V SQL datový sklad jsou žádná indexované zobrazení


## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].
Pro `CREATE VIEW` syntaxe získáte [Vytvořit zobrazení][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje SQL datový sklad]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[VYTVOŘENÍ ZOBRAZENÍ]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
