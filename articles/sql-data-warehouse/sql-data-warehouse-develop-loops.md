<properties
   pageTitle="Smyčky v SQL datový sklad | Microsoft Azure"
   description="Tipy pro smyčky jazyce Transact-SQL a nahrazení kurzory v Azure SQL datový sklad k vývoji řešení."
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

# <a name="loops-in-sql-data-warehouse"></a>Smyčky v SQL datový sklad
SQL datový sklad podporuje smyčka [během][] pro opakovaně spuštění příkazu bloky. To, zůstanou pro, pokud jsou zadané podmínky PRAVDA nebo až kód konkrétně ukončí, pomocí smyčka `BREAK` klíčových slov. Smyčky jsou užitečné zejména za účelem nahrazení kurzory podle kód SQL. Naštěstí skoro všech kurzory napsané v kódu SQL jsou rychlý přechod, přečtěte si jenom různých. [Během] smyčky proto jsou skvělé alternativní, když zjistíte, museli nahradit jednu.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Využívání smyčky a nahrazování kurzory v SQL datový sklad
Však před potápěním v záhlaví první zeptejte se sami následující otázky: "Tento kurzor lze znovu zapsat používat sadu založenou operace?". V mnoha případech odpověď bude Ano a je často nejlepším řešením. Operace sadu založenou často provede výrazně vyšší než přístupu k iterativních, po řádcích.

Rychlé dopředného kurzory jen pro čtení můžete snadno nahrazen příkazem opakování konstrukce. Níže je jednoduchý příklad. Tento příklad kódu aktualizuje statistiku všech tabulek v databázi. Pomocí tabulek v obraze provádí iterace budou moct provedení každého příkazu postupně.

Nejprve vytvořte dočasné tabulka, která obsahuje číslo jedinečné řádku slouží k identifikaci jednotlivé příkazy:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Za druhé inicializace proměnné potřebné k provedení opakovat:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Teď smyčku příkazy provádění jeden po druhém:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Nakonec přetáhněte dočasné tabulky vytvořené v prvním kroku

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[PŘI]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
