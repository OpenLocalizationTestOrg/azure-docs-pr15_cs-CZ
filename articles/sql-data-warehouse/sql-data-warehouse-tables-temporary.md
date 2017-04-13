<properties
   pageTitle="Dočasné tabulky v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s dočasnými tabulkami v Azure SQL datový sklad."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Dočasné tabulky v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

Dočasné tabulky jsou velmi užitečná při zpracování dat – zejména během transformace, kde jsou přechodná intermediate výsledky. V SQL datový sklad dočasné tabulky existují na úrovni relace.  Budou viditelné pouze relace, ve kterém jsou vytvořené a když relace odhlásí se automaticky nezobrazí.  Dočasné tabulky nabízejí výkonu výhoda protože jejich výsledky jsou aby došlo k zápisu místní místo vzdálené úložiště.  Dočasné tabulky jsou v Azure SQL datový sklad mírně liší od databáze SQL Azure, jak lze k nim z libovolné místo uvnitř relaci, včetně uvnitř a mimo uložená procedura.

Tento článek obsahuje základní doprovodné materiály pro používání dočasné tabulky a zvýrazní se zásadami relace úrovně dočasné tabulky. Podle pokynů v tomto článku vám můžou pomoct modularize kódu vylepšení opětovné použití a snadnější údržby kódu.

## <a name="create-a-temporary-table"></a>Vytvoření dočasné tabulky

Dočasné tabulky vytvořené tak, že jednoduše obsahující předponu názvu tabulky se `#`.  Příklad:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Dočasné tabulky lze také vytvořit pomocí `CTAS` použití přesně stejné přístupu:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`je hodně výkonná příkaz a má výhodu, je velmi efektivní v jeho použití prostoru protokolu transakce. 


## <a name="dropping-temporary-tables"></a>Odstranění dočasných tabulek

Po vytvoření novou relaci by měl existovat žádné dočasné tabulky.  Ale pokud voláte stejné uložená procedura, který vytváří dočasný se stejným názvem, zajistit, aby vaše `CREATE TABLE` příkazy jsou úspěšné jednoduché zaškrtnutí před existenci s `DROP` mohou sloužit jako v příkladu:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Kódování konzistence, je vhodné použít tento vzor pro tabulky a dočasné tabulky.  Je taky dobré použít `DROP TABLE` odebrat dočasné tabulky po dokončení s nimi v kódu.  V uložená procedura se vyčistí vývoj, které je úplně běžné najdete v tématu příkazy rozevírací seskupené dohromady konci postup zajistit tyto objekty.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kód

Protože dočasné tabulky můžete vidět kdekoli v relaci uživatele, můžete to zneužil vám modularize kód aplikace.  Například následující uložená procedura spojují doporučené postupy shora generovat DDL, které se aktualizuje všechny statistiky databáze podle názvu statistický.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

V této fázi, ke kterému se pouze akce, že došlo k vytvoření uložená procedura, která bude jednoduše vygeneruje dočasné tabulky #stats_ddl s příkazy DDL.  Uložená procedura bude přetáhnout #stats_ddl, pokud již existuje zajistit, že že není nezdaří, pokud víckrát spustit v rámci relace.  Protože neexistuje žádné `DROP TABLE` na konci uložená procedura, až se dokončí uložená procedura ho ponechá vytvořenou tabulku tak, aby ho mohli číst mimo uložená procedura.  V SQL datový sklad na rozdíl od jiných databáze systému SQL Server je možné použít dočasné tabulku mimo postup, který byl vytvořen.  Dočasné tabulky SQL datový sklad může být použité **kdekoli** v relaci. Může dojít k další moduly a spravovatelné s kódem jako v příkladu:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Dočasná tabulka omezení

SQL datový sklad stanovení pár omezení při provádění dočasné tabulky.  V současné době pouze relaci obory dočasné tabulky jsou podporovány.  Globální dočasné tabulky nejsou podporované.  Kromě toho zobrazení nelze vytvořit pro dočasné tabulky.

## <a name="next-steps"></a>Další kroky

Další informace najdete v článcích [Přehled tabulky][Přehled], [Tabulky datové typy][Datové typy], [Distribuce tabulky][distribuovat], [indexování tabulky][Index], [rozdělení tabulky][oddíl] a [Zachování statistiky tabulek][statistiky].  Další informace o doporučených postupech najdete v tématu [SQL Data Warehouse doporučené postupy][].

<!--Image references-->

<!--Article references-->
[Základní informace]: ./sql-data-warehouse-tables-overview.md
[Datové typy]: ./sql-data-warehouse-tables-data-types.md
[Distribuce]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Oddíl]: ./sql-data-warehouse-tables-partition.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Dočasné]: ./sql-data-warehouse-tables-temporary.md
[SQL datový sklad doporučené postupy]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
