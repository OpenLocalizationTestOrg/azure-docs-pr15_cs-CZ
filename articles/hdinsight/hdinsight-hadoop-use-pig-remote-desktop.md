<properties
   pageTitle="Použití Hadoop Prasátko s Vzdálená plocha v HDInsight | Microsoft Azure"
   description="Zjistěte, jak pomocí příkazu Prasátko spuštění Prasátko latinka příkazy z připojení ke vzdálené ploše serveru s Windows Hadoop clusteru v HDInsight."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Spuštění úlohy Prasátko z připojení ke vzdálené ploše

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tento dokument obsahuje návod pro použití příkazu Prasátko běžet Prasátko latinka příkazy připojení ke vzdálené ploše clusteru serveru s Windows HDInsight. Prasátko latinka umožňuje vytvářet MapReduce aplikace podle popisu transformace dat, spíše než mapování a zmenšení funkcí.

V tomto dokumentu, zjistěte, jak

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat.

* Shluk serveru s Windows HDInsight (Hadoop na HDInsight)

* Klientského počítače se systémem Windows 10, Windows 8 nebo Windows 7

##<a id="connect"></a>Připojit se ke vzdálené ploše

Povolit připojení ke vzdálené ploše pro HDInsight clusteru, potom je připojíte k němu postupujte podle pokynů na [připojit k clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="pig"></a>Použití příkazu Prasátko

2. Po připojení ke vzdálené ploše začněte **Hadoop příkazového řádku** pomocí ikony na ploše.

2. Spuštění příkazu Prasátko pomocí následující:

        %pig_home%\bin\pig

    Zobrazí se `grunt>` dotaz.

3. Zadejte následující příkaz:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Tento příkaz načte obsah souboru sample.log do souboru protokoly. Obsah souboru můžete zobrazit pomocí následujícího příkazu:

        DUMP LOGS;

4. Transformace dat použitím regulárních výrazů extrahování jenom požadovanou úroveň protokolování z jednotlivých záznamů:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Výpis** slouží k zobrazení data po transformace. V tomto případě `DUMP LEVELS;`.

5. Pokračujte v nanášení transformace pomocí následující příkazy. Použití `DUMP` k zobrazení výsledku transformace po každém kroku.

    <table>
    <tr>
    <th>Příkaz</th><th>Co dělá</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = úrovně FILTROVAT podle LOGLEVEL není null.</td><td>Odebere řádky, které obsahují hodnotu null pro úroveň protokolování a výsledek uloží do FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS skupiny tak, že LOGLEVEL;</td><td>Seskupí řádků podle úroveň protokolování a výsledek uloží do GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>ČETNOSTI = foreach GROUPEDLEVELS generovat skupiny jako LOGLEVEL, počet (FILTEREDLEVELS. LOGLEVEL) jak SPOČÍTAT;</td><td>Vytvoří novou sadu dat, která obsahuje jednotlivé jedinečné protokoly dojde k hodnotě úrovně a jak často se. To je uložena do četnosti</td>
    </tr>
    <tr>
    <td>VÝSLEDEK = pořadí FREKVENCÍ počet desc;</td><td>Objednávky úrovně protokolu podle počtu (sestupně) a uloží do výsledků</td>
    </tr>
    </table>

6. Můžete také uložit výsledky transformace pomocí `STORE` údajů. Například následující příkaz uloží `RESULT` k adresáři **/example/data/pigout** v kontejneru výchozí úložiště pro vaše obrázku:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Uložení dat v adresáři zadané v souborech s názvem **část nnnnn**. Pokud již existuje v adresáři, zobrazí se chybová zpráva.

7. Ukončíte výzva grunt zadejte následující příkaz.

        QUIT;

###<a name="pig-latin-batch-files"></a>Soubory dávku Prasátko (latinka)

Příkaz Prasátko můžete taky spustit Prasátko latinka, které jsou obsaženy v souboru.

3. Ukončili řádku grunt otevřete **Poznámkový blok** a vytvořte nový soubor s názvem **pigbatch.pig** v adresáři **PIG_HOME %** .

4. Zadejte nebo vložte následující řádky do souboru **pigbatch.pig** a uložte ho:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Ke spuštění **pigbatch.pig** soubor pomocí příkazu Prasátko použijte následující.

        pig %PIG_HOME%\pigbatch.pig

    Po dokončení úlohy byste měli vidět následující výstup, které by měl být stejný jako při použití `DUMP RESULT;` v předchozích krocích:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Souhrn

Jak můžete vidět, příkaz Prasátko můžete interaktivně spustit MapReduce operace, nebo Prasátko latinka úloh, které jsou uložené v dávkový soubor.

##<a id="nextsteps"></a>Další kroky

Obecné informace o Prasátko v HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
