<properties
   pageTitle="Pomocí otočení v HDInsight Hadoop Sqoop | Microsoft Azure"
   description="Zjistěte, jak vzdáleně posílat Sqoop úkoly k Hdinsightu pomocí otočení."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Spuštění úlohy Sqoop s Hadoop v Hdinsightu pomocí otočení

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Naučte se používat otočení spustit Sqoop úlohy clusteru Hadoop v HDInsight.

Otočení slouží k ukazují můžete interakci se HDInsight pomocí zdrojové požadavky HTTP spustit, sledovat a načítání výsledků Sqoop úlohy. To funguje pomocí rozhraní WebHCat REST API (dříve označované jako Templeton) poskytovanou svůj cluster HDInsight.

> [AZURE.NOTE] Pokud znají už pomocí na základě Linux Hadoop servery, ale začínáte s HDInsight, přečtěte si [informace o používání HDInsight na Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Hadoop HDInsight clusteru (Linux nebo serveru s Windows)

* [Otočení](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Odeslat Sqoop úlohy pomocí otočení

> [AZURE.NOTE] Když používáte otočení nebo jiné ostatní komunikace s WebHCat, je třeba ověřit žádosti poskytnutím uživatelské jméno a heslo pro správce clusteru HDInsight. Také je nutné použít název obrázku jako součást aplikace identifikátor URI (Uniform Resource) slouží k odesílání požadavky na server.
>
> Příkazy v této části nahraďte **uživatelské jméno** uživatele k ověření clusteru a **heslo** pomocí hesla k uživatelskému účtu. **NÁZEV_CLUSTERU** nahraďte názvem vašeho obrázku.
>
> Rozhraní REST API je zabezpečená prostřednictvím [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy je třeba žádosti o pomocí zabezpečené HTTP (HTTPS), a pomáhají zajistit, aby vaše přihlašovací údaje bezpečně posílají serveru.

1. Z příkazového řádku ověřte, že se můžete připojit ke svůj cluster HDInsight pomocí tento příkaz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Dostanete odpověď podobná této:

        {"status":"ok","version":"v1"}

    Parametry použité v tento příkaz je takto:

    * **-u** – uživatelské jméno a heslo pro požadavek ověřit.
    * **-G** - označuje, že to požadavek GET.

    Na začátek adresu URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, bude stejné pro všechny žádosti o. Cesta **/status/status**označuje, že žádosti vrátit stav WebHCat (označovaná taky jako Templeton) pro server. 

2. Odeslání sqoop úlohy pomocí následující:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Parametry použité v tento příkaz je takto:

    * **-d** - od `-G` není použit, žádost ve výchozím nastavení metodu příspěvek. `-d`Určuje datové hodnoty, které jsou odeslány s žádostí o.

        * **user.name** - uživatele, který běží příkazu.

        * **příkaz** – Sqoop příkaz spustit.

        * **statusdir** - adresář, který stav pro tuto úlohu bude došlo k zápisu.

    Tento příkaz, měly vrátit ID práce, které mohou sloužit k Kontrola stavu projektu.

        {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav projektu, zadejte následující příkaz. Nahraďte **JOBID** hodnota vrácená v předchozím kroku. Například, pokud byla vrácenou hodnotu `{"id":"job_1415651640909_0026"}`, pak bude **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Pokud úlohy dokončí, budou stavu **byl úspěšný**.

    > [AZURE.NOTE] Otočení požadavek vrátí JavaScript Object Notation (JSON) dokument s informacemi o zaměstnání, jq slouží k načtení pouze hodnoty stav.

4. Po změně stavu úlohy na **byl úspěšný**, můžete načítat výsledky úlohy z úložiště objektů Blob Azure. `statusdir` Parametr předaný s dotazem obsahuje umístění ve výstupním souboru; v tomto případě **wasbs: / / / Příklad/otočení**. Tuto adresu ukládá výstup projektu v adresáři **Příklad/otočení** na výchozí úložiště kontejner používat svůj cluster HDInsight.

    Můžete seznam a stáhnout tyto soubory pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md). Příklad seznamu souborů v **příkladu/otočení**použijte tento příkaz:

        azure storage blob list <container-name> example/curl

    Stažení souboru, následujícím způsobem:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Buď zadejte název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry nebo sady **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_přístup\_klíč** proměnné. V tématu < href = cílový "hdinsight data.md nahrát" = "_blank" Další informace.

##<a name="limitations"></a>Omezení

* Hromadné export - na základě s Linux HDInsight, konektoru Sqoop použít pro export dat do aplikace Microsoft SQL Server nebo databáze SQL Azure aktuálně nepodporuje vloží hromadně.

* Dávkové - se systémem Linux HDInsight při použití `-batch` při provádění vloží přepnout, Sqoop provede více vloží místo dávky operace vložení.

##<a name="summary"></a>Souhrn

Jak je ukázáno v tomto dokumentu, můžete použít jako nezpracovaná žádost HTTP spustit, sledovat a zobrazit výsledky Sqoop úlohy na svůj cluster HDInsight.

Další informace o rozhraní REST použitých v tomto článku najdete v tématu <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Průvodce rozhraním REST API Sqoop</a>.

##<a name="next-steps"></a>Další kroky

Obecné informace o podregistru s HDInsight:

* [Použití Sqoop s Hadoop na HDInsight](hdinsight-use-sqoop.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


