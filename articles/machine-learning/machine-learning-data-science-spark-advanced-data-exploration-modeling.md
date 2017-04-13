<properties
    pageTitle="Pokročilé průzkum a modelování pomocí Spark | Microsoft Azure"
    description="Použití HDInsight Spark na průzkum a školení binární klasifikace a regresní modely pomocí křížového ověřování a hyperparameter optimalizace."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Rozšířené průzkum a modelování pomocí Spark 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tento návod používá HDInsight Spark na průzkum a školení binární klasifikaci a regresní modelů pomocí křížového ověřování a optimalizace hyperparameter výběru NYC taxi ze služební cesty a jízdenky datovou sadu 2013. Vás provede jednotlivými kroky [Procesu vědy dat](http://aka.ms/datascienceprocess)začátku do konce, pomocí HDInsight Spark clusteru pro zpracování a objektů BLOB Azure pro ukládání data a modely. Proces prozkoumá vizualizují data přenášet z úložiště objektů Blob Azure a připraví dat za účelem vytvoření prediktivní modely. Python použil kódu řešení a zobrazit relevantní pozemků. Sestavení provádět binární klasifikaci a regresní modelování pomocí nástrojů Spark MLlib jsou tyto modely. 

- **Binární klasifikace** úkolu je předpovídání též tip platí pro cestu. 
- **Regresní** úkolu je předpovídání množství tip založený na dalších funkcí tip. 

Kroky modelování také obsahovat kód znázorňující, jak proškolit, jsou vyhodnoceny a uložení jednotlivých typů modelu. Téma popisuje stejné základu jako téma [průzkum a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) . Ale ho je "složitější" taky používala křížového ověřování ve spojení s hyperparameter komínů jak proškolit optimálně přesné klasifikace a regresní modely. 

**Křížového ověřování (CV)** je postup, který vyhodnocuje chod zobecňuje modelu školení na známé množiny dat na předpovídání funkce datové sady, na které nebylo byla školení. Základní představu za tento postup je modelu školení na datovou sadu známé dat na a pak přesnost jejich předpovědí testováno proti nezávisle na datovou sadu. Běžné implementace používá tady je datovou sadu rozdělit přeložení K a potom proškolit modelu způsobem kruhového ve všech pouze jednu přeložení. 

**Optimalizace Hyperparameter** potížím volba sady hyperparameters algoritmu výukové obvykle s cílem optimalizace měření výkonu na algoritmus na nezávislou sadu dat. **Hyperparameters** jsou zobrazené hodnoty určených mimo postup školení modelu. Předpoklady o tyto hodnoty může být ovlivněné flexibilitu a přesnost modely. Stromové struktury rozhodování máte hyperparameters, například třeba požadovaný název hloubkové nebo počtu ponechá ve stromové struktuře. Podpora vektorové počítačích (SVMs) je nutné nastavení chybnou snížení termínu. 

Běžným způsobem optimalizovat hyperparameter používá tady je mřížka vyhledávání nebo **Možnosti Uklidit parametr**. To je tvořen provádění vyčerpávající vyhledávání pomocí hodnoty zadané podmnožinu hyperparameter místa pro algoritmu výukové. Křížové ověření můžete zadat metriky výkonu seřadíte optimální výsledky vytvořené pomocí algoritmu hledání mřížky. CV spolu se rozsáhlé pomáhá hyperparameter limit problémy, například overfitting modelu dat školení tak, aby modelu zachová kapacity vyrovnat obecné množiny dat, ze kterého byla školení data extrahovaných.

Modely, které budeme používat zahrnutím a spoje lineární regresní, náhodné struktury a přechodu zesílen stromy:

- [Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je lineární regrese modelu, který používá metodu Stochastic přechodu sestup (SGD) a měřítka odhadnout částky tip zaplacený pro optimalizaci a funkce. 
- Regresní [logistickou regresní s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" je regresní model, který lze použít po kategorií dělat dat klasifikace závislé proměnné. LBFGS je algoritmus optimalizace kvazi-Petr, blíží algoritmu Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a, které se běžně používají v počítači výukové.
- [Náhodné strukturami](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety stromové struktury rozhodování.  Že kombinují mnoho stromové struktury rozhodování snížení rizik overfitting. Náhodné strukturami se používají pro regresní a klasifikace zpracovat kategorií funkcí a může se prodloužit nastavení multiclass klasifikace. Není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Náhodné strukturami jsou jednou z nejčastěji úspěšné počítače výukové modelů na verzi pro klasifikaci a regresní.
- [Přechod zesílen stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety stromové struktury rozhodování. GBTs školení stromové struktury rozhodování opakované, aby byl minimalizován funkci ztráty. GBTs se používají pro regresní a klasifikace zpracovat kategorií funkcí, není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Můžete taky používají v nastavení multiclass klasifikace.

Modelování příklady použití CV a Hyperparameter jsou zobrazené možnosti Uklidit problém binární klasifikace. Jednodušší příklady (bez parametrů posunuje) jsou uvedeny v hlavního tématu regresní úkolů. Ale v dodatku potvrzení pružná čistého pro lineární regresi a CV pomocí parametrů možnosti Uklidit pro regresní náhodné doménové prezentovány taky. **Pružná čistého** je vyřešeno regresní metoda přizpůsobení lineární regrese modely lineárně kombinující metriky L1 a L2 jako podle práva [Nepravidelný výběr](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) a [hřeben](https://en.wikipedia.org/wiki/Tikhonov_regularization) metod.   



>[AZURE.NOTE] Ačkoli sada nástrojů Spark MLlib je navržený pro práci v případě velkých sad dat, relativně malého vzorového (pomocí řádky 170K, o 0,1 % původní datovou sadu NYC ~ 30 Mb) je zde použita pro usnadnění. Cvičení uvedeny běží efektivní (asi 10 minut) HDInsight clusteru s 2 uzly kolegy. Téhož kódu s menší změny mohou sloužit k zpracovat větší-sady dat, se příslušné změny pro ukládání do mezipaměti dat v paměti a změna velikosti obrázku.


## <a name="prerequisites"></a>Zjistit předpoklady pro

Potřebujete účet Azure a HDInsight Spark je potřeba HDInsight 3.4 Spark 1,6 clusteru služby k dokončení tohoto postupu. V tématu [Přehled dat vědy pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny o tom, jak tyto požadavky splnit. Toto téma obsahuje také popis NYC 2013 Taxi data použitá v tomto poli a pokyny pro spuštění kódu z poznámkového bloku Jupyter Spark clusteru. **PySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** Poznámkový blok, který obsahuje ukázky v tomto tématu je k dispozici v [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Instalační program: úložišť knihoven a přednastavených Spark kontextu

Spark je možné pro čtení a zápis Azure úložiště objektů Blob (označovaná taky jako WASB). Aby některé z existujících dat uložených tam budou zpracovány pomocí Spark a výsledky znovu uložené v WASB.

Uložit modely nebo souborů v WASB, cestu musí být zadán správně. Výchozí kontejner připojené k obrázku Spark můžete odkázat pomocí cesty začínající: "wasb: / / /". Je odkazováno jiných umístění "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Nastavení adresářové cesty pro úložišť v WASB

Následující příklad kódu určuje umístění data, která chcete číst a cestu k adresáři modelu úložiště, ke které se uloží výstupu modelu:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**VÝSTUP**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Import knihoven

Importujte potřebné knihoven s Tento kód:

    # LOAD PYSPARK LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime
    

### <a name="preset-spark-context-and-pyspark-magics"></a>Přednastavené Spark kontext a PySpark magics

Oříšky PySpark, které jsou k dispozici s poznámkovými bloky Jupyter měli přednastavené kontext. Takže není potřeba nastavit Spark nebo kontexty podregistru explicitně začnete práci s aplikací připravují. Kontexty jsou k dispozici pro ve výchozím nastavení. Kontexty jsou:

- sc - pro Spark 
- sqlContext - pro podregistru

Jádra PySpark obsahuje několik předdefinovaných "magics", které jsou speciální příkazy, které můžete volat s %%. Existují dva tyto příkazy, které se používají v těchto ukázek kódu.

- **%% místní** Určuje, že kód v dalších řádcích se má provádět místně. Kód musí být platná Python kód.
- **%%sql -o <variable name>** Spustí dotaz podregistru proti sqlContext. Je-li parametr -o, je výsledek dotazu zachován v %% místní kontext Python jako Pandas DataFrame.
 

Pro další informace o jádra pro Jupyter poznámkových bloků nebo předdefinované "magics", která poskytují, najdete v článku [jádra umožňující Jupyter poznámkových bloků pomocí HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="data-ingestion-from-public-blob"></a>Požití data z veřejného objektů blob: 

Cílem prvního kroku v procesu vědy dat je jedí data, která chcete analyzovat z zdrojů, kde se nachází na průzkum a prostředí pro modelování. Toto prostředí je Spark v tomto návodu. Tato část obsahuje kód dokončete řadu úkolů:

- jedí ukázkových dat modelovat
- Přečtěte si v zadávání datovou sadu (uložená jako soubor TSV)
- formátování a vymazání data
- vytvářet a ukládat do mezipaměti objektů (RDDs nebo dat rámce) v paměti
- Zaregistrujte ho jako tabulku temp v kontextu SQL.

Tady je kód letiště požití data.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
        
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba k provedení nad buňku: 276.62 sekund


## <a name="data-exploration--visualization"></a>Průzkum dat a vizualizaci 

Jakmile se data má přenesený do Spark, dalším krokem v procesu vědy dat je získat hlubší porozumění dat prostřednictvím průzkum a vizualizace. V této části prozkoumat taxi dat pomocí SQL dotazů jsme vykreslete cílových proměnných a potenciálního funkce vizuální prohlídka. Konkrétně jsme vykreslete četnost osobní počtem v taxi cest četnost tip částky a jak se liší podle částka platby a typ tipy.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Vykreslete histogram osobní počet frekvencí ve výběru taxi cest

Tento kód a následné fragmenty použít k vytvoření dotazu vzorku a místní magické vynesení dat magické SQL.

- **Magické SQL (`%%sql`)** HDInsight PySpark jádra podporuje snadno vložené HiveQL dotazů sqlContext. (-O VARIABLE_NAME) argument ukládá výstup dotazu SQL jako Pandas DataFrame na serveru Jupyter. To znamená, že je k dispozici v místním režimu.
- ** `%%local` Magické** slouží ke spuštění kódu místně na server Jupyter, což je headnode clusteru HDInsight. Obvykle se používá `%%local` magic ve spojení s `%%sql` magic s parametrem – o. Parametr -o by uchovávat výstup dotazu SQL místně a potom %% místní magické by aktivace následujícího fragment kódu místně kontrolovat výstup dotazy SQL, který je zachován místně

Výstup je znázorněn automaticky po spuštění kód.

Tento dotaz načte cest podle počtu osobní. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Tento kód vytvoří místní data rámce z výstupu dotazu a vykreslí data. `%%local` Magické vytvoří místní snímek dat, `sqlResults`, který se dá použít pro zakreslení s matplotlib. 

>[AZURE.NOTE] Tento PySpark magické slouží tisknutím v tomto návodu. Při velké množství dat, měli vzorek vytvoření dat rámeček, který můžete přizpůsobit v místní paměti.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Tady je kód, který chcete vykreslit cest tak, že osobní počty

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**VÝSTUP**

![Četnost cest podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Pomocí tlačítek nabídka **Typ** v poznámkovém bloku, můžete vybrat mezi několika různých typů vizualizací (tabulky, výsečový, spojnicový, oblast nebo panelu). Pruhový graf je zobrazena zde.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Vykreslete histogram tip částek a jak množství tip se liší podle osobní počet a tarif částky.

Použití příkazu jazyka SQL pro ukázková data.
    
    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25
    

Tento kód buňky slouží k vytváření tři pozemků data dotaz SQL zobrazený.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**VÝSTUP:** 

![Tip: rozdělení částky](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tip: částka podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip: Částka tak, že tarif množství](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funkce inženýrské, transformace a dat Příprava modelování

Tato část popisuje a poskytuje kód pro postupy Příprava dat pro použití v modelu ML. Ukazuje, jak provádět následující úkoly:

- Vytvořit novou funkci binning hodin do provozu čas bloky
- Index a na aktivní kódovat kategorií funkcí
- Vytvoření s popisky čárky objektů k zadání vstupních hodnot do ML funkcí
- Vytvoření výběrová dílčí dat a ho rozdělit na školení a testování sady
- Funkce měřítka
- Mezipaměti objektů v paměti


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Vytvořit novou funkci binning hodin do provozu čas bloky

Tento kód ukazuje, jak se dá vytvořit novou funkci binning hodin do bloků čas přenosy a jak mezipaměti rámečku Výsledná data v paměti. Pokud pružné Distributed datových sad (RDDs) nebo jako rámečky dat se používají opakovaně, ukládání do mezipaměti vede k vylepšení čas spuštění. Proto jsme mezipaměti RDDs nebo jako rámečky dat v několika fázích v tomto návodu.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**VÝSTUP**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Index a za běhu jeden kódovat kategorií funkcí

Tato část popisuje index nebo kódování kategorií funkcí k zadání vstupních hodnot do funkcí modelování. Modelování a předpovídání funkce MLlib vyžadují funkce bez kategorií zadávání dat indexované nebo zakódovaný před použitím. 

V závislosti na modelu budete muset index nebo kódovat různé způsoby. Například Logistic a lineární regrese modely vyžadují za běhu jeden kódování, kde, například funkci se tři kategorie lze rozbalit do tří sloupců funkce se každý obsahující 0 či 1 v závislosti na kategorii hodnotu. MLlib poskytuje funkci [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) dělat za běhu jeden kódování. Tento kódovací mapuje sloupec popisek rejstříky se sloupcem binární vektory obsahujících maximálně jednoho jeden hodnotu. Tato kódování umožňuje algoritmech očekávat číselných hodnotami funkce, jako jsou logistickou regresní mohou být použity pro kategorií funkcí.

Tady je kód index a kódovat kategorií funkcí:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba k provedení nad buňku: 3.14 sekund


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Vytvoření s popisky čárky objektů k zadání vstupních hodnot do ML funkcí

Tato část obsahuje kód, který ukazuje, jak indexovat kategorií textových dat jako datový typ s popisky čárky a jeho kódování tak, aby mohou sloužit k školení a otestujte logistickou regresní MLlib a jiných klasifikace. Objekty s popisky čárky jsou pružné Distributed datových sad (RDD) naformátovaná tak, že není potřeba jako vstupní data tak, že většina ML algoritmy MLlib. [Popisky čárky](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je místní vektorové hustotu nebo řídký, přidružené popisek/odpověď.

Tady je kód pro index a kódování textu funkcí pro binární klasifikace.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Tady je kód kódování a indexování funkcí kategorií textu pro lineární regresní analýzu.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Vytvoření výběrová dílčí dat a ho rozdělit na školení a testování sady

Tento kód vytvoří výběrová dat (25 % slouží sem). I když není potřeba v tomto příkladu kvůli velikost datové, jsme ukazují, jak je můžete přehrajte tady tak víte, jak se používá pro vlastní problém v případě potřeby. Po velkých ukázky to můžete významné při ušetřit čas školení modely. Další jsme vzorku rozdělit na části školení (tady 75 %) a testování část (tady 25 %) pro použití v klasifikace a modelování regresní.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Doba k provedení nad buňku: 0.31 sekund


### <a name="feature-scaling"></a>Funkce měřítka

Funkce měřítko, označovaná jako normalizace dat zajistí, že funkce bez široce Celková uhrazená hodnoty není vzhledem k tomu nadbytečné porovnali ve funkci cíle. Kód pro změnu velikosti funkce používá [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) zobrazit funkcí, které odchylka jednotku. Pochází od MLlib pro použití v lineární regrese s Stochastic přechodu sestup (SGD), Oblíbené algoritmus pro vzdělávání širokou škálu druhý počítač výukové modely Vyřešeno regrese ATP podpory vektorové počítačích (SVM).   

>[AZURE.TIP] Nalezeny algoritmu LinearRegressionWithSGD považovány za citlivé funkci proměnné velikosti.   

Tady je kód rozsahu proměnné pro použití s regularized lineární SGD algoritmus.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Doba k provedení nad buňku: 11.67 sekund


### <a name="cache-objects-in-memory"></a>Mezipaměti objektů v paměti

Doba školení a testování ML algoritmů lze snížit ukládání do mezipaměti rámečku zadávání dat objekty používané pro třídění, regresní a měřítko funkce.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP** 

Doba k provedení nad buňku: 0,13 sekund


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Předpovídání též tip je zaplacené s modely binární klasifikace

Tento oddíl vysvětluje, jak používat tři modely pro daný úkol binární klasifikace předpovědi též tip je zaplacené výletu taxi. Modely prezentovány jsou:

- Logistickou regresní 
- Náhodné struktury
- Zvýšení stromy přechodu

Každý model vytváření části s kódem je rozdělen do kroků: 

1. **Školení modelu** dat pomocí jednu sadu parametrů
2. **Model hodnocení** se sadou dat testu s metriky
3. **Uložení modelu** ve objektů blob pro budoucí spotřebu

Ukážeme, jak provádět křížového ověřování (CV) s parametrem komínů dvěma způsoby:

1. Pomocí **obecných** vlastního kódu, které se dají použít libovolnou algoritmus MLlib a k zadání parametru Nastaví algoritmus. 
1. Použití **pySpark funkce CrossValidator kanálem k odesílání zpráv**. Upozorňujeme, že i když je vhodné zejména podle našeho prostředí CrossValidator má určitá omezení pro Spark 1.5.0: 

    - Modely kanálem k odesílání zpráv nemůžou být uloženy/zachován pro budoucí spotřeba.
    - Není možné použít pro každý parametr v modelu.
    - Nelze použít pro každý MLlib algoritmus.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Pole Obecné křížové ověření a hyperparameter sweeping používá pro algoritmu logistickou regresní binární klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložení logistickou regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a tarif datové sady. Model je školení pomocí křížové ověření (CV) a hyperparameter sweeping implementovaná s vlastní kód, který lze použít k některým výukové algoritmy MLlib.   

>[AZURE.NOTE] Spuštění vlastního kódu CV může trvat několik minut.

**Školení logistickou regresní modelu pomocí CV a hyperparameter sweeping**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

INTERCEPT:-0.0111216486893

Doba k provedení nad buňku: 14.43 sekund


**Vyhodnocení binární klasifikace modelu pomocí standardní metriky**

Kód v této části ukazuje, jak chcete zjistit logistickou regresní model proti test-množině dat, včetně grafu křivky ROC.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Oblasti cena = 0.985336538462

Oblasti ROC = 0.983383274312

Souhrnné stat

Přesnost = 0.984174341679

Odvolání = 0.984174341679

F1 Skóre = 0.984174341679

Doba k provedení nad buňku: 2.67 sekund


**Vykreslete křivky ROC.**

*PredictionAndLabelsDF* je registrovaná jako tabulku, *tmp_results*v předchozí buňky. *tmp_results* lze dotazů a výstup do data snímku sqlResults ke kreslení. Tady je kód.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Tady je kódu pro volání předpovědí a ROC křivka.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()
    

**VÝSTUP**

![Logistickou regresní křivky ROC obecný přístupu](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Zachování modelu v objektů blob pro budoucí spotřebu**

Kód v této části ukazuje, jak uložit logistickou regresní model spotřebu.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**VÝSTUP**

Doba k provedení nad buňku: 34.57 sekund


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Funkce společnosti MLlib CrossValidator kanálem k odesílání zpráv s modelem logistickou regresní (pružná regresní)

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložení logistickou regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a tarif datové sady. Model je školení pomocí křížové ověření (CV) a hyperparameter sweeping implementovaná s funkcí kanálem k odesílání zpráv MLlib CrossValidator CV s parametrem možnosti Uklidit.   


>[AZURE.NOTE] Po provedení této MLlib CV kódu může trvat několik minut.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÝSTUP**

Doba k provedení nad buňku: 107.98 sekund


**Vykreslete křivky ROC.**

*PredictionAndLabelsDF* je registrovaná jako tabulku, *tmp_results*v předchozí buňky. *tmp_results* lze dotazů a výstup do data snímku sqlResults ke kreslení. Tady je kód.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Tady je kód, který chcete ROC křivka.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**VÝSTUP**

![Logistickou regresní křivky ROC pomocí CrossValidator MLlib společnosti](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Náhodné doménové klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložit náhodné doménové regrese předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a tarif datové sady.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Oblasti ROC = 0.985336538462

Doba k provedení nad buňku: 26.72 sekund


### <a name="gradient-boosting-trees-classification"></a>Přechod zvýšení stromy klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložte přechodu zvýšení stromy modelu předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a jízdenky datovou sadu.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Oblasti ROC = 0.985336538462

Doba k provedení nad buňku: 28.13 sekund


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Předpovídání částka tip s regresní modely (nepoužíváte CV)

Tento oddíl vysvětluje, jak pomocí tří modelů pro daný úkol regresní předpovědi množství tip zaplacené taxi výletu založený na dalších funkcí tip. Modely prezentovány jsou:

- Vyřešeno lineární regrese
- Náhodné struktury
- Zvýšení stromy přechodu

Tyto modely jsou popsány v úvodu. Každý model vytváření části s kódem je rozdělen do kroků: 

1. **Školení modelu** dat pomocí jednu sadu parametrů
2. **Model hodnocení** se sadou dat testu s metriky
3. **Uložení modelu** ve objektů blob pro budoucí spotřebu   


>AZURE Poznámka: Křížově ověření se nepoužívá modely tři regresní v této části, protože to bylo vidět podrobně modelům logistickou regresní. Příklad ukazující, jak pomocí CV pružná čisté lineární regrese je k dispozici v dodatku v tomto tématu.


>AZURE Poznámka: V naší prostředí, může být problémy s sladění LinearRegressionWithSGD modely a parametry potřeba změnit/optimalizované pečlivě pro získání platný model. Změna měřítka proměnných výrazně usnadňují konvergence. Pružná čisté regresní zobrazené v dodatku k Toto téma lze také místo LinearRegressionWithSGD.


### <a name="linear-regression-with-sgd"></a>Lineární regrese s SGD

Kód v této části ukazuje, jak můžete pomocí funkcí škálovanou školení lineární regrese používající stochastic přechodu sestup (SGD) pro optimalizaci a jak skóre, jsou vyhodnoceny a uložte model v úložišti objektů Blob Azure (WASB).

>[AZURE.TIP] V naší prostředí může být problémy s sladění LinearRegressionWithSGD modely a parametry potřeba změnit/optimalizované pečlivě pro získání platný model. Změna měřítka proměnných výrazně usnadňují konvergence.


    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Koeficienty: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

INTERCEPT: 0.854507624459

MÍRA RMSE = 1.23485131376

Funkce sqr R = 0.597963951127

Doba k provedení nad buňku: 38.62 sekund


### <a name="random-forest-regression"></a>Analytický nástroj Regrese náhodná struktury

Kód v této části ukazuje, jak školení, jsou vyhodnoceny a uložit náhodné doménové modelu předpovídá tip množství dat NYC taxi ze služební cesty.   

>[AZURE.NOTE] Křížové ověření s parametrem komínů pomocí vlastního kódu je k dispozici v dodatku.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

MÍRA RMSE = 0.931981967875

Funkce sqr R = 0.733445485802

Doba k provedení nad buňku: 25.98 sekund


### <a name="gradient-boosting-trees-regression"></a>Přechod zvýšení regresní stromy

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložení přechodu zvýšení stromy modelu předpovídá tip množství dat ze služební cesty taxi NYC.

**Školení a vyhodnocení**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

MÍRA RMSE = 0.928172197114

Funkce sqr R = 0.732680354389

Doba k provedení nad buňku: 20.9 sekund


**Grafu**
    
*tmp_results* je registrovaná jako tabulku podregistru do předchozí buňky. Výstup do dat – rámečku *sqlResults* pro zakreslení jsou výsledky z tabulky. Tady je kód

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Tady je kód vynesení dat pomocí serveru Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Skutečné časově a předpovídanými tip částky](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Dodatek: Dodatečné regresní úkolů pomocí křížového ověření s parametrem změny

Tento dodatek obsahuje kód zobrazující školá CV pomocí pružná čistého pro lineární regresi a postup CV s možnosti Uklidit parametrů pomocí vlastního kódu pro regresní náhodné doménové.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Křížové ověření pomocí ohebné čisté lineární regrese

Kód v této části ukazuje, jak přes ověření pomocí pružná čistého pro lineární regresi a jak chcete zjistit modelu proti testovací data.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba k provedení nad buňku: 161.21 sekund

**Vyhodnocení s metrikou R SQR**

*tmp_results* je registrovaná jako tabulku podregistru do předchozí buňky. Výstup do dat – rámečku *sqlResults* pro zakreslení jsou výsledky z tabulky. Tady je kód

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Tady je kód pro výpočet R sqr.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**VÝSTUP**

Funkce sqr R = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Křížové ověření se možnosti Uklidit parametrů pomocí vlastního kódu pro regresní náhodné struktury

Kód v této části ukazuje, jak přes ověření se možnosti Uklidit parametrů pomocí vlastního kódu pro regresní náhodné struktury a jak chcete zjistit modelu proti testovací data.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

MÍRA RMSE = 0.906972198262

Funkce sqr R = 0.740751197012

Doba k provedení nad buňku: 69.17 sekund


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Vyčistit objekty z paměti a tisku modelu umístění

Použití `unpersist()` odstranění objektů uložených v mezipaměti v paměti.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**VÝSTUP**

PythonRDD [122] v RDD na PythonRDD.scala: 43


**Výtisku cestu k modelu soubory se nemusí používat v poznámkovém bloku spotřebu.** A používat skóre nezávisle na sadu dat, musíte zkopírovat a vložit tyto názvy souborů v "Poznámkový blok spotřebu".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**VÝSTUP**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016 05 0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016 05 0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016 05 0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016 05 0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016 05 0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016 05 0316_52_18.827237"

## <a name="whats-next"></a>Co je další krok?

Teď, když jste vytvořili regresní a klasifikace modely s Spark MlLib, budete chtít zjistěte, jak skóre a vyhodnotíte tyto modely.

**Model spotřebu:** Informace o skóre a vyhodnotíte klasifikaci a regresní modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnotíte integrované Spark počítače výukové modely](machine-learning-data-science-spark-model-consumption.md).
