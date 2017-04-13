<properties 
   pageTitle="Ovládací prvek směrování a používat virtuální spotřebiče pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení | Microsoft Azure"
   description="Zjistěte, jak určit směrování VNets pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Ovládací prvek směrování a jejich použití virtuální spotřebiče (klasický) pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete také [ovládací prvek směrování a používání virtuální zařízení v modelu nasazení Správce prostředků](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Ukázkové příkazy Azure rozhraní příkazového řádku pod očekávat jednoduché prostředí vytvořen podle výše uvedených scénáře. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, vytvořte prostředí znázorněno [vytváření VNet (klasické) pomocí rozhraní příkazového řádku Azure](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Vytvoření UDR podsítě front-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě front-end podle výše uvedených scénář, postupujte následujícím způsobem.

1. Spustit **`azure config mode`** přepněte do klasického režimu.

        azure config mode asm

    Výstup:

        info:    New mode is asm

3. Spustit **`azure network route-table create`** příkaz a vytvořte tabulku směrování podsítě front-end.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Výstup:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parametry:
    - **-l (nebo – umístění)**. Kde se bude vytvořena nová NSG Azure oblast. Pro naše scénář *westus*.
    - **n (nebo – název)**. Název pro nový NSG. Pro naše scénář *NSG FrontEnd*.

4. Spustit **`azure network route-table route set`** příkaz k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené do back-end podsítě (192.168.2.0/24) **FW1** OM (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Výstup:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parametry:
    - **-r (nebo – název tabulky směrování)**. Název tabulky směrování místo, kam se přidají trasy. Pro naše scénář *UDR FrontEnd*.
    - **-(nebo – předponu adresy)**. Předpona adresy podsítě, kde jsou určeny paketů pro. Naše scénář *192.168.2.0/24*.
    - **-(nebo – typ směrování Další –)**. Typ objektu přenosu se odešle. Možné hodnoty jsou *VirtualAppliance* *VirtualNetworkGateway*, *VNETLocal*, *Internetu*nebo *žádný*.
    - **-p (nebo – další směrování ip adresy**). IP adresy pro přesměrování. Naše scénář *192.168.0.4*.

5. Spustit **`azure network vnet subnet route-table add`** příkaz přiřadit směrování tabulky vytvořené nad s podsítí **FrontEnd** .

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Výstup:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parametry:
    - **-(nebo – vnet název)**. Název VNet, kde je uložena podsítě. Pro naše scénář *TestVNet*.
    - **- n (nebo – podsítě název**. Název tabulky směrování adres podsítí se přidají na. Naše scénář *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Vytvoření UDR podsítě back-end
Pokud chcete vytvořit tabulku směrování a směrování potřebné pro podsítě back-end podle výše uvedených scénáře, postupujte následujícím způsobem.

3. Spustit **`azure network route-table create`** příkaz a vytvořte tabulku směrování podsítě back-end.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Spustit **`azure network route-table route set`** příkaz k vytvoření cesty v tabulce směrování vytvořili nad odeslat všechny přenosy určené front-end připravenost (192.168.1.0/24) **FW1** OM (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Spustit **`azure network vnet subnet route-table add`** příkaz přiřadit směrování tabulky vytvořené nad s podsítí **back-end** .

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

