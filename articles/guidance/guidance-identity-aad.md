<properties
   pageTitle="Provádění Azure Active Directory | Microsoft Azure"
   description="Jak implementovat architektura sítě zabezpečené hybridní pomocí služby Azure Active Directory."
   services="guidance,virtual-network,active-directory"
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
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Provádění Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro integraci místní domény Active Directory (AD) a struktury službou Azure Active Directory kvůli ověření identity cloudové.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení.

Můžete procházet adresář a identity služby, třeba můžou být poskytovanou AD DS Directory Services (AD) k ověření identity. Tyto identity náleží uživatelům, počítačů, aplikací nebo jiných zdrojů, které jsou součástí hranici zabezpečení. Můžete hostovat adresář a identity služby místní, ale ve scénáři hybridní umístění prvky aplikace v Azure může být efektivnější rozšiřte prvek pro tuhle funkci do cloudu. Tento přístup snížit zpoždění způsobená odesílání ověřování a požadavky na místní ověření z cloudu zpátky na adresář a identity služeb spuštěných místní.

Azure poskytuje dva řešení pro implementaci služby directory a identity v cloudu:

1. Můžete použít [Azure Active Directory (AAD)] [ azure-active-directory] v cloudu vytvořit doméně služby AD a odkaz na místního AD domény. AAD umožňují konfigurovat jednotné přihlašování (SSO) pro uživatele používající aplikací prostřednictvím cloudu. AAD využívá [Azure AD Connect] [ azure-ad-connect] systém místních replikovat objekty a identity uskutečňuje v místním úložišti do cloudu.

    Můžete implementovat AAD bez použití místního adresáře. V tomto případě AAD slouží jako primární zdroj všechny informace o identitě nikoli obsahující data replikovat z místního adresáře.

    Všimněte si, že je v adresáři v cloudu **není** rozšíření místního adresáře. Místo toho je kopií obsahující stejné objekty a identity. Změny, které tyto položky místní se zkopírují do cloudu, ale změny provedené v cloudu **nejsou** replikovat zpět na místní domény.

    >[AZURE.NOTE] Edice Azure Active Directory Premium podporují zpětného zápisu uživatelská hesla, povolení místní uživatele k provedení Samoobslužná resetování hesla v cloudu. Toto je pouze informace, které AAD synchronizuje zpátky do místního adresáře. Další informace o různých vydání AAD a jejich funkcí naleznete v tématu [Azure Active Directory edice][aad-editions].

2. Z místního datacentra nástroje můžete nasazovat OM spuštěný služby AD DS jako řadiče domény v Azure rozšíření stávající infrastrukturu AD (včetně služby AD DS AD FS). Tento přístup je další běžné scénáře, které v místní síti a Azure virtuální sítě se připojují připojení VPN a/nebo ExpressRoute. Tato řešení taky podporuje obousměrné replikace umožňuje provádět změny v cloudu a místní, ať je nejvhodnější.

Tato architektura se zaměřuje na řešení 1. Další informace o druhé řešení, najdete v článku [Rozšíření Active Directory, aby Azure][guidance-adds].

Typické použití případů pro tuto architekturu patří:

- Poskytování jednotného přihlašování pro koncové uživatele systém SaaS a dalších aplikací v cloudu, pomocí stejné přihlašovací údaje, které určují pro místní aplikace.

- Publikování místní webových aplikací do cloudu zajistit přístup k vzdálených uživatelů, kteří patří do vaší organizace.

- Implementace samoobslužné funkce pro koncové uživatele, například resetování hesla a delegování Správa skupiny. 

    >[AZURE.NOTE] Tyto možnosti můžete řídit pomocí správce prostřednictvím zásad skupiny.

- Situace, kde v místní síti a Azure virtuální sítě hostingu cloudové aplikace nebyly propojené přímo pomocí tunelem virtuální privátní sítě nebo ExpressRoute okruh.

Je třeba si uvědomit, že AAD neposkytuje všechny funkce AD. Například AAD aktuálně jenom zpracovává ověřování uživatelů spíše než ověřování počítače. Některé aplikace a služby, jako je SQL Server, může vyžadovat ověřování počítače v tomto případě není vhodné toto řešení. Navíc AAD nemusí být vhodné pro systémy kde mohli migrovat součásti přes hranice na místní/cloudu nikoli pouze kopírovaný.

> [AZURE.NOTE] Podrobné vysvětlení fungování Azure Active Directory, podívejte se na [Microsoft Active Directory vysvětlení][aad-explained].

## <a name="architecture-diagram"></a>Diagram architektury

Následující diagram zvýrazní důležitá součástí tohoto architektura. Další informace o prvcích zátěží na projektu v Azure číst [Systém VMs N-vrstvy architektury na Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Zjednodušení tento graf pouze zobrazí připojení přímo související s AAD a ne znázornění přesměrování žádost webového prohlížeče nebo jiné protokol související přenosy v síti, která může nastat jako součást procesu federace ověřování a identity. Například mít uživatel (místní nebo vzdálené) obvykle přístup do webových aplikací pomocí prohlížeče a web appu můžou transparentně přesměrovat webového prohlížeče ověření žádosti prostřednictvím AAD. Po ověření, může být žádost předán zpátky na web appu společně s příslušnou identity informace.

[! [0]][0]

- **Klienta služby azure Active Directory**. Toto je instanci AAD vytvořené ve vaší organizaci. Funguje jako jednoduchý adresářové služby získáte cloudu (obsahuje objekty zkopírovali z místní AD) a poskytuje identity služby.

- **Web osy podsítě**. Tento podsítě obsahuje VMs, které implementovat vlastní aplikace cloudového vyvinuté ve vaší organizaci a pro které AAD můžou představovat zprostředkovatele identit.

- **Místní server služby AD DS**. Vaše organizace pravděpodobně už má existující adresářové služby například služba AD DS. Můžete synchronizovat všechny položky v adresáři služby AD DS (například informace o uživatelích a skupiny) s AAD povolit AAD tyto informace použít k ověření identity.

- **Server azure AD Connect synchronizace**. Toto je místním počítači spuštěná služba Azure AD Connect synchronizace. Nainstalovat tuto službu pomocí Azure AD Connect software. Synchronizace služby Azure AD Connect synchronizuje informace (uživatelů, skupin, kontakty, atd.) v místní AD AAD v cloudu. Například zřízení nebo deprovision uživatele a skupiny místních a tyto změny se rozšíří do AAD. Synchronizace služby Azure AD Connect předá informace AAD klienta.

    >[AZURE.NOTE] Z bezpečnostních důvodů přímo do AAD neukládají uživatelského hesla. Místo toho AAD obsahuje algoritmus hash každý heslo. Toto je dostatečně ověření uživatelského hesla. Pokud uživatel vyžaduje resetování hesla, musí být provedena místním prostředím a nové hash posílat AAD. Edice AAD Premium obsahují funkce, které automatizují tento úkol umožníte uživatelům resetování vlastního hesla.

## <a name="recommendations"></a>Doporučení

Tato část obsahuje souhrn doporučení pro implementaci AAD následujícím způsobem.

- AD Connect
- Zabezpečení

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect synchronizační služba doporučení

Synchronizace se týká zajistit konzistentní s uloženy místně informace identit uživatelů v cloudu. Má synchronizace služby Azure AD Connect účel udržovat tento konzistence. Následující body sumarizovat doporučení pro implementaci synchronizace služby Azure AD Connect:

- Před implementaci synchronizace Azure AD Connect, měli byste určit synchronizace požadavky organizace (co chcete synchronizovat, ze které domény a jak často. V článku [požadavky synchronizace adresářů určení] [ aad-sync-requirements] popisuje body, které byste měli zvážit.

- Spuštění synchronizace služby Azure AD Connect pomocí virtuálního počítače nebo počítač hostované místní. V závislosti na kolísání informace ve vašem adresáři AD po provedl počáteční synchronizace s AAD zatížení synchronizace služby Azure AD Connect je pravděpodobně vysoká. Použití virtuálního počítače umožňuje snadněji měřítko serveru (monitor aktivity podle popisu v části [Sledování aspektech](#monitoring-considerations) a zjistit, zda je to nutné).

- Pokud máte víc domén v místním ve struktuře, můžete ukládat a synchronizovat informace pro celou strukturu do jednoho klienta AAD (Toto je doporučený postup). Filtrování informací identit probíhajících v víc než jednu doménu tak, aby každý identit se zobrazí pouze jednou AAD spíše než duplikovaný, jak je to může vést k nekonzistence při synchronizaci data. Tento postup vyžaduje implementaci více Azure AD Connect synchronizace serverů. Další informace najdete v tématu scénář více AAD v části [topologie aspektech](#topology-considerations) . 

- Filtrováním můžete omezit data uložená v AAD pouze, což je potřeba. Vaše organizace nemusí například pro uložení informací o neaktivní nebo jiné než osobní účty v AAD. Filtrování může být skupina založená, domén, na základě OU nebo podle atributů a zkombinováním filtrů generovat složitější pravidla. Například může vyberete k synchronizaci jenom objekty v doméně, které obsahují určité hodnoty v vybraný atribut. Podrobné informace najdete v tématu [Azure AD Connect synchronizační: Konfigurace filtrování][aad-filtering].

- Provádět dostupnost pro synchronizační službu AD Connect spusťte sekundární pracovní server. Další informace naleznete v části [topologie aspektech](#topology-considerations) .

### <a name="security-recommendations"></a>Doporučení týkající se zabezpečení

Následující položky souhrnu primární zabezpečení doporučení pro implementaci AAD, podle toho, požadavky organizace:

- Správa uživatelských hesel.

    Pokud používáte premium edition AAD, můžete zapnout heslo zpětného zápisu z AAD do adresáře vaší místní vlastní instalace Azure AD Connect:

    [! [9]][9]

    Tato funkce umožňuje uživatelům vlastní hesel z v portálu Azure, ale jenom používat po zkontrolování zásady zabezpečení heslo vaší organizace. Například je možné omezit určující, kteří uživatelé mohou měnit jejich hesla a přizpůsobení možností správy heslo. Další informace najdete v tématu [Přizpůsobení Správa hesel potřebám vaší organizace][aad-password-management].

- Správa ochrany pro místní aplikace, které jsou přístupné externě.

    Použijte proxy server Azure AD aplikace k poskytnutí řízený přístup místní webových aplikací z externích uživatelů prostřednictvím AAD. Tento přístup chrání aplikace vystavením není je přímo k Internetu. Pouze uživatelé, kteří mají platných přihlašovacích údajů v adresáři Azure se dostanete aplikace. Další informace najdete v článku [Povolení aplikace Proxy Azure portálu][aad-application-proxy].

- Sledování aktivně AAD znaménka argumentu podezřelé aktivity.

    Zvažte použití AAD Premium P2 edition, který obsahuje ochrana Identity AAD. Ochrana identity pomocí algoritmů výukové adaptivní počítače a heuristiku zjistí odchylky a rizik události, které mohou poukazovat, aby byly ohroženo identitu. Například rozpozná potenciálně neobvyklých aktivitě, jako třeba Nesouměrné přihlašovací aktivity, přihlášení z neznámých zdrojů nebo IP adres pomocí podezřelé aktivity nebo přihlášení ze zařízení, která může infekce. Pomocí těchto dat, ochrana Identity vygeneruje sestavy a výstrahy, které umožňují prozkoumat tyto rizika události a odpovídající remediation nebo omezení rizik akce. Další informace najdete v tématu [Ochrana Azure Active Directory Identity][aad-identity-protection].

    Vytváření sestav funkce AAD Azure portálu taky slouží ke sledování podezřelé nebo jiné související se zabezpečením aktivity vyskytující se v systému. AAD může způsobit řadu souhrnné sestavy:

    [! [17]][17]

    Další informace o použití těchto zpráv najdete v článku [Azure Active Directory vykazování Průvodce][aad-reporting-guide].

## <a name="topology-considerations"></a>Důležité informace o topologii

Pokud místního adresáře jsou integrace s AAD, nakonfigurujte Azure AD Connect implementovat topologie, která nejvíc odpovídá požadavky organizace. Topologie, které podporuje Azure AD Connect patřit následující úkoly:

- **Jeden struktury, jeden AAD adresář**. V této topologii používat Azure AD Connect synchronizace objekty a identity informace v jedné nebo víc domén v jednoduchém místních struktuře s jednoho tenanta AAD. Toto je výchozí topologii prováděným Expresní instalace Azure AD Connect.

    [! [1]][1]

    Nevytvoříte více Azure AD Connect synchronizace serverů, připojit různé domény ve stejné struktuře místní na stejném klientovi AAD, pokud používáte server Azure AD Connect synchronizace přípravu režim (viz přípravu Server).

- **Více strukturami, jeden AAD adresář**. Tato topologie použijte, pokud máte víc než jeden doménové místní. Informace o identitě můžete sloučit tak, aby každý uživatel jedinečné představuje jednou v adresáři AAD i v případě stejné uživatel vyskytuje v víc doménové. Všechny strukturami používat stejný server Azure AD Connect synchronizace. Synchronizace server Azure AD Connect nemusí být součástí libovolné domény, ale musí být dostupná ze všech strukturami:

    [! [2]][2]

    V této topologii nepoužívejte samostatné Azure AD Connect synchronizovat servery připojení jednotlivé místní domény do jednoho klienta AAD. Výsledkem může duplicitní identity informace v AAD Pokud uživatelé se účastní víc doménové.

- **Více strukturami, samostatné topologií**. Tento přístup umožňuje hromadnou korespondenci identity samostatné strukturami do jednoho tenanta AAD. Tato strategie je užitečné, pokud se součtem strukturami od jiné organizace (po fúze nebo pořízení, například) a informace o identitě pro každého uživatele směřuje pouze v jedné struktuře:

    Jsou synchronizovány GAL v každé doménové, uživatel v jedné doménové může být prezentace v jiném jako kontakt. Pokud například vaše organizace provedla GALSync s správce Forefront identit 2010 nebo Microsoft správce identit 2016 to může dojít. V tomto scénáři můžete určit, aby uživatelé měli označenými jejich atribut *Pošta* . Můžete taky porovnat identit pomocí atributů *ObjectSID* a *msExchMasterAccountSID* . To je užitečné, pokud máte jednu nebo více strukturami zdroje s zakázané účty.

- **Pracovní serveru**. V této konfiguraci spustíte druhý výskyt synchronizace server Azure AD Connect souběžně s prvním. Tuto strukturu podporuje scénáře, jako:

    - Dostupnost.

    - Testování a nasazení nové konfigurační server Azure AD Connect synchronizace.

    - Úvodní informace o nové serveru a vyřadit z staré konfigurace. 

    Podobnému sledu druhé instance spuštěna v *přípravu režimu*. Server záznamy importované objekty a synchronizace dat v databázi, ale ne předat data AAD. Pouze v případě, že zakážete pracovní režimu serveru začít psát dat AAD a spuštění provést heslo zapsat mailům do místního adresáře v případě potřeby:

    [! [4]][4]

    Další informace najdete v tématu [Azure AD Connect synchronizace: provozní úkoly a informace, které][aad-connect-sync-operational-tasks].

- **Více AAD adresáře**. Se doporučuje vytvořit jednu adresář AAD pro organizaci, ale může být situace, kdy je potřeba informací o oddílech přes samostatné AAD adresáře. V tomto případě je třeba zajistit, že všech objektů ze struktury místní se zobrazí pouze v jednom adresáři AAD, chcete-li předejít zpětného zápisu problémy synchronizace a heslo.

    Provádět tento scénář konfigurace samostatné Azure AD Connect synchronizace servery pro každý AAD adresář a používat funkci filtrování tak, aby každý server Azure AD Connect synchronizace funguje na sady vzájemně se vylučujících objektů: 

    [! [5]][5]

Další informace o těchto topologie najdete v tématu [topologie pro Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Uživatelské přihlašovací co byste měli zvážit

Ve výchozím nastavení služba AAD předpokládá, že uživatel přihlásí zadáním stejné heslo, použít místní a synchronizace server Azure AD Connect nakonfiguruje synchronizaci hesel mezi místní domény a AAD. Mnoho v organizacích je to vhodné, ale byste měli zvážit existující zásady a infrastruktury vaší organizace. Příklad:

- Zásady zabezpečení vaší organizace může zakázat synchronizace hash heslo do cloudu.

- Při přístupu k prostředkům cloudu z domény počítačích spojují podnikové síti může vyžadovat, aby uživatelé zaznamenat bezproblémové jednotné přihlašování (bez zobrazí výzvu další heslo).

- Vaše organizace může být už máte služby AD FS nebo u jiného federace poskytovatele nasazení. Můžete nakonfigurovat AAD používat tuto infrastrukturu implementace ověřování a jednotné přihlašování ne pomocí hesla údaje uložené v cloudu.

Další informace najdete v tématu [Azure AD připojit uživatele přihlášení možnosti][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD aplikace proxy co byste měli zvážit

Zajistit přístup k aplikacím místní použijte Azure AD.

- Zveřejnění proxy aplikace, které spojovací čáry ovládat součást aplikace proxy server Azure AD pomocí webové aplikace místní. Konektor aplikace proxy otevře odchozí připojení na proxy server Azure AD aplikace. Požadavky na vzdálené uživatele jsou směrovány zpět z AAD pomocí tohoto připojení do webových aplikací. Tento postup odebere potřebné k otevření příchozí portů brány firewall místní snížení napadení zveřejněné pro vaši organizaci.

Další informace najdete v tématu [publikování aplikací pomocí proxy server Azure AD aplikace][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Důležité údaje související se synchronizací objektu

Výchozí konfigurace Azure AD Connect synchronizuje objekty ze svého místního adresáře AD podle sady pravidel uvedené v následujícím článku [Azure AD Connect synchronizace: Principy výchozí konfigurace][aad-connect-sync-default-rules]. Jenom objekty, které splňují těchto pravidel jsou synchronizovány, ostatní jsou ignorovány. Například objekty uživatelů musí mít jedinečný *sourceAnchor* atribut a atribut *accounEnabled* musí vyplněné. Objekty uživatelů, které nemají atribut *sAMAccountName* nebo, začněte s textem *AAD_* nebo *MSOL_* nejsou synchronizovány. Azure AD Connect platí některá pravidla pro objektů uživatelů, kontaktů, skupiny, ForeignSecurityPrincipal a počítačů. Pokud potřebujete změnit výchozí sadu pravidel, použití editoru pravidla synchronizace součástí Azure AD Connect (rovněž popsány v [Azure AD Connect synchronizace: Principy výchozí konfigurace][aad-connect-sync-default-rules]).

Můžete definovat vlastní filtry pro omezení objekty, které chcete stáhnout nový nebo aktualizovaný tak, že doménu nebo organizační jednotku. Nebo provádět složitější vlastního filtrování podle popisu v [Azure AD Connect synchronizace: Konfigurace filtrování][aad-filtering].

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Pomocí řízení přístupu podmíněné odmítnout požadavky ověřování z neočekávaných zdrojů:

- Aktivace [Azure Multi-Factor Authentication (MFA)] [ azure-multifactor-authentication] Pokud uživatel pokouší připojit z jiných důvěryhodného umístění (například všude na Internetu) místo v síti důvěryhodné.

- Pomocí typ platformy zařízení uživatele (iOS, Android, Windows Mobile Windows) stanovit zásady přístupu k aplikacím a funkce.

- Zaznamenat povolit nebo zakázat stav zařízení uživatelů a začlenit tyto informace do kontroly zásady přístupu. Například případě ztráty nebo krádeže uživatele telefonní ho zaznamenávají jako zakázané kvůli kterým ho použít k získání přístupu.

- Určují úroveň přístupu pro uživatele podle členství ve skupinách. Použití [pravidel dynamické členství AAD] [ aad-dynamic-membership-rules] ke zjednodušení správy skupiny. Stručný přehled o tom, jak to funguje, najdete v článku [Úvod k dynamické členství ve skupinách][aad-dynamic-memberships].

- Pomocí podmíněného přístupu rizika zásady ochrana Identity AAD rozšířené ochrany na základě neobvyklé přihlašovací aktivity nebo jiné události.

Další informace najdete v tématu [podmíněného přístupu Azure Active Directory][aad-conditional-access].

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Škálovatelnost řešili službu AAD a konfigurace synchronizace server Azure AD Connect:

- Pro službu AAD nemáte konfigurace všechny možnosti implementace škálovatelnost. Služba AAD podporuje škálovatelnost podle repliky. AAD používá jeden primární otevřené, které úchyty psaní operace, a více replikách vedlejší jen pro čtení. AAD transparentně přesměruje pokus o zápisů proti sekundární repliky provedené primární otevřené. AAD poskytuje případné konzistence. Všechny změny provedené primární otevřené se rozšíří do vedlejší repliky. Většina operace u AAD jsou čtení spíše než zápisy, tato architektura dobře měřítko.

    Další informace najdete v tématu [Azure AD: Rozšířená našeho adresáře geo nadbytečné, vysoce dostupné, distribuované cloudu][aad-scalability].

- Synchronizace server Azure AD Connect byste měli zjistit, kolik objekty budete pravděpodobně synchronizovat z místního adresáře. Pokud máte míň než 100 000 objektů, můžete použít výchozí SQL serveru Express LocalDB softwaru dodaného s Azure AD Connect. Pokud máte velký počet objektů, doporučujeme nainstalovat výrobní verzi serveru SQL Server a vlastní instalace Azure AD Connect určit, že by měl použít existující instanci systému SQL Server.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Stejně jako u škálovatelnost pochybnosti dostupnost přesahuje službu AAD a konfigurace Azure AD Connect:

- Služba AAD slouží k poskytování vysoké dostupnosti. Nenabízí žádné možnosti konfigurovat uživatel dostupnosti. Je distribuovaná geo a spustí víc dat centra šíření celém světe automatické převzetí. Pokud datovém centru nebude k dispozici, AAD zajistí adresáře data k dispozici pro přístup k instanci v aspoň dva další regionální rozdělený datacentrech.

    >[AZURE.NOTE] SLA dostupnosti AAD základní a Premium služby záruky aspoň 99,9 %. Neexistuje žádný SLA bezplatné osy AAD. Další informace najdete v tématu [SLA pro službu Azure Active Directory][sla-aad].

- Pro zvýšení použitelnosti synchronizační server Azure AD Connect spuštěním druhou instanci v pracovní režimu, podle popisu v části [topologie aspektech](#topology-considerations) . 

    Kromě toho pokud nepoužíváte instanci systému SQL Server Express LocalDB, které jsou součástí Azure AD Connect, pak zvažte dostupnost pro systém SQL Server. Všimněte si, že řešení pouze dostupnost podporované SQL clusterů. Řešení například odrážející strukturu a vždy o nepodporuje Azure AD Connect.

    Další informace o ochraně dostupnost synchronizace server Azure AD Connect a jak obnovit po selhání, najdete v článku [Azure AD Connect synchronizace: provozní úkoly a informace – havárie obnovení][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Důležité informace

Existují dva aspekty správu AAD:

1. Správa AAD v cloudu.

2. Správa synchronizace servery Azure AD Connect.

AAD poskytuje následující možnosti pro správu domén a adresáře v cloudu:

- [Modul Azure Active Directory prostředí PowerShell][aad-powershell]. Pokud potřebujete skriptu běžné Azure AD pro správu úkoly, například Správa uživatelských účtů správy domén a konfigurace jednotného přihlašování, použijte tento modul.

- Azure AD správy zásuvné Azure portálu. Tento zásuvné poskytuje zobrazení interaktivní správy adresáře a umožňuje řízení a konfiguraci většina aspektů AAD.

    [! [10]][10]

Azure AD Connect nainstaluje následující nástroje, které používáte ke správě synchronizační služby Azure AD Connect z místního počítače:

- Microsoft Azure Active Directory připojení konzoly. Tento nástroj umožňuje změnit konfigurační server Azure AD Sync, přizpůsobit, jak dochází k synchronizaci, povolení nebo zakázání pracovní režimu a přepnutí přihlašovací režim uživatelů (můžete povolit použití infrastrukturu místní přihlašovací AD FS).

- Správce služby synchronizace. Na kartě *operace* v tomto nástroji pro správu synchronizaci a zjistit, jestli se nepodařilo některé části procesu. Synchronizace ručně tímto nástrojem můžete aktivovat. 

    [! [12]][12]

    *Spojnice* karta umožňuje řídit připojení pro domény (místní i schránkami v cloudu) na které je připojen modul synchronizace:

    [! [13]][13]

-  Editor pravidel synchronizace. Pomocí tohoto nástroje přizpůsobit způsob, ve kterém jsou objekty transformována kopírování mezi místního adresáře a AAD v cloudu. Tento nástroj umožňuje zadat další atributy a objektů k synchronizaci a filtrů určit, které instance měli nebo neměli synchronizovat.

    Další informace naleznete v části synchronizace Editor pravidel v dokumentu [Azure AD Connect synchronizace: Principy výchozí konfigurace][aad-connect-sync-default-rules].

Na stránce [Azure AD Connect synchronizační: doporučené postupy pro změnu výchozí konfigurace] [ aad-sync-best-practices] obsahuje další informace a tipy pro správu Azure AD Connect.

## <a name="monitoring-considerations"></a>Sledování co byste měli zvážit

Sledování stavu se provádí pomocí řady nainstalovaná agenti v místním nasazení:

- Azure AD Connect nainstaluje agent shromažďuje informace o synchronizace. Sledování stavu a výkonu Azure AD Connect pomocí zásuvné Azure Active Directory připojení stavu na portálu Azure. Další informace najdete v tématu [Použití Azure AD připojení stav synchronizace][aad-health].

- Chcete-li sledovat stav služby AD DS domén a adresářů z Azure, nainstalujte Azure AD připojení stavu služby AD DS agenta v počítači v rámci místní domény. Použijte zásuvné Azure Active Directory připojení stavu na portálu Azure AD DS monitor. Další informace najdete v tématu [Stavu připojení pomocí Azure AD pomocí služby AD DS][aad-health-adds]

- Nainstalujte Azure AD připojení stavu služby AD FS Agent sledovat stav služby AD FS spuštěná na místním a sledování služby AD DS pomocí zásuvné Azure Active Directory připojení stavu na portálu Azure. Další informace najdete v tématu [Použití Azure AD připojení stavu se službou AD FS][aad-health-adfs]

Další informace o instalaci agenti stavu připojení AD a jejich požadavky, přečtěte si téma [Azure AD připojení stavu Agent instalací][aad-agent-installation].

## <a name="sample-solution"></a>Ukázka řešení

Tato část obsahuje kroky pro vytváření ukázkové řešení, které můžete použít k testování konfigurace AAD. Řešení zahrnuje následující prvky:

- N osy webové aplikace, sledovat struktuře označená [Systém VMs N-vrstvy architektury na Azure][implementing-a-multi-tier-architecture-on-Azure].

- Test klientský počítač. Doporučujeme používat jiné OM pro tento počítač. Konfigurace tento OM je důležité, pokud máte přístup správce a můžete připojit k n osy webové aplikace.

- Simulovaný místního prostředí, které obsahuje vlastní domény vytvořené pomocí služby AD DS.

Scénáře, které ukazují tyto kroky jsou:

- Povolení přístupu k n osy webové aplikace spuštěné v cloudu pro externí uživatele, se AAD poskytující ověřování hesla.

- Povolení přístupu k n osy webové aplikace spuštěné v cloudu pro uživatele používající v rámci organizace, s AAD poskytující ověřování hesla.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Popisují následující kroky předpokládají následující požadavky:

- Máte existující Azure předplatné, ve kterém můžete vytvořit skupiny zdrojů.

- Máte nainstalované [rozhraní příkazového řádku Azure][azure-cli].

- Jste stáhli a nainstalovali nejnovější Tvůrce dotazů. Najdete [tady] [ azure-powershell-download] pokyny.

- Máte nainstalované kopii [makecert] [ makecert] nástroje na klientském počítači.

- Máte přístup k počítači vývoj, který má Visual Studio nainstalovaný.

### <a name="create-the-n-tier-web-application-architecture"></a>Vytvoření architektura n osy webových aplikací

1. Vytvoření složky s názvem `Scripts` , která obsahuje podsložky s názvem `Parameters`.

2. Stažení [Nasazení ReferenceArchitecture.ps1] [ solution-script] skript Powershellu ke složce skriptů.

3. Ve složce parametry vytvořte další podsložku s názvem Windows.

4. Stáhněte si následující soubory do složky parametry/Windows:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Upravte soubor **Nasazení ReferenceArchitecture.ps**1 ve složce skripty a změňte následující řádek můžete určit, která má být vytvořené nebo používá k ukládání OM a vytvoření skriptem zdrojů Skupina zdroje:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Upravte následující soubory ve složce s **Parametry/okno**a nastavte `resourceGroup` hodnota argumentu `virtualNetworkSettings` v každé z těchto souborů, které chcete být stejné jako v části, kterou jste zadali v soubor skriptu **ReferenceArchitecture.ps1 nasazení** .

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Otevřete okno Azure Powershellu, přejděte do složky skripty, spusťte tento příkaz:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Nahrazení **<subscription id>** pomocí svého ID Azure předplatného.

    Pro **<location>**, zadejte Azure oblast, například *eastus* nebo *westus*.

8. Po dokončení skript použijte portál Azure získat veřejnou IP adresu Vyrovnávání zatížení web osy (*Vzdálená pomoc aad-ntier – web-lb*):

    [! [18]][18]

9. Přihlaste se k test klientského počítače (nebo OM) a ověřte, že se dostanete na úrovni webu pomocí aplikace Internet Explorer umožňuje přecházet na veřejnou IP adresu Vyrovnávání zatížení web osy. Výchozí IIS stránky objevit.

### <a name="simulate-configuration-of-a-public-web-site"></a>Simulovat konfigurace veřejného webu

>[AZURE.NOTE] Podle těchto kroků simulovat přidružení na úrovni webu s názvem www.contoso.com DNS změnou souboru hosts v klientském počítači. Kromě toho VMs web osy nakonfigurovaná s automatickým podpisem certifikáty a phishing hostingu autorita. Toto se týká testování pouze a **že neměli používat tyto postupy v pracovním prostředí**.

1. Test klientského počítače upravte soubor **C:\Windows\System32\drivers\etc\hosts** pomocí programu Poznámkový blok a přidejte položku, která přidružuje název www.contoso.com veřejnou IP adresu Vyrovnávání zatížení oddělených web:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Ověřte, že můžete nyní přecházet na www.contoso.com v klientském počítači. Stejné stránce IIS výchozí objevit jako předtím.

3. Na klientském počítači otevřete okno příkazového řádku jako správce a použijte nástroj makecert c
4. vytvořit certifikát falešné kořenového certifikační autorita:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Ověřte, že se vytvářejí následující soubory:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Spusťte tento příkaz nainstalovat test kořenová certifikační autorita:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Umožňuje generovat certifikát www.contoso.com test certifikační autorita:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Spuštění příkazu **mmc** a přidání modulu snap-in Certifikáty pro účet počítače v místním počítači.

7. V */Certificates (místní počítač) / osobní/certifikát/* úložiště, exportujte certifikát www.contoso.com s soukromým klíčem do souboru s názvem www.contoso.com.pfx:

    [! [20]][20]

8. Vraťte se do portálu Azure a připojte k řízení osy OM (*Vzdálená pomoc aad-ntier – Správa vm1*). Výchozí uživatelské jméno je *testuser* heslem *AweS0me@PW*:

    [! [21]][21]
    
9. Z správy osy OM připojení pro každý web osy VMs zároveň s *testuser* uživatelské jméno a heslo *AweS0me@PW*a dělat tyto věci. Všimněte si, že soukromé adresy IP 10.0.1.4 10.0.1.5 a 10.0.1.6 VMs:

    >[AZURE.NOTE] Webová vrstva VMs zbývá soukromé IP adres. Můžete připojovat se k jejich pomocí správy osy OM.

    1. Kopírování souborů *www.contoso.com.pfx* a *MyFakeRootCertificateAuthority.cer* v klientském počítači.
    
    2. Otevřete okno příkazového řádku jako správce, přejděte do složky blokování souborů, které jste zkopírovali a následující příkazy:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Spustit `mmc` příkaz, přidání modulu snap-in Certifikáty pro účet počítače v místním počítači a ověřte nainstalované certifikáty těchto věcí:

        - Www.contoso.com \Personal\Certificates\ \Certificates (místní počítač)

        - \Certificates (místní počítač) \Trusted kořenovou certifikace Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Spusťte konzolu Správce Internetové informační služby (IIS) a přejděte na *Web servery\Výchozí* v počítači.

    5. V podokně *akcí* klikněte na *vazby*a přidejte https vazba pomocí certifikátu SSL www.contoso.com:

        [! [22]][22]

10. Vraťte se do klientském počítači a ověřte, že nyní je možné procházet k https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Vytvoření tenanta služby Azure Active Directory

1. Na portálu Azure, vytvořte tenanta služby Azure Active Directory například *myaadname*. onmicrosoft.com, kde *myaadname* je název adresáře tak, že jste vybrali.

2. Přidání uživatele s názvem *Správce* s rolí GlobalAdmin do adresáře. Poznamenejte si nově generovaného hesla.

3. V Internet Exploreru přejděte na https://account.activedirectory.windowsazure.com/ a přihlaste se jako admin@ *myaadname*. onmicrosoft.com. Změna hesla po zobrazení výzvy.

### <a name="create-and-deploy-a-test-web-application"></a>Vytvoření a nasazení test webové aplikace

1. Pomocí aplikace Visual Studio v počítači vývoj vytvořit webovou aplikaci ASP.NET s názvem ContosoWebApp1 (podržením .NET Framework 4.5.2):

    [! [23]][23]

2. Vyberte šablonu *MVC* , přejděte do *práce a školních účtů*ověřování a zadejte název vašeho klienta AAD. Nevytvářejte služby aplikace v cloudu.

    [! [24]][24]

3. Vytvoření a spuštění aplikace otestovat ověřování. Na přihlašovací stránce zadejte název účtu admin@ *myaadname*. onmicrosoft.com, zadejte heslo a klepněte na tlačítko *přihlásit*:

    [! [25]][25]

4. Ověřte, zda AAD žádostí o souhlas se můžete přihlásit a číst váš profil a potom spuštění webové aplikace pomocí identity správce.

5. Ukončete aplikaci Internet Explorer a vraťte se do aplikace Visual Studio.

6. Otevřete soubor web.config a `<appSettings>` oddíl, změňte hodnotu klávesy *ida: PostLogoutRedirectUri* *https://www.contoso.com:443 /*. Uložte soubor.

7. V okně *Průzkumník řešení* klikněte pravým tlačítkem myši ContosoWebApp1 projektu, klikněte na *Publikovat*.

8. V okně *Publikovat Web* klikněte na *vlastní*. Vytvoření vlastního profilu s názvem *ContosoWebApp1*.

9. Na stránce *připojení* nastavení *Metoda publikování* k *Systému souborů* a zadejte do složky nazvané *ContosoWebApp*, nachází na vhodné místo ve vašem počítači vývoj.

10. Na stránce *Nastavení* nastavte *konfiguraci* *verzi*.

11. Na stránce *náhledu* klikněte na *Publikovat*.

12. Připojení ke každé webovému serveru zase (přes správy osy OM) a dělat tyto věci:

    1. Zkopírujte *ContosoWebApp* složku a její obsah z vývojového počítače do složky *C:\inetpub* .

    2. Pomocí konzoly Správce Internetové informační služby (IIS), přejděte na *webu servery\Výchozí* v počítači.

    3. V podokně *akcí* klikněte na *Základě nastavení*a změna fyzické cesty webu *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Publikování webové aplikace test prostřednictvím AAD

1. Přihlaste se k portálu Azure a přejděte do adresáře AAD.

2. Na kartě *aplikace* klepněte na aplikaci ContosoWebApp1.

3. Zkontrolujte, zda aplikace úspěšně přidána do složky a potom klikněte na kartu *KONFIGUROVAT* .

4. Přejděte *Na PŘIHLAŠOVACÍ adresa URL* https://www.contoso.com:443 a nastavte adresu *URL odpovědět* na https://www.contoso.com:443 (adresu URL).

5. Uložte konfiguraci.

6. Na klientském počítači přejděte do https://www.contoso.com. Ověření, že AAD výzvu k zadání přihlašovacích údajů a pak se přihlaste.

>[AZURE.NOTE] Koncový bod pro první scénář: povolení přístupu k n osy webové aplikace spuštěné v cloudu pro externí uživatele, se AAD poskytující ověřování hesla.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Vytvoření simulovaný místního prostředí se službou Active Directory

Místního prostředí obsahuje dva řadiče domény pro `contoso.com` doménu a dvě servery pro hostování Azure AD Connect synchronizace služby. Servery pro hostování Azure AD Connect nejsou doméně.

1. V Průzkumníku souborů vraťte se do skripty složku obsahující skript použitá k vytvoření webové aplikace N-osy.

2. Ve složce parametry přidat další sub složku s názvem `Onpremise`.

3. Stažení následující soubory do místní parametry/edici složky:

    - [Přidání Přidá doménu – controller.parameters.json][add-adds-domain-controller-parameters]

    - [Vytvoření přidá doménové extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines adc joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork přidá dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Upravte soubor nasazení ReferenceArchitecture.ps1 ve složce skripty a změňte následující řádek můžete určit, která má být vytvořené nebo používá k ukládání OM a vytvoření skriptem zdrojů Skupina zdroje:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Upravte následující soubory ve složce parametry/místní edici a nastavte `resourceGroup` hodnota argumentu `virtualNetworkSettings` v každé z těchto souborů, které chcete být stejné jako v části, kterou jste zadali v soubor skriptu ReferenceArchitecture.ps1 nasazení.

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork přidá dns.parameters.json

7. Otevřete okno Azure Powershellu, přejděte do složky skripty, spusťte tento příkaz:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Nahrazení `<subscription id>` pomocí svého ID Azure předplatného.

    Pro `<location>`, zadejte Azure oblast, jako například `eastus` nebo `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Instalace a konfigurace synchronizace služby Azure AD Connect

Nastavení podle těchto kroků se skládá ze dvou instancí služby Azure AD Connect synchronizace. První je aktivní, když je druhý konfigurovaná v pracovní režimu poskytnout rychlé přepnutí a dostupnost, pokud prvního dojde k chybě.

1. Na portálu Azure, přejděte na skupina zdroje blokování VMs pro synchronizaci služby Azure AD Connect (*Vzdálená pomoc aad místní edici rg* ve výchozím nastavení) a připojte k *Vzdálená pomoc aad-místní edici adc-vm1* OM. Přihlaste se jako *testuser* heslem *AweS0me@PW*.

2. Stahování Azure AD Connect [tady][aad-connect-download].

3. Spusťte instalační program Azure AD Connect a povolenou instalaci možnost *vlastní* .

    >[AZURE.NOTE] Možnost pro *Expresní nastavení* nelze použít, protože není OM synchronizační službu Azure AD Connect doméně.

4. Na stránce *nainstalovat požadované součásti* nezadáte volitelná nastavení přijměte výchozí nastavení.

5. Na stránce *přihlášení uživatele* vyberte *Synchronizace hesel*.

6. Na stránce *připojit k Azure AD* zadejte admin@ *myaadname*. onmicrosoft.com, kde *myaadname* je název vašeho klienta AAD. Zadejte heslo k účtu správce.

7. Na stránce *Připojit vašeho adresáře* zadat contoso.com struktuře (protože v rozevíracím seznamu nezobrazí, zadejte požadovanou hodnotu v), zadejte CONTOSO\testuser pro uživatelské jméno, zadejte AweS0me@PW k zadání hesla a potom klikněte na *Přidat adresář*.

8. Na stránce *Azure AD přihlašovací konfigurace* přijměte výchozí hodnotu pro hlavní název uživatele. Zaškrtněte políčko *bez ověřených domén*.

    >[AZURE.NOTE] V adresáři contoso.com jsou uvedeny *Nejsou ověřené*. Pokud chcete ověřit název domény, musíte vytvořit záznam TXT registrátora název domény. V tomto příkladu není místní domény registrované externě. Další informace najdete v tématu [Přidání vlastního názvu domény pro službu Azure Active Directory][aad-custom-directory].

9. Na stránce *domény a OU filtrování* vyberte *Synchronizovat vše domény a organizačních jednotek* (výchozí).

10. Na stránce *jedinečně identifikující vaši uživatelé* nechejte výchozí hodnoty.

11. Na stránce *filtrovat uživatelům a zařízením* vyberte *synchronizovat všem uživatelům a zařízením* (výchozí).

12. Na stránce *volitelné funkce* vyberte *zpětným heslo*.

13. Na stránce *Připraveno ke konfiguraci* zaškrtněte políčko *Spustit synchronizaci po dokončení konfigurace*se zachováním *Povolit pracovní režim* vypnutá.

14. Ověřte, že proces konfigurace dokončí bez chyb a pak ukončete instalačního programu.

15. Z portálu Microsoft Azure připojte k *Vzdálená pomoc aad-místní edici adc-vm2* OM. Přihlaste se jako *testuser* heslem *AweS0me@PW*.

16. Stáhněte si Azure AD Connect a znovu spusťte instalační program.

17. Kroků průvodce a odpovídání podle popisu v kroky 3 až 12.

18. Na stránce *chtít konfigurace* vyberte *Spustit synchronizaci po dokončení konfigurace*a **také vybrat** *Povolit pracovní režim*. To způsobí, že synchronizace služby Azure AD Connect získat informace o účtech a objekty z doménu contoso.com a byly ukládány v databázi, ale není předá tyto údaje k vašemu tenantovi AAD.

14. Ověřte, že proces konfigurace dokončí bez chyb a pak ukončete instalačního programu.

### <a name="test-the-aad-configuration"></a>Testování konfigurace AAD

1. Na portálu Azure, přepněte do adresáře AAD, otevřete zásuvné Azure Active Directory, klikněte na *Uživatelé a skupiny*a klikněte na *všechny uživatele* zobrazíte seznam uživatelů a skupin synchronizovat s adresářem. Měli byste vidět uživatelů pro tyto účty:

    - správce (admin@ *myaadname*. onmicrosoft.com)

    - Účet služby synchronizace adresářů místní (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - Účet služby synchronizace adresářů místní (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Na portálu Azure přejít do skupiny zdrojů blokování VMs řadiče domény služby AD DS (*Vzdálená pomoc aad místní edici rg* ve výchozím nastavení) a připojte k *Vzdálená pomoc aad-místní edici ad-vm1* OM. Přihlaste se jako *testuser* heslem *AweS0me@PW*.

3. Pomocí konzoly *Uživatelé služby Active Directory a počítačů* přidat nového uživatele domény s názvem Vlasta Tibbot přihlašovací jméno dianet@contoso.com. Zadejte heslo podle svého výběru. Zkontrolujte uživatel členem skupiny místních správců (Toto je pouze tak, aby se můžete přihlásit místně jako tomuto uživateli později - pouze počítačích v doméně jsou řadiče domény).

4. Přepnutí do *Vzdálená pomoc aad-místní edici adc-vm1* OM, otevřete okno prostředí PowerShell a následující příkazy:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Ověřte, zda příkazu vrací *Úspěch*.

    >[AZURE.NOTE] Tento krok je nutné, protože ve výchozím nastavení je naplánováno synchronizaci spuštění každých 30 minut. Tyto příkazy vynutit spuštění synchronizace okamžitě.

5. Vraťte se do portálu Azure, přepněte do adresáře AAD otevřít zásuvné Azure Active Directory, klikněte na *Uživatelé a skupiny*a klikněte na *všechny uživatele*. Teď byste měli vidět Vlasta Tibbot (dianet@ *myaadname*. onmicrosoft.com) se zobrazí v seznamu uživatelů.

6. V Azure Active Directory zásuvné klikněte *Podnikových aplikací*a potom klikněte na *všechny aplikace*. Klikněte na tlačítko *ContosoWebApp1* aplikace a potom klikněte na *Uživatelé a skupiny*. Na panelu nástrojů klikněte na *Přidat*. Klikněte na *Uživatelé a skupiny*a vyberte *Vlasta Tibbot*. Klikněte na *přiřadit*.

7. V Internet Exploreru přejděte na web https://account.activedirectory.windowsazure.com. Přihlaste se jako dianet@ *myaadname*. onmicrosoft.com s dříve zadané heslo.

    >[AZURE.NOTE] Je třeba kliknout na ikonu ContosoWebApp v seznamu aplikací. Při hledání webové aplikace na adrese, veřejně uvedených DNS www.contoso.com, které se liší od adresy Vyrovnávání zatížení web osy se snaží AAD.

8. Klikněte na kartu *profil* . Podrobné informace o uživateli (Pokud jste zadali některou při vytváření) mají být zobrazeny.

    >[AZURE.NOTE] Pokud kliknete na tlačítko *změnit heslo*, nemáte oprávnění k provedení tohoto úkolu jako operaci musí povolit správce nejdřív. Další informace najdete v tématu [Začínáme s Správa hesel][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Povolení místních uživatelů ke spuštění aplikace pomocí ověřování prostřednictvím AAD

1. Vraťte se do řadiče domény *Vzdálená pomoc aad-místní edici ad-vm1* OM.

2. Spusťte konzolu *Správce DNS* a přidejte nový záznam o hostiteli pro www.contoso.com. Zadejte veřejnou IP adresu Vyrovnávání zatížení web osy.

3. Zkopírujte soubor *MyFakeRootCertificateAuthority.cer* z klientském počítači (jste vytvořili tento soubory postupu [Simulovat konfigurace veřejného webu](#simulate-configuration-of-a-public-web-site)
    
4. Otevřete okno příkazového řádku jako správce, přejděte do složky podržte soubor, který jste zkopírovali a spusťte tento příkaz:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. V Internet Exploreru přejděte na https://www.contoso.com. Ověřte, že se zobrazí AAD přihlašovací stránky pro webovou aplikaci ContosoWebApp1.

6. Přihlaste se jako dianet@ *myaadname*. onmicrosoft.com. Aplikace by měl spustit a jste přihlášení správně.

## <a name="next-steps"></a>Další kroky

- Přečtěte si doporučené postupy pro [rozšíření místních domény přidá do Azure][adds-extend-domain]

- Přečtěte si doporučené postupy pro [vytváření doménové poskytujícího přidá] [ adds-resource-forest] v Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Architektura identity cloudu pomocí služby Azure Active Directory"
[1]: ./media/guidance-identity-aad/figure2.png "Jeden struktury, jedna topologie AAD adresáře"
[2]: ./media/guidance-identity-aad/figure3.png "Více strukturami, jednoho topologie AAD adresáře"
[4]: ./media/guidance-identity-aad/figure5.png "Topologie pracovní serveru"
[5]: ./media/guidance-identity-aad/figure6.png "Více topologie AAD adresáře"
[6]: ./media/guidance-identity-aad/figure7.png "Výběr vlastní instalace Azure AD připojení synchronizovat s konkrétní instanci systému SQL Server"
[7]: ./media/guidance-identity-aad/figure8.png "Určení metody jednotného přihlašování pro přihlášení uživatele"
[8]: ./media/guidance-identity-aad/figure9.png "Zadání domény a OU možnosti filtrování"
[9]: ./media/guidance-identity-aad/figure10.png "Povolení heslo zpětného zápisu"
[10]: ./media/guidance-identity-aad/figure11.png "Správa zásuvné Azure AD na portálu"
[11]: ./media/guidance-identity-aad/figure12.png "Konzole Azure AD Connect"
[12]: ./media/guidance-identity-aad/figure13.png "Karta operace v okně Správce služeb synchronizace"
[13]: ./media/guidance-identity-aad/figure14.png "Karta spojnic ve Správci služby synchronizace"
[14]: ./media/guidance-identity-aad/figure15.png "Editor pravidel synchronizace"
[15]: ./media/guidance-identity-aad/figure16.png "Zásuvné Azure Active Directory připojení stavu na portálu Azure zobrazující stav synchronizace"
[16]: ./media/guidance-identity-aad/figure17.png "Zásuvné Azure Active Directory připojení stavu na portálu Azure zobrazující stav služby AD DS"
[17]: ./media/guidance-identity-aad/figure18.png "Zabezpečení sestav dostupných na portálu Azure"
[18]: ./media/guidance-identity-aad/figure19.png "Portál Azure zvýraznění veřejnou IP adresu Vyrovnávání zatížení web vrstvy"
[19]: ./media/guidance-identity-aad/figure20.png "V Internet Exploreru umožňuje přecházet na veřejnou IP adresu Vyrovnávání zatížení web vrstvy"
[20]: ./media/guidance-identity-aad/figure21.png "Certifikáty modulu snap-in zobrazující www.contoso.com certifikátu"
[21]: ./media/guidance-identity-aad/figure22.png "Připojení k řízení osy OM"
[22]: ./media/guidance-identity-aad/figure23.png "Vytvoření vazby HTTPS pro výchozí web"
[23]: ./media/guidance-identity-aad/figure24.png "Vytvoření webové aplikace ContosoWebApp1"
[24]: ./media/guidance-identity-aad/figure25.png "Nastavení vlastností ověřování ContosoWebApp1 webové aplikace"
[25]: ./media/guidance-identity-aad/figure26.png "Přihlášení k Azure AAD z ContosoWebApp1 webové aplikace"
[26]: ./media/guidance-identity-aad/figure27.png "Změna složky pro výchozí web"