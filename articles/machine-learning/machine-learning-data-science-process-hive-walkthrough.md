<properties
    pageTitle="Proces týmu dat pro výzkum v akci: použití Hadoop clusterů | Microsoft Azure"
    description="Použití procesu týmu dat pro výzkum pro scénáři začátku do konce využívající HDInsight Hadoop clusteru sestavovat a nasazovat modelu pomocí veřejně dostupný datovou sadu."
    services="machine-learning,hdinsight"
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
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Proces týmu dat pro výzkum v akci: použití HDInsight Hadoop clusterů

V tomto návodu používáme [Týmu dat pro výzkum obrázku (TDSP)](data-science-process-overview.md) ve scénáři začátku do konce ukládat, zkoumání a doporučení zpětnou analýzu dat z veřejně dostupný datovou sadu [NYC Taxi cest](http://www.andresmh.com/nyctaxitrips/) a dolů ukázková data pomocí služby [Azure HDInsight Hadoop obrázku](https://azure.microsoft.com/services/hdinsight/) . Modely dat jsou vytvořené pomocí Azure počítače výukové zpracovávání binární a multiclass klasifikaci a regresní prediktivní úkoly.

Návod, který ukazuje, jak řešit větší datovou sadu (1 terabajt) pro podobné situace pomocí HDInsight Hadoop clusterů pro zpracování dat najdete v článku [Týmu dat pro výzkum proces – pomocí Azure HDInsight Hadoop clusterů na datovou sadu 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Také je možné použít Poznámkový blok IPython provádění úkolů prezentovány návodu pomocí datovou sadu 1 TB. Uživatelé, kteří chtějí opakujte tento postup obraťte se na téma [návod Criteo pomocí připojení podregistru ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Popis NYC Taxi cest datové sady

Data ze služební cesty Taxi NYC o 20GB souborů komprimovaných hodnoty oddělené čárkami (CSV) (nekomprimované ~ 48GB), obsahující více než 173 miliony jednotlivé cest a tarify zaplacený při každé ze služební cesty. Každý záznam ze služební cesty obsahuje číslo licence odběru a odkládací umístění a čas, anonymních napadení (ovladač) a číslo Medailon (společnosti taxi jedinečné id). Data za rok 2013 zahrnuje všechny cesty a pro každý měsíc není uvedený v následující dvě datové sady:

1. Soubory CSV "trip_data" obsahují informace ze služební cesty, jako je třeba počet cestující odběru a dropoff body, doba trvání cesty a délka ze služební cesty. Tady je pár ukázkových záznamů:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Soubory CSV "trip_fare" obsahují podrobnosti o tarif zaplacený při každé ze služební cesty, jako je typ platby, tarif částka, přirážky daně, tipy a mýtné a celkovou částku zaplacenou. Tady je pár ukázkových záznamů:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinečný klíč připojení ze služební cesty\_dat a ze služební cesty\_tarif se skládá z polí: medailonu, napadení\_licence a odběru\_data a času.

Pokud chcete všechny podrobnosti týkající se určitého ze služební cesty, stačí připojení přes tři klíče: "medailonu", "nabourat\_licenci" a "odběru\_data a času".

Když jsme ukládány do tabulek podregistru krátce popisu některé další podrobnosti o data.

## <a name="mltasks"></a>Příklady předpovědí úkoly
Kdy blíží údaje určující druh předpovědí, které mají být na základě jeho analýzy pomáhá objasňují úkoly, které budete muset do procesu zahrnout.
Nabízíme tři scénáře předpovědí problémy, které jsme do tohoto návodu podle jehož formulace adresu *tip\_částka*:

1. **Binární klasifikace**: předpověď též tip byl zaplacené výletu, tedy *tip\_částka* , která je větší než 0 Kč je kladné příklad, zatímco *tip\_částka* 0 Kč je záporné příklad.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass klasifikace**: odhadnout oblast pro cestu vyplacené částky tip. Jsme rozdělit *tip\_částka* do pět intervalů nebo třídy:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Analytický nástroj Regrese úkolu**: odhadnout množství tip zaplacené výletu.  


## <a name="setup"></a>Nastavení obrázku HDInsight Hadoop pro pokročilé technologie pro analýzu

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

Můžete nastavit Azure prostředí pro pokročilé technologie pro analýzu, který využívá HDInsight obrázku ve třech krocích:

1. [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md): Tento úložiště účet se používá k ukládání dat v úložišti objektů Blob Azure. Data použitá v HDInsight clusterů také nachází tady.

2. [Přizpůsobení Azure HDInsight Hadoop clusterů pokročilé technologie pro analýzu procesy a technologii](machine-learning-data-science-customize-hadoop-cluster.md). Tento krok vytvoří Azure HDInsight Hadoop obrázku s 64-bit Anaconda Python 2.7 nainstalovaným všech uzlů. Existují dva kroky důležité pamatovat při úpravách svůj cluster HDInsight.

    * Upozorňujeme, že chcete vytvořit odkaz úložiště účtu v kroku 1 s svůj cluster HDInsight během jejího vytváření. Tento účet úložiště se používá pro přístup k datům, který se zpracovávají v rámci clusteru.

    * Po vytvoření clusteru povolení vzdáleného přístupu k uzlu hlavy clusteru. Přejděte na kartu **Konfigurace** a klikněte na **Povolit vzdálené**. Tento krok určuje přihlašovací údaje uživatele pro vzdálené přihlášení.

3. [Vytvořit Azure počítače výukové schůzek](machine-learning-create-workspace.md): Tento Azure počítače výukové pracovního prostoru slouží k vytvoření modelů výukové počítače. Tento úkol je určeno po dokončení počáteční průzkum a dolů použití clusteru HDInsight.

## <a name="getdata"></a>Přístup k datům z veřejných zdroje

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

Získat datovou sadu [NYC Taxi cest](http://www.andresmh.com/nyctaxitrips/) z veřejné umístění, může používáte některou z metod popsaných v [Přesunout Data mezi libovolnými úložišti objektů Blob Azure](machine-learning-data-science-move-azure-blob.md) Pokud chcete zkopírovat data do počítače.

Zde naleznete popis použití AzCopy pro přenos souborů obsahujících data. Stažení a instalace AzCopy postupujte podle pokynů na [Začínáme s nástroje příkazového řádku AzCopy](../storage/storage-use-azcopy.md).

1. Z okna příkazového řádku problému následující příkazy AzCopy nahrazení *< path_to_data_folder >* požadované cílové umístění:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Po dokončení výtisku celkem 24 zip soubory jsou ve složce data zvolili. Rozbalení stažené soubory do stejné složky na místním počítači. Poznamenejte si složku obsahující nekomprimované soubory. Tato složka označenou jako *< cesta\_k\_unzipped_data\_soubory\> * je následující.


## <a name="upload"></a>Odeslání dat do kontejneru výchozí Azure HDInsight Hadoop obrázku

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

V následujících příkazů AzCopy nahraďte následujících parametrů skutečné hodnoty, které jste zadali při vytváření clusteru Hadoop a rozbalení datové soubory.

* ***& #60; path_to_data_folder >*** v adresáři (spolu s cesta) v počítači, které obsahují rozbaleny datových souborů  
* ***& #60; název účtu úložiště Hadoop clusteru >*** úložiště účet spojený s HDInsight obrázku
* ***& #60; výchozí kontejner Hadoop obrázku >*** kontejneru výchozí používat svůj cluster. Všimněte si, že jméno container výchozí obvykle stejný název jako samotného clusteru. Pokud clusteru nazývá "abc123.azurehdinsight.net", je výchozí kontejner například abc123.
* ***& #60; klíč účtu úložiště >*** klíč účtu úložiště používat svůj cluster

Z příkazového řádku nebo v okně prostředí Windows PowerShell v počítači spusťte následující dvě AzCopy příkazy.

Tento příkaz odešle data ze služební cesty ***nyctaxitripraw*** adresáře v kontejneru výchozí Hadoop clusteru.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Tento příkaz odešle data tarif ***nyctaxifareraw*** adresáře v kontejneru výchozí Hadoop clusteru.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Data by nyní v úložišti objektů Blob Azure a je připraven k spotřebované v rámci clusteru HDInsight.

## <a name="#download-hql-files"></a>Přihlaste se do hlavního uzlu clusteru Hadoop a a připravte pro analýzu průzkumné dat

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

Hlavy uzel obrázku pro analýzu průzkumné dat a dolů odběr dat zobrazíte podle pokynů uvedených v [aplikaci Access vedoucí uzel Hadoop obrázku](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

V tomto návodu používáme primárně dotazy napsané v [podregistru](https://hive.apache.org/), v jazyce SQL profesionálové dotazu, provádět explorations předběžné údaje. Dotazy podregistru jsou uložené v souborech .hql. Jsme pak dolů ukázková data pro použití v rámci Azure počítače studium pro vytváření modelů.

Příprava clusteru analýza průzkumné dat, můžeme stahovat soubory .hql obsahující na příslušné podregistru skripty z [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) místního adresáře (C:\temp) na uzel hlavy. K tomuto účelu otevřete **Příkazový řádek** z v rámci hlavičky uzel clusteru a problémů následujících příkazů:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Tyto dvě příkazy stáhnou všechny soubory .hql potřeby v tomto návodu místního adresáře ***C:\temp & #92;*** v hlavní uzel.

## <a name="#hive-db-tables"></a>Vytvoření podregistru databázi a tabulky na oddíly podle měsíce

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

Teď připraveni vytvořit podregistru tabulky pro datovou sadu naše NYC taxi.
V hlavní uzel clusteru Hadoop otevřete ***Hadoop příkazového řádku*** na ploše hlavního uzlu a zadejte adresáři podregistru zadáním příkazu

    cd %hive_home%\bin

>[AZURE.NOTE] **Spuštění všechny příkazy podregistru v tomto návodu z výše uvedených intervalu podregistru / adresáře dotaz. To budou starat o všech problémů, cestu automaticky. Budeme používat termíny "Podregistru adresáře výzva", "podregistru Koš / adresáře výzva" a "Hadoop příkazového řádku" místo v tomto návodu.**

Podregistru adresáře na příkazový řádek zadejte tento příkaz v Hadoop příkazového řádku z hlavního uzlu můžou odeslat podregistru dotazu k vytvoření podregistru databázi a tabulky:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Tady je obsah ***C:\temp\sample\_podregistru\_vytvořit\_db\_a\_tables.hql*** soubor, který vytvoří podregistru databáze ***nyctaxidb*** a tabulek ***ze služební cesty*** a ***tarif***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Tento skript podregistru vytvoří dvě tabulky:

* "ze služební cesty" tabulka obsahuje ze služební cesty podrobnosti každé jízdní (podrobnosti, výdeje času, vzdálenost ze služební cesty a časů)
* "tarif" tabulka obsahuje podrobnosti tarif (tarif částka tip, mýtné a přirážky).

Pokud potřebujete další pomoc s těmito postupy nebo zajímat alternativní z nich, najdete v části [Odeslat podregistru dotazů přímo z Hadoop příkazového řádku ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Načtení dat do tabulky podregistru oddíly

>[AZURE.NOTE] Je to obvykle **Správce** úloh.

Datové taxi NYC má natural rozdělení podle měsíce, který používáme k povolení zpracování a dotaz bylo rychlejší. Příkazy Powershellu dole (vydaná z adresáře podregistru pomocí **Hadoop příkazového řádku**) načíst data do tabulky podregistru "ze služební cesty" a "tarif" oddíly podle měsíce.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

*Vzorku\_podregistru\_načíst\_dat\_tak, že\_partitions.hql* soubor obsahuje následující příkazy **NAČÍST** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Všimněte si, že počet dotazů podregistru, který používáme tady v procesu průzkum zahrnují hledání na pouze jeden oddíl nebo jenom pár oddílů. Ale tyto dotazy může proběhnout přes celou data.

### <a name="#show-db"></a>Zobrazení databáze v HDInsight Hadoop obrázku

Zobrazit databáze vytvořené v HDInsight Hadoop obrázku do okna příkazového řádku Hadoop, spusťte tento příkaz v Hadoop příkazového řádku:

    hive -e "show databases;"

### <a name="#show-tables"></a>Zobrazit podregistru tabulky v databázi nyctaxidb

Zobrazit tabulek v databázi nyctaxidb, spusťte tento příkaz v Hadoop příkazového řádku:

    hive -e "show tables in nyctaxidb;"

Můžete si ověřujeme, že tabulky jsou rozděleny pomocí příkazu níže:

    hive -e "show partitions nyctaxidb.trip;"

Očekávaný výstup je uveden níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Podobně můžeme zajistit, že v tabulce tarif oddíly pomocí příkazu níže:

    hive -e "show partitions nyctaxidb.fare;"

Očekávaný výstup je uveden níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Průzkum a inženýrské funkce v podregistru

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Průzkum a funkce konstrukce úkoly pro data načtená tabulky podregistru lze provést pomocí podregistru dotazů. Tady jsou příklady těchto úkolů, že si projdeme v této části:

- Zobrazení 10 hlavních záznamů v obou tabulkách.
- Zkoumání dat distribuce několika polí v různých systému windows.
- Prozkoumejte kvality dat polí šířky a délky.
- Binární a multiclass klasifikace popisky na základě generovat **tip\_částka**.
- Funkce generovat výpočtem vzdálenosti přímo ze služební cesty.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Průzkum: Zobrazení prvních 10 záznamů v tabulce ze služební cesty

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Se pokud chcete podívat, jak vypadá data, doporučujeme zabývat 10 záznamy ze všech tabulek. Spusťte následujících dvou dotazů samostatně z příkazového řádku podregistru adresáře v konzole Hadoop příkazového řádku kontrolovat záznamy.

Chcete-li získat prvních 10 záznamy v tabulce "ze služební cesty" od prvního měsíce:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Chcete-li získat horní 10 záznamů v tabulce "tarif" od prvního měsíce:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Je často užitečné záznamy uložíte do souboru pro pohodlný zobrazení. Změna malé výše uvedený dotaz se musí splnit takto:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Průzkum: Zobrazení počtu zobrazených záznamů ve všech 12 oddíly

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Potřebné takhle počet cest liší během kalendářní rok. Seskupení podle měsíce umožňuje najdete v článku jak vypadá rozdělení cest.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Tím dostaneme výstup:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Tady první sloupec představuje měsíce a druhý je počet cesty pro tento měsíc.

Celkový počet záznamů jsme také můžete spočítat v naší uvedenou množinu dat ze služební cesty zadáním následujícího příkazu do příkazového řádku podregistru adresář.

    hive -e "select count(*) from nyctaxidb.trip;"

To dává:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Pomocí příkazů podobají profilům vidíte uvedenou množinu dat ze služební cesty, doporučujeme zadejte podregistru dotazů z příkazového řádku podregistru adresářů pro uvedenou množinu dat tarif ověřuje počtu zobrazených záznamů.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Tím dostaneme výstup:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Všimněte si, že pro obě množiny dat je vrácena přesné stejný počet cest za měsíc. Díky tomu první ověření správně načtení dat.

Počítání celkový počet záznamů v uvedenou množinu dat tarif lze provést pomocí příkazu pod z příkazového řádku adresáře podregistru:

    hive -e "select count(*) from nyctaxidb.fare;"

To dává:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Celkový počet záznamů v obou tabulkách je také stejné. Díky tomu druhý ověření správně načtení dat.

### <a name="exploration-trip-distribution-by-medallion"></a>Průzkum: Distribuce ze služební cesty Medailon

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

V tomto příkladu označuje Medailon (taxi čísla) s víc než 100 cesty v rámci daného období. Výhody dotaz z Accessu rozdělený tabulky od stabilizuje proměnné oddíl **měsíc**. Výsledky dotazu jsou aby došlo k zápisu queryoutput.tsv místní soubor v `C:\temp` na uzel hlavy.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Tady je obsah *vzorku\_podregistru\_ze služební cesty\_počet\_tak, že\_medallion.hql* souboru ke kontrole.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medailon množiny dat taxi NYC Určuje jedinečný cab. Jsme můžete určit, které kabinách jsou "něčem pracuje" požádá, které z nich provedené větší než počet cesty v určitém časovém období. V následujícím příkladu označuje kabinách, které udělali více než 100 cesty v první tři měsíce a uloží výsledků dotazu do místního souboru C:\temp\queryoutput.tsv.

Tady je obsah *vzorku\_podregistru\_ze služební cesty\_počet\_tak, že\_medallion.hql* souboru ke kontrole.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Z adresáře řádku podregistru problému následující příkaz:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Průzkum: Rozdělení ze služební cesty Medailon a hack_license

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Při prohlížení datovou sadu, často chcete prozkoumat počet výskytů co skupin hodnot. V této části obsahují příklad toho, jak to udělat u kabinách a ovladače.

*Vzorku\_podregistru\_ze služební cesty\_počet\_tak, že\_Medailon\_license.hql* soubor seskupuje uvedenou množinu dat tarif na "Medailon" a "hack_license" a vrátí počty každou kombinaci. Tady jsou jeho obsah.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Dotaz vrátí cab a ovladače kombinace seřazené podle sestupně počet cest.

Z adresáře řádku podregistru spuštění:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Aby došlo k místní soubor C:\temp\queryoutput.tsv jsou zápisu výsledků dotazu.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Průzkum: Hodnocení kvality dat kontrolou neplatné délky/šířky záznamů

>[AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Společné cíle analýza průzkumné dat je odstraňování plevele článku neplatný nebo špatné záznamy. Příklad v této části určuje, zda délky nebo šířky pole obsahovat hodnotu úplně mimo oblast NYC. Protože je pravděpodobné, že tyto záznamy obsahují hodnoty chybné délky šířky, chceme odstranění z všechna data, která má být použita pro modelování.

Tady je obsah *vzorku\_podregistru\_kvality\_assessment.hql* souboru ke kontrole.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Z adresáře řádku podregistru spuštění:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Argument *– Ne* součástí tohoto příkazu potlačí obrazovky výtisk stavu úloh podregistru mapování nebo zmenšit. To je užitečné, protože ji obrazovky tisku z výstupu dotazu podregistru čitelnější.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Průzkum: Rozdělení binární třídy tipy ze služební cesty

> [AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Binární klasifikace problém uvedené v části [Příklady předpovědí úkoly](machine-learning-data-science-process-hive-walkthrough.md#mltasks) je vhodné vědět, jestli byl zadán tip nebo ne. Toto rozdělení tipy hodnotu binární:

* Tip zadaných (třídy 1, tip\_částka > 0 Kč)  
* tip (třídy 0, tip\_částka = 0 Kč).

*Vzorku\_podregistru\_vysypávány\_frequencies.hql* soubor ukázáno v následujícím příkladu dělá.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Z adresáře řádku podregistru spuštění:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Průzkum: Třídy rozdělení v multiclass nastavení

> [AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Problém multiclass klasifikace uvedené v části [příkladů úloh prediktivní vkládání](machine-learning-data-science-process-hive-walkthrough.md#mltasks) této sadě dat taky různě natural klasifikace kde rádi předpovídání výše uvedených tipů zadaných. Můžeme definování názvů tipu v dotazu použít intervalů. Distribuce třídy pro různé tip oblastí získáte používáme *vzorku\_podregistru\_tip\_oblast\_frequencies.hql* soubor. Tady jsou jeho obsah.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Z konzoly Hadoop příkazovém řádku spusťte tento příkaz:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Průzkum: Výpočet přímé vzdálenost mezi dvěma délky šířky umístění

> [AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

S míru přímé vzdálenosti umožňuje zjistit rozdíl mezi tak vzdálenost skutečné ze služební cesty. Jsme jak motivovat, jak tuto funkci tak, že poukázání na osobní může pravděpodobně nižší Tip: Pokud se zjistit, že ovladač úmyslně je přijaly déle cestou, aby.

Srovnání vzdálenost skutečné ze služební cesty a [Haversine vzdálenost](http://en.wikipedia.org/wiki/Haversine_formula) mezi dvěma body šířky délky ("výborně kruh" vzdálenost) zobrazíte používáme trigonometrické k dispozici v rámci podregistru, takto:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

V nahoře query R je poloměr Zemitých v mil a pí se převede na radiány. Všimněte si, že body délky šířky jsou "filtrovaný" odebrat hodnoty, které jsou vzdáleném od oblasti NYC.

V tomto případě jsme naše výsledky k zápisu adresář s názvem "queryoutputdir". Posloupnost příkazů ukázáno v následujícím příkladu nejprve vytvoří tento výstupní adresář a potom spustí příkaz podregistru.

Z adresáře řádku podregistru spuštění:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Výsledky dotazu jsou aby došlo k zápisu 9 objektů BLOB Azure ***queryoutputdir/000000\_0*** k ***queryoutputdir/000008\_0*** v části výchozí kontejner Hadoop obrázku.

Velikost jednotlivé objekty BLOB zobrazíte jsme z příkazového řádku adresáře podregistru spusťte tento příkaz:

    hdfs dfs -ls wasb:///queryoutputdir

Zobrazte obsah souboru dané, vyslovte 000000\_0, používáme společnosti Hadoop `copyToLocal` příkaz takto.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`může být pomalé velmi velké soubory a se nedoporučuje pro použití s nimi.  

Klíčové výhodou s tato data uložena v objektů blob Azure je to, že společnost Microsoft může zkoumání dat v rámci studium počítače Azure pomocí [Importovat Data] [ import-data] modul.


## <a name="#downsample"></a>Dolů ukázkových dat a sestavení modelů na verzi Azure počítač přečíst

> [AZURE.NOTE] Toto je obvykle **Vědecký pracovník Data** úkolu.

Za fázi analýzy průzkumné dat jsme jste připraveni dolů ukázkových dat pro vytváření modelů Azure počítač přečíst. V této části, ukážeme, jak pomocí dotazu podregistru dolů ukázková data, která je pak zobrazená po kliknutí na [Importovat Data] [ import-data] modul Azure počítač přečíst.

### <a name="down-sampling-the-data"></a>Dolů odběr data

Existují dva kroky v tomto postupu popsaný dál. Nejdřív jsme spojení tabulek **nyctaxidb.trip** a **nyctaxidb.fare** na tři klíče, které jsou k dispozici ve všech záznamech: "medailonu", "nabourat\_licenci", a "odběru\_data a času". Potom generovat, binární klasifikace popisek, **vysypávány** a popisku více třídy klasifikace **tip\_třídy**.

Nebudou moct používat dolů vzorky dat přímo z [Importu dat] [ import-data] modul Azure počítač přečíst, je nutné uložit výsledky výše uvedený dotaz na tabulku podregistru vnitřní. V následující vytvořit vnitřní tabulku podregistru jsme naplní její obsah s spojených a dolů vybraných dat.

Dotaz se týká standardních funkcí podregistru přímo do generovat hodinu dne, týdne roku den v týdnu (1 zastupuje pondělí a 7 zastupuje neděle) z "odběru\_data a času" pole a přímé vzdálenost mezi odběru a dropoff umístění. Uživatelé mohou odkazovat na [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) úplný seznam těchto funkcí.

Dotaz pak dolů vzorky data tak, aby výsledků dotazu můžete zapadají do celkového Studio výukové Azure počítače. Jenom o 1 % původní sady dat se importuje do Studio.

Tady jsou obsah *vzorku\_podregistru\_Příprava\_pro\_aml\_full.hql* soubor, který připraví datového modelu vytváření Azure počítač přečíst.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Spusťte tento dotaz z příkazového řádku adresáře podregistru:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Nyní je vnitřní tabulku "nyctaxidb.nyctaxi_downsampled_dataset", které jsou přístupné z [Importu dat] [ import-data] modul z Azure počítače výukové. Navíc může použijeme tento datovou sadu pro vytváření modely výukové počítače.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Pomocí modulu importovat Data ve Azure počítače výukové otevřete dolů vybraných dat

Jako požadavky pro vydávání podregistru dotazy v okně [Importovat Data] [ import-data] modul Azure počítače učení potřebujeme přístup k Azure počítači výukové pracovní prostor a přístup k přihlašovací údaje clusteru a jeho přidružený úložiště účtu.

Některé informace o [Importu dat] [ import-data] modul a parametrech zadat:

**Identifikátor URI HCatalog serveru**: Jestliže název clusteru je abc123, bude to jednoduše: https://abc123.azurehdinsight.net

**Název účtu uživatele Hadoop** : uživatelské jméno pro cluster (**Ne** vzdáleného přístupu uživatelské jméno)

**Hadoop uživatelského účtu heslo** : heslo pro cluster (**Ne** heslo vzdáleného přístupu)

**Umístění výstupní data** : to je vybrán být Azure.

**Název účtu úložiště Azure** : název účtu úložiště výchozí přidružené clusteru.

**Název Azure kontejneru** : to je výchozí název kontejneru clusteru a obvykle se shoduje s názvem obrázku. U obrázku s názvem "abc123" je to jenom abc123.

> [AZURE.IMPORTANT] **Všechny tabulky přejeme k vytvoření dotazu s [Importovat Data] [ import-data] modul Azure počítač přečíst musí být vnitřní tabulku.** Jeden tip pro zjištění, zda je tabulka T v databázi D.db vnitřní tabulka vypadá takto.

Z adresáře řádku podregistru problému příkazu:

    hdfs dfs -ls wasb:///D.db/T

Pokud je tabulka vnitřní tabulku a je vyplněné, musí tady zobrazit jeho obsah. Další způsob, jak zjistit, zda tabulky je vnitřní tabulku je pomocí Průzkumníka úložišť Azure. Přejděte na jméno container výchozí clusteru a potom filtrovat podle názvu tabulky pomocí ho. Pokud v tabulce a její obsah zobrazena, potvrdíte, že je vnitřní tabulku.

Tady je snímek podregistru dotazu a [Importovat Data] [ import-data] modulu:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Poznámka: od vybraných dat je umístěn v kontejneru výchozí naše dolů, výsledný dotaz podregistru z Azure počítače výukové je velmi jednoduché a je právě "vyberte * z nyctaxidb.nyctaxi\_převzorkovány\_dat".

Datové nyní lze jako výchozí bod pro vytváření modelů výukové počítače.

### <a name="mlmodel"></a>Vytváření modelů Azure počítač přečíst

Teď schopni přejít k vytváření modelů a nasazení modelu [Azure počítač](https://studio.azureml.net)přečíst. Data je připraven k nám chcete použít v řešení problémů předpovědí výše uvedený:

**1. binární klasifikace**: odhadnout též tip byl zaplacené výletu.

**Student používá:** Regresní logistickou dvě třídy

na. Tento problém naše popisek cílové (nebo třídy) je "vysypávány". Náš původní datovou sadu odběr dolů má několik sloupců, které jsou cílové nevrací pro tento klasifikace testu. Zejména: tip\_třídy, tip\_množství a součet\_částka zobrazit informace o cílové popisek, který není k dispozici na testování čas. Odebrání tyto sloupce v úvahu pomocí [Vybrat sloupce v sadě dat] [ select-columns] modul.

Následující snímek zobrazuje naše pokus předpovídání, zda byla tip zaplacené dané ze služební cesty.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Pro tento experiment byly naše distribuce popisek cílové zhruba 1:1.

Následující snímek zobrazuje distribuci tip třídy popisky na binární klasifikace problém.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

V důsledku toho získáváme AUC 0.987, jak je znázorněno na následujícím obrázku.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass klasifikace**: odhadnout oblast pro ze služební cesty pomocí předdefinovaných tříd vyplacené částky tip.

**Student používá:** Multiclass logistickou regresní

na. Tento problém naše popisek cílové (nebo třídy) je "tip\_předmětu" které proveďte jednu z pěti hodnoty (0,1,2,3,4). Stejně jako v případě binární klasifikace máme pár sloupce, které jsou cílové nevrací pro tento testu. Zejména: vysypávány, tip\_celková částka\_částka zobrazit informace o cílové popisek, který není k dispozici na testování čas. Odebrání tyto sloupce pomocí [Vybrat sloupce v sadě dat] [ select-columns] modul.

Následující snímek zobrazuje naše pokus předpovídání, ve které koš tip se zobrazí je pravděpodobně spadají (třídy 0: tip = 0 Kč, třídy 1: tip > 0 Kč a tip < = $5, třídy 2: tip > 5 a tip < = 10, třídy 3: tip > 10 a Nápověda < = 20, předmětu 4: tip > 20 USD)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Teď zobrazí vypadá naše skutečné test třídy rozdělení. Vidíme, že jsou rozšířené třídy 0 a 1 předmětu, jiných tříd jsou méně častých.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Pro tento experiment používáme matici zmatky podívejte se na našeho přesnostmi předpovědí. Zobrazí se pod.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Všimněte si, že při úplně dobré naše přesnostmi třídy na rozšířené třídy modelu nedělá výborně "výukové" na vzácnějších třídy.


**3. regresní úkolu**: odhadnout částka zaplacená za výletu tip.

**Student používá:** Zesílen rozhodovacího stromu

na. Tento problém naše popisek cílové (nebo třídy) je "tip\_množství". V tomto případě se nevrací naše cílové: vysypávány, tip\_předmětu, celkové\_částka; Tyto proměnné zobrazit informace o velikosti tip, který obvykle není k dispozici v testování čas. Odebrání tyto sloupce pomocí [Vybrat sloupce v sadě dat] [ select-columns] modul.

Snímek belows zobrazuje naše pokus předpovídání množství dané tip.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regresní potíže jsme změřit přesnostmi naše předpovědí prohlížíte čtverců chyby v předpovědí, koeficient určení a podobně. Ukážeme tyto dole.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Vidíme, že o koeficient určení je 0.709 zdání o 71 % rozptyl si vysvětlíme naše koeficienty modelu.

> [AZURE.IMPORTANT] Další informace o výukové Azure počítače a jak si zobrazit a použít ho, získáte [Co je to studijní počítače?](machine-learning-what-is-machine-learning.md). Je velmi užitečné zdroj k přehrávání s spoustu počítače výukové pokusy na výukové počítače Azure [Cortana Intelligence Galerie](https://gallery.cortanaintelligence.com/). V galerii zahrnuje rozsah pokusy a poskytuje důkladné Úvod do oblasti možnosti Azure počítače učení.

## <a name="license-information"></a>Informace o licenci

Tento návod vzorku a doprovodnou skripty jsou sdíleny Microsoft klikněte v části licence MIT. Zkontrolujte soubor LICENSE.txt v adresáři ukázek kódu na GitHub další podrobnosti.

## <a name="references"></a>Odkazy

• [Andrés Monroy NYC Taxi cest stáhnout stránky](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing společnosti NYC Taxi ze služební cesty Data podle Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi a Limousine provize výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
