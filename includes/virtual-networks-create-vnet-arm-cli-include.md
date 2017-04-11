## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Jak vytvořit VNet pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure slouží ke správě Azure zdrojů z příkazového řádku z libovolného počítače s Windows, Linux nebo OSX. Vytvoření VNet pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte rozhraní příkazového řádku Azure, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Příkaz **azure konfigurace režimu** přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

3. V případě potřeby pracovat **Vytvoření azure skupiny** k vytvoření nové skupiny prostředků, vidíte na obrázku. Všimněte si výstup příkazu. Seznam zobrazený po výstup vysvětluje parametry použité. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **n (nebo – název)**. Název nové skupiny prostředků. Naše scénář *TestRG*.
    - **-l (nebo – umístění)**. Azure oblast, kde se bude vytvořena nová skupina zdrojů. Pro naše scénář *centralus*.

4. Příkaz **vytvořit azure sítě vnet** vytvářet VNet a podsítě, jak je ukázáno v následujícím příkladu. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (nebo – skupina zdroje)**. Název skupiny prostředků místo, kam se bude vytvořena VNet. Naše scénář *TestRG*.
    - **n (nebo – název)**. Název VNet vytvořit. Pro naše scénář *TestVNet*
    - **-(nebo – předpony adres)**. Seznam CIDR bloků VNet adresní prostor. Pro naše scénář *192.168.0.0/16*
    - **-l (nebo – umístění)**. Kde se bude vytvořena VNet Azure oblast. Pro naše scénář *centralus*.

5. Příkaz **vytvořit azure podsítě vnet** Vytvoření podsítě, jak je ukázáno v následujícím příkladu. Všimněte si výstup příkazu. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (nebo – vnet název**. Název VNet místo, kam se bude vytvořena podsítě. Naše scénář *TestVNet*.
    - **n (nebo – název)**. Název nového podsítě. Pro naše scénář *FrontEnd*.
    - **-(nebo – předponu adresy)**. Blok CIDR podsítě. Čtyři naše scénář, *192.168.1.0/24*.

6. Opakujte krok 5 výše uvedeného jiných podsítí, v případě potřeby. Naše scénáře spusťte následující příkaz a vytvořte podsítě *back-end* .

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Příkaz **Zobrazit vnet azure sítě** zobrazte vlastnosti nové vnet, jak je ukázáno v následujícím příkladu.

        azure network vnet show -g TestRG -n TestVNet

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
