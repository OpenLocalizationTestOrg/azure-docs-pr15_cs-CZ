<properties
   pageTitle="Přehled zabezpečení Azure sítě | Microsoft Azure"
   description=" Tento článek usnadňuje porozumět tomu, co má Microsoft Azure nabízejí v části zabezpečení sítě. Připravili jsme základní vysvětlení core síťových zabezpečení konceptů a informace a požadavky na Azure má nabízet v každé z těchto oblastí. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Přehled zabezpečení Azure sítě

Microsoft Azure včetně robustní síťovou infrastrukturu na podporu aplikace a služby připojení požadavky. Připojení k síti je možné mezi prostředky umístěné v Azure, mezi místním a Azure hostovaný zdrojů a a od Internet a Azure.

Hledání v tomto článku je snadněji pochopit Microsoft Azure má nabízet v části zabezpečení sítě. Připravili jsme tady základní vysvětlení core síťových zabezpečení konceptů a požadavky. Také nabízíme můžete informace v Azure má nabízet v každé z těchto oblastí. Existuje spoustu odkazy na další obsah, který vám umožní získat hlubší porozumění pro oblasti, které vás zajímají.

Přehled zabezpečení sítě Azure článek se zaměřuje na následujících zdrojích:

- Azure sítě
- Řízení přístupu k síti
- Zabezpečené vzdálený přístup a křížově místní připojení
- Dostupnost
- Zapnout protokolování
- Překlad
- Architektura DMZ
- Centrum zabezpečení Azure

## <a name="azure-networking"></a>Azure sítě

Virtuálních počítačích třeba připojení k síti. K podpoře tohoto požadavku, vyžaduje Azure virtuálních počítačích k připojení k síti virtuální Azure. Virtuální sítě Azure je logické konstrukce této technologii postavená struktury fyzické Azure sítě. Každý logické virtuální síť Azure je izolovaný od všech dalších Azure virtuální sítí. Tato funkce pomáhá zajistit, že síťová komunikace ve vaší nasazení není přístupný pro ostatních zákazníků Microsoft Azure.

Víc se uč:

- [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Řízení přístupu k síti
Řízení přístupu k síti je příznaky omezení konektivitu a od určitá zařízení nebo podsítí v rámci virtuální sítě Azure. Cíl řízení přístupu k síti je abyste měli jistotu, že jsou dostupné pro pouze uživatelům a zařízením, do kterých chcete je přístupných osobám s postižením virtuálních počítačích a služeb. Přístup k ovládacím prvkům založené na povolit nebo odepřít rozhodnutí pro připojení k a od virtuálního počítače nebo služby.

Azure podporuje několik typů řízení přístupu k síti. Jedná se o:

- Ovládací prvek vrstvy sítě
- Směrování ovládací prvek a vynucená tunneling
- Virtuální sítě zabezpečení zařízení

### <a name="network-layer-control"></a>Ovládací prvek vrstvy sítě
Zabezpečené nasazení vyžaduje některé míra řízení přístupu k síti. Cíl řízení přístupu k síti je abyste měli jistotu, že virtuálních počítačích a síťových služeb spuštěné v operačním systému tyto virtuálních počítačích můžete komunikovat jenom jiná síťových zařízení, budou muset komunikovat a všechny ostatní pokusy o připojení blokovány.

Pokud potřebujete základní síťový řízení přístupu (na základě IP adresy a protokoly TCP a UDP), můžete skupiny zabezpečení sítě. Skupina zabezpečení síti (NSG) je základní paketů filtrování bránu firewall a umožňuje řízení přístupu na základě na [5 n-tici](https://www.techopedia.com/definition/28190/5-tuple). NSGs nenabízejí kontroly vrstvy aplikace nebo ověřený přístup k ovládacím prvkům.

Víc se uč:

- [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Ovládací prvek směrování a vynuceného Tunneling
Možnost řídit směrování chování v síti virtuální Azure je funkce kritické síťové zabezpečení a přístupu ovládacího prvku. Pokud směrování nakonfigurovaný nesprávně, aplikací a služeb hostovaných na počítači virtuální může připojit k zařízení nechcete, aby připojit, včetně zařízení vlastní a provozují potenciálních útočníků.

Azure sítě podporuje možnost přizpůsobit směrování chování v síti na virtuálních sítí Azure. Umožňuje změnit výchozí směrování tabulky položky ve svojí síti virtuální Azure. Ovládací prvek směrování chování vám pomáhá zkontrolujte, že všechny přenosy ze zařízení nebo skupinu zařízení zadá nebo ponechá virtuální síť Azure pomocí na určité místo.

Například pravděpodobně zabezpečení zařízení virtuální síť ve vaší síti virtuální Azure. Chcete, abyste měli jistotu, že všech příchozích a odchozích virtuální sítě Azure prochází tento virtuální zabezpečení zařízení. Lze provést pomocí konfigurace [Směruje definované uživatele](../virtual-network/virtual-networks-udr-overview.md) v Azure.

[Vynucená tunneling](https://www.petri.com/azure-forced-tunneling) je mechanismus, který umožňuje zajistit, aby vaše služby nebudou zahajte připojení k zařízení na Internetu. Všimněte si, že se jedná o liší od přijímat příchozí připojení a potom na ně reagovat. Webových front-end serverech potřebujete odpověď na žádost o z Internetu hosts a jsou povoleny tak Internet získaná přenosy příchozích zpráv na těchto serverech web a webových serverů jsou povoleny reagovat.

Co chcete povolit je front-end webový server a nejde podat žádost odchozí. Tyto požadavky může představují rizika zabezpečení, protože tato připojení lze použít ke stažení malware. I když chcete tyto front-end serverech zahajte odchozí požadavky na Internetu, můžete chtít, aby absolvovat proxy servery svůj místní web tak, aby mohli využívat adresu URL, filtrování a protokolování vynutit.

Místo toho chcete chcete umožňuje vynuceného tunneling předejít. Po povolení vynuceného tunneling všechna připojení k Internetu vynucená prostřednictvím místní brány. Můžete nakonfigurovat vynuceného tunneling využijete směruje definované uživatele.

Víc se uč:

- [Jaké jsou definované směruje uživatele a IP přesměrování](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuální sítě zabezpečení zařízení
Během skupiny zabezpečení sítě směruje definované uživatele a vynuceného tunneling umožňují úroveň zabezpečení na síť a transport vrstvy [OSI modelu](https://en.wikipedia.org/wiki/OSI_model), někdy může stát Pokud chcete povolit zabezpečení na úrovni vyšší než sítě.

Například můžou obsahovat vašim požadavkům na zabezpečení:

- A mohli ověřovat před povolení přístupu k aplikaci
- Průnik zjišťování a průnik odpověď
- Kontrola vrstvy aplikace uceleném protokoly
- Filtrování adres URL
- Úrovně antivirový program sítě a antimalware
- Ochrana proti bot
- Řízení přístupu aplikace
- Dodatečnou ochranu Denial (nad Denial ochrana předpokladu Azure struktury samotné)

Tyto funkce zabezpečení rozšířené sítě dostanete pomocí Azure partnerských řešení. Najdete nejnovější aktualizaci Azure partner network solutions zabezpečení návštěva [Webu Azure Marketplace](https://azure.microsoft.com/marketplace/) a vyhledáním "zabezpečení" a "zabezpečení sítě".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Křížové místní připojení a zabezpečeného vzdálený přístup
Instalace, konfigurace a správa vašim potřebám Azure zdroje zbývá dokončit vzdáleně. Kromě toho můžete chtít [hybridní IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) řešení, které jsou součástí místního nasazení a v Azure veřejné cloudu. Podobnému sledu vyžaduje zabezpečené vzdálený přístup.

Azure sítě podporuje následující scénáře zabezpečené vzdáleného přístupu:

- Připojení k síti virtuální Azure jednotlivých pracovních míst
- V místní síti připojení k síti Azure virtuální prostřednictvím sítě VPN
- V místní síti připojení k síti Azure virtuální prostřednictvím odkazu vyhrazené sítě WAN
- Připojení Azure virtuálních sítí k sobě

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Připojení k síti Azure virtuální jednotlivé pracovních míst
Může nastat situace, kdy chcete povolit jednotlivé vývojáři nebo operace týkající se pracovníků ke správě virtuálních počítačích a služby Azure. Například musíte mít přístup k virtuálního počítače v síti virtuální Azure a zásady zabezpečení neumožňuje RDP nebo SSH vzdálený přístup k jednotlivým virtuálních počítačích. V takovém případě můžete připojení k síti VPN čárky webu.

Připojení VPN čárky webu používá protokol [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) umožňuje nastavit soukromé a zabezpečené připojení mezi uživatelem a virtuální sítě Azure. Po připojení VPN, uživatel bude moct RDP nebo SSH přes VPN odkaz do libovolné virtuálního počítače v síti virtuální Azure (za předpokladu, že uživatel může ověřovat a znovu je povoleno).

Víc se uč:

- [Konfigurace čárky webu připojení k síti virtuální pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>V místní síti připojení k síti Azure virtuální prostřednictvím sítě VPN
Je vhodné celý podnikové síti nebo její části, připojit k virtuální sítě Azure. Toto je běžné v hybridním IT scénáře kde společnosti [rozšířit jejich místní datacentra do Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). V mnoha případech bude společnosti hostovat části služby v Azure a části místní, například při řešení zahrnuje webových front-end serverech Azure a back-end databáze místní. Tyto druh "křížově místní" připojení také usnadnění správy Azure nachází zdroje víc zabezpečit a povolit scénáře například rozšíření řadiče domény Active Directory do Azure.

Jedním ze způsobů k tomu je použít [VPN k webu](https://www.techopedia.com/definition/30747/site-to-site-vpn). Rozdíl mezi VPN k webu a VPN čárky webu je, že čárky webu VPN ke kterému je připojen jednoho zařízení Azure virtuální sítě během VPN k webu připojuje k virtuální sítě Azure celé síti (například místních sítě). Webu webu VPN k síti virtuální Azure v režimu vysokého zabezpečení IPsec tunelem VPN protokol.

Víc se uč:

- [Vytvoření správce prostředků VNet pomocí připojení VPN k webu pomocí portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Plánování a návrh Brána VPN](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Připojení k síti Azure virtuální vyhrazené sítě WAN odkazem v místní síti
Bod webů a webů webu připojení VPN platí pro povolení připojení mezi místním nasazení. Však v některých organizacích vezměte v úvahu, aby měl následující nevýhody:

- Připojení k síti VPN přesunout data přes Internet – to poskytuje tyto připojení k zabezpečení problémům souvisejících se přesunutí dat přes veřejnou síť. Kromě toho nemůže být zaručena spolehlivost a dostupnost pro připojení k Internetu.
- Připojení VPN k virtuální sítím Azure považovat, že šířka pásma omezena pouze pro některé aplikace a účely, jako jsou mimo maximální počet na kolem 200Mbps.

Organizacím, které potřebujete nejvyšší úroveň zabezpečení a dostupnost pro jejich křížově místní připojení obvykle pomocí vyhrazené sítě WAN odkazů připojení ke vzdálené weby. Azure poskytuje možnost používání funkce vyhrazené sítě WAN odkaz, který slouží k připojení k síti virtuální Azure v místní síti. Je to možné prostřednictvím Azure ExpressRoute.

Víc se uč:

- [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Připojení Azure virtuálních sítí k sobě
Je možné použít mnoho Azure virtuálních sítí pro vaše nasazení. Existuje řada důvodů, proč to můžete udělat. Z důvodů může být zjednodušení správy; jiné může být z bezpečnostních důvodů. Bez ohledu na to pracovat nebo při použití zdrojů v různých Azure virtuální sítích může nastat situace, kdy se má zdroje na všech sítí propojí.

Jednou z možností bude služby na jedné sítě virtuální Azure připojení ke službám na jiné Azure virtuální sítě tak, že "opakování zpět" přes Internet. Připojení by zahájen jedné sítě virtuální Azure, přejděte prostřednictvím Internetu a pak se vrátit do cíle Azure virtuální sítě. Tuto možnost zpřístupňuje připojení k zabezpečením inherentní všechny internetové komunikace.

Může být vhodnější vytvořit Azure Virtuální VPN na webu Network virtuální sítě Azure. Tento Azure Virtuální VPN na webu Network virtuální sítě Azure používá stejný protokol [režimu tunelem IPsec](https://technet.microsoft.com/library/cc786385.aspx) jako připojení VPN k webu křížově místních poštovních uvedených výše.

Výhodou používání Azure Virtuální VPN na webu Network virtuální sítě Azure je, že VPN připojení přes Azure sítě struktury; nepřipojí přes Internet. Nabízí další úroveň zabezpečení ve srovnání s webu na webu pomocí virtuální privátní sítě, která se připojují přes Internet.

Víc se uč:

- [Konfigurace připojení VNet VNet přes správce prostředků Azure a prostředí PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Dostupnost
Dostupnost je komponentu klíče rohu kteréhokoli programu zabezpečení. Pokud uživatelů a systémy mít přístup k potřebují pro přístup k v síti, služba považovat za ohroženo. Azure má síťové technologie, které podporují následující mechanismy vysoká dostupnost:

- Vyrovnávání zatížení protokolu HTTP
- Úrovně Vyrovnávání zatížení sítě
- Globální služby Vyrovnávání zatížení

Vyrovnávání zatížení je mechanismus navržený rovnoměrně z vnější distribuovat připojení mezi několika zařízeních. Cíle Vyrovnávání zatížení jsou:

- Zvýšení dostupnost – při načítání zůstatek připojení na několika zařízeních, nejméně jeden zařízení nemusí být k dispozici a služeb spuštěných na zbývající online zařízení můžete pokračovat v práci s obsahem služby
- Zvýšení výkonu – při načítání zůstatek připojení na několika zařízeních jednoho zařízení nemá umožní procesor přístupů. Místo toho zpracování a paměti požadavky pro podávání obsah šířit na několika zařízeních.

### <a name="http-based-load-balancing"></a>Vyrovnávání zatížení protokolu HTTP
Chcete, aby Vyrovnávání zatížení protokolu HTTP před těchto webových služeb pomůže zajistit odpovídající úrovně výkonu a dostupnost své oblíbené organizacím, které často spuštění webové služby. Na rozdíl od Vyrovnávání zatížení tradiční založené na síti rozhodnutí protokolu HTTP Vyrovnávání zatížení vyrovnávání zatížení vycházejí vlastnosti protokolu HTTP na síť a transport protokolů vrstvy.

Aby uživatel na základě HTTP Vyrovnávání zatížení pro webové služby Azure umožňuje brány aplikace Azure. Brána Azure aplikace podporuje:

- Na základě HTTP Vyrovnávání zatížení – rozhodnutí Vyrovnávání zatížení jsou na základě charakteristikami zvláštní protokolu HTTP
- Na základě souborů cookie relace spřažení – tato možnost zajišťuje, připojení k některému z serverů za tento Vyrovnávání zatížení zůstane nedotčený mezi klienta a serveru. To zajistí stability transakce.
- Převedení SSL – při vytvoření klienta připojení s Vyrovnávání zatížení, že je relace mezi klientem a vyrovnávání zatížení šifrovaná pomocí HTTPS (SSL /) protokol. Však pro zvýšení výkonu, máte možnost připojení mezi Vyrovnávání zatížení a webovým serverem za použití vyrovnávání zatížení protokolu HTTP (bez šifrování). Tím se označuje jako "SSL převzít" protože webových serverů za vyrovnávání zatížení není příjemnější procesor potřebného pomocí šifrování a proto by měla žádost o službu rychleji.
- Založené na adrese URL obsahu směrování – tato funkce umožňuje Vyrovnávání zatížení rozhoduje o kam předána připojení založené na cílovou adresu URL. To umožňuje mnohem větší flexibilitu než řešení, díky kterým načíst vyrovnávání rozhodnutí podle IP adres.

Víc se uč:

- [Přehled aplikací brány](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Úrovně Vyrovnávání zatížení sítě
Na rozdíl od Vyrovnávání zatížení protokolu HTTP Vyrovnávání zatížení úrovně rozhoduje zatížení vyrovnávání podle IP adresa a port (čísla TCP a UDP).
Můžete získat výhodách úrovni Vyrovnávání zatížení sítě v Azure pomocí služby Vyrovnávání zatížení Azure. Některé klíčové vlastnosti Vyrovnávání zatížení Azure patří:

- Vyrovnávání zatížení úrovně podle IP adresa a čísla portů
- Podpora protokolu vrstvy všechny aplikace
- Načtení zůstatků Azure virtuálních počítačích a cloud services instance rolí
- Lze použít pro internetové (Vyrovnávání zatížení externí) a jiných Internet protilehlé aplikací (interní Vyrovnávání zatížení) a virtuálních počítačích
- Koncový bod sledování, který určíte, pokud libovolnou z těchto služeb za vyrovnávání zatížení mít k dispozici

Víc se uč:

- [Internet protilehlé Vyrovnávání zatížení mezi více virtuálních počítačích nebo služeb](../load-balancer/load-balancer-internet-overview.md)
- [Přehled Vyrovnávání zatížení vnitřní](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globální služby Vyrovnávání zatížení
V některých organizacích moci nejvyšší úroveň dostupnost možné. Jedním ze způsobů kontaktovat tento cíl je hostování aplikací v globálně distribuované datacentrech. Při aplikace je hostovaný ve datacentrech nachází v celém světě, je možné pro celý geopolitické oblast nedostupná a pořád ještě aplikaci a spuštěnou.

Kromě dostupnost výhody, které dostanete tak, že hostování aplikací v globálně distribuované datacentrech můžete taky získat výkon. Tyto výhody výkon můžete získat pomocí mechanismus, který směruje žádosti o službu datacentra, které je nejbližší zařízení, které vyvíjí žádost.

Vyrovnávání zatížení globální vám může poskytnout obě tyto výhody. V Azure můžete získat výhody globální služby Vyrovnávání zatížení pomocí Azure přenosy správce.

Víc se uč:

- [Co je přenosy správce?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Zapnout protokolování
Protokolování na úrovni sítě je klíčové funkce pro všechny scénáři zabezpečení. V Azure se můžete přihlásit údaje pro skupiny zabezpečení sítě získat sítě úroveň protokolování informací. S NSG protokolování, získáte informace z:

- Protokolů auditování – tyto protokoly slouží k zobrazení všechny operace odesílání do předplatné Azure. Tyto protokolech jsou standardně a lze použít v portálu Azure.
- Protokoly událostí – tyto protokoly obsahují informace o jaké NSG pravidla byla použita.
- Čítač protokoly – tyto protokoly kancelářskou sponkou, kolikrát byl použitý každé pravidlo NSG odmítnout nebo přenosy.

[Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), výkonné datové nástroje vizualizace, můžete použít taky k prohlížení a analýze tyto protokoly.

Víc se uč:

- [Protokol analýzy sítě skupin zabezpečení (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Překlad
Překlad je důležité funkce pro všechny služby, který hostujete v Azure. Z hlediska zabezpečení vyzrazení funkci rozlišení název může vést k mohl přesměrování žádostí o z webů na jeho webu. Požadavky pro všechny služby cloudu hostované je zabezpečené překlad.

Existují dva typy překlad, které bude nutné adresy:

- Vnitřní překlad – interní překlad používanou služby na virtuálních sítí Azure nebo místní sítě. Názvy používané pro překlad interní nejsou dostupné přes Internet. Optimální cenného papíru je důležité, že schéma rozlišení název interního není dostupné pro externí uživatele.
- Externí překlad – externí překlad používanou uživatele a zařízení mimo místním prostředím a Azure virtuální sítě. Toto jsou názvy, které jsou viditelné k Internetu a slouží k přímé připojení ke cloudové služby.

Pro interní překlad máte dvě možnosti:

- Server Azure virtuální DNS sítě – při vytváření nové Azure virtuální sítě serveru DNS se vytvoří. Tento server DNS můžete jmen počítačích umístěn v této síti virtuální Azure. Tento server DNS není konfigurovatelné a spravuje správce Azure struktury a tím řešení rozlišení zabezpečené název.
- Přenesení serveru DNS – máte možnost vložení serveru DNS vlastního výběru ve vaší síti virtuální Azure. Tento server DNS mohou být že služby Active Directory integrované DNS server nebo vyhrazené řešení serveru DNS poskytovanou Azure partnera, kterou můžete získat z Azure Marketplace.

Víc se uč:

- [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)
- [Správa servery DNS používané virtuální sítě (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

U externích služeb překládá názvy DNS máte dvě možnosti:

- Hostovat svůj vlastní externí DNS server místně
- Hostovat externí server DNS u poskytovatele služeb

Mnoho velkých organizacích uspořádá svoje vlastní DNS servery místní. Je možné to proto mají sítě znalosti a globální informací o stavu k tomu nevyzve.

Ve většině případů je lepší hostování svoje služby DNS název rozlišení u poskytovatele služeb. Tito poskytovatelé služby mít všechno pod kontrolou sítě a globální informací o volném čase zajištění dostupnosti velmi vysoké rozlišení služeb název. Dostupnost pro je nezbytné služby DNS vzhledem k tomu, že pokud se nezdaří služby rozlišení názvů nikdo bude mít přístup vaší internetové služby.

Azure obsahuje vysoce dostupné a performant externí DNS řešení ve formě Azure DNS. Tento název externího rozlišení řešení využívá Celosvětová dostupnost infrastruktury služby Azure DNS. Umožňuje hostovat svoji doménu v Azure pomocí stejných přihlašovacích údajů, API, nástrojů a fakturace jako jiných Azure služeb. Jako součást Azure také dědí silné zabezpečení ovládacích prvků integrovaná platformu.

Víc se uč:

- [Přehled Azure DNS](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>Architektura DMZ
Mnoho velké organizace umožňuje DMZs segmentech jejich sítě k vytvoření zónu vyrovnávací paměť mezi Internetu a svých služeb. DMZ část sítě se považuje za zóny zabezpečení minimum a bez prostředky vysoké hodnoty jsou umístěná v tomto segmentu sítě. Zobrazí se obvykle síťových zabezpečení zařízení, které mají segmentu DMZ rozhraní a sítě jiné síťové připojení k síti, která má virtuálních počítačích a služby, které přijímat příchozí připojení z Internetu.

Existuje celá řada varianty DMZ návrh a rozhodnutí o nasazení DMZ, a potom jaký druh DMZ použijte, pokud se rozhodnete používat, podle vašim požadavkům na zabezpečení sítě.

Víc se uč:

- [Microsoft cloudovými službami a zabezpečení sítě](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Centrum zabezpečení Azure
Centrum zabezpečení pomáhá zabránit, zjišťování a odpovědět na hrozeb a poskytuje že lepší přehled o a publikum nemůže ovládat, zabezpečení Azure prostředků. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Centrum zabezpečení Azure vám pomůže optimalizovat a sledovat zabezpečení sítě tak, že:

- Poskytování sítě zabezpečení doporučení
- Sledování stavu konfigurace zabezpečení sítě
- Upozorňování na sítě hrozeb i na úrovni koncového bodu a sítě

Víc se uč:

- [Úvod k Centru zabezpečení Azure](../security-center/security-center-intro.md)
