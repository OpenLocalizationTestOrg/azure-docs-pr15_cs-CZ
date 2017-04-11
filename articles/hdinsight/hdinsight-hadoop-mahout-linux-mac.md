<properties
    pageTitle="Generovat doporučení pomocí Mahout a na základě Linux HDInsight | Microsoft Azure"
    description="Naučte se používat počítač Apache Mahout výukové knihovny generovat doporučení video s na základě Linux HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Generování film doporučení pomocí Apache Mahout na základě Linux Hadoop v HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Naučte se používat počítač [Apache Mahout](http://mahout.apache.org) výukové knihovny s Azure HDInsight generovat doporučení filmu

Mahout je [počítač výukové] [ ml] Apache Hadoop v knihovně. Mahout obsahuje algoritmy pro zpracování dat, například filtry, zařazení a clusterů. V tomto článku se bude používat modul doporučení generovat doporučení video, které jsou založeny na filmy přáteli zobrazila.

> [AZURE.NOTE] Kroky v tomto dokumentu vyžadují Hadoop na základě Linux clusteru HDInsight. Informace o použití Mahout s clusteru serveru s Windows najdete v článku [Generovat film doporučení pomocí Apache Mahout se systémem Windows Hadoop v HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Základě Linux Hadoop clusteru HDInsight. Informace o vytváření jednu najdete v tématu [Začínáme s používáním na základě Linux Hadoop v HDInsight][getstarted]

##<a name="mahout-versioning"></a>Správa verzí mahout

Další informace o verzi Mahout zahrnutý v sadě svůj cluster Hdinsightu najdete v tématu [HDInsight verzí a Hadoop součástí](hdinsight-component-versioning.md).

> [AZURE.WARNING] Když je možné odeslat jinou verzi Mahout HDInsight clusteru, pouze součásti součástí clusteru HDInsight jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.
>
> Vlastní součásti dostávat komerčně rozumné podpory pomáhají při další řešení problému. To může mít tento problém vyřešit nebo s žádostí o zapojit dostupných kanálů technologie otevřít zdroj, kde se nacházejí hloubkové odborných informací, které technologie. Například existuje mnoho webů komunity, které lze použít, třeba: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mít taky webů projektů na [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [jiskrou](http://spark.apache.org/).

##<a name="recommendations"></a>Principy doporučení

Jednou z těchto funkcích, které poskytuje společnost Mahout je modul doporučení. Tento modul přijme dat ve formátu `userID`, `itemId`, a `prefValue` (předvoleb uživatele položky). Mahout můžete pak analýza spolu occurance k určení: _Uživatelé, kteří mají předvoleb pro položku také přednost u těchto položek_. Mahout pak určuje uživatelé, kteří mají předvolby profesionálové položky, které mohou sloužit k doporučení.

Následujícím obrázku je příklad velmi jednoduché, který používá videa:

* __Spoluvytváření occurance__: Jana, Alice a Jan dali lajk _hvězda válek_ _Empire stávkám zpět_a _vrácení Jedi_. Mahout Určuje, že uživatelé, kteří taky jako jednu z těchto filmy jako ostatní dvě.

* __Spoluvytváření occurance__: Jan a Alice taky dali lajk _Menace fiktivní_ _útok klonů_a _odvety Sith_. Mahout Určuje, že uživatelé, kteří dali lajk předchozí tři filmy také jako tyto tři.

* __Podobnosti doporučení__: vzhledem k tomu, Jan dali lajk první tři filmy, Mahout sleduje videa, které ostatní uživatelé s podobným předvolby dali lajk, ale nebyla Jana sledované (dali lajk/hodnocení). V tomto případě Mahout doporučuje _Menace fiktivní_ _útok klonů_a _odvety Sith_.

###<a name="understanding-the-data"></a>Principy datového

Pohodlně, [GroupLens výzkumu] [ movielens] poskytuje hodnocení data pro video ve formátu, který je kompatibilní se službou Mahout. Tato data jsou k dispozici na svůj cluster výchozí úložiště na `/HdiSamples/HdiSamples/MahoutMovieData`.

Existují dva soubory `moviedb.txt` (informace o videa,) a `user-ratings.txt`. Uživatel ratings.txt soubor je používán během analýzy, zatímco moviedb.txt slouží k poskytnutí informace o popisný text při zobrazování výsledky analýzy.

Data obsažená v uživatele ratings.txt obsahuje strukturu z `userID`, `movieID`, `userRating`, a `timestamp`, která říká cz jak vysoce každý uživatel hodnocení videa. Tady je příklad dat:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Spuštění analýzy

Pomocí následujícího příkazu úlohu doporučení:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Úlohy může trvat několik minut, dokončete a může spustit více MapReduce úloh.

##<a name="view-the-output"></a>Zobrazit výstup

1. Po dokončení projektu pomocí následujícího příkazu zobrazíte generovaný výstup:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Výstup bude vypadat takto:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    První sloupec představuje `userID`. Hodnoty obsažené v ' ["a"] "jsou `movieId`:`recommendationScore`.

2. Výstup spolu s moviedb.txt, můžete použít zobrazíte další informace o uživateli popisný. Nejdřív potřeba soubory kopírovat místně pomocí následujících příkazů:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Tím zkopírujete výstupní data do souboru s názvem **recommendations.txt** v adresáři aktuální spolu s film datové soubory.

3. Umožňuje vytvořit nové Python skript, který bude vypadat film názvů data z výstupu doporučení tento příkaz:

        nano show_recommendations.py

    Po otevření editoru, jako obsah souboru můžete použít následující:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Stiskněte **CTRL + X**, **Y**a konečně **Enter** uložit i data.

3. Chcete-li spustitelný soubor zadejte následující příkaz:

        chmod +x show_recommendations.py

4. Spusťte skript Python. Následující kroky předpokládají, že se nacházíte v adresáři, které jste stáhli všechny soubory:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    To bude vypadat na doporučení vytvořený pro uživatelské ID 4.

    * Soubor **uživatele ratings.txt** slouží k načtení videa, které má uživatel hodnocení
    * Soubor **moviedb.txt** slouží k načtení jména videa
    * **Recommendations.txt** slouží k načtení film doporučení pro tohoto uživatele

    Výstup tento příkaz bude vypadat podobně jako takto:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Odstranění dočasných dat

Úlohy mahout neodebírat dočasná data, která se vytvoří při zpracování úlohy. `--tempDir` Parametr není zadán úlohy příklad vyčlenění dočasné soubory do určité cesty pro snadné odstranění. Pokud chcete odebrat dočasné soubory, použijte tento příkaz:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Pokud budete chtít znova spusťte příkaz, je potřeba odstranit výstupní adresář. Chcete-li odstranit tento adresář použijte následující:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili používání Mahout, seznamte se s další způsoby, jak pracovat s daty v HDInsight:

* [Podregistru s HDInsight](hdinsight-use-hive.md)
* [Prasátko s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
