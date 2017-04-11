<properties
    pageTitle="Poznámky k Hadoop prvky na Azure HDInsight | Microsoft Azure"
    description="Poznámky k nejnovější verzi a verze součástí Hadoop pro Azure HDInsight. Získejte vývoj tipy a podrobnosti pro Hadoop Apache bouře a HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Poznámky k verzi pro Hadoop prvky na Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Poznámky k verzi 10/26/2016 R serveru na HDInsight

- Server R místní zřizování clusteru HDInsight má Modernější.
- Server R místní HDInsight je teď k dispozici jako běžný HDInsight "R Server" clusteru typ a už nainstalovaný jako samostatnou aplikaci HDInsight. Uzel okraje a R serveru binární jsou teď zřízení jako součást nasazení serveru R obrázku. To zlepšuje rychlost a spolehlivosti zřizování. Ceny modelu doplňku pro R serveru se aktualizuje podle toho.
- R Server clusteru typu cena se teď podle standardní osy cena plus R serveru přirážky cena. Vrstvy Premium bude teď rezervováno pro prémiových funkcí dostupných v různých typů a nebudou se dá použít pro typ obrázku R serveru. Tato změna nemá vliv efektivní ceny serveru R, změní se pouze kalendářního nákladů v rozpisu. Všechny existující clusterů R serveru bude pokračovat v práci a ARM šablony, zůstanou fungovat až do odstranění oznámení. **Když chcete aktualizovat skriptů nasazení použití nové šablony ARM se doporučuje.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Poznámky k verzi 08/30/2016 R serveru na HDInsight

Plné verze čísla na základě Linux HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP   |Sestavení Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Čísla plnou verzi serveru s Windows HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Poznámky k verzi 08/17/2016 R serveru na HDInsight

- R Server 8.0.5 – hlavně vydání oprava chyb. V části [Poznámky k verzi serveru R](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) dalších informací. 
- Balení AzureML na uzel okraj – [balíček R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) umožňuje R modely publikování a spotřebované množství jako webové služby Azure ML.  V části ["umožňují modelu"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) náš ["přehled o R serveru na HDInsight"](hdinsight-hadoop-r-server-overview.md) článku Další informace.
- Závislosti mezi Linux [horních 100 nejoblíbenější balíčků R](https://github.com/metacran/cranlogs) – tyto Linux balíčku závislosti jsou teď jsou předinstalované.  
- Možnost používání CRAN repo při přidání R balíčky datových uzlů. Naleznete v části ["Balíčků Instalační R"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) náš ["Začít používat R Server místní HDInsight"](hdinsight-hadoop-r-server-get-started.md) článku Další informace.
- Vylepšené spolehlivosti R serveru zřizování uplatněna clusterů.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 08/01/2016

Plné verze čísla na základě Linux HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP   |Sestavení Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Čísla plnou verzi serveru s Windows HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Typ obrázku (třeba Spark Hadoop, HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Provádění změn HDInsight 3.4 clusterů | Výchozí hodnoty pro následující konfigurace podregistru se změní pro lepší výkon <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Služba    | Všechny| NENÍ K DISPOZICI|
| Tato verze zahrnuje tyto opravy | 13632 PODREGISTRU, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Služba    | Všechny| NENÍ K DISPOZICI

## <a name="notes-for-07142016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 07/14/2016

Plné verze čísla na základě Linux HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP   |Sestavení Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Čísla plnou verzi serveru s Windows HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 07/07/2016

Plné verze čísla na základě Linux HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Čísla plnou verzi serveru s Windows HDInsight clusterů používaný v této verzi:

|HDI |HDI verze obrázku   |HDP |Sestavení HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Typ obrázku (třeba Spark Hadoop, HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [HDInsight nástroje pro IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | Modul plug-in IntelliJ MYŠLENCE HDInsight Spark clusterů je teď integrovaný s Azure nástrojů pro IntelliJ. Podporuje Azure SDK v2.9.1 nejnovější Java SDK a obsahuje všechny funkce verze ze samostatného HDInsight modul plug-in pro IntelliJ.| Nástroje    | Spark| NENÍ K DISPOZICI|
| [HDInsight nástroje pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure sada nástrojů pro Eclipse nyní podporuje clusterů HDInsight Spark. Umožňuje tyto funkce. <ul><li>Vytvoření a aplikace Spark snadno Scala a Java s prvotřídní vytváření podpora technologie IntelliSense automatický formát kontroly chyb, atd.</li><li>Vyzkoušejte aplikaci Spark místně.</li><li>Odeslat úlohy HDInsight Spark clusteru a načtěte výsledky.</li><li>Přihlaste se k Azure a přístup k všech clusterů Spark přidružené předplatné Azure.</li><li>Najděte všechny zdroje přidružené úložiště clusteru HDInsight Spark.</li></ul>| Nástroje    | Spark| NENÍ K DISPOZICI

Začnete s touto verzí, jsme změnili hodnotu Host OS oprav zásad pro clusterů na základě Linux HDInsight. Cílem nové zásady je značně sníží počet restartování počítače z důvodu opravy. Nové zásady zůstanou oprava virtuálních počítačích (VMs) na Linux clusterů každé pondělí nebo čtvrtek od UTC 12 dopoledne Střídavý způsobem přes uzlů v dané obrázku. Však dané OM restartuje pouze maximálně jednou každých 30 dní kvůli hosta OS opravy. Kromě toho prvním restartování nově vytvořený clusteru nikdy nedojde dříve než 30 dní od datum vytvoření obrázku.

>[AZURE.NOTE] Tyto změny se použijí jenom u nově vytvořený clusterů rovna nebo větší než tato prodejní verze.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 06/06/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

|HDP    |HDI verze    |Spark verze  |Číslo Ambari sestavení    |Číslo sestavení HDP|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Typ obrázku (třeba Spark Hadoop, HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark na HDInsight obecně neexistuje | Tato verze přináší vylepšení dostupnost, škálovatelnost a produktivita otevřít zdroj Apache Spark na HDInsight. <ul><li>Odvětví vést dostupnost SLA 99,9 %, což je vhodné pro náročné enterprise úloh.</li><li>Použití úložiště jezera dat Azure Scalable úložiště vrstvy.</li><li>Nástroje pro práci se pro každé fázi průzkum a vývoje. Jupyter poznámkových bloků pomocí přizpůsobené jádra Spark povolit interaktivní průzkum, integrace s odkazem řídicí panely BI jako Power BI, Tableau a Qlik je vhodný k sdílení snadné dat a vytváření sestav funkce nepřetržitý, IntelliJ modul plug-in je spolehlivé možnost dlouhodobou kód artefakt vývoje a ladění.</li></ul>| Služba    | Spark| NENÍ K DISPOZICI|
| HDInsight nástroje pro IntelliJ | Toto je IntelliJ MYŠLENCE modul plug-in pro clusterů HDInsight Spark. Umožňuje tyto funkce.<ul><li>Vytvoření a aplikace Spark snadno Scala a Java s prvotřídní vytváření podpora technologie IntelliSense automatický formát kontroly chyb, atd.</li><li>Vyzkoušejte aplikaci Spark místně.</li><li>Odeslat úlohy HDInsight Spark clusteru a načtěte výsledky.</li><li>Přihlaste se k Azure a přístup k všech clusterů Spark přidružené předplatné Azure.</li><li>Najděte všechny zdroje přidružené úložiště clusteru HDInsight Spark.</li><li>Procházení všech historie úlohy a úlohách pro svůj cluster HDInsight Spark.</li><li>Ladění Spark úlohy vzdáleně ze stolního počítače.</li></ul>| Nástroje    | Spark| NENÍ K DISPOZICI

## <a name="notes-for-05132016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 05/13/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Typ obrázku (třeba Spark Hadoop, HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Podnítit verze aktualizace a další opravy chyb | Tato verze aktualizace v HDInsight clusteru verze Spark 1.6.1 a opravy další chyby| Služba    | Spark| NENÍ K DISPOZICI

## <a name="notes-for-04112016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 04/11/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-beze změny)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Upgrade vlastní metastore při potížích pro HDI 3.4 | Vytváření clusteru selže, pokud jste použili vlastní metastore, která byla dříve použité na nižší verzi jiného clusteru HDInsight. To byla kvůli chybu upgradu skript, která má nyní pevné| Vytvoření obrázku    | Všechny | NENÍ K DISPOZICI
| Obnovení pád Livius | Poskytuje odolnosti stavu projektu pro úlohy ověřeny pomocí Livius | Spolehlivosti | Spark na Linux| NENÍ K DISPOZICI
| Obsah Jupyter HA | Poskytuje možnost Uložit a načíst Jupyter Poznámkový blok obsah do a z úložiště účet spojený s clusteru. Další informace najdete v tématu [jádra umožňující Jupyter poznámkové bloky](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Poznámkové bloky | Spark na Linux| NENÍ K DISPOZICI
| Odebrání hiveContext v poznámkových blocích Jupter | Použití `%%sql` magic namísto `%%hive` magické. SqlContext odpovídá hiveContext. Další informace najdete v tématu [jádra umožňující Jupyter poznámkových bloků](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Poznámkové bloky    | Spark clusterů na Linux| NENÍ K DISPOZICI
| Odstranění starších verzí Spark | Starší verze Spark 1.3.1 Odstraní ze služby na 5/31 | Služba | Spark clusterů v systému Windows | NENÍ K DISPOZICI

## <a name="notes-for-03292016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 03/29/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - beze změny)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - beze změny)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - beze změny)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Přidané HDInsight 3.4 aktualizované HDP verzemi a u všech HDInsight clusterů | V této verzi jsme přidali HDInsight v3.4 (podle HDP 2.4) a také aktualizuje jiných verzí HDP. Poznámky k verzi HDP 2.4 jsou dostupné [tady](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) a další informace o HDInsight verze mohou být nalezených [tady](hdinsight-component-versioning.md).| Služba    | Všechny Linux clusterů| NENÍ K DISPOZICI
| HDInsight Premium | HDInsight je teď dostupná v dvě kategorie - Standard a Premium. HDInsight Premium momentálně v náhledu a k dispozici pouze pro Hadoop a Spark clusterů Linux. Další informace najdete [tady](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Služba    | Hadoop a Spark na Linux| NENÍ K DISPOZICI
| Server Microsoft R | HDInsight Premium poskytuje Microsoft R serveru, který může být součástí systému Hadoop a Spark clusterů na Linux. Další informace najdete v článku [Přehled R Server, místní HDInsight](hdinsight-hadoop-r-server-overview.md).| Služba    | Hadoop a Spark na Linux| NENÍ K DISPOZICI
| Spark 1.6.0 | HDInsight 3.4 clusterů nyní obsahují Spark 1.6.0| Služba    | Spark clusterů na Linux| NENÍ K DISPOZICI
| Vylepšení Jupyter poznámkového bloku | Poznámkové bloky Jupyter dostupné Spark clusterů teď poskytují další Spark jádra. Obsahují vylepšení jako je použití %% magické automatického vizualizace a integrace s knihovnami vizualizace Python (například matplotlib). Další informace najdete v tématu [jádra umožňující Jupyter poznámkové bloky](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Služba | Spark clusterů na Linux | NENÍ K DISPOZICI

## <a name="notes-for-03222016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 03/22/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - beze změny)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - beze změny)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - beze změny)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů | V této verzi jsme aktualizovali HDInsight verze pro všechny HDInsight clusterů| Služba    | Všechny| NENÍ K DISPOZICI


## <a name="notes-for-03102016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 03/10/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - beze změny)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů | V této verzi jsme aktualizovali HDInsight verze pro všechny HDInsight clusterů| Služba    | Všechny| NENÍ K DISPOZICI

## <a name="notes-for-01272016-release-of-hdinsight"></a>Poznámky k verzi HDInsight 01/27/2016

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - beze změny)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů | V této verzi jsme aktualizovali HDInsight verze pro všechny HDInsight clusterů| Služba    | Všechny| NENÍ K DISPOZICI

## <a name="notes-for-12022015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 12/02/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - beze změny)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - beze změny)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Přidané HDInsight 3.3 aktualizované HDP verzemi a u všech HDInsight clusterů | V této verzi jsme přidali HDInsight v3.3 (podle HDP 2.3) a také aktualizuje jiných verzí HDP. Poznámky k verzi HDP 2.3 jsou dostupné [tady](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) a další informace o HDInsight verze mohou být nalezených [tady](hdinsight-component-versioning.md).| Služba    | Všechny| NENÍ K DISPOZICI

## <a name="notes-for-11302015-release-of-hdinsight"></a>Poznámky k verzi 11/30/2015 HDInsight

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů a HDP verze pro HDInsight 3,2 clusterů (Windows a Linux) | V této verzi jsme aktualizovali HDInsight a HDP verze | Služba    | Všechny| NENÍ K DISPOZICI


## <a name="notes-for-10272015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 10/27/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - beze změny)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů (Windows a Linux) | V této verzi jsme aktualizovali HDInsight a HDP verze | Služba    | Všechny| NENÍ K DISPOZICI
| Pevné Jupyter pro Windows Spark clusterů s velkými písmeny clusterů | Clusterů, ve kterých názvy DNS určené velkými písmeny dosáhl problémy s poznámkovými bloky Jupyter kvůli žádost o kontrolu origin. Oprava bylo změnit název DNS pro konfiguraci společnosti Jupyter na malá písmena. | Služba    | HDInsight Spark (Windows)| NENÍ K DISPOZICI


## <a name="notes-for-10202015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 10/20/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Výchozí verze HDP změní na HDP 2.2 | Na výchozí verzi HDInsight Windows clusterů se změní na HDP 2.2. HDInsight verze 3,2 (HDP 2.2) byl obecně k dispozici v od únor 2015. Tato změna pouze překlápět obrázku na výchozí verzi při explicitní výběru nedošlo při zřizování clusteru pomocí portálu Azure, rutiny prostředí PowerShell nebo SDK. | Služba    | Všechny| NENÍ K DISPOZICI                  |
|Změny v seznamu Formát názvu OM pro nasazení více HDInsight na Linux clusterů v jedné virtuální sítě | Podpora zavádění více clusterů HDInsight Linux v jedné virtuální sítě se přidá v této verzi. V rámci tohoto formátu jmen virtuálního počítače v clusteru změnil z headnode\*, workernode\* a zookeepernode\* na kola hn\*, wn\*a zk\* v tomto pořadí. Je doporučený postup převzít přímé závislost formátu jmen virtuálního počítače, protože to je měnit. Stiskněte klávesovou zkratku "hostname -f" na místním počítači nebo rozhraní API Ambari k určení seznam hosts a mapování komponenty pro hosts. Můžete najít další informace na [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) a [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Služba | HDInsight clusterů na Linux | NENÍ K DISPOZICI |
| Změny konfigurace | HDInsight 3.1 clusterů jsou nyní k dispozici následující konfigurace: <ul><li>tez.yarn.ATS.Enabled a yarn.log.server.url. Díky aplikační Server časovou osu a protokol serveru moct vytisknout se protokoly.</li></ul>HDInsight 3,2 clusterů byly změněny následující konfigurace: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version byl nastaven do 2. Díky použití V2 verzi FileOutputCommitter.</li></ul> | Služba | Všechny | NENÍ K DISPOZICI |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 09/09/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů | V této verzi jsme aktualizovali HDInsight verze | Služba    | Všechny| NENÍ K DISPOZICI                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 07/31/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Oprava Spark clusteru uzel znovu obrázků pracovního postupu | Pevná chyba, která je příčinou Spark uzlech není obnovit po znovu obrázek | Služba    | Spark| NENÍ K DISPOZICI                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 07/31/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDInsight u všech HDInsight clusterů | V této verzi jsme aktualizovali HDInsight verze | Služba    | Všechny| NENÍ K DISPOZICI                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 07/07/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Tato verze obsahuje tyto aktualizace.

| Název                                           | Popis                                          | Dotčeném oblasti (například služba, komponenty nebo SDK) | Clusteru typu (například Hadoop HBase a bouře) | JIRA (pokud existuje) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualizované verze HDP HDInsight 3,2 clusterů | V této verzi HDInsight 3,2 nasadí HDP 2.2.6.1-0012 | Služba    | Všechny                                                 | NENÍ K DISPOZICI                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 06/26/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Aktualizované verze HDP HDInsight 3,2 clusterů</td>
<td>V této verzi HDInsight 3,2 nasadí HDP 2.2.6.1</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 06/18/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Otevření další porty HTTPS</td>
<td>Cloudové služby teď otevře 5 porty 8001 k 8005 clusteru např na https://<clustername>.azurehdinsight.net:8001/. Požadavky na tyto adresy URL jsou ověřovány stejné heslo mechanismus základní ověřování jako port 443. Tyto porty vytvořit vazbu s portem stejné na aktivní headnode. Akce skriptu mohou sloužit k naslouchají relacím na tyto porty headnode a směrovat mimo clusteru služeb zákazníkům.</td>
<td>Cloudová služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Přerušované problém náhodně MapReduce pro 3,2 HDInsight</td>
<td>Oprava málo, chybovému spor v náhodně snížit mapy na velkých clusterů výsledkem occassional úkolu selhání. Další informace najdete v tématu <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> .</td>
<td>Základní Hadoop</td>
<td>Všechny</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>

<tr>
<td>Přesunutí na poslední Azure Java SDK 2.2 HDInsight 3,2</td>
<td>Přesune do nejnovější verzi Azure SDK jazyka Java používá ovladač WASB. Nejnovější SDK obsahuje několik opravy a poznámky k verzi pro stejný jsou k dispozici na https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Základní Hadoop</td>
<td>Všechny</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>Přesunutí HDP 2.1.15 HDInsight 3.1 clusterů</td>
<td>Poznámky k verzi Hortonworks aktualizací jsou dostupné <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">tady</a>.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 06/04/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - beze změny)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Oprava chyby 502 Chybná brána bouře clusterů</td>
<td>Tato verze se dá vyřešit chybu došlo k ovlivnění odeslání úlohy rozhraní API, která způsobila webu tak, aby předcházet restartovat počítač.</td>
<td>Služba</td>
<td>Bouře</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 06/01/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - beze změny))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - beze změny)
* SDK 1.5.8


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Různé opravy chyb</td>
<td>Tato verze se dá vyřešit chyby týkající se vytváření obrázku.</td>
<td>Služba</td>
<td>Všechny typy obrázku</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 05/27/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Další verze obrázku a SDK nejsou nasazeny jako součást této verzi.


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Aktualizace HDP 2.2</td>
<td>Tato verze HDInsight 3,2 obsahuje HDP 2.2.6 a přináší několik důležitých opravy chyb HDInsight. Poznámky vydání plné verze je k dispozici v <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 poznámky</a>.</td>
<td>HDP</td>
<td>Všechny typy obrázku</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Změna výchozí vláken kontejneru paměti konfigurace</td>
<td>V této aktualizaci výchozí dostupnou pamětí vláken kontejnerů (yarn.nodemanager.resource.memory mb a yarn.scheduler.maximum přidělení mb), spuštění uzel správcem, zvětšené tak, aby 5632MB. Dříve to byl snížené 4608 MB, ale podle různých spuštění úlohy, bude nová hodnota musí nabídnout lepší spolehlivosti a výkonu na většině projekty, tedy je lepší výchozí. Jako obvykle, když jste mají závislostí důležité této konfiguraci paměti, nastavte ho explicitně při vytváření clusteru.</td>
<td>HDP</td>
<td>Všechny typy obrázku</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Výchozí konfigurace dostupná pro HBase a bouře clusterů</td>
<td>Tuto aktualizaci obnoví Hbase a bouře clusterů jako Hadoop clusterů použít stejné hodnoty vláken configs. Důvodem je k dostupná všech typů obrázku.</td>
<td>HDP</td>
<td>HBase, bouře</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 05/20/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Podpora EventHub SCP.NET</td>
<td>Balíčky aktualizované obrázku pro HDInsight bouře přenesení nových funkcí SCP.NET. Teď bude mít přístup do nového rozhraní API v Tvůrci topologie usnadňují jejich použití EventHubSpout nebo Java Spouts. Musíte aktualizovat SCP.NET klienta SDK fungovala s novou clusterů tak, jak byly aktualizovány smlouvy. Informace o nové rozhraní API, použití a vydání poznámky (včetně opravy chyb) získáte v souboru Readme součástí balíčku nuget SCP.NET.</td>
<td>A nástroje</td>
<td>Bouře HDInsight 3,2 clusterů</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Aktualizace ovladače JDBC</td>
<td>Aktualizovat ovladač v sqljdbc_4.1.5605.100 verze SQL serveru podporované.</td>
<td>Metastore</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 04/27/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Oprava DLL závislostí</td>
<td>Odebere HDInsight závislost na Jednotková testovací Framework.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Oprava chyby časování</td>
<td>Clusteru vytvořit žádost o nyní čeká na přijmout před dotazování na stav žádost umístění</td>
<td>SDK</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 04/14/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Opravy chyb tez</td>
<td>Opravy. Apache TEZ 2214 a TEZ 1923 najdete v této verzi aplikace HDI 3,2. Konkrétně jsou potřebné pro určité podregistru dotazy na Tez, které vyžadují náhodné značné množství dat.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ. 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 04/06/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Aktualizace pro odebrání některých vnitřní třídy HDInsight na Linux.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Knihovna Avro 1.5.6</td>
<td>Přidané <b>KnownTypeAttribute</b> metoda <b>GetAllKnownTypes</b>. Pevné NullReferenceException po null GetAllKnownTypes metody typu.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Opravy chyb</td>
<td>Různé opravy chyb služby</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 04/01/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Možnost povolit nebo zakázat vzdálené plochy přihlašovacích údajů v systému Windows clusterů prostřednictvím .NET SDK</td>
<td>Programový podpora pro povolení nebo zakázání RDP přihlašovacích údajů ve Windows.</td>
<td>SDK</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Možnost povolit vzdálené plochy přihlašovací údaje na clusterů při jejich zřízení</td>
<td>Programový podpora pro povolení vzdálené plochy přihlašovací údaje, jako se vytváří clusteru. Tato možnost odebere krocích nejdřív zřizování clusteru a povolení vzdálené plochy.</td>
<td>SDK</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Upgradované Python 2.7.8</td>
<td>Upgradované Python na HDInsight clusterů Python 2.7.8, která obsahuje několik důležitých zabezpečení opravy HDInsight verze 2.1, 3.0, 3.1 a 3,2</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Změna konfigurace vláken</td>
<td>Změněné vláken konfigurace yarn.resourcemanager.max dokončení aplikací 1000 pro všechny typy clusteru HDInsight verze 3.1 a 3,2. Tato hodnota řídí pouze seznam dokončené aplikací v uživatelském rozhraní vláken. Pokud chcete získat informace o aplikacích, které byly odeslány před verzí v seznamu aplikací zobrazují v uživatelském rozhraní, můžete přímo přejít na server historie.</td>
<td>VLÁKEN</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Změna velikosti uzlů v HBase obrázku</td>
<td>HBase clusterů teď povolit změny velikosti uzly (nahoru a dolů) HDInsight verze 3.1 a 3,2</td>
<td>Služba</td>
<td>HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>JDBC upgrade</td>
<td>Upgradovaný na verzi sqljdbc_4.0.2206.100 ovladačů SQL JDBC verzi pro HDInsight verze 3,2. Tato verze obsahuje rozšíření důležité zabezpečení.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Aktualizace konfigurace JVM</td>
<td>Aktualizované JVM konfigurace networkaddress.cache.ttl na 300 sekund z výchozí hodnoty pro HDInsight verze 3.1 a 3,2 -1. Tato hodnota konfigurace řídí mezipaměti zásad pro vyhledávání úspěšné názvů názvové služby. To opravy chyb souvisejících s Kvetoucí a zmenšením HBase clusterů.</td>
<td>Služba</td>
<td>HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Upgrade na Azure úložiště SDK jazyka Java 2.0</td>
<td>HDInsight verze 3,2 je upgradovat na nejnovější verzi Azure úložiště SDK určenou Java. Tato stránka obsahuje několik důležitých opravy chyb nad aktuálním 0.6.0 verze.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Upgrade na Apache WASB zdrojového kódu</td>
<td>HDInsight verze 3,2 teď používá nejnovější kódu pro ovladače WASB systému souborů Apache Hadoop. Díky této změně ovladač WASB teď sbalí jako samostatný sklenice. Toto je čistě změnu balení a neobsahuje žádné změny v chování ovladače WASB. Je v poli Název tohoto souboru SKLENICE hadoop azure 2.6.0.jar.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Aktualizace názvu souboru v HDInsight 3,2 JAR</td>
<td>Tato aktualizace k Hdinsightu verze 3,2 obsahuje několik opravy chyb a přešli několik sklenic po interní g jako součást HDP. Všimněte si, že tyto soubory SKLENICE interní HDP zabalit a ne pro přímého použití aplikacemi zákazníka. Aplikace by měl balíček vlastní verzi sklenic po g, tak, aby upgradu sklenic po g interní HDP přerušení aplikací zákazníka.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 03/03/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - beze změny)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-. 2340 - beze změny)
* SDK 1.5.0 (beze změny)

Tato verze obsahuje následující aktualizaci.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Vyšší spolehlivost</td>
<td>Jsme volání oprav, které umožňují službu rozšířit lépe lepší načíst týkající vytváření clusteru.</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 02/18/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - beze změny)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-. 2340)
* SDK 1.5.0

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>HDInsight 3,2 clusterů</td>
<td>Hadoop 2.6/HDP2.2 neexistuje clusterů 3,2 HDInsight. Obsahuje hlavní změny u všech částí Otevřít zdroj. Další informace najdete v tématu Co je nového v HDInsight a <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">poznámky k verzi HDP 2.2.0.0</a>.</td>
<td>Otevřete zdrojový software</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>HDinsight na Linux (verze Preview)</td>
<td>Clusterů můžete nasadit spuštěna systémem Ubuntu Linux. Další informace najdete v tématu Začínáme s HDInsight na Linux.</td>
<td>Služba</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Obecné bouře dostupnosti</td>
<td>Apache bouře clusterů jsou obvykle k dispozici. Další informace najdete v článku Začínáme pomocí bouře v HDInsight.</td>
<td>Služba</td>
<td>Bouře</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Virtuální počítač velikosti</td>
<td>Azure HDInsight je dostupná na další typy virtuálního počítače a velikosti. HDInsight můžete využít A2 A7 formáty vytvořené pro obecné účely; Uzly D řady, které funkce pevných látek jednotky (disky SSD) a 60 procent rychlejší procesory; a velikosti A8 a A9, které mají InfiniBand podpory pro rychlé sítě.</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Změna velikosti obrázku</td>
<td>Změna počtu uzlů data pracovního clusteru HDInsight bez odebrání nebo opětovné vytvoření. V současné době jenom typy clusteru Hadoop dotazu a Apache bouře mají tuto možnost, ale podpora Apache HBase typu clusteru je brzy bude k dispozici ke sledování. Další informace najdete v tématu Správa HDInsight clusterů.</td>
<td>Služba</td>
<td>Hadoop, bouře</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Nástrojů Visual Studio</td>
<td>Kromě dokončení nástrojů pro Apache bouře byl aktualizovaný nástrojů pro Apache podregistru ve Visual Studiu zahrnout dokončování, místní ověření a lepší ladění podpora. Další informace najdete v tématu Začínáme s HDInsight Hadoop nástroje for Visual Studio.</td>
<td>Nástroje</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Konektor Hadoop pro DocumentDB</td>
<td>Konektor Hadoop pro DocumentDB můžete provádět složité agregace, analýza a manipulace přes schématu bez JSON dokumentů uložených v kolekcích DocumentDB nebo u účtů databáze. Další informace a návod najdete v tématu spuštění Hadoop úlohy pomocí DocumentDB a HDInsight.</td>
<td>Služba</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Opravy chyb</td>
<td>Provedli jsme různé menší opravy chyb služby HDInsight. Očekává se žádné změny chování směřující zákazníka.</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 02/06/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* NENÍ K DISPOZICI SDK

Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Opravy chyb</td>
<td>Provedli jsme různé menší opravy chyb služby HDInsight. Očekává se žádné změny chování směřující zákazníka.</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Aktualizace HDP 2.1 údržby</td>
<td>HDInsight 3.1 se aktualizuje na nasazení HDP 2.1.10.0. Další informace najdete v tématu <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">HDP 2.1.10 poznámky k verzi</a>. </td>
<td>Otevřete zdrojový software</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Binární aktualizace HDP</td>
<td>Existuje pár souborů SKLENICE v HBase u souborů, které byly aktualizovány názvy. Tyto soubory SKLENICE používá interně HBase, takže není předpokládá, že zákazníci mají závislost na názvy těchto souborů SKLENICE. Jedná se o:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Otevřete zdrojový software</td>
<td>HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 1/29/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - beze změny)
* NENÍ K DISPOZICI SDK

Tato verze obsahuje následující aktualizaci.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Dotčeném oblasti (například služba, komponenty nebo SDK)</p></th>
<th>Clusteru typu (například Hadoop HBase a bouře)</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>

<td>Opravy chyb</td>
<td>Jsme provedli několik důležitých opravy chyb vylepšují spolehlivost clusterů HDInsight během Azure aktualizuje.</td>
<td>Služba</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Poznámky k verzi HDInsight 1/5/2015

Plné verze čísla HDInsight clusterů používaný v této verzi:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - beze změny)


Tato verze obsahuje tyto aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Vzorky k analýze trendů Twitter a na základě Mahout film doporučení</td>
<td><p>V této verzi konzole HDInsight dotaz obsahuje dva další příklady:</p>

<p><b>Analýza trendů Twitter</b><br>
Veřejná rozhraní API poskytovanou podobné Twitter weby jsou užitečné zdroje dat pro analýzu a principy Oblíbené trendy. V tomto kurzu Naučte se používat podregistru zobrazte seznam Twitter uživatelů, kterým odeslaná většina tweety obsahující určitá slova. </p>

<p><b>Doporučení mahout filmu</b><br>
Apache Mahout je Apache Hadoop počítač výukové knihovny. Mahout obsahuje algoritmy pro zpracování dat (třeba filtrování klasifikace a clusterů). V tomto kurzu umožňuje modul doporučení generovat film doporučení založené na video, které přáteli zobrazila.</p></td>
<td>Konzoly dotazu</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Změna výchozí hodnotu pro konfiguraci podregistru: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Toto nastavení velikosti platí pro automaticky převedený mapy spojení. Hodnota představuje součet velikostí tabulky, které lze převést na hash mapování, které odpovídají v paměti. V předchozí verzi tato hodnota zvýší s výchozí hodnotou 10 MB 128 MB. Nová hodnota 128 MB však byla příčinou úlohy selhání termín nedostatku paměti. Tato verze se vrátí zpět výchozí hodnotu 10 MB. Zákazníci dál možné přepsat tuto hodnotu během vytváření clusteru zadaných jejich dotazů a velikost tabulky. Další informace o toto nastavení a jak ho změnit najdete v článku <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimalizace převodu automatického připojení</a> v si přečtěte následující dokumentaci Hortonworks. </p></td>
<td>Podregistru</td>
<td>Hadoop Hbase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 12/23/2014

Čísla plné verze HDInsight clusterů používaný v této verzi jsou:

* HDInsight 2.1.10.420.1246783 (beze změny HDP verze)
* HDInsight 3.0.6.420.1246783 (beze změny HDP verze)
* HDInsight 3.1.1.420.1246783 (beze změny HDP verze)

Tato verze obsahuje následující aktualizaci.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>


<tr>
<td>Vytvoření selhání chybovému clusteru kvůli nadbytečné zatížení</td>
<td><p>Vylepšené algoritmus pro její stažení HDP balíčků během vytváření clusteru umožňuje robustnější zpracování chyby kvůli nadbytečné načíst.</p></td>
<td>Služba</td>
<td>Bouře Hadoop, Hbase,</td>
<td>NENÍ K DISPOZICI</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 12/18/2014

Tato verze obsahuje následující komponenty aktualizaci.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Přizpůsobení clusteru Avalability obecné</a></td>
<td><p>Přizpůsobení poskytuje možnost můžete přizpůsobit clusterů Azure HDInsight s projekty, které jsou k dispozici ekosystému Apache Hadoop. S touto nových funkcí můžete vyzkoušet a nasazení Hadoop projektů Azure HDInsight. Je to možné prostřednictvím funkce **Skript akci** , která můžete upravit Hadoop clusterů libovolného způsoby pomocí vlastních skriptů. Toto přizpůsobení je k dispozici pro všechny typy včetně Hadoop, HBase a bouře clusterů HDInsight. Prokázat power tuto možnost, jsme jsou popsány proces instalace Oblíbené <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>a <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> moduly. Tato verze taky přidá funkci pro zákazníky stanovit své vlastní skript akce prostřednictvím portálu Azure obsahuje pokyny k používání a osvědčené postupy o tom, jak vytvořit vlastní skript akcí pomocí pomocné metody a pokyny, jak otestovat akci skriptu. </p></td>
<td>Obecné funkce dostupnosti</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 12/05/2014

Čísla plné verze HDInsight clusterů používaný v této verzi jsou:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK není k dispozici

Tato verze obsahuje následující komponenty aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>

<tr>
<td>Oprava chyby: chybovému při přidávání většího počtu oddíly do tabulky v DDL podregistru </td>
<td><p>Pokud dojde k chybě chybovému připojení k databázi metastore podregistru při přidávání hodně oddílů do tabulku podregistru, může docházet k chybám DDL podregistru. Následující příkaz se zobrazí v protokolu chyb podregistru dojde k této chybě: </p><p>"Chyba [hlavní]: ql. Ovladač (SessionState.java:printError(547)) - se nezdařilo: zpáteční kód 1 z org.apache.hadoop.hive.ql.exec.DDLTask spuštění chyby. MetaException (message:java.lang.RuntimeException: commitTransaction označovalo jako ale openTransactionCalls = 0. To pravděpodobně označuje, že jsou chybné párování volání openTransaction/commitTransaction)"</p></td>
<td>Podregistru</td>
<td>Hadoop Hbase</td>
<td>PODREGISTRU 482 (Toto je vnitřní JIRA, tak nemůžete bude nabízet externě. Poznamenat kdykoliv při ruce.)</td>
</tr>

<tr>
<td>Oprava chyby: občas zavěsit v konzole HDInsight dotazu</td>
<td>V takovém případě následující příkaz lze zobrazit v protokolu WebHCat WebHCat pro spuštění úlohy: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Nelze načíst soubor historie {wasb adresa url historie souboru} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>PODREGISTRU 482 (Toto je vnitřní JIRA, tak nemůžete bude nabízet externě. Poznamenat kdykoliv při ruce.)</td>
</tr>

<tr>
<td>Oprava chyby: občas zásobníku v latence dotazů Hbase</td>
<td>V takovém případě uživatelé Všimněte si občas zásobníku 3 sekundy v latence dotazů Hbase. </td>
<td>HDInsight clusteru brány</td>
<td>HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>HDP JAR změny názvu souboru</td>
<td>HDI clusteru verze 3.0, tam několik změn interní SKLENICE souborů kliknutím HDP nainstalovaný. molo 6.1.26.jar byl nahrazen příkazem molo 6.1.26.hwx.jar. molo util 6.1.26.jar byl nahrazen příkazem molo util 6.1.26.hwx.jar. Tyto změny vliv na Hadoop Mahout, WebHCat a Oozie projekty.</td>
<td>Hadoop, Mahout, WebHCat Oozie</td>
<td>Hadoop HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 11/21/2014

Čísla plné verze HDInsight clusterů používaný v této verzi jsou:

* HDInsight 2.1.9.382.1169709 (stejně jako 11/14/2014)
* HDInsight 3.0.5.382.1169709 (stejně jako 11/14/2014)
* HDInsight 3.1.1.382.1169709 (stejně jako 11/14/2014)
* HDINsight SDK 1.4.0

Tato verze obsahuje následující komponenty aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>

<tr>
<td>Přístup k protokoly aplikace</td>
<td>Schopnost programově umožňuje zobrazit výčet aplikace spuštěné v clusterů a stáhněte si důležité specifické pro aplikaci nebo kontejneru specifické protokoly pomůže ladění problematický aplikací.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Možnost zadáte název oblasti v IHdInsightClient.DeleteCluster </td>
<td>V Azure HDInsight SDK umožňuje zadat název oblasti při použití **DeleteCluster**. Díky odblokování zákazníci, kteří měli dva zdroje se stejným názvem v různých oblastech a nelze odstranit některou z nich.</td>
<td>SDK</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Objekt **ClusterDetails** vrátí **DeploymentID** pole, které představuje jedinečný identifikátor clusteru. Je to musí být jedinečný přes pokusy o vytvoření obrázku se stejnými názvy.</td>
<td>SDK</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 11/14/2014

Čísla plné verze HDInsight clusterů používaný v této verzi jsou:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Tato verze obsahuje následující nové funkce, součásti aktualizace a opravy chyb.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>

<tr>
<td>Skript akce (verze Preview)</td>
<td>Náhled funkce vlastního nastavení obrázku, který umožňuje úpravy Hadoop clusterů libovolného způsoby pomocí vlastních skriptů. Pomocí této funkce uživatelé vyzkoušet a nasazení projektů, které jsou k dispozici z ekosystému Apache Hadoop clusterů Azure HDInsight. Tato funkce vlastního nastavení je k dispozici pro všechny typy HDInsight clusterů, včetně Hadoop, HBase a bouře.</td>
<td>Nové funkce</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Předdefinované úlohy Azure weby a úložiště protokolu analýzy</td>
<td>Konzola dotazu HDInsight má Galerie Začínáme, který podporuje řešení, které spolupracují s daty nebo na ukázková data.
<p>**Řešení, které můžete pracovat s daty**:<br>
Vytvořili jsme úlohy pro některé nejčastější scénáře analýzy dat k poskytování výchozí bod pro vytváření vlastních řešení. Data můžete pomocí úlohy vás jak to funguje. Potom použijte Jakmile budete připraveni, co jste se naučili vytvořit řešení, které je modelovat po přednastavených projektu.</p>
<p>**Řešení, které dopracování ukázkových dat**:<br>
Informace o práci s HDInsight tak, že rozbor některé základní scénáře (například analýza web protokoly a data snímače). Naučíte se, jak používat HDInsight a analyzujte data tyto údaje a jak připojit jiných aplikací a služeb k těmto datům. Vizualizace dat pomocí připojení k aplikaci Microsoft Excel poskytuje příklad power tento přístup.</p></td>
<td>Konzoly dotazu</td>
<td>Hadoop</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Oprava únik paměti v Templeton</td>
<td>Únik paměti v Templeton, který zákazníkům, kteří měli dlouhodobě spuštěné clusterů nebo byly odeslání 100s úlohy požadavky že určila sekundy. Tento problém se jeví jako Templeton 5xx chyby a řešení bylo restartovat službu. Řešení již není potřeba.</td>
<td>Templeton</td>
<td>Všechny</td>
<td>https://issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Poznámka**: prokázat nové možnosti přizpůsobení obrázku, které udělali k dispozici, byly popsány postupy pomocí skriptu akce nainstalovat moduly Spark a R clusteru. Další informace najdete v tématu:

* [Instalace a používání Spark 1.0 v HDInsight clusterů](hdinsight-hadoop-spark-install.md)
* [Instalace a používání R v HDInsight Hadoop clusterů](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Poznámky k verzi HDInsight 11/07/2014

Čísla plné verze HDInsight clusterů, které používaný v této verzi jsou:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Tato verze obsahuje následující komponenty aktualizace.

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Součásti</th>
<th>Typ obrázku</th>
<th>JIRA (pokud existuje)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Tato verze je založená na Hortonworks dat platformu (HDP) 2.1.7. Další informace najdete v tématu <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Poznámky k verzi pro HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>Server vláken časové osy</td>
<td>Ve výchozím nastavení je povolená serveru časovou osu vláken (označovaná taky jako obecný aplikace historie Server). Obecné informace o dokončených aplikací (například ID aplikace název aplikace, stav aplikace, čas odeslání aplikace a čas ukončení aplikací) obsahuje serveru časovou osu.

Informace o této aplikaci můžou načtená z kontingenčního seznamu hlavního uzlu přístupem URI http://headnodehost:8188 nebo spuštěním příkazu vláken: vláken aplikace – seznam – appStates všechny.

Tyto informace lze také načíst vzdáleně když rozhraní REST API na https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Podrobnější informace najdete v tématu <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Vláken časovou osu serveru</a>.</td>
<td>Služby, vláken</td>
<td>Hadoop HBase</td>
<td>NENÍ K DISPOZICI</td>
</tr>

<tr>
<td>ID nasazení obrázku</td>
<td>Začnete s SDK verze 1.3.3.1.5426.29232, mohou uživatelé jedinečných ID pro každou obrázku, který vystavil HDInsight. Umožňuje uživatelům zjistit jedinečné instance clusterů po opětovné použití názvu DNS napříč vytvořit nebo přetáhnout scénáře.</td>
<td>SDK</td>
<td>Všechny</td>
<td>NENÍ K DISPOZICI</td>
</tr>
</table>
<br>

**Poznámka**: Chyba zabraňující číslo plnou verzi z neprojevují na portálu nebo vrácení SDK nebo prostředí Windows PowerShell byla pevnou v této verzi.

## <a name="notes-for-10152014-release"></a>Poznámky k verzi 10/15/2014

Tato verze hotfix pevně paměť v Templeton, který ovlivněné náročné uživatele Templeton. V některých případech uvidí uživatelé, kteří často uplatněna Templeton chyby se jeví jako 500 kódy chyb, protože žádosti nebude mít ke spuštění dostatek paměti. Řešení: pro tento problém se restartovat službu Templeton. Tento problém byl odstraněn.


## <a name="notes-for-1072014-release"></a>Poznámky k verzi 10/7/2014

* Při použití koncový bod Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" pole *název_hostitele* vrací plně kvalifikovaný název domény (FQDN) uzlu místo pouze název hostitele. Třeba místo vrací "**headnode0**", dojde plně kvalifikovaný název domény "**headnode0. { .Net .azurehdinsight ClusterDNS}**". Tato změna byla potřebná k usnadnění scénáře, kde můžete nasazenou více typů obrázku (například HBase a Hadoop) v jedné virtuální sítě. K tomu dojde, například při použití HBase jako back-end platformu pro Hadoop.

* Uvádíme nové nastavení paměti nasazení výchozí clusteru HDInsight. Předchozí výchozí nastavení paměti není účtu dostatečně pokyny pro počet procesoru jádra nasazení. Toto nové nastavení paměti by měl obsahovat lepší výchozí (podle Hortonworks doporučení). Pokud chcete změnit to, získáte dokumentaci SDK o změně konfigurace clusteru. V následující tabulce jsou rozpis nové nastavení paměti používané výchozí 4 procesoru core (8 kontejneru) HDInsight obrázku. (Hodnoty používané před této verzi jsou rovněž k dispozici parenthetically.)

<table border="1">
<tr><th>Součásti</th><th>Přidělování paměti</th></tr>
<tr><td> přidělení yarn.Scheduler.minimum</td><td>768 MB (dříve o velikosti 512 MB)</td></tr>
<tr><td> přidělení yarn.Scheduler.maximum</td><td>6144 MB (beze změny)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (beze změny)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (dříve o velikosti 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>rozhodne =-Xmx512m (dříve - Xmx410m)</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1536 MB (dříve 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>rozhodne =-Xmx1024m (dříve - Xmx819m)</td></tr>
<tr><td>yarn.app.mapreduce.am.Resource</td><td>768 MB (dříve 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.am.Command</td><td>rozhodne =-Xmx512m (dříve - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.Sort</td><td>256 MB (dříve 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (beze změny)</td></tr>

</table><br>

Další informace o nastavení konfigurace paměti používaných vláken a MapReduce na platformu Hortonworks Data, která používá HDInsight najdete v článku [Určit HDP paměti nastavení](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks také poskytuje nástroj pro výpočet nastavení správné paměti.

Týká se prostředí PowerShell Azure a HDInsight SDK chybová zpráva: "*clusteru není nakonfigurován pro HTTP services přístup*":

* Tato chyba je známý [problém s kompatibilitou](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , ke kterým může dojít kvůli rozdíl ve verzi HDInsight SDK nebo Azure PowerShell a verzi clusteru. Clusterů vytvořil 8/15 nebo novější je podpora nová zřizovací funkce do virtuálního sítí. Ale tato možnost není správně interpretováno ve starších verzích HDInsight SDK nebo Azure Powershellu. Výsledkem je chyba instalace v některé úlohy odeslání operace. Pokud používáte HDInsight SDK API nebo rutiny prostředí PowerShell Azure (**Použití AzureRmHDInsightCluster** nebo **Vyvolat AzureRmHDInsightHiveJob**) odeslat úlohy, tyto operace může selhat s chybovou zprávou "*clusteru <clustername> není nakonfigurován pro HTTP services přístup*." Nebo (v závislosti na operaci), může se ostatní chybové zprávy, například "*nelze se připojit k obrázku*".

* Nejnovější verze HDInsight SDK a Azure PowerShell řeší těchto problémů kompatibility. Doporučujeme aktualizovat HDInsight SDK verze 1.3.1.6 nebo novější a Azure nástroje prostředí PowerShell verzi 0.8.8 nebo novější. Můžete získat přístup k nejnovější SDK HDInsight z [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) a nástroje Azure Powershellu v [tom, jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Poznámky k verzi HDInsight 3.1 9/12/2014

* Tato verze je založená na Hortonworks dat platformu (HDP) 2.1.5. Seznam chyby pevnou v této verzi najdete v článku [pevnou v této verzi](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) stránky na webu Hortonworks.
* Ve složce knihovny Prasátko byl změněn souboru "avro mapred 1.7.4.jar" na "avro-mapred-1.7.4-hadoop2.jar." Obsah tohoto souboru obsahuje menší oprava chyb pevné. Doporučujeme, abyste, pokud zákazníci závisí přímo na název souboru SKLENICE Chcete-li předejít konce při přejmenování souborů.


## <a name="notes-for-8212014-release"></a>Poznámky k verzi 8/21/2014

* Jsme přidáváte následující konfigurace WebHCat (PODREGISTRU-7155), který nastaví výchozí limit paměti úlohy řadiče Templeton 1 GB. (Nebo předchozí rovná hodnotě byl o velikosti 512 MB).

     templeton.Mapper.Memory.MB (= 1 024)

    * Tato změna adresy následující chyba, která některých podregistru dotazů měli mít s něčím kvůli nižší limity paměti: "Kontejneru běží za limity paměti fyzické."
    * Pokud chcete vrátit na původní výchozí nastavení, můžete nastavit tuto hodnotu konfigurace až 512 pomocí prostředí PowerShell Azure času vytvoření obrázku pomocí následujícího příkazu:

        Přidání AzureRmHDInsightConfigValues – základní@{"templeton.mapper.memory.mb"="512";}


* Název hostitele roli zookeeper naposledy změnil na *zookeeper*. Tento problém se týká překlad v rámci clusteru, ale nebude nemá vliv na externí rozhraní REST API. Pokud máte součástí, které používají název hostitele *zookeepernode* , musíte aktualizovat je, aby použili nový název. Nové názvy uzly zookeeper jsou:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase verze podpory matice se aktualizuje. Pouze HDInsight podverzí 3.1 (HBase verze 0,98) je podporovaný pro výrobní HBase úloh. Verze 3.0 (který verzi preview k dispozici) není podporována přesunutí vpřed

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Poznámky k clusterů vytvořené před 8/15/2014

Prostředí PowerShell Azure HDInsight SDK nebo chybovou zprávu, "clusteru <clustername> není nakonfigurován HTTP services přístup pomocí protokolu" (nebo v závislosti operace jiných chybové zprávy jako: "Nelze se připojit ke clusteru") může vyskytnout kvůli verze rozdíl mezi Azure PowerShell nebo HDInsight SDK a clusteru. Clusterů vytvořil 8/15 nebo novější je podpora nová zřizovací funkce do virtuálního sítí. Tato možnost není správně interpretováno ve starších verzích Azure PowerShell nebo SDK HDInsight, jehož výsledkem je selhání odeslání operací projektu. Pokud používáte HDInsight SDK API nebo Azure PowerShell rutin (například použití AzureRmHDInsightCluster nebo vyvolat AzureRmHDInsightHiveJob) posílat úkoly, tyto operace může být s některým z chybové zprávy popsané.

Nejnovější verze HDInsight SDK a Azure PowerShell řeší těchto problémů kompatibility. Doporučujeme aktualizovat HDInsight SDK verze 1.3.1.6 nebo novější a Azure nástroje prostředí PowerShell verzi 0.8.8 nebo novější. Můžete získat přístup k nejnovější SDK HDInsight z [NuGet][nuget-link]. Máte přístup k Powershellu nástroje Azure pomocí [Webové platformy Microsoft][webpi-link].


## <a name="notes-for-7282014-release"></a>Poznámky k verzi 7/28/2014

* **HDInsight k dispozici v nové oblasti**: můžeme rozbalenou zeměpisné stavu HDInsight třech oblastech. HDInsight zákazníci vytvářet clusterů v těchto oblastech:
    * Východní Asie
    * Severní centrální USA
    * Jižní centrální USA
* HDInsight verze 1,6 (HDP 1.1 a Hadoop 1.0.3) a HDInsight verze 2.1 (HDP1.3 a Hadoop 1.2) jsou odebraná z portálu Microsoft Azure. Můžete dál vytvářet Hadoop clusterů těchto verzí pomocí rutiny prostředí PowerShell Azure, [Nový AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) nebo pomocí [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Získáte na stránce [HDInsight součást správy verzí](hdinsight-component-versioning.md) pro další informace.
* Hortonworks dat platformu (HDP) změny v této verzi:

<table border="1">
<tr><th>HDP</th><th>Změny</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Žádné změny</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Žádné změny</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: [3.4.5.2.1.3.0-1948] -> [3.4.5.2.1.3.2-0002]</td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Poznámky k verzi 6/24/2014

Tato verze obsahuje vylepšení ke službě HDInsight:

* **Dostupnost HDP 2.1**: 3.1 HDInsight (který obsahuje HDP 2.1) je obecně k dispozici a na výchozí verzi pro nové clusterů.
* **HBase – Azure portálu vylepšení**: jsme zpřístupnili HBase clusterů ve verzi Preview. Z portálu pouhými několika kliknutími můžete vytvořit HBase clusterů. 

S HBase je možné vytvořit celou řadu v reálném čase úloh HDInsight, z interaktivních webů, které fungují s velkých sad dat ke službám ukládání senzor a telemetrie data z miliony koncové body. Dalším krokem bude k analýze dat v těchto úloh s Hadoop úlohy a je to možné v HDInsight pomocí prostředí PowerShell Azure a na řídicím panelu podregistru obrázku.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout předinstalovaný v HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/) je předinstalovaná na HDInsight 3.1 Hadoop clusterů, takže spustíte Mahout úlohy bez nutnosti konfigurace další clusteru. Například lze vzdálené do Hadoop clusteru pomocí vzdálené plochy RDP (Protocol) a bez dalších kroků můžete spusťte tento příkaz Ahoj světě Mahout:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Detailní popis tohoto postupu najdete v dokumentaci [Breiman příklad](https://mahout.apache.org/users/classification/breiman-example.html) na webu Apache Mahout.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Podregistru dotazů můžete použít Tez HDInsight 3.1

Podregistru 0,13 je k dispozici v HDInsight 3.1 a může spouštění dotazů pomocí Tez, který lze využít který nabízí podstatně vyšší výkon.
Tez není povolit ve výchozím nastavení podregistru dotazů. Používat, musí vyjádření výslovného souhlasu. Povolení Tez spuštěním následující fragment kódu:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks publikovala podrobný rozpis podregistru zvýšení výkonu dotazu s Tez dodaných v standardní ukazatele. Podrobnosti najdete v tématu [porovnávání podregistru 13 Apache pro Hadoop organizace](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Podrobné informace o použití podregistru s Tez najdete v článku [podregistru na Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globální dostupnosti
Verze HDInsight na Hadoop 2.2 Microsoft zpřístupnil HDInsight ve všech geografických hlavní kde Azure neexistuje. Konkrétně západní Evropa a jihovýchodní Asie datacentrech mít byly do online režimu. Díky zákazníci vyhledejte clusterů datacentru, který je dostatečně blízko a potenciálně v zóně podobné vyžadovaných předpisů.


###<a name="dos--donts-between-cluster-versions"></a>Pokyny, jak & Dont společnosti mezi verze obrázku

**Používá se HDInsight 3.1 clusteru metastores Oozie není zpětně kompatibilní se HDInsight 2.1 clusterů, a nelze je použít s předchozí verze**.

Vlastní databázi metastore Oozie nasazený s HDInsight 3.1 clusteru nelze znovu použít s HDInsight 2.1 obrázku. To je případ, i když metastore pochází s HDInsight 2.1 obrázku. Tento scénář není podporováno, protože schématu metastore získá upgradovali při použití s HDInsight 3.1 clusteru, takže už není kompatibilní s metastore vyžadované clusterů HDInsight 2.1. Pokus o opětovné použití Oozie metastore, něhož s HDInsight 3.1 clusteru bude vykreslení obrázku HDInsight 2.1 nelze.

**Oozie metastores nemůže být sdíleny clusterů.**

Oozie metastores jsou připojené k určité clusterů a nemůže být sdíleny clusterů.

###<a name="breaking-changes"></a>Zrušení změn

**Připojit znak před syntaxe**: pouze "wasbs: / /" syntaxe je podporované v HDInsight 3.1 a 3.0 clusterů. Starší "objemových: / /" syntaxe je podporované v HDInsight 2.1 a 1,6 clusterů, ale není podporováno v HDInsight 3.1 nebo 3.0 seskupení. To znamená, že všechny úlohy odeslaný HDInsight 3.1 nebo 3.0 clusteru explicitně používající "objemových: / /" syntaxe se nezdaří. "Wasbs: / /" syntaxe by měla být použita. Navíc úlohy odesílání do libovolné HDInsight 3.1 nebo 3.0 clusterů, které jsou vytvořené pomocí existující metastore, obsahující explicitní odkazy na prostředky pomocí "objemových: / /" syntaxe se nezdaří. Tyto metastores nutné znovu vytvořit "wasbs: / /" syntaxi adresu zdroje.


**Porty**: porty používaný službou HDInsight změnily. Číslo portu, které byly použit byly rozsahu dočasné port operačního systému Windows. Porty přiřazených automaticky z předdefinovaných dočasné oblasti pro komunikaci založené na protokolu krátkodobý Internet. Novou sadu povolené čísla portů služby Hortonworks dat platformu (HDP) jsou mimo tato oblast Vyhněte se vzniku konflikty, ke kterým může dojít k porty používaných služeb spuštěných na uzel hlavy. Vyberte nová čísla portů neměl způsobit změny jazycích. Čísla použit jsou následujícím způsobem:

 **HDInsight 1,6 (HDP 1.1)**
<table border="1">
<tr><th>Jméno</th><th>Hodnota</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 a 3.0 (HDP 2.1 a 2.0)**
<table border="1">
<tr><th>Jméno</th><th>Hodnota</th></tr>
<tr><td>DFS.namenode.http adresu</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.HTTPS adresu</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.http adresu</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Závislosti

Následující závislosti byly přidány HDInsight 3.x (HDP2.x):

* guice servlet
* základní optiq
* javax.inject
* Aktivace
* jsr305
* geronimo-jaspic_1.0_spec
* červenec slf4j
* Java xmlbuilder
* a
* Commons kompilátoru
* rozhraní api jdo
* Commons math3
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* Jersey servlet
* spouštění Commons
* rozhraní api jaxb
* molo all-server
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase – vlastnosti
* groovy-all
* základní hamcrest
* pošta
* linq4j
* jpam
* Jersey klienta
* aopalliance
* geronimo-annotation_1.0_spec
* Ikona pro otevření a
* Jersey guice
* rozhraní API XML
* rozhraní api stax
* asm commons
* asm stromu
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* rychlost
* rychlého vypuštění
* Tenhle java
* molo all
* Commons dbcp

Následující závislosti, které už v HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* břečťan
* aspectjrt
* JSON
* základní
* rozhraní api jdo2
* avro mapred
* datanucleus vůni
* JSP
* rozhraní api Commons protokolování
* matematické Commons
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* Tenhle

###<a name="version-changes"></a>Změny verze

Tyto verze změny provedené mezi HDInsight 2.x (HDP1.x) a HDInsight 3.x (HDP2.x):

* základní metriky: [2.1.2] -> [3.0.0]
* derbynet: [10.4.2.0] -> [10.10.1.1]
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* jasper kompilátoru: [5.5.12] -> [5.5.23]
* log4j: ["1.2.15", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: [10.4.2.0] -> [10.10.1.1]
* httpcore: [4.2.4] -> [4.2.5]
* hsqldb: [1.8.0.10] -> [2.0.0]
* jets3t: [0.6.1] -> [0.9.0]
* protobuf java: [2.4.1] -> [2.5.0]
* Derby: [10.4.2.0] -> [10.10.1.1]
* jasper: ["runtime-5.5.12'] -> [" runtime-5.5.23']
* Commons démon: [1.0.1] -> [1.0.13]
* základní datanucleus: [3.0.9] -> [3.2.10]
* datanucleus rozhraní api jdo: [3.0.7] -> [3.2.6]
* zookeeper: [3.4.5.1.3.9.0-01320] -> [3.4.5.2.1.3.0-1948]
* bonecp: [0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Ovladače
Ovladač Java databáze Connnectivity (JDBC) pro systém SQL Server používá HDInsight interně a není použit pro externí operace. Pokud chcete připojit k Hdinsightu pomocí připojení ODBC (Open Database), stiskněte klávesovou zkratku ovladač ODBC podregistru Microsoft. Další informace najdete v tématu [Připojit Excel k Hdinsightu pomocí ovladač ODBC podregistru Microsoft](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Opravy chyb

V této verzi jsme po aktualizaci následující verze HDInsight s několika opravy chyb:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Poznámky k verzi Hortonworks

Poznámky k verzi pro platformy dat Hortonworks (HDPs), které využívají HDInsight verzemi jsou dostupné v těchto umístěních:

* HDInsight podverzí 3.1 používá Hadoop rozdělení, na které je založeno na [Hortonworks datovou platformu 2.1.7][hdp-2-1-7]. Toto je výchozí cluster Hadoop vytvořili pomocí portálu Azure po 11/7/2014. HDInsight 3.1 clusterů vytvořené před 11/7/2014 byly založeny na [Hortonworks datovou platformu 2.1.1][hdp-2-1-1]

* HDInsight verze 3.0 používá Hadoop rozdělení založený na [Hortonworks dat Platform 2.0][hdp-2-0-8].

* HDInsight verze 2.1 používá Hadoop rozdělení založený na [Hortonworks dat platformy 1.3][hdp-1-3-0].

* HDInsight verze 1,6 používá Hadoop rozdělení založený na [Hortonworks dat platformy 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
