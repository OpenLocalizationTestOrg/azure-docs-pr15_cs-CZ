<properties
    pageTitle="Procesu vědy dat tým v akci: použití serveru SQL Server | Microsoft Azure"
    description="Advanced Analytics procesy a technologii v akci"  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Procesu vědy dat tým v akci: použití serveru SQL Server

V tomto kurzu, můžete návodu vytváření a zavádění modelu strojovém učení pomocí serveru SQL Server a veřejně dostupné datové sady-- [Cest Taxi NYC](http://www.andresmh.com/nyctaxitrips/) dataset. Postup následuje standardní data pracovního postupu vědy: jedí a zkoumat data, inženýr funkce usnadnění učení, pak sestavit a nasadit model.


## <a name="dataset"></a>Výlet taxíkem NYC popis objektu Dataset

Dat cesty Taxi NYC je asi 20GB komprimované soubory CSV (nekomprimovaný ~ 48GB), skládající se z více než 173 milionů jednotlivých cest a jízdné placené pro každou cestu. Každý záznam cesty zahrnuje licenční číslo odběru a odkládací místo a čas, anonymních napadení (ovladač) a číslo Medailon (jedinečné id na taxi). Data se vztahuje na všechny cesty v roce 2013 a je k dispozici v následujících dvou objektů DataSet každého měsíce:

1. "Trip_data" CSV obsahuje podrobnosti o služební cesty, jako je počet cestujících, výdej a dropoff body, doba trvání cesty a délka cesty. Zde je několik vzorových záznamů:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. "trip_fare" CSV obsahuje podrobnosti o jízdné placené pro každou cestu, typ platby, částka jízdné, příplatek a daně, tipů a mýtné a celkovou částku zaplacenou. Zde je několik vzorových záznamů:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinečný klíč připojení cesty\_dat a cesty\_jízdného se skládá z polí: Medailon napadení\_licence a výdej\_datetime.

## <a name="mltasks"></a>Příklady úloh předpověď

Jsme tři problémy předpověď na základě dává *tip\_částka*, jmenovitě:

1. Binární klasifikace: předpovědět, zda tip byl vyplacen výlet, tj *tip\_částka* větší než 0 Kč je pozitivní příklad, zatímco *tip\_částka* $ 0 je negativní příklad.

2. Multiclass klasifikace: odhadnout rozsah tip zaplacené cesty. Můžeme rozdělit *tip\_množství* do pěti tříd nebo přihrádky:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regresní úloha: odhadnout výši zaplacené cesty tip.  


## <a name="setup"></a>Nastavení nahoru Azure data science prostředí pro pokročilé analytiky

Jak můžete vidět z průvodce [Plán daného prostředí](machine-learning-data-science-plan-your-environment.md) , existuje několik možností pro práci s datovou sadu cest Taxi NYC v Azure:

- Práce s daty v Azure objektů BLOB v Azure Machine Learning pak model
- Načtení dat do databáze serveru SQL Server potom model v Azure Machine Learning

V tomto kurzu budeme ukazují paralelní hromadný import dat do serveru SQL Server, průzkum, funkce inženýrství a dolů odběr vzorků pomocí SQL Server Management Studio, stejně jako pomocí IPython Notebook. [Ukázky skriptů](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) a [IPython notebooky](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) jsou sdíleny v GitHub. Vzorku IPython notebook pro práci s daty v Azure objektů blob je také k dispozici ve stejném umístění.

Nastavení prostředí Azure Data vědy:

1. [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md)

2. [Vytvoření pracovního prostoru Azure Machine Learning](machine-learning-create-workspace.md)

3. [Poskytování dat vědy virtuálního počítače](machine-learning-data-science-setup-sql-server-virtual-machine.md), který bude sloužit jako SQL Server serveru IPython Notebook.

    > [AZURE.NOTE] Ukázky skriptů a IPython notebooky budou staženy do virtuálního počítače vědy dat během procesu instalace. Po dokončení skriptu po instalaci modulu VM, vzorky budou v knihovně dokumentů v modulu VM:  
    > - Ukázkové skripty:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Notebooky IPython vzorku:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > Pokud `<user_name>` se váš VM Windows přihlašovací jméno. Jsme bude odkazovat ukázkové složek jako **Ukázkové skripty** a **Notebooky IPython vzorku**.


Podle velikosti objektu dataset, umístění zdroje dat a vybrané cílové Azure prostředí, tento scénář je podobný [scénář \#5: cíl velké datové sady v místních souborech serveru SQL Azure VM](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Získat Data z veřejné zdroje

Dataset [Cest Taxi NYC](http://www.andresmh.com/nyctaxitrips/) získat z veřejné umístění, můžete použít libovolnou z metod popsaných v [Přesunutí dat z úložiště objektů Blob Azure a](machine-learning-data-science-move-azure-blob.md) kopírování dat na nový virtuální počítač.

Kopírování dat pomocí AzCopy:

1. Přihlášení do virtuálního počítače (VM)

2. Vytvoření nového adresáře v modulu VM datový disk (Poznámka: Nepoužívejte dočasné disku, který je součástí modulu VM jako datový Disk).

3. V okně příkazového řádku spusťte následující příkazového řádku Azcopy nahraďte < path_to_data_folder > složky dat vytvořených v (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Po dokončení AzCopy celkem 24 zip soubory CSV (12 pro cestu\_dat a 12 pro cestu\_tarif) by měl být ve složce data.

4. Rozbalit stažené soubory. Poznámka: složky, kde jsou uloženy nekomprimované soubory. Tato složka bude označována jako < cesta\_k\_dat\_soubory\>.

## <a name="dbload"></a>Hromadný Import dat do databáze serveru SQL Server

Výkon načítání nebo přenos velkého množství dat do databáze serveru SQL a následné dotazy lze zvýšit pomocí _oddílů tabulky a zobrazení_. V této části jsme se podle pokynů popsané v [Paralelní hromadný Import pomocí SQL oddílu tabulky dat](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) k vytvoření nové databáze a načtení dat do rozdělených tabulek současně.

1. Při přihlášení do VM, spustíte **SQL Server Management Studio**.

2. Připojte pomocí ověřování systému Windows.

    ![SSMS připojení][12]

3. Pokud jste dosud změněn režim ověřování serveru SQL Server a vytvořit nového uživatele přihlášení SQL, otevřete soubor skriptu s názvem **Změna\_auth.sql** ve složce **Ukázky skriptů** . Změňte výchozí uživatelské jméno a heslo. Klepněte na tlačítko **! Spustit** v panelu nástrojů, bude skript spuštěn.

    ![Spuštění skriptu][13]

4. Ověřit nebo změnit SQL Server výchozí složky databáze a protokolu k zajištění které nově vytvořené databáze bude uložena v datový Disk. SQL Server VM obraz, který je optimalizován pro datawarehousing zatížení je předem nakonfigurované s disky dat a protokolů. Pokud váš VM neobsahuje datový Disk a přidat nové virtuální pevné disky během procesu instalace modulu VM, změňte výchozí složky takto:

    - Klepněte pravým tlačítkem myši na název serveru SQL Server v levém panelu a klepněte na příkaz **Vlastnosti**.

        ![Vlastnosti serveru SQL][14]

    - **Nastavení databáze** vyberte ze seznamu **vyberte na stránce** vlevo.

    - Ověřit nebo změnit **výchozí umístění databáze** na **Disku Data** umístění dle vašeho výběru. Toto je, kde jsou uloženy nové databáze, pokud je vytvořen ve výchozím nastavení umístění.

        ![Výchozí nastavení databáze SQL][15]  

5. Chcete-li vytvořit novou databázi a sadu skupin souborů pro uložení rozdělených tabulek, otevřete ukázkový skript **vytvořit\_db\_default.sql**. Skript vytvoří novou databázi s názvem **TaxiNYC** a 12 skupin souborů ve výchozím umístění pro data. Každá skupina souborů bude obsahovat jeden měsíc cesty\_dat a cesty\_jízdenky data. Změňte název databáze v případě potřeby. Klepněte na tlačítko **! Spustit** Chcete-li spustit skript.

6. Dále vytvořte dvě tabulky oddílů, jeden pro cestu\_dat a jiný pro cestu\_jízdného. Otevřete ukázkový skript **vytvořit\_oddíly\_table.sql**, který bude:

    - Vytvoření oddílu funkce, rozdělit data podle měsíce.
    - Vytvořte schéma oddílu mapování dat každý měsíc na různých skupina souborů.
    - Vytvořit dva oddíly tabulky namapovat schéma oddílu: **nyctaxi\_cesty** bude obsahovat cestu\_data a **nyctaxi\_tarif** bude obsahovat cestu\_jízdenky data.

    Klepněte na tlačítko **! Spustit** skript spustit a vytvořit rozdělených tabulek.

7. Ve složce **Ukázky skriptů** jsou dva ukázkové skripty prostředí PowerShell prokazujících, že paralelní hromadný importuje data do tabulky serveru SQL.

    - **bcp\_paralelní\_generic.ps1** je obecný skript pro paralelní hromadný import dat do tabulky. Upravte tento skript nastavit vstupní a cílové proměnné uvedené v řádcích komentáře ve skriptu.
    - **bcp\_paralelní\_nyctaxi.ps1** je předem nakonfigurované verze obecného skriptu a slouží k načtení tabulek dat cest Taxi NYC.  

8. Klepněte pravým tlačítkem myši **bcp\_paralelní\_nyctaxi.ps1** název skriptu a klepněte na tlačítko **Upravit** otevřete v prostředí PowerShell. Přednastavené proměnné a upravit podle název vybrané databáze, složku vstupních dat, cílové složky protokolu a cesty k vzorku formát souborů **nyctaxi_trip.xml** a **nyctaxi\_fare.xml** (k dispozici ve složce **Ukázky skriptů** ).

    ![Hromadného importu dat][16]

    Můžete také vybrat režim ověřování, výchozí ověřování systému Windows. Klepněte na zelenou šipku na panelu nástrojů spustit. Skript spustí 24 operace hromadného importu v paralelní, 12 pro každou tabulku oddílů. Průběh importu dat může sledovat otevřením složky SQL Server výchozí data výše.

9. Skriptu PowerShell hlásí počáteční a koncové časy. Když všechny hromadné dovoz kompletní, je hlášena na čase. Zkontrolujte cílovou složku protokolu ověřte, že hromadné importy byly úspěšné, tj, v cílové složce protokolu nebyly hlášeny žádné chyby.

10. Databáze je nyní připraven k průzkumu, funkce inženýrství a dalších činností podle potřeby. Vzhledem k tomu, že tabulky jsou rozděleny podle **pickup\_datetime** pole dotazy, které zahrnují **pickup\_datetime** podmínek v klauzule **WHERE** bude využívat schéma oddílu.

11. V **SQL Server Management Studio**, prozkoumat poskytnutý ukázkový skript **vzorek\_queries.sql**. Chcete-li spustit některé dotazy vzorku, označte všechny řádky dotazu klepněte na tlačítko **! Spustit** v panelu nástrojů.

12. Načtení dat cest Taxi NYC ve dvou samostatných tabulek. Ke zlepšení operace spojení, důrazně doporučujeme pro index tabulky. Ukázkový skript **vytvořit\_rozdělených\_index.sql** vytvoří indexy oddílů na klíče složeného spojení **Medailon napadení\_licence a výdej\_datum a čas**.

## <a name="dbexplore"></a>Průzkum a inženýrství funkce serveru SQL Server

V této části jsme provede průzkum a funkce generování spuštěním dotazů SQL přímo v **SQL Server Management Studio** pomocí databáze serveru SQL Server, který vytvořili dříve. Ukázkový skript s názvem **vzorek\_queries.sql** je k dispozici ve složce **Ukázky skriptů** . Upravte skript Chcete-li změnit název databáze, pokud se liší od výchozí: **TaxiNYC**.

V tomto cvičení bude provedena tato akce:

- Připojte k **Serveru SQL Server Management Studio** pomocí buď ověřování systému Windows nebo ověřování serveru SQL a SQL přihlašovací jméno a heslo.
- Seznamte se s distribucí dat několik polí v různých časových oken.
- Prověřte kvalitu údajů zeměpisné délky a šířky polí.
- Generovat na základě štítky klasifikace binárních a multiclass **tip\_částka**.
- Funkce generování a výpočetní nebo porovnat vzdálenosti cesty.
- Spojení dvou tabulek a extrahovat namátkový vzorek, který bude použit k sestavení modelů.

Jakmile budete připraveni přejít k Azure Machine Learning, můžete se buď:  

1. Uložit poslední dotaz SQL extrahovat a vzorová data a kopírovat vložit dotaz přímo do [Importu dat] [ import-data] modulu v Azure Machine Learning, nebo
2. Uchovávat vzorky a analýzou dat chcete použít pro vytvoření nové tabulky v databázi model a pomocí [Importovat Data] do nové tabulky[ import-data] modulu v Azure Machine Learning.

V této části jsme se uloží poslední dotaz extrahovat a vzorová data. Druhá metoda je znázorněn v části [průzkum a inženýrské funkce v IPython Notebook](#ipnb) .

Pro rychlou kontrolu počtu řádků a sloupců v tabulkách, které jsou naplněny dříve pomocí paralelní hromadný import

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Průzkum: Cesty distribuce Medailon

V tomto příkladu označuje Medailon (taxi čísla) s více než 100 zpráv v daném časovém období. Dotazu by měla prospěch z rozdělených tabulek aplikace access od náležitého schéma oddílu **pickup\_data a času**. Dotaz na celou dataset lze také dosáhnout pomocí oddílů tabulky nebo indexu vyhledávání.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Průzkum: Distribuční cesty Medailon a hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Posouzení kvality dat: Ověření záznamů se nesprávné délky nebo šířky

V tomto příkladu kontroluje, pokud žádné z polí délky a šířky buď obsahuje neplatnou hodnotu (radián stupňů by měla být mezi -90 a 90), nebo (0, 0) souřadnice.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Průzkum: Tipped vs. není šikmý cesty distribuce

Tento příklad vyhledá počet cest, které byly šikmý šikmý není v daném časovém období (nebo úplné sady dat, pokud pokrývající celý rok). Toto rozdělení odpovídá popisku binární distribuce má být použita později pro modelování klasifikace binárních.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Průzkum: Tip třídy nebo rozsah distribuce

Tento příklad vypočítá rozdělení oblastí tip v daném časovém období (nebo úplné sady dat, pokud pokrývající celý rok). Toto je distribuce label tříd, které budou později použity pro modelování multiclass klasifikace.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Průzkum: Vypočítat a porovnat vzdálenost cesty

Tento příklad převede odběru a odkládací délky a šířky na zeměpis SQL odkazuje, vypočítá vzdálenost cesty pomocí SQL zeměpis bodů rozdíl a vrátí náhodný vzorek pro porovnání výsledků. V příkladu omezuje výsledky na platné souřadnice pouze pomocí dotazu hodnocení kvality dat uvedených výše.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Funkce analýzy v SQL dotazů

Popisek pro generování a zeměpis převodu průzkum dotazy lze také generovat odstraněním části inventur štítků a funkce. Další funkce engineering SQL příklady jsou uvedeny v části [průzkum a inženýrské funkce v IPython Notebook](#ipnb) . Je efektivnější spustit funkci generování dotazů na celou dataset nebo rozsáhlé podmnožině pomocí SQL dotazů, které spustit přímo v instanci databáze serveru SQL Server. Dotazy mohou být provedeny v **SQL Server Management Studio**, IPython Notebook nebo jakýkoli vývoj nástroj/prostředí, které lze získat přístup k databázi místně nebo vzdáleně.

#### <a name="preparing-data-for-model-building"></a>Příprava dat pro vytváření modelů

Následující dotaz spojení **nyctaxi\_cesty** a **nyctaxi\_jízdného** tabulky, vytvoří klasifikace binárních popisek **šikmý**, popisek zařazení více tříd **tip\_třídy**a vybere namátkový vzorek 1 % od úplné sady dat spojených. Tento dotaz může zkopírován a vložen přímo v [Azure Machine Learning Studio](https://studio.azureml.net) [Importovat Data] [ import-data] modul pro požití Přímá data z instance serveru SQL Server databáze v Azure. Dotaz vyloučí záznamy s nesprávným (0, 0) souřadnice.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Průzkum a inženýrství funkce v IPython Notebook

V této části můžeme provést průzkum a funkce generování pomocí Python a SQL dotazy vůči databázi serveru SQL Server, který vytvořili dříve. IPython notebook vzorku s názvem **machine-Learning-data-science-process-sql-story.ipynb** je k dispozici ve složce **Poznámkové bloky IPython vzorku** . Tento poznámkový blok je také k dispozici na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Doporučené pořadí při práci s big daty je následující:

- Přečtěte si v malém vzorku dat do rámečku dat v paměti.
- Proveďte některé vizualizace a explorations s navzorkovanými daty.
- Experimentujte s pomocí vybraných dat inženýrství funkce.
- Pro větší průzkum, manipulaci s daty a inženýrství funkce používejte Python vydat dotazy SQL přímo proti databázi serveru SQL Server v Azure VM.
- Určit velikost vzorku, který chcete použít pro vytváření modelů Azure Machine Learning.

Jste-li připraveni přejít k Azure Machine Learning, můžete se buď:  

1. Uložit poslední dotaz SQL extrahovat a vzorová data a kopírovat vložit dotaz přímo do [Importu dat] [ import-data] modulu v Azure Machine Learning. Tato metoda je znázorněn v části [Vytváření modelů v Azure Machine Learning](#mlmodel) .    
2. Uchovávat vzorky a analýzou dat, budete používat pro vytváření nové tabulky databáze model a použít [Importovat Data] do nové tabulky[ import-data] modulu.

Následuje několik průzkum, vizualizaci dat a technické příklady funkce. Další příklady naleznete v tématu SQL IPython notebook vzorku ve složce **Poznámkové bloky IPython vzorku** .

#### <a name="initialize-database-credentials"></a>Inicializace databáze pověření

Inicializace nastavení připojení databáze v následující proměnné:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Vytvoření připojení databáze

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sestavy počet řádků a sloupců v tabulce nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Celkový počet řádků = 173179759  
- Celkový počet sloupců = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Čtení ve vzorku malé data z databáze serveru SQL Server

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Doba čtení je ukázková tabulka 6.492000 sekund  
Počet řádků a sloupců načtených = (84952, 21.)

#### <a name="descriptive-statistics"></a>Popisná statistika

Nyní jsou připraveny k prozkoumání dat vzorkování. Můžeme začít s popisnou statistiku pro prohlížení **cestu\_vzdálenost** (nebo jiný) polí:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Vizualizace: Příklad grafu

Dále se podíváme na Krabicový vzdálenost cesty k vizualizaci quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![Zobrazit #1][1]

#### <a name="visualization-distribution-plot-example"></a>Vizualizace: Příklad grafu distribuce

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Zobrazit #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Vizualizace: Pruh a čára pozemků

V tomto příkladu jsme přihrádky vzdálenost cesty do pěti přihrádek a vizualizovat binning výsledky.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Můžeme kreslit nad distribuce přihrádky v pruhu nebo čárový graf, jak je uvedeno níže

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Zobrazit #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Zobrazit #4][4]

#### <a name="visualization-scatterplot-example"></a>Vizualizace: Příklad Scatterplot

Můžeme Zobrazit zobrazovanou bodový mezi **cesty\_čas\_v\_sekund** a **cesty\_vzdálenost** Chcete-li zjistit, zda je jakékoli korelace

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Zobrazit #6][6]

Podobně můžeme zkontrolovat vztah mezi **sazba\_kód** a **cesty\_vzdálenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Zobrazit #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>Vzorkování dat v SQL

Při přípravě dat pro model budovy v [Azure Machine Learning Studio](https://studio.azureml.net), můžete rozhodnout o **dotazu SQL přímo v modulu Import dat** nebo uchovávat data analyzována a vzorky do nové tabulky, můžete [Importovat Data] [ import-data] modul s jednoduchou * *Vyberte* z < váš\_nové\_tabulka\_jméno >**.

V této části vytvoříme novou tabulku pro uložení dat vzorkování a analyzovány. Příklad přímého dotazu SQL pro vytváření modelů je k dispozici v části [průzkum a inženýrské funkce serveru SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Vytvořit vzorek, tabulky a naplnit 1 % spojených tabulek. Přetažení tabulky první, pokud existuje.

V této části jsme spojení tabulek **nyctaxi\_cesty** a **nyctaxi\_tarif**, náhodný vzorek 1 % extraktu a zachování vybraných dat v názvu nové tabulky **nyctaxi\_jeden\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Průzkum dat pomocí dotazů SQL v IPython Notebook

V této části jsme prozkoumat rozdělení dat pomocí 1 % vzorkována data, která je trvale uložen v nové tabulce, kterou jsme vytvořili výše. Všimněte si, že podobné explorations lze provést pomocí původní tabulky, můžete také omezit průzkum vzorků nebo tím, že omezí výsledky na dané časové období pomocí pomocí **TABLESAMPLE** **pickup\_datetime** oddíly, jak je znázorněno v části [průzkum a inženýrské funkce serveru SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Průzkum: Denní distribučních cest

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Průzkum: Distribuční cesty za Medailon

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Funkce generování pomocí SQL dotazů v IPython Notebook

V této části vám bude generovat nové popisky a funkce přímo pomocí SQL dotazů, pracující v tabulce 1 % jsme vytvořili v předchozím oddílu.

#### <a name="label-generation-generate-class-labels"></a>Vytváření štítků: Generovat štítky třída

V následujícím příkladu můžeme způsobit dvě sady štítků určených pro modelování:

1. Binární třídy popisky **šikmý** (odhad, pokud dostanou tip)
2. Multiclass popisky **tip\_třídy** (odhad tip přihrádky nebo rozsah)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Funkce inženýrství: Funkce počet sloupců kategorií

V tomto příkladu pole kategorií transformuje na číselné pole počet jejích výskytů dat. nahrazením jednotlivých kategorií.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Funkce inženýrství: Funkce přihrádky pro číselné sloupce

V tomto příkladu převede do oblasti přednastavených kategorií, tedy transformace číselného pole do pole s kategorií souvislé číselné pole.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Funkce inženýrství: Funkce umístění extrakt z desítkové zeměpisnou šířku a délku

V tomto příkladu rozdělí desítkové vyjádření zeměpisné šířky a délky pole do více polí oblasti různých podrobností, jako země, Město, Město, blok, atd. Všimněte si, že nová geo pole není mapováno na skutečné umístění. Informace o umístění geocode mapování viz [Služby REST mapy Bing](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Ověřte na konečné podobě tabulky featurized

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Nyní jsme připraveni přejít k vytváření modelů a modelů nasazení v [Azure Machine Learning](https://studio.azureml.net). Data jsou připravena pro všechny předpovědi problémy identifikovat dříve, a to:

1. Binární klasifikace: předpovědět, zda tip byl vyplacen výlet.

2. Multiclass klasifikace: odhadnout rozsah tip placeni podle dříve definovaných tříd.

3. Regresní úloha: odhadnout výši zaplacené cesty tip.  


## <a name="mlmodel"></a>Vytváření modelů v Azure Machine Learning

Chcete-li začít výkonu modelování přihlášení do pracovního prostoru Azure Machine Learning. Pokud jste ještě nevytvořili stroj učení pracovního prostoru, viz [vytvoření prostoru učení Azure Machine](machine-learning-create-workspace.md).

1. Začínáme s Azure Machine Learning, viz [Co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Přihlaste se k [učení Azure Machine Studio](https://studio.azureml.net).

3. Studio Domovská stránka poskytuje širokou řadu informace, videa, výukové lekce, odkazy odkaz moduly a další prostředky. Další informace o Azure Machine Learning naleznete v [Dokumentaci k učení Azure stroj](https://azure.microsoft.com/documentation/services/machine-learning/).

Experimentu typické školení se skládá z následujících:

1. **+ Nový** pokus vytvořte.
2. Získáte data k Azure Machine Learning.
3. Předběžné zpracování, transformace a zpracovávat data podle potřeby.
4. Generovat funkce podle potřeby.
5. Rozdělení dat do datových sad školení, ověření nebo testování (nebo mít samostatné datové sady pro každý).
6. Vyberte jeden nebo více algoritmů učení počítače v závislosti na studijní problém vyřešit. Například klasifikace binárních multiclass klasifikace, regrese.
7. Výuka jednoho nebo více modelů pomocí dataset školení.
8. Ověření objektu dataset pomocí vyškolených modely skóre.
9. Modely pro výpočet příslušné metriky pro učení problém vyhodnoťte.
10. Jemné vyladění modely a vyberte nejvhodnější model nasazení.

V tomto cvičení můžeme mít již prozkoumány analýzou dat na serveru SQL Server a rozhodl na velikost vzorku jedí v Azure Machine Learning. Vytvořit jeden nebo více modelů předpovědi jsme se rozhodli:

1. Získat data k Azure Machine Learning [Importovat Data] [ import-data] modulu v části **Data vstupu a výstupu** . Další informace naleznete v tématu [Import dat] [ import-data] modul referenční stránku.

    ![Importovat Data Azure Machine Learning][17]

2. Jako **zdroj dat** v panelu **Vlastnosti** vyberte možnost **Databáze SQL Azure** .

3. Do pole **název databázového serveru** zadejte název databáze DNS. Formát:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Do odpovídajícího pole zadejte **název databáze** .

5. Zadejte **uživatelské jméno SQL** v **aqccount uživatelského jména a hesla **serveru uživatelský účet heslo **.

6. Zaškrtněte políčko **přijmout jakýkoli certifikát serveru** .

7. Vložit dotaz, který extrahuje nezbytná databáze pole (včetně jakékoli vypočítaného pole jako popisky) a dolů v textové oblasti upravit **dotaz na databázi** vzorkuje data na požadovanou velikost.

V následujícím obrázku je například klasifikace binárních experimentu čtení dat přímo z databáze serveru SQL Server. Podobné experimenty lze sestavit pro klasifikaci multiclass a regresní problémy.

![Azure Machine Learning vlaku][10]

> [AZURE.IMPORTANT] Modelování dat extrakce a odběr příklady dotazu uvedeny v předchozích částech, **všechny popisky pro modelování tři cvičení jsou zahrnuty v dotazu**. (Požadováno) důležitým krokem v každém cvičení modelování je **vyloučit** zbytečné popisky pro dva problémy a jakékoli jiné **cílové nevrací**. Například při použití klasifikace binárních, použijte popisek **šikmý** a vyloučit pole **tip\_třídy**, **tip\_částka**, a **Celkem\_částka**. Jsou cílové netěsnosti neboť jejich tip zaplaceno.
>
> Vyloučit nepotřebné sloupce nebo cíl úniku informací, můžete [Vybrat sloupce v Dataset] [ select-columns] modul nebo [Upravit Metadata][edit-metadata]. Další informace naleznete v tématu [Vybrat sloupce v Dataset] [ select-columns] a [Upravit Metadata] [ edit-metadata] odkazovat na stránky.

## <a name="mldeploy"></a>Zavedení modelů v Azure Machine Learning

Pokud váš model je připraven, je lze snadno zavést jako webové služby přímo z experimentu. Další informace o nasazení Azure Machine Learning webové služby naleznete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Chcete-li nasadit novou webovou službu, je třeba:

1. Vytvořte hodnocení experimentu.
2. Nasazení webové služby.

Vytvořit hodnocení experimentu experimentu školení **Dokončeno** , klepněte na tlačítko **Vytvořit BODOVÁNÍ EXPERIMENTOVAT** v dolním panelu akcí.

![Bodování v Azure][18]

Azure Machine Learning se pokusí vytvořit hodnocení testu založené na součásti vzdělávání experimentu. Zejména bude:

1. Uložit model vyškolených a odebrat model vzdělávacích modulů.
2. Určete logické **vstupního portu** představovat schématu očekávané vstupní data.
3. Určete logické **výstupní port** pro reprezentaci schématu očekávané webové služby výstupní.

Při hodnocení experimentu jej zkontrolujte a upravte podle potřeby. Typické nastavení je nahrazovat vstupní objekt dataset nebo dotaz s jedním, což vylučuje popisek pole, tyto nebudou k dispozici při volání služby. Je také vhodné zmenšit velikost vstupní objekt dataset nebo dotazu na několik záznamů právě dost označte vstupní schématu. Výstupní port je společná pro všechny vstupní pole vyloučit a zahrnout pouze **Skóre popisky** a **Pravděpodobnost skóre** ve výstupu, [Vyberte sloupce v Dataset] pomocí[ select-columns] modulu.

Na obrázku níže je vzorek hodnocení experimentu. Až budete připraveni k nasazení, klepněte na tlačítko **Publikovat webovou službu** v dolním panelu akcí.

![Publikování Azure Machine Learning][11]

Recap v tomto výukovém programu návodu jste vytvořili prostředí Azure data science pracovali velké veřejné datové sady úplně od data pořízení modelu výcviku a nasazení webové služby Azure Machine Learning.

### <a name="license-information"></a>Informace o licenci

Tento návod vzorku a jeho doprovodná skriptů a IPython notebook(s) sdílené Microsoft pod licencí MIT. Zkontrolujte v souboru LICENSE.txt v adresáři ukázkový kód na GitHub pro další podrobnosti.

### <a name="references"></a>Odkazy

• [Cest NYC Taxi Andrés Monroy stažení stránky](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC's Taxi výlet Data podle Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
