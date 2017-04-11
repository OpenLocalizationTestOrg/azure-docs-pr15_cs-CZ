<properties
    pageTitle="Instalace R na základě Linux HDInsight | Microsoft Azure"
    description="Zjistěte, jak nainstalovat a používat R přizpůsobení clusterů Hadoop na základě Linux."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instalace a používání R v HDInsight Hadoop clusterů

R můžete nainstalovat na libovolný typ obrázku v Hadoop na HDInsight pomocí **Skriptu akce** clusteru přizpůsobení. Díky vědeckých dat a analytici pomocí R nasadit výkonné programování framework MapReduce/vláken zpracovat velké objemy dat na Hadoop clusterů, které jsou nasazenou v HDInsight.

> [AZURE.IMPORTANT] [Vrstvy premium](https://azure.microsoft.com/pricing/details/hdinsight/) nabízející pro HDInsight obsahuje R Server v rámci svůj cluster HDInsight. Díky R skripty MapReduce a Spark spuštění distribuované výpočty. Další informace najdete v tématu [Začínáme s používáním R Server místní HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Co je R?

<a href="http://www.r-project.org/" target="_blank">R projektu statistické výpočetních</a> je otevřít zdrojového jazyka a prostředí statistické výpočetních. R obsahuje stovky předdefinované statistických funkcí a vlastní programovací jazyk, který kombinuje aspekty funkční a objektově orientovaném programování. Také poskytuje rozsáhlé grafické možnosti. R je upřednostňované programovacím prostředí pro většinu profesionální statistiků a vědeckých v celé řadě polí.

Skripty R je možné spouštět na Hadoop clusterů HDInsight, které byly upraveny pomocí skriptu akce při vytvoření nainstalovat prostředí R. R je kompatibilní s úložiště objektů Blob Azure (WASB) tak, že data, která je uložena tam můžete zpracovány pomocí R na HDInsight.

## <a name="what-the-script-does"></a>Co znamená skriptu

Akci skriptu použili k instalaci R HDInsight clusteru nainstaluje následující balíčky se systémem Ubuntu, které poskytují základní R instalace:

* [r base](http://packages.ubuntu.com/precise/r-base): základní R GNU balíčku
* [r základu odchylka](http://packages.ubuntu.com/precise/r-base-dev): balíčků zařízení AUX GNU R

Následující balíčky RHadoop také máte nainstalované, které poskytují integrace s MapReduce a HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): umožňuje vývojářům R použít Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): umožňuje vývojářům R použít HDFS Hadoop (WASB pro HDInsight)

Dále jsou nainstalované následující balíčky R:

| R balíčku | Co obsahuje |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Nízké R Java rozhraní. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | Integrace R a C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Serializovat/rekonstruovat R objekty JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funkce pro bitové operace s vektory celé číslo. |
| [Digest](https://cran.r-project.org/web/packages/digest/index.html) | Vytvoření kryptografický algoritmus Hash výluhy R objektů. |
| [funkčnosti](https://cran.r-project.org/web/packages/functional/index.html) | Kari, vytváření a další funkce vyšší pořadí |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexibilně struktury a agregovat data. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Jednoduchý a konzistentní obálky pro používaných řetězcových operací. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Nástroje pro rozdělení, použití a kombinování dat. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Nástroje pro přesunutí okno statistiky, GIF, ve formátu Base 64, ROC AUC atd. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Aproximaci počtu porovnávání řetězců a řetězec vzdálenost funkcí. |

## <a name="install-r-using-script-actions"></a>Instalace R pomocí skriptu akce

Následující akci skriptu slouží k instalaci R clusteru HDInsight. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.SH
    
Tato část obsahuje pokyny, jak pomocí skriptu při vytváření nového clusteru pomocí portálu Azure. 

> [AZURE.NOTE] Azure Powershellu, rozhraní příkazového řádku Azure, HDInsight .NET SDK nebo správce prostředků Azure šablony lze také použít skript akce. Můžete taky použít akce skriptu na spuštěná clusterů. Další informace najdete v tématu [přizpůsobení HDInsight clusterů akcím skriptu](hdinsight-hadoop-customize-cluster-linux.md).

1. Spuštění zřizování clusteru pomocí kroků v [bázi poskytování Linux HDInsight clusterů](hdinsight-hadoop-provision-linux-clusters.md#portal), ale není dokončena zřizování.

2. Na zásuvné **Volitelná konfigurace** vyberte **Akce skriptu**a zadejte následující informace:

    * __Název__: Zadejte popisný název akci skriptu.
    * __Identifikátor URI skript__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Hlavy__: Toto políčko zaškrtněte,
    * __Pracovní__: Toto políčko zaškrtněte,
    * __ZOOKEEPER__: Zkontrolujte tato možnost nainstalovat na uzel Zookeeper.
    * __Parametry__: nechte toto pole prázdné

3. V dolní části **Akce skriptu**pomocí tlačítka **Vybrat** konfiguraci uložte. Nakonec pomocí tlačítka **Vybrat** v dolní části zásuvné **Volitelná konfigurace** pro uložení informací o volitelná konfigurace.

4. Pokračujte v zřizování clusteru podle popisu v [clusterů na základě poskytování Linux HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>Spustit skripty R

Po dokončení clusteru zřizování pomocí následujících kroků R provedení operace MapReduce na clusteru.

1. Připojení k clusteru HDInsight pomocí SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Další informace o použití SSH s Hdinsightu najdete v těchto článcích:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. V `username@hn0-CLUSTERNAME:~$` objeví se výzva, zadejte tento příkaz zahájit relaci interaktivní R:

        R

3. Zadejte následující R program. Vygeneruje čísla 1 až 100 a pak vynásobí číslem 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    První řádek hovorů RHadoop rmr2 knihovny, které se používá pro operace MapReduce.

    Na druhém řádku generuje hodnoty 1-100, pak ukládá k systému souborů Hadoop pomocí `to.dfs`.

    Třetím řádku vytvoří MapReduce proces, který používá funkce poskytované rmr2 a zpracování, nebude zahájen. Měli byste vidět několik řádků posuňte dřívější při zpracování, nebude zahájen.

4. Pak použijte následující zobrazíte dočasné cestu uložené výstupu MapReduce na:

        print(calc())

    Je vhodné podobnou `/tmp/file5f615d870ad2`. Pokud chcete zobrazit aktuální výstup, použijte následující:

        print(from.dfs(calc))

    Výstup by měl vypadat takto:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Na konec R zadejte tyto údaje:

        q()


## <a name="next-steps"></a>Další kroky

- [Instalace a použití clusterů odstín na HDInsight](hdinsight-hadoop-hue-linux.md). Odstínem je web uživatelského rozhraní, který umožňuje snadno vytvářet, spustit a uložit Prasátko a podregistru úlohy a procházet výchozí úložiště pro vaše HDInsight clusteru.

- [Instalace Giraph na clusterů HDInsight](hdinsight-hadoop-giraph-install.md). Pomocí úprav obrázku Giraph nainstalovat clusterů HDInsight Hadoop. Giraph umožňuje provádět zpracování grafu pomocí Hadoop a lze jej využít s Azure HDInsight.

- [Instalace Solr na clusterů HDInsight](hdinsight-hadoop-solr-install.md). Pomocí úprav obrázku Solr nainstalovat clusterů HDInsight Hadoop. Solr umožňuje provádět výkonné vyhledávání operací na uložená data.

- [Instalace odstín na clusterů HDInsight](hdinsight-hadoop-hue-linux.md). Pomocí úprav obrázku nainstalovat HDInsight Hadoop clusterů odstín. Odstín je sada webových aplikací slouží k interakci s Hadoop obrázku.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
