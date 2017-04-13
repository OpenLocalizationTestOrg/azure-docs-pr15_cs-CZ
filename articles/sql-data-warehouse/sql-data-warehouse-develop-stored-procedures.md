<properties
   pageTitle="V SQL Data Warehouse uložené procedury | Microsoft Azure"
   description="Tipy pro implementaci v Azure SQL Data Warehouse uložené procedury pro vývoj řešení."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>V SQL Data Warehouse uložené procedury

SQL datový sklad podporuje spoustu funkcí jazyce Transact-SQL serveru SQL Server. Existuje důležitější měřítko se konkrétních funkcí, které chceme využíval pro zvýšení výkonu řešení.

Chcete-li zachovat měřítko a výkonu SQL datový sklad tam jsou však také některé funkce a funkce, které mají chování rozdíly i další uživatelé, které nejsou podporovány.

Tento článek vysvětluje, jak provádět v rámci SQL Data Warehouse uložené procedury.

## <a name="introducing-stored-procedures"></a>Úvodní informace o uložené procedury
Uložené procedury jsou vhodné pro zapouzdření kódu SQL; uložením zavřít dat v datový sklad. Zapouzdřením kód na samostatně zvládnutelné jednotky uložené procedury vývojáři modularize jejich řešení; usnadnění větší znovuvyužití kódu. Všechny uložené procedury jsou přijatelné také parametry pro jejich i flexibilnější.

SQL datový sklad poskytuje implementaci zjednodušené a optimalizovaného uložená procedura. Největší rozdíl v porovnání s SQL Server je uložená procedura není předem zkompilovaný kód. V datových úložištích nás zajímala obecně méně času kompilace. Je další důležité, že kód uložená procedura správně optimalizována pracuje proti velkých objemů dat. Cílem je uložit hodiny, minuty a sekundy není milisekund. Je proto užitečnější přemýšlet o uložené procedury jako kontejnery pro použití logických operátorů SQL.     

Při SQL Data Warehouse uložené procedury jsou příkazy SQL analyzovat, přeložený a optimalizované za běhu. Během tohoto procesu každého příkazu převedený na distribuované dotazů. Kód SQL, který je opravdu spouštět oproti data se liší dotazu odeslaný.

## <a name="nesting-stored-procedures"></a>Vnoření uložené procedury
Uložené procedury při volání jiné uložené procedury nebo spuštění dynamického příkazu sql pak vnitřní uložené procedury nebo kód vyvolání se označuje jako vnořený.

SQL datový sklad podporují maximálně 8 počet úrovní vnoření. Toto je mírně odlišnou SQL Server. Úroveň nest na serveru SQL Server je 32.

Volání nejvyšší úrovně uložené procedury rovná vnořit úrovně 1

```sql
EXEC prc_nesting
```
Pokud uložená procedura také provede jiný spouštění volání pak zvýší úroveň nest 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Pokud druhý postup provede některých dynamických sql a pak to zvýší úroveň nest 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Poznámka: SQL datový sklad aktuálně nepodporuje @@NESTLEVEL. Potřebujete sledovat úrovně nest sami. Není pravděpodobné stisknutí 8 nest úroveň omezení, ale když uděláte je třeba znovu práce a "sloučit" to tak, aby byla v rámci této aplikaci.

## <a name="insertexecute"></a>VLOŽENÍ. SPUŠTĚNÍ
SQL datový sklad nedovoluje používat sadu výsledků uložené procedury příkazem Vložit. Je ale alternativní přístup, které můžete použít.

Získáte v článku znalostní báze [dočasné tabulky] například o tom, jak to udělat.

## <a name="limitations"></a>Omezení

Existují některé aspekty postupy Transact-SQL uložené, které nejsou součástí SQL datový sklad.

Jsou:

- dočasné uložené procedury
- Číslovaný uložené procedury
- Rozšířené uložené procedury
- CLR uložené procedury
- možnost šifrování
- Možnosti replikace
- Parametry vracející tabulku
- parametry jen pro čtení
- výchozí hodnoty parametrů
- spuštění kontexty
- příkaz vrácení

## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[dočasné tabulky]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Přehled vývoje]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
