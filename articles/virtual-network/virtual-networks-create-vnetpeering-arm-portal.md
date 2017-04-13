<properties
   pageTitle="Vytvoření VNet prozkoumávání pomocí portálu Azure | Microsoft Azure"
   description="Zjistěte, jak vytvořit virtuální síť na portálu Azure správce prostředků."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Vytvořit virtuální sítě prozkoumávání pomocí portálu Azure

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit VNet prozkoumávání podle scénáře nad pomocí portálu Azure, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Stanovit prozkoumávání VNET je potřeba vytvořit dva odkazy, jedno pro každé směr, mezi dvěma VNets. Vytvoříte VNET prozkoumávání odkaz pro VNET1 VNET2 nejdřív. Na portálu, klikněte na **Procházet** > **Zvolte virtuálních sítí**

    ![Vytvoření VNet prozkoumávání Azure portálu](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. V zásuvné virtuálních sítí zvolte VNET1, klikněte na Peerings a potom klikněte na Přidat

    ![Zvolte prozkoumávání](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Zásuvné přidat prozkoumávání pojmenujte peering odkaz LinkToVnet2, vyberte předplatné a mezi dvěma účastníky virtuální VNET2 sítě, klikněte na OK.

    ![Odkaz na VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Jakmile se vytvoří tento odkaz peering VNET. Můžete zobrazit stav připojení jako následující:

    ![Stav připojení](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Dále vytvořte VNET peering odkaz VNET2 VNET1. V zásuvné virtuálních sítí zvolte VNET2, klikněte na Peerings a potom klikněte na Přidat

    ![Mezi dvěma účastníky z jiných VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. V zásuvné přidat prozkoumávání pojmenujte peering odkaz LinkToVnet1, vyberte předplatné a účastníky virtuální sítě, klikněte na tlačítko OK.

    ![Vytváření virtuálních sítí dlaždice](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Jakmile se vytvoří tento odkaz peering VNET. Můžete zobrazit stav připojení jako následující:

    ![Stav konečný připojení](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Kontrola stavu LinkToVnet2 a jeho se změní na připojit taky.  

    ![Stav konečný odkaz 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] Prozkoumávání VNET je vytvořen jenom Pokud jsou oba odkazy připojeni.

Existuje několik konfigurovat vlastnosti u všech odkazů:

|Možnost|Popis|Výchozí|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Zda adresy mezery mezi dvěma účastníky VNet být součástí značku Virtual_network|Ano|
|AllowForwardedTraffic|Zda je přenos není pocházející z peered VNet přijaté nebo nezobrazí|Ne|
|AllowGatewayTransit|Umožňuje VNet používat bránu VNet mezi dvěma účastníky|Ne|
|UseRemoteGateways|Použijte bránu VNet vaše mezi dvěma účastníky. Mezi dvěma účastníky VNet musí mít brány nakonfigurován a je zaškrtnuté AllowGatewayTransit. Nelze použít tuto možnost, pokud máte nakonfigurované brány|Ne|

Každý odkaz v VNet prozkoumávání obsahuje sadu nad vlastnosti. Z portálu můžete odkaz VNet prozkoumávání a změnit všechny dostupné možnosti, klikněte na Uložit, aby změnit efekt.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. V tomto příkladu použijeme dvě předplatná A a B a dva uživatelé Uživatel_a a Uživateli_b s oprávněními, abyste v předplatných v uvedeném pořadí
3. Na portálu, klikněte na Procházet, zvolte virtuální sítě. Klikněte VNET a klikněte na Přidat.

    ![Scénář 2 Procházet](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Na zásuvné přístup přidat, klikněte na vybrat roli a zvolte síť přispěvatelů klikněte na Přidat uživatele, znaménko Uživateli_b do pole Název zadejte a klikněte na OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Toto není povinné, prozkoumávání lze vytvořit i v případě, že uživatelé jednotlivě posunout výš, prozkoumávání požadavky pro jejich odpovídajících Vnets dlouhou, jak shodují žádosti. Přidání privilegovaných uživatelů z jiných VNet jako uživatele v místním VNet usnadňuje nastavení portálu.

5. Potom se přihlaste k Azure portál Uživateli_b, kdo je uživatel oprávnění pro SubscriptionB. Postupujte podle výše uvedené kroky potřebné k přidání Uživatel_a jako přispěvatelů sítě.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Můžete odhlásit a znovu přihlásit na obou uživatelských relací v prohlížeči a zajistěte, aby že tak mohli ověřovat aktivované úspěšně.

6. Přihlášení k portálu jako Uživatel_a, přejděte na zásuvné VNET3, klikněte na tlačítko Peering, zkontrolujte "znáte Moje pole číslo ID zdroje" zaškrtávací políčko a zadejte tento zdroj ID pro VNET5 v pod formát.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Pole číslo ID zdroje](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Přihlaste se k portálu jako Uživateli_b a postupujte podle výše krok vytvoření prozkoumávání odkaz z VNET5 VNet3.

    ![Pole číslo ID zdroje 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Bude stanovit prozkoumávání a všechny virtuálního počítače v VNet3 by měl být komunikovat s všechny virtuálního počítače v VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Jako první krok VNET prozkoumávání propojení HubVnet VNET1. Všimněte si, že není vybraná možnost Povolit přesměrování přenosy na odkaz.

    ![Základní prozkoumávání](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. V dalším kroku se dají vytvářet prozkoumávání odkazy ze VNET1 HubVnet. Všimněte si, jestli je vybraná možnost přenosy povolit přesměrování.

    ![Základní prozkoumávání](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Po vytvoření prozkoumávání můžete naleznete v tomto [článku](virtual-network-create-udr-arm-ps.md) a definovat uživatele definované Route(UDR) kterým přesměrujete přenosy VNet1 prostřednictvím virtuální zařízení na používání jeho funkcí. Když zadáte adresu další směrování do směrování, můžete nastavit k IP adrese virtuální zařízení mezi dvěma účastníky VNet HubVNet


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.

2. Stanovit prozkoumávání v tomto scénáři VNET je potřeba vytvořit jenom jeden odkaz z virtuální sítě ve Správci Azure zdroje té klasickém. To znamená z **VNET1** **VNET2**. Na portálu, klikněte na **Procházet** > zvolit **Virtuálních sítí**

3. V sítích zásuvné virtuální zvolte **VNET1**. Klikněte na **Peerings**a potom klikněte na **Přidat**.

4. Do zásuvné přidat prozkoumávání název kliknout na odkaz. Tady je označená jako **LinkToVNet2**. V části Podrobnosti mezi dvěma účastníky vyberte **klasické**.

5. Klikněte na předplatné a virtuální sítě **VNET2**mezi dvěma účastníky. Klikněte na OK.

    ![Propojení Vnet1 Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Po vytvoření tohoto odkazu peering VNet peered dvě virtuální sítě a budete moct najdete v těchto článcích:

    ![Kontrola prozkoumávání připojení](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Odebrání prozkoumávání VNet

1.  V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2.  Přejděte na zásuvné virtuální sítě, klikněte na Peerings, klikněte na odkaz, který chcete odebrat, klikněte na tlačítko Odstranit.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Po odebrání jeden odkaz v VNET prozkoumávání stavu připojení mezi dvěma účastníky budou moct odpojeno.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. V tomto stavu nelze znovu vytvořit odkaz, tak, aby stavu připojení mezi dvěma účastníky změnil zahájil. Doporučujeme že odebrat obou propojení před vytvořením znovu VNET prozkoumávání.
