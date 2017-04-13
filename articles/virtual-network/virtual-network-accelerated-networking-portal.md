<properties 
   pageTitle="Zrychlit sítě pro virtuální počítač - portál | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat zrychlit Networking Azure virtuálního počítače pomocí portálu Azure."
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
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Urychlení sítě virtuálního počítače

> [AZURE.SELECTOR]
- [Azure portálu](virtual-network-accelerated-networking-portal.md)
- [Prostředí PowerShell](virtual-network-accelerated-networking-powershell.md)

Urychlení sítí umožňuje jednoho kořenové v/v virtualizace (SR IOV) do virtuálního počítače (OM) výrazně zlepšit sociální sítě. Tato cesta výkonné obchází hostiteli z datapath latence, kolísání a využití procesoru pro použití se nejčastěji náročné sítě úlohami na podporovaných typů OM. Tento článek vysvětluje, jak používat portál Azure konfigurace zrychlit sítě v modelu nasazení Správce prostředků Azure. Můžete taky vytvořit virtuálního počítače s zrychlit sítě pomocí prostředí PowerShell Azure. Se dozvíte, jak, klikněte na pole Powershellu v horní části tohoto článku.

Následující obrázek znázorňuje komunikaci mezi virtuálních počítačích dva (OM) s a bez zrychlit sítě:

![Porovnání](./media/virtual-network-accelerated-networking-portal/image1.png)

Bez zrychlit sociální sítě třeba všechny provozu v síti a odhlášení z OM procházet hostiteli a přepínač virtuální. Virtuální přepnout obsahuje všechny vynucení zásad, například skupiny zabezpečení sítě, seznamy řízení přístupu, izolace a dalšími službami síťových virtualizované chcete síťová komunikace. Další informace najdete v článku [virtualizace Hyper-V síti a virtuální přepínač](https://technet.microsoft.com/library/jj945275.aspx) .

Zrychlit sítě v síti dorazí síťové karty (NIC) a předá bude v angličtině. Všechny zásady sítě, které slouží k použití virtuální přepínač bez zrychlit Networking jsou přesměrovat a nastavili v hardwaru. Použití zásad hardwaru umožňuje NIC k přesměrování v síti přímo do OM vynechání hostiteli a přepínač virtuální a zachovat všechny zásad, které se projeví v hostiteli.

Výhody zrychlit Networking platí pouze pro OM, která je zapnuta. Nejlepších výsledků dosáhnete je ideální pro povolení této funkce na aspoň dva VMs připojené k stejné VNet. Při komunikaci mezi VNets nebo připojení místních má tuto funkci minimálními vliv na Celková latence.

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

1. Zaregistrujte si náhled tak, že [Zrychlit sítě předplatných](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) pomocí svého ID předplatného a zamýšlené použití e-mailu. Není dokončete zbývající až po obdržíte e-mailové oznámení, že jste přijato do náhledu.
2. Přihlaste se k portálu Microsoft Azure na http://portal.azure.com.
3. Vytvořte virtuálního počítače pomocí kroků v článku [Vytvoření Windows OM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) výběr z následujících možností:
    - Vyberte operační systém uvedené v tomto článku v části omezení.
    - Vyberte umístění (oblast) uvedené v tomto článku v části omezení.
    - Vyberte velikost OM uvedené v tomto článku v části omezení. Pokud jeden z podporovaných formátů nevidíte, klikněte na **Zobrazit vše** v zásuvné **Zvolit rozměry** vyberte velikost v rozbaleném seznamu.
    - V **Nastavení** zásuvné klikněte na *Povolit* **Accelerated sítě**, jak je znázorněno na následujícím obrázku:

        ![Nastavení](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Možnost zrychlit sítě budou zobrazeny pouze pokud jste:
    >
    >- Přijetí do náhledu
    >- Vybraná podporované operační systém, umístění a velikosti OM uvedené v tomto článku v části omezení.

5. Po vytvoření OM Stáhnout [zrychlit Networking ovladač](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), připojovat se a přihlaste se k OM a spuštění instalačního programu ovladačů uvnitř OM.
6. Klikněte pravým tlačítkem myši na tlačítko Windows a klikněte na **Správce zařízení**. Zkontrolujte, jestli **Mellanox ConnectX 3 virtuální funkce Ethernet adaptér** vypadá ve skupinovém rámečku Možnosti **sítě** po rozbalení, jak je znázorněno na následujícím obrázku:

    ![Správce zařízení](./media/virtual-network-accelerated-networking-portal/image2.png)