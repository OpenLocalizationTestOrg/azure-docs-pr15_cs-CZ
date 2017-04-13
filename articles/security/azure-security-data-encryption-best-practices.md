<properties
   pageTitle="Doporučené postupy pro zabezpečení dat a šifrování | Microsoft Azure"
   description="Tento článek obsahuje sadu doporučené postupy pro zabezpečení dat a šifrování pomocí integrované Azure možnosti."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Doporučené postupy pro zabezpečení Azure dat a šifrování

Jedna z kláves ochranu dat v cloudu je zúčtování možné stavy může dojít, dat a jaké ovládací prvky jsou k dispozici pro tento stav. Pro účely Azure dat zabezpečení šifrování doporučené postupy a doporučení bude kolem státy následující údaje:

- V ostatních: Jedná se o všechny informace, které úložiště objektů, kontejnerů a typy, které jsou staticky na fyzické médiu, bude magnetické nebo optického disku.

- Na cestě: Když data přenášena mezi součásti, umístění nebo programy, jako je třeba v síti přes bus služby (z místních do cloudu a naopak, včetně hybridní připojení například ExpressRoute), nebo během vstupní a výstupní, to je představit jako v pohybu.

V tomto článku se probereme tato sady dat Azure zabezpečení a šifrování doporučené postupy. Tyto doporučené postupy jsou odvozeny od Naše zkušenosti s zabezpečení Azure dat a šifrování a prostředí zákazníků sami.

Pro každý doporučený postup budete popisují jsme:

- Jaký je doporučený postup
- Proč chcete povolit tento doporučený postup
- Pokud se vám nepovede povolit doporučený postup co může být výsledek
- Možné alternativ doporučený postup
- Jak se dozvíte, chcete-li povolit doporučený postup

Tento článek zabezpečení dat Azure a osvědčené postupy šifrování se podle shody názoru a Azure platformu funkce a funkce sady platných v době, kdy napsané v tomto článku. Názory a technologií pro server v průběhu času mění a v tomto článku se aktualizují v pravidelných intervalech, aby odrážely provedené změny.

Azure dat zabezpečení a šifrování doporučené postupy popisované v tomto článku, patří:

- Vynutit vícefaktorové ověřování
- Na základě rolí pomocí řízení přístupu (RBAC)
- Šifrování Azure virtuálních počítačích
- Použití modely zabezpečení hardwaru
- Správa pracovních zabezpečené míst
- Povolit šifrování dat SQL
- Chránit data při přenosu šifrovaná
- Vynucení šifrování dat úrovně souborů


## <a name="enforce-multi-factor-authentication"></a>Vynutit vícefaktorové ověřování

Cílem prvního kroku přístup k datům a ovládací prvek v Microsoft Azure je k ověření uživatele. [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) je způsob, jak ověřit identitu uživatele jiným způsobem než jenom uživatelské jméno a heslo. Tento ověřování metoda pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení.

Povolením Azure MFA pro uživatele, který přidáváte druhý vrstvy cenného papíru uživatele přihlášení, začátky transakce. V tomto případě transakce může být přístupu k dokumentu nachází na souborovém serveru nebo na svůj Sharepointu Online. Azure MFA taky pomůže IT zmenšit pravděpodobnost, že hostují pověření bude mít přístup k datům organizace.

Příklad: Pokud vynutit Azure MFA pro uživatele a nakonfigurujte ji telefonní hovor nebo textovou zprávu jako použít ověřování, pokud je ohroženo přihlašovací údaje uživatele, mohl nebudou mít přístup k všechny zdroje, protože mu nebudou mít přístup k telefonu uživatele. Organizacím, které se přidat tento další úroveň ochrany identity jsou větší pravděpodobnost útok krádež přihlašovacích údajů, což může vést k narušení dat zabezpečení.

Jedna alternativa v organizacích, které chcete zachovat ověřování ovládací prvek místní je použití [Azure Multi-Factor ověřování serveru](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), neboli MFA místní. Tímto způsobem se budou pořád moct vynutit vícefaktorové ověřování a zachovat přitom MFA místním serveru.

Další informace o Azure MFA přečtěte si článek [Začínáme s Azure Vícefaktorové ověřování v cloudu](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Na základě rolí pomocí řízení přístupu (RBAC)
Omezení přístupu na základě zásad zabezpečení [znát](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižších možných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . Toto je nezbytné v organizacích, které chcete musí vynucovat nějaké zásady zabezpečení pro přístup k datům. Azure na základě rolí přístup ovládacího prvku (RBAC) mohou sloužit k přiřazení oprávnění uživatelů, skupin a aplikací na určitý obor. Obor přiřazování rolí můžou být předplatné, skupina zdroje nebo jeden zdroj.

Můžete využít [předdefinované RBAC role](../active-directory/role-based-access-built-in-roles.md) v Azure přiřadit oprávnění uživatelům. Zvažte využití *Úložiště účtu přispěvatelů* pro cloudu operátory, které potřebujete ke správě účtů úložiště a role *Klasické Přispěvatel účtu úložiště* ke správě účtů klasické úložiště. Pro cloudu operátory, které je potřeba spravovat VMs a účtu úložiště můžete do ní přidat role *Přispěvatele virtuálního počítače* .

Organizacím, které nejsou vynutit data řízení přístupu na základě funkce, například RBAC může poskytuje další oprávnění než potřebné pro své uživatele. Může vést k narušení zabezpečení dat tím, že někteří uživatelé, kteří mají přístup k datům, která by neměli mít na prvním místě.

Další informace o Azure RBAC můžete přečíst v článku [Řízení přístupu Azure Role-Based](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Šifrování Azure virtuálních počítačích
Pro mnoho organizace je [šifrování dat na zbývající](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) povinné krokem k dat ochrany osobních údajů a dodržování předpisů zůstala zachovaná svrchovanost data. Azure šifrování disku umožňuje správcům IT šifrování disků Windows a Linux IaaS virtuálního počítače (OM). Azure šifrování disku využívá odvětví standardní nástroje BitLocker funkce systému Windows a DM Crypt Linux poskytnout hlasitost šifrování operačního systému a disků data.

Můžete využít šifrování disku Azure k ochraně a chránit data splňují organizační zabezpečení a dodržování předpisů požadavky. Organizace byste taky měli vědět pomocí šifrování související s neoprávněným datové rizika zmírnit. Doporučujeme také šifrování jednotky před psaní citlivá data do nich.

Zkontrolujte, že šifrování vaší OM objemů dat a spuštění hlasitost Pokud chcete chránit data na ostatních ve vašem účtu Azure úložiště. Zajištění šifrovací klíče a tajemství využitím [Azure klíč trezoru](../key-vault/key-vault-whatis.md).

Místní servery systému Windows zvažte následující šifrování doporučené postupy:

- Použití [nástroje BitLocker](https://technet.microsoft.com/library/dn306081.aspx) pro šifrování
- Obsahují informace o obnovení ve službě AD DS.
- Pokud tam je všechny klíčů nástroje BitLocker mít ohroženo, doporučujeme buď formátování jednotky odebrat všechny výskyty metadata nástroje BitLocker z disku nebo dešifrování a znovu zašifrovat celou jednotku.

Organizacím, které nejsou vynucení šifrování velmi pravděpodobně vystaven otázek integrity dat, například škodlivý nebo rogue uživatelů krádeže dat a ohroženo účty neoprávněnému přístupu k datům v Vymazat formátování. Kromě těchto riskantní musí firmy, které mají dodržovat předpisy odvětví prokázat, že jsou důslednost a pomocí ovládacích prvků správné zabezpečení zabezpečit data.

Další informace o šifrování disku Azure přečíst v článku [Azure disku šifrování pro Windows a Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Použití hardwarové zabezpečení moduly

Řešení pro šifrování odvětví klávesami skrytou šifrování data. Proto je důležité, jsou klávesy bezpečně uloženy. Správy klíčů bude nedílnou součástí ochranu dat, protože ho bude využít k ukládání tajné klíčů, které se používají k šifrování data.

Šifrování Azure disku používá [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/) vám pomůže určit a Správa šifrovacího klíče disku a tajemství předplatné klíčové trezoru zároveň zajistit, že jsou všechna data v disků virtuálního počítače šifrované v klidu v úložišti Azure. Měli byste použít Azure klíč trezoru mají být auditovány klíče a použití zásad.

Existuje mnoho rizika spojená související s nemají odpovídající zabezpečení ovládacích prvků pro ochranu tajné klíče, které jste použili k šifrování vaše data. Pokud útočníků mít přístup k tajné klíče, bude moct dešifrování data a potenciálně mít přístup na důvěrné informace.

Další informace o obecné doporučení pro Správa certifikátů v Azure přečíst v článku [Správa certifikátů v Azure: pokyny, jak máte a nemáte postupovat](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Další informace o trezoru klíč Azure přečtěte si [začít pracovat s Azure klíč trezoru](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Správa pracovních zabezpečené míst

Vzhledem k tomu, že většina útoků zacílit koncového uživatele, se změní na koncový bod jednu primární bodů útok. Pokud se zlými úmysly zneužije koncový bod, si můžete využít přihlašovací údaje uživatele pro získání přístupu k datům organizace. Většina útoky koncový bod jsou využít faktů koncoví uživatelé jsou správci v místním pracovní.

Tato rizika můžete zmenšit pomocí workstation zabezpečené správy. Doporučujeme použít [Pracovních míst privilegovaných aplikace Access (TLAPA)](https://technet.microsoft.com/library/mt634654.aspx) zmenšit napadení v pracovních míst. Tyto zabezpečené Správa pracovních míst můžete zmírnění některé z těchto útoky zajistil dat je bezpečnější. Zkontrolujte, že pomocí TLAPA stupňovat odolnost a uzamknout počítači. Toto je důležitým krokem k poskytování záruky důkladné zabezpečení pro citlivé účty, úkoly a ochrana dat.

Nedostatku koncový bod ochrany může vaše data umístit na rizika, zkontrolujte, že musí vynucovat nějaké zásady zabezpečení na všech zařízeních, které se používají k používání dat, bez ohledu na umístění dat (cloudu a místní).

Se dozvíte další informace o oprávněními k workstation tak, že v článku [Zabezpečení privilegovaných přístup](https://technet.microsoft.com/library/mt631194.aspx)pro čtení.

## <a name="enable-sql-data-encryption"></a>Povolit šifrování dat SQL

[Šifrování databáze SQL Azure průhledné dat](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) pomáhá chránit před hrozbou, že škodlivým aktivitám tak, že provedete v reálném čase šifrování a dešifrování databáze, přidružené zálohování a souborů protokolu transakce v klidu bez nutnosti změny aplikace.  TDE šifruje úložiště pro celou databázi pomocí symetrickou klíče s názvem šifrovací klíč databáze.

I když je zašifrován celý úložiště, je velmi důležité také šifrování databáze samotné. Toto je implementace obrana při přístupu název hloubkové pro ochranu dat. Pokud používáte [Databázi SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) a chcete chránit citlivé data například platební kartou nebo čísla sociálního pojištění, můžete šifrování databáze pomocí FIPS 140-2 ověřit 256 AES šifrování splňující požadavky mnoha oborovými standardy (například HIPAA, PCI).

Je důležité pochopit, že nejsou při databáze je šifrovaná pomocí TDE šifrované soubory související s [vyrovnávací paměť fondu linky](https://msdn.microsoft.com/library/dn133176.aspx) (BPE). Je nutné použít šifrování na úrovni nástroje systému souborů jako nástroje BitLocker nebo [Šifrování systému souborů](https://technet.microsoft.com/library/cc700811.aspx) (EFS) BPE souvisejících souborů.

Od ověřený uživatel jako správce zabezpečení či správce databáze můžete získat přístup k datům i v případě, že databáze je šifrovaná s TDE, byste měli taky podle pokynů následující doporučení:

- Ověřování serveru SQL na úrovni databáze
- Ověřování Azure AD pomocí RBAC rolí
- Uživatelé a aplikací používejte samostatném účty ověření. Tímto způsobem můžete omezit vaše oprávnění uživatelů a aplikací a snížení rizik škodlivým aktivity
- Implementace databáze úrovně zabezpečení pomocí role pevných databáze (například db_datareader nebo db_datawriter), nebo můžete vytvořit vlastní role pro aplikaci k udělování explicitní oprávnění k vybrané databázových objektů

Organizacím, které nepoužíváte úrovně šifrování databáze může být větší pravděpodobnost pro útokům ohrožuje data umístěná v databázích SQL.

Další informace o šifrování SQL TDE přečíst v článku [Průhledné šifrování databáze SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Chránit data při přenosu šifrovaná

Ochrana dat při přenosu šifrovaná by měl být důležitou součástí strategie ochranu dat. Protože dat bude sebou poslalo Přesunované z mnoha umístění, obecné doporučujeme vždy použít protokol SSL/TLS protokoly pro výměnu dat na různých místech. V některých případech je vhodné izolovat kanálu celé komunikace mezi místním prostředím a cloudové infrastruktury pomocí virtuální privátní sítě (VPN).

Pro přechody mezi místním infrastrukturu a Azure data byste měli zvážit odpovídající záruky HTTPS ATP VPN.

V organizacích, které je potřeba zabezpečený přístup z více pracovních míst umístěn místní na Azure, použijte [Azure VPN k webu](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Pro organizace, které potřebují zajistit přístup z jednoho workstation nachází místní Azure, použijte [čárky sítě VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Větší datové sady přesouvat prostřednictvím vyhrazené vysokorychlostní sítě WAN linky například [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Pokud se rozhodnete sdělit nám ExpressRoute navíc dá zašifrovat data na úrovni aplikace pomocí [SSL/TLS](https://support.microsoft.com/kb/257591) nebo jiné protokoly pro Přidat zámek.

Pokud jsou interakce s úložištěm Azure pomocí portálu Azure, všechny transakce dojít prostřednictvím HTTPS. [Úložiště REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) přes protokol HTTPS lze také komunikovat s [Azure úložiště](https://azure.microsoft.com/services/storage/) a [Databáze SQL Azure](https://azure.microsoft.com/services/sql-database/).

Organizacím, které se nepodařilo chránit data při přenosu šifrovaná jsou větší pravděpodobnost pro [útoky člověka v – uprostřed](https://technet.microsoft.com/library/gg195821.aspx) [odposlechu](https://technet.microsoft.com/library/gg195641.aspx) a zneužití relace. Tyto útoky může být prvním krokem při přístupu k důvěrné údaje.

Další informace o možnost Azure VPN přečíst v článku [plánování a návrh VPN brány](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Vynucení šifrování dat úrovně souborů

Další úroveň ochrany, můžete zvýšit úroveň zabezpečení pro typ vašich dat je šifrování souboru, bez ohledu na umístění souboru.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) používá šifrování, identity a povolení zásady zabezpečení souborů a e-mailu. Azure RMS označené jako pracuje na několika zařízeních – telefonů, tabletů a počítačů ochranou ve vaší organizaci i mimo vaši organizaci. Tato možnost je možné, protože službou Azure RMS přidá úroveň ochrany zůstává s daty, i když ponechá hranice vaší organizace.

Pokud používáte službou Azure RMS Ochrana souborů, používáte standardní kryptografický se úplné podpory [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Když využít službou Azure RMS pro ochranu dat assurance, který ochrana zůstává se souborem, máte i v případě, že zkopírujete do úložiště, který není pod kontrolou IT, například službě cloudového úložiště. Stejný dojde k souborům sdíleným prostřednictvím e-mailu, soubor se po zamknutí jako přílohu na e-mailovou zprávu s pokyny, jak chráněné přílohu otevřít.

Při plánování službou Azure RMS zavádění doporučujeme takto:

- Nainstalujte si [RMS sdílení aplikace](https://technet.microsoft.com/library/dn339006.aspx). Tato aplikace lze integrovat s Office aplikací instalací aplikace Office doplněk tak, aby uživatelé mohli snadno chránit soubory přímo.
- Konfigurace aplikace a služby pro podporu službou Azure RMS
- Vytvoření [vlastní šablony](https://technet.microsoft.com/library/dn642472.aspx) , které zobrazují vašim požadavkům pro firmy. Příklad: šablony pro začátek skrytá data, použitý ve všech horní tajná související e-mailů.

Organizacím, které jsou slabými o ochraně [dat klasifikaci](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) a soubor může být více únik data. Bez správného soubor ochrany organizace nebude moct získat firmy přehledy, sledovat zneužívání a zabránit škodlivému přístupu k souborům.

Další informace o službou Azure RMS můžete přečíst v článku [Začínáme se službou Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
