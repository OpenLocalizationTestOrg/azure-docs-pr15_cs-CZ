<properties
    pageTitle="Použití Hadoop Sqoop v HDInsight | Microsoft Azure"
    description="Naučte se používat HDInsight .NET SDK Sqoop import a export mezi clusteru Hadoop a databáze Azure SQL."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Spuštění úlohy Sqoop pomocí .NET SDK pro Hadoop v HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Naučte se používat HDInsight .NET SDK spuštění úlohy Sqoop HDInsight pro import a export mezi HDInsight obrázku a databáze Azure SQL nebo databáze SQL serveru.

> [AZURE.NOTE] Kroky v tomto článku se dá používat se některý z Windows nebo Linux HDInsight clusteru; Tento postup bude pouze fungovat z klienta se systémem Windows. Umožňuje vybrat jiné metody výběr umístění tabulátoru horní části tohoto článku.

###<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **A Hadoop obrázku v HDInsight**. V tématu [Vytvoření obrázku a databáze SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Spuštění Sqoop pomocí .NET SDK

HDInsight .NET SDK poskytuje knihovny .NET klienta, což usnadňuje pro práci s HDInsight clusterů z .NET. V této části vytvoříte C# aplikace konzoly exportovat do tabulky databáze SQL, který jste vytvořili dříve v tomto kurzy hivesampletable.

**Odeslání Sqoop projektu**

1. Vytvoření aplikace konzoly C# ve Visual Studiu.
2. Z konzoly Visual Studio balíčku správce, spusťte tento příkaz Nuget k importu balíček.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Použijte následující kód v souboru Program.cs:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Stisknutím klávesy **F5** nespustíte. 

##<a name="limitations"></a>Omezení

* Hromadné export - na základě s Linux HDInsight, konektoru Sqoop použít pro export dat do aplikace Microsoft SQL Server nebo databáze SQL Azure hromadné vloží aktuálně nepodporuje.

* Dávkové - se systémem Linux HDInsight při použití `-batch` při provádění vloží přepnout, Sqoop provede více vloží místo dávky operace vložení.

##<a name="next-steps"></a>Další kroky

Teď jste se naučili používání Sqoop. Další informace najdete v tématu:

- [Použití Oozie s HDInsight](hdinsight-use-oozie.md): použití Sqoop akce v pracovním postupu Oozie.
- [Daty zpoždění letů analyzovat pomocí HDInsight](hdinsight-analyze-flight-delay-data.md): použití podregistru a analyzujte data letu zpoždění data a pak pomocí Sqoop export dat do databáze Azure SQL.
- [Odeslání dat do HDInsight](hdinsight-upload-data.md): Najít další způsoby uložení dat k úložišti objektů Blob HDInsight/Azure.


