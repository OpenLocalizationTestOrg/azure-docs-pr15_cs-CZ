<properties
    pageTitle="Jak funguje obnovení webu? | Microsoft Azure"
    description="Tento článek obsahuje přehled o obnovení webu architektura"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Jak funguje obnovení webu Azure?

V tomto článku se pochopit základní architekturu služby Azure obnovení webu a součásti, které usnadňují práci. 

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.


## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie. 

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzických serverů a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Obnovení webu Azure portálu

Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure modelu a model klasické služby správy. Azure má dvě portály – i [Azure klasické portál](https://manage.windowsazure.com/) , který podporuje modelu klasické nasazení a [Azure portál](https://portal.azure.com) s podporou pro oba modely nasazení.

Obnovení webu je k dispozici v portálu klasické a portálu Azure. Na portálu Azure klasické podporují obnovení webu s modelem klasické služby správy. Na portálu Azure může podporovat klasické modelu nebo nasazení modelu zdroje. [Přečtěte si další](site-recovery-overview.md#site-recovery-in-the-azure-portal) informace o instalaci pomocí portálu Azure.

Informace v tomto článku platí pro klasickou a Azure portálu nasazení. Rozdíly jsou uvedeny v případě potřeby.

## <a name="deployment-scenarios"></a>Scénáře nasazení

Obnovení webu může být nasazené na organizovat replikace v mnoha případech:

- **Replikovat VMware virtuálních počítačích**: replikovat místní VMware virtuálních počítačích Azure nebo vedlejší datacentra.
- - **Replikovat fyzické stroje**: replikovat fyzické počítače se systémem Windows nebo Linux Azure nebo vedlejší datacentra. Proces replikace fyzických počítačů je téměř stejný jako proces replikace VMware VMs
- **Replikovat Hyper-V VMs (bez VMM)**: replikovat Hyper-V VMs, které nejsou spravuje VMM Azure.
- **Replikovat VMs Hyper-V spravovaný v System Center VMM mračnech**: replikovat místní Hyper-V virtuálních počítačích umístěnými na serverech hostitele Hyper-V v VMM mračnech Azure nebo vedlejší datacentra. Můžete replikovat pomocí standardní Hyper-V otevřené nebo SAN replikace.
- **Migrace VMs**: můžete použít zotavení webu [migrace Azure IaaS VMs](site-recovery-migrate-azure-to-azure.md) mezi oblastmi nebo migrace [instance AWS Windows](site-recovery-migrate-aws-to-azure.md) Azure IaaS VMs. Aktuálně jenom migrace je podporována což znamená, můžete převzít tyto VMs ale vám nejde nepovede zpět.

Obnovení webu můžete replikovat většina aplikace spuštěné v těchto VMs a fyzické servery. Úplné můžete získat souhrnné podporovaných aplikací v [Jaké úloh obnovení webu Azure chránit?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Souběžné Azure: VMware virtuálních počítačích nebo pole fyzicky Windows/Linux serverů

Existuje několik způsobů replikovat VMware VMs s obnovení webu.

- **Pomocí portálu Azure**-když nasadíte obnovení webu Azure portálu může selhat přes VMs klasické služby Správce úložiště nebo do Správce zdrojů. Replikace VMware VMs Azure portálu přináší řadu výhod, včetně možnost replikovat do classic nebo Správce úložiště v Azure. [Další informace](site-recovery-vmware-to-azure.md).
- **Pomocí portálu klasické**– klasické portálu pomocí rozšířené možnosti nástroje můžete nasazovat obnovení webu. To bude použito pro všechny nové nasazení klasické portálu. V tomto nasazení se mohou pouze převzít VMs klasické úložiště v Azure a ne k základnímu úložišti Správce prostředků. [Další informace](site-recovery-vmware-to-azure-classic.md). Je také [starší verze prostředí](site-recovery-vmware-to-azure-classic-legacy.md) pro nastavení VMware replikace klasické portálu. Toto není vhodné používat novou nasazení.  Pokud jste již nainstalovali pomocí starší verze prostředí [informace o migraci](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) se vylepšené nasazení.



Požadavky na architekturu pro nasazení obnovení webu replikovat VMware VMs/fyzické servery v portálu Azure nebo portálu Azure klasické (rozšířené) jsou podobné pomocí několika jednoduchých rozdíly:

- Pokud nasadíte v portálu Azure replikovat na základě správce prostředků úložiště a používat správce sítě pro připojení Azure VMs po převzetí.
- Při nasadíte v portálu Azure obou LRS GRS úložiště je podporované a. Na portálu klasické požaduje GRS.
- Procesu nasazení je zjednodušené a srozumitelnější Azure portálu.


Tady je, co budete potřebovat:

- **Účet Azure**:, musíte mít účet Microsoft Azure.
- **Azure úložiště**:, musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Můžete použít klasické účet nebo účet správce prostředků úložiště. Účet může být LRS nebo GRS při nasazení Azure portálu. Replikovanou data se ukládají v Azure úložiště a Azure VMs jsou nespředený nahoru, když dojde k selhání. 
- **Azure sítě**:, musíte mít Azure virtuální sítě, který Azure VMs se připojí k, když jste vytvořili v převzetí. Na portálu Azure mohou být sítě vytvořené v model správce klasické služeb nebo správce prostředků modelu.
- **Konfigurace místního serveru**:, musíte mít počítač Windows serveru 2012 R2 místní, které se spouští konfigurační server a jiných komponent obnovení webu. Pokud jste replikace VMware VMs to by měl být vysoce dostupné OM VMware. Pokud chcete replikovat fyzické servery může být počítači fyzické. Tyto součásti obnovení webu budou nainstalovaná v počítači:
    - **Konfigurace serveru**: souřadnice komunikace mezi místním prostředím a Azure a spravuje replikace dat a obnovení.
    - **Proces serveru**: slouží jako replikace brány. Načte data replikace z počítačů chráněného zdroj, optimalizuje ukládání do mezipaměti, komprese a šifrování a odešle data do Azure úložiště. Je taky zpracovává nabízených instalace služby mobilita chráněné počítačích a provádí automatické zjišťování VMware VMs. Narůstající velikostí nasazení můžete přidat další samostatný proces vyhrazené serverů zpracovávání rostoucí objemy replikace.
    - **Hlavní cílový server**: zpracovává replikace dat během navrácení z Azure. 
- **VMware VMs nebo pole fyzicky serverů replikovat**: každý počítač, který chcete replikovat Azure potřebovat součást služby nastavení mobilních zařízení nainstalovaný. Tuto službu zaznamenává zápis dat v počítači a předá ho server procesu. Tato součást můžete být nainstalovány ručně, nebo můžete být posunuto a automaticky serverem proces po povolení replikace pro počítač.
- **vSPhere tabulkami hosts a vCenter serveru**:, musíte mít jeden nebo více vSphere hostitele serverů spouštění VMware virtuálních. Doporučujeme nasazení serveru vCenter ke správě těchto hostitelů.
- **Navrácení**: tady je budete potřebovat:
    - **Není podporovaná fyzické fyzické navrácení**: znamená to, že pokud převzetí fyzických serverů Azure a pak chcete navrácení, vám musí nepovede zpět VMware OM. Nelze nepovede zpět na fyzické server. Budete potřebovat Azure OM selhání zpět a neměli nasazení serveru konfigurace jako VMware OM musíte nastavit oddělené předlohy cílové server jako VMware OM. To je potřeba, protože server předlohy cílové spolupracuje a připojí k základnímu úložišti VMware obnovíte disků VMware OM.
    - - **Dočasné proces serveru v Azure**: Chcete-li po převzetí budete muset nastavit Azure OM nakonfigurování jako serveru proces zpracování replikace z Azure selhat zpět z Azure. Po dokončení navrácení můžete odstranit tento OM.
    - **Připojení VPN**: navrácení, musíte mít připojení VPN (nebo Azure ExpressRoute) nastavit z Azure sítě k místnímu webu.
    - **Samostatné místního předlohy na cílový**: místního serveru předlohy cílové zpracovává navrácení. Předlohy cílový server je ve výchozím nastavení na serveru správy nainstalovaný, ale pokud jste selhání zpět větší množství přenosů by měl nastavit samostatném místního předlohy na cílový k tomuto účelu.

**Obecné architektura**

![Rozšířené](./media/site-recovery-components/arch-enhanced.png)

**Komponenty pro nasazení**

![Vylepšené](./media/site-recovery-components/arch-enhanced2.png)

**Překlopení zpět**

![Vylepšené navrácení](./media/site-recovery-components/enhanced-failback.png)


- [Další informace](site-recovery-vmware-to-azure.md#azure-prerequisites) o požadavcích pro Azure portálu nasazení.
- [Další informace](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o požadavcích vylepšené nasazení klasické portálu.
- [Další informace](site-recovery-failback-azure-to-vmware.md) o navrácení na portálu Auzre.
- [Další informace](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) o navrácení klasické portálu.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Souběžné Azure: není spravuje VMM VMs Hyper-V

Můžete replikovat Hyper-V VMs, které nejsou spravuje System Center VMM na Azure s obnovení webu následujícím způsobem:

- **Pomocí portálu Azure**-když nasadíte obnovení webu Azure portálu může selhat přes VMs k základnímu úložišti klasické nebo správci zdrojů. [Další informace](site-recovery-hyper-v-site-to-azure.md).
- **Pomocí portálu klasické**– klasické portálu nástroje můžete nasazovat obnovení webu. V tomto nasazení se mohou pouze převzít VMs klasické úložiště v Azure a ne k základnímu úložišti Správce prostředků. [Další informace](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura obou nasazení je podobná, s tím rozdílem, že:

- Pokud nasadíte v portálu Azure replikovat k základnímu úložišti Správce prostředků a používat správce sítě pro připojení Azure VMs po překlopení.
- Procesu nasazení je zjednodušené a srozumitelnější Azure portálu.

Tady je, co budete potřebovat:

- **Účet Azure**:, musíte mít účet Microsoft Azure.
- **Azure úložiště**:, musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Na portálu Azure můžete klasické účet nebo účet správce prostředků úložiště. Na portálu klasické můžete použít jenom klasické účet. Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání.
- **Azure sítě**:, musíte mít Azure sítě, který Azure VMs se připojí k, když jste vytvořili po převzetí. 
- **Host (hostitel) pro Hyper-v**:, musíte mít jeden nebo více serveru Windows Server 2012 R2 Hyper-V. Během obnovení webu nasazení se nainstalujete poskytovatel obnovení webu Azure a agenta Microsoft Azure obnovení Services na hostiteli.
- **Hyper-V VMs**:, musíte mít jeden nebo více VMs na hostitelském serveru Hyper-V. Azure zprostředkovatele obnovení webů a služby Azure Recovery agent na hostiteli Hyper-V během obnovení webu nasazení. Zprostředkovatel souřadnic a orchestrates replikace ke službě obnovení webu přes internet. Agent zpracovává data replikace dat přes HTTPS 443. Komunikace od poskytovatele a agent se zabezpečené šifrované. Šifrovány také replikovanou dat v Azure úložiště.

**Obecné architektura**

![Web pro Hyper-V Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Další informace](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o požadavcích pro Azure portálu nasazení.
- [Další informace](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o požadavcích pro klasické portálu nasazení.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Souběžné Azure: spravuje VMM VMs Hyper-V

Azure s obnovení webu můžete replikovat Hyper-V VMs v VMM mračnech následujícím způsobem:

- **Pomocí portálu Azure**-když nasadíte obnovení webu Azure portálu může selhat přes VMs k základnímu úložišti klasické nebo správci zdrojů. [Další informace](site-recovery-vmm-to-azure.md).
- **Pomocí portálu klasické**– klasické portálu nástroje můžete nasazovat obnovení webu. V tomto nasazení se mohou pouze převzít VMs klasické úložiště v Azure a ne k základnímu úložišti Správce prostředků. [Další informace](site-recovery-vmm-to-azure-classic.md).

Architektura obou nasazení je podobná, s tím rozdílem, že:

- Pokud nasadíte v portálu Azure replikovat na základě správce prostředků úložiště a používat správce sítě pro připojení Azure VMs po převzetí.
- Procesu nasazení je zjednodušené a srozumitelnější Azure portálu.


Tady je, co budete potřebovat:

- **Účet Azure**:, musíte mít účet Microsoft Azure.
- **Azure úložiště**:, musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Na portálu Azure můžete klasické účet nebo účet správce prostředků úložiště. Na portálu klasické můžete použít jenom klasické účet. Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání.
- **Azure sítě**: budete muset nastavit síť mapování tak, aby Azure VMs připojeni k odpovídající sítě jsou vytvořené po překlopení. 
- **VMM server**: budete potřebovat jeden nebo více místní VMM serverů spuštěných pro System Center 2012 R2 a nastavit jeden nebo více soukromé mračnech. Pokud nasazujete v Azure portálu, musíte mít logické a OM sítí nastavení můžete konfigurovat mapování sítě. Na portálu klasické Toto je nepovinný krok.  V síti OM by měl být propojené s logické sítě, který máte přidružený k cloudu.
- **Host (hostitel) pro Hyper-v**:, musíte mít jeden nebo více serverů hostitele Windows serveru 2012 R2 Hyper-V v cloudu VMM.
- **Hyper-V VMs**:, musíte mít jeden nebo více VMs na hostitelském serveru Hyper-V.

**Obecné architektura**

![VMM Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Další informace](site-recovery-vmm-to-azure.md#azure-requirements) o požadavcích pro Azure portálu nasazení.
- [Další informace](site-recovery-vmm-to-azure-classic.md#before-you-start) o požadavcích pro klasické portálu nasazení.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Replikovat na vedlejší web: VMware virtuálních počítačích nebo pole fyzicky serverů 

Replikace VMware VMs nebo pole fyzicky servery s webem sekundární ke stažení InMage Scout, která je součástí předplatného obnovení webu Azure. Je můžete stáhnout z portálu Microsoft Azure nebo z portálu Microsoft Azure klasické. 

Nastavení servery součástí každého webu (konfigurace, obrázku, předlohy cílové) a nainstalujte Unified Agent na počítačích, které chcete replikovat. Po počáteční replikace na každém počítači odešle delta replikace změny na server obrázku. Server procesu optimalizuje data a přenos předlohy cílový server na vedlejší webu. Konfigurace serveru spravuje procesu replikace.

Tady je budete potřebovat:

**Účet Azure**: nasazení tento scénář pomocí InMage Scout. Získání musíte mít předplatné Azure. Po vytvoření trezoru obnovení webu InMage Scout stáhněte a nainstalujte nejnovější aktualizace nastavení nasazení.
**Proces server (primární webu)**: nastavení součást procesu serveru na primární webu zpracovávání ukládání do mezipaměti, kompresi a optimalizace data. Zpracuje také nabízených instalace služby Unified agenta do počítačů, které chcete zamknout. 
**VMware ESX/ESXi a vCenter server (primární webu)**: Pokud jste ochrana VMware VMs budete potřebovat hypervisor VMware EXS/ESXi a volitelně ke správě hypervisory VMware vCenter server.
- **Servery VMs/fyzické (hlavní web)**: VMware VMs Windows/Linux fyzické serverech nebo chcete zamknout bude potřeba instalaci agenta jednotné. Unified Agent nainstalovaný také v počítačích budou sloužit jako hlavní cílový server. Agent funguje jako poskytovatele služby komunikace mezi všechny prvky. 
- - **Konfigurační server (sekundární webu)**: konfigurační server je první součást nainstalujete a je nainstalovaný na vedlejší webu jejich správa, konfigurace a sledovat nasazení, buď pomocí správy webu nebo v konzole vContinuum. Existuje pouze jeden konfigurační server v nasazení a musí být nainstalovaný na počítači se systémem Windows Server 2012 R2.
- **vContinuum server (sekundární webu)**: hned po instalaci používat ve stejném umístění (sekundární webu) jako konfigurační server. Poskytuje konzola pro správu a sledování chráněné prostředí. Ve výchozím nastavení je vContinuum server první předlohy cílový server a nainstaloval Unified Agent.
- **Hlavní cílový server (sekundární webu)**: hlavní cílový server obsahuje replikovanou data. Načte data ze serveru proces, sekundární webu vytvoří počítači otevřené a obsahuje datových bodů uchovávání informací. Počet předlohy cílových serverů, které bude nutné závisí na počtu počítačů, které jste ochrana. Pokud chcete selhání zpátky na stránku primární musíte mít hlavní cílový server nějaké příliš. 

**Obecné architektura**

![VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Replikovat na vedlejší web: spravuje VMM VMs Hyper-V


Můžete replikovat Hyper-V VMs, která spravuje System Center VMM k sekundární datacentru s obnovení webu následujícím způsobem:

- **Pomocí portálu Azure**-když nasadíte obnovení webu Azure portálu. [Další informace](site-recovery-hyper-v-site-to-azure.md).
- **Pomocí portálu klasické**– klasické portálu nástroje můžete nasazovat obnovení webu. [Další informace](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura obou nasazení je podobná, s tím rozdílem, že:

- Pokud nasadíte v portálu Azure musíte nastavit mapování sítě. Toto je nepovinný krok klasické portálu.
- Procesu nasazení je zjednodušené a srozumitelnější Azure portálu.
- - Pokud nasadíte v Azure klasické portálu [mapování úložiště](site-recovery-storage-mapping.md) je k dispozici.

Tady je, co budete potřebovat:

- **Účet Azure**:, musíte mít účet Microsoft Azure.
- **VMM server**: doporučujeme serveru VMM primární webu a jednu vedlejší webu, každý obsahující alespoň jeden VMM soukromé cloudu. Server by měla běžet aspoň System Center 2012 SP1 nejnovějších aktualizacích a připojení k Internetu. Mračnech by měla být profilu funkce Hyper-V nastavení. Obnovení poskytovatel webu Azure budete nainstalujte na VMM server. Zprostředkovatel souřadnic a orchestrates replikace ke službě obnovení webu přes internet. Komunikace mezi poskytovatele a Azure se zabezpečené šifrované.
- **Hyper-V server**: Hyper-V hostitelské servery by měl být umístěny ve VMM mračnech primárních a sekundárních. Host spuštěné servery by měl být aspoň Windows Server 2012 s nejnovějšími aktualizacemi nainstalovaný a připojení k Internetu. Replikace dat mezi servery hostitele primárních a sekundárních Hyper-V přes VPN pomocí ověřování protokolem Kerberos nebo certifikát nebo místní sítě.  
- **Chráněný stroje**: serveru zdroj Hyper-V by měla být alespoň jeden OM, který chcete zamknout.

**Obecné architektura**

![Místní do místního nasazení](./media/site-recovery-components/arch-onprem-onprem.png)


- [Další informace](site-recovery-vmm-to-vmm.md#azure-prerequisites) o nasazení požadavky na portálu Azure.
- - [Další informace](site-recovery-vmm-to-vmm-classic.md#before-you-start) o požadavcích na portálu Azure klasické nasazení.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Souběžné sekundárním web s opakováním SAN: spravuje VMM VMs Hyper-V

Můžete replikovat Hyper-V VMs spravovaný v VMM mračnech k sekundární webu pomocí účtu na portálu Azure klasické replikace SAN. Tento scénář není aktuálně podporován v novém portálu Azure. 

V tomto scénáři během obnovení webu nasazení nainstalujete zprostředkovatele obnovení Azure webů na serverech VMM. Zprostředkovatel souřadnic a orchestrates replikace ke službě obnovení webu přes internet. Replikace dat mezi pomocí synchronní replikace SAN primárních a sekundárních úložištích.

Tady je, co budete potřebovat:

**Účet Azure**:, musíte mít předplatné Azure
- **Pole SAN**: [podporované pole SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) spravuje primární VMM server. SAN sdílí síťovou infrastrukturu s jiného pole SAN sekundární webu.
- **VMM server**: doporučujeme serveru VMM primární webu a jednu vedlejší webu, každý obsahující alespoň jeden VMM soukromé cloudu. Server by měla běžet aspoň System Center 2012 SP1 nejnovějších aktualizacích a připojení k Internetu. Mračnech by měla být profilu funkce Hyper-V nastavení.
- **Hyper-V server**: Hyper-V hostitelské servery součástí VMM mračnech primárních a sekundárních. Host spuštěné servery by měl být aspoň Windows Server 2012 s nejnovějšími aktualizacemi nainstalovaný a připojení k Internetu.
- **Chráněný stroje**: serveru zdroj Hyper-V by měla být alespoň jeden OM, který chcete zamknout.

**SAN replikace architektura**

![Replikace SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Další informace](site-recovery-vmm-san.md#before-you-start) o požadavcích na nasazení.
### <a name="on-premises"></a>Místní



## <a name="hyper-v-protection-lifecycle"></a>Ochrana Hyper-V cyklus

Tento pracovní postup znázorňuje proces ochrana replikace a nedaří myši Hyper-V virtuálních počítačích. 

1. **Povolení ochrany**: nastavení trezoru obnovení webu a nastavení replikace VMM cloudu nebo Hyper-V webu a povolte ochranu VMs. Práce s názvem **Povolit ochranu** je spuštěná a můžete sledovat na kartě **projekty** . Úlohy zkontroluje, zda počítač splňuje požadavky na a potom vyvolá [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) metodu, která nastavuje replikace Azure pomocí nastavení, které jste nakonfigurovali. **Povolte ochranu** úlohu také vyvolá metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) inicializace plné replikace OM.
2. **Počáteční replikace**: virtuální počítač snímku, se považuje a virtuální pevných discích jsou replikovanou postupně, dokud všechny zkopírují Azure nebo vedlejší datacentra. Doba potřebné k dokončení to závisí na velikosti OM, šířka pásma a metody počáteční replikace. Pokud probíhá počáteční replikace dochází ke změnám disku sledování Hyper-V otevřené replikace sleduje tyto změny jako protokoly replikace Hyper-V (.hrl), které se nacházejí v sešitu do stejné složky jako discích. Každý disk má soubor přidružené .hrl odeslání na sekundárním úložiště. Všimněte si, že snímek a souborů protokolu využití prostředků disku probíhá počáteční replikace. Po dokončení počáteční replikace snímek OM odstraněna a synchronizovat a sloučení změn disku delta v protokolu.
3. **Ochrana dokončit**: po počáteční replikace dokončí úloha **Dokončit ochranu** nakonfiguruje sítě a další nastavení po replikace tak, aby se po zamknutí virtuální počítač. Pokud jste replikace na Azure můžete vylepšit nastavení pro virtuální počítač tak, aby je připraven k převzetí. V tomto okamžiku spuštěním testovací přepojení ke kontrole, jestli že všechno funguje očekávaným způsobem.
4. **Replikace**: po počáteční replikace synchronizace delta, nebude zahájen, v souladu s nastavením replikace. 
    - **Replikace selhání**: Pokud delta replikace selže a plné replikace bude náročné šířka pásma nebo čas, dojde k synchronizace. Pro soubory .hrl dosáhla 50 % velikosti disku klikněte OM-li například bude označen pro synchronizace. Synchronizace minimalizuje množství dat odeslaných výpočetních součty virtuálních počítačích zdrojové a cílové a posílání pouze delta. Po dokončení synchronizace delta replikace bude pokračovat. Ve výchozím nastavení synchronizace naplánován automatické spuštění mimo office hodin, ale virtuálního počítače můžete synchronizovat ručně.
    - **Chyba replikace**: Pokud dojde k chybě s replikace není předdefinované opakovat. Pokud je jiné Zotavitelná chyba například chybu ověřování nebo se tak mohli ověřovat nebo je počítač otevřené v neplatném stavu, bude použita žádné opakování. Pokud je například chybě sítě nebo volného místa/paměti Zotavitelná chyba, dojde k opakování se vzrůstajícími intervalů mezi opakované pokusy (1, 2, 4, 8, 10 a potom každých 30 minut).
4. **Plánovaná/neplánované převzetí služeb při selhání**: v případě potřeby můžete spustit plánované nebo neplánované převzetí služeb při selhání. Pokud vám nestalo plánované převzetí pak zdroje jsou VMs vypnout a zajistit, aby beze ztráty dat. Po vytvoření otevřené VMs se uloží do potvrdit nevyřízená. Potřebujete potvrdit jejich dokončení záložní, pokud jste s SAN replikace v takovém případě potvrdit automatické. Po primární webu do začátků navrácení může dojít. Pokud jste replikovat na Azure obráceném replikace je budou se automaticky. Jinak můžete zahájit obráceném replikace ručně.
 

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky

[Připravte pro nasazení](site-recovery-best-practices.md)
