<properties 
    pageTitle="Použití externích balíčků s poznámkovými bloky Jupyter v Apache Spark clusterů na HDInsight | Azure"
    description="Podrobný postup pro nastavení Jupyter poznámkových bloků dostupné s HDInsight Spark clusterů používat externí Spark balíčků." 
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
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Použití externích balíčků s poznámkovými bloky Jupyter v Apache Spark clusterů na HDInsight Linux

>[AZURE.NOTE] Tento článek je k dispozici Spark 1.5.2 na HDInsight 3.3 a Spark 1.6.2 na HDInsight 3.4. 

Naučíte se konfigurovat Jupyter poznámkového bloku v Apache Spark clusteru na HDInsight (Linux) používat externí, balíčků přispěla komunity, které nejsou součástí clusteru mimo pole. 

Můžete vyhledávat [Maven úložiště](http://search.maven.org/) pro celý seznam balíčků, které jsou k dispozici. Otevřete seznam dostupných balíčků taky z jiných zdrojů. Úplný seznam přispěla komunity balíčků je například jsou dostupné na webu [Spark balíčků](http://spark-packages.org/).

V tomto článku se dozvíte, jak použil funkci balíček [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) s poznámkovým blokem Jupyter.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Je nutné mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Použití externích balíčků pomocí Jupyter poznámkových bloků 

1. Na [Portálu Azure](https://portal.azure.com/)z startboard klikněte na dlaždici pro svůj cluster Spark (Pokud připnuté k startboard). Můžete taky přejít na svůj cluster v části **Procházet vše** > **Clusterů HDInsight**.   

2. Z zásuvné Spark obrázku klikněte na **Odkazy**a od zásuvné **Clusteru řídicího panelu** klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Jupyter Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Vytvoření nového poznámkového bloku. Klikněte na **Nový**a potom klikněte na **Spark**.

    ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Vytvoření nového poznámkového bloku Jupyter")

3. Nový poznámkový blok vytvoří a otevřeli s názvem Untitled.pynb. Klikněte na název poznámkového bloku nahoře a zadejte popisný název.

    ![Poskytovat název poznámkového bloku] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Poskytovat název poznámkového bloku")

4. Použijete `%%configure` magické pro nastavení poznámkového bloku použít externí balíček. Ujistěte se, zavoláte v poznámkové bloky, které používají externí balíčků `%%configure` magic do první buňky kód. Zajistíte tak, že jádra nakonfigurovaný tak, aby použil funkci balíček před spuštěním relace.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Pokud je zapomenete konfigurace jádra do první buňky, můžete `%%configure` s `-f` parametr, ale bude restartujte relaci a všechny průběh budou ztraceny.

5. Ve výše uvedená fragment kódu `packages` očekává seznam maven souřadnice v centrálním úložišti Maven. V této fragment `com.databricks:spark-csv_2.10:1.4.0` je souřadnice maven balíčku **spark csv** . Tady je, jak vytvářet souřadnice balíčku.

    na. Vyhledejte v úložišti Maven balíček. Pro účely tohoto návodu používáme [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Z úložiště shromážděte hodnoty **ID skupiny**, **ArtifactId**a **verze**.

    ![Použití externích balíčků s Jupyter poznámkového bloku] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Použití externích balíčků s Jupyter poznámkového bloku")

    c. Zřetězí tři hodnot oddělených středníky (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Spustit kód buňky s `%%configure` magické. To nastaví její konfiguraci a základní relace Livius použil funkci balíček, který jste zadali. Do dalších buněk v poznámkovém bloku teď můžete balíček, jak je ukázáno v následujícím příkladu.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Můžete spustit fragmenty kódu, třeba následujícího obrázku, pokud chcete zobrazit data z dataframe jste vytvořili v předchozím kroku.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teplotu pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
