<properties
   pageTitle="Přehled Azure virtuální sítě (VNet)"
   description="Informace o virtuálních sítí (VNets) v Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Přehled virtuální sítě

Azure virtuální síť (VNet) je formátovanými jako vlastní síti v cloudu.  Je logické izolace Azure cloudu vyhrazené k předplatnému. Plně můžete určit blok adresy IP, nastavení DNS, zásady zabezpečení a směrování tabulek v rámci této sítě. Můžete taky další segmentech vaší VNet do podsítí a spuštění Azure IaaS virtuálních počítačích (VMs) a/nebo [Cloudovým službám (PaaS role instance)](../cloud-services/cloud-services-choose-me.md). Virtuální sítě navíc můžete připojit k místní sítě pomocí jedné z [možností připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) k dispozici v Azure. V podstatě můžete rozbalit vaší sítě na Azure, s úplnou kontrolu na blok adresy IP s využitím škála enterprise Azure poskytuje.

Chcete-li lépe porozumět tomu VNets, podívejte se na na obrázku dole, který ukazuje zjednodušené místní síti.

![V místní síti](./media/virtual-networks-overview/figure01.png)

Na obrázku výše zobrazuje v místní síti připojení k veřejné Internetu přes směrovačem. Zobrazí se také bránu firewall mezi směrovačem a DMZ hostingu DNS server a serverové farmy web. Serverová farma web je rozloženy pomocí vyrovnávání zatížení hardwaru, který je vystaven na Internetu a spotřebovává zdroje z vnitřní podsítě. Vnitřní podsítě je oddělená od DMZ jinou bránu firewall a servery hosts Active Directory řadiče domény, databázový server a servery aplikace.

Stejné síti mohou umístěny v Azure, jak je znázorněno na následujícím obrázku.

![Azure virtuální sítě](./media/virtual-networks-overview/figure02.png)

Všimněte si, jak Azure infrastruktury převezme role směrovači umožňují přístup z vaší VNet veřejné Internet bez nutnosti jakékoli konfigurace. Brány firewall mohou být nahrazeny sítě skupin zabezpečení (NSGs) u každého jednotlivé podsítě. A vyrovnávání zatížení fyzické jsou nahrazeny internet vystaveného a interních Vyrovnávání zatížení v Azure.

>[AZURE.NOTE] Existují dva způsoby nasazení v Azure: klasické (označovaná taky jako správce služby správy) a Azure zdroje správce (ARM). Klasický VNets může přidat do skupiny spřažení nebo vytvořen jako místní VNet. Pokud máte VNet ve skupině spřažení, doporučujeme [přenesení místní VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Výhody virtuální sítě

- **Izolace**. VNets jsou úplně izolovaný od sebe. Který umožňuje vytvářet nesouvislé sítích pro vývoj, testování a výrobní, které používají stejné CIDR blok adresy.

- **Přístup k veřejnému Internetu**. Všechny IaaS VMs a PaaS instancí role v VNet přístup k Internetu veřejné ve výchozím nastavení. Řízení přístupu pomocí sítě skupin zabezpečení (NSGs).

- **Přístup k VMs v VNet**. Role instance PaaS a IaaS VMs lze spustit ve stejné síti virtuální a můžete připojit k sobě navzájem pomocí soukromé IP adresy, i když jsou v různých podsítí bez nutnosti konfigurace brány nebo použijte veřejnou IP adres.

- **Překlad**. Azure poskytuje interní překlad IaaS VMs a instance role PaaS nasazenou v vaší VNet. Můžete nasadit vlastní servery DNS a konfigurace VNet jejich použití.

- **Zabezpečení**. Přenosy zadávání a její ukončení virtuálních počítačích a PaaS role instancí v VNet můžete řídit pomocí skupin zabezpečení sítě.

- **Připojení**. VNets můžete být připojeni k sobě navzájem pomocí sítě bran nebo VNet prozkoumávání. VNets můžete být připojeni k místním datacentrech prostřednictvím sítě VPN na webu nebo Azure ExpressRoute. Další informace o připojení VPN k webu, navštěvujte blog [o VPN brány](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Další informace o ExpressRoute, navštěvujte blog o [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md). Další informace o VNet prozkoumávání, navštěvujte blog o [VNet prozkoumávání](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Zkontrolujte, že vytvoříte VNet před nasazením IaaS VMs nebo PaaS role instance Azure prostředí. Na základě ARM VMs vyžadují VNet a pokud nezadáte existující VNet, Azure vytvoří výchozí VNet, který může mít CIDR adresu blok kolidovat s místní síti. Vytváření možné se připojit k místní síti vaší VNet.

## <a name="subnets"></a>Podsítí

Podsítě rozsah IP adres v VNet je VNet můžete rozdělit na víc podsítí pro organizaci a zabezpečení. VMs a instance role PaaS používaný podsítí (stejném nebo jiném) v rámci VNet můžou vzájemně komunikovat bez žádnou další konfiguraci. Můžete taky nakonfigurovat směrování tabulek a NSGs do podsítě.

## <a name="ip-addresses"></a>IP adresy


Existují dva typy IP adres přiřazených zdrojů v Azure: *veřejných* a *privátních*. IP adresy povolit Azure zdrojům komunikovat s Internet a další služby Azure veřejné stejně jako [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/) [Rozbočovače Azure události](https://azure.microsoft.com/documentation/services/event-hubs/). Soukromé IP adresy umožňuje komunikaci mezi zdrojů v síti virtuální spolu s můžou být připojení přes VPN, bez použití internetové adresy IP adres.

Další informace o IP adresách v Azure, navštěvujte blog o [IP adres v virtuální sítě](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Vyrovnávání zatížení Azure

Virtuálních počítačích a cloudovým službám v síti virtuální můžete vystaven na Internetu pomocí vyrovnávání zatížení Azure. Aplikací line of Business, které jsou interní webových pouze lze rozloženy pomocí vyrovnávání zatížení vnitřní.

- **Vyrovnávání zatížení externí**. Vyrovnávání zatížení externí umožňuje poskytovat dostupnost role instance IaaS VMs a PaaS k nim získat přístup z veřejného Internetu.

- **Služba Vyrovnávání zatížení interní**. Vyrovnávání zatížení vnitřní umožňuje poskytovat dostupnost IaaS VMs a instance role PaaS k nim získat přístup z jiných služeb ve vaší VNet.

Další informace o vyrovnávání zatížení v Azure, navštěvujte blog o [načtení vyrovnávání přehled](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Skupina zabezpečení síti (NSG)

Můžete vytvořit NSGs příchozí a odchozí přístup k síti rozhraní (NIC), můžete řídit VMs a podsítí. Každý NSG obsahuje jednu nebo více pravidel určující též přenosy schválené nebo odepřen na základě Zdrojová IP adresa, zdrojového portu, cílová IP adresa a cílový port. Další informace o NSGs, navštěvujte blog o [Novinky skupinu zabezpečení sítě](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuální zařízení

Virtuální zařízení je jakékoli jiné OM ve vaší VNet, které se spouští softwaru na základě zařízení funkce, například bránu firewall, sítě WAN optimalizace nebo detekce průnik. Vytvoření trasu v Azure chcete směrovat přenosy pro vaši VNet prostřednictvím virtuální zařízení na používání jeho funkcí.

Například NSGs mohou sloužit k poskytnutí zabezpečení u svého VNet. Však NSGs poskytují vrstvu 4 seznamu řízení přístupu (ACL) na příchozí a odchozí pakety. Pokud chcete použít model 7 zabezpečení vrstvy, budete muset používat bránu firewall zařízení.

Virtuální spotřebiče, závisí na [cesty a předávání IP definované uživatelem](virtual-networks-udr-overview.md).

## <a name="limits"></a>Omezení
Existují limity počtu virtuálních sítí povolené v předplatné, [Azure sítě](../azure-subscription-service-limits.md#networking-limits) omezení další informace naleznete.

## <a name="pricing"></a>Ceny
Je není extra zdarma prostřednictvím virtuální sítí v Azure. Instance výpočetním spuštění v Vnet se bude účtovat standardní sazba podle popisu v [Azure OM ceny](https://azure.microsoft.com/pricing/details/virtual-machines/). [Virtuální privátní sítě brány](https://azure.microsoft.com/pricing/details/vpn-gateway/) a [veřejné adresy IP] (https://azure.microsoft.com/pricing/details/ip-addresses/) použitý v VNet bude také účtovaných standardní sazby.

## <a name="next-steps"></a>Další kroky

- [Vytvoření VNet](virtual-networks-create-vnet-arm-pportal.md) a podsítí.
- [Vytvoření OM v VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Informace o [NSGs](virtual-networks-nsg.md).
- Informace o [Směrování a předávání IP definované uživatelem](virtual-networks-udr-overview.md).
