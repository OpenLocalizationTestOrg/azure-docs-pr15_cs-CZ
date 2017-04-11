<properties
   pageTitle="Vytvoření serveru s Windows Hadoop clusterů v HDInsight pomocí .NET SDK | Microsoft Azure"
    description="Naučte se vytvářet HDInsight clusterů pro Azure Hdinsightu pomocí .NET SDK."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-net-sdk"></a>Vytvoření serveru s Windows Hadoop clusterů v HDInsight pomocí .NET SDK

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]


Naučte se vytvářet clusterů HDInsight pomocí .NET SDK. Vytváření jiných clusteru nástrojů a funkcí, a klikněte na kartu horní části této stránky nebo si přečtěte [způsobů vytvoření obrázku](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Požadavky:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Než začnete pokyny v tomto článku, musíte mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Visual Studio 2013 nebo 2015.

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Vytvoření clusterů

HDInsight .NET SDK poskytuje .NET klienta knihoven, které usnadňují práci s HDInsight z aplikace .NET Framework. Postupujte podle těchto pokynů k vytvoření aplikace Visual Studio konzoly a vložte kód pro vytvoření clusteru.

Aplikace vyžaduje Azure skupina a výchozí účet úložiště.  [Dodatek A](#appx-a-create-dependent-components) obsahuje skript Powershellu pro vytvoření závislých součástí.

**Vytvoření aplikace Visual Studio konzoly**

1. Vytvořte novou aplikaci konzoly C# ve Visual Studiu.
2. Spusťte tento příkaz Nuget v konzole Správa balíčku Nuget.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

6. V Průzkumníku poklepejte **Program.cs** otevřela, zkopírujte následující kód a zadejte hodnoty pro proměnné:

        using System;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Net.Http;
        
        namespace CreateHDInsightCluster
        {
            class Program
            {
                // The client for managing HDInsight
                private static HDInsightManagementClient _hdiManagementClient;
                // Replace with your AAD tenant ID if necessary
                private const string TenantId = UserTokenProvider.CommonTenantId; 
                private const string SubscriptionId = "<Your Azure Subscription ID>";
                // This is the GUID for the PowerShell client. Used for interactive logins in this example.
                private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
                private const string ExistingResourceGroupName = "<Azure Resource Group Name>";
                private const string ExistingStorageName = "<Default Storage Account Name>.blob.core.windows.net";
                private const string ExistingStorageKey = "<Default Storage Account Key>";
                private const string ExistingBlobContainer = "<Default Blob Container Name>";
                private const string NewClusterName = "<HDInsight Cluster Name>";
                private const int NewClusterNumWorkerNodes = 2;
                private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
                private const OSType NewClusterOsType = OSType.Windows;
                private const string NewClusterType = "Hadoop";
                private const string NewClusterVersion = "3.2";
                private const string NewClusterUsername = "admin";
                private const string NewClusterPassword = "<HTTP User password>";
                

        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");
        
                    // Authenticate and get a token
                    var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                    // Flag subscription for HDInsight, if it isn't already.
                    EnableHDInsight(authToken);
                    // Get an HDInsight management client
                    _hdiManagementClient = new HDInsightManagementClient(authToken);

                    // Set parameters for the new cluster
                    var parameters = new ClusterCreateParameters
                    {
                        ClusterSizeInNodes = NewClusterNumWorkerNodes,
                        UserName = NewClusterUsername,
                        Password = NewClusterPassword,
                        Location = NewClusterLocation,
                        DefaultStorageAccountName = ExistingStorageName,
                        DefaultStorageAccountKey = ExistingStorageKey,
                        DefaultStorageContainer = ExistingBlobContainer,
                        ClusterType = NewClusterType,
                        OSType = NewClusterOsType
                    };
                    // Create the cluster
                    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

                    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                /// <summary>
                /// Authenticate to an Azure subscription and retrieve an authentication token
                /// </summary>
                /// <param name="TenantId">The AAD tenant ID</param>
                /// <param name="ClientId">The AAD client ID</param>
                /// <param name="SubscriptionId">The Azure subscription ID</param>
                /// <returns></returns>
                static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
                {
                    var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                    var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                        ClientId, 
                        new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                        PromptBehavior.Always, 
                        UserIdentifier.AnyUser);
                    return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
                }
                /// <summary>
                /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
                /// </summary>
                /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
                /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
                /// <param name="authToken">An authentication token for your Azure subscription</param>
                static void EnableHDInsight(TokenCloudCredentials authToken)
                {
                    // Create a client for the Resource manager and set the subscription ID
                    var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                    resourceManagementClient.SubscriptionId = SubscriptionId;
                    // Register the HDInsight provider
                    var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
                }
            }
        }

7. Stisknutím klávesy **F5** spustit aplikaci. Okno konzoly by měl otevřít a zobrazit stav aplikace. Můžete taky výzva k zadání přihlašovacích údajů účet Azure. Může trvat několik minut k vytvoření clusteru HDInsight.



##<a name="next-steps"></a>Další kroky
V tomto článku jste se naučili HDInsight clusteru vytvářet několika různými způsoby. Další informace naleznete v následujících článcích:

- [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – zjistěte, jak začít pracovat s HDInsight obrázku
- [Průběh úloh podregistru HDInsight pomocí .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md)
- [Průběh úloh Prasátko HDInsight pomocí .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md)
- [Průběh úloh Sqoop HDInsight pomocí .NET SDK](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
- [Průběh úloh Oozie HDInsight](hdinsight-use-oozie.md)
- [Následující dokumentaci pro Azure HDInsight SDK]  [ hdinsight-sdk-documentation] -Seznamte se s HDInsight SDK

[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx


##<a name="appx-a-create-dependent-components"></a>Závislé součásti aplikace A vytvořit

Následující Powershellu Azure skript lze použít k vytvoření závislých součástí potřeby tak, že aplikace .NET v tomto kurzu.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
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

    Write-Host "Use the following names in your .NET application" -ForegroundColor Green
    Write-host "Resource Group Name: $resourceGroupName"
    Write-host "Default Storage Account Name: $defaultStorageAccountName"
    Write-host "Default Storage Account Key: $defaultStorageAccountKey"
    Write-host "Default Blob Container Name: $defaultBlobContainerName"
