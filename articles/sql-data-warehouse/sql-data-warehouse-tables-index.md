<properties
   pageTitle="Indexování tabulek v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s tabulkou indexování v Azure SQL datový sklad."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Indexování tabulek v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

SQL datový sklad nabízí několik možností indexování včetně [Skupinový columnstore indexy][] [Skupinový indexy a neseskupené indexy][].  Nabízí také index možnost bez označovaná taky jako [haldy][].  Tento článek popisuje výhody pro každý typ indexu společně s tipy k získání ze svého indexy většina výkon. V tématu [Vytvoření tabulky syntaxe][] podrobnější informace o tom, jak vytvořit tabulku v SQL datový sklad.

## <a name="clustered-columnstore-indexes"></a>Skupinový columnstore indexy

Ve výchozím nastavení vytvoří SQL datový sklad index skupinový columnstore požádání žádné možnosti indexu v tabulce. Skupinový columnstore tabulkami nabízejí nejvyšší stupeň komprese dat jak nejlepší celkový výkon dotazu.  Skupinový columnstore tabulkami obecně překonat skupinový index nebo halda tabulkami a jsou obvykle nejlepší volbou pro rozsáhlých tabulek.  Skupinový columnstore z těchto důvodů je vhodné místo, kde začít Pokud nevíte, jak chcete indexovat tabulky.  

Vytvoření tabulky skupinový columnstore, jednoduše zadat SKUPINOVÝ COLUMNSTORE INDEX v klauzuli WITH nebo vynechat klauzule WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Existuje několik scénáře kde skupinový columnstore nemusí být vhodné možnost:

- Columnstore tabulkami nepodporují sekundární neseskupené indexy.  Místo toho zvažte haldy nebo skupinový rejstřík seznamy.
- Columnstore tabulkami nepodporují varchar(max), nvarchar(max) a varbinary(max).  Místo toho zvažte haldy nebo skupinový index.
- Columnstore tabulkami může být starší efektivní pro přechodná data.  Zvažte haldy a dokonce dočasné tabulky.
- Malé tabulky s menší než 100 miliónů řádků.  Zvažte haldy tabulkami.

## <a name="heap-tables"></a>Haldy tabulkami

Když jsou dočasně úvodní dat v SQL datový sklad, můžete zjistit, že pomocí haldy tabulky, aby celkové proces rychlejší.  Toto je vzhledem k tomu, aby haldách rychlejší než rejstřík seznamy a v některých případech pozdější čtení se teď dá z mezipaměti.  Pokud jsou načítání dat jenom pro fáze před spuštěním další transformace, budou rychleji než načtení dat do tabulky skupinový columnstore načítání do haldy tabulky. Kromě toho načítání dat do [dočasná tabulka][dočasná] taky načte mnohem rychleji než načtení tabulky trvalé úložiště.  

Pro malé vyhledávací tabulky je menší než 100 miliónů řádky, často haldy tabulkami smysl.  Shluk columnstore tabulkami docházet k dosažení optimálního komprese po víc než 100 miliónů řádků.

Pokud chcete vytvořit tabulku haldy, stačí zadejte HALDY v klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Skupinový a bez clusterů indexy

Skupinový indexy může překonat skupinový columnstore tabulkami jedním řádkem potřebujete-li rychle načíst.  Dotazy na místo, kam typu single nebo málo řádek vyhledávání požaduje pro její výkonu s krajní rychlosti zvažte možnost index clusteru nebo bez clusterů sekundární index.  Nevýhody použití seskupený index je, že se jenom dotazy, které používají vysoce výběrové filtr na skupinový indexový sloupec využívat.  Zlepšit filtru u ostatních sloupců indexu založeného na bez clusterů lze přidat do ostatních sloupců.  Ale každý index, který bude přidán do tabulky přidáte místa a zpracovávání času načítání prodloužit.

Pokud chcete vytvořit tabulku seskupený index, stačí zadejte SESKUPENÝ INDEX v klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Přidání neseskupené indexu v tabulce, jednoduše použijte následující syntaxi:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Index neseskupené se vytvoří ve výchozím nastavení při použití CREATE INDEX. Kromě toho index neseskupené pouze povolen v tabulce úložiště řádku (HALDY nebo SKUPINOVÝ INDEX). V současné době není povoleno není skupinový indexy nad SESKUPENÝ COLUMNSTORE INDEX.


## <a name="optimizing-clustered-columnstore-indexes"></a>Optimalizace skupinový columnstore indexy

Skupinový columnstore tabulky jsou uspořádané v datech do segmentů.  S vysokou segmentu kvality, je naprosto zásadní k dosažení výkonu optimální dotazů v tabulce columnstore.  Zlepšení kvality segmentu můžete měřit počtem řádků ve skupině mu tuhle zkomprimovanou řádku.  Pokud jsou aspoň řádky 100 kB na mu tuhle zkomprimovanou řádek seskupení a získat výkonu jako počet řádků na řádku skupiny přístup 1 048 576 řádků, což je většinu řádky, které může obsahovat skupinu řádků schůzce je kvalita segmentu nejčastěji optimální.

Pod zobrazení lze vytvořit a použití systému pro výpočet průměru řádků na řádku seskupení a identifikovat žádné indexy columnstore optimální obrázku.  Poslední sloupec zobrazení vygeneruje jako výraz SQL, které lze použít nové vytvoření indexů.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Teď, když jste vytvořili zobrazení, spusťte tento dotaz, který určí tabulky se skupinami řádku s menší než 100 kB řádky.  Samozřejmě můžete zvětšit mezní 100 kB Pokud hledáte další kvality optimální segment. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Po spuštění dotazu, který můžete začít oblast dat a analýza výsledků. Tato tabulka vysvětluje, co můžete vyhledávat v analýzách skupinu řádků.


| Sloupec                             | Jak používat tyto údaje                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Pokud je oddíly v tabulce, pak může očekáváte najdete v tématu spočítá vyšší skupina otevřít řádků. Každý oddíl v distribuci teoreticky mít skupinu otevřít řádku s ním spojené. Faktor to do analýzy. Malou tabulku, která obsahuje oddíly může být optimalizovaný odebráním rozdělení úplně odebrat jak to by mohl zlepšit komprese.                                                                        |
| [row_count_total]                  | Celkový počet řádků v tabulce. Například můžete tuto hodnotu k výpočtu procent řádků se mu tuhle zkomprimovanou.                                                                      |
| [row_count_per_distribution_MAX]   | Pokud jsou všechny řádky rovnoměrně tuto hodnotu bude cílové počet řádků na rozdělení. Porovnejte tuto hodnotu compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Celkový počet řádků v columnstore formátu tabulky.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Průměrný počet řádků není výrazně menší než maximální počet řádků pro skupinu řádků, můžete použít CTAS nebo změnit znovu vytvořit INDEX zkomprimujete data                     |
| [COMPRESSED_rowgroup_count]        | Počet řádků skupiny ve formátu columnstore. Pokud toto číslo je velmi vysoké vzhledem k tabulce bude indikátor nedostatku hustotu columnstore.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Řádky jsou ve formátu columnstore logicky odstraněny. Pokud je číslo vysoké relativní velikost tabulky, zvažte možnost znovu vytvářeli oddílu nebo opětovné sestavení index, jak je to odebere fyzicky. |
| [COMPRESSED_rowgroup_rows_MIN]     | Slouží ve spojení se sloupci průměr a MAX k pochopení rozsahy hodnot v řádku skupin ve vaší columnstore. Nízké číslo nad mezní zatížení (102,400 za oddíl zarovnaný rozdělení) o tom, že optimalizace jsou k dispozici v načtení dat                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Jako výše                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Otevřít řádku skupiny jsou normální. Jednu prakticky byste čekali jedna skupina otevřít řádku na každou tabulku rozdělení (60). Nadbytečné čísla byste dat načítání mezi oddíly. Kontrola dvojité rozdělení strategii Ujistěte se, zda je zvuku                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Každá skupina řádků můžete mít 1 048 576 řádků v něm maximálně. Použijte tuto hodnotu k najdete v článku jak celé skupině otevřít řádku jsou v současnosti                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Otevřít skupiny, který označuje, že data skapat načítání do tabulky nebo, že předchozí načíst jejichž vypuštění zbývající řádky do této skupiny řádku. Použití MIN a MAX, AVG sloupců tak, aby najdete v článku množství zpracovávaných dat je byla ve otevřít řádku skupiny. Pro malé tabulky může to být 100 % všechna data! V takovém případě změnit INDEX znovu vytvořit k tomu, aby columnstore data.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Jako výše                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Jako výše                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Podívejte se na seskupení řádků skrytých řádků pro kontrolu vhodnosti.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Počet skrytých řádků skupin by měl být nízká, pokud některé se zobrazují vůbec. Skrytých řádků skupiny můžete převést na mu tuhle zkomprimovanou rowg roups pomocí indexu změnit... REORGANIZOVAT příkaz. Však to nepožaduje běžným způsobem. Skryté skupiny se automaticky převedou na skupin řádku columnstore procesem "n-tici přesunování" pozadí.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Skupiny skrytých řádků musí mít sazbu velmi vysoké výplně. Pokud nedostatku rychlosti výplně pro skupinu skrytých řádků, další analýza columnstore není potřeba.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Jako výše                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Jako výše                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | ABYSTE znovu vytvořit index columnstore pro tabulku                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Příčiny špatné columnstore index kvality

Pokud jste určili tabulek s segmentu špatnou kvalitu, budete chtít příčinu kořenové.  Tady jsou některé další běžné příčiny špatné segmentu quaility:

1. Tlaku paměti při vytváření rejstříku
2. Velkého množství DML operace
3. Malé nebo skapat operace zatížení
4. Příliš mnoho oddíly

Následujících skutečností mohou způsobit columnstore index mít výrazně menší než optimální řádky 1 milion skupinu řádků.  Můžete také způsobit řádků, které chcete přejít do skupiny řádku delta místo skupina mu tuhle zkomprimovanou řádků. 

### <a name="memory-pressure-when-index-was-built"></a>Tlaku paměti při vytváření rejstříku

Počet řádků skupinu mu tuhle zkomprimovanou řádku souvisejí přímo šířku řádku a velikosti paměti dostupné zpracuje skupina řádků.  Když řádky jsou napsali columnstore tabulkami stlačení paměti, může dojít k columnstore segmentu kvalitu.  Doporučený postup tedy dát relaci, která zapisuje přístupu tabulky index columnstore tolik paměti největšímu.  Protože poměr paměti a souběžné, pokyny ke přidělování paměti správné závisí na data v jednotlivých řádcích tabulky, množství DWU jste přidělit systému a počtu souběžné sloty, kterou přiřadíte k relaci, která je zápis dat do tabulky.  Osvědčený doporučujeme počáteční s xlargerc Pokud používáte DW300 nebo méně largerc používáte DW400 DW600 a mediumrc Pokud používáte DW1000 hodnoty i všech.

### <a name="high-volume-of-dml-operations"></a>Velkého množství DML operace

Velkého množství DML operacích, které aktualizovat a odstranit řádky může způsobovat neefektivnost do columnstore. To platí zejména při změně většinou řádky ve skupině řádku.

- Odstranění řádku ze skupiny mu tuhle zkomprimovanou řádku pouze logicky označí řádku jako Odstraněná. Řádku zůstane ve skupině mu tuhle zkomprimovanou řádku, dokud znovu oddílu nebo tabulky.
- Vložení řádku přidá řádku, až do interního rowstore tabulky s názvem skupiny řádku delta. Vložený řádek není převedeny na columnstore tak, aby skupina řádků delta je už plná a je označený jako uzavřené. Řádek skupiny uzavřeny po uplynutí maximální kapacita 1 048 576 řádků. 
- Aktualizace řádku ve formátu columnstore zpracován jako logické odstranit a potom vložit. Vložený řádek mohou být uloženy v úložišti delta.

Dávce aktualizace a vložte operacích, které být větší než mezní hodnota hromadné činí 102 400 řádků na oddíl zarovnanými rozdělení odesílán přímo do formátu columnstore. Však za předpokladu, že rovnoměrné rozdělení, musíte být úpravy více než 6.144 miliony řádků v jedné operace pro tuto funkci používat. Pokud počet řádků pro daný oddíl zarovnaný rozdělení je menší než 102,400 řádky budou moct úložišti delta a zůstanou tam, dokud vložené a úpravách zavřete skupina řádků dostatečné řádků nebo index se znovu.

### <a name="small-or-trickle-load-operations"></a>Malé nebo skapat operace zatížení

Malé načte, že toku do SQL datový sklad jsou někdy nazývá skapat zatížení. Obvykle představují blízké konstantní toku dat je požití systém. Při tomto proudu blíží nepřetržitý objemu řádků je však není zvlášť velké. Častěji data jsou výrazně v části potřebných pro přímého načíst do formátu columnstore mezní hodnota.

V těchto situacích je často lepší nejdřív má data v úložišti objektů blob Azure a nechejte ji nahromadit před načítání. Tento postup je často jmenoval *micro dávky*.

### <a name="too-many-partitions"></a>Příliš mnoho oddíly

Další věc vzít v úvahu je dopad oddílů na skupinový columnstore tabulkami.  Před rozdělení, SQL datový sklad už rozdělí data do 60 databází.  Rozdělení další rozdělí data.  Pokud oddíl datům budete chtít vzít v úvahu, že **každý** oddíl musí mít aspoň 1 milion řádků využívat skupinový columnstore index.  Pokud oddíl tabulky do 100 oddílů tabulky musí být alespoň na úrovni 6 miliard řádky těžit z indexu založeného na skupinový columnstore (60 distribuce *100 oddíly* 1 milion řádky). Pokud tabulka 100 oddílu nemá žádné řádky 6 miliard, snižte počet oddílů nebo zvažte místo toho použít haldy tabulky.

Jakmile vloženy tabulek s daty, postupujte pod kroky k identifikaci a opětovné sestavení tabulek s optimální clusteru columnstore indexy.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Opětovné vytvoření indexy zlepšit kvalitu segmentu

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Krok 1: Určete nebo vytvořte uživatele, který používá třídu správné zdroje

Jeden rychle okamžitě zlepšit kvalitu segmentu je znovu vytvořit index.  Tento kód SQL vrácené výše zobrazení vrátí příkazu změnit znovu vytvořit INDEX, které lze použít nové vytvoření indexů.  Pokud opětovné vytvoření indexů, je potřeba přidělit dostatek paměti pro relaci, která bude sestavit index.  K tomuto účelu rozšiřte třídy prostředků uživatele, který má oprávnění k znovu vytvořit index v této tabulce doporučené minimum.  Třídy prostředků uživatele vlastník databáze nelze změnit, pokud jste nevytvořili uživatele systému, musíte nejdřív udělat.  Minimální doporučujeme hodnota je xlargerc Pokud používáte DW300 nebo nižší largerc používáte DW400 DW600 a mediumrc Pokud používáte DW1000 a vyšší.

Tady je příklad toho, jak přiřadit víc paměti uživateli zvýšením jeho třídy zdroje.  Další informace o zdroji třídy a jak vytvořit nového uživatele najdete v tématu [Správa souběžné a pracovní zátěž] [ Concurrency] článek.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Krok 2: Znovu seskupený columnstore indexy s uživatelem třídy vyšší zdroje
Přihlášení jako uživatel v kroku 1 (například LoadUser), která je teď pomocí vyšší třídy zdroje a spusťte příkazy změnit INDEX.  Ujistěte se, že má tento uživatel změnu oprávnění k tabulkám, kde je právě index znovu.  Tyto příklady ukazují, jak nové vytvoření celé columnstore index nebo opětovné sestavení jeden oddíl. Pro velké tabulky je další praktických nové vytvoření indexy jeden oddíl po druhém.

Můžete taky může místo opětovné vytvoření indexu, zkopírujte tabulku do nové tabulky pomocí [CTAS][].  Jaký způsob je nejlepší? Pro velké objemy dat [CTAS][] bývá rychleji než [Změnit INDEX][]. Pro menší objemy dat [Změnit INDEX][] je jednodušší a není třeba zaměnit v tabulce.  Další informace o tom, jak znovu vytvořit indexy s CTAS najdete v článku **Rebuilding indexy s CTAS a přechodu na jiný oddíl** níže.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Opětovné vytvoření indexu v SQL datový sklad je offline operace.  Další informace o opětovném indexy naleznete v části měnit znovu vytvořit INDEX v [Columnstore indexy defragmentace][] a v syntaxi [Změnit INDEX][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Krok 3: Ověření, že vylepšili skupinový columnstore segmentu kvality
Znovu spustit dotaz identifikované tabulky s špatné segmentech kvality a ověřte kvalitu segmentu vylepšili.  Pokud ke zlepšení kvality segmentu, může to být řádků v tabulce jsou extra široké.  Zvažte použití vyšší třídy prostředků nebo DWU po opětovné vytvoření indexů.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Opětovné vytvoření indexy s CTAS a přechodu na jiný oddíl

Tento příklad používá [CTAS][] a oddíl přepínání nové vytvoření oddíl tabulky. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Další informace o opětovné vytvoření oddílů pomocí `CTAS`, najdete v článku [oddíl][] .

## <a name="next-steps"></a>Další kroky

Další informace najdete v článcích [Přehled tabulky][Přehled], [Tabulky datové typy][Datové typy], [Distribuce tabulky][distribuovat], [rozdělení tabulky][oddílu], [Zachování statistiky tabulek][Statistika] a [Dočasné tabulky][dočasné].  Další informace o doporučených postupech najdete v tématu [SQL Data Warehouse doporučené postupy][].

<!--Image references-->

<!--Article references-->
[Základní informace]: ./sql-data-warehouse-tables-overview.md
[Datové typy]: ./sql-data-warehouse-tables-data-types.md
[Distribuce]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Oddíl]: ./sql-data-warehouse-tables-partition.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Dočasné]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL datový sklad doporučené postupy]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[PŘÍKAZ ALTER INDEXU]: https://msdn.microsoft.com/library/ms188388.aspx
[haldy]: https://msdn.microsoft.com/library/hh213609.aspx
[Skupinový indexy a neseskupené indexy]: https://msdn.microsoft.com/library/ms190457.aspx
[Vytvoření tabulky syntaxe]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indexy Defragmentace]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[Skupinový columnstore indexy]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
