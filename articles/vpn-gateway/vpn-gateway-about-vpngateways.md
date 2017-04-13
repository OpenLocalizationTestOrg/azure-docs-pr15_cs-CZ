<properties 
   pageTitle="Informace o Brána VPN | Microsoft Azure"
   description="Informace o připojení VPN brány Azure virtuálních sítí."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Informace o VPN brány


Brána virtuální sítě se používá k odeslání síťová komunikace mezi Azure virtuálních sítí a místního umístění a také mezi virtuální sítí v rámci Azure (VNet VNet). Při konfiguraci brány VPN, musíte vytvořit a konfigurace brány virtuální sítě a virtuální síťové připojení brány.

Správce prostředků nasazení modelu když vytvoříte zdroj brány virtuální sítě, zadáte několik nastavení. Jednou z požadovaná nastavení je "-GatewayType". Existují dva typy brány virtuální sítě: virtuální privátní sítě a ExpressRoute. 

Po odeslání v síti vyhrazené soukromé připojení použijete typ brány "ExpressRoute". Tím se taky nazývá ExpressRoute brány. Pokud v síti přenášena jsou zašifrované veřejné připojení, použijte typ brány "Vpn". Takto označujeme jako brána VPN. Webu na webu, čárky webu a VNet VNet připojení všech používat bránu VPN.

Každý virtuální sítě může mít jenom jednu bránu virtuální sítě podle typu brány. Můžete třeba mít jedné sítě virtuální bránu využívající - GatewayType ExpressRoute a ten, který používá - GatewayType Vpn. Tento článek se zaměřuje na Brána VPN. Další informace o ExpressRoute najdete v tématu [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Ceny

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Skladové jednotky brány

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Podrobnosti o bráně skladové jednotky najdete v článku [Skladové jednotky brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Pole Předpokládaná agregační výkon tak, že SKU

Následující tabulka zobrazuje typy brány a odhadovaná agregační výkon. V této tabulce platí pro správce zdrojů a modelů klasické nasazení.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Konfigurace brány VPN

Při konfiguraci brány VPN pokynů, které používáte, závisí na nasazení modelu, který jste použili k vytvoření virtuální sítě. Například pokud jste vytvořili svůj VNet pomocí klasické nasazení modelu, použijete pokyny a pokyny pro klasické nasazení modelu pro vytváření a konfigurace nastavení sítě VPN brány. Další informace o nasazení modelů najdete v článku [Principy správce zdrojů a modelů klasické nasazení](../resource-manager-deployment-model.md).

Připojení k síti VPN brány závisí na více zdrojů, u kterých budou nakonfigurována zvláštní nastavení. Většinu zdrojů je možné konfigurovat samostatně, i když musí být nakonfigurované v určitém pořadí v některých případech. Můžete začít, vytváření a konfigurace zdrojů pomocí jednoho nástroje Konfigurace, jako je portál Azure. Můžete pak později rozhodnete přepnout do jiného nástroje, například Powershellu, nakonfigurovat další zdroje informací nebo upravit stávající zdroje-li k dispozici. V současné době nejde nakonfigurujete každý zdroj a nastavení zdroje na portálu Azure. Pokyny v článcích pro každé připojení topologii určete, když je potřeba nástroji konkrétní konfigurace. Informace o jednotlivých zdrojů a nastavení brány VPN najdete v článku [o Brána VPN nastavení](vpn-gateway-about-vpn-gateway-settings.md).

V následujících částech obsahují tabulky, které seznamu:

- k dispozici nasazení modelu
- k dispozici konfigurační nástroje
- odkazy, které můžete přejít přímo na články, pokud je k dispozici

Pomocí diagramů a popisy vyberte topologii připojení, aby vyhovovaly vašim požadavkům. Diagramy zobrazit topologií hlavní podle směrného plánu, ale je možné vytvářet složitější konfigurace pomocí diagramy jako vodítko.

## <a name="site-to-site-and-multi-site"></a>Na webu a více webů

### <a name="site-to-site"></a>Na webu

Připojení k síti VPN webu (S2S) brány je připojení přes VPN IPsec/IKE (IKEv1 nebo IKEv2) tunelem. Tohoto typu připojení vyžaduje zařízení VPN nachází místní, které má veřejnou IP adresu přiřazenou a nenachází za službou NAT. Připojení S2S se dá použít pro více místní a hybridní konfigurace.   

![S2S připojení] (./media/vpn-gateway-about-vpngateways/demos2s.png "na webu")


### <a name="multi-site"></a>Více webů

Můžete vytvořit a nakonfigurovat brány připojení VPN mezi vaší VNet a více místní sítě. Když pracujete s víc připojení, je nutné použít typu RouteBased virtuální privátní sítě (dynamic Brána pro klasické VNets). Protože jedné brány VPN mohou obsahovat pouze VNet, všechna připojení prostřednictvím brány sdílet dostupné šířky pásma. Toto je někdy označovány jako "více webu" připojení.
 

![Připojení k více webu] (./media/vpn-gateway-about-vpngateways/demomulti.png "více webů")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Modely nasazení a metody webu webu a více webů

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet VNet

Připojení k jiné síti virtuální virtuální sítě je podobný připojení VNet k umístění webu místní (VNet VNet). Oba typy připojení se k zajištění zabezpečené tunelem pomocí IPsec/IKE používají VPN brány. Můžete dokonce kombinovat VNet VNet komunikace s konfigurací připojení více webu. Toto oprávnění umožňuje vytvořit topologie sítě kombinující křížově místní připojení s připojením mezi virtuální sítě.

Může být VNets, ke kterému se můžete připojit:

- ve stejném nebo jiném oblastech
- ve stejném nebo jiném předplatná 
- ve stejné modelů různých nasazení


![VNet VNet připojení] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet vnet")

#### <a name="connections-between-deployment-models"></a>Připojení mezi modely nasazení

Azure má nyní dvě modelů nasazení: klasické a správce prostředků. Pokud používáte Azure delší dobu, pravděpodobně máte Azure VMs a instance role spuštěné v klasickém VNet. Novější VMs a rolí instancí může běžet v VNet vytvořené ve Správci zdrojů. Můžete vytvořit spojení mezi VNets umožňuje zdrojů v jedné VNet přímo komunikovat s zdrojů v jiném.

#### <a name="vnet-peering"></a>Prozkoumávání VNet

Možná budete moct používat VNet prozkoumávání k vytvoření připojení k, dokud virtuální sítě splňuje požadavky na určitých. Prozkoumávání VNet nepoužívá Brána virtuální sítě. Další informace najdete v tématu [VNet prozkoumávání](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Modely nasazení a metody VNet VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Bod webu

Připojení k síti VPN čárky webu (P2S) brány umožňuje vytvořit zabezpečené připojení k síti virtuální z jednotlivých klientského počítače. Připojení k síti VPN na P2S překročení SSTP (Secure Socket Tunneling Protocol). Připojení P2S nevyžadují zařízení virtuální privátní sítě nebo IP adresa veřejného pracovat. Připojení VPN spuštěním z klientského počítače. Toto řešení je užitečné, když budete chtít připojit k VNet ze vzdáleného místa, jako je třeba z pro domácnosti nebo pro konference, nebo pokud máte jenom několik klienti, které je potřeba se připojit k VNet. Připojení P2S lze ve spojení s S2S připojení prostřednictvím stejné bráně VPN za předpokladu, že všechny konfigurace požadavky pro obě připojení jsou kompatibilní.


Připojení k ![bodu webu] (./media/vpn-gateway-about-vpngateways/demop2s.png "bod webu")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Modely nasazení a metod pro bod webu

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

V připojení ExpressRoute brány virtuální sítě nakonfigurovaný s typem brány "ExpressRoute", nikoli "Vpn". Další informace o ExpressRoute najdete v tématu [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Připojení k webu a ExpressRoute existujících

ExpressRoute je přímé vyhrazené propojení z vaší sítě WAN (ne přes veřejnou Internet) ke službě Microsoft Services, včetně Azure. Na webu virtuální privátní sítě přenosy přenese zašifrovaných veřejné Internetu. Je možné konfigurovat připojení VPN k webu a ExpressRoute pro stejný virtuální sítě má několik výhod.

Nastavení sítě VPN na webu jako bezpečné převzetí cesty pro ExpressRoute nebo používat virtuální privátní sítě – sítěmi pro připojení k serverům, které nejsou součástí síť, ale připojení přes ExpressRoute. Oznámení, že při této akci musí dvě brány virtuální sítě pro stejný virtuální sítě jej pomocí - GatewayType Vpn a druhý pomocí - GatewayType ExpressRoute.


![Coexist připojení] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Modely nasazení a metody S2S a ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Další kroky

Naplánování konfigurace brány VPN. V tématu [Plánování sítě VPN brány a návrh](vpn-gateway-plan-design.md) a [připojení k místní síti a Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
