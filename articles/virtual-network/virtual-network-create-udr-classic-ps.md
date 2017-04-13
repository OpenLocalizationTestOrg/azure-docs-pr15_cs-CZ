<properties 
   pageTitle="Ovládací prvek směrování a používat virtuální spotřebiče pomocí Powershellu v modelu klasické nasazení | Microsoft Azure"
   description="Zjistěte, jak určit směrování VNets pomocí Powershellu v modelu klasické nasazení"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Ovládací prvek směrování a jejich použití virtuální spotřebiče (klasický) pomocí prostředí PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Vzorku Azure PowerShell příkazy dole očekávat jednoduché prostředí vytvořen podle výše uvedených scénář. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, vytvořte prostředí znázorněno [vytváření VNet (klasické) pomocí Powershellu](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Vytvoření UDR podsítě front-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě front-end podle výše uvedených scénář, postupujte následujícím způsobem.

3. Spustit **`New-AzureRouteTable`** rutina chcete vytvořit tabulku směrování podsítě front-end.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Výstup:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Spustit **`Set-AzureRoute`** rutina k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené do back-end podsítě (192.168.2.0/24) **FW1** OM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Výstup:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Spustit **`Set-AzureSubnetRouteTable`** rutiny, aby přidružení směrovací tabulky vytvořené nad s podsítí **FrontEnd** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Vytvoření UDR podsítě back-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě back-end podle výše uvedených scénáře, postupujte následujícím způsobem.

3. Spustit **`New-AzureRouteTable`** rutina chcete vytvořit tabulku směrování podsítě back-end.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Spustit **`Set-AzureRoute`** rutina k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené front-end připravenost (192.168.1.0/24) **FW1** OM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Spustit **`Set-AzureSubnetRouteTable`** rutiny, aby přidružení směrovací tabulky vytvořené nad s podsítí **back-end** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Povolit přesměrování IP na FW1 OM
Chcete-li povolit přesměrování v OM FW1 IP, postupujte následujícím způsobem.

1. Spustit **`Get-AzureIPForwarding`** rutiny, aby kontrola stavu IP přesměrování

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Výstup:

        Disabled

2. Spustit **`Set-AzureIPForwarding`** příkaz Povolit přesměrování IP pro *FW1* OM.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
