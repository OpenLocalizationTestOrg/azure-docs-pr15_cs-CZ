<properties 
   pageTitle="Plánování sítě VPN brány a návrh | Microsoft Azure"
   description="Další informace o plánování Brána VPN a návrh křížově místní hybridní a VNet VNet připojení"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Plánování a návrh Brána VPN

Plánování a návrh křížově místní a VNet VNet konfigurace může být jednoduché nebo složité podle potřeb sociální sítě. Tento článek vás provede základní informace o plánování a návrh.

## <a name="planning"></a>Plánování


### <a name="compare"></a>Možnosti připojení mezi místním nasazení

Pokud chcete k síti virtuální zabezpečené připojení místních webů, máte tři různé způsoby, jak to udělat: webu na webu, oprávnění čárky webů a ExpressRoute. Porovnání různých křížově místní připojení, které jsou dostupné. Možnosti, které zvolíte mohou být závislé na různé aspekty, jako například:


- Jaký druh výkon řešení vyžadují?
- Chcete komunikovat přes veřejnou Internet prostřednictvím zabezpečeného VPN nebo přes připojení k soukromé?
- Máte veřejnou IP adresu k dispozici?
- Plánujete prostřednictvím sítě VPN zařízení? Pokud ano, je kompatibilní?
- Jste připojení jen pomocí několika počítačů nebo budete chtít trvalé připojení na webu?
- Jaký druh VPN brány je potřebný pro řešení, které chcete vytvořit?
- Které brány SKU byste měli použít?


V následující tabulce můžete rozhodnout, nejlepší možností připojení pro vaše řešení.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Požadavky na brány tak, že typ VPN a SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Další informace o bráně skladové jednotky najdete v článku [nastavení brány VPN](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Agregační výkon podle typu SKU a VPN

Následující tabulka zobrazuje typy brány a odhadovaná agregační výkon. Odhadovaná agregační výkon pravděpodobně důležitých při rozhodování o faktor pro návrh.
Ceny liší skladové jednotky brány. Informace o cenách najdete v článku [Ceny VPN brány](https://azure.microsoft.com/pricing/details/vpn-gateway/). V této tabulce platí pro správce zdrojů a modelů klasické nasazení.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Podporované konfigurace podle typu SKU a VPN

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Pracovní postup

Následující seznam obsahuje běžné pracovní postup pro připojení cloudu:

1.  Navrhněte a plánování topologii připojení a seznam adres místo pro všechny sítí, ke kterým se chcete připojit.
2.  Vytvoření Azure virtuální sítě. 
3.  Vytvoření brány VPN pro virtuální sítě.
4.  Vytvoření a konfigurace připojení k místní síti nebo jiných virtuální sítí (v případě potřeby).
5.  Vytváření a konfigurace připojení čárky webu Azure VPN brány (v případě potřeby).
 

## <a name="design"></a>Návrh

### <a name="topologies"></a>Topologie připojení

Začněte tím, že prohlížíte diagramů v článku [O brány VPN](vpn-gateway-about-vpngateways.md) . Článek obsahuje základní diagramech modelů nasazení pro každou topologii (Správce prostředků nebo klasický) a který nasazení nástroje můžete použít k nasazení konfiguraci.   

### <a name="designbasics"></a>Základy návrhu

Následující části se zabývají základy brány VPN. Zvažte také [sítě omezení služeb](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Informace o podsítí

Při vytváření připojení, je nutné zvážit podsítě oblastí. Nesmí obsahovat překrývající se rozsahy adres podsítí. Překrývající se podsítě při jeden virtuální síť nebo místní umístění obsahuje stejné adresní prostor, který obsahuje jiné umístění. To znamená, že je třeba vaší sítě výkonní pracovníci sítích místní místního k doložka mimo rozsah použít pro svoji IP Azure adresování místo/podsítí. Potřebujete adresní prostor, který není použitý místní místní síti se systémem. 

Překrývající se podsítí, že je také důležité při práci s připojením k VNet VNet. Pokud vaší podsítě překrývat a IP adresy existuje v odesílání i cílová VNets, VNet VNet připojení se nezdaří. Azure nelze směrovat data VNet protože cílovou adresu je součástí odesílání VNet. 

VPN bran vyžadují určité podsítě s názvem brány podsítě. Všechny podsítě brány musí mít název GatewaySubnet správně fungovat. Udělali, nezapomeňte jiný název vaší podsítě brány a není nasazení VMs nebo jiný podsítě brány. V tématu [podsítí brány](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Informace o místní síti brány

Místní síti brány obvykle odkazuje místního pracoviště. Klasický nasazení modelu místní síti brány se nazývá webem místní síti. Při konfiguraci brány místní síti ji pojmenovat, zadejte veřejnou IP adresu zařízení místní VPN a zadejte předpony adres, které jsou v místního pracoviště. Azure sleduje předpony cílovou adresu pro v síti, konfiguraci, která jste zadali požádá o místní síti brány a přesměrovává pakety příslušným způsobem. V případě potřeby můžete změnit tyto předpony adres. Další informace najdete v tématu [bran místní síti](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>O typech brány

Výběr typu správné Brána pro topologii je naprosto zásadní. Pokud jste vybrali správný typ, bránu nebude fungovat správně. Typ brány určuje, jak brány samotné připojí a je požadovaná konfigurace nastavení pro nasazení modelu správce prostředků.

Typy brány jsou:

- Virtuální privátní sítě
- ExpressRoute

#### <a name="connectiontype"></a>O typech připojení

Každý konfigurace vyžaduje typu konkrétní připojení. Typy připojení jsou:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>O typech VPN

Každý konfigurace vyžaduje určitý typ VPN. Pokud se součtem dvě konfigurace, jako je vytvoření připojení k webu a čárky webu připojení k stejné VNet je nutné použít typu VPN, který splňuje požadavky na obou připojení.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

V následujících tabulkách zobrazit typ VPN mapy na každé konfigurace připojení. Zkontrolujte, jestli že typ VPN brány shoduje konfigurace, který chcete vytvořit. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>Virtuální privátní sítě zařízení pro připojení k webu

Konfigurace připojení k webu, bez ohledu na nasazení modelu, je třeba následující položky:

- Virtuální privátní sítě zařízení, které je kompatibilní se službou Azure VPN brány
- Veřejné IPv4 IP adresu, která není za překladu síťových adres

Budete muset experience konfigurací zařízení virtuální privátní sítě nebo se někdo můžete nakonfigurovat zařízení za vás. Další informace o počítače v síti VPN najdete v článku [o počítače v síti VPN](vpn-gateway-about-vpn-devices.md). Virtuální privátní sítě zařízení článek obsahuje informace o ověřený zařízení, požadavky pro zařízení, které nebyly ověřeny a odkazy na dokumenty konfigurace zařízení, pokud je k dispozici.

### <a name="forcedtunnel"></a>Zvažte vynuceného tunelem směrování

U většiny konfigurací můžete nakonfigurovat vynuceného tunneling. Vynuceného tunneling umožňuje přesměrovat nebo "platnost" všechny přenosy vazbou na Internetu zpět do místního pracoviště přes VPN k webu tunelem pro kontrolu a auditování. Toto je důležité zabezpečení požadavku na většině podnikové zásady. 

Bez vynuceného tunneling vazbou internetových adres VMs v Azure vždy prochází z Azure síťovou infrastrukturu přímo se k Internetu, bez možnost umožňuje zkontrolovat nebo auditování přenos. Neoprávněným přístup k Internetu, může vést k informacím a další typy porušení zabezpečení.

**Vynucená tunelování diagramu**

![Vynucená Tunneling připojení] (./media/vpn-gateway-plan-design/forced-tunnel.png "Vynucená tunneling")

V obou modelech nasazení a pomocí různých nástrojů, může být nastaveno vynuceného tunelového připojení. V tématu Další informace v následující tabulce. Jakmile nové články, nové modely nasazení a další nástroje dostupné pro tuto konfiguraci aktualizujeme v této tabulce. Při článek neexistuje, jsme propojit přímo ji z tabulky.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Další kroky

Naleznete v článcích [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md) a [O Brána VPN](vpn-gateway-about-vpngateways.md) pro další informace týkající se návrhu.

Další informace o nastavení konkrétní brány najdete v článku [O VPN brány nastavení](vpn-gateway-about-vpn-gateway-settings.md).




