## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Jak vytvořit klasické VNet pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure slouží ke správě Azure zdrojů z příkazového řádku z libovolného počítače s Windows, Linux nebo OSX. Vytvoření VNet pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Příkaz **vytvořit azure sítě vnet** vytvářet VNet a podsítě, jak je ukázáno v následujícím příkladu. Seznam zobrazený po výstup vysvětluje parametry použité.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Očekávaný výstup:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **– vnet**. Název VNet vytvořit. Pro naše scénář *TestVNet*
    - **-e (nebo – adresní prostor)**. VNet adresní prostor. Pro naše scénář *192.168.0.0*
    - **-i (nebo – cidr)**. Maska sítě ve formátu CIDR. Pro naše scénář *16*.
    - **- n (nebo – podsítě název**). Název první podsítě. Pro naše scénář *FrontEnd*.
    - **-p (nebo – sítě ip adres podsítí start)**. Počáteční adresu IP adres podsítí nebo podsítě adresní prostor. Pro naše scénář *192.168.1.0*.
    - **-r (nebo – podsítě cidr)**. Maska sítě ve formátu CIDR podsítě. Pro naše scénář *24*.
    - **-l (nebo – umístění)**. Kde se bude vytvořena VNet Azure oblast. Pro naše scénář *Centrální USA*.

3. Příkaz **vytvořit azure podsítě vnet** Vytvoření podsítě, jak je ukázáno v následujícím příkladu. Seznam zobrazený po výstup vysvětluje parametry použité.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Tady je očekávané výstup pro výše uvedené příkaz:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (nebo – vnet název**. Název VNet místo, kam se bude vytvořena podsítě. Naše scénář *TestVNet*.
    - **n (nebo – název)**. Název nového podsítě. Naše scénář *back-end*.
    - **-(nebo – předponu adresy)**. Blok CIDR podsítě. Čtyři naše scénář, *192.168.2.0/24*.

4. Příkaz **Zobrazit vnet azure sítě** zobrazte vlastnosti nové vnet, jak je ukázáno v následujícím příkladu.

            azure network vnet show

    Tady je očekávané výstup pro výše uvedené příkaz:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
