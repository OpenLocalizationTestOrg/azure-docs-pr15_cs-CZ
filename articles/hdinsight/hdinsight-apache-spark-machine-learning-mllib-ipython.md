<properties 
    pageTitle="Pomocí Apache Spark vytvořte počítače výukové aplikace na HDInsight | Microsoft Azure" 
    description="Podrobné pokyny, jak pomocí služby poznámkové bloky Apache Spark k vytváření aplikací výukové počítače" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Počítač výukové: Prediktivní analýza jídla kontroly dat pomocí MLlib s Apache Spark clusteru na HDInsight Linux

> [AZURE.TIP] Tento kurz je také k dispozici Jupyter poznámkového bloku na Spark (Linux) obrázku, který jste vytvořili v HDInsight. Prostředí Poznámkový blok vám umožní spustit Python výstřižky z poznámkového bloku samotné. Abyste mohli provést kurz z do poznámkového bloku, vytvoření Spark clusteru, spuštění poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a pak spusťte **Spark počítače učení - prediktivní analýzy dat kontroly jídla** bloku MLLib.ipynb ve složce **Python** .


Tento článek ukazuje, jak provádět jednoduché prediktivní analýzy otevřít datovou sadu pomocí **MLLib**, Spark na předdefinované počítače výukové knihovny. MLLib je základní Spark knihovny, která přináší celou řadu nástrojů, které jsou vhodné k počítači výukové úkolů, včetně nástroje, které jsou vhodné pro:

* Klasifikace

* Analytický nástroj Regrese

* Clusterů

* Téma modelování

* Rozkladový jednotném hodnotu (SVD) a hlavní komponenty analýzy (kompatibilitou)

* Hypotéz testování a výpočtům statistik ukázka

Tento článek uvádí jednoduchý přístup k *klasifikace* prostřednictvím logistickou regresní.

## <a name="what-are-classification-and-logistic-regression"></a>Co je klasifikace a logistickou regresní?

*Zařazení*, velmi běžné počítače výukové úkolu, je proces třídění zadávání dat do kategorií. Je úlohy algoritmu klasifikace zjistit, jak přiřadit "štítky", aby se při zadávání dat, který zadáte. Například mohl by podle vás algoritmu výukové počítače, který přijme informace akciích předávat na vstupu a rozdělí populace na dvě kategorie: populací, které by měl prodej a populací, které by měly zachovat.

Logistickou regresní je algoritmu, který používáte pro klasifikace. Na Spark logistickou regresní rozhraní API je užitečné pro *binární klasifikace*nebo klasifikaci zadávání dat do jedné ze dvou skupin. Další informace o logistickou regrese najdete v článku [wikipedii](https://en.wikipedia.org/wiki/Logistic_regression).

V souhrnu proces logistickou regresní vytváří s *logistickou funkce* mohou sloužit k předpovídání pravděpodobnost, že vstupní vektorové patří v jedné skupině nebo další.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Co nám chcete provést v tomto článku?

K provedení některých prediktivní analýzy na jídla kontroly data (**Food_Inspections1.csv**), která byla pořízených [portál dat město Chicago](https://data.cityofchicago.org/)použijete Spark. Tento datovou sadu obsahuje informace o jídla kontroly, které byly provedeny Ostrava, včetně informací o každé jídla zařízení, které byly zkontrolovány, porušení, které nebyly nalezeny (pokud existuje) a výsledky kontroly. Datový soubor CSV už účet úložiště přidružený k obrázku na **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**dostupné.

V následujících krocích je vytvořit si model, který najdete v článku co je potřeba k úspěch nebo selhání jídla kontroly. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Začít vytvářet aplikace výukové počítače pomocí Spark MLlib

1. Z [Portálu Azure](https://portal.azure.com/)z startboard klikněte na dlaždici pro svůj cluster Spark (Pokud připnuté na startboard). Můžete taky přejít na svůj cluster v části **Procházet vše** > **Clusterů HDInsight**.   

2. Na zásuvné Spark obrázku klikněte na **Řídicí panel obrázku**a klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Jupyter Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Vytvoření nového poznámkového bloku. Klikněte na **Nový**a potom klikněte na **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Vytvoření nového poznámkového bloku Jupyter")

3. Nový poznámkový blok vytvoří a otevřeli s názvem Untitled.pynb. Klikněte na název poznámkového bloku nahoře a zadejte popisný název.

    ![Poskytovat název poznámkového bloku] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Poskytovat název poznámkového bloku")

3. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark poznámkového bloku, ne potřebujete vytvořit libovolný kontexty explicitně. Kontextů Spark a podregistru se automaticky vytvoří pro vás při spuštění na první buňku kód. Můžete začít vytvářet počítači výukové aplikace importováním typy pro tento scénář. Postup, umístěte kurzor do buňky a stisknutím **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Vytváření vstupních dataframe

Můžete použít `sqlContext` provést transformací na strukturovaná data. Načtení ukázkových dat ((**Food_Inspections1.csv**)) do Spark SQL *dataframe*je první úkol. 

1. Protože nezpracovaná data jsou ve formátu CSV, potřebujeme kontextu Spark pomocí můžete načítat každý řádek souboru do paměti jako nestrukturovaná textu. potom používat knihovnu CSV společnosti Python analyzovat každý řádek jednotlivě. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Nyní je soubor CSV jako RDD. Dejte nám z RDD pochopit schématu data načítejte jeden řádek.


        inspections.take(1)


    Měli byste vidět výstup takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Výše uvedené výstup dostaneme představu o schématu vstupní souboru. soubor obsahuje název každého zařízení, typ zařízení, adresy a data kontroly a umístění, mimo jiné. Vybereme několika sloupců, která má být užitečné pro naše Prediktivní analýza a seskupit výsledky jako dataframe, který používáme pak vytvořit tabulku dočasné.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Teď máme *dataframe* `df` ve kterém můžete provést Naše analýza. Máme také dočasné tabulky volání **CountResults**. Jsme zadali 4 sloupci zájem dataframe: **id**, **název**, **výsledky**a **porušení**. 
    
    Nastavíme malého vzorového data:

        df.show(5)

    Měli byste vidět výstup takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Princip data

1. Začněme abyste získali přehled o obsahuje naše datovou sadu. Co je třeba různých hodnot ve sloupci **výsledky** ?


        df.select('results').distinct().show()

    
    Měli byste vidět výstup takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Rychlé vizualizace může pomoct při důvod, proč o distribuci tyto výsledky. Společnost Microsoft už máte data v dočasná tabulka **CountResults**. Můžete použít následující dotaz SQL v tabulce chcete-li lépe porozumět z rozdělení výsledky.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql` Magické následovaný `-o countResultsdf` zaručuje, že výstup dotazu místně zachovat na serveru Jupyter (obvykle headnode clusteru). Výstup je uložena jako [Pandas](http://pandas.pydata.org/) dataframe se zadaným názvem **countResultsdf**.
    
    Měli byste vidět výstup takto:
    
    ![Výstup dotazu SQL] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "Výstup dotazu SQL")

    Další informace o `%%sql` magické, jakož i ostatní magics dostupné jádra PySpark najdete v článku [jádra k dispozici na poznámkové bloky Jupyter s clusterů Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Matplotlib knihovny slouží k vytvoření vizualizace dat, můžete také vytvořit grafu. Protože grafu třeba vytvořit z dataframe místně trvalých **countResultsdf** , vždy začíná fragment kódu `%%local` magické. Zajistíte tak, že je kód místně spouštějí na serveru Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Měli byste vidět výstup takto:

    ![Výsledek výstup](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Uvidíte, že jsou 5 odlišné výsledky, které můžou mít kontrolu:
    
    * Obchodní není umístěn 
    * Selhalo:
    * Předání
    * Technická podpora s podmínky, a
    * Odhlášení z firmy 

    Dejte nám můžete vyvinout modelu, který můžete uhodnout výsledku kontroly jídla zadaných porušení. Protože logistickou regresní metodu binární klasifikace, dává smysl seskupit data do dvou kategorií: **při chybě** a **předat**. "Předat s podmínek" je stále průchodu, tak když jsme školení modelu, jsme zvážíme dvěma výsledky odpovídající. Data s jinými výsledky ("Firmy není našlo se", "firmy mimo") nejsou užitečné, takže jsme se odebere ze sady naše školicí kurzy. Je vhodné nevadí od těchto dvou kategorií příliš malý procenta výsledků tvoří přesto.

5. Dejte nám pokračovat a převeďte naše existující dataframe (`df`) do nového dataframe kde každé kontrole tvaru pár porušení popisek. V našem případě popisku z `0.0` představuje se nepovede, popisku z `1.0` představuje úspěchu a popisek `-1.0` představuje některé výsledky kromě těchto dvou. Jsme bude odfiltrovat těchto výsledků při výpočtu nový snímek data.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Pojďme načíst jeden řádek z s popisky dat zobrazíte, jak to vypadá.


        labeledData.take(1)


    Měli byste vidět výstup takto:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Vytvoření modelu logistickou regresní ze vstupní dataframe

Náš konečný úkol je označený jako data převedete na formát, který lze analyzovat logistickou regresní. Vstupní logistickou regresní algoritmu by měl být sadu *dvojice vektorové funkce popisků*, kde "funkce vektorové" je vektor čísel, která představuje vstupní bod určitým způsobem. Ano potřebujeme způsob, jak převést sloupce "porušení", která je částečně strukturovaná a obsahuje velké množství komentářů v prostém textu, do pole reálná čísla, které by mohly snadno pochopili do počítače. 

V jednom počítači standardní výukové přístup pro zpracování přirozeného jazyka je přiřaďte každé různých slovo "rejstřík" a vektor předat počítače výukové algoritmus tak, aby každý index hodnotu obsahuje relativní četnost daného slova v textovém řetězci. 

MLLib poskytuje snadný způsob, jak tuto operaci. Nejdřív jsme budete "manipulačním" každý řetězec porušení zobrazíte jednotlivá slova v každý řetězec a pak použijeme `HashingTF` k převodu všech sadách tokenů vektorové funkce, které lze potom předat algoritmu logistickou regresní vytvářet modelu. Některé z těchto kroků budete provádění v posloupnosti pomocí "kanálem k odesílání zpráv".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Vyhodnocení model na datovou sadu samostatné test

Používáme modelu jsme vytvořili *odhadnout* jaké výsledky kontroly nových bude, založené na porušení, které byly pozorovanými. Jsme školení tento model na datovou sadu **Food_Inspections1.csv**. Dejte nám pomocí druhou sadu, **Food_Inspections2.csv** *vyhodnocení* : Zkontrolujte sílu tento model na nová data. Tento druhý sadu dat (**Food_Inspections2.csv**) už měli v kontejneru úložiště výchozí přidružené clusteru.

1. Fragment dole vytvoří nový dataframe **predictionsDf** obsahující předpovědí generované modelem. Fragment také vytvoří tabulku dočasné **předpovědí** , na kterých založený dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Měli byste vidět výstup takto:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Podívejte se na jeden z předpovědí. Spusťte tento fragment kódu:

        predictionsDf.take(1)

    Zobrazí se předpovědí pro první položku v uvedenou množinu dat testu.

3. `model.transform()` Metoda použít stejné transformace do nových dat pomocí na stejné schéma a zahltit předpověď jak klasifikovat data. Můžeme některé jednoduché statistiky získáte představu o jak přesný byly naše předpovědí:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Výstup vypadá takto:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Pomocí logistickou regresní Spark dostaneme přesné modelu vztah mezi porušení popisy v angličtině a jestli chcete daný obchodní úspěch nebo selhání jídla kontroly. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Vytvoření vizuální znázornění odhadu

Jsme můžete nyní vytvářet konečný vizualizaci softwaru a služeb důvod, proč o výsledcích tento test. 

1. Začneme tak, že extrahování různých předpovědí a výsledky z dočasná tabulka **předpovědí** vytvořili. Následující dotazy oddělte výstup jako *true_positive*, *false_positive*, *true_negative*a *false_negative*. V následujícím dotazů jsme vypnout vizualizace pomocí `-q` a také uložit výstup (pomocí `-o`) jako dataframes použitelná pak s `%%local` magické. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Nakonec pomocí následující úryvek generovat grafu pomocí **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Měli byste vidět následující výstup.
    
    ![Výstup předpovědí](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    V tomto grafu "" kladný odkazuje kontroly selhalo jídla přitom záporný výsledek odkazuje na předané kontroly.

## <a name="shut-down-the-notebook"></a>Vypnutí poznámkového bloku

Po spuštění aplikace, měli byste vypnutí Poznámkový blok, který uvolnit prostředky. Postup, v nabídce **soubor** na Poznámkový blok, klikněte na **Zavřít a zastavit**. Tento bude vypnutí a zavřít Poznámkový blok.


## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teplotu pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)

* [Analýza protokolu webu pomocí Spark HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spuštění aplikací

* [Vytvoření samostatného aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Spuštění úlohy vzdáleně Spark clusteru pomocí Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Modul plug-in nástroje HDInsight IntelliJ představu umožňuje vytvořit a odeslat Spark Scala aplikace](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Modul plug-in pro použití HDInsight nástroje pro IntelliJ NÁPAD vzdáleně ladění Spark aplikací](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Pomocí obrázku Spark na HDInsight Zeppelin poznámkových bloků](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Oříšky umožňující Jupyter poznámkového bloku na Spark obrázku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
