<properties
   pageTitle="Více IP adres pro virtuálních počítačích - prostředí PowerShell | Microsoft Azure"
   description="Naučte se přiřadit virtuálního počítače pomocí prostředí PowerShell Azure více IP adres."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Přiřadit virtuálních počítačích více IP adres

Virtuální počítač Azure (OM) může obsahovat jedno nebo více sítí rozhraní (NIC) připojené k němu. Všechny NIC můžete mít minimálně jednu veřejné nebo soukromé IP adresy přiřazené k němu. Pokud znáte nejsou IP adresy v Azure, přečtěte si článek [IP adres v Azure](virtual-network-ip-addresses-overview-arm.md) Další informace o nich znáte. Tento článek vysvětluje, jak pomocí Powershellu Azure přiřadit OM v modelu nasazení Správce prostředků Azure více IP adres.

Přiřazení více IP adres do virtuálního počítače umožňuje následující možnosti:

- Hostování více weby nebo služby s různými adresami IP a certifikáty SSL na jednom serveru.
- Sloužit jako síť virtuální spotřebiče, například bránu firewall nebo služba Vyrovnávání zatížení.
- Možnost přidejte některou z soukromé IP adresy z některého z nic do fondu back-end Vyrovnávání zatížení Azure. V minulosti jenom primární IP adresa primární NIC nelze přidat do fondu back-end.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Zaregistrovat k náhledu, odešlete e-mailu [Více IP adresy](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) pomocí svého ID předplatného a zamýšlené použití.

## <a name="scenario"></a>Scénář

V tomto článku přiřadíte tři IP uživatelského rozhraní sítě.
Následující příklad konfigurace budou vytvořené a přiřazené k NIC obsahujících tři soukromé IP adresy a jeden veřejnou IP adresu přiřazenou:

- IPConfig-1: Dynamické soukromé IP adresu (výchozí) a veřejnou IP adres z veřejné prostředek IP adresy s názvem PIP1.
- IPConfig-2: Statické soukromé IP adresa a bez veřejnou IP adresu.
- IPConfig-3: Dynamické soukromé IP adresa a bez veřejnou IP adresu.

    ![Ikona alternativní text](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Tento scénář předpokládá, že máte skupina zdroje s názvem *RG1* , ve kterém je VNet s názvem *VNet1* a podsítě s názvem *Podsíť1*. Kromě toho předpokládá, že máte OM s názvem *VM1*, rozhraní sítě s názvem *VM1 NIC1* přidružené k němu a veřejnou IP adresu s názvem *PIP1*.

[Tento článek](./virtual-machines/virtual-machines-windows-ps-create.md ) provede postup vytvoření zdroje zmíněnou v případě, že jste ještě nevytvořili před.

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

1. Otevřete okno příkazového řádku prostředí PowerShell a postupujte podle pokynů v této části v relaci Powershellu. Pokud ještě nemáte prostředí PowerShell nainstalovali a nakonfigurovali, postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) .

2. Změňte "hodnoty" následující $Variables na požadované Azure [umístění](https://azure.microsoft.com/regions) virtuální sítě nachází, klepněte název [pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md#resource-groups), VNet v rámci skupiny zdrojů, podsítě se chcete připojit NIC do a název síťovou Postupujte podle pokynů přidat více IP adres pro všechny NIC připojené k OM, tak, jak požadujete.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Pokud neznáte název existující Azure umístění nebo skupina zdroje, zadejte následující příkazy:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>NIC musí být připojená ke podsítě v rámci existující síť Azure virtuální (VNet). Tři části: NIC podsítě a VNet, třeba všechny existují ve stejné oblasti a [předplatné](../azure-glossary-cloud-terminology.md#subscription).  Pokud znáte není VNets, přečtěte si článek [Přehled virtuální sítě](virtual-networks-overview.md) a další informace o nich v článku [Vytvoření VNet](virtual-networks-create-vnet-arm-ps.md) se dozvíte, jak ho vytvořit.

    Pokud neznáte název existující VNet, zadejte následující příkaz a nahraďte *VNet1* v předchozí proměnná Název VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Pokud je seznam vrátil prázdný, je potřeba vytvořit VNet. Se dozvíte, jak, přečtěte si článek [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-ps.md) .

    Zadejte následující příkazy a získejte název existující podsítí v rámci VNet *Podsíť1* nad nahraďte názvem podsítě:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Zadejte tento příkaz Načíst podsítě a přiřadit ji někomu proměnné.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Definovat, které chcete přiřadit síťovou konfigurace IP Jednotlivé konfigurace můžete mít statické nebo dynamické soukromé IP adres a adresu statické nebo dynamické jednu přidružené veřejnou IP adresu zdroje.

    Přidání nebo odebrání libovolný počet konfigurace, které postupujte podle toho, kolik chcete přidružit k nastavení a NIC IP adresy, kterou chcete konfigurovat.

    **IPConfig-1**

    Změňte hodnotu *PIP1* název existující veřejný prostředek adresy IP rozlohu v umístění, který vytváříte NIC v a, který není aktuálně přidružený k nic jiného. Změna *RG1* na název skupiny prostředků veřejné prostředek IP adresy existuje v. Změnit název, který chcete poskytnout ke konfiguraci první IP *IPConfig-1* . Zadejte následující příkazy:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Poznámka *-primární* přepnout. Když přiřadíte NIC více IP konfigurací, musí mít přiřazené konfigurací jako *primární*. Pokud neznáte název existující veřejný zdroj IP adresu, zadejte tento příkaz: Get-AzureRMPublicIPAddress | Formátovat tabulku název, umístění, adresa IP, IpConfiguration

    Pokud **IPConfiguration** sloupec neobsahuje žádnou hodnotu do výstupu vrácena, veřejné prostředek IP adresy není přidružený k existující NIC a lze použít. Pokud v seznamu odkazuje na prázdnou buňku nebo nejsou žádné veřejnou IP adresu prostředky, můžete vytvořit pomocí příkazu **Nový AzureRmPublicIPAddress** .

    >[AZURE.NOTE] Veřejné adresy IP mít nominální poplatek. Další informace o IP adresu ceny, přečtěte si stránku [ceny IP adres](https://azure.microsoft.com/pricing/details/ip-addresses) .

    **IPConfig-2**

    Změňte název, který chcete poskytnout ke druhé konfigurace IP a změnit *10.0.0.5* na nepoužívané platné IP adresu podsítě, které přiřazujete NIC do *IPConfig-2* . Zadejte následující příkazy:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Zadejte následující příkaz, pokud si nejste jisti IP adresu oblast přiřazená podsítě:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Změna *IPConfig 3* na název, který chcete poskytnout ke konfiguraci třetí IP a zadejte následující příkazy:

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Až 250 soukromé IP adresu můžete přiřadit NIC. Existuje limit počty veřejné IP adresy, které lze použít v rámci předplatného. Další informace, přečtěte si článek [omezuje Azure](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Vytvoření NIC pomocí konfigurace IP definované v předchozím kroku.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. Připojte NIC při vytváření virtuálního počítače pomocí kroků uvedených v článku [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-ps-create.md) . Když je v článku vytvoří OM serverem Windows, kroky zůstávají stejné pro Linux OM, než vyberete jiný operační systém. Kroky 1 až 3 tohoto článku. Přeskočit kroky 4 a 5 a potom dokončení kroku 6 v tématu Vytvoření OM článek.

    >[AZURE.WARNING] Krok 6 v tématu Vytvoření OM článek selže, pokud změnili jste proměnné s názvem $nic na jinou v kroku 6 v tomto článku, nebo nedokončili předchozích kroků v tomto článku.

8. Zobrazení soukromých IP adresy, které Azure DHCP přiřazená NIC a veřejnou IP adresu přiřazené zdroje NIC zadáním následujícího příkazu:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Ruční přidání všech soukromých IP adres (včetně primární) konfigurace TCP/IP v operačním systému. 

**Windows**

1. Na příkazovém řádku zadejte *ipconfig/všechny*.  Zobrazí jenom *primární* soukromé IP adresu (přes DHCP).
2. Další typ *ncpa.cpl* v okně příkazového řádku. Otevře se nové okno.
3. Otevřete dialogové okno vlastností **Připojení k místní síti**.
4. Dvojitým kliknutím na verzi Internet Protocol (IPv4 4)
5. Vyberte **použít následující IP adresa** a zadejte tyto hodnoty:
    - **IP adresy**: Zadejte *primární* soukromé IP adresa
    - **Maska podsítě**: nastavení založený na vaší podsítě. Například, pokud podsítě je /24 podsítě pak maska podsítě je 255.255.255.0.
    - **Výchozí brána**: první IP adresu podsítě. Při vaší podsítě 10.0.0.0/24, IP adresa brány je 10.0.0.1.
    - Klikněte na **použít následující adresy serverů DNS** a zadejte tyto hodnoty:
        - **Upřednostňovaný server DNS:** Pokud nepoužíváte serveru DNS, zadejte 168.63.129.16.  Pokud jste, zadejte IP adresu serveru DNS.
    - Klikněte na tlačítko **Upřesnit** a přidejte další IP adres. Přidejte všechny sekundární soukromé IP adres uvedené v kroku 8 NIC se stejnou podsítě určeným pro primární IP adresu.
    - Klikněte na **OK** zavřete nastavení TCP/IP a potom na **OK** zavřete okno nastavení adaptér. Potom se znovu vytvořit připojení RDP.

6. Na příkazovém řádku zadejte *ipconfig/všechny*. Zobrazují se všechny IP adresy, které jste přidali a DHCP je normálně vypnuté.


**Linux (se systémem Ubuntu)**

1. Otevřete okno terminálu.
2. Zkontrolujte, jestli že je kořenové uživatelem. Pokud nejste, můžete to udělat pomocí následujícího příkazu:

            sudo -i
3. Aktualizujte konfiguračního souboru rozhraní sítě (za předpokladu, že "eth0").
    - Zachovat stávající položku řádku dhcp. To nastaví její konfiguraci a primární IP adresy používané k předcházet.
    - Přidání konfigurace další statické IP adresy pomocí následujících příkazů:

                cd /etc/network/interfaces.d/
                ls

        Měli byste vidět .cfg souboru.
4. Otevřete soubor: vi *název souboru*.

    Měli byste vidět následující řádky na konci souboru:

            auto eth0
            iface eth0 inet dhcp
5. Přidejte následující řádky po řádcích, které jsou v tomto souboru:

            iface eth0 inet static
            address <your private IP address here>
6. Uložte soubor pomocí tento příkaz:

            :wq
7.  Obnovení rozhraní sítě s Tento příkaz:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Pokud používáte připojení ke vzdálené spusťte ifdown a ifup ve stejném řádku.
8. Ověřte, že na IP adresu se přidá na rozhraní sítě s Tento příkaz:

            ip addr list eth0

    Měli byste vidět na IP adresu, kterou jste přidali jako součást seznamu.

**Linux (Redhat, CentOS a další)**

1. Otevřete okno terminálu.
2. Zkontrolujte, jestli že je kořenové uživatelem. Pokud nejste, můžete to udělat pomocí následujícího příkazu:

            sudo -i
3. Zadejte svoje heslo a postupujte podle pokynů. Jakmile se dostanete uživatele root, přejděte do síťové složky skriptů pomocí následujícího příkazu:

            cd /etc/sysconfig/network-scripts
4. Seznam souvisejících ifcfg souborů pomocí následujícího příkazu:

            ls ifcfg-*

    Měli byste vidět *ifcfg eth0* jako jeden z těchto souborů.
5. Zkopírujte soubor *ifcfg eth0* a pojmenujte ho *ifcfg-eth0:0* pomocí následujícího příkazu:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Upravit *ifcfg-eth0:0* soubor se tento příkaz:

            vi ifcfg-eth1
7. Změna zařízení na příslušný název v souboru. *eth0:0* v tomto případě se tento příkaz:

            DEVICE=eth0:0
8. Změna *IPADDR = YourPrivateIPAddress* řádku, aby odrážely IP adresu.
9. Uložte soubor pomocí následujícího příkazu:

            :wq
10. Restartujte síťových služeb a ujistěte se, že změny úspěšné spuštěním následující příkazy:

            /etc/init.d/network restart
            Ipconfig

    Měli byste vidět na IP adresu, kterou jste přidali, *eth0:0*v seznamu vrácena.

## <a name="add"></a>Přidání IP adres pro existující OM

Provedení následujících kroků můžete přidat další IP adresy do existujícího NIC:

1. Otevřete okno příkazového řádku prostředí PowerShell a postupujte podle pokynů v této části v relaci Powershellu. Pokud ještě nemáte prostředí PowerShell nainstalovali a nakonfigurovali, postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) .

2. Změňte "hodnoty" následující $Variables název NIC, kterou chcete přidat IP adresy a pole Skupina zdroje a umístění, do kterého NIC existuje v:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Pokud neznáte název NIC, který chcete změnit, zadejte následující příkazy, změňte hodnoty předchozí proměnných:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Vytvoření proměnné a nastavte ji na existující NIC tak, že zadáte tento příkaz:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Načtení ID podsítě NIC je připojen k provedením- [Krok 3](#subnet) / vytvořit virtuálního počítače s více IP adres části tohoto článku.

5. Vytvoření konfigurace IP, který chcete přidat k síti postupujte podle pokynů v [kroku 5](#ipconfigs) vytvořit virtuálního počítače s více IP adres části tohoto článku.

6. Změna *$IPConfigName4* název konfigurace IP, který jste vytvořili v předchozím kroku. Pokud chcete přidat konfiguraci, zadejte tento příkaz:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Pokud chcete nastavit NIC s nastavením IP, zadejte tento příkaz:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Přidání IP adresy, kterou jste přidali NIC operační systém OM postupujte podle pokynů v [kroku 9](#os) vytvořit OM s více IP adres části tohoto článku.
