<properties
    pageTitle="Vytvoření a načtení dat do tabulky podregistru z úložiště objektů Blob | Microsoft Azure"
    description="Vytvoření podregistru tabulek a načtení dat do objektů blob do podregistru tabulek"
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Vytvoření a načtení dat do tabulky podregistru z úložišti objektů blob Azure

Toto téma uvádí obecný podregistru dotazech vytvoření podregistru tabulek a načtení dat z úložiště objektů blob Azure. Několik rad k dispozici je také na rozdělení podregistru tabulek a o použití optimalizované řádku sloupcovém (ORC) formátování pro zvýšení výkonu dotazu.

Této **nabídce** odkazy na témata, která popisují, jak jedí data do cílové prostředí, kde můžete data uložená a zpracování během týmu dat pro výzkum obrázku (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte:

* Vytvoření účtu Azure úložiště. Pokud potřebujete pokyny, přečtěte si článek [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). 
* Zřízení přizpůsobený clusteru Hadoop ke službě HDInsight.  Pokud potřebujete pokyny najdete v článku [přizpůsobení Azure HDInsight Hadoop clusterů pokročilé technologie pro analýzu](machine-learning-data-science-customize-hadoop-cluster.md).
* Povolené vzdálený přístup k obrázku, přihlášení a otevřeli konzole Hadoop příkazového řádku. Pokud potřebujete pokyny najdete v článku [přístup vedoucí uzel Hadoop obrázku](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Odeslání dat do úložišti objektů blob Azure
Pokud jste vytvořili Azure virtuálního počítače podle pokynů uvedených v [Nastavení Azure virtuálního počítače pro pokročilé technologie pro analýzu](machine-learning-data-science-setup-virtual-machine.md), tento soubor skriptu mají stažené do *C:\\uživatelé\\\<uživatelské jméno\>\\dokumenty\\dat pro výzkum skripty* adresáře v počítači virtuální. Tyto dotazy podregistru vyžadují pouze zapojte vlastní schéma dat a konfiguraci úložiště objektů blob Azure do příslušných polí je připravená k odeslání.

Budeme se předpokládá, že data pro podregistru tabulek jsou ve formátu tabulky **nekomprimované** a že data byl odeslán do výchozího stavu (a dalších) kontejneru úložiště účtu použít Hadoop clusteru.

Pokud budete chtít praktické cvičení **NYC Taxi ze služební cesty dat**, potřebujete:

- **stažení** 24 [NYC Taxi ze služební cesty](http://www.andresmh.com/nyctaxitrips) datům (12 ze služební cesty a 12 tarif souborů)
- **Rozbalení** všech souborů do souborů CSV a potom
- **nahrání** je výchozí (nebo odpovídající kontejner) účet Azure úložiště, který byl vytvořen procedurou zobrazené v tématu [přizpůsobení Azure HDInsight Hadoop clusterů pokročilé technologie pro analýzu procesy a technologii](machine-learning-data-science-customize-hadoop-cluster.md) . Proces nahrát soubory CSV do kontejneru výchozí na účtu úložiště najdete na této [stránce](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Způsob odeslání podregistru dotazů

Dotazy podregistru lze odeslat pomocí:

1. [Odeslání podregistru dotazů prostřednictvím Hadoop příkazového řádku v headnode Hadoop obrázku](#headnode)
2. [Odesílání podregistru dotazů pomocí editoru podregistru](#hive-editor)
3. [Odesílání podregistru dotazů s příkazy Powershellu Azure](#ps)

Dotazy podregistru jsou podobné SQL. Pokud máte zkušenosti s SQL, užitečné [podregistru SQL uživatelé cheaty listu](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) .

Po odeslání dotazu podregistru, můžete také určit cílový výstup podregistru dotazů, jestli se na obrazovce nebo místní soubor na uzel hlavy nebo objektů blob Azure.


###<a name="headnode"></a>1. odesílání podregistru dotazů prostřednictvím Hadoop příkazového řádku v headnode Hadoop obrázku

Pokud dotaz podregistru složité, jeho odesláním přímo do hlavního uzlu Hadoop obrázku obvykle vede k rychleji zapnutí než podávání pomocí skriptů podregistru redaktora nebo Azure Powershellu.

Přihlášení do hlavního uzlu Hadoop obrázku, otevřete příkazového řádku Hadoop na ploše hlavního uzlu a zadejte příkaz `cd %hive_home%\bin`.

Máte tři způsoby, jak odesílání dotazů podregistru do příkazového řádku Hadoop:

* přímo
* používání souborů .hql
* v konzole příkaz podregistru

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Odesílání podregistru dotazů přímo v Hadoop příkazového řádku.

Spusťte příkaz jako `hive -e "<your hive query>;` můžou odeslat jednoduché podregistru dotazy přímo v Hadoop příkazového řádku. Tady je příklad červeným ohraničením osnovy zobrazí na příkaz, odešle dotaz podregistru, kde pole zelené osnovy zobrazí výstup dotazu podregistru.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Odesílání dotazů podregistru .hql souborů se změnami

Když podregistru dotazu je složitější, má několik řádků úpravy dotazů v příkazového řádku nebo podregistru příkaz konzoly nejsou praktické. Další možností je umožňuje textovém editoru v hlavní uzel clusteru Hadoop uložit dotazy podregistru do souboru .hql místního adresáře hlavního uzlu. Dotaz podregistru do souboru .hql lze odeslat pomocí a pak `-f` argument takto:

    hive -f "<path to the .hql file>"

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Potlačit tisk obrazovky stav průběhu podregistru dotazů**

Ve výchozím nastavení po odeslání dotazu podregistru v Hadoop příkazového řádku průběh mapy a zmenšení úlohy se vytiskne na obrazovce. Potlačí tisk obrazovky postupu projektu mapování nebo zmenšit, můžete použít jako argument `-S` ("N" ve na velká písmena) v příkazového řádku následujícím způsobem:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Odesílání podregistru dotazů v konzole příkaz podregistru.

Můžete taky nejdřív zadat konzole příkaz podregistru pomocí příkazu `hive` v Hadoop příkazového řádku a odešlete podregistru dotazy v konzole příkaz podregistru. Tady je příklad. V tomto příkladu dvě červené pole zvýraznění příkazů pro zadejte příkaz konzoly podregistru a dotaz podregistru odeslané ve podregistru příkaz konzoly. Zelené pole zvýrazní výstup dotazu podregistru.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

V předchozích příkladech přímo výstup podregistru výsledků dotazu na obrazovce. Můžete taky napíšete výstup do místního souboru na uzel hlavy, včetně rad k objektů blob Azure. Můžete pomocí dalších nástrojů pro další analýzu výstup podregistru dotazů.

**Výstup výsledků dotazu podregistru do místního souboru.**

Výstup výsledků dotazu podregistru do místního adresáře na uzel hlavy, budete muset odeslat dotaz podregistru do příkazového řádku Hadoop následujícím způsobem:

    hive -e "<hive query>" > <local path in the head node>

V následujícím příkladu je výstup dotazu podregistru zapsán do souboru `hivequeryoutput.txt` v adresáři `C:\apps\temp`.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Výstup podregistru výsledků dotazu objektů blob Azure**

Můžete taky vytvořit výsledků dotazu podregistru do Azure blob v kontejneru výchozí Hadoop obrázku. Podregistru dotaz na to vypadá takto:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

V následujícím příkladu je napsali výstup dotazu podregistru do adresáře objektů blob `queryoutputdir` v kontejneru výchozí Hadoop obrázku. Tady stačí zadat název adresáře bez názvu objektů blob. Pokud například zadat adresář a objektů blob názvy, vyvolá chybu `wasb:///queryoutputdir/queryoutput.txt`.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Pokud jste otevřeli kontejneru výchozí Hadoop clusteru pomocí Průzkumníka úložišť Azure, uvidíte výstup dotazu podregistru, jak je znázorněno na následujícím obrázku. Filtr (zvýraznit pomocí červeným ohraničením) můžete použít jenom načítat objektů blob s zadaná písmena v názvech.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. odesílání podregistru dotazů pomocí editoru podregistru

Můžete také konzole dotazu (podregistru Editor) zadáním adresy URL formuláře *https://&#60; Název clusteru Hadoop >.azurehdinsight.net/Home/HiveEditor* do webového prohlížeče. Musíte být přihlášení k lyncu v vidí tento konzoly a tak musíte Hadoop clusteru přihlašovacích údajů v tomto poli.

###<a name="ps"></a>3. odesílání podregistru dotazů s příkazy Powershellu Azure

Můžete taky prostředí PowerShell můžou odeslat podregistru dotazů. Pokyny najdete v tématu [odeslání podregistru úlohy pomocí prostředí PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Vytvoření podregistru databázi a tabulky

Dotazy podregistru někdo sdílí [Github úložiště](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) a dají stáhnout odsud.

Tady je podregistru dotaz, který vytvoří tabulku podregistru.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Tady je popis pole, které je potřeba zapojit a ostatní konfigurace:

- **& #60; název databáze >**: název databáze, které chcete vytvořit. Pokud chcete použít výchozí databáze, můžete dotaz *vytvořit databázi...* vynechány.
- **& #60; název tabulky >**: název tabulky, který chcete vytvořit v zadané databázi. Pokud chcete použít výchozí databázi, tabulku můžete přímo předložit *& #60; název tabulky >* bez & #60; název databáze >.
- **& #60; oddělovač polí >**: Pokud chcete oddělovač oddělující polí v datovém souboru nepovolila tabulku podregistru.
- **& #60; řádek oddělovač >**: Pokud chcete oddělovač oddělující řádků v datovém souboru.
- **& #60; úložiště >**: Azure úložiště místo pro uložení dat podregistru tabulek. Pokud nezadáte *umístění & #60; úložiště >*, databázi a tabulky jsou uloženy v *podregistru/sklad/* adresáře v kontejneru výchozí clusteru podregistru ve výchozím nastavení. Pokud chcete určit umístění úložiště, umístění úložiště musí být v kontejneru výchozí pro databázi a tabulky. Toto umístění musí být označují jako místo vzhledem k kontejneru výchozího obrázku ve formátu *"wasb: / / / & #60; adresář 1 > /"* nebo *"wasb: / / / & #60; adresář 1 > / & #60; adresář 2 > /"*atd. Po spuštění dotazu relativní adresáře vytvořené v kontejneru výchozí.
- **TBLPROPERTIES("Skip.Header.line.Count"="1")**: Pokud soubor dat neobsahuje řádek záhlaví, budete muset přidat tato vlastnost **na konci** dotazu *vytvořit tabulku* . Řádek záhlaví v opačném vložen jako záznamu do tabulky. Pokud soubor dat neobsahuje řádek záhlaví, můžete tuto konfiguraci vynechány v dotazu.

## <a name="load-data"></a>Načtení dat do podregistru tabulek
Tady je podregistru dotaz, který načítá data do tabulku podregistru.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; cesta k datům objektů blob >**: Pokud je soubor objektů blob nepovolila tabulku podregistru v kontejneru výchozí clusteru HDInsight Hadoop *& #60; cesta k datům objektů blob >* by měl být ve formátu *"wasb: / / / & #60; adresáře v tomto kontejneru > / & #60; název souboru objektů blob >"*. Soubor objektů blob lze také v kontejneru další clusteru HDInsight Hadoop. V tomto případě *& #60; cesta k datům objektů blob >* by měl být ve formátu *"wasb: / / & #60; kontejneru name>@&#60;storage název účtu >.blob.core.windows.net/ & #60; objektů blob název souboru >"*.

    >[AZURE.NOTE] Kulatý data, která chcete nahrát tabulku podregistru musí být v části výchozí nebo další kontejner účtu úložiště pro Hadoop obrázku. V opačném *NAČTENÍ dat* dotaz se nezdaří nesouhlasících, nemůžete získat přístup k datům.


## <a name="partition-orc"></a>Rozšířené témata: oddíly tabulky a úložiště přihlašovacích údajů podregistru dat ve formátu ORC

Pokud jsou data velký, rozdělení tabulce je výhodné dotazy, které stačí skenování několik oddíly v tabulce. Například je vhodné oddíl dat protokolu webu podle kalendářních dat.

Kromě rozdělení podregistru tabulek, je také užitečné pro uložení dat podregistru ve formátu optimalizované řádku sloupcovém (ORC). Další informace o ORC formátování najdete v článku <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">použití ORC souborů zlepšuje výkon při čtení, psaní a zpracování dat podregistru</a>.

### <a name="partitioned-table"></a>Rozdělený tabulky
Tady je podregistru dotaz, který vytvoří tabulku oddíly a načte data do ní.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Při dotazování na rozdělený tabulek, doporučuje se přidat podmínku oddíl na **začátku** `where` klauzule jako to se vylepšuje s účinnost hledání výrazně.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Uložte podregistru data ve formátu ORC

Nemůžete načítat přímo data z úložiště objektů blob do tabulek podregistru, která jsou uložená ve formátu ORC. Tady je postup, které budete muset udělat k načtení dat z Azure objektů BLOB uložené ve formátu ORC tabulkami podregistru.

Vytvořte externí tabulka **ULOŽENÁ jako textový soubor** a načtení dat z úložiště objektů blob do tabulky.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Vytvoření vnitřní tabulku s na stejné schéma jako externí tabulku v kroku 1, s stejný oddělovač a uložit podregistru data ve formátu ORC.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Vyberte data z externího tabulky v kroku 1 a vložit do tabulky ORC

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Pokud v tabulce textový soubor *& #60; název databáze >. & #60; název tabulky externí textový soubor >* má oddíly v KROKU 3 `SELECT * FROM <database name>.<external textfile table name>` příkaz vybere proměnnou oddíl jako pole vrácené uvedenou množinu dat. Vložením do *& #60; název databáze >. & #60; ORC název tabulky >* selže od *& #60; název databáze >. & #60; ORC název tabulky >* nemá proměnnou oddíl jako pole schématu tabulky. V takovém případě budete muset konkrétně vyberte pole, která má být vložena do *& #60; název databáze >. & #60; ORC název tabulky >* následujícím způsobem:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

To sice přetáhnout *& #60; název tabulky externí textový soubor >* při použití následující dotaz po všechna data vložena do *& #60; název databáze >. & #60; ORC název tabulky >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Po provedení kroků tohoto postupu, byste měli mít tabulku s daty ve formátu ORC připravená k použití.  
