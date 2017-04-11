<properties
   pageTitle="Prasátko Hadoop pomocí prostředí PowerShell v HDInsight | Microsoft Azure"
   description="Zjistěte, jak můžou odeslat Prasátko úlohy Hadoop clusteru na pomocí prostředí PowerShell Azure HDInsight."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Spuštění úlohy Prasátko pomocí prostředí PowerShell

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tento dokument obsahuje příklady online přes Azure odesílat Prasátko úlohy do Hadoop clusteru HDInsight. Prasátko umožňuje psaní MapReduce úlohy pomocí jazyka (latinka Prasátko,) modely transformace dat, spíše než mapování a zmenšení funkcí.

> [AZURE.NOTE] Tento dokument neposkytuje podrobný popis čemu Prasátko latinka příslušným použité v příkladech. Informace o latinka Prasátko v tomto příkladě použita najdete v tématu [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat.

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Spuštění úlohy Prasátko pomocí prostředí PowerShell

Azure Powershellu poskytuje *rutin* , pomocí kterých můžete vzdáleně spustit Prasátko úlohy na HDInsight. To je interně, provést pomocí ZBÝVAJÍCÍ volání [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve označované jako Templeton) výpočetnímu clusteru HDInsight.

Spuštěním Prasátko úloh vzdálené clusteru HDInsight jsou použity tyto rutiny:

* **Přihlášení AzureRmAccount**: ověří Azure Powershellu k předplatnému Azure

* **Nový AzureRmHDInsightPigJobDefinition**: vytvoří novou *definici úlohy* pomocí zadaného příslušným Prasátko (latinka)

* **Zahájení AzureRmHDInsightJob**: odešle definici úlohy HDInsight, spustí úlohu a vrátí objektu *úlohy* něhož chcete zkontrolovat stav projektu

* **Počkat AzureRmHDInsightJob**: pomocí objektu úlohy Kontrola stavu projektu. Ho bude počkejte na dokončení projektu nebo překročil prodleva.

* **Get-AzureRmHDInsightJobOutput**: použitý k načtení výstup projektu

Podle těchto kroků demonstrují použití těchto rutin spuštění úlohy na svůj cluster HDInsight.

1. Následující kód pomocí editoru uložte jako **pigjob.ps1**. **NÁZEV_CLUSTERU** musí nahraďte názvem svůj cluster HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Otevřete nový příkazovém řádku prostředí Windows PowerShell. Změna adresáře **pigjob.ps1** soubor a potom zadejte následující příkaz následujícím způsobem:

        .\pigjob.ps1
        
    Nejdřív se výzva k přihlášení k předplatnému Azure. Potom se zobrazí výzva pro HTTPs/účtu jménem a heslem správce pro clusteru HDInsight.

7. Až se dokončí úloha, měly vrátit informace podobná této:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Řešení potíží

Pokud nejsou žádné informace vrácena až se dokončí úloha, může mít došlo k chybě při zpracování. K zobrazení informací o chybě u této úlohy, přidejte následující příkaz Konec **pigjob.ps1** soubor uložit, abyste ji a znovu ho spusťte.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tento postup vrátí informace, které vytvořené pomocí STDERR na serveru při spuštění úlohy a může pomoct zjistit, proč se nedaří projektu.

##<a id="summary"></a>Souhrn

Jakmile uvidíte, Azure PowerShell vám bude radit snadný způsob, jak spustit Prasátko úlohy HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o Prasátko v HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
