<properties
    pageTitle="SQL v paměti, Začínáme | Microsoft Azure"
    description="Technologií SQL v paměti výrazně zvýšit výkon transakční a technologie pro analýzu úloh. Zjistěte, jak umožní využít výhod těchto technologií."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Začínáme s v paměti (verze Preview) do SQL databáze

V paměti funkce výrazně zvýšit výkon transakční a technologie pro analýzu úloh v pravém situacích.

Toto téma zvýrazní dvě ukázky, jeden pro OLTP v paměti a druhý pro analýzy v paměti. Každý ukázku je naplněný souhrnnými kroky a kód, které jsou potřeba ke spuštění videa. Můžete buď:

- Kód použít k testování varianty najdete v článku rozdíly ve výsledcích výkonu; nebo
- Přečtěte si kód pochopit scénáře a naučte se vytvářet a používat objektů v paměti.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [1 rychlý Start: V paměti OLTP technologie pro rychlejší T-SQL výkonu](http://msdn.microsoft.com/library/mt694156.aspx) – je taky Tento článek vám pomůžou začít.

#### <a name="in-memory-oltp"></a>OLTP v paměti

Funkce v paměti [OLTP](#install_oltp_manuallink) (online transakce processing) jsou:

- Paměť optimalizována tabulky.
- Nativně kompilovaný uložené procedury.


Paměť optimalizována tabulka obsahuje jeden znázornění přímo v aktivní paměti kromě standardního znázornění na pevném disku. Obchodní transakce v tabulce rychleji, protože přímo interakci s pouze v aktivním paměti znázornění.

S v paměti OLTP dosáhnete až 30 časy získat v transakce výkon, v závislosti na konkrétní pracovní zátěž.


Nativně zkompilované uložené procedury vyžadují méně počítače pokyny za běhu než tradiční interpretovaný uložené procedury. Jsme zobrazila výsledek nativní kompilace v doby trvání, které jsou 1/100 interpretovaný doby trvání.


#### <a name="in-memory-analytics"></a>Analýzy v paměti 

Funkce [analýzy](#install_analytics_manuallink) v paměti je:

Indexy Columnstore vylepšení výkonu technologie pro analýzu a vytváření sestav dotazů. 


#### <a name="real-time-analytics"></a>Data v reálném analýzy

Pro [Analýzy v reálném čase](http://msdn.microsoft.com/library/dn817827.aspx) kombinovat v paměti OLTP a technologie pro analýzu získat:

- Data v reálném organizace podle provozní data.


#### <a name="availability"></a>Dostupnost


GA, všeobecně dostupná:

- [Indexy Columnstore](http://msdn.microsoft.com/library/dn817827.aspx) , které jsou *na disku*.


Náhled:

- OLTP v paměti
- Data v reálném provozním analýzy


Důležité informace o době, kdy je funkce v paměti v náhledu jsou popsané [dál v tomto článku](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Tyto funkce jsou dostupné jenom u databází [*Premium*](sql-database-service-tiers.md) neúčastní pružná fondů a není dostupné pro všechny základní nebo standardní databáze.  Podpora pro Premium databáze v pružná fondů je brzy k dispozici. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>ODPOVĚĎ: Instalace ukázek OLTP v paměti

Několika kliknutími [Azure portál](https://portal.azure.com/)vytvoříte ukázkové databázi AdventureWorksLT [V12]. Potom kroků v této části popisují, jak můžete rozšířit AdventureWorksLT databáze můžete použít:

- Tabulky v paměti.
- Nativně zkompilované uložená procedura.


#### <a name="installation-steps"></a>Postup instalace

1. [Azure portál](https://portal.azure.com/)vytvoření Premium databáze na serveru V12. Nastavení **zdroje** na databázi ukázkové AdventureWorksLT [V12].
 - Podrobné pokyny můžete v tématu [vytvoření první databáze Azure SQL](sql-database-get-started.md).

2. Připojení k databázi s SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Zkopírujte do schránky [skriptů v paměti OLTP Transact-SQL](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) .
 - Skript T-SQL vytvoří objekty potřebné paměti v ukázkové databázi AdventureWorksLT, který jste vytvořili v kroku 1.

4. Vložení skriptu T-SQL do SSMS a potom spusťte skript.
 - Je důležité `MEMORY_OPTIMIZED = ON` příkazy CREATE TABLE klauzule jako v:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Chyba 40536


Pokud se zobrazí chyba 40536 při spuštění skriptu T-SQL, spusťte tento skript T-SQL pro ověření, zda databáze podporuje v paměti:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Výsledek **0** znamená, že není podporováno v paměti a 1 znamená, že je podporována. Chcete-li diagnostikovat potíže:

- Zkontrolujte, zda že databáze byla vytvořená po funkcí v paměti OLTP stala aktivním (verze Preview).
- Zkontrolujte, zda že je databáze na vrstvy služeb Premium.


#### <a name="about-the-created-memory-optimized-items"></a>Informace o vytvořené paměť optimalizována položek

**Tabulky**: vzorek obsahuje v následujících tabulkách optimalizované paměti:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Můžete zkontrolovat paměť optimalizována tabulky pomocí **Průzkumníka objektu** ve SSMS tak, že:

- Klikněte pravým tlačítkem myši **tabulky** > **Filtr** > **Nastavení filtru** > **Optimalizován paměti** roven 1.


Nebo můžete dotazu katalogu zobrazení, například:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Nativně zkompilované uložené procedury**: SalesLT.usp_InsertSalesOrder_inmem můžete zkontrolovat pomocí katalogu zobrazení dotazu:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Spuštění pracovního vytížení OLTP ukázka

Jediný rozdíl mezi následující dvě *uložené procedury* je první postup používá paměť optimalizována verze tabulek, zatímco druhý postup používá pravidelné tabulky na disku:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


V této části najdete v článku jak používat nástroj po ruce **ostress.exe** provést dva uložené procedury stressful na úrovni. Je možné porovnávat, jak dlouho trvá spuštěna dvě namáhání dokončete.


Když spustíte ostress.exe, doporučujeme předat hodnoty parametrů navržený současně:

- Spuštění velkého počtu souběžné připojení pomocí - n100.
- Podle mít každé připojení smyčce stovky časy, použití - r500.


Však můžete chtít začít mnohem nižšími hodnotami jako - n10 a - 50 zajistit vše funguje.


### <a name="script-for-ostressexe"></a>Skript pro ostress.exe


Tato část slouží k zobrazení skript T-SQL, který je vložen v naší ostress.exe příkazového řádku. Skript používá položky, které se vytvářely T-SQL skript, který jste si nainstalovali dříve.


Tento skript vloží ukázkové prodejní objednávky s pět řádkové položky do následujících paměť optimalizována *tabulky*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Chcete-li _ondisk verzi předchozí T-SQL pro ostress.exe, by jednoduše nahradíte obou výskyty podřetězec *_inmem* *_ondisk*. Tyto slouží k nahrazení vliv na názvy tabulek a uložené procedury.


### <a name="install-rml-utilities-and-ostress"></a>Instalace nástroje RML a ostress


V ideálním případě by plánujete ostress.exe na OM Azure. Ve stejné Azure zeměpisné oblasti, kde jsou uložená databázi AdventureWorksLT vytvoříte [Azure virtuálního počítače](https://azure.microsoft.com/documentation/services/virtual-machines/) . Ale můžete na přenosném počítači spustit ostress.exe místo.


V OM nebo na něco jiného, co můžete hostovat zvolte, nainstalujte nástroje znova se přehrávat Markup Language (RML), které zahrnují ostress.exe.

- Přečtěte si diskuzi ostress.exe v [Ukázkové databázi pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).
 - Nebo v [Databázi ukázkové pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).
 - Nebo najdete v [blogu pro instalaci ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>První spuštění pracovní zátěž namáhání _inmem


*RML Cmd výzvy* okno můžete použít ke spuštění naše ostress.exe příkazového řádku. Parametry příkazového řádku nastavíte ostress na:

- Souběžně spustit 100 připojení (-n100).
- Máte u každého připojení následujícím způsobem T-SQL 50 časy (-50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Spuštění předchozího ostress.exe příkazového řádku:


1. Obnovení dat obsah databáze spuštěním následujícího příkazu v SSMS, pokud chcete odstranit všechna data, která byla vložena předchozích spuštění:
```
EXECUTE Demo.usp_DemoReset;
```

2. Zkopírujte text předchozí přepínač příkazového řádku ostress.exe do schránky.

3. Nahrazení `<placeholders>` pro parametry -S - U -P -d se správnou reálných hodnoty.

4. V okně RML Cmd spusťte upravený příkazového řádku.


#### <a name="result-is-a-duration"></a>Výsledek je doba trvání


Po dokončení ostress.exe zapíše spuštění dobu trvání jako jeho poslední řádek výstup v okně RML Cmd. Například kratší spustit test činí asi 1,5 minut:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Obnovení, upravují _ondisk a pak znovu


Až budete mít výsledek _inmem spouštět, proveďte následující kroky pro _ondisk spuštění:


1. Obnovení databáze spuštěním následujícího příkazu v SSMS, pokud chcete odstranit všechna data, vložených předchozí spustit:
```
EXECUTE Demo.usp_DemoReset;
```

2. Úprava příkazového řádku ostress.exe chcete nahradit všechny *_inmem* *_ondisk*.

3. Spusťte ostress.exe podruhé a zachytit výsledku doby trvání.

4. Znovu obnovte databázi pro příslušné odstranění co může být velké množství testovací data.


#### <a name="expected-comparison-results"></a>Porovnání očekávané výsledky

Testů v paměti projevili zlepšení výkonu **9 časy** pro tento pracovní zátěž zneužívající vlastností prohlížeče ostress provozu v Azure OM ve stejné oblasti Azure jako databáze.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Instalace ukázek analýzy v paměti


V této části porovnají výsledky vstupu a výstupu a statistiky při použití columnstore index versus tradiční b – strom index.


Pro data v reálném analýzy na pracovní zátěž OLTP je často nejlepší používat bez clusterů columnstore index. Podrobnosti najdete v [Popisu Columnstore indexy](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Příprava test columnstore analýzy


1. Pomocí portálu Azure můžete vytvářet nové databáze AdventureWorksLT ze vzorku.
 - Pomocí této přesný název.
 - Vyberte libovolnou vrstvy služeb Premium.

2. Zkopírujte [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) do schránky.
 - Skript T-SQL vytvoří objekty potřebné paměti v ukázkové databázi AdventureWorksLT, který jste vytvořili v kroku 1.
 - Skript vytvoří tabulka dimenzí a dvě tabulky faktů. Tabulky faktů jsou vyplněné 3.5 miliony řádků.
 - Skript může trvat 15 minut.

3. Vložení skriptu T-SQL do SSMS a potom spusťte skript.
 - Důležité probíhá pomocí klíčových slov **COLUMNSTORE** na příkaz **CREATE INDEX** jako:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Nastavte AdventureWorksLT na úroveň kompatibility 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Úroveň 130 nesouvisí přímo k funkcím v paměti. Ale úroveň 130 obecně poskytuje dotazu rychleji než 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Důležité tabulky a columnstore indexy


- dbo. FactResellerSalesXL_CCI je tabulku obsahující skupinový **columnstore** index, který obsahuje rozšířené komprese na úrovni *data* .

- dbo. FactResellerSalesXL_PageCompressed je tabulka, která je ekvivalentní běžná skupinový index, který je komprimované jenom na úrovni *stránky* .


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Důležité dotazů a umožňuje porovnávat columnstore indexu


[Tady](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) je několik typů T-SQL dotazu můžete spustit zobrazíte zvýšení výkonu. Krok 2 v skript T-SQL je dvojice dotazy, které jsou předmětem zájmu přímé. Dvou dotazů lišit jenom na jednom řádku:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Skupinový columnstore index je v tabulce**_CCI** FactResellerSalesXL.

Následující úryvek skript T-SQL vytiskne statistiky vstupu a výstupu a času pro dotaz každou tabulku.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Náhled aspektech pro OLTP v paměti


Funkce OLTP v paměti v databázi SQL Azure stal [aktivní pro náhled na 28 říjen 2015](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


V aktuálním náhledu v paměti OLTP podporované jenom pro:

- Databáze, které jsou ve vrstvě služby *Premium* .

- Databáze vytvořené po funkce OLTP v paměti stala aktivním.
 - Nové databáze nepodporuje OLTP v paměti, pokud se obnoví z databáze, která byla vytvořená před funkcí v paměti OLTP stala aktivním.


Pokud jste na pochybách, vždy spuštěním následující T-SQL vyberte zjistit, zda databáze podporuje OLTP v paměti. Výsledkem **1** znamená, že databázi podporuje OLTP v paměti:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Pokud dotaz vrátí hodnotu **1**, v paměti OLTP je podporované v této databázi a kopie databáze a obnovení databáze vytvořené založené na této databázi.


#### <a name="objects-allowed-only-at-premium"></a>Objekty povoleno pouze na Premium


Pokud databáze obsahuje jednu z následujících typů nebo objekty v paměti OLTP, přechodu osy služby základní nebo standardní databázi z Premium nepodporuje. Chcete-li omezit databázi, nejdřív uvolnit tyto objekty:

- Paměť optimalizována tabulek
- Typy paměť optimalizována tabulek
- Nativně zkompilované moduly


#### <a name="other-relationships"></a>Další relace


- Funkce pomocí OLTP v paměti s databázemi v pružná fondů nepodporuje během zobrazení náhledu.
 - Přesunout databázi, která obsahuje nebo byla v paměti OLTP objekty do fondu pružná, postupujte takto:
  - 1. Uvolnění paměť optimalizována tabulek, typy tabulek a nativně zkompilované moduly T-SQL databáze
  - 2. Změnit na standardní formát osy služby databáze
  - 3. Přesunutí databáze do fondu pružná

- Použití v paměti OLTP s SQL datový sklad nepodporuje.
 - Funkce index columnstore analýzy v paměti je podporované v SQL datový sklad.

- Úložiště dotaz není zachytit dotazům vnitřní nativně kompilovaný moduly.

- V paměti OLTP nepodporuje některé funkce jazyce Transact-SQL. To se týká Microsoft SQL Server a databázi SQL Azure. Podrobnosti najdete v tématu:
 - [Podpora Transact-SQL OLTP v paměti](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Nejsou podporovány v paměti OLTP konstrukce Transact-SQL](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Další kroky


- Zkuste [použití OLTP v paměti v existující aplikaci SQL Azure.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Další zdroje informací

#### <a name="deeper-information"></a>Důkladnější informace

- [Další informace o OLTP v paměti, která platí pro aplikace Microsoft SQL Server a databázi SQL Azure](http://msdn.microsoft.com/library/dn133186.aspx)

- [Další informace o data v reálném provozním analýzy na webu MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Dokument White paper na [běžná pracovní zátěž schémata a aspekty migrace](http://msdn.microsoft.com/library/dn673538.aspx), který popisuje pracovní zátěž vzorků kde OLTP v paměti běžně poskytuje výrazné zvýšení výkonu.

#### <a name="application-design"></a>Návrh aplikace

- [Paměť OLTP (Optimalizace v paměti)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Použití OLTP v paměti v existující aplikaci SQL Azure.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Nástroje

- [SQL Server Data nástroje náhled (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), měsíční nejnovější verzi.

- [Popis nástroje znova se přehrávat Markup Language (RML) pro systém SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Sledování v paměti úložiště](sql-database-in-memory-oltp-monitoring.md) OLTP v paměti.

