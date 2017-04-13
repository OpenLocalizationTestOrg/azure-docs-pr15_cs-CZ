<properties
    pageTitle="Scalable vědy dat v Azure dat jezera: návod začátku do konce | Microsoft Azure"
    description="Jak používat jezera dat Azure dělat průzkum a binární klasifikace úkoly dat na datovou sadu."  
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
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Scalable vědy dat v Azure dat jezera: návod začátku do konce

Tento návod ukazuje, jak používat jezera dat Azure na průzkum data a binární klasifikace úkoly výběru cesty taxi NYC a jízdenky datovou sadu odhadnout též věnuje tip tarif. Vás provede jednotlivými kroky [Procesu vědy dat týmu](http://aka.ms/datascienceprocess)začátku do konce, z získávání dat k modelování školení a nasazení webové služby, který publikuje modelu.


### <a name="azure-data-lake-analytics"></a>Azure dat jezera analýzy

[Microsoft Azure dat jezera](https://azure.microsoft.com/solutions/data-lake/) obsahuje všechny možnosti muset Usnadněte vědeckých dat pro uložení dat velikosti, obrazce a rychlosti a k provedení zpracování dat, pokročilé technologie pro analýzu a automatické učení modelování s vysokou škálovatelnost v efektivní.   Platíte na základě na úlohu pouze v případě, že data ve skutečnosti zpracovávání. Azure jezera analýzy dat obsahuje U SQL, jazyk, který prolnutí deklarativní přírodní SQL výrazové silou C# poskytnout scalable distributed funkce dotazu. Umožňuje proces Nestrukturovaná data použitím schématu na pro čtení, vložit vlastní logiky a funkce definované uživatelem (UDF) a obsahuje rozšiřitelnost povolit graf nebo barev publikum nemůže ovládat způsobu spuštění ve velkém měřítku. Další informace o návrhu poznámky filozofie za U SQL, najdete v článku [Visual Studio blogu](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Jezera analýzy dat je také klíčové součástí sady Cortana technologie pro analýzu a spolupracuje datový sklad SQL Azure, Power BI a Data Factory. To vám dá dokončení cloudu velkých dat a platformy pokročilé technologie pro analýzu.

Tento návod začíná popisující požadavky a zdroje informací, které jsou potřebné k dokončení úkolů pomocí analýzy jezera dat, která formuláře procesu dat pro výzkum a jak je nainstalovat. Popisuje kroky zpracování dat pomocí U SQL a uzavře znázorňující, jak používat Python a podregistru s Azure počítače výukové Studio sestavovat a nasazovat prediktivní modely. 


### <a name="u-sql-and-visual-studio"></a>U SQL a Visual Studio

Tento návod doporučuje používat Visual Studio upravovat skripty U SQL zpracuje datové sady. U SQL skripty jsou popsané a podle do samostatného souboru. Proces zahrnuje ingesting, zkoumání a odběr data. Také ukazuje, jak úlohu U SQL skriptovány z portálu Microsoft Azure. Podregistru tabulky vytvořené data z přidružené clusteru HDInsight usnadnit budovy a nasazení modelu binární klasifikace v Azure počítače výukové Studio.  


### <a name="python"></a>Python

Tento návod obsahuje také oddíl, který ukazuje, jak sestavovat a nasazovat prediktivní modelu pomocí Python s Azure počítače výukové Studio.  Poznámkový blok Jupyter poskytneme skripty Python k provedení těchto kroků v tomto procesu. Poznámkový blok obsahuje kód pro některé další funkce technickým kroky a modely stavbu například multiclass klasifikaci a regresní modelování kromě modelu binární klasifikace zde uváděného. Regresní úkolu je předpovídání množství tip založený na dalších funkcí tip. 


### <a name="azure-machine-learning"></a>Výukové Azure počítače
Azure Studio výukové počítač používá sestavovat a nasazovat prediktivní modely. Důvodem je pomocí dvou přístupy: nejdřív Python skripty a potom s tabulkami podregistru clusteru HDInsight (Hadoop).


### <a name="scripts"></a>Skriptů

Pouze hlavní kroky jsou uvedené v tomto návodu. Úplné **skript U SQL** a **Jupyter Poznámkový blok** můžete stáhnout z [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete tato témata, musíte mít takto:

- Předplatné Azure. Pokud už nemáte jeden, najdete v článku [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Doporučené] Visual Studio 2013 nebo 2015. Pokud už nemáte jeden z těchto verzí, můžete si stáhnout bezplatné edition komunity z [tady](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Klikněte na tlačítko **Stáhnout 2015 komunity** části Visual Studio. 

>[AZURE.NOTE] Místo Visual Studio můžete také portálu Azure můžou odeslat jezera dat Azure dotazů. Poskytneme pokyny o tom, jak to udělat i s Visual Studiu a na portálu v oddílu s názvem **data procesu s U-SQL**. 

- Registrace náhled jezera Azure dat

>[AZURE.NOTE] Potřebujete k získání schválení použití Azure dat jezera úložiště přihlašovacích údajů (ADLS) a Azure dat jezera analýzy (ADLA), jsou tyto služby ve verzi preview. Zobrazí se výzva k registraci při vytváření prvního ADLS nebo ADLA. Chcete-li sigh nahoru, klepněte na **přihlásit k zobrazení náhledu**, přečtěte si smlouvu a klikněte na **OK**. Tady je například ADLS stránce registrace:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Příprava dat pro výzkum prostředí pro jezera dat Azure
Příprava dat pro výzkum prostředí pro tento návod, vytvořte v následujících zdrojích:

- Azure dat jezera úložiště přihlašovacích údajů (ADLS) 
- Azure dat jezera analýzy (ADLA)
- Účet úložiště objektů Blob Azure
- Účet počítače výukové Studio Azure
- Azure datové nástroje jezera for Visual Studio (doporučeno)

Tato část obsahuje pokyny k vytváření každý z těchto prostředků. Pokud se rozhodnete sdělit nám podregistru tabulek s Azure počítače výuka, místo Python, vytvoření modelu taky musíte zřízení clusteru HDInsight (Hadoop). Postup alternativní v popsané v následující části.
<br/>
>AZURE. Poznámka: **Úložiště jezera dat Azure** lze vytvořit buď samostatně nebo při vytváření **Analýz jezera dat Azure** jako výchozí úložiště. Pokyny jsou uvedeny pro vytváření každý z těchto prostředků samostatně níže, ale jezera datový úložiště účtu nemusí vytvořený zvlášť.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Vytvoření úložiště jezera dat Azure

Vytvoření ADLS z [Azure portálu](http://portal.azure.com). Podrobnosti najdete v tématu [Vytvoření HDInsight obrázku s úložiště jezera dat pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Ujistěte se, že nastavení identitu AAD obrázku ve **zdroji dat** zásuvné zásuvné **Volitelná konfigurace** popsanou. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Vytvořte účet Azure dat jezera analýzy
Vytvořte účet ADLA z [Portálu Azure](http://portal.azure.com). Podrobnosti najdete v tématu [kurz: Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Vytvoření účtu úložiště objektů Blob Azure
Vytvořte účet úložiště objektů Blob Azure z [Portálu Azure](http://portal.azure.com). Další informace najdete v tématu Vytvoření oddíl účtu úložiště [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Nastavit účet Azure počítače výukové Studio
Na stránce [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) nejdřív se př nahoru nebo do výukového Studio Azure počítače. Klikněte na tlačítko **Začít nyní** a pak zvolte "Volného prostoru" nebo "Standardní pracovní prostor". Po uplynutí tohoto budou moct vytvářet pokusů v Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Instalace nástroje jezera Azure dat [doporučené]
Instalace nástroje jezera dat Azure pro vaši verzi aplikace Visual Studio nástrojích [jezera dat Azure for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Po úspěšném dokončení instalace otevřete aplikaci Visual Studio. Měli byste vidět jezera dat tabulátorem na nabídku v horní. Azure zdroje by se měly v levém panelu při přihlašování ke svému účtu Azure.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Datovou sadu NYC Taxi cest
Sadu dat, kterou jsme použili tady je veřejně dostupný datovou sadu – [datovou sadu NYC Taxi cest](http://www.andresmh.com/nyctaxitrips/). Data ze služební cesty Taxi NYC sestává z o 20GB komprimované soubory CSV (nekomprimované ~ 48GB), nahrávání 173 milionů jednotlivé cest a tarify zaplacený při každé ze služební cesty. Každý záznam ze služební cesty obsahuje odběru a odkládací umístění a čas, anonymních napadení (ovladač) číslo licence a číslo Medailon (společnosti taxi jedinečné id). Data za rok 2013 zahrnuje všechny cesty a pro každý měsíc není uvedený v následující dvě datové sady:

 - "Trip_data" CSV obsahuje podrobnosti o ze služební cesty, jako je třeba počet cestující odběru a dropoff body, doba trvání cesty a délka ze služební cesty. Tady je pár ukázkových záznamů:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - "trip_fare" CSV obsahuje podrobnosti o tarif zaplacený při každé ze služební cesty, jako je typ platby, tarif částka, přirážky daně, tipy a mýtné a celkovou částku zaplacenou. Tady je pár ukázkových záznamů:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinečný klíč připojení ze služební cesty\_dat a ze služební cesty\_tarif se skládá z následujících tří polí: medailonu, napadení\_licence a odběru\_data a času. Jako nezpracovaná soubory CSV můžete k nim získat přístup z veřejné Azure úložiště objektů blob. Skript U jazyka SQL pro tento spojení je v části [tabulky ze služební cesty a tarif spojení](#join) .

## <a name="process-data-with-u-sql"></a>Zpracování dat s U SQL

Zpracování dat úkoly popsané v této části zahrnout ingesting kontrola kvality, zkoumání a odběr data. Jsme také předvedení spojení tabulek ze služební cesty a tarif. Poslední část zobrazuje spustit skripty úlohu U SQL z portálu Microsoft Azure. Tady jsou odkazy na jednotlivé dílčí:

- [Požití dat: další data z veřejného objektů blob](#ingest)
- [Kontroly kvality dat](#quality)
- [Průzkum dat](#explore)
- [Spojení tabulek ze služební cesty a tarif](#join)
- [Analytický nástroj vzorkování dat](#sample)
- [Spuštění úlohy U SQL](#run)

U SQL skripty jsou popsané a podle do samostatného souboru. Úplné **skripty U SQL** si můžete stáhnout z [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

K provedení U jazyka SQL, otevřete aplikaci Visual Studio, klikněte na **Soubor--> Nový--> projektu**, zvolte **Projekt U SQL**, názvu a uložte ho do složky.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Je možné používat portál Azure provést U SQL místo Visual Studio. Můžete přejít ke zdroji analýzy jezera Azure dat na portálu a odeslat dotazy přímo jako ilustrovaný na následujícím obrázku.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Požití dat: V data z veřejného objektů blob číst

Umístění dat v objektů blob Azure uváděný jako **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** a lze extrahovat pomocí **Extractors.Csv()**. Nahraďte vlastní jméno container a název účtu úložiště v následujících skriptů pro container_name@blob_storage_account_name wasb adresu. Protože názvy souborů do stejné formátování, můžete používáme **ze služební cesty\_data_ {\*\}.csv** zobrazíte všechny soubory 12 ze služební cesty. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Protože jsou v prvním řádku záhlaví, potřebujeme odebrat záhlaví a změnit typy sloupců do příslušných políček. Můžeme buď uložit zpracovaných dat Azure jezera úložný prostor pomocí **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ nebo k úložišti objektů Blob Azure účet pomocí **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Podobně jsme může číst ve výše uvedené množiny dat tarif. Klikněte pravým tlačítkem myši úložiště jezera dat Azure, můžete se podívat, dat v **portálu Azure--> Průzkumník dat** nebo **Průzkumníka souborů** aplikace Visual Studio. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Kontroly kvality dat

Po tabulek ze služební cesty a tarif četl v, kontroly kvality dat se teď dá následujícím způsobem. Výsledné soubory CSV může být výstup k úložišti objektů Blob Azure nebo úložiště jezera dat Azure. 

Získat počet medallions a jedinečné číslo medallions:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Vyhledání tyto medallions, ve kterých víc než 100 cest:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Najděte záznamy neplatné z hlediska pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Vyhledejte chybějící hodnoty pro některé proměnné:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Průzkum dat

Můžeme některé průzkum Chcete-li lépe porozumět data.

Vyhledání rozdělení šikmý a šikmý cest:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Vyhledání rozdělení částka tip s mezní hodnoty: 0,5,10 a 20 korun.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Vyhledání základní statistiky ze služební cesty distance (vzdálenost):

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Vyhledání percentily ze služební cesty distance (vzdálenost):

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Spojení tabulek ze služební cesty a tarif

Ze služební cesty a tarif tabulky můžete spojeny Medailon, hack_license a pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Pro každou úroveň osobní počet Vypočítejte počet záznamů tip Průměrná částka, rozptyl argumentu tip množství, procenta šikmý cest.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Analytický nástroj vzorkování dat

Nejdřív jsme náhodně vyberte 0,1 % dat z spojených tabulky:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Jsme proveďte vrstveného odběr tak, že binární proměnné tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Spuštění úlohy U SQL

Po dokončení úprav skripty U SQL, odešlete je na webu pomocí účtu jezera analýzy dat Azure. Klikněte **Jezera dat**, **Odešlete projektu**, vyberte svůj **Účet analýzy**, zvolte **paralelismus**a klikněte na tlačítko **Odeslat** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Jakmile projektu je úspěšně splněny, stav úlohy zobrazí se ve Visual Studiu pro sledování. Po dokončení projektu můžete dokonce přehrát proces spuštění úlohy a zjistíte postup kritický efektivitu projektu. Můžete taky přejít na portál Kontrola stavu úloh U SQL Azure.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Teď můžete zkontrolovat, že výstupní soubory v úložišti objektů Blob Azure nebo portál Azure. Použijeme vrstveného ukázkových dat pro naše modelování v dalším kroku.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Vytvořte a nasaďte modely Azure počítač přečíst

Jsme ukazují dvě možnosti k dispozici můžete přidávat data do Azure počítače výukové vytvářet a 

- V první možnost používáte vybraných dat vzniku Blob Azure (v předchozím kroku **Data odběr** ) a používáte Python sestavovat a nasazovat modely z Azure počítače učení. 
- Na druhou možnost můžete dotaz na data v Azure dat jezera přímo pomocí podregistru dotazu. Tato možnost vyžaduje vytvoření nového clusteru HDInsight nebo použít existující clusteru HDInsight přejděte na místo, kam tabulky podregistru NY Taxi data v úložišti jezera dat Azure.  Probereme tato obou těchto možností. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Možnost 1: Použití Python sestavovat a nasazovat počítače výukové modely

Sestavovat a nasazovat modelů výukové počítače pomocí Python, vytvořte poznámkový blok Jupyter na místním počítači nebo v Azure počítače výukové Studio. Poznámkový blok Jupyter umístěna na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) obsahuje celou kód pro procházení, vizualizace dat, funkce inženýrské, modelování a nasazení. V tomto článku ukážeme jenom modelování a nasazení. 

### <a name="import-python-libraries"></a>Import Python knihoven

Chcete-li spustit vzorku Jupyter Poznámkový blok nebo Python skriptu souboru, následující Python balíčků jsou potřebné. Pokud používáte službu AzureML Poznámkový blok, byly tyto balíčky předinstalovanou.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Přečtěte si v data z objektů blob

- Připojovací řetězec   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Přečtěte si ve formátu textu.

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Přidání názvů sloupců a oddělte sloupců

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Změnit některé sloupce na numerické

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Vytváření modelů výukové počítače

Tady abychom vytvářet modelu binární klasifikace odhadnout, zda je výletu vysypávány nebo ne. V poznámkovém bloku Jupyter můžete najít další dva modely: multiclass klasifikaci a regresní modely.

- Nejdřív potřeba vytvořit formální proměnných, které lze použít v scikit – další modely

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Vytvořit pro modelování dat rámeček

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Školení a testování 60-40 rozdělení

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistickou regresní množiny školení

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Skóre testování uvedenou množinu dat

        Y_test_pred = logit_fit.predict(X_test)

- Výpočet metriky hodnocení

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Vytvoření rozhraní API webových služeb a používání v Python

Chceme umožňují počítače výukové modelu po byla vytvořena. Tady používáme binární logistickou modelu jako příklad. Zkontrolujte, že scikit – informace v místním počítači verze 0.15.1. Nemusíte starat o to, pokud používáte službu Azure ML studio.

- Najděte svoje přihlašovací údaje pracovního prostoru z Azure ML studio nastavení. V Azure počítače výukové Studio, klikněte na **Nastavení** --> **název** --> **Tokeny se tak mohli ověřovat**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Vytvoření webové služby

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Získání pověření webové služby

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Zavolejte na linku rozhraní API webových služeb. Budete muset počkat 5 až 10 sekund po předchozím kroku.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Možnost 2: Vytvoření a nasazení modelu přímo v Azure počítače výukové

Azure Studio výukové počítače můžete číst data přímo z úložiště jezera dat Azure a pak lze použít k vytvoření a nasazení modely. Tento postup používá tabulku podregistru, který odkazuje na úložiště jezera dat Azure. Tento postup vyžaduje, aby se zřízení samostatného obrázku Azure Hdinsightu, na které se vytvoří tabulku podregistru. Následující části ukazují, jak to udělat. 

### <a name="create-an-hdinsight-linux-cluster"></a>Vytvoření clusteru Linux HDInsight

Vytvoření clusteru HDInsight (Linux) z [Azure portálu](http://portal.azure.com). Podrobnosti najdete v tématu v [vytvořit HDInsight obrázku s úložiště jezera dat pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)části **Vytvoření HDInsight obrázku s přístupem k úložiště jezera dat Azure** .

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Vytvořit tabulku podregistru do HDInsight

Teď můžeme vytvořit tabulku podregistru se nemusí používat v Studio výukové počítače Azure HDInsight clusteru z dat uložených v úložišti jezera dat Azure v předchozím kroku. Přejděte k obrázku HDInsight právě vytvořili. Klikněte na **Nastavení** --> **Vlastnosti** --> **Clusteru AAD Identity** --> **ADLS přístup**, se ujistěte, váš účet Azure úložišti jezera přidaný v seznamu s dlouhým, psaní a spustit práva. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Klikněte na **řídicí panel** vedle na tlačítko **Nastavení** a bude překryvné okno. Klikněte na **Zobrazení podregistru** v horním pravém horním rohu stránky a uvidí **Editoru dotazů**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Vložte následující skripty podregistru vytvořte tabulku. Umístění zdroje dat se nachází v Azure úložišti jezera odkaz tímto způsobem: **adl://data_lake_store_name.azuredatalakestore.net:443/název_složky/název_souboru**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Po dokončení dotazu, uvidíte výsledky takto:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Vytvořte a nasaďte modely v Azure počítače výukové Studio

Teď jsme připravení sestavovat a nasazovat modelu předpovídá též tip je zaplacené s Azure počítače výukové. Vrstveného ukázkových dat je připravená k použití v binární klasifikace (tip nebo neuchovával) problém. Prediktivní modely pomocí multiclass klasifikace (tip_class) a regresní (tip_amount) taky můžete vytvořené a nasazení s Azure počítače výukové Studio, ale tady pouze ukážeme postup v případě případ použití modelu binární klasifikace.

1. Přístup k datům v Azure ML používání modulu **Importovat Data** k dispozici v části **Data vstupní a výstupní** . Další informace najdete na stránce Přehled [modul importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) .
2. Výběr **Podregistru dotazu** jako **zdroje dat** na panelu **Vlastnosti** .
3. Vložte tento skript podregistru do editoru **podregistru databázových dotazů**

        select * from nyc_stratified_sample;

4. Zadejte identifikátor URI HDInsight obrázku (lze najít Azure portálu), Hadoop přihlašovací údaje, umístění výstupní data a název název / / kontejneru klíčů účet Azure úložiště.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Na následujícím obrázku je uveden příklad binární klasifikace pokusu čtení dat z tabulku podregistru.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Po vytvoření testu klikněte na **Nastavení webové služby** --> **Prediktivní webové služby**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Spuštění automaticky vytvořené bodování testu, když byl dokončen, klikněte na **Nasazení webové služby**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Řídicí panel služeb web zobrazí krátce:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Souhrn

Provedením tohoto návodu nevytvoříte prostředí pro výzkum dat pro vytváření scalable začátku do konce řešení v jezera dat Azure. Toto prostředí byla použita k analýze velké veřejné datovou sadu, přičemž kanonický kroky procesu vědy Data z získávání dat prostřednictvím modelu školení a pak pro nasazení modelu jako webové služby. U SQL použitý k obrázku, zkoumání a přehrajte si data. Python podregistru byly použity a s Azure počítače výukové Studio sestavovat a nasazovat prediktivní modely.

## <a name="whats-next"></a>Co je další krok?

Naučná stezka [Týmu dat pro výzkum obrázku (TDSP)](http://aka.ms/datascienceprocess) obsahuje odkazy na témata popisující každého kroku v procesu pokročilé technologie pro analýzu. Existuje řada návody rozpis na stránce [týmu dat pro výzkum proces návody](data-science-process-walkthroughs.md) , které prezentovat v různých situacích prediktivní technologie pro analýzu používání zdrojů a služeb:

- [Proces týmu dat pro výzkum v akci: pomocí SQL datový sklad](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Proces týmu dat pro výzkum v akci: použití HDInsight Hadoop clusterů](machine-learning-data-science-process-hive-walkthrough.md)
- [Proces pro výzkum dat týmu: pomocí serveru SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Přehled procesu vědy dat pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md)

