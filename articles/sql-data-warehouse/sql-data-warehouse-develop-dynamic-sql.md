<properties
   pageTitle="Dynamické SQL v SQL datový sklad | Microsoft Azure"
   description="Tipy pro použití dynamického příkazu SQL v Azure SQL datový sklad k vývoji řešení."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamické SQL v SQL datový sklad
Při vytváření kódu aplikace pro systém SQL Data Warehouse budete muset předvádění flexibilní, obecný a moduly řešení pomocí dynamického příkazu sql. SQL datový sklad nepodporuje objektů blob datové typy v současné době. Jako typy objektů blob zahrnují typy varchar(max) a nvarchar(max) to může omezit velikost vašich řetězců. Pokud použijete tyto typy v kódu aplikace při vytváření obrovské řetězce, bude muset kód rozdělí bloky a použijte příkaz spouštění.

Jednoduchý příklad:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Pokud je krátké řetězec můžete [sp_executesql][] jako normálně.

> [AZURE.NOTE] Příkazy spouštět jako dynamické SQL budou nadále vyměřené poplatky za jeho všechna pravidla ověření TSQL.

## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
