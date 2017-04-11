<properties
    pageTitle="Ladění a analyzovat Hadoop služeb se haldy výpisy | Microsoft Azure"
    description="Automaticky shromažďovat haldy výpisy služby Hadoop a umístit do účtu úložiště objektů Blob Azure ladění a analýzy."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Shromažďovat haldy vypíše v úložišti objektů Blob ladění a analyzovat Hadoop služby

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Vypíše haldy obsahovat snímek paměti aplikace, včetně hodnoty proměnných v době, kdy byla vytvořená výpisu. Tak, aby byly velmi užitečné pro diagnostiku problémů, ke kterým dochází při spuštění. Vypíše haldy můžete automaticky shromažďují služby Hadoop a umístěn uvnitř účtu úložiště objektů Blob Azure uživatele v části HDInsightHeapDumps /. 

Kolekce haldy výpisy pro různé služby musí být povolené služby na jednotlivé clusterů. Výchozí Tato funkce je vypnutí clusteru. Tyto haldy výpisy může být velký, tak je vhodné sledování účtu úložiště objektů Blob, kde jsou se ukládá po povolil kolekci.

> [AZURE.NOTE] Informace v tomto článku platí jenom pro HDInsight serveru s Windows. Informace na základě Linux Hdinsightu najdete v tématu [Povolení haldy výpisy Hadoop služeb na základě Linux HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Služby pro haldy výpisy

Můžete povolit haldy výpisy pro následující služby:

*  **hcatalog** - tempelton
*  **podregistru** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **vláken** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurace prvky podporující nástroj haldy výpisy

Zapnout haldy výpisy pro službu, budete muset nastavit správné konfiguraci prvky v části pro službu, které nastavil **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Hodnota **service_name** může být v kterémkoliv z výše uvedených služeb: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, nebo namenode.

## <a name="enable-using-azure-powershell"></a>Povolit pomocí prostředí PowerShell Azure

Například zapnout haldy výpisy pomocí prostředí PowerShell Azure pro jobhistoryserver, které by takto:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Povolení používání .NET SDK

Například zapnout haldy výpisy pomocí Azure HDInsight .NET SDK pro jobhistoryserver, které by takto:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
