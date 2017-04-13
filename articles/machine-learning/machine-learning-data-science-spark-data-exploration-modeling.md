<properties
    pageTitle="Průzkum dat a modelování pomocí Spark | Microsoft Azure"
    description="Hodnotí možnosti průzkum a modelování dat ze sady nástrojů Spark MLlib."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="data-exploration-and-modeling-with-spark"></a>Průzkum dat a modelování pomocí Spark

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tento návod používá HDInsight Spark dělat průzkum a binární klasifikace regresní modelování úkoly na základě vzorku NYC taxi ze služební cesty a jízdenky datovou sadu 2013.  Vás provede jednotlivými kroky [Procesu vědy dat](http://aka.ms/datascienceprocess)začátku do konce, pomocí HDInsight Spark clusteru pro zpracování a objektů BLOB Azure pro ukládání data a modely. Proces prozkoumá vizualizují data přenášet z úložiště objektů Blob Azure a připraví dat za účelem vytvoření prediktivní modely. Sestavení provádět binární klasifikaci a regresní modelování pomocí nástrojů Spark MLlib jsou tyto modely.

- **Binární klasifikace** úkolu je předpovídání též tip platí pro cestu. 
- **Regresní** úkolu je předpovídání množství tip založený na dalších funkcí tip. 

Modely, které budeme používat zahrnutím a spoje lineární regresní, náhodné struktury a přechodu zesílen stromy:

- [Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je lineární regrese modelu, který používá metodu Stochastic přechodu sestup (SGD) a měřítka odhadnout částky tip zaplacený pro optimalizaci a funkce. 
- Regresní [logistickou regresní s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" je regresní model, který lze použít po kategorií dělat dat klasifikace závislé proměnné. LBFGS je algoritmus optimalizace kvazi-Petr, blíží algoritmu Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a, které se běžně používají v počítači výukové.
- [Náhodné strukturami](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety stromové struktury rozhodování.  Že kombinují mnoho stromové struktury rozhodování snížení rizik overfitting. Náhodné strukturami se používají pro regresní a klasifikace zpracovat kategorií funkcí a může se prodloužit nastavení multiclass klasifikace. Není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Náhodné strukturami jsou jednou z nejčastěji úspěšné počítače výukové modelů na verzi pro klasifikaci a regresní.
- [Přechod zesílen stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety stromové struktury rozhodování. GBTs školení stromové struktury rozhodování opakované, aby byl minimalizován funkci ztráty. GBTs se používají pro regresní a klasifikace zpracovat kategorií funkcí, není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Můžete taky používají v nastavení multiclass klasifikace.

Kroky modelování také obsahovat kód znázorňující, jak proškolit, jsou vyhodnoceny a uložení jednotlivých typů modelu. Python použil kódu řešení a zobrazit relevantní pozemků.   


>[AZURE.NOTE] Ačkoli sada nástrojů Spark MLlib je navržený pro práci v případě velkých sad dat, relativně malého vzorového (pomocí řádky 170K, o 0,1 % původní datovou sadu NYC ~ 30 Mb) je zde použita pro usnadnění. Cvičení uvedeny běží efektivní (asi 10 minut) HDInsight clusteru s 2 uzly kolegy. Téhož kódu s menší změny mohou sloužit k zpracovat větší-sady dat, se příslušné změny pro ukládání do mezipaměti dat v paměti a změna velikosti obrázku.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Potřebujete účet Azure a HDInsight Spark je potřeba HDInsight 3.4 Spark 1,6 clusteru služby k dokončení tohoto postupu. V tématu [Přehled dat vědy pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny o tom, jak tyto požadavky splnit. Toto téma obsahuje také popis NYC 2013 Taxi data použitá v tomto poli a pokyny pro spuštění kódu z poznámkového bloku Jupyter Spark clusteru. **PySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** Poznámkový blok, který obsahuje ukázky v tomto tématu je k dispozici v [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Instalační program: úložišť knihoven a přednastavené Spark kontextu

Spark je možné pro čtení a zápis Azure úložiště objektů Blob (označovaná taky jako WASB). Aby některé z existujících dat uložených tam budou zpracovány pomocí Spark a výsledky znovu uložené v WASB.

Uložit modely nebo souborů v WASB, cestu musí být zadán správně. Výchozí kontejner připojené k obrázku Spark můžete odkázat pomocí cesty začínající: "wasb: / / /". Je odkazováno jiných umístění "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Nastavení adresářové cesty pro úložišť v WASB

Následující příklad kódu určuje umístění data, která chcete číst a cestu k adresáři modelu úložiště, ke které se uloží výstupu modelu:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Import knihoven

Nastavení je třeba import nezbytné knihovny. Nastavte spark kontext a import potřebné knihoven s Tento kód:


    # IMPORT LIBRARIES
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
 

## <a name="data-ingestion-from-public-blob"></a>Požití data z veřejného objektů blob

Cílem prvního kroku v procesu vědy dat je jedí data, která chcete analyzovat z zdrojů kde se nachází v prostředí průzkum a modelování dat. Prostředí je Spark v tomto návodu. Tato část obsahuje kód dokončete řadu úkolů:

- jedí ukázkových dat modelovat
- Přečtěte si v zadávání datovou sadu (uložená jako soubor TSV)
- formátování a vymazání data
- vytvářet a ukládat do mezipaměti objektů (RDDs nebo dat rámce) v paměti
- Zaregistrujte ho jako tabulku temp v kontextu SQL.

Tady je kód letiště požití data.

    # INGEST DATA

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

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÝSTUP:**

Doba k provedení nad buňku: 51.72 sekund


## <a name="data-exploration--visualization"></a>Průzkum dat a vizualizaci

Jakmile se data má přenesený do Spark, dalším krokem v procesu vědy dat je získat hlubší porozumění dat prostřednictvím průzkum a vizualizace. V této části prozkoumat taxi dat pomocí dotazů SQL jsme vykreslete cílových proměnných a potenciální funkce vizuální prohlídka. Konkrétně jsme vykreslete četnost osobní počtem v taxi cest četnost tip částky a jak se liší podle částka platby a typ tipy.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Vykreslete histogram osobní počet frekvencí ve výběru taxi cest

Tento kód a následné fragmentů použít k vytvoření dotazu vzorku a místní magické vynesení dat magické SQL.

- **Magické SQL (`%%sql`)** HDInsight PySpark jádra podporuje snadno vložené HiveQL dotazů sqlContext. (-O VARIABLE_NAME) argument ukládá výstup dotazu SQL jako Pandas DataFrame na serveru Jupyter. To znamená, že je k dispozici v místním režimu.
- ** `%%local` Magické** slouží ke spuštění kódu místně na server Jupyter, což je headnode clusteru HDInsight. Obvykle se používá `%%local` magic ve spojení s `%%sql` magic s parametrem – o. Parametr -o by uchovávat výstup dotazu SQL místně a potom %% místní magické by aktivace následujícího fragment kódu místně kontrolovat výstup dotazy SQL, který je zachován místně

Výstup je znázorněn automaticky po spuštění kód.

Tento dotaz načte cest podle počtu osobní. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Tento kód vytvoří místní data rámce z výstupu dotazu a vykreslí data. `%%local` Magické vytvoří místní snímek dat, `sqlResults`, který se dá použít pro zakreslení s matplotlib. 

>[AZURE.NOTE] Tento PySpark magické slouží tisknutím v tomto návodu. Při velké množství dat, měli vzorek vytvoření dat rámeček, který můžete přizpůsobit v místní paměti.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Tady je kód, který chcete vykreslit cest tak, že osobní počty

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**VÝSTUP:**

![Četnost ze služební cesty podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Pomocí tlačítek nabídka **Typ** v poznámkovém bloku, můžete vybrat mezi několika různých typů vizualizací (tabulky, výsečový, spojnicový, oblast nebo panelu). Pruhový graf je zobrazena zde.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Vykreslete histogram tip částek a jak množství tip se liší podle osobní počet a tarif částky.

Pomocí příkazu jazyka SQL pro ukázková data.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**VÝSTUP:** 

![Tip: rozdělení částky](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tip: částka podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip: Částka tarif částku](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funkce inženýrské, transformace a dat Příprava modelování
Tato část popisuje a poskytuje kód pro postupy Příprava dat pro použití v modelu ML. Ukazuje, jak provádět následující úkoly:

- Vytvořit novou funkci binning hodin do provozu čas bloky
- Index a kódovat kategorií funkcí
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

**VÝSTUP:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Index a kódovat kategorií funkcí k zadání vstupních hodnot do modelování funkcí

Tato část popisuje index nebo kódování kategorií funkcí k zadání vstupních hodnot do funkcí modelování. Modelování a předpovídání funkce MLlib vyžadují funkce bez kategorií zadávání dat indexované nebo zakódovaný před použitím. V závislosti na modelu je potřeba vytvořit index ani kódovat různé způsoby:  

- **Stromové modelování** vyžaduje kategorií kódovaný jako číselné hodnoty (například funkci se tři kategorie může být zakódovaný s 0 a 1, 2). To je poskytovanou na MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funkce. Tato funkce kódování řetězec sloupec s popisky sloupec indexů popisek, které jsou uspořádány podle frekvence popisek. Přestože indexované s číselné hodnoty pro zadávání a zpracování dat, může být zadán algoritmů stromové zacházet s nimi řádně podporovat jako kategorie. 

- **Modely Logistic a lineární regrese** vyžadují za běhu jeden kódování, kde, například funkci se tři kategorie lze rozbalit do tří sloupců funkce se každý obsahující 0 či 1 v závislosti na kategorii hodnotu. MLlib poskytuje funkci [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) dělat za běhu jeden kódování. Tento kódovací mapuje sloupec popisek rejstříky se sloupcem binární vektory obsahujících maximálně jednoho jeden hodnotu. Tato kódování umožňuje algoritmech očekávat číselných hodnotami funkce, jako jsou logistickou regresní mohou být použity pro kategorií funkcí.

Tady je kód index a kódovat kategorií funkcí:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
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

**VÝSTUP:**

Doba k provedení nad buňku: 1,28 sekund

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Vytvoření s popisky čárky objektů k zadání vstupních hodnot do ML funkcí

Tato část obsahuje kód, který ukazuje, jak indexovat kategorií textových dat jako datový typ s popisky čárky a jeho kódování tak, aby mohou sloužit k školení a otestujte logistickou regresní MLlib a jiných klasifikace. Objekty s popisky čárky jsou pružné Distributed datových sad (RDD) naformátovaná tak, že není potřeba jako vstupní data tak, že většina ML algoritmy MLlib. [Popisky čárky](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je místní vektorové hustotu nebo řídký, přidružené popisek/odpověď.  

Tato část obsahuje kód, který ukazuje, jak indexovat kategorií textových dat jako datový typ [označeného čárky](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) a jeho kódování tak, aby mohou sloužit k školení a otestujte logistickou regresní MLlib a jiných klasifikace. Objekty s popisky čárky jsou pružné Distributed datových sad (RDD) obsahující popisek (cílový/odpověď proměnná) a funkce vektorové. Tento formát je nutný jako vstup pro mnoho ML algoritmy MLlib.

Tady je kód pro index a kódování textu funkcí pro binární klasifikace.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
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
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
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

**VÝSTUP:**

Doba k provedení nad buňku: 0,24 sekund


### <a name="feature-scaling"></a>Funkce měřítka

Funkce měřítko, označovaná jako normalizace dat zajistí, že funkce bez široce Celková uhrazená hodnoty není vzhledem k tomu nadbytečné porovnali ve funkci cíle. Kód pro změnu velikosti funkce používá [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) zobrazit funkcí, které odchylka jednotku. Pochází od MLlib pro použití v lineární regrese s Stochastic přechodu sestup (SGD), Oblíbené algoritmus pro vzdělávání širokou škálu druhý počítač výukové modely Vyřešeno regrese ATP podpory vektorové počítačích (SVM).

>[AZURE.NOTE] Nalezeny algoritmu LinearRegressionWithSGD považovány za citlivé funkci proměnné velikosti.

Tady je kód rozsahu proměnné pro použití s regularized lineární SGD algoritmus.

    # FEATURE SCALING

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

**VÝSTUP:**

Doba k provedení nad buňku: 13.17 sekund


### <a name="cache-objects-in-memory"></a>Mezipaměti objektů v paměti

Doba školení a testování ML algoritmů lze snížit ukládání do mezipaměti rámečku zadávání dat používané pro třídění, regresní, objekty a měřítko funkce.

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

**VÝSTUP:** 

Doba k provedení nad buňku: 0,15 sekund


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Předpovídání též tip je zaplacené s modely binární klasifikace

Tento oddíl vysvětluje, jak používat tři modely pro daný úkol binární klasifikace předpovědi též tip je zaplacené výletu taxi. Modely prezentovány jsou:

- Vyřešeno logistickou regresní 
- Náhodné doménové modelu
- Zvýšení stromy přechodu

Každý model vytváření části s kódem je rozdělen do kroků: 

1. **Školení modelu** dat pomocí jednu sadu parametrů
2. **Model hodnocení** se sadou dat testu s metriky
3. **Uložení modelu** ve objektů blob pro budoucí spotřebu

### <a name="classification-using-logistic-regression"></a>Použití logistickou regresní klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložení logistickou regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a tarif datové sady.

**Školení logistickou regresní modelu pomocí CV a hyperparameter sweeping**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP:** 

Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

INTERCEPT:-0.0111216486893

Doba k provedení nad buňku: 14.43 sekund

**Vyhodnocení binární klasifikace modelu pomocí standardní metriky**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
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


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÝSTUP:** 

Oblasti cena = 0.985297691373

Oblasti ROC = 0.983714670256

Souhrnné stat

Přesnost = 0.984304060189

Odvolání = 0.984304060189

F1 Skóre = 0.984304060189

Doba k provedení nad buňku: 57.61 sekund

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

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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
    

**VÝSTUP:**

![Logistickou regresní ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Náhodné doménové klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložit náhodné doménové modelu předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a tarif datové sady.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

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
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
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

**VÝSTUP:**

Oblasti ROC = 0.985297691373

Doba k provedení nad buňku: 31.09 sekund


### <a name="gradient-boosting-trees-classification"></a>Přechod zvýšení stromy klasifikace

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložte přechodu zvýšení stromy modelu předpovídá též tip je zaplacené cesty v NYC taxi ze služební cesty a jízdenky datovou sadu.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
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


**VÝSTUP:**

Oblasti ROC = 0.985297691373

Doba k provedení nad buňku: 19.76 sekund


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Předpovídání tip částky taxi cest s regresní modely

Tento oddíl vysvětluje, jak pomocí tří modelů pro daný úkol regresní předpovědi množství tip zaplacené taxi výletu založený na dalších funkcí tip. Modely prezentovány jsou:

- Vyřešeno lineární regrese
- Náhodné struktury
- Zvýšení stromy přechodu

Tyto modely jsou popsány v úvodu. Každý model vytváření části s kódem je rozdělen do kroků: 

1. **Školení modelu** dat pomocí jednu sadu parametrů
2. **Model hodnocení** se sadou dat testu s metriky
3. **Uložení modelu** ve objektů blob pro budoucí spotřebu

### <a name="linear-regression-with-sgd"></a>Lineární regrese s SGD 

Kód v této části ukazuje, jak můžete pomocí funkcí škálovanou školení lineární regrese používající stochastic přechodu sestup (SGD) pro optimalizaci a jak skóre, jsou vyhodnoceny a uložte model v úložišti objektů Blob Azure (WASB).

>[AZURE.TIP] V naší prostředí může být problémy s sladění LinearRegressionWithSGD modely a parametry potřeba změnit/optimalizované pečlivě pro získání platný model. Změna měřítka proměnných výrazně usnadňují konvergence. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

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
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP:**

Koeficienty: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

INTERCEPT: 0.853872718283

MÍRA RMSE = 1.24190115863

Funkce sqr R = 0.608017146081

Doba k provedení nad buňku: 58.42 sekund


### <a name="random-forest-regression"></a>Analytický nástroj Regrese náhodná struktury

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložit náhodné doménové regrese předpovídá tip množství dat NYC taxi ze služební cesty.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
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

**VÝSTUP:**

MÍRA RMSE = 0.891209218139

Funkce sqr R = 0.759661334921

Doba k provedení nad buňku: 49.21 sekund


### <a name="gradient-boosting-trees-regression"></a>Přechod zvýšení regresní stromy

Kód v této části ukazuje, jak proškolit, jsou vyhodnoceny a uložení přechodu zvýšení stromy modelu předpovídá tip množství dat ze služební cesty taxi NYC.

**Školení a vyhodnocení**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP:**

MÍRA RMSE = 0.908473148639

Funkce sqr R = 0.753835096681

Doba k provedení nad buňku: 34.52 sekund

**Grafu**

*tmp_results* je registrovaná jako tabulku podregistru do předchozí buňky. Výstup do dat – rámečku *sqlResults* pro zakreslení jsou výsledky z tabulky. Tady je kód

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Tady je kód vynesení dat pomocí serveru Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**VÝSTUP:**

![Skutečné časově a předpovídanými tip částky](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Vyčistit objekty z paměti

Použití `unpersist()` odstranění objektů uložených v mezipaměti v paměti.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
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


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Záznam úložišť modely spotřeby a bodování

Používání a skóre nezávisle na datovou sadu podle [skóre a vyhodnotíte integrované Spark počítače výukové modely](machine-learning-data-science-spark-model-consumption.md) téma, musíte zkopírovat a vložit tyto názvy souborů obsahující uložené modelů vytvořených tady do poznámkového bloku spotřebu Jupyter. Tady je kód, který chcete vytisknout cest k souborům modelu, které bude nutné tam.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**VÝSTUP**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016 05 0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016 05 0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016 05 0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016 05 0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016 05 0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016 05 0317_06_51.737282"


## <a name="whats-next"></a>Co je další krok?

Teď, když jste vytvořili regresní a klasifikace modely s Spark MlLib, budete chtít zjistěte, jak skóre a vyhodnotíte tyto modely. Průzkum pokročilé modelování Poznámkový blok dives hlouběji do včetně křížového ověřování, hyper parametr komínů a model hodnocení. 

**Model spotřebu:** Informace o skóre a vyhodnotíte klasifikaci a regresní modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnotíte integrované Spark počítače výukové modely](machine-learning-data-science-spark-model-consumption.md).

**Křížového ověřování a hyperparameter komínů**: najdete v článku [Pokročilé průzkum a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak můžete být modely školení pomocí křížového ověřování a hyper parametr sweeping



