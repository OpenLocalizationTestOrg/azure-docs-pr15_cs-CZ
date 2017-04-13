<properties 
   pageTitle="Jak nastavit statické IP soukromé ausing klasický režim rozhraní příkazového řádku | Microsoft Azure"
   description="Principy statické soukromé IP adresy (poklesu) a jak mají ovládat v klasického režimu rozhraní příkazového řádku"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Jak nastavit statická soukromé IP adresu (klasické) v Azure rozhraní příkazového řádku

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete taky [Spravovat statické soukromé IP adresu v modelu nasazení Správce prostředků](virtual-networks-static-private-ip-arm-cli.md).

Ukázkové příkazy Azure rozhraní příkazového řádku pod očekávat jednoduché prostředí vytvořen. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovacím prostředí podle [vytvořit vnet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Určení statické IP adresu soukromé při vytváření virtuálního počítače
Pokud chcete vytvořit nové OM s názvem *DNS01* v nové cloudové službě s názvem *TestService* podle výše uvedených scénáře, postupujte takto:

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
1. Příkaz **vytvořit služby azure** k vytvoření cloudovou službu.

        azure service create TestService --location uscentral

    Očekávaný výstup:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Příkaz **azure vytvořit OM** vytvořit OM. Všimněte si hodnotu statickou IP adresu soukromé. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Očekávaný výstup:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (nebo – umístění)**. Kde se bude vytvořena OM Azure oblast. Naše scénář *centralus*.
    - **n (nebo – OM název)**. Název OM vytvořit.
    - **-w (nebo – virtuální network name)**. Název VNet tam, kde bude OM vytvořili. 
    - **-Ne (nebo – statické ip)**. Statická soukromé IP adresy OM.
    - **TestService**. Název cloudové služby, kde se OM vytvoří.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage – Windows – 2012R2-x64-v14.2**. Obrázek použít k vytvoření OM.
    - **adminuser**. Místní správce pro OM Windows.
    - **AdminP@ssw0rd**. Heslo místního správce OM Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak získat statické soukromé informace o IP adrese pro virtuálního počítače
Pokud chcete zobrazit statické soukromé informace o IP adresu pro OM vytvořená pomocí skriptu výše uvedených, spusťte tento příkaz Azure rozhraní příkazového řádku a sledovat hodnotu pro *StaticIP sítě*:

    azure vm static-ip show DNS01

Očekávaný výstup:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak odebrat soukromé statickou IP adresu z virtuálního počítače
Odebrat soukromé statickou IP adresu doplňuje OM skriptu výše, spusťte tento příkaz Azure rozhraní příkazového řádku:
    
    azure vm static-ip remove DNS01

Očekávaný výstup:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Jak přidat statické IP soukromé do existující OM
Chcete-li přidat statické soukromé IP adresu OM vytvořené pomocí skriptu nad o má následující příkaz:

    azure vm static-ip set DNS01 192.168.1.101

Očekávaný výstup:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Další kroky

- Další informace o [Rezervovaná veřejnou IP](virtual-networks-reserved-public-ip.md) adresách.
- Další informace o adresách [úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Použijte [User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
