<properties
    pageTitle="Přizpůsobení clusterů HDInsight pomocí skriptu akce | Microsoft Azure"
    description="Zjistěte, jak přizpůsobit clusterů HDInsight pomocí skriptu akce."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Přizpůsobení clusterů serveru s Windows HDInsight pomocí skriptu akce

**Akci skriptu** mohou sloužit k vyvolat [vlastních skriptů](hdinsight-hadoop-script-actions.md) během procesu vytváření clusteru při instalaci další software clusteru.

Informace v tomto článku jsou specifické pro clusterů serveru s Windows HDInsight. Na základě Linux clusterů najdete v článku [clusterů na základě přizpůsobení Linux HDInsight pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight clusterů lze přizpůsobit v různých další způsoby, například včetně dalších účtů Azure úložiště, změna Hadoop konfigurace soubory (základní site.xml, podregistru site.xml atd.) nebo přidání sdílené knihovny (například podregistru, Oozie) do běžné umístění v clusteru. Vlastní nastavení lze provést pomocí prostředí PowerShell Azure, Azure HDInsight .NET SDK nebo portálu Azure. Další informace najdete v tématu [Vytvoření Hadoop clusterů HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skript akce v proces vytváření obrázku

Skript akce se používá pouze během clusterů probíhá vzniku. Následující obrázek znázorňuje při spuštění akce skript během s vytvářením:

![Přizpůsobení clusteru HDInsight a dílčí fáze při vytváření obrázku][img-hdi-cluster-states]

Při spuštění skript clusteru slouží k vložení fázi **ClusterCustomization** . V této fázi skript běží pod účtem správce systému současně ve všech zadaný uzlech clusteru a nabízí celou správy oprávnění v jednotlivých uzlech.

> [AZURE.NOTE] Protože nemáte oprávnění správce uzlech clusteru fázi **ClusterCustomization** , můžete k provádění operací, jako je krátkou zprávu a od služby, třeba služby související Hadoop skript. Ano, v rámci skript musíte se ujistit, že služby Ambari a jiných služeb souvisí Hadoop jsou do začátků před skript dokončení. Tyto služby musí úspěšně zjištění stavu a stav clusteru je při vytváření. Pokud změníte jakékoli konfigurace clusteru, která má vliv na těchto služeb, musí používat funkce Pomocník, které jsou k dispozici. Další informace o funkcích pomocníka najdete v článku [skriptů vyvíjet akci skriptu pro HDInsight][hdinsight-write-script].

Výstup a protokolů chyb u skript jsou uložené ve výchozí účet úložiště, které jste zadali clusteru. Protokoly jsou uložené v tabulce s názvem **u < \cluster-name-fragment >< \time-stamp > setuplog**. Toto jsou agregační protokoly z skriptu spustit ve všech uzlech (hlavního uzlu a pracovní uzly) v clusteru.

Každý cluster přijatelné více akcí skript vyvolané v pořadí, ve které byla vyplněná. Skript můžete spustili na uzel hlavy a pracovníka uzlů.

HDInsight obsahuje několik skriptů HDInsight clusterů nainstalovat tyto prvky:

Jméno | Skript
----- | -----
**Instalace Spark** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. V tématu [instalace a použití podnítit na HDInsight clusterů][hdinsight-install-spark].
**Instalace R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Přečtěte si téma [instalace a používání R na HDInsight clusterů][hdinsight-install-r].
**Instalace Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. V tématu [instalace a použití clusterů Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalace Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. V tématu [instalace a použití clusterů Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).
| **Předem načíst podregistru knihovny** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Přečtěte si článek [Přidání podregistru knihoven na HDInsight clusterů](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Volání skriptů pomocí portálu Azure

**Z portálu Microsoft Azure**

1. Zahájení vytváření clusteru, jak je uvedeno na stránce [vytvořit Hadoop clusterů HDInsight](hdinsight-provision-clusters.md#portal).
2. Pod volitelná konfigurace pro zásuvné **Skript akce** klikněte na **Přidat akci skriptu** k poskytování údajů o akci skript, jak je ukázáno v následujícím příkladu:

    ![Použití akce skript k přizpůsobení clusteru] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Použití akce skript k přizpůsobení clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Jméno</td>
            <td>Zadejte název akce skriptu.</td></tr>
        <tr><td>Skript URI</td>
            <td>Zadejte identifikátor URI skript, který je zavolat a přizpůsobení clusteru. s</td></tr>
        <tr><td>Vedoucí/pracovníka</td>
            <td>Určete uzly (**vedoucího** nebo **pracovníka**) na skript přizpůsobení je spouštěn. </b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry, v případě potřeby skriptem.</td></tr>
    </table>

    Stisknutím klávesy ENTER přidejte více akci skriptu pro instalaci více součástí na clusteru.

3. Klikněte na **Vybrat** uložte konfigurace akcí skriptu a budeme pokračovat v vytváření clusteru.

## <a name="call-scripts-using-azure-powershell"></a>Volání skriptů pomocí prostředí PowerShell Azure

Tento skript Powershellu ukazuje, jak nainstalovat Spark na základě clusteru HDInsight systému Windows.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Pokud chcete nainstalovat jiného softwaru, budete potřebovat k nahrazení soubor skriptu v skript:


Po zobrazení výzvy zadejte přihlašovací údaje pro clusteru. Může trvat několik minut, než bude vytvořena clusteru.

## <a name="call-scripts-using-net-sdk"></a>Volání skriptů pomocí .NET SDK 

Následující příklad ukazuje, jak nainstalovat Spark na základě clusteru HDInsight systému Windows. Pokud chcete nainstalovat jiného softwaru, musíte se nahradit soubor skriptu v kódu.

**K vytvoření clusteru HDInsight s Spark** 

1. Vytvoření aplikace konzoly C# ve Visual Studiu.
2. Z konzoly Správce balíčků Nuget, spusťte tento příkaz.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Můžete použít následující pomocí příkazů v souboru Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Kód umístíte ve třídě s takto:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
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


4. Stisknutím klávesy **F5** spustit aplikaci.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Podpora pro otevřít zdroj software byl použit na HDInsight clusterů
Služba Microsoft Azure HDInsight je flexibilní platformy, který umožňuje vytvářet velký dat aplikace v cloudu pomocí ekosystém otevřít zdroj technologií vytvořené kolem Hadoop. Microsoft Azure obsahuje obecné úroveň podpory technologie otevřít zdroj, jak je popsáno v části **Podpora obor** <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure podporují nejčastější dotazy týkající se webu</a>. HDInsight služba poskytuje další úroveň podpory pro některé prvky, jak je popsáno níže.

Existují dva typy součástí otevřít zdroje, které jsou k dispozici ve službě HDInsight:

- **Předdefinované prvky** - tyto součásti jsou předinstalované ve HDInsight clusterů a poskytují základní funkce clusteru. Například vláken ResourceManager podregistru dotazovací jazyk (HiveQL) a knihovně Mahout patří této kategorii. Úplný seznam součástí clusteru je k dispozici v [Co je nového v verze obrázku Hadoop poskytovanou HDInsight?](hdinsight-component-versioning.md) </a>.
- **Vlastní součásti** -, jako uživatel clusteru, můžete nainstalovat nebo použití ve vaší pracovní zátěž všechny komponenty k dispozici v rámci komunity nebo vytvoření vy.

Předdefinované prvky jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.

> [AZURE.WARNING] Součásti součástí clusteru HDInsight jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.
>
> Vlastní součásti dostávat komerčně rozumné podpory pomáhají při další řešení problému. To může mít tento problém vyřešit nebo s žádostí o zapojit dostupných kanálů technologie otevřít zdroj, kde se nacházejí hloubkové odborných informací, které technologie. Například existuje mnoho webů komunity, které lze použít, třeba: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mít taky webů projektů na [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [jiskrou](http://spark.apache.org/).

Služba HDInsight nabízí několik způsobů použití vlastních součástí. Bez ohledu na to, jak je používat komponentu nebo se nainstalovaným clusteru platí stejné úrovni podpory. Tady je přehled nejčastěji vlastní komponent lze na HDInsight clusterů:

1. Odeslání úlohy - Hadoop a dalších typů úloh, které spustit komponenty ani používat vlastní lze odeslat do clusteru.
2. Vlastní nastavení obrázku – během vytváření clusteru, můžete zadat další nastavení a vlastní součásti nainstalované uzlech clusteru.
3. Ukázky - Oblíbené vlastní součásti, Microsoft a ostatní stanovit příklady použití těchto složek ve HDInsight. Tyto příklady jsou k dispozici bez podpory.

## <a name="develop-script-action-scripts"></a>Můžete vyvíjet skripty akci skriptu

V tématu [skriptů vyvíjet akci skriptu pro HDInsight][hdinsight-write-script].


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
