<properties
   pageTitle="MapReduce a Vzdálená plocha s Hadoop v HDInsight | Microsoft Azure"
   description="Naučte se používat ke vzdálené ploše pro připojení k Hadoop na HDInsight a spuštění MapReduce úloh."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Použití MapReduce v Hadoop na Hdinsightu pomocí vzdálené plochy

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

V tomto článku se dozvíte, jak se připojit k Hadoop clusteru HDInsight pomocí vzdálené plochy a spusťte MapReduce úlohy pomocí příkazu Hadoop.

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Shluk serveru s Windows HDInsight (Hadoop na HDInsight)

* Klientského počítače se systémem Windows 10, Windows 8 nebo Windows 7

##<a id="connect"></a>Připojit se ke vzdálené ploše

Povolit připojení ke vzdálené ploše pro HDInsight clusteru, potom je připojíte k němu postupujte podle pokynů na [připojit k clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hadoop"></a>Použití příkazu Hadoop

Když jste připojeni k plochy clusteru HDInsight úlohu MapReduce pomocí příkazu Hadoop pomocí následujících kroků:

1. Z počítače HDInsight začněte **Hadoop příkazového řádku**. Otevře se nové příkazového řádku v **c:\apps\dist\hadoop-&lt;číslo verze >** adresář.

    > [AZURE.NOTE] Číslo verze změní, když se aktualizuje Hadoop. Proměnná prostředí **HADOOP_HOME** mohou sloužit k vyhledání cestu. Například `cd %HADOOP_HOME%` adresáře se změní na Hadoop adresáře, aniž by bylo potřeba vědět číslo verze.

2. Použití příkazu **Hadoop** úlohu MapReduce příklad, použijte tento příkaz:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Spustí se třídy **wordcount** , která je součástí souboru **hadoop mapreduce examples.jar** aktuálního adresáře. Předávat na vstupu, použije **wasbs://example/data/gutenberg/davinci.txt** dokumentu a výstup je uložený na: **wasbs: / / / Příklad/data/WordCountOutput**.

    > [AZURE.NOTE] Další informace o této úlohy MapReduce a ukázkovými daty najdete v článku <a href="hdinsight-use-mapreduce.md">Použití MapReduce v HDInsight Hadoop</a>.

2. Úlohy posílá podrobnosti a zpracuje se vrátí informace podobně jako tento po dokončení projektu:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po dokončení projektu pomocí následujícího příkazu zobrazíte seznam výstup souborů uložených na **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    To by měl zobrazit dva soubory, **_SUCCESS** a **část r-00000**. **Část r-00000** soubor obsahuje výstup pro tento projekt.

    > [AZURE.NOTE] Některé úlohy MapReduce může rozdělení výsledky napříč několika **část r-###** soubory. Pokud ano, použít ### přípona označíte pořadí souborů.

4. Pokud chcete zobrazit výstup, použijte tento příkaz:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Zobrazí se seznam slov, které jsou obsaženy v souboru **wasbs://example/data/gutenberg/davinci.txt** spolu s číslem, kolikrát došlo k každého slova. Následující obrázek je příkladem data, která bude součástí souboru:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Souhrn

Jakmile uvidíte, příkaz Hadoop vám bude radit snadný způsob, jak spustit MapReduce úlohy na HDInsight obrázku a potom zobrazit výstup projektu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o MapReduce úlohy v HDInsight:

* [Použití MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)
