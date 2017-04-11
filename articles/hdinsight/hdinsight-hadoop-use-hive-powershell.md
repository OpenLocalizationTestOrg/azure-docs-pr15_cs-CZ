<properties
   pageTitle="Pomocí prostředí PowerShell v HDInsight Hadoop podregistru | Microsoft Azure"
   description="Použití Powershellu ke spouštění dotazů podregistru v Hadoop na HDInsight."
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

#<a name="run-hive-queries-using-powershell"></a>Spouštění dotazů podregistru pomocí prostředí PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tento dokument obsahuje příklady spouštění dotazů podregistru v Hadoop clusteru HDInsight pomocí prostředí PowerShell Azure v režimu Azure pole Skupina zdroje.

> [AZURE.NOTE] Tento dokument neposkytuje podrobný popis čemu HiveQL příkazy, které se používají v příkladech. Další informace o HiveQL, který je v tomto příkladě použita najdete v článku [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md).


**Zjistit předpoklady pro**

Kroky v tomto článku, budete potřebovat.

- **Clusteru služby Azure HDInsight (Hadoop na HDInsight) (serveru s Windows nebo na základě Linux)**
- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Spouštění dotazů podregistru pomocí Powershellu Azure

Azure Powershellu poskytuje *rutin* , které nastavil, abyste vzdáleně spouštění dotazů podregistru na HDInsight. To je interně provést pomocí ZBÝVAJÍCÍ volání [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve označované jako Templeton) výpočetnímu clusteru HDInsight.

Spouštění dotazů podregistru ve vzdáleném clusteru HDInsight jsou použity tyto rutiny:

* **Přidat AzureRmAccount**: ověří Azure PowerShell k předplatnému Azure

* **Nový AzureRmHDInsightHiveJobDefinition**: vytvoří novou *definici úlohy* pomocí zadaného HiveQL příkazů

* **Zahájení AzureRmHDInsightJob**: odešle definici úlohy HDInsight, spustí úlohu a vrátí objektu *úlohy* něhož chcete zkontrolovat stav projektu

* **Počkat AzureRmHDInsightJob**: pomocí objektu úlohy Kontrola stavu projektu. Bude Počkejte, dokud dokončí úloha byla překročena dobu čekání či.

* **Get-AzureRmHDInsightJobOutput**: použitý k načtení výstup projektu

* **Vyvolat AzureRmHDInsightHiveJob**: slouží k spusťte HiveQL příkazy. To bude blokovat dotaz dokončení a potom vrátí tedy výsledek

* **Použití AzureRmHDInsightCluster**: Nastaví aktuální obrázku můžete u příkazu **Vyvolat AzureRmHDInsightHiveJob**

Podle těchto kroků demonstrují použití těchto rutin spuštění úlohy v HDInsight obrázku:

1. Následující kód pomocí editoru uložte jako **hivejob.ps1**. **NÁZEV_CLUSTERU** musí nahraďte názvem svůj cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otevřete nový příkazovém řádku **Prostředí PowerShell Azure** . Změna adresáře **hivejob.ps1** soubor a potom zadejte následující příkaz následujícím způsobem:

        .\hivejob.ps1

    Při spuštění skriptu se výzva k zadání přihlašovacích údajů účtu HTTPS/správce pro svůj cluster. Taky můžete být vyzváni k přihlášení k předplatnému Azure.
    
7. Až se dokončí úloha, měly vrátit informace podobná této:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Jak jsme zmínili dříve, **Vyvolat podregistru** mohou sloužit k spuštění dotazu a čekat na odpověď. Pomocí následujících příkazů a **NÁZEV_CLUSTERU** nahraďte názvem vašeho obrázku:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Výstup bude vypadat takto:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Pro delší HiveQL dotazy můžete použít rutinu Powershellu Azure **Tady řetězce** nebo HiveQL skript soubory. Následující úryvek ukazuje, jak použít rutinu **Vyvolat podregistru** spusťte soubor skriptu HiveQL. Soubor skriptu HiveQL musí nahrát wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Další informace o **Řetězce tady**najdete v článku <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Pomocí Windows Powershellu použijte-řetězce</a>.

##<a name="troubleshooting"></a>Řešení potíží

Pokud žádná doručeno až se dokončí úloha, může mít došlo k chybě při zpracování. Pokud chcete zobrazit informace o chybě pro tento projekt, na konci souboru **hivejob.ps1** přidejte následující text, uložit, abyste ji a znovu ho spusťte.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tato akce vrátí informace, které je došlo k zápisu STDERR na serveru při spuštění úlohy a může pomoct zjistit, proč se nedaří projektu.

##<a name="summary"></a>Souhrn

Jakmile uvidíte, Azure PowerShell vám bude radit snadný způsob, jak ke spouštění dotazů podregistru HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a name="next-steps"></a>Další kroky

Obecné informace o podregistru v HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
