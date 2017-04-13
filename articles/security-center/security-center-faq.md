<properties
   pageTitle="Centrum zabezpečení Azure časté otázky ke | Microsoft Azure"
   description="V tomto článku najdete odpovědi na otázky týkající se Azure centra zabezpečení."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Centrum zabezpečení Azure nejčastější dotazy

V tomto článku najdete odpovědi na otázky týkající se Centra zabezpečení Azure, služba, která pomáhá zabránit, zjišťování a odpovědět hrozeb lepší viditelnost do a publikum nemůže ovládat zabezpečení Microsoft Azure zdroje.

## <a name="general-questions"></a>Obecné otázky

### <a name="what-is-azure-security-center"></a>Co je Centrum zabezpečení Azure?
Centrum zabezpečení Azure pomáhá zabránit, zjišťování a odpovědět hrozeb lepší viditelnost do a publikum nemůže ovládat zabezpečení Azure prostředků. Poskytuje integrované zabezpečení sledování a zásady správy v rámci předplatného, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

### <a name="how-do-i-get-azure-security-center"></a>Jak se dostanu Centrum zabezpečení Azure?
Centrum zabezpečení Azure povolit k vašemu předplatnému Microsoft Azure nebo k nim získat přístup z [Azure portálu](https://azure.microsoft.com/features/azure-portal/). ([Přihlášení k portálu](https://portal.azure.com), vyberte **Procházet**a přejděte na **Centrum zabezpečení**).  

## <a name="billing"></a>Fakturace

### <a name="how-does-billing-work-for-azure-security-center"></a>Jak funguje fakturace pracovní zátěž: Centrum zabezpečení Azure?
Centrum zabezpečení je k dispozici v dvou úrovní: bezplatné a standardní.

Bezplatné osy umožňuje nastavit zásady zabezpečení a přijímat upozornění zabezpečení, událostí a doporučení, které by vás provede procesem konfigurace potřebných ovládacích prvků. Pomocí bezplatné osy můžete také sledovat stav zabezpečení partnerských řešení integrovaný s Azure předplatné a Azure zdroje.

Standardní osy poskytuje zjišťování rozšířenou plus bezplatné osy: ochrany proti novým ohrožením měřítka, chování analýzy, dojde k chybě analýzy a zjišťování odchylky. Neexistuje bezplatnou zkušební verzi standardní osy 90 dnů. Pokud chcete upgradovat, vyberte ceny osy v [zásady zabezpečení](security-center-policies.md#setting-security-policies-for-subscriptions). Další informace najdete v článku [ceny Centrum zabezpečení](security-center-pricing.md) .

## <a name="data-collection"></a>Shromažďování dat

Centrum zabezpečení shromažďuje údaje o virtuálních počítačích k posouzení stavu zabezpečení, poskytovat doporučení týkající se zabezpečení a upozornění na hrozeb. Při prvním přístup k Centru zabezpečení, shromažďování dat povolený na všechny virtuálních počítačích ve vašem předplatném. Shromažďování dat je vhodné, ale je můžete vyjádření výslovného nesouhlasu [Zakázání shromažďování dat](#how-do-i-disable-data-collection) v Centru zabezpečení zásadách.

### <a name="how-do-i-disable-data-collection"></a>Jak můžu vypnout shromažďování dat?

**Shromažďování dat** můžete zakázat pro přihlášení k odběru v zásady zabezpečení kdykoli. ([Přihlásit se k portálu Azure](https://portal.azure.com), vyberte **Procházet**, vyberte **Centrum zabezpečení**a vyberte **zásadu**.)  Při výběru předplatného nové zásuvné otevře a nabízí možnost vypnout **shromažďování dat** . Vyberte požadovanou možnost **Odstranění agentů** na horním pásu karet odebrat agentů z existující virtuálních počítačích.

> [AZURE.NOTE] Zásady zabezpečení lze nastavit na úrovni Azure předplatné a úrovně seskupení zdroje, ale je nutné vybrat předplatné shromažďování dat vypnout.

### <a name="how-do-i-enable-data-collection"></a>Jak povolit shromažďování dat?
Shromažďování dat můžete povolit pro vašich Azure předplatných zásady zabezpečení. Chcete-li povolit shromažďování dat, [Přihlaste se k portálu Azure](https://portal.azure.com), vyberte **Procházet**, vyberte **Centrum zabezpečení**a vyberte **zásadu**. Nastavení **shromažďování dat** na **zapnuto** a konfigurace účtů úložiště místo, kam chcete data, která chcete shromažďovat k (zobrazit dotaz "[kam se ukládají Moje data?](#where-is-my-data-stored)"). Při zapnuté funkci **shromažďování dat** , automaticky shromažďuje informace o konfiguraci a událostí zabezpečení ze všech podporovaných virtuálních počítačích v předplatného.

> [AZURE.NOTE] Nastavit zásady zabezpečení na úrovni Azure předplatné a úrovně seskupení zdrojů dochází ke konfiguraci shromažďování dat na úrovni předplatného.

### <a name="what-happens-when-data-collection-is-enabled"></a>Co se stane, když je povolený shromažďování dat?
Shromažďování dat je povoleno prostřednictvím agenta sledování Azure a rozšíření sledování zabezpečení Azure. Rozšíření sledování zabezpečení Azure vyhledává různých relevantní nastavení zabezpečení a odešle do trasování [Trasování událostí pro systém Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (trasování událostí pro Windows). Kromě toho operační systém vytvoří položky v protokolu událostí.  Agent sledování Azure přečte položky protokolu událostí a trasování událostí pro Windows sleduje a zkopíruje ke svému účtu úložiště pro analýzu.  Jedná se o ukládání účet, který jste nakonfigurovali ve zásady zabezpečení. Další informace o účtu úložiště najdete v tématu otázku "[kam se ukládají Moje data?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Sledování Agent nebo sledování zabezpečení e-mailem má vliv na výkon Moje servery?
Agent a rozšíření spotřebovává nominal počtu systémové prostředky a měli malé dopad na výkon.

### <a name="where-is-my-data-stored"></a>Kam se ukládají Moje data?
Pro každou oblast, ve kterém máte virtuálních počítačích systém zvolte úložiště účet, kde je uložena data shromážděná z těchto virtuálních počítačích. Usnadňuje můžete uchovávat data v stejné zeměpisné pro účely svrchovanost data a osobní údaje. Zásady zabezpečení zvolte účtu úložiště pro předplatné. ([Přihlásit se k portálu Azure](https://portal.azure.com), vyberte **Procházet**, vyberte **Centrum zabezpečení**a vyberte **zásadu**.) Po kliknutí na předplatné, otevře se nové zásuvné. Vyberte **Zvolit úložiště účty** vyberte oblast.

> [AZURE.NOTE] Nastavit zásady zabezpečení na úrovni Azure předplatné a úrovně seskupení zdrojů dochází ke výběru oblasti účtu úložiště na úrovni předplatného.

Další informace o Azure úložiště a úložiště účty, najdete v článku [Si přečtěte následující dokumentaci úložiště](https://azure.microsoft.com/documentation/services/storage/) a [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Použití Centra zabezpečení Azure

### <a name="what-is-a-security-policy"></a>Jaké jsou zásady zabezpečení?
Zásady zabezpečení definuje sadu ovládacích prvků, které jsou vám doporučené zdrojů v zadaném předplatné nebo skupina zdroje. V Centru zabezpečení Azure definování zásad pro Azure předplatná a skupin zdrojů podle toho, požadavky na zabezpečení vaší společnosti a typ aplikace nebo citlivosti dat v každé předplatného.

Například prostředky pro vývoj nebo testování pravděpodobně požadavky na zabezpečení jiné než pro výrobní aplikace. Podobně aplikací s regulovaném dat jako Umožňujících (určitelné osobní údaje) může vyžadovat do vyšší úrovně zabezpečení. Zásady zabezpečení v Centru zabezpečení Azure povolený bude řídit zabezpečení doporučení a sledování. Další informace o zásady zabezpečení najdete v tématu [sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md).

> [AZURE.NOTE] V případě ke konfliktu mezi předplatné úrovně zásady a zdroje skupiny úrovně úrovní zásad skupiny zdrojů přednost.

### <a name="who-can-modify-a-security-policy"></a>Kdo může upravit zásady zabezpečení?
Konfigurace zásad zabezpečení pro každého předplatného nebo skupina zdroje. Chcete-li změnit zásady zabezpečení na úrovni předplatného nebo na úrovni skupiny zdrojů, musíte být vlastníkem nebo Přispěvatel, které předplatné.

Další informace o konfigurace zásad zabezpečení, viz [nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Co je zabezpečení doporučení?
Centrum zabezpečení Azure analyzuje stav zabezpečení Azure prostředků. Když jsou určeny potenciální zabezpečením, doporučení vytvářejí. Doporučení vás provede procesem konfigurace potřebné ovládacího prvku. Příklady:

- Zřizování antimalware pomůže identifikovat a odebrání škodlivým softwarem
- Konfigurace [Skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) a pravidla pro ovládací prvek přenosy na virtuálních počítačích
- Zřízení brány firewall webové aplikace k chránit před útoky zacílení webových aplikací
- Nasazení chybějící aktualizace systému
- Adresování konfigurace OS, které neodpovídají doporučené směrných plánů

Tady jsou zobrazeny pouze doporučení, které jsou povolené v zásady zabezpečení.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Jak můžu zobrazit aktuální stav zabezpečení Azure zdrojů?
Na dlaždici **prostředky stavu** na zásuvné **Centrum zabezpečení** zobrazuje celkové zabezpečení postoje prostředí rozdělené podle virtuálních počítačích, webových aplikací a další zdroje informací. Jednotlivé zdroje má indikátor zobrazení, pokud identifikovali všech možných zabezpečením. Kliknutí na dlaždici prostředky stavu zobrazí zdroje a identifikuje tam, kde se vyžaduje pozornost nebo se mohou vyskytnout problémy.

### <a name="what-triggers-a-security-alert"></a>Jaký vyvolá výstrahy zabezpečení?
Centrum zabezpečení Azure automaticky shromažďují analyzuje a fuses protokolu dat z Azure zdrojů, sítě a partnerských řešení jako antimalware a brány firewall. Když nejsou nalezeny hrozeb, vytvoří se výstrahy zabezpečení. Jako příklad lze uvést zjišťování:

- Hostují virtuálních počítačích komunikaci s známé škodlivým IP adres
- Rozšířené malware detekovaný pomocí zasílání zpráv o chybách systému Windows
- Přímým útokům proti virtuálních počítačích
- Výstrahy zabezpečení integrovaných partnerských řešení zabezpečení proti malwaru ATP bránách firewall webové aplikace

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Jaký je rozdíl mezi hrozeb zjišťování a upozornění na použít Microsoft Security Response Center versus Centrum zabezpečení Azure?
Centrum pro odpověď zabezpečení Microsoft (Nejčastější) provádí vyberte zabezpečení sledování Azure sítě a infrastruktury volání a přijímání hrozbou, že měřítka a zneužívání stížností od třetích stran. Pokud Nejčastější zjistí, že data o zákaznících přístup neoprávněným nebo neoprávněným strana nebo že zákazníka využívání Azure není v souladu s podmínkami vzhledem ke přijatelná, incidentu správce zabezpečení upozorněním zákazníka. Oznámení obvykle způsobeno pošlete e-mailovou zabezpečení kontaktů podle Centrum zabezpečení Azure nebo vlastník Azure předplatného, pokud není zadán zabezpečení kontakt.

Centrum zabezpečení je Azure služba, která sleduje Azure prostředí zákazníka neustálé platí analýzy automaticky zjišťovat širokou škálu potenciálně nebezpečný aktivity. Tyto zjišťování jsou vytažená jako výstrahy zabezpečení v řídicím panelu Centra zabezpečení.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Jak fungují oprávnění v Centru zabezpečení Azure?
Centrum zabezpečení Azure podporuje přístupu na základě rolí. Další informace o řízení přístupu na základě rolí (RBAC) v Azure najdete v tématu [Řízení přístupu na základě rolí Azure Active Directory](../active-directory/role-based-access-control-configure.md).

Při otevření Centra zabezpečení, doporučení pouze a zobrazí upozornění, které se vztahují k prostředky, které má uživatel přístup k. To znamená, že uživatelé uvidí jenom položky vztahující se ke zdroji kde uživatel je role vlastníka, Přispěvatel nebo čtenáře předplatné nebo skupina zdroje, které zdroj patří.

Pokud potřebujete:

- **Úprava zásady zabezpečení**, musíte být vlastníkem nebo přispěvatelů předplatného.
- **Použít doporučení**, musíte být vlastník nebo Přispěvatel předplatného.
- **Mít přehled o stavu zabezpečení ve všech předplatných**, musí být vlastníka, Přispěvatel nebo čtenáře (správce IT, zabezpečení týmu) každého předplatného.
- **Mít přehled o stavu zabezpečení prostředků**, musí být zdrojů skupina vlastník skupiny přispěvatelů nebo Reader (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Které Azure zdroje jsou kontroloval Centrum zabezpečení Azure?
Centrum zabezpečení Azure sleduje Azure následující zdroje:

- Virtuálních počítačích (VMs) (včetně [Cloud Services](../cloud-services/cloud-services-choose-me.md))
- Azure virtuálních sítí
- Služba SQL Azure
- Integrovaná s předplatným Azure například web aplikace bránu firewall na VMs a [Aplikace služeb prostředí](../app-service/app-service-app-service-environments-readme.md) partnerských řešení

## <a name="virtual-machines"></a>Virtuálních počítačích

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Jaké typy virtuálních počítačích budou podporované?
Sledování stavu zabezpečení a doporučení jsou k dispozici pro virtuálních počítačích (VMs) vytvořené pomocí obou [klasické a modelů nasazení Správce prostředků](../azure-classic-rm.md).

Podporované Windows VMs:

- Windows Server 2008 R2
- Windows Server 2012
- Windows serveru 2012 R2

Podporované Linux VMs:

- Se systémem Ubuntu verze 12.04, 14.04, 16.04
- Debian verze 7, 8
- CentOS verze 6. \*, 7.*
- Červené klobouk Enterprise Linux (RHEL) verze 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) verze 11. \*, 12.*
- Oracle Linux verze 6. \*, 7.*

VMs spuštěné v cloudové službě jsou taky podporované. Web a pracovní role aplikaci výrobní, které jsou sledovány sloty služby pouze cloudu. Další informace o cloudové služby, najdete v článku [Přehled Cloudovým službám](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Proč Centrum zabezpečení Azure nerozpozná řešení antimalware spuštěna Moje OM Azure?

Centrum zabezpečení Azure pouze obsahuje přehled o antimalware nainstalovaný prostřednictvím Azure rozšíření. Například Centrum zabezpečení nedokáže zjišťování antimalware, která byla předinstalovaná na obrázek, který jste zadali nebo pokud jste nainstalovali antimalware virtuální počítače pomocí vlastní procesy (například systémy řízení konfigurace).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Proč dostávám zprávu "Chybějící skenování Data" pro svůj OM?

Může trvat delší dobu (obvykle menší než jedna hodina) prohledávání dat k naplnění povolíte shromažďování dat v Centru zabezpečení Azure. Prohledávání nebude naplnění pro VMs zastaveno stav.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Proč dostávám zprávu "OM Agent je chybějící?"

Agent OM musí být nainstalovaný na VMs za účelem povolení shromažďování dat. Agent OM nainstalovaný ve výchozím nastavení pro VMs, které jsou nasazeny z Azure Marketplace. Další informace o tom, jak nainstalovat jiné VMs agenta OM, najdete v blogovém příspěvku [OM Agent a rozšíření](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
