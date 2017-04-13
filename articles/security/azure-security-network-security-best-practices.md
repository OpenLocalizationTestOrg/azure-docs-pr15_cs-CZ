<properties
   pageTitle="Doporučené postupy pro zabezpečení Azure sítě | Microsoft Azure"
   description="Tento článek obsahuje sadu osvědčené postupy pro používání zabezpečení sítě integrované Azure možnosti."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Doporučené postupy pro zabezpečení Azure sítě

Microsoft Azure umožňuje připojit virtuálních počítačích a zařízeních do jiných síťových zařízení tak, že zaškrtnete v Azure virtuální sítích. Virtuální síť Azure je konstrukce virtuální sítě, která umožňuje připojit virtuální síťových karet k síti virtuální umožňuje založené na/TCP/IP komunikace mezi povolené síťových zařízení. Připojení k síti virtuální Azure Azure virtuálních počítačích se mohou připojit k zařízení ve stejné Azure virtuální síti, jiné Azure virtuální sítích, na Internetu nebo dokonce v síti místní.

V tomto článku se probereme Tato kolekce doporučené postupy pro zabezpečení Azure sítě. Tyto doporučené postupy jsou odvozeny od Naše zkušenosti s Azure sítě a prostředí zákazníků jako sami.

Pro každý doporučený postup budete popisují jsme:

- Jaký je doporučený postup
- Proč chcete povolit tento doporučený postup
- Pokud se vám nepovede povolit doporučený postup co může být výsledek
- Možné alternativ doporučený postup
- Jak se dozvíte, chcete-li povolit doporučený postup

Tento článek doporučené postupy pro zabezpečení sítě Azure vychází shody názoru a Azure platformu funkce a funkce sady platných v době, kdy napsané v tomto článku. Názory a technologií pro server v průběhu času mění a v tomto článku se aktualizují v pravidelných intervalech, aby odrážely provedené změny.

Azure sítě doporučené postupy pro zabezpečení popisované v tomto článku, patří:

- Logicky podsítí segmentu
- Určit způsob směrování
- Povolení vynuceného Tunneling
- Použití virtuální sítě zařízení
- Nasaďte DMZs zón zabezpečení
- Vyhněte se vystavení Internet s vyhrazené sítě WAN odkazy
- Optimalizace provozu a výkon
- Použití služby Vyrovnávání zatížení globální
- Vypnutí RDP přístupu k Azure virtuálních počítačích
- Centrum zabezpečení povolit Azure
- Rozšíření vaší datacentra do Azure


## <a name="logically-segment-subnets"></a>Logicky podsítí segmentu

[Azure virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) se podobají LAN ve vaší místní síti. Cílem virtuální sítě Azure je, že vytvoříte jednoho soukromé IP adresu založené na místo síť, na které můžete provádět všechny vaše [Azure virtuálních počítačích](https://azure.microsoft.com/services/virtual-machines/). Soukromé IP adresu mezery k dispozici jsou v předmětu A (10.0.0.0/8), tříd B (172.16.0.0/12) a třídy C oblasti (192.168.0.0/16).

Co podobně jako do místní, je vhodné rozdělit do podsítí větší adresní prostor. Principy podsítí [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) na základě slouží k vytvoření vaší podsítě.

Směrování mezi podsítí proběhne automaticky a nepotřebujete ruční konfigurace směrovací tabulky. Ve výchozím nastavení je ale, že nejsou žádné ovládací prvky Accessu sítě mezi podsítí, které vytvoříte v síti virtuální Azure. Chcete-li vytvořit síť přístup k ovládacím prvkům mezi podsítí, musíte něco mezi podsítí umístění.

Jednou z věcí, na které můžete použít k provedení tohoto úkolu je [Skupiny zabezpečení síti](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs jsou jednoduché paketů kontroly zařízení, které používají 5-n-tici (zdroj IP zdrojového portu, cílová adresa IP, cílový port a protokol vrstvy 4) přístup k vytvoření povolit nebo odepřít pravidla pro v síti. Můžete povolit nebo odepřít příchozích a odchozích adresy IP a od více IP adres nebo dokonce a z celého podsítí.

Použití NSGs řízení přístupu k síti mezi podsítí umožňuje umístění zdroje, které patří do stejné zóny zabezpečení nebo role ve své vlastní podsítí. Například, že aplikace jednoduché 3 osy, který má web osy, aplikační logiky vrstvy a úroveň databáze. Umístěte virtuálních počítačích, které patří do všech těchto úrovní do své vlastní podsítí. Pak můžete používat NSGs k řízení komunikaci mezi podsítí:

- Web osy virtuálních počítačích může jenom připojit k použití logických operátorů počítačích aplikace a pouze jsou přijatelné připojení z Internetu
- Použití logických operátorů virtuálních počítačích můžete zahájit pouze připojení k databázi osy a pouze jsou přijatelné připojení od osy web
- Databáze osy virtuálních počítačích nelze navázat spojení s nic mimo vlastní podsítě a pouze jsou přijatelné připojení z logických vrstvy aplikace

Další informace o skupiny zabezpečení sítě a jak je můžete používat k logicky segmentech virtuálních sítí Azure, přečtěte si článek [Co je skupinu zabezpečení sítí](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Určit způsob směrování

Když vložíte virtuálního počítače v síti virtuální Azure, všimnete virtuálního počítače můžete připojit k jiné virtuálního počítače na stejné Azure virtuální sítě, i když virtuálních počítačích jsou umístěná na různých podsítí. Důvod, proč je možné je, že kolekci systému cest, kteří mají povolenou ve výchozím nastavení, které umožňují tento typ komunikace. Tyto výchozí trasy povolit virtuálních počítačích ve stejné síti Azure virtuální zahajte připojení mezi sebou a k Internetu (pro odchozí komunikaci k Internetu).

Výchozí systém trasy jsou užitečné pro mnoho scénáře nasazení, existují situace, pokud chcete přizpůsobit směrování konfiguraci vašeho nasazení. Vlastní nastavení vám umožní konfigurace adresa dalšího směrování ke komunikaci s konkrétní cíli.

Doporučujeme, abyste při nasazení spotřebiče zabezpečení virtuální sítě, které podíváme na pozdější doporučený postup konfigurovat směruje definované uživatele.

> [AZURE.NOTE] uživatel definované směruje není povinný a výchozí systém trasy budou fungovat ve většině případů.

Další informace o uživateli definované cesty a jak nakonfigurovat tak, že čtení v článku [Co je uživatel definované směruje a přesměrování IP](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Povolení vynuceného Tunneling

Pochopíte lépe vynuceného tunneling, je užitečné, pokud chcete zjistit, jaké "rozdělené tunneling" je.
Nejběžnější příklad rozdělení tunneling dochází sítě VPN. Představte si vytvořit připojení VPN z hotelu místnosti k podnikové síti. Toto připojení umožňuje připojení k prostředkům ve vaší podnikové síti a všechny komunikace k prostředkům ve vaší podnikové síti přejděte tunelem VPN.

Co se stane, když se chcete připojit k prostředkům na Internetu? Při zapnuté funkci rozdělení tunneling, tyto připojení přejděte přímo na Internetu a ne tunelem VPN. Některé odborníků zabezpečení zvažte jej možné riziko a proto doporučujeme rozdělit tunneling zakázané a všechna připojení, uživatelům, určené pro Internet a uživatelům, určené pro podnikové prostředky, přejděte tunelem VPN. Výhodou to je to, že připojení k Internetu se pak vynucená prostřednictvím zabezpečení zařízení podnikové síti, která by tomu Pokud klienta VPN připojení k Internetu mimo tunelem VPN.

Teď Pojďme přenést tomto zpět na virtuálních počítačích v síti virtuální Azure. Výchozí trasy virtuální sítě Azure povolit virtuálních počítačích zahajte přenosy k Internetu. To moc může výkres znázorňovat rizika zabezpečení a tyto odchozí připojení může zvýšit napadení virtuálního počítače využít útočníků.
Proto doporučujeme povolit vynuceného tunneling na virtuálních počítačích obsahujících křížově místní připojení mezi Azure virtuální sítě a v místní síti. Bude budeme křížové místní připojení dále v tomto Azure sítě osvědčené postupy dokumentu.

Pokud nemáte křížového místní připojení, ujistěte se, můžete využít sítě skupin zabezpečení (bylo popsáno dříve) nebo Azure virtuální sítě zabezpečení zařízení (popsané Další) nechcete, aby odchozí připojení k Internetu z vaší Azure virtuálních počítačích.

Další informace o vynucená tunneling a jak ji povolit, přečtěte si článek [Konfigurace vynucená Tunneling pomocí prostředí PowerShell a správce prostředků Azure](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Použití spotřebiče virtuální sítě

Během definované směrování uživatele a skupiny zabezpečení sítě umožňují míru sítě zabezpečení na síť a transport vrstvy [OSI model](https://en.wikipedia.org/wiki/OSI_model), tam budou nastat situace, kdy budete nechcete nebo nepotřebujete povolit zabezpečení na vysoké úrovni zásobníku. V těchto situacích doporučujeme nasazení virtuální sítě zabezpečení zařízení poskytovanou Azure partnery.

Azure sítě zabezpečení zařízení můžete poskytování výrazně rozšířené úrovně zabezpečení přes co poskytuje společnost úrovně ovládací prvky sítě. Možnosti zabezpečení sítě poskytované virtuální sítě zabezpečení zařízení patří:

- Fungující brána firewall může
- Průnik zjišťování/průnik zabránění
- Správa zabezpečení
- Řízení aplikace
- Založené na síti odchylky zjišťování
- Filtrování webové
- Antivirový program.
- Ochrana Botnet

Pokud požadujete vyšší úroveň zabezpečení sítě, než lze získat s ovládacími prvky sítě úroveň přístupu, pak doporučujeme prošetřit a nasazení Azure virtuální sítě zabezpečení zařízení.

Informace o jaké Azure virtuální sítě upozornění zabezpečení zařízení jsou k dispozici a o možnostech, navštivte [Azure Marketplace](https://azure.microsoft.com/marketplace/) a vyhledejte "zabezpečení" a "zabezpečení sítě".

##<a name="deploy-dmzs-for-security-zoning"></a>Nasaďte DMZs zón zabezpečení
DMZ nebo "obvod síť" je segmentu fyzické nebo logické sítě, který poskytuje další úroveň zabezpečení mezi vaší majetek i na Internetu. Záměr DMZ je umístit specializované síťových zařízení řízení přístupu na okraji DMZ sítě tak, že jsou povoleny pouze požadované přenosy přes síťové zabezpečení zařízení a do sítě virtuální Azure.

DMZs je užitečné, protože se můžete zaměřit řízení řízení přístupu vaší sítě sledování, protokolování a oznámení na zařízeních na okraji virtuální sítě Azure. Tady můžete obvykle umožní Denial zabránění, zabránění duplicit/průnik průnik systémy (ID/IP adresy), pravidla brány firewall a zásady, filtrování webové, antimalware sítě a více. Síťových zařízení zabezpečení sednout mezi Internetu a Azure virtuální sítě a mít rozhraní v obou sítích.

Když to je základní návrhu DMZ, existují mnoho různé designy DMZ jako jeden list papíru tri adresami, více adresami a další.

Doporučujeme, abyste pro všechna nasazení důkladné zabezpečení zvažte nasazení DMZ zvýšit úroveň zabezpečení sítí pro vaše Azure zdroje.

Další informace o DMZs a způsobu jejich nasazení v Azure, přečtěte si článek [Cloudovým službám společnosti Microsoft a zabezpečení sítě](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vyhněte se vystavení Internet s vyhrazené sítě WAN odkazy
Mnoho organizací zvolili směrování hybridní IT. V hybridním IT některé majetku firmy informace jsou v Azure, zatímco ostatní zůstanou místní. V mnoha případech některé součásti služby bude běžet v Azure zatímco jiné součásti zůstanou místní.

V hybridním IT scénář je obvykle určitý typ křížově místní připojení. To křížově místní připojení umožňuje společnosti k jejich místní síti virtuální sítím Azure. K dispozici jsou dvě křížově místní připojení řešení:

- Na webu VPN
- ExpressRoute

[Na webu VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) představuje virtuální privátní připojení mezi místní síti a virtuální sítě Azure. Toto připojení probíhá přes Internet a umožňuje informace "tunelem" uvnitř šifrované propojení mezi síť a Azure. Zabezpečené zdokonaleným technologie, které byly nasazeny podniky všech velikostí pro desetiletí představují VPN k webu. Šifrování tunelem se provádí pomocí [režimu tunelem IPsec](https://technet.microsoft.com/library/cc786385.aspx).

Technologie důvěryhodných spolehlivý a stanovené při VPN k webu přenosy v tunelem přecházet na Internetu. Kromě toho šířky pásma je poměrně omezena pouze maximálně o 200Mbps.

Pokud nastavíte výjimečné úroveň zabezpečení nebo vlastností pro více místní připojení, doporučujeme použít Azure ExpressRoute pro více místní připojení. ExpressRoute je vyhrazený WAN propojení mezi místního pracoviště nebo poskytovatelem hostingu Exchange. Protože telco připojení dat není cestovní přes Internet a proto, nebude vystaven potenciální rizika spojená s internetové komunikace.

Další informace o fungování Azure ExpressRoute a nasazení, přečtěte si článek [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimalizace provozu a výkon
Důvěrnost, integrity a dostupnosti (CIA) zahrnují chaloupka aktuálnímu největší vliv model zabezpečení. Je důvěrnost informací o šifrování a ochrany osobních údajů, integrity je o tom, jak zajistit, že data nezmění neoprávněným uživatelem a dostupnosti je o tom, jak zajistit, že ověření uživatelé budou moct pracovat s informacemi, které jsou oprávnění pro přístup k. Chyba instalace v některé z těchto oblastí představuje potenciální porušení zabezpečení.

Dostupnost si můžete představit jako provozu a výkon. Pokud je služba dolů, nelze získat informace o. Pokud je to špatné tak, aby byla data nelze použít výkon, můžete nám zvažte data, která mají být nedostupné. Proto z hlediska zabezpečení jsme musíte udělat něco jiného, co můžeme musí jednat služeb společnosti Microsoft optimální provozu a výkonu.
Oblíbené a efektivní způsob lze zlepšit dostupnost a výkon, je použít vyrovnávání zatížení. Vyrovnávání zatížení je způsob, jak distribuce v síti na serverech, které jsou součástí služby. Například pokud máte v rámci služby webových front-end serverech, můžete Vyrovnávání zatížení distribuce komunikace mezi více webových front-end serverů.

Toto rozdělení přenosy zvyšuje dostupnost, protože pokud jeden z webových serverů nebude k dispozici, Vyrovnávání zatížení zastavit odesílání přenosy na tento server a přesměrujete přenosy na servery, které jsou pořád online. Vyrovnávání zatížení také pomáhá výkon, protože procesor, sítě a paměti, že režijních pro podávání žádosti o rozvržena všech zatížení rovnováha servery.

Doporučujeme, aby se využívat Vyrovnávání zatížení kdykoli je to možné a podle potřeby pro své služby. Budeme se adresa vhodnosti v následujících částech.
Na úrovni virtuální sítě Azure Azure poskytuje že s tři primární načtete vyrovnávání možnosti:

- Vyrovnávání zatížení protokolu HTTP
- Externí Vyrovnávání zatížení
- Vnitřní Vyrovnávání zatížení

## <a name="http-based-load-balancing"></a>Vyrovnávání zatížení protokolu HTTP
Vyrovnávání zatížení protokolu HTTP počítá rozhodovat, jaký server odeslat připojení pomocí protokolu HTTP vlastnosti. Azure má Vyrovnávání zatížení HTTP, který odkazuje na název aplikace brány.

Doporučujeme, aby se nám Azure aplikace brány při:

- Aplikace, které vyžadují žádosti o stejné uživatelské/klientské relace k dosažení stejném počítači virtuální back-end. Příklady to by nákupní košík apps a webové poštovní servery.
- Aplikace, které chcete uvolnit webové serverové farmy shora ukončení SSL využijete funkce [SSL převzít](https://f5.com/glossary/ssl-offloading) aplikace brány.
- Aplikace, například síť pro doručování obsahu, které vyžadují více HTTP požadavků na stejné připojení pomocí protokolu TCP dlouho probíhajících na směrovány nebo načtení rovnováha jiné servery back-end.

Další informace o fungování Azure aplikace brány a jak můžete používat ve vaší nasazení, přečtěte si článek [Přehled brány aplikace](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Externí Vyrovnávání zatížení
Vyrovnávání zatížení externí provede po příchozí připojení z Internetu rozloženy mezi servery součástí virtuální sítě Azure. Vyrovnávání zatížení externí Azure můžete tuto možnost můžete poskytnout a doporučujeme, abyste se doporučuje používat ho nevyžadují rychlé relace nebo převzít SSL.

Na rozdíl od Vyrovnávání zatížení protokolu HTTP externí služby Vyrovnávání zatížení informace používá na síť a transport vrstvy síťový model OSI rozhoduje o jaké serveru načíst zůstatek připojení k.

Doporučujeme používat externí služby Vyrovnávání zatížení pokaždé, když máte [příslušnosti aplikací](http://whatis.techtarget.com/definition/stateless-app) přijímat příchozí žádosti z Internetu.

Další informace o tom, jak funguje externí Vyrovnávání zatížení Azure a jak můžete nasadit, ujistěte se, přečtěte si článek [Seznámení vytváření Internet protilehlé Vyrovnávání zatížení v správce prostředků pomocí Powershellu](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Vnitřní Vyrovnávání zatížení
Vyrovnávání zatížení interní je podobný Vyrovnávání zatížení externí a používá stejnou k načtení zůstatek připojení k serverům za ně. Jediný rozdíl je, aby Vyrovnávání zatížení v tomto případě je přijímat připojení z virtuálních počítačích, které nejsou na Internetu. Ve většině případů připojení, která přijaté pro vyrovnávání zatížení zahájí zařízení v síti virtuální Azure.

Doporučujeme použít interní Vyrovnávání zatížení pro scénáře, které se budou využívat tuto možnost, třeba když potřebujete načíst zůstatek připojení k SQL či interní webových serverech.

Další informace o fungování Azure interní služby Vyrovnávání zatížení a jak ho nasadit, přečtěte si článek [Seznámení vytvoření interního Vyrovnávání zatížení pomocí Powershellu](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Použití služby Vyrovnávání zatížení globální
Veřejné cloudu výpočetního něco v ní je možné nasadit globálně distribuované aplikace, které mají součásti nachází v datacentrech z celého světa. To je možné na Microsoft Azure vzhledem k jeho Azure globální datacentra stavu. Na rozdíl od výše zmíněné technologie pro vyrovnávání zatížení globální služby Vyrovnávání zatížení umožňuje zpřístupnit služby i v případě, že celý datacentrech může být k dispozici.

Dostanete tento typ globální Vyrovnávání zatížení v Azure využijete [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/). Přenosy správce něco v ní je možné načíst zůstatek připojení ke službám na základě polohy uživatele.

Třeba pokud uživatel je žádost o služby z Evropské unie, připojení přesměrován služby součástí EU datacentra. Tato část z provozu správce globální načíst vyrovnávání pomáhají zlepšit výkon, protože se připojit k nejbližšímu datacentru rychleji než připojení k datacentrech sloužící jako daleko.

Na straně dostupnost Vyrovnávání zatížení globální zajišťuje, aby vaše služba dostupné i v případě celý datacentra má k dispozici.

Například pokud Azure datacentra by měl nedostupná kvůli klimatizace důvodů nebo z důvodu výpadků (například chyb v místní síti), připojení ke službě by být přesměrovány do nejbližšího datacentra online. Tento Vyrovnávání zatížení globální dosáhnete využijete DNS zásad, které vytvoříte ve Správci přenosy.

Doporučujeme použít přenosy správce pro všechny cloudové řešení vytvářené, které má obor odesílaných napříč několika oblastí a vyžaduje nejvyšší úroveň provozu možné.

Další informace o Správci přenosy Azure a jak ji nasadit, přečtěte si článek [Co je přenosy správce](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Vypnutí RDP/SSH přístupu k Azure virtuálních počítačích
Je možné přístup pomocí [Protokolu RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) a [Zabezpečené prostředí](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokoly Azure virtuálních počítačích. Tyto protokoly volání umožňuje spravovat virtuálních počítačích ze vzdáleného místa a jsou standardní v počítačové datacentra.

Potenciální zabezpečení pomocí těchto protokolů přes Internet je to, že útočníků můžete použít různé postupy [hrubou silou](https://en.wikipedia.org/wiki/Brute-force_attack) k získání přístupu k Azure virtuálních počítačích. Po útoků získat přístup, budou určenou virtuálního počítače jako bod spuštění nebezpečné jiných počítačů ve vaší síti virtuální Azure nebo dokonce útoku síťových zařízení mimo Azure.

Proto doporučujeme zakázat přímý přístup pomocí protokolu RDP nebo SSH vaší virtuálních počítačích Azure z Internetu. Po zakázání přímý přístup pomocí protokolu RDP nebo SSH z Internetu máte další možnosti, které používáte pro přístup k tyto virtuálních počítačích Vzdálená správa systému:

- Bod webu VPN
- Na webu VPN
- ExpressRoute

[Čárky sítě VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) je jiný termín pro vzdálené připojení VPN klient server. Bod webu VPN umožňuje jednoho uživatele pro připojení k síti virtuální Azure přes Internet. Po připojení čárky webu budou moct připojení k jakékoli virtuálních počítačích umístění v síti virtuální Azure, který uživatel připojený přes VPN čárky webu pomocí RDP nebo SSH uživatele. Tím se předpokládá, že uživatel oprávnění k dosažení tyto virtuálních počítačích.

Bod webu VPN totiž bezpečnější než přímé připojení RDP nebo SSH má uživatel ověření dvakrát před připojením k virtuálního počítače. Nejdřív uživatel musí ověřit (a povoluje) připojení VPN čárky webu; za druhé uživatel musí ověřit (a povoluje) jak vytvořit relaci RDP nebo SSH.

[Na webu VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) připojí celé síti k jiné síti přes Internet. Na webu VPN slouží k připojení k síti virtuální Azure v místní síti. Pokud nasadíte VPN k webu, uživatelé v místní síti bude moct připojit k virtuálních počítačích ve vaší síti virtuální Azure pomocí protokolu RDP nebo SSH přes připojení VPN k webu a nevyžaduje umožňující přímý přístup RDP nebo SSH přes Internet.

Můžete taky vyhrazený sítě WAN odkaz mohl vám poskytovat funkce podobný VPN na webu. Hlavní rozdíly hodnotu 1. vyhrazené sítě WAN odkazu nemá přecházet na Internetu a 2. vyhrazené sítě WAN odkazů jsou obvykle další stabilní a performant. Azure poskytuje řešení vyhrazené sítě WAN odkazu ve formě [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Centrum zabezpečení povolit Azure
Centrum zabezpečení Azure pomáhá zabránit, zjišťování a odpovědět na hrozeb a poskytuje že lepší přehled o a publikum nemůže ovládat, zabezpečení Azure zdroje. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Centrum zabezpečení Azure vám pomůže optimalizovat a sledovat zabezpečení sítě tak, že:

- Poskytování sítě zabezpečení doporučení
- Sledování stavu konfigurace zabezpečení sítě
- Upozorňování na sítě hrozeb i na úrovni koncového bodu a sítě

Důrazně doporučujeme povolit Centrum zabezpečení Azure jednotlivých Azure nasazení.

Další informace o Azure Centrum zabezpečení a jak povolit vaše nasazení, najdete v článku [Úvod k Centru zabezpečení Azure](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Bezpečné rozšířit vaše datacentra do Azure
Mnoho podnikové pracují na hledané rozbalte do cloudu místo Kvetoucí jejich datacentrech místní organizace. Toto rozšíření představuje rozšíření existující IT infrastrukturu do veřejné cloudu. Využitím místní více možností připojení je možné považovat za jakékoli jiné podsítě na vaši síťovou infrastrukturu místní síti virtuální Azure.

Je ale spoustu plánování a návrh problémy, které je potřeba nejdřív adresovaná. To je zvlášť důležité oblasti zabezpečení sítě. Jednou z nejlepších způsobů zjištění, jak řešit takový návrh je příklad.

Společnost Microsoft vytvořila [Datacentra rozšíření odkaz architektura diagramu](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) a referenční materiály vám pomůže pochopit, jak prodloužení datacentra vypadat. Tato funkce poskytuje příklad implementace odkaz, který slouží k plánování a návrh s příponou datacentra zabezpečené enterprise do cloudu. Doporučujeme, abyste si prošli tento dokument k určitou představu o klíčové součásti bezpečné řešení.

Další informace o tom, jak bezpečně rozšířit vaše datacentra do Azure, přečtěte si videa [Rozšíření Datacentra na Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
