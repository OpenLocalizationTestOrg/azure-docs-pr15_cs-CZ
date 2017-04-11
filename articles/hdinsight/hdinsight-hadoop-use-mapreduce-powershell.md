<properties
   pageTitle="Pomocí Hadoop MapReduce a Powershellu | Microsoft Azure"
   description="Zjistěte, jak vzdáleně pracovat MapReduce úlohy s Hadoop HDInsight pomocí Powershellu."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Spuštění úlohy MapReduce s Hadoop na HDInsight pomocí prostředí PowerShell

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tento dokument obsahuje příklady pomocí Powershellu Azure HDInsight clusteru spuštění úlohy MapReduce Hadoop.

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

- **Clusteru služby Azure HDInsight (Hadoop na HDInsight) (serveru s Windows nebo na základě Linux)**

- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Spuštění úlohy MapReduce pomocí prostředí PowerShell Azure

Azure Powershellu obsahuje *rutiny* , aby bylo možné vzdáleně spouštět MapReduce úlohy na HDInsight. To je interně, provést pomocí ZBÝVAJÍCÍ volání [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve označované jako Templeton) výpočetnímu clusteru HDInsight.

Tyto rutiny se používají při spuštění úlohy MapReduce v vzdáleného clusteru HDInsight.

* **Přihlášení AzureRmAccount**: ověří Azure PowerShell k předplatnému Azure

* **Nový AzureRmHDInsightMapReduceJobDefinition**: vytvoří novou *definici úlohy* pomocí zadané informace MapReduce

* **Zahájení AzureRmHDInsightJob**: odešle definici úlohy HDInsight, spustí úlohu a vrátí objektu *úlohy* něhož chcete zkontrolovat stav projektu

* **Počkat AzureRmHDInsightJob**: pomocí objektu úlohy Kontrola stavu projektu. Ho čeká dokončí úloha byla překročena dobu čekání či.

* **Get-AzureRmHDInsightJobOutput**: použitý k načtení výstup projektu

Podle těchto kroků demonstrují použití těchto rutin spuštění úlohy v svůj cluster HDInsight.

1. Následující kód pomocí editoru uložte jako **mapreducejob.ps1**. **NÁZEV_CLUSTERU** musí nahraďte názvem svůj cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otevřete nový příkazovém řádku **Prostředí PowerShell Azure** . Změna adresáře **mapreducejob.ps1** soubor a potom zadejte následující příkaz následujícím způsobem:

        .\mapreducejob.ps1
    
    Když spustíte skript, můžete být vyzváni k ověření k předplatnému Azure. Můžete taky vyzváni k zadání HTTPS/účtu jménem a heslem správce pro clusteru HDInsight.

3. Až se dokončí úloha mají přijímat výstup následujícím způsobem:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Tento výstup označuje, že úloha byla úspěšně dokončena.

    > [AZURE.NOTE] Pokud **ExitCode** má hodnotu než 0, přečtěte si článek [Poradce při potížích](#troubleshooting).

    V tomto příkladě se uloží stažené soubory do souboru **výstup.txt** adresář, který spuštění skriptu z.

###<a name="view-output"></a>Zobrazit výstup

Otevřete soubor **výstup.txt** v textovém editoru zobrazíte slov a počtu vytvořené pomocí projektu.

> [AZURE.NOTE] Výstupní soubory úlohy MapReduce jsou neměnné. To je znovu spustit tento příklad potřebnosti chcete změnit jméno ve výstupním souboru.

##<a id="troubleshooting"></a>Řešení potíží

Pokud nejsou žádné informace vrácena až se dokončí úloha, může mít došlo k chybě při zpracování. K zobrazení informací o chybě u této úlohy, přidejte následující příkaz Konec **mapreducejob.ps1** soubor uložit, abyste ji a znovu ho spusťte.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tato akce vrátí informace, které vytvořené pomocí STDERR na serveru při spuštění úlohy a může pomoct zjistit, proč selhává projektu.

##<a id="summary"></a>Souhrn

Jakmile uvidíte, Azure PowerShell vám bude radit snadný způsob, jak spustit MapReduce úlohy HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o MapReduce úlohy v HDInsight:

* [Použití MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)
