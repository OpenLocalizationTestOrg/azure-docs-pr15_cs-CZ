<properties 
    pageTitle="Známé problémy Apache Spark v HDInsight | Microsoft Azure" 
    description="Známé problémy Apache Spark v HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Známé problémy s Apache Spark obrázku na HDInsight Linux

Tento dokument uchovává informace o všech známých problémech HDInsight Spark veřejné (verze Preview).  

##<a name="livy-leaks-interactive-session"></a>Livius nevrací interaktivní relace
 
Po restartování Livius s interaktivní relaci (z Ambari nebo vzhledem k restartování počítače virtuální headnode 0) stále aktivní relaci interaktivní úlohy prozrazena. Z toho důvodu nové úlohy můžete zasekly ve stavu přijato a nelze ji spustit.

**Řešení:**

Použijte následující postup řešení problému:

1. SSH do headnode. 
2. Spusťte tento příkaz najít ID aplikace interaktivní úloh spustit prostřednictvím Livius. 

        yarn application –list

    Názvy budou zadané Livius by úlohy spustil s Livius interaktivní relace bez explicitní názvů pro Livius relace zahájené Poznámkový blok Jupyter úlohy výchozí název úlohy bude začínat remotesparkmagics_ *. 

3. Spusťte tento příkaz ukončit tyto úlohy. 

        yarn application –kill <Application ID>

Nové úlohy se spustí spuštěný. 

##<a name="spark-history-server-not-started"></a>Server historie Spark Nezahájeno 

Spark historie serveru není spuštěná automaticky po vytvoření clusteru.  

**Řešení:** 

Ruční spuštění serveru historie Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Problém s oprávněním v adresáři Spark protokolu 

Když hdiuser odešle úlohu s spark-submit, je chyba java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (oprávnění odepřen) a protokol ovladač není napsané. 

**Řešení:**
 
1. Přidání hdiuser Hadoop skupiny. 
2. Zadejte 777 oprávnění na /var/log/spark po vytvoření obrázku. 
3. Aktualizujte umístění protokolu spark pomocí Ambari být v adresáři, 777 oprávnění.  
4. Spustit spark – odeslat jako sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Otázky týkající se Jupyter poznámkových bloků

Tady jsou některé známé problémy související s Jupyter poznámkových bloků.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Poznámkové bloky se jiné ASCII znaky v názvech souborů

Jupyter poznámkové bloky, které můžete použít v Spark HDInsight clusterů neměli mít jiné ASCII znaky v názvech souborů. Pokud se pokusíte nahrát soubor prostřednictvím uživatelského rozhraní Jupyter, který má název souboru jiné ASCII, se nepovede tiše (to znamená Jupyter nebude umožňují nahrání souboru, ale nebude vyvolat zobrazen chybová buď). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Chyba při načítání poznámkové bloky větších

Může se zobrazit chyba **`Error loading notebook`** při načtení poznámkové bloky, které jsou větší velikost.  

**Řešení:**

Zobrazí chyba neznamená, že vaše data je poškozený nebo ztraceny.  Poznámkové bloky jsou stále na disku `/var/lib/jupyter`, a můžete provést tyto akce SSH do clusteru k nim získat přístup. Poznámkové bloky ze svého obrázku můžete zkopírovat do místního počítače (pomocí spojovací bod služby nebo WinSCP) jako zálohu abyste předešli ztrátě důležitá data v poznámkovém bloku. Potom můžete SSH tunelem do headnode přístavu 8001 pro přístup k Jupyter bez opakovaného brány.  Odtud můžete zrušte výstupu poznámkového bloku a znovu uložte minimalizovat jeho velikost.

Chcete-li zabránit v budoucnosti tuto chybu, postupujte některé osvědčené postupy:

* Je nutné zachovat malou velikost poznámkového bloku. Výstup z Spark úlohy, odesílané zpět Jupyter zachován v poznámkovém bloku.  Je doporučený postup s Jupyter obecně se zabránilo spuštění `.collect()` na velké RDD společnosti nebo dataframes; Místo toho, pokud chcete prohlížet obsah RDD, zvažte možnost spuštění `.take()` nebo `.sample()` tak, aby výstup do nich všichni moc velký.
* Také při ukládání poznámkového bloku, zrušte zaškrtnutí políčka všech výstupních buněk zmenšit velikost.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Spuštění poznámkového bloku trvá déle než obvykle 

První použití příkazu kód v poznámkovém bloku Jupyter pomocí Spark magické může trvat víc než jednu minutu.  

**Vysvětlení:**
 
K tomu dojde, protože při spuštění na první buňku kód. Na pozadí vyvolá konfigurace relací a Spark, SQL a podregistru kontexty se nastavují. Po nastavení těchto kontextech, první použití příkazu Spustit a to vám to vypadá, pořídili pomocí příkazu dlouhou dobu dokončení.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Časový limit Poznámkový blok Jupyter při vytváření relace

Když Spark clusteru je mimo zdrojů, bude Spark a Pyspark jádra v poznámkovém bloku Jupyter časový limit pokusu o vytvoření relace. 

**Mitigations:** 

1. Uvolněte několik zdrojů informací v Spark obrázku tak, že:

    - Zastavení jiných poznámkových bloků Spark tak, že přejdete do nabídky zavřít a zastavení nebo klepnutí na tlačítko Vypnout v Průzkumníku poznámkového bloku.
    - Zastavení jiných aplikací Spark z vláken.

2. Restartujte požadovaný poznámkový blok jste chtěli spouštění. By měly být umožňující vytvořit relace teď dost zdroje.

##<a name="see-also"></a>Viz taky

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

* [Použití externích balíčků pomocí Jupyter poznámkových bloků](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
