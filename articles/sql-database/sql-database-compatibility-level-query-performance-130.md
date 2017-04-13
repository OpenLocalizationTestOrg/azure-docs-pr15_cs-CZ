<properties
    pageTitle="Funkce pro kompatibilitu úroveň, jak posuďte | Microsoft Azure"
    description="Kroky a nástroje pro určení, které úroveň kompatibility je nejvhodnější pro databáze aplikace na databázi SQL Azure nebo Microsoft SQL Server"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Vylepšení výkonu dotazu s kompatibilitou 130 úroveň v databázi SQL Azure


Databáze SQL Azure běží transparentně stovky tisíce databází na mnoha různých kompatibility úrovních zachovávání a zajistit zpětnou kompatibilitu zkontrolovanou na odpovídající verzi aplikace Microsoft SQL Server pro všem svým zákazníkům!

Proto nic zabrání zákazníkům, kteří změnit existující databáze na úroveň nejnovější funkce pro kompatibilitu z využívající nové optimalizace dotazů a funkce procesorů dotazu. Připomínáme historie jsou zarovnání verze SQL pro výchozí kompatibility úrovně:

- 100: v SQL Server 2008 a Azure SQL databáze V11.
- 110: v SQL Server 2012 a Azure SQL databáze V11.
- 120: v SQL Server 2014 a Azure SQL databáze V12.
- 130: v jazyce SQL serveru 2016 a Azure SQL databáze V12.


> [AZURE.IMPORTANT] Spuštění ve **2016 mid dne**v databázi SQL Azure, budou výchozí úroveň kompatibility 130 místo 120 pro **nově vytvořený** databáze.
> 
> Databáze vytvořené před mid června 2016 se *není* to mít vliv na a bude udržovat aktuální kompatibility úroveň (100 110 a 120). Databáze, které migrovat z databáze SQL Azure verze že V11 V12 nebudete mít na úroveň svého kompatibility změnit buď.


V tomto článku jsme přečtěte si o výhodách úroveň kompatibility 130 a jak můžete využít tyto výhody. Jsme adresu nežádoucí vliv na výkon dotazu pro existující SQL aplikace.


## <a name="about-compatibility-level-130"></a>O úroveň kompatibility 130


Pokud budete chtít zjistit aktuální úroveň kompatibility databázi, nejprve, spusťte následující příkaz Transact-SQL.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Před tuto změnu na úrovni 130 se **nově** vytvořený databází, Pojďme zkontrolujte k čemu tato změna je vše o prostřednictvím příklady velmi jednoduché dotazu a najdete v článku jak kdokoliv můžete využívat.

Zpracování dotazů v relační databáze může být velmi složité a může vést k počítači pro výzkum a matematických výrazů pomocí pochopit inherentní návrh možností a chování. V tomto dokumentu má byla obsah úmyslně zjednodušené a zajistit, aby každý, kdo má některé základní minimální technické znát důsledek prováděných Změna úrovně kompatibility a zjistit, jak se vám může hodit aplikací.

Podívejme se na rychlý přehled úroveň kompatibility 130 přináší v tabulce.  Další informace naleznete na [Změnit úroveň kompatibility databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale tady je krátké shrnutí:

- Operace vložení příkazu Vložit vyberte může být přepočet ve více vláknech nebo můžete mít paralelní plán, zatímco před operaci jedním podprocesem.
- Paměť optimalizovaný tabulek a dotazů proměnné tabulky mohou mít v tomto paralelní plánů, zatímco před operaci byl také jedním podprocesem.
- Statistiky paměť optimalizována tabulky se automaticky aktualizován a můžete nyní odeberou. V tématu [Co je nového v databázový stroj: V paměti OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) další podrobnosti.
- V režimu dávku/s režimu řádku změní indexů sloupců úložiště
  - Seřadí v tabulce s indexový sloupec úložiště jsou teď dávku režimu.
  - Práce s okny agregace teď pracovat v režimu dávku například TSQL prodlevy a PŘEDSTIHU příkazy.
  - Dotazy na úložiště sloupec tabulky se několik různých klauzulí pracovat v režimu dávku.
  - Dotazy používajícími DOP = 1 nebo sériové plánu také spouštění v režimu dávku.
- Poslední mohutnosti odhad vylepšení pocházejí skutečně s úroveň kompatibility 120, ale výsledky používáte nižší úrovně kompatibility (tedy 100 nebo 110), přesunutí kompatibility úroveň 130 také přenést vylepšení a také tyto výhodách výkonu dotazu aplikací.


## <a name="practicing-compatibility-level-130"></a>Cvičení úroveň kompatibility 130


První nastavíme několik tabulek, indexy a náhodná data vytvořené vyzkoušet některé tyto nové funkce. Příklady skriptů TSQL mohou být provedeny ve skupinovém rámečku SQL serveru 2016 nebo v databázi SQL Azure. Ale při vytváření databáze Azure SQL, ujistěte se, že vyberete minimálně databázi P2 vzhledem k tomu potřebujete alespoň několik jádra povolit multithreading využívaný a tedy těžit z těchto funkcí.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Teď Pojďte si to vyzkoušet na některé z funkcí zpracování dotazů chodit s úroveň kompatibility 130.


## <a name="parallel-insert"></a>Paralelní vložení


Spuštění pod příslušným TSQL provede vložení operaci v části funkce pro kompatibilitu výši 120 a 130, respektive provádí operace vložení v jednom s vlákny modelu (120) a v modelu přepočet ve více vláknech (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Vyžádat skutečné plán dotazů prohlížíte grafické znázornění nebo jeho obsah XML můžete určit, které mohutnosti odhad funkce je na přehrát. Na obrázku 1 prohlížíte plány vedle sebe, jasně vidíme, že spuštění vložit sloupec úložiště, půjde z sériové v 120 paralelní v 130. Všimněte si také, že je změna ikonu iterace v plánu 130 zobrazující dvě paralelní šipky znázorňující skutečnost nyní spuštění iterace skutečně paralelní. Pokud máte velký operace vložení dokončete paralelního spuštění propojena s číslem základní, ke kterým máte k dispozici pro databázi, provede lépe; až 100 časy rychlejší podle to!


*Obrázek 1: Vložení operace měnit sériové na rovnoběžně s kompatibilitou úroveň 130.*


![Obrázek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>SÉRIOVÉ dávku režimu


Podobně přesun kompatibility úrovně 130 při zpracování řádků dat umožňuje dávkové zpracování režimu. Nejdřív operace v režimu dávku jsou k dispozici pouze když budete mít indexu sloupce úložiště na místě. Za druhé dávky obvykle představuje ~ 900 řádky, používá kód logiky optimalizované pro vícejádrových procesor vyšší výkon paměti a přímo využívá zkomprimovaná data úložišti sloupec kdykoli je to možné. Za těchto podmínek SQL serveru 2016 můžete najednou, zpracuje ~ 900 řádků místo 1 řádek v době, a v důsledku toho celkové režijních nákladů operace teď sdílí celý list, snížení celkové náklady zobrazíte řádku. Tento sdílený počet operací spojení s komprese úložiště sloupce v podstatě snižuje zahrnuté do operace režimu vyberte dávku zpoždění. Můžete najít další informace o úložišti sloupce a dávkové režimu na [Průvodce Columnstore indexy](https://msdn.microsoft.com/library/gg492088.aspx).


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Jako viditelné pod tak, že pozorování dotaz plány jednotného zasílání zpráv vedle sebe na obrázek 2, můžeme projděte si, že došlo ke změně režim zpracování úroveň kompatibility a v důsledku toho při provádění dotazů v obou úroveň kompatibility úplně odebrat, vidíme, že ve většině případů zpracování je na něm v režimu řádku (86 %) ve srovnání s režimu dávku (14 %), kde byly zpracovány 2 listy. Zvětšení datovou sadu, výhody zvětšíte.


*Obrázek 2: Vyberte operace změny sériové dávku režim kompatibility úroveň 130.*


![Obrázek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Dávkový režim na spuštění řazení


Podobně jako výše uvedené, ale pro operaci řazení přechodu z režimu řádku (úroveň kompatibility 120) do režimu dávku (úroveň kompatibility 130) zvýšíte výkon operace řazení stejných důvodů.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Viditelné vedle sebe na obrázek 3, vidíme, že operaci řazení v režimu řádek představuje 81 % nákladů, zatímco režimu dávku pouze představuje 19 % nákladů (v tomto pořadí 81 % a 56 % na vlastní řazení).


*Obrázek 3: Operaci řazení měnit řádku na dávku režim kompatibility úroveň 130.*


![Obrázek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Samozřejmě tyto ukázky obsahovat pouze desítky tisíce řádky, tedy nic při prohlížení dat k dispozici ve většině servery SQL tyto dny. Jenom tyto proti miliony řádků místo toho projektu, a to mohli překládat za několik minut provádění ušetřena každý den až přírodní vaše pracovní zátěž.


## <a name="cardinality-estimation-ce-improvements"></a>Vylepšení mohutnosti odhad (CE)


Zavádí se SQL Server 2014, všechny databáze s kompatibilitou úrovni 120 nebo nad bude můžete využít nové odhad mohutnosti funkce. Odhad mohutnosti je v podstatě logickou požívaný k určení, jak serveru SQL server spustí dotaz založený na odhad nákladů. Odhad se počítají pomocí vstupní z statistiky přidružený k objektům zahrnuté do tohoto dotazu. Prakticky, vysoké úrovni mohutnosti odhad funkce jsou řádek počet odhady spolu s informacemi o rozdělení hodnot, vrátí odlišné hodnoty a duplicitní počty obsažené v tabulkách a objekty, které odkazují na dotaz. Zobrazuje tyto odhady nepovedlo, může vést nepotřebných disk vstupu a výstupu kvůli nedostatku paměti podpory (tedy TempDB polití) nebo k výběru sériové plán spuštění přes paralelní plán spuštění pojmenování několik. Uzavření nesprávné odhady může vést k celkový výkon snížení úrovně spuštění dotazu. Na druhé straně lepší odhady přesnější odhady vede k lepší spuštění dotazu!

Jak je uvedeno před, optimalizace dotazu a vypočte jsou složité věci, ale pokud chcete další informace o plánech dotazu a mohutnosti odhad, můžete vytvořit odkaz v dokumentu na [Optimalizace svůj dotaz plány SQL Server 2014 mohutnosti odhad](https://msdn.microsoft.com/library/dn673537.aspx) hlubší postupy.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Které mohutnosti odhad momentálně používáte?


Chcete-li zjistit v části které mohutnosti odhad jsou spuštěné dotazech, stačí použijeme následující dotaz ukázky. Všimněte si, že tento první příklad se spustí v části úroveň kompatibility 110, za použití funkce staré mohutnosti odhad.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Po dokončení spuštění klikněte na odkaz XML a podívejte se na vlastnosti první iterace, jak je ukázáno v následujícím příkladu. Poznámka: název vlastnosti s názvem aktuálně nastavené na 70 CardinalityEstimationModelVersion. Není znamená, že úroveň kompatibility databáze je nastavena na verzi SQL Server 7.0 (je nastavena na 110 jako viditelné v příkazech TSQL nahoře), ale hodnota 70 jednoduše představuje starší mohutnosti odhad funkci dostupný od 7.0 serveru SQL, který měl žádné hlavní revize do SQL Server 2014 (což je součástí úroveň kompatibility 120).


*Obrázek 4: CardinalityEstimationModelVersion nastavenou 70 při použití funkce pro kompatibilitu úroveň 110 nebo pod.*


![Obrázek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Můžete taky změnit úroveň kompatibility 130 a zakázat používání této nové funkci mohutnosti odhad pomocí LEGACY_CARDINALITY_ESTIMATION nastavte do polohy zapnuto s [Změnit rozsah konfigurace databáze](https://msdn.microsoft.com/library/mt629158.aspx). Půjde přesně stejné jako při použití 110 z mohutnosti odhad funkce hlediska, při používání posledního dotazu zpracování kompatibility úroveň. Tím, můžete využít nový dotaz zpracování funkce chodit s úroveň nejnovější funkce pro kompatibilitu (tedy dávku režimu), ale pořád spolehnout funkci staré mohutnosti odhad v případě potřeby.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Jednoduše přesun Úrovně kompatibility 120 nebo 130 umožňuje nových funkcí mohutnosti odhad. V takovém případě výchozí CardinalityEstimationModelVersion nastaví se příslušným způsobem 120 nebo 130 jako viditelné níže.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Obrázek 5: CardinalityEstimationModelVersion nastavenou 130 při použití funkce pro kompatibilitu úroveň 130.*


![Obrázek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Odhad mohutnosti rozdíly při jeho práci


Teď Pojďme spustit trochu složitější dotazy týkající se operace INNER JOIN s klauzuli WHERE s některé predikáty a Podívejme se na odhad počet řádků ze staré funkce mohutnosti odhad nejdřív.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Spuštění tohoto dotazu efektivně vrátí 200,704 řádky během odhad řádku s funkci staré mohutnosti odhad deklarace 194,284 řádky. Samozřejmě jako said před těchto řádků počet výsledků taky závisí intervalu jste spustili předchozí výběry, který naplní ukázkové tabulky pořád dokola při každé spuštění. Samozřejmě predikáty v dotazu bude mít taky zajištěný vliv na skutečný odhad kromě obrazec stolu dat obsahu, a jak tato data skutečně sladit navzájem.


*Obrázek 6: Odhad počet řádků vypnutá 194,284 nebo 6 000 řádků z 200,704 řádky by měly.*


![Obrázek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Stejným způsobem Pojďme teď spuštění stejného dotazu s novými funkcemi mohutnosti odhad.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


V tady vidíte, teď vidíme, že je odhad řádku 202,877, nebo mnohem bližší a vyšší než staré odhad mohutnosti.

*Obrázek 7: Odhad počet řádků je teď 202,877 místo 194,284.*


![Obrázek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


Ve skutečnosti je 200,704 řádky (všechno závisí, jak často spouštění dotazů předchozí ukázky ale důležitější, protože TSQL pomocí příkazu RAND(), skutečné hodnoty vrácené se může lišit z jednoho spustit na další). V tomto příkladu konkrétní proto nové odhad mohutnosti znamená lepší úlohy na odhad počet řádků, protože je mnohem blíž k 200,704, než 194,284 202,877! Příjmení, se při změně klauzule WHERE predikáty rovnosti (nekoupili ">" například), to může výrazně odhady mezi původní a novou funkci mohutnosti ještě víc lišit podle toho, kolik odpovídá dostali.

Samozřejmě v tomto případě se řádky ~ 6000 vypnout z skutečný počet nepředstavuje velké množství dat v některých případech. Transpozice, kterou chcete miliony řádků napříč několika tabulek a dalších složitějších dotazy a v některých případech odhad může to být vypnout tak, že miliony řádků a proto riziko nakreslenými výběru plán nepovedlo spuštění nebo vyžádání podpory nedostatku paměti vést k TempDB polití a vstupu a tak další výstupu, jsou teď podstatně vyšší.

Pokud máte možnost praktická část v porovnání s některé běžné dotazy a datových sad a uvidíte sami podle kolik některé odhadů starý i nový se to týká, zatímco jenom některé může být více vypnout z skutečnosti nebo některé ostatní stejně jednoduše blízkosti na aktuální řádek spočítá ve skutečnosti vráceném ve výše uvedené množiny výsledek. Všechno závisí obrazce dotazech, vlastnosti databáze Azure SQL, vlastnosti a velikost vaší datové sady a statistiky dostupné o nich znáte. Pokud jste právě vytvořili instance databáze SQL Azure, bude mít tento nástroj pro optimalizaci dotazu vytvářet své znalosti od začátku místo opakované použití statistik z předchozích spuštění dotazu. Ano odhady jsou velmi kontextové a téměř specifické pro každé situaci serveru a aplikace. Je důležitým aspektem mít na paměti.


## <a name="some-considerations-to-take-into-account"></a>Několik tipů k vzít v úvahu


I když většina úloh byste měli využívat úroveň kompatibility 130, před přijetím úroveň kompatibility pro provozním prostředí, máte v podstatě 3 možnosti:

1. Přesunutí na úroveň kompatibility 130 a najdete v článku jak provádět věci. V případě Všimněte si některé regrese stejně jednoduše nastavování kompatibilitu úrovně zpátky na původní úroveň nebo při zachování 130 a pouze obráceném odhad mohutnosti zpět na starší režim (jak je vysvětleno, to samostatně může problém vyřešit).
2. Graf doladit důkladně otestujte existující aplikace podobné výrobní zatížení a ověřit výkon před přechodem do výroby. V případě problémech se dočtete víc stejný příkaz jako výše uvedené, můžete vždy přejděte zpátky na původní úroveň kompatibility, nebo jednoduše odhad mohutnosti vrátit zpět do starším režimu.
3. Je jako konečný možnost a poslední způsob, jak tyto otázky, můžete využít úložišti dotazu. To je možnost doporučená aktuálnímu! Pomáhat analýzy svoje dotazy v části funkce pro kompatibilitu úroveň 120 nebo pod versus 130, nemůžete doporučujeme dostatečně použít úložiště dotazu. Dotaz úložiště je k dispozici v rámci nejnovější verze V12 databáze SQL Azure a je navržen tak, aby vám pomohl řešení potíží s výkonem dotazu. Představit úložišti dotazu jako záznam dat letu databáze shromažďování a prezentování historické podrobné informace o všech dotazů. To výrazně zjednodušuje výkonu forenzních jejich zmenšením čas Diagnostika a řešení problémů s. Další informace najdete [dotazu Store: záznam dat letu databáze](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


Na vysoké úrovni, pokud už máte sadu databází systém úrovni kompatibility 120 nebo pod a plánování některé z nich přesunout na 130 nebo váš pracovní zátěž automaticky vytvořit nové databáze, které budou brzy nutné nastavit, protože ve výchozím nastavení 130, zvažte těchto hodnot:

- Před změnou nové úrovni kompatibility v výrobní povolte úložiště dotazu. [Změna režimu kompatibility databáze](https://msdn.microsoft.com/library/bb895281.aspx) a používat úložišti dotazu naleznete další informace.
- Potom vyzkoušejte všechny kritické zatížení s pomocí zástupce dat a dotazy výrobní profesionálové prostředí a porovnejte výkon praxí a nahlášeného při ukládání dotazu. Pokud narazíte na některé regrese, můžete určit regressed dotazů se obchodu dotazu a používat plán vynucení možnost z úložiště dotazu (označovaná taky jako plán Připnutí). V takovém případě používáte platností pořád používat úroveň kompatibility 130 a používáte plán bývalého dotazů navržené tak, že úložišti dotazu.
- Pokud chcete, můžete využít nové funkce a možnosti databáze SQL Azure, (které běží SQL serveru 2016), ale jsou citlivé na změny vedená úroveň kompatibility 130, jako poslední zvažte vynucení úroveň kompatibility zpět na úroveň, která odpovídá vaší pracovní zátěž pomocí příkazu Vlastnosti databáze. Ale poprvé, mějte na paměti, že Připnutí možnost plán úložiště dotazu je nejlepší možností vzhledem k tomu, že nepoužíváte 130 je v podstatě zachování na úrovni funkce starší verzí systému SQL Server.
- Pokud máte víceklientské aplikace, která trvá více databázím, může být nutné aktualizovat zřizovací logickou databáze zajistit konzistentní kompatibility úroveň přes všechny databáze; starý i nově vytvořený z nich. Výkon pracovní zátěž aplikací může být citlivé na to, že některé databáze používají na úrovni různé funkce pro kompatibilitu a tedy kompatibility úrovně konzistenci mezi jakékoli databáze může být nutné pro všechny ve na vývěsku poskytnout stejné prostředí zákazníků. Všimněte si, že není pověření skutečně závisí aplikace je vlivu úroveň kompatibility.
- Poslední týkající se odhad mohutnosti a stejně jako změnit úroveň kompatibility, než budete pokračovat ve výrobním, doporučujeme otestovat výrobní vytížení nové podmínek a zjistit, pokud vaše aplikace využívá vylepšení mohutnosti odhad.


## <a name="conclusion"></a>Uzavření


Použití databáze SQL Azure využívat všech vylepšení SQL serveru 2016 můžete vylepšit jasně spuštění dotazu. Stejně jako-je! Samozřejmě stejně jako nové funkce, VELKÁ2 hodnocení musí provést určit přesné podmínky, za jakých pracovní zátěž databáze funguje maximum. Prostředí ukazuje, že většina pracovní zátěž se předpokládá aspoň transparentně v části úroveň kompatibility 130, při využití nový dotaz zpracování funkcí a nové mohutnosti odhad. Která říká neměli překročit, jsou vždy zvláštní případy a udělat správné termín péčí je důležité hodnocení a zjistit, kolik můžete využít tato vylepšení. A znovu úložišti dotazu může být skvělý nápovědy v udělat to fungovalo!

Jak SQL Azure vývoj, můžete očekávat, že úroveň kompatibility 140 v budoucnu. Kdy je vhodné čas, bude začneme konverzace o co této úrovni budoucí kompatibilitu 140 přinese, stejně jako jsme stručně zde popsané úroveň kompatibility 130 uvedení dnes.

Prozatím Pojďme není zapomenete, od června 2016, databáze SQL Azure se změní výchozí úroveň kompatibility ze 120 130 nově vytvořený databází. Mějte na paměti.


## <a name="references"></a>Odkazy


- [Co je nového v databázový stroj](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Dotazu úložiště: záznam letu dat databáze, tak, že Borko Novakovic 8 2016 dne](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [Příkaz ALTER databáze kompatibility úrovně (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [ZMĚNIT OBORY KONFIGURACE DATABÁZE](https://msdn.microsoft.com/library/mt629158.aspx)

- [Funkce pro kompatibilitu úroveň 130 V12 databáze Azure SQL](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Optimalizace dotazu plány jednotného zasílání zpráv se serverem SQL odhad mohutnosti 2014](https://msdn.microsoft.com/library/dn673537.aspx)

- [Příručka Columnstore indexy](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blogu: Lepší dotazu výkon úroveň kompatibility 130 v databázi Azure SQL pomocí Alain Lissoir, 6 2016 dne](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
