<properties
   pageTitle="Vývoj Python MapReduce úlohy s HDInsight | Microsoft Azure"
   description="Informace o vytváření a spouštění Python MapReduce úlohy na základě Linux HDInsight clusterů."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Vývoj Python streamování programy pro HDInsight

Hadoop poskytuje streamování rozhraní API pro MapReduce, který umožňuje psaní mapa a zmenšení funkce v jazyce než Java. V tomto článku se dozvíte, jak používat Python provádět operace MapReduce.

> [AZURE.NOTE] Zatímco se systémem Windows HDInsight obrázku lze použít kód Python v tomto dokumentu, jsou specifické pro na základě Linux clusterů kroky v tomto dokumentu.

Tento článek je založena na informace a příklady publikovaných Jan Noll při [psaní aplikaci Hadoop MapReduce v Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Na základě Linux Hadoop clusteru HDInsight

* V textovém editoru

    > [AZURE.IMPORTANT] Textovém editoru musí používat LF jako pole Konec řádku. Pokud se používá Line FEED, to způsobí chyby při spuštění úlohy MapReduce na základě Linux HDInsight clusterů. Pokud si nejste jisti, umožňuje volitelný krok v oddílu [Spustit MapReduce](#run-mapreduce) převést všechny Line FEED LF.

* Pro klienty Windows, nátěrové a PSCP. Nástroje jsou dostupné na <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Stránku Stáhnout nátěrové</a>.

##<a name="word-count"></a>Počet slov

V tomto příkladu provede základní slov pomocí mapování a reduktorem. Mapování rozdělí vět na jednotlivá slova a reduktorem sloučí slova a spočítá k vytvoření výstupu.

Následující vývojový diagram znázorňuje, co se stane při mapy a sníží fáze.

![zmenšit obrázek map](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Proč Python?

Python je obecný uceleném programovací jazyk, který vám umožní express koncepty méně řádky kódu než mnoho dalších jazyků. Se má naposledy stal Oblíbené s vědeckých dat jako jazyk vytváření prototypů protože jeho interpretovaný přírodní dynamické psát a elegantní syntaxe usnadňují vhodný pro rychlý vývoj aplikací.

Python nainstalovaný na všech clusterů HDInsight.

##<a name="streaming-mapreduce"></a>Streamování MapReduce

Hadoop umožňuje zadat soubor, který obsahuje mapu a sníží použití logických operátorů používanou projektu. Požadavky pro danou mapu a sníží logiku jsou:

* **Zadávání**: mapy a sníží součásti musí číst vstupní data ze standardního.

* **Výstup**: mapy a sníží součásti zapsal výstupní data StdOut.

* **Formát dat ve sloupcích**: data spotřebované množství a vyrobeno musí být pár klíče/hodnoty oddělené znak tabulátoru.

Python můžete snadno zpracovávat tyto požadavky používání modulu **systémových** číst ze standardního a tisku STDOUT pomocí **vytisknout** . Zbývajících úkolů je jednoduše formátování dat s tabulátory (`\t`) znak mezi klíče a hodnoty.

##<a name="create-the-mapper-and-reducer"></a>Vytvoření mapování a reduktorem

Mapování a reduktorem jsou textové soubory v tomto případě **mapper.py** a **reducer.py** (aby bylo jasné co dělá). Můžete vytvořit tyto slouží editor jazyka podle svého výběru.

###<a name="mapperpy"></a>Mapper.PY

Vytvoření nového souboru s názvem **mapper.py** a použijte následující kód jako obsah:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Číst pomocí kódu, abyste srozumitelný akce chvíli trvat.

###<a name="reducerpy"></a>Reducer.PY

Vytvoření nového souboru s názvem **reducer.py** a použijte následující kód jako obsah:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Nahrání souborů

**Mapper.py** a **reducer.py** musí být na uzel hlavy clusteru jsme mohlo by umožnit spuštění je. Nejjednodušším způsobem účelem jejich nahrání je používat **spojovací bod služby** (Pokud používáte Windows klienta**pscp** ).

Z klienta v adresáři stejný jako **mapper.py** a **reducer.py**, použijte následující příkaz. Nahraďte **uživatelské jméno** uživatele SSH a **název_clusteru** s názvem svůj cluster.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Tím zkopírujete soubory z místního počítače do hlavního uzlu.

> [AZURE.NOTE] Pokud jste použili heslo k zabezpečení SSH účtu, zobrazí se výzva k zadání hesla. Pokud jste použili SSH klíč, bude pravděpodobně nutné použít `-i` parametry a cestu k privátním klíčem, například `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Spuštění MapReduce

1. Připojte se k němu pomocí SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Pokud jste použili heslo k zabezpečení SSH účtu, zobrazí se výzva k zadání hesla. Pokud jste použili SSH klíč, bude pravděpodobně nutné použít `-i` parametry a cestu k privátním klíčem, například `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Volitelné) Pokud jste použili v textovém editoru, který používá Line FEED jako řádku končícím při vytváření souborů mapper.py a reducer.py nebo nemáte vědět, co řádek koncové editor používá, použijte následující příkazy pro převod výskyty Line FEED mapper.py a reducer.py LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Pomocí následujícího příkazu úlohu MapReduce spustíte.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Tento příkaz má následující části:

    * **hadoop streaming.jar**: při provádění operací streamování MapReduce. Rozhraní Hadoop kódem externí MapReduce poskytovaného.

    * **-soubory**: říká Hadoop zadaný soubory, které jsou potřebné pro tento projekt MapReduce, který by měla zkopírovala do všech pracovních uzlů.

    * **-mapování**: říká Hadoop souborů, které chcete použít jako mapování.

    * **-reduktorem**: říká Hadoop souborů, které chcete použít jako reduktorem.

    * **-vstupní**: vstupního souboru, který jsme měli počítat slova z.

    * **-výstup**: adresář, který výstup bude došlo k zápisu.

        > [AZURE.NOTE] Tento adresář vytvoří projektu.

Měli byste najdete v tématu spoustu příkazy **informace** mezi spuštění úlohy a nakonec najdete v článku operaci **mapy** a **sníží** zobrazené jako procenta.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Informace o projektu stavu dostanou při dokončení.

##<a name="view-the-output"></a>Zobrazit výstup

Po dokončení projektu, zadejte tento příkaz Zobrazit výstup:

    hdfs dfs -text /example/wordcountout/part-00000

To by měl zobrazit seznam slov a jak často word došlo k chybě. Následující obrázek je ukázka výstupní data:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Další kroky

Teď, když jste se naučili použití datových proudů úlohy MapRedcue s HDInsight, použijte následující odkazy, které můžete prozkoumat další způsoby, jak pracovat s Azure HDInsight.

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)
* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
* [Použití MapReduce úlohy s HDInsight](hdinsight-use-mapreduce.md)
