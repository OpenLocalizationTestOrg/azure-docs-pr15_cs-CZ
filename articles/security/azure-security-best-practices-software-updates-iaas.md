<properties
   pageTitle="Doporučené postupy pro Software aktualizacích v blogu o Microsoft Azure IaaS | Microsoft Azure"
   description="Článek obsahuje kolekce doporučené postupy pro aktualizace softwaru v prostředí Microsoft Azure IaaS.  Je určená pro odborníky a analytiky zabezpečení, kteří se zabývají změnit ovládací prvek, software aktualizace a správa majetku denně, včetně těch pověřený úsilí zabezpečení a dodržování předpisů jejich organizace."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Doporučené postupy pro software aktualizacích v blogu o Microsoft Azure IaaS

Před začte jakýkoli druh diskuse o doporučených postupech pro prostředí Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) , je důležité pochopit scénáře jaké, která se mají se správou aktualizace softwaru a odpovědností. Následující diagram potřebné informace vyhledat v pochopit tato omezení:

![Modely cloudu a povinnostmi](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Sloupec úplně vlevo zobrazuje sedm povinnosti (definované v následujících částech), že organizace měli vzít v úvahu, které přispívají k zabezpečení a ochranu osobních údajů výpočetního prostředí.
 
Data klasifikaci a zodpovědnost a ochranu klienta a koncovou odpovědností, které jsou výhradně v doméně zákazníků a fyzické, Host (hostitel) a síťové povinnosti v doméně poskytovatele služeb cloudu v PaaS a SaaS modely. 

Zbývající povinnosti jsou sdíleny zákazníci i cloudových poskytovatele služeb. Některé povinnosti vyžadují CSP a zákazník ke správě odpovědnost dohromady, včetně auditování své domény. Například zvažte Identity a přístup k řízení při použití Azure Active Directory Services; Konfigurace služeb, jako je vícefaktorové ověřování až zákazníka, ale zajištění efektivní funkce zodpovědnost Microsoft Azure.

> [AZURE.NOTE] Další informace o sdíleném povinnosti v cloudu přečtěte si [Sdílené povinnosti výpočetních cloudu](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Tyto stejné zásady použití ve scénáři hybridní, pokud vaše společnost používá VMs IaaS Azure, který komunikovat s místní zdroje uvedeno v diagramu pod.

![Typické hybridní scénáře s Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Zhodnocení

I když vaše společnost používá systému správy aktualizace a už máte zásady aktualizace softwaru na místě, je důležité často návštěvě předchozí zásad hodnocení a aktualizovat je aktuální požadavkům. To znamená, že budete muset znát aktuální stav zdroje ve vaší firmě. Kontaktovat tento stav, je třeba vědět:

-   Pole fyzicky a virtuálních počítačích ve vašem podniku.

-   Operační systémy a verzí na každý z těchto fyzické a virtuálních počítačích.

-   Aktualizace softwaru nainstalovaný na každém počítači (service pack verze aktualizace softwaru a další úpravy).

-   Funkce každý počítač provádí ve vašem podniku.

-   Aplikace a aplikace spuštěné na každém počítači.

-   Vlastnictví a kontaktní informace pro každý počítač.

-   V prostředí a jeho relativní hodnotu určit oblasti, ve kterých je třeba největší pozornost a ochrana indexcolumnstable nahrazuje aktiva.

-   Zabezpečení známé problémy a procesy vaše společnost používá na místě identifikační nové potíže se zabezpečením nebo změny v úroveň zabezpečení.

-   Opatření, které již byly nasazeny zajistit prostředí.

Byste měli pravidelně aktualizovat tyto informace a by měl být snadno dostupné zahrnuté do svého proces správy aktualizace softwaru.

## <a name="establish-a-baseline"></a>Nastavit směrný plán

Důležitou součástí proces správy aktualizace softwaru je ve vašem podniku; vytvoření počáteční instalace standardní verze operačního systému, aplikací a hardware pro počítače nazývají směrné plány. Konfigurace produktů nebo systému podle času v určitém místě je směrný plán. Aplikace nebo operační systém podle směrného plánu, například umožňuje sestavit počítače nebo služby do určité stavu.

Směrné plány základem najít a opravit problémy a zjednodušit proces správy aktualizace softwaru omezit tak počet aktualizace softwaru, který je třeba nasadit ve vašem podniku i zvýšením možnost sledování dodržování předpisů.

Po dokončení počáteční auditování vašeho podniku, byste měli použít informace, které pochází z auditování definovat provozní podle směrného plánu pro IT součásti uvnitř provozním prostředí. Počet směrných plánů můžou být nutné, v závislosti na různé typy hardware a software nasadit do výroby.

Například některé servery vyžadují aktualizaci a zabránit tak předsazení zadávání vypínání při spuštění systému Windows Server 2012. Směrný plán těchto serverech by měl obsahovat tato aktualizace softwaru.

Ve velkých organizacích je často vhodné rozdělit kategorie aktiv počítačů v organizaci a uchovávají pro každou kategorii standardní směrný plán tak, že používáte stejnou verzi softwaru a aktualizace softwaru. Pak můžete tyto materiály kategorie v priority aktualizace distribuci softwaru.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Přihlášení k odběru služeb příslušný software a otevřel aktualizace oznámení

Po provedení počáteční auditování software používat ve vašem podniku, by měl určete takovou metodu pro příjem oznámení o nových aktualizacích softwaru pro každý produkt software a verze. V závislosti na produktu může být nejlepší způsob oznámení e-mailová oznámení, weby nebo počítač publikací.

Například Centrum odpověď zabezpečení Microsoft (Nejčastější) odpovídá všechny související se zabezpečením pochybnosti o produktech společnosti Microsoft a poskytuje službu zabezpečení aplikace Microsoft Bulletin, bezplatná e-mailová oznámení o nově identifikované chyby a aktualizace softwaru vydané při řešení těchto chyb. Se můžete přihlásit k odběru tuto službu v http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Důležité informace o aktualizaci software

Po provedení počáteční auditování software používat ve vašem podniku, měli byste určit požadavky nastavit váš software aktualizace systému řízení, který závisí na systému řízení aktualizace software, který používáte. WSUS přečtěte si [Doporučené postupy služby systému Windows Server aktualizace](https://technet.microsoft.com/library/Cc708536)pro System Center naleznete v [Plánování aktualizace softwaru ve Správci konfigurace](https://technet.microsoft.com/library/gg712696).

Můžou ale nastat některé důležité obecné informace a doporučené postupy, které můžete použít bez ohledu na řešení, které používáte, jak je vidět v částech, který následuje.

### <a name="setting-up-the-environment"></a>Nastavení prostředí

Zvažte následující postupy při plánování nastavení softwaru aktualizovat správy prostředí:

-   **Vytvořit výrobní software aktualizace kolekce na základě stabilní kritérií**: obecně vytvářet kolekce pro váš software aktualizace stavu skladových zásob a distribuce kritériem stabilní vám pomůže zjednodušit všechny stadií proces správy aktualizace softwaru. Stabilní kritéria může zahrnovat nainstalované klientské operační systém verze a úroveň aktualizací service pack, role systému nebo cílové organizaci.

-   **Vytvořit před výrobní kolekce, které zahrnují odkaz počítače**: kolekce před výrobní by měl obsahovat zástupce konfigurace verze operačních systémů, řádek softwaru business a jiného softwaru spuštěné ve vašem podniku.

Zvažte taky kde serveru aktualizace softwaru budou umístěné, pokud bude v Azure IaaS infrastruktury v cloudu nebo pokud je místní. Důvodem je důležité rozhodnutí potřebujete zjistit množství přenosů mezi místních zdrojů a Azure infrastrukturu. Další informace o tom, jak připojit infrastrukturu místní Azure číst [připojit v místní síti k síti virtuální Microsoft Azure](https://technet.microsoft.com/library/Dn786406.aspx) .

Možnosti návrhu, které bude určovat, kdy budou umístěné aktualizovat server se také lišit podle toho, aktuální infrastrukturu a systém aktualizací softwaru, který právě používáte. WSUS číst [Nasazení systému Windows Server aktualizace služby ve vaší organizace](https://technet.microsoft.com/library/hh852340.aspx) a pro Centrum Správce konfigurace pro System číst [plánování webů a hierarchie ve Správci konfigurace](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Zálohování

Pravidelného zálohování jsou důležité nejen pro software aktualizace správy platformu samotné ale i k serverům, které se aktualizují. Organizacím, které mají [Změnit proces správy](https://technet.microsoft.com/library/cc543216.aspx) na místě budou vyžadovat IT zarovnání důvody proč serveru potřeba aktualizovat, odhadovaná výpadek služeb a možný dopad. Abyste měli jistotu, že máte konfigurace vrácení zpět na místě v případě, že aktualizace selže, zkontrolujte, že zálohujete systému.

Některé možnosti zálohování Azure IaaS patří:

-   [Azure ochranu pracovní zátěž IaaS pomocí Správce ochrana dat](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Obecnějším údajům Azure virtuálních počítačích](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Sledování

By měla běžet běžná sestavy sledování počet chybějící nebo nainstalovaný aktualizace nebo aktualizace se stavem neúplná pro každou aktualizaci software, který má oprávnění. Vytváření sestav pro aktualizace softwaru, které se dosud oprávnění Podobně mohou usnadnit snazší rozhodnutí nasazení.

Také je třeba zvážit následující úkoly:

-   Vedení auditu použitelné a nainstalované aktualizací pro všechny počítače ve vaší společnosti.

-   Povolte a nainstalovat do příslušné počítačů.

-   Sledovat zásoby a aktualizace stavu instalace a postup pro všechny počítače ve vaší společnosti.

Praktické cvičení navíc obecné důležité informace, které byly popsaných v tomto článku, zvažte také každý produkt je nejvhodnější, například: Pokud máte virtuálního počítače v Azure se serverem SQL Server, ujistěte se, že jsou následující doporučení aktualizací softwaru pro tento výrobek.

## <a name="next-steps"></a>Další kroky

Pomoc při určení nejlepší možností aktualizací softwaru pro virtuálních počítačích v Azure IaaS se řiďte pokyny popisované v tomto článku. Obsahuje mnoho podobnosti mezi software aktualizace doporučené postupy ve srovnání s Azure IaaS tradiční datacentru, proto doporučujeme vyhodnocení vaše aktuální software aktualizace zásady obsahuje Azure VMs a zahrnout relevantní osvědčené postupy v tomto článku celkový proces aktualizace softwaru.
