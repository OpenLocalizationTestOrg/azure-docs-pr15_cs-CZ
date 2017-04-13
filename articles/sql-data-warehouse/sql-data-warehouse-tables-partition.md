<properties
   pageTitle="Rozdělení tabulek v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s v Azure SQL datový sklad vytvoření oddílů tabulky."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Rozdělení tabulek v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

Rozdělení je podporována pro všechny typy tabulky SQL datový sklad; včetně skupinový columnstore seskupený index a haldy.  Rozdělení podporuje i pro všechny typy rozdělení, včetně hash nebo kruhového rozdělením.  Rozdělení umožňuje k datům rozdělit menšími skupinami dat a ve většině případů rozdělení probíhá na sloupec kalendářních dat.

## <a name="benefits-of-partitioning"></a>Výhody rozdělení

Rozdělení přináší výkon údržbu a dotaz data.  Zda výhody obě nebo jenom jeden je závisí na tom, jak je načtení dat a jestli stejného sloupce se dá použít pro oba účely, protože rozdělení lze provést pouze jeden sloupec.

### <a name="benefits-to-loads"></a>Výhody zatížení

Primární výhody rozdělení v SQL datový sklad je zvýšení efektivity a výkonu načtení dat použitím oddílu odstranění přepínání a sloučení.  Ve většině případů dat rozdělit na sloupec kalendářních dat, který je těsně svázané se posloupnosti, což je načtení dat do databáze.  Jednou z výhod použití oddílů ke správě dat největší ho zabránění transakce protokolování.  Během jednoduše vložení, aktualizace nebo odstranění dat může být nejjednodušší přístup s nevelká myšlenkových a plánování řízené úsilí, pomocí rozdělení během procesu načíst podstatně dosáhnout zvýšení výkonu.

Oddíl přechody mohou sloužit k rychle odebrat nebo nahrazení části tabulky.  Například tabulce faktů prodejní může obsahovat pouze data pro minulých 36 měsíců.  Na konci každého měsíce po nejstarší měsíc data o prodeji odstraněna z tabulky.  Tato data může odstranit pomocí příkazu odstranit ke smazání dat od nejstaršího měsíce.  Však odstranění velkého množství dat řádek po řádku příkazem Odstranit může trvat velmi dlouho, jakož i vytvořit riziko velké transakce, které může trvat dlouho k vrácení zpět, pokud dojde k chybě.  Další optimální přístup je jednoduše přetáhnout oddílu od nejstaršího data.  Pokud odstraníte jednotlivé řádky může trvat hodin, odstranění celého oddílu může trvat sekund.

### <a name="benefits-to-queries"></a>Výhody dotazů

Rozdělení lze také ke zlepšení výkonu dotazu.  Pokud dotaz použije filtr na sloupec rozdělení, toto omezení kontroly pouze vyhovující oddíly, které mohou být mnohem menší podmnožinu dat, že prohledání celé tabulky.  Zavedení skupinový columnstore indexy výhody výkonu predikátu odstranění jsou menší skutečným, ale v některých případech může být přínos pro dotazy.  Například tabulky faktů prodejní rozdělit na 36 měsíců pomocí pole Datum prodeje a nakonec dotazy filtr na datum prodeje můžete přejít hledání v zobrazení oddílů, které neodpovídají zadanému filtr.

## <a name="partition-sizing-guidance"></a>Pokyny pro změnu velikosti oddílu

Při rozdělení lze zlepšit výkon některé scénáře, můžete vytvořit tabulku s **příliš mnoho** oddíly snížit výkon za určitých okolností.  Těmto problémům jsou především pro skupinový columnstore tabulkami.  Pro rozdělení být užitečné, je důležité pochopit použití oddílů a počet oddílů na vytvořit.  Neexistuje žádné pevné rychlé pravidlo tak, aby oddíly kolik je příliš mnoho, závisí na vaše data a kolik oddíly načítáte do současně.  Ale jako obecné pravidlo, si můžete představit přidání 10s k 100s oddílů, ne 1000s.

Při vytváření oddílů na **Skupinový columnstore** tabulkami, je důležité zvážit, kolik řádků se má v každém oddílu.  Pro optimální komprese a výkonu skupinový columnstore tabulkami je potřeba minimálně 1 milion řádků na rozdělení a oddíl.  Před vytvořením oddíly SQL datový sklad už rozdělí každou tabulku do 60 distribuované databází.  Všechny rozdělení přidána do tabulky je kromě distribuce vytvořili na pozadí.  V tomto příkladu s použitím Pokud tabulce faktů prodejní obsažené 36 měsíční oddíly a vzhledem má SQL datový sklad 60 distribuce a potom prodejní faktů tabulka by měla obsahovat 60 milionů řádky za měsíc, nebo pouze 2.1 miliardy po naplnění všechny měsíce.  Pokud tabulka obsahuje výrazně méně řádků, než doporučené minimální počet řádků na oddíl, zvažte použití méně oddíly před uskutečněním zvětšit počet řádků na oddíl.  Taky v tématu [indexování][Index] zahrnující dotazy, které lze použít v SQL datový sklad k posouzení kvalitu obrázku columnstore indexy.

## <a name="syntax-difference-from-sql-server"></a>Syntaxe rozdíl ze serveru SQL Server

SQL datový sklad uvádí zjednodušené definice oddíly, což je mírně odlišnou ze serveru SQL Server.  Rozdělení funkcí a schémata nepoužívaná ve datový sklad SQL, jsou na serveru SQL Server.  Místo toho, které musíte udělat stačí popsána sloupec rozdělení a ukazatel okraj.  Syntaxe rozdělení může být trochu jiný ze serveru SQL Server, jsou základní koncepty stejné.  SQL Server nebo SQL datový sklad podporují jeden oddíl sloupce pro každou tabulku, která může být pohyboval oddíl.  Další informace o vytváření oddílů najdete v tématu [oddíly tabulky a indexy][].

Příkladu příkazu SQL datový sklad oddíly [vytvořit tabulku][] , oddíly tabulce FactInternetSales ve sloupci OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrace rozdělení ze serveru SQL Server

Migrace definice oddíl SQL Server SQL datový sklad jednoduše:

- Odstranění SQL serveru [schéma oddílu][].
- [Funkce partition][] definice dodejte vytvořit tabulku.

Pokud ze kterého migrujete rozdělený tabulky instanci systému SQL Server pod SQL můžete k dotazování počet řádků, které mají každý oddíl.  Mějte na paměti, že pokud stejné rozlišení dělení se použije ve SQL datový sklad, kolik řádků na oddíl sníží koeficientem 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Správa zátěží na projektu

Jeden pozornost výsledné faktor oddílu rozhodnutí tabulky je [Správa zátěží na projektu][].  Správa zátěží na projektu v SQL datový sklad je primárně správy paměti a souběžné.  Maximální paměť přidělit každé rozdělení při spuštění dotazu v SQL datový sklad je třídy upraveny zdroje.  Oddíly v ideálním případě budou velké s ohledem na jiných faktorů jako potřeb paměti vytváření indexů skupinový columnstore.  Skupinový columnstore indexy výhoda výrazně při rozdělování víc paměti.  Budete proto, ujistěte se, že není opětovného vytvoření indexu oddíl omezeny paměti. Zvětšení velikosti paměti dostupné do dotazu lze dosáhnout přechod z výchozí role smallrc, na jednu z ostatních rolí například largerc.

Informace o přidělování paměti za rozdělení je k dispozici pomocí dotazu Správa dynamických zobrazení guvernér zdrojů. Ve skutečnosti udělit paměti bude menší než následující hodnoty. Však to poskytuje úroveň pokyny, které se dají používat při pro změnu velikosti oddílů pro správu operace s daty.  Zkuste Chcete-li předejít pro změnu velikosti oddílů za udělení paměti poskytovanou třídy největší prostředků. Pokud oddílů nárůst na tomto obrázku spustíte rizika tlaku paměti, která zase vede k méně optimální komprese.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Přepínání oddíl

SQL datový sklad podporuje oddíl rozdělení, sloučení a přechodu na jiný. Každá z těchto funkcí je excuted pomocí příkaz [ALTER TABLE][] .

Přepnutí oddíly mezi dvěma tabulkami musíte se ujistit, že oddíly zarovnání na jejich odpovídajících hranice a zda definice tabulky shodují. Při zaškrtnutí omezení ostatní uživatelé vás k jejímu vynucení rozsah hodnot v tabulce zdrojové tabulky musí obsahovat hranice oddíl jako cílovou tabulku. Pokud to není případ, přepínačem oddíl se nepovede metadata oddíl nebude synchronizované.

### <a name="how-to-split-a-partition-that-contains-data"></a>Jak rozdělit do oddílu, který obsahuje data

Co nejefektivněji metody můžete rozdělit oddíl obsahující data, je použít `CTAS` údajů. Pokud je rozdělený tabulka skupinový columnstore pak oddílu tabulky musí být prázdné před můžete rozdělit.

Tady je rozdělený columnstore ukázkovou tabulku obsahující jeden řádek v každý oddíl:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Vytvořením objekt statistický můžeme zajistit této tabulky metadat přesnější. Jsme-li vytvořit statistických údajů, budou používat SQL datový sklad výchozí hodnoty. Podrobné informace o statistiky Zrevidujte [statistiky][].

Jsme dotazu pro používání počet řádků `sys.partitions` katalogu zobrazení:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Pokud pokusíme rozdělení v této tabulce, bude jsme dojde k chybě:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Zpráva 35346, 15 úroveň stavu 1, řádek 44 ROZDĚLIT klauzule změnit oddíl prohlášení o zásadách ochrany nezdařila oddílu je prázdná.  Pouze prázdné oddíly můžete rozdělit v, pokud existuje columnstore index v tabulce. Zvažte zakázání index columnstore před vydání příkaz ALTER oddíl a potom opětovné vytvoření indexu columnstore po dokončení změnit oddíl.

Však můžete používáme `CTAS` vytvořit novou tabulku pro uložení data.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Jako zarovnaný hranice oddíl je povolen přepínače. Tím se prázdný oddíl, který může následně rozdělit ponechá zdrojové tabulky.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Vše, co zbývá je zarovnání naše data na nový oddíl hranice pomocí `CTAS` a zaměňte naše data zpět na hlavní tabulku

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Jakmile jste dokončili přesunu dat je vhodné aktualizace statistik cílovou tabulku zajistit že přesně odrážejí nové rozdělení množiny dat do příslušných oddílů:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabulky ovládacího prvku zdroje

Chcete-li předejít definice tabulky z **koroze** v systému správy zdroje je vhodné vzít v úvahu následující postup:

1. Vytvoření tabulky jako rozdělený tabulku, ale neobsahuje žádnou hodnotu oddíl

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`Tabulka jako součást procesu nasazení:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Tento přístup kód v zdrojového zůstane statické a rozdělení hodnot hranice mohou být dynamické; dlouhodobým s sklad určitou dobu.

## <a name="next-steps"></a>Další kroky

Další informace najdete v článcích [Přehled tabulky][Přehled], [Tabulky datové typy][Datové typy], [Distribuce tabulky][distribuovat], [indexování tabulky][Index], [Zachování statistiky tabulek][Statistika] a [Dočasné tabulky][dočasné].  Další informace o doporučených postupech najdete v tématu [SQL Data Warehouse doporučené postupy][].

<!--Image references-->

<!--Article references-->
[Základní informace]: ./sql-data-warehouse-tables-overview.md
[Datové typy]: ./sql-data-warehouse-tables-data-types.md
[Distribuce]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Oddíl]: ./sql-data-warehouse-tables-partition.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Dočasné]: ./sql-data-warehouse-tables-temporary.md
[Správa zátěží na projektu]: ./sql-data-warehouse-develop-concurrency.md
[SQL datový sklad doporučené postupy]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Rozdělený tabulky a indexy]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[VYTVOŘENÍ TABULKY]: https://msdn.microsoft.com/library/mt203953.aspx
[Funkce Partition]: https://msdn.microsoft.com/library/ms187802.aspx
[schéma oddílu]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
