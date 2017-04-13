<properties
   pageTitle="Distribuce tabulek v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s distribuce tabulek v Azure SQL datový sklad."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Distribuce tabulek v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

SQL datový sklad je že datových souběžné zpracování (MPP) rozdělením databáze systému.  Rozdělením dat a zpracování funkce v několika uzlech, nabízí SQL datový sklad velké škálovatelnost – Pokud za jeden systém.  Rozhodnutí o způsobu k distribuci dat v SQL datový sklad je jednou z nejdůležitějších faktorů k dosažení optimálního výkonu.   Klíč k dosažení optimálního výkonu minimalizace přesun dat a zase klíč pro minimalizaci přesun dat je výběr strategie správné rozdělení.

## <a name="understanding-data-movement"></a>Principy přesun dat

V systému MPP data ze všech tabulek rozdělena napříč několika podkladového databází.  Většina optimalizované dotazy na systému MPP můžete jednoduše projde provést na jednotlivé distribuované databází bez zásahu mezi dalších databází.  Například Řekněme, že máte otevřenu databázi s daty prodeje, která obsahuje dvě tabulky, prodej a Zákazníci.  Pokud máte dotaz, který potřebuje ke spojení tabulky prodejů a tabulky zákazníka a můžete vydělte prodeje a zákazníka tabulky nahoru číslo zákazníka, uvedení jednotlivé zákazníky v samostatném databáze, můžete všechny dotazy, které spojení prodeje a zákazníka vyřešit v jednotlivých databázích bez znalosti dalších databází.  Naopak, pokud při dělení data o prodeji podle pořadí čísla a zákaznická data číslem zákazníka, pak danou databázi nebudou mít odpovídající data pro jednotlivé zákazníky a tedy pokud byste chtěli spojení data o prodeji a zákaznická data, potřebujete přístup k datům pro jednotlivé zákazníky z jiné databáze.  V tomto příkladu druhý by přesun dat potřebují problémy přejdete data o zákaznících k data o prodeji, takže dvě tabulky můžete spojeny.  

Přesun dat není vždy chybná věc, někdy je nezbytné k vyřešení dotazu.  Ale pokud tento krok navíc můžete předejít, přirozeným dotaz bude běžet rychleji.  Přesun dat nejčastěji nastane při tabulky jsou spojeny nebo agregace provádí.  Často potřebujete oba, zatímco budete moci optimalizovat pro jeden scénář, jako je spojení, potřebujete přesun dat k řešení jiných scénáře, třeba agregaci.  Vtip je řešení problémů s tedy snižují pracovní.  Ve většině případů distribuce tabulky faktů velké běžně spojených sloupec je nejvhodnějších ke snížení většina přesun dat.  Distribuce data ve sloupcích spojení je mnohem víc běžným způsobem snížit přesun dat než distribuci dat ve sloupcích zahrnuté do agregaci.

## <a name="select-distribution-method"></a>Vyberte způsob rozdělení.

Na pozadí SQL datový sklad rozdělí data do 60 databází.  Každý jednotlivé databáze se nazývá **rozdělení**.  Po načtení dat do jednotlivých tabulek SQL datový sklad používá se dozvědět, jak rozdělit dat přes tyto 60 distribuce.  

Metoda rozdělení je definována na úrovni tabulky a aktuálně existuje dvě možnosti:

1. **Kruhové** distribuovat data rovnoměrně ale náhodně.
2. **Distribuované hash** který distribuuje data podle algoritmus hash hodnoty z jednoho sloupce

Ve výchozím nastavení při nedefinujete metodu distribuci dat tabulky rozdělena metodou **kruhového** rozdělení.  Však jako se stanete jejím složitější v implementaci budete chtít zvažte použití **hash distributed** tabulky minimalizovat přesun dat, která zase optimalizaci výkonu dotazu.

### <a name="round-robin-tables"></a>Kruhové tabulek

Pomocí metody kruhového distribuce dat je velmi mnohem jak vypadá.  Jak se načte data, každý řádek odeslaný jednoduše další rozdělení.  Tento způsob distribuci dat bude vždy náhodně data velmi rovnoměrné rozmístění ve všech rozdělení.  To znamená je třídění mít během procesu kruhového, které umístí vaše data.  Kruhové rozdělení je někdy se jí říká náhodné hash z tohoto důvodu.  S tabulkou distribuované kruhového je potřeba porozumět data.  Z tohoto důvodu kruhového tabulky často zkontrolujte dobré načítání cílů.

Ve výchozím nastavení pokud je vybrána žádná metoda rozdělení, metodu distribuce kruhového se použijí.  Během kruhového tabulky jsou snadno se použije, protože data náhodně rozvržena systému, který znamená to, že systému nezaručuje, které rozdělení každý řádek je však na.  V důsledku toho systému někdy potřeba spustit operaci přesunutí dat do lépe uspořádat data před může vyřešit dotazu.  Tento krok navíc mohou zpomalovat dotazů.

Zvažte použití kruhové rozdělení pro vaši tabulku v následujících situacích:

- Když jako výchozí bod jednoduché Začínáme
- Pokud není zřejmé spojovací klíč
- Pokud není k dispozici pole mohlo sloužit sloupec pro hash distribuce v tabulce
- Pokud v tabulce Nesdílet společný klíč spojení s jinou tabulkou
- Pokud spojení je důležité menší než ostatní spojení v dotazu
- Pokud je tabulka dočasné pracovní tabulky

Oba následující příklady vytvoří tabulku s každým:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Když je kruhového výchozí typ tabulky je definován v vaší DDL je považován za doporučený postup, tak, aby úmyslech rozložení tabulky vymazat ostatním.

### <a name="hash-distributed-tables"></a>Hash distribuované tabulek

Použití algoritmus **Hash distributed** k distribuci tabulkách dosáhnout zvýšení výkonu v mnoha scénářích jejich zmenšením přesun dat v době dotazu.  Hash distributed tabulky jsou tabulky, které jsou rozdělit mezi distribuované databáze využívající algoritmus hash na jeden sloupec, který jste vybrali.  Rozdělení sloupce je co určuje, jakým způsobem se data rozdělí mezi distribuované databází.  Funkce hash obsahuje sloupec rozdělení řádků k rozdělení.  Algoritmus hash a výsledné rozdělení je deterministický.  Je to stejné hodnoty stejný datový typ bude vždy má stejné rozdělení.    

Tento příklad vytvoří tabulku distribuované na id:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Vyberte sloupec rozdělení.

Když budete chtít **hash distribuce** tabulky, musíte vybrat jeden rozdělení sloupce.  Při výběru rozdělení sloupce, existují tři hlavní faktorem.  

Vyberte jeden sloupec, který bude:

1. Nelze aktualizovat
2. Stejně široké dat, že data skew
3. Minimalizace přesun dat

### <a name="select-distribution-column-which-will-not-be-updated"></a>Vyberte distribuční sloupec, podle kterého se nebudou aktualizovat

Rozdělení sloupce není aktualizovatelné, proto, vyberte sloupec s statických hodnot.  Pokud sloupec potřebovali aktualizovat, není obecně jako kandidát dobré rozdělení.  Pokud tam je případ, kde je nutné aktualizovat rozdělení sloupce, stačí tak, že nejdřív odstranění řádku a potom vložení nového řádku.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Vyberte rozdělení sloupce, který bude rovnoměrné rozmístění dat

Protože distribuovaného systému provádí pouze stejně rychle jako nejnižší rozdělení, je důležité rozdělte práci rovnoměrně přes rozdělení pro dosažení rovnováha spuštění systému.  Způsob práce je rozdělen distribuované systému vychází z kde jsou umístěná data pro každé rozdělení.  Díky tomu velmi důležité vyberte sloupec vpravo rozdělení pro distribuování data tak, aby každý distribuční má stejnou práci a bude trvat současně dokončete jeho část práce.  Při práci se dobře dělení celém systému, jsou data rovnoměrně mezi rozdělení.  Když dat není rovnováha rovnoměrně, název tato **data zkosení**.  

Rovnoměrné rozdělení data a vyhněte se data skew, je potřeba zvážit následující při výběru rozdělení sloupce:

1. Vyberte sloupec, který obsahuje významné počet různých hodnot.
2. Vyhněte se distribuci dat ve sloupcích s několika různých hodnot. 
3. Vyhněte se distribuci dat ve sloupcích s vysokou četnost Null.
4. Vyhněte se distribuci dat ve sloupcích datum.

Vzhledem k tomu, že každá hodnota hešováno 1 60 rozdělení, dosáhnout rovnoměrné rozdělení budete chtít vyberte sloupec, který je velmi jedinečný a obsahuje více než 60 jedinečné hodnoty.  Ke znázornění, zvažte případu sloupce pouze mají 40 jedinečné hodnoty.  Pokud v tomto sloupci byla vybrána jako distribuční klíč, data pro tuto tabulku by má na 40 distribuce maximálně, nechte 20 rozdělení s žádná data a žádné zpracování dělat.  Naopak ostatní 40 rozdělení má více práce, pokud jsou data byla rovnoměrně rozloženy více než 60 rozdělení.  Tento scénář je příklad dat zkosení.

V systému MPP čeká každý krok dotazu všechny distribuce dokončete jejich sdílení práce.  Pokud jeden rozdělení je víc práce než ostatní uživatelé, pak zdroje distribuce jsou v podstatě požadovaných plně využívána jenom čeká na něčem pracuje rozdělení.  Při práci není rovnoměrné rozdělení přes všechny rozdělení, název tohoto **zpracování zkosení**.  Zpracování zkosení způsobí dotazů poběží pomaleji než pokud pracovní zátěž může být rovnoměrně rozšířit mezi rozdělení.  Data skew vede k zpracování zkosení.

Vyhněte se distribuce vysoce s možnou hodnotou Null sloupec podle hodnoty null budou má stejné rozložení. Distribuce na sloupec kalendářních dat můžete taky způsobit zpracování zkosení, protože všechna data pro dané datum se má stejné rozložení. Pokud několik uživatelů provádí dotazy všechny filtry ke dni, pak pouze 1 60 distribuce bude by veškerou práci od dané datum bude pouze na jednu distribuční. V tomto scénáři dotazy spustí pravděpodobně 60 časy nižší než-li data byly rovnoměrně z vnější rozděleno všechny rozdělení. 

Pokud neexistují žádné sloupce, aby pole mohlo sloužit, můžete použít kruhového jako prostředek rozdělení.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Vyberte rozdělení sloupce, který bude minimalizovat přesun dat

Minimalizace přesun dat tak, že vyberete sloupci vpravo rozdělení je jednou z nejdůležitějších strategie pro optimalizaci výkonu datový sklad SQL.  Přesun dat nejčastěji nastane při tabulky jsou spojeny nebo agregace provádí.  Sloupce užívané v `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` a `HAVING` klauzule všechny usnadňují práci s **dobré** hash rozdělení kandidátů. 

Na druhé straně sloupce v `WHERE` klauzule **teprve** pro dobré hash sloupce kandidáty příčinou zpracování zkosení protože omezují které distribuce účast v dotazu.  Sloupec kalendářních dat je dobré příklad sloupce, který může být tempting rozdělit na, ale často může způsobit, že tento zpracování zkosení.

Obecně Pokud máte dvě tabulky velké faktů často zahrnuté v spojení, získáte většina výkonu pomocí distribuce obou tabulek v jednom sloupci spojení.  Pokud máte tabulku, která nikdy připojen k jiné velké faktů tabulky, podívejte se na sloupce, které se často ve `GROUP BY` klauzule.

Existuje několik klíčových kritéria, která musí být splněná zabráníte přesouvání dat během spojení:

1. Tabulky zahrnuté v spojení musí být hash distribuované na **jednu** ze sloupců účast ve spojení.
2. Datové typy sloupců spojení se musí shodovat mezi oběma tabulkami.
3. Operátor rovná se musí být členem sloupce.
4. Typ spojení nemusí být `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Poradce při potížích s dat skew

Když data v tabulce je distribuovaná pomocí metody rozdělení hash je pravděpodobnost, že některé distribuce bude zkosená mít nepřiměřeně další data, než ostatní. Skew nadbytečné dat může ovlivnit výkon dotazu, protože konečný výsledek distribuované dotazu musí čekat nejdelší pracovního rozdělení dokončete. V závislosti na stupeň zkosení dat může musíte jej vyřešit.

### <a name="identifying-skew"></a>Identifikace zkosení

Jednoduchý způsob, jak identifikovat tabulky jako zkosená, je použít `DBCC PDW_SHOWSPACEUSED`.  Toto je velmi rychlý a jednoduchý způsob, jak zjistit počet řádků tabulky, které jsou uložené ve všech 60 rozdělení databáze.  Myslete na to, že většina rovnováha výkon řádků v tabulce distribuované by měl rovnoměrné rozdělení přes všechny rozdělení.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ale pokud dotaz Správa dynamických zobrazení Azure SQL datový sklad (DMV) můžete provádět podrobnější analýza.  Zahájíte vytvořte zobrazení [dbo.vTableSizes][] pomocí SQL v článku[Přehled] [Přehled tabulky].  Po vytvoření zobrazení spuštěním tohoto dotazu zjistit, tabulky, které mají víc než 10 % dat zkosení.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Řešení dat skew

Ne všechny skew se vyžadují oprava.  V některých případech předčit výkon tabulky v některé dotazy poškození dat zkosení.  Rozhodnout, pokud je třeba vyřešit dat skew v tabulce, je třeba porozumět míře objemů dat a dotazů ve vaší pracovní zátěž.   Jedním ze způsobů visiové dopad zkosení je Pokud chcete sledovat dopad zkosení na výkonu dotazu a konkrétně vliv na dobu dotazů trvat dokončení na jednotlivé rozdělení postupujte podle pokynů v článku [Sledování dotazu][] .

Distribuce dat je předmětem hledání správné poměr minimalizace dat zkosení a minimalizace přesun dat. Tyto můžete protichůdné cíle a někdy budete chtít uchovávat data skew ke snížení přesun dat. Například po rozdělení sloupce se často sdílený sloupec ve spojení a agregace se bude mít minimalizace přesun dat. Výhodou přesun minimální dat může převážit dopad mít data zkosení. 

Typické způsob, jak vyřešit zkosení dat je znovu vytvořit tabulku se sloupcem jiného distribučního. Protože nejde žádným způsobem změnit rozdělení sloupce na existující tabulky, je možné změnit hodnotu rozdělení tabulky ho začínat s [[CTAS]].  Zde jsou dva příklady vyřešení problému s daty skew:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Příklad 1: Znovu vytvořte tabulku se sloupcem nové rozdělení.

Tento příklad používá [[CTAS]] a znovu vytvořit tabulku se sloupcem různých hash rozdělení. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Příklad 2: Znovu vytvořte tabulku pomocí kruhové rozdělení

Tento příklad používá [[CTAS]] a znovu vytvořit tabulku s kruhového místo hash rozdělení. Tato změna vytvoří rovnoměrné rozdělení data druhou stranu pohybu lepší data. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Další kroky

Další informace o návrhu tabulky, najdete v článku [distribuovat][], [Index][], [oddílu][], [Datové typy][], [Statistika][] a[dočasné] články [Dočasné tabulky].

Přehled doporučené postupy naleznete v tématu [SQL datový sklad doporučené postupy][].


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
[Sledování dotazu]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
