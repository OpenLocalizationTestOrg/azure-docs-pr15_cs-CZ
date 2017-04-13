<properties
   pageTitle="Optimalizace transakce pro SQL datový sklad | Microsoft Azure"
   description="Doporučené postupy pro moduly na psát efektivní transakce aktualizace v Azure SQL datový sklad"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimalizace transakce pro datový sklad SQL

Tento článek vysvětluje, jak optimalizovat výkon transakční kódu při minimalizace rizika dlouhé vrácení zpět.

## <a name="transactions-and-logging"></a>Protokolování a transakce

Budou transakce důležitou součástí stroje relační databáze. SQL datový sklad používá transakce během úpravy dat. Tyto transakce může být explicitní nebo implicitní. Jeden `INSERT`, `UPDATE` a `DELETE` příkazy jsou všechny příklady implicitní transakce. Explicitní transakce píší explicitně vývojář pomocí `BEGIN TRAN`, `COMMIT TRAN` nebo `ROLLBACK TRAN` a se obvykle používají, když potřebujete vázat společně v jednom atomová celku několik výrokových změny. 

Azure SQL datový sklad potvrdí změny databázi pomocí transakce protokoly. Každý distribuční má vlastní transakční protokol. Transakce protokolu zápisy probíhají automaticky. Není nutná žádná konfigurace. Zatímco zaručuje tento proces zápisu ho v systému Představte režijních. Minimalizace tento dopad pomocí využití transakce efektivně kódu jazyka. Využití transakce efektivně kód obecně spadá do dvou kategorií.

- Pokud je to možné využít konstrukce minimální úroveň protokolování
- Proces dat pomocí rozsah listů, abyste se vyhnuli jednotném dlouhodobé transakce
- Přijmout oddíl přepínání vzor pro velké změny v daném oddílu

## <a name="minimal-vs-full-logging"></a>Minimální porovnání úplné protokolování

Na rozdíl od plně protokolované operace, které používají protokol transakce Pokud chcete mít přehled o každé změně řádku, udržení přehledu o minimálně protokolované operace rozsahu přidělení a pouze změny metadata. Proto minimální úroveň protokolování zahrnuje protokolování pouze informace, které se členství požaduje pro vrácení transakce v případě selhání nebo žádost o konverzaci explicitní (`ROLLBACK TRAN`). Jak méně informací sledované transakční protokol, minimálně protokolované operací, jako například půjde vám ještě snadněji podobné velikosti plně protokolované operace. Kromě toho vzhledem k tomu méně zápisy přejít transakční protokol, zpráva mnohem menší množství dat protokolu a je vlastně vstupu a další výstupu efektivně.

Limity zabezpečení transakce platí pouze pro plně protokolované operace.

>[AZURE.NOTE] Minimálně protokolované operace může účastnit explicitní transakce. Jak všechny změny v přidělení struktury sledují, je možné vrátit zpět minimálně protokolované operace. Je důležité pochopit "minimálně" zaznamenané změnu není zrušit zaznamenané.

## <a name="minimally-logged-operations"></a>Minimálně protokolované operace

Tyto operace mohou minimálně protokolování:

- Vytvoření tabulky jako vyberte ([CTAS][])
- VLOŽENÍ. VYBERTE
- VYTVOŘENÍ INDEXU
- PŘÍKAZ ALTER OPĚTOVNÉHO VYTVOŘENÍ INDEXU
- UVOLNĚNÍ INDEXU
- VYMAZAT DATA TABULKY
- ODSTRANIT TABULKU
- PŘÍKAZ ALTER TABULKY PŘEPNOUT ODDÍL

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Operace pohyb interních dat. (například `BROADCAST` a `SHUFFLE`) neovlivní limit bezpečnosti transakce.

## <a name="minimal-logging-with-bulk-load"></a>Minimální úroveň protokolování pomocí hromadného zatížení

`CTAS`a `INSERT...SELECT` obě hromadně operace načíst. Však jsou ovlivněny definice tabulky cílové i závisí na scénář načíst. Tady je tabulka, která vysvětluje, pokud hromadné operace se plně nebo minimálně Zaprotokolují:  

| Primární Index               | Scénář zatížení                                            | Režim protokolování |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Haldy                        | Všechny                                                      | **Minimální**  |
| Seskupený Index             | Prázdný cílovou tabulku                                       | **Minimální**  |
| Seskupený Index             | Načtení řádků nepřekrývá s stávající stránky v cílové | **Minimální**  |
| Seskupený Index             | Načtení řádků překrývat s stávající stránky v cílové        | Úplné         |
| Seskupený Columnstore Index | Dávkové velikost > = 102,400 za oddíl zarovnaný rozdělení | **Minimální**  |
| Seskupený Columnstore Index | Dávkové velikost < 102,400 za oddíl zarovnaný rozdělení  | Úplné         |

Je vhodné poznamenat všech zápisu aktualizovat sekundární nebo neseskupené indexy vždycky plně protokolované operace.

> [AZURE.IMPORTANT] SQL datový sklad má 60 rozdělení. Proto za předpokladu, že všechny řádky jsou rovnoměrně a úvodní v jednom oddílu, dávku, musí obsahovat 6,144,000 řádky nebo větší minimálně Zaprotokolují při psaní Columnstore indexovat skupinový. Pokud oddíly v tabulce a oddílu omezení rozsahu řádků vkládaný, bude nutné 6,144,000 řádky za hranice oddílu za předpokladu, že rovnoměrné rozdělení data. Každý oddíl v každé rozdělení nesmí být nezávisle na sobě větší než mezní hodnota činí 102 400 řádek pro vložení minimálně Zaprotokolují do rozdělení.

Načtení dat do tabulky neprázdné s seskupený index často může obsahovat kombinaci plně protokolované a minimálně přihlášeným řádky. Seskupený index je rovnováha strom (b – strom) stránek. Pokud již obsahuje řádky z jiného transakce zápis stránky, potom tyto zápisy se plně Zaprotokolují. Však pokud je prázdná stránka potom zápisu k této stránce se minimálně Zaprotokolují.

## <a name="optimizing-deletes"></a>Optimalizace odstraní

`DELETE`je plně protokolované operace.  Pokud potřebujete odstranit velké množství dat v tabulce nebo oddílu, často je lepší `SELECT` dat chcete zachovat, která poběží jako operace minimálně protokolu.  K tomu vytvoření nové tabulky s [CTAS][].  Po vytvoření můžete [Přejmenovat][] zaměnit staré tabulky s nově vytvořené tabulky.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimalizace aktualizace

`UPDATE`je plně protokolované operace.  Pokud byste potřebovali aktualizovat velkého počtu řádků v tabulce nebo oddílu často možná mnohem efektivnější minimálně protokolované operaci například [CTAS][] lze použít k tomu nevyzve.

V dalším příkladu vrací celou tabulku aktualizace byla převedena na `CTAS` tak, že je možné minimální úroveň protokolování.

V tomto případě jsme zpětně přidáváte částku slevy k prodeji v tabulce:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Opětovné vytvoření velkým tabulkám výhodách jejího používání funkce správy SQL datový sklad zátěží na projektu. Další informace získáte v části Správa zátěží na projektu v článku [souběžné][] .

## <a name="optimizing-with-partition-switching"></a>Optimalizace s přechodu na jiný oddíl

Při velkém měřítku změny uvnitř [tabulky oddíl][], oddíl přepínání vzorku je hodně smysl. Pokud změnu dat je důležité a přesahuje více oddílů, klikněte jednoduše iterace oddíly dosahuje stejný výsledek.

K provedení přepínače oddíl se následujícím způsobem:
1. Vytvoření prázdné se oddíl
2. Zastupování CTAS tato aktualizace
3. Přepněte se existujících dat do mimo tabulku
4. Přepnutí do nových dat
5. Vyčistit data

Však k identifikaci oddíly přejdete bude nejdřív potřebujeme sestavit proceduru helper třeba takové, jaké dole. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Tento postup maximalizuje kód opakované použití a zachová oddílu přechodu na jiný příklad kompaktnější.

Následující kód ukazuje pět kroků uvedených výše dosáhnout úplné oddíl přepínání rutina.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Minimalizace přihlásit pomocí malých dávkách

Pro velké data poslední změny operace může být smysl rozdělení operace do bloků nebo listy oboru jednotky práce.

Konkrétní příklad je uveden níže. Velikost dávky byl nastaven důvodu číslo ke zvýraznění techniku. Ve skutečnosti by být velikost dávky podstatně vyšší. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Pozastavit a pokyny pro změnu velikosti

Azure SQL datový sklad vám umožní pozastavení, obnovení a měřítko datový sklad jako služba. Když pozastavit nebo měřítko SQL datový sklad je důležité pochopit okamžitě; zrušeno letu transakce Příčinou jakýchkoli otevřených transakcí vrátit zpět. Pokud váš pracovní zátěž vystavila dlouho spuštěný a neúplné úpravy dat před operace pozastavit nebo měřítko, to fungovalo bude muset je vrátit zpět. To má vliv dobu potřebnou k pozastavit nebo změně velikosti databáze SQL Azure datový sklad. 

> [AZURE.IMPORTANT] Obě `UPDATE` a `DELETE` jsou plně protokolované operace a tak, aby tyto zpět/znovu operací můžete trvá výrazně ekvivalent přihlášení minimálně operace. 

Scénář Nejlepší je, aby v transakce úpravy letu data dokončení před pozastavení nebo změna velikosti SQL datový sklad. Však to nemusí být vždy praktické. Chcete-li zmírnění rizik dlouhé vrácení zpět, zvažte jednu z následujících možností:

- Znovu psát pomocí [CTAS][] dlouhodobé operace.
- Operace rozdělí na bloky; pracují na podmnožinu řádky

## <a name="next-steps"></a>Další kroky

V tématu [transakce v SQL datový sklad][] zobrazíte další informace o úrovních izolace a transakční omezení.  Přehled dalších doporučené postupy naleznete v tématu [SQL Data Warehouse doporučené postupy][].

<!--Image references-->

<!--Article references-->
[Transakce v SQL datový sklad]: ./sql-data-warehouse-develop-transactions.md
[oddíl tabulky]: ./sql-data-warehouse-tables-partition.md
[Souběžné]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL datový sklad doporučené postupy]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[PŘEJMENOVAT]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

