<properties
    pageTitle="Odeslání MapReduce úlohy pomocí HDInsight .NET SDK | Microsoft Azure"
    description="Zjistěte, jak můžou odeslat MapReduce úlohy Hadoop Azure HDInsight pomocí HDInsight .NET SDK."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Spuštění úlohy MapReduce pomocí HDInsight .NET SDK

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Zjistěte, jak můžou odeslat MapReduce úlohy pomocí HDInsight .NET SDK. HDInsight clusterů jsou součástí souboru sklenice pár ukázek MapReduce. Sklenice soubor je */example/jars/hadoop-mapreduce-examples.jar*.  Jednou z vzorky je *wordcount*. Vyvíjíte C# konzoly aplikace předložit wordcount úlohy.  Úlohy přečte soubor */example/data/gutenberg/davinci.txt* a zobrazí výsledky */example/data/davinciwordcount*.  Pokud chcete znovu spusťte aplikaci, musíte vyčistit složku výstupu.

> [AZURE.NOTE] Kroky v tomto článku je třeba provést z klienta se systémem Windows. Informace o použití Linux, OS X nebo Unix klienta pro práci s podregistru použijte zobrazené v horní v článku výběr umístění tabulátoru.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **A Hadoop obrázku v HDInsight**. Najdete v článku [Začínáme s používáním na základě Linux Hadoop v HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Odeslání MapReduce úlohy pomocí HDInsight .NET SDK

HDInsight .NET SDK poskytuje knihovny .NET klienta, což usnadňuje pro práci s HDInsight clusterů z .NET. 

**Chcete-li odeslat úlohy**

1. Vytvoření aplikace konzoly C# ve Visual Studiu.
2. Z konzoly Správce balíčků Nuget, spusťte tento příkaz.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Použijte tento kód:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Stisknutím klávesy **F5** spustit aplikaci.


## <a name="next-steps"></a>Další kroky

V tomto článku jste se naučili HDInsight clusteru vytvářet několika různými způsoby. Další informace naleznete v následujících článcích:

- Vytvoření clusteru a odeslání úlohy podregistru, najdete v článku [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- Vytvoření HDInsight clusterů, najdete v článku [na základě vytvořit Linux Hadoop clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
- Správa HDInsight clusterů, najdete v článku [Správa Hadoop clusterů HDInsight](hdinsight-administer-use-management-portal.md).
- Výuka HDInsight .NET SDK, najdete v článku Principy [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).
- Pro interaktivním ověřování Azure najdete v článku [Vytvoření aplikace .NET HDInsight interaktivním ověřování](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




