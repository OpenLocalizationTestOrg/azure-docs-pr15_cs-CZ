<properties
   pageTitle="Použití Hadoop Prasátko s otočení v HDInsight | Microsoft Azure"
   description="Naučte se používat otočení spustit Prasátko latinka úlohy clusteru Hadoop v Azure HDInsight."
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

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Spuštění úlohy Prasátko s Hadoop na HDInsight pomocí otočení

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

V tomto dokumentu budou zjistěte, jak spustit Prasátko latinka úlohy na clusteru služby Azure Hdinsightu pomocí otočení. Prasátko latinky programovacího jazyka umožňuje popisují transformace, která platí pro zadávání dat k vytvoření požadovaného výstupního.

Otočení slouží k ukazují můžete interakci se HDInsight pomocí zdrojové požadavky HTTP spustit, sledovat a načítání výsledků Prasátko úlohy. To funguje pomocí WebHCat rozhraní REST API (dříve označované jako Templeton), která poskytuje společnost svůj cluster HDInsight.

> [AZURE.NOTE] Pokud jste již znáte pomocí na základě Linux Hadoop servery, ale začínáte s HDInsight, najdete v článku [Na základě Linux tipy HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Clusteru služby Azure HDInsight (Hadoop na HDInsight) (na základě Linux nebo serveru s Windows)

* [Otočení](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Spuštění úlohy Prasátko pomocí otočení

> [AZURE.NOTE] Když používáte otočení nebo jiné ostatní komunikace s WebHCat, je třeba ověřit žádosti poskytnutím správce uživatelské jméno a heslo pro clusteru HDInsight. Také je nutné použít název obrázku jako součást aplikace identifikátor URI (Uniform Resource), který slouží k odeslání požadavky na server.
>
> Příkazy v této části nahraďte **uživatelské jméno** uživatele k ověření clusteru a **heslo** pomocí hesla k uživatelskému účtu. **NÁZEV_CLUSTERU** nahraďte názvem vašeho obrázku.
>
> Rozhraní REST API je zabezpečená prostřednictvím [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy je třeba žádosti o pomocí zabezpečené HTTP (HTTPS), a pomáhají zajistit, aby vaše přihlašovací údaje bezpečně posílají serveru.

1. Z příkazového řádku ověřte, že se můžete připojit ke svůj cluster HDInsight pomocí tento příkaz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Dostanete odpověď podobná této:

        {"status":"ok","version":"v1"}

    Parametry použité v tento příkaz je takto:

    * **-u**: uživatelské jméno a heslo pro ověření žádosti
    * **-G**: označuje, že se jedná požadavek GET

    Na začátek adresu URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, bude stejné pro všechny žádosti o. Cesta **/status/status**označuje, že žádosti vrátit stav WebHCat (označovaná taky jako Templeton) pro server.

2. Odeslání Prasátko latinka úlohy clusteru pomocí následující kód:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Parametry použité v tento příkaz je takto:

    * **-d**: protože `-G` není použit, žádost ve výchozím nastavení metodu příspěvek. `-d`Určuje datové hodnoty, které jsou odeslány s žádostí o.

        * **User.Name**: uživatel, který běží příkazu
        * **spuštění**: příkazy latinka Prasátko provést
        * **statusdir**: adresář, který stav pro tuto úlohu bude došlo k zápisu

    > [AZURE.NOTE] Všimněte si, že mezery v příkazech Prasátko latinka nahrazují `+` character při použití s otočení.

    Tento příkaz, měly vrátit ID práce, které mohou sloužit k Kontrola stavu úlohy, například:

        {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav projektu, zadejte následující příkaz. Nahraďte **JOBID** hodnota vrácená v předchozím kroku. Například, pokud byla vrácenou hodnotu `{"id":"job_1415651640909_0026"}`, pak bude **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Pokud úlohy dokončí, budou stavu **byl úspěšný**.

    > [AZURE.NOTE] Otočení požadavek vrátí dokumentu JavaScript Object Notation (JSON) vyplnili informace o projektu a jq slouží k načtení pouze hodnoty stav.

##<a id="results"></a>Zobrazit výsledky

Když stav projektu se změnil na **byl úspěšný**, můžete získat výsledky úlohy z úložiště objektů Blob Azure. `statusdir` Parametr předaný s dotazem obsahuje umístění ve výstupním souboru; v tomto případě **wasbs: / / / Příklad/pigcurl**. Tuto adresu ukládá výstup projektu v adresáři **Příklad/pigcurl** v kontejneru úložiště výchozí používat svůj cluster HDInsight.

Můžete seznam a stáhnout tyto soubory pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md). Příklad seznamu souborů v **příkladu/pigcurl**použijte tento příkaz:

    azure storage blob list <container-name> example/pigcurl

Stažení souboru, následujícím způsobem:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Zadejte název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry nebo sady **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_přístup\_klíč** proměnné.

##<a id="summary"></a>Souhrn

Jak je ukázáno v tomto dokumentu, můžete použít jako nezpracovaná žádost HTTP spustit, sledovat a zobrazit výsledky Prasátko úlohy na svůj cluster HDInsight.

Další informace o rozhraní REST použitých v tomto článku najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Další kroky

Obecné informace o Prasátko na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
