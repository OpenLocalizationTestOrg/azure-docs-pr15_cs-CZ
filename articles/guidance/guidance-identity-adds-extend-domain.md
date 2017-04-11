<properties
   pageTitle="Odkaz Azure architektura - IaaS: Rozšíření služby Active Directory Azure | Microsoft Azure"
   description="Jak implementovat zabezpečené hybridní architektura sítě s povolení služby Active Directory v Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Rozšíření adresářové služby Active Directory (přidá) na Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro rozšíření prostředí Active Directory (AD) Azure k poskytování služby distribuované ověřování pomocí [Active Directory Domain Services (AD DS)][active-directory-domain-services]. Tato architektura slouží k rozšíření, které popsané v článcích [Implementace zabezpečeného hybridní architektura sítě v Azure] [ implementing-a-secure-hybrid-network-architecture] a [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení.

Pomocí služby AD DS ověření identity. Tyto identity náleží uživatelům, počítačů, aplikací nebo jiných zdrojů, které jsou součástí domény zabezpečení. Můžete hostovat služby AD DS místní, ale ve scénáři hybridní umístění prvky aplikace v Azure může být efektivnější replikovat prvek pro tuhle funkci a úložišti AD do cloudu. Tento přístup snížit zpoždění způsobená odesílání ověřování a požadavky na místní ověření z cloudu zpět do služby AD DS místní systém. 

K hostování vašeho adresáře služby v Azure dvěma způsoby:

1. Můžete použít [Azure Active Directory (AAD)] [ azure-active-directory] v cloudu vytvořit novou doménu AD a ho propojit do místního AD domény. Nastavení [Azure AD Connect] [ azure-ad-connect] na sliby replikovat identity v místní úložiště do cloudu. Všimněte si, že je v adresáři v cloudu **není** rozšíření místních systému, raději je kopii, která obsahuje stejné identit. Změny provedené v těchto identit, které místní budou zkopírovány do cloudu, ale změny provedené v cloudu, **nebude** se replikovat zpět na místní domény. Resetování hesla například musí být provedena místních a použít Azure AD Connect pro změnu do cloudu. Všimněte si také, že stejné instanci aplikace AAD jde propojit s více než jedna instance služby AD DS; AAD bude obsahovat identit každý AD úložiště pro které je propojený.

    AAD je užitečné pro situace, kde v místní síti a Azure virtuální sítě hostingu cloudové zdroje nebyly propojené přímo pomocí tunelem virtuální privátní sítě nebo ExpressRoute okruh. I když toto řešení je jednoduché, nemusí být vhodné pro systémy kde součásti mohli migrovat přes hranice na místní/cloudu jako to může vyžadovat změnu konfigurace AAD. AAD pouze zpracuje také ověřování uživatelů spíše než ověřování počítače. Některé aplikace a služby, jako je SQL Server, může vyžadovat ověřování počítače v tomto případě není vhodné toto řešení.

2. Můžete nasadit OM spuštěný služby AD DS jako řadiče domény v Azure, rozšíření stávající infrastrukturu AD z místního datacentra. Tento přístup je další běžné scénáře, které v místní síti a Azure virtuální sítě se připojují připojení VPN a/nebo ExpressRoute. Tato řešení taky podporuje obousměrné replikace umožňuje provádět změny v cloudu a místní, ať je nejvhodnější. V závislosti na vašim požadavkům na zabezpečení může být instalace AD v cloudu:
    - část tu samou doménu jako této uloženými místní
    - novou doménu sdílené doménové
    - strukturu samostatný

<!-- we might want to add info on how to choose from the three options above -->

Tato architektura se zaměřuje na řešení 2, pomocí tu samou doménu služby AD DS v Azure a místních.

Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace kde úloh spustit částečně místní a částečně v Azure.

- Aplikace a služby, které ověřování pomocí služby Active Directory.

## <a name="architecture-diagram"></a>Diagram architektury

Následující diagram zvýrazní důležitá součástí tato architektura (*klikněte na zvětšit*). Další informace o prvcích šedě, přečtěte si [Implementace zabezpečeného hybridní architektura sítě v Azure] [ implementing-a-secure-hybrid-network-architecture] a [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **V místní síti.** V místní síti obsahuje místní servery AD, které mohou provádět a tak mohli ověřovat součásti nachází místní.

- **Servery AD.** Toto jsou řadiče domény provádění directory services (AD DS) spuštěný jako VMs v cloudu. Tyto servery můžete přihlašovacích údajů součásti spuštěné v Azure virtuální sítě.

- **Active Directory podsítě.** Servery služby AD DS jsou umístěny v samostatné podsítě. Pravidla NSG chránit servery služby AD DS a může poskytovat bránu firewall proti z neočekávané zdrojů.

- **Synchronizaci azure brány a AD.**. Azure brány obsahuje spojení mezi místní síti a Azure VNet. Může to být [připojení VPN] [ azure-vpn-gateway] nebo [Azure ExpressRoute][azure-expressroute]. Všechny žádosti o synchronizaci mezi servery AD v cloudu a místní projít brány. Uživatelem definované směruje (UDRs) úchyt pro směrování přenosů synchronizace, které přímo předá server AD v cloudu a ne projít existující síť virtuální spotřebiče (NVAs) používané v tomto scénáři

    Další informace o konfiguraci UDRs a NVAs naleznete v tématu [Implementace zabezpečeného hybridní architektura sítě v Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Doporučení

Tato část obsahuje souhrn doporučení pro implementaci služby AD DS v Azure, překrývajícím:

- Doporučení OM.

- Doporučení sítě.

- Doporučení týkající se zabezpečení. 

- Active Directory webu doporučení.

- Active Directory FSMO umístění doporučení.

>[AZURE.NOTE] Dokument [pokyny pro nasazení systému Windows služby Active Directory Server na virtuálních počítačích Azure] [ ad-azure-guidelines] obsahuje podrobnější informace o mnoha těchto bodů.

### <a name="vm-recommendations"></a>Doporučení OM

Vytvoření VMs s dostatečné prostředky pro zpracování očekávané objem provozu. Použijte velikost počítačích hostitelské služby AD DS místně jako výchozí bod. Sledování využití prostředků; můžete změnit velikost VMs a měřítko, jako jsou příliš velká. Další informace o pro změnu velikosti řadiče domény služby AD DS najdete v tématu [Plánování kapacity Active Directory Domain Services][capacity-planning-for-adds].

Vytvořte disk samostatné virtuální dat pro ukládání protokolů, databáze a SYSVOL pro AD. Neukládat tyto položky na stejném disku jako operačního systému. Poznámka: ve výchozím nastavení disků dat, které jsou připojené k virtuálního počítače použít zápisu prostřednictvím mezipaměti. Tento způsob ukládání do mezipaměti můžete však konfliktu s požadavky na služby AD DS. Z tohoto důvodu nastavení *Předvoleb mezipaměti hostitele* na disku dat na *žádný*. Další informace najdete v tématu [umístění databáze systému Windows Server AD DS a SYSVOL][adds-data-disks].

Nasazení aspoň dva VMs spuštění služby AD DS jako řadiče domény k síti Azure virtuální [dostupnost](#Availability-considerations) důvodů.

### <a name="networking-recommendations"></a>Doporučení sítě

Konfigurace rozhraní sítě pro každou VMs hostitelské služby AD DS pomocí statické IP adresy soukromé. Konfigurace lépe podporuje DNS značí AD VMs. Další informace najdete v tématu [jak nastavit statická soukromé IP adresu na portálu Azure][set-a-static-ip-address].

> [AZURE.NOTE] Nedávají AD DS VMs veřejných IP adres. V tématu [otázky bezpečnosti pro] [ security-considerations] další podrobnosti.

Musíte se ujistit, že můžete mezi servery AD v cloudu a místní volně plovoucí přenosy:

- Přidání pravidel NSG podsítě AD, která povolit příchozích z místního. Podrobné informace o porty, které využívají služby AD DS najdete v tématu [služby Active Directory a Active Directory Domain Services požadavky na Port][ad-ds-ports].

- Zkontrolujte, že UDR tabulkami není směrovat přenosy v síti služby AD DS prostřednictvím NVAs používané v tomto scénáři. Vlastní nasazení, podle toho, jaké NVAs používáte je vhodné přesměrovat která vysílání.

### <a name="security-recommendations"></a>Doporučení týkající se zabezpečení

Servery AD DS zpracovat ověřování a proto jsou velmi citlivé položky. Můžete zabránit přímé působení servery služby AD DS k Internetu. Umístěte servery služby AD DS samostatné podsítě s vlastním bránu firewall. Zajistit, aby zůstat otevřené porty potřebné pomocí služby AD DS a tak mohli ověřovat a odstranění synchronzing servery, ale zavřete všechny ostatní porty. Další informace najdete v tématu [služby Active Directory a Active Directory Domain Services požadavky na Port][ad-ds-ports]. [Součásti řešení] [ solution-components] uvedenou dál v tomto dokumentu se zobrazí NSG pravidla používané ukázkové řešení otevřete porty.

Pravidla NSG slouží k vytvoření jednoduchého bránu firewall. Pokud požadujete komplexnější ochranu využijete větší zabezpečení obvod kolem servery pomocí pár prvků podsítí a NVAs, podle popisu v dokumentu [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

Disk, na který je hostitelem databáze služby AD DS šifrování databáze pomocí nástroje BitLocker nebo Azure šifrování disku.

### <a name="active-directory-site-recommendations"></a>Active Directory webu doporučení

Můžete používat weby ve službě AD DS skupiny společném domény řadiče připojených rychlé propojením. Řadiče domény na stejném webu služby AD DS automaticky replikovat své změny adresář a malé konfigurace je nutné zpracovat replikace.

URČIT replikace komunikaci mezi Azure a místních datacentru, doporučujeme přidat samostatných webů služby AD DS představující adresní prostor používaný Azure. Konfigurace odkazů webu mezi vaší místní služby AD DS weby a řídit webů replikace efektivněji.

Chcete-li použít jinou skupinu zásad objekty připojili počítačů v Azure a umožní využít výhod aplikace, které jsou "webu vědět", například Centrum Správce konfigurace pro System můžete oddělení webu.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO umístění doporučení

Flexibilní předlohy operace FSMO (Single) servery jsou specializované domény řadiče reposnsible pro konzistenci dat mezi různým nastavením ve službě AD DS uvedených pod ním.

- **Schémat**. Toto je doménové roli, která spravuje konstrukce schématu používané služby AD DS.

- **Hlavní server názvů domén**. Toto je doménové roli, která spravuje informace o názvech domén doménové.

- **Primární domény řadiče**domény. Jedná se o roli domény. Primární zpracovává aktualizace hesel a uzamčení účtu. Po projednání tak, že ostatní řadiče domény žádosti o služby ověřování obsahujících chybná hesla. Primární taky zpracovává aktualizace zásad skupiny a je cílový Datacentrum pro starší verze aplikace a některé nástroje pro správu, které provedení některých zapisovatelné operací.

- **Hlavní relativní ID (serveru relativních ID)**. RID slouží k identifikaci jedinečně objektů v rámci adresáře. Tento server je zodpovědný za vytvoření fondu domény relativních pro použití všemi servery AD v rámci domény.

- **Role infrastruktury**. Objekt v jedné domény můžete odkázat objektu v jiné doméně. Tato role domény je zodpovědný za aktualizace ID zabezpečení objektu a jeho rozlišující název objektu doménami odkazu. Všimněte si, že server implementaci této role nejde taky zajišťovat serveru globální katalog (° c).

Další informace najdete v tématu [Active Directory rolí FSMO v systému Windows][AD-FSMO-roles-in-windows].

Tento scénář doporučujeme že vyhnout se nasazení role FSMO řadiče domény v Azure. 

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Vytvoření dostupné pro servery AD. Ujistěte se, že jsou v sadě aspoň dva servery. Servery AD v cloudu by měl být v rámci stejné domény řadiče domény. To vám umožní automatické replikace mezi servery.

Zvažte taky, jestli označením servery jako [předlohy úsporném operace] [ standby-operations-masters] v případě, že připojení k serveru budou sloužit jako roli FSMO se nezdaří.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Je-li všechny úkoly správy domény mohou provést pomocí místní řadiče domény, zvažte řadiče domény v cloudu jen pro čtení. Jen pro čtení Datacentrum pouze udržuje podmnožinu pověření uživatelů (tak, abyste ověřování místně) a můžete nakonfigurovat tak, aby informace v mezipaměti pouze určitým uživatelům. Proto pokud je ohroženo Datacentrum jen pro čtení, jenom ty informace uložené v mezipaměti uživatelům se projeví, místo podrobnosti každé účtu v doméně. Další informace najdete v tématu [Řadiče domény jen pro čtení][read-only-dc].

Minimalizovat chybu jednotlivých uživatelských účtů a zabránit pokusy o break-in, postupujte podle doporučený postup pro nastavení a Správa hesla uživatelů ve službě Active Directory. Další informace najdete v tématu [Doporučené postupy pro vynucení zásady pro hesla][best_practices_ad_password_policy]. Navíc nezapomeňte skupiny, které se uživatelům přiřadit. Například abyste běžným uživatelům členové skupiny správců Enterprise, skupiny správců schématu a skupiny Domain Admins.

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

AD je automaticky přizpůsobit pro řadiče domény, které jsou součástí tu samou doménu. Žádosti o jsou rozvržena všechny řadiče domény. Přidání dalšího řadiče domény a bude automaticky synchronizovat s doménou. Konfigurovat při vyrovnávání zatížení samostatné směrování adres řadiče v rámci domény. Zajistit, aby všechny řadiče domény měli dostatečná paměti a úložiště zdroje zpracovávání domény databázi. v ideálním případě proveďte všechny řadiče domény VMs stejnou velikost.

## <a name="management-considerations"></a>Důležité informace

Nekopírujte soubory virtuální pevný disk řadiče domény místo provádění pravidelného zálohování, protože jejich obnovování můžete mít za následek nekonzistencím v stavu mezi řadiče domény.

Ukončete a restartujte OM, které se spouští roli řadiče domény v Azure v operačním systému hosta místo použití příkazu vypnout na portálu Azure. Pomocí portálu Azure vypnout virtuálního počítače způsobí, že OM být odebrána. Obnoví OM-GenerationID, což nežádoucí u řadiče domény. Po obnovení OM GenerationID invocationID úložišti AD také resetovat, vyřazené fondu serveru relativních ID a SYSVOL je označený jako neautoritativních.

## <a name="monitoring-considerations"></a>Sledování co byste měli zvážit

Nejsou-li sledování a údržby sítě se servery AD může způsobit problémy, jako:

- **Chyba při přihlášení**. Chyba při přihlášení může dojít v celém domény nebo doménové selže zabezpečení relace nebo název rozlišení, případně globální katalog nelze určit univerzální členství ve skupinách.

- **Uzamčení účtu**. Účty uživatelů a služby můžete uzamknou Pokud není k dispozici primárního řadiče domény nebo neúspěšné replikace mezi několika řadiče domény.

- **Chyba při řadiče domény**. Pokud jednotku, která obsahuje soubor Ntds.dit dostatek místa, můžete domény řadiče přestane fungovat.

- **Chyba aplikace**. Aplikace, které jsou důležité pro vaše podnikání, jako je Microsoft Exchange nebo jiného e-mailové aplikaci, může selhat, pokud dochází k selhání adresu knihy dotazů do adresáře.

- **Nekonzistentní adresáře data**. Pokud replikace selže delší dobu, duplicitní objekty lze vytvořit v adresáři a může vyžadovat rozsáhlou diagnostiku a čas na odstranit.

- **Vytvoření účtu se nezdařilo**. Řadiče domény nejde vytvořit účty uživatelů a počítačů vyčerpá jeho dodávky relativních ID a hlavní server relativních ID není k dispozici.

- **Chyba zásady zabezpečení**. Pokud není na sdílenou složku SYSVOL replikovat správně, objekty zásad skupiny a zásady zabezpečení nejsou použita správně klienty.

Důkladně sledujte servery AD a připravte se na akce korekční rychle. Vytvoření kontrolního seznamu sledování úkolů příslušný intervalech. Například může plánujete kritické úkoly podle těchto pokynů denně:

- Zkontrolujte a vyřešení upozornění vykázaného řadiče domény 

- Zkontrolujte, že všechny řadiče domény komunikovat replikace pracuje.

- Zajistit, aby zůstal SYSVOL sdílené.

- Vyřešení problémů se synchronizací čas.

- Vyhledání místa na disku na fyzických jednotek používaný AD.

Může provádět jiné běžné úlohy méně často. Může například prohlížení vztahy důvěryhodnosti a kontrolu zastaralým nebo zlomkové vztahy důvěryhodnosti týdenní a zkontrolujte, že všechny řadiče domény běží pomocí stejného service Pack a oprava opravy každý měsíc.

Další informace najdete v tématu [Sledování Active Directory][monitoring_ad]. Instalace nástrojů, jako jsou [Microsoft System Center] [ microsoft_systems_center] na monitorovací server (viz [diagram architektury][architecture]) můžete provést tyto úlohy. 

## <a name="solution-components"></a>Součásti řešení

Řešení podle tohoto architektura vytvoří síti zabezpečené hybridní podle popisu v dokumentech [Implementace zabezpečeného hybridní architektura sítě v Azure] [ implementing-a-secure-hybrid-network-architecture] a [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], ale doplněný následující položky:

- Skupina Azure zdroje s názvem dns *basename*-rg, kde je *basename* předponu určíte, když nasazením řešení.

- Dva Azure VMs s názvem *basename*-ad1-OM a *basename*-ad2-OM vytvořené ve skupině *basename*dns rg zdroje. Tyto VMs nakonfigurovaná jako servery AD s adresářovými službami a DNS nainstalovali a nakonfigurovali.

- NSG s názvem *basename*-ad-nsg ve skupině *basename*- ntwk rg Azure zdroje. Tato skupina zdroje je součástí infrastrukturu, která představují zabezpečené hybridní síť, ale nové NSG je doplněk, který definuje pravidla příchozí zabezpečení pro servery AD uvedeno v následující tabulce:


    Priority (priorita)|Jméno|Zdroje|Cíl|Služba|Akce|
    --------|----|------|-----------|-------|------|
    170|vnet port53|10.0.0.0/16|Všechny|Custom(ANY/53)|Povolit|
    180|vnet port88|10.0.0.0/16|Všechny|Custom(ANY/88)|Povolit|
    190|vnet port135|10.0.0.0/16|Všechny|Custom(ANY/135)|Povolit|
    200|vnet až port137 9|10.0.0.0/16|Všechny|Custom(ANY/137-139)|Povolit|
    210|vnet port389|10.0.0.0/16|Všechny|Custom(ANY/389)|Povolit|
    220|vnet port464|10.0.0.0/16|Všechny|Custom(ANY/464)|Povolit|
    230|vnet k protokolu rpc dynamické|10.0.0.0/16|Všechny|Custom(ANY/49152-65535)|Povolit|
    240|onprem ad k port53|192.168.0.0/24|Všechny|Custom(ANY/53)|Povolit|
    250|onprem ad k port88|192.168.0.0/24|Všechny|Custom(ANY/88)|Povolit|
    260|onprem ad k port135|192.168.0.0/24|Všechny|Custom(ANY/135)|Povolit|
    270 stupňů|onprem ad k port389|192.168.0.0/24|Všechny|Custom(ANY/389)|Povolit|
    280|onprem ad k port464|192.168.0.0/24|Všechny|Custom(ANY/464)|Povolit|
    290|Správa rdp povolit|10.0.0.128/25|Všechny|Custom(ANY/3389)|Povolit|
    300|Povolit brány|10.0.255.224/27|Všechny|Custom(ANY/any)|Povolit|
    310|Automatické povolení|10.0.255.192/27|Všechny|Custom(ANY/any)|Povolit|
    320|vnet-Odepřít|Všechny|Všechny|Custom(ANY/any)|Povolit|

    Služby AD DS používá porty 53 89, 135, 389 a 464 přijmout příchozí replikace a komunikace ověřování. V této tabulce řadiče domény místního probíhá 192.168.0.0/24 místo adresu (adresní prostor se může lišit - určete tyto informace jako parametr k šablonám nasazení řešení.

    NSG definuje také následující pravidla Výstupní zabezpečení, které povolení synchronizace a povolení přenosů na plovoucí dlaždice zpátky do místního sítě:

    Priority (priorita)|Jméno|Zdroje|Cíl|Služba|Akce|
    --------|----|------|-----------|-------|------|
    100|mimo port53|Všechny|192.168.0.0/24|Custom(ANY/53)|Povolit|
    110|mimo port88|Všechny|192.168.0.0/24|Custom(ANY/88)|Povolit|
    120|mimo port135|Všechny|192.168.0.0/24|Custom(ANY/135)|Povolit|
    130|mimo port389|Všechny|192.168.0.0/24|Custom(ANY/389)|Povolit|
    140|mimo port445|Všechny|192.168.0.0/24|Custom(ANY/445)|Povolit|
    150|out port464|Všechny|192.168.0.0/24|Custom(ANY/464)|Povolit|
    160|dynamické rpc Out|Všechny|192.168.0.0/24|Custom(ANY/49152-65535)|Povolit|

Skript, který se řešení také provádět následující úkoly:

- Přidá *basename*-ad1-OM a *basename*-ad2-OM servery jako domény řadiče domény. V konzole *Uživatelé služby Active Directory a počítačů* v místním doménovém můžete zobrazit těchto serverech:

![[1]][1]

- Vytvoří novou podsítě (10.0.0.0/16) pro kolekci webů pro AD Azure VNet Ad webů s názvem domény. Tento web obsahuje *basename*-ad1-OM a *basename*-ad2-OM servery. 

- Přidá IP přenosu mezi sítěmi nastaveními konfigurovat interval replikace mezi místních webů a řadiče domény v cloudu. Zobrazí se nastavení podsítě, webů a nastavení přenosu v konzole *sítě a Active Directory servery* v místním doménovém:

![[2]][2]

## <a name="deployment"></a>Nasazení

Ukázka řešení obsahuje následující prerequsites:

- Už jste nakonfigurovali místní domény a, že máte nakonfigurované DNS a nainstalovaný směrování a vzdáleného přístupu služby pro podporu VPN připojení přes Azure VPN brány.


- Jste nainstalovali nejnovější verzi Azure rozhraní příkazového řádku. [Postupujte podle těchto pokynů podrobnosti][cli-install].

- Pokud jste nasazení řešení z Windows, musíte nainstalovat nástroj, který obsahuje prostředí flám, třeba [GitHub plochu][github-desktop].

>[AZURE.NOTE] Pokud nemáte přístup do existující místní domény, můžete vytvořit testovacím prostředí pomocí [onpremdeploy.sh] [ onpremdeploy] flám skriptu. Tento skript vytvoří sítě a OM v cloudu, který napodobuje velmi jednoduché místního nastavení. Upravte tento skript před spuštěním a nastavte následující proměnné definované na začátku souboru:
>
> - **BASE_NAME**. Uživatelem definované předponu pro pole Skupina zdroje a OM vytvořené pomocí skriptu. Tato položka by měl být **delší než 5 znaků** jinak skript pokusí generovat OM s neplatným názvem a nezdaří.
> 
> - **PŘEDPLATNÉ**. ID Azure předplatného. V tomto suscription se vytvoří skupina zdroje.
> 
> - **Umístění**. Azure umístění, do kterého chcete vytvořit skupinu zdrojů, jako je *eastus* nebo *westus*.
> 
> - **ADMIN_USER_NAME**. Název pro účet správce v OM.
> 
> - **ADMIN_PASSWORD**. Heslo k účtu správce.

Proveďte následující kroky k vytvoření ukázkové řešení:

1. stažení a úpravy [azuredeploy.sh] [ azuredeploy] skriptu a nastavte následujících parametrů na začátku souboru:

    - **BASE_NAME**. Uživatelem definované předponu pro skupiny zdrojů a VMs vytvořené pomocí skriptu. Tato položka jako dříve by měl být **delší než 5 znaků**.

    - **PŘEDPLATNÉ**. ID Azure předplatného.

    - **Operační_systém**. Operační systém (*Windows* nebo *Linux*) a použijte pro web, business a dat přístup k VMs osy. Poznámka: všechny servery AD vytvořené pomocí skriptu bezchybnou systému Windows Server 2012, bez ohledu na toto nastavení.

    - **Název_domény**. Název místní domény. Všimněte si, že používáte vytvořil onpremdeploy.sh skriptu prostředí, můžete to musí být *contoso.com*.

    - **Umístění**. Azure umístění, do kterého chcete vytvořit skupiny zdrojů.

    - **ADMIN_USER_NAME**. Název použitý u účtů správce v různých VMs.

    - **ADMIN_PASSWORD**. Heslo k účtu správce.

    - **ON_PREMISES_PUBLIC_IP**. Veřejnou IP adresu počítače VPN místní.

    - **ON_PREMISES_ADDRESS_SPACE**. Vnitřní adresní prostor v místní síti. Pokud používáte prostředí vytvořili skriptem onpremdeploy.sh, musí být 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. Klíč sdílené IPSec použitý pro vytváření připojení VPN mezi místní síti a brána Azure VPN.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. IP adresy místního serveru DNS. Pokud používáte prostředí vytvořené pomocí skriptu onpremdeploy.sh, musí být 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Předpona adresy podsítě místní. Pokud používáte prostředí vytvořené pomocí skriptu onpremdeploy.sh, musí být 192.168.0.0/24.

    >[AZURE.NOTE] ULOŽENÍ zdrojů a čas, skript nevytvoří firmy či dat úrovní. Pokud požadujete tyto položky, můžete zrušte komentář v azuredeploy.sh skript k následující části:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Otevřete prostředí výzva flám a přejděte do složky obsahující skript azuredeploy.sh.

3. Přihlaste se k účtu Azure. V prostředí flám zadejte tento příkaz:

    ```
    azure login
    ```

    Postupujte podle pokynů pro připojení k Azure.

4. Spusťte příkaz `./azuredeploy.sh`a počkejte, než skript vytvoří síťovou infrastrukturu.

5. Do příkazového řádku *Ověřte, že byl vytvořen VNet*použijte portál Azure kontrola vytvořené skupina zdroje s názvem *basename*- ntwk-rg a, aby obsahoval položky podobají profilům vidět na následujícím obrázku:

    ![[3]][3]

    >[AZURE.NOTE] Příklady uvedené *basename* byl nastavené *cloudu* při spuštění skriptu.

    Klikněte na *basename*- vnet VNet, klikněte na *podsítí*a ověřte, že byly vytvořeny podsítí ukázáno v následujícím příkladu:

    ![[4]][4]

6. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte, dokud se vytvoří web osy a vyrovnávání zatížení.

7. Do příkazového řádku *Ověřte, zda Web osy byl vytvořen správně*použijte portál Azure kontrola vytvořené skupina zdroje s názvem *basename*web osy rg a, aby obsahoval položky podobají profilům ukázáno v následujícím příkladu:

    ![[5]][5]

8. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte NVAs vytvořené.

9. Do příkazového řádku *Ověřte, zda NVA byl vytvořen správně*pomocí portálu Azure zkontrolujte, že skupina zdroje s názvem *basename*– Správa-rg byl vytvořen pomocí následujícího obsahu:

    ![[6]][6]

10. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte jumpbox se vytvoří.

11. Do příkazového řádku *Ověřte, zda jumpbox byl vytvořen správně*pomocí portálu Azure zkontrolujte následující položky přidané do skupiny *basename*rg – Správa zdrojů:

    - Sady dostupnost s názvem *basename*- jb-jako.

    - OM s názvem *basename*- jb-OM.

    - Rozhraní sítě s názvem *basename*- jb-nic.

12. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte vytvoří brána Azure VPN a připojení. Všimněte si, že tento krok může trvat až 30 minut.

13. Do příkazového řádku *Ověřte, zda brána VPN byl vytvořen správně*pomocí portálu Azure zkontrolujte následující položky přidané do skupiny *basename*- ntwk rg zdrojů:

    - Brána místní sítě s názvem lgw na nasazení.
    
    - Brána virtuální sítě s názvem *basename*- vpngw.

    - Připojení brány s názvem *basename*- vnet-vpnconn. Nezapomeňte, že stav připojení může *není připojený* , pokud jste ještě nenakonfigurovali místní konec připojení. bude věnovat to později.

14. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte, dokud se vytvářejí VMs a další zdroje pro DMZ.

15. Do příkazového řádku *Ověřte, zda DMZ byl vytvořen správně*pomocí portálu Azure zkontrolujte, že skupina zdroje s názvem *basename*-dmz-rg byl vytvořen pomocí následujícího obsahu:

    ![[7]][7]

16. Do příkazového řádku v okně flám prostředí stisknutím klávesy. By se měly následujících pokynů:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Přihlaste se k počítači místní, který se připojuje k Azure brány a nakonfiguruje připojení řádně podporovat. Přidáte statické trasy do zařízení brány místní, který směruje requestsfor oblast 10.0.0.0/16 adresu až bránu VNet. Pokyny pro tímto způsobem se budou lišit podle toho, jak se připojit. V tématu [implementace hybridní architektura sítě s Azure a místních VPN] [ implementing-a-hybrid-network-architecture-with-vpn] Další informace.

    Všimněte si, že můžete najít veřejnou IP adresu brána Azure VPN pomocí portálu Azure zkoumat brány - vpngw *basename*ve skupině *basename*- ntwk rg zdroje:

    ![[8]][8]

    Můžete určit, zda připojení správně pohledem stavu připojení - vnet vpnconn *basename*. Je třeba nastavit na *Připojit*.

    Chcete-li otestovat připojení, připojení ke vzdálené ploše se otevře jumpbox (10.0.0.254) z počítače nachází ve svojí místní síti.

17. Do příkazového řádku v okně flám prostředí stisknutím klávesy. V dalšího řádku, *stisknutím libovolné klávesy aktualizovat nastavení VNet VNet tak, aby ukazovaly na místní DNS*, stisknutím klávesy a počkejte nastavení DNS VNet se aktualizují hodnotu, kterou jste zadali jako parametr **ON_PREMISES_DNS_SERVER_ADDRESS** skriptu azuredeploy.sh.

18. Do příkazového řádku, *Ověřte, že nastavení serveru DNS na VNet byl aktualizovaný*, použijte portál Azure zkoumat nastavení *serverů DNS* *basename*- vnet VNet ve skupině *basename*- ntwk rg zdroje. Je třeba nastavit na *Vlastní DNS*a *primární server DNS* by měl být adresy místního serveru DNS:

    ![[9]][9]

19. Do příkazového řádku *stisknutím libovolné klávesy vytvořit skupinu prostředků pro servery AD* v okně flám prostředí stisknutím klávesy a počkejte, dokud se vytvoří skupina zdroje při blokování servery AD v cloudu.

20. Do příkazového řádku *stisknutím libovolné klávesy vytvořit VMs servery AD* v okně flám prostředí stisknutím klávesy a počkejte VMs vytvořené a přidané do skupiny zdrojů.

21. Po zobrazení *stisknutím libovolné klávesy připojení VMs na místní doménu* přejděte na portál Azure a ověřte vytvořené skupinu nazvanou *basename*dns-rg a obsahuje dva VMS (*basename*-ad1-OM a *basename*-ad2-OM):

    ![[10]][10]

22. Ve skupinovém rámečku zdroj - ntwk rg *basename*zaškrtnutí vytvořené NSG s názvem *basename*-ad-nsg. Prozkoumejte pravidla příchozí a odchozí zabezpečení pro tento NSG. Odpovídají by měl datové typy v tabulkách [součásti řešení] [ solution-components] oddíl.

23. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte VMs se přidají do místní domény.

24. V *přejděte místní server AD pro ověření počítačů přidané k doménám* výzvu, připojte k počítači místní a zjistit, jak VMs přidané do domény pomocí konzole *uživatele služby Active Directory a počítače* :

    ![[11]][11]

25. Do příkazového řádku v okně flám prostředí stisknutím klávesy a počkejte na web replikace AD se vytvoří v doméně.

26. Na *přejděte místní server AD ověřit, že byl vytvořen webu replikace* objeví se výzva, používat konzolu *sítě a Active Directory Services* na zkontrolovat, že replikace webu s názvem *Azure Vnet Ad webů* úspěšné vytvoření, spolu s spojení přenosu mezi sítěmi IP s názvem *AzureToOnpremLink*a podsítě, který odkazuje VNet:

    ![[12]][12]

27. Do příkazového řádku v okně flám prostředí stisknutím klávesy a instaluje skript adresářovými službami a DNS jednotlivých hodnot AD VMs.

28. Po zobrazení výzvy *přihlášení zadejte podrobnosti každé server Azure AD pro ověření, že byl úspěšně nakonfigurován adresářovými službami* otevřete připojení ke vzdálené ploše z místního počítače a jumpbox (*basename*OM - jb) a pak otevřete jiné připojení ke vzdálené ploše z jumpbox první server AD (*basename*-ad1-OM). Přihlaste se pomocí **název_domény**, **ADMIN_USER_NAME**a **ADMIN_PASSWORD** , kterou jste zadali v skriptu azuredeploy.sh. Pomocí Správce serveru zkontrolujte, že rolí služby AD DS a DNS obě byly přidány. Tenhle postup opakovat pro druhý server AD (*basename*-ad2-OM).

29. Do příkazového řádku v okně flám prostředí stisknutím klávesy. Po zobrazení výzvy *stisknutím libovolné klávesy nastavení Azure VNet DNS tak, aby ukazovaly na server DNS v Azure* stisknutím klávesy a povolit skript, který chcete aktualizovat nastavení DNS VNet.

30. Když na výzvu *Ověřte, že DNS VNet nastavení byl aktualizovaný odkaz DNS OM Azure servery* se zobrazí, pomocí Azure portálu kontroly nastavení *Serverů DNS* *basename*- vnet VNet ve skupině *basename*- ntwk rg zdroje. Servery DNS primárních a sekundárních má nyní odkazovat dvou VMs AD:

    ![[13]][13]

31. Restartujte všech AD VMs před pokračováním. Tento krok je nutné zajistit, že každá vystopovat správné nastavení DNS z Azure. Počkejte, až obou VMs běží před pokračováním.

32. Do příkazového řádku v okně flám prostředí stisknutím klávesy. Do dalšího řádku *stisknutím libovolné klávesy použít brány UDR podsítě brány (ji může mohly být odebrány)*, stisknutím klávesy a povolit skript k aktualizaci brány UDR.

33. Ověření úspěšné dokončení skriptu.

## <a name="next-steps"></a>Další kroky

- Přečtěte si doporučené postupy pro [vytváření doménové poskytujícího přidá] [ adds-resource-forest] v Azure.

- Přečtěte si doporučené postupy pro [vytváření infrastruktury služby AD FS] [ adfs] v Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Zabezpečené hybridní architektura sítě se službou Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Uživatelé služby Active Directory a počítačů konzoly výpis dvou VMs Azure jako servery"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Konzole sítě a Active Directory Services s nastavením replikace webu v cloudu"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Seznam členů skupiny basename ntwk rg zdroje"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Podsítě v VNet basename vnet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Položky ve vrstvě web"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "NVAs ve skupině basename Správa rg zdroje"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Zdroje ve skupině basename dmz rg zdroje"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Jak najít veřejnou IP adresu brána Azure VPN"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "Nastavení serveru DNS pro * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "* Basename *-dns-rg obsahující AD server VMs pole Skupina zdroje"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Uživatelé služby Active Directory a počítačů konzoly výpis AD server VMs jako členové domény"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Konzole sítě a Active Directory Services zobrazující replikace webu Azure AD serverů"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "Nastavení serveru DNS pro VNet basename vnet odkazování na servery AD v cloudu"