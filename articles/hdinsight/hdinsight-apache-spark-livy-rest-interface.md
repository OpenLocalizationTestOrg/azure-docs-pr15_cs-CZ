<properties
    pageTitle="Odeslat Spark úlohy vzdáleně používat Livius | Microsoft Azure"
    description="Zjistěte, jak pomocí služby Livius HDInsight clusterů vzdáleně posílat Spark úkoly."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Odeslání Spark úlohy vzdáleně Apache Spark obrázku na HDInsight Linux pomocí Livius

Apache Spark cluster v Azure HDInsight obsahuje Livius rozhraní REST odesláním úloh vzdáleně Spark obrázku. Podrobné si přečtěte následující dokumentaci najdete v článku [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Livius slouží ke spuštění interaktivní Spark prostředí nebo odeslat dávku úlohy proběhnout na Spark. Tento článek pojednává o používání Livius odesílat dávkové úlohy. Níže uvedené syntaxe volat ZBÝVAJÍCÍ koncový bod Livius pomocí otočení.

**Požadavky:**

Je nutné mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Odeslání dávku clusteru

Před odesláním dávku, musí nahrát sklenice aplikace v úložišti clusteru přidružené clusteru. [**AzCopy**](../storage/storage-use-azcopy.md), nástroj příkazového řádku, můžete to udělat. Existuje spoustu dalších klienty, se kterými můžete odeslat data. Můžete najít další informace o jejich na [Odeslat data pro Hadoop projekty v HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Příklady**:

* Pokud je soubor sklenice v úložišti clusteru (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Pokud chcete předat sklenice názvu souboru a název třídy jako součást vstupní soubor (v tomto příkladu vstup.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Získat informace o dávkách spuštěné v clusteru

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Příklady**:

* Pokud chcete načíst všechny listy spuštěných obrázku:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Pokud chcete načíst konkrétní dávka se dané batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Odstraňte dávku

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Příklad**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livius a vysoké dostupnosti

Livius poskytuje vysoké dostupnosti Spark úlohami na clusteru. Tady je několik příkladů.

* Pokud službu Livius přejde po jste odeslali úlohy vzdáleně Spark clusteru, úlohy pokračuje běží na pozadí. Je-li Livius zálohovat, obnoví stav úlohy a sestavy zpátky.

* Poznámkové bloky Jupyter pro HDInsight jsou technologii Livius v back-end. Pokud poznámkový blok je spuštěná úloha Spark a získá restartovat službu Livius, poznámkového bloku zůstanou spustit kód buňky. 

## <a name="show-me-an-example"></a>Zobrazení příkladu

V této části Podíváme se na příklady o používání Livius podat žádost Spark, sledovat průběh aplikace a potom odstraňte úkoly. Aplikace, které budeme používat v tomto příkladu je vytvořené v článku [vytvoření samostatného Scala aplikace a spuštění clusteru HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md). Následující kroky předpokládají takto:

* Jste už zkopírovali myší aplikační sklenice úložiště účet spojený s clusteru.
* Máte nainstalované na počítači, kde jsou vyzkoušení tohoto postupu otočení.

Proveďte následující kroky.

1. Dejte nám nejdřív zkontrolujte, jestli je spuštěný Livius na clusteru. Můžeme udělat získáním seznam spuštěných listy. Pokud používáte úlohy pomocí Livius poprvé, to měly vrátit nula.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Výstup by měla získat podobná této:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Všimněte si, jak na poslední řádek výstup říká **Celkem: 0**, který navrhne bez pracovního listy.

2. Dejte nám odešlete dávku. Fragment dole používá vstupní soubor (vstup.txt) předat sklenice název a název třídy jako parametry. Toto je doporučený postup, pokud používáte takto z počítače s Windows.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Parametry v souboru **vstup.txt** je definována následujícím způsobem:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Měli byste vidět výstup podobná této:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Všimněte si, jak poslední řádek výstup říká **Stav: spuštění**. Taky s textem, **id: 0**. To je ID dávku.

3. Můžete teď vyvolat stav této konkrétní dávku pomocí ID dávku.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Měli byste vidět výstup podobná této:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Výstup se nyní zobrazí **Stav: Úspěch**, který navrhne úloha byla úspěšně dokončena.

4. Pokud chcete, můžete teď odstranit dávku.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Měli byste vidět výstup podobná této:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Poslední řádek výstup zobrazuje dávku byla úspěšně odstraněna. Pokud byste odstranili projektu je spuštěná, bude v podstatě ukončit úlohu. Pokud byste odstranili úlohu, která dokončí, úspěšně nebo jinak slouží k odstranění informací projektu úplně.

## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teplotu pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)

* [Analýza protokolu webu pomocí Spark HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spuštění aplikací

* [Vytvoření samostatného aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Modul plug-in nástroje HDInsight IntelliJ představu umožňuje vytvořit a odeslat Spark Scala aplikace](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Modul plug-in pro použití HDInsight nástroje pro IntelliJ NÁPAD vzdáleně ladění Spark aplikací](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Pomocí obrázku Spark na HDInsight Zeppelin poznámkových bloků](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Oříšky umožňující Jupyter poznámkového bloku na Spark obrázku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Použití externích balíčků pomocí Jupyter poznámkových bloků](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
