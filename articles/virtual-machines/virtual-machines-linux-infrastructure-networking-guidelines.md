<properties
    pageTitle="Síťové infrastruktury pokyny | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro nasazení virtuální síť služby Azure infrastruktury."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Pokyny pro infrastrukturu sítě

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na principy plánování kroky virtuální sítě v Azure a připojení mezi existující prostředí místní.


## <a name="implementation-guidelines-for-virtual-networks"></a>Pokyny pro implementaci virtuálních sítí

Rozhodnutí:

- Jaký druh virtuální sítě potřebujete IT vytížení nebo infrastruktury hostitele (jen cloudu nebo více místní)?
- Křížové místní virtuálních sítí kolik adresní prostor potřebujete hostovat podsítí a VMs teď a rozumné rozšíření v budoucnu?
- Chystáte se vytvořit centralizované virtuální sítě nebo jednotlivé virtuální sítě pro každé pole Skupina zdroje?

Úkoly:

- Definování adresní prostor virtuálních sítí vytvořit.
- Definování sady podsítí a adresní prostor pro jednotlivá pole.
- Virtuální sítích křížově místní sadu definujte v místní síti adresního prostoru pro místní umístění, které je potřeba kontaktovat VMs v virtuální sítě.
- Práce s místním systémem sítě týmu zajistit příslušného směrování nakonfigurovaný při vytváření více místní virtuální sítě.
- Vytvořte virtuální sítě používat stejný systém názvů.


## <a name="virtual-networks"></a>Virtuální sítě

Virtuální sítě jsou potřebné pro podporu komunikace mezi virtuálních počítačích (VMs). Můžete definovat podsítí vlastní IP adresu, nastavení serveru DNS, filtrování zabezpečení a vyrovnávání zatížení s fyzické sítě. Pomocí [Brána VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) nebo [okruh Express směrování](../expressroute/expressroute-introduction.md)můžete Azure virtuálních sítí připojit k místní sítě. Další informace o [virtuálních sítí a jejich součástí](../virtual-network/virtual-networks-overview.md).

Pomocí skupin zdroje máte flexibilitu při navrhování součásti virtuální sítě. VMs můžete připojit k virtuální sítě mimo vlastní pole Skupina zdroje. Běžné přístup návrh bude vytvářet skupiny centralizované zdroje, které obsahují síťovou infrastrukturu vaše základní, které můžete spravovat běžné týmu. VMs a jejich nasazené na samostatné skupiny prostředků. Tento přístup umožňuje aplikace vlastníci přístup ke skupině zdroje, které obsahuje jejich VMs bez otevření přístup ke konfiguraci širší virtuální sítě prostředky.

## <a name="site-connectivity"></a>Připojení k webu

### <a name="cloud-only-virtual-networks"></a>Jen cloudu virtuálních sítí
Pokud místních uživatelů a počítačů nevyžadují probíhající připojení k VMs v Azure virtuální sítě, návrhu virtuální sítě je přímo vpřed:

![Základní jen cloudu virtuální síťový diagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Tento přístup je obvykle pro internetové úloh, třeba internetové webový server. Můžete spravovat tyto VMs pomocí SSH nebo připojení VPN čárky webu.

Protože se nepřipojujete k místní síti, jen Azure virtuálních sítí můžete použít libovolnou část soukromé adresní prostor IP. Adresní prostor může být stejný soukromé prostor, který je v použít místní.


### <a name="cross-premises-virtual-networks"></a>Křížové místní virtuálních sítí
Pokud místních uživatelů a počítačů vyžadují probíhající připojení k VMs v Azure virtuální sítě, vytvořte virtuální křížově místní sítě. Virtuální síťové připojení k síti místní s ExpressRoute nebo připojení VPN k webu.

![Křížové místní virtuální síťového diagramu](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

V této konfiguraci je Azure virtuální sítě v podstatě cloudové příponu v místní síti.

Protože se připojit k místní síti, více místní virtuální sítě musí používat část adresní prostor používané v organizaci, které jsou jedinečné. V stejným způsobem jako různé podnikové umístění jsou udělovány konkrétní IP adres podsítí stane Azure jinam jako rozšíření se na vaší síti.

Umožňuje přenosu ze sítě virtuální křížově místní síti místní paketů musíte nakonfigurovat nastavení předpony adres příslušné místní jako součást definici virtuální sítě v místní síti. V závislosti na adresní prostor virtuální sítě a sadu relevantních místního umístění může být mnoho předpony adres v místní síti.

Jen cloudu virtuální sítě můžete převést na více místní virtuální sítě, ale s největší pravděpodobností vyžaduje, abyste do nové IP virtuální sítě adresní prostor a Azure zdroje. Proto pečlivě zvažte, pokud je potřeba připojení k síti místní přiřadíte podsítě IP virtuální sítě.

## <a name="subnets"></a>Podsítí
Podsítí umožňují uspořádat prostředky, které se týkají, buď logicky (například jeden podsítě VMs přidružené k stejné aplikaci), nebo fyzicky (například jeden podsítě za pole Skupina zdroje). Můžete také využívat podsítě izolace technik pro zvýšené zabezpečení.

Křížové místní virtuálních sítí byste měli navrhnout podsítí s stejné konvence, které používáte pro místní zdroje. **Azure vždy použije první tři IP adres adresní prostor pro každého podsítě**. Chcete-li zjistit počet požadovaných podsítě adres, začněte tím, počítání VMs, které potřebujete, teď. Odhad pro budoucí růst a můžete určit velikost podsítě pomocí následující tabulky.

Počet VMs potřeby | Číslo posunuto hostitele potřeby | Velikost podsítě
--- | --- | ---
1 – 3 | 3 | / 29
4 – 11     | 4 | / 28
12-27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Pro normální místního podsítě hostitele adres podsítě n hostitele bitů maximálně 2<sup>n</sup> -2. Azure podsítě hostitele adres podsítě n hostitele bitů maximálně 2<sup>n</sup> – 5 (2 a 3 pro adresy používané Azure podsítí).

Pokud se rozhodnete podsítě velikost, která je příliš malá, je nutné nové IP a přeinstalujte VMs v podsítě.


## <a name="network-security-groups"></a>Skupiny zabezpečení sítě
Filtrování pravidel můžete použít na přenosy, která se zobrazí přes virtuální sítě pomocí skupin zabezpečení sítě. Je možné vytvářet podrobného filtrování pravidla pro zabezpečení prostředí virtuální sítě řízení příchozí a odchozí přenosy, zdrojové a cílové rozsahy IP adres, povolené porty atd. Skupiny zabezpečení sítě se dají použít na podsítí v rámci virtuální sítě nebo přímo do rozhraní dané virtuální sítě. Je vhodné mít určitou úroveň skupiny zabezpečení sítě přenosy v síti virtuální filtrování. Další informace o [Skupinách zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Další součásti sítě
Stejně jako u místních fyzické síťovou infrastrukturu, může obsahovat Azure virtuální sítě větší kontrolu nad podsítí a IP adres. Při návrhu aplikace infrastruktury, je vhodné začlenit některé z těchto dodatečný:

- [VPN bran](../vpn-gateway/vpn-gateway-about-vpngateways.md) - připojit Azure virtuální sítě pro jiné Azure virtuální sítě nebo připojení k síti místní pomocí připojení VPN k webu. Implementace Express směrování připojení pro vyhrazené a zabezpečené připojení. Můžete také přímý přístup uživatelům poskytnout připojení VPN čárky webu.
- [Služba Vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) - poskytuje Vyrovnávání zatížení provoz přenosů externích a interních podle potřeby.
- [Aplikace brány](../application-gateway/application-gateway-introduction.md) - služba Vyrovnávání zatížení HTTP Vyrovnávání zatížení ve vrstvě aplikací, poskytnutí některé další výhody pro webové aplikace spíše než nasazení Azure.
- [Přenosy správce](../traffic-manager/traffic-manager-overview.md) – rozdělení založené na DNS přenosy směrování koncových uživatelů na koncový bod nejblíže k dispozici aplikace umožňuje hostovat aplikace z Azure datacentrech v různých oblastech.


## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 