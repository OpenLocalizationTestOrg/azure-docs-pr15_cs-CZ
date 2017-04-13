<properties
   pageTitle="Vytvoření VNet prozkoumávání pomocí rutin prostředí Powershell | Microsoft Azure"
   description="Zjistěte, jak vytvořit virtuální síť na portálu Azure správce prostředků."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
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
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Vytvoření VNet prozkoumávání pomocí rutin prostředí Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit VNet prozkoumávání pomocí prostředí PowerShell, postupujte následujícím způsobem:

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.

> [AZURE.NOTE] Rutiny prostředí PowerShell pro správu VNet prozkoumávání je součástí [Azure prostředí PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Čtení objektů virtuální sítě:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Stanovit prozkoumávání VNet je potřeba vytvořit dva odkazy, jedno pro každé směru. Podle následujících pokynů VNet peering odkaz pro vytvoření VNet1 VNet2 nejdřív:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Výstup zobrazuje:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Tento krok pro VNet2 VNet1 vytvoření VNet peering odkaz:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Výstup zobrazuje:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Po vytvoření VNet peering odkaz se zobrazí stav odkaz:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Výstup zobrazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Existuje několik nastavitelné vlastnosti prozkoumávání VNet:

  	|Možnost|Popis|Výchozí|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Zda adresy mezery mezi dvěma účastníky VNet být součástí značku Virtual_network|Ano|
  	|AllowForwardedTraffic|Zda je přenos není pocházející z peered VNet přijaté nebo nezobrazí|Ne|
  	|AllowGatewayTransit|Umožňuje VNet používat bránu VNet mezi dvěma účastníky|Ne|
  	|UseRemoteGateways|Použijte bránu VNet vaše mezi dvěma účastníky. Mezi dvěma účastníky VNet musí mít brány nakonfigurované a AllowGatewayTransit vybrané. Nelze použít tuto možnost, pokud máte nakonfigurované brány|Ne|

    Každý odkaz v VNet prozkoumávání obsahuje sadu vlastností výše. Můžete třeba nastavit AllowVirtualNetworkAccess True VNet odkazu na prozkoumávání VNet1 VNet2 a nastavena na hodnotu False peering odkazu VNet opačným směrem.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Můžete spustit Get-AzureRmVirtualNetworkPeering dvojité zaškrtnutí hodnotu po změně. Z výstupu zobrazí se AllowForwardedTraffic změny nastavit na hodnotu True po spuštění výše rutiny.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Po prozkoumávání v tomto scénáři, je třeba moct zahájit připojení z libovolné virtuálního počítače do libovolné virtuálního počítače obou VNets. Ve výchozím nastavení AllowVirtualNetworkAccess je PRAVDA a VNet prozkoumávání bude zřízení správné ACL povolit komunikaci mezi VNets. Můžete také použít pravidla skupiny (NSG) zabezpečení sítě blokovat připojení mezi konkrétní podsítě nebo virtuálních počítačích získat kontrolu pořádku zrno přístupu mezi dvěma virtuální sítě.  Další informace o vytváření pravidel NSG naleznete v tomto [článku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Pokud chcete vytvořit VNet prozkoumávání u předplatných pomocí Powershellu, postupujte následujícím způsobem:

1. Přihlaste se k Azure s oprávněními uživatele A je účtu u předplatného A a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Toto není povinné, prozkoumávání lze vytvořit i v případě, že uživatelé jednotlivě posunout výš, prozkoumávání požadavky pro jejich odpovídajících VNets dlouhou, jak shodují žádosti. Přidání privilegovaných uživatelů VNet jako uživatele v místním VNet usnadňuje proveďte nastavení.

2. Přihlaste se k Azure účtem privilegovaných uživatelů-B pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. V prvku uživatele A přihlašovací relace, spusťte následující rutinu:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. V relaci přihlášení uživatele-B spusťte následující rutinu:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Po prozkoumávání, třeba všechny virtuálního počítače v VNet3 komunikovat s všechny virtuálního počítače v VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. V tomto scénáři spuštěním rutiny prostředí PowerShell dole stanovit prozkoumávání VNet.  Budete muset nastavit vlastnost AllowForwardedTraffic true (pravda) a propojit VNET1 HubVNet, které umožňuje příchozích z mimo peering adresní prostor VNet.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Po vytvoření prozkoumávání můžete naleznete v tomto [článku](virtual-network-create-udr-arm-ps.md) a definici cesty definované uživatelem (UDR), kterým přesměrujete přenosy VNet1 prostřednictvím virtuální zařízení na používání jeho funkcí. Když zadáte adresu dalšího směrování v postupu, nastavíte ho k IP adrese virtuální zařízení v VNet HubVNet mezi dvěma účastníky. Tady je ukázka:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Pokud chcete vytvořit VNet prozkoumávání mezi klasické virtuální sítě a virtuální síť správce prostředků Azure v prostředí PowerShell, postupujte následujícím způsobem:

1. Co najdete na objekt virtuální sítě za **VNET1**, virtuální sítě Správce prostředků Azure následujícím způsobem:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Stanovit prozkoumávání v tomto scénáři VNet není potřeba pouze jeden odkaz, konkrétně odkaz z **VNET1** **VNET2**. Tento krok vyžaduje znalost ID klasické VNet zdroje. Formát ID skupiny zdroje vypadá takto:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Ujistěte se, že nahradit SubscriptionID ResourceGroupName a VirtualNetworkName odpovídající názvy.

    Můžete to provést takto:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Jednou VNet prozkoumávání odkaz se vytvoří uvidíte stavu připojení uvedeno v výstupu níže:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Odebrání prozkoumávání VNet

1.  Pokud chcete odebrat VNet prozkoumávání, je potřeba spustit následující rutinu:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Po odebrání jeden odkaz v VNET prozkoumávání stavu připojení mezi dvěma účastníky budou moct odpojeno. V tomto stavu nelze znovu vytvořit odkaz, tak, aby stavu připojení mezi dvěma účastníky změnil zahájil. Doporučujeme, abyste že před znovu vytvořit VNet prozkoumávání odebrat odkazy.
