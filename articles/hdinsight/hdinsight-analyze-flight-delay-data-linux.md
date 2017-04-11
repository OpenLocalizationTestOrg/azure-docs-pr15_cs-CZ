<properties 
    pageTitle="Analýza dat zpoždění letů pomocí podregistru na základě Linux HDInsight | Microsoft Azure" 
    description="Zjistěte, jak používat podregistru k analýze dat letu na základě Linux HDInsight a potom exportovat data do SQL databáze pomocí Sqoop." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analýza dat zpoždění letů pomocí podregistru v HDInsight

Zjistěte, jak k analýze dat zpoždění letů pomocí podregistru na základě Linux HDInsight a export dat do databáze SQL Azure pomocí Sqoop.

> [AZURE.NOTE] Zatímco lze použít jednotlivé tohoto dokumentu se systémem Windows HDInsight clusterů (Python a podregistru například), jsou specifické pro na základě Linux clusterů několika krocích. Postup, která budou fungovat s clusteru serveru s Windows najdete v tématech [analyzovat letu zpoždění dat pomocí podregistru v HDInsight](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __HDInsight obrázku__. Postup pro vytvoření nového obrázku na základě Linux Hdinsightu najdete v článku [Začínáme s používáním Hadoop s podregistru v HDInsight na Linux](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Databáze azure SQL__. Databáze Azure SQL použije jako cílového úložiště. Pokud už nemáte databázi SQL, přečtěte si článek [databáze SQL kurz: vytvoření SQL databáze v minutách](../sql-database/sql-database-get-started.md).

- __Azure rozhraní příkazového řádku__. Pokud jste nenainstalovali rozhraní příkazového řádku Azure, naleznete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) další kroky.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Stažení dat letech

1. Běžte na [zdroje informací a inovativní technologii Správa Bureau s jednou variantou dopravy][rita-website].
2. Na stránce vyberte tyto hodnoty:

  	| Jméno | Hodnota |
  	| ---- | ---- |
  	| Filtr rok | 2013 |
  	| Filtrování období | Leden |
  	| Pole | Rok FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, cíl, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Zrušte zaškrtnutí všech polí |

3. Klikněte na **Stáhnout**. 

##<a name="upload-the-data"></a>Odeslání dat

1. K nahrání souboru zip na uzel hlavy clusteru HDInsight použijte tento příkaz:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Nahraďte __název souboru__ název souboru zip. Nahraďte __uživatelské jméno__ přihlášení SSH clusteru HDInsight. Nahraďte NÁZEV_CLUSTERU název clusteru HDInsight.
    
    > [AZURE.NOTE] Pokud ověření SSH přihlášení pomocí hesla, zobrazí se výzva k zadání hesla. Pokud jste použili veřejným klíčem, budete muset používat `-i` parametr a zadejte cestu k odpovídající privátním klíčem. Příklad `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Po dokončení nahrávání připojte k obrázku pomocí SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Další informace o použití SSH s na základě Linux Hdinsightu najdete v následujících článcích:
    
    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Po připojení, použijte následující soubor ZIP rozbalit:

        unzip FILENAME.zip
    
    To bude extrahovat soubor CSV, který je přibližně 60MB velikost.
    
4. Pomocí následujících můžete vytvářet nové adresář na WASB (distribuované úložišti používá HDInsight) a zkopírujte soubor:

    hdfs distribuovaného systému souborů - mkdir -p /tutorials/flightdelays/data hdfs distribuovaného systému souborů – umístění FILENAME.csv a výukové programy pro/flightdelays/data /
    
##<a name="create-and-run-the-hiveql"></a>Vytváření a spouštění HiveQL

Import dat ze souboru .csv do tabulku podregistru s názvem __zpoždění__pomocí následujících kroků.

1. Vytváření a upravování nový soubor s názvem __flightdelays.hql__použijte následující:

        nano flightdelays.hql
        
    Použijte následující jako obsah tohoto souboru:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Uložte soubor pomocí __kombinace kláves Ctrl + X__a __Y__ .

3. Použijte následující dál podregistru __flightdelays.hql__ souboru:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] V tomto příkladu `localhost` slouží vzhledem k tomu, že jste připojeni k hlavního uzlu clusteru HDInsight, což je místo, kam se systémem HiveServer2.

4. Pokud chcete otevřít relaci interaktivní Beeline použijte tento příkaz:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Když dostanete `jdbc:hive2://localhost:10001/>` objeví se výzva, použijte následující k načtení dat z importovaného letu daty zpoždění.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Tím načtěte seznam měst, které praxí zpoždění počasí, spolu s časem průměr zpoždění a uložit, aby šlo `/tutorials/flightdelays/output`. Sqoop později, bude číst data z tohoto umístění a exportovat do databáze SQL Azure.

6. Beeline opustíte zadejte `!quit` příkazového řádku.

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL

Pokud už máte databázi SQL musíte zjištění názvu serveru. To můžete najít v [Portálu Azure](https://portal.azure.com) výběrem __SQL databáze__a poté filtrování na název databáze, kterou chcete použít. Název serveru uvedené ve sloupci __serveru__ .

Pokud už nemáte databázi SQL, pomocí informací v [databáze SQL kurz: vytvoření SQL databáze v minutách](../sql-database/sql-database-get-started.md) a vytvořte si ho. Je třeba uložit názvu serveru pro databázi.

##<a name="create-a-sql-database-table"></a>Vytvoření tabulky databáze SQL

> [AZURE.NOTE] Připojení k databázi SQL vytvořte tabulku mnoha různými způsoby. Následující postup používá [FreeTDS](http://www.freetds.org/) z clusteru HDInsight.

1. Připojení k obrázku na základě Linux HDInsight pomocí SSH a spusťte následující kroky z SSH relace.

3. Umožňuje instalaci FreeTDS tento příkaz:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Po instalaci FreeTDS pomocí následujícího příkazu pro připojení k databázi SQL serveru. Nahraďte název databáze SQL serveru __webu název serveru__ . Nahraďte __adminLogin__ a __adminPassword__ přihlášení databáze SQL. Nahraďte __název databáze__ název databáze.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Zobrazí se výstup následujícím způsobem:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. V `1>` objeví se výzva, zadejte následující příkazy:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Když `GO` údajů se zadává, bude vyhodnocen předchozí příkazy. Tím vytvoříte novou tabulku s názvem __zpoždění__seskupený index (musí tak, že databáze SQL).

    Použijte následující ověřit, že byl vytvořen v tabulce:

        SELECT * FROM information_schema.tables
        GO

    Měli byste vidět výstup následujícím způsobem:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Zadejte `exit` v `1>` výzva k ukončení nástroje tsql.
    
##<a name="export-data-with-sqoop"></a>Export dat s Sqoop

2. Pomocí následujícího příkazu můžete ověřit, že Sqoop vidí databázi SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To, měly vrátit seznam databází, včetně databáze, který jste vytvořili v tabulce zpoždění v.

3. Pomocí následujícího příkazu Exportovat data z hivesampletable mobiledata tabulky:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Tím se nastaví Sqoop připojení k databázi SQL na databázi obsahující tabulku zpoždění a export dat z wasbs: / / / výukové programy pro/flightdelays/výstup (jsme uložení výstup dotazu podregistru dříve) k tabulce zpoždění.

4. Po dokončení příkazu připojení k databázi pomocí TSQL pomocí následující:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Po připojení, použijte následující příkazy pro ověření, že se data byla exportovat tabulku mobiledata:
    
        SELECT * FROM delays
        GO

    Měli byste vidět přehled dat v tabulce. Typ `exit` ukončete nástroj tsql.

##<a id="nextsteps"></a>Další kroky

Teď víte, jak nahrát soubor k úložišti objektů Blob Azure, jak k naplnění tabulku podregistru pomocí dat z úložiště objektů Blob Azure, k tomu podregistru dotazů a jak používat Sqoop exportovat data z HDFS k databázi Azure SQL. Další informace naleznete v následujících článcích:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Použití podregistru s HDInsight][hdinsight-use-hive]
* [Použití Oozie s HDInsight][hdinsight-use-oozie]
* [Použití Sqoop s HDInsight][hdinsight-use-sqoop]
* [Použití Prasátko s HDInsight][hdinsight-use-pig]
* [Můžete vyvíjet aplikace Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]
* [Můžete vyvíjet Python Hadoop streamování programy pro HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
