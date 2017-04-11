<properties
    pageTitle="Spuštění Hadoop MapReduce ukázek na základě Linux HDInsight | Microsoft Azure"
    description="Začínáte používat MapReduce vzorky se systémem Linux HDInsight. Připojení k clusteru pomocí SSH a pak pomocí příkazu Hadoop ke spuštění úlohy vzorku."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Spuštění ukázek Hadoop v HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Na základě Linux clusterů HDInsight zadejte sadu MapReduce ukázky, které lze použít k seznámení s spuštěním Hadoop MapReduce úloh. V tomto dokumentu informace o dostupných ukázky a projděte si postup spuštěný několik z nich.

##<a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatné**: najdete v článku [bezplatnou zkušební verzi Azure získat](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **Na základě A Linux HDInsight clusteru**: najdete v článku [Začínáme s používáním Hadoop s podregistru v HDInsight na Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Klient SSH**: informace o použití SSH s HDInsight naleznete v následujících článcích:

    - [Použití SSH s bázi Linux Hadoop na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Vzorky ##

**Umístění**: vzorky umístěných na clusteru HDInsight na **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Obsah**: Následující příklady jsou obsaženy do archivu v:

- **aggregatewordcount**: agregační na základě mapy a zmenšení program, který počítá slova vstupní souborů se změnami
- **aggregatewordhist**: agregační na základě mapy a zmenšení program, který vypočítá histogram slov v seznamu vstupní soubory
- **BBP**: mapy a zmenšení program, který používá Baileyův Borwein Plouffe pro výpočet přesné číslic pí
- **dbcount**: Příklad úlohy, který počítá uložené v protokolech Stránkové zobrazení databázi
- **distbbp**: mapy a zmenšení program, který používá BBP typ vzorce pro výpočet přesné bitů pí
- **GREP**: mapy a zmenšení program, který počítá shody regex ve vstupním
- **spojení**: úlohy, která efekty spojení delší než seřazené, rovnoměrně z vnější oddíly datové sady
- **multifilewc**: projektu, který počítá slova z více souborů
- **pentomino**: mapy a zmenšení dlaždice, kterým se program, který chcete najít řešení problémů pentomino
- **pi**: mapy a zmenšení program, který odhadne pí pomocí kvazi-Monte Carlo metody
- **randomtextwriter**: mapy a zmenšení programu, který data zapisuje 10 GB na uzel náhodné textových dat
- **randomwriter**: mapy a zmenšení program, který data zapisuje 10 GB náhodná data na uzel
- **secondarySort**: Příklad definování snížit sekundárního řazení
- **řazení**: mapy a zmenšení program, který seřadí data napsal náhodné zápis
- **sudoku**: sudoku Řešitele
- **teragen**: generovat data pro terasort
- **terasort**: spustit terasort
- **teravalidate**: Kontrola výsledků terasort
- **WordCount**: mapy a zmenšení program, který počítá slova vstupní souborů se změnami
- **wordmean**: mapy a zmenšení program, který počítá průměrná délka slov v seznamu vstupní soubory
- **wordmedian**: mapy a zmenšení program, který počítá median délku slova vstupní souborů se změnami
- **wordstandarddeviation**: mapy a zmenšení programu, který počítá směrodatnou odchylku délku slova vstupní souborů se změnami

**Zdrojový kód**: zdrojového kódu pro tyto ukázky zahrnuta clusteru HDInsight na **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] `2.2.4.9-1` v cesta je verze platformy dat Hortonworks clusteru HDInsight a měnit v závislosti na aktualizace HDInsight.

## <a name="how-to-run-the-samples"></a>K tomu vzorky ##

1. Připojení ke službě HDInsight pomocí SSH popsané v těchto článcích:

    - [Použití SSH s bázi Linux Hadoop na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. V `username@#######:~$` objeví se výzva, zadejte následující příkaz seznam vzorky:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Tím se vytvoří seznam vzorku s předchozím oddílem tohoto dokumentu.

3. Pomocí následujícího příkazu získání nápovědy pro konkrétní výběru. V tomto případě ukázku **wordcount** :

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Zobrazí se následující zpráva:

        Usage: wordcount <in> [<in>...] <out>

    Tento údaj označuje, že můžete přidat několik vstupní cest zdrojových dokumentů. Koncová cesta je tam, kde je uložen výstup (počet slov v dokumentech zdroje).

4. Spočítání všechna slova v poznámkové bloky z Leonardo Da Vinci, které jsou k dispozici jako ukázkových dat se svůj cluster pomocí následujících akcí:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Zadání vstupních hodnot pro tento úkol je číst z **wasbs:///example/data/gutenberg/davinci.txt**.

    Výstup v tomto příkladu je uložená ve **wasbs: / / / Příklad/data/davinciwordcount**.

    > [AZURE.NOTE] Jak je uvedeno v nápovědě k výběru wordcount, lze zadat více vstupních souborů. Například `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` by zjistit počet slov v davinci.txt a ulysses.txt.

5. Po dokončení projektu pomocí následujícího příkazu zobrazíte výstup:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Spojí všechny výstupní soubory vytvořené pomocí úlohy a jejich zobrazení. Tento příklad základní existuje pouze jeden soubor, ale pokud došlo k více tento příkaz způsobem iteraci všem poznámkám.

    Výstup vypadá přibližně takto:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Každý řádek představuje slova a kolikrát došlo v vstupní data.

## <a name="sudoku"></a>Sudoku

Příklad Sudoku obsahuje pokyny k poněkud neužitečné použití; "Zahrnout Hlavolam do příkazového řádku."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) je logický Hlavolam tvořen devět 3 × 3 mřížky. Některé buňky v tabulce mají čísla, zatímco ostatní jsou prázdné a cílem je řešení pro prázdné buňky. Na odkaz obsahuje další informace o Hlavolam, ale v tomto příkladu slouží k řešení pro prázdné buňky. Náš vstupní tak by měl být soubor, který je v tomto formátu:

- Devět řádky devět sloupců

- Každý sloupec oblasti může obsahovat buď číslo nebo `?` (což znamená, prázdné buňky)

- Buňky se oddělená mezerou

Je určitým způsobem vytvářet Sudoku hádanek; nelze opakovat čísla ve sloupci nebo řádku. V clusteru Hdinsightu, který je správně vytvořen je znázorněn příklad. Je umístěný na **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** a obsahuje následující:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] `2.2.4.9-1` Část cesty měnit v závislosti na aktualizace jsou určené pro clusteru HDInsight.

Použít tuto Sudoku příkladu, použijte tento příkaz:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Výsledky by měl vypadat takto:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

Příklad pí používá statistického (kvazi-Monte Carlo) způsob, jak odhadnout hodnotu čísla pí. Ukazatele umístí náhodně uvnitř celku čtvercovou taky spadají do kruhu v rámci tohoto hranatých označeny pravděpodobnost odpovídající oblasti kruh, pí/4. Z hodnoty 4R, kde R představuje poměr hodnot počet bodů, které jsou uvnitř na kroužek celkový počet bodů, které jsou v rámci druhou mocninu můžete odhadovanou hodnotu čísla pí. Je větší ukázku bodů používá, tím lepší odhad.

Mapování pro tento příklad generuje počet bodů náhodně uvnitř obdélníku jednotky a potom spočítá počet tyto body, které jsou v kruhu.

Reduktorem potom sečteny body počítá podle mappers a vrátí hodnotu čísla pí z vzorce 4R, kde R představuje poměr hodnot počet bodů počítá uvnitř kruhu celkový počet bodů, které jsou v rámci druhou mocninu.

Pomocí následujícího příkazu Spustit v tomto příkladu. To je definována 16 mapy s 10 000 000 vzorky k odhadu hodnotu čísla pí:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Hodnota vrácená to by měl být stejný **3.14159155000000000000**. Pro odkazy jsou 3.1415926535 prvních 10 desetinná čísla pí.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort je srovnávacích řazení jehož míru je kurz řazení (TB/minutu), který nedosáhnete při řazení velmi velké objemy dat, obvykle 100TB minimální.

V tomto příkladu menší 10GB dat, takže ji můžete spustit relativně rychle. Použije aplikace MapReduce vyvinutý Owen O'Malley a Arun Murthy získané roční srovnávacích řazení terabajt univerzální ("daytona") v roce 2009 s rychlostí 0.578 TB/min (100 TB 173 minut). Další informace o těchto i jiných řazení ukazatele najdete v článku [Sortbenchmark](http://sortbenchmark.org/) webu.

Tento příklad používá třemi sady MapReduce programů:

- **TeraGen**: MapReduce program, který generuje řádků dat k seřazení

- **TeraSort**: vzorky vstupních dat a používá MapReduce seřadit data do celkového pořadí

    TeraSort je standardní druh MapReduce funkcí, s výjimkou vlastní rozdělovač používající seřazené seznam zkratek N-1 vzorky, které definují klíčové oblasti pro každou snížit. Zejména všechny zkratky takové, které přehrajte [i-1] < = klíč < ukázkové [i] se odesílají zmenšit i. To záruky výstupy snížit i jsou všechny menší než výstup snížit i + 1.

- **TeraValidate**: MapReduce program, který ověří, že je globálně seřazené výstup

    Vytvoří jedno mapování na jeden soubor v adresáři výstup a každé mapování zajišťuje, aby každý klíč menší nebo rovna na předchozí snímek. Funkce mapy taky generuje záznamy o první a poslední klíčích jednotlivých souborů a funkci zmenšení zajistí klávesu první souboru i větší než poslední klíč i-1 soubor. Problémy se hlášené jako výstup snížit pomocí klávesy, pomocí kterých jsou mimo pořadí.

Použijte následující postup Generovat data, řazení a potom ověřit výstup:

1. Generování 10GB dat, která se uloží do obrázku HDInsight výchozí úložiště na **wasbs: / / / příklad / / 10GB-řazení zadávání dat**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    `-Dmapred.map.tasks` Říká Hadoop kolik úkoly mapu použít pro tento projekt. Konečný dvěma parametry pokyn úlohy 10GB jmění dat vytvářet a ukládat na **wasbs: / / / příklad / / 10GB-řazení zadávání dat**.

2. Zadejte následující příkaz seřadíte data:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    `-Dmapred.reduce.tasks` Říká Hadoop kolik snížit úkoly používat pro daný úkol. Konečný dvěma parametry jsou jenom vstupní a výstupní umístění pro data.

3. Ověření dat generovaného při řazení pomocí následující:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Další kroky ##

V tomto článku se naučíte spuštění ukázek zahrnutý v sadě clusterů na základě Linux HDInsight. Podrobné pokyny k používání Prasátko, podregistru a MapReduce s HDInsight najdete v těchto tématech:

* [Použití Prasátko s Hadoop na HDInsight][hdinsight-use-pig]
* [Použití podregistru s Hadoop na HDInsight][hdinsight-use-hive]
* [Použití MapReduce s Hadoop na HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
