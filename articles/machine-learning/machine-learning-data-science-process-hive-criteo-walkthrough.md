<properties
    pageTitle="Proces týmu dat pro výzkum v akci: pomocí HDInsight Hadoop clusterů datovou sadu Criteo 1 TB | Microsoft Azure"
    description="Použití procesu týmu dat pro výzkum pro scénáři začátku do konce využívající HDInsight Hadoop clusteru sestavovat a nasazovat modelu pomocí velké datovou sadu veřejně dostupný (1 TB)"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Proces týmu dat pro výzkum v akci – pomocí Azure HDInsight Hadoop clusterů na datovou sadu 1 TB

V tomto návodu jsme ukazují použití procesu týmu dat pro výzkum ve scénáři začátku do konce s služby [Azure HDInsight Hadoop clusteru](https://azure.microsoft.com/services/hdinsight/) ukládat, průzkum, funkce zpětnou analýzu, a dolů ukázková data z jednoho z veřejně dostupný [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datové sady. Výukové počítače Azure používáme k vytvoření modelu binární klasifikace na tato data. Také ukážeme, jak publikovat jednu z těchto modelů jako webové služby.

Také je možné použít Poznámkový blok IPython provádění úkolů prezentovány v tomto návodu. Uživatelé, kteří chtějí opakujte tento postup obraťte téma [návod Criteo pomocí připojení podregistru ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Popis datovou sadu Criteo

Criteo, ke kterému se dat klikněte na datovou sadu předpovědí, která je přibližně 370 gzip komprimovány TSV souborů (~1.3TB nekomprimované), obsahující více než 4.3 miliard záznamy. Dostali z 24 dnů klikněte na data k dispozici [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Pro zjednodušení dat vědeckých jsme mít rozbaleny nejsou k dispozici pro nás experiment s data.

Každý záznam v této sadě dat obsahuje 40 sloupce:

- první sloupec představuje popisek sloupce, který označuje, zda uživatel klikne **Přidat** (s hodnotou 1) nebo není klepněte na jednu (hodnota 0)
- Další 13 sloupců jsou číselné, a
- poslední 26 jsou kategorií sloupce

Sloupce se anonymních a se používá řada výčtu názvy: "Sloupec1" (ve sloupci popisek) na "Col40" (pro poslední sloupec kategorií).            

Tady je výpisem prvních 20 sloupců dvěma pozorování (řádků) z této sadě dat:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

V této sadě dat jsou chybějících hodnot ve sloupcích číselné a kategorií. Popis jednoduchý způsob zpracování chybějících hodnot. Další podrobnosti o data jsou prozkoumat při jsme ukládány do podregistru tabulek.

**Definice:** *Kliknutí na reklamu sazba (PEV.cenu):* Jedná se o procento klepnutími v okně data. V této sadě dat Criteo PEV.cenu je o 3.3 % nebo 0.033.

## <a name="mltasks"></a>Příklady předpovědí úkoly
Dva problémy předpovědí ukázkových popsaných v tomto návodu:

1. **Binární klasifikace**: odhadne, zda může uživatel kliknutí na Přidat:
    - Skupina 0: Žádné kliknutím
    - Třídy 1: klikněte na

2. **Analytický nástroj Regrese**: předpovídá hodnotu pravděpodobnosti ad klikněte na z uživatelskými funkcemi.


## <a name="setup"></a>Nastavte si HDInsight Hadoop obrázku pro výzkum dat

**Poznámka:** Je to obvykle **Správce** úloh.

Nastavení prostředí vědy dat Azure pro vytváření prediktivní analýzy řešení s HDInsight clusterů ve třech krocích:

1. [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md): Tento úložiště účet se používá k ukládání dat v úložišti objektů Blob Azure. Data použitá v HDInsight clusterů je uložený tady.

2. [Přizpůsobení Azure HDInsight Hadoop clusterů dat pro výzkum](machine-learning-data-science-customize-hadoop-cluster.md): Tento krok vytvoří clusteru služby Azure HDInsight Hadoop s 64-bit Anaconda Python 2.7 ve všech uzlech nainstalováno. Existují dva důležitých kroků (popsaných v tomto tématu) dokončete přizpůsobování clusteru HDInsight.

    * Je nutné propojit vytvořili v kroku 1 s svůj cluster HDInsight při vytvoření účtu úložiště. Tento účet úložiště se používá pro přístup k datům, která lze vykonat v rámci clusteru.

    * Po vytvoření otevřela, musíte povolit vzdáleného přístupu hlavy uzel clusteru. Mějte na paměti vzdáleného přístupu přihlašovací údaje zadané tady (jiné než určeným pro clusteru v jeho vytvoření): je k dokončení těchto postupů budete potřebovat.

3. [Vytvoření pracovního prostoru Azure ML](machine-learning-create-workspace.md): Tento Azure počítače výukové pracovního prostoru slouží k vytváření modelů výukové počítače po počáteční průzkum a dolů odběr clusteru HDInsight.

## <a name="getdata"></a>Získání a používání dat z veřejné zdroje

Datovou sadu [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) můžete k nim získat přístup tak, že kliknete na odkaz, přijetí podmínky použití a názvem. Snímek obrazovky, jak to vypadá je zobrazena zde:

![Přijměte podmínky Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Kliknutím na **pokračovat a stáhnout** Další informace o sadě dat a jeho dostupnost.

Data uložena v na veřejné místo [úložiště objektů blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" odkazuje na umístění v úložišti objektů Blob Azure. 

1. Data v této úložiště objektů blob veřejné obsahují tři podsložek rozbaleny data.

    1. Podsložce *jako nezpracovaná nebo počet/* obsahuje první 21 dnů od data – z dne\_00 den\_20
    2. Podsložce *nezpracovanými/vlaku/* obsahuje jeden den dat, den\_21
    3. Podsložce *jako nezpracovaná nebo zkoušení/* se skládá ze dvou dnů od data, den\_22 a dne\_23

2. Uživatelům, kteří chcete začít s nezpracovanými gzip daty, Tyhle také k dispozici ve složce hlavní *nezpracovanými /* jako day_NN.gz, kde NN přejde od 00 do 23.

Alternativní přístup k přístupu, prozkoumat a model, který tato data, který nevyžaduje místní stahování je vysvětleno dále v tomto návodu při vytvoření podregistru tabulek.

## <a name="login"></a>Přihlaste se k headnode obrázku

Přihlaste se k headnode clusteru, můžete [Azure portál](https://ms.portal.azure.com) vyhledejte clusteru. Klikněte na ikonu Slon HDInsight na levé straně a potom poklikejte na název svého obrázku. Přejděte na kartu **Konfigurace** , poklikejte na ikonu připojit v dolní části stránky a zadejte svoje přihlašovací údaje vzdáleného přístupu po zobrazení výzvy. Tím přejdete do headnode clusteru.

Takto vypadá typické první přihlášení k headnode obrázku:

![Přihlaste se k clusteru](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


V levé části vidíme "Hadoop příkazového řádku", což je naše Centrem pro průzkum data. Také vidíme dva užitečné adresy URL - "Hadoop vláken stav" a "Hadoop název uzel". Adresa URL stavu vláken zobrazuje průběh projektu a název URL uzel uvádí podrobnosti o konfiguraci clusteru.

Teď můžeme jsou nastavené a začněte první část návodu: průzkum dat pomocí podregistru a příprava dat Azure počítače výukové.

## <a name="hive-db-tables"></a>Vytvoření podregistru databázi a tabulky

Vytvoření tabulky podregistru pro naše Criteo datovou sadu, otevřete ***Hadoop příkazového řádku*** na ploše hlavního uzlu a zadejte adresáři podregistru zadáním příkazu

    cd %hive_home%\bin

>[AZURE.NOTE] Spuštění všechny příkazy podregistru v tomto návodu z koše podregistru / adresáře dotaz. To má na starosti všech problémů, cestu automaticky. Budeme používat termíny "Podregistru adresáře výzva", "podregistru Koš / adresáře výzva" a "Hadoop příkazového řádku" jako synonyma.

>[AZURE.NOTE]  K provedení všech podregistru dotazu, jednu vždy použít následující příkazy:

        cd %hive_home%\bin
        hive

Až se zobrazí REPL podregistru s "podregistru >"odhlásit, jednoduše vyjmout a vložit dotazu k provedení ho.

Následující kód vytvoří databázi "criteo" a vygeneruje 4 tabulky:


* *Tabulka pro generování počty* vytvořenou dnů den\_00 den\_20,
* *tabulku pro použití jako datovou sadu vlaku* vytvořenou v den\_21 a
* dvě *tabulky použít jako datové sady test* založená na den\_22 a dne\_23 v tomto pořadí.

Jsme můžete rozdělit naše datovou sadu test dva různé tabulky protože dny reprodukujte svátek a chcete zjistit, pokud model zjistit rozdíly mezi není svátků a dovolené rychlosti kliknutí na reklamu.

Skript [vzorku #95; podregistru & #95; vytvořit & #95; criteo & #95; #95; & databáze a & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) zde se zobrazí pro usnadnění:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Jsme Všimněte si, že v této tabulce externí jak můžeme jednoduše přejděte na umístění v úložišti objektů Blob Azure (wasb).

**Existují dva způsoby provádět jakékoli podregistru dotaz, který teď zmínit.**

1. **Pomocí příkazového řádku podregistru REPL**: prvním je problém "podregistru" příkaz a kopírovat a vložit dotazu na podregistru REPL příkazového řádku. K tomuto účelu dělat:

        cd %hive_home%\bin
        hive

    Teď na příkazového řádku REPL vyjmutí a vložení dotaz spustí jej.

2. **Ukládání dotazů do souboru a spuštění příkazu**: druhý je uložit dotazy do souboru .hql ([vzorku #95; podregistru & #95; vytvořit & #95; criteo & #95; #95; & databáze a & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) a potom tento příkaz následujících situací:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Potvrďte vytváření databázi a tabulky

Dále si ověřujeme vytvoření databáze pomocí následujícího příkazu z koše podregistru / adresáře tento dotaz:

        hive -e "show databases;"

To vám:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Vytvoření nové databáze "criteo" potvrdíte.

Jaké tabulky jsme vytvořili zobrazíte můžeme jednoduše problémů příkazu tady z koše podregistru / adresáře tento dotaz:

        hive -e "show tables in criteo;"

Potom vidíme výstupu takto:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Průzkum dat v podregistru

Teď můžeme připraveni proveďte některé základní průzkum v podregistru. Začněte tím, že počítání příklady vlaku jsme testování tabulek dat.

### <a name="number-of-train-examples"></a>Počet vlaku příklady

Obsah [vzorku a #95; podregistru & #95; počet & #95; vlaku & #95; tabulky & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) se zobrazí tady:

        SELECT COUNT(*) FROM criteo.criteo_train;

To dává:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Můžete taky jednu taky vydat tento příkaz z koše podregistru / adresáře tento dotaz:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Počet test příklady ve dvou datových sad test

Jsme teď počtu příklady ve dvou datových sad testu. Obsah [vzorku a #95; podregistru & #95; počet & #95; criteo & #95; test & #95; den a #95; 22 & #95; tabulka & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) zde:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

To dává:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Jako obvykle jsme taky obrátit skript z koše podregistru / adresáře výzva zadáním příkazu:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Nakonec jsme prozkoumejte číslo test příklady v datové sadě test podle dne\_23.

Příkaz akce je podobná té právě zobrazené (najdete [Ukázkové & #95; podregistru & #95; počet & #95; criteo & #95; test & #95; den a #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

To vám:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Popisek rozvržení datovou sadu vlaku

Distribuce popisku v datové sadě vlaku je potřebné. Tím zobrazíte ukážeme obsah [vzorku a #95; podregistru & #95; criteo & #95; popisek & #95; rozdělení a #95; vlaku & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

To dává popisek rozdělení:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Všimněte si, že procento kladná popisky o 3.3 % (konzistentní s původním datovou sadu).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Distribuce Histogram některé číselné proměnných v datové sadě vlaku

Používáme podregistru na nativní "histogram\_číselné" (funkce) Pokud chcete zjistit, vypadá rozdělení číselné proměnné. Tady je obsah [vzorku a #95; podregistru #95; criteo & #95; histogram & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

To dává takto:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

BOČNÍ zobrazení – rozbalit kombinace v podregistru slouží k vytváření podobných SQL výstupu místo obvykle seznamu. Všimněte si, že v této tabulce v prvním sloupci odpovídá centru Koš a druhý na požadovanou frekvenci Koš.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Přibližná percentily některé číselné proměnných v datové sadě vlaku

Potřebné s číselné proměnné je také výpočtu přibližnou percentily. Podregistru je nativní "percentilu\_přibližně" to znamená pro us. Obsah [vzorku a #95; podregistru #95; criteo & #95; přibližnou & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) jsou:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

To dává:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Jsme remark, že rozdělení percentily souvisí s rozdělení histogram kterékoli číselné proměnné obvykle.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Zjistit počet jedinečných hodnot pro některé kategorií sloupce v datové sadě vlaku

Pokračování průzkum dat, jsme teď najdete pro některé kategorií sloupce počet jedinečných hodnot, které přijaly. K tomuto účelu ukážeme obsah [vzorku a #95; podregistru #95; criteo & #95; jedinečné & #95; hodnoty a #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

To dává:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Jsme Všimněte si, že Col15 obsahuje jedinečné hodnoty 19M! Použití naïve techniky jako "jedné aktivní kódování" kódování takové maximum dimenzionální kategorií proměnné je tuto obě stejné. Zejména vysvětlit a ukazují výkonné a robustní postup s názvem [Učení s počtem](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) pro efektivní řešení tohoto problému.

Ukončete Zde jsme prohlížíte počet jedinečných hodnot pro některé kategorií sloupce stejně. Obsah [vzorku a #95; podregistru #95; criteo & #95; jedinečné & #95 hodnoty a #95; více & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) jsou:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

To dává:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Znovu vidíme, s výjimkou Col20, máte všechny ostatní sloupce počet jedinečných hodnot.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Spoluvytváření výskyt spočítá párů kategorií proměnných v datové sadě vlaku

Počty spolu výskyt dvojice kategorií proměnných je také zájmu. Takto můžete zjistit pomocí kódu v [vzorku a #95; podregistru #95; criteo & #95; párových & #95; kategorií & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Zpětné počty order chronologickém jsme v tomto případě Hledat v horní 15. Tím dostaneme:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Dolů ukázkové datové sady výukové počítače Azure

Prozkoumat datové sady s prokázáno jsme může postup tohoto typu průzkum u všech proměnných (včetně kombinace), jsme teď dolů ukázkové výše uvedené množiny dat, abychom vytvářet modely Azure počítač přečíst. Odvolání, který jsme zaměřit na je to: možnost sada příklad atributů (funkce hodnoty z Sloupec2 - Col40), doporučujeme předpovídání Sloupec1 při 0 (bez kliknutím) nebo hodnotu 1 (kliknutím).

Dolů Přehrajte si naše vlaku a otestovat datové sady 1 % původní velikosti, používáme nativní funkce RAND() podregistru společnosti. Další skript, [vzorku a #95; podregistru & #95; criteo & #95; převzorkovat & #95; vlaku & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) dělá pro datovou sadu vlaku:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

To dává:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Skript [vzorku a #95; podregistru & #95; criteo & #95; převzorkovat & #95; test & #95; den a #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) znamená pro testovací data den\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

To dává:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Nakonec skript [vzorku a #95; podregistru & #95; criteo & #95; převzorkovat & #95; test & #95; den a #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) znamená pro testovací data den\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

To dává:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

S tím je připraven k použití naše dolů vybraných vlaku a otestujte datové sady pro vytváření modelů Azure počítač přečíst.

Existuje konečný důležitou součástí před jsme přesunout na výukové počítače Azure, což je pochybnosti tabulce count. V části další podřízené probereme tato témata to podrobně.

##<a name="count"></a>Stručný diskuse v tabulce počet

Jak můžeme viděli, máte několik kategorií proměnných velmi vysoké dimenzionalitu. V našem návodu jsme prezentovat výkonné postup s názvem [Učení s počtem](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) kódování tyto proměnné efektivně, robustní způsobem. Další informace o tento postup je v uvedeného odkazu.

**Poznámka:** V tomto návodu jsme zaměření se na základě počet tabulek k vytvoření kompaktní formátovaná jako maximum dimenzionální kategorií funkcí. Toto není možné jenom kódovat kategorií funkcí. Další informace o jinými postupy zúčastněných uživatelů můžete najdete v článku [jeden za běhu kódování](http://en.wikipedia.org/wiki/One-hot) a [algoritmus hash funkce](http://en.wikipedia.org/wiki/Feature_hashing).

Pro vytváření počet tabulek na počet dat, údaje použijeme v jako nezpracovaná nebo počet položek ve složce. V části modelování jsme ukažte uživatelům, jak vytvářet v této tabulce počet kategorií funkcí od začátku, případně pro účely jejich explorations předdefinovaných počet tabulky. V jaké takto při označovány "předdefinovaných počet tabulek", jsme nechtěli pomocí počet tabulek, které nabízíme. Podrobné informace o tom, jak získat přístup k v této tabulce jsou uvedeny v následující části.

## <a name="aml"></a>Vytvoření modelu s výukové počítače Azure

Náš modelu procesu dozvědět počítače Azure vytváření takto:

1. [Přístup k datům z tabulek podregistru do výukové počítače Azure](#step1)
2. [Vytvoření testu: Vyčistit data, vyberte Student a featurize s počet tabulek](#step2)
3. [Školení modelu](#step3)
4. [Skóre modelu na testovací data](#step4)
5. [Vyhodnocení modelu](#step5)
6. [Publikování modelu jako webové služby na být spotřebované množství](#step6)

Nyní jsme připraveni k vytvoření modelů v Azure počítače výukové studio. Náš dolů vybraných dat se uloží jako podregistru tabulky v clusteru. Zobrazíte tyto údaje použijeme modul Azure počítače výukové **Importovat Data** . Přihlašovací údaje pro přístup k účtu úložiště tohoto clusteru jsou uvedeny v následující.

### <a name="step1"></a>Krok 1: Získání dat z tabulek podregistru do Azure počítače učení pomocí modulu importovat Data a vyberte pro počítač výukové testu

Začněte tím, že vyberete **+ Nový** -> **EXPERIMENT** -> **Prázdné testu**. Z **vyhledávací** pole na levé horní, vyhledejte "Importovat Data". Přetažení modulu **Importovat Data** k plátno experiment (prostřední části okna) použití modulu pro přístup k datům.

Je to, jak **Importovat Data** vypadá při načítání dat z tabulku podregistru:

![Import dat získává data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Modulu **Importovat Data** hodnoty parametrů, které jsou k dispozici v obrázku příkladů jenom seřadit hodnoty, které musíte zadat. Tady je několik obecné pokyny o tom, jak vyplňovat uživatelé parametr nastaven pro modul **Importovat Data** .

1. Zvolte "Podregistru dotaz" pro **Zdroj dat**
2. Do pole **podregistru databázových dotazů** jednoduchý příkaz SELECT * FROM < vaše\_databáze\_name.your\_tabulky\_název > –, že se.
3. **Identifikátor URI Hcatalog serveru**: Jestliže clusteru je "abc", bude to jednoduše: https://abc.azurehdinsight.net
4. **Název účtu uživatele Hadoop**: uživatelské jméno zvolili v době uvedení do provozu obrázku. (Není vzdáleného přístupu uživatelské jméno!)
5. **Hadoop heslo**: heslo pro vybrané v době uvedení do provozu clusteru uživatelské jméno. (Není heslo vzdáleného přístupu!)
6. **Umístění výstupní data**: Zvolte "Azure"
7. **Název účtu úložiště Azure**: úložiště účet spojený s clusteru
8. **Klíč účtu Azure úložiště**: klíč účtu úložiště přidružené clusteru.
9. **Název Azure kontejneru**: Pokud název clusteru je "abc", je to jednoduše "abc", obvykle.


Po **Importu dat** dokončení získávání dat (zelené značky na uvidíte modulu) uložte tato data jako datovou sadu (s názvem podle svého výběru). Jak to vypadá:

![Import dat uložení dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Klikněte pravým tlačítkem myši na výstupní port modulu **Importovat Data** . Tím zobrazíte možnost **uložení jako sady dat** a možnost **vizualizace** . Možnost **vizualizace** kliknutí na zobrazí 100 řádků dat spolu s pravém panelu, který je užitečné pro některé Přehled. Uložení dat, jednoduše vyberte **Uložit jako datovou sadu** a postupujte podle pokynů.

Vyberte uložený datovou sadu pro použití ve počítače výukové experiment, vyhledejte datové sady pomocí **vyhledávacího** pole vidíte na následujícím obrázku. Jednoduše napište název dala datovou sadu částečně chcete přejít na ho a přetáhněte datovou sadu na hlavní panel. Uvolnění na hlavním panelu vybere pro použití v počítači výukové modelování.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] To udělejte pro vlaku a test datové sady. Nezapomeňte také, použijte název databáze a názvy tabulek, které jste zadali k tomuto účelu. Použít obrázek hodnoty jsou pouze pro obrázek purposes.* *

### <a name="step2"></a>Krok 2: Vytvoření jednoduchého experiment Azure počítač přečíst odhadnout kliknutími / bez kliknutí

Náš Azure ML experiment vypadat takto:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Jsme teď zkontrolujte klíčové součástí tohoto testu. Připomínáme potřebujeme přetáhněte naše uložené vlaku a otestujte nejdřív datové sady k naše experiment plátno.

#### <a name="clean-missing-data"></a>Vyčistit chybějící Data

Modul **Vyčistit chybějící Data** znamená naznačuje jeho název: vyčistí chybějící data způsobem, který může být zadané uživatelem. Hledání na tento modul, vidíme takto:

![Vyčistit chybějící data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Tady zvolili jsme chcete nahradit všechny chybějící hodnoty 0. Existují další možnosti, které uvidí prohlížíte rozevíracích seznamů v modulu.

#### <a name="feature-engineering-on-the-data"></a>Inženýrské funkce na kartě data

Může být miliony jedinečné hodnoty některých kategorií funkcí velkých datových sad. Pomocí metody naïve například za běhu jeden kódování představující maximum dimenzionální kategorií funkcí, je úplně tuto obě stejné. V tomto návodu jsme demonstrují použití funkce count pomocí předdefinovaných modulů Azure počítače výukové generovat kompaktní formátovaná jako tyto maximum dimenzionální kategorií proměnné. V konečném důsledku je zmenšené modelu, školení rychlejší a měřítka, které jsou úplně srovnatelná s použitím jinými postupy.

##### <a name="building-counting-transforms"></a>Stavebních započítání transformace

Vytvoření funkce počet, používáme **Sestavit počítání transformace** moduly, které je k dispozici v Azure počítače výukové. Modul vypadat takto:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Poznámka** : V poli **počet sloupců** jsme zadejte sloupce, které nám chcete provést počty na. Obvykle jde (jak je uvedeno) maximum dimenzionální kategorií sloupců. Na začátku, jsme uvedené, že datovou sadu Criteo má 26 kategorií sloupce: z Col15 Col40. Tady, abychom spolehnout všem poznámkám a jejich indexy (z 15 40 oddělených středníky, jak je znázorněno).

Pokud chcete používat modul v režimu MapReduce (hledaným velkých sad dat), potřebujeme přístup k obrázku HDInsight Hadoop (pro funkci průzkum lze opakovaně použít k tomuto účelu i) a její přihlašovací údaje. Předchozí obrázky ilustrují jaké vyplněné hodnoty vypadá (nahradit hodnoty zadané pro obrázku s těmi, které jsou důležité pro vlastní případu použití).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Na obrázku nahoře jsme předvedení zadejte umístění vstupní objektů blob. Toto umístění obsahuje data vyhrazená pro vytváření počet tabulek.


Po dokončení tohoto modulu jsme transformaci pro uložení později tak, že kliknete pravým tlačítkem myši na složku a vyberete možnost **Uložit jako transformace** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

V našem experiment architektuře uveden nad datovou sadu "ytransform2" přesně odpovídají transformace uložené count. Zbývající část tohoto experiment nemůžeme Předpokládejme, že čtenáře umožňuje **Vytvářet počítání transformace** modulu na některá data generovat počty a pak můžete tyto počty generovat funkce count ve vlaku a otestujte datové sady.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Volba, jaké počet funkce zahrnout vlaku a test datových sad

Po máme počet transformace připravené, uživatel může vybrat, které pokud chcete zahrnout do svých vlaku a otestujte datové sady používání modulu **Změnit počet tabulky parametry** funkce. Ukážeme jenom tento modul tady pro dokončení, ale v zájmu jednoduchosti není ve skutečnosti ho použít v naší testu.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

V tomto případě jak je vidět, jsme zvolili používat jenom pravděpodobnost protokolu a přeskočit regrese sloupce. Jsme můžete nastavit taky parametrů, například mezní Koš uvolnění, kolik částečně předchozí příklady a přidejte pro nástroj vyrovnání a jestli se mají použít libovolnou Laplacian hluku nebo ne. Vše Toto jsou pokročilé funkce a má poznamenat, výchozí hodnoty jsou vhodná výchozí hodnota pro uživatele, kteří jsou nové na tento typ generování funkce.

##### <a name="data-transformation-before-generating-the-count-features"></a>Transformace před generování funkce count

Teď zaměřit na důležitá transformace naše vlaku a otestujte data před skutečným generování funkce count. Všimněte si, že jsou dva moduly **Spustit skript R** používali, doporučujeme použít transformaci počet na naše data.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Tady je první pravidlo skript:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


V tomto skriptu R jsme přejmenujte naše sloupce na názvy "Sloupec1", "Col40". Je to proto transformace počet očekává jména v tomto formátu.

Ve druhém skriptu R jsme zůstatek rozdělení mezi kladné a záporné třídy (třídy 0 a 1) tak, že převzorkování záporné předmětu. Skript R tady ukazuje, jak můžete to udělat:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Tento jednoduchý skript R používáme "pos\_záporné\_poměr" nastavte velikost poměr kladný a záporný třídy. Toto je třeba udělat, protože vylepšení třídy neodpovídá obvykle mají výkon klasifikace problémy se rozdělení třídy zkosená (odvolat, že v našem případě máme kladné a záporné třídu 96.7 % 3,3 %).

##### <a name="applying-the-count-transformation-on-our-data"></a>Použití funkce count transformaci na naše data

Nakonec můžete používáme modulu **Použít transformace** použít transformace počet na naše vlaku a otestovat datové sady. Tento modul načte transformace uložené počet jeden předávat na vstupu a datové sady vlaku nebo test vstupu a vrátí dat pomocí funkce count. Je zobrazena zde:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Vypadat výpisem funkce count

Je významné najdete v článku funkce count prohlédnout, jak v našem případě. Tady ukážeme část takto:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

V tomto úryvek ukážeme, že pro sloupce, které jsme počítá na získání počty jsme protokolu pravděpodobnost kromě všechny relevantní backoffs.

Teď připraveni vytvořit Azure počítače výukové modelu pomocí těchto transformovaných datové sady. V následující části ukážeme, jak to provést.

#### <a name="azure-machine-learning-model-building"></a>Vytváření modelů Azure výukové počítače

##### <a name="choice-of-learner"></a>Volba student

Nejdřív potřeba zvolte student. Nyní klikněte na tlačítko použít dva třídy zesílen rozhodovacího stromu jako naše student. Tady jsou výchozí možnosti pro tento student:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Pro naše experiment budeme zvolit výchozí hodnoty. Jsme Všimněte si, že jsou výchozí hodnoty jsou obvykle smysluplné a spolehlivých způsobů, jak získat rychlý směrné plány na výkon. Na výkon můžete zlepšit tak, že komínů parametry, pokud se rozhodnete po uložení směrného plánu.

#### <a name="train-the-model"></a>Školení modelu

Školení, můžeme jednoduše vyvolat modulu **Modelu vlaku** . Dva vstupy k němu se student dvě třídy zesílen rozhodovacího stromu naše datovou sadu vlaku. To je zobrazena zde:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Skóre modelu

Jakmile máme modelu školení, jsme připravení skóre na datovou sadu test a chcete zjistit jeho výkon. Bychom pomocí modulu **Skóre modelu** ukazuje následující obrázek spolu s modul **Vyhodnocení modelu** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Krok 5: Vyhodnocení modelu

Nakonec byste rádi Analýza výkonu modelu. Dvě třídy (binární) klasifikace potíže, je dobré míru obvykle AUC. Vizualizace, to, jsme připojit **Skóre modelu** modul modul **Vyhodnocení modelu** pro tuto. Klepnutí na příkaz **vizualizace** v modulu **Vyhodnocení modelu** dává obrázku jako následující:

![Vyhodnocení modul Tabelární modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

V binární (nebo dvou třídy) klasifikace potíže, dobré míra přesnosti předpovědí je oblasti v části křivky (AUC). V následující ukážeme naše výsledků pomocí tento model na naše test datovou sadu. Aby to, klikněte pravým tlačítkem myši na výstupní port modul **Vyhodnocení Model** a **vizualizace**.

![Vizualizace modul vyhodnocení modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Krok 6: Publikování modelu jako webové služby
Možnost publikovat modelu Azure počítače výukové jako webovým službám s nejméně fuss je cenný funkce týkající se uplatňování dostup dostupný. Po dokončení, kdokoliv můžete volat do webové služby pomocí vstupní data, která potřebují předpovědí pro a webová služba používá modelu vrátit tyto předpovědí.

K tomuto účelu jsme nejdřív je uložit naše školení modelu jako objekt školení modelu. Důvodem je tak, že kliknete pravým tlačítkem na modul **Vlaku modelu** a možnost **Uložit jako školení modelu** .

Pak potřeba vytvořit vstupní a výstupní porty pro naše webové služby:

* vstupní port trvá dat stejným způsobem jako data, která potřebujeme předpovědí pro
* výstupní port vrátí dosáhne popisky a spojena pravděpodobnost.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Vyberte několik řádků dat pro vstupní port

Je vhodné používat modul **Použít transformace SQL** vyberte jenom 10 řádků sloužit jako vstupní port data. Vyberte jenom tyto řádky dat pro naše vstupní port pomocí příkazu SQL ukazuje tento obrázek:

![Vstupní port dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webové služby
Teď můžeme připraveni spustit malé experiment něhož chcete publikovat naše webové služby.

#### <a name="generate-input-data-for-webservice"></a>Generování zadávání dat pro webservice

Pomohla zeroth protože tabulka počet je velká, trvat několik řádků testovací data jsme generovat výstupní data z něho pomocí funkcí počet. Mohou sloužit jako formát zadávání dat pro naše webservice. To je zobrazena zde:

![Vytvoření Tabelární zadávání dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Pro formátu vstupní data použijeme teď výstup modulu **Featurizer počet** . Po dokončení tohoto experiment výstup uložte z modulu **Počet Featurizer** jako datovou sadu. Tento datovou sadu se používá pro zadávání dat v webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Hodnocení experiment pro publikování webservice

Nejdřív ukážeme, jak to vypadá. Základní struktura představuje modul **Skóre modelu** , který přijme naše školení modelu objektu a několik řádků zadávání dat, generovaný v předchozích krocích používání modulu **Count Featurizer** . Použijeme "Vyberte sloupce v datovou sadu" k projektu se štítky Scored a pravděpodobnosti skóre.

![Vyberte sloupce v sadě dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Všimněte si, jak se dá použít modulu **Vybrat sloupce v sadě dat** pro "odfiltrování" data z datovou sadu. Ukážeme obsah tady:

![Filtrování sloupce vyberte datovou sadu modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Získat modré vstupní a výstupní porty, jednoduše kliknete na **připravit webservice** v pravé dolní. Spuštění této experiment také umožňuje publikovat webové služby: klikněte na ikonu **Publikovat webové služby** na najdete vpravo dole ukazuje tento obrázek:

![Publikování webové služby](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Po publikování webservice jsme získat přesměruje na stránku, která vypadá takto:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Na levé straně vidíme dva odkazy pro webservices:

* **Žádostí a odpovědí** služby (nebo prostředku) je určen pro jeden předpovědí a je co jsme využít tento Workshop.
* **Spuštění dávky** služby (BES) se používá pro dávku předpovědí a vyžaduje, aby vstupních dat používají k vytvoření předpovědí uložena v úložišti objektů Blob Azure.

Po kliknutí na odkaz, který **Žádostí a odpovědí** trvá na stránku, která dostaneme předem konzervované kód v jazyce C# python a R. Tento kód můžete snadno použít pro volání webservice. Všimněte si, že má být použita pro ověřování potřebuje klávesu rozhraní API na této stránce.

Je vhodné zkopírujte tento kód python myší na novou buňku v poznámkovém bloku IPython.

Tady ukážeme segment python kódu se správným rozhraní API klíčem.

![Python kód](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Poznámka: jsme výchozí rozhraní API klíč nahrazen příkazem naše webservices rozhraní API klíče. Klikněte na příkaz **Spustit** v této buňce v poznámkovém bloku IPython poskytuje následující odpověď:

![IPython odpověď](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Vidíme, že dvě test příklady, které jsme dotaz (v rámci JSON skriptu python), doporučujeme odpovědi zpět ve formuláři "Scored štítky, Scored pravděpodobnosti". Všimněte si, že v tomto případě zvolili jsme výchozí hodnoty, která předem konzervách kód obsahuje (0 je řetězec "hodnotu" všech kategorií sloupců a všechny číselné sloupce).

Tímto dokončíte naše začátku do konce návodu znázorňující postup v případě rozsáhlých datovou sadu pomocí výukové počítače Azure. Začínáme s terabajt dat, vytvořen modelu prediktivní vkládání jsme nasazený jako webové službě v cloudu.
