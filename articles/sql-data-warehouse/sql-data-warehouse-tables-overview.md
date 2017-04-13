<properties
   pageTitle="Základní informace o tabulkách v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s tabulkami sklad dat SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Základní informace o tabulkách v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

Začínáme s vytvářením tabulek v SQL datový sklad je velmi jednoduché.  Základní syntaxe [CREATE TABLE][] spočívá v běžných syntaxe pravděpodobně už znáte z práce s dalších databází.  Pokud chcete vytvořit tabulku, stačí název nové tabulky, název sloupce a definice datové typy pro všechny sloupce.  Pokud vytvoříte tabulky do jiné databáze by měl vypadat takto povědomý vám.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Výše uvedený příklad vytvoří tabulku s názvem Zákazníci se dvěma sloupci, jméno a příjmení.  Každý sloupec je definován s datovým typem VARCHAR(25), které omezí data na 25 znaků.  Tyto základní atributy tabulky, jakož i ostatním, jsou převážně dalších databází.  Datové typy jsou definované pro všechny sloupce a zajištění integrity dat.  Indexy lze přidat ke zlepšení výkonu zmenšením vstupu a výstupu.  Rozdělení lze přidat ke zlepšení výkonu, když potřebujete změnit data.

[Přejmenování] [ RENAME] SQL datový sklad tabulka vypadá takto:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Distribuované tabulek

Nový základní atribut zavedený distribuované systémy jako SQL datový sklad je **rozdělení sloupce**.  Rozdělení sloupce je velmi mnohem co vypadá jako.  Je sloupec, který určuje, jak distribuovat nebo dělit dat na pozadí.  Po vytvoření tabulky bez zadání rozdělení sloupce v tabulce automaticky distribuovaná pomocí **Kruhové**.  Kruhového tabulky mohou být v některých scénářích dostatečném, definování rozdělení sloupce můžete značně snížit přesun dat v dotazech, tedy optimalizaci výkonu.  V tématu [Distribuce tabulky][Rozmístit] zobrazíte další informace o tom, jak vybrat sloupce rozdělení.

## <a name="indexing-and-partitioning-tables"></a>Indexování a rozdělení tabulek

Se stanou pokročilejší při používání SQL datový sklad a chcete optimalizovat výkon, je vhodné zobrazíte další informace o tabulky – návrh.  Další informace naleznete v článcích na [Tabulky datové typy][Datové typy], [Distribuce tabulky][distribuovat], [indexování tabulky][Index] a [rozdělení tabulky][oddíl].

## <a name="table-statistics"></a>Statistiky tabulek

Statistiky jsou velice důležité, jak optimálního výkonu mimo datový sklad SQL.  Vzhledem k tomu SQL datový sklad ještě automaticky nevytvoří a aktualizace statistiky za vás, jako se může pocházet očekávat při databáze SQL Azure, čtení naše článek [statistik][] může jít o některý z nejdůležitějších článků si přečíst zajistí, že optimálního výkonu z dotazů.

## <a name="temporary-tables"></a>Dočasné tabulky

Dočasné tabulky jsou tabulky, které jenom existují dobu trvání přihlášení a nelze prezentací, které ostatním uživatelům.  Dočasné tabulky může být užitečné při zabránit ostatním tak dočasné výsledků a také sníží nutnosti vyčištění.  Protože dočasné tabulky také použít místní úložiště, nabízejí dosažení vyššího výkonu pro některé operace.  Naleznete v článcích [Dočasná tabulka][dočasné] další podrobnosti o dočasné tabulky.

## <a name="external-tables"></a>Externí tabulky

Externí tabulky, nazývaný také Polybase tabulkami, jsou tabulky, které jde zpracovávat z SQL datový sklad, ale bodu na externí data z SQL datový sklad.  Můžete například vytvořit externí tabulku která odkazuje soubory v úložišti objektů Blob Azure.  Podrobné informace o tom, jak vytvořit a dotaz externí tabulku najdete v článku [načítání dat s Polybase][].  

## <a name="unsupported-table-features"></a>Nepodporované funkce tabulky

I když SQL datový sklad obsahuje spoustu stejných funkcí tabulky nabízená dalších databází, existují některé funkce, které zatím nejsou podporované.  Tady je seznam některé z funkcí tabulky, které zatím nejsou podporované.

| Nepodporované funkce |
| --- |
|[Vlastnost identity][] (viz téma [Přiřazení náhradní celočíselné klíče alternativní řešení:][])|
|Primární klíč, cizích klíčů, jedinečný a kontrola [Omezení tabulky][]|
|[Jedinečné indexy.][]|
|[Vypočítané sloupce][]|
|[Sparse sloupců][]|
|[Typy definované uživatelem][]|
|[Pořadí][]|
|[Aktivace][]|
|[Indexovaná zobrazení][]|
|[Synonyma][]|

## <a name="table-size-queries"></a>Dotazy velikosti tabulky

Jednoduchý způsob k určení místa a řádky využívané tabulky ve všech 60 rozdělení je použít [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Však pomocí příkazů DBCC může být docela omezení.  Správa dynamických zobrazení (DMVs) vám umožní zobrazit víc podrobností i umožňují mnohem větší kontrolu nad výsledky dotazu.  Začněte tak, že vytvoříte toto zobrazení, které budou uvedené v mnoha příkladech v tomto a další články.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tabulkový prostor Souhrn

Tento dotaz vrátí řádky a místo v tabulce.  Je skvělé dotazu zobrazíte tabulky, které jsou největší tabulek a jestli kruhového nebo hash distributed.  U tabulek hash distributed také ukazuje rozdělení sloupce.  Ve většině případů by měl být největší tabulkách hash rozdělení s skupinový columnstore index.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabulky místo podle typu rozdělení.

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabulky místo podle typu indexu

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Rozdělení místo souhrn

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Další kroky

Další informace najdete v článcích [Tabulky datové typy][Datové typy], [Distribuce tabulky][distribuovat], [indexování tabulky][Index], [rozdělení tabulky][oddílu], [Zachování statistiky tabulek][Statistika] a [Dočasné tabulky][dočasné].  Další informace o doporučených postupech najdete v tématu [SQL Data Warehouse doporučené postupy][].

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
[Načtení dat s Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[VYTVOŘENÍ TABULKY]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Vlastnost identity]: https://msdn.microsoft.com/library/ms186775.aspx
[Přiřazení řešení náhradní celočíselné klíče:]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Omezení tabulky]: https://msdn.microsoft.com/library/ms188066.aspx
[Vypočítané sloupce]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse sloupců]: https://msdn.microsoft.com/library/cc280604.aspx
[Typy definované uživatelem]: https://msdn.microsoft.com/library/ms131694.aspx
[Pořadí]: https://msdn.microsoft.com/library/ff878091.aspx
[Aktivace]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexovaná zobrazení]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyma]: https://msdn.microsoft.com/library/ms177544.aspx
[Jedinečné indexy.]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
