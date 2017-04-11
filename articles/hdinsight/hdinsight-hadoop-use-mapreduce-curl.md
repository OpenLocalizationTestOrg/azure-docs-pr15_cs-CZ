<properties
   pageTitle="Použití MapReduce a otočení s Hadoop v HDInsight | Microsoft Azure"
   description="Zjistěte, jak vzdáleně pracovat MapReduce úlohy s Hadoop HDInsight pomocí otočení."
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Spuštění úlohy MapReduce s Hadoop na HDInsight pomocí otočení

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

V tomto dokumentu budou zjistěte, jak používat otočení spustit MapReduce úlohy na Hadoop clusteru HDInsight.

Otočení slouží k demonstrují můžete interakci se HDInsight pomocí nezpracovanými HTTP požadavků pro spuštění MapReduce úloh. To funguje pomocí rozhraní WebHCat REST API (dříve označované jako Templeton) poskytovanou svůj cluster HDInsight.

> [AZURE.NOTE] Pokud už máte zkušenosti s použitím na základě Linux Hadoop servery, ale začínáte HDInsight, přečtěte si téma [Co je potřeba vědět o Hadoop Linux založené na HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Hadoop HDInsight clusteru (Linux nebo serveru s Windows)

* [Otočení](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Spuštění úlohy MapReduce pomocí otočení

> [AZURE.NOTE] Pokud používáte otočení nebo jiné ostatní komunikace s WebHCat, je třeba ověřit žádosti zadáním HDInsight clusteru správce uživatelské jméno a heslo. Také je nutné použít název obrázku jako součást identifikátor URI, který slouží k odesílání požadavky na server.
>
> Příkazy v této části nahraďte **uživatelské jméno** uživatele k ověření obrázku a **heslo** pomocí hesla k uživatelskému účtu. **NÁZEV_CLUSTERU** nahraďte názvem vašeho obrázku.
>
> Rozhraní REST API je zabezpečená pomocí [ověřování základní přístupu](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy je třeba žádosti o pomocí HTTPS zajistit, aby vaše přihlašovací údaje bezpečně posílají serveru.

1. Z příkazového řádku pomocí následujícího příkazu můžete ověřit, že můžete připojit k obrázku HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Dostanete odpověď podobná této:

        {"status":"ok","version":"v1"}

    Parametry použité v tento příkaz je takto:

    * **-u**: označuje uživatelské jméno a heslo pro ověření žádosti
    * **-G**: označuje, že se jedná požadavek GET

    Na začátek URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**je stejný pro všechny žádosti o.

2. Odeslání MapReduce úlohy, použijte tento příkaz:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Na konec identifikátor URI (/ mapreduce/sklenice), bude WebHCat, že tento požadavek začne MapReduce projektu třídy v souboru sklenice. Parametry použité v tento příkaz je takto:

    * **-d**: `-G` není použit, tak žádost ve výchozím nastavení metodu příspěvek. `-d`Určuje datové hodnoty, které jsou odeslány s žádostí o.

        * **User.Name**: uživatel, který běží příkazu
        * **sklenice**: spustili umístění sklenice souboru, který obsahuje třída
        * **třídy**: třídy, která obsahuje MapReduce logiky
        * **argument**: argumentem bude předán MapReduce úlohy; v tomto případě, zadávání textový soubor a na adresář, který se používají pro výstup

    Tento příkaz, měly vrátit ID práce, které mohou sloužit k Kontrola stavu úlohy:

        {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav projektu, zadejte následující příkaz. Nahraďte **JOBID** hodnota vrácená v předchozím kroku. Například, pokud byla vrácenou hodnotu `{"id":"job_1415651640909_0026"}`, pak bude JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Pokud dokončení projektu, stav bude třeba "byla úspěšně DOKONČENA".

    > [AZURE.NOTE] Otočení požadavek vrátí JSON dokument s informacemi o zaměstnání, jq slouží k načtení pouze hodnoty stav.

4. Když stav projektu se změnil na **byl úspěšný**, můžete získat výsledky úlohy z úložiště objektů Blob Azure. `statusdir` Parametr, které je s dotazem obsahuje umístění ve výstupním souboru; v tomto případě **wasbs: / / / Příklad/otočení**. Tuto adresu ukládá výstup projektu v adresáři **Příklad/otočení** v kontejneru úložiště výchozí používat svůj cluster HDInsight.

Můžete seznam a stáhnout tyto soubory pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md). Například seznam souborů v **příkladu/otočení**použijte tento příkaz:

    azure storage blob list <container-name> example/curl

Stažení souboru, následujícím způsobem:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Zadejte název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry nebo sady **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_přístup\_klíč** proměnné. Zjistěte, [jak nahrát data, která chcete HDInsight](hdinsight-upload-data.md) Další informace.

##<a id="summary"></a>Souhrn

Jak je ukázáno v tomto dokumentu, můžete použít jako nezpracovaná žádost HTTP ke spuštění, sledování a zobrazení výsledků podregistru úlohy v svůj cluster HDInsight.

Další informace o rozhraní REST, které se používá v tomto článku najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Další kroky

Obecné informace o MapReduce úlohy v HDInsight:

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)
