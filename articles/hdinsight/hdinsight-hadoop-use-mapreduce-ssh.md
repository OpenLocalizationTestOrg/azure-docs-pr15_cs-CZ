<properties
   pageTitle="MapReduce a SSH připojení s Hadoop v HDInsight | Microsoft Azure"
   description="Naučte se používat SSH spustit MapReduce úlohy pomocí Hadoop na HDInsight."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Použití MapReduce s Hadoop na Hdinsightu pomocí SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

V tomto článku se dozvíte, jak k připojení k Hadoop HDInsight clusteru a odešlete MapReduce úlohy pomocí příkazů Hadoop použít zabezpečené prostředí (SSH).

> [AZURE.NOTE] Pokud už máte zkušenosti s použitím na základě Linux Hadoop servery, ale začínáte HDInsight, najdete v článku [tipy na základě Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Na základě Linux HDInsight (Hadoop na HDInsight) obrázku

* SSH klienta. Operační systémy Linux, Unix a Mac by měl jsou součástí SSH klienta. Uživatelé Windows stáhněte klientů, jako je [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Spojení s SSH

Připojení k plně kvalifikovaný název domény (FQDN) svého clusteru HDInsight pomocí příkazu SSH. Úplný název domény budou název jste přiřadili clusteru, následovaný **. azurehdinsight.net**. Například následující by připojit k obrázku s názvem **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali certifikát klíč pro ověřování SSH** při vytvoření HDInsight obrázku, budete muset zadat umístění privátním klíčem na klientské systémy, třeba:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali heslo pro ověřování SSH** při vytvoření obrázku HDInsight, musíte k zadání hesla po zobrazení výzvy.

Další informace o použití SSH s Hdinsightu najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Nátěrové (klienty Windows)

Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, které si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové najdete v tématu [Použití SSH s Hadoop Linux založen na HDInsight z Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Pomocí příkazů Hadoop

1. Když jste připojeni k obrázku HDInsight příkazem následující **Hadoop** zahájení MapReduce úlohy:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Spustí se třídy **wordcount** , která je součástí souboru **hadoop mapreduce examples.jar** . Předávat na vstupu, použije **wasbs://example/data/gutenberg/davinci.txt** dokumentu a výstup je uložen v **wasbs: / / / Příklad/data/WordCountOutput**.

    > [AZURE.NOTE] Další informace o této úlohy MapReduce a ukázkovými daty najdete v článku [Použití MapReduce v Hadoop na HDInsight](hdinsight-use-mapreduce.md).

2. Úlohy posílá podrobnosti a zpracuje vrátí informace podobně jako tento po dokončení úlohy:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po dokončení úlohy pomocí následujícího příkazu zobrazíte seznam výstupní soubory, které jsou uložené na **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    To by měl zobrazit dva soubory, **_SUCCESS** a **část r-00000**. **Část r-00000** soubor obsahuje výstup pro tento projekt.

    > [AZURE.NOTE] Některé úlohy MapReduce může rozdělení výsledky napříč několika **část r-###** soubory. Pokud ano, použít ### přípona označíte pořadí souborů.

4. Pokud chcete zobrazit výstup, použijte tento příkaz:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Zobrazí se seznam slov, které jsou obsaženy v souboru **wasbs://example/data/gutenberg/davinci.txt** a toho, kolikrát došlo k každého slova. Následující obrázek je příkladem data, která bude součástí souboru:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Souhrn

Jak je vidět příkazy Hadoop poskytují snadný způsob průběh úloh MapReduce HDInsight obrázku a potom zobrazit výstup projektu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o MapReduce úlohy v HDInsight:

* [Použití MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)
