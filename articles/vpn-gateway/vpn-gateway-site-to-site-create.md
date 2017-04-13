<properties
   pageTitle="Vytvořit virtuální sítě s připojení k webu Brána VPN na portálu Azure klasické | Microsoft Azure"
   description="Vytvoření VNet s připojením Brána VPN S2S pro více místní a hybridní konfigurací pomocí klasické nasazení modelu."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Vytvoření VNet s připojení k webu na portálu Azure klasické

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasický – klasické portálu](vpn-gateway-site-to-site-create.md)

Tento článek vás provede vytváření virtuální sítě a připojení k webu VPN brány k místní síti pomocí modelu klasické nasazení a portálu klasické. Připojení k webu se dá použít pro více místní a hybridní konfigurace.

![Diagram webu webů] (./media/vpn-gateway-site-to-site-create/site2site.png "na webu")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody pro připojení k webu

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace webů serveru. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace 

Pokud chcete propojení VNets, přečtěte si téma [Konfigurace připojení k VNet VNet pro klasické nasazení modelu](virtual-networks-configure-vnet-to-vnet-connection.md). Pokud chcete přidat připojení k webu VNet kterém už je připojení, najdete v článku [Přidání S2S připojení k VNet pomocí existujícího připojení VPN brány](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfigurace.

- Kompatibilní zařízení VPN a někoho, kdo je možné ji nakonfigurovat. Přečtěte si [téma VPN zařízení](vpn-gateway-about-vpn-devices.md). Pokud nejste obeznámení s konfigurací zařízení virtuální privátní sítě nebo neznají IP adresu oblastech umístěny v konfiguraci sítě vaší místní, budete muset koordinaci s někým, kdo může poskytnout tyto údaje za vás.

- Externě vystaveného veřejnou IP adresa pro zařízení s VPN. Tato IP adresa nebyla nalezena za službou NAT.

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Vytvořit virtuální sítě

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com/).

2. V levém dolním rohu obrazovky klikněte na **Nový**. V navigačním podokně klikněte na **Síť služby**a potom klikněte na **Virtuální sítě**. Klikněte na **Vytvořit vlastní** spustíte Průvodce konfigurací.

3. Pokud chcete vytvořit svůj VNet, zadejte nastavení konfigurace na následujících stránkách:

## <a name="Details"></a>Stránka Podrobnosti virtuální sítě

Zadejte následující informace:

- **Název**: název virtuální sítě. Například *EastUSVNet*. Tento virtuální síťový název budete používat při nasazení VMs a PaaS instance, nemusí má být název příliš komplikované.
- **Umístění**: umístění přímo souvisí s fyzické místo (oblast), kde se má zdroje (VMs) jsou uloženy. Pokud budete potřebovat VMs nasazené tento virtuální sítě fyzicky umístit *Východoasijských USA*, například vyberte uvedeném místě. Není možné změnit oblasti přidružené k síti virtuální po jeho vytvoření.

## <a name="DNS"></a>Servery DNS a stránky připojení VPN

Zadejte následující informace a potom klikněte na další šipku v pravém dolním.

- **Servery DNS**: Zadejte název serveru DNS a IP adresu nebo vyberte dříve registrované DNS server z místní nabídky. Toto nastavení serveru DNS nevytvoří. Umožňuje uživateli určit servery DNS, které chcete použít pro překlad pro tento virtuální sítě.
- **Konfigurace VPN na webu**: Zaškrtněte políčko **Konfigurovat VPN k webu**.
- **Místní síti**: místní síti představuje vaší fyzické místního pracoviště. Můžete vybrat místní síti, která jste dříve vytvořili, nebo můžete vytvořit novou místní síti. Pokud vyberete možnost použít místní síti, která jste dříve vytvořili, přejděte na stránce konfigurace **Místní sítě** a ověření správnosti VPN zařízení IP adresa (veřejné vystaveného adresy IPv4) zařízení virtuální privátní sítě.

## <a name="Connectivity"></a>Připojení k webu stránky

Pokud vytváříte novou místní síti, zobrazí se stránka **Připojení k webu** . Pokud chcete použít místní síti, která jste dříve vytvořili, tato stránka se nezobrazí v průvodci a můžete přesunout na další části.

Zadejte následující informace a potom klikněte na Další šipky.

-   **Název**: název, které chcete volat na místní (místní) sítě webu.
-   **Virtuální privátní sítě zařízení IP adresa**: veřejné vystaveného adresu IPv4 místní VPN zařízení, které používáte pro připojení k Azure. Zařízení VPN nebyla nalezena za službou NAT.
-   **Adresní prostor**: zahrnovat spuštění IP a CIDR (počet adresa). Zadejte adresu range(s), které chcete nechat zasílat prostřednictvím virtuální sítě bránu vaší místní místního pracoviště. Pokud cílová IP adresa spadá do oblasti, které tady zadáte, je směrovaná přes virtuální sítě brány.
-   **Přidat adresní prostor**: Pokud máte víc rozsahy adres, které chcete nechat zasílat prostřednictvím virtuální sítě brány, zadejte každou oblast další adresa. Můžete přidat nebo odebrat oblastí později na stránce **Místní síti** .

## <a name="Address"></a>Virtuální sítě adresu mezery stránky

Zadejte adresu oblast, kterou chcete použít pro síť virtuální. Toto jsou dynamické IP adresy (POKLESU) přiřazené VMs a ostatních rolí instancí nasazené tento virtuální sítě.

Je důležité zejména vybrat oblast nepřekrývá pomocí kterékoli z oblastí, které se používají v místní síti. Potřebujete koordinaci s správce sítě. Správce sítě může být nutné doložka mimo rozsah IP adres z místního adresní prostor sítě použít pro síť virtuální.

Zadejte následující informace a potom klikněte na zaškrtnutí v pravé dolní ke konfiguraci sítě.

- **Adresní prostor**: zahrnovat spuštění IP a počtu adresu. Ověřte, že mezery adresu zadanou nepřekrývaly některou z adresy mezery, ke kterým máte ve vaší místní síti.
- **Přidat podsítě**: zahrnout spuštění IP a počtu adresu. Další podsítí nejsou povinná, ale můžete chtít vytvořit samostatné podsítě pro VMs, které budou mít statické POKLES. Nebo můžete chtít mít vaše VMs v podsítě, která je oddělená od ostatních instancí role.
- **Přidat brány podsítě**: Kliknutím vložíte podsítě brány. Podsítě brány se používá pouze pro brány virtuální sítě a je nutný pro tuto konfiguraci.

Klikněte na zaškrtnutí v dolní části stránky a začne se vytvořit virtuální sítě. Až se dokončí, zobrazí se, že **vytvořeno** zařazený do kategorie **Stav** na stránce **sítí** v portálu klasické Azure. Po vytvoření VNet můžete nakonfigurujte bránu virtuální sítě.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Konfigurace brány virtuální sítě

Nakonfigurujte bránu virtuální sítě vytvořit zabezpečené připojení k webu. Přečtěte si téma [Konfigurace virtuální sítě brány na portálu Azure klasické](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Další kroky

Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Další informace v dokumentaci [virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/) .
