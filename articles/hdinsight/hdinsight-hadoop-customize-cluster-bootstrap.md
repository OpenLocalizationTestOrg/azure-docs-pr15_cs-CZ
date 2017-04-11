<properties
    pageTitle="Přizpůsobení clusterů HDInsight pomocí zavádění | Microsoft Azure"
    description="Zjistěte, jak můžete přizpůsobit pomocí zavádění clusterů HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Přizpůsobení clusterů HDInsight pomocí zavádění

Někdy který chcete konfigurovat konfigurační soubory, které zahrnují:

- clusterIdentity.xml
- základní site.xml
- Gateway.XML
- hbase env.xml
- hbase site.xml
- hdfs site.xml
- podregistru env.xml
- podregistru site.xml
- mapred webu
- oozie site.xml
- oozie env.xml
- bouře site.xml
- tez site.xml
- webhcat site.xml
- site.xml vláken

Clusterů nelze zachovat změny kvůli znovu pro zpracování obrázků. Další informace o znovu pro zpracování obrázků najdete v článku [Role Instance restartuje termín inovací s operačním systémem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx). Aby změny prostřednictvím clusterů životnost můžete HDInsight clusteru přizpůsobení během s vytvářením. Toto je doporučené postupy pro změnu konfigurace clusteru a zachovat přes tyto Azure reimage restartujte počítač restart události. Tyto změny konfigurace použijí před služby počáteční tak nemusí spuštění služeb. 

Použití zavádění 3 způsoby:

- Použití Powershellu Azure

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
- Použití .NET SDK
- Použití šablony správce prostředků Azure

Informace o instalaci dodatečný HDInsight clusteru během údaje o času vytvoření najdete v tématu:

- [Přizpůsobení clusterů HDInsight pomocí skriptu akce (Linux)](hdinsight-hadoop-customize-cluster-linux.md)
- [Přizpůsobení clusterů HDInsight pomocí skriptu akce (Windows)](hdinsight-hadoop-customize-cluster.md)

## <a name="use-azure-powershell"></a>Použití Powershellu Azure

Následující kód prostředí PowerShell upravila podregistru konfigurace:

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
    
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -Config $config 

Dokončení práce skript Powershellu najdete v [Dodatku A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).

**Potvrďte změny:**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. V levém podokně klikněte na tlačítko **Procházet**a pak klikněte na **Clusterů HDInsight**.
3. Klikněte na obrázku, který jste právě vytvořili pomocí skriptů Powershellu.
4. Klikněte na **řídicí panel** v horní části zásuvné otevřete Ambari uživatelského rozhraní.
5. Klikněte v levé nabídce **podregistru** .
6. Klikněte na tlačítko **HiveServer2** z **Souhrn**.
7. Klikněte na kartu **Configs** .
8. Klikněte v levé nabídce **podregistru** .
9. Klikněte na kartu **Upřesnit** .
10. Přejděte dolů a potom rozbalte položku **Upřesnit podregistru webu**.
11. Vyhledejte **hive.metastore.client.socket.timeout** v části.

Některé další ukázky k úpravám další konfigurační soubory:

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

Další informace najdete v tématu blog Azim Uddin s názvem [Vytvoření přizpůsobení clusteru HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).

## <a name="use-net-sdk"></a>Použití .NET SDK

V tématu [Vytvoření Linux založené clusterů v HDInsight pomocí .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Správce prostředků použití šablony

Správce prostředků šablony můžete použít zavádění:

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![hdinsight hadoop přizpůsobení šablony správce zavádění azure zdroje obrázku](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)



## <a name="see-also"></a>Viz taky

- [Vytvoření Hadoop clusterů v HDInsight] [ hdinsight-provision-cluster] obsahuje pokyny k vytvoření clusteru HDInsight pomocí jiné možnosti vlastního nastavení.
- [Můžete vyvíjet skripty akci skriptu pro HDInsight][hdinsight-write-script]
- [Instalace a používání Spark v HDInsight clusterů][hdinsight-install-spark]
- [Instalace a používání R v HDInsight clusterů][hdinsight-install-r]
- [Instalace a použití clusterů Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalace a použití clusterů Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fáze při vytváření obrázku"

## <a name="appx-a-powershell-sample"></a>Ukázka aplikace A: Powershellu

Tento skript Powershellu vytvoří HDInsight obrázku a přizpůsobí podregistru nastavení:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<ENTER AN ALIAS>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

    $sshUserName = "sshuser" #HDInsight ssh user name
    $sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
    #endregion

    ####################################
    # Service names and varialbes
    ####################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect to Azure
    ####################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use the cluster name as the container name

    ####################################
    # Create a configuration object
    ####################################
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
        
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    ####################################
    # Create an HDInsight cluster
    ####################################
    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    $sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
    $sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes 1 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -SshCredential $sshCredential `
        -Config $config

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion
