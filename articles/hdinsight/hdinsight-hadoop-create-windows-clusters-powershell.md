<properties
   pageTitle="Vytvoření serveru s Windows Hadoop clusterů v pomocí prostředí PowerShell Azure HDInsight | Microsoft Azure"
    description="Naučte se vytvářet clusterů pro Azure Hdinsightu pomocí prostředí PowerShell Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Vytvoření serveru s Windows Hadoop clusterů v pomocí prostředí PowerShell Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Naučte se vytvářet pomocí prostředí PowerShell Azure HDInsight clusterů. Azure Powershellu je moduly, které poskytuje rutin ke správě Azure pomocí prostředí Windows PowerShell. Vytváření jiných clusteru nástrojů a funkcí, a klikněte na kartu horní části této stránky nebo si přečtěte [způsobů vytvoření obrázku](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Požadavky:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Než začnete pokyny v tomto článku, musíte mít takto:

- Azure předplatného. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure Powershellu.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Vytvoření clusterů
Azure Powershellu je výkonné skriptu prostředí, které můžete použít k ovládání a automatizovat nasazení a řízení úloh v Azure. Tato část obsahuje pokyny k vytvoření clusteru HDInsight pomocí prostředí PowerShell Azure. Informace o konfiguraci workstation abyste mohli spouštět rutiny prostředí Windows PowerShell Hdinsightu najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Další informace o pomocí prostředí PowerShell Azure Hdinsightu najdete v článku [Správa HDInsight pomocí Powershellu](hdinsight-administer-use-powershell.md). Seznam rutin prostředí Windows PowerShell Hdinsightu najdete v tématu [reference pro rutiny HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Následující postupy jsou potřebné k vytvoření clusteru HDInsight pomocí prostředí PowerShell Azure:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Vytvoření clusterů pomocí šablony ARM

Azure PowerShell můžete použít pro nasazení ARM šablonu, která vytvoří HDInsight obrázku.  V tématu [volání šablony pomocí prostředí PowerShell Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Přizpůsobení clusterů

- V tématu [použití zavádění clusterů přizpůsobení HDInsight](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- V tématu [clusterů na základě vlastní nastavení systému Windows HDInsight pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Další kroky
V tomto článku jste se naučili HDInsight clusteru vytvářet několika různými způsoby. Další informace naleznete v následujících článcích:

* [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – zjistěte, jak začít pracovat s HDInsight obrázku
* [Odeslání Hadoop úlohy programově](hdinsight-submit-hadoop-jobs-programmatically.md) – zjistěte, jak můžou programově odeslat úlohy HDInsight
* [Správa Hadoop clusterů HDInsight pomocí prostředí PowerShell](hdinsight-administer-use-powershell.md) – informace o práci s HDInsight pomocí prostředí PowerShell Azure
* [Následující dokumentaci pro Azure HDInsight SDK]  [ hdinsight-sdk-documentation] -Seznamte se s HDInsight SDK




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
