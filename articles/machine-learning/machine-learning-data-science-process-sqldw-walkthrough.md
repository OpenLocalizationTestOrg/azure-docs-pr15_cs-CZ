<properties
    pageTitle="Proces týmu dat pro výzkum v akci: pomocí SQL datový sklad | Microsoft Azure"
    description="Pokročilé technologie pro analýzu procesy a technologii v akci"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Proces týmu dat pro výzkum v akci: pomocí SQL datový sklad

V tomto kurzu budeme vás provede procesem vytvoření a nasazení počítače výukové modelu pomocí SQL datový sklad (SQL DW) pro veřejně dostupný datovou sadu – datovou sadu [NYC Taxi cest](http://www.andresmh.com/nyctaxitrips/) . Binární klasifikace modelu vytvořen předpovídá též tip je zaplacené výletu a modely pro multiclass klasifikaci a regresní, které předpovídání rozdělení pro vyplacené částky tip rovněž popisované.

Proces spočívá v pracovním postupu [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Ukážeme, jak nastavit prostředí pro výzkum dat, jak data načítáte do SQL DW a jak pomocí SQL DW nebo Poznámkový blok IPython zkoumání dat a analýza funkce do modelu. Potom ukážeme, jak sestavovat a nasazovat model s Azure počítače učení.


## <a name="dataset"></a>Datovou sadu NYC Taxi cest

Data ze služební cesty Taxi NYC sestává z o 20GB komprimované soubory CSV (nekomprimované ~ 48GB), nahrávání 173 milionů jednotlivé cest a tarify zaplacený při každé ze služební cesty. Každý záznam ze služební cesty obsahuje odběru a odkládací umístění a čas, anonymních napadení (ovladač) číslo licence a číslo Medailon (společnosti taxi jedinečné id). Data za rok 2013 zahrnuje všechny cesty a pro každý měsíc není uvedený v následující dvě datové sady:

1. Soubor **trip_data.csv** obsahuje podrobnosti o ze služební cesty, jako je třeba počet cestující odběru a dropoff body, doba trvání cesty a délka ze služební cesty. Tady je pár ukázkových záznamů:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Soubor **trip_fare.csv** obsahuje podrobnosti o tarif zaplacený při každé ze služební cesty, jako je typ platby, tarif částka, přirážky daně, tipy a mýtné a celkovou částku zaplacenou. Tady je pár ukázkových záznamů:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Jedinečný klíč** pro přístup ke ze služební cesty\_dat a ze služební cesty\_tarif se skládá z následujících tři pole:

- medailonu,
- nabourat\_licence a
- odběru\_data a času.

## <a name="mltasks"></a>Adresa tři typy úkolů předpovědí

Jsme formulace tři problémy předpověď na základě *tip\_částka* znázorňující tři typy modelování úkoly:

1. **Binární klasifikace**: odhadnout též tip byl zaplacené výletu, tedy *tip\_částka* , která je větší než 0 Kč je kladné příklad, zatímco *tip\_částka* 0 Kč je záporné příklad.

2. **Multiclass klasifikace**: odhadnout oblast tip zaplacené cesty. Jsme rozdělit *tip\_částka* do pět intervalů nebo třídy:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Analytický nástroj Regrese úkolu**: odhadnout částka zaplacená za výletu tip.  


## <a name="setup"></a>Nastavení prostředí pro výzkum Azure dat pro pokročilé technologie pro analýzu

Nastavení prostředí vědy dat Azure, takto:

**Vytvoření vlastního účtu úložiště objektů blob Azure**

- Když zřizujete úložišti objektů blob Azure, vyberte umístění geo k úložišti objektů blob Azure v nebo jak nejblíže k **Centrální USA jih**, což je kam se ukládají data NYC Taxi. Data se zkopírují pomocí AzCopy z kontejneru úložiště objektů blob veřejné do kontejneru v účtu úložiště. Bližší úložišti objektů blob Azure jih centrální námi, tím rychleji bude dokončení tohoto úkolu (krok 4).
- Pokud chcete vytvořit účet Azure úložiště, postupujte podle kroků uvedených v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Nezapomeňte poznámky o hodnotách následující přihlašovací údaje účtu úložiště jako potřebovali dál v tomto návodu.

  - **Název účtu úložiště**
  - **Klíč účtu úložiště**
  - **Název kontejneru** (který se má data, která mají být uložené v úložišti objektů blob Azure)

**Zřízení instance Azure SQL DW.**
Postupujte podle následující dokumentaci pro [vytvořit datový sklad SQL](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) zřízení instanci SQL datový sklad. Ujistěte se, že jste notace na následující pověření datový sklad SQL, která se použije v dalších krocích.

  - **Název serveru**: <server Name>. database.windows.net
  - **Název SQLDW (databáze)**
  - **Uživatelské jméno**
  - **Heslo**

**Instalace aplikace Visual Studio 2015 a SQL Server Data Tools.** Pokyny najdete v tématu [instalace aplikace Visual Studio 2015 nebo SSDT (SQL Server Data Tools) pro datový sklad SQL](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Připojení k DW Azure SQL pomocí aplikace Visual Studio.** Pokyny najdete v tématu kroky 1 a 2 v části [připojit ke datový sklad SQL Azure pomocí aplikace Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Spuštěním následujícího dotazu SQL na databázi, kterou jste vytvořili v SQL datový sklad (místo dotaz v kroku 3 v tématu připojení k dispozici) při **vytváření klíče předlohy**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Vytvoření pracovního prostoru Azure počítače výuka podle předplatného Azure.** Pokyny najdete v tématu [Vytvoření pracovního prostoru výukové počítače Azure](machine-learning-create-workspace.md).

## <a name="getdata"></a>Načtení dat do SQL datový sklad

Spusťte konzolu příkazu prostředí Windows PowerShell. Spusťte následující prostředí PowerShell příkazů, které chcete stáhnout příklad SQL skriptu soubory, které nám s vámi sdílet na Github místního adresáře, které zadáte s parametrem *– DestDir*. Můžete změnit hodnotu parametru *-DestDir* místního adresáře. Pokud *- DestDir* neexistuje, vytvoří se skriptem Powershellu.

>[AZURE.NOTE] Je potřeba **Spustit jako správce** při provádění tento skript Powershellu, pokud adresáři *DestDir* vyžaduje oprávnění správce k vytvoření nebo zápisu.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Po úspěšném spuštění svého aktuálního pracovního adresáře změní na *- DestDir*. Mají být k dispozici obrazovky jako níže:

![][19]

V *-DestDir*spusťte tento skript Powershellu v režimu správce:

    ./SQLDW_Data_Import.ps1

Pokud skript Powershellu spuštěna poprvé, se zobrazí výzva k zadání informace z vašeho DW SQL Azure a vaším účtem úložiště objektů blob Azure. Po dokončení tohoto skriptu prostředí PowerShell spuštěna poprvé, pověření vstupní se bude byla vytvořilo konfiguračního souboru SQLDW.conf v adresáři prezentovat pracovní. Budoucí spustit tento soubor skriptu prostředí PowerShell mají možnost přečtete že všechny potřebné parametry z tohoto souboru konfigurace. Pokud potřebujete změnit některé parametry, můžete na vstupní parametry na obrazovce na výzvu odstraněním tohoto souboru konfigurace a vložení hodnoty parametrů vyzvání nebo změňte hodnoty parametrů úpravou SQLDW.conf souboru v adresáři *- DestDir* .

>[AZURE.NOTE] Pokud se chcete vyhnout konflikty jmen schéma s těmi, které již ve vaší DW SQL Azure, existují při čtení parametry přímo ze souboru SQLDW.conf, náhodné číslo 3místný přibude název schématu ze souboru SQLDW.conf jako výchozí název schématu pro každé spuštění. Skript Powershellu můžete být vyzváni k zadání názvu schématu: název může být zadán v uživatel v případě potřeby.

Tento soubor **skriptu prostředí PowerShell** dokončí tyto věci:

- **Stáhne a nainstaluje AzCopy**, pokud už není nainstalovaná AzCopy

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopie dat do účtu úložiště objektů blob soukromé** z veřejné objektů blob s AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Načtení dat pomocí Polybase (spuštěním LoadDataToSQLDW.sql) do svého DW SQL Azure** ze svého účtu úložiště objektů blob soukromé pomocí následujících příkazů.

    - Vytvoření schématu

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Vytvoření databáze omezené přihlašovacích údajů

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Vytvoření externího zdroje dat pro objektů blob Azure úložiště

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Vytvořte externí formátu souboru csv. Data nekomprimované a polí jsou oddělené pomocí znakem.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Vytvořte externí tarif a tabulek ze služební cesty pro datovou sadu taxi NYC v úložišti objektů blob Azure.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Načtení dat z externích tabulek v úložišti objektů blob Azure SQL datový sklad

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Vytvoření tabulky dat s vzorku (NYCTaxi_Sample) a vložte data k němu zvýšenými dotazy SQL na tabulky ze služební cesty a tarif. (Některé kroky tohoto návodu musí používat ukázkové tabulky.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Zeměpisné polohy účtů úložiště ovlivňuje časy načítání.

>[AZURE.NOTE] Podle geografické polohy účtu úložiště objektů blob soukromé kopírování dat z veřejné objektů blob ke svému účtu soukromé úložiště může trvat asi 15 minut, nebo delší dobu, a od načítání dat ze svého účtu úložiště pro vaše DW SQL Azure může trvat 20 minut nebo delší.  

Budete muset rozhodnout které, pokud máte duplicitní zdrojové a cílové soubory.

>[AZURE.NOTE] Pokud souboru .csv soubory zkopírovaly z úložiště objektů blob veřejné ke svému účtu úložiště objektů blob soukromé již existují ve vašem účtu úložiště objektů blob soukromé, AzCopy se zeptá, jestli chcete přepsat. Pokud nechcete přepsat, **n** po zobrazení výzvy při zadávání. Pokud chcete přepsat **všechny** z nich, zadávání **** po zobrazení výzvy. Můžete také vstupní **y** uvedeného postupu přepsat soubory CSV jednotlivě.

![Vykreslete #21][21]

Můžete použít vlastní data. Pokud se data v místním počítači v aplikaci skutečných, můžete dál používat AzCopy odeslání místních dat do vašeho soukromé úložišti objektů blob Azure. Potřebujete změnit umístění **zdroje** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, AzCopy příkazu soubor skriptu prostředí PowerShell místní adresář, který obsahuje vaše data.


>[AZURE.TIP] Pokud už vaše data v aplikaci skutečných v úložišti objektů blob Azure soukromé, můžete přeskočte na krok AzCopy skript Powershellu a přímo odeslat data do Azure SQL DW. To vyžaduje další úpravy obrazovky se skriptem pro přizpůsobení formátu dat.


Tento skript Powershellu taky připojuje Azure SQL DW informace do datových souborů příklad průzkum SQLDW_Explorations.sql SQLDW_Explorations.ipynb a SQLDW_Explorations_Scripts.py tak, aby tyto tři soubory připraveni být vyzkoušeli hned po dokončení skript Powershellu.

Po úspěšném spuštění, zobrazí se obrazovka jako níže:

![][20]

## <a name="dbexplore"></a>Průzkum a funkcí analýzy v Azure SQL datový sklad

V této části doporučujeme provést průzkum a funkce generování provádění dotazů SQL Azure SQL DW přímo pomocí **Aplikace Visual Studio datové nástroje**. Všechny dotazy SQL použitý v této části najdete v ukázkové skript s názvem *SQLDW_Explorations.sql*. Tento soubor už stažené do místního adresáře skript Powershellu. Můžete také zadat ji z [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Ale soubor v Github neobsahuje zapojené informace Azure SQL DW.

Připojení k vaší DW SQL Azure pomocí aplikace Visual Studio s SQL DW přihlašovací jméno a heslo a otevřete **Průzkumníka objektu SQL** a potvrďte, že jste importovali databázi a tabulky. Načtení souboru *SQLDW_Explorations.sql* .

>[AZURE.NOTE] K otevření editoru dotazů paralelní datový sklad (PDW), použijte příkaz **Nový dotaz** v **Průzkumníkovi objektu SQL**se zapnutým vaší PDW. Standardní SQL editor dotazů se nepodporuje PDW.

Tady je typ dat provádět průzkum a funkce generování úkoly v této části:

- Zkoumání dat distribuce několika polí v různých systému windows.
- Prozkoumejte kvality dat polí šířky a délky.
- Binární a multiclass klasifikace popisky na základě generovat **tip\_částka**.
- Generování funkcí a výpočetním/porovnat vzdálenosti ze služební cesty.
- Spojení dvou tabulek a extrahujte výběrová, který bude sloužit k vytváření modelů.

### <a name="data-import-verification"></a>Import ověření dat

Tyto dotazy poskytovat rychlé ověření počet řádků a import sloupce v tabulkách vyplněné dříve pomocí hromadného paralelní Polybase společnosti

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Výstup:** By měla získat 173,179,759 řádků a sloupců 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Průzkum: Distribuce ze služební cesty Medailon

Tento příklad dotazu označuje medallions (taxi čísla), které dokončeny víc než 100 zpráv v rámci určeného časového období. Dotaz byste měli využívat rozdělený tabulky aplikace access od stabilizuje schématem oddílu z **odběru\_data a času**. Dotazování celou sadu také bude pomocí oddílů tabulky a/nebo indexovat naskenovaný obrázek.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Výstup:** Dotaz má vracejí tabulku s řádky určující 13,369 medallions (taxi) a počet jejích ze služební cesty udělat v 2013. Poslední sloupec obsahuje počet cesty, dokončení.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Průzkum: Rozdělení ze služební cesty Medailon a hack_license

V tomto příkladu označuje medallions (taxi čísla) a hack_license čísla (ovladače) dokončení více než 100 zpráv v rámci určeného časového období.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Výstup:** Dotaz má vracejí tabulku s 13,369 řádky určující 13,369 auta/ovladači ID, které jste dokončili další této 100 cesty v 2013. Poslední sloupec obsahuje počet cesty, dokončení.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Hodnocení kvality dat: ověření záznamů pomocí nesprávné šířky a/nebo šířky

V tomto příkladu kontroluje Pokud žádné z polí šířky a/nebo šířky buď obsahují neplatnou hodnotu (radián stupňů by měl být mezi -90 a 90), nebo mají (0; 0) souřadnice.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Výstup:** Dotaz vrátí 837,467 cesty, které obsahují neplatné šířky a/nebo šířky pole.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Průzkum: Vysypávány porovnání není šikmý cest rozdělení

V tomto příkladu najde počet cesty, které byly vysypávány porovnání číslo, které nebyly vysypávány v určeném časovém období (nebo v celé datové překrývajícím úplný rok, jako je tady nastaven-li). Toto rozdělení odráží rozdělení binární štítku pro pozdější použití pro modelování binární klasifikace.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Výstup:** Dotaz měly vrátit následující frekvencí tip pro rok 2013: 90,447,622 vysypávány a vysypávány není 82,264,709.

### <a name="exploration-tip-classrange-distribution"></a>Průzkum: Tip třídy/oblast rozdělení

V tomto příkladu vypočítá hodnotu rozdělení tip oblastí v dané časové období (nebo v celé datovou sadu Pokud vztahující se na úplný rok). Toto je rozdělení popisek třídy, které se použije později pro modelování multiclass klasifikace.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Výstup:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Porovnání vzdálenost ze služební cesty a průzkum: Výpočet

Tento příklad převede délky odběru a odkládací a šířky na SQL geography bodů vypočítá vzdálenost ze služební cesty pomocí SQL geography body rozdíl a vrátí náhodné vzorek výsledky porovnání. V příkladu omezuje výsledky platné souřadnice pouze pomocí dotazu hodnocení kvality dat uvedených výše.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Inženýrské funkce pomocí funkce jazyka SQL

Funkce jazyka SQL může někdy být efektivně možnost pro inženýrské funkce. V tomto návodu byla definována funkce SQL pro výpočet přímé vzdálenost mezi odběru a dropoff umístění. Spusťte následující skripty SQL ve **Visual Studiu datové nástroje**.

Tady je skript SQL, který definuje funkce vzdálenost.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Tady je příklad volání této funkce můžete generovat funkcí v dotazu SQL:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Výstup:** Tento dotaz vytvoří tabulku (s 2,803,538 řádky) s odběru a dropoff zeměpisná šířka a délka a odpovídající přímé vzdálenosti v mil. Tady jsou výsledky pro první 3 řádky:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Příprava dat pro vytváření modelů

Následující dotaz spojení **nyctaxi\_ze služební cesty** a **nyctaxi\_tarif** tabulky, vytvoří binární klasifikace popisek **vysypávány**, popisku více třídy klasifikace **tip\_třídy**a extrahuje výběru z celé spojených datovou sadu. Odběr vystavila společnost načítání podmnožinu cest na základě výdeje času.  Tento dotaz můžete zkopírovat a vložit přímo v [Azure počítače výukové Studio](https://studio.azureml.net) [Importovat Data] [ import-data] modul pro požití přímé dat z databáze instanci systému SQL v Azure. Dotaz vyloučí záznamů pomocí nesprávné (0; 0) souřadnice.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Jakmile budete připraveni a přejděte na výukové Azure počítače, můžete se buď:  

1. Uložení konečný dotaz SQL zobrazený extrahovat vzorek data a kopírování vkládání dotaz přímo do [Importovat Data] [ import-data] modul Azure počítač přečíst nebo
2. Zachování vybraných a analýzou dat chcete použít pro vytvoření nové tabulky SQL DW modelu a použití nové tabulky v okně [Importovat Data] [ import-data] modul Azure počítač přečíst. Skript Powershellu v předchozím kroku to bude hotové, za vás. Si můžete přečíst přímo z této tabulky v modulu importovat Data.


## <a name="ipnb"></a>Průzkum a inženýrské funkce IPython poznámkového bloku

V této části jsme provede průzkum a funkce generování pomocí obou Python a SQL dotazů SQL DW vytvořili. Ukázka IPython Poznámkový blok s názvem **SQLDW_Explorations.ipynb** a soubor skriptu Python **SQLDW_Explorations_Scripts.py** jste stáhli místního adresáře. Jsou k dispozici na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Tyto dva soubory jsou stejné v Python skriptů. Soubor skriptu Python je k dispozici pro vás v případě, že nemáte server IPython Poznámkový blok. V části **Python 2.7**jsou určeny tyto dva ukázkové Python soubory.

Azure SQL DW potřebné informace ve výběru IPython Poznámkový blok a soubor skriptu Python stahovat do místního počítače má byla zapojené skript Powershellu dříve. Jsou spustitelný bez nutnosti jakékoli úpravy.

Pokud už jste nastavili pracovního prostoru AzureML, můžete přímo nahrajte ukázku Poznámkový blok IPython služby AzureML IPython Poznámkový blok a spuštění ho. Tady je postup nahrajte Poznámkový blok IPython AzureML služby:

1. Přihlášení do pracovního prostoru AzureML, klikněte na "Studio" nahoře a klikněte na "Poznámkové BLOKY" na levé straně webové stránky.

    ![Vykreslete #22][22]

2. Klikněte na "Nový" v levém dolním rohu obrazovky na webovou stránku a vyberte "Python 2". Potom zadejte název poznámkového bloku a zaškrtněte políčko Vytvořit nový prázdný IPython Poznámkový blok.

    ![Vykreslete #23][23]

3. Klikněte na symbol "Jupyter" v levém horním rohu nový poznámkový blok IPython.

    ![Vykreslete #24][24]

4. Přetahování ukázkové IPython poznámkového bloku na stránce **stromové** služby AzureML IPython Poznámkový blok a klikněte na **Odeslat**. Potom vzorek IPython Poznámkový blok nahraje ke službě AzureML IPython Poznámkový blok.

    ![Vykreslete #25][25]

Chcete-li spustit vzorku IPython Poznámkový blok nebo Python skriptu souboru, následující Python balíčků jsou potřebné. Pokud používáte službu AzureML IPython Poznámkový blok, byly tyto balíčky předinstalovaný.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Doporučené pořadí při vytváření pokročilé analýzy řešení na AzureML s velkými daty je následující:

- Přečtěte si v malého vzorového data do rámečku dat v paměti.
- Proveďte několik vizualizací a explorations z vybraných dat.
- Inženýrské funkce pomocí vybraných dat vyzkoušet.
- Pro větší průzkum, manipulaci s daty a inženýrské funkce umožňuje Python problému SQL dotazů přímo SQL DW.
- Rozhodněte, velikost hodnoty je vhodné pro vytváření modelů výukové počítače Azure.

Těchto hodnot jsou několik průzkum vizualizaci dat a funkce konstrukce příklady. Další explorations dat najdete v ukázce IPython Poznámkový blok a ukázkový soubor skriptu Python.

### <a name="initialize-database-credentials"></a>Inicializace přihlašovací údaje pro databázi

Inicializace nastavení připojení k databázi v následujících proměnných:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Vytvoření připojení k databázi

Tady je připojovací řetězec, který vytvoří připojení k databázi.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sestava počet řádků a sloupců v tabulce < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Celkový počet řádků = 173179759  
- Celkový počet sloupců = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Sestava počet řádků a sloupců v tabulce < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Celkový počet řádků = 173179759  
- Celkový počet sloupců = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Čtení se změnami ukázku malé dat z databáze SQL datový sklad

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Přečtěte si, že ukázkové tabulky je 14.096495 sekund, než je čas.  
Počet řádků a sloupců načtené = (1000: 21).

### <a name="descriptive-statistics"></a>Analytický nástroj Popisná statistika

Teď jste připraveni na prozkoumání vybraných dat. Budeme začínat prohlížíte některé Popisná statistika pro **ze služební cesty\_vzdálenost** (nebo jiným polím určíte).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Vizualizace: Příklad grafu pole

Další podíváme na Krabicový vizualizace quantiles vzdálenost od ze služební cesty.

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslete #1][1]

### <a name="visualization-distribution-plot-example"></a>Vizualizace: Příklad grafu rozdělení

Pozemků, která vizualizuje rozdělení a histogram vzdálenosti vybraných ze služební cesty.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslete #2][2]

### <a name="visualization-bar-and-line-plots"></a>Vizualizace: Pruhové a vykreslí řádku

V tomto příkladu jsme intervalu ze služební cesty vzdálenost do pět intervalů a vizualizovat binning výsledky.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Můžeme vykreslete výše rozdělení Koš na panelu nebo spojnicový graf se:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslete #3][3]

a

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslete #4][4]

### <a name="visualization-scatterplot-examples"></a>Vizualizace: Příklady Scatterplot

Ukážeme bodového grafu mezi **ze služební cesty\_čas\_v\_sekundy** a **ze služební cesty\_vzdálenost** zobrazíte, pokud je libovolné korelace

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslete #6][6]

Podobně jsme můžete zkontrolovat vztah mezi **sazba\_kód** a **ze služební cesty\_vzdálenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslete #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Průzkum vybraných dat pomocí dotazy SQL v poznámkovém bloku IPython

V této části jsme zkoumat data distribuce pomocí vybraných dat, která je zachován do nové tabulky, kterou jsme vytvořili nad. Všimněte si, že podobné explorations lze provést pomocí původní tabulky.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Průzkum: Sestava počet řádků a sloupců v tabulce vzorky

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Průzkum: Vysypávány/není přerušovačů rozdělení.

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Průzkum: Tip třídy rozdělení

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Průzkum: Vykreslete rozdělení tip podle předmětu

    tip_class_dist['tip_freq'].plot(kind='bar')

![Vykreslete #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Průzkum: Denní rozdělení cest

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Průzkum: Cesta rozdělení za Medailon

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Průzkum: Rozdělení ze služební cesty Medailon a napadení licencí

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Průzkum: Distribuce doby ze služební cesty

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Průzkum: Rozdělení vzdálenost ze služební cesty

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Průzkum: Distribuce typ platby

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Ověření formuláři konečný featurized tabulky

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Vytváření modelů Azure počítač přečíst

Teď připraveni přejít k vytváření modelů a nasazení modelu [Azure počítač](https://studio.azureml.net)přečíst. Data je připravená k použití v některém z předpovědí problémy identifikovat dříve, a to:

1. **Binární klasifikace**: odhadnout též tip byl zaplacené výletu.

2. **Multiclass klasifikace**: odhadnout oblast tip zaplacený podle dříve definované třídy.

3. **Analytický nástroj Regrese úkolu**: odhadnout částka zaplacená za výletu tip.  



Zahájíte cvičení modelování přihlášení do pracovního prostoru **Výukové počítače Azure** . Pokud jste ještě nevytvořili výukové pracovního prostoru do počítače, najdete v článku [Vytvoření pracovního prostoru Azure ML](machine-learning-create-workspace.md).

1. Pokud chcete začít pracovat s Azure počítače výuka, přečtěte si téma [Co je Azure počítače výukové Studio?](machine-learning-what-is-ml-studio.md)

2. Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net).

3. Na domovskou stránku Studio obsahuje na množství informací, videa, odkazy a výukové programy pro na odkaz moduly a další zdroje. Další informace o Azure počítače výuka použijte [Azure počítače výukové si přečtěte následující dokumentaci Centrum](https://azure.microsoft.com/documentation/services/machine-learning/).

Typické školení experimentovat se skládá z následujících kroků:

1. Vytvoření **+ Nový** testu.
2. Přístup k datům do Azure ML.
3. Předem procesu, transformace a pracovat s data podle potřeby.
4. Funkce generovat podle potřeby.
5. Rozdělení data na školení / / testování ověření datových sad (nebo samostatné datové sady pro jednotlivá pole).
6. Vyberte jeden nebo více algoritmů výukové počítače v závislosti na výukové problém vyřešit. Například binární klasifikace multiclass klasifikace, regresní.
7. Školení jednoho nebo více modelů pomocí školení datovou sadu.
8. Skóre ověření datovou sadu pomocí školení modely.
9. Vyhodnocení modely pro výpočet relevantní metriky výukové problém.
10. Graf doladit modely a vyberte nejlepší modelu pro nasazení.

V tomto cvičení jsme jste již prozkoumat a analýzou dat v SQL datový sklad a rozhodli, že na velikost hodnoty jedí v Azure ML. Tady je postup, můžete vytvořit jednu nebo více modelů prediktivní vkládání:

1. Přístup k datům do ML Azure pomocí [Importovat Data] [ import-data] modul, k dispozici v části **Data vstupní a výstupní** . Další informace najdete v tématu [Import dat] [ import-data] modul referenční stránku.

    ![Azure ML importovat Data][17]

2. Vyberte **Databázi SQL Azure** jako **zdroj dat** na panelu **Vlastnosti** .

3. Zadejte název databáze DNS v poli **název databázového serveru** . Formát:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Zadejte **název databáze** do odpovídajícího pole.

5. Zadejte *SQL uživatelské jméno* do pole **název serveru uživatelského účtu**a *heslo* do pole **heslo uživatelského účtu serveru**.

6. Zkontrolujte možnosti **Přijmout všechny certifikát serveru** .

7. V oblasti **databázových dotazů** úprav textu vzorky vložit dotaz, který extrahuje takové databáze (včetně polí všechny počítanou například popisky) a dolů data, která chcete velikost požadované hodnoty.

Na následujícím obrázku je příklad binární klasifikace experiment čtení dat přímo z databáze SQL datový sklad (Nezapomeňte nahradit nyctaxi_trip názvy tabulky a nyctaxi_fare název schématu a názvy tabulek jste použili v vaší návodu). Podobně jako pokusy lze vytvořit pro multiclass klasifikaci a regresní problémy.

![Azure ML vlaku][10]

> [AZURE.IMPORTANT] Modelování dat extrakci a odběr příklady dotazu k dispozici v předchozích částech **všechny popisky cvičení tři modelování zahrnuté v dotazu**. Důležitým krokem (požadováno) ve všech modelování cvičení je **vyloučit** nepotřebných štítků pro další dvě problémy a všechny ostatní **nevrací cílové**. Třeba při použití binární klasifikace pomocí popisek **vysypávány** a vyloučit pole **tip\_třídy**, **tip\_částka**, a **celkové\_částka**. Jsou cílové nevrací od znamenat tip vyplacena.
>
> Vyloučení zbytečných sloupců nebo obrázku nevrací, mohli byste použít [Vybrat sloupce v sadě dat] [ select-columns] modul nebo [Upravit Metadata][edit-metadata]. Další informace najdete v tématu [Vybrat sloupce v sadě dat] [ select-columns] a [Upravit Metadata] [ edit-metadata] odkazovat stránky.

## <a name="mldeploy"></a>Nasazení modely Azure počítač přečíst

Pokud modelu je připravená, můžete ho nasadit snadno jako webové služby přímo z testu. Další informace o nasazení Azure ML webové služby najdete v článku [nasazení webové služby Azure počítače výukové](machine-learning-publish-a-machine-learning-web-service.md).

Abyste mohli nasadit nové webové služby, musíte:

1. Vytvoření bodování testu.
2. Nasazení webové služby.

Pokud chcete vytvořit bodování experiment experiment školení **Dokončeno** , klikněte na **Vytvořit BODOVÁNÍ EXPERIMENTOVAT** na dolním panelu akcí.

![Azure bodování][18]

Azure výukové počítače se pokusí vytvořit bodování experiment podle komponent školení testu. Zejména toto:

1. Školení model uložte a odeberte moduly školení modelu.
2. Určete logické **vstupní port** představující schématu očekávaná vstupní data.
3. Určete logické **výstupní port** představující schéma výstupu očekávané webové služby.

Po vytvoření bodování testu zkontrolovat a zkontrolujte jezdcem jas. Typické úpravy je nahrazovat vstupní datovou sadu a/nebo dotaz s jednou, které vyloučením pole popisků tyto nebudou dostupné při volání služby. Také je vhodné pokusit se zmenšit velikost vstupní datovou sadu a/nebo dotaz s několika záznamy, právě dost označíte schématu zadávání. Číslo portu výstup není společné vyloučit všech vstupních polí a pouze zahrnout **Dosáhne popisky** a **Dosáhne pravděpodobnosti** do výstupu pomocí [Vybrat sloupce v sadě dat] [ select-columns] modul.

Ukázka bodování experiment je k dispozici v následujícím obrázku. Až budete připraveni nasazení, klikněte na tlačítko **Publikovat webové služby** na dolním panelu akcí.

![Azure ML publikování][11]


## <a name="summary"></a>Souhrn
K recap, co jsme vytvořili v tomto kurzu návod, nevytvoříte prostředí pro výzkum Azure dat spolupracovali velké veřejné datovou sadu, trvá procesem týmu dat pro výzkum, úplně od získávání dat k modelování školení a nasazení webové služby Azure počítače výukové.

### <a name="license-information"></a>Informace o licenci

Tento návod vzorku a jeho doprovodným skripty a IPython notebook(s) jsou sdíleny Microsoft klikněte v části licence MIT. Zkontrolujte soubor LICENSE.txt v adresáři ukázek kódu na GitHub další podrobnosti.

## <a name="references"></a>Odkazy

• [Andrés Monroy NYC Taxi cest stáhnout stránky](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing společnosti NYC Taxi ze služební cesty Data podle Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi a Limousine provize výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
