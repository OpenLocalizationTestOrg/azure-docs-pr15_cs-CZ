<properties 
   pageTitle="Zrychlit sítě pro virtuální počítač - prostředí PowerShell | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat zrychlit sítě Azure virtuálního počítače pomocí Powershellu."
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
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Urychlení sítě virtuálního počítače

> [AZURE.SELECTOR]
- [Azure portálu](virtual-network-accelerated-networking-portal.md)
- [Prostředí PowerShell](virtual-network-accelerated-networking-powershell.md)

Urychlení sítí umožňuje jednoho kořenové v/v virtualizace (SR IOV) do virtuálního počítače (OM) výrazně zlepšit sociální sítě. Tato cesta výkonné obchází hostiteli z datapath latence, kolísání a využití procesoru pro použití se nejčastěji náročné sítě úlohami na podporovaných typů OM. Tento článek vysvětluje, jak pomocí Powershellu Azure konfigurace zrychlit sítě v modelu nasazení Správce prostředků Azure. Můžete taky vytvořit virtuálního počítače s zrychlit sítě pomocí portálu Azure. Se dozvíte, jak, klikněte na pole Azure portál v horní části tohoto článku.

Následující obrázek znázorňuje komunikaci mezi virtuálních počítačích dva (OM) s a bez zrychlit sítě:

![Porovnání](./media/virtual-network-accelerated-networking-powershell/image1.png)

Bez zrychlit sociální sítě třeba všechny provozu v síti a odhlášení z OM procházet hostiteli a přepínač virtuální. Virtuální přepnout obsahuje všechny vynucení zásad, například skupiny zabezpečení sítě, seznamy řízení přístupu, izolace a dalšími službami síťových virtualizované chcete síťová komunikace. Další informace najdete v článku [virtualizace Hyper-V síti a virtuální přepínač](https://technet.microsoft.com/library/jj945275.aspx) .

Zrychlit sítě v síti dorazí síťové karty (NIC) a předá bude v angličtině. Všechny zásady sítě, které slouží k použití virtuální přepínač bez zrychlit Networking jsou přesměrovat a nastavili v hardwaru. Použití zásad hardwaru umožňuje NIC k přesměrování v síti přímo do OM vynechání hostiteli a přepínač virtuální a zachovat všechny zásad, které se projeví v hostiteli.

Výhody zrychlit Networking platí pouze pro OM, která je zapnuta. Nejlepších výsledků dosáhnete je ideální pro povolení této funkce na aspoň dva VMs připojené k stejné VNet.  Při komunikaci mezi VNets nebo připojení místních má tuto funkci minimálními vliv na Celková latence.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Výhody

- **Nižší latence / vyšší paketů za druhý (pps):** Odebrání virtuální přepnout datapath odebere doba, po kterou paketů v hostiteli pro zpracování zásad a zvyšuje se počet paketů zpracovaných uvnitř OM.
- **Snížené kolísání:** Virtuální přepnout zpracování závisí na požadovanou velikost prázdného zásad, které je nutné použít a vytížení procesoru, který dělá zpracování. Odstranění vynucení zásad hardwaru odebere tento variabilitě tak, že doručování paketů přímo do OM odebrání hostiteli OM komunikace všechny přerušení software a přepnutí kontextu.
- **Využití procesoru zmenšená:** Vynechání virtuální přepínače v hostiteli vede k méně využití procesoru pro zpracování v síti.

## <a name="limitations"></a>Omezení

Při používání této funkce existují tato omezení:
 
- **Sítě rozhraní vytváření:** Urychlení sítě lze povolit pouze pro nové rozhraní sítě.  Nelze povolit na existující rozhraní sítě.
- **OM vytváření:** Vytvořili OM může rozhraní sítě se urychlení sítí povolené pouze připojen virtuálního počítače. Rozhraní sítě nelze připojit k existující OM.
- **Oblastí:** K dispozici v pouze oblastí západní centrální US a západní Europe Azure. Nastavení oblasti rozbalíte v budoucnu.
- **Podporované operační systém:** Microsoft Windows Server 2012 R2 a Windows Server 2016 Technical Preview 5. Podpora Linux a Windows Server 2012 přidá brzy bude k dispozici.
- **OM velikost:** Standard_D15_v2 a Standard_DS15_v2 jsou že pouze podporované OM instance velikosti. Další informace naleznete v článku [Windows OM velikosti](../virtual-machines/virtual-machines-windows-sizes.md) . Sadě velikostí podporované instancí OM rozbalíte v budoucnu.

Změny těchto omezení se ohlásí prostřednictvím [virtuální Networking Azure aktualizace](https://azure.microsoft.com/updates/accelerated-networking-in-preview) stránky.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Vytvoření Windows OM se urychlení sítí

1. Otevřete okno příkazového řádku prostředí PowerShell a postupujte podle pokynů v této části v relaci Powershellu. Pokud ještě nemáte prostředí PowerShell nainstalovali a nakonfigurovali, postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) .
2. Zaregistrovat k náhledu, odešlete e-mailovou zprávu [Zrychlit předplatná sítě](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s ID předplatného a zamýšlené použití. Není dokončete zbývající až po obdržíte e-mailové oznámení, že jste přijato do náhledu.
3. Registrace možnost k vašemu předplatnému zadáním následujících příkazů:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. *Westcentralus* nahraďte názvem jinam nepodporuje tuto funkci uvedené v tomto článku v části [omezení](#limitations) (v případě potřeby). Zadejte tento příkaz nastavit proměnnou umístění:

        $locName = "westcentralus"

5. Nahraďte název skupiny zdroje, které budou obsahovat rozhraní nové sítě a zadejte následující příkazy vytvořit *RG1* :

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Změna *VM1 NIC1* odpovídá vašim požadavkům na název rozhraní sítě a zadejte tento příkaz:

        $NICName = "VM1-NIC1"

7. Rozhraní sítě, musí být připojená ke podsítě v rámci existující síť Azure virtuální (VNet) ve stejném umístění a [předplatné](../azure-glossary-cloud-terminology.md#subscription) jako rozhraní sítě. Další informace o VNets v článku [Přehled sítě virtuální](virtual-networks-overview.md) Pokud si nejste s nimi. Pokud chcete vytvořit VNet, postupujte podle pokynů v článku [Vytvoření VNet](virtual-networks-create-vnet-arm-ps.md) . Změňte "hodnoty" následující $Variables název VNet a podsítě se chcete připojit rozhraní sítě.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Pokud neznáte název existující VNet v adresáři, který jste vybrali v kroku 3, zadejte následující příkazy:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Pokud je seznam vrátil prázdný, je potřeba vytvořit VNet v umístění. Pokud chcete vytvořit VNet, postupujte podle pokynů v článku [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-ps.md) .

    Zjištění názvu podsítí v rámci VNet, zadejte následující příkazy a nahraďte názvem podsítě *Podsíť1* nad:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Zadejte následující příkazy k načtení VNet a podsítě a jejich přiřazení k proměnné.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Určení existující veřejnou IP adresu prostředek, který může být přidružené k síti rozhraní k němu můžete připojit přes Internet. Pokud nechcete pro přístup k OM s rozhraní sítě přes Internet, můžete tento krok přeskočit. Bez na veřejnou IP adresu musíte se připojit k OM z jiného OM připojené k stejné VNet. 

    Změňte název existující veřejný prostředek adresy IP rozlohu na místě vytváříte rozhraní na kartě sítě a, který není aktuálně přidružený k jiné síti rozhraní *PIP1* . V případě potřeby změňte $rgName na název skupiny prostředků veřejné prostředek IP adresy existuje v a zadejte tento příkaz:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Pokud neznáte název existující veřejný zdroj IP adresu, zadejte následující příkazy:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Pokud **IPConfiguration** sloupec neobsahuje žádnou hodnotu do výstupu vrácena, veřejné prostředek IP adresy není přidružený k existující rozhraní sítě a lze použít. Pokud v seznamu odkazuje na prázdnou buňku nebo nejsou žádné veřejnou IP adresu prostředky, můžete vytvořit pomocí příkazu Nový AzureRmPublicIPAddress.

    >[AZURE.NOTE] Veřejné adresy IP mít nominální poplatek. Další informace o ceny o stránce [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) .
10. Pokud jste se rozhodli přidání veřejné zdroje IP adresu rozhraní, odeberte *- PublicIPAddress $PIP1* na konci příkazu, který následuje. Vytvoření rozhraní sítě s urychlení sítě tak, že zadáte tento příkaz:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Přiřaďte rozhraní sítě virtuálního počítače při vytváření OM podle pokynů uvedených v kroky 3 až 6 tohoto článku [vytvořit virtuálního počítače](../virtual-machines/virtual-machines-windows-ps-create.md) . V kroku 6-2 nahraďte *Standard_A1* jedním velikostí OM uvedené v tomto článku v části [omezení](#limitations) .

    >[AZURE.NOTE] Pokud změníte *název* $locName, $rgName nebo $nic proměnné v tomto článku se nepovede krok 6 v tématu Vytvoření OM článek. Můžete však změňte *hodnoty* proměnných.

12. Po vytvoření OM Stáhnout [zrychlit sítě ovladač](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84)připojení bude v angličtině a spusťte instalační program ovladač uvnitř OM.

13. Klikněte pravým tlačítkem myši na tlačítko Windows a klikněte na **Správce zařízení**. Zkontrolujte, jestli **Mellanox ConnectX 3 virtuální funkce Ethernet adaptér** vypadá ve skupinovém rámečku Možnosti **sítě** po rozbalení, jak je znázorněno na následujícím obrázku:

    ![Správce zařízení](./media/virtual-network-accelerated-networking-powershell/image2.png)