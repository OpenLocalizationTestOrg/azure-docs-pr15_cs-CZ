<properties 
   pageTitle="Pomocí funkce okno U SQL Azure dat jezera Aanlytics úloh | Azure" 
   description="Informace o použití funkce okno U SQL. " 
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


# <a name="using-u-sql-window-functions-for-azure-data-lake-analytics-jobs"></a>Použití funkcí okno U SQL Azure dat jezera analýzy úloh  

Okno funkce byly vydané ISO/ANSI SQL standardní 2003. U SQL přijme podmnožiny okno funkce způsobem definovaným ve standardu ANSI SQL.

Funkce okno slouží k provádění výpočtu v rámci sady řádků s názvem *windows*. Windows jsou definovaných touto klauzulí přes. Funkce okno vyřešit některé scénáře vysoce efektivní způsobem.

Tato příručka výukové používá dvou ukázkové datové sady vás provede některé scénáře výběru kterém je možné použít funkce okna. Další informace najdete v článku Principy [U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

Funkce okno zařazené do: 

- [Vytváření sestav funkce agregace](#reporting-aggregation-functions), například SUM nebo AVG
- [Řazení funkcí](#ranking-functions), jako jsou DENSE_RANK, textový, NTILE a pořadí
- [Analytický funkcí](#analytic-functions), jako je součtového rozdělení, percentily nebo umožňuje přístup k datům z předchozí řádek v stejný výsledek nastaví bez použití vlastní spojení

**Požadavky:**

- Přejděte pomocí následující dvě výukové programy pro:

    - [Začínáme s používáním Azure datové jezera nástroje for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
    - [Začínáme s používáním U SQL Azure dat jezera analýzy úloh](data-lake-analytics-u-sql-get-started.md).
- Vytvořte účet dat jezera analytický podle pokynů uvedených v [začít používat nástroje jezera dat Azure for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Vytvoření projektu Visual Studio U-SQL podle pokynů uvedených v [začít používat U SQL Azure dat jezera analýzy úloh](data-lake-analytics-u-sql-get-started.md).

## <a name="sample-datasets"></a>Ukázkové datové sady

Tento kurz používá dvě datové sady:

- QueryLog 

    QueryLog představuje seznam co lidé vyhledáván ve vyhledávacím webu. Každý protokol dotazu zahrnuje:
    
        - Query - What the user was searching for.
        - Latency - How fast the query came back to the user in milliseconds.
        - Vertical - What kind of content the user was interested in (Web links, Images, Videos).
    
    Zkopírujte a vložte následující skriptu do projektu U jazyka SQL pro vytváření QueryLog řádků:
    
        @querylog = 
            SELECT * FROM ( VALUES
                ("Banana"  , 300, "Image" ),
                ("Cherry"  , 300, "Image" ),
                ("Durian"  , 500, "Image" ),
                ("Apple"   , 100, "Web"   ),
                ("Fig"     , 200, "Web"   ),
                ("Papaya"  , 200, "Web"   ),
                ("Avocado" , 300, "Web"   ),
                ("Cherry"  , 400, "Web"   ),
                ("Durian"  , 500, "Web"   ) )
            AS T(Query,Latency,Vertical);
    
    Ve skutečnosti data nejspíš uložené v datovém souboru. Byste měli přístup k datům uvnitř oddělený tabulátory soubor pomocí následující kód: 
    
        @querylog = 
        EXTRACT 
            Query    string, 
            Latency  int, 
            Vertical string
        FROM "/Samples/QueryLog.tsv"
        USING Extractors.Tsv();

- Zaměstnanců

    Datovou sadu zaměstnance obsahuje následující pole:
   
        - EmpID - Employee ID.
        - EmpName  Employee name.
        - DeptName - Department name. 
        - DeptID - Deparment ID.
        - Salary - Employee salary.

    Zkopírujte a vložte následující skript do projektu U jazyka SQL pro construcint řádků zaměstnanci:

        @employees = 
            SELECT * FROM ( VALUES
                (1, "Noah",   "Engineering", 100, 10000),
                (2, "Sophia", "Engineering", 100, 20000),
                (3, "Liam",   "Engineering", 100, 30000),
                (4, "Emma",   "HR",          200, 10000),
                (5, "Jacob",  "HR",          200, 10000),
                (6, "Olivia", "HR",          200, 10000),
                (7, "Mason",  "Executive",   300, 50000),
                (8, "Ava",    "Marketing",   400, 15000),
                (9, "Ethan",  "Marketing",   400, 10000) )
            AS T(EmpID, EmpName, DeptName, DeptID, Salary);
    
    Následující příkaz ukazuje vytváření řádků pomocí extrahování z datového souboru.
    
        @employees = 
        EXTRACT 
            EmpID    int, 
            EmpName  string, 
            DeptName string, 
            DeptID   int, 
            Salary   int
        FROM "/Samples/Employees.tsv"
        USING Extractors.Tsv();

Při testování vzorky v kurzu musí obsahovat definice řádků. U SQL musíte definovat pouze sady řádků, které se používají. Některé příklady, stačí jenom jeden řádků.

Taky musíte přidat následující příkaz výstup výsledek řádků do datového souboru:

    OUTPUT @result TO "/wfresult.csv" 
        USING Outputters.Csv();
 
 Většina vzorků používá proměnná s názvem **@result** výsledků.

## <a name="compare-window-functions-to-grouping"></a>Porovnání funkcí okno seskupení

Práce s okny a seskupování entity souvisejí tak, že i jiné. Je mohli bychom si vysvětlit této relace.

### <a name="use-aggregation-and-grouping"></a>Použití agregace a seskupování

Následující dotaz používá pro výpočet celkové platu pro všechny zaměstnance agregace:

    @result = 
        SELECT 
            SUM(Salary) AS TotalSalary
        FROM @employees;
    
>[AZURE.NOTE] Pokyny pro testování a vrácení výstup najdete v tématu [Začínáme s používáním U SQL Azure dat jezera analýzy úloh](data-lake-analytics-u-sql-get-started.md).

Výsledek je jedním řádkem s jedním sloupcem. 165000 $ je součtem hodnot platu z celou tabulku. 

|TotalSalary
|-----------
|165000

>[AZURE.NOTE] Pokud začínáte funkce systému windows, je vhodné mít na paměti čísla v výstupy.  

Následující příkaz použít klauzuli GROUP BY pro výpočet celkové salery pro každé oddělení:

    @result=
        SELECT DeptName, SUM(Salary) AS SalaryByDept
        FROM @employees
        GROUP BY DeptName;

Výsledky jsou:

|DeptName|SalaryByDept
|--------|------------
|Inženýrské funkce|60000
|HR|30000
|Vedoucími|50000
|Marketingové|25000

Součet sloupce SalaryByDept je $165000, která odpovídá částka v poslední skript.
 
V obou těchto případech počet tam jsou menší než vstupní řádků výstup řádky:
 
- Bez GROUP BY agregace sbalí všechny řádky do jednoho řádku. 
- Seskupit podle existuje N řádků výstup kde N je počet různých hodnot, které se zobrazí v okně data v tomto případě že obdržíte 4 řádky v výstupu.

###  <a name="use-a-window-function"></a>Použití funkce okna

Klauzule přes v následujícím příkladu je prázdná. Tato možnost definuje "okna" zahrnout všechny řádky. Součet v tomto příkladu se použije pro klauzule přes, který předchází.

Může číst tento dotaz jako: "Součet platu přes okno všech řádků".

    @result=
        SELECT
            EmpName,
            SUM(Salary) OVER( ) AS SalaryAllDepts
        FROM @employees;

Na rozdíl od GROUP BY se tolik výstupních řádky jako vstupní řádky: 

|EmpName|TotalAllDepts
|-------|--------------------
|Noah|165000
|Sophia|165000
|Počítači|165000
|Anny|165000
|Jakub|165000
|Olivia|165000
|Mason|165000
|Ava|165000
|Nábytkářská|165000


Hodnota 165000 (součet všech platy) je umístěn v jednotlivých řádcích výstupu. Celková pochází z "okna" všechny řádky, tak, aby obsahoval na platy. 

Následující příklad ukazuje, jak chcete vylepšit "okna" Chcete-li zobrazit všechny zaměstnance, oddělení a celkové platu pro oddělení. ODDÍL tak, že je přidán do klauzule přes.

    @result=
    SELECT
        EmpName, DeptName,
        SUM(Salary) OVER( PARTITION BY DeptName ) AS SalaryByDept
    FROM @employees;

Výsledky jsou:

|EmpName|DeptName|SalaryByDep
|-------|--------|-------------------
|Noah|Inženýrské funkce|60000
|Sophia|Inženýrské funkce|60000
|Počítači|Inženýrské funkce|60000
|Mason|Vedoucími|50000
|Anny|HR|30000
|Jakub|HR|30000
|Olivia|HR|30000
|Ava|Marketing|25000
|Nábytkářská|Marketingové|25000

Existuje znovu stejný počet řádků vstupní jako výstup řádky. Ale každý řádek obsahuje celkové platu pro odpovídající oddělení.




## <a name="reporting-aggregation-functions"></a>Vytváření sestav funkce agregace

Funkce okno také podporují následující agregace:

- POČET
- SUMA
- MIN
- MAX
- AVG
- SMODCH.VÝBĚR
- VAR

Syntaxe:

    <AggregateFunction>( [DISTINCT] <expression>) [<OVER_clause>]

Poznámka: 

- Ve výchozím nastavení agregační funkce jazyka, s výjimkou počet, Ignorovat hodnoty null.
- Když agregační funkce jsou zadány spolu s klauzule přes, klauzule ORDER BY není povolené v klauzuli přes.

### <a name="use-sum"></a>Používá funkce Součet

Následující příklad sečte celkové platu tak, že oddělení IT, aby každý vstupní řádek:
 
    @result=
        SELECT 
            *,
            SUM(Salary) OVER( PARTITION BY DeptName ) AS TotalByDept
        FROM @employees;

Tady je výstup:

|EmpID|EmpName|DeptName|DeptID|Mzdy|TotalByDept
|-----|-------|--------|------|------|-----------
|1|Noah|Inženýrské funkce|100|10000|60000
|2|Sophia|Inženýrské funkce|100|20000|60000
|3|Počítači|Inženýrské funkce|100|30000|60000
|7|Mason|Vedoucími|300|50000|50000
|4|Anny|HR|200|10000|30000
|5|Jakub|HR|200|10000|30000
|6|Olivia|HR|200|10000|30000
|8|Ava|Marketingové|400|15000|25000
|9|Nábytkářská|Marketingové|400|10000|25000

### <a name="use-count"></a>Pomocí kombinace funkcí počet

Následující příklad přidává dalších polí pro každý řádek v každé oddělení zobrazit celkové čísla zaměstnanců.

    @result =
        SELECT *, 
            COUNT(*) OVER(PARTITION BY DeptName) AS CountByDept 
        FROM @employees;

Výsledek:

|EmpID|EmpName|DeptName|DeptID|Mzdy|CountByDept
|-----|-------|--------|------|------|-----------
|1|Noah|Inženýrské funkce|100|10000|3
|2|Sophia|Inženýrské funkce|100|20000|3
|3|Počítači|Inženýrské funkce|100|30000|3
|7|Mason|Vedoucími|300|50000|1
|4|Anny|HR|200|10000|3
|5|Jakub|HR|200|10000|3
|6|Olivia|HR|200|10000|3
|8|Ava|Marketingové|400|15000|2
|9|Nábytkářská|Marketingové|400|10000|2


### <a name="use-min-and-max"></a>Použití funkce MIN a MAX

Následující příklad přidává dalších polí pro každý řádek zobrazíte nejnižší platu každé oddělení:

    @result =
        SELECT 
            *,
            MIN(Salary) OVER( PARTITION BY DeptName ) AS MinSalary
        FROM @employees;

Výsledky:

|EmpID|EmpName|DeptName|DeptID|Mzdy|MinSalary
|-----|-------|--------|------|-------------|----------------
|1|Noah|Inženýrské funkce|100|10000|10000
|2|Sophia|Inženýrské funkce|100|20000|10000
|3|Počítači|Inženýrské funkce|100|30000|10000
|7|Mason|Vedoucími|300|50000|50000
|4|Anny|HR|200|10000|10000
|5|Jakub|HR|200|10000|10000
|6|Olivia|HR|200|10000|10000
|8|Ava|Marketingové|400|15000|10000
|9|Nábytkářská|Marketingové|400|10000|10000

Nahraďte maximální počet MIN a potom vyzkoušejte si to.


## <a name="ranking-functions"></a>Řazení funkcí

Definované oddílu podle a přes věty, vrátí funkce hodnocení pro každý řádek v každém oddílu hodnotu hodnocení (dlouhé). Uspořádání pořadí řídí Order v klauzuli přes.

Následující podporuje řazení funkcí:

- POŘADÍ
- DENSE_RANK 
- NTILE
- TEXTOVÝ

**Syntaxe:**

    [ RANK() | DENSE_RANK() | ROW_NUMBER() | NTILE(<numgroups>) ]
        OVER (
            [PARTITION BY <identifier, > …[n]]
            [ORDER BY <identifier, > …[n] [ASC|DESC]] 
    ) AS <alias>

- Klauzule ORDER BY je nepovinný krok pro řazení funkcí. Pokud není zadán ORDER BY ho určuje pořadí pořadí. Není-li Order zadán U SQL přiřadí hodnoty na základě objednávky a pak bude číst záznamu. Do jiných deterministický hodnota argumentu číslo řádku, pořadovou nebo hustotu tak výsledná rank v případě byly order klauzule není zadán.
- NTILE vyžaduje výraz, který je vyhodnocen kladné celé číslo. Toto číslo určuje počet skupiny, do kterých musí být každý oddíl rozdělen. Tento identifikátor se používá pouze NTILE řazení (funkce). 

Podrobné informace o klauzule přes najdete v článku Principy [U-SQL]().

Textový, pořadí a DENSE_RANK přiřadit čísel řádků v okně. Namísto zahrnovat jejich samostatně je intuitivnější najdete v článku jak reagovat na stejné vstupní.

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank, 
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank 
    FROM @querylog;
        
Poznámka: klauzule přes jsou stejné. Výsledek:

|Dotaz|Latence: int|Svislé ose|RowNumber|Pořadí|DenseRank
|-----|-----------|--------|--------------|---------|--------------
|Banánů|300|Obrázek|1|1|1
|Cherry|300|Obrázek|2|1|1
|Durian|500|Obrázek|3|3|2
|Apple|100|Web|1|1|1
|Fík|200|Web|2|2|2
|Papája|200|Web|3|2|2
|Fík|300|Web|4|4|3
|Cherry|400|Web|5|5|4
|Durian|500|Web|6|6|5

### <a name="rownumber"></a>TEXTOVÝ

V každém okně (svisle, obrázek nebo Web), zvýší číslo řádku 1 seřazené podle latence.  

![Funkce okno U SQL textový](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-row-number-result.png)

### <a name="rank"></a>POŘADÍ

Liší od ROW_NUMBER(), RANK() bere v úvahu hodnoty latence, které je určeno v klauzule ORDER BY okna.

Pořadí začíná (1,1,3), protože první dvě hodnoty latence jsou stejné. Potom další hodnota je 3, protože přesunutí hodnotu latence 500. Klíč umístěte ukazatel myši, že i když je duplicitní hodnoty jsou uvedeny stejném pořadí, číslo pořadí "přeskočí" na další hodnotu textový. Zobrazí se tento způsob opakování sekvence (2,2,4) ve svislém Web.

![Funkce okno U SQL pořadí](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-rank-result.png)

### <a name="denserank"></a>DENSE_RANK
    
DENSE_RANK je úplně stejně, jako SEŘADIT kromě ho není "Přeskočit" Další textový místo toho, že přejde na další číslo uvedené v pořadí. Všimněte si řady (1,1,2) a (2,2,3) ve výběru.

![Funkce okno U SQL DENSE_RANK](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-dense-rank-result.png)

### <a name="remarks"></a>Poznámky:

- Není-li Order zadán než řazení funkce použije se řádků bez jakékoli řazení. Vznikne do jiných definovaného chování na použitý řazení (funkce)
- Je možné, že řádky vrácená dotazem pomocí textový bude objednat stejně s každé spuštění Pokud jsou splněny následující podmínky.

    - Hodnoty ve sloupci oddíly jsou jedinečné.
    - Jsou jedinečné hodnoty sloupců řadit podle.
    - Kombinace hodnot sloupce oddíl a řadit podle sloupců jsou jedinečné.

### <a name="ntile"></a>NTILE

NTILE distribuuje řádků v oddílu uspořádaných do zadaný počet skupiny. Skupiny jsou číslovány, najednou. 


Následující příklad rozdělí sady řádků v každém oddílu (svislá osa) na 4 skupin v pořadí latence dotazu a vrátí číslo skupiny pro každý řádek. 

Obrázek svislé obsahuje 3 řádky, tedy obsahuje 3 skupiny. 

Svislá osa Web má 6 řádky, dva řádky jsou úměrně prvních dvou skupiny. Má proč mívají 2 řádky v skupiny 1 a 2 jenom 1 řádek v seskupení 3 a seskupit 4.  

    @result =
        SELECT 
            *,
            NTILE(4) OVER(PARTITION BY Vertical ORDER BY Latency) AS Quartile   
        FROM @querylog;
        
Výsledky:

|Dotaz|Latence|Svislé ose|Funkce QUARTIL
|-----|-----------|--------|-------------
|Banánů|300|Obrázek|1
|Cherry|300|Obrázek|2
|Durian|500|Obrázek|3
|Apple|100|Web|1
|Fík|200|Web|1
|Papája|200|Web|2
|Fík|300|Web|2
|Cherry|400|Web|3
|Durian|500|Web|4

NTILE má parametr ("numgroups"). Numgroups je kladný int nebo dlouho konstanta, která určuje počet skupiny, do kterých musí být každý oddíl rozdělen. 

- Pokud je počet řádků v oddílu dělitelný tak, že numgroups skupiny bude mít stejné velikosti. 
- Pokud není počet řádků v oddílu dělitelný numgroups, dojde k skupiny dvou formáty, které se liší podle jeden člen. Větších skupin předchází menších skupinách v uvedeném pořadí touto klauzulí přes. 

Příklad:

- 100 řádků rozdělit do skupin 4: [25, 25, 25, 25]
- 102 řádky devided do 4 skupin: [26 26, 25, 25]


### <a name="top-n-records-per-partition-via-rank-denserank-or-rownumber"></a>Horních N záznamy na oddíl prostřednictvím pořadí, DENSE_RANK nebo textový

Mnoho uživatelů chcete vybrat pouze HORNÍCH n řádků podle skupiny. Toto není možné s tradiční Seskupit podle. 

Následující příklad na začátek oddílu Funkce hodnocení zobrazila. Nezobrazuje horních N záznamy pro každý oddíl:

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;

Výsledky:

|Dotaz|Latence|Svislé ose|Pořadí|DenseRank|RowNumber
|-----|-----------|--------|---------|--------------|--------------
|Banánů|300|Obrázek|1|1|1
|Cherry|300|Obrázek|1|1|2
|Durian|500|Obrázek|3|2|3
|Apple|100|Web|1|1|1
|Fík|200|Web|2|2|2
|Papája|200|Web|2|2|3
|Fík|300|Web|4|3|4
|Cherry|400|Web|5|4|5
|Durian|500|Web|6|5|6

### <a name="top-n-with-dense-rank"></a>HORNÍCH N s HUSTOTU pořadí

Následující příklad vrátí začátek 3 záznamy z každé skupiny s bez mezer v sekvenční pořadí číslování řádků v každém práce s okny oddílu.

    @result =
    SELECT 
        *,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE DenseRank <= 3;

Výsledky:

|Dotaz|Latence|Svislé ose|DenseRank
|-----|-----------|--------|--------------
|Banánů|300|Obrázek|1
|Cherry|300|Obrázek|1
|Durian|500|Obrázek|2
|Apple|100|Web|1
|Fík|200|Web|2
|Papája|200|Web|2
|Fík|300|Web|3

### <a name="top-n-with-rank"></a>HORNÍCH N s POŘADÍM

    @result =
        SELECT 
            *,
            RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank
        FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE Rank <= 3;

Výsledky:    

|Dotaz|Latence|Svislé ose|Pořadí
|-----|-----------|--------|---------
|Banánů|300|Obrázek|1
|Cherry|300|Obrázek|1
|Durian|500|Obrázek|3
|Apple|100|Web|1
|Fík|200|Web|2
|Papája|200|Web|2


### <a name="top-n-with-rownumber"></a>HORNÍCH N s textový

    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber
        FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE RowNumber <= 3;

Výsledky:   
    
|Dotaz|Latence|Svislé ose|RowNumber
|-----|-----------|--------|--------------
|Banánů|300|Obrázek|1
|Cherry|300|Obrázek|2
|Durian|500|Obrázek|3
|Apple|100|Web|1
|Fík|200|Web|2
|Papája|200|Web|3

### <a name="assign-globally-unique-row-number"></a>Přiřazení čísla globálně duplicitních řádků

Často je vhodné přiřadit globálně jedinečným číslem pro každý řádek. Je to snadné (a efektivnější než použití reduktorem) s funkcemi jejich pořadí.

    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER () AS RowNumber
        FROM @querylog;

<!-- ################################################### -->
## <a name="analytic-functions"></a>Analytických funkcí

Analytický funkce se používají porozumět rozdělení hodnot v systému windows. Nejběžnější scénář pro používání analytických funkcí je výpočtu percentily.

**Podporované funkce analytickou okna**

- CUME_DIST 
- PERCENT_RANK
- PERCENTILE_CONT
- PERCENTILE_DISC

### <a name="cumedist"></a>CUME_DIST  

CUME_DIST vypočítá relativní pozici určité hodnoty ve skupině hodnot. Vypočítá procento dotazy, které mají latence menší nebo rovna aktuální latence dotazu v stejné svislé ose. Pro R řádek za předpokladu, že vzestupné řazení, že cume_dist R je počet řádků s hodnotou nižší než nebo rovná hodnotě R, děleno počet řádků v sadě výsledků oddílu nebo dotazu. CUME_DIST vrátí hodnotu čísla v rozsahu 0 < x < = 1.

**Syntaxe**

    CUME_DIST() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
    ) AS <alias>

Tento příklad používá funkci CUME_DIST pro výpočet percentilu latence pro každý dotaz v rámci svislé ose. 

    @result=
        SELECT 
            *,
            CUME_DIST() OVER(PARTITION BY Vertical ORDER BY Latency) AS CumeDist
        FROM @querylog;

Výsledky:
    
|Dotaz|Latence|Svislé ose|CumeDist
|-----|-----------|--------|---------------
|Durian|500|Obrázek|1
|Banánů|300|Obrázek|0.666666666666667
|Cherry|300|Obrázek|0.666666666666667
|Durian|500|Web|1
|Cherry|400|Web|0.833333333333333
|Fík|300|Web|0.666666666666667
|Fík|200|Web|o 0,5.
|Papája|200|Web|o 0,5.
|Apple|100|Web|0.166666666666667

V oddílu kde klíč oddílu je "Web" jsou 6 řádků (4. řádek a dolů):

- 6 řádků s hodnota rovna nebo menší než 500, aby CUME_DIST rovná 6/6 = 1
- 5 řádků s hodnota rovna nebo menší než 400, tak CUME_DIST se rovná 5/6 = 0,83
- 4 řádků s hodnota rovna nebo menší než 300, tak CUME_DIST se rovná 4/6 = 0.66
- Existují 3 řádky s hodnota rovna nebo menší než 200, tak CUME_DIST se rovná 3/6 = 0,5. Existují dva řádky stejné hodnoty latence.
- Existuje 1 řádek s hodnotou rovna nebo menší než 100, aby CUME_DIST rovná se 1/6 = číslo 0,16. 


**Použití poznámek:**

- Spojenými hodnotami vždy jsou vyhodnoceny jako stejnou hodnotu součtového rozdělení.
- Hodnoty NULL jsou považovány za nejnižších možných hodnot.
- Je nutné zadat klauzule ORDER BY pro výpočet CUME_DIST.
- CUME_DIST je podobná funkci PERCENT_RANK

Poznámka: Klauzule ORDER BY není povoleno, pokud není příkaz SELECT následuje výstupu. Klauzule ORDER BY v příkazu výstup tedy určuje pořadí zobrazení výsledné řádků.


### <a name="percentrank"></a>PERCENT_RANK

PERCENT_RANK vypočítá relativní pořadí řádek v rámci skupiny řádků. PERCENT_RANK slouží k vyhodnocení relativní postavení hodnotu řádků nebo oddílu. Oblast hodnoty vrácené PERCENT_RANK je větší než 0 a menší nebo rovna hodnotě 1. Na rozdíl od CUME_DIST PERCENT_RANK je vždy 0 pro první řádek.
    
**Syntaxe**

    PERCENT_RANK() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
        ) AS <alias>

**Poznámky**

- První řádek v nastavených obsahuje PERCENT_RANK 0.
- Hodnoty NULL jsou považovány za nejnižších možných hodnot.
- Je nutné zadat klauzule ORDER BY pro výpočet PERCENT_RANK.
- CUME_DIST je podobná funkci PERCENT_RANK 


Tento příklad používá funkci PERCENT_RANK pro výpočet percentilu latence pro každý dotaz v rámci svislé ose. 

Klauzule oddíl tak, že není zadán oddíl řádků v sadě svislou výsledků. Klauzule ORDER BY v klauzuli přes seřadí řádky v každém oddílu. 

Hodnota vrácená funkcí PERCENT_RANK představuje pořadí dotazy zpoždění v rámci svislé jako procentuální hodnotu. 


    @result=
        SELECT 
            *,
            PERCENT_RANK() OVER(PARTITION BY Vertical ORDER BY Latency) AS PercentRank
        FROM @querylog;

Výsledky:

|Dotaz|Latence: int|Svislé ose|PercentRank
|-----|-----------|--------|------------------
|Banánů|300|Obrázek|0
|Cherry|300|Obrázek|0
|Durian|500|Obrázek|1
|Apple|100|Web|0
|Fík|200|Web|0,2
|Papája|200|Web|0,2
|Fík|300|Web|0,6
|Cherry|400|Web|0,8
|Durian|500|Web|1

### <a name="percentilecont--percentiledisc"></a>PERCENTILE_CONT & PERCENTILE_DISC

Tyto dvě funkce vypočítá percentilu podle nepřetržitý nebo samostatné rozdělení množiny hodnot sloupce.

**Syntaxe**

    [PERCENTILE_CONT | PERCENTILE_DISC] ( numeric_literal ) 
        WITHIN GROUP ( ORDER BY <identifier> [ ASC | DESC ] )
        OVER ( [ PARTITION BY <identifier,>…[n] ] ) AS <alias>

**numeric_literal** - percentilu pro výpočet. Hodnota musí nacházet v rozsahu mezi 0,0 a 1,0.

V rámci skupiny (řadit podle <identifier> [ASC | [[[DESC]) - určuje seznam číselných hodnot řadit a výpočet percentil myší. Identifikátor URI pouze jeden sloupec jsou povolené. Výraz musí být číselného typu. Jiné typy dat nejsou povolené. Výchozí pořadí řazení je vzestupné.

PŘES ([oddílu podle < identifikátor > … [n]]) - rozdělí vstupní řádků do oddílů podle klíč oddílu, do kterého se použije funkce PERCENTIL. Další informace najdete v tématu řazení části v tomto dokumentu.
Poznámka: Všechny hodnoty Null v uvedenou množinu dat jsou ignorovány.

**PERCENTILE_CONT** vypočítá percentilu podle nepřetržitý rozdělení hodnotu ve sloupci. Výsledek je interpolované a nemusí být rovna libovolné určitých hodnot ve sloupci. 

**PERCENTILE_DISC** vypočte percentil podle abstraktní rozdělení množiny hodnot sloupce. Výsledek je rovno určitou hodnotu ve sloupci. Jinými slovy, PERCENTILE_DISC, naopak PERCENTILE_CONT, vždy vrátí skutečné (původní vstup) hodnotu.

Uvidíte, jak fungování v dalším příkladu vrací které snaží najít medián (percentilu = 0,50) hodnotu pro latence v rámci každé svisle

    @result = 
        SELECT 
            Vertical, 
            Query,
            PERCENTILE_CONT(0.5) 
                WITHIN GROUP (ORDER BY Latency)
                OVER ( PARTITION BY Vertical ) AS PercentileCont50,
            PERCENTILE_DISC(0.5) 
                WITHIN GROUP (ORDER BY Latency) 
                OVER ( PARTITION BY Vertical ) AS PercentileDisc50 
        
        FROM @querylog;

Výsledky:

|Dotaz|Latence: int|Svislé ose|PercentileCont50|PercentilDisc50
|-----|-----------|--------|-------------------|----------------
|Banánů|300|Obrázek|300|300
|Cherry|300|Obrázek|300|300
|Durian|500|Obrázek|300|300
|Apple|100|Web|250|200
|Fík|200|Web|250|200
|Papája|200|Web|250|200
|Fík|300|Web|250|200
|Cherry|400|Web|250|200
|Durian|500|Web|250|200


Pro PERCENTILE_CONT vzhledem k tomu můžete interpolované hodnoty, pro web se střední 250 i když žádný dotaz na webu svislé dříve latence 250. 

PERCENTILE_DISC není zjistit hodnoty tak, aby střední hodnotu pro Web 200 – tedy skutečná hodnota nalezena v zadávání řádků.











## <a name="see-also"></a>Viz taky

- [Přehled analýzy dat jezera Microsoft Azure](data-lake-analytics-overview.md)
- [Začínáme s jezera analýzy dat pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s jezera analýzy dat pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Použití interaktivní výukové programy pro analýzy jezera dat Azure](data-lake-analytics-use-interactive-tutorials.md)
- [Analýza protokoly webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md)
- [Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)
- [Správa portálu Azure analýzy jezera dat Azure](data-lake-analytics-manage-use-portal.md)
- [Správa Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
- [Sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy pomocí portálu Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
