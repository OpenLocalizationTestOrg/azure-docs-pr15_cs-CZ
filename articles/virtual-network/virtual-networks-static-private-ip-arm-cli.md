<properties 
   pageTitle="Jak nastavit statické IP soukromé v režimu ARM pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Principy statické IP adresy (poklesu) a jak mají ovládat v režimu ARM pomocí rozhraní příkazového řádku"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Jak nastavit statická soukromé IP adresu v Azure rozhraní příkazového řádku

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [Spravovat soukromé IP adresa v modelu klasické nasazení](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ukázkové příkazy Azure rozhraní příkazového řádku pod očekávat jednoduché prostředí vytvořen. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovacím prostředí podle [vytvořit vnet](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Určení statické IP adresu soukromé při vytváření virtuálního počítače
Pokud chcete vytvořit OM s názvem *DNS01* v *FrontEnd* podsítě VNet s názvem *TestVNet* statické soukromé IP *192.168.1.101*, postupujte následujícím způsobem:

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Příkaz **azure konfigurace režim** přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Očekávaný výstup:

        info:    New mode is arm

3. Spusťte **vytvořit veřejné ip azure sítě** pro OM vytvořit veřejnou IP. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Očekávaný výstup:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (nebo – skupina zdroje)**. Název skupiny prostředků veřejnou IP vytvoří v.
    - **n (nebo – název)**. Název veřejnou IP.
    - **-l (nebo – umístění)**. Kde se bude vytvořena veřejnou IP Azure oblast. Pro naše scénář *centralus*.

3. Příkaz **vytvořit nic azure network** vytvořit NIC s statické IP soukromé. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Očekávaný výstup:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(nebo – soukromá adresa)**. Statická soukromé IP adresy síťovou
    - **-m (nebo – podsítě vnet název-)**. Název VNet tam, kde bude NIC vytvořili.
    - **-k (nebo – podsítě název)**. Název podsítě tam, kde bude NIC vytvořili.

4. Vytvoření OM pomocí veřejné adresy IP pomocí příkazu **vytvořit azure OM** a NIC vytvoří výše. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Očekávaný výstup:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (nebo – os typ)**. Typ operační systém OM, *Windows* a *Linux*.
    - **-f (nebo – nic název)**. Název NIC OM budou používat.
    - **-i (nebo – veřejné ip název-)**. Název veřejnou IP OM budou používat.
    - **-F (nebo – vnet název)**. Název VNet tam, kde bude OM vytvořili.
    - **-j (nebo – vnet podsítě název-)**. Název podsítě tam, kde bude OM vytvořili.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak získat statické soukromé informace o IP adrese pro virtuálního počítače

Pokud chcete zobrazit statické soukromé informace o IP adresu pro OM vytvořená pomocí skriptu výše uvedené, spusťte tento příkaz Azure rozhraní příkazového řádku a sledovat hodnoty pro *soukromé IP přiřazení metoda* a *soukromé IP adresa*:

    azure vm show -g TestRG -n DNS01

Očekávaný výstup:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak odebrat soukromé statickou IP adresu z virtuálního počítače
Statická soukromé IP adresu nemůžete odebrat z NIC v Azure rozhraní příkazového řádku pro správce prostředků. Musí vytvořit nové NIC, který používá dynamické IP, odeberte předchozí NIC z OM a pak přidejte nové NIC bude v angličtině. Chcete-li změnit NIC pro int OM používá e příkazy výše, postupujte následujícím způsobem.
    
1. Příkaz **vytvořit nic azure network** k vytvoření nové NIC pomocí dynamického přidělení IP. Všimněte si, jak se nemusí zadejte IP adresu tentokrát.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Očekávaný výstup:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Příkaz **Nastavení azure OM** změnit NIC používaných OM.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Očekávaný výstup:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Pokud je potřeba, zadejte příkaz **Odstranit nic azure network** odstraňte staré NIC.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Očekávaný výstup:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak přidat statické soukromé IP adresu do existující OM
Pokud chcete přidat statické IP adresu soukromé NIC používaný OM vytvořené pomocí skriptu pro výše uvedené, spusťte tento příkaz:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Očekávaný výstup:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Další kroky

- Další informace o [Rezervovaná veřejnou IP](virtual-networks-reserved-public-ip.md) adresách.
- Další informace o adresách [úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Použijte [User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
