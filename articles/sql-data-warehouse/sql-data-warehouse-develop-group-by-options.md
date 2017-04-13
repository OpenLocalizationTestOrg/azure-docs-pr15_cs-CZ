<properties
   pageTitle="Seskupit podle možností v SQL datový sklad | Microsoft Azure"
   description="Tipy pro implementaci skupiny možností v Azure SQL datový sklad k vývoji řešení."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Seskupit podle možnosti v SQL datový sklad

Klauzule [GROUP BY][] se používá k agregace dat do sady souhrnných řádků. Obsahuje také několik možností, které rozšířit jeho funkcí, které potřebujete pracovat kolem, jako jsou přímo nepodporuje datový sklad SQL Azure.

Tyto možnosti jsou
- Seskupit podle s funkcí ROLLUP
- SESKUPENÍ SADY
- Seskupit podle s datové KRYCHLE

## <a name="rollup-and-grouping-sets-options"></a>Nastaví možnosti kumulativní a seskupení
Nejjednodušší možnost, je použít `UNION ALL` místo toho provádět zahrnutí místo může na explicitní syntaxe. Výsledek je přesně totéž

Tady je příklad skupiny pomocí příkazu `ROLLUP` možnost:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

S použitím KUMULATIVNÍ jsme vyžadovány následující agregace:
- Země a oblasti
- Země
- Celkový součet

Nahrazení to je nutné použít `UNION ALL`; určení agregace muset explicitně vrátí stejné výsledky:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Pro seskupení sady všechny potřebujeme dělat je přijmout hlavní stejné ale vytvořit UNION ALL částech úrovně agregace, které chcete zobrazit pouze

## <a name="cube-options"></a>Možnosti krychle
Je možné vytvořit skupiny tak, že se datovou KRYCHLI pomocí UNION ALL přístup. Je to, že kód můžete rychle stát se jejím náročný a nepraktické. Zmírnění to tímto způsobem můžete pokročilejší přístup.

Použijeme výše uvedeném příkladu.

Cílem prvního kroku je k definování "datové krychle", který definuje všechny úrovně agregace, kterou chcete vytvořit. Je důležité si poznamenejte křížové spojení dvou odvozené tabulek. Tím se vytvoří všechny úrovně nám. Zbytek kód je ale skutečně pro formátování.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Výsledky CTAS můžete vidět níže:

![][1]

Druhým krokem je můžete určit cílovou tabulku pro uložení dočasné výsledky:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Třetím krokem je smyčku naše datové krychle provádění agregace sloupců. Dotaz spustí jednou pro každý řádek v tabulce dočasné #Cube a uložení výsledků v tabulce temp #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Nakonec výsledky můžete vrátit tak, že pouze pro čtení z #Results dočasné tabulky

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Rozdělení kód do oddílů a generování opakování konstrukce kód bude spravovat a údržba.


## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[SESKUPIT PODLE]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
