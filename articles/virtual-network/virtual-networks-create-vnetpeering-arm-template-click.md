<properties
   pageTitle="Vytvoření VNet prozkoumávání pomocí šablon správce prostředků | Microsoft Azure"
   description="Zjistěte, jak vytvořit virtuální sítě prozkoumávání použití šablon v správce prostředků."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Vytvoření VNet prozkoumávání pomocí Správce prostředků šablon

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit VNet prozkoumávání pomocí šablon správce prostředků, postupujte následujícím způsobem:

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.

    > [AZURE.NOTE] Rutiny prostředí PowerShell pro správu VNet prozkoumávání je součástí [Azure prostředí PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Následující text zobrazí definici peering odkaz VNet VNet1 VNet2 podle výše uvedených scénáře. Zkopírujte následující obsah a uložte jej do souboru nazvaného VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. V části zobrazí definici peering odkaz VNet VNet2 VNet1 podle výše uvedených scénáře.  Zkopírujte následující obsah a uložte jej do souboru nazvaného VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Jak je vidět v šabloně výše uvedené, nejsou několik konfigurovat vlastnosti prozkoumávání VNet:

  	|Možnost|Popis|Výchozí|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Zda je součástí značku virtual_network adresní prostor mezi dvěma účastníky VNet.|Ano|
  	|AllowForwardedTraffic|Zda je přenos není pocházející z peered VNet přijaté nebo nezobrazí.|Ne|
  	|AllowGatewayTransit|Umožňuje VNet používat bránu VNet mezi dvěma účastníky.|Ne|
  	|UseRemoteGateways|Použijte bránu VNet vaše mezi dvěma účastníky. Mezi dvěma účastníky VNet musí mít brány nakonfigurované a AllowGatewayTransit vybrané. Nelze použít tuto možnost, pokud máte nakonfigurované brány.|Ne|

    Každý odkaz v VNet prozkoumávání obsahuje sadu vlastností výše. Můžete třeba nastavit AllowVirtualNetworkAccess True VNet odkazu na prozkoumávání VNet1 VNet2 a nastavena na hodnotu False peering odkazu VNet opačným směrem.


4. Abyste mohli nasadit soubor šablony, můžete použít rutinu New-AzureRmResourceGroupDeployment vytvoření nebo aktualizace nasazení. Další informace o použití šablon správce prostředků naleznete v tomto [článku](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Nahraďte prosím zdroje skupinu název šablony souboru a podle potřeby.

    Níže je příklad podle výše scénář:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Výstup zobrazuje:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Výstup zobrazuje:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Po dokončení nasazení, můžete spustit dole a zobrazte peering stav rutinu:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Výstup zobrazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Po prozkoumávání v tomto scénáři, je třeba moct zahájit připojení z libovolné virtuálního počítače do libovolné virtuálního počítače v obou VNets. Ve výchozím nastavení AllowVirtualNetworkAccess je PRAVDA a VNet prozkoumávání bude zřízení správné ACL povolit komunikaci mezi VNets. Můžete také použít pravidla skupiny (NSG) zabezpečení sítě blokovat připojení mezi konkrétní podsítě nebo virtuálních počítačích získat kontrolu jemnou přístupu mezi dvěma virtuální sítě.  Další informace o vytváření pravidel NSG naleznete v tomto [článku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Pokud chcete vytvořit VNet prozkoumávání u předplatných, postupujte následujícím způsobem:

1. Přihlaste se k Azure s oprávněními uživatele A na účtu v předplatné A a spusťte tuto rutinu:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Toto není povinné, prozkoumávání lze vytvořit i v případě, že uživatelé jednotlivě posunout výš, prozkoumávání požadavky pro jejich odpovídajících Vnets dlouhou, jak shodují žádosti. Přidání privilegovaných uživatelů VNet jako uživatele v místním VNet usnadňuje proveďte nastavení.

2. Přihlaste se k Azure účtem privilegovaných uživatelů-B pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. V prvku uživatele A přihlašovací relace, spusťte tuto rutinu:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Tady najdete, jak je definováno souboru JSON.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. V relaci přihlášení uživatele-B spusťte následující rutinu:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Tady je způsob je definován JSON souboru:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Po prozkoumávání v tomto scénáři, je třeba moct zahájit připojení z libovolné virtuálního počítače do libovolné virtuálního počítače obou VNets u různých předplatných.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. V tomto scénáři nástroje můžete nasazovat Ukázková šablona níže stanovit prozkoumávání VNet.  Musíte nastavit vlastnost AllowForwardedTraffic logickou hodnotu PRAVDA, umožňující virtuální zařízení sítě v peered VNet odesílat a přijímat vysílání.

    Tady je šablona pro vytváření VNet prozkoumávání z HubVNet VNet1. Všimněte si, že AllowForwardedTraffic je nastavený na hodnotu false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Tady je šablona pro vytváření VNet prozkoumávání z VNet1 HubVnet. Všimněte si, že AllowForwardedTraffic nastaveno true (pravda).

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Po vytvoření prozkoumávání můžete odkázat na tento [článek](virtual-network-create-udr-arm-ps.md) definovat routes(UDR) kterým přesměrujete přenosy VNet1 prostřednictvím virtuální zařízení na používání jeho funkcí definovaných uživatelem. Když zadáte adresu dalšího směrování postupu, nastavíte ho k IP adrese virtuální zařízení v VNet HubVNet mezi dvěma účastníky.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Pokud chcete vytvořit prozkoumávání mezi virtuálních sítí z různých nasazení modelů, postupujte následujícím způsobem:
1. Následující text zobrazí definici peering odkaz VNet VNET1 VNET2 v tomto scénáři. Pouze jeden odkaz je potřeba k druhé strany klasické virtuální sítě k síti virtuální správce Azure zdroje.

    Ujistěte se, vložte do ID předplatného, pro které je umístěn na klasické virtuální sítě nebo VNET2 a změňte MyResouceGroup na název skupiny příslušné zdroje.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametry": {}, "proměnné": {}, "zdroje": [{"apiVersion": "2016-06-01", "typ": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "název": "VNET1/LinkToVNET2", "umístění": "[resourceGroup () .location]", "vlastnosti": {"allowVirtualNetworkAccess": true "allowForwardedTraffic": false, "allowGatewayTransit": false, "useRemoteGateways": false, "remoteVirtualNetwork": {"identifikátor": "[resourceId (" Microsoft.ClassicNetwork/virtualNetworks', "VNET2")] "}}}]}

2. Abyste mohli nasadit soubor šablony, spusťte následující rutinu k vytvoření nebo aktualizace nasazení.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Po úspěšném nasazení můžete spustit následující rutinu peering stavu zobrazení:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Po prozkoumávání mezi klasické VNet a správce prostředků VNet, je třeba moct zahájit připojení z libovolné virtuálního počítače v VNET1 do libovolné virtuálního počítače VNET2 a naopak.
