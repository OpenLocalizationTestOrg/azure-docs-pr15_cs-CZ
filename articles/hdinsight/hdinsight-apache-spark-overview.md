<properties 
    pageTitle="Základní informace o Apache Spark v HDInsight | Microsoft Azure" 
    description="Úvod do Apache Spark v HDInsight a scénáře, ve kterých můžete Spark na HDInsight v aplikacích." 
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
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Přehled: Apache Spark na HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> je otevřít zdroj paralelní zpracování systém, který podporuje zpracování v paměti zvýšit výkon analytických aplikací velký data. Modulu zpracování Spark vytvořené pro rychlé, snadné použití a pokročilých analýz. Možnosti výpočtu v paměti společnosti Spark usnadnit Dobrá volba, pokud iterativních algoritmů ve počítače učení s pomocí grafu výpočty. Spark je kompatibilní se službou úložiště objektů Blob Azure (WASB) taky tak, aby existující data uložená v Azure lze snadno zpracovat prostřednictvím Spark.

Při vytváření Spark obrázku v HDInsight vytvoříte Azure výpočetním zdroje s Spark nainstalovali a nakonfigurovali. Pouze trvá asi deset minut vytvořit Spark cluster v HDInsight. Data, která chcete zpracovat uložené v úložišti objektů Blob Azure. V tématu [použití úložišti objektů Blob Azure s HDInsight][hdinsight-storage].

![Apache Spark na Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark na Azure HDInsight")


**Chcete začít pracovat s Apache Spark na Azure HDInsight?** V tématu [Rychlý úvod: Vytvoření obrázku Spark na HDInsight Linux a spuštění ukázkové aplikace Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Seznam známých problémů a omezení s ální najdete v tématu [Známé problémy Apache Spark v Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Proč používat Spark na Azure HDInsight? 

Azure HDInsight nabízí plně spravované Spark služby. Výhody používání Spark na HDInsight patří následující:

| Funkce                             | Popis       |
|-------------------------------------|-------------------|
| Snadnější vytvoření clusterů            | Můžete vytvořit nový cluster Spark na HDInsight v minutách pomocí portálu pro správu Azure, Azure PowerShell nebo HDInsight .NET SDK. V tématu [Začínáme s Spark obrázku v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Snadnější použití                     | Spark v HDInsight clusterů obsahuje poznámkové bloky Jupyter přednastavených. Můžete použít pro interaktivní zpracování dat a vizualizaci. Adresy URL pro je https://CLUSTERNAME.azurehdinsight.net/jupyter. __NÁZEV_CLUSTERU__ nahraďte názvem svůj cluster Spark HDInsight.|
| Rozhraní REST API                       | Spark v HDInsight obsahuje [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), REST API na základě server úloh Spark vzdáleně odešlete a sledovat pracovního úlohy. |
| Podpora pro úložiště jezera dat Azure | Použití úložiště jezera dat Azure jako dalšího úložiště je možné konfigurovat Spark na HDInsight. Další informace o úložišti jezera najdete v článku [Základní informace o Azure jezera úložiště](../data-lake-store/data-lake-store-overview.md).
| Integrace se službou Azure services | Spark na HDInsight získáváte spojnice rozbočovače Azure události. Zákazníci můžete vytvářet streamování aplikací pomocí rozbočovače události kromě [Kafka](http://kafka.apache.org/), který je dostupný jako součást Spark. |
| Podpora pro R Server  | R serverem HDInsight Spark obrázku můžete nastavit pro práci s rychlostí přislíbené s clusteru Spark distribuované R výpočty. Další informace najdete v tématu [Začínáme s používáním R Server místní HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integrace se službou IntelliJ věci  | Modul plug-in HDInsight pro IntelliJ slouží k vytvoření a odeslání aplikací clusteru HDInsight Spark. Další informace najdete v článku [Použití HDInsight nástroje modul plug-in pro IntelliJ vhodné vytvářet Spark žádosti o clusteru HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Souběžné dotazů              | Spark v HDInsight podporuje souběžné dotazy. Díky víc dotazů z jednoho uživatele nebo víc dotazů z různých uživatelů a aplikací pro sdílení stejné zdroje obrázku. |
| Ukládání do mezipaměti na disky SSD                 | Je možné data v mezipaměti v paměti nebo v disky SSD připojené k uzlů. Ukládání do mezipaměti v paměti poskytuje optimálního výkonu dotazu, ale může být drahé; ukládání do mezipaměti v disky SSD poskytuje Skvělá volba pro zvýšení výkonu dotazu bez nutnosti k vytvoření obrázku o velikosti, která je potřebná k tvar celou sadu paměti.|
| Integrace se službou nástrojích BI       | Spark HDInsight poskytuje spojnic nástrojích BI jako je [Power BI](http://www.powerbi.com/) a [Tableau](http://www.tableau.com/products/desktop) pro analýzu dat.|
| Předdefinovaný Anaconda knihovny        | Spark clusterů na HDInsight jsou součástí jsou předinstalované Anaconda knihoven. [Anaconda](http://docs.continuum.io/anaconda/) poskytuje možnost Zavřít 200 knihoven výukové počítače a analýza dat, vizualizace, atd.|
| Škálovatelnost:                     | I když můžete zadat počtu uzlů v svůj cluster při vytváření, můžete zvětšit nebo zmenšit clusteru podle zátěží na projektu. Všechny HDInsight clusterů umožňují Změna počtu uzlů v clusteru. Navíc Spark clusterů možné přetáhnout beze ztráty dat od všech dat uložených v úložišti objektů Blob Azure. |
| Podpora 24/7                    | Spark na HDInsight získáváte podpora 24/7 úrovni organizace a SLA 99,9 % včasných nahoru.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Jaké jsou případy použití Spark na HDInsight?

Apache Spark v HDInsight umožňuje následující uvedené hlavní příklady situací.

### <a name="interactive-data-analysis-and-bi"></a>Interaktivní analýza dat a BI

[Podívejte se na kurz](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark v HDInsight ukládá dat objektů BLOB Azure. Obchodními experty a klíčových rozhodování analyzovat a vytvářet sestavy průběhu tato data a pomocí Microsoft Power BI a interaktivní sestavy z analyzovat data. Analytici lze spustit z nestrukturovaná skupinou/strukturovaný dat z Azure úložiště definice schématu pro data pomocí poznámkových bloků a pak vytvoříte datové modely pomocí Microsoft Power BI. Spark v HDInsight taky podporuje řadu nástrojů třetí strany BI například Tableau, Qlikview a SAP Lumira a platformu ideální pro analytici dat, obchodními experty a klíče vedoucím pracovníkům.

### <a name="iterative-machine-learning"></a>Výukové iterativních počítače

[Podívejte se na kurz: předpověď vytváření teploty uisng TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Podívejte se na kurz: předpověď výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark získáváte [MLlib](http://spark.apache.org/mllib/), počítače výukové knihovny této technologii postavená Spark. Kromě toho také Spark na HDInsight Anaconda, Python rozdělení s různými balíčky výukové počítače. To spojit s integrovanou podporu pro poznámkové bloky Jupyter a máte horní řádek prostředí pro vytváření aplikací výukové počítače.  

### <a name="streaming-and-real-time-data-analysis"></a>Analýza dat streamování a v reálném čase

[Podívejte se na kurz](hdinsight-apache-spark-eventhub-streaming.md)

Data v reálném čase analýzy se používá pro scénáře od zkrácení doby přehled dat zpracováním dat jako výkopu vytváření PRAVDA streamování řešení. Spark v HDInsight nabízí bohatou podporu pro vytváření řešení v reálném čase analýzy. Během Spark už spojnic jedí dat z více zdrojů jako Kafka Flume, Twitter, ZeroMQ nebo TCP sockets, přidává Spark v HDInsight prvotřídní podporu pro ingesting dat z Azure události rozbočovače. Událost rozbočovače jsou nejčastěji používanými fronty služby na Azure. S mimo pole podpory pro událost rozbočovače něco v ní podnítit v HDInsight platformu ideální pro vytváření kanálem k odesílání zpráv analýzy reálném čase.

##<a name="next-steps"></a>K čemu součástí jsou součástí clusteru Spark?

Spark v HDInsight obsahuje následující součásti, které jsou dostupné ve ve výchozím nastavení.

- [Základní Spark](https://spark.apache.org/docs/1.5.1/). Obsahuje Spark základní, Spark SQL, Spark datových proudů rozhraní API, GraphX a MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter poznámkového bloku](https://jupyter.org)

Spark v HDInsight umožňuje také [ovladač ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) pro připojení k Spark clusterů HDInsight nástrojích BI jako je Microsoft Power BI a Tableau.

## <a name="where-do-i-start"></a>Kde se spouští?

Začněte s vytvářením Spark obrázku na HDInsight Linux. V tématu [Rychlý úvod: Vytvoření obrázku Spark na HDInsight Linux a spuštění ukázkové aplikace Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Další kroky

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teploty pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

* [Použití externích balíčků pomocí Jupyter poznámkových bloků](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
