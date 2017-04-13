<properties
   pageTitle="Přehled zabezpečení Azure virtuálních počítačích | Microsoft Azure"
   description=" Azure virtuálních počítačích co flexibilitě virtualizace aniž by bylo nutné koupit a spravovat fyzické hardwaru, které se spouští virtuální počítač.  Tento článek obsahuje přehled základních funkcí Azure zabezpečení, které lze použít s Azure virtuálních počítačích. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Přehled zabezpečení Azure virtuálních počítačích

Azure virtuálních počítačích umožňuje nasadit širokou škálu výpočetních řešení aktivní způsobem. S podporou společnosti Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP a služeb BizTalk Azure nástroje můžete nasazovat jakékoli pracovního vytížení a libovolný jazyk na téměř libovolném operační systém.

Azure virtuálního počítače vám umožní virtualizace aniž by bylo nutné koupit a spravovat fyzické hardwaru, které se spouští virtuální počítač.  Můžete vytvořit a nasazení aplikace s assurance, že vaše data jsou chráněné a bezpečné v naší vysoce zabezpečené datacentrech.

S Azure, což je možné vytvářet zabezpečením, kompatibilní se standardem řešení, která:

- Ochrana před viry a malwarem virtuálních počítačích
- Šifrování citlivá data
- Zabezpečení v síti
- Určení a zjistit hrozeb
- Dodržování předpisů

Hledání v tomto článku je uvádějí přehled základních funkcí Azure zabezpečení, které se dá používat se virtuálních počítačích. Také nabízíme odkazy na články poskytující podrobnosti každé funkce, které se můžou dozvědět víc.  

Základní funkce zabezpečení Azure virtuálního počítače vztahovat v tomto článku:

- Antimalware
- Hardwarová zabezpečení modulu
- Šifrování disku virtuálního počítače
- Zálohování virtuálního počítače
- Obnovení Azure webu
- Virtuální sítě
- Správa zásad zabezpečení a vytváření sestav
- Dodržování předpisů

## <a name="antimalware"></a>Antimalware

S Azure můžete software antimalware dodavatelům zabezpečení například Microsoft Symantec, Trend Micro, McAfee a Kaspersky chránit virtuálních počítačích z nebezpečného soubory, adware a jiných hrozeb. V tématu Další informace v části potřebujete najít článek partnera řešení.

Antimalware Microsoft Azure Cloudovým službám a virtuálních počítačích je funkce ochrany v reálném čase, která vám pomůže identifikovat a odebrání viry, spywarem a dalším škodlivým softwarem.  Microsoft Antimalware poskytuje konfigurovatelné upozornění při známo, že škodlivý nebo nežádoucí software pokusí nainstalovat nebo spustit v Azure sítě.

Microsoft Antimalware je agent jednoduchým řešení pro aplikace a prostředí klienta, která běží na pozadí bez zásahu člověka. Ochrana podle potřeby aplikace zatížení, se buď základní zabezpečené--standardně nebo Rozšířené vlastní konfigurace, včetně antimalware sledování nástroje můžete nasazovat.

Při nasazení a povolení Microsoft Antimalware tyto základní funkce jsou k dispozici:

- Ochrana v reálném čase - monitory aktivity v Cloudovým službám a na virtuálních počítačích ke zjištění a blokování spuštění malware.
- Naplánování prohledávání - pravidelně provede cílových virů zjišťování malware, včetně aktivně spuštěné programy.
- Malware remediation - automaticky převezme akce zjištěná malware, například odstranění nebo umístění do karantény nebezpečné soubory a čištění položky škodlivým registru.
- Aktualizace podpis – automaticky instalace nejnovějších podpisy protection (definice virů) zajistit ochranu je aktuální předem určených četnosti.
- Modul Antimalware – automaticky aktualizován aktualizuje modul Microsoft Antimalware.
- Platformu Antimalware – automaticky aktualizován aktualizuje platformu Microsoft Antimalware.
- Aktivní ochrana - sestav Azure telemetrie metadat o zjištěné hrozeb a podezřelé prostředky k zajištění rychlou odpověď a doručování v reálném čase synchronní podpis prostřednictvím aktivní ochranu systému (MAPY) společnosti Microsoft.
- Vzorky vykazování - poskytuje sestavách a vzorky ke službě Microsoft Antimalware k upřesnění službu a povolení Poradce při potížích.
- Vyloučení – umožňuje aplikace a Správce služby nakonfigurovat některé soubory, procesy a vede k vyloučit z protection a vyhledání výkonu a dalších důvodů.
- Shromažďování událostí Antimalware - zaznamenává stav služby antimalware, podezřelých aktivit a remediation akce v protokolu událostí operační systém a shromažďuje zohledňovala úložišti Azure zákazníka.

Další informace: Další informace o antimalware softwaru chránit virtuálních počítačích najdete v tématu:

- [Microsoft Antimalware pro Azure cloudovými službami a virtuálních počítačích](../security/azure-security-antimalware.md)
- [Nasazení řešení Antimalware na Azure virtuálních počítačích](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Jak nainstalovat a nakonfigurovat Trend Micro důkladné zabezpečení jako služba ve Windows OM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Jak nainstalovat a nakonfigurovat aplikaci Symantec Endpoint Protection na Windows OM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nové možnosti Antimalware chránících Azure virtuálních počítačích – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Řešení zabezpečení v Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardwarová zabezpečení modulu

Šifrování a ověřování nezlepší zabezpečení zamknutí samotné klíče. Správa a cenného kritický tajemství a klíče můžete zjednodušit tak, že je uložíte do Azure klíč trezoru. Klíč trezoru nabízí možnost uložení klíčů v hardwarové zabezpečení moduly (moduly hardwarového zabezpečení) oprávnění FIPS standardy úrovně 2 140-2. Vaše SQL Server šifrování klíče pro [šifrování průhledné dat](https://msdn.microsoft.com/library/bb934049.aspx) nebo zálohování můžete všechny ukládat do trezoru klíč s klíče či tajemství z aplikace. Oprávnění a přístup k těmto položkám chráněné se spravuje přes [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Víc se uč:

- [Co je Azure klíč trezoru?](../key-vault/key-vault-whatis.md)
- [Začínáme s trezoru klíč Azure](../key-vault/key-vault-get-started.md)
- [Azure klíč trezoru blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Šifrování disku virtuálního počítače

Azure šifrování disku je novou funkci, která umožňuje šifrování disků Windows a Linux Azure virtuálního počítače. Azure šifrování disku využívá odvětví standardní [nástroje BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux multilicenční šifrování operačního systému a disků data.

Řešení je integrována s Azure klíč trezoru vám pomůže určit a Správa šifrovacího klíče disku a tajemství předplatné klíčové trezoru zároveň zajistit, že jsou všechna data v disků virtuálního počítače šifrované v klidu v úložišti Azure.

Víc se uč:

- [Šifrování Azure disku pro Windows a Linux IaaS VMs](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Šifrování Linux a virtuálních počítačích Windows Azure disku](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Šifrování virtuálního počítače](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Zálohování virtuálního počítače

Azure zálohování bylo scalable řešení, které chrání data aplikace s nulovou kapitálových investic a minimální provozní náklady. Chyby aplikací může dojít k poškození dat a lidské chyby může způsobovat chyby do aplikace. Pomocí Azure zálohování není zamčený vaší virtuálních počítačích s Windows a Linux.

Víc se uč:

- [Co je Azure zálohování?](../backup/backup-introduction-to-azure-backup.md)
- [Azure záložní Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure záložní služby – nejčastější dotazy](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Obnovení Azure webu

Důležitou součástí organizace BCDR strategie zjistit, jak nepřijít podnikové pracovního vytížení a aplikací se spuštěný plánované a neplánované výpadků dojít. Obnovení Azure webu pomůže organizovat replikace, převzetí a obnovení pracovního vytížení a aplikací tak, aby byly přístupné z sekundární umístění Pokud havaruje svém hlavním pracovišti.

Obnovení webu:

- **Simplifies strategie BCDR** – obnovení webu usnadňuje zpracovávání replikace, převzetí a obnovení víc obchodních pracovního vytížení a aplikace z jednoho místa. Obnovení webu orchestrates replikace a překlopení, ale není zachytit data aplikace nebo máte žádné informace.
- **Flexibilní replikace poskytuje** – pomocí obnovení webu můžete replikovat pracovní vytížení Hyper-V virtuálních počítačích, VMware virtuálních počítačích a fyzické servery systému Windows nebo Linux.
- **Převzetí podporuje a obnovení** – obnovení webu poskytuje převzetí služeb při selhání test pro podporu havárie obnovení cvičení beze změny provozním prostředí. Můžete taky spustit plánované převzetí služeb při selhání se ztrátou dat nula pro očekávané výpadků nebo neplánované převzetí služeb při selhání s minimálními ke ztrátě (podle frekvence replikace) pro neočekávané havárie. Po selhání můžete navrácení primární weby. Obnovení webu poskytuje obnovení plánů, které mohou obsahovat skripty a Azure automatizaci sešity tak, že je možné upravit převzetí a obnovení vícevrstvého aplikací.
- **Sekundární datacentra eliminuje** – můžete replikovat na vedlejší místního web nebo na Azure. Použití Azure jako cíle pro obnovení havárie eliminuje náklady a složitost zachování sekundární webu. Replikovanou data se ukládají v úložišti Azure.
- **Integrates s existujících technologií BCDR** – obnovení webu spolupracuje s dalšími funkcemi BCDR aplikace. Můžete například obnovení webu chránit SQL Server konci podnikové úloh. Jedná se o podporu pro SQL Server AlwaysOn ke správě převzetí dostupnost skupiny.

Víc se uč:

- [Co je obnovení webu Azure?](../site-recovery/site-recovery-overview.md)
- [Jak funguje obnovení Azure webu?](../site-recovery/site-recovery-components.md)
- [Co jsou chráněny úloh obnovení webu Azure?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuální sítě

Virtuálních počítačích třeba připojení k síti. K podpoře tohoto požadavku, vyžaduje Azure virtuálních počítačích k připojení k síti virtuální Azure. Virtuální sítě Azure je logické konstrukce této technologii postavená struktury fyzické Azure sítě. Každý logické virtuální síť Azure je izolovaný od všech dalších Azure virtuální sítí. Tato izolace pomáhá zajistit, že síťová komunikace ve vaší nasazení není přístupný pro ostatních zákazníků Microsoft Azure.

Víc se uč:

- [Přehled zabezpečení Azure sítě](security-network-overview.md)
- [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)
- [Funkce sítě a partnerství scénářích Enterprise](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Správa zásad zabezpečení a vytváření sestav

Centrum zabezpečení Azure pomáhá zabránit, zjišťování a odpovědět na hrozeb a poskytuje že lepší přehled o a publikum nemůže ovládat, zabezpečení Azure zdroje. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Centrum zabezpečení Azure vám pomůže optimalizovat a sledovat virtuálního počítače zabezpečení:

- Poskytování virtuálního počítače [zabezpečení doporučení](../security-center/security-center-recommendations.md) , jako použití aktualizací systému, konfigurace ACL koncové body, povolení antimalware, povolení skupiny zabezpečení sítě a použít šifrování disku.
- Sledování stavu virtuálních počítačích

Víc se uč:

- [Úvod k Centru zabezpečení Azure](../security-center/security-center-intro.md)
- [Centrum zabezpečení Azure nejčastější dotazy](../security-center/security-center-faq.md)
- [Centrum zabezpečení Azure plánování a provoz](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Dodržování předpisů

Azure virtuálních počítačích osvědčené k FISMA FedRAMP, HIPAA, PCI rozhodování úrovně 1 a jiných aplikacích klíčové dodržování předpisů. Této certifikace usnadňuje pro vlastní Azure aplikace dodržování předpisů a pro svoji firmu při řešení širokou škálu pro vnitrostátní a mezinárodní zákonných požadavků.

Víc se uč:

- [Centrum zabezpečení aplikace Microsoft: dodržování předpisů](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Důvěryhodné cloudu: Microsoft Azure zabezpečení ochrany osobních údajů a dodržování předpisů](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
