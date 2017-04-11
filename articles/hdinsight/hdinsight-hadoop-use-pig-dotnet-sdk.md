<properties
   pageTitle="Použití Hadoop Prasátko s .NET v HDInsight | Microsoft Azure"
   description="Naučte se používat SDK .NET pro Hadoop Prasátko úlohy do Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Spuštění úlohy Prasátko pomocí .NET SDK pro Hadoop v HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tento dokument obsahuje příklad použití SDK .NET pro Hadoop Prasátko úlohy do Hadoop clusteru HDInsight.

HDInsight .NET SDK poskytuje knihovny klienta .NET, které usnadňují práci s HDInsight clusterů z .NET. Prasátko umožňuje vytvořit MapReduce operací modelování řadu transformace dat. Naučíte se použití základní C# aplikace k odeslání Prasátko úlohy HDInsight obrázku.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat.

* Clusteru služby Azure HDInsight (Hadoop na HDInsight) (systém Windows nebo na základě Linux).
* Visual Studio 2012 nebo 2013 nebo 2015.

## <a name="create-the-application"></a>Vytvoření aplikace

HDInsight .NET SDK poskytuje knihovny .NET klienta, což usnadňuje pro práci s HDInsight clusterů z .NET. 


1. Otevřete aplikaci Visual Studio 2012 nebo 2013
2. V nabídce **soubor** vyberte **Nový** a pak vyberte **projektu**.
3. Nového projektu zadejte nebo vyberte následující hodnoty.

    <table>
    <tr>
    <th>Vlastnost</th>
    <th>Hodnota</th>
    </tr>
    <tr>
    <th>Kategorie</th>
    <th>Šablony/Visual Basic a Windows</th>
    </tr>
    <tr>
    <th>Šablony</th>
    <th>Aplikace konzoly</th>
    </tr>
    <tr>
    <th>Jméno</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Klikněte na **OK** vytvořte projekt.
5. V nabídce **Nástroje** vyberte knihovnu balíčku nebo **Správce** **Nuget balíčku**a vyberte **Konzoly balíčku správce**.
6. Spusťte tento příkaz v konzole k instalaci balíčků .NET SDK.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. V Průzkumníku poklepejte **Program.cs** a otevřete ho. Nahraďte stávající kód takto.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Stisknutím klávesy **F5** spustit aplikaci.
8. Stisknutím klávesy **ENTER** ukončete aplikaci.

## <a name="summary"></a>Souhrn

Jak vidíte, SDK .NET pro Hadoop umožňuje vytvářet .NET aplikace, které odeslat Prasátko úlohy HDInsight clusteru a sledování stavu projektu.

## <a name="next-steps"></a>Další kroky

Obecné informace o Prasátko v HDInsight.

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete spolupracovat s Hadoop na HDInsight.

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md) [Náhled portál]: https://portal.azure.com/
