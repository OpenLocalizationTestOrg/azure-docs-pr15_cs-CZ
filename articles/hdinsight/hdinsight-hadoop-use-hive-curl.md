<properties
   pageTitle="Pomocí otočení v HDInsight Hadoop podregistru | Microsoft Azure"
   description="Zjistěte, jak vzdáleně posílat Prasátko úkoly k Hdinsightu pomocí otočení."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Spouštění dotazů podregistru s Hadoop v Hdinsightu pomocí otočení

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto dokumentu budou Naučte se spouštění dotazů podregistru na Hadoop clusteru Azure Hdinsightu pomocí otočení.

Otočení slouží k demonstrují můžete interakci se HDInsight pomocí zdrojové požadavky HTTP spustit, sledovat a načítání výsledků podregistru dotazů. To funguje pomocí rozhraní WebHCat REST API (dříve označované jako Templeton) poskytovanou svůj cluster HDInsight.

> [AZURE.NOTE] Pokud jste již znáte pomocí na základě Linux Hadoop servery, ale začínáte s HDInsight, přečtěte si téma [Co je potřeba vědět o Hadoop na základě Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Hadoop HDInsight clusteru (Linux nebo serveru s Windows)

* [Otočení](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Spouštění dotazů podregistru pomocí otočení

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

    Na začátek adresu URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, bude stejné pro všechny žádosti o. Cesta **/status/status**označuje, že žádosti vrátit stav WebHCat (označovaná taky jako Templeton) pro server. Můžete taky požádat o verzi podregistru pomocí tento příkaz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Odpověď, to měly vrátit podobná této:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Umožňuje vytvořit novou tabulku s názvem **log4jLogs**následující:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Parametry použité v tento příkaz je takto:

    * **-d** - od `-G` není použit, žádost ve výchozím nastavení metodu příspěvek. `-d`Určuje datové hodnoty, které jsou odeslány s žádostí o.

        * **user.name** - uživatele, který běží příkazu.

        * **spuštění** – HiveQL příkazy provést.

        * **statusdir** - adresář, který stav pro tuto úlohu bude došlo k zápisu.

    Tyto příkazy provádět následující akce:

    * **PŘETAŽENÍ tabulky** – odstraní tabulku a datovém souboru, pokud již existuje v tabulce.

    * **Vytvořit externí tabulka** – vytvoří nová tabulka "externí" v podregistru. Externí tabulky obsahují pouze definici tabulky v podregistru. Data ještě zbývá do původního umístění.

        > [AZURE.NOTE] Externí tabulky by měl použít, když je očekávaná podkladová data, která chcete aktualizovat externího zdroje, jako je proces nahrávání automatické dat nebo jinou MapReduce operací, ale stále chcete podregistru dotazů použít nejnovější data.
        >
        > Uvolnění externí tabulce znamená **neodstraňujte data, pouze definici tabulky** .

    * **Formát řádku** - říká podregistru formátování data. V tomto případě polí v jednotlivých protokolu oddělená mezerou.

    * **ULOŽENÁ jako textový soubor umístění** - říká podregistru, kde jsou data uložená (příklad/datového adresáře) a že je uložená jako text.

    * **Vyberte** – slouží k výběru počet řádků místo, kam sloupec **t4** obsahuje hodnotu **[Chyba]**. Jsou tři řádky, které obsahují hodnotu to by vrátit hodnotu **3** .

    > [AZURE.NOTE] Všimněte si, že mezery mezi příkazy HiveQL nahrazují `+` character při použití s otočení. Hodnoty v uvozovkách, které obsahují mezeru, například jako oddělovač neměli nahrazuje `+`.

    * **INPUT__FILE__NAME jako % 25.log** – tato omezení hledání jenom používání souborů končící. protokolu. Pokud to není k dispozici, pokusí se podregistru prohledat všechny soubory v této adresář a jeho podadresáře, včetně souborů, které neodpovídají schématu sloupce definované pro tuto tabulku.

    > [AZURE.NOTE] Všimněte si, že % 25 je adresa URL zakódovaný formulář %, tak, aby skutečný stav `like '%.log'`. % Musí být překódován jako, adresa URL považuje se za speciální znak v adresy URL.

    Tento příkaz, měly vrátit ID práce, které mohou sloužit k Kontrola stavu projektu.

        {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav projektu, zadejte následující příkaz. Nahraďte **JOBID** hodnota vrácená v předchozím kroku. Například, pokud byla vrácenou hodnotu `{"id":"job_1415651640909_0026"}`, pak bude **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Pokud úlohy dokončí, budou stavu **byl úspěšný**.

    > [AZURE.NOTE] Otočení požadavek vrátí JavaScript Object Notation (JSON) dokument s informacemi o zaměstnání, jq slouží k načtení pouze hodnoty stav.

4. Po změně stavu úlohy na **byl úspěšný**, můžete získat výsledky úlohy z úložiště objektů Blob Azure. `statusdir` Parametr předaný s dotazem obsahuje umístění ve výstupním souboru; v tomto případě **wasbs: / / / Příklad/otočení**. Tuto adresu ukládá výstup projektu v adresáři **Příklad/otočení** na výchozí úložiště kontejner používat svůj cluster HDInsight.

    Můžete seznam a stáhnout tyto soubory pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md). Příklad seznamu souborů v **příkladu/otočení**použijte tento příkaz:

        azure storage blob list <container-name> example/curl

    Stažení souboru, následujícím způsobem:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Buď zadejte název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry nebo sady **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_přístup\_klíč** proměnné. V tématu < href = cílový "hdinsight data.md nahrát" = "_blank" Další informace.

6. Umožňuje vytvořit novou "interní" tabulku s názvem **errorLogs**následující příkazy:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Tyto příkazy provádět následující akce:

    * **Vytvoření tabulky IF NOT EXISTS** - vytvoří tabulku, pokud už neexistuje. Protože nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.

        > [AZURE.NOTE] Na rozdíl od externích tabulek uvolnění vnitřní tabulku odstraníte podkladová data.

    * **ULOŽENÝ jako ORC** - jsou uložená data ve formátu optimalizované řádku sloupcovém (ORC). Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.
    * PŘEPSAT **vložení... Vyberte** – slouží k výběru řádků z tabulky **log4jLogs** , které obsahují **[Chyba]**a vloží data do tabulky **errorLogs** .
    * **Vyberte** – slouží k výběru všechny řádky z nové tabulky **errorLogs** .

7. Pomocí ID práce do Kontrola stavu projektu. Po úspěšném, pomocí Azure rozhraní příkazového řádku pro Mac a Linux Windows, jak je popsáno dříve stáhnout a zobrazte výsledky. Výstup by měl obsahovat tři řádky, které obsahují **[Chyba]**.


##<a id="summary"></a>Souhrn

Jak je ukázáno v tomto dokumentu, můžete použít jako nezpracovaná žádost HTTP spustit, sledovat a zobrazit výsledky podregistru úlohy na svůj cluster HDInsight.

Další informace o rozhraní REST použitých v tomto článku najdete v tématu <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat odkaz</a>.

##<a id="nextsteps"></a>Další kroky

Obecné informace o podregistru s HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez se podregistru, přečtěte si článek tyto dokumenty pro ladění informace:

* [Použití rozhraní Tez na serveru s Windows HDInsight](hdinsight-debug-tez-ui.md)

* [Použití zobrazení Ambari Tez na základě Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

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


