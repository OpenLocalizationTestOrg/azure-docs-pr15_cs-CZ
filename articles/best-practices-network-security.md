<properties
   pageTitle="Microsoft cloud services a síťové zabezpečení | Microsoft Azure"
   description="Přečtěte si některé klíčové funkce dostupné v Azure pomohou vytvořit zabezpečené síťových prostředích"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Zabezpečení služeb a síťové cloudu společnosti Microsoft

Cloudovým službám Microsoftu předvádění mnoha služby a infrastruktury, možnosti podnikové a řadu možností pro hybridní připojení. Zákazníci si mohou vybrat pro přístup k tyto služby prostřednictvím Internetu nebo s Azure ExpressRoute, který zajišťuje připojení k soukromé síti. Platformu Microsoft Azure umožňuje zákazníkům Bezproblémová rozšířit svou infrastrukturu do cloudu a vytvořte vícevrstvé architektury. Třetí strany navíc můžete povolit rozšířenými možnostmi nabídkou zabezpečení služeb a virtuální zařízení. Tento dokument white paper obsahuje přehled o zabezpečení a architektonických problémy, které zákazníci měli zvážit při použití cloudovým službám společnosti Microsoft k nim získat přístup prostřednictvím ExpressRoute. Zahrnuje se také vytváření bezpečnější služeb v Azure virtuální sítě.

## <a name="fast-start"></a>Rychlý start
V následující tabulce logiky odkázat k konkrétní příklad technik mnoho zabezpečení s informacemi o platformě Azure k dispozici. Stručný přehled najdete příklad, který nejlépe vyhovuje váš případ. Podrobnější vysvětlení pokračujte ve čtení přes papír.
![Vývojový diagram možnosti zabezpečení][0]

[Příklad 1: Vytvoření obvod síti (označovaná taky jako DMZ demilitarizovaná zóna a chráněná podsítě) se můžete pomoci chrá aplikace se skupinami zabezpečení síti (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Příklad 2: Vytvoření obvod sítě se můžete pomoci chrá aplikací s bránu firewall a NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Příklad 3: Vytvoření obvod sítě se můžete pomoci chrá sítě s bránu firewall, uživatelem definovaných směrování (UDR) a NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Příklad 4: Přidání hybridní připojení na webu na webu, virtuální zařízení virtuální privátní sítě (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Příklad 5: Přidání hybridní připojení k webu, Azure Brána VPN.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Příklad 6: Přidání hybridní připojení s ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Příklady pro přidání spojení mezi virtuálních sítí, dostupnost a zřetězení služby se přidají na tento dokument za další několik měsíců.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Ochrana dodržování předpisů a infrastruktury společnosti Microsoft
Microsoft uplatnilo vedoucí pozici podpůrné dodržování předpisů iniciativy vyžadované podnikoví uživatelé. Následují některé certifikace dodržování předpisů pro Azure: ![odznáčky Azure dodržování předpisů][1]

Další informace najdete v článku informace o dodržování předpisů na [Centrum zabezpečení aplikace Microsoft](https://azure.microsoft.com/support/trust-center/compliance/).

Společnost Microsoft nemá úplný přístup k ochraně cloudové infrastruktury potřebné ke spuštění mnoha globální služby. Cloudové infrastruktury společnosti Microsoft včetně hardwaru, software, sítích a pro správu a zaměstnance školy operace kromě fyzické datacentrech.

![Funkce Azure zabezpečení][2]

Tento postup představuje bezpečnější základ pro zákazníky, abyste mohli nasadit jejich služby v cloudu společnosti Microsoft. Dalším krokem je určený pro zákazníky navrhování a vytváření Architektura zabezpečení chránit těchto služeb.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Tradiční zabezpečení architektury a hraničních sítí
Přestože Microsoft investuje velkém při ochraně cloudové infrastruktury, zákazníci musí taky chránit jejich cloudovými službami a skupiny zdrojů. S více vrstvami přístup k zabezpečení poskytuje nejlepší obrana. Zóny zabezpečení sítě obvod chrání vnitřní síti zdrojů z nedůvěryhodných sítě. Hraniční síti odkazuje okraje nebo části sítě umístěné mezi Internetu a chráněné enterprise IT infrastrukturu.

V typické podnikové sítě infrastruktury core velkém přidá za hranice s více vrstev zabezpečení zařízení. Hranice každé vrstvy se skládá ze zařízení a body vynucení zásady. Zařízení můžou, patří: bránách firewall, zabránění Distributed odmítnutí služby (Denial), průnik zjišťování systémů ochrany (ID/IP adresy) nebo VPN zařízení. Vynucení zásad můžete formu zásady brány firewall, seznamy řízení přístupu (ACL) nebo konkrétní směrování. První řádek obrana v síti, přímo přijímat příchozí přenos z Internetu, je tvořen kombinací tyto mechanismy blok útoky a nebezpečnými přenosy zároveň legitimní žádosti o další do sítě. Tento přenos přesměrovává přímo k prostředkům ve hraniční síti. Daný zdroj může potom "" Promluvte si s zdroje hlouběji v síti, projíždějícím další okraj ověřovacích první. Vnější vrstva se nazývá obvod síť, protože tato část sítě je vystaven na Internetu, obvykle pomocí některého zamknutí po obou stranách. Následující obrázek znázorňuje příklad sítě obvod jediné podsítě v podnikové síti se dvěma hranice zabezpečení.

![Hraniční síti v podnikové síti][3]

Existuje mnoho architektury slouží k provádění obvod síti, z při vyrovnávání zatížení jednoduché před webové farmy k síti více obvod podsítě paletami mechanismy u každého okraj blokovat provoz a ochrana hlubší vrstvy podnikovou síť. Jak sítě obvod sestavují závisí na specifických potřeb organizace a jeho celkový odchylky rizika.

Jak zákazníky přesunout jejich pracovního vytížení veřejné Oblaka, je naprosto zásadní podobné podporou architektura sítě obvod v Azure plnit požadavky na zabezpečení a dodržování předpisů. Tento dokument obsahuje pokyny na jak zákazníci můžete vytvářet prostředí zabezpečené sítě v Azure. Se zaměřuje na obvod síť, ale také úplný diskuse aspektech zabezpečení sítě. Na následující otázky informovat této diskuse:

- Jak lze obvod síť v Azure sestavují?
- Jaké jsou některé z Azure funkcí k dispozici pro vytvoření obvod sítě?
- Jak můžete být chráněny back-end úloh?
- Jak se internetové komunikace řízená na úloh v Azure?
- Jak lze místní sítě chráněn před nasazení v Azure?
- Pokud funkce nativního Azure zabezpečení bude použito a zařízeními jiných výrobců nebo služeb?

Na následujícím obrázku vidíte různé vrstvy cenného papíru, který obsahuje Azure zákazníkům. Tyto vrstvy jsou nativní v Azure platformu samotné a uživatelsky definovaná funkce:

![Architektura zabezpečení Azure][4]

Příchozí z Internetu Azure Denial pomáhá chránit před rozsáhlých útoky Azure. Je další vrstva uživatelsky definovaná veřejné koncové body, které se používají k určení, které přenos dat přes cloudovou službu virtuální sítě. Nativní Azure virtuální izolace sítě zajistí dokončení izolace z jiných sítích, což, budou návštěvníci pouze prochází uživatel nakonfigurovaný cesty a metody. Tyto cesty a metody jsou následující vrstvy, kde NSGs, UDR a sítě virtuální zařízení můžete použít k vytvoření hranice zabezpečení chránit nasazení aplikace v chráněné sdílené síťové.

Následující části Přehled Azure virtuální sítě. Tyto virtuální sítě jsou vytvářeny ve zákazníky a jsou co jejich nasazeném pracovního vytížení připojeni k. Virtuální sítě jsou základ všechny funkce zabezpečení sítě muset vytvořit obvod síť pro ochranu nasazení zákazníka v Azure.

## <a name="overview-of-azure-virtual-networks"></a>Základní informace o Azure virtuálních sítí
Před internetový provoz dostali na Azure virtuálních sítí, jsou dvě vrstvy cenného papíru inherentní Azure platformy:

1.  **Ochrana Denial**: ochrana Denial je vrstvě Azure fyzické síti, která chrání Azure platformu přímo z rozsáhlé internetové útoky. Tyto útoky umožňuje více "bot" uzlů v pokus o overwhelm internetové služby. Azure má robustní ochranu Denial mřížky na všechny příchozí připojení k Internetu. Tento vrstva ochranu Denial atributy žádné uživatele, která dokáže nahradit a není dostupný zákazníkům. Chrání Azure jako platformu z rozsáhlé útoky, ale nebude chránit přímo aplikace pro jednotlivé zákazníky. Další vrstvy odolnost je možné konfigurovat zákazníkem proti útok lokalizované. Třeba při zákazníka A byla napadení s útok rozsáhlé Denial veřejné koncový bod, Azure blokuje připojení ke službě. Zákazníka A může převzít jiné virtuální sítě nebo služby koncový bod není souvisejících se útok obnovíte služby. Je třeba poznamenat, že sice zákazníka A může mít vliv na tohoto koncového bodu, jiných služeb, mimo tohoto koncového bodu by to mít vliv na. Kromě toho jiné služby a zákazníky, uvidí žádný vliv, které útoky.
2.  **Koncové body služby**: koncové body povolit cloud services nebo zdroje skupin veřejné Internet IP adres a porty zveřejněným. Koncový bod použije překládání adres (NAT) Pokud chcete provoz směrovat na vnitřní adresu a port Azure virtuální síti se systémem. Toto je primární cestu pro externí komunikace do virtuálního sítě. Koncové body služby jsou předaný uživateli, která dokáže nahradit určit, které přenosy a jak a kam ho je přeložit virtuální síti se systémem.

Jakmile přenosy dosáhne virtuální síť, máte řadu funkcí, které uplatnit. Azure virtuální sítě se základem pro zákazníky připojit jejich pracovního vytížení a kdy se použije základní sítě úrovně zabezpečení. Privátní sítě (překrytím virtuální sítě) se nachází v Azure pro zákazníky pomocí následující funkce a vlastnosti:
 
- **Přenosy izolace**: virtuální sítě je hranice izolace přenosy na Azure platformu. Virtuálních počítačích (VMs) v jedné sítě virtuální nemůžete komunikovat přímo do VMs v jiné virtuální síti, i když oba virtuální sítě jsou vytvářeny ve stejné zákazníka. Toto je důležité vlastnost, která zajišťuje VMs zákazníka a komunikace zůstane soukromá ve virtuální sítě.
- **Topologie multitier**: virtuální sítě povolit uživatelům definovat multitier topologie přidělování podsítí a určení mezer samostatnou adresu pro jinou prvků nebo jejich pracovního vytížení "úrovní". Tyto logické seskupení topologií povolit uživatelům definovat různých přístupu na základě typů pracovního vytížení a také nastavit přenosy jsou přenášena mezi aplikacemi úrovně.
- **Křížové místní připojení**: Zákazníci můžete vytvořit více místních poštovních připojení mezi virtuální sítě a víc webů místní nebo jiných virtuální sítích v Azure. K tomuto účelu můžete použít zákazníci brány VPN Azure nebo jiných výrobců sítě virtuální zařízení. Azure podporuje virtuální privátní sítě (S2S) na webu pomocí standardních protokolů IPsec/IKE a ExpressRoute soukromé připojení.
- **NSG** mohou uživatelé vytvářet pravidla (ACL) na požadovanou úroveň podrobností: síťové rozhraní, jednotlivé VMs nebo virtuální podsítí. Zákazníci můžete řídit přístup povolení nebo zakázání komunikaci mezi úloh v síti virtuální z systémů v sítích zákazníků prostřednictvím křížově místní připojení, nebo přímé komunikace po Internetu.
- **UDR** a **Přesměrování IP** povolit uživatelům definovat cesty komunikace mezi různých úrovní v rámci virtuální sítě. Zákazníci můžete nasadit bránu firewall, ID nebo IP adresy a další virtuální zařízení a směrovat přenosy v síti síť pomocí těchto zařízeních zabezpečení pro vynucení zásad zabezpečení hranice auditování a kontroly.
- **Virtuální spotřebiče sítě** v Azure Marketplace: zabezpečení zařízení například bránách firewall, Vyrovnávání zatížení a ID/IP adresy jsou k dispozici v Azure Marketplace a Galerie obrázků OM. Zákazníci nástroje můžete nasazovat těchto zařízeních do svých virtuální sítí a konkrétně na jejich hranice zabezpečení (včetně podsítí obvod síť) dokončete prostředí možných zabezpečené sítě.

Pomocí těchto funkcí a možností jeden příklad, jak může architektura sítě obvod vytvořen v Azure je následující:

![Hraniční síti v Azure virtuální sítě][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Vlastnosti obvod sítě a požadavky
Hraniční síti je navržen front-end sítě přímo propojení komunikace z Internetu. Příchozí pakety by měl postupují zabezpečení zařízení, třeba bránu firewall, ID a IP adresy, před dosažením servery back-end. Internet vazbou paketů z úloh můžete taky postupují zabezpečení zařízení v síti obvod vynucení zásad, kontroly a auditování účely před přímo v síti. Hraniční síti navíc můžete hostovat křížově místní VPN brány mezi virtuálních sítí zákazníka a místní sítě.

### <a name="perimeter-network-characteristics"></a>Vlastnosti obvod sítě
Odkazování na předchozím obrázku, některé vlastnosti dobré hraniční sítě jsou následujícím způsobem:

- Internetové:
    - Obvod sítě síť je internetové, přímo komunikaci s na Internetu.
    - Veřejné IP adresy, virtuální nebo koncové body služby přenášena Internet front-end sítě a zařízení.
    - Příchozí data z Internetu prochází zabezpečení zařízení před další zdroje front-end síti se systémem.
    - Pokud je povoleno odchozí zabezpečení, budou návštěvníci prochází zabezpečovacích, jako poslední krok, před předávání k Internetu.
- Chráněné sdílené síťové:
    - Neexistuje přímé cesta z Internetu na infrastrukturu core.
    - Kanály infrastruktury core musí procházet pomocí zabezpečovacích například NSGs, brány firewall nebo VPN zařízení.
    - Jiná zařízení nesmí propojí Internet a infrastruktury core.
    - Zabezpečení zařízení v internetového a chráněné sdílené síťové protilehlé hranice sítě obvod (například dvě brány firewall ikony ukazuje předchozí obrázek) může být skutečně jednoho virtuální zařízení s odlišné pravidla nebo rozhraní pro každý okraj. (To znamená jednoho zařízení logicky oddělené zpracování načíst pro oba hranice sítě obvod.)
- Další běžné postupy a omezeními:
    - Pracovního vytížení nesmí obsahují důležitých informací o zaměstnání.
    - Přístup a aktualizace obvod konfigurace sítě a nasazení se omezí na pouze oprávnění správci.

### <a name="perimeter-network-requirements"></a>Požadavky na obvod sítě
Aby tyto vlastnosti, postupujte podle následujících pokynů virtuální sítě požadavkům implementovat úspěšné hraniční sítě:

- **Podsítě architektura:** Určení virtuální sítě tak, aby se celý podsítě se snaží jako síť obvod oddělená od ostatních podsítí ve stejné síti virtuální. Zajistíte tím přenos mezi obvodovou sítí a ostatní toky úrovně soukromé nebo interní podsítě přes bránu firewall nebo ID/IP adresy virtuální zařízení na hranice podsítě s uživatelem definovaných trasy.
- **NSG:** Obvod sítě síť by měl být otevřen komunikaci prostřednictvím Internetu, ale, který není znamená, že zákazníci měli vynechání NSGs. Dodržujte běžné postupy zabezpečení minimalizovat ploch sítě vystaven na Internetu. Uzamknout oblasti Vzdálená adresa povolených pro přístup k nasazením nebo konkrétní aplikaci protokoly a porty, které jsou otevřené. Doména může obsahovat okolností, ve kterých to není možné. Například pokud zákazníci máte externí web v Azure, sítě obvod tomu příchozí žádosti web ze všech veřejných IP adres, ale měli otevírat pouze ty webové aplikace: TCP:80 a TCP:443.
- **Směrování tabulky:** Obvod sítě síť by měla přímo komunikovat k Internetu funguje, ale neměli komunikovat přímo z zpět end nebo místní sítě a nemusíte přecházet přes bránu firewall nebo zabezpečení zařízení.
- **Nastavení zabezpečení zařízení:** Abyste mohli směrování a zkontrolovat pakety mezi obvodovou sítí a zbytek chráněných sítí, spotřebiče zabezpečení například bránu firewall, ID a IP adresy zařízení však mohou být více adresami. Mohou mít samostatný nic pro obvodovou sítí a podsítí back-end. Nic v síti obvod komunikovat přímo do a z Internetu s odpovídajícím NSGs a obvod síťové směrování tabulky. Připojení k podsítí back-end nic více omezit NSGs a směrování tabulky odpovídající podsítí back-end.
- **Funkcích zabezpečení zařízení:** Zabezpečení zařízení používaný v síti obvod obvykle provádět následující funkce:
    - Brána firewall: Vynucení pravidla brány firewall nebo zásady řízení přístupu pro příchozí žádosti.
    - Ochrany proti novým ohrožením zjišťování a zabránění: zjišťování a zmírnit škodlivými útoky z Internetu.
    - Sestavy auditování a protokolování: zachování podrobné protokoly auditování a analýzy.
    - Otočení proxy: přesměrovat příchozí žádosti odpovídající servery back-end. Zahrnuje mapování a překladu cílové adrese na front-end zařízeních obvykle brána firewall, adresy serveru back-end.
    - Přeposlání proxy server: poskytuje překladu síťových adres a provádění auditování komunikace, které iniciuje z v rámci virtuální sítě k Internetu.
    - Směrovači: Přesměrování příchozích a křížově podsítě přenosy uvnitř virtuální sítě.
    - Zařízení virtuální privátní sítě: budou sloužit jako bran VPN křížově místní pro více místní VPN propojení zákazníka místní sítě a Azure virtuální sítě.
    - VPN server: přijetí VPN připojení klientů k Azure virtuální sítě.

>[AZURE.TIP] Oddělit následující dvě skupiny: jednotlivců oprávnění k přístupu k ozubeného kola obvod sítě zabezpečení a jednotlivce povolených jako správci vývoj, nasazení nebo operace aplikací. Nemít těchto skupin samostatné umožňuje zodpovědnosti a zabrání jedna osoba nepoužití zabezpečení aplikace a ovládací prvky zabezpečení sítě.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Otázek, na které se výzva při vytváření hranice síť
V této části Pokud konkrétně uvedených termínů "sítí" odkazuje Azure virtuální sítím vytvořené správcem předplatného. Termín neodkazuje na základní fyzických sítí v Azure.

Navíc Azure virtuální sítě se často používá rozšiřte tradiční místní sítě. Je možné zahrnout na webu nebo ExpressRoute hybridní síťová řešení s obvodovou síťové architektury. Toto je důležitým aspektem při vytváření hranice zabezpečení sítě.

Následující tři otázky je považován za kritický odpovědět, když vytváříte v síti s obvodovou sítí a více hranice zabezpečení.

#### <a name="1-how-many-boundaries-are-needed"></a>1) je třeba kolik omezení?
První bod rozhodnutí se rozhodnete, kolik hranice zabezpečení jsou potřebné v dané situace:

- Jedinou hranici: sítě front-end obvod mezi virtuální síť a Internet.
- Dva hranice: jeden na Internetu straně obvod sítě a jiné mezi podsítě obvod a back-end podsítě v Azure virtuální sítě.
- Tři omezení: jeden na straně obvod sítě Internet mezi obvodovou sítí a back-end podsítí a mezi podsítí back-end a v místní síti.
- Hranice N: Číslo proměnné. V závislosti na splníte požadavky na zabezpečení je skutečně libovolný počet hranic zabezpečení, které lze použít v síti dané.

Počet a typ omezení potřeby budou lišit v závislosti na společnosti rizika odchylky a konkrétním scénáři prováděna. Toto je často rozhodnutí více skupin v rámci organizace, často včetně riziko a dodržování předpisů týmu, v síti a platformy týmu a týmu vývoje aplikace, které udělali. Tělesně znalost zabezpečení, souvisejících dat a technologie použit měli mít přivítejte v tomto rozhodnutí zajistit postoj odpovídající zabezpečení pro každou implementaci.

>[AZURE.TIP] Použijte nejmenší počet hranic splňující požadavky na zabezpečení pro dané situaci. S další omezení tím složitější operace a řešení problémů s může být, a správa režijních souvisejících se správou více zásad hranice určitou dobu. Nedostatečná hranice však zvýšit rizika. Hledání zůstatek je naprosto zásadní.

![Hybridní síť s tři hranice zabezpečení][6]

Předchozím obrázku je znázorněn souhrnné zobrazení v síti okraj tři zabezpečení. Hranice jsou mezi obvod síť a Internet, Azure front-end a back-end soukromé podsítí a Azure podsítě back-end a místní podnikové síti.

#### <a name="2-where-are-the-boundaries-located"></a>2) hranice umístění?
Až počet hranic rozhodnou, kde jejich implementace je další rozhodnutí bod. Jsou obvykle tři možnosti:
- Použití internetových zprostředkovatel služby (třeba cloudové webové aplikace bránu firewall, která není popisované v tomto dokumentu)
- Pomocí vestavěných funkcí a/nebo síti virtuální zařízení v Azure
- Pomocí pole fyzicky zařízení v místní síti

Možnosti v čistě Azure sítích, jsou nativní Azure funkce (například Vyrovnávání zatížení Azure) nebo síti virtuální zařízení ze ekosystému bohaté partnera Azure (například bránách firewall zaškrtnutí bod).

V případě potřeby hranici mezi Azure a v místní síti zabezpečení zařízení mohou být umístěny na obou stranách připojení (nebo obě strany listu). Proto rozhodnutí třeba na umístění umístit zabezpečení zařízení.

Na předchozím obrázku sítě Internet obvod hranice přední do back konce zcela obsažené v Azure a musí být nativní Azure funkce nebo síti virtuální zařízení. Zabezpečení zařízení na hranici mezi Azure (back-end podsítě) a podnikové síti může být buď na straně Azure nebo místní straně nebo i kombinace zařízení na obou stranách. Může být významný výhody a nevýhody obou možnost, která musí vážně považovat za.

Použití stávající zařízení fyzicky zabezpečení na straně místní síti například má tu výhodu, že není potřeba žádná nová ozubeného kola. Stačí konfigurace. Nevýhodou, ale je, že všechny přenosy musí vraťte z Azure na místní síti prezentací, které zabezpečení zařízení. Proto může Azure Azure přenosy vzniknou významné latence a mají vliv na výkon aplikace a uživatel dojít, pokud byla vynucená zpět v místní síti vynucení zásad zabezpečení.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) jsou hranice implementaci?
Každý hranice zabezpečení pravděpodobně bude mít požadavky různé možnosti (například ID a brány firewall pravidla na straně Internet obvodovou síť, ale jenom ACL mezi obvodovou sítí a back-end podsítě). Rozhodování, které zařízení na používání závisí na požadavky scénář a zabezpečení. V následující části jsou uvedeny příklady 1, 2 a 3 některé možnosti, které lze použít. Používání velkého počtu možnosti dostupné vyřešit libovolných scénář revizí funkcí Azure nativní síti a k dispozici v Azure od partnera ekosystému zařízení zobrazuje.

Jiný bod rozhodnutí klíčové implementaci se dozvíte, jak připojit v místní síti s Azure. Můžete použít Azure virtuální brány nebo síti virtuální zařízení? Tyto možnosti jsou popsány podrobněji v následující části (příklady 4, 5 a 6).

Kromě toho můžou být potřeba komunikaci mezi virtuální sítí v rámci Azure. Podobnému sledu se přidá k pozdějšímu datu.

Po nastavení znát odpovědi na otázky předchozí oddílu [Rychlý Start](#fast-start) pomůže určit, které příklady jsou nejvhodnější pro dané situace.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Příklady: Vytváření hranice zabezpečení s Azure virtuálních sítí
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Příklad 1: Vytvoření obvod sítě se můžete pomoci chrá aplikací s NSGs
[Zpátky na začátek rychlé](#fast-start) | [podrobné pokyny v tomto příkladu pro vytvoření][Example1]

![Příchozí obvod sítě s NSG][7]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Dvě cloudových služeb: "FrontEnd001" a "BackEnd001"
- Virtuální sítě, "CorpNetwork", s dvěma podsítí: "FrontEnd" a "Back-end"
- Skupina zabezpečení sítě, která se použije pro obě podsítí
- Windows server, který představuje serveru webových aplikací ("IIS01")
- Dva Windows servery, které představují servery back-end aplikace ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

Skripty a šablony pro správce prostředků Azure najdete v tématu [Tvůrce dotazů podrobné pokyny][Example1].

#### <a name="nsg-description"></a>Popis NSG
V tomto příkladu je skupinu NSG integrované a potom načíst se šesti pravidel.

>[AZURE.TIP] Obecně je třeba vytvořit konkrétní "Povolit" pravidel nejdřív následovaný obecnější pravidla "Odepřít". Dané Priorita určí, která pravidla jsou vyhodnoceny nejdřív. Jakmile přenosy nachází použít u určitého pravidla, jsou vyhodnoceny žádná další pravidla. V obou příchozí nebo odchozí směru (z hlediska podsítě) můžete použít pravidla NSG.

Deklarativně jsou u příchozích vytvářeného následující pravidla:

1.  Interní přenos DNS (port 53) jsou povolené.
2.  RDP umožnění datových přenosů (port 3389) z Internetu do libovolné virtuálního počítače jsou povolené.
3.  Přenosy protokolu HTTP (port 80) z Internetu na webový server (IIS01) jsou povolené.
4.  Veškerou odchozí komunikaci (všechny porty) služby IIS01 k AppVM1 jsou povolené.
5.  Všechny přenosy (všechny porty) z Internetu na celou virtuální síť (obě podsítí) byl odepřen.
6.  Veškerou odchozí komunikaci (všechny porty) služby front-end podsítě podsítě back-end odepřen.

S následujícími pravidly vázaný na každé podsítě, pokud žádost HTTP příchozí z Internetu k webovému serveru, jak pravidla 3 (Povolit) a 5 (Odepřít) platit. Ale protože pravidlo 3 musí mít vyšší prioritu, jenom byste měli použít pravidlo 5 nebude možné uplatnit. Žádost HTTP bude mít takto možnost webový server. Pokud tento stejné přenos pokoušel kontaktovat DNS01 server, pravidlo 5 (Odepřít) by měl mít první použití a přenos nebude moct pořizovat předejte na server. Pravidlo 6 (Odepřít) blokuje front-end podsítě z spolu mluvit podsítě back-end (s výjimkou povolené přenosy v pravidlech 1 a 4). To chrání síť back-end v případě, že se zlými úmysly zneužije webové aplikace na front-end. Mohl by omezený přístup k síti "chráněné" back-end (jenom pro zdroje vystaven na serveru AppVM01).

Je výchozí odchozí pravidlo, které umožňuje přenosy se k Internetu. V tomto příkladu jsme jste povolení odchozí přenosy a není změna odchozího pravidla. Chcete-li uzamknout přenosy v obou směrech, uživatelem definovaných směrování požaduje (viz příklad 3).

#### <a name="conclusion"></a>Uzavření
Toto je celkem jednoduchá a jednoduchý způsob izolace podsítě back-end z příchozích. Další informace najdete v tématu [Tvůrce dotazů podrobné pokyny][Example1]. Tyto pokyny, patří:

- Postup vytvoření této sítě obvod pomocí skriptů Powershellu.
- Postup vytvoření této sítě obvod šablonou správce prostředků Azure.
- Podrobný popis jednotlivých NSG příkaz.
- Scénáře toku podrobné přenosy znázorňující, jak přenos povolený nebo odepřen v každé vrstvy.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Příklad 2: Vytvoření obvod sítě se můžete pomoci chrá aplikací s bránu firewall a NSGs
[Zpátky na začátek rychlé](#fast-start) | [podrobné pokyny v tomto příkladu pro vytvoření][Example2]

![Příchozí obvod síť s NVA NSG][8]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Dvě cloudových služeb: "FrontEnd001" a "BackEnd001"
- Virtuální sítě, "CorpNetwork", s dvěma podsítí: "FrontEnd" a "Back-end"
- Skupina zabezpečení sítě, která se použije pro obě podsítí
- Virtuální spotřebiče sítě, v tomto případě a bránu firewall, připojení k front-end podsítě
- Windows server, který představuje serveru webových aplikací ("IIS01")
- Dva Windows servery, které představují servery back-end aplikace ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

Skripty a šablony pro správce prostředků Azure najdete v tématu [Tvůrce dotazů podrobné pokyny][Example2].

#### <a name="nsg-description"></a>Popis NSG
V tomto příkladu je skupinu NSG integrované a potom načíst se šesti pravidel.

>[AZURE.TIP] Obecně je třeba vytvořit konkrétní "Povolit" pravidel nejdřív následovaný obecnější pravidla "Odepřít". Dané Priorita určí, která pravidla jsou vyhodnoceny nejdřív. Jakmile přenosy nachází použít u určitého pravidla, jsou vyhodnoceny žádná další pravidla. V obou příchozí nebo odchozí směru (z hlediska podsítě) můžete použít pravidla NSG.

Deklarativně jsou u příchozích vytvářeného následující pravidla:

1.  Interní přenos DNS (port 53) jsou povolené.
2.  RDP umožnění datových přenosů (port 3389) z Internetu do libovolné virtuálního počítače jsou povolené.
3.  Všechny internetový provoz (všechny porty) virtuální spotřebiče síti (firewallem) jsou povolené.
4.  Veškerou odchozí komunikaci (všechny porty) služby IIS01 k AppVM1 jsou povolené.
5.  Všechny přenosy (všechny porty) z Internetu na celou virtuální síť (obě podsítí) byl odepřen.
6.  Veškerou odchozí komunikaci (všechny porty) služby front-end podsítě podsítě back-end odepřen.

S následujícími pravidly vázaný na každé podsítě, pokud žádost HTTP příchozí z Internetu bránu firewall, jak pravidla 3 (Povolit) a 5 (Odepřít) platit. Ale protože pravidlo 3 musí mít vyšší prioritu, jenom byste měli použít pravidlo 5 nebude možné uplatnit. Žádost HTTP bude mít takto možnost si bránu firewall. Pokud tento stejné přenos pokoušel kontaktovat IIS01 server, i když je zapnuté front-end podsítě, pravidlo 5 (Odepřít) byste měli použít, a přenos nebude moct pořizovat předejte na server. Pravidlo 6 (Odepřít) blokuje front-end podsítě z spolu mluvit podsítě back-end (s výjimkou povolené přenosy v pravidlech 1 a 4). To chrání síť back-end v případě, že se zlými úmysly zneužije webové aplikace na front-end. Mohl by omezený přístup k síti "chráněné" back-end (jenom pro zdroje vystaven na serveru AppVM01).

Je výchozí odchozí pravidlo, které umožňuje přenosy se k Internetu. V tomto příkladu jsme jste povolení odchozí přenosy a není změna odchozího pravidla. Chcete-li uzamknout přenosy v obou směrech, uživatelem definovaných směrování požaduje (viz příklad 3).

#### <a name="firewall-rule-description"></a>Popis pravidla brány firewall
V bráně firewall třeba vytvořit pravidel přesměrování. Vzhledem k tomu v tomto příkladu pouze přesměrovává přenosy Internet v vazbou na bránu firewall a pak webového serveru, jedinou přesměrování síťového adresu překladu není potřeba pravidlo.

Pravidlo předávání přijme adresami příchozí zdroje, který narazí bránu firewall snaží kontaktovat HTTP (port 80 a 443 HTTPS). Má odeslané z místního rozhraní bránu firewall a přesměrování na webový server s IP adresu 10.0.1.5.

#### <a name="conclusion"></a>Uzavření
Toto je relativně jednoduchý způsob, jak chránit vaše aplikace s bránu firewall a izolace podsítě back-end z příchozích. Další informace najdete v tématu [Tvůrce dotazů podrobné pokyny][Example2]. Tyto pokyny, patří:

- Postup vytvoření této sítě obvod pomocí skriptů Powershellu.
- Jak vytvořit v tomto příkladu šablonou správce prostředků Azure.
- Podrobný popis jednotlivých NSG příkaz a brány firewall pravidla.
- Scénáře toku podrobné přenosy znázorňující, jak přenos povolený nebo odepřen v každé vrstvy.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Příklad 3: Vytvoření obvod sítě se můžete pomoci chrá sítě s bránu firewall, UDR a NSG
[Zpátky na začátek rychlé](#fast-start) | [podrobné pokyny v tomto příkladu pro vytvoření][Example3]

![Obousměrné obvod síť s NVA, NSG UDR][9]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Tři cloudových služeb: "SecSvc001", "FrontEnd001" a "BackEnd001"
- Virtuální síť, "CorpNetwork", pomocí tří podsítí: "SecNet", "FrontEnd" a "Back-end"
- Virtuální spotřebiče síť, v tomto případě a bránu firewall, připojení k podsítě SecNet
- Windows server, který představuje serveru webových aplikací ("IIS01")
- Dva Windows servery, které představují servery back-end aplikace ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

Skripty a šablony pro správce prostředků Azure najdete v tématu [Tvůrce dotazů podrobné pokyny][Example3].

#### <a name="udr-description"></a>Popis UDR
Ve výchozím nastavení se následující postupy systém definují takhle:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal je vždy definovaný adresu prefix(es) virtuální síť pro konkrétní síť (to znamená ho měnit virtuální síť na virtuální sítě, podle toho, jak je definován každý konkrétní virtuální sítě). Zbývající trasy systém jsou statické a výchozí jak je uvedeno v tabulce.

V tomto příkladu směrování dvě tabulky vytvořené, jedno pro podsítí front-end a back-end. Každou tabulku se načte s statické trasy vhodné pro dané podsítě. Pro účely v tomto příkladu každá tabulka obsahuje tří postupů, které všechny směrovaly (0.0.0.0/0) přes bránu firewall (dalšího směrování = virtuální zařízení IP adresa):

1. Vysílání otevřeny další směrování definované přenosy otevřeny obejít bránu firewall.
2. Virtuální v síti s další směrování rozumí brány firewall; Tato možnost přepíše výchozí pravidlo, které umožňuje místní virtuální v síti ke směrování přímo.
3. Všechny zbývající přenosy (0/0) se další směrování rozumí bránu firewall.

>[AZURE.TIP] V UDR nemají položce otevřeny přerušíte otevřeny komunikace. 
> - V našem příkladu 10.0.1.0/24 ukazujícím na VNETLocal, je naprosto zásadní jako jinak, paketu byste opustili, webového serveru (10.0.1.4) určených pro jiné místního serveru (například) 10.0.1.25 se nepovede, jak se odešle myší na NVA, který vám pošle do podsítě, a podsítě se znovu odeslat brožuru NVA a tak dál.
> - Pravděpodobnost směrování smyčka jsou obvykle vyšší na zařízení s víc nic, která jsou přímo připojen k, které jsou komunikaci s, což je často tradiční, v místním nasazení, spotřebiče. 

Po vytvoření tabulky směrování je vázaný na jejich podsítí. Směrovací tabulky front-end podsítě jednou vytvořené a vázaný na podsítě by vypadal takto:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR nyní lze podsítě brány, na které je připojen okruh ExpressRoute.
>
> Příklady, jak povolit obvodovou síť s ExpressRoute nebo na webu sítě jsou zobrazeny v příkladech 3 a 4.


#### <a name="ip-forwarding-description"></a>Přesměrování IP Popis
Přesměrování IP je funkce companion UDR. Toto je údaj v virtuální zařízení, která umožňuje přijímat přenosy adresovanou není výslovně zařízení a nastavíte přesměrování která vysílání cíle ultimate.

Například pokud adres AppVM01 provede žádost o službu DNS01, UDR by směrovat to bránu firewall. Přesměrování povolené IP přenosy pro určení DNS01 (10.0.2.4) je přijal zařízení (10.0.0.4) a potom přepošlou cíle ultimate (10.0.2.4). Bez IP přesměrování povolené v bráně firewall přenosy by se dají přijímat tak, že zařízení Přestože směrování tabulka obsahuje bránu firewall jako přesměrování. Použití virtuální spotřebiče, je naprosto zásadní nezapomeňte povolit přesměrování IP ve spojení s UDR.

#### <a name="nsg-description"></a>Popis NSG
V tomto příkladu je skupinu NSG vytvořené a potom načte s jedno pravidlo. Tato skupina je potom svázáno pouze podsítí front-end a back-end (ne SecNet). Deklarativně je vytvářeného následující pravidlo:

- Všechny přenosy (všechny porty) z Internetu na celou virtuální síť (všechny podsítě) byl odepřen.

I když jsou v tomto příkladě použita NSGs, jejím hlavním účelem je jako sekundární vrstvy obrana před ručně stav. Cílem je blokovat všechny příchozí data z Internetu na front-end a back-end podsítě. Přenosy by měl pouze postupují podsítě SecNet brány firewall (a potom podle potřeby k podsítí front-end a back-end). Navíc s pravidly UDR na místě, všech přenosů vytvořte z nich podsítí front-end a back-end by být směrovány se brány (díky UDR). Brána firewall, uvidí to jako asymetrické tok a by přetáhnout odchozí přenosy. Proto jsou tři vrstvy zabezpečení, ochrana podsítí:
- Žádné otevřených koncové body na FrontEnd001 a BackEnd001 cloudových služeb.
- NSGs odepření přenosy z Internetu.
- Odstranění brány firewall asymetrické komunikaci.

Jeden bod zajímavé týkající se NSG v tomto příkladu je, aby obsahoval jenom jedno pravidlo, které je odepřít internetových přenosů na celý virtuální sítě, včetně podsítě zabezpečení. Však, protože NSG pouze vázaný na front-end a back-end podsítí, pravidla není zpracovány zatížení příchozí podsítě zabezpečení. Přenosy se v důsledku toho flow podsítě zabezpečení.

#### <a name="firewall-rules"></a>Pravidla brány firewall
V bráně firewall třeba vytvořit pravidel přesměrování. Od brána firewall je blokování nebo přesměrování všechny příchozí, odchozí a uvnitř virtuální v síti, je třeba mnoho pravidla brány firewall. Všechny příchozí přenosy také, bude narazit zabezpečení služeb veřejnou IP adresu (na různé porty), zpracování bránou firewall. Doporučený postup je diagram logickou toky před nastavením podsítí a brány firewall pravidla pro omezení přepracovat později. Na následujícím obrázku je logické zobrazení pravidla brány firewall v tomto příkladu:
 
![Zobrazení Logická pravidel brány firewall][10]

>[AZURE.NOTE] Podle sítě virtuální zařízení používá porty pro správu se budou lišit. V tomto příkladu bránu NextGen Firewall Barracuda odkazuje, který využívá porty 22 801 a 807. V dokumentaci zařízení dodavatele najít přesnou porty pro správu ze zařízení.

#### <a name="firewall-rules-description"></a>Popis pravidla brány firewall
V předchozím Logický diagram nebude zobrazen podsítě zabezpečení. Je to proto brána firewall je pouze zdroje na tomto podsítě a tento graf zobrazuje pravidla brány firewall a jak budou logicky povolit nebo odepřít tok, ne skutečné směrované cesty. Také externí porty vybraná pro přenos RDP jsou vyšší pohyboval porty (8014 – 8026) a jste vybírali poněkud zarovnáte poslední dvě oktetů místní IP adresa pro snazší čitelnost (například adresa místního serveru 10.0.1.4 souvisí s externím portu 8014). Vyšší než konfliktní jakýkoli, ale lze použít.

V tomto příkladu potřebujeme sedm typy pravidel:

- Externí pravidla (u příchozích):
  1.    Pravidlo brány firewall Správa: Tato aplikace přesměrovat pravidlo umožňuje přenos předat porty virtuální zařízení sítě pro správu.
  2.    Pravidla RDP (pro každý systému Windows server): tyto čtyři pravidla (jedno pro každé server) povolit správu jednotlivé serverů prostřednictvím RDP. Může to také seskupeny do jedno pravidlo, podle toho, funkce virtuální zařízení sítě použitý.
  3.    Pravidel přenosu aplikací: existují dva jednotlivá pravidla, první pro přenos front-end web a druhý pro přenos back-end (například webový server osy dat). Konfigurace jednotlivá pravidla závisí na architektura sítě (serverech umístění) a provoz toků (směr se používají tok a které porty).
      - První pravidlo umožňuje přenos skutečné aplikace kontaktovat aplikační server. Tato pravidla povolit zabezpečení a správy, jsou pravidel přenosu aplikací co povolit externí uživatele nebo služby pro přístup k aplikací. V tomto příkladu je jediný webového serveru na portu 80. Pravidlo aplikace jedné brány firewall tedy přesměruje příchozích externí IP na web servery interní IP adresu. Relace přesměrované přenosy by přeložit prostřednictvím překladu síťových adres na interní server.
      - Druhé pravidlo spočívá ve využití jakýkoli back-end pravidlo umožňuje webového serveru chcete mluvit AppVM01 serveru (ale ne AppVM02).
- Vnitřní pravidla (uvnitř virtuální v síti)
  4.    Odchozího pravidla Internetu: Pokud chcete toto pravidlo umožňuje přenos ze jakákoli síť předat vybrané sítě. Pokud chcete toto pravidlo je obvykle výchozí pravidlo už v bráně firewall, ale v zakázaném stavu. Pokud chcete toto pravidlo lze povolit v tomto příkladu.
  5.    DNS pravidlo: Pokud chcete toto pravidlo umožňuje pouze DNS (port 53) předání přenosu na DNS server. Pro toto prostředí je blokován většina umožnění datových přenosů z front-end do back-end. Pokud chcete toto pravidlo konkrétně umožňuje DNS z místní podsítě.
  6.    Podsítě podsítě pravidlo: Pokud chcete toto pravidlo je tak, aby všechny serveru podsítě back-end připojit k jakémukoli serveru na front-end podsítě (ale ne obrácené pořadí).
- Fail-Safe pravidla (pro přenos, který je nesplňuje některou z předchozího):
  7.    Zakázat všechny přenosy pravidlo: vždycky je vhodné konečný pravidlo (z hlediska priorita) a jako takové Pokud tok přenosu přestane odpovídat žádné z předchozí pravidla ho dojde ke ztrátě toto pravidlo. Toto je výchozí pravidla která obvykle aktivovali. Obecně je třeba provádět žádné změny.

>[AZURE.TIP] Na druhé pravidlo přenosu aplikace zjednodušit v tomto příkladu jakýkoli jsou povolené. Ve scénáři reálnou nejvíce portů a rozsahy adres by měl použít k omezení napadení toto pravidlo.

Po vytvoření všechna předchozí pravidla je důležité kontroloval priority každé pravidlo zajistit přenos bude povolený nebo odepřen podle potřeby. V tomto příkladu pravidla jsou v pořadí priorit.

#### <a name="conclusion"></a>Uzavření
Toto je složitější, ale detailní způsob ochrany a izolace sítě než předchozích příkladů. (Příklad 2 chrání jenom aplikace, a příkladu 1 jenom izoluje podsítí). Tento návrh umožňuje sledování provozu v obou směrech a chrání nejen příchozí aplikační server ale uplatňuje sítě zásady zabezpečení u všech serverů v této síti. Také v závislosti na zařízení použít úplné přenosy auditování a plánování informovanosti lze dosáhnout. Další informace najdete v tématu [Tvůrce dotazů podrobné pokyny][Example3]. Tyto pokyny, patří:

- Postup vytvoření této sítě obvod příklad pomocí skriptů Powershellu.
- Jak vytvořit v tomto příkladu šablonou správce prostředků Azure.
- Podrobný popis jednotlivých UDR NSG příkaz a pravidlo brány firewall.
- Scénáře toku podrobné přenosy znázorňující, jak přenos povolený nebo odepřen v každé vrstvy.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Příklad 4: Přidání hybridní připojení na webu na webu, virtuální zařízení virtuální privátní sítě (VPN)
[Zpátky na začátek rychlé](#fast-start) | Podrobné pokyny sestavení k dispozici brzy bude k dispozici

![Obvod síť NVA připojeny hybridní sítě][11]

#### <a name="environment-description"></a>Popis prostředí
Hybridní síti pomocí sítě virtuální zařízení (NVA) můžete přidali k některým z následujících typů sítě obvod popsané v příkladech 1, 2 nebo 3.

Jak ukazuje předchozí obrázek, připojení VPN přes Internet (webu webu) slouží k připojení k síti Azure virtuální prostřednictvím NVA v místní síti.

>[AZURE.NOTE] Pokud používáte ExpressRoute s povolenou možností Azure veřejné prozkoumávání, třeba vytvořit statické trasy. Směrování by měl NVA VPN IP adresu vaší podnikové sítě Internet a ne prostřednictvím sítě WAN ExpressRoute. Převaděč povinné na možnost prozkoumávání veřejné Azure ExpressRoute můžete rozdělit relace VPN.

Po na místě je VPN, stane NVA centrálního bodu u všech sítí a podsítí. Pravidla pro předávání brány firewall zjistit, jaký tok jsou povoleny, jsou převést prostřednictvím překladu síťových adres, přesměrováni nebo se nezobrazí (i pro přenos jsou přenášena mezi aplikacemi v místní síti a Azure).

Tok by měl být považován opatrně, může být optimalizovaný nebo sníženou kvalitu tak, že tento způsob návrhu v závislosti na konkrétní případu použití.

Použití prostředí vytvořené z příkladu 3 a pak přidejte na webu VPN hybridní připojení k síti, vytvoří následující:

![Hraniční síti s NVA připojení prostřednictvím sítě VPN na webu][12]

Místní směrovači nebo jiné zařízení sítě, která je kompatibilní s vaší NVA VPN, bude klient VPN. Toto pole fyzicky zařízení zodpovídá za zahájení a Správa připojení VPN k vaší NVA.

Logicky k NVA, v síti vypadá čtyři samostatné "zóny zabezpečení" s pravidly na NVA je primární ředitele přenosy mezi těmito oblastmi:

![Logická síť z perspektivu NVA][13]

#### <a name="conclusion"></a>Uzavření
Přidání webu webu VPN hybridní síť Azure virtuální sítě můžete rozšířit v místní síti do Azure zabezpečené způsobem. V poli pomocí připojení VPN přenosy pro vaši musí být zašifrovaný a přesměrovává přes Internet. NVA v tomto příkladu představuje centrální umístění k jejímu vynucení a správě zásad zabezpečení. Další informace najdete v tématu Tvůrce dotazů podrobné pokyny (řádně doloženo). Tyto pokyny, patří:

- Postup vytvoření této sítě obvod příklad pomocí skriptů Powershellu.
- Jak vytvořit v tomto příkladu šablonou správce prostředků Azure.
- Scénáře toku podrobné přenosy znázorňující, jak přenosy prochází tento návrh.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Příklad 5: Přidání hybridní připojení k webu, Azure Brána VPN
[Zpátky na začátek rychlé](#fast-start) | Podrobné pokyny sestavení k dispozici brzy bude k dispozici

![Hraniční síti k síti připojených hybridní brány][14]

#### <a name="environment-description"></a>Popis prostředí
Hybridní síti pomocí brány Azure VPN lze přidat do libovolného obvod sítě popsané v příkladech 1 nebo 2.

Jak ukazuje předchozí obrázek, připojení VPN přes Internet (webu webu) slouží k připojení k síti Azure virtuální přes Azure VPN brány v místní síti.

>[AZURE.NOTE] Pokud používáte ExpressRoute s povolenou možností Azure veřejné prozkoumávání, třeba vytvořit statické trasy. Směrování by měl NVA VPN IP adresu vaší podnikové sítě Internet a ne prostřednictvím sítě WAN ExpressRoute. Převaděč povinné na možnost prozkoumávání veřejné Azure ExpressRoute můžete rozdělit relace VPN.

Následující obrázek znázorňuje dva okraje sítě v této možnosti. Na okraji první NVA a NSGs řídit tok uvnitř Azure sítích a mezi Azure a na Internetu. Druhý okraj je brána Azure VPN, což je úplně oddělené a izolace sítě okraje mezi místním prostředím a Azure.

Tok by měl být považován opatrně, může být optimalizovaný nebo sníženou kvalitu tak, že tento způsob návrhu v závislosti na konkrétní případu použití.

Použití prostředí integrované v příkladu 1 a pak přidejte na webu VPN hybridní připojení k síti, vytvoří následující:

![Hraniční síti s brány připojení pomocí připojení ExpressRoute][15]

#### <a name="conclusion"></a>Uzavření
Přidání webu na webu VPN hybridní síťové připojení k síti Azure virtuální můžete rozšířit v místní síti do Azure zabezpečené způsobem. Použití nativního brány Azure VPN, přenosy pro vaši je IPSec zašifrovaných a přesměrovává přes Internet. Pomocí brána Azure VPN můžete taky poskytnout nižší náklady možnost (žádné další licence nákladů stejně jako u jiných výrobců NVAs). Toto je vhodný způsob v příkladu 1, použití žádné NVA. Další informace najdete v tématu Tvůrce dotazů podrobné pokyny (řádně doloženo). Tyto pokyny, patří:

- Postup vytvoření této sítě obvod příklad pomocí skriptů Powershellu.
- Jak vytvořit v tomto příkladu šablonou správce prostředků Azure.
- Podrobné přenosy toku scénáře, znázorňující, jak přenosy prochází tento návrh.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Příklad 6: Přidání hybridní připojení s ExpressRoute
[Zpátky na začátek rychlé](#fast-start) | Podrobné pokyny sestavení k dispozici brzy bude k dispozici

![Hraniční síti k síti připojených hybridní brány][16]

#### <a name="environment-description"></a>Popis prostředí
Hybridní síti pomocí připojení soukromé peering ExpressRoute lze přidat do libovolného obvod sítě popsané v příkladech 1 nebo 2.

Jak ukazuje předchozí obrázek, ExpressRoute soukromé prozkoumávání obsahuje přímé připojení mezi místní síti a Azure virtuální sítě. Přenosy transits pouze síť poskytovatele služeb a Microsoft Azure síť, nikdy klepnutím na Internetu.

>[AZURE.NOTE] Existují některá omezení při používání UDR s ExpressRoute, kvůli složitost dynamické směrování použité v Azure virtuální brány. Toto jsou následujícím způsobem:
>
>- UDR by neměly být použity pro podsítě brány, na které je připojen ExpressRoute propojené Azure virtuální brány.
>- ExpressRoute propojené Azure virtuální brány nelze že přesměrování zařízení pro ostatní UDR vázaná podsítí.
>
>
<br />

>[AZURE.TIP] Použití ExpressRoute zachová podnikové síti z Internetu za účelem lepšího zabezpečení a výrazně zvýšit výkon. Umožňuje rovněž smlouvy o úrovni služeb od svého poskytovatele služeb ExpressRoute. Brána Azure předat až 2 Gb/s ExpressRoute, že se na webu VPN, maximální výkon Azure brány je 200 Mb/s.

Jak je vidět v následujícím diagramu, vyberete tuto možnost prostředí má nyní dvě sítě okraje. NVA a NSG řídit tok uvnitř Azure sítí a mezi Azure a Internet, zatímco brána je úplně oddělené a izolace sítě okraje mezi místním prostředím a Azure.

Tok by měl být považován opatrně, může být optimalizovaný nebo sníženou kvalitu tak, že tento způsob návrhu v závislosti na konkrétní případu použití.

Použití prostředí integrované v příkladu 1 a pak přidejte uživatele ExpressRoute hybridní připojení k síti, vytvoří následující:

![Hraniční síti s brány připojení pomocí připojení ExpressRoute][17]

#### <a name="conclusion"></a>Uzavření
Přidání připojení k soukromé ExpressRoute Peering sítě můžete rozšířit v místní síti do Azure zabezpečení, nižší latence vyšší provádění způsobem. Také při použití nativního Azure brána, jako například získáte nižší náklady možnost (žádné další licence stejně jako u jiných výrobců NVAs). Další informace najdete v tématu Tvůrce dotazů podrobné pokyny (řádně doloženo). Tyto pokyny, patří:

- Postup vytvoření této sítě obvod příklad pomocí skriptů Powershellu.
- Jak vytvořit v tomto příkladu šablonou správce prostředků Azure.
- Podrobné přenosy toku scénáře, znázorňující, jak přenosy prochází tento návrh.

## <a name="references"></a>Odkazy
### <a name="helpful-websites-and-documentation"></a>Užitečné weby a si přečtěte následující dokumentaci
- Access Azure s Azure správce:
- Přístup k Azure pomocí prostředí PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuální sítě si přečtěte následující dokumentaci: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Sítě skupiny zabezpečení si přečtěte následující dokumentaci: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Uživatelem definované směrování si přečtěte následující dokumentaci: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtuální bran: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Na webu VPN: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- Si přečtěte následující dokumentaci ExpressRoute (Nezapomeňte najdete v článku oddíly "Začínáme" a "How To"): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Vývojový diagram možnosti zabezpečení"
[1]: ./media/best-practices-network-security/compliancebadges.png "Odznáčky Azure dodržování předpisů"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Funkce Azure zabezpečení"
[3]: ./media/best-practices-network-security/dmzcorporate.png "DMZ v podnikové síti"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Architektura zabezpečení Azure"
[5]: ./media/best-practices-network-security/dmzazure.png "DMZ v Azure virtuální sítě"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybridní sítě s tři hranice zabezpečení"
[7]: ./media/best-practices-network-security/example1design.png "Příchozí DMZ s NSG"
[8]: ./media/best-practices-network-security/example2design.png "Příchozí DMZ s NVA a NSG"
[9]: ./media/best-practices-network-security/example3design.png "Obousměrné DMZ s NVA, NSG a UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Zobrazení Logická pravidel brány Firewall"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ s NVA připojení sítě hybridní"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ s NVA připojení prostřednictvím sítě VPN na webu"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logická síť z perspektivu NVA"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure brána připojení sítě hybridního webu na webu"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ Azure brána prostřednictvím sítě VPN na webu"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure brána připojení ExpressRoute hybridní síť"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ s Azure brány pomocí připojení ExpressRoute"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
