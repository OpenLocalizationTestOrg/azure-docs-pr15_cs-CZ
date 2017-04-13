<properties
   pageTitle="Vytvoření tabulky vybrání (CTAS) v SQL datový sklad | Microsoft Azure"
   description="Tipy pro kódování s tabulkou vytvoření příkazu Select (CTAS) v Azure SQL datový sklad k vývoji řešení."
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

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Vytvoření tabulky jako vyberte (CTAS) v SQL datový sklad
Vytvoření tabulky jako vyberte nebo `CTAS` neexistuje jednou z nejdůležitějších funkcí T-SQL. Je plně parallelized operaci, která vytvoří novou tabulku na základě výstup příkazu SELECT. `CTAS`je nejjednodušší a nejrychlejší způsob, jak vytvořit kopii tabulky. Můžete mít přeplňované verzi `SELECT..INTO` Pokud libovolný text. Tento dokument obsahuje příklady a osvědčené postupy pro `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Kopírování tabulky pomocí CTAS

Možná jednu nejběžnější využívání `CTAS` vytváří kopii tabulky, ve kterém můžete změnit DDL. Pokud například původně vytvořil tabulky jako `ROUND_ROBIN` a teď chcete změnit na tabulku distributed na nějaký sloupec `CTAS` se, jak by změnit rozdělení sloupce. `CTAS`lze také změnit typ rozdělení indexování a sloupec.

Řekněme, že jste vytvořili v této tabulce pomocí výchozí typ distribuční `ROUND_ROBIN` distributed od v byl zadán žádný sloupec rozdělení `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Teď chcete vytvořit novou kopii v této tabulce s seskupený Columnstore Index, takže můžete využít výkonu skupinový Columnstore tabulkami. Také rozeslat v této tabulce na ProductKey, protože jsou předvídání spojení podle tohoto sloupce a chcete zabránit přesun dat během spojení na ProductKey. Nakonec taky chcete přidat oddílů na OrderDateKey tak, aby stará data můžete rychle odstranit umístěním staré oddíly. Tady je CTAS příkaz, který chcete zkopírovat staré tabulky do nové tabulky.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Nakonec můžete přejmenovat tabulek zaměnit do nové tabulky a pak vložíte staré tabulky.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL datový sklad znamená dosud podpory automaticky vytvořit nebo automatické aktualizace statistiky.  Abyste mohli získávat optimálního výkonu z dotazech, je důležité statistiky být vytvořené ve všech sloupcích všech tabulek po první načtení nebo nastanou jakékoliv podstatné změny data.  Podrobné vysvětlení statistiky naleznete v tématu [statistiky][] ve skupině vývoje témat.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Použití CTAS informace k alternativním řešením nepodporované funkce

`CTAS`lze také informace k alternativním řešením počet vidět nepodporované funkce vypsané dole. Často to může být bude situace vzestupy/win nejen kódu bude požadavkům, ale se často prováděny rychleji na SQL datový sklad. Toto je důsledku plně parallelized návrh. Scénáře, které můžou být kolem spolupracovali CTAS patří:

- VYBERTE. DO
- SPOJENÍ na aktualizace
- Spojení na odstraní
- SLOUČENÍ údajů

> [AZURE.NOTE] Zkuste přemýšlet "CTAS první". Pokud by podle vás může vyřešit problém pomocí `CTAS` , která je obecně nejlepší způsob, jak dosáhnout ho – i když vytváříte víc dat jako výsledek.
>

## <a name="selectinto"></a>VYBERTE. DO
Můžete narazit na `SELECT..INTO` se zobrazí v počet míst v řešení.

Tady je příklad `SELECT..INTO` údajů:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Chcete-li převést výše uvedeného `CTAS` je úplně vážný do dalšího období:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS aktuálně musí že být zadán rozdělení sloupce.  Pokud se pokoušíte úmyslně změna rozdělení sloupce vaší `CTAS` provede nejrychleji, pokud vyberete rozdělení sloupce, který je stejná jako podkladové tabulky jako tato strategie zabrání přesun dat.  Pokud vytváříte malou tabulku kde výkonu není faktoru a pak můžete zadat `ROUND_ROBIN` vyhnout se rozhodnout, ve sloupci rozdělení.

## <a name="ansi-join-replacement-for-update-statements"></a>Nahrazení spojení ANSI příkazy aktualizace

Můžete zjistit, jestli že máte složité aktualizace, který propojuje více než dvě tabulky spolupráce pomocí ANSI připojení syntaxe provádět aktualizace nebo odstranění.

Představte si, že jste měli aktualizovat v této tabulce:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Původní dotazu může mít prohledat přibližně takto:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Od SQL datový sklad nepodporuje ANSI spojení v `FROM` klauzule `UPDATE` údajů, nelze zkopírujte tento kód přes beze změny mírně.

Pomocí kombinací `CTAS` a implicitní spojení tak, aby nahradit tento kód:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>Nahrazení spojení ANSI pro odstranění příkazy
Někdy, je nejlepším řešením pro odstraňování dat použít `CTAS`. Místo odstranění dat jednoduše vyberte data, která chcete zachovat. Tento především pro `DELETE` příkazy, které používají ansi spojování syntaxe od SQL datový sklad nepodporuje ANSI spojení v `FROM` klauzule `DELETE` údajů.

Příklad převedené příkaz Odstranit je k dispozici následující:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Nahrazení příkazy hromadné korespondence
Sloučení příkazy jde nahradit, aspoň v části pomocí `CTAS`. Můžete sloučit `INSERT` a `UPDATE` do jednoho příkazu. Všechny odstraněné záznamy by bylo nutné uzavřít vypnout v podobě příkazu druhé.

Příklad `UPSERT` je k dispozici následující:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>Doporučení CTAS: explicitně uveďte datový typ a Null výstupu

Při migraci kód může stát, že spustíte přes tento typ vzorku kódování:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinktivně domníváte se mají migrovat tento kód do CTAS a by byly správné. Je ale skryté problémem týkajícím se tady.

Následující kód není výnos stejný výsledek:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Všimněte si, že sloupec "výsledek" přenáší vpřed datový typ a Null hodnoty výrazu. Pokud si nejste opatrní může dojít k jemné rozptyly v hodnoty.

Vyzkoušejte následující kroky jako příklad:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Hodnoty uložené pro výsledek se liší. Trvalé hodnotu ve sloupci Výsledek po použití v dalších výrazů bude ještě víc významné chyby do schránky.

![][1]

Toto je velmi důležitým v případě migrací data. I když druhého dotazu je pravděpodobně přesnější došlo k potížím. Data se liší ve srovnání s zdrojového systému a, které vedou k otázek integrity při migraci. Toto je jeden z těchto méně častých případech, kdy "nepovedlo" odpovědí skutečně ten správný!

Důvodem že vidíme tento rozdíl mezi dvěma výsledky je dolů implicitní typ hlasováním. V prvním příkladu definuje tabulku definice sloupce. Po vložení řádku dojde k převodu implicitní typů. Druhý příklad je bez implicitní typu převodu jako výraz definuje datový typ sloupce. Všimněte si také, že sloupec v druhém příkladu definována jako s možnou hodnotou Null sloupec že v prvním příkladu nebyla. Vytvoření tabulky v první sloupec Null příkladu byl explicitně definován. Ve druhém příklad, který je právě nechaný výraz a ve výchozím nastavení to bude mít za následek definici NULL.  

K řešení takových problémů musíte explicitně nastavit převod typu a Null v `SELECT` část `CTAS` údajů. Tyto vlastnosti nemůžete nastavit v části vytvořit tabulku.

Následující příklad ukazuje, jak opravit kód:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Poznámka:
- CAST nebo převést byl použit
- Funkce ISNULL slouží k vynucení Null není v rámci SLUČOVACÍHO
- Funkce ISNULL je vnější funkce
- Druhá část ISNULL je konstanta, tedy 0

> [AZURE.NOTE] Pro Null být správně ji nastavit je důležité používat `ISNULL` a ne `COALESCE`. `COALESCE`není deterministický funkci a tak výsledku výrazu bude vždy s možnou hodnotou Null. `ISNULL`je něco jiného. Je deterministický. Proto při druhá část `ISNULL` jsou konstanty nebo literál pak bude výsledné hodnoty není NULL.

Tento tip není právě užitečné pro zajištění integrity výpočtů. Je také důležité pro přechod oddíl tabulky. Představte si, že máte v této tabulce rozumí vaše skutečnosti:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

V poli hodnota je ale počítané výraz není součástí zdrojová data.

Vytvořit rozdělený datovou sadu můžete chtít udělat toto:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Spustí dotaz by dokonale pořádku. Problém se dodává při pokusu o provedení přepínačem oddíl. Definice tabulky se neshodují. Aby definice tabulky odpovídat CTAS musí změnit.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Zobrazí se tedy typ konzistenci a udržování NULL vlastnosti CTAS je vhodné technickým nejlepší. Umožňuje k zachování integrity do výpočtů a také zajišťuje, že je možné přechodu na jiný oddíl.

Další informace o použití [CTAS][]získáte MSDN. Je jednou z nejdůležitějších příkazy v Azure SQL datový sklad. Zkontrolujte, že důkladně porozumět.

## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
