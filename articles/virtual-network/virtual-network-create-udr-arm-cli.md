<properties 
   pageTitle="Ovládací prvek směrování a správce virtuální zařízení v zdrojů pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Zjistěte, jak určit směrování a používat virtuální spotřebiče pomocí rozhraní příkazového řádku Azure"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Vytvoření vlastní cesty (UDR) v Azure rozhraní příkazového řádku

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [vytvořit UDRs v modelu klasické nasazení](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Ukázkové příkazy Azure rozhraní příkazového řádku pod očekávat jednoduché prostředí vytvořen podle výše uvedených scénáře. Pokud budete chtít spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejdřív vytvořit testovacím prostředí nasazením [tuto šablonu](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na **Deploy Azure**, nahraďte výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů na portálu.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Vytvoření UDR podsítě front-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě front-end podle výše uvedených scénář, postupujte následujícím způsobem.

3. Spustit **`azure network route-table create`** příkaz a vytvořte tabulku směrování podsítě front-end.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Výstup:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parametry:
    - **-g (nebo – skupina zdroje)**. Název skupiny prostředků místo, kam se bude vytvořena UDR. Pro naše scénář *TestRG*.
    - **-l (nebo – umístění)**. Kde se bude vytvořena nová UDR Azure oblast. Pro naše scénář *westus*.
    - **n (nebo – název)**. Název pro nový UDR. Pro naše scénář *UDR FrontEnd*.

4. Spustit **`azure network route-table route create`** příkaz k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené do back-end podsítě (192.168.2.0/24) **FW1** OM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Výstup:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parametry:
    - **-r (nebo – název tabulky směrování)**. Název tabulky směrování místo, kam se přidají trasy. Pro naše scénář *UDR FrontEnd*.
    - **-(nebo – předponu adresy)**. Předpona adresy podsítě, kde jsou určeny paketů pro. Naše scénář *192.168.2.0/24*.
    - **-y (nebo – typ směrování Další –)**. Typ objektu přenosu se odešle. Možné hodnoty jsou *VirtualAppliance* *VirtualNetworkGateway*, *VNETLocal*, *Internetu*nebo *žádný*.
    - **-p (nebo – další směrování ip adresy**). IP adresy pro přesměrování. Naše scénář *192.168.0.4*.

5. Spustit **`azure network vnet subnet set`** příkaz přiřadit směrování tabulky vytvořené nad s podsítí **FrontEnd** .

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Výstup:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parametry:
    - **-e (nebo – vnet název)**. Název VNet, kde je uložena podsítě. Pro naše scénář *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Vytvoření UDR podsítě back-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě back-end podle výše uvedených scénáře, postupujte následujícím způsobem.

1. Spustit **`azure network route-table create`** příkaz a vytvořte tabulku směrování podsítě back-end.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Spustit **`azure network route-table route create`** příkaz k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené front-end připravenost (192.168.1.0/24) **FW1** OM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Spustit **`azure network vnet subnet set`** příkaz přiřadit směrování tabulky vytvořené nad s podsítí **back-end** .

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>Povolit přesměrování IP na FW1
Povolit přesměrování v NIC používaný **FW1**IP, postupujte následujícím způsobem.

1. Spustit **`azure network nic show`** příkaz a Všimněte si hodnoty pro **IP povolit přesměrování**. Je třeba nastavit na *hodnotu false*.

        azure network nic show -g TestRG -n NICFW1

    Výstup:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Spustit **`azure network nic set`** příkaz Povolit přesměrování IP.

        azure network nic set -g TestRG -n NICFW1 -f true

    Výstup:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parametry:

    - **-f (nebo--povolení předávání ip)**. *true* nebo *false*.
