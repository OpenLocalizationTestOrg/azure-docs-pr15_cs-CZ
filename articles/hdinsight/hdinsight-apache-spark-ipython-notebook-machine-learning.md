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


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Vytváření aplikací počítače výukové dělat Apache Spark clusterů na HDInsight Linux

Naučte se vytvářet počítače učení pomocí obrázku Apache Spark HDInsight aplikaci. Tento článek ukazuje, jak pomocí služby dostupné Poznámkový blok Jupyter clusteru sestavovat a otestujte naše aplikaci. Ukázková data HVAC.csv, který je k dispozici na všech clusterů ve výchozím nastavení používá aplikace.

**Požadavky:**

Je nutné mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Zobrazit data

Před můžeme začít vytvářet aplikace, dejte nám pochopení struktury dat a druh analýzy děláme budou na kartě data. 

V tomto článku používáme ukázkové **HVAC.csv** datového souboru, který je k dispozici v úložišti Azure účtu, který je přidružený k obrázku HDInsight. V rámci účtu úložiště je soubor na **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Stáhněte a otevřete soubor CSV získat snímek data.  

![Snímek TVK dat] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Snímek TVK dat")

Data zobrazuje teplotu cílové a skutečné teplotu budovy nainstalovány TVK systémy. Předpokládejme sloupci **systému** představuje ID systému a **SystemAge** sloupce Počet roků systému TVK zjištění na místě na budovy.

Tyto údaje použijeme odhadnout, zda budovy bude hotter nebo colder založené na cílové teplotu, ID systému a stáří systému.

##<a name="app"></a>Psát pomocí Spark MLlib výukové aplikace počítače

V této aplikaci používáme Spark ML kanálu provádět klasifikace dokumentu. V kanálu jsme rozdělit slova dokument převést text na vektor číselných funkce a konečně sestavení předpovědí modelu pomocí funkce vektory a popisků. Proveďte následující kroky k vytvoření aplikace.

1. Na [Portálu Azure](https://portal.azure.com/)z startboard klikněte na dlaždici pro svůj cluster Spark (Pokud připnuté k startboard). Můžete taky přejít na svůj cluster v části **Procházet vše** > **Clusterů HDInsight**.   

2. Na zásuvné Spark obrázku klikněte na **Řídicí panel obrázku**a klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Jupyter Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Vytvoření nového poznámkového bloku. Klikněte na **Nový**a potom klikněte na **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Vytvoření nového poznámkového bloku Jupyter")

3. Nový poznámkový blok vytvoří a otevřeli s názvem Untitled.pynb. Klikněte na název poznámkového bloku nahoře a zadejte popisný název.

    ![Poskytovat název poznámkového bloku] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Poskytovat název poznámkového bloku")

3. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark poznámkového bloku, ne potřebujete vytvořit libovolný kontexty explicitně. Kontextů Spark a podregistru se automaticky vytvoří pro vás při spuštění na první buňku kód. Můžete začít importováním typy, které jsou potřeba pro tento scénář. Vložte následující úryvek do prázdné buňky a stiskněte kombinaci kláves **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Musí nyní načítání dat (hvac.csv), analyzovat ho a použijte ji k vlaku modelu. V takovém případě definujete funkci, která zkontroluje, jestli je větší než teploty cílové skutečné odstínem budovy. Skutečné teplotu větší, budovy je aktivní, symbolem hodnotu **1.0**. Skutečné teplotu menší, budovy je chlad, označené hodnotu **0,0**. 

    Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfigurace kanálů výukové Spark počítače, což se skládá ze tří fází: tokenizátoru hashingTF a lr. Další informace o tom, co je kanálů a jak to funguje najdete v tématu <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark počítače výukové kanálem k odesílání zpráv</a>.

    Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Přizpůsobení kanálu školení dokumentu. Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Ověřte školení dokument kontroly průběhu s aplikací. Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.

        training.show()

    To by vám měl dát výstupu podobná této:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Přejděte zpět a ověřte výstupu proti nezpracovanými soubor CSV. Například první řádek souboru .csv obsahuje tyto údaje:

    ![Snímek TVK dat] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Snímek TVK dat")

    Všimněte si, jak skutečné teplotu menší než cílovém teplotu znázorňující, že je studenou budovy. Proto do výstupu školení pro **Popisek** v prvním řádku hodnotu **0,0**, což znamená, že budovy není aktivní.

8.  Příprava k sadám dat ke spuštění školení modelu proti. K tomu, nám předat na ID systému a věk systému (označený jako **SystemInfo** do výstupu školení) a modelu by předpovídání, zda bude stavební pomocí tohoto ID systému a systému stáří hotter (symbolem 1.0) nebo studenější (symbolem 0,0).

    Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Nakonec zvětšit předpovědí na testovací data. Vložte následující úryvek prázdnou buňku a stisknutím **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Měli byste vidět výstup podobná této:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Z prvního řádku při odhadu, zobrazí se, že pro systému TVK s ID 20 a systém věku 25 let budovy bude aktivní (**předpovědí = 1.0**). První hodnotu DenseVector (0.49999) odpovídá předpovědi 0,0 a druhé hodnoty (0.5001) odpovídá předpovědi 1.0. Do výstupu, i když je druhá hodnota pouze nepatrně vyšší modelu se zobrazí **předpovědí = 1.0**.

11. Po spuštění aplikace, měli byste vypnutí Poznámkový blok, který uvolnit prostředky. Postup, v nabídce **soubor** na Poznámkový blok, klikněte na **Zavřít a zastavit**. Tento bude vypnutí a zavřít Poznámkový blok.
           

##<a name="anaconda"></a>Použití Anaconda scikit – další knihovny pro výukové počítače

Apache Spark clusterů na HDInsight obsahovat Anaconda knihovny. Také jedná se o **scikit – další** v knihovně výukové počítače. Knihovnu obsahuje taky různé sady dat, které můžete použít k vytvoření ukázkové aplikace přímo z poznámkového bloku Jupyter. Příklady použití scikit – další knihovny najdete v článku [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Viz taky

* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
