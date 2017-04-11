<properties
   pageTitle="Jak vytvořit NSGs v režimu ARM pomocí šablony | Microsoft Azure"
   description="Naučte se vytvářet a publikovat NSGs v ARM použít šablonu?"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>Jak vytvořit NSGs použít šablonu?

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [vytvořit NSGs v modelu klasické nasazení](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG zdrojů v souboru šablony

Můžete zobrazit a stáhnout [ukázkové šablony](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

Oddíl níž znázorňuje definici front-end NSG, podle výše uvedených scénáře.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Chcete-li přidružit NSG podsítě front-end, budete muset změnit definici podsítě v šabloně a použít id odkazu NSG.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Všimněte si stejné prováděné back-end NSG a back-end podsítě v šabloně.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Nasazení šablony ARM pomocí klikněte na nasazení

Ukázková šablona k dispozici v úložišti veřejné používá parametr soubor, který obsahuje výchozí hodnoty použito k vygenerování scénář jsme je popsali výše. Nasazení tuto šablonu používat klikněte na nasazení, přejděte na [Tento odkaz](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klikněte na **Deploy Azure**, nahraďte výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů na portálu.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Nasazení šablony ARM pomocí prostředí PowerShell

Abyste mohli nasadit ARM šablonu, kterou jste si stáhli pomocí prostředí PowerShell, postupujte následujícím způsobem.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.

3. Spustit **`New-AzureRmResourceGroup`** rutina vytvořit skupinu zdrojů pomocí šablony.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Očekávaný výstup:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure

Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Spustit **`azure config mode`** příkaz přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

4. Spustit **`azure group deployment create`** rutina nasazení nové VNet pomocí šablony a parametr soubory můžete stáhnout a kterou nad. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Očekávaný výstup:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **n (nebo – název)**. Název skupiny prostředků vytvořit.
    - **-l (nebo – umístění)**. Azure oblast, kde se bude vytvořena skupina zdroje.
    - **-f (nebo – soubor šablony)**. Cesta k souboru šablony ARM.
    - **-e (nebo – souboru parametrů)**. Cesta k souboru ARM parametry.
