<properties
   pageTitle="Konfigurace virtuální sítě a Brána pro ExpressRoute klasické portálu | Microsoft Azure"
   description="Tento článek vás provede nastavení virtuální sítě pro ExpressRoute pomocí klasické nasazení modelu a portálu klasické."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Vytvořit virtuální síť pro ExpressRoute klasické portálu

Kroky v tomto článku vás provede jednotlivými konfigurace virtuální sítě a Brána virtuální sítě pro použití s ExpressRoute pomocí klasické nasazení modelu a klasické portálu.

Pokud hledáte pokyny pro nasazení modelu správce prostředků, můžete v následujících článcích: [vytvořit virtuální sítě pomocí prostředí PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) a [Přidat VPN brány tak, aby VNet správce prostředků pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Vytvoření klasické VNet a brány

Podle těchto kroků vytvořit klasické VNet a bránu virtuální sítě. Pokud už máte klasické VNet, naleznete v části [Konfigurovat existující klasické VNet](#config) v tomto článku.

1. Přihlaste se k [Azure klasické portálu](http://manage.windowsazure.com).

2. V levém dolním rohu obrazovky klikněte na **Nový**. V navigačním podokně klikněte na **Síť služby**a potom klikněte na **Virtuální síť**. Klikněte na **Vytvořit vlastní** spustíte Průvodce konfigurací.

3. Na stránce **Virtuální sítě podrobnosti** zadejte následující údaje:

    - **Název** – název virtuální sítě. Tento virtuální síťový název budete používat při nasazení VMs a PaaS instance, takže nemusí má být název příliš komplikovaně.
    - **Umístění** – umístění přímo souvisí s fyzické místo (oblast), kde se má zdroje (VMs) jsou uloženy. Pokud budete potřebovat VMs nasazené tento virtuální sítě fyzicky umístit východoasijských USA, například vyberte uvedeném místě. Není možné změnit oblasti přidružené k síti virtuální po jeho vytvoření.

4. Na stránce **servery DNS a připojením VPN** zadejte tyto informace a potom klikněte na další šipku v pravém dolním. 

    - **Servery DNS** – zadejte název serveru DNS a IP adresu nebo vyberte dříve registrované DNS server z místní nabídky. Toto nastavení serveru DNS nevytvoří. Umožňuje uživateli určit servery DNS, které chcete použít pro překlad pro tento virtuální sítě.
    - **Připojení k webu** – zaškrtněte políčko **Konfigurovat VPN na webu**.
    - **ExpressRoute** – zaškrtněte políčko **Použít ExpressRoute**. Tato možnost se zobrazí, jenom Pokud jste vybrali **Konfigurovat VPN k webu**.
    - **Místní síti** – je nutné mít místní síti web pro ExpressRoute. Však v případě připojení k ExpressRoute předpony adres určeným pro místní síti webu budou ignorovat. Místo toho předpony adres nabízené společnosti Microsoft prostřednictvím okruh ExpressRoute se použije pro účely směrování.<BR>Pokud už máte v místní síti vytvořené pro připojení k ExpressRoute, můžete vybírat z rozevíracího seznamu. Pokud ne, vyberte **použít nový místní síti**.

5. Pokud jste vybrali zadáte novou místní síť v předchozím kroku, zobrazí se stránka **připojení k webu** . Pro nastavení místní sítě, zadejte tyto informace a potom klikněte na Další šipky. 

    - **Název** - jméno, které chcete zavolat na místní (místní) síť webu.
    - **Adresní prostor** – včetně spuštění IP a CIDR (počet adresa). Všechny adresy oblasti můžete určit, dokud nepřekrývá rozsahem adres virtuální sítě. Obvykle to vhodné zadat rozsahy adres pro místní sítě, ale v případě ExpressRoute, nejsou použity tato nastavení. Toto nastavení se však vyžaduje k vytvoření místní síti při klasické portálu.
    - **Přidat adresní prostor** - toto nastavení se netýká ExpressRoute.


6. Na stránce **Virtuální adresu mezery sítě** zadejte následující informace a klikněte na zaškrtnutí v pravé dolní ke konfiguraci sítě. 

    - **Adresní prostor** – včetně spuštění počet IP a adresa. Ověřte, že mezery adresu zadanou nepřekrývaly některou z adresy mezery, které máte v místní síti.
    - **Přidat podsítě** – včetně spuštění IP adresu Count. Další podsítí není povinný.
    - **Přidat brány podsítě** - kliknutím vložíte podsítě brány. Podsítě brány se používá pouze pro brány virtuální sítě a je nutný pro tuto konfiguraci.<BR>Brána podsítě CIDR (počet adres) pro ExpressRoute musí být /28 nebo větší (/ 27, / 26 atd.). To umožňuje dost IP adres v této podsítě umožňuje konfigurace pro práci. Na portálu klasické Pokud jste zaškrtli políčko použít ExpressRoute, portálu určuje brány podsítě s /28.  Nemůžete upravovat počet adres CIDR klasické portálu. Podsítě brány se zobrazí jako **brány** na portálu klasické skutečně **GatewaySubnet**sice skutečné jméno podsítě brány, která se vytvoří. Tento název můžete zobrazit pomocí prostředí PowerShell nebo na portálu Azure.

7. Klikněte na zaškrtnutí v dolní části stránky a začne se vytvořit virtuální sítě. Až se dokončí, zobrazí se, že **vytvořeno** zařazený do kategorie **Stav** na stránce **sítí** klasické portálu.

## <a name="gw"></a>Vytvoření brány

1. Na stránce **sítí** virtuální sítě, který jste právě vytvořili, klepněte **řídicího panelu** v horní části stránky.

2. V dolní části stránky **řídicího panelu** klikněte na **Vytvořit brány** a vyberte **Dynamické směrování**. Klikněte na tlačítko **Ano** potvrďte, že chcete vytvořit bránu.

3. Po spuštění vytváření brány uvidíte odesílat zprávy, které víte, že byl spuštěn brány. Může trvat až 45 minut u brány na vytvořit.

4. Odkaz na okruh sítě. Postupujte podle pokynů v článku [jak propojit VNets ExpressRoute obvody](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurace existující klasické VNet pro ExpressRoute

Pokud už máte klasické VNet, můžete nakonfigurovat na připojení k ExpressRoute klasické portálu. Nastavení shodovaly oddílů nahoře, takže prostudujte si tyto části se seznamte s požadované nastavení. Pokud chcete vytvořit připojení existujících ExpressRoute/server-server, naleznete [v tomto článku](expressroute-howto-coexist-classic.md) kroky. Jsou jiné než kroků v tomto článku.
 
1. Je potřeba vytvořit místní síti před aktualizací zbytek nastavení VNet. Vytvoření nové místní síti, je potřeba při konfiguraci ExpressRoute prostřednictvím portálu klasické, klikněte na **Nový** **>** **Síť služby** **>** **Virtuální sítě** **>** **Přidat místní síti**. Postupujte podle pokynů průvodce k vytvoření místní síti.

2. Na stránce **Konfigurovat** a aktualizovat zbytek nastavení vašeho VNet přidružení VNet k místní síti.

3. Po konfiguraci nastavení, přejděte do části [Vytvoření brány](#gw) tohoto článku vytvořte bránu.


## <a name="next-steps"></a>Další kroky

- Pokud chcete přidat virtuálních počítačích k síti virtuální, přečtěte si článek [virtuálních počítačích naučné stezky](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Pokud chcete další informace o ExpressRoute, najdete v článku [Přehled ExpressRoute](expressroute-introduction.md).


 
