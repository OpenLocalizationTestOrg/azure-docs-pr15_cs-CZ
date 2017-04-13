<properties
    pageTitle="Příprava obnovení webu nasazení | Microsoft Azure"
    description="Tento článek popisuje, jak připravit nasazení replikace s obnovení webu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Příprava obnovení webu Azure nasazení

V tomto článku najdete podrobný přehled nasazení požadavky pro každý replikace scénář podporované službou Azure obnovení webu. Po přečtení obecné požadavky pro jednotlivé scénáře, můžete propojit konkrétními podrobnosti každé nasazení.

Po přečtení tento příspěvek článek komentáře nebo dotazy vespodu tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak aplikace, pracovního vytížení a data zůstat spuštěn a k dispozici během plánované a neplánované výpadek služeb a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie. 

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzické servery a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Vyberte model nasazení

Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure modelu a model klasické služby správy. Azure má dvě portály – i [Azure klasické portál](https://manage.windowsazure.com/) , který podporuje modelu klasické nasazení a [Azure portál](https://ms.portal.azure.com/) s podporou pro oba modely nasazení.

Obnovení webu je k dispozici v portálu klasické a portálu Azure. Na portálu Azure klasické podporují obnovení webu s modelem klasické služby správy. Na portálu Azure může podporovat klasické modelu nebo správce prostředků nasazení. [Přečtěte si další](site-recovery-overview.md#site-recovery-in-the-azure-portal) informace o instalaci pomocí portálu Azure.

Když zvolíte poznámku nasazení modelu, který:

- Doporučujeme že použít [Azure portál](https://portal.azure.com) a modelem správce prostředků případného.
- Obnovení webu poskytuje jednodušší a intuitivnější Začínáme prostředí Azure portálu.
- Na portálu Azure, můžete replikovat počítačích klasické a správce prostředků úložiště v Azure. Kromě toho na portálu Azure můžete LRS nebo GRS úložiště účtů.
- Portál Azure sloučí zálohování a obnovení webu služby do jednoho trezoru služby Recovery tak, aby se daly nastavení a správa BCDR služeb z jednoho místa.
- Uživatelé, kteří mají Azure předplatná zřízení v aplikaci cloudové řešení poskytovatele (CSP) můžete spravovat teď operace obnovení webu Azure portálu.
- Replikace VMware VMs nebo pole fyzicky počítačích Azure na portálu Azure obsahuje mnoho nových funkcí včetně podpora premium úložiště a možnost vyloučit konkrétní disků z replikace.


## <a name="select-your-deployment-scenario"></a>Vyberte nasazení nefunguje

**Nasazení** | **Podrobnosti** | **Azure portálu** | **Klasický portálu** | **Prostředí PowerShell**
---|---|---|---|---
**VMware VMs Azure** | Replikovat VMware VMs k základnímu úložišti Azure | V Azure portál VMs můžete převzít classic nebo Správce úložiště<br/><br/> Nasazení [Azure portál](site-recovery-vmware-to-azure.md) poskytuje setkat i v případě optimalizovaného nasazení a řadu výhod funkce. | Na portálu Azure klasické můžete nasadit s [Rozšířené modelu](site-recovery-vmware-to-azure-classic.md) a převzít klasické Azure úložiště.<br/><br/> Je také starší verze modelu, který není určená k použití nového nasazení. |  Prostředí PowerShell není aktuálně podporován.
**VMs Hyper-V Azure** | VMs Hyper-V Azure úložiště. VMs může být na Hyper-V hostitelů spravovaných v System Center VMM mračnech nebo bez VMM. | V Azure portál VMs můžete převzít classic nebo Správce úložiště.<br/><br/> Portál Azure poskytuje možnosti optimalizovaného nasazení. [Další informace](site-recovery-vmm-to-azure.md) o replikace Hyper-V VMs v VMM mračnech. [Další informace](site-recovery-hyper-v-site-to-azure.md) o replikace Hyper-V VMs (bez VMM).| Na portálu klasické Azure může selhat VMs myší do klasického Azure úložiště | Prostředí PowerShell nasazení je podporovaná.
**Pole fyzicky serverů Azure** | Replikovat fyzické Windows/Linux servery k základnímu úložišti Azure | V Azure portálu servery můžete převzít classic nebo Správce úložiště.<br/><br/> Nasazení [Azure portál](site-recovery-vmware-to-azure.md) poskytuje prostředí optimalizovaného nasazení a řadu výhod funkce. | Na portálu Azure klasické můžete nasadit s [Rozšířené modelu](site-recovery-vmware-to-azure-classic.md) a převzít klasické Azure úložiště.<br/><br/> Je také starší verze modelu, který není určená k použití nového nasazení. | Nasazení prostředí PowerShell není aktuálně podporován.
**Servery VMware VMs/fyzické sekundární Web* | Sekundární web replikovat VMware VMs nebo pole fyzicky servery systému Windows nebo Linux. [Další informace a stahování](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) v uživatelské příručce InMage Scout. | Není k dispozici v portálu Azure | Na portálu klasické stáhnete InMage Scout z trezoru obnovení webu. | Nasazení prostředí PowerShell není aktuálně podporován
**VMs Hyper-V sekundární Web** | Replikovat Hyper-V VMs v VMM mračnech sekundární cloudu | [Azure portál](site-recovery-vmm-to-vmm.md) můžete replikovat Hyper-V VMs v VMM mračnech s webem vedlejší jen pomocí Hyper-V otevřené. SAN není aktuálně podporován. | Na portálu Azure klasické můžete Hyper-V VMs v VMM mračnech replikovat do vedlejší webu pomocí účtu [Hyper-V otevřené](site-recovery-vmm-to-vmm-classic.md) nebo [SAN replikace](site-recovery-vmm-san.md) | Podporované prostředí PowerShell nasazení



## <a name="check-what-you-need-for-deployment"></a>Kontrola, co potřebujete pro nasazení

### <a name="replicate-to-azure"></a>Replikovat na Azure

**Požadavek** | **Podrobnosti** 
---|---
**Účet Azure** | Musíte mít účet [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání. Replikace na Azure, musíte mít [účet Azure úložiště](../storage/storage-introduction.md).<br/><br/>Když nasazujete obnovení webu na portálu klasické musíte mít jeden nebo více [Standardní GRS úložiště účty](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Pokud jste nasazení Azure portálu můžete GRS nebo LRS úložiště.<br/><br/> Pokud jste replikace VMware VMs nebo pole fyzicky servery v úložišti Azure portálu premium je podporovaná. Všimněte si, že pokud používáte účet úložiště premium taky musíte účet standardní úložiště uložit protokoly replikace, které zachytit probíhající změny místních dat. [Úložiště Premium](../storage/storage-premium-storage.md) se obvykle používá pro virtuálních počítačích, které potřebují neustále vysoké ZVYŠUJÍ výkon a nízké latence hostitele vstupu a výstupu náročné úloh.<br/><br/> Pokud chcete používat účet premium k ukládání replikovanou dat, musíte taky účet standardní úložiště uložit protokoly replikace, které zachytit probíhající změny místních dat.
**Azure sítě** | Replikace na Azure, musíte mít Azure sítě, který Azure VMs se připojí k, když jste vytvořili po převzetí.<br/><br/> Když nasazujete klasické portálu použijete klasické sítě. Pokud jste nasazení na portálu Azure, můžete classic nebo správce sítě.<br/><br/> Síť musí být ve stejné oblasti jako trezoru.
**Mapování sítě (VMM na Azure)** | Pokud jste replikace z VMM na Azure, [mapování sítě](site-recovery-network-mapping.md) zaručuje, že Azure VMs připojeni k správné sítě po převzetí.<br/><br/> Nastavení sítě mapování musíte nakonfigurovat OM sítí v portálu VMM.
**Místní** | **VMware VMs**:, musíte mít v místním počítači spuštěná součástí obnovení webu, VMware vSphere tabulkami hosts a vCenter serveru a VMs chcete replikovat. [Přečtěte si další](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Pole fyzicky servery**: Pokud jste replikace fyzické servery budete potřebovat místního počítače se systémem součásti obnovení webu a fyzické servery chcete replikovat. [Přečtěte si další](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Pokud chcete po přepnutí Azure [navrácení](site-recovery-failback-azure-to-vmware.md) musíte infrastrukturu VMware to udělat.<br/><br/> **Hyper-V VMs**: Pokud chcete replikovat Hyper-V VMs v VMM Oblaka, musíte mít VMM serveru a nacházejí Hyper-V hosts, na které VMs chcete zamknout. [Přečtěte si další](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Pokud chcete replikovat Hyper-V VMs bez VMM musíte mít ve kterých jsou umístěny VMs tabulkami hosts Hyper-V. [Přečtěte si další](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Chráněný počítačích** | Chráněný stroje, které bude replikovat na Azure musí splňovat [Azure požadavky](#azure-virtual-machine-requirements) píše níže.


### <a name="replicate-to-a-secondary-site"></a>Replikovat na vedlejší Web

**Požadavek** | **Podrobnosti** 
---|---
**Účet Azure** | Musíte mít účet [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Místní** | **VMware VMs**: primární webu musíte mít proces serveru pro ukládání do mezipaměti, kompresi a šifrování replikace před odesláním sekundární Web. Sekundární webu nainstalujete konfigurační server pro správu a sledování nasazení a předlohy cílový server. Doporučujeme také vContinuum serveru pro snadnější správu. Kromě toho je třeba jeden nebo více vSphere tabulkami hosts VMware a volitelně vCenter serveru. [Další informace](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V VMs v VMM mračnech**: budete muset nastavit VMM servery a obsahující VMs tabulkami hosts Hyper-V chcete replikovat. [Přečtěte si další](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Mapování sítě (VMM k VMM)** | Pokud jste replikace z VMM do VMM, [mapování sítě](site-recovery-network-mapping.md) zajišťuje, aby otevřené VMs připojeni k odpovídající sítě po převzetí a jsou optimální umístěny na Hyper-V hosts. Nastavení sítě mapování musíte nakonfigurovat OM sítí na serverech VMM.
**Mapování úložiště** | Pokud jste replikace z VMM do VMM můžete volitelně nastavit [úložiště mapování](site-recovery-storage-mapping.md) můžete určit cílový úložiště pro replikovanou VMs. Úložiště mapování můžete nastavit pro Hyper-V otevřené a SAN replikace.<br/><br/> Mapování úložiště není aktuálně podporován v portálu Azure.


## <a name="verify-url-access"></a>Ověření adresy URL přístup

Zkontrolujte, jestli že jsou servery k dispozici tyto adresy URL.

**ADRESA URL** | **VMM VMM** | **VMM Azure** | **Hyper-V Azure** | **VMware Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Povolit | Povolit | Povolit | Povolit
*. backup.windowsazure.com | Nevyžaduje | Povolit | Povolit | Povolit
*. hypervrecoverymanager.windowsazure.com | Povolit | Povolit | Povolit | Povolit
*. store.core.windows.net | Povolit | Povolit | Povolit | Povolit
*. blob.core.windows.net | Nevyžaduje | Povolit | Povolit | Povolit
https://www.msftncsi.com/ncsi.txt | Povolit | Povolit | Povolit | Povolit
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nevyžaduje | Nevyžaduje | Nevyžaduje | Povolit

## <a name="azure-virtual-machine-requirements"></a>Požadavky na Azure virtuálního počítače

Obnovení webu replikovat virtuálních počítačích a fyzické serverech musí běžet žádný operační systém nepodporuje Azure nástroje můžete nasazovat. Jedná se o Většina verzí systému Windows a Linux. Musíte zajistit, místním virtuálních počítačích, které chcete zamknout v souladu s požadavky na Azure.


**Funkce** | **Požadavky** | **Podrobnosti**
---|---|---
Host (hostitel) pro Hyper-V | By měla běžet Windows serveru 2012 R2 | Zjistit předpoklady pro kontrolu se nezdaří, pokud nepodporovaný operační systém
VMware hypervisoru  | Podporované operační systém | [Kontrola požadavků na](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Host operační systém | Pro Hyper-V Azure replikace: obnovení webu podporuje všech operačních systémech, které jsou [podporované kodérem Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Pro VMware a replikace fyzické serveru: Zkontrolujte Windows a Linux [požadavky](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Kontrola požadavky selže, když není podporován.
Architektura operačního systému hosta | 64bitová verze | Předpoklady zaškrtnutí selže, pokud není podporována
Velikost disku operační systém |  Až 1023 GB | Předpoklady zaškrtnutí selže, pokud není podporována
Počet disků operační systém | 1 | Kontrola požadavky selže, když není podporován.
Počet disku dat | 16 nebo méně (výchozí hodnota je funkce velikost virtuálního počítače vzniku. 16 = XL) | Předpoklady zaškrtnutí selže, pokud není podporována
Velikost virtuální pevný disk disku dat | Až 1023 GB | Předpoklady zaškrtnutí selže, pokud není podporována
Síťové adaptéry | Podporuje více adaptéry |
Statické IP adresy | Podporované | Pokud primární virtuální počítač používá statickou IP adresa zadejte statickou IP adresu virtuální počítač, který se vytvoří Azure. Všimněte si, že statickou IP adresu počítače virtuální linux spuštěných pro Hyper-v nepodporuje.
iSCSI disk | Nepodporovaná funkce | Předpoklady zaškrtnutí selže, pokud není podporována
Sdílené virtuální pevný disk | Nepodporovaná funkce | Předpoklady zaškrtnutí selže, pokud není podporována
FC disku | Nepodporovaná funkce | Předpoklady zaškrtnutí selže, pokud není podporována
Formát pevném disku.| VIRTUÁLNÍ PEVNÝ DISK <br/><br/> VHDX | I když VHDX není aktuálně podporován v Azure, obnovení webu automaticky převede VHDX virtuálního pevného disku při můžete převzít Azure. Když se zpátky do místního nasazení virtuálních počítačích dál používat formát VHDX.
Nástroje BitLocker | Nepodporovaná funkce | Nástroje BitLocker musíte zakázat před ochrana virtuálního počítače.
Název virtuálního počítače| Mezi 1 až 63 znaků. Omezit na písmena, číslice a spojovníky. Zahájení a ukončení s písmeno nebo číslici | Aktualizovat hodnotu ve vlastnosti virtuálního počítače v obnovení webu
Typ Virtual počítače | <p>Analytický nástroj Generátor 1</p> <p>Analytický nástroj Generátor 2 – Windows</p> |  Generování 2 virtuálního počítače s typem disku OS základní disku, který obsahuje 1 nebo 2 objemů dat s formátem disku jako VHDX, která je menší než 300GB podporované. Analytický nástroj Generátor 2 Linux virtuálních počítačích nejsou podporované. [Přečtěte si další informace](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optimalizace nasazení

Následující tipy použijte můžete optimalizovat měřítko nasazení.

- **Velikost svazku operační systém**: když replikovat virtuálního počítače na Azure hlasitost operační systém musí být menší než 1 TB. Pokud máte víc svazky než to můžete ručně je přesunout na jiné diskové před zahájením nasazení.
- **Data disk velikost**: Pokud jste replikace na Azure můžete mít až 32 disků dat na počítač virtuální, oba objekty mají do velikosti 1 TB. Můžete efektivně replikovat a převzetí virtuálního počítače ~ 32 TB.
- **Obnovení plán limity**: obnovení webu můžete přizpůsobit tisíce virtuálních počítačích. Obnovení plány jsou určeny jako model aplikací, které by měl převzít společně tak jsme omezení počtu počítačích podle plánu obnovení 50.
- **Omezení služby Azure**: všechny Azure předplatné je součástí sady výchozí omezení na jádra, cloudových služeb atd. Doporučujeme spouštět přepojení test k ověření dostupnosti zdrojů ve vašem předplatném. Můžete změnit následující omezení přes Azure podpory.
- **Plánování kapacity**: Přečtěte si o [plánování kapacity](site-recovery-capacity-planner.md) pro obnovení webu.
- **Replikace pásma**: Pokud jste krátké na šířku pásma replikace Všimněte si, že:
    - **ExpressRoute**: obnovení webu spolupracuje Azure ExpressRoute a WAN optimizers například Riverbed. [Přečtěte si další](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) informace o ExpressRoute.
    - **Replikace**: použití obnovení webu provede inteligentní počáteční replikace pomocí pouze data bloků a není celém virtuální pevný disk. Pouze změny jsou replikovat během probíhající replikace.
    - **V síti**: můžete určit používaný replikace nastavení [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) zásady na základě cílová IP adresa a port v síti.  Kromě toho pokud jste replikace k obnovení webu Azure pomocí agenta Azure zálohování můžete nakonfigurovat omezení, které agenta. [Přečtěte si další](https://support.microsoft.com/kb/3056159).
- **RTO**: změřit cíle obnovení čas (RTO) můžete očekávat s obnovení webu doporučujeme spusťte test selhání a zobrazení úloh obnovení webu a analyzujte data kolik času trvat operace. Pokud jste nedaří myši na Azure, pro nejlepší RTO doporučujeme automatizovat všechny ruční akce tak, že integrace s Azure plány automatizaci a obnovení.
- **Operace RPO**: obnovení webu podporuje cíle bodu v blízkosti synchronní obnovení (operace RPO) při replikovat na Azure. Předpokladem dostatečnou šířku pásma mezi datacentra a Azure.


##<a name="service-urls"></a>Adresy URL služby
Zkontrolujte, jestli tyto adresy URL přístupných osobám s postižením ze serveru


**Adresy URL** | **VMM VMM** | **VMM Azure** | **Web Hyper-V Azure** | **VMware Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 \*. backup.windowsazure.com |  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 \*. hypervrecoverymanager.windowsazure.com | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 \*. store.core.windows.net | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 \*. blob.core.windows.net |  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 https://www.msftncsi.com/ncsi.txt | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup  | Vyžadují přístup
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Vyžadují přístup


## <a name="next-steps"></a>Další kroky

Po učení a porovnávání obecné nasazení požadavky můžete číst podrobné požadavky a začněte nasazení jednotlivé scénáře.

- [Replikace VMware virtuálních počítačích na Azure](site-recovery-vmware-to-azure-classic.md)
- [Replikace fyzické servery na Azure](site-recovery-vmware-to-azure-classic.md)
- [Replikace Hyper-V server v VMM mračnech na Azure](site-recovery-vmm-to-azure.md)
- [Replikace Hyper-V virtuálních počítačích (bez VMM) na Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replikovat VMs Hyper-V sekundární Web](site-recovery-vmm-to-vmm.md)
- [Replikovat VMs Hyper-V sekundární web s SAN](site-recovery-vmm-san.md)
- [Replikovat VMs Hyper-V jednom VMM serveru](site-recovery-single-vmm.md)
