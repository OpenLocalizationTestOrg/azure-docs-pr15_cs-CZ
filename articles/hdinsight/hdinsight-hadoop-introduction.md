 <properties
    pageTitle="Úvod k Hadoop – co je Hadoop na HDInsight? | Microsoft Azure"
    description="Úvod do Hadoop, distribuované velký zpracování dat a analýzy a komponent ekosystému Hadoop vstoupit cloudu na HDInsight."
    keywords="Analýza velkých dat, úvod k hadoop, co je hadoop, hadoop technologii zásobníku, hadoop ekosystému"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Úvod k ekosystému Hadoop na Azure HDInsight

 Tento článek týkající se Úvod do Hadoop Azure HDInsight jeho ekosystému a velký data. Informace o součásti Hadoop, běžné terminologie a scénáře pro analýzu dat velký.

## <a name="what-is-hadoop-on-hdinsight"></a>Co je Hadoop na HDInsight?

Hadoop odkazuje ekosystém otevřít zdroj software, který je rámec pro distribuované zpracování, ukládání a analýza velkých sad dat na clusterů počítačů. Azure HDInsight zpřístupní komponenty Hadoop normálním rozdělením **Hortonworks dat platformu (HDP)** v cloudu, nasadí a ustanovení spravovaných clusterů s vysokou spolehlivost a dostupnosti.  

Apache Hadoop byl původní zdrojový otevřít projekt pro velké zpracování dat. Následující byl vývoj související software a nástroje považovány za část do zásobníku technologii Hadoop, včetně Apache podregistru, Apache HBase, Apache Spark a mnoha dalších. Podrobnosti najdete v článku [Přehled ekosystému Hadoop v HDInsight](#overview) .

## <a name="what-is-big-data"></a>Co jsou velký data?

Velký data popisují velkou část digitálních informací u textu v Twitter kanálu senzor informací z průmyslové zařízení, na informace o procházení vztahů se zákazníky a nákupu na webu. Velký dat může být historických (to znamená uložená data) nebo v reálném čase (jen streamují přímo ze zdroje). Velký dat shromážděny v někdy escalating objemy za stále větších jsou rychlosti a rozbalení různé formáty.

Pro velké dat akce intelligence nebo Přehled musíte shromáždit relevantních dat a zeptejte se pravém. Můžete třeba zkontrolujte, že data přístupný, vyčistit, analýza a potom prezentovat užitečné způsobem. Je to, kde můžou pomoct Analýza velkých dat na Hadoop v HDInsight.

## <a name="overview"></a>Základní informace o ekosystému Hadoop v HDInsight

HDInsight je rozdělení cloudu na Microsoft Azure rychle zvětšující zásobníku Apache Hadoop technologie pro analýzu dat velký. Zahrnuje implementaci Apache Spark HBase, bouře, Prasátko, podregistru, Sqoop, Oozie, Ambari a tak dál. HDInsight lze integrovat s nástrojů business intelligence (BI) například Power BI, Excelu, SQL Server Analysis Services a služby SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop HBase, Spark, bouře a vlastní clusterů

HDInsight obsahuje konfigurace clusteru Apache Hadoop, Spark, HBase nebo bouře. Nebo můžete [Přizpůsobit clusterů akcím skriptu](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: poskytuje spolehlivé úložný prostor [HDFS](#hdfs)a jednoduché programovací model [MapReduce](#mapreduce) procesu a analyzovat data souběžně.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: souběžné zpracování systém, který podporuje zpracování v paměti zvýšit výkon a analýza velkých dat aplikace podnítit works pro SQL přenos dat a strojních učení. V tématu [základní informace: co je Apache Spark v HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: databáze NoSQL založená na Hadoop poskytující RAM a silných konzistence pro velké objemy dat částečně strukturovaná a nestrukturovaná - potenciálně miliardy řádků časy miliony sloupců. Přečtěte si téma [Přehled HBase na HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache bouře</a>**: systém distribuované, v reálném čase výpočtu rychlé zpracování velké datové proudy. Bouře nabízených jako spravovanou cluster v HDInsight. Zobrazit [data v reálném čase snímače analyzovat pomocí bouře a Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Příklady přizpůsobení skriptů

Skript akce jsou skripty, které spustit během vytváření obrázku a mohou sloužit k dodatečný nainstalovat clusteru. Na základě Linux clusterů jedná se o flám skriptů.

Následující příklad skripty jsou poskytnutých týmem HDInsight:

* [Odstín](hdinsight-hadoop-hue-linux.md): Sada webových aplikací slouží k interakci s clusteru. Pouze clusterů Linux.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): grafu zpracování relací modelu mezi věci nebo lidi.

* [R](hdinsight-hadoop-r-scripts-linux.md): otevřít zdrojového jazyka a prostředí statistické výpočetních použít počítač přečíst.

* [Solr](hdinsight-hadoop-solr-install-linux.md): platformu hledání enterprise měřítko, která umožňuje fulltextové vyhledávání na datech.

Informace o vývoji vlastní skript akce, najdete v článku [akci skriptu vývoj s HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Co je součástí Hadoop a nástroje?

Následující součásti a nástroje jsou zahrnuty ve HDInsight.

* **[Ambari](#ambari)**: clusteru zřizování správy, sledování a nástroje.

* **[Avro](#avro)** (Knihovny Microsoft .NET pro Avro): serializaci dat pro rozhraní Microsoft .NET prostředí.

* **[Podregistru & HCatalog](#hive)**: jazyk SQL (Structured Query) – třeba nemusí dotazy a správa vrstvy tabulky a úložiště.

* **[Mahout](#mahout)**: scalable počítače výukové aplikací.

* **[MapReduce](#mapreduce)**: zastaralý rámec pro Hadoop distributed zpracování a řízení zdrojů. V tématu [vláken](#yarn), framework generování další zdroje.

* **[Oozie](#oozie)**: Správa pracovního postupu.

* **[Phoenix](#phoenix)**: vrstva relační databáze přes HBase.

* **[Prasátko](#pig)**: jednodušší skriptování pro MapReduce transformace.

* **[Sqoop](#sqoop)**: Data import a export.

* **[Tez](#tez)**: umožňuje dat vyžadující značnou procesů efektivní ve velkém měřítku.

* **[Vláken](#yarn)**: součást Hadoop základní knihovny a další generování Framework MapReduce software.

* **[ZooKeeper](#zookeeper)**: koordinaci procesů v distribuované systémy.

> [AZURE.NOTE] Informace o konkrétní prvky a informace o verzi najdete v článku [Hadoop součásti, Správa verzí a nabízené služby v HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari je určený pro vytváření, Správa a sledování Apache Hadoop clusterů. Zahrnuje intuitivní kolekce operátor nástroje a robustní sadu rozhraní API, která skrýt složitost Hadoop zjednodušení operaci clusterů. Na základě Linux HDInsight clusterů tématech obě Ambari webové uživatelského rozhraní a rozhraní REST API Ambari zatímco serveru s Windows clusterů poskytují podmnožinu rozhraní REST API. Zobrazení Ambari na HDInsight clusterů povolit modul plug-in možností uživatelského rozhraní.

V tématu [clusterů spravovat HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md) (pouze Linux), [Monitor Hadoop clusterů HDInsight pomocí rozhraní API Ambari](hdinsight-monitor-use-ambari-api.md)a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">rozhraní API Ambari Apache odkaz</a>.

### <a name="avro"></a>Avro (rozhraní Microsoft .NET knihovna Avro)

Knihovna rozhraní Microsoft .NET pro Avro používá formát výměny Apache Avro kompaktní binární data pro serializaci pro rozhraní Microsoft .NET prostředí. Definovat schéma bez ohledu na jazyk, který uzavře interoperability jazyk pomocí <a target="_blank" href="http://www.json.org/">Skriptu JavaScript Object Notation (JSON)</a> , význam dat serializován v jednom jazyce můžete přečíst v jiném. Podrobné informace o formátu najdete v < cíl = "prázdné" href _ = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specifikace</a>.
Formát souborů Avro podporuje distribuované MapReduce programovací model. Soubory jsou "rozdělit tabulku", což znamená, můžete požádat o jakékoli místo v souboru a spuštění čtení od určitého bloku. Pokud chcete zjistit, jak, najdete v článku [serializovatelnou dat s knihovnou rozhraní Microsoft .NET pro Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Systém distribuované souboru na Hadoop (HDFS) je, s MapReduce a vláken, je základní ekosystému Hadoop distributed file system. HDFS je standardní soubor systému Hadoop clusterů na HDInsight.

### <a name="hive"></a>Podregistru & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache podregistru</a> je datový sklad software založená na Hadoop, který umožňuje dotazů a správa velkých sad dat v distribuované úložiště pomocí jazyka SQL profesionálové s názvem HiveQL. Podregistru, jako je Prasátko, je odběru nad MapReduce. Po spuštění podregistru převádí dotazy na řadu MapReduce úlohy. Podregistru je entity blíž k relační databáze systému řízení než Prasátko a tedy vhodné pro použití s více strukturovaná data. Prasátko Nestrukturovaná data, je lepší volbou. V tématu [použití podregistru s Hadoop v HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je tabulka a úložiště vrstvy správy pro Hadoop, který obsahuje zobrazení relačních dat. V HCatalog můžete číst a psát soubory v jiném formátu, ve kterém můžete zadán podregistru SerDe (serializer převodník).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> je knihovna scalable počítače výukové algoritmů spuštěné v operačním systému Hadoop. Pomocí Principy statistiky, počítače výukové aplikací naučit systémy Další z dat a použití dřívější výsledky k určení budoucí chování. Zobrazit [pomocí Mahout na Hadoop doporučení film generovat](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je rámec starší software pro Hadoop pro vytváření aplikací pro dávkové proces velký sady dat souběžně. Úlohy MapReduce rozdělí velkých datových sad a jsou data uspořádána do dvojice klíč pro zpracování.

[Vláken](#yarn) je další generování zdroje správce a aplikace rámec Hadoop a se označuje jako MapReduce 2.0. MapReduce úloh spustit vláken.

Další informace o MapReduce najdete v článku <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> na wikiwebu Hadoop.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinaci pracovního postupu, který má na starosti Hadoop úlohy. Integrovaný s zásobníku Hadoop a podporuje Hadoop úlohy MapReduce, Prasátko, podregistru a Sqoop. Jej lze také naplánovat úlohy specifické pro systém, jako je programy nebo skriptů prostředí. V tématu [použití Oozie s Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> překročení HBase vrstvě relační databáze. Phoenix obsahuje JDBC ovladač, který umožňuje uživatelům k vytváření dotazů a Správa tabulky SQL přímo. Phoenix překládá dotazů a dalších údajů do nativní volání rozhraní API NoSQL – místo použití MapReduce – tedy umožňuje rychlejší aplikace NoSQL ukládá. V tématu [použití Apache Phoenix a SQuirreL s HBase clusterů](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Prasátko
<a  target="_blank" href="http://pig.apache.org/">Prasátko Apache</a> je uceleném platformu, která umožňuje provádět složité transformace MapReduce velkých sad dat pomocí jednoduchého skriptovacího jazyka s názvem Prasátko (latinka). Prasátko převádí skripty Prasátko latinka tak, aby se spouštějí v rámci Hadoop. Funkce definované uživatelem (UDF) rozšiřte Prasátko latinka můžete vytvořit. V tématu [použití Prasátko s Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj, že přenosy hromadně co nejefektivněji dat mezi Hadoop a relačních databází SQL nebo jiných ukládá strukturovaná data. V tématu [použití Sqoop s Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založená na vláken Hadoop, který se spustí složité, Acyklické grafy obecné zpracování dat. Je následník flexibilní a výkonných rámce MapReduce umožňující dat vyžadující značnou procesy, třeba podregistru, spusťte mnohem efektivněji ve velkém měřítku. V tématu ["Použití Apache Tez pro lepší výkon" v používání podregistru a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>VLÁKEN
Apache vláken je další generování MapReduce (MapReduce 2.0 nebo MRv2) a podporuje zpracování dat scénáře za MapReduce dávkové zpracování s větší škálovatelnost a v reálném čase zpracování. VLÁKEN poskytuje řízení zdrojů a rámec distribuované aplikace. MapReduce úloh spustit vláken.

Další informace o vláken najdete v tématu <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (vláken)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> souřadnic procesy v velkých distribuované systémů pomocí obor sdílené hierarchických dat Registry (znodes). Znodes obsahovat krátký meta informace potřebné k koordinaci procesů: stav, umístění, konfigurace a tak dál.

## <a name="programming-languages-on-hdinsight"></a>Programování jazyků na HDInsight

HBase, bouře a Spark clusterů serveru HDInsight clusterů – Hadoop, – podpora počet programovacím jazyce, ale některé nejsou nainstalovány ve výchozím nastavení. Pro knihovny moduly nebo balíčků Nenainstalováno ve výchozím nastavení, použijte akci skriptu komponentu nainstalujte. V tématu [vývoj akce skriptů s HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Výchozí programování podpora jazyků

Ve výchozím nastavení clusterů HDInsight podpory:

* Java

* Python

Další jazyky je možné nainstalovat pomocí skriptu akce: [vývoj akce skriptů s HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jazyky Java virtuálního počítače (JVM)

Spousty jazyků než Java spuštěním pomocí jazyka Java virtuálního počítače (JVM); spuštění některé z těchto jazyků může vyžadovat dodatečný nainstalovaným clusteru.

Tyto na základě JVM jazyky jsou podporovány na HDInsight clusterů:

* Clojure

* Jython (Python jazyka Java)

* Scala

### <a name="hadoop-specific-languages"></a>Specifické pro Hadoop jazyky

HDInsight clusterů podporují následující jazyky, které jsou specifické pro ekosystému Hadoop:

* Prasátko latinka Prasátko úloh

* HiveQL podregistru úlohy a SparkSQL


## <a name="advantage"></a>Výhody Hadoop v cloudu

V rámci ekosystému Azure cloudu Hadoop v HDInsight nabízí celou řadu výhod mezi nimi:

* Automatické vytváření Hadoop clusterů. HDInsight clusterů jsou mnohem snadnější vytvořit než ruční konfigurace Hadoop clusterů. Podrobnosti najdete v tématu [poskytování Hadoop clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Součásti Hadoop stavu obrázek. Podrobnosti najdete v tématu [Hadoop součásti, Správa verzí a nabízené služby v HDInsight][component-versioning].

* Dostupnost a spolehlivosti clusterů. Podrobnosti najdete v části [dostupnost a spolehlivost Hadoop clusterů HDInsight](hdinsight-high-availability-linux.md) .

* Jsou efektivní a vyplatí úložný prostor úložiště objektů Blob Azure, možnost kompatibilní s prohlížečem Hadoop. Podrobnosti najdete v části [úložiště objektů Blob Azure použití s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md) .

* Integrace s dalšími Azure službami, včetně [webových aplikací Web apps](https://azure.microsoft.com/documentation/services/app-service/web/) a [SQL databáze](https://azure.microsoft.com/documentation/services/sql-database/).

* Další formáty OM a typy výpočetnímu clusteru HDInsight. Přečtěte si téma [Hadoop součásti, Správa verzí a nabízené služby v HDInsight] [ component-versioning] podrobnosti.

* Clusteru měřítka. Změna velikosti obrázku umožňuje změnit počet zobrazených uzly pracovního clusteru HDInsight bez odebrání nebo opětovné vytvoření.

* Virtuální podpora sítě. HDInsight clusterů lze s Azure virtuální sítě podporují izolace cloudové zdroje nebo hybridní scénáře, které odkazují cloudové zdroje s hodnotami ve vaší datacentra.

* Nízké zadávání nákladů. Zahájení [bezplatnou zkušební verzi](https://azure.microsoft.com/free/)nebo použijte [Podrobnosti o cenách HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Chcete další informace o výhodách na Hadoop v HDInsight, podívejte se na [stránce funkce Azure HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight standardních a HDInsight Premium

HDInsight obsahuje velký dat pro cloud ve dvou kategorií, Standard a Premium. HDInsight Standard poskytuje enterprise měřítko obrázku, které mohou organizace použít ke spuštění jejich pracovního vytížení velký data. HDInsight Premium je založena na a poskytuje pokročilé analýzy a možnosti zabezpečení pro HDInsight obrázku. Další informace najdete v tématu [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Další informace o Analýza velkých dat, Hadoop a HDInsight učební zdroje pro

Vytvořte v tomto úvodu k Hadoop v cloudu a analýzy velký dat pomocí níže uvedených zdrojů.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop si přečtěte následující dokumentaci pro HDInsight

* [HDInsight si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/hdinsight/): na stránce si přečtěte následující dokumentaci pro Hadoop na Azure HDInsight s odkazy na články, videa a další zdroje.

* [Začínáme s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): rychlého kurz pro zřizování clusterů HDInsight Hadoop a spouštění dotazů podregistru vzorku.

* [Začínáme s Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): rychlého kurz pro vytváření Spark obrázku a spouštění dotazů interaktivní Spark SQL.

* [Použít R Server na HDInsight](hdinsight-hadoop-r-server-get-started.md): použití webových aplikací R serveru ve HDInsight Premium.

* [Poskytování HDInsight clusterů](hdinsight-hadoop-provision-linux-clusters.md): Zjistěte, jak zřídit HDInsight Hadoop clusteru prostřednictvím portálu pro Azure, rozhraní příkazového řádku Azure nebo Azure Powershellu.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Další informace o knihovně software Apache Hadoop rámec, který umožňuje distribuované zpracování velkých sad dat přes clusterů počítačů.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Další informace o architektura a návrh systému Hadoop Distributed soubor, na primární úložiště systému Hadoop aplikacemi.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">Kurz MapReduce</a>: Další informace o programovací rámec pro psaní Hadoop aplikací, které rychle zpracovat velké objemy dat paralelně na velkých clusterů výpočetním uzlů.


### <a name="microsoft-business-intelligence"></a>Funkce business intelligence společnosti Microsoft

Nástroje známé business intelligence (BI) – třeba Excel, PowerPivot, SQL Server Analysis Services a služby SQL Server Reporting Services - načíst, analyzovat a vytvářet sestavy dat integrovaný s HDInsight pomocí doplňku Power Query nebo ovladač ODBC podregistru Microsoft.

Tyto nástroje BI můžou pomoct v analýzách velký dat:

* [Připojit Excel k Hadoop pomocí Power Query](hdinsight-connect-excel-power-query.md): Přečtěte si, jak připojit Excel k účtu Azure úložiště jsou uložená data spojená se svůj cluster HDInsight pomocí Microsoft Power Query pro Excel. Windows workstation povinné. Práce s Windows nebo Linux pomocí obrázku.

* [Připojit Excel k Hadoop s ovladač ODBC podregistru Microsoft](hdinsight-connect-excel-hive-odbc-driver.md): Přečtěte si, jak import dat z Hdinsightu pomocí ovladač ODBC podregistru Microsoft. Windows workstation povinné. Práce s Windows nebo Linux pomocí obrázku.

* [Platformu Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Další informace o Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavení SharePoint serveru 2013 a SQL Server BI.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
