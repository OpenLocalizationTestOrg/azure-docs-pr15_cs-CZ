<properties
    pageTitle="Obnovení webu Azure: Nejčastější dotazy | Microsoft Azure"
    description="Tento článek popisuje populární otázky o obnovení webu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Obnovení webu Azure: Nejčastější dotazy

Tento článek obsahuje nejčastější dotazy k obnovení webu Azure. Pokud máte nějaké otázky po přečtení v tomto článku, vystavit je na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Obecné

### <a name="what-does-site-recovery-do"></a>Co dělá obnovení webu?

Obnovení webu přispívá k business kontinuitu a havárie obnovení (BCDR) strategie, tak, že orchestrating a automatizace replikace z místního virtuálních počítačích a fyzické servery Azure nebo vedlejší datacentra. [Další informace](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Co můžete chránit obnovení webu?

- **Hyper-V virtuálních počítačích**: obnovení webu můžete chránit všechny pracovní zátěž spuštěných pro Hyper-V OM.
- **Pole fyzicky servery**: obnovení webu můžete chránit fyzické servery se systémem Windows nebo Linux.
- **VMware virtuálních počítačích**: obnovení webu můžete chránit všechny pracovní zátěž aplikaci VMware OM.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Podporuje obnovení webu modelu správce prostředků Azure?

Kromě obnovení webu Azure klasické portálu obnovení webu je dostupná v portálu Azure s podpora pro správce prostředků. Pro většinu nasazení scénářů obnovení webu v Azure portál poskytuje optimalizovaného nasazení a replikovat VMs a fyzické servery do klasického úložiště nebo Správce úložiště. Tady jsou podporované nasazení:

- [Replikace VMware VMs nebo pole fyzicky servery na Azure na portálu Azure](site-recovery-vmware-to-azure.md)
- [Replikovat Hyper-V VMs v VMM mračnech Azure na portálu Azure](site-recovery-vmm-to-azure.md)
- [Replikace Hyper-V VMs (bez VMM) na Azure na portálu Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replikovat Hyper-V VMs v VMM mračnech sekundární web na portálu Azure](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Co je potřeba v Hyper-V organizovat replikace s obnovení webu?

Pro Hyper-V hostiteli server co byste měli závisí na tom scénář nasazení. Podívejte se na požadavky pro Hyper-V v:

- [Replikace Hyper-V VMs (bez VMM) na Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Replikace Hyper-V VMs (plus pár VMM) na Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Replikace Hyper-V VMs na vedlejší datacentru](site-recovery-vmm-to-vmm.md#before-you-start)

- Pokud jste replikace k sekundární datacentru, přečtěte si o [podporované hostované operační systémy pro Hyper-V VMs](https://technet.microsoft.com/library/mt126277.aspx).
- Pokud jste replikace na Azure, obnovení webu podporuje všechny hosta operační systémy, které jsou [podporované kodérem Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Můžete chránit VMs, když Hyper-V běží v operačním systému klienta?

Ne, musí být umístěné VMs na hostitelském serveru Hyper-V, na kterém běží počítače podporované serveru Windows. Pokud nepotřebujete chránit klientský počítač může ji replikovat jako fyzické počítač [Azure](site-recovery-vmware-to-azure.md) nebo [vedlejší datacentra](site-recovery-vmware-to-vmware.md).


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>K čemu úloh můžete chránit pomocí obnovení webu?

Obnovení webu umožňuje chránit většina pracovní vytížení podporované OM nebo pole fyzicky serveru. Obnovení webu podporuje podporujících aplikaci replikace tak, aby aplikace můžete obnovit až inteligentní stavu. Lze integrovat s aplikací Microsoft, jako je Sharepointu, Exchange, Dynamics, SQL Server a služby Active Directory a spolupracuje s počáteční dodavatelům, a to včetně Oracle SAP, IBM a červené klobouk. [Další informace](site-recovery-workload.md) o ochraně zátěží na projektu.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Potřebují pro Hyper-V hosts být VMM mračnech?

Pokud chcete replikovat na vedlejší datacentra Hyper-V VMs musí být na Hyper-V hostuje servery umístěné v cloudu VMM. Pokud budete chtít replikovat na Azure, můžete na serverech hostitele Hyper-V pomocí hesla nebo bez VMM mračnech replikovat VMs. [Přečtěte si další](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Můžete nasadit obnovení webu s VMM, když máme pouze jeden VMM server?

Ano. Můžete buď replikovat VMs Hyper-V serverů v VMM obláčkem, která Azure nebo můžete replikovat mezi VMM mračnech na stejný server. Místní místní replikace doporučujeme mít VMM server v primárních a sekundárních weby.  [Další informace](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>K čemu fyzické servery můžete chránit?

Můžete replikovat fyzické servery systému Windows a Linux Azure nebo vedlejší webu. [Přečtěte si víc o](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) požadavky na operační systém.  Požadavky na stejné platí pro fyzické servery jste replikace Azure nebo vedlejší webu.

Poznámka: fyzické servery bude bezchybnou jako VMs v Azure Pokud havaruje místního serveru. Navrácení do místního serveru fyzické není aktuálně podporován, ale může selhat zpátky do virtuálního počítače se systémem Hyper-V nebo VMware.


### <a name="what-vmware-vms-can-i-protect"></a>K čemu VMware VMs můžete chránit?

K ochraně VMware VMs musíte mít vSphere hypervisor a spuštění nástroje VMware virtuálních počítačích. Doporučujeme taky, jestli máte ke správě hypervisory vCenter server VMware. [Další informace](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) o přesné požadavky pro replikace VMware servery a VMs Azure nebo vedlejší webu.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Můžete spravovat havárie obnovení Moje poboček s obnovení webu?

Ano. Když použijete obnovení webu k organizovat replikace a převzetí do poboček společnosti, zobrazí se jednotné průběhu a zobrazit všechny úlohami větev office na jednom centrálním místě. Můžete snadno spustit převzetí služeb při selhání a spravovat havárie využívání všech poboček z vaší sídlo nepoužijete informace na pobočky.

## <a name="security"></a>Zabezpečení

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Odeslaný replikace dat ve službě obnovení webu?

Ne, obnovení webu nemá zachytit replikovanou data a nemá žádné informace o co běží na virtuálních počítačích nebo pole fyzicky servery.
Replikace je zasílané mezi hosts Hyper-V místním, VMware hypervisory fyzické servery a Azure úložiště nebo vedlejší webu. Obnovení webu má schopnost zachytit tato data. Pouze metadata potřebné k organizovat replikace a převzetí odeslaný ke službě obnovení webu.

Obnovení webu je ISO 27001:2013 27018, HIPAA DPA certifikované a probíhá hodnocení SOC2 a FedRAMP JAB.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Dodržování předpisů důvodů musí zůstat i naše místní metadata v rámci stejné zeměpisnou oblast. Můžete obnovení webu pomoc s abychom?

Ano. Když vytvoříte trezoru obnovení webu v oblasti, můžeme zajistit, aby všechny metadata, která je třeba povolit a organizovat replikace a převzetí zůstane v této oblasti uživatele zeměpisnou okraj.

### <a name="does-site-recovery-encrypt-replication"></a>Obnovení webu zašifrovat replikace?

Virtuálních počítačích a fyzické servery replikace mezi místním weby šifrování na cestě jsou podporovány. Virtuálních počítačích a fyzické servery replikace Azure šifrování na cestě a šifrování at rest (v Azure) jsou podporované.


## <a name="replication"></a>Replikace


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Jsou všechny požadavky pro replikace virtuálních počítačích Azure?

Virtuálních počítačích, které chcete replikovat na Azure musí odpovídat požadavkům na [Azure](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Můžete replikovat Hyper-V generování 2 virtuálních počítačích na Azure?

Ano. Obnovení webu převede z generování 2 generování 1 při selhání. Na navrácení počítači převést zpět na generování 2. [Přečtěte si další](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Pokud se replikovat na Azure Jak můžu zaplatit Azure VMs?

Během pravidelné replikace dat replikovat na geo nadbytečné Azure úložiště a nemusíte zaplatit poplatky všechny Azure IaaS virtuálního počítače, poskytnutí významnou výhodu. Když spustíte přepojení Azure, obnovení webu automaticky vytvoří Azure IaaS virtuálních počítačích a až to budete fakturovat pro výpočetním zdroje, které můžete používat v Azure.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Existuje SDK se dá použít k automatizaci automatickým obnovením systému pracovního postupu?

Ano. Můžete automatizovat pracovní postupy obnovení webu pomocí rozhraní Rest API, prostředí PowerShell nebo Azure SDK. Aktuálně podporované scénáře pro nasazení obnovení webu pomocí Powershellu:

- [Replikovat Hyper-V VMs v VMMs mračnech klasické prostředí PowerShell Azure](site-recovery-deploy-with-powershell.md)
- [Replikovat Hyper-V VMs v VMMs mračnech správci zdrojů Azure prostředí PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replikovat Hyper-V VMs bez VMM klasické prostředí PowerShell Azure](site-recovery-hyper-v-site-to-azure-classic.md)
- [Replikovat Hyper-V VMs bez VMM vedoucímu Azure prostředí PowerShell zdroje](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Pokud se replikovat na Azure jaký druh účtu úložiště je potřeba?

- **Azure klasické portálu**: když nasazujete obnovení webu Azure klasické portálu, musíte mít [účet standardní geo nadbytečné úložiště](../storage/storage-redundancy.md#geo-redundant-storage). Úložiště Premium není aktuálně podporován. Účet musí být ve stejné oblasti jako trezoru obnovení webu.

- **Azure portálu**: když nasazujete obnovení webu na portálu Azure, musíte mít účet úložiště LRS nebo GRS. Doporučujeme, abyste GRS tak, aby data pružné místní výpadek, zda oblasti primární možné obnovit. Účet musí být ve stejné oblasti jako služby Recovery trezoru. Premium úložiště je podporována pouze v případě, že jste replikace VMware VMs nebo pole fyzicky servery.

### <a name="how-often-can-i-replicate-data"></a>Jak často replikovat data?

- **Hyper-V:** Pro Hyper-V VMs replikovat každých 30 sekund, 5 minut nebo 15 minut. Pokud jste nastavili SAN replikace je replikace synchronní.
- **VMware a fyzické servery:** Četnosti replikace není důležité tady. Replikace je průběžný.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Můžete rozšířit replikace ze stávajícího webu obnovení na jiný web třetího?

Rozšířené nebo zřetězené replikace není podporována. Požádat o tato funkce ve [fóru svůj názor](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Můžu offline replikace poprvé, můžu replikovat na Azure?

Toto není podporovaná. Požádat o tato funkce ve [fóru názory](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).


### <a name="can-i-exclude-specific-disks-from-replication"></a>Můžete vyloučit konkrétní disků z replikace?

To je podporované, když jste [replikace VMware VMs a fyzické servery](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) na Azure pomocí portálu Azure.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Můžete replikovat virtuálních počítačích s dynamických disků?

Dynamické disků jsou podporované, když replikace Hyper-V virtuálních počítačích. Jsou taky podporované při replikace VMware VMs a fyzických počítačů do Azure. Základní disk musí být disk operační systém.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Můžete přidat do nového počítače do existující skupiny replikace?

Přidání nového počítače do existující skupiny replikace je podporovaná. K tomu, vyberte skupinu replikace (z zásuvné replikovaná položky) a klikněte pravým tlačítkem myši a vyberte místní nabídka ve skupině replikace a vyberte požadovanou možnost.

![Přidat do skupiny replikace](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Můžete šířku pásma přidělené replication přenosů Hyper-V?

Ano. Další informace o omezení šířky pásma v článcích nasazení:

- [Plánování replikace VMware VMs a fyzické servery kapacity](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Plánování replikace Hyper-V VMs v VMM mračnech kapacity](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Plánování replikace Hyper-V VMs bez VMM kapacity](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Překlopení


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Pokud se mi nedaří myši Azure, jak se dostanu ke Azure virtuálních počítačích po převzetí služeb?

Azure VMs můžete přistupovat přes zabezpečené připojení k Internetu, přes VPN na webu nebo přes Azure ExpressRoute. Musíte Příprava některé věci budete moct připojit. Další informace v:

- [Připojení k Azure VMs po převzetí VMware VMs nebo pole fyzicky serverů](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Připojení k Azure VMs po převzetí Hyper-V VMs v VMM mraky](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Připojení k Azure VMs po převzetí Hyper-V VMs bez VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Pokud mám převzít Azure jak Azure, aby se je pružné Moje data?

Azure je určený pro odolnost. Obnovení webu je už vytvořena pro přepnutí sekundární Azure datacentru v souladu s Azure SLA případě potřeby. V takovém případě jsme zkontrolujte metadata a trezorů zůstávají ve stejném zeměpisnou oblast, kterou jste vybrali trezoru.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Pokud se mi replikace mezi dvěma datacentrech co se stane, když Moje primární datacentra dojde k neočekávané výpadku?

Můžete aktivovat neplánované převzetí z sekundární webu. Obnovení webu nevyžaduje připojení z primární webu provádět záložní.

### <a name="is-failover-automatic"></a>Pokud je se převzetí automaticky?

Převzetí není automatické. Zahájení převzetí služeb při selhání s jedním klepnutím na portálu nebo pomocí [Powershellu obnovení webů](https://msdn.microsoft.com/library/dn850420.aspx) aktivovat přepojení. Nejsou-li znovu je jednoduchý akce na portálu obnovení webu.

K automatizaci můžete využívat Orchestrator místní nebo Operations Manager zjišťování selhání virtuálního počítače a potom aktivace převzetí pomocí SDK.

- [Přečtěte si další](site-recovery-create-recovery-plans.md) informace o obnovení plány.
- [Přečtěte si další](site-recovery-failover.md) informace o převzetí.
- [Přečtěte si další](site-recovery-failback-azure-to-vmware.md) informace o selhání zpět VMware VMs a fyzických serverů


## <a name="service-providers"></a>Poskytovatelé


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Můžu poskytovatele služeb. Funguje obnovení webu pro vyhrazené a sdílené infrastrukturu modely?

Ano, obnovení webu podporuje oba vyhrazené a sdílené infrastrukturu modely.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>U poskytovatele je identity klienta nasdílel službu obnovení webu?

Ne. Identita klienta zůstane anonymní. Vaše student, nemusíte přístup na portál obnovení webu. Pomocí portálu bude spolupracovat jenom správce poskytovatele služeb.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Data aplikace klienta někdy přejde na Azure?

Pokud replikace mezi weby vlastní poskytovatele služeb dat aplikace nikdy půjde Azure. Data musí být zašifrovaný na cestě a replikovanou přímo mezi weby poskytovatele služeb.

Pokud jste replikace na Azure, data aplikací odeslaný Azure úložiště, ale ne ve službě obnovení webu. Data jsou zašifrované na cestě a zůstane zašifrovaný v Azure.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Moje student, dostanou faktuře za služby Azure?

Ne. Vztah fakturace na Azure je přímo u poskytovatele služeb. Pro generování konkrétní faktury pro jejich klienti odpovídají poskytovatele služeb.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Pokud mám replikace Azure, potřebujeme spuštění virtuálních počítačích Azure vždy?

Ne, Data replikovat na účet Azure úložiště ve vašem předplatném. Při provádění přepojení test (DR AD) nebo skutečné převzetí obnovení webu automaticky vytvoří virtuálních počítačích předplatné.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Můžete zajistit, že klienta úroveň izolace můžu replikovat na Azure?

Ano.

### <a name="what-platforms-do-you-currently-support"></a>Jaké platformy je momentálně podporují?

Podporujeme Azure Pack cloudu platformu systému a System Center na základě nasazení (2012 nebo novější). [Další informace](https://technet.microsoft.com/library/dn850370.aspx) o integration Azure Pack a obnovení webu.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Podporují se jeden Azure Pack a jednoho nasazení serveru VMM?

Ano, můžete replikovat Hyper-V virtuálních počítačích Azure nebo replikovat mezi weby poskytovatele služeb.  Všimněte si, že pokud replikovat mezi weby poskytovatele služby Azure postupu runbook integrace není dostupná.


## <a name="next-steps"></a>Další kroky

- Přečtěte si [Přehled obnovení webu](site-recovery-overview.md)
- Další informace o [obnovení webu architektura](site-recovery-components.md)  
