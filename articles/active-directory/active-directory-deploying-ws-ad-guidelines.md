<properties
   pageTitle="Pokyny pro nasazení systému Windows Server služby Active Directory v Azure virtuálních počítačích | Microsoft Azure"
   description="Pokud víte, jak nasazení služeb AD domény a AD Federation Services místně, zjistěte, jak fungují na Azure virtuálních počítačích."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Pokyny pro nasazení služby Active Directory pro Windows Server na virtuálních počítačích Azure

Tento článek vysvětluje důležité rozdíly mezi nasazení systému Windows Server Active Directory Domain Services (AD DS) a Active Directory Federation Services (AD FS) místní versus nasazení na virtuálních počítačích Microsoft Azure.

## <a name="scope-and-audience"></a>Rozsah a cílové skupiny

Tento článek je určen osobám, které již došlo s nasazením služby Active Directory místní. Popisuje rozdíly mezi zavedení služby Active Directory na Microsoft Azure virtuálních počítačích/Azure virtuálních sítí a tradiční místního nasazení služby Active Directory. Azure virtuálních počítačích a Azure virtuální sítě jsou součástí služby Pomocník pro infrastrukturu – jako-(IaaS) nabízející určené pro organizace můžete využít výpočetních prostředků v cloudu.

Ty, které nejsou ničím novým AD nasazení najdete v článku [Průvodce nasazením AD DS](https://technet.microsoft.com/library/cc753963) nebo [plánu zavedení služby AD FS](https://technet.microsoft.com/library/dn151324.aspx) podle potřeby.

Tento článek předpokládá, že je čtenáře znát následující pojmy:

- Správa a nasazení systému Windows Server AD DS
- Nasazení a konfiguraci služby DNS pro podporu infrastrukturu systému Windows Server AD DS
- Správa a nasazení systému Windows Server AD FS
- Nasazení, konfiguraci a správě předávající dodavatelů (weby a webové služby), které můžete používat tokeny systému Windows Server AD FS
- Obecné virtuálního počítače koncepty, například konfiguraci virtuálního počítače, virtuálních disků a virtuálních sítí

Tento článek popisuje požadavky pro hybridní nasazení situace které systému Windows Server AD DS nebo AD FS jsou částečně nasazené místního a částečně nasazené na Azure virtuálních počítačích. Dokument nejdřív zahrnuje důležité rozdíly mezi systémem Windows Server AD DS a AD FS v Azure virtuálních počítačích mezi místním prostředím a důležité rozhodnutí, které ovlivňují návrhu a nasazení. Zbytek papíru vysvětluje pokyny pro jednotlivá pole bodů rozhodnutí podrobněji a postup při použití pravidla platí pro různé scénáře nasazení.

Tento článek se nezabývá konfigurace [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), která je na základě ZBÝVAJÍCÍ služba, která poskytuje Správa identit a možnosti řízení přístupu k získáte cloudu. Azure Active Directory (Azure AD) a Windows Server služby AD DS, ale, vám mají vzájemně ladily a poskytují identit a přístup k řešení správy pro hybridní nasazení aktuálnímu IT prostředí a moderní aplikace. Porozumět tomu rozdíly a vztahy mezi systému Windows Server AD DS a Azure AD, zvažte následující skutečnosti:

1. Můžete narazit systému Windows Server AD DS v cloudu na Azure virtuálních počítačích při použití Azure pro rozšíření místních datacentra do cloudu.
2. Můžete použít Azure AD a udělte vaší uživatelé jednotného přihlašování aplikacím Software jako-Service (SaaS). Microsoft Office 365 používá technologii, například a spuštěné v Azure nebo dalších platformách cloudu můžete je použít i.
3. Používáte Azure AD (jeho službě Řízení přístupu) při používání identit z Facebooku, Google, Microsoft a ostatní zprostředkovatelé identit jiní aplikacím, které jsou hostované v cloudu a místní umožníte uživatelům podepsat.

Další informace o těchto rozdílů najdete v tématu [Azure Identity](fundamentals-identity.md).

## <a name="related-resources"></a>Související materiály

Můžete stáhnout a spustit [Azure virtuálního počítače připravenosti hodnocení](https://www.microsoft.com/download/details.aspx?id=40898). Hodnocení automaticky prozkoumat místním prostředím a vygenerovat vlastní sestavu podle pokynů v tomto tématu můžete migrovat prostředí Azure.

Doporučujeme, abyste nejprve zkontrolujte výukové programy pro, vodítkům a videa, které najdete v těchto tématech:

- [Konfigurace jen cloudu virtuální sítě v portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Konfigurace webů webu VPN na portálu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Instalace nové doménové služby Active Directory na Azure virtuální sítě](active-directory-new-forest-virtual-machine.md)
- [Instalace řadiče domény Active Directory otevřené na Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: Základy (01) virtuálního počítače](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) vytváření virtuálních sítí a křížově místní připojení](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Úvod

Základní požadavky pro instalaci služby Active Directory pro Windows Server na Azure virtuálních počítačích lišit velmi málo ho nasazováním ve virtuálních počítačích místní (a do určité míry fyzických počítačů). Například v případě systému Windows Server AD DS, pokud domény řadiče nasazené na Azure virtuálních počítačích jsou kopie v existujícím místní podnikové domény a struktury, Azure nasazení můžete převážně považují stejným způsobem jako zacházet jiných dalšího webu služby Active Directory pro Windows Server a pak. To znamená je třeba definovat podsítí v systému Windows Server AD DS, web vytvořen, podsítí propojené k tomuto webu a připojení k jiné weby pomocí příslušné odkazy webu. Existuje, ale některé rozdíly, která jsou společná pro všechny Azure nasazení a některé, lišit podle toho, konkrétní scénáře. Níže jsou uvedeny dva zásadní rozdíly:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtuálních počítačích může být nutné připojení k firemní síti místní.

Připojení Azure virtuálních počítačích zpět k podnikové síti místní vyžaduje Azure virtuální sítě, která obsahuje webu webu nebo webu čárky virtuální privátní sítě (VPN) součást Bezproblémová připojit Azure virtuálních počítačích a využití místního počítače. Součásti tohoto sítě VPN mohou taky Povolit místní domény člena počítačů získali přístup ke službě Active Directory pro Windows Server domény řadiče domény se jejichž hostiteli jsou výhradně Azure virtuálních počítačích. Je důležité mít na paměti, ale, že když sítě VPN nepovede, ověřování a další operace, které jsou závislé na služby Active Directory pro Windows Server také nezdaří. Když uživatelé můžou přihlásit pomocí stávající mezipaměti přihlašovací údaje, všechny-účastníky nebo klient server ověřování pokusů o kterých lístky ještě vydává nebo se stali zastaralé se nezdaří.

Video ukázku a seznam podrobné výukové programy, včetně [Konfigurace webů webu VPN na portálu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)najdete v článku [Virtuální sítě](http://azure.microsoft.com/documentation/services/virtual-network/) .

> [AZURE.NOTE] Můžete taky nasadit služby Active Directory pro Windows Server Azure virtuální sítě, který nemá připojení s místní síti. Pokyny v tomto tématu se však předpokládá, že je Azure virtuální síti nepoužívají, protože poskytuje IP adres funkcí, které jsou důležité systému Windows Server.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statické IP adresy musí být nakonfigurované prostředí PowerShell Azure.

Dynamické adresy přiřazených ve výchozím nastavení, ale použít rutinu Set-AzureStaticVNetIP přiřadit statickou IP adresu. Nastaví statické IP adresy, kterou bude trvat až retušování služby a OM vypnutí a znovu ho spusťte. Další informace najdete v tématu [statický interní IP adresa virtuálních počítačích](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Pojmy a definice

Následuje seznam není vyčerpávající termínů pro různé Azure technologie, které se odkazuje v tomto článku.

- **Azure virtuálních počítačích**: IaaS nabízet Azure, který umožňuje uživatelům nasazení VMs systém téměř libovolném obvykle místní zatížení serveru.

- **Azure virtuální sítě**: službě sociální sítě v Azure, který umožňuje zákazníci vytvářet a spravovat virtuální sítě Azure a bezpečně propojit svoje vlastní místní síťovou infrastrukturu pomocí virtuální privátní sítě (VPN).

- **Virtuální IP adresa**: internetového IP adresu, která není vázaný na určité karty rozhraní počítači nebo v síti. Cloudové služby, mají přiřazenou virtuální IP adresu pro příjem síťová komunikace, které je přesměruje do OM Azure. Virtuální IP adresu je vlastnost do cloudové služby, které může obsahovat jedno nebo více Azure virtuálních počítačích. Navíc nezapomeňte, že Azure virtuální sítě může obsahovat jedno nebo více cloudovým službám. Virtuální IP adresy poskytují nativní Vyrovnávání zatížení možnosti.

- **Dynamické IP adresa**: to je IP adresy, kterou je vnitřní jenom. Ho měli jako statické IP adresy (můžete použít rutinu Set-AzureStaticVNetIP) pro nakonfigurovaný VMs hostujících role serverů DNS/řadiče domény.

- **Retušování služby**: proces, ve kterém Azure automaticky vrátí službu spuštěna znovu po zjistí, že došlo k chybě služby. Služba retušování je jeden z hlediska Azure, který podporuje dostupnost a odolnost proti chybám. Pokud pravděpodobné, výsledek po službu retušování incidentu pro Datacentrum virtuálního počítače se systémem je podobný neplánované restartujte počítač, ale má několik důsledky:

 - Virtuální síťový adaptér v OM se změní
 - Adresa MAC virtuální síťový adaptér se změní
 - Procesor pro platformu/procesoru ID OM se změní
 - Konfigurace IP virtuální síťový adaptér nezmění dlouhou, jak OM je připojen k síti virtuální a OM IP adresu je statický.

 Žádné z těchto chování vliv služby Active Directory pro Windows Server, protože nemá žádná závislá na adresu MAC nebo ID procesor/procesoru a všechna nasazení služby Active Directory pro Windows Server na Azure doporučují se mají v Azure virtuální sítě podle pokynů uvedených výše.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Sice při virtualizaci řadiče domény Windows Server Active Directory?

Nasazení serveru Active Directory řadiče domény systému Windows Azure virtuálních počítačích podléhá pokynů stejný jako řadiče domény místního aplikaci virtuálního počítače. Spuštění virtualizovaném řadiče domény je bezpečných si také řídit pokyny pro zálohování a obnovování řadiče domény. Další informace o omezení a pokyny pro spuštění virtualizovaném řadiče domény najdete v článku [Spuštěný řadiče domény v Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisory poskytnout nebo trivialize technologiemi, které mohou způsobit problémy s mnoha distribuované systémy v počítačích, včetně služby Active Directory pro Windows Server. Například na serveru fyzické můžete zkopírovat na disk nebo použít nepodporovaných metod k vrácení stavu ze serveru, včetně použití SANs a tak dále, ale řešením konfliktů na serveru fyzické je mnohem složitější než obnovení snímek virtuálního počítače v hypervisor. Azure nabízí funkce, které můžou mít za následek nežádoucích stejným. Například nekopírujte virtuální pevný disk soubory řadiče domény místo provádění pravidelného zálohování vzhledem k jejich obnovování může být v podobné situaci pomocí funkce obnovení snímek.

Tyto vrácení zpět Představte USN bubliny, které mohou vést trvale odlišných stav mezi řadiče domény. Například, které mohou způsobit problémy:

- Mít objektů
- Nekonzistentní hesla
- Nekonzistentní atributů
- Schéma neshoduje Pokud schémat vrátit zpět

Další informace o tom, jak jsou ovlivněny řadiče domény najdete v tématu [USN a vrácení USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Začínající systému Windows Server 2012, [doplňkové záruky jsou součástí služby AD DS](https://technet.microsoft.com/library/hh831734.aspx). Tyto záruky chránit virtualizovaném domény řadiče proti výše uvedených problémů, dokud základní platformě hypervisoru podporuje OM GenerationID. Azure podporuje OM GenerationID, což znamená, že doplňkové záruky řadiče domény, na kterých běží Windows Server 2012 nebo novějším Azure virtuálních počítačích.

> [AZURE.NOTE] Mají vypnout a restartujte OM, které se spouští roli řadiče domény v Azure v operačním systému hosta místo použití **příkazu Vypnout** Azure klasické portálu. V současné době pomocí portálu klasické vypnout virtuálního počítače způsobí, že OM být odebrána. Zrušeny, takže OM má tu výhodu není nabíhání poplatků za, ale je také obnoví OM GenerationID tedy nežádoucí u řadiče domény. Po obnovení OM GenerationID invocationID databáze služby AD DS také resetovat, vyřazené fondu serveru relativních ID a SYSVOL označen jako neautoritativních. Další informace najdete v tématu [Úvod do služby Active Directory Domain Services (AD DS) virtualizaci](https://technet.microsoft.com/library/hh831734.aspx) a [Bezpečné virtualizace DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Proč nasazení systému Windows Server AD DS na virtuálních počítačích Azure?

Mnoha způsobech nasazení systému Windows Server AD DS jsou vhodné pro nasazení jako VMs Azure. Předpokládejme například, že jste ze společnosti v Evropě, kterou je potřeba ověřování uživatelů ve vzdáleném umístění v Asii. Společnosti nebyla nasazené dříve Windows Server Active Directory v sekundách Asie kvůli náklady na nasazení je a omezené odborných informací ke správě po nasazení servery. Požadavky na ověření od Asie jsou vyřízení v důsledku toho řadiče domény v Evropě s získat výsledky. V tomto případě nástroje můžete nasazovat Datacentrum na OM, který jste zadali spuštěním v Azure datacentru v Asii. Připojení, které Datacentrum Azure virtuální sítě, které je připojené přímo k vzdáleném umístění zlepšíte výkon ověřování.

Azure, není vhodné jako náhrada na weby v opačném případě nákladné havárie obnovení (DR). Relativně minimum náklady hostingu malým počtem poštovních řadiče domény a jedné sítě virtuální na Azure představuje atraktivní alternativy.

Nakonec můžete nasadit síť na Azure, například SharePoint, který vyžaduje Windows Server Active Directory, ale mám žádná závislost na místní síti nebo firemní Windows Server Active Directory. V tomto případě nasazení izolace doménové na Azure plnit služby SharePoint serveru požadavky je optimální. Znovu nasazení síťové aplikace, které vyžadují připojení k místní síti a podnikové služby Active Directory podporuje i.

> [AZURE.NOTE] Od poskytuje layer 3 připojení, virtuální privátní sítě součásti, která poskytuje propojení mezi Azure virtuální sítě a v místní síti můžete taky povolit členské servery se systémem místních můžete využít řadiče domény, které fungují jako Azure virtuálních počítačích Azure virtuální síti se systémem. Ale pokud sítě VPN není k dispozici, komunikace mezi místním prostředím počítačů a založené na Azure domény řadiče nebude fungovat, následek ověřování a různé jiné chyby.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Kontrastu mezi nasazení řadiče domény služby Active Directory pro Windows Server na virtuálních počítačích Azure a místních

- Pro všechny služby Active Directory pro Windows Server nasazení scénář, který obsahuje více než jednoho OM je nutné použít Azure virtuální sítě IP adresu konzistence. Všimněte si, že tato příručka předpokládá, že řadiče domény používáte v síti Azure virtuální.

- Stejně jako u řadiče domény místního doporučují statické IP adres. Statickou IP adresu je možné konfigurovat pouze pomocí prostředí PowerShell Azure. Další informace najdete v článku [statický interní IP adresa VMs](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Pokud máte sledování systémy nebo jiné řešení, která Kontrola konfigurace statické IP adresy v operačním systému Host, můžete přiřadit vlastnosti adaptér sítě OM stejné statickou IP adresu. Ale mějte na paměti, že budou ignorována síťový adaptér, pokud OM nastanou retušování služby nebo ukončení práce v portálu klasické a byla odebrána jeho adresu. V takovém případě statickou IP adresu v rámci hosta muset obnovit.

- Nasazení VMs virtuální síti se systémem není naznačující (nebo vyžadovat) připojení zpátky na místní síti. virtuální sítě pouze umožňuje tuto možnost. Je třeba vytvořit virtuální síť pro soukromé komunikaci mezi Azure a v místní síti. Potřebujete nasazení VPN koncového bodu v místní síti. Virtuální privátní síť upraveného z Azure na místní síti. Další informace najdete v tématu [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md) a [Konfigurace webu webu VPN na portálu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Možnost [vytvoření VPN čárky webu](../vpn-gateway/vpn-gateway-point-to-site-create.md) neexistuje jednotlivé počítače se systémem Windows přímé připojení k síti Azure virtuální.

- Bez ohledu na to, zda vytváříte virtuální v síti nebo Ne, Azure poplatky za výstupní vysílání, ale ne průniku. Různé možnosti návrhu služby Active Directory pro Windows Server můžou mít vliv množství přenosů výstupní je generováno aplikací na nasazení. Například zavádění řadiče domény jen pro čtení (jen pro čtení) omezuje výstupní přenosy vzhledem k tomu, že není replikovat odchozí. Ale rozhodnutí o nasazení řadiče musí být porovnat třeba provádět operace zápisu u řadiče domény a [funkce pro kompatibilitu](https://technet.microsoft.com/library/cc755190) obsahujících aplikací a služeb na webu s řadiče. Další informace o nákladech přenosy najdete v článku [Azure ceny na přehled](http://azure.microsoft.com/pricing/).

- Když máte dokončení určit, přes který server materiály pro účely VMs místní, například RAM, disk velikost a tak dále, na Azure musíte vybrat ze seznamu velikostí předkonfigurované serveru. Pro Datacentrum disk dat potřebné kromě disk operačního systému k uložení databáze služby Active Directory pro Windows Server.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Můžete nasadit systému Windows Server AD FS v Azure virtuálních počítačích?

Ano, nasazením systému Windows Server AD FS v Azure virtuálních počítačích a [osvědčené postupy pro nasazení služby AD FS](https://technet.microsoft.com/library/dn151324.aspx) místního nasazení služby AD FS v Azure vyrovnat rovnoměrně z vnější. Ale některé osvědčené postupy jako Vyrovnávání zatížení a dostupnost vyžadují technologie za co AD FS nabízí samotné. Musí být uvedeny základní infrastruktura webových aplikací. Pojďme zkontrolujte některé z těchto osvědčené postupy a najdete v článku Jak lze dosáhnout pomocí Azure VMs a Azure virtuální sítě.

1. **Nikdy vystavit zabezpečení tokenu service (Služba tokenů zabezpečení) servery přímo k Internetu.**

    To je důležité, protože služba tokenů zabezpečení problémy tokenů zabezpečení. Výsledkem je služba tokenů zabezpečení servery například servery služby AD FS zacházet se stejnou úrovní ochrany jako řadiče domény. Pokud je služba tokenů zabezpečení ohroženo, zlými úmysly dokážou o vystavení přístupové tokeny potenciálně obsahující uplatnění jejich výběru předávající aplikacích stran a jiné servery služba tokenů zabezpečení v důvěryhodnosti organizace.

2. **Nasazení řadiče domény Active Directory pro všechny domény uživatelů ve stejné síti jako servery služby AD FS.**

    Servery AD FS pomocí Active Directory Domain Services pro ověřování uživatelů. Je vhodné pro nasazení řadiče domény ve stejné síti jako servery služby AD FS. Nepřerušený provoz nabízí v případě přerušení propojení mezi Azure síť a v místní síti a umožňuje nižší latence a lepší výkon pro přihlášení.

3. **Nasazení více uzlů AD FS dostupnost a vyrovnávání zatížení.**

    Ve většině případů selhání aplikace, která umožňuje AD FS totiž nepřijatelná klíčové aplikace, které vyžadují tokenů zabezpečení, jsou často. V důsledku toho a protože AD FS teď nachází v kritickou cestu k otevření aplikace jsou kritické, musí být službu AD FS vysoce dostupné prostřednictvím více proxy AD FS a servery služby AD FS. Dosáhnout rozdělení žádostí o jsou obvykle Vyrovnávání zatížení nasazeny před proxy servery AD FS a servery služby AD FS.

4. **Nasazení jeden nebo více uzlů proxy serveru webové aplikace pro přístup k Internetu.**

    Když uživatelé potřebují pro přístup k aplikacím chráněné službou AD FS, služby AD FS musí být k dispozici z Internetu. Je to dosáhnout nasazení služeb proxy serveru webové aplikace. Důrazně doporučujeme nasadit více než jeden uzel pro účely dostupnost a vyrovnávání zatížení.

5. **Omezení přístupu k prostředkům vnitřní síti z uzlů proxy serveru webové aplikace.**

    Umožňuje externím uživatelům přístup k AD FS z Internetu, budete muset nasazení uzly proxy serveru webové aplikace (nebo Proxy AD FS ve starších verzích Windows Server). Uzly proxy serveru webové aplikace jsou přímo vystaven na Internetu. Nejsou musí být doméně a potřebují přístup k serverům služby AD FS přes porty TCP 80 a 443. Doporučujeme, že je blokován komunikace pro všechny ostatní počítače (zejména řadiče domény).

    Toto je obvykle dosažená místní prostřednictvím DMZ. Bránách firewall, používejte režim povolených operace zabránit přenosy DMZ v místní síti (to znamená pouze jsou povoleny přenosy ze zadaných IP adres a zadaný porty a všechny ostatní přenosy jsou blokovány).

Následující diagram ukazuje a tradiční místního nasazení služby AD FS.

![Diagram tradiční místního nasazení Active Directory Federation Services](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Protože však Azure neposkytuje nativní funkce plnohodnotně vybaveného bránu firewall, další možnosti třeba chcete použít pro omezení přenosy. V následující tabulce jsou uvedeny jednotlivých možností a jeho výhody a nevýhody.

| Možnost | Výhody | Nevýhodou |
| ------ | --------- | ------------ |
| [ACL Azure sítě](virtual-networks-acl.md) | Méně nákladná a jednodušší počáteční konfigurace | Konfigurace ACL další sítě povinné, pokud novou VMs se přidají do nasazení |
| [Brána firewall barracuda NG](https://www.barracuda.com/products/ngfirewall) | Režim povolených operace a vyžaduje konfiguraci ACL bez sítě | Vyšší náklady a složitost počáteční instalace |

Vysoké úrovni nasadit služby AD FS v tomto případě jsou kroky:

1. Vytvořte virtuální sítě s více místních poštovních připojení, virtuální privátní sítě nebo [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Nasazení řadiče domény virtuální síti se systémem. Tento krok je nepovinný doporučené.

3. Nasazení doméně servery služby AD FS virtuální síti se systémem.

4. Vytvoření [interní zatížení rovnováha nastavení](http://azure.microsoft.com/blog/internal-load-balancing/) , která obsahuje servery služby AD FS a používá soukromé IP adresu nového ve virtuálních síti (dynamic IP adresa).

  1. Aktualizace DNS vytvořit plně kvalifikovaný název domény tak, aby ukazovaly na soukromé (dynamic) IP adresu množiny interní rozloženy.

5. Vytvoření do cloudové služby (nebo samostatné virtuální sítě) pro uzly proxy serveru webové aplikace.

6. Nasazení uzly proxy serveru webové aplikace do cloudové služby, nebo virtuální sítě

  1. Vytvoření externího rozloženy obsahující uzly proxy serveru webové aplikace.

  2. Aktualizace názvu externí DNS (FQDN) tak, aby ukazovaly na cloudu služby veřejnou IP adresu (virtuální IP adresa).

  3. Konfigurace proxy AD FS používat plně kvalifikovaný název domény, který odpovídá sadu interní rozloženy pro servery služby AD FS.

  4. Aktualizujte na základě deklarací identit weby určenou poskytovatelem deklarací externí plně kvalifikovaný název domény.

7. Omezení přístupu mezi proxy serveru webové aplikace na všechny počítače v síti virtuální AD FS.

Pokud chcete omezit přenosy, sadu Vyrovnávání zatížení pro vyrovnávání zatížení Azure interní vyžaduje konfiguraci pouze přenosů na porty TCP 80 a 443 a všechny přenosy na vnitřní dynamické IP adresu množiny rozloženy nezobrazí.

![Diagram sítě ACL služby AD FS s TCP 443 + 80 povoleno.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Umožnění datových přenosů do servery služby AD FS by být povoleném jenom v následujících zdrojích:

- Vyrovnávání zatížení Azure vnitřní.
- IP adresy správce v místní síti.

> [AZURE.WARNING] Návrh musí proxy serveru webové aplikace uzly zabránit doručení jiných VMs v Azure virtuální sítě nebo všechna umístění v místní síti. Který lze provést pomocí konfigurace pravidel brány firewall v zařízení místní připojení Express směrování nebo zařízení virtuální privátní sítě pro připojení VPN k webu.

Nevýhodou tato možnost je nutné ke konfiguraci sítě ACL pro víc zařízení, ať Vyrovnávání zatížení interní servery služby AD FS a jiné servery, které se přidat virtuální síť. Pokud libovolného zařízení se přidá do nasazení bez konfigurace sítě ACL omezte přenosy, může být celé nasazení riziko. Pokud IP adresy proxy serveru webové aplikace uzlů kdykoliv změnit, sítě ACL musí být obnovit (což znamená, že náhledy by měl být nakonfigurované pro použít [dynamické statické IP adresy](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![Služby AD FS v Azure k síti ACL.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Další možností je pomocí zařízení [Brány Firewall NG Barracuda](https://www.barracuda.com/products/ngfirewall) můžete určit, komunikaci mezi servery proxy AD FS a servery služby AD FS. Tato možnost je v souladu s doporučené postupy pro zabezpečení a dostupnost a vyžaduje menší správu po počáteční instalaci, protože zařízení brány Firewall NG Barracuda poskytuje režim povolených správy bránu firewall a možné nainstalovat přímo na Azure virtuální sítě. Eliminuje potřebujete ke konfiguraci sítě ACL kdykoli nového serveru se přidá do nasazení. Ale tuto možnost přidá počáteční nasazení složitostí i náklady.

V tomto případě dvě virtuální sítě jsou nasazené místo jedné. Budeme se jim zavolá VNet1 a VNet2. VNet1 obsahuje náhledy a VNet2 STSs a připojení k síti zpět k podnikové síti. VNet1 tedy fyzicky (i když prakticky) samostatný z VNet2 a zároveň z podnikové sítě. VNet1 je připojen k VNet2 pomocí speciálních tunelování technologie označované jako Transport nezávisle na síťové architektury (VPDI). Tunelem VPDI je připojen k jednotlivým virtuálních sítí používá bránu firewall Barracuda NG – jeden Barracuda jednotlivých hodnot virtuální sítě.  Pro vysokou dostupnost doporučujeme nasazení dvou barakudy v každé virtuální síti; aktivních, jiné pasivní. Nabízejí velmi spoustu možností fungující brána firewall může, které umožňují, abychom vám napodobit operace tradiční místního DMZ v Azure.

![Služby AD FS v Azure s bránou firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Další informace najdete v tématu [AD FS: rozšíření aplikaci používající deklarace místního front-end k Internetu](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Alternativy k nasazení služby AD FS pokud cílem je Office 365 jednotného přihlašování k sám o sobě

Je další možnost nasazením AD FS úplně, pokud je vaším cílem pouze povolit přihlášení k Office 365. V takovém případě můžete jednoduše nasazení DirSync s heslo synchronizaci místních a dosáhnout stejný konečný výsledek s minimálními nasazení složitost, protože tento přístup nevyžaduje služby AD FS nebo Azure.

Následující tabulka porovnává fungování procesy přihlásit se a bez nasazení služby AD FS.

| Office 365 jednoho přihlašování pomocí služby AD FS a DirSync | Office 365 stejné přihlašování pomocí DirSync + synchronizaci hesel |
| ------------- | ------------- |
| 1. na uživatel přihlásí k podnikové síti a ověření služby Active Directory pro Windows Server. | 1. na uživatel přihlásí k podnikové síti a ověření služby Active Directory pro Windows Server. |
| 2. na uživatel pokusí o přístup k Office 365 (jsem @contoso.com). | 2. na uživatel pokusí o přístup k Office 365 (jsem @contoso.com). |
| 3. office 365 přesměruje uživatele Azure AD. | 3. office 365 přesměruje uživatele Azure AD. |
| 4. vzhledem k tomu Azure AD nemůže ověřit uživatele a rozumí je zabezpečení pomocí služby AD FS místní, přesměruje uživatele služby AD FS. | 4. azure AD nepřijímá přímo lístky protokolu Kerberos a vztah důvěryhodnosti existuje, a proto je vyžaduje, aby uživatel zadat pověření. |
| 5. uživatel odešle lístek protokolu Kerberos AD FS STS. | 5. uživatel zadá stejné heslo místního a Azure AD ověřuje je uživatelské jméno a heslo, které synchronizovaná DirSync. |
| 6. služby AD FS transformací lístek Kerberos k požadovaný formát/deklarace tokenů a přesměruje uživatele Azure AD. | 6. azure AD přesměruje uživatele do Office 365. |
| 7. uživatel se ověří Azure AD (jiné transformace dojde). |  7. uživatele se přihlásit k Office 365 a pomocí tokenu Azure AD OWA. |
| 8. azure AD přesměruje uživatele do Office 365. |  |
| 9. uživatel tiše přihlášení do Office 365. |  |

V Office 365 pomocí DirSync s heslo synchronizace scénář (bez AD FS) jednotné přihlašování nahrazuje "stejné přihlašování" kde "stejné" jednoduše znamená, že uživatelé musí zadejte znova jejich stejné místní přihlašovací údaje při přístupu k Office 365. Všimněte si, že tato data můžete mít na paměti, tak, že v prohlížeči uživatele objemu následujících pokynů.

### <a name="additional-food-for-thought"></a>Další myšlenkových pro jídla

- Pokud nasadíte proxy AD FS v Azure virtuálního počítače, je potřeba připojení k servery služby AD FS. Pokud jsou místní, je vhodné využít připojení VPN k webu poskytovanou virtuální sítě umožňuje uzly proxy serveru webové aplikace komunikovat s jejich servery služby AD FS.

- Pokud jste nasazení server služby AD FS v Azure virtuálního počítače, je nutné připojení k řadiče domény služby Active Directory pro Windows Server, atribut ukládá a konfigurační databáze a taky požadovat ExpressRoute nebo připojení VPN k webu mezi Azure virtuální sítě a v místní síti.

- Náklady platí pro všechny přenosy z Azure virtuálních počítačích (výstupní přenos). Pokud náklady jsou řidiče k jízdě faktor, je vhodné uzly proxy serveru webové aplikace v Azure, nechte AD FS servery místního nasazení. Pokud jsou servery služby AD FS nasazený na Azure virtuálních počítačích i, další náklady vynaložené ověření místních uživatelů. Výstupní přenosy poněkud náklady bez ohledu na to, zda je procházení ExpressRoute nebo připojení VPN k webu.

- Pokud se rozhodnete používat v Azure nativní server, Vyrovnávání zatížení funkcí pro dostupnost servery služby AD FS, mějte na paměti, že vyrovnávání zatížení poskytuje sond, které se používají k určení stavu virtuálních počítačích do cloudové služby. V případě Azure virtuálních počítačích (namísto webu nebo pracovního role) je nutné použít vlastní zkušební od agent, který odpovídá sond výchozí není k dispozici v Azure virtuálních počítačích. Pro zjednodušení, můžete použít vlastní zkušební TCP – při této akci musí pouze, že TCP připojení (segmentu TCP SYN poslali a odpověděl segmentu TCP SYN ACK) úspěšně vytvořit virtuální počítač stav. Můžete nakonfigurovat vlastní zkušební použít jakýkoli TCP, ke kterému virtuálních počítačích naslouchají aktivně.

> [AZURE.NOTE] Stroje, které je potřeba vystavit stejnou sadu porty přímo k Internetu (například port 80 a 443) nejde sdílet stejné cloudové služby. Proto doporučujeme vytvořit vyhrazené cloudové služby pro servery systému Windows Server AD FS a zabránit tak potenciální překrývá mezi požadavky na port pro aplikace a Windows Server AD FS.

## <a name="deployment-scenarios"></a>Nasazení scénářů

V následující části popisuje scénáře běžné nasazení a upozornit tak důležité informace, které musíte vzít v úvahu. Každý scénář obsahuje odkazy na další podrobnosti o rozhodnutí a faktory zvážit.

1. [Služba AD DS: Nasazení aplikace podporující AD DS s nepožaduje pro připojení k firemní síti](#BKMK_CloudOnly)

    Například služby SharePoint internetového nasazené na Azure virtuálního počítače. Aplikace nemá žádné závislosti v podnikové síti zdroje. Aplikace vyžaduje Windows Server AD DS ale nevyžaduje podnikové služby AD DS systému Windows Server.

2. [Služba AD FS: Rozšiřte front-end aplikaci používající deklarace místního k Internetu](#BKMK_CloudOnlyFed)

    Příklad používající deklarace aplikace, které byly úspěšně nasazené místního a používá podnikoví uživatelé musí osvobozením od přístupné z Internetu. Aplikace potřebuje k nim získat přístup přímo přes Internet partnery firmy pomocí vlastní firemní identity i tak, že stávající podnikové uživatele.

3. [Služba AD DS: Nasazení systému Windows Server AD DS podporujících aplikaci, která vyžadují připojení k firemní síti](#BKMK_HybridExt)

    Například podporující LDAP aplikace, která podporuje ověřování integrované v systému Windows a používá Windows Server AD DS jako úložiště pro konfiguraci a profilů uživatelů data nasazené na Azure virtuálního počítače. Je vhodné pro aplikaci pro využít stávající podnikové systému Windows Server AD DS a poskytování jednotného přihlašování. Aplikace není používající deklarace.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Nasazení aplikace podporující AD DS s nepožaduje pro připojení k firemní síti

![Cloudové služby AD DS nasazení](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Obrázek 1**

#### <a name="description"></a>Popis

Nasazení Sharepointu v Azure virtuální počítači a aplikace nemá žádné závislosti v podnikové síti zdroje. Aplikace vyžaduje Windows Server AD DS ale neobsahuje *není* vyžadují podnikové systému Windows Server AD DS. Protokol Kerberos ani federované vztahy důvěryhodnosti služby podporují vzhledem k tomu, že uživatelé můžou vlastním zřizování prostřednictvím aplikace do systému Windows Server AD DS domény, který hostuje taky v cloudu na Azure virtuálních počítačích.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scénář aspektech a jak technologické oblasti použijte scénáře

- [Topologie sítě](#BKMK_NetworkTopology): vytvoření Azure virtuální sítě bez připojení mezi místní (označovaná taky jako připojení k webu).

- [Konfigurace nasazení Datacentrum](#BKMK_DeploymentConfig): nasazení nového řadiče domény do nového, jednou doménou, struktury služby Active Directory pro Windows Server. To by měly být nasazeny spolu s server DNS systému Windows.

- [Topologie služby Active Directory pro Windows Server](#BKMK_ADSiteTopology): použijte výchozí web služby Active Directory pro Windows Server (všech počítačů budete mít ve výchozí-webu – jméno).

- [IP adresy a DNS](#BKMK_IPAddressDNS):

 - Nastavte statickou IP adresu pro řadiče domény pomocí rutiny prostředí PowerShell Set-AzureStaticVNetIP Azure.
 - Instalace a nastavení systému Windows Server DNS domény řídicímu na Azure.
 - Konfigurace vlastností virtuální sítě s názvem a IP adresu OM, který je hostitelem role serveru řadiče domény a DNS.

- [Globální katalog](#BKMK_GC): první Datacentrum ve struktuře, musíte být globální katalog. Další řadiče domény by měl taky nakonfigurovat jako globálních, protože ve struktuře jednu doménu, globální katalog není nutné zadávat jakékoli další práce z řadiče domény.

- [Umístění databáze systému Windows Server AD DS a SYSVOL](#BKMK_PlaceDB): přidání dat disku řadiče domény, abyste mohli uložit databázi služby Active Directory pro Windows Server, protokoly a SYSVOL spuštěný jako Azure VMs.

- [Zálohování a obnovení](#BKMK_BUR): Určete, kde má být ukládat zálohování stavu systému. V případě potřeby přidáte další data disk OM Datacentrum pro ukládání záložní kopie.

### <a name="BKMK_CloudOnlyFed"></a>ADFS 2: rozšíření aplikaci používající deklarace místního front-end k Internetu

![Federaci s více místní připojení](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Obrázek 2**

#### <a name="description"></a>Popis

Používající deklarace aplikace, které byly úspěšně nasazeném místních a používá podnikoví uživatelé musí být přístupné přímo z Internetu. Aplikace slouží jako web frontend k SQL databázi, ve kterém ukládá data. Servery SQL používané aplikace nacházejí na podnikové sítě. Dva STSs systému Windows Server AD FS a vyrovnávání zatížení byly nasazeném místní zajistit přístup k podnikové uživatele. Teď potřebuje navíc přístup ke přímo přes Internet partnery firmy pomocí vlastní firemní identity i tak, že stávající podnikové uživatele.

V úsilí ke zjednodušení a potřebám nasazení a konfiguraci tento nový požadavek je rozhodla dva další webové frontends a dva servery proxy serveru Windows Server AD FS nainstalovat na Azure virtuálních počítačích. Všechny čtyři VMs bude vystaven přímo na Internetu a se dozvíte při připojení k síti místní pomocí funkce VPN k webu Azure virtuální sítě.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scénář aspektech a jak technologické oblasti použijte scénáře

- [Topologie sítě](#BKMK_NetworkTopology): vytvoření Azure virtuální sítě a [Konfigurace křížově místní připojení](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] U každé certifikáty systému Windows Server AD FS zajistit, aby adresa URL smyslu šabloně certifikátu a výsledná certifikáty zastižení instancemi systému Windows Server AD FS spuštěna Azure. Může být nutné křížově místní připojení k díly infrastrukturu KLÍČŮ. Pro příklad Pokud koncového bodu CRL je založený na LDAP a hostovaný výhradně místní, pak křížově místní, že připojení, budete vyzváni. Pokud to není vhodné, může být nutné použít vydaná CA, jehož CRL přístupný přes Internet.

- [Konfigurace služby cloudu](#BKMK_CloudSvcConfig): Zkontrolujte, máte dvě cloudové služby v pořadí poskytují dva Vyrovnávání zatížení virtuální IP adres. První cloudové služby virtuální IP adresu přesměrovaní na dvě VMs proxy serveru Windows Server AD FS na porty 80 a 443. Proxy systému Windows Server AD FS VMs bude nakonfigurované tak, aby ukazovaly na IP adresu Vyrovnávání zatížení místní této naskenováno předních STSs systému Windows Server AD FS. Druhá cloudové služby virtuální IP adresu přesměrovaní na dvě VMs spuštěním web frontend na porty 80 a 443. Konfigurace vlastních zkušební zajistit, aby že vyrovnávání zatížení pouze směrovat tak, aby umožnění datových přenosů do funkční systému Windows Server AD FS proxy a web frontend VMs.

- [Konfigurace federace serveru](#BKMK_FedSrvConfig): Konfigurace systému Windows Server AD FS jako federačním serverem STS () generovat tokenů zabezpečení pro struktuře služby Active Directory pro Windows Server vytvořené v cloudu. Nastavit federaci vztahy důvěryhodnosti poskytovatele deklarací s jinou partnerů, které chcete přijmout identit z a konfigurace předávající vztahy důvěryhodnosti stran pomocí různých aplikacích chcete generovat tokeny k.

    Ve většině případů servery proxy serveru Windows Server AD FS nasazenou v internetového kapacita z bezpečnostních důvodů, přičemž jejich protějšky federace systému Windows Server AD FS, zůstávají stále izolovaný od přímé připojení k Internetu. Bez ohledu na to nefunguje nasazení je třeba nakonfigurovat cloudové služby virtuální IP adresu, poskytující veřejně vystavovaných IP adresa a portu, které je možné Vyrovnávání zatížení přes dvě instance systému Windows Server AD FS STS nebo instance proxy.

- [Konfigurace systému Windows Server AD FS dostupnost](#BKMK_ADFSHighAvail): je vhodné nasazení systému Windows Server AD FS farma s nejméně dvěma servery pro převzetí a vyrovnávání zatížení. Může být vhodnější pomocí Windows interní databáze souboru () pro data konfigurace systému Windows Server AD FS a použít funkci Vyrovnávání zatížení vnitřní systému Azure k distribuci příchozí žádosti na serverech ve farmě.

Další informace najdete v tématu [Průvodce nasazením AD DS](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Nasazení systému Windows Server AD DS podporujících aplikaci, která vyžadují připojení k firemní síti

![Křížově místního nasazení služby AD DS](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Obrázek 3**

#### <a name="description"></a>Popis

LDAP podporujících aplikaci nasazené na Azure virtuálního počítače. Podporuje ověřování integrované v systému Windows a používá Windows Server AD DS jako úložiště pro konfiguraci a uživatelského profilu data. Cílem je určený pro aplikaci pro využít existující podnikové systému Windows Server služby AD DS a poskytování jednotného přihlašování. Aplikace není používající deklarace. Uživatelé potřebovat přístup k aplikaci přímo z Internetu. Optimalizace výkonu a náklady, je určeno nasadit dva další řadiče domény, které jsou součástí domény společnosti vedle aplikace na Azure.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scénář aspektech a jak technologické oblasti použijte scénáře

- [Topologie sítě](#BKMK_NetworkTopology): vytvoření Azure virtuální sítě s [více místní připojení](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Způsob instalace](#BKMK_InstallMethod): nasazení řadiče domény otevřené z domény společnosti služby Active Directory pro Windows Server. Pro otevřené řadiče domény můžete nainstalovat OM systému Windows Server AD DS a volitelně použít funkci nainstalovat z médií (instalaci z média) snížit množství dat, kterou je potřeba se během instalace replikovat na nové řadiče domény. Kurz najdete v článku [instalace řadiče domény Active Directory otevřené na Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). I když použijete instalaci z média, může být více efektivně sestavovat virtuální Datacentrum místní a přesunutí celý virtuální pevného disku (virtuální pevný disk) do cloudu místo replikace systému Windows Server AD DS během instalace. Zabezpečení se doporučuje po byly zkopírovány do Azure odstranit virtuální pevný disk z místní síti.

- [Topologie služby Active Directory pro Windows serveru](#BKMK_ADSiteTopology): vytvoření nového webu Azure Active Directory sítě a služby. Vytvoření objektu služby Active Directory pro Windows Server podsítě ke znázornění Azure virtuální sítě a přidejte podsítě na web. Vytvoření nového webu odkazu, která obsahuje nové Azure webů a webů, ve kterém se nachází za účelem kontroly a optimalizace služby Active Directory pro Windows Server příchozích a odchozích Azure Azure virtuální sítě VPN koncového bodu.

- [IP adresy a DNS](#BKMK_IPAddressDNS):

 - Nastavte statickou IP adresu pro řadiče domény pomocí rutiny prostředí PowerShell Set-AzureStaticVNetIP Azure.
 - Instalace a nastavení systému Windows Server DNS domény řídicímu na Azure.
 - Konfigurace vlastností virtuální sítě s názvem a IP adresu OM, který je hostitelem role serveru řadiče domény a DNS.

- [Distribuované GEO řadiče domény](#BKMK_DistributedDCs): Konfigurace další virtuální sítě podle potřeby. Pokud topologii webu služby Active Directory vyžaduje v zeměpisných, které odpovídají různé regiony a Azure, než chcete vytvářet weby služby Active Directory příslušným způsobem sekundách.

- [Jen pro čtení řadiče domény](#BKMK_RODC): může nasadit řadiče Azure webu, v závislosti na vašim požadavkům pro provádění psaní operace u řadiče domény a kompatibilita aplikací a služeb na webu s řadiče. Podrobnosti o kompatibilitě aplikací najdete v článku [Průvodce kompatibility aplikace řadiče domény jen pro čtení](https://technet.microsoft.com/library/cc755190).

- [Globální katalog](#BKMK_GC): globálních jsou potřeba k žádosti o přihlášení služby v rámci doménových. Pokud není nasadit globální Katalog Azure webu, se budou vynakládá výstupní přenosy jako požadavky ověřování způsobit dotazů globálních v jiných sítích. Minimalizovat tento přenos, můžete povolit univerzální skupina členství ukládání do mezipaměti pro Azure web ve službě Active Directory webům a službám.

    Pokud nasadíte globální Katalog, konfigurace odkazů webu a náklady odkazů webu ° c Azure webu nedoporučujeme jako zdroj řadiče domény tak, že ostatní globálních, které je potřeba replikovat stejné oddílů částečné domény.

- [Umístění databáze systému Windows Server AD DS a SYSVOL](#BKMK_PlaceDB): Přidat disk dat do provozu v Azure VMs k uložení databáze služby Active Directory pro Windows Server, protokoly a SYSVOL řadiče domény.

- [Zálohování a obnovení](#BKMK_BUR): Určete, kde má být ukládat zálohování stavu systému. V případě potřeby přidáte další data disk OM Datacentrum pro ukládání záložní kopie.

## <a name="deployment-decisions-and-factors"></a>Nasazení rozhodnutí a faktory

Tato tabulka shrnuje oblasti technologii služby Active Directory pro Windows Server, které jsou ovlivněny v předchozích případů a odpovídající rozhodnutí vzít v úvahu, s odkazy na další podrobnosti dole. Některé oblasti technologie nemusí být k dispozici všechny scénáře a některých oblastech technologie může být více naprosto zásadní scénáři nasazení než jiné oblasti technologii.

Třeba když nasadíte otevřené Datacentrum v síti virtuální a doménové struktury obsahuje pouze jednu doménu, zadáním požadavku na nasazení serveru globální katalog v takovém případě nebude naprosto zásadní scénář nasazení protože nevytvoří jakékoli další replikace požadavky. Na druhé straně Pokud struktuře má několik domén, pak rozhodnutí o nasazení globální katalog virtuální síti se systémem může ovlivnit dostupné šířky pásma výkonu, ověřování, vyhledávání v adresáři a tak dál.

| Oblast technologii služby Active Directory pro Windows Server | Rozhodnutí | Faktory |
| ---- | ---- | ---- |
| [Topologie sítě](#BKMK_NetworkTopology) | Vytvořit virtuální sítě? | <li>Požadavky pro přístup k Corp zdroje</li> <li>Ověřování</li> <li>Správa účtů</li> |
| [Konfigurace Datacentrum nasazení](#BKMK_DeploymentConfig) | <li>Nasazení strukturu samostatné bez vztahy?</li> <li>Nasazení strukturu nový federace?</li> <li>Nasazení nový struktuře s doménové důvěryhodnosti služby Active Directory pro Windows Server nebo Kerberos?</li> <li>Rozšíření Corp doménové nasazením otevřené Datacentrum?</li> <li>Rozšíření Corp doménové nasazením nové podřízené domény nebo domény strom?</li> | <li>Zabezpečení</li> <li>Dodržování předpisů</li> <li>Pole náklady</li> <li>Odolnost proti chybám a odolnost proti chybám</li> <li>Použití funkce pro kompatibilitu</li> |
| [Topologie služby Active Directory pro Windows Server](#BKMK_ADSiteTopology) | Jakým způsobem můžete nakonfigurovat podsítí, weby a odkazy webu s Azure virtuální sítě pro přenos a minimalizuje náklady? | <li>Definice podsítě a webů</li> <li>Odkaz vlastností webu a upozornění na změnu</li> <li>Replikace komprese</li> |
| [IP adresy a DNS](#BKMK_IPAddressDNS) | Jak nakonfigurovat a překlad IP adresy? | <li>Přiřadit statickou IP adresu získáte pomocí rutiny použití AzureStaticVNetIP nastavení</li> <li>Instalace serveru Windows Server DNS a konfigurace vlastností virtuální sítě s názvem a IP adresu OM, který je hostitelem role serveru řadiče domény a DNS</li> |
| [Distribuované GEO řadiče domény](#BKMK_DistributedDCs) | Jak se dá replikovat do řadiče domény na jednotlivé virtuální sítě? | Pokud topologii webu služby Active Directory vyžaduje v zeměpisných, které odpovídá různé regiony a Azure, než chcete vytvářet weby služby Active Directory příslušným způsobem sekundách. [Konfigurace virtuální sítě pro virtuální síťové připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) mezi řadiče domény na jednotlivé virtuální sítě. |
| [Řadiče domény jen pro čtení](#BKMK_RODC) | Použití jen pro čtení a zápis řadiče domény? | <li>Atributy HBI/Umožňujících filtru</li> <li>Filtrování tajemství</li> <li>Odchozí přenosy limit</li> |
| [Globální katalog](#BKMK_GC) | Nainstalovat globální katalog? | <li>Jednou doménou doménové proveďte všechny globálních řadiče domény</li> <li>Doménová struktura globálních jsou potřeba pro ověřování</li> |
| [Způsob instalace](#BKMK_InstallMethod) | Jak nainstalovat Datacentrum v Azure? | Buď: <li>Instalace služby AD DS pomocí prostředí Windows PowerShell nebo Dcpromo</li> <li>Přesunutí virtuální pevný disk virtuální Datacentrum místní</li> |
| [Umístění databáze systému Windows Server AD DS a SYSVOL](#BKMK_PlaceDB) | Umístění pro uložení systému Windows Server AD DS databáze protokoly a SYSVOL? | Změna Dcpromo.exe výchozí hodnoty. Tyto kritické služby Active Directory soubory *musí* být umístěny na discích dat Azure místo disků operační systém, které implementace ukládání do mezipaměti. |
| [Zálohování a obnovení](#BKMK_BUR) | Jak se chránit a obnovit data? | Vytvoření zálohy stavu systému |
| [Konfigurace federace serveru](#BKMK_FedSrvConfig) | <li>Nasazení strukturu nový federace v cloudu?</li> <li>Služba AD FS místního nasazení a vystavit proxy serveru v cloudu?</li> | <li>Zabezpečení</li> <li>Dodržování předpisů</li> <li>Pole náklady</li> <li>Přístup k aplikacím firmy partnery</li> |
| [Konfigurace služby cloudu](#BKMK_CloudSvcConfig) | Do cloudové služby je implicitně nasazené při prvním vytvoření virtuálního počítače. Potřebujete další cloud services nasazovat? | <li>Virtuálního počítače nebo VMs vyžaduje přímé vystavení k Internetu?</li> <li> Vyžaduje službu Vyrovnávání zatížení?</li> |
| [Federace serveru požadavky na veřejných a privátních IP adres (dynamické IP porovnání virtuální IP)](#BKMK_FedReqVIPDIP) | <li>Instanci systému Windows Server AD FS musím přejít přímo z Internetu?</li> <li>Vyžaduje aplikací nasazena v cloudu vlastní internetového IP adresa a port?</li> | Vytvoření jednoho cloudové služby pro každý virtuální IP adresy, kterou je potřeba nasazení |
| [Konfigurace systému Windows Server AD FS dostupnost](#BKMK_ADFSHighAvail) | <li>Kolik uzlů v serverové farmy Moje systému Windows Server AD FS?</li> <li>Kolik uzlů nasazovat ve farmy proxy serveru Windows Server AD FS?</li> | Odolnost proti chybám a odolnost proti chybám |

### <a name="BKMK_NetworkTopology"></a>Topologie sítě

Za účelem splnění IP adresu konzistenci a DNS požadavky na systém Windows Server služby AD DS, je nutné nejprve vytvořit [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) a připojte k němu virtuálních počítačích. Během jejího vytvoření musí rozhodnout, jestli volitelně rozšíření připojení k podnikové síti místní, transparentně připojený Azure virtuálních počítačích do místního počítače – to dosahuje tradiční technologie VPN a vyžaduje, aby koncový bod VPN zveřejněn na okraji podnikové sítě. To znamená virtuální privátní síť zahájená z Azure k podnikové síti, nikoli naopak.

Všimněte si, že další poplatky při rozšiřování virtuální sítě k místní síti za standardní poplatky, které platí pro každou OM. Konkrétně mívají poplatky dobu procesoru brány Azure virtuální síť pro přenos výstupní generovaného při každé OM komunikující s místním počítači prostřednictvím sítě VPN. Další informace o nákladech sítě přenosy najdete v článku [Azure ceny na přehled](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Konfigurace Datacentrum nasazení

Způsob konfigurace řadiče domény závisí na požadavky pro službu, že kterou chcete spustit na Azure. Například nasazené novou strukturu samostatný vlastní podnikové struktuře testování ověření koncepce, novou aplikaci nebo některé další krátkodobá projektu, který vyžaduje adresářovými službami, ale nejsou specifické přístupu k interním podnikové prostředky.

Jako výhody izolace struktury, které Datacentrum není replikovat s místním řadiče domény, výsledná méně odchozí provozu v síti vygeneruje se sebe sama přímo snížení nákladů. Další informace o nákladech sítě přenosy najdete v článku [Azure ceny na přehled](http://azure.microsoft.com/pricing/).

Jiný příklad předpokládejme, že máte požadavky ochrany osobních údajů pro službu, ale služba závisí na přístup k vaší vnitřní systému Windows Server služby Active Directory. Pokud jsou povoleny k datům hostitele službě v cloudu, nasazené nové podřízené domény pro interní domény na Azure. V tomto případě nástroje můžete nasazovat Datacentrum nové podřízené domény (bez globální katalog za účelem pomoci adresu soukromí). Tento scénář spolu s otevřené Datacentrum nasazení vyžaduje virtuální sítě pro připojení protokolem řadiče domény místního.

Pokud vytvoříte nové struktuře, zvolte, zda chcete použijte [vztahy důvěryhodnosti služby Active Directory](https://technet.microsoft.com/library/cc771397) [Federation důvěryhodnosti](https://technet.microsoft.com/library/dd807036). Zůstatek požadavky dáno kompatibility zabezpečení, dodržování předpisů, náklady a odolnost proti chybám. Například umožní využít výhod [Výběrové ověřování](https://technet.microsoft.com/library/cc755844) můžete nasadit strukturu nový na Azure a vytvoření služby Active Directory pro Windows Server vztah důvěryhodnosti mezi místní struktury a struktuře cloudu. Pokud aplikaci používající deklarace, však může nasadíte místo vztahy důvěryhodnosti služby Active Directory federation vztahy důvěryhodnosti služby. Dalším faktorem budou náklady replikovat víc dat prodloužením svého místního systému Windows Server služby Active Directory do cloudu nebo generovat více odchozí přenosy důsledku ověřování a načítání dotazu.

Požadavky na dostupnost a odolnost proti chybám mít vliv na svojí volbě. Například pokud přerušení propojení aplikace využívání Kerberos zabezpečení nebo federačním důvěřovat, jsou všechny pravděpodobně úplně porušený Pokud nasadili dostatečné infrastruktury na Azure. Konfigurace alternativní nasazení například otevřené řadiče domény (zapisovatelný nebo řadiče) zvýšit pravděpodobnost moci tolerovat výpadků odkaz.

### <a name="BKMK_ADSiteTopology"></a>Topologie služby Active Directory pro Windows Server

Potřebujete definovat správně webů a odkazy webu pro přenos a minimalizuje náklady. Weby, odkazy webu a podsítí ovlivnit topologii replikace mezi řadiče domény a tok ověřování přenosy. Zvážit následující poplatky přenosy nasazení a konfigurace řadiče domény podle požadavky nefunguje nasazení:

- Existuje nominal poplatku za hodinu brány samotné:

 - Můžete spustit a zastavit podle potřeby

 - Zastavení Azure VMs jsou izolovaný od podnikovou síť

- Příchozích je zadarmo

- Odchozí přenosy bude účtováno podle předpisů rozhraní [Azure ceny na přehled](http://azure.microsoft.com/pricing/). Vlastnosti webů propojení mezi místním weby a weby cloudu můžete optimalizovat následujícím způsobem:

 - Pokud používáte více virtuální sítí, nakonfigurujte odkazy webu a jejich nákladů řádně podporovat zabráníte systému Windows Server AD DS priority Azure webu na jeden poskytující stejné úrovně služby zdarma. Zvažte taky zakázáním mostu všech webů možnost propojení (BASL) (což je standardně zapnutá). Zajistíte tím, že jenom přímo – propojené weby replikovat k sobě navzájem. V přechodně připojené weby sekundách můžou už replikovat mezi sebou, ale musí replikovat prostřednictvím běžné webu nebo weby. Pokud zprostředkovatel weby nedostupné z nějakého důvodu replikace mezi řadiče domény přechodně připojené weby nedojde i v případě, že je k dispozici připojení mezi weby. Nakonec pokud oddíly chování přenosné replikace zůstávají žádoucí, vytvořte web odkaz mosty, které obsahují odpovídající odkazy na weby a weby, například místním podnikové síti weby.

 - [Konfigurace webů odkaz náklady](https://technet.microsoft.com/library/cc794882) řádně podporovat Chcete-li předejít před nechtěným přenosy. Například pokud **Vyzkoušejte další lokalitě nejbližší** umístěná, zkontrolujte, jestli že weby virtuální sítě nejsou další nejblíže zvýšením náklady spojené objektu odkaz webů, která bude propojena Azure webu zpět k podnikové síti.

 - Konfigurace webů odkaz [intervalů](https://technet.microsoft.com/library/cc794878) a [plány](https://technet.microsoft.com/library/cc816906) podle požadavky konzistence a sazby objekt změny. Plán replikace zarovnáte latence odchylka. Řadiče domény replikovat poslední stav pouze určité hodnotě, takže zmenšení interval replikace můžete ukládat náklady, pokud je objekt dostatečné změnit rychlost.

- -Li minimalizace náklady prioritu, zajistit, aby je naplánovaný replikace a upozornění na změnu není povolená. Při je to výchozí konfigurace replikace mezi weby. Toto není důležité: Pokud nasazujete řadiče virtuální síti se systémem, protože ten nebude replikovat odchozí změny. Ale pokud nasazení zapisovatelný Datacentrum by měl zkontrolujte, jestli že odkaz webů není nakonfigurován pro replikovat nepotřebných frekvenci aktualizací. Pokud nasazení serveru globální katalog (° c), ujistěte se, že každý web, který obsahuje globální Katalog zreplikuje domény oddíly ze zdroje řadiče domény na webu, který je spojený s odkaz webů nebo web – odkazy, které mají nižší náklady, než ° c Azure webu.


- Je možné a pořád dál tak snížit generovaných replikace mezi weby změnou algoritmus komprese replikace v síti. Kompresní algoritmus řídí algoritmu REG_DWORD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator položka registru komprese. Výchozí hodnota je 3, která odpovídá algoritmu Xpress komprimovat. Můžete změnit hodnotu 2, kterým se změní algoritmu MSZip. Ve většině případů zvýší to komprese, ale stejně tak na využití procesoru. Další informace najdete v tématu [jak Active Directory replikace topologie funguje](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP adresy a DNS

Azure virtuálních počítačích přiřazených "pevnou DHCP adresy" ve výchozím nastavení. Protože dynamické adresy Azure virtuální sítě potrvají s virtuálního počítače dobu trvání virtuálního počítače, Windows Server AD DS požadavků.

Jako výsledek při použití dynamického adresu v Azure jste platná pomocí statické IP adresy, protože je směrovatelné dobu zapůjčení a období zapůjčení je rovno životnost cloudové služby.

Dynamická adresa je však uvolnit OM při vypnutí. Zabránit právě uvolnit IP adresu, můžete [použít nastavení AzureStaticVNetIP přiřazení statické IP adresy](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Pro překlad, nasadit vlastní (nebo využít stávající) infrastruktura DNS server; Pokud Azure DNS není potřebám Upřesnit název rozlišení systému Windows Server AD DS. Například ho není podporují dynamické záznamy SRV a podobně. Překlad je položka kritické konfigurace doméně klientů a řadiče domény. Řadiče domény musí být může registrace záznamy o prostředcích a řešení jiných Datacentrum zdroj záznamů.
Důvodů odolnost proti chybám a výkonu je optimální nainstalovat službu systému Windows Server DNS řadiče domény spuštěna Azure. Nakonfigurujte vlastnosti Azure virtuální sítě se název a IP adresu serveru DNS. Při spuštění jiné VMs virtuální síti se systémem, jejich nastavení klienta překládání DNS nakonfigurována DNS server v rámci dynamické přidělování IP adres.

> [AZURE.NOTE] V místním počítači nemůžete se připojit k systému Windows Server AD DS Active Directory doméně, která je hostitelem Azure přímo přes Internet. Požadavky na port pro služby Active Directory a operace připojení k doméně vykreslení nepraktické přímo vystavit potřebné porty a platná, celý Datacentrum k Internetu.

VMs registrace názvu DNS automaticky při spuštění nebo v případě změny názvu.

Další informace o tomto příkladu je a jiný příklad, který ukazuje, jak zřídit první OM a nainstalovat služby AD DS najdete v tématu [instalace nové doménové služby Active Directory na Microsoft Azure](active-directory-new-forest-virtual-machine.md). Další informace o používání Windows Powershellu najdete v tématu [Instalace prostředí PowerShell Azure](../powershell-install-configure.md) a [Rutiny pro správu Azure](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Distribuované GEO řadiče domény

Azure nabízí výhody hostování více řadiče domény na různých virtuální sítě:

- Odolnost proti chybám více webů

- Pole fyzicky blízkosti pobočky (nižší latence)

Informace o konfiguraci přímé komunikaci mezi virtuálních sítí najdete v článku [Konfigurace virtuální sítě pro připojení k síti virtuální](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Řadiče domény jen pro čtení

Potřebujete rozhodnutí o nasazení jen pro čtení a zápis řadiče domény. Může být sklon nasazení řadiče nebudete mít fyzickou kontrolu nad nimi, protože řadiče vám mají být nasazené na místa, kde je jejich fyzické zabezpečení u rizika, například pobočky.

Azure nezobrazují fyzické rizika zabezpečení pobočkové, ale řadiče mohou pořád ukázat být nákladů efektivnější, protože funkce, které poskytují jsou vhodné k těmto prostředím i když hodně různých důvodů. Například řadiče máte odchozí replikace a budou moct selektivně naplnění tajemství (heslo). Nevýhodou chybějící tyto tajemství může vyžadovat odchozí přenosy na vyžádání ověřuje jako uživatel, nebo počítač ověří. Ale tajemství můžete selektivním který je předběžně a v mezipaměti.

Řadiče poskytují další výhody v a kolem HBI a Umožňujících pochybnosti vzhledem k tomu můžete přidat atributy, které obsahují citlivá data na ten filtrováno sady atributů (FAS). FAS momentový přizpůsobitelné atributy, které nejsou replikovat na řadiče. V případě nejsou povoleny nebo nechcete ukládat Umožňujících nebo HBI na Azure, můžete použít FAS jako ochranu. Další informace najdete v tématu [filtrované atribut jen pro čtení nastavit [(https://technet.microsoft.com/library/cc753459)].

Ujistěte se, že aplikace budou kompatibilní s řadiče budete používat. Řada aplikací Windows Server Active Directory povolena dobře fungují s řadiče, ale některé aplikace můžete provádět neefektivnímu nebo nezdaří v případě, že nemají přístup k zapisovatelný Datacentrum. Další informace najdete v [příručce kompatibility aplikace řadiče domény jen pro čtení](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globální katalog

Potřebujete rozhodnutí o instalaci globální katalog. Ve struktuře jednu doménu byste měli označit všechny řadiče domény jako globální katalog servery. Protože budou žádné další replikace přenosy se nebude prodloužit náklady.

V rámci struktuře globálních jsou nutné k rozbalení členství ve skupinách univerzální během procesu ověření. Pokud není nasadit globální Katalog, pracovního vytížení virtuální síti se systémem, které ověřují proti Datacentrum v Azure nepřímo generovat provoz ověřování odchozích připojení k vytvoření dotazu globálních místní při pokusu o každé ověřování.

Náklady spojené s globálních jsou předvídatelná menší, protože hostovat každá doména (v části). Pokud pracovní zátěž hostitelem internetové služby a ověřuje uživatele oproti systému Windows Server AD DS, může být úplně neočekávané náklady. Pokud chcete snížit ° c dotazů webu cloudu při ověřování, můžete [Povolit univerzální skupina členství ukládání do mezipaměti](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Způsob instalace

Je potřeba zvolit způsob instalace budou řadiče domény na virtuální sítě:

- Zvýšení úrovně nové řadiče domény. Další informace najdete v tématu [Instalace nových doménové služby Active Directory Azure virtuální síti se systémem](active-directory-new-forest-virtual-machine.md).

- Virtuální pevný disk virtuální Datacentrum místní přesunutí do cloudu. V tomto případě musí zajistit, že virtuální Datacentrum místní "přesunutí" není "zkopírovaný" nebo "klonovaný."

Vyberte pouze Azure virtuálních počítačích řadiče domény (na rozdíl od Azure "www" nebo "pracovníka" role VMs). Jsou trvalé a životnost stavem Datacentrum je nutný. Azure virtuálních počítačích jsou sice pro úloh třeba řadiče domény.

Nepoužívejte SYSPREP pro nasazení nebo klonovat řadiče domény. Možnost klonovat řadiče domény je pouze k dispozici začátek v systému Windows Server 2012. Funkce klonování vyžaduje podporu pro VMGenerationID v podkladovém hypervisoru. V systému Windows Server 2012 a Azure virtuální sítě pro Hyper-V obou podporují VMGenerationID, dodavatelé softwaru virtualizace třetích stran.

### <a name="BKMK_PlaceDB"></a>Umístění databáze systému Windows Server AD DS a SYSVOL

Vyberte umístění databáze systému Windows Server AD DS, protokoly a SYSVOL. Musí být nasazené na dat Azure discích.

> [AZURE.NOTE] Azure disků dat jsou omezen na 1 TB.

Data diskových jednotek to není mezipaměti zápisy ve výchozím nastavení. Diskových jednotek dat, které jsou připojené k virtuálního počítače pomocí zápisu prostřednictvím mezipaměti. Zápis prostřednictvím mezipaměti vytvoří se velmi dbá na trvalé Azure úložiště zápisu, před dokončením transakce z pohledu OM operační systém. Poskytuje životnost, za cenu mírně pomaleji zápisy.

To je důležitá pro Windows Server AD DS, protože diskové mezipaměti zápis zruší platnost předpoklady řadiče domény. Windows Server AD DS pokusí zakázat ukládání do mezipaměti, ale je disk vstupu a výstupu systém přijmout ji. Chyba při vypnutí ukládání do mezipaměti, za určitých okolností zavést USN vrácení výsledkem lingering objekty a jiné potíže.

Osvědčený pro virtuální řadiče domény postupujte takto:

- Nastavení předvoleb mezipaměti hostitele na disku Azure dat pro žádný. Tím problémy s ukládání do mezipaměti pro operace služby AD DS.

- Uložení databáze, protokoly a SYSVOL na některý z stejná data u samostatný datový disků. Obvykle je to samostatný disk z disku pro samotný operační systém. Klíčové takeaway je databáze systému Windows Server AD DS a SYSVOL nesmí být uloženy na disku typu Azure operační systém. Ve výchozím nastavení nainstaluje proces instalace služby AD DS tyto součásti z % kořenové % složky, které se nedoporučuje Azure.

### <a name="BKMK_BUR"></a>Zálohování a obnovení

Uvědomte si, co se nepodporuje pro zálohování a obnovení Datacentrum obecně a konkrétně těm, kteří používají v virtuálního počítače. V tématu [zálohování a obnovení pro virtualizovaném řadiče domény](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Vytvoření zálohy stavu systému pomocí pouze záložní software, který je konkrétně vědomi záložní požadavků pro Windows Server AD DS, například zálohování serveru.

Zkopírujte nebo klonovat virtuální pevný disk soubory řadiče domény místo provádění pravidelného zálohování. Obnovení někdy třeba vyžadovat, provedete pomocí klonovaný nebo zkopírovány VHD bez systému Windows Server 2012 a podporované hypervisor zavede USN bubliny.

### <a name="BKMK_FedSrvConfig"></a>Konfigurace federace serveru

Konfigurace systému Windows Server AD FS federační servery (STSs) v části závisí na tom, jestli potřebujete přístup k prostředkům ve vaší místní síti aplikace, které chcete nasadit na Azure.

Pokud aplikace splňovat následující kritéria, může nasazení aplikace v izolace z místní síti.

- Přijmutí tokenů zabezpečení SAML

- Jsou exposable k Internetu

- Není přístupu ke zdrojům, místní

V tomto případě konfigurace systému Windows Server AD FS STSs následujícím způsobem:

1. Konfigurace izolace doménové jednou doménou v Azure.

2. Zpřístupnit federované struktuře pomocí konfigurace serverové farmy federace systému Windows Server AD FS.

3. Konfigurace systému Windows Server AD FS (federace serverové farmy a farmy proxy federačních serverů) ve struktuře místní.

4. Nastavit federaci vztah důvěryhodnosti mezi místním prostředím a Azure instance systému Windows Server AD FS.

Na druhé straně Pokud aplikace vyžadují přístup k místním zdrojům, může nakonfigurujete službu systému Windows Server AD FS s aplikací na Azure následujícím způsobem:

1. Konfigurace připojení mezi místní sítě a Azure.

2. Konfigurace serverové farmy federace systému Windows Server AD FS ve struktuře místní.

3. Konfigurace farma proxy federačních serverů systému Windows Server AD FS v Azure.

Konfigurace má tu výhodu snížení působení místních zdrojů, podobně jako konfigurace systému Windows Server AD FS s aplikací v síti obvod.

Všimněte si, že v některém z firem relace můžete nastavit vztahy důvěryhodnosti služby s více Zprostředkovatelé identit jiní v případě potřeby firmy obchodní spolupráce.

### <a name="BKMK_CloudSvcConfig"></a>Konfigurace služby cloudu

Cloud services jsou potřebná, pokud chcete vystavit OM přímo k Internetu nebo vystavit internetového Vyrovnávání zatížení aplikace. Je to možné, protože každý cloudové služby nabízí jednoho konfigurovatelné virtuální IP adresu.

### <a name="BKMK_FedReqVIPDIP"></a>Federace serveru požadavky na veřejných a privátních IP adres (dynamické IP porovnání virtuální IP)

Každý Azure virtuální počítač obdrží dynamické IP adresu. Dynamické IP adresu je dostupné jenom v Azure soukromé adresa. Ve většině případů ale bude nutné nakonfigurovat virtuální IP adresu nasazení systému Windows Server AD FS. Virtuální IP je nutné vystavit koncové body systému Windows Server AD FS k Internetu a použijí se federované partnery a klienti ověřování a průběžné správě. Virtuální IP adresu je vlastnost do cloudové služby, který obsahuje jeden nebo více Azure virtuálních počítačích. Pokud aplikaci používající deklarace nasazený na Azure a Windows Server AD FS internetového a sdílet běžné porty, každý budou vyžadovat virtuální IP adresu vlastní a proto bude nutné vytvořit jednu cloudové služby pro aplikaci a druhý pro Windows Server AD FS.

Definice podmínek virtuální IP adresa a dynamické IP adresu, najdete v tématu [pojmy a definice](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Konfigurace systému Windows Server AD FS dostupnost

Když je možné nasadit samostatného systému Windows Server AD FS federation services, doporučujeme nasaďte farmě s nejméně dvěma uzly AD FS STS a proxy servery pro provozním prostředí.

V tématu [služby AD FS 2.0 nasazení topologie aspektech](https://technet.microsoft.com/library/gg982489) v [AD FS 2.0 návrh průvodce](https://technet.microsoft.com/library/dd807036) rozhodnout, jaký konfigurace možnostech nejlepší svým potřebám konkrétní.

> [AZURE.NOTE] Abyste mohli získávat Vyrovnávání zatížení pro Windows Server AD FS koncové body Azure, nakonfigurujte všem členům farmy systému Windows Server AD FS ve stejném cloudové služby a pomocí možností Vyrovnávání zatížení Azure http (výchozí nastavení 80) a HTTPS porty (výchozí nastavení 443). Další informace najdete v tématu [Azure Vyrovnávání zatížení zkušební](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server (vyrovnávání sítě) není podporována ve Azure.
