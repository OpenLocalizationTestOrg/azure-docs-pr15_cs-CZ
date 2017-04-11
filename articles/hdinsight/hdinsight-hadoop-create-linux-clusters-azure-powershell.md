<properties
    pageTitle="Vytvoření Hadoop, HBase, bouře nebo Spark clusterů na Linux v pomocí prostředí PowerShell Azure HDInsight | Microsoft Azure"
    description="Naučte se vytvářet Hadoop, HBase, bouře nebo Spark clusterů na Linux pro HDInsight pomocí prostředí PowerShell Azure."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Vytvoření na základě Linux clusterů v HDInsight pomocí prostředí PowerShell Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure Powershellu je výkonné skriptu prostředí, které můžete použít k ovládání a automatizovat nasazení a řízení úloh v Microsoft Azure. Tento dokument obsahuje informace o tom, jak vytvořit obrázku na základě Linux HDInsight pomocí prostředí PowerShell Azure. Obsahuje taky ukázkového skriptu.

> [AZURE.NOTE] Azure Powershellu neexistuje pouze v klientských počítačích Windows. Pokud používáte klienta Linux, Unix nebo Mac OS X, v tématu [Vytvoření clusteru na základě Linux HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md) informace o používání rozhraní příkazového řádku Azure k vytvoření clusteru.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Musí mít následující před zahájením tohoto postupu:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure Powershellu.
    Další informace o pomocí prostředí PowerShell Azure Hdinsightu najdete v článku [Správa HDInsight pomocí Powershellu](hdinsight-administer-use-powershell.md). Seznam rutin prostředí Windows PowerShell Hdinsightu najdete v tématu [reference pro rutiny HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Vytvoření clusterů

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

K vytvoření clusteru HDInsight pomocí prostředí PowerShell Azure, je nutné provést následující postupy:

- Vytvoření skupiny Azure zdroje
- Vytvořte účet Azure úložiště
- Vytvoření kontejneru objektů Blob Azure
- Vytvoření clusteru HDInsight

Dva nejdůležitější parametry, které musíte nastavit, aby vytvořit Linux clusterů jsou ty, které určit typ OS a podrobné informace o uživateli SSH:

- Zkontrolujte, že zadáte parametr **– OSType** jako **Linux**.
- Pro účely SSH relacích na clusterů, můžete zadat heslo uživatele SSH nebo veřejným klíčem SSH. Pokud zadáte heslo uživatele SSH a SSH veřejný klíč, bude ignorována klávesu. Pokud chcete pomocí klávesy SSH pro vzdálené relace, je nutné zadat prázdné heslo SSH po zobrazení výzvy za jiný. Další informace o použití SSH s Hdinsightu najdete v následujících článcích:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Tento skript ukazuje, jak vytvořit nového obrázku:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Hodnoty, které zadáte **$clusterCredentials** slouží k vytvoření uživatelského účtu Hadoop clusteru. Připojení k clusteru pomocí tohoto účtu.

Hodnoty, které zadáte **$sshCredentials** slouží k vytvoření uživatele SSH clusteru. Použijte tento účet dál vzdálenou relaci SSH na clusteru úlohy.

> [AZURE.IMPORTANT] V tomto skriptu zadejte počtu uzlů pracovníka, které budou v clusteru. Pokud máte v plánu používat víc než 32 uzly pracovníka (nebo při vytváření clusteru roztažením clusteru po vytvoření), musíte taky zadáte hlavy uzel velikost s nejméně 8 jádra a 14 GB paměti RAM.
>
> Další informace o velikosti uzel a související náklady najdete v článku [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Může trvat až 20 minut vytvoření clusteru.

Následující příklad ukazuje, jak přidat další úložiště účet:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Přizpůsobení clusterů

- V tématu [použití zavádění clusterů přizpůsobení HDInsight](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- V tématu [clusterů na základě vlastní nastavení systému Windows HDInsight pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Odstranění clusteru

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

Teď úspěšně jste vytvořili HDInsight clusteru, použijte v následujících zdrojích se dozvíte, jak pracovat s svůj cluster.

### <a name="hadoop-clusters"></a>Hadoop clusterů

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)
* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
* [Použití MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase clusterů

* [Začínáme s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Zabývají vývojem aplikací Java pro HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Bouře clusterů

* [Můžete vyvíjet Java topologie pro bouře na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití Python součástí bouře na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a sledovat topologií s bouře na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark clusterů

* [Vytvoření samostatného aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Spuštění úlohy vzdáleně Spark clusteru pomocí Livius](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)
