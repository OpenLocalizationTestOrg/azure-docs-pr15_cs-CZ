<properties
   pageTitle="Návrh rozhodnutí a kódovací techniky pro vývoj SQL datový sklad | Microsoft Azure"
   description="Vývojové koncepty, navrhování, doporučení a kódování technik pro SQL datový sklad."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Navrhování a kódování technik pro datový sklad SQL

Prohlédněte si pomocí těchto článků vývoj a snadněji pochopit hlavních rozhodnutí, doporučení a kódování technik pro SQL datový sklad.

## <a name="key-design-decisions"></a>Klíčové navrhování
V následujících článcích zvýraznění pár základních koncepcí a navrhování potřebných pochopit pro vývoj distribuované datový sklad pomocí SQL datový sklad:

- [připojení][]
- [souběžné][]
- [transakce][]
- [schémata definované uživatelem][]
- [distribuce tabulky][]
- [indexy tabulek][]
- [Tabulka oddíly][]
- [CTAS][]
- [statistiky][]

## <a name="development-recommendations-and-coding-techniques"></a>Vývojové doporučení a kódovací techniky
Tyto články zvýraznění konkrétní kódovací techniky, tipy a doporučení k vývoji datový sklad SQL:

- [uložené procedury][]
- [popisky][]
- [zobrazení][]
- [dočasné tabulky][]
- [dynamické SQL][]
- [opakování][]
- [Seskupit podle možností][]
- [proměnné přiřazení][]

## <a name="next-steps"></a>Další kroky
Po až článků vývoj podívejte se na stránce [jazyce Transact-SQL odkaz][] podrobné informace o podporovaných syntaxi pro SQL datový sklad.

<!--Image references-->

<!--Article references-->
[souběžné]: ./sql-data-warehouse-develop-concurrency.md
[připojení]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamické SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Seskupit podle možností]: ./sql-data-warehouse-develop-group-by-options.md
[popisky]: ./sql-data-warehouse-develop-label.md
[opakování]: ./sql-data-warehouse-develop-loops.md
[statistiky]: ./sql-data-warehouse-tables-statistics.md
[uložené procedury]: ./sql-data-warehouse-develop-stored-procedures.md
[distribuce tabulky]: ./sql-data-warehouse-tables-distribute.md
[indexy tabulek]: ./sql-data-warehouse-tables-index.md
[Tabulka oddíly]: ./sql-data-warehouse-tables-partition.md
[dočasné tabulky]: ./sql-data-warehouse-tables-temporary.md
[transakce]: ./sql-data-warehouse-develop-transactions.md
[schémata definované uživatelem]: ./sql-data-warehouse-develop-user-defined-schemas.md
[proměnné přiřazení]: ./sql-data-warehouse-develop-variable-assignment.md
[zobrazení]: ./sql-data-warehouse-develop-views.md
[Odkaz Transact-SQL]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
