<properties
   pageTitle="Vytvoření serveru s Windows Hadoop clusterů v používání šablon správce prostředků Azure HDInsight | Microsoft Azure"
    description="Naučte se vytvářet clusterů pro Azure Hdinsightu pomocí Správce prostředků Azure šablon."
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
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Vytvoření serveru s Windows Hadoop clusterů v používání šablon správce prostředků Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Naučte se vytvářet pomocí šablon správce prostředků Azure HDInsight clusterů. Další informace najdete v tématu [nasazení aplikace šablonou správce prostředků Azure](../resource-group-template-deploy.md). Vytváření jiných clusteru nástrojů a funkcí, a klikněte na kartu horní části této stránky nebo si přečtěte [způsobů vytvoření obrázku](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Požadavky:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Než začnete pokyny v tomto článku, musíte mít takto:

- [Azure předplatného](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure prostředí PowerShell nebo Azure rozhraní příkazového řádku

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Správce prostředků šablony

Správce prostředků šablony usnadňuje vytváření HDInsight clusterů, jejich závislé prostředky (například výchozí úložiště účet) a další zdroje (například databáze SQL Azure používat Apache Sqoop) aplikace v operaci jediné, koordinovaný. V šabloně definujte zdroje, které jsou potřebné pro aplikace a zadejte parametry nasazení zadat hodnoty na jiném prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnoty pro nasazení.

Správce prostředků šablony pro vytváření HDInsight obrázku a závislá účet Azure úložiště naleznete v [Dodatku A](#appx-a-arm-template). Pomocí textového editoru šablonu uložte do souboru na počítači. Se dozvíte, jak volání šablony pomocí různých nástrojů.

Další informace o šabloně správce prostředků najdete v tématu

- [Autor správce prostředků Azure šablony](../resource-group-authoring-templates.md)
- [Nasazení aplikace šablonou správce prostředků Azure](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Nasazení pomocí prostředí PowerShell

Následující postup vytvoří HDInsight obrázku.

**Chcete-li nasadit clusteru pomocí šablony správce prostředků**

1. Uložte soubor json v [dodatku A](#appx-a-arm-template) na počítači.
2. Nastavení parametrů v případě potřeby.
3. Spusťte šablony pomocí tento skript Powershellu:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
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

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    Skript Powershellu pouze nakonfiguruje clusteru název a název účtu úložiště.  Nastavení dalších hodnot v šabloně správce prostředků. 
    
Další informace najdete v tématu [nasazení pomocí prostředí PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Nasazení s Azure rozhraní příkazového řádku

Následujícím příkladu je vytvořeno clusteru a jeho závislá úložiště účtu a kontejneru tak, že zavoláte správce prostředků šablony:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Nasazení pomocí rozhraní REST API

V tématu [nasazení pomocí rozhraní REST API](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Nasazení aplikace Visual Studio

Pomocí aplikace Visual Studio můžete vytvořit projekt skupiny zdrojů a ho nasadit do Azure prostřednictvím uživatelského rozhraní. Vyberte typ zdroje, chcete-li vložit do projektu a zdroje se automaticky přidají do šablony správce prostředků. Projekt také poskytuje skript Powershellu pro nasazení šablony.

Úvod do používání aplikace Visual Studio se skupinami zdrojů najdete v článku [Vytvoření a nasazení Azure zdroje skupiny pomocí aplikace Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Další kroky
V tomto článku jste se naučili HDInsight clusteru vytvářet několika různými způsoby. Další informace naleznete v následujících článcích:


- Příklad nasazení prostředků přes klienta knihovnu .NET najdete v článku [nasazení zdrojů pomocí knihoven .NET a šablony](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Příklad hloubkovou nasazení aplikace v tématu [poskytování a nasazení microservices předvídatelností v Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Nasazení řešení různých prostředích, najdete v článku [test prostředí v Microsoft Azure a vývoj](../solution-dev-test-environments.md).
- Další informace o v částech šablony správce prostředků Azure, najdete v článku [vytváření šablon](../resource-group-authoring-templates.md).
- Seznam funkce, které můžete použít v šabloně aplikace Správce prostředků Azure najdete v tématu [funkce šablony](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Správce prostředků aplikace A: šablony

Následující šablonu správce prostředků Azure vytvoří cluster serveru s Windows Hadoop účtem závislá Azure úložiště.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

