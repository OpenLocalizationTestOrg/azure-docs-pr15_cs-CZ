<properties
   pageTitle="Základní informace o BGP s Azure VPN bran | Microsoft Azure"
   description="Tento článek obsahuje přehled BGP s Azure VPN brány."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Základní informace o BGP s Azure VPN brány

Tento článek obsahuje přehled podpory BGP (ohraničení brány Protocol) v Azure VPN brány.

## <a name="about-bgp"></a>Informace o BGP

BGP je standardní směrování protokol běžně používaných v Internetu na exchange informace o směrování a dostupnosti mezi dvěma nebo víc sítí. Při použití v kontextu Azure virtuálních sítí, BGP umožňuje bran VPN Azure a zapojit místní zařízení VPN, označovaný taky jako BGP kolegové nebo sousední na exchange "přesměrovává", které informuje obě brány na dostupnost a dostupnosti pro tyto předpony projít bran nebo směrovači. BGP můžete taky povolit dopravní směrování mezi více sítí šíření trasy, které brány BGP učí z jedné mezi dvěma účastníky BGP jiných BGP kolegové.
 
### <a name="why-use-bgp"></a>Proč používat BGP?

BGP je volitelná funkce, které můžete použít pro Azure VPN založené na směrování brány. By měl taky zkontrolujte, že vaše zařízení VPN místní podporují BGP před povolit funkci. Můžete dál používat Azure VPN brány a zařízení místní VPN bez BGP. Jedná se o ekvivalent pomocí statické trasy (bez BGP) *a* pomocí dynamického směrování s BGP mezi sítě a Azure.

Mívají několik výhod nových funkcí s BGP:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Podpora a opakovaně dělá sám flexibilní předponu aktualizace

S BGP stačí deklarovat minimální předpony pro konkrétní partnera BGP přes IPsec S2S VPN tunelem. Může být nejnižší předponu hostitele (/ 32) BGP mezi dvěma účastníky IP adresy zařízení VPN místní. Můžete určit, které místní předpon sítě, který chcete inzerce Azure umožňuje Azure virtuální síti přístup.
    
Můžete taky přístupné větší předpony, která může mít adresu vaší VNet předpony například velké soukromé IP adresu mezeru (například 10.0.0.0/8). Poznámka: když předpony nesmí být shodný s některou z VNet předpony. Tyto směruje shodné VNet předpony bude odmítnuto.

>[AZURE.IMPORTANT] V současné době reklamní výchozí trasy (0.0.0.0/0) Azure VPN brány budou blokovány. Další aktualizace budou poskytovány, jakmile je tato možnost povolená.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Podpora více tunelů mezi VNet a místních webů s automatické převzetí podle BGP

Vytvoření více připojení mezi Azure VNet a místních VPN zařízení ve stejném umístění. Tato možnost obsahuje více tunelů (cesty) mezi dvěma sítěmi v konfiguraci aktivní. Pokud jeden z jejich tunely odpojeno odpovídajícím tras bude stažené prostřednictvím BGP a provoz se automaticky posunou tak, aby zbývající tunelů.
    
Na následujícím obrázku vidíte jednoduchý příklad vysoce dostupné instalace:
    
![Více aktivní cest](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Podpora směrování přenosu mezi místním sítí a více VNets Azure

BGP umožňuje několika bran šířit předpony z různých sítích, a zjistěte, jestli jsou přímo nebo nepřímo připojeni. To můžete povolit směrování přenosu s Azure VPN bran mezi weby v místním nebo ve více Azure virtuální sítích.
    
Na následujícím obrázku vidíte příklad vícenásobné topologie s více cest, které můžete přechodu komunikaci mezi dvěma sítěmi místní přes Azure VPN bran v rámci Networks Microsoft:

![Vícenásobné dopravní](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>Nejčastější dotazy týkající se BGP


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Další kroky

Najdete v článku [Začínáme s BGP na Azure VPN brány](./vpn-gateway-bgp-resource-manager-ps.md) postup konfigurace BGP křížové místní a VNet VNet připojení.

