<properties
    pageTitle="Instalace a používání Giraph na základě Linux HDInsight (Hadoop) | Microsoft Azure"
    description="Zjistěte, jak nainstalovat Giraph na základě Linux HDInsight clusterů pomocí skriptu akce. Akce skriptu umožňuje přizpůsobit clusteru při vytváření změnou konfigurace clusteru nebo instalaci služby a nástroje."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Instalace Giraph na clusterů HDInsight Hadoop a používání Giraph zpracuje rozsáhlé grafy

Giraph můžete nainstalovat na libovolný typ obrázku v Hadoop na Azure Hdinsightu pomocí **Skriptu akce** můžete přizpůsobit clusteru.

V tomto tématu se dozvíte, jak nainstalovat Giraph pomocí skriptu akce. Po instalaci Giraph taky naučíte pro účely Giraph některé běžné aplikace, což je zpracuje rozsáhlé grafy.

> [AZURE.NOTE] Informace v tomto článku jsou specifické pro clusterů na základě Linux HDInsight. Informace o práci s clusterů serveru s Windows najdete v tématu [Instalace Giraph na clusterů HDInsight Hadoop (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Co je Giraph?

[Apache Giraph](http://giraph.apache.org/) umožňuje provádět grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight. Grafy modelu vztahy mezi objekty, například připojení mezi směrovači na velké síť skutečně Internetu nebo vztahy mezi lidé na sociálních sítích (někdy označovány jako sociální grafu). Zpracování grafu umožňuje důvod, proč k relacím mezi objekty v grafu, jako například:

- Identifikace potenciální přáteli na základě aktuální relací.
- Identifikace nejkratší mezi dva počítače v síti.
- Výpočet pořadí stránek webových stránek.

> [AZURE.WARNING] Součásti součástí clusteru HDInsight jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.
>
> Vlastní součásti, například Giraph, dostanou komerčně rozumné podpory pomáhají při další řešení problému. To může mít tento problém vyřešit nebo s žádostí o zapojit dostupných kanálů technologie otevřít zdroj, kde se nacházejí hloubkové odborných informací, které technologie. Například existuje mnoho webů komunity, které lze použít, třeba: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache mít projekty webů projektů na [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).

##<a name="what-the-script-does"></a>Co znamená skriptu

Tento skript provede následující akce:

* Instalace Giraph k`/usr/hdp/current/giraph`
* Kopie `giraph-examples.jar` souboru do výchozí úložiště (WASB) pro svůj cluster:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Instalace Giraph pomocí skriptu akce

Ukázka skriptu nainstalovat Giraph HDInsight clusteru je k dispozici v následujícím umístění.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Tato část obsahuje pokyny k použití ukázkový skript při vytváření clusteru pomocí portálu Azure. 

> [AZURE.NOTE] Azure Powershellu, rozhraní příkazového řádku Azure, HDInsight .NET SDK nebo správce prostředků Azure šablony lze také použít skript akce. Můžete taky použít akce skriptu na spuštěná clusterů. Další informace najdete v tématu [přizpůsobení HDInsight clusterů akcím skriptu](hdinsight-hadoop-customize-cluster-linux.md).

1. Zahájení vytváření clusteru pomocí kroků v [na základě vytvořit Linux HDInsight clusterů](hdinsight-hadoop-create-linux-clusters-portal.md), ale není dokončena vytvoření.

2. Na zásuvné **Volitelná konfigurace** vyberte **Akce skriptu**a zadejte následující informace:

    * __Název__: Zadejte popisný název akci skriptu.
    * __Identifikátor URI skript__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __Hlavy__: Toto políčko zaškrtněte,
    * __PRACOVNÍKA__: Ponechte zrušené zaškrtnutí políčka
    * __ZOOKEEPER__: Ponechte zrušené zaškrtnutí políčka
    * __Parametry__: nechte toto pole prázdné

3. V dolní části **Akce skriptu**pomocí tlačítka **Vybrat** konfiguraci uložte. Nakonec pomocí tlačítka **Vybrat** v dolní části zásuvné **Volitelná konfigurace** pro uložení informací o volitelná konfigurace.

4. Pokračujte ve vytváření clusteru podle popisu v [clusterů na základě vytvořit Linux HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Použití Giraph v HDInsight

Po dokončení clusteru vytváření spustit příklad SimpleShortestPathsComputation zahrnutý v sadě Giraph pomocí podle těchto kroků. To používá základní implementaci <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> vyhledání nejkratší cesta mezi objekty v grafu.

1. Připojení k clusteru HDInsight pomocí SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Další informace o použití SSH s Hdinsightu najdete v těchto článcích:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Vytvoření nového souboru s názvem __tiny_graph.txt__pomocí následující:

        nano tiny_graph.txt

    Použijte následující jako obsah tohoto souboru:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Tato data popisuje relace mezi objekty v řízené grafu pomocí formátu [zdroj\_id, source\_hodnotu, [[cíl\_id], [okraj\_hodnotu];...]]. Každý řádek představuje relace mezi **zdroj\_id** objektu a jeden nebo více **cíl\_id** objekty. **Okraj\_hodnotu** (nebo tloušťku) můžete považovat za síly nebo vzdálenost připojení mezi **source_id** a **cíl\_id**.

    Nakreslili, a pomocí hodnoty (nebo tloušťku) jako vzdálenost mezi objekty, výše uvedených dat může vypadat takto:

    ![tiny_graph.txt nakreslili jako kruhy s odlišnými vzdálenost mezi řádky](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. Pro uložení souboru, použijte __Kombinaci kláves Ctrl + X__a __Y__a konečně __Enter__ potvrďte název souboru.

3. Ukládání dat do primární úložiště pro svůj cluster HDInsight pomocí následující:

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. Spusťte SimpleShortestPathsComputation příklad pomocí následujícího příkazu.

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    Parametry použité pomocí tohoto příkazu jsou popsané v následující tabulce.

  	| Parametr | Co dělá |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | Sklenice souboru, který obsahuje příklady. |
  	| `org.apache.giraph.GiraphRunner` | Třídy použít ke spuštění příklady. |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | Příklad, který bude spuštěn. V tomto případě vypočítá nejkratší cestu ID 1 až všechna ID v grafu. |
  	| `-ca mapred.job.tracker=headnodehost:9010` | Headnode clusteru. |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | Vstupní formát pro vstupní data. |
  	| `-vip /example/data/tiny_graph.txt` | Vstupní datového souboru. |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | Výstupní formát. V tomto případě ID a hodnoty jako prostý text. |
  	| `-op /example/output/shortestpaths` | Umístění výstupu. |
  	| `-w 2` | Počet zaměstnanců, kteří mají použít. V tomto případě 2. |

    Další informace o těchto a ostatní parametry Giraph ukázky najdete v tématu [Rychlý úvod Giraph](http://giraph.apache.org/quick_start.html).

5. Po dokončení projektu, výsledky budou uložené v __wasbs: / / / Příklad/out/shotestpaths__ adresář. Soubory vytvořené začne se __část-m -__ a končí číslo označující první, druhý, soubor atd. Chcete-li zobrazit výstup použijte následující:

        hdfs dfs -text /example/output/shortestpaths/*

    Výstup by měl vypadat takto:

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Příklad je pevné kódovaný začínat SimpleShortestPathComputation objektu ID 1 a najděte nejkratší cestu k jiným objektům. Aby výstup obsaženou jako `destination_id distance`, kde je vzdálenost hodnotu (nebo weight (váha)) okrajů cesty mezi objektem ID 1 a cílové ID.

    Vizualizace to můžete ověřit výsledky cestování nejkratší cesty mezi ID 1 a jiných objektů. Všimněte si, že nejkratší cestu ID 1 až ID 4 5. Toto je celková vzdálenost mezi <span style="color:orange">ID 1 3</span>a potom <span style="color:red">ID 3 a 4</span>.

    ![Kreslení objektů jako kruhy s nejkratší cesty mezi](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>Další kroky

- [Instalace a použití clusterů odstín na HDInsight](hdinsight-hadoop-hue-linux.md). Odstínem je web uživatelského rozhraní, který umožňuje snadno vytvářet, spustit a uložit Prasátko a podregistru úlohy a procházet výchozí úložiště pro vaše HDInsight clusteru.

- [Instalace R na HDInsight clusterů](hdinsight-hadoop-r-scripts-linux.md): pokyny, jak používat clusteru vlastního nastavení instalace a používání R na clusterů HDInsight Hadoop. R je otevřít zdrojového jazyka a prostředí statistické výpočetních. Poskytuje stovky předdefinované statistických funkcí a vlastní programovací jazyk, který kombinuje aspekty funkční a objektově orientovaném programování. Také poskytuje rozsáhlé grafické možnosti.

- [Instalace Solr na clusterů HDInsight](hdinsight-hadoop-solr-install-linux.md). Pomocí úprav obrázku Solr nainstalovat clusterů HDInsight Hadoop. Solr umožňuje provádět výkonné vyhledávání operací dat uložených.
