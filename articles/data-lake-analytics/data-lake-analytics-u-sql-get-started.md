<properties
   pageTitle="Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio | Azure"
   description="Zjistěte, jak nainstalovat datové jezera nástroje pro Visual Studiu, jak se dají a skripty test U SQL. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Kurz: Začínáme s jazykem Azure dat jezera analýzy U SQL

U SQL je jazyk, který sjednocuje výhody SQL s výrazové power vlastního kódu zpracuje všechna data v libovolné měřítku. U jazyka SQL možnost scalable distribuovaný dotaz umožňuje efektivní analýze dat v úložišti a přes relační úložišť, jako jsou databáze SQL Azure.  To umožňuje proces Nestrukturovaná data použitím schématu na číst vložit vlastní logiky a jeho UDF a obsahuje rozšiřitelnost povolit graf nebo barev publikum nemůže ovládat způsobu spuštění ve velkém měřítku. Další informace o návrhu poznámky filozofie za U SQL, získáte tento [příspěvek blogu Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Existují některé rozdíly z ANSI SQL nebo T-SQL. Například jeho klíčových slov DESCENDING SELECT musí být velkými písmeny.

To je typ systému a výraz jazyka uvnitř klauzule FROM, kde jsou v jazyce C# predikáty atd.
To znamená datové typy jsou typy C# a datových typů pomocí C# NULL sémantiku a operace porovnání uvnitř predikátem následují C# syntaxe (například == "foo").  Také znamená to, že hodnoty jsou úplné .NET objekty, což umožňuje snadno způsobem použít k ovládání na požadovaném objektu (například "f o o o". Split(' ')).

Další informace najdete v článku Principy [U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Zjistit předpoklady pro

Musíte nejdřív udělat [kurz: vývoj U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

V tomto kurzu spustili úloha jezera analýzy dat s Tento skript U SQL:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Tento skript nemá žádné transformace kroky. Přečte ze zdrojového souboru s názvem **SearchLog.tsv**, schematizes ho a uloží řádků zpět do souboru s názvem **SearchLog první u sql.csv**.

Všimněte si otazník u datového typu pole Doba trvání. To znamená, že pole Doba trvání mohou být null.

Některé koncepty a klíčových slov skript:

- **Proměnné řádků**: každý dotazu výraz, který vytváří řádků můžete přidělovat proměnné. U SQL používá se následující T-SQL proměnné názvů, například **@searchlog** ve skriptu.
    Poznámka: přiřazení bez vynucení spuštění. Pouze názvy výrazu a nabídne vám schopnost vytvoření složitější výrazů.
- **EXTRAHOVÁNÍ** umožňuje definovat schéma na pro čtení. Schéma nastavil název sloupce a C# pár název typ součásti pro jeden sloupec. Použije takzvané **extrakci**, například **Extractors.Tsv()** extrahovat tsv soubory. Můžete vyvinout vlastní výsledkem.
- **Výstup** přenese řádků a jsou ho. Outputters.Csv() výstupní soubor hodnotami oddělenými čárkou do zadaného umístění. Můžete taky vytvořit vlastní Outputters.
- Všimněte si, že jsou dvě cesty relativní cesty. Můžete taky použít absolutní cesty.  Příklad

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Absolutní cesta musí používat pro přístup k souborům v propojené účty úložiště.  Syntaxe pro soubory uložené v propojený klient Azure úložiště je:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontejneru objektů Blob s objekty BLOB veřejné nebo veřejné kontejnery přístupová oprávnění aktuálně nepodporuje.

## <a name="use-scalar-variables"></a>Pomocí skalární proměnných

Skalární proměnné můžete použít i usnadnit údržbu skriptu. Předchozí skript U SQL můžete taky se měly zapisovat takto:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformace řádků

Umožňuje **Výběr** transformaci řádků:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Klauzule WHERE používá [C# logický výraz](https://msdn.microsoft.com/library/6a71f45d.aspx). Výraz jazyce C# můžete použít vlastní výrazy a funkce. Můžete dokonce provádět složitější filtrování podle jejich sloučením s logické spojek (a) nebo disjunctions (ORs).

Tento skript používá metodu DateTime.Parse() a spojky.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Všimněte si, že druhého dotazu pracuje na výsledek první řádků a tedy výsledek sloučení dvou filtrů. Můžete taky znovu použít název proměnné a názvy je omezené lexikálně.

## <a name="aggregate-rowsets"></a>Agregační řádků

U SQL umožňuje známé **ORDER** **Seskupit podle** a agregace.

Následující dotaz najde celkovou dobou trvání jednotlivých oblastech a poté uloží horní 5 doby trvání v pořadí.

U SQL řádků nezachová jejich pořadí pro další dotaz. Tedy objednejte výstup, budete muset přidat Order výstup údajů, jak je ukázáno v následujícím příkladu:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U SQL klauzule ORDER by se musel zkombinovat s vzdálené použití klauzule SELECT výrazu.

Klauzule U SQL máte mohou sloužit k omezit výstup do skupin, které odpovídají podmínce HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Připojení dat

U SQL obsahuje běžné operátory spojení, jako například vnitřní spojení doleva nebo doprava/plné vnější spojení, SKUPINOU připojit se ke nejen tabulky, ale žádné řádků (i ty vytvořené ze souborů).

Tento skript spojí searchlog s protokolem dojem reklamní a dostaneme inzerce řetězce dotazu pro dané datum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL podporuje pouze syntaxe kompatibilní se standardem spojení ANSI: Predikát Rowset1 spojení Rowset2 zapnuto. Stará syntaxe z Rowset1, kde Rowset2 predikát není podporována.
Predikát ve spojení musí být na rovnosti spojení a bez výrazu. Pokud chcete zadat pomocí výrazů, můžete ho přidáte do klauzule select předchozí řádků. Pokud chcete udělat jiného porovnání, můžete ji přesunout do klauzule WHERE.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Vytvoření databáze, funkce vracející tabulku, zobrazení a tabulky

U SQL umožňuje použít data v rámci a schématu databáze. Takže nemusíte vždy číst nebo psát do souborů.

Každé skript U SQL pracuje s výchozí databáze (Předloha) a výchozí schéma (DBO) jako výchozí kontext. Můžete vytvořit vlastní databáze a/nebo schéma. Kontext, můžete změnit **pomocí** příkazu kontext lze změnit.


### <a name="create-a-table-valued-function-tvf"></a>Vytvoření funkce vracející tabulku (TVF)

V předchozí skript U SQL se opakuje EXTRAHOVAT ze stejného zdrojového souboru pro čtení. Funkce vracející tabulku U SQL umožňuje zapouzdřit data pro budoucí použití.   

Tento skript vytvoří TVF s názvem *Searchlog()* ve výchozím nastavení databáze a schématu:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Tento skript ukazuje, jak používat TVF podle předchozí skript:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Vytvoření zobrazení

Pokud máte jenom jednu dotazu výraz, který chcete abstraktní a nechcete, aby ho parametrizovat, můžete vytvořit zobrazení obsahující místo funkce vracející tabulku.

Tento skript vytvoří zobrazení s názvem *SearchlogView* ve výchozím nastavení databáze a schématu:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Tento skript ukazuje, jak pomocí definovaný zobrazení:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Vytvoření tabulky

Podobně jako tabulky relační databáze U SQL umožňuje vytvořte tabulku obsahující předdefinované schématu nebo vytvoření tabulky a odvodit schéma z dotazu, který naplní tabulce (označovaná taky jako vytvořit vybrat jako tabulku nebo CTAS).

Tento skript vytvořit databázi a dvě tabulky:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Tabulky dotazů

Můžete dotazů tabulky (vytvořeného v předchozí skript) stejným způsobem jako dotazu můžete přes datové soubory. Místo abyste vytvářeli řádků pomocí EXTRAHOVAT, teď můžete jenom vytvořit odkaz na název tabulky.

Číst z tabulky se mění transformační skript, které jste dříve použili:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Všimněte si, že se právě nedá spustit SELECT na tabulky ve stejné skriptu jako skript kde vytvořit tuto tabulku.


##<a name="conclusion"></a>Uzavření

Co je uvedené v tomto kurzu se jenom malou část U SQL. Z důvodu rozsah tohoto kurzu nelze nevztahuje všechny položky, jako například:

- Použití křížové platí pro rozbalení částí řetězce, maticích a mapy do řádků.
- Ovládání rozdělený sadami dat (souboru sady a oddíly tabulky).
- Vývoj uživatelsky definované operátory, jako například výsledkem outputters, procesorů, agregátory definované uživatelem v jazyce C#.
- Funkce práce s okny U-SQL.
- Správa kód U SQL s zobrazení a uložené procedury a funkce vracející tabulku.
- Spustíte libovolný vlastní kód uzlech zpracování.
- Připojení k databázím SQL Azure a federate dotazů napříč nimi a dat U SQL a jezera dat Azure.

## <a name="see-also"></a>Viz taky

- [Přehled analýzy dat jezera Microsoft Azure](data-lake-analytics-overview.md)
- [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Použití funkcí okno U SQL Azure dat jezera analýzy úloh](data-lake-analytics-use-window-functions.md)
- [Sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy pomocí portálu Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Napište nám, co si myslíte

- [Odešlete žádost o funkci](http://aka.ms/adlafeedback)
- [Pokud potřebujete pomoc na fórech](http://aka.ms/adlaforums)
- [Poskytnout zpětnou vazbu U SQL](http://aka.ms/usqldiscuss)
