<properties
   pageTitle="Nasazení VMs NIC více pomocí šablony v správce prostředků | Microsoft Azure"
   description="Naučte se nasadit VMs NIC více pomocí šablony ve Správci zdrojů"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Nasazení VMs NIC více použít šablonu?

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Protože v tomto okamžiku nesmí obsahovat VMs s jednoho NIC a VMs s více nic ve stejné skupině zdroje, provede servery back-end ve skupině zdrojů a jiné součásti v jiné skupině zabezpečení. Postupem použít skupina zdroje s názvem *IaaSStory* skupina hlavní zdroje a *IaaSStory back-end* back-end serverů.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před nasazením servery back-end, budete muset nasazení skupina hlavní zdroje s všechny potřebné prostředky pro tento scénář. Abyste mohli nasadit tyto materiály, postupujte následujícím způsobem.

1. Přejděte na [stránku šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Na stránce šablony vpravo od **nadřazené pole Skupina zdroje**klikněte na **nasazení Azure**.
3. V případě potřeby změňte hodnoty parametrů a pak postupujte podle pokynů v portálu Azure náhled nasazení skupina zdroje.

> [AZURE.IMPORTANT] Zkontrolujte, jestli že jsou jedinečné názvy účtů úložiště. Názvy duplicitních úložiště účtů nesmí obsahovat v Azure.

## <a name="understand-the-deployment-template"></a>Principy šablony nasazení

Před nasazením šablony součástí tohoto si přečtěte následující dokumentaci, zkontrolujte, že víte, jak funguje. Následující kroky představují užitečný přehled předmětné šablony.

1. Přejděte na [stránku šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klikněte na tlačítko **azuredeploy.json** otevřete soubor šablony.
3. Všimněte si parametr *osType* vypsané dole. Tento parametr slouží k výběru OM obrázky můžete u databázovém serveru, spolu s více operační systém související nastavení.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Posuňte se dolů na seznamu proměnných a zkontrolovat definici pro proměnné **dbVMSetting** vypsané dole. Obdrží jednu prvků matici obsažené v **dbVMSettings** proměnné. Pokud máte zkušenosti s terminologií software development, můžete zobrazit proměnnou **dbVMSettings** jako hashtable nebo dictionay.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Předpokládejme, že se rozhodnete nasadit aplikaci SQL back-end VMs Windows. Potom hodnota pro **osType** bude *Windows*a proměnnou **dbVMSetting** by obsahoval elementu uvedených pod ním, který představuje první hodnotu do proměnné **dbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Všimněte si, že **vmSize** obsahuje hodnotu *Standard_DS3*. Jenom určité velikosti OM povolit pro použití více nic. Můžete ověřit, které OM velikosti jsou více NIC povolit tak, že návštěva [více NIC přehled](virtual-networks-multiple-nics.md).
7. Posuňte se dolů na **zdroje** a Všimněte si první prvek. Popisuje účet úložiště. Tento účet úložiště se použijí k udržovat disků dat každý databáze OM používá. V tomto scénáři každou databázi OM má OS disk uložen v pravidelných úložiště a dvě disků dat uložených v úložišti SSD (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Posuňte se dolů na další zdroje podle těchto pokynů. Tento zdroj představuje NIC používat pro přístup k databázi na každou databázi OM. Všimněte si, použijte funkci **Kopírovat** do tohoto zdroje. Šablona umožňuje nasazení tolik VMs požadovaného, na základě parametru **dbCount** . Proto je potřeba vytvořit stejnou velikost nic pro přístup k databázi, jedno pro každé OM.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Posuňte se dolů na další zdroje podle těchto pokynů. Tento zdroj představuje NIC pro správu na každou databázi OM. Ještě jednou budete potřebovat jeden z těchto nic na každou databázi OM. Všimněte si element **networkSecurityGroup** propojení NSG, který umožňuje přístup k RDP/SSH na jenom tento NIC.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Posuňte se dolů na další zdroje podle těchto pokynů. Tento zdroj představuje dostupné nastavit mají být sdíleny všechny VMs databáze. Tímto způsobem záruku, že bude vždy jeden OM v sadě systém údržbě.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Posuňte se dolů na další zdroje. Tento zdroj představuje databázi VMs, jak je vidět v prvním pár řádků vypsané dole. Všimněte si použití funkce **Kopírovat** znovu zajistit, že více VMs vytvořené podle parametr **dbCount** . Všimněte si také kolekci **dependsOn** . Zobrazí se seznam dva nic je nutné vytvořit před nasazením OM, spolu s sadu dostupnost a účtu úložiště.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Přejděte dolů v OM zdroj **networkProfile** element podle těchto pokynů. Všimněte si, že jsou dvě nic se odkaz pro každou OM. Při vytváření více nic pro virtuálního počítače se musí nastavit vlastnost **primární** některého nic *true*a podržte na *hodnotu false*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Nasazení šablony ARM pomocí klikněte na nasazení

> [AZURE.IMPORTANT] Zkontrolujte, že provedení kroků [předpoklady](#Pre-requisites) před níže uvedených pokynů.

Ukázková šablona k dispozici v úložišti veřejné používá parametr soubor, který obsahuje výchozí hodnoty použito k vygenerování scénář jsme je popsali výše. Abyste mohli nasadit tuto šablonu používat klikněte na nasazení, přejděte na [Tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), napravo od **zdrojů back-end skupiny (viz si přečtěte následující dokumentaci)** klikněte **nasazení Azure**, nahraďte výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů na portálu.

Následující obrázek ukazuje obsah nová skupina zdrojů po nasazení.

![Pole Skupina zdroje back-end](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Nasazení šablony pomocí prostředí PowerShell

Abyste mohli nasadit na šablonu, kterou jste si stáhli pomocí prostředí PowerShell, postupujte následujícím způsobem.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Spustit **`New-AzureRmResourceGroup`** rutina vytvořit skupinu zdrojů pomocí šablony.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Očekávaný výstup:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Nasazení šablony pomocí rozhraní příkazového řádku Azure

Nasazení šablony pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Spustit **`azure config mode`** příkaz přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

3. Otevřete [soubor parametr](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), vyberte jeho obsah a si ji uložit do souboru v počítači. V tomto příkladu jsme parametry soubor uložili *parameters.json*.

4. Spustit **`azure group deployment create`** rutina nasazení nové VNet pomocí šablony a parametr soubory můžete stáhnout a kterou nad. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Očekávaný výstup:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
