<properties
    pageTitle="Výzkum dat pomocí Scala a Spark na Azure | Microsoft Azure"
    description="Jak používat Scala sledovaných počítače výukové úkolů s Spark scalable MLlib a Spark ML balíčky Azure HDInsight Spark clusteru."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Výzkum dat pomocí Scala a Spark na Azure

V tomto článku se dozvíte, jak používat Scala sledovaných počítače výukové úkolů s Spark scalable MLlib a Spark ML balíčky Azure HDInsight Spark clusteru. Ho vás provede úkoly, které představují [dat pro výzkum proces](http://aka.ms/datascienceprocess): požití dat a průzkum vizualizace, inženýrské funkce, modelování a spotřeby modelu. Modely v článku zahrnutím a spoje lineární regresní, náhodné strukturami a přechod zesílen stromy (GBTs), kromě dva běžné úkoly výukové sledovaných počítače:

- Regresní problém: předpověď zda částce tip ($) za výletu taxi
- Binární klasifikace: předpověď tip nebo tip (1/0) taxi cest

Proces modelování vyžaduje školení a hodnocení test uvedenou množinu dat a příslušné přesnost metriky. V tomto článku se dozvíte, jak ukládat tyto modely v úložišti objektů Blob Azure a jak skóre a vyhodnotíte výkon prognostické. Tento článek obsahuje taky pokročilejší témata optimalizace modelů pomocí křížového ověřování a hyper parametr sweeping. Data použitá je ukázka 2013 NYC taxi ze služební cesty a tarif uvedenou množinu dat v GitHub k dispozici.

[Scala](http://www.scala-lang.org/), jazykem podle virtuálního počítače Java integruje objektově orientovaného a funkční jazykové koncepty. Je scalable jazyk, který je vhodné pro distribuované zpracování v cloudu a běží na Azure Spark clusterů.

[Spark](http://spark.apache.org/) je framework paralelní zpracování otevřít zdroj, který podporuje zpracování v paměti zvýšit výkon aplikací technologie pro analýzu dat velký. Modul zpracování Spark vytvořené pro rychlé, snadné použití a pokročilých analýz. Možnosti distribuované výpočtu v paměti společnosti Spark usnadnit Dobrá volba, pokud iterativních algoritmů ve počítače učení s pomocí grafu výpočty. Balíček [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) k dispozici jednotné sadu uceleném rozhraní API této technologii postavená data snímky, které vám pomohou vytvořit a ladění praktické počítače výukové kanály. [MLlib](http://spark.apache.org/mllib/) je Spark na výukové scalable počítače knihovny, která přiblíží možnosti modelování k této distribuované prostředí.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je hostovaný Azure nabízení Spark otevřít zdroj. Také podporuje poznámkové bloky Jupyter Scala Spark clusteru a interaktivní dotazy Spark SQL transformace, filtrování a vizualizaci dat uložených v úložišti objektů Blob Azure by umožnit spuštění. Fragmenty kódu Scala v tomto článku poskytující řešení a zobrazit relevantní pozemků vizualizovat data spustit v poznámkových blocích Jupyter nainstalovaným clusterů Spark. Postup modelování v těchto tématech mít kód, který ukazuje, jak proškolit, jsou vyhodnoceny, uložení a používání každého typu model.

Kroky a doručení s kódem v tomto článku jsou určeny pro Azure HDInsight 3.4 Spark 1,6. Kód v tomto článku a v [Poznámkovém bloku Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) se však obecné a měli spolupracovat na libovolnou Spark obrázku. Může být mírně liší od obsah zobrazený v tomto článku, pokud nepoužíváte HDInsight Spark postup nastavení a Správa obrázku.

> [AZURE.NOTE] Téma, které vám ukáže, jak používat Python spíše než Scala k dokončení úkolů v procesu dat pro výzkum začátku do konce najdete v článku [Vědy dat pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro

-   Pokud nemáte předplatné Azure. Pokud už nemáte jeden [získat Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Potřebujete clusteru služby Azure HDInsight 3.4 Spark 1,6 dokončete následující postupy. Vytvoření clusteru, tady naleznete pokyny v [Začínáme: vytvoření Apache Spark na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Nastavte typ obrázku a verze v nabídce **Vybrat typ obrázku** .

![Konfigurace typu HDInsight obrázku](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Popis NYC taxi ze služební cesty dat a pokyny pro spuštění kódu z poznámkového bloku Jupyter clusteru Spark najdete v tématu příslušných částí [Přehled dat vědy pomocí Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spuštění kódu Scala z poznámkového bloku Jupyter clusteru Spark

Samostatné Jupyter poznámkového bloku z portálu Microsoft Azure. Najděte Spark obrázku na řídicím panelu a potom klikněte na ni přejděte na stránku správy pro svůj cluster. Potom klikněte na **Řídicí panely obrázku**a klikněte na **Poznámkový blok Jupyter** otevřete Poznámkový blok přidružené Spark obrázku.

![Řídicí panel obrázku a Jupyter poznámkových bloků](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Můžete taky můžete přístup k Jupyter poznámkovým blokům na https://&lt;název_clusteru&gt;.azurehdinsight.net/jupyter. *Název_clusteru* nahraďte názvem vašeho obrázku. Potřebujete heslo k vašemu účtu správce pro přístup k poznámkovým blokům Jupyter.

![Přejděte k poznámkovým blokům Jupyter tak, že nazvanou podle obrázku](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Vyberte **Scala** zobrazíte adresář, který obsahuje několik příkladů hotových poznámkových bloků, které používají rozhraní API PySpark. Průzkum modelování a bodování Scala.ipynb poznámkového bloku, který obsahuje ukázky tento sadu témat Spark je dostupná na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Na svůj cluster Spark můžete nahrajte Poznámkový blok přímo z GitHub server Jupyter Poznámkový blok. Na domovské stránce Jupyter klikněte na tlačítko **Odeslat** . V Průzkumníku souborů vložte adresu URL (jako nezpracovaná obsah) GitHub Scala poznámkového bloku a pak klikněte na **Otevřít**. Poznámkový blok Scala je k dispozici následující adresu URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Instalační program: Kontextů Spark předvolba a podregistru Spark magics a Spark knihoven

### <a name="preset-spark-and-hive-contexts"></a>Přednastavené Spark a podregistru kontexty

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Oříšky Spark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontexty. Nemusíte výslovně nastavené Spark nebo kontexty podregistru před zahájením práce s aplikací připravují. Přednastavené kontexty jsou:

- `sc`pro SparkContext
- `sqlContext`pro HiveContext


### <a name="spark-magics"></a>Spark magics

Jádra Spark obsahuje některé předdefinované "magics", které jsou speciální příkazy, které můžete volat s `%%`. Dvě tyto příkazy se používají v následující příklady kódu.

- `%%local`Určuje, že kód v dalších řádcích bude spouštět místně. Kód musí být platná Scala kód.
- `%%sql -o <variable name>`Spustí dotaz podregistru proti `sqlContext`. Pokud `-o` předán parametr, je výsledek dotazu zachován v `%%local` Scala kontextu jako snímek Spark data.

Pro další informace o jádra Jupyter poznámkových bloků a jejich předdefinované "magics", zavoláte s `%%` (například `%%local`), najdete v článku [jádra umožňující Jupyter poznámkových bloků pomocí HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Import knihoven

Importujte Spark MLlib a jiných knihoven, které budete potřebovat pomocí následující kód.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Požití dat

Cílem prvního kroku v procesu vědy dat je jedí data, která chcete analyzovat. Přenesení dat z externích zdrojů nebo systémy, kde se nachází prostředí průzkum a modelování dat. V tomto článku jsou data, která jste jedí spojených výběrem 0,1 % taxi ze služební cesty a tarif souboru (uložená jako soubor TSV). Prostředí průzkum a modelování dat je Spark. Tato část obsahuje kód k provedení následujících řadu úkolů:

1. Nastavte adresářové cesty pro ukládání dat a modelu.
2. Přečtěte si v zadávání uvedenou množinu dat (uložená jako soubor TSV).
3. Definice schématu pro data a vymazání data.
4. Vytvořte rámeček vyčištěný dat a mezipaměti v paměti.
5. Jako dočasné tabulku SQLContext zaregistrujte data.
6. Dotaz v tabulce a do rámečku data importovat výsledky.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Nastavení adresářové cesty pro úložišť v úložišti objektů Blob Azure

Spark můžete číst a psát k úložišti objektů Blob Azure. Můžete použít Spark zpracuje některé z existujících dat a uložení výsledků znovu v úložišti objektů Blob.

Pokud chcete uložit modely nebo souborů v úložišti objektů Blob, budete muset správně zadejte cestu. Odkaz kontejneru výchozí připojené k obrázku Spark pomocí cesty, který začíná `wasb:///`. Odkaz jiných umístění pomocí `wasb://`.

Následující příklad kódu určuje umístění zadávání dat ke čtení a cestu k úložišti objektů Blob, který je připojen k obrázku Spark pro uložení modelu.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Import dat, vytvoření RDD a definovat rámeček data podle schématu

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 8 sekund.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Dotaz v tabulce a importovat výsledky v rámci dat

Pak dotazu tabulce tarif, osobní a tip data. odfiltrovat poškozený a Vzdálená data; a tisk několik řádků.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Výstup:**

fare_amount|passenger_count|tip_amount|šikmý
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16.0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Průzkum dat a vizualizaci

Přenést data do Spark dalšímu kroku v procesu vědy Data po získat hlubší porozumění dat prostřednictvím průzkum a vizualizace. V této části zkoumat taxi data pomocí dotazy SQL. Importujte výsledků do rámečku dat vykreslování hodnot v průběhu cílových proměnných a potenciálního funkce prohlídky pomocí funkce automatického vizualizace Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Vykreslovat data pomocí místní a magické SQL

Ve výchozím nastavení je k dispozici v rámci relace, která je zachován uzlech pracovníka výstup všech fragmentů kódu, kdy spustíte z poznámkového bloku Jupyter. Pokud chcete uložit výletu uzly pracovníka pro každý výpočtu a pokud všechna data, je třeba pro vaše výpočtu je k dispozici místně na uzel Jupyter serveru (což je hlavního uzlu), můžete `%%local` magické na serveru Jupyter spuštění fragment kódu.

- **Magické SQL** (`%%sql`). HDInsight Spark jádra podporuje snadno vložené HiveQL dotazů SQLContext. (`-o VARIABLE_NAME`) Argument ukládá výstup dotazu SQL jako snímek Pandas dat na serveru Jupyter. To znamená, že bude dostupný v místním režimu.
- `%%local`**magické**. `%%local` Magické spustí kód místně na server Jupyter, což je hlavního uzlu clusteru HDInsight. Obvykle se používá `%%local` magic ve spojení s `%%sql` magic s `-o` parametr. `-o` Parametr by zachovány výstup dotazu SQL místně a potom `%%local` magické by aktivace následujícího místně kontrolovat výstup dotazy SQL, který je zachován místně fragment kódu.

### <a name="query-the-data-by-using-sql"></a>Dotaz na data pomocí jazyka SQL
Tento dotaz načte cest taxi tarif částka, počet osobní a částka tip.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Následující kód `%%local` magické vytvoří rámeček místních dat sqlResults. SqlResults Umožňuje vykreslit pomocí matplotlib.

> [AZURE.TIP] Místní magické slouží tisknutím v tomto článku. U velkých množiny dat se Přehrajte si prosím vytvořit snímek dat, který můžete přizpůsobit v místní paměti.

### <a name="plot-the-data"></a>Vykreslovat data

Můžete zobrazit pomocí kódu Python po rámečku data v místním kontextu jako snímek Pandas data.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Po spuštění kód, jádra Spark automaticky vizualizují výstup dotazy SQL (HiveQL). Můžete si vybrat mezi několik typů vizualizací:
 
- Tabulky
- Výsečový
- Řádek
- Oblast
- Panel

Tady je kód vynesení dat:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Výstup:**

![Tip: histogram množství](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tip: částka podle počtu osobní](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip: Částka tarif částku](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Vytvoření funkcí a transformace funkcí a příprava dat o zadání kritérií do modelování funkce

U funkcí modelování stromové z Spark ML a MLlib budete muset Příprava cílové a funkce pomocí různých metod, například binning indexování, za běhu jeden kódování a vectorization. Tady je postup pro v této části:

1. Vytvořte novou funkci **binning** hodin do provozu čas bloky.
2. Použití **klávesových jeden kódování a indexování** kategorií funkcí.
3. **Výběr a rozdělení uvedenou množinu dat** do školení a otestujte zlomky.
4. **Zadat školení proměnné a funkce**a pak vytvořte indexovaných nebo za běhu jeden zakódovaný školení a testování vstupní bod s popisky pružné distributed datových sad (RDDs) nebo snímky dat.
5. Automaticky **Zařadit do kategorií a vectorize funkcí a cílů** chcete použít jako vstupů pro počítač výukové modely.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Vytvořit novou funkci binning hodin do provozu čas bloky

Tento kód se dozvíte, jak vytvořit novou funkci binning hodin do bloků čas přenosy a jak do mezipaměti rámečku Výsledná data v paměti. Snímky RDDs a data použití opakovaně ukládání do mezipaměti vede k vylepšení čas spuštění. Podle toho budete mezipaměti RDDs nebo jako rámečky dat v několika fázích v následující postupy.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Za běhu jeden kódování kategorií funkcí a indexování

Modelování a předpovídání funkce MLlib vyžadují funkce bez kategorií zadávání dat indexované nebo zakódovaný před použitím. V této části se dozvíte, jak index nebo kódovat kategorií funkcí k zadání vstupních hodnot do funkcí modelování.

Potřebujete index nebo kódovat vaše modely různými způsoby podle toho, model. Například modely a spoje lineární regresní je nutné za běhu jeden kódování. Například funkci se tři kategorie lze rozbalit do tří sloupců funkce. Každý sloupec obsahuje hodnot 0 a 1 v závislosti na kategorii hodnotu. MLlib poskytuje funkci [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) za běhu jeden kódování. Tento kódovací mapuje sloupec popisek rejstříky se sloupcem binární vektory obsahujících maximálně jednoho jeden hodnotu. Tato kódování algoritmech očekávat číselných hodnotami funkce, jako jsou logistickou regresní lze kategorií funkcí.

Tady transformace pouze čtyři proměnné zobrazíte příklady, které jsou řetězce znaků. Je taky můžete vytvořit index jiné proměnné, například den v týdnu, představované číselné hodnoty, jako kategorií proměnné.

Indexování, když použijete `StringIndexer()`a za běhu jeden kódování, když použijete `OneHotEncoder()` funkcí z MLlib. Tady je kód index a kódovat kategorií funkcí:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 4 sekundy.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Výběr a rozdělené do školení a otestujte zlomky uvedenou množinu dat

Tento kód vytvoří výběrová dat (v tomto příkladu 25 %). Analytický nástroj vzorkování je sice není potřeba v tomto příkladu kvůli velikost uvedenou množinu dat, v článku se dozvíte, jak přehrajte, abyste věděli, jak se používá pro vlastní potíží v případě potřeby. Po velkých ukázky to můžete ušetřit čas významné během školení modely. Pak můžete rozdělte vzorku část školení (v tomto příkladu 75 %) a testování část (v tomto příkladu 25 %) pro použití v klasifikace a modelování regresní.

Náhodné číslo (mezi 0 a 1) přidejte do každého řádku (ve sloupci "NÁHČÍSLO"), něhož chcete-li vybrat více ověření přeložení během školení.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 2 sekund.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Určení proměnné školení a funkce a pak vytvořte indexovaných nebo za běhu jeden zakódovaný školení a testování vstupní označeného RDDs čárky nebo dat snímků

Tato část obsahuje kód, který ukazuje, jak index kategorií textových dat jako datový typ s popisky čárky a kódovat, abyste ho mohli použít na školení a otestujte logistickou regresní MLlib a jiných klasifikace. Objekty s popisky čárky jsou RDDs formátovaná tak, že není potřeba jako vstupní data většinou počítače výukové algoritmy MLlib. [Popisky čárky](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je místní vektorové hustotu nebo řídký, přidružené popisek/odpověď.

V tomto kódu zadáte (závislými) proměnnou cílové a funkcí, které umožňuje školení modely. Pak vytvoříte indexovaných nebo za běhu jeden zakódovaný školení a testování vstupní označeného RDDs čárky nebo dat snímků.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 4 sekundy.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automaticky zařadit do kategorií a vectorize funkcí a cílů, které chcete použít jako vstupů pro počítač výukové modely

Pomocí Spark ML cílové a funkce používat ve stromové modelování funkcích zařadit do kategorií. Kód dokončí dva úkoly:

-   Vytvoří binární cílové hodnotě klasifikace přiřazením hodnoty z hodnot 0 a 1 pro všechny datové body mezi 0 a 1 pomocí mezní hodnota čísla 0,5.
- Automaticky rozdělíte do kategorií funkcí. Pokud počet jedinečných číselných hodnot pro všechny funkce je menší než 32, je tato funkce zařazené do kategorií.

Tady je kód letiště těchto dvou úkolů.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binární klasifikace model: předpověď, zda tip věnovat

V této části vytvoříte tři typy modelů binární klasifikace odhadnout též věnovat tip:

- **Logistickou regresní model** pomocí Spark ML `LogisticRegression()` (funkce)
- **Náhodné doménové klasifikace modelu** pomocí Spark ML `RandomForestClassifier()` (funkce)
- **Přechod zvýšení modelu klasifikace stromu** pomocí MLlib `GradientBoostedTrees()` (funkce)

### <a name="create-a-logistic-regression-model"></a>Vytvoření modelu logistickou regresní

Dále vytvořte logistickou regresní model pomocí Spark ML `LogisticRegression()` (funkce). Vytvoření modelu vytváření doručení s kódem v řadu kroků:

1. Sada **vlaku do modelu** dat s jedním parametrem.
2. **Vyhodnotit modelu** se sadou dat testu s metriky.
3. Úložiště objektů Blob pro budoucí spotřebu **model uložte** .
4. **Skóre modelu** proti testovací data.
5. **Zobrazit výsledky** s sluchátko provozní křivky vlastnostmi (ROC).

Tady je kód letiště těchto postupů:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Načtení, skóre a uložení výsledků.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Výstup:**

ROC na testovací data = 0.9827381497557599


Umožňuje Python na místní snímky dat Pandas křivka ROC.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


**Výstup:**

![Tip nebo bez tip ROC křivky](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Vytvoření modelu náhodné doménové klasifikace

Dále vytvořte modelu klasifikace náhodné doménové pomocí Spark ML `RandomForestClassifier()` fungovat a potom vyhodnotit model na testovací data.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Výstup:**

ROC na testovací data = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Vytvoření modelu GBT klasifikace

Dále vytvořte modelu klasifikace GBT pomocí společnosti MLlib `GradientBoostedTrees()` fungovat a potom vyhodnotit model na testovací data.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Výstup:**

Oblasti ROC křivky: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regresní model: předpověď tip množství

V této části vytvoříte dva typy modelů regresní odhadnout množství tip:

- **Vyřešeno lineární regrese modelu** pomocí Spark ML `LinearRegression()` (funkce). Ušetříte modelu a vyhodnotit model na testovací data.
- **Zvýšení přechodu stromu regresní model** pomocí Spark ML `GBTRegressor()` (funkce).


### <a name="create-a-regularized-linear-regression-model"></a>Vytvoření modelu Vyřešeno lineární regrese

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 13 sekund.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Výstup:**

R-sqr na testovací data = 0.5960320470835743


Pak dotazu výsledky testů jako snímek dat, můžete ho vizualizace AutoVizWidget a matplotlib.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Kód vytvoří rámeček místních dat z výstupu dotazu a vykreslí data. `%%local` Magické vytvoří rámeček místních dat `sqlResults`, které můžete použít k vykreslete s matplotlib.

>[AZURE.NOTE] Tento magické Spark slouží tisknutím v tomto článku. Při velké množství dat, měli vzorek vytvořit snímek dat, který můžete přizpůsobit v místní paměti.

Vytvoření pozemků pomocí Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Výstup:**

![Tip: částka: skutečnost a předpovídané](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Vytvoření modelu regresní GBT

Vytvoření modelu regresní GBT pomocí Spark ML `GBTRegressor()` fungovat a potom vyhodnotit model na testovací data.

[Přechod zesílen stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety stromové struktury rozhodování. GBTs školení stromové struktury rozhodování opakované, aby byl minimalizován funkci ztráty. Můžete provádět GBTs regresní a klasifikace. Budou můžete zpracovávají kategorií funkcí, není nutné zadávat funkce měřítko a můžete uvést nonlinearities a interakcí funkce. Je taky můžete používat v nastavení multiclass klasifikace.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Výstup:**

Je testovací R-sqr: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Rozšířené možnosti modelování nástroje pro optimalizaci

V této části používáte počítač výukové nástroje, které často vývojáři pro optimalizaci modelu. Konkrétně můžete optimalizovat počítače výukové modely třemi různými způsoby pomocí parametrů sweeping a křížově ověření:

-   Rozdělení data do sady vlaku a ověření optimalizovat modelu pomocí hyper parametr sweeping se sadou školení a vyhodnocení se sadou ověření (lineární regrese)
-   Optimalizace modelu pomocí křížového ověřování a hyper parametr komínů funkcí Spark ML CrossValidator (binární klasifikace)
-   Optimalizace modelu pomocí vlastního kódu křížového ověřování a parametr sweeping používat jakéhokoli stroje výuky (funkce) a parametr sad (lineární regrese)


**Křížové ověření** je postup, který vyhodnocuje, jak bude modelu školení na známé množiny dat generalize odhadnout funkce sady dat, na které nebylo byla školení. Základní představu za tento postup je, že modelu školení se sadou dat známé dat, a pak přesnost jejich předpovědí testováno vůči nezávisle na sadě dat. Běžná implementace má rozdělit *k*sadám dat – přeložení a potom školení modelu způsobem kruhového ve všech pouze jednu přeložení.

**Optimalizace Hyper parametr** potížím volba sady hyper parametry algoritmu výukové obvykle s cílem optimalizace měření výkonu na algoritmus na nezávislou sadu dat. Hyper parametr je hodnota, která je nutné zadat mimo postup školení modelu. Předpoklady o hodnoty hyper parametrů může ovlivnit flexibilitu a přesnost modelu. Stromové struktury rozhodování máte hyper parametrů, například třeba požadovaný název hloubkové nebo počtu ponechá ve stromové struktuře. Je třeba nastavit termínu snížení chybnou pro počítač vektorové podpory (SVM).

Běžným způsobem provádět hyper parametr optimalizace je pomocí mřížky hledání taky se mu říká **parametr možnosti Uklidit**. V mřížce hledání vyčerpávající hledání prováděna hodnoty zadané podmnožinu hyper parametr prostoru pro výukové algoritmus. Křížové ověření můžete zadat metriky výkonu seřadíte optimální výsledky vytvořené pomocí algoritmu hledání mřížky. Pokud používáte více ověření hyper parametr sweeping, můžete lépe limit problémy, například overfitting modelu dat školení. Tímto způsobem modelu zachová kapacita vyrovnat obecné množiny dat, ze kterého byla školení data extrahovaných.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optimalizace lineární regrese modelu s hyper parametr sweeping

Pak můžete rozdělit datové sady vlaku a ověření, použití hyper parametr komínů se sadou školení optimalizovat modelu a vyhodnocení se sadou ověření (lineární regrese).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Výstup:**

Je testovací R-sqr: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimalizace binární klasifikace modelu pomocí křížového ověřování a hyper parametr sweeping

V této části se dozvíte, jak optimalizovat model binární klasifikace pomocí křížového ověřování a hyper parametr sweeping. Pomocí této Spark ML `CrossValidator` (funkce).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas spuštění buňku: 33 sekund.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimalizace lineární regrese modelu pomocí vlastního kódu křížového ověřování a parametr sweeping

Potom optimalizace modelu pomocí vlastního kódu a určit nejlepší parametry modelu pomocí kritéria nejvyšší přesnosti. Vytvořte konečný modelu, vyhodnotit model na testovací data a model uložte v úložišti objektů Blob. Nakonec načtení modelu skóre testovací data a vyhodnocení přesnosti.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Výstup:**

Čas spuštění buňku: 61 sekund.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Používání modely výukové integrované Spark počítači automaticky s Scala

Základní informace o tématech, která vás provede jednotlivými úkoly, které zahrnují procesu dat pro výzkum v Azure najdete v článku [Týmu dat pro výzkum obrázku](http://aka.ms/datascienceprocess).

[Návody týmu dat pro výzkum proces](data-science-process-walkthroughs.md) popisuje jiných začátku do konce návody, které ukazují postup v procesu týmu dat pro výzkum pro konkrétní scénáře. Návody také ukazují, jak do cloudu a místní nástroje a služby zkombinovat pracovní postup nebo vytvořit aplikaci inteligentní kanálem k odesílání zpráv.

[Skóre integrované Spark počítače výukové modely](machine-learning-data-science-spark-model-consumption.md) ukazuje, jak použít kód Scala automaticky zavádění a skóre nové datové sady s počítače výukové modely integrované Spark a uložené v úložišti objektů Blob Azure. Postupujte podle pokynů tam uvedených a jednoduše nahradit kód Python Scala kód v tomto článku najdete automatické spotřeba.
