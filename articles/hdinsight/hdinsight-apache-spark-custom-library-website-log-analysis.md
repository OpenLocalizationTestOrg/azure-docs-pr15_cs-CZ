<properties 
    pageTitle="Pomocí vlastní knihovny HDInsight Spark clusteru analyzovat webu protokoly | Microsoft Azure" 
    description="Použití vlastní knihoven s HDInsight Spark obrázku a analyzujte data protokoly webu" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analýza protokoly webu pomocí vlastního knihovny Apache Spark obrázku na HDInsight Linux

Tento poznámkový blok ukazuje, jak k analýze dat protokolu vlastní knihovny pomocí Spark na HDInsight. Vlastní knihovny, které budeme používat je s názvem **iislogparser.py**Python knihovna.

> [AZURE.TIP] Tento kurz je také k dispozici Jupyter Poznámkový blok na Spark (Linux) obrázku, který jste vytvořili v HDInsight. Možnosti poznámkového bloku vám umožní spustit Python výstřižky z poznámkového bloku samotné. Abyste mohli provést kurz z do poznámkového bloku, vytvoření Spark clusteru, spuštění poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a pak spusťte Poznámkový blok **analyzovat protokoly s Spark pomocí vlastní library.ipynb** ve složce **PySpark** .

**Požadavky:**

Je nutné mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Uložit jako RDD neformátovaná data

V této části použijeme k provádění úloh, které zpracuje jako nezpracovaná ukázkových dat a uložit ho jako tabulku podregistru Poznámkový blok [Jupyter](https://jupyter.org) přidružené Apache Spark obrázku v HDInsight. Ukázková data jsou souboru CSV (hvac.csv) k dispozici na všech clusterů ve výchozím nastavení.

Po uložení data jako tabulku podregistru v následující části jsme se připojí k tabulku podregistru pomocí nástroje BI například Power BI a Tableau.

1. Na [Portálu Azure](https://portal.azure.com/)z startboard klikněte na dlaždici pro svůj cluster Spark (Pokud připnuté k startboard). Můžete taky přejít na svůj cluster v části **Procházet vše** > **Clusterů HDInsight**.   

2. Na zásuvné Spark obrázku klikněte na **Řídicí panel obrázku**a klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Jupyter Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Vytvoření nového poznámkového bloku. Klikněte na **Nový**a potom klikněte na **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Vytvoření nového poznámkového bloku Jupyter")

3. Nový poznámkový blok vytvoří a otevřeli s názvem Untitled.pynb. Klikněte na název poznámkového bloku nahoře a zadejte popisný název.

    ![Poskytovat název poznámkového bloku] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Poskytovat název poznámkového bloku")

4. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark poznámkového bloku, ne potřebujete vytvořit libovolný kontexty explicitně. Kontextů Spark a podregistru se automaticky vytvoří pro vás při spuštění na první buňku kód. Můžete začít importováním typy, které jsou potřeba pro tento scénář. Vložte následující úryvek do prázdné buňky a stiskněte kombinaci kláves **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Vytvoření RDD používání ukázkových dat protokolu dostupným na clusteru. Přístupu k datům v okně výchozí účet úložiště přidružený k obrázku na **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Načtení ukázkové protokol nastavena na ověřte, že v předchozím kroku byla úspěšně dokončena.

        logs.take(5)

    Měli byste vidět výstup podobná této:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analýza dat protokolu pomocí vlastní Python knihovny

7. Ve výše uvedené výstupu prvních pár řádků obsahovat informace v záhlaví a čárami zbývající odpovídá schématu popsaná v této záhlaví. Analýza takové protokoly může být složité. Ano používáme vlastní Python knihovně (**iislogparser.py**), díky kterému analýza takové protokoly mnohem snadnější. Ve výchozím nastavení je součástí svůj cluster Spark na HDInsight na **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**tuto knihovnu.

    Ale tato knihovna není v `PYTHONPATH` tak nelze ji používáme pomocí příkazu importovat jako `import iislogparser`. Použít tuto knihovnu, jsme musí rozešlete všechny uzly kolegy. Spusťte následující fragment kódu.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`poskytuje funkci `parse_log_line` vracející `None` Pokud řádek protokolu tvoří jeden řádek záhlaví a vrátí instanci `LogLine` třídy Pokud najde řádek protokolu. Použití `LogLine` kurz extrahování pouze řádky protokolu z RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Načtení několik extrahované protokolu čáry a ověřte, že krok byla úspěšně dokončena.

        logLines.take(2)

    Výstup by měl být stejný následujícím způsobem:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. `LogLine` Třídy, třeba zase obsahuje několik užitečných metod `is_error()`, která vrací, jestli položka protokolu má kód chyby. Slouží k výpočtu počet chyb v řádcích extrahované protokolu a potom zaznamenávat všechny chyby na jiný soubor.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Měli byste vidět výstup takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. Můžete taky **Matplotlib** vytvářet vizualizace dat. Pokud chcete najít příčinu požadavků na kterých běží poměrně dlouho, můžete chtít vyhledat soubory, které chvíli nejvíce průměrně vytisknout.
Fragment dole načte horní 25 zdrojů, provedené většinu času a bude předávat žádost o.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Měli byste vidět výstup takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Můžete zobrazit tyto informace ve formě grafu. Jako první krok k vytvoření grafu dejte nám nejdřív vytvořte tabulku dočasné **AverageTime**. V tabulce seskupí podle času zobrazíte, pokud došlo k jakékoli vrcholy pole neobvyklé latence konkrétní kdykoli protokoly.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Potom můžete použít následující dotaz SQL zobrazíte všechny záznamy v tabulce **AverageTime** .

        %%sql -o averagetime
        SELECT * FROM AverageTime

    `%%sql` Magické následovaný `-o averagetime` zaručuje, že výstup dotazu místně zachovat na serveru Jupyter (obvykle headnode clusteru). Výstup je uložena jako [Pandas](http://pandas.pydata.org/) dataframe se zadaným názvem **averagetime**.

    Měli byste vidět výstup takto:

    ![Výstup dotazu SQL] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Výstup dotazu SQL")

    Další informace o `%%sql` magické, jakož i ostatní magics dostupné jádra PySpark najdete v článku [jádra k dispozici na poznámkové bloky Jupyter s clusterů Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Teď můžete Matplotlib knihovny slouží k vytvoření vizualizace dat, vytvoření grafu. Protože grafu třeba vytvořit z dataframe místně trvalých **averagetime** , vždy začíná fragment kódu `%%local` magické. Zajistíte tak, že je kód místně spouštějí na serveru Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Měli byste vidět výstup takto:

    ![Výstup Matplotlib] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Výstup Matplotlib")

16. Po spuštění aplikace, měli byste vypnutí Poznámkový blok, který uvolnit prostředky. Postup, v nabídce **soubor** na Poznámkový blok, klikněte na **Zavřít a zastavit**. Tento bude vypnutí a zavřít Poznámkový blok.
    

## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teploty pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)

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
