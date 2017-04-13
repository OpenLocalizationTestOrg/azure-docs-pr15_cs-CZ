<properties 
   pageTitle="Virtuální sítě VPN brány nejčastější dotazy k | Microsoft Azure"
   description="Brána VPN časté otázky. Nejčastější dotazy týkající se Microsoft Azure virtuální křížově místní připojení, hybridní konfigurace připojení a bran VPN"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>Brána VPN časté otázky

## <a name="connecting-to-virtual-networks"></a>Připojení k virtuální sítě

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Můžete připojit virtuálních sítí v různých oblastech Azure?
Ano. Ve skutečnosti je bez omezení oblast. Jeden virtuální můžete připojit k jiné síti virtuální ve stejné oblasti nebo v jiné oblasti Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Můžete připojit virtuální sítě jiné předplatné?
Ano.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Můžete připojit víc webů z jedné virtuální sítě?

Můžete se připojit k více webů pomocí Windows Powershellu a rozhraní REST API Azure. V části [více webů a VNet VNet připojení](#multi-site-and-vnet-to-vnet-connectivity) – nejčastější dotazy.
## <a name="what-are-my-cross-premises-connection-options"></a>Jaké mám možnosti připojení mezi místní?

Následující křížově místní připojení jsou podporovaná:

- [Na webu](vpn-gateway-site-to-site-create.md) – připojení VPN přes IPsec (IKE v1 a IKE v2). Tohoto typu připojení vyžaduje zařízení virtuální privátní sítě nebo RRAS.

- [Bodu-webu](vpn-gateway-point-to-site-create.md) – připojení VPN přes SSTP (Secure Socket Tunneling Protocol). Toto připojení nevyžaduje VPN zařízení.

- [VNet VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – tohoto typu připojení je stejná jako konfigurace webů pro web. Připojení k síti VPN na VNet VNet překročení IPsec (IKE v1 a IKE v2). Nevyžaduje VPN zařízení.

- [Více webu](vpn-gateway-multi-site.md) – to je variant konfiguraci webu na webu, která umožňuje připojit víc webů místní virtuální sítě.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute je přímé připojení k Azure z vaší sítě WAN, ne přes Internet veřejné. V tématu [Technický přehled ExpressRoute](../expressroute/expressroute-introduction.md) a [Nejčastější dotazy týkající se ExpressRoute](../expressroute/expressroute-faqs.md) pro další informace.

Další informace o připojení najdete v článku [O Brána VPN](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Jaký je rozdíl mezi připojení k webu a bod k webu?

Připojení **K webu** umožňují připojení mezi některé z počítače, najdete ve svém místním prostředí virtuální počítač nebo instanci rolí v rámci virtuální sítě, podle toho, jak budete chtít konfigurace směrování. Je Skvělá volba pro připojení k vždy dostupná více místních poštovních a je vhodné u hybridních konfigurací. Tohoto typu připojení závisí na spotřebiče IPsec virtuální privátní sítě (hardware nebo softwarová zařízení), které musí být nasazené na okraji sítě. Pokud chcete vytvořit tohoto typu připojení, musí mít požadovaný hardware VPN a externě vystaveného adresy IPv4.

**Bodu-webu** připojení umožňuje připojení z jednoho počítače z libovolného místa na jinou hodnotu umístěn v síti virtuální. Používá klienta VPN v poli systému Windows. Jako součást konfigurace čárky webu můžete nainstalovat certifikát a konfigurace balíček klienta VPN, který obsahuje nastavení, které umožňují počítače k připojení k virtuálního počítače nebo instanci rolí v rámci virtuální sítě. Je dobré když se chcete připojit virtuální síť, ale nejsou uložena místního. Je taky dobré možnost Pokud nebudete mít přístup k hardwaru virtuální privátní sítě nebo externě vystaveného adresu IPv4, které jsou potřeba pro připojení k webu. 

Virtuální sítě webu webu a čárky webu současně, můžete nakonfigurovat za předpokladu, že vytvořit připojení na webu s použitím VPN směrování textu pro brány. Typy VPN založené na směrování nazývají dynamické bran v modelu klasické nasazení.

### <a name="what-is-expressroute"></a>Co je ExpressRoute?

ExpressRoute umožňuje vytvářet soukromé připojení mezi datacentrech Microsoft a infrastrukturu, která je ve svém místním prostředí nebo v prostředí spolu umístění. ExpressRoute můžete vytvořit připojení ke cloudovým službám společnosti Microsoft jako je Microsoft Azure a Office 365 v zařízení společné umístění ExpressRoute partnera nebo přímo propojte stávající sítě WAN (například poskytovanou poskytovatele služeb sítě VPN MPLS).

Připojení ExpressRoute nabízí lepší zabezpečení, další spolehlivost, větší šířku pásma a čekacích dob nižší než typický připojení přes Internet. V některých případech můžete pomocí připojení ExpressRoute pro přenos dat mezi místní síti a Azure taky výnos výhod nákladů. Pokud jste už vytvořili připojení mezi místní z místní síti k Azure, můžete migrovat do připojení ExpressRoute zachováním virtuální sítě beze změny.

V tématu [Nejčastější dotazy týkající se ExpressRoute](../expressroute/expressroute-faqs.md) pro další podrobnosti.

## <a name="site-to-site-connections-and-vpn-devices"></a>Připojení k webu a počítače v síti VPN

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Co je nutné zvážit při výběru VPN zařízení?

Ověření sadu standardní zařízení VPN k webu ve spolupráci s dodavateli zařízení. Seznam známých kompatibilní zařízení VPN, odpovídající pokyny ke konfiguraci nebo vzorků a zařízení specifikace najdete [tady](vpn-gateway-about-vpn-devices.md). Všechna zařízení řady zařízení uvedeno známé kompatibilní měli spolupracovat s virtuální sítě. Vám pomohou při konfiguraci VPN zařízení, podívejte se do zařízení konfigurace vzorku nebo odkaz, který odpovídá příslušné zařízení řady.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Co mám dělat, když mám VPN zařízení, která není v seznamu známé kompatibilní zařízení?

Pokud nevidíte zařízení uvedeno známé kompatibilní VPN zařízení a můžete ji chtít použít pro připojení k síti VPN, bude nutné ověřit, že splňuje podporovaných IPsec/IKE konfigurace možnosti a parametrech uvedený [tady](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Zařízení splňuje minimální požadavky by měl použít i s VPN brány. Výrobce zařízení další podporu a konfigurace pokyny.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Proč moje zásadami VPN tunelem přejděte, době nečinnosti přenosy?

Toto je očekávaná zásadami (označovaná taky jako statické směrování) VPN brány. Když přenosy myší tunelem nečinnosti dobu delší než 5 minut tunelem bude být bylo odstraněno. Po spuštění toku v obou směrech přenosy tunelem se znovu vytvoří okamžitě.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Můžete používat software virtuální privátní sítě se připojit k Azure?

Konfigurace křížově místní webu webu podporujeme Windows serveru 2012 směrování a servery vzdáleného přístupu (RRAS).

Další software VPN by měl řešení s naše brány, dokud odpovídají odvětví standardní IPsec implementace. Dodavatele softwaru pokyny konfigurace a podpora.

## <a name="point-to-site-connections"></a>Připojení k bodu na serveru

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Jaké operační systémy můžete používat s čárky webu?

Podporuje následující operační systémy:

- Windows 7 (32bitové a 64bitové systémy)

- Windows Server 2008 R2 (pouze 64bitový)

- Windows 8 (32bitové a 64bitové systémy)

- Windows 8.1 (32bitové a 64bitové systémy)

- Windows Server 2012 (pouze 64bitový)

- Windows Server 2012 R2 (pouze 64bitový)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Můžete použít libovolný klienta VPN software pro bodu-server, který podporuje SSTP?

Ne. Podpora není omezena pouze na verze operačního systému Windows výše uvedené.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Kolik koncové body klienta VPN můžete definovat v konfiguraci čárky webu?

Podporujeme až 128 VPN klientům umožnit připojení k síti virtuální ve stejnou dobu.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Můžete použít vlastní interní KLÍČŮ kořenové CA připojení čárky webu?

Ano. Dříve lze použít pouze vlastních kořenových certifikátů. Pořád můžete nahrávat 20 kořenových certifikátů.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Můžete procházet proxy servery a brány firewall s použitím funkce čárky webu?

Ano. Tunelem bran používáme SSTP (Secure Socket Tunneling Protocol). Tento tunelem se zobrazí jako připojení HTTPs.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Pokud se znovu spustit klientský počítač nakonfigurován pro bod webu, bude sítě VPN automaticky znovu připojit?

Ve výchozím nastavení se klientský počítač obnovit připojení VPN automaticky.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Znamená čárky web podpory automatického připojit a DDNS v klientech virtuální privátní sítě?

Automatické připojení a DDNS aktuálně nepodporuje VPN čárky webu.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Můžete mít na webu a konfigurace čárky serveru být nainstalovány pro stejný virtuální sítě?

Ano. Obě tato řešení fungovalo, pokud máte typu RouteBased VPN brány. Ve klasické nasazení modelu třeba dynamická brány. V takovém není podpora bodu-server pro statické směrování bran virtuální privátní sítě nebo brány pomocí - VpnType PolicyBased.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Můžete nakonfigurovat čárky webu klientům připojení k víc sítí virtuální ve stejnou dobu?

Ano, je možné. Ale virtuálních sítí nelze překrývajícími se IP předpony a mezery adresu čárky webu nesmí překrývat mezi virtuální sítě.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Kolik výkon můžete očekávat přes připojení k webu nebo čárky webu?

Je obtížné udržovat přesné výkon tunelů VPN. IPsec a SSTP protokoly požadavkům těžké VPN. Výkon omezují také latence a šířku pásma mezi vaší místní organizaci a na Internetu.

## <a name="gateways"></a>Brány

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Co je brána zásadami (statického směrování)?

Zásadami bran implementovat zásadami VPN. Zásady VPN šifrování a přímé paketů tunely IPsec založené na kombinaci předpony adres mezi místní síti a Azure VNet. Zásady (nebo výběr přenosy) se obvykle rozumí seznamu aplikace access v VPN configuration.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Co je na základě směrování brány (dynamické směrování)?

Na základě směrování bran implementace směrování VPN. Postup připojení VPN pomocí "směruje" v IP předáváte dál nebo směrování tabulky na přímé pakety do jejich odpovídající tunelem rozhraní. Rozhraní tunelem potom šifrování nebo dešifrování paketů a odhlášení z jejich tunely. Výběr zásad nebo přenosy pro směrování na základě VPN jsou nakonfigurované tak, jako jakékoli na libovolného (nebo zástupné znaky).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Můžete získat Moje VPN brány IP adresa, před můžu vytvořit?

Ne. Budete muset vytvořit brána nejdřív zjistit IP adresu. Pokud odstraníte a znovu vytvořte bránu VPN změní IP adresa.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Jak se dostat ověření Moje tunelem virtuální privátní sítě?

Azure VPN používá ověřování PSK (Pre-Shared Key). Doporučujeme při vytvoříme tunelem VPN generovat předem sdílený klíč (PSK). Automaticky generované PSK můžete změnit na vlastní rutiny prostředí PowerShell Set klíč Pre-Shared nebo rozhraní REST API.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Dá se pomocí rozhraní API nastavit klíč Pre-Shared konfigurace Moje zásadami (statické směrování) brány virtuální privátní sítě?

Ano, rutinu nastavit Pre-Shared klíč rozhraní API aplikace a prostředí PowerShell lze konfigurovat Azure zásad (statická) VPN a směrování (dynamické) směrování VPN.

### <a name="can-i-use-other-authentication-options"></a>Můžete použít jiné možnosti ověřování?

Budeme se omezuje ověřování pomocí předem sdílené klíče (PSK).

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Co je "brány podsítě" a proč je potřeba?

Máme spuštění umožňující propojení mezi místní služba brány. 

Potřebujete vytvořit brány podsítě pro VNet konfigurace brány VPN. Všechny podsítě brány musí mít název GatewaySubnet správně fungovat. Není název vaší podsítě brány něco jiného. A nechcete nasazení VMs nebo jiný podsítě brány.

Minimální velikost podsítě brány zcela závisí na konfiguraci, které chcete vytvořit. Přestože je možné vytvořit nejnižší /29 pro některé konfigurace brány podsítě, doporučujeme, abyste vytvoření brány podsítě /28 nebo větší (/ 28, /27, /26 atd.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Můžete nasadit virtuálních počítačích nebo role instance Moje brány podsítě?

Ne.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Jak můžu určení přenosy prochází Brána VPN?

Pokud používáte klasické portálu Azure, přidejte každou oblast, která se má posílat přes bránu pro síť virtuální na stránce sítě klikněte v části místní sítě.

### <a name="can-i-configure-forced-tunneling"></a>Můžete nakonfigurovat vynucená Tunneling?

Ano. V tématu [Konfigurace vynucená tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Můžete nastavit vlastní server VPN v Azure a můžete se připojit k místní síti?

Ano, můžete nasadit vlastní VPN brány nebo servery v Azure z Azure Marketplace nebo vytvoření vlastní VPN směrovači. Je třeba konfigurovat směruje definované uživatelem v síti virtuální zajistit správně směrovat přenosy mezi místní sítě a podsítí virtuální sítě.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Proč jsou některé porty Moje Brána VPN otevřené?

Jsou potřeba pro Azure infrastruktury komunikace. Jsou chráněny (uzamčen dolů) Azure certifikáty. Bez správného certifikáty externí entity včetně zákazníci těchto bran nebude moct způsobit, že žádný vliv na tyto koncové body.

Brána VPN je zásadně více adresami zařízení s jedno klepnutí NIC do privátní sítě zákazníka a jeden NIC protilehlé veřejné sítě. Azure infrastruktury entity nemůžete klepněte do zákazníka privátní sítě důvodů dodržování předpisů, budou muset používat veřejné koncové body pro komunikaci infrastruktury. Veřejné koncové body jsou pravidelně kontrolován Azure zabezpečení auditování.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Další informace o typech brány, požadavky a výkon

Další informace najdete v tématu [Informace o nastavení brány VPN](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Více webů a VNet VNet připojení

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Jaký typ bran podporují více webů a VNet VNet připojení?

Pouze na základě směrování virtuální privátní sítě (dynamické směrování).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Můžete VNet připojit k typu RouteBased VPN a jiné VNet s typem PolicyBased VPN

Žádné, obojí virtuálních sítí používejte na základě směrování (dynamické směrování) virtuální privátní sítě.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Je přenosy VNet VNet zabezpečené?

Ano, je chráněn IPsec/IKE šifrování.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Cestovní VNet VNet přenosy přes Azure páteřní síti?

Ano.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Kolik místních webů a virtuálních sítí můžete jeden virtuální připojit k?

Max. 10 kombinované Basic a standardní dynamické směrování bran; 30 bran vysoký výkon VPN.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Můžete použít čárky web VPN s virtuální síť s více tunelů virtuální privátní sítě?

Ano, čárky webu (P2S) VPN mohou sloužit s připojení k víc místních webů a jiné virtuální sítě VPN brány.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Můžete nakonfigurovat více tunelů mezi virtuální síti a osobní web místní prostřednictvím sítě VPN více webu?

Ne, nadbytečné tunelů mezi síť virtuální Azure a místních webů nejsou podporované.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Můžete tam být překrývající se adresa mezery mezi připojení přes virtuální sítě a místní místních webů?

Ne. Překrývající se adresa mezery způsobí nahrávání souboru konfigurace sítě nebo "Vytváření virtuálních sítí" selhání.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Můžu získat větší šířku pásma s víc na webu pomocí virtuální privátní sítě než u jednoho virtuální sítě?

Ne, všechny tunelů virtuální privátní sítě VPN čárky webu, včetně sdílení stejné bráně Azure VPN a dostupné šířky pásma.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Dá se pomocí brána Azure VPN přechodu mezi weby v místním nebo do jiného virtuální sítě přenosy?

**Klasický nasazení modelu**<br>
Přenosy dopravní přes Azure VPN brány je možné pomocí klasické nasazení modelu, ale závisí na staticky definovaný adresu mezery v souboru konfigurace sítě. BGP není zatím podporované s Azure virtuální sítě a VPN brány pomocí klasické nasazení modelu. Bez BGP ručně definování dopravní adresu mezery je velmi chyby chybám a nedoporučuje.<br>
**Správce prostředků nasazení modelu**<br>
Pokud používáte nasazení modelu správce prostředků, naleznete v části [BGP](#bgp) Další informace.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Generuje Azure stejný IPsec/IKE předem sdílený klíč pro všechny mé sítě VPN pro stejný virtuální sítě?

Ne, Azure ve výchozím nastavení vytvoří různých předem sdílené klíče pro různá připojení VPN. Však můžete použít rutinu nastavit VPN brány klíč rozhraní REST API nebo prostředí PowerShell nastavení hodnoty klíče, kterému dáváte přednost. Klíč musí být alfanumerické řetězec délku mezi 1 až 128 znaků.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Azure účtovat poplatky za komunikaci mezi virtuální sítě?

Komunikace mezi různých Azure virtuálních sítí Azure poplatky jenom pro přenos procházení z Azure oblastí do jiného. Sazba nákladů je uvedená na stránce Azure [Ceny VPN brány](https://azure.microsoft.com/pricing/details/vpn-gateway/) .


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Můžete připojit virtuální sítě s IPsec VPN Moje ExpressRoute okruh?

Ano, to je podporovaná. Další informace najdete v tématu [Konfigurace ExpressRoute a připojení VPN k webu, které být nainstalovány](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Křížové místní připojení a VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Pokud virtuálního počítače probíhá virtuální sítě a mít více místní připojení, jak by měl připojit angličtině?

Máte několik možností. Pokud máte RDP povolené a vytvoření koncového bodu, můžete pomocí VIP připojit k počítači virtuální. V takovém případě zadáte VIP a portu, které se chcete připojit. Musíte nakonfigurovat port v počítači virtuální pro přenos. Obvykle byste měli přejděte na portál klasické Azure a uložení nastavení pro připojení RDP s vaším počítačem. Nastavení obsahují informace potřebné připojení.

Pokud máte virtuální sítě s připojením mezi místní nakonfigurovali, můžete pomocí interní DIP nebo soukromé IP adresu připojit k virtuálního počítače. Můžete taky připojit k počítači virtuální tak, že interní DIP z jiného virtuální počítač, který se nachází ve stejné síti virtuální. Nejde RDP do virtuálního počítače pomocí DIP, pokud se připojujete ze umístění mimo virtuální sítě. Například pokud máte čárky webu virtuální sítě nakonfigurované a nechcete vytvořit připojení z počítače, se nemůže připojit k virtuální počítač tak, že DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>V případě virtuálního počítače probíhá virtuální sítě s více místní připojení, se všechny přenosy na svůj OM absolvovat toto připojení?

Ne. Pouze data, která má v poli Cíl IP, který je součástí rozsahy virtuální sítě místní síti IP adres, které jste zadali budou přesměrovány prostřednictvím virtuální sítě brány. Přenosy má určení, které IP nacházejícího virtuální sítě nepřekročil virtuální sítě. Provoz odeslání prostřednictvím Vyrovnávání zatížení veřejných sítích, nebo pokud vynuceného tunneling, odeslaná prostřednictvím sítě VPN Azure brány. Pokud odstraňujete, je důležité, abyste měli jistotu, že máte všech rozsahů v místní síti, který chcete odeslat prostřednictvím brány. Ověřte, že adresa oblasti místní síti nepřekrývaly pomocí kterékoli z rozsahů adres v virtuální sítě. Navíc chcete ověřit, že je serveru DNS, který používáte řešení název odpovídající IP adresu.


## <a name="virtual-network-faq"></a>Virtuální sítě – nejčastější dotazy

Zobrazit další virtuální sítě informace v [Virtuální nejčastější dotazy týkající se sítí](../virtual-network/virtual-networks-faq.md).
 
