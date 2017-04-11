<properties
    pageTitle="Skóre modely výukové integrované Spark počítači | Microsoft Azure"
    description="Jak skóre výukové modelů, které byly uložené v úložišti objektů Blob Azure (WASB)."
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

# <a name="score-spark-built-machine-learning-models"></a>Skóre modely výukové integrované Spark počítače 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Toto téma popisuje, jak načíst počítače výuky (ML) modelů na verzi, které byly vytvořené pomocí Spark MLlib a uložené v úložišti objektů Blob Azure (WASB) a jak skóre s, máte taky uložené v WASB datové sady. Zobrazuje jak předem zpracování zadávání dat, transformace funkce pomocí funkce indexování a kódování v MLlib nástrojů a jak vytvořit objekty bodu s popisky dat, které mohou sloužit jako vstup hodnocení s ML modely. Modely používané hodnocení zahrnutím lineární regrese, logistickou regresní, náhodné modely struktury a přechodu zvýšení stromu modelů na verzi.


## <a name="prerequisites"></a>Zjistit předpoklady pro

1. Potřebujete účet Azure a HDInsight Spark je potřeba HDInsight 3.4 Spark 1,6 clusteru služby k dokončení tohoto postupu. V tématu [Přehled dat vědy pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny o tom, jak tyto požadavky splnit. Toto téma obsahuje také popis NYC 2013 Taxi data použitá v tomto poli a pokyny pro spuštění kódu z poznámkového bloku Jupyter Spark clusteru. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** Poznámkový blok, který obsahuje ukázky v tomto tématu je k dispozici v [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Musíte taky vytvořit počítače výukové modely, které mají být tady dosáhne projdete téma [průzkum a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) .   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Instalační program: úložišť knihoven a přednastavené Spark kontextu

Spark je možné pro čtení a zápis úložišti objektů Blob Azure (WASB). Aby některé z existujících dat uložených tam budou zpracovány pomocí Spark a výsledky znovu uložené v WASB.

Uložit modely nebo souborů v WASB, cestu musí být zadán správně. Výchozí kontejner připojené k obrázku Spark můžete odkázat pomocí cesty začínající: *"wasb / /"*. Následující příklad kódu určuje umístění data, která chcete číst a cestu k adresáři modelu úložiště, ke které se uloží výstupu modelu. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Nastavení adresářové cesty pro úložišť v WASB

Modely se ukládají do: "wasb: / / / / remoteuser/NYCTaxi/modely uživatelů". Pokud tato cesta není správně nastavená, modely nezavádějí hodnocení.

Scored výsledky byly uloženy v: "wasb: / / / uživatel/remoteuser/NYCTaxi/ScoredResults". Pokud cestu ke složce, je nesprávné, výsledky neukládají do této složky.   


>[AZURE.NOTE] Cesta k umístění souboru můžete kopírovat a vkládat do zástupné symboly v tento kód z výstupu na poslední buňku **machine-learning-data-science-spark-data-exploration-modeling.ipynb** poznámkového bloku.   


Tady je kódu pro nastavení adresářům: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**VÝSTUP:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Import knihoven

Nastavit spark kontext a import potřebné knihoven s následující kód

    #IMPORT LIBRARIES
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

Oříšky PySpark, které jsou k dispozici s poznámkovými bloky Jupyter měli přednastavené kontext. Takže není potřeba nastavit Spark nebo kontexty podregistru explicitně začnete práci s aplikací připravují. Toto jsou k dispozici pro ve výchozím nastavení. Kontexty jsou:

- sc - pro Spark 
- sqlContext - pro podregistru

Jádra PySpark obsahuje několik předdefinovaných "magics", které jsou speciální příkazy, které můžete volat s %%. Existují dva tyto příkazy, které se používají v těchto ukázek kódu.

- **%% místní** Zadat místně spuštění kódu v dalších řádcích. Kód musí být platná Python kód.
- **%% sql -o<variable name>** 
- Spustí dotaz podregistru proti sqlContext. Je-li parametr -o, je výsledek dotazu zachován v %% místní kontext Python jako Pandas dataframe.
 

Pro další informace o jádra pro Jupyter poznámkových bloků nebo předdefinované "magics", která poskytují, najdete v článku [jádra umožňující Jupyter poznámkových bloků pomocí HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Jedí dat a vytváření rámeček vyčištěný dat

Tato část obsahuje kód pro řadu muset jedí data, která mají být úkoly. Přečtěte si ve spojených vzorku 0,1 % taxi ze služební cesty a tarif souboru (uložená jako soubor TSV), formát data a potom vytvoří rámeček vyčistit dat.

Soubory ze služební cesty a tarif taxi byly spojují podle postupu uvedeného v: [týmu dat pro výzkum proces v akci: použití HDInsight Hadoop clusterů](machine-learning-data-science-process-hive-walkthrough.md) tématu.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP:**

Doba k provedení nad buňku: 46.37 sekund


## <a name="prepare-data-for-scoring-in-spark"></a>Příprava dat pro bodování ve Spark 

Tato část popisuje index, kódování a měřítko kategorií funkcí, které je připravit pro použití v MLlib kontrolována výukové algoritmů pro klasifikaci a regresní.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Funkce transformaci: index a kódovat kategorií funkcí k zadání vstupních hodnot do modelů hodnocení 

Tato část popisuje indexovat kategorií dat pomocí `StringIndexer` a kódovat funkce bez `OneHotEncoder` zadávání do modelů.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kódování řetězec sloupec s popisky sloupec popisek indexy. Indexy jsou uspořádány podle frekvence popisek. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapy sloupec popisek rejstříky se sloupcem binární vektory s maximálně jedna jeden hodnota. Tato kódování umožňuje algoritmech očekávat nepřetržitý hodnotami funkce, jako jsou logistickou regresní mohou být použity pro kategorií funkcí.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP:**

Doba k provedení nad buňku: 5.37 sekund


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Vytvoření RDD objekty s maticemi funkce o zadání kritérií do modely

Tato část obsahuje kód, který ukazuje, jak indexovat kategorií textových dat jako objekt RDD a za běhu jeden jeho kódování tak mohou sloužit k školení a otestovat logistickou regresní MLlib a na základě stromu modely. Indexovaná data se ukládají v objekty [Pružné Distributed datovou sadu (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Toto jsou základní odběru v Spark. Objekt RDD představuje neměnný, oddíly kolekci prvků, které lze ovládat na souběžně s Spark.

Obsahuje také kód, který ukazuje, jak zobrazit data `StandardScalar` poskytovanou MLlib pro použití v lineární regrese s Stochastic přechodu sestup (SGD), Oblíbené algoritmus pro školení širokou škálu modely výukové počítače. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) slouží ke změně měřítka funkcí, které odchylka jednotku. Funkce měřítko, označovaná jako normalizace dat zajistí, že funkce bez široce Celková uhrazená hodnoty není vzhledem k tomu nadbytečné porovnali ve funkci cíle. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP:**

Doba k provedení nad buňku: 11.72 sekund


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Skóre s logistickou regresní Model a uložit výstupní objektů blob

Kód v této části ukazuje, jak načtení modelu logistickou regresní, uložené v úložišti objektů blob Azure a použijte ji k předpovídání též tip je splacenou výletu taxi, skóre se standardní klasifikace metriky uložit a zobrazit výsledky k úložišti objektů blob. Scored výsledky jsou uloženy v RDD objekty. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÝSTUP:**

Doba k provedení nad buňku: 19.22 sekund


## <a name="score-a-linear-regression-model"></a>Skóre modelu lineární regrese

Použijeme [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ke školení lineární regrese modelu pomocí Stochastic přechodu sestup (SGD) pro optimalizaci odhadnout částka zaplacená tip. 

Kód v této části ukazuje, jak načtení modelu lineární regresní z úložiště objektů blob Azure, skóre pomocí škálovanou proměnné a pak uložení výsledků zpět objektů blob.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP:**

Doba k provedení nad buňku: 16.63 sekund


## <a name="score-classification-and-regression-random-forest-models"></a>Skóre klasifikace regresní náhodné doménové modely a

Kód v této části ukazuje, jak načíst uložené klasifikace a regresní náhodné doménové modely uložené v úložišti objektů blob Azure skóre jejich výkonu s standardní třídění a regresní míry a uložte výsledků zpět k úložišti objektů blob.

[Náhodné strukturami](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety stromové struktury rozhodování.  Že kombinují mnoho stromové struktury rozhodování snížení rizik overfitting. Náhodné strukturami zpracovat kategorií funkcí rozšířit nastavení multiclass klasifikace, není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Náhodné strukturami jsou jednou z nejčastěji úspěšné počítače výukové modelů na verzi pro klasifikaci a regresní.

[Spark.mllib](http://spark.apache.org/mllib/) podporuje náhodné strukturami, binární a multiclass klasifikace a regresní, pomocí funkcí nepřetržitý a kategorií. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÝSTUP:**

Doba k provedení nad buňku: 31.07 sekund


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Skóre klasifikace a regresní přechodu zvýšení stromu modely

Kód v této části ukazuje, jak načíst klasifikace a regresní přechodu zvýšení stromu modely z úložiště objektů blob Azure, skóre jejich výkonu s standardní třídění a regresní míry a uložte výsledků zpět k úložišti objektů blob. 

**Spark.mllib** podporuje GBTs binární klasifikace a regresní, pomocí funkcí nepřetržitý a kategorií. 

[Zvýšení stromy přechodu](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety stromové struktury rozhodování. GBTs školení stromové struktury rozhodování opakované, aby byl minimalizován funkci ztráty. GBTs můžete zpracovávají kategorií funkcí, není nutné zadávat funkce měřítko a budou moct zachytit nelineárností a funkcí interakce. Můžete taky používají v nastavení multiclass klasifikace.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**VÝSTUP:**

Doba k provedení nad buňku: 14.6 sekund


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Vyčistit objekty z paměti a tisk dosáhne umístění souboru

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**VÝSTUP:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016 05 0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016 05 0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016 05 0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016 05 0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Používání Spark modely prostřednictvím webového rozhraní

Spark poskytuje mechanismus předkládat vzdáleně dávkové úlohy nebo interaktivní dotazů pomocí rozhraní REST s složkou s názvem Livius. Livius je standardně zapnutá u svůj cluster HDInsight Spark. Další informace o Livius najdete v tématu: [odeslání Spark úlohy vzdáleně pomocí Livius](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Můžete Livius vzdáleně odešlete úlohy, která dávkové skóre soubor, který je uložený v objektů blob Azure a výsledky zapíše do jiného objektů blob. K tomuto účelu nahrajete skript Python z  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) k objektů blob Spark obrázku. Pomocí nástroje, jako je **Microsoft Azure úložiště Explorer** nebo **AzCopy** skript zkopírovat objektů blob obrázku. V našem případě jsme Nahrát skript ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Kombinace kláves, které budete potřebovat se nachází na portálu pro úložiště účet spojený s Spark obrázku. 


Po nahrání do tohoto umístění tento skript běží v rámci obrázku Spark v rámci distribuované. Načtení modelu a spustit předpovědí vstupních souborů založené na modelu.  

Vyvoláte tento skript vzdáleně pomocí jednoduchého HTTPS/ZBÝVAJÍCÍ požadavku na Livius.  Tady je příkaz otočení vytvářet žádost HTTP skript Python vzdáleně spustit. Nahrazení CLUSTERLOGIN, CLUSTERPASSWORD NÁZEV_CLUSTERU s příslušnými hodnotami pro svůj cluster Spark.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Můžete použít libovolný jazyk ve vzdáleném systému vyvolat úlohu Spark prostřednictvím Livius voláním jednoduché HTTPS s základní ověřování.   


>[AZURE.NOTE] Je vhodné použít knihovnu Python požadavky při volání HTTP, ale není aktuálně nainstalovaný ve výchozím nastavení ve funkcích Azure. Aby starší knihovny HTTP se místo toho použít.   


Tady je kód Python HTTP volání:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Můžete také přidat tento kód Python [Azure funkce](https://azure.microsoft.com/documentation/services/functions/) aktivovat odeslání úlohy Spark, že skóre objektů blob podle různých události, jako třeba časovače, vytvoření nebo aktualizace objektů blob. 

Podle potřeby bezplatné klientského rozhraní kód vyvolat dávky Spark bodování definující akce HTTP na **Použití logických operátorů aplikace Návrhář** a nastavením parametrů pomocí [Aplikace logiky Azure](https://azure.microsoft.com/documentation/services/app-service/logic/) . 

- Z Azure portálu vytvoříte novou aplikaci logiky klepnutím na **+ Nový** -> **Web + Mobile** -> **Logiky aplikace**. 
- Vyvoláte **Logiky aplikace Návrhář**zadejte název logiky aplikace a aplikace služeb plán.
- Vyberte akci, která HTTP a zadejte parametry vidíte na následujícím obrázku:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Co je další krok? 

**Křížového ověřování a hyperparameter komínů**: najdete v článku [Pokročilé průzkum a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak můžete být modely školení pomocí křížového ověřování a hyper parametr sweeping.
