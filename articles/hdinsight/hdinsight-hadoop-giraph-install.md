<properties
    pageTitle="Instalace a používání Giraph v Hadoop clusterů HDInsight | Microsoft Azure"
    description="Zjistěte, jak přizpůsobit HDInsight obrázku s Giraph a jak používat Giraph."
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
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-giraph-in-hdinsight"></a>Instalace a používání Giraph v HDInsight


Zjistěte, jak můžete přizpůsobit podle obrázku HDInsight Windows s Giraph pomocí skriptu akce a jak používat Giraph zpracuje rozsáhlé grafy. Informace o použití Giraph s clusteru bázi Linux najdete v tématu [Instalace Giraph na clusterů HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).
 
Giraph typy dosažených obrázku (Hadoop bouře, HBase, Spark) můžete nainstalovat na Azure Hdinsightu pomocí *Skriptu akce*. Ukázka skriptu nainstalovat Giraph HDInsight clusteru je k dispozici jen pro čtení Azure úložiště objektů blob na [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Ukázka skriptu funguje jenom s HDInsight clusteru podverzí 3.1. Další informace o verzích clusteru Hdinsightu najdete v článku [verze obrázku HDInsight](hdinsight-component-versioning.md).

**Související články**

- [Instalace Giraph na clusterů HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce.
- [Vývoj akci skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Co je Giraph?

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> umožňuje provádět grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight. Grafy modelu vztahy mezi objekty, například připojení mezi směrovači na velké síť skutečně Internetu nebo vztahy mezi lidé na sociálních sítích (někdy označovány jako sociální grafu). Zpracování grafu umožňuje důvod, proč k relacím mezi objekty v grafu, jako například:

- Identifikace potenciální přáteli na základě aktuální relací.
- Identifikace nejkratší mezi dva počítače v síti.
- Výpočet pořadí stránek webových stránek.


## <a name="install-giraph-using-portal"></a>Instalace Giraph portálu

1. Zahájení vytváření clusteru pomocí možnost **Vytvořit vlastní** , jak je uvedeno na stránce [vytvořit Hadoop clusterů HDInsight](hdinsight-provision-clusters.md#portal).
2. Na stránce **Akce skriptu** průvodce klikněte na **Přidat akci skript** k poskytování údajů o akci skript, jak je ukázáno v následujícím příkladu:

    ![Použití akce skript k přizpůsobení clusteru] (./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Použití akce skript k přizpůsobení clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Jméno</td>
            <td>Zadejte název akce skriptu. Například <b>Instalace Giraph</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Zadejte identifikátor URI (Uniform Resource) skript, který je zavolat a přizpůsobení clusteru. Například <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Typ uzel</td>
            <td>Určete uzly, na kterých běží skript přizpůsobení. Můžete vybrat <b>všechny uzly</b>, <b>pouze záhlaví uzlů</b>nebo <b>pouze pracovníka uzlů</b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry, v případě potřeby skriptem. Skript, který chcete nainstalovat Giraph nevyžaduje všechny parametry, takže to můžou být prázdné.</td></tr>
    </table>

    Můžete přidat víc akci skriptu pro instalaci více součástí na clusteru. Až přidáte skripty, klikněte na zaškrtnutí začnete vytvářet clusteru.

## <a name="use-giraph"></a>Použití Giraph

Příklad SimpleShortestPathsComputation používáme prokázat základní implementaci <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> vyhledání nejkratší cesta mezi objekty v grafu. Pomocí následujících kroků nahrát ukázkových dat a sklenice vzorku, spuštění úlohy pomocí příklad SimpleShortestPathsComputation a prohlédněte si výsledky.

1. Ukázka datového souboru nahrajte do úložiště objektů Blob Azure. Na počítači místní nejradši s názvem **tiny_graph.txt**. By měl obsahovat následující řádky:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Nahrání souboru tiny_graph.txt k základnímu úložišti primární pro svůj cluster HDInsight. Další informace o tom, jak uložit dat najdete v článku [Odeslat data pro Hadoop projekty v HDInsight](hdinsight-upload-data.md).

    Tato data popisuje relace mezi objekty v řízené grafu pomocí formátu [zdroj\_id, zdroje\_hodnotu, [[cíl\_id], [okraj\_hodnotu];...]]. Každý řádek představuje relace mezi **zdroj\_id** objektu a jeden nebo více **cíl\_id** objekty. **Okraj\_hodnotu** (nebo tloušťku) můžete považovat za síly nebo vzdálenost připojení mezi **source_id** a **cíl\_id**.

    Nakreslili, a pomocí hodnoty (nebo tloušťku) jako vzdálenost mezi objekty, výše uvedených dat může vypadat takto:

    ![tiny_graph.txt nakreslili jako kruhy s odlišnými vzdálenost mezi řádky](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)



4. Spuštění SimpleShortestPathsComputation příkladu. Pomocí následující rutiny prostředí PowerShell Azure spuštění příkladu pomocí souboru tiny_graph.txt předávat na vstupu. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasbs:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasbs:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasbs:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments

        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    Ve výše uvedeném příkladu nahraďte **název_clusteru** název clusteru Hdinsightu, který má Giraph nainstalovaný.

5. Zobrazte výsledky. Po dokončení projektu, výsledky uloží do dvou výstup souborů v __wasbs: / / / Příklad/out/shotestpaths__ složky. Soubory, které se označují jako __součást m 00001__ a __část m 00002__. Provedení následujících kroků můžete stáhnout a zobrazit výstup:

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    To vytvořte strukturu adresářů __Příklad/výstup/shortestpaths__ aktuálního adresáře na počítači a stahovat soubory dvou výstup do tohoto umístění.

    Zobrazení obsahu soubory získáte pomocí rutiny __kočka__ :

        Cat example/output/shortestpaths/part*

    Výstup by měl vypadat takto:


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Příklad je pevné kódovaný začínat SimpleShortestPathComputation objektu ID 1 a najděte nejkratší cestu k jiným objektům. Aby výstup obsaženou jako `destination_id distance`, kde je vzdálenost hodnotu (nebo weight (váha)) okrajů cesty mezi objektem ID 1 a cílové ID.

    Vizualizace to můžete ověřit výsledky cestování nejkratší cesty mezi ID 1 a jiných objektů. Všimněte si, že nejkratší cestu ID 1 až ID 4 5. Toto je celková vzdálenost mezi <span style="color:orange">ID 1 3</span>a potom <span style="color:red">ID 3 a 4</span>.

    ![Kreslení objektů jako kruhy s nejkratší cesty mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Instalace Giraph pomocí prostředí Aure PowerShell

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Vzorku ukazuje, jak nainstalovat Spark pomocí prostředí PowerShell Azure. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Instalace Giraph pomocí .NET SDK

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Vzorku ukazuje, jak nainstalovat Spark pomocí .NET SDK. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).


## <a name="see-also"></a>Viz taky

- [Instalace Giraph na clusterů HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce.
- [Vývoj akci skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions.md).
- [Instalace a používání Spark v HDInsight clusterů][hdinsight-install-spark]: Ukázka skriptu akci o instalaci Spark.
- [Instalace R na HDInsight clusterů][hdinsight-install-r]: Ukázka skriptu akci o instalaci R.
- [Instalace Solr na HDInsight clusterů](hdinsight-hadoop-solr-install.md): Ukázka skriptu akci o instalaci Solr.



[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: ../powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
