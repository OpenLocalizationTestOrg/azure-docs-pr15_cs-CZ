<properties
    pageTitle="Vytvoření virtuálního počítače se šablonou správce prostředků | Microsoft Azure"
    description="Použijte Správce prostředků šablonu a prostředí PowerShell můžete snadno vytvářet nové virtuálního počítače Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Vytvoření virtuálního počítače Windows se šablonou správce prostředků

Tento článek vás seznámí s šablony pro správce prostředků Azure a uvidíte, jak pomocí Powershellu ho nasadit. Šablona nasadí jednoho virtuálního počítače v síti nové virtuální pomocí jednoho podsítě serverem Windows.

By měl trvá asi o 20 minut kroků v tomto článku.

> [AZURE.IMPORTANT] Pokud budete potřebovat OM jako součást sady dostupnost, ho přidáte do sady při vytváření OM. Momentálně není k dispozici způsob, jak přidat virtuálního počítače dostupné po byl vytvořen.

## <a name="step-1-create-the-template-file"></a>Krok 1: Vytvoření šablony souboru

Můžete vytvořit vlastní šablony pomocí informací v [šablon pro vytváření správce prostředků Azure](../resource-group-authoring-templates.md). Můžete taky nasadit šablony, které byly vytvořeny za vás z [Azure Rychlé prohlídky šablon](https://azure.microsoft.com/documentation/templates/).

1. Otevřete svůj oblíbený textovém editoru a přidejte požadované schéma a část požadované contentVersion:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parametry](../resource-group-authoring-templates.md#parameters) vždy nejsou povinná, ale poskytují způsob, jak vstupní hodnoty při zavedení šabloně. Přidání prvku parametry a podřízené elementy po contentVersion element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Proměnné](../resource-group-authoring-templates.md#variables) lze do šablony zadejte hodnoty, které se mohou často měnit nebo hodnoty, které je potřeba vytvořit z kombinací hodnoty parametrů. Přidání prvku proměnné pod oddílem parametry:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Zdroje](../resource-group-authoring-templates.md#resources) , jako jsou virtuální počítač, virtuální sítě a úložiště účtu se definují další šablony. Přidání části zdroje pod oddílem proměnné:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Tento článek vytvoří virtuální počítač s verzí operačního systému Windows Server. Další informace o výběru dalších obrázků najdete v tématu [Navigace a obrázky vyberte Azure virtuální počítač s Windows Powershellu a rozhraní příkazového řádku Azure](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Uložte soubor šablony jako *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Krok 2: Vytvoření souboru parametrů

Chcete-li zadat hodnoty parametrů zdroje, které byly definované v šabloně, vytvořte parametry soubor, který obsahuje hodnoty, které se používají při nasazení šablony.

1. V textovém editoru zkopírujte tento obsah JSON na nový soubor s názvem *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Další informace o [požadavcích na uživatelské jméno a heslo](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Uložte soubor parametry.

## <a name="step-3-install-azure-powershell"></a>Krok 3: Instalace modulu Azure PowerShell

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="step-4-create-a-resource-group"></a>Krok 4: Vytvoření skupiny zdrojů

Všechny zdroje musí být nasazené ve [pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md).

1. Získání seznamu dostupná umístění, kde se dají vytvářet zdroje.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Nahraďte hodnotu **$locName** umístění v seznamu, například **Centrální USA**. Vytvoření proměnné.

        $locName = "location name"
        
3. Nahraďte hodnotu **$rgName** název nové skupiny prostředků. Vytvoření proměnné a skupina zdroje.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Měli byste vidět vypadá podobně jako tento příklad:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Krok 5: Vytvoření zdrojů pomocí šablony a parametrech

Nahraďte hodnotu **$templateFile** cesty a názvu souboru šablony. Nahraďte hodnotu **$parameterFile** cestu a název souboru s parametry. Vytvořte proměnné a pak nasazení šablony. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Můžete taky nasadit šablony a parametrů s klientem Azure úložiště. Další informace najdete v tématu [použití Azure s úložištěm Azure](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Další kroky

- Pokud došlo k problémy s nasazení, dalším krokem bude visiové [nasazení skupina zdroje Poradce při potížích s Azure portálu](../resource-manager-troubleshoot-deployments-portal.md)
- Naučte se spravovat virtuální počítač, který jste vytvořili kontrolou [Spravovat virtuálních počítačích pomocí Správce prostředků Azure a Powershellu](virtual-machines-windows-ps-manage.md).
