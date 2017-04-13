<properties
   pageTitle="Vytvoření virtuálního počítače s více nic"
   description="Zjistěte, jak můžete vytvořit a nakonfigurovat vms s více nic"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Vytvoření virtuálního počítače s více nic

Můžete vytvořit virtuálních počítačích (VMs) v Azure a připojit víc sítí rozhraní (NIC) na jednotlivé vaší VMs. Více NIC je povinné pro mnoho sítě virtuální spotřebiče, například aplikace doručení a sítě WAN optimalizační řešení. Více NIC také poskytuje další funkce správy přenosy sítě, včetně izolace komunikace mezi popředí ukončit NIC a zpět end adaptérů NIC: nebo oddělení dat rovině přenosy z provozu rovině správy.

![Více NIC pro OM](./media/virtual-networks-multiple-nics/IC757773.png)

Na obrázku výše zobrazuje OM s tři nic že každý připojené jiné podsítě.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Internetové VIP (klasické nasazení) je podporována pouze u NIC. "Výchozí" Existuje pouze jeden VIP IP výchozí NIC.
- V současné době nejsou podporovány Instance úroveň veřejnou IP (LPIP) adresy (klasické nasazení) pro více NIC VMs.
- Pořadí nic z uvnitř OM náhodně a také můžete změnit přes Azure infrastruktury aktualizace. Však IP adresy a odpovídající ethernet MAC adresy bude zůstávají stejné. Předpokládejme například, že **Eth1** má adresou IP 10.1.0.100 a MAC 00-0D-3A-B0-39-0D; Po aktualizaci Azure infrastruktury a restartujte počítač by se změnit **Eth2**, ale IP a MAC párování zůstane. Po restartování počítače se inicializovaný zákazníka směru NIC zůstane.
- Adresa pro každou NIC na každý OM musí být umístěné v podsítě, více nic na jeden OM každý je možné stanovit adres, které jsou ve stejném podsítě.
- Velikost OM určuje číslem nic, které můžete vytvořit pro virtuálního počítače. Odkaz [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) a [Linux](../virtual-machines/virtual-machines-linux-sizes.md) OM velikosti články vám zjistit, kolik NIC podporuje každý OM velikost. 

## <a name="network-security-groups-nsgs"></a>Skupiny zabezpečení síti (NSGs)
V nasazení Správce prostředků může být všechny NIC virtuálního počítače související s sítě zabezpečení skupiny (NSG), včetně všech nic na OM obsahující více nic povolené. Pokud NIC přiřadí adresu v rámci podsítě, kde je přidružený NSG podsítě, pak pravidel v podsítě NSG taky použijte tento NIC. Kromě přidružení podsítí k NSGs, můžete také přidružit NIC NSG.

Pokud podsítě souvisí s NSG a NIC v rámci této podsítě jednotlivě přidružené NSG je přidružená NSG pravidla platí v **toku pořadí** podle směru komunikace, které se předávají funkcím nebo stmívání NIC:

- **Přenosy příchozí** jehož cílem je předmětné NIC prochází nejdřív podsítě aktivaci pravidla NSG podsítě, před předávání do NIC a potom aktivaci pravidla NIC NSG.
- **Odchozí přenosy** nichž je zdroj v NIC pokračuje nejdřív se NIC aktivaci pravidla NSG NIC, před procházející podsítě a po aktivaci pravidla NSG podsítě.

Další informace o [Skupiny zabezpečení sítě](virtual-networks-nsg.md) a jak se použijí podle přidružení podsítí, VMs a nic.

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Postup při konfiguraci více NIC OM v klasické nasazení

Následující pokyny vám pomůže vytvořit více NIC OM obsahující 3 nic: výchozí NIC a dva další nic. Postup konfigurace vytvoří OM, které budou nakonfigurovány podle fragmentu souboru konfigurace služby níže:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Následující požadavky musíte před pokusem o spuštění příkazu prostředí PowerShell v příkladu.

- Předplatné Azure.
- Nakonfigurované virtuální sítě. Další informace o VNets naleznete v tématu [Přehled virtuální sítě](virtual-networks-overview.md) .
- Nejnovější verzi Azure PowerShell stažení a instalaci. Přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

Pokud chcete vytvořit virtuálního počítače s více nic, postupujte následujícím způsobem:

1. Vyberte OM obrázek z Galerie obrázků OM Azure. Všimněte si, že obrázky často mění a jsou dostupné v oblasti. Obrázek zadaný v následujícím příkladu nemusí můžete změnit nebo může být ve vaší oblasti, takže nezapomeňte zadat obrázek, který budete potřebovat.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Vytvoření OM konfigurace.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Vytvoření přihlášení správce výchozí.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Přidejte další nic OM konfiguraci.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Zadejte podsítě a IP adresu pro výchozí NIC.

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Vytvoření OM v síti virtuální.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] VNet, který tady zadáte, musí existovat (jak je uvedeno v požadavky). V příkladu níže Určuje virtuální sítě s názvem **MultiNIC VNet**.

## <a name="limitations"></a>Omezení

Při použití funkce NIC více platí následující omezení:

- Více NIC VMs je vytvořit v Azure virtuálních sítí (VNets). Bez VNet VMs se nedají konfigurovat se nic více.
- Všechny VMs množiny dostupnost muset používat buď nic jednoho nebo více NIC Nesmí být tvořen kombinací s víc NIC VMs a jeden VMs NIC v rámci sady dostupnosti. Stejná pravidla platí pro VMs do cloudové služby.
- OM s jednoho NIC se nedají konfigurovat se nic s víc (a naopak) po nasazení bez odstraníte a znovu vytvořit.


## <a name="secondary-nics-access-to-other-subnets"></a>Sekundární nic přístup do dalších podsítí

Sekundární nic nebude ve výchozím nastavení nakonfigurována výchozí bránu, kvůli které bude tok sekundární nic omezenou být v rámci stejné podsítě. Pokud chcete uživatele povolit sekundární nic chcete mluvit mimo vlastní podsítě, budou se muset přidat položky v tabulce směrování ke konfiguraci brány podle níže uvedeného popisu.

>[AZURE.NOTE] VMs vytvořené před červenec 2015 pravděpodobně výchozí bránu nakonfigurován pro všechny nic. Brána pro sekundární nic budou odebrány až restartování tyto VMs. V operačních systémech, které používají slabými hostitele směrování modelu, třeba Linux můžete připojení k Internetu přerušit používáte-li přenosy průniku a výstupním různých nic.

### <a name="configure-windows-vms"></a>Konfigurace VMs systému Windows

Předpokládejme, že máte Windows OM s dvěma nic následujícím způsobem:

- Primární NIC IP adresu: 192.168.1.4
- Sekundární NIC IP adresa: 192.168.2.5

V tabulce směrování IPv4 pro tento OM by vypadal takto:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Všimněte si, že výchozí trasy (0.0.0.0) je dostupná jen primární NIC. Nebudete mít přístup k prostředkům vně podsítě pro sekundární NIC, jak je vidět níže:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Přidání trasy výchozí na vedlejší NIC, postupujte následujícím způsobem:

1. Na příkazovém řádku spusťte příkaz dole k identifikaci pořadové číslo pro sekundární NIC:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Všimněte si druhá položka v tabulce s indexu 27 (v tomto příkladu).
3. Z příkazového řádku příkaz **Přidat směrování** jak je ukázáno v následujícím příkladu. V tomto příkladu zadáváte 192.168.2.1 jako výchozí brána pro sekundární NIC:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Testovat připojení, vraťte se do příkazového řádku a zkuste ping jiné podsítě z sekundární NIC jako zachycujících int e následujícím příkladu:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Můžete taky zaškrtnout tabulka směrování kontrola nově přidaný trasy, jak je ukázáno v následujícím příkladu:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurace Linux VMs

Linux VMs protože výchozí chování používá slabými hostitele směrování, doporučujeme sekundární nic je omezen na tok pouze v rámci stejné podsítě. Pokud některé scénáře požadovat připojení vně podsítě, ale měli uživatelé povolit zásady na základě směrování zajistit, aby používala přenosy průniku a výstupním stejné NIC.

## <a name="next-steps"></a>Další kroky

- Nasazení [MultiNIC VMs ve scénáři 2 osy aplikace v nasazení Správce prostředků](virtual-network-deploy-multinic-arm-template.md).
- Nasazení [MultiNIC VMs ve scénáři 2 osy aplikace klasické nasazení](virtual-network-deploy-multinic-classic-ps.md).
