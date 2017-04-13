<properties 
   pageTitle="Jaké jsou definované směruje uživatele a IP přesměrování?"
   description="Naučte se používat uživatele definované směruje (UDR) a IP přesměrování pro předávání přenosu k síti virtuální zařízení v Azure."
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

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Jaké jsou definované směruje uživatele a IP přesměrování?
Když přidáte k virtuální síti (VNet) v Azure virtuálních počítačích (VMs), zjistíte, že VMs můžou vzájemně komunikovat v síti automaticky. Nepotřebujete k určení brány, i když VMs jsou v různých podsítí. Platí pro sdělení VMs veřejné Internet a i do vaší místní síti hybridní spojení z Azure na vlastní datacentra nachází.

Tato cesta komunikace je možné, protože Azure používá řadu směruje systému, který definuje jak přenosu IP přetékat. Směruje systému řízení toku komunikaci v následujících situacích:

- Z v rámci stejné podsítě.
- Z podsítě na druhý ve VNet.
- Z VMs k Internetu.
- Z VNet do jiného VNet přes VPN brány.
- Z VNet k místní síti prostřednictvím sítě VPN brány.

Následující obrázek ukazuje jednoduché nastavení s VNet, dvě podsítí a několik VMs spolu s směruje systému, které umožňují IP přenosy na plovoucí dlaždice.

![Směruje systém v Azure](./media/virtual-networks-udr-overview/Figure1.png)

Přestože použití systému směruje usnadňuje přenosy automaticky pro nasazení, jsou případy, ve kterých chcete určit směrování paketů prostřednictvím virtuální zařízení. Můžete to vytvořením směruje uživatelem definované určit přesměrování paketů toku do určité podsítě přejděte na zařízení virtuální a povolit přesměrování IP pro OM spuštěný jako virtuální zařízení.

Následující obrázek znázorňuje příklad směruje definované uživatelem a předávání IP vynutit pakety odeslané na jednu podsítě od druhé kvůli absolvovat virtuální zařízení na třetí podsítě.

![Směruje systém v Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Uživatelsky definované směruje platí pouze pro přenosů z podsítě. Nelze vytvořit směruje můžete určit, jak přenosy stává podsítě z Internetu, například. Zařízení, které předáváte dál umožnění datových přenosů do také, nesmí být ve stejném podsítě kde přenos pochází. Vždy vytvořte samostatné podsítě pro vaše zařízení. 

## <a name="route-resource"></a>Směrování zdroje
Pakety jsou směrovány přes síť TCP/IP podle postupu tabulce určené v jednotlivých uzlech fyzické síti se systémem. Je tabulka směrování kolekci jednotlivé cest slouží k rozhodnutí, kam předávání paketů podle cílová IP adresa. Postup se skládá z následujících akcí:

|Vlastnost|Popis|Omezení|Co byste měli zvážit|
|---|---|---|---|
| Předpona adresy | Určení CIDR, ke kterému postup slouží k použití, například 10.1.0.0/16.|Musí být platný rozsah CIDR představující adresy veřejného Internet, Azure virtuální síť nebo místní datacentra.|Zkontrolujte, že **předponu adresy** neobsahuje **adresa dalšího směrování**, jinak pakety zadá ve smyčce přejdete ze zdroje na přesměrování bez dostat cílovou adresu. |
| Další typ směrování | Typ Azure směrování paketu by měly posílat. | Musí mít jednu z následujících hodnot: <br/> **Virtuální sítě**. Místní virtuální síť reprezentovat. Například pokud mají dvě podsítí, 10.1.0.0/16 a 10.2.0.0/16 ve stejné síti virtuální postupu pro každý podsítě v tabulce směrování bude mít hodnotu dalšího směrování *Virtuální*sítě. <br/> **Virtuální sítě brány**. Představuje brána VPN Azure S2S. <br/> **Internet**. Představuje výchozí brány Internet poskytovanou infrastruktury Azure. <br/> **Virtuální zařízení**. Představuje virtuální zařízení, které jste přidali do Azure virtuální sítě. <br/> **Žádná**. Představuje černé díry. Pakety předávané černé díry nebude přepošlou vůbec.| Zvažte použití typu **žádné** ukončíte paketů toku do zadaného umístění. | 
| Adresa dalšího směrování | Adresa dalšího směrování obsahuje IP adresu, kterou bude předávat paketů. Další směrování hodnoty jsou povoleny pouze v postupech, kde je další typ směrování *Virtuální zařízení*.| Musí být IP adresu, která je dostupná v rámci virtuální sítě použití směrování definované uživatelem. | Pokud na IP adresu představuje virtuálního počítače, zkontrolujte, jestli že povolíte [přesměrování IP](#IP-forwarding) v Azure OM. |

V prostředí PowerShell Azure některé z hodnot "NextHopType" mají jiné názvy:
- Virtuální sítě je VnetLocal
- Virtuální sítě brána je VirtualNetworkGateway
- Virtuální zařízení je VirtualAppliance
- Internet je Internet
- Žádná zadán žádný

### <a name="system-routes"></a>Směruje systému
Každý podsítě vytvořené v síti virtuální se automaticky přiřazovat k směrování tabulku, která obsahuje následující pravidla směrování systému:

- **Místní Vnet pravidlo**: Pokud chcete toto pravidlo se automaticky vytvoří pro každé podsítě v síti virtuální. Určuje, že je přímý odkaz mezi VMs v VNet a že intermediate přesměrování.
- **Pravidlo místního**: Toto pravidlo platí pro všechny přenosy určené rozsah adres místní a používá Brána VPN jako další cíl směrování.
- **Pravidla Internetu**: Toto pravidlo úchyty všechny přenosy určených k veřejnému Internetu (adresa předponu 0.0.0.0/0) a používá bránu internet infrastruktury jako přesměrování pro všechny přenosy určené k Internetu.

### <a name="user-defined-routes"></a>Směruje definované uživatelem
Pro většinu prostředí budou stačí směruje systém už definované Azure. Ale budete muset vytvořit tabulku směrování a přidat jeden nebo více směruje v určitých případech patří:

- Platnost tunneling na Internetu přes místní síti.
- Použití virtuální zařízení v prostředí Azure.

Ve výše uvedené scénáře budete muset vytvořit tabulku směrování a do ní přidávat uživatelem definované trasy. Máte víc tabulek směrování a stejné tabulky směrování může být přidružené k jedné nebo více podsítí. A každý podsítě můžete jenom přidruží k jednomu tabulky. Všechny VMs a cloudovým službám podsítě používá směrovací tabulky přidružené k této podsítě.

Podsítí závisí na systém trasy do tabulky směrování souvisí s podsítě. Po přidružení existuje, probíhá směrování založené na nejdelší předpona POZVYHLEDAT (LPM) mezi uživatelem definované cesty a trasy systému. Pokud existuje více než jeden trasa s stejné shodě LPM potom trasu je vybrán podle původnímu v následujícím pořadí:

1. Definovaný směrovat uživatele
1. Směrování BGP (při ExpressRoute)
1. Systém směrování

Informace o vytvoření směruje definované uživatelem, najdete v tématu [Vytvoření cesty a povolit přesměrování IP v Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Uživatelsky definované směruje platí pouze pro služby Azure VMs a cloudu. Například pokud chcete přidat virtuální zařízení brány firewall mezi místní síti a Azure, máte k vytvoření cesty uživatelem definované Azure směrování tabulkách, přepošle všechny přenosy adresní prostor místní virtuální zařízení. Můžete také přidat trasu definované uživatelem (UDR) do GatewaySubnet předat všechny přenosy z místního Azure prostřednictvím virtuální zařízení. Toto je poslední sčítání.

### <a name="bgp-routes"></a>Směruje BGP
Pokud máte připojení k ExpressRoute mezi místní síti a Azure, můžete povolit BGP se rozšíří postupy v místní síti Azure. Tyto BGP trasy se používají stejným způsobem jako směruje systému a trasy definované uživatelem v jednotlivých Azure podsítě. Další informace najdete v článku [Úvod ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Můžete nakonfigurovat prostředí Azure používat platnost tunneling přes síť místní vytvořením trasu definované pro podsítě 0.0.0.0/0 používající Brána VPN jako přesměrování. Však to funguje, jenom když používáte VPN brány není ExpressRoute. ExpressRoute vynuceného tunneling je použit prostřednictvím BGP.

## <a name="ip-forwarding"></a>Předávání IP
Jak popisují nad jedním z hlavní důvody k vytvoření cesty definovaný uživatele je přenosu virtuální zařízení. Virtuální zařízení je nic jiného než OM, které se spustí aplikace, která slouží ke zpracování v síti nějak, například bránu firewall nebo zařízení.

Tento virtuální zařízení OM mají dostávat příchozí přenos, který není určená k sobě. Pokud chcete povolit OM pro příjem vysílání adresovanou na jiná místa, musíte povolit přesměrování IP pro OM. Toto je Azure nastavení, nikoli hostovaného operačního systému.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [vytvořit směruje v modelu nasazení Správce zdrojů](virtual-network-create-udr-arm-template.md) a jejich přiřazení k podsítí. 
- Zjistěte, jak [vytvořit směruje v modelu klasické nasazení](virtual-network-create-udr-classic-ps.md) a jejich přiřazení k podsítí.
