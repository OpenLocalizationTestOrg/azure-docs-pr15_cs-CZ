<properties
   pageTitle="Nasazení OM statické veřejnou IP pomocí šablony v správce prostředků | Microsoft Azure"
   description="Naučte se nasadit VMs statické veřejnou IP pomocí šablony ve Správci zdrojů"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Nasazení OM pomocí statické IP veřejné použít šablonu?

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Veřejné zdroje IP v souboru šablony

Můžete zobrazit a stáhnout [ukázkové šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Oddíl níž znázorňuje definici veřejné zdroje IP podle výše uvedených scénáře.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Všimněte si **publicIPAllocationMethod** vlastnost, která je nastavený na *statický*. Tato vlastnost může být *dynamické* (výchozí hodnota) nebo *statické*. Nastavení na statický záruky, které veřejnou IP adresu přiřazené nezmění.

V části zobrazuje přidružení veřejnou IP adresu rozhraní sítě.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Všimněte si vlastnost **publicIPAddress** přejdete **Id** zdroje s názvem **variables('webVMSetting').pipName**. Se jmenuje veřejné zdroje IP nahoře.

Nakonec rozhraní sítě je uvedený ve vlastnosti **networkProfile** OM vzniku.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Nasazení šablony pomocí klikněte na nasazení

Ukázková šablona k dispozici v úložišti veřejné používá parametr soubor, který obsahuje výchozí hodnoty použito k vygenerování scénář jsme je popsali výše. Abyste mohli nasadit tuto šablonu používat klikněte na nasazení, klikněte na **nasazení Azure** v souboru Readme.md [OM s statické PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) šablony. Nahraďte výchozí hodnoty parametrů v případě potřeby a zadejte hodnoty prázdné parametry.  Postupujte podle pokynů v portálu vytvořit virtuální počítač s statické veřejnou IP adresu.

## <a name="deploy-the-template-by-using-powershell"></a>Nasazení šablony pomocí prostředí PowerShell

Abyste mohli nasadit na šablonu, kterou jste si stáhli pomocí prostředí PowerShell, postupujte následujícím způsobem.

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) a postupujte podle kroků 1 až 3 v.

2. V konzole prostředí PowerShell spusťte rutinu **New-AzureRmResourceGroup** k vytvoření nové skupiny prostředků, pokud je to potřeba. Pokud už máte skupina zdroje vytvořili, přejděte ke kroku 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Očekávaný výstup:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. V konzole prostředí PowerShell spusťte rutinu **New-AzureRmResourceGroupDeployment** zavést šablonu.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Očekávaný výstup:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Nasazení šablony pomocí rozhraní příkazového řádku Azure

Nasazení šablony pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud jste použili nikdy Azure rozhraní příkazového řádku, postupujte podle kroků v článku [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a potom kroků pro připojení rozhraní příkazového řádku do vašeho předplatného v části "Použít azure přihlášení k ověření interaktivně" v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-connect.md) .
2. Příkaz **azure konfigurace režim** přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

3. Otevřete [soubor parametr](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), vyberte její obsah a si ji uložit do souboru v počítači. V tomto příkladu parametry uloží do souboru nazvaného *parameters.json*. Změňte hodnoty parametrů v souboru, pokud budete chtít, ale minimálně, doporučujeme změňte hodnotu parametru adminPassword na jedinečný a složitý heslo.

4. Spusťte rutinu **azure skupiny nasazení vytvořit** nasazení nové VNet pomocí šablony a parametr soubory můžete stáhnout a kterou nad. V tomto příkazu nahraďte <path> s cestou jste soubor uložili, a. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Očekávaný výstup (seznamy hodnoty parametrů použité):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
