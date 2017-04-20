<properties
   pageTitle="Prase Hadoop pomocí SSH na clusteru služby HDInsight | Microsoft Azure"
   description="Zjistěte, jak připojit ke clusteru Hadoop systémem Linux s SSH a pak pomocí příkazu prase prase Latinské příkazy spustit interaktivně nebo jako dávkovou úlohu."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Spuštění úloh prasat v clusteru systémem Linux pomocí příkazu prase (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

V tomto dokumentu vás provede procesem připojení do clusteru se systémem Linux Azure HDInsight pomocí Secure Shell (SSH), potom pomocí příkazu prase prase Latinské příkazy spustit interaktivně, nebo jako dávkovou úlohu.

Latinka prase programovací jazyk umožňuje popsat převody, které jsou použita vstupní data pro výrobu požadovaného výstupního.

> [AZURE.NOTE] Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové HDInsight, viz [Systémem Linux tipy HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Předpoklady

Chcete-li provést kroky v tomto článku, budete potřebovat.

* Systémem Linux HDInsight (Hadoop v HDInsight) clusteru.

* Klientem SSH. Linux, Unix a Mac OS by měla přijít s klientem SSH. Uživatelé systému Windows, musíte si stáhnout klienta, například [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Připojit pomocí SSH

Připojte plně kvalifikovaný název domény (FQDN) clusteru HDInsight pomocí příkazu SSH. Úplný název domény, bude název pak dala clusteru, **. azurehdinsight.net**. Například následující by se připojit ke clusteru s názvem **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali klíč certifikátu pro ověřování SSH** při vytvoření clusteru HDInsight, musíte zadat umístění soukromého klíče v počítači klienta.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Pokud jste zadali heslo pro ověřování SSH** při vytvoření clusteru HDInsight, musíte zadat heslo při zobrazení výzvy.

Další informace o použití SSH s HDInsight v tématu [Použití SSH s operačním systémem Linux Hadoop v HDInsight z OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Nátěrové (klienty založené na systému Windows)

Systém Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, který lze stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové naleznete v tématu [Použití SSH s operačním systémem Linux Hadoop v HDInsight ze systému Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Použití příkazu prasat

2. Po připojení, pomocí následujícího příkazu spuste prase rozhraní příkazového řádku (CLI).

        pig

    Po chvíli, měli byste vidět `grunt>` řádku.

3. Zadejte následující příkaz.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Tento příkaz načte obsah souboru sample.log do PROTOKOLŮ. Obsah souboru můžete zobrazit pomocí následujících kroků.

        DUMP LOGS;

4. Dále transformovat data použitím regulární výraz extrahovat úroveň protokolování z jednotlivých záznamů pomocí následujících kroků.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Výpis** slouží k zobrazení dat po transformaci. V takovém případě pomocí `DUMP LEVELS;`.

5. Pokračujte v použití transformací pomocí následujících příkazů. Použití `DUMP` Chcete-li zobrazit výsledek transformace po každém kroku.

    <table>
    <tr>
    <th>Prohlášení</th><th>Co dělá</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = filtr úrovně podle LOGLEVEL není null;</td><td>Odstraní řádky, které obsahují hodnotu null pro úroveň protokolu a výsledek uloží do FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS skupiny podle LOGLEVEL;</td><td>Skupiny řádků a úroveň protokolování a výsledek uloží do GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREKVENCE = foreach GROUPEDLEVELS generování skupin jako LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) jak SPOČÍTAT;</td><td>Vytvoří novou sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnotě úrovně a jak často ji. Toto je uloženo do FREKVENCÍ.</td>
    </tr>
    <tr>
    <td>VÝSLEDEK = pořadí FREKVENCÍ počet desc;</td><td>Úrovně protokolu objednávky podle počtu (sestupně) a uloží do výsledku.</td>
    </tr>
    </table>

6. Výsledky transformace můžete také uložit pomocí `STORE` prohlášení. Například následující uloží `RESULT` do adresáře **/example/data/pigout** na výchozí kontejner úložiště pro cluster.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Data je uložen v adresáři zadaném v souborech s názvem **část nnnnn**. Pokud adresář již existuje, dojde k chybě.

7. Chcete-li ukončit řádku grunt, zadejte následující příkaz.

        QUIT;

###<a name="pig-latin-batch-files"></a>Dávkové soubory prase (latinka)

Můžete také použít příkaz prase spuštění prase (latinka) obsaženého v souboru.

3. Po ukončení řádku grunt, použijte následující příkaz kanálu STDIN do souboru s názvem **pigbatch.pig**. Tento soubor bude vytvořen v domovském adresáři pro účet, který jste se přihlásili k SSH relace.

        cat > ~/pigbatch.pig

4. Zadejte nebo vložte následující řádky a potom pomocí Ctrl + D po dokončení.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Použijte následující spuštění souboru **pigbatch.pig** pomocí příkazu prasete.

        pig ~/pigbatch.pig

    Po dokončení dávkové úlohy se zobrazí následující výstup, který by měl být stejný jako při použití `DUMP RESULT;` v předchozích krocích.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Souhrn

Jak vidíte, prase příkaz umožňuje interaktivně spustit MapReduce operací pomocí latinky prasat, jakož i spusťte příkazy v dávkovém souboru uloženy.

##<a id="nextsteps"></a>Další kroky

Obecné informace o prase v HDInsight.

* [Pomocí prasat Hadoop v HDInsight](hdinsight-use-pig.md)

Informace o dalších způsobech, jak můžete pracovat s Hadoop v HDInsight.

* [Pomocí podregistru Hadoop v HDInsight](hdinsight-use-hive.md)

* [Pomocí MapReduce Hadoop v HDInsight](hdinsight-use-mapreduce.md)
