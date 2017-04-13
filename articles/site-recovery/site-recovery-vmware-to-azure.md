<properties
    pageTitle="Replikace VMware virtuálních počítačích a fyzické servery na Azure s obnovení webu Azure na portálu Azure | Microsoft Azure"
    description="Popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení místní VMware virtuálních počítačích a Windows/Linux fyzických serverů Azure pomocí portálu Azure"
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
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Souběžné VMware virtuálních počítačích a fyzické stroje Azure s obnovení webu Azure pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmware-to-azure.md)
- [Klasický Azure](site-recovery-vmware-to-azure-classic.md)
- [Azure klasické (starší verze)](site-recovery-vmware-to-azure-classic-legacy.md)

Vítá vás obnovení Azure webu! Pokud chcete replikovat místní VMware virtuálních počítačích nebo pole fyzicky servery Windows/Linux Azure pomocí obnovení webu Azure na portálu Azure pomocí tohoto článku.

> [AZURE.NOTE] Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Azure zdroje správce (ARM) a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení.

Obnovení webu Azure portálu nabízí celou řadu nových funkcí:

- Služby Azure zálohování a obnovení webu Azure jsou sloučena do jednoho trezoru služby Recovery tak, aby se nastavení a správa nepřerušený a obnovení (BCDR) z jednoho místa. V řídicím panelu jednotné můžete sledovat a spravovat operace různých místních webů a Azure veřejné cloudu.
- Uživatelé, kteří mají Azure předplatná zřízení v aplikaci cloudové řešení poskytovatele (CSP) můžete spravovat teď operace obnovení webu Azure portálu.
- Obnovení webu Azure portálu replikovat počítačích ARM úložiště účty. Obnovení webu na selhání vytvoří procesor arm VMs v Azure.
- Obnovení webu i nadále podporuje replikace k účtům klasické úložiště. Obnovení webu na selhání vytvoří VMs pomocí klasického modelu.

Po přečtení tento příspěvek článek komentář dole v komentářích Disqus. Zeptejte se technické ve [Fóru služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie.

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzických serverů a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)

Tento článek obsahuje všechny informace, které je potřeba replikovat místní VMware VMs a fyzických serverů Azure Windows nebo Linux. Obsahuje přehled architektury plánování informace a nasazení kroků pro konfiguraci Azure místní servery, nastavení replikace a plánování kapacity. Poté, co jste nastavili infrastruktury můžete povolit replikace v počítačích, které chcete zamknout a zkontrolujte, že tento převzetí funguje.

## <a name="business-advantages"></a>Výhody Business

- Obnovení webu poskytuje mimo ochranu firmy pracovního vytížení a spuštěné v VMware VMs a fyzické servery.
- Portál služeb obnovení obsahuje jedno místo pro nastavení a správa a sledování replikace, převzetí a obnovení.
- Obnovení webu, mohli automaticky zjišťovat VMware VMs přidána do vSphere tabulkami hosts.
- Můžete snadno spustit převzetí služeb při selhání z místního infrastruktury Azure a navrácení (obnovení) z Azure VMware OM servery v místní síti.
- Můžete povolit více OM a vytváření skupin replikace tak, aby aplikace úloh vrstveny napříč několika počítačích replikace ve stejnou dobu. Všechny počítače ve skupině replikace mají pád konzistentní a konzistentní aplikace obnovení body při jejich převzít. Pro překlopení můžete získat víc počítačů v plánech obnovení tak, aby společně selhání pracovního vytížení vrstvené aplikace.

## <a name="supported-operating-systems"></a>Podporované operační systémy

### <a name="windows64-bit-only"></a>Windows (64-bit pouze)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows serveru 2012 R2

### <a name="linux-64-bit-only"></a>Linux (64-bit pouze)
- Červené klobouk Enterprise Linux 6,7, 7.1, 7.2
- CentOS 6.5, 6.6 6,7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Scénář architektura

Toto jsou součástí scénáře:

- **Konfigurace serveru**: místního počítače, který souřadnic komunikace a spravuje dat replikace a obnovení procesů. V tomto počítači spustíte jednoho instalační soubor nainstalujte konfigurační server a tyto komponenty:
    - **Proces serveru**: slouží jako replikace brány. Přijímá replikace data z počítače chráněného zdroj, optimalizuje ukládání do mezipaměti, komprese a šifrování a odešle Azure úložiště. Je taky zpracovává nabízených instalace služby mobilita chráněné počítačích a provádí automatické zjišťování VMware VMs. Výchozí proces server je nainstalovaný na konfigurační server. Můžete nasadit další samostatný proces servery zobrazit nasazení.
    - **Hlavní cílový server**: zpracovává replikace dat během navrácení z Azure.

- **Nastavení mobilních zařízení služby**: součást nasazený na všechny počítače (VMware OM nebo pole fyzicky server), které chcete replikovat Azure. Zaznamenává zápis dat v počítači a předá ho server procesu.
- **Azure**: není potřeba vytvářet všechny Azure VMs zpracovávání replikace a přepnutí Azure.  Potřebujete předplatné Azure účet Azure úložiště pro ukládání replikovanou dat a Azure virtuální sítě tak, aby Azure VMs připojení k síti po překlopení. Úložiště klienta a sítě musí být ve stejné oblasti jako služby Recovery trezoru.
- **Navrácení**: když budete chtít po selhání selhat na svůj místní web zpátky z Azure, musíte vytvořit Azure OM jako dočasné proces server. Po dokončení navrácení můžete ho odstranit. Pro navrácení taky musíte mít virtuální privátní sítě (nebo Azure ExpressRoute) připojení mezi místních webů a Azure síti, ve kterém jsou umístěny Azure VMs. Pokud navrácení přenosy je hodně může taky musíte nastavení vyhrazené předlohy na cílový počítači místní. Světlejší přenosů lze použít výchozí server předlohy cílové spuštěna konfigurační server.


Obrázek zobrazuje interakce tyto součásti.

![Architektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Obrázek 1: VMware/fyzické na Azure**

## <a name="azure-prerequisites"></a>Požadavky na Azure

Tady je, co budete potřebovat v Azure nasazení tento scénář.

**Předpoklady** | **Podrobnosti**
--- | ---
**Účet Azure**| Musíte mít účet [Microsoft Azure](http://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání. <br/><br/>Pro uložení dat musíte mít účet standardní nebo premium úložiště ve stejné oblasti jako služby Recovery trezoru.<br/><br/>Můžete použít LRS nebo GRS účtu úložiště. Doporučujeme, abyste GRS tak, aby data pružné místní výpadek, zda oblasti primární možné obnovit. [Další informace](../storage/storage-redundancy.md).<br/><br/> [Úložiště Premium](../storage/storage-premium-storage.md) se obvykle používá pro virtuálních počítačích, které potřebují neustále vysoké ZVYŠUJÍ výkon a nízké latence hostitele vstupu a výstupu náročné úloh.<br/><br/> Pokud chcete používat účet premium k ukládání replikovanou dat, musíte taky účet standardní úložiště uložit protokoly replikace, které zachytit probíhající změny místních dat.<br/><br/> Všimněte si, že úložiště účtů vytvořených v portálu Azure nejdou přesouvat mezi skupinami zdroje. Ochrana účtům premium úložiště v centrální Indie a států Jižní Indie není aktuálně podporuje i.<br/><br/> [Přečtěte si informace o](../storage/storage-introduction.md) Azure úložiště.
**Azure sítě** | Budete potřebovat Azure virtuální sítě, který Azure VMs připojí k selháním. Azure virtuální sítě musí být ve stejné oblasti jako služby Recovery trezoru.
**Navrácení z Azure** | Budete potřebovat dočasný server procesu nastavení jako OM Azure. To můžete vytvořit, když budete chtít navrácení a odstraňte ji po ukončení selhání zpět.<br/><br/> Selhání zpět musíte připojení VPN (nebo Azure ExpressRoute) z Azure sítě k místnímu webu.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Konfigurace serveru / rozšiřování požadavky obrázku

Můžete se nastavit v místním počítači jako konfigurační server / škálování server procesu.

**Předpoklady** | **Podrobnosti**
--- | ---
**Konfigurace serveru**| Potřebujete fyzické místní nebo virtuálního počítače se systémem Windows Server 2012 R2. Všechny prvky obnovení webu místní nainstalovaných v počítači.<br/><br/>Replikace VMware OM doporučujeme že nasazení serveru jako vysoce dostupné OM VMware. Pokud jste replikace fyzických počítačů může být počítači fyzický server.<br/><br/> Překlopení zpět k místnímu webu z Azure je vždy VMware VMs bez ohledu na to, jestli je selhání VMs nebo pole fyzicky servery. Pokud nemáte nasadíte konfigurační server jako VMware OM, budete muset nastavit server oddělené předlohy cílové jako VMware OM pro příjem navrácení vysílání.<br/><br/>Pokud je server VMware OM, třeba typ adaptér sítě VMXNET3. Pokud používáte jiný typ síťový adaptér musíte nainstalovat [aktualizace VMware](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) vSphere 5.5 serveru.<br/><br/>Server by měla být statické IP adresy.<br/><br/>Server by neměly být řadiče domény.<br/><br/>Název hostitele serveru by měl být 15 znaků nebo menší.<br/><br/>Operační systém by měl být jenom angličtiny.<br/><br/> Budete muset nainstalovat VMware vSphere PowerCLI 6.0. Konfigurace serveru.<br/><br/>Konfigurační server vyžaduje přístup k Internetu. Odchozí přístup požaduje následujícím způsobem:<br/><br/>Dočasný přístup na HTTP 80 během instalace součásti obnovení webu (Pokud si chcete stáhnout MySQL)<br/><br/>Probíhající přístup pro odchozí připojení na HTTPS 443 správy replikace<br/><br/>Probíhající přístup pro odchozí připojení na HTTPS 9443 přenosů replikace (Tento port lze upravit)<br/><br/>Server bude potřebovat přístup k následujícím adresy URL tak, aby se můžete připojit k Azure: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>Pokud máte IP adresu brány firewall pravidla na serveru, zkontrolujte, jestli tato pravidla sdělení Azure. Musíte povolit [Rozsahy IP Azure Datacentra](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (443).<br/><br/>Počkejte, až rozsahy IP adres pro Azure oblast předplatného a západní USA.<br/><br/>Povolit tuto adresu URL pro stažení MySQL:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>Předpoklady vCenter/vSphere hostitele VMware

**Předpoklady** | **Podrobnosti**
--- | ---
**vSphere**| Potřebujete jednu nebo více hypervisory vSphere VMware.<br/><br/>Hypervisory by měl být verzí vSphere 6.0, 5.5 nebo 5.1 s nejnovějšími aktualizacemi.<br/><br/>Doporučujeme, aby vSphere tabulkami hosts a vCenter servery jsou umístěny ve stejné síti jako obrázku server (to bude síti, ve kterém je umístěn konfigurační server, pokud jste nastavili vyhrazené proces serveru).
**vCenter** | Doporučujeme nasazení serveru vCenter VMware ke správě vSphere hosts. Ho by měl být verzí vCenter 6.0 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/>Poznámka: obnovení webu, které nejsou podporovány nové vCenter vSphere 6.0 funkcí jako křížové vCenter vMotion virtuální svazky a úložiště DRS. Podpora obnovení webů se omezí na funkce, které byly také k dispozici ve verzi 5.5.


## <a name="protected-machine-prerequisites"></a>Požadavky na zamknutém počítače

**Předpoklady** | **Podrobnosti**
--- | ---
**Místní (VMware VMs)** | VMware VMs, které chcete zamknout měli VMware nástroje nainstalovanou a spuštěnou.<br/><br/> Stroje, které chcete zamknout by měly odpovídat s [Azure požadavky](site-recovery-best-practices.md#azure-virtual-machine-requirements) pro vytváření Azure VMs.<br/><br/>Jednotlivé kapacita disku chráněné počítačích nesmí být více než 1023 GB. Virtuálního počítače můžete mít až 64 disků (tedy až 64 TB). <br/><br/>Minimální 2 GB volného místa na disku instalace pro instalaci součásti.<br/><br/>Ochrana VMs s šifrované disků nepodporuje.<br/><br/>Sdílené hosta disku, které nejsou podporovány clusterů.<br/><br/>**Port 20004** otevřen, na zamknutém virtuálního počítače místní brána firewall, pokud chcete povolit **konzistence aplikace**.<br/><br/>Strojů, které mají Unified Extensible Firmware rozhraní (UEFI) / spouštěcí Extensible Interface(EFI) firmwaru není podporovaná.<br/><br/>Názvy by měl obsahovat mezi 1 až 63 znaků (písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslem a končit písmeno nebo číslici. Poté, co jste povolili replikace pro počítač Azure název lze upravit.<br/><br/>Pokud má zdroj OM týmové NIC se převede na jeden NIC po přepnutí Azure.<br/><br/>Pokud chráněné virtuálních počítačích disku iSCSI obnovení webu převede chráněné disku iSCSI OM do souboru virtuálního pevného disku při OM selhání Azure. Pokud cíl iSCSI zastižení OM Azure se k ní připojit a v podstatě najdete v článku dvou nebo více discích – disk virtuální pevný disk na OM Azure a disku iSCSI zdroje. V tomto případě musíte odpojit cíle iSCSI, která se zobrazí na OM Azure.
**Počítačích Windows (fyzické nebo VMware)** | Počítači by měl být podporované 64-bit operačním systémem: Windows serveru 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 s na nejnižších SP1.<br/><br/> Operační systém, měli byste mít nainstalované na disku C:\. OS disk by měl být základní disk Windows a ne dynamické. Disk dat může být dynamické.<br/><br/>Obnovení webu podporuje VMs s diskem RDM. Během navrácení obnovení webu opakovaně používat disku RDM Pokud původní zdrojový OM a RDM disk je k dispozici. Pokud nejsou k dispozici, během navrácení obnovení webu vytvoří nový soubor VMDK pro každý disk.
**Linux počítačích** (phyical nebo VMware)|  Budete potřebovat podporované 64bitové operační systém: červená klobouk Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts soubory chráněné počítačích smí obsahovat položky, které namapujte název místní hostitele IP adresy přidružené k některé síťové adaptéry.<br/><br/>Pokud chcete připojit k Azure virtuální počítači spuštěná Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), ověřit, že služba zabezpečené Shell na zamknutém počítač je nastavena na spustit automaticky při spuštění systému a že povolit pravidla brány firewall ssh připojení k ní.<br/><br/>Název hostitele, připojovací body, názvy zařízení a Linux systémových cest a názvy souborů (například/atd /; USR) třeba v angličtině pouze.<br/><br/>Ochrana lze povolit pouze pro Linux počítačích s následující úložiště: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.<br/><br/>Obnovení webu podporuje VMs s diskem RDM.  Během navrácení Linux nemá obnovení webu znovu použít RDM disku. Místo toho vytvoří nový soubor VMDK pro každý odpovídající RDM disk.<br/><br/>Ověřte, zda je nastavení disk.enableUUID=true v konfiguraci parametry OM v VMware. Vytvořte položku, pokud ho neexistuje. Je potřeba zajistit konzistentní UUID VMDK tak, aby správně připojí. Přidání toto nastavení také zaručuje, že se změny pouze delta přenesených zpátky do místního nasazení během navrácení a ne celou replikace.
**Nastavení mobilních zařízení služby** |  **Windows**: bude automaticky posunout službu mobilita VMs s Windows, budete muset poskytnout účtu správce (místní správce v počítači Windows), aby server obrázku můžete udělat nabízených instalace.<br/><br/>**Linux**: Automaticky posunout službu mobilita VMs systémem Linux bude nutné vytvořit účet, který lze serverem proces provést instalaci připínáčku.<br/><br/> Ve výchozím nastavení jsou replikovat všech discích na počítač. [Vyloučení disk z replikace](#exclude-disks-from-replication)musí být službu mobilita ručně nainstalovaný v počítači dříve než povolíte replikace.<br/>

## <a name="prepare-for-deployment"></a>Připravte pro nasazení

Příprava na nasazení budete potřebovat:

1. [Nastavení služby Azure sítě](#set-up-an-azure-network) ve kterém Azure VMs budou umístěné když jste nespředený nahoru po překlopení. Kromě toho pro navrácení musíte nastavit připojení k síti VPN (nebo Azure ExpressRoute) z Azure sítě na svůj místní web.
2. [Nastavit účet Azure úložiště](#set-up-an-azure-storage-account) pro replikovanou data.
3. [Příprava účet](#prepare-an-account-for-automatic-discovery) na serveru vCenter nebo vSphere hostuje tak, aby obnovení webu mohli automaticky zjišťovat VMs VMware, se přidají.
4. [Příprava konfigurační server](#prepare-the-configuration-server) tak, aby můžete zpřístupnit požadované adresy URL a nainstalovat vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Nastavte si síť Azure

- Síť by měl být stejný Azure oblasti, ve kterém se nasadit služby Recovery trezoru.
- V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavíte Azure sítě [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasického režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Selhání zpět z Azure na svůj místní web VMware musíte připojení VPN (nebo připojení k Azure ExpressRoute) z Azure sítě, ve kterém jsou umístěny, v místní síti, ve kterém se nachází konfigurační server replikovanou VMs Azure.
- [Přečtěte si víc o](../vpn-gateway/vpn-gateway-site-to-site-create.md) podporovaných nasazení modely pro připojení VPN k webu a jak [nastavit připojení](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Můžete také nastavit [Azure ExpressRoute](../expressroute/expressroute-introduction.md). [Další informace](../expressroute/expressroute-howto-vnet-portal-classic.md) o nastavení Azure sítě s ExpressRoute.

> [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.

### <a name="set-up-an-azure-storage-account"></a>Nastavit účet Azure úložiště

- Musíte mít standardní nebo účet premium Azure úložiště pro uložení dat replikovat na Azure. Účet musí být ve stejné oblasti jako služby Recovery trezoru. V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavíte účet v [ARM](../storage/storage-create-storage-account.md) nebo [klasického režimu](../storage/storage-create-storage-account-classic-portal.md).
- Pokud používáte účet premium replikovanou dat je potřeba vytvořit další standardní účet uložit protokoly replikace, které zachytit probíhající změny místních dat.  

> [AZURE.NOTE] [Migrace z úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

### <a name="prepare-an-account-for-automatic-discovery"></a>Příprava účet automatického zjišťování

Server procesu obnovení webu mohli automaticky zjišťovat VMware VMs vSphere tabulkami hosts nebo na serveru vCenter, která spravuje hosts. Provádět automatické zjišťování obnovení webu pověření, které zpřístupníte VMware serveru. Toto není důležité, pokud jste replikace fyzických počítačů.

1. Můžete vytvořit účet vyhrazené pro automatické zjišťování roli (například Azure_Site_Recovery) s [potřebná oprávnění](#vmware-account-permissions)na úrovni vCenter.
2. Vytvoření nového uživatele na serveru host nebo vCenter vSphere a uživateli přiřadit roli. Budete později nechat vědět o těchto přihlašovacích údajů, se kterými můžete provádět automatické zjišťování obnovení webu.

    >[AZURE.NOTE] Uživatelský účet vCenter s rolí jen pro čtení mohlo by umožnit spuštění převzetí ale nelze ukončit chráněné zdroj počítačích. Pokud chcete vypnout těchto počítačů musíte roli [Azure_Site_Recovery](#vmware-account-permissions) . Pokud máte jenom migrace VMs z VMware Azure a nemusíte navrácení dostatečné je roli jen pro čtení.

### <a name="prepare-the-configuration-server"></a>Příprava konfigurační server

1.  Ujistěte se, že počítač, který používáte pro konfigurační server splňuje [požadavky](#configuration-server-prerequisites). Zejména zkontrolujte, že počítač připojení k Internetu Tato nastavení:

    - Povolení přístupu k tyto adresy URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - Povolení přístupu k [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) ke stažení MySQL.
    - Povolte komunikaci brány firewall pro Azure s [Azure datacentra rozsahy IP adres](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (443).

2.  Stažení a instalace [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) na konfigurační server. (Aktuálně jiných verzích PowerCLI nepodporuje doplňky, včetně R verzích verze 6.0.)


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru obnovení služby

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **správy** > **zálohování a obnovení webu (OMS)**. Můžete taky můžete kliknutím na **Procházet** > **Obnovení služby trezoru** > **Přidat**.

    ![Nové trezoru](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Do pole **název** zadejte popisný název k identifikaci trezoru. Pokud máte víc předplatných, vyberte jednu z nich.
4. [Vytvoření nové skupiny prostředků](../resource-group-template-deploy-portal.md) nebo vyberte stávající. Zadejte Azure oblast. K této oblasti bude replikovat počítačích. Všimněte si, že Azure úložiště a sítěmi pro obnovení webu, musí být ve stejné oblasti. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Pokud chcete rychle zobrazit trezoru na řídicím panelu klikněte na **Připnout k řídicího panelu** a pak klikněte na **vytvořit**.

    ![Nové trezoru](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Nové trezoru se zobrazí na **řídicím panelu** > **všechny zdroje**a na hlavní **služby Recovery trezorů** zásuvné.

## <a name="getting-started"></a>Začínáme

Obnovení webu poskytuje Začínáme setkat i v případě navržené tak, abyste mohli a systémem co nejdříve. Zkontroluje, požadavky a vás provede kroky budete potřebovat sadu obnovení webu nasazený.

Vyberete typ strojů chcete replikovat a místo, kam chcete replikovat do. Nastavení infrastruktury, včetně místní servery Azure nastavení, zásady replikace a plánování kapacity. Po infrastrukturu na místě je povolit replikace VMs a fyzické serverů. Můžete spustit převzetí služeb při selhání pro konkrétní počítače nebo vytvořit plány obnovy překlopení více počítačů.

Začněte Začínáme – zvolte, jak chcete nasadit obnovení webu. Začínáme toku změny mírně v závislosti na vašim požadavkům replikace.


## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Volba cíle ochrany

Vyberte, co chcete replikovat a místo, kam chcete replikovat do.

1. V zásuvné **služby Recovery trezorů** vyberte trezoru a klikněte na **Nastavení**.
2. V **dialogovém okně Nastavení** > **Začínáme** klikněte na **Obnovení webu** > **Krok 1: Příprava infrastruktury** > **cíl zámek**.

    ![Volba cíle](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. V **ochranu hledání** vyberte **Azure na**a vyberte **Ano, pomocí VMware vSphere hypervisoru**. Klikněte na **OK**.

    ![Volba cíle](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Nastavení zdrojového prostředí

Nastavení konfigurační server a zaregistrovat služby Recovery trezoru. Pokud jste replikace VMware VMs zadat VMware účet, kterou používáte pro automatické zjišťování.

1. Klikněte na **Krok 1: Příprava infrastruktury** > **zdroje**. **Příprava** zdroje Pokud nemáte konfigurační server klikněte na **+ konfigurační server** ho vytvořit.

    ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source1.png)

2. Při kontrole zásuvné **Přidat Server** **Konfigurační Server** se zobrazí na **serveru zadejte**.
3. Než nastavíte konfigurační server zkontrolujte [požadavky](#configuration-server-prerequisites). V počítači přístup k požadované URL konkrétní kontroly.
4.  Nastavení služby Unified obnovení webů instalace budou moct soubor stáhněte.
5.  Stáhněte si klávesu registrace trezoru. Musíte mít takto při instalaci služby Unified. Klíč platí 5 dní po jeho vytvoření.

    ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  V počítači používáte jako konfigurační server, spusťte instalaci služby Unified nainstalovat konfigurace serveru, serveru obrázku a předlohy cílový server.


### <a name="run-site-recovery-unified-setup"></a>Obnovení spuštění webu Unified instalačního programu

1.  Spuštění instalačního souboru Unified nastavení.
2.  **Než začnete** vyberte **nainstalovat konfigurační server a proces server**.

    ![Než začnete](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. V **Jiných výrobců softwarová licence** klikněte na **mám přijmout** si stáhněte a nainstalujte MySQL.

    ![Třetí = software výrobce](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. V **Registrace** Procházet a vyberte klávesu registrace, kterou jste si stáhli z trezoru.

    ![Registrace](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. V **Dialogovém okně nastavení Internet** určete, jak bude poskytovatele na serveru konfigurace připojení k obnovení webu Azure přes internet.

    - Pokud se chcete připojit k proxy, která je aktuálně nastavena na počítači vyberte **připojit k existující nastavení proxy**.
    - Pokud chcete poskytovatele, který má přímé připojení vyberte **připojení přímo bez proxy**.
    - Pokud existující proxy server vyžaduje ověření a chcete použít vlastní proxy poskytovatel připojení vyberte **připojení s nastavením proxy serveru vlastní**.
        - Pokud používáte vlastní proxy, budete muset zadat adresu, portů a přihlašovací údaje
        - Pokud používáte proxy server, kterou jste měli už povolili adresy URL podle [požadavky](#configuration-server-prerequisites).

    ![Brána firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Zkontrolujte požadavky na** instalaci spuštěn zaškrtnutí a ujistěte se, mohlo by umožnit spuštění instalace. Pokud se zobrazí upozornění o **globální čas synchronizace zkontrolujte** ověřte čas na systémové hodiny (nastavení**data a času** ) se stejná jako časové pásmo.

    ![Zjistit předpoklady pro](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. **Konfigurace MySQL** Vytvořte přihlašovací údaje pro přihlášení k instanci server MySQL, která budou nainstalovány.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. V poli vyberte **Podrobnosti prostředí** jestli budete chtít replikovat VMware VMs. Pokud se nastavení zkontroluje nainstalované PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. V poli **Umístění instalace** vyberte místo, kam chcete nainstalovat binární soubory a ukládání do mezipaměti. Můžete vybrat jednotku, která obsahuje alespoň na úrovni 5 GB úložiště, ale doporučujeme jednotku mezipaměti s nejméně 600 GB volného místa.

    ![Umístění instalace](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. **Network** výběru určete posluchače (síťový adaptér a SSL port) ve kterém bude konfigurační server odesílat a přijímat replikace data. Můžete změnit výchozí port (9443). Kromě tento port port 443 se bude používat tak, že na webový server, který orchestrates replikace operace. pro příjem replikace není vhodné používat 443.


    ![Výběr sítě](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  V **souhrnu** zkontrolujte informace a klikněte na tlačítko **nainstalovat**. Po dokončení instalace vygeneruje se přístupové heslo. Budete potřebovat, když povolíte replikace tak zkopírujte ho a udržovat její na zabezpečeném místě.

    ![Souhrn](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Po registraci dokončení serveru se zobrazí v seznamu **Nastavení** > zásuvné**servery** v trezoru.



#### <a name="run-setup-from-the-command-line"></a>Spuštění instalačního programu z příkazového řádku

Můžete nastavit konfigurační server z příkazového řádku:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametry:

- / ServerMode: povinný. Určuje, zda by měly být nainstalovány konfigurace a proces servery nebo jenom server obrázku. Vstupní hodnoty: CS, PS.
- InstallLocation: povinný. Složka, ve kterém jsou nainstalované součásti.
- / MySQLCredsFilePath. Povinný. Cesta k souboru, ve kterém jsou uložena pověření server MySQL. Soubor by měl být v tomto formátu:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Povinný. Umístění souboru trezoru přihlašovací údaje
- / EnvType. Povinný. Typ instalace. Hodnoty: VMware NonVMware
- / PSIP a /CSIP. Povinný. IP adresu serveru obrázku a konfigurace serveru.
- / PassphraseFilePath. Povinný. Umístění souboru heslo.
- / BypassProxy. Volitelné. Určuje, že konfigurace serveru připojuje k Azure bez na proxy server.
- / ProxySettingsFilePath. Volitelné. Nastavení proxy serveru (výchozí proxy server vyžaduje ověření nebo vlastní proxy). Soubor by měl být v tomto formátu:
    - [ProxySettings]
    - ProxyAuthentication = "Ano/Ne"
    - Proxy IP = "IP adresu >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Volitelné. Číslo portu pro data replikace.
- SkipSpaceCheck. Volitelné. Přeskočit místo vyhledat mezipaměti.
- AcceptThirdpartyEULA. Povinný. Příznak znamená přijetí třetí strany EULA.
- ShowThirdpartyEULA. Povinný. Zobrazí EULA třetích stran. Pokud jako vstupní všechny ostatní parametry jsou ignorovány.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Přidání účtu VMware použít pro automatické zjišťování

 Když jste připraveni nasazení byste si měli [vytvořený účet služby VMware](#prepare-an-account-for-automatic-discovery) využívající obnovení webu pro automatické zjišťování. Tento účet přidáte takto:

1. Otevřete **CSPSConfigtool.exe**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
2. Klikněte na **Správa účtů** > **Přidat účet**.

    ![Přidání účtu](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Podrobně **Účet** přidejte účet, který bude sloužit k automatické zjišťování. Všimněte si, že to může trvat a 15 minut pro název účtu, který se zobrazí na portálu. Okamžitou aktualizaci, klikněte na položku **Konfigurace servery** > název serveru > **Aktualizovat Server**.

    ![Podrobnosti](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Připojení k vSphere tabulkami hosts a servery vCenter

Pokud jste replikace VMware VMs připojení k vSphere tabulkami hosts a vCenter servery.

1. Ověřte, zda konfigurační server má přístup k síti na vSphere tabulkami hosts a vCenter servery.
2. Klikněte na **připravit infrastruktury** > **zdroje**. **Příprava** zdroje vyberte konfigurační server a klikněte na **+ vCenter** přidáte vSphere host nebo vCenter serveru.
3. V **Přidat vCenter** zadejte popisný název serveru hostitele nebo vCenter vSphere a zadejte IP adresa nebo plně kvalifikovaný název domény serveru. Pokud serverech VMware nakonfigurovaný pro příjem žádosti o jiný port ponechte port 443. Vyberte účet, který se používá pro připojení k serveru VMware. Klikněte na **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Pokud přidáváte vCenter serveru nebo vSphere host pomocí účtu, který nemá oprávnění správce serveru vCenter nebo Host (hostitel), zkontrolujte, že účet obsahuje tato oprávnění povolené: datacentru, úložiště, složky, Host, sítě, zdrojů, virtuální počítače, vSphere distribuované přepínač. Kromě toho vCenter serveru musí oprávnění zobrazení úložiště.

Obnovení webu připojuje k serverům VMware pomocí nastavení zadané a zjistí VMs.

## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Nastavení cílové prostředí

Ověřte, jestli že máte účet úložiště replikace a síť Azure, ke kterému Azure VMs připojí po překlopení.

1.  Klikněte na **připravit infrastruktury** > **cílové** a vyberte Azure předplatné, které chcete použít.
2.  Určení nasazení modelu, který chcete použít pro VMs po překlopení.
3.  Obnovení webu zkontroluje, jestli máte jedno nebo více účtů kompatibilní Azure úložiště a sítě.

    ![Cíl](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Pokud jste ještě nevytvořili účet úložiště a chcete ji vytvořit pomocí ARM klikněte na **+ účet úložiště** udělat tento vložený.  Na zásuvné **vytvořit úložiště účtu** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejné oblasti jako trezoru obnovení služby.

    ![Úložiště](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Všimněte si, že:

    - Pokud chcete vytvořit účet úložiště pomocí klasického modelu se to udělat v portálu Azure. [Víc se uč](../storage/storage-create-storage-account-classic-portal.md)
    - Pokud používáte účet premium úložiště pro replikovanou data, která bude potřeba nastavit další standardní úložiště klienta, kterého chcete úložiště replikace protokoly probíhající změny, které zachycení místní data.

    > [AZURE.NOTE] Ochrana účtům premium úložiště v centrální Indie a států Jižní Indie aktuálně nepodporuje.

4.  Vyberte Azure sítě. Pokud jste nevytvořili v síti a chcete to udělat pomocí ARM klikněte na **+ sítě** se tento vložený. Na zásuvné **vytvořit virtuální síť** zadejte název sítě, rozsah adres, podsítě podrobnosti, předplatné a umístění. Síť by měl být ve stejném umístění jako služby Recovery trezoru.

    ![Sítě](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Pokud chcete vytvořit síť pomocí klasického modelu se to udělat v portálu Azure. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Krok 4: Nastavení replikace

1. Vytvořit nové replikace zásady klikněte na možnost **připravit infrastruktury** > **Nastavení replikace** > **+ Vytvoření a přiřazení**.
2. **Vytvoření** a přiřazení zásad zadejte název zásady.
3. V **operace RPO mezní**: Určete omezení operace RPO. Když toto omezení překračuje nepřetržitý replikace se vygeneruje upozornění.
5. V **obnovení přejděte uchovávání informací**zadejte do několika hodin doby okně uchovávání informací pro vás bude každý obnovení bod. Chráněný počítačích můžete obnovit až kamkoli do okna. Až 24 hodin uchovávání informací je podporovaný pro počítačích replikovat na premium úložiště.
6. V **aplikaci konzistentní počet_plateb snímku**, zadejte, jak často (v minutách) se vytvoří body obnovení obsahující konzistenci aplikací snímky.
7. Při vytváření zásad replikace ve výchozím nastavení odpovídající zásady se automaticky vytvoří navrácení. Například pokud zásady replikace **zákazníkům zásadách** pak zásad navrácení bude **zákazníkům zásadách navrácení**. Zahájení navrácení, dokud nebude tento zásada použita.  
8. Klikněte na **OK** vytvořte zásadu.

    ![Zásady replikace](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Při vytváření nové zásady máte automaticky přidružený k konfigurační server. Klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte v basic infrastruktury nastavení se může přemýšlet o plánování kapacity a zjistit, jestli budete potřebovat další zdroje informací.

Obnovení webu poskytuje kapacita plánování vám přidělit správné zdroje na prostředí zdroje, součásti obnovení webů, sítě a úložiště. Plánování můžete spustit v rychlém režimu odhady podle průměrný počet VMs, disků a úložišť nebo v podrobné režimu, ve kterém se vstupní hodnoty na úrovni zátěží na projektu. Než začnete musíte:

- Shromážděte informace o prostředí replikace, včetně VMs disků za VMs a úložiště na disku.
- Odhad denní změnit sazba (konve), které budete mít k dispozici pro replikovanou data. Můžete to udělat můžete [vSphere kapacita plánování zařízení](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) .

1.  Klikněte na **Stáhnout** pro stažení nástroje a pak ho spusťte. [Přečtěte si tento článek](site-recovery-capacity-planner.md) , který doprovází nástroj.
2.  Po dokončení vyberte možnost **Ano** **jste dokončili plánování kapacity?**

    ![Plánování kapacity](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

Následující tabulka zaznamenává počet bodů vám pomůže při plánování pro tento scénář kapacity.


**Součásti** | **Podrobnosti**
--- | --- | ---
**Replikace** | **Maximální denně změnit rychlost**– chráněné počítače můžete použít jenom jeden proces server a serveru pro jednoho obrázku můžete obsloužení denní změnit ohodnocení až do velikosti 2 TB. 2 TB tedy maximální denní dat změní míra, která je podporovaný pro chráněné počítače.<br/><br/> **Maximální výkon**– replikovanou počítače můžete patří do jednoho účtu úložiště v Azure. Standardní úložiště účtu můžete zpracování maximálně 20 000 požadavky za sekundu a doporučujeme, abyste počet procesorů v počítači zdroje 20 000. Pro příklad Pokud máte zdrojového počítače s 5 disků a každý disk generuje 120 procesorů (8K velikost) ve zdroji, bude nacházet v Azure na disku procesorů maximálně 500. Požadovanou velikost úložiště účtů povinné = zdroje celkem procesorů/20000.
**Konfigurace serveru** | Konfigurační server by měla zpracovat denní kapacity změnit rychlost přes všechny úlohami chráněné počítačích se systémem a třeba dostatečnou šířku pásma pro nepřetržitě replikovat data k základnímu úložišti Azure.<br/><br/> Osvědčený doporučujeme, aby konfigurační server být umístěny ve stejné síti a LAN segmentu jako stroje, které chcete zamknout. Mohou být umístěny na jiné síti ale stroje, které chcete zamknout by měla být L3 sítě viditelnost k němu.<br/><br/> Velikost doporučení pro konfigurační server jsou shrnuté v následující tabulce.
**Proces serveru** | Ve výchozím nastavení konfigurační server je nainstalovaný první server obrázku. Můžete nasadit další proces servery zobrazit prostředí. Všimněte si, že:<br/><br/> Server proces přijímá replikace data z chráněné počítačů a optimalizuje ukládání do mezipaměti, komprese a šifrování před odesláním Azure. Počítač serveru proces by měla být dostatečné prostředky k provedení těchto úkolů.<br/><br/> Server proces používá mezipaměti založené na disku. Doporučujeme disk samostatné mezipaměti nejméně 600 GB provádět změny dat uložených v případě kritický sítě nebo výpadku.

### <a name="size-recommendations-for-the-configuration-server"></a>Velikost doporučení pro konfigurační server

**PROCESOR** | **Paměti** | **Velikost diskové mezipaměti** | **Změnit rychlost dat** | **Chráněný počítačích**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz) | 16 GB | 300 GB | 500 GB nebo nižší | Replikovat menší než 100 počítačích.
12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz) | 18 GB | 600 GB | 500 GB až 1 TB | Replikovat mezi 100 150 počítačích.
16 vCPUs (2 sockets * 8 jádra @ 2,5 GHz) | 32 GB | 1 TB | 1 TB až 2 TB | Replikovat mezi 150-200 počítačích.
Nasazení serveru jiného obrázku | | | > 2 TB | Nasazení další proces serverů, pokud jste replikace více než 200 počítačích nebo pokud denní dat změnit rychlost větší než 2 TB.

Kde je:

- Každý počítač zdroj nastaven 3 disků 100 GB.
- Použijeme benchmarking úložiště 8 jednotek přidružení zabezpečení 10 kB ot RAID 10 měření diskové mezipaměti.

### <a name="size-recommendations-for-the-process-server"></a>Velikost doporučení pro server procesu

Pokud nepotřebujete chránit více než 200 počítače nebo je větší než 2 TB denní změnit rychlost můžete přidat další proces servery zpracovávání načtení replikace. Zobrazit si můžete provést tyto akce:

- Zvýšení počtu konfigurace servery. Můžete třeba chránit až 400 počítačích se dvěma konfigurace servery.
- Přidat další proces servery a používat tyto zpracovávání přenosy místo (nebo kromě) konfigurační server.

Tato tabulka popisuje scénáři, ve kterém:

- Nejsou plánujete používat konfigurační server jako server obrázku.
- Jste nastavili server další obrázku.
- Nakonfigurujete chráněné virtuálních počítačích pro použití dalších proces serveru.
- Každý počítač chráněného zdroj nastaven tři disků 100 GB.

**Konfigurace serveru** | **Další proces serveru**| **Velikost diskové mezipaměti** | **Změnit rychlost dat** | **Chráněný počítačích**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 4 vCPUs (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti | 300 GB | 250 GB nebo nižší | Replikovat 85 nebo menší počítačích.
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti | 600 GB | 250 GB až 1 TB | Replikovat mezi 85 150 počítačích.
12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz), 18 GB paměti | 12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz) 24 GB paměti | 1 TB | 1 TB až 2 TB | Replikovat mezi 150 225 počítačích.


Způsob, ve kterém můžete změnit velikost serverech závisí na předvolby pro změnu velikosti nahoru nebo rozšiřování modelu.  Škálování nasazením několik kvalitní konfigurace a proces servery nebo rozšiřování nasazením další servery s méně prostředků. Příklad: v případě potřeby můžete chránit 220 počítačích může udělejte jednu z následujících akcí:

- Nastavení konfigurační server s 12vCPU, 18 GB paměti, serveru další proces s 12vCPU 24 GB paměti a konfigurace chráněné počítačů používat jenom server další obrázku.
- Můžete taky může konfigurace dvou konfigurace (2 x 8vCPU, 16 GB paměti RAM) a dva další proces servery (1 x 8vCPU) a 4vCPU x 1 pro zpracování 135 + 85 (220) počítačích a konfiguraci chráněné počítačů používat pouze servery další obrázku.

[Postupujte podle těchto pokynů](#deploy-additional-process-servers) k nastavení serveru další obrázku.

### <a name="network-bandwidth-considerations"></a>Aspektech šířky pásma sítě

Nástroj Plánovač kapacita slouží k výpočtu šířku pásma, které potřebujete pro replikace (počáteční replikace a potom delta). K řízení počtu využití šířky pásma pro replikace máte několik možností:

- **Omezení šířky pásma**: přenosy VMware zreplikuje na Azure prochází server specifický proces. Můžete šířku pásma v počítačích spuštěný jako obrázku serverů.
- **Vliv šířky pásma**: může mít vliv šířku pásma pro replikace pomocí několika jednoduchých klíče registru:
    - Hodnotu registru **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** určuje počet podprocesů, které se používají pro přenos dat (počáteční nebo delta replikace) disku. Vyšší hodnota zvýší šířka pásma pro replikační.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** určuje počet podprocesů používaný pro přenos dat během navrácení.

#### <a name="throttle-bandwidth"></a>Omezení šířky pásma

1. Otevřete modulu snap-in konzoly MMC Zálohování Microsoft Azure v počítači budou sloužit jako server procesu. Ve výchozím nastavení je k dispozici na stolním počítači nebo v C:\Program Files\Microsoft Azure obnovení služby Agent\bin\wabadmin zástupce na ploše pro aplikaci Microsoft Azure zálohování.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.

    ![Omezení šířky pásma](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Na kartě **Throttling** zaškrtněte políčko **Povolit omezení pro zálohování využití šířky pásma Internetu**a nastavit omezení pro práci a není funkční hodiny. Platné oblasti mají z 512 kB/s 102 MB / sekundu.

    ![Omezení šířky pásma](./media/site-recovery-vmware-to-azure/throttle2.png)

Taky můžete použít rutinu [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) nastavit omezení. Tady je ukázka:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Nastavení OBMachineSetting NoThrottle** označuje, že není potřeba žádná omezení.


#### <a name="influence-network-bandwidth"></a>Vliv šířka pásma

1. V registru přejděte **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - K ovlivnění šířky pásma přenosy na replikace disku, změňte hodnotu **UploadThreadsPerVM**nebo vytvořte klávesu Pokud neexistuje.
    - Chcete-li ovlivnit šířky pásma pro přenos navrácení z Azure, změňte hodnotu **DownloadThreadsPerVM**.
2. Výchozí hodnota je 4. V síti "overprovisioned" by měl být tyto klíče registru změnil z výchozích hodnot. Maximální hodnota je 32. Sledování provozu optimalizovat hodnotu.

## <a name="step-6-replicate-applications"></a>Krok 6: Replikovat aplikací

Ujistěte se, že stroje, které chcete replikovat jsou připravené pro instalaci služby nastavení mobilních zařízení a pak ji povolit replikace.

### <a name="install-the-mobility-service"></a>Instalace služby mobilita

Cílem prvního kroku s povolením ochranu virtuálních počítačích a fyzických serverů je instalace služby nastavení mobilních zařízení. Můžete provést několika způsoby:

- **Proces serveru nabízených**: Pokud zapnete replikace na počítač, nabízená a nainstalujte mobilita služby ze serveru obrázku. Všimněte si, že instalace nabízených nedojde, pokud počítačích se systémem až do data verzi součásti.
- **Pole organizace nabízená**: automaticky nainstalujte pomocí procesu enterprise nabízených ATP WSUS nebo Správce konfigurace pro System Center [Azure automatizace a konfigurace požadovaná stavu](./site-recovery-automate-mobility-service-install.md). Než to uděláte, nastavení konfigurační server.
- **Ruční instalace**: komponentu nainstalujte ručně na každém počítači, který chcete replikovat. Než to uděláte, nastavení konfigurační server.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Příprava na automatické nabízená v počítačích Windows

Tady je postup, jak připravit počítačích Windows tak, aby může proces serveru automaticky nainstalována služba nastavení mobilních zařízení.

1.  Vytvořte účet, který lze serverem proces přístup k počítači. Účet by měl oprávnění správce (místní nebo doména) a se používá pouze pro instalaci připínáčku.

    >[AZURE.NOTE] Pokud nepoužíváte účet domény musíte zakázat ovládací prvek vzdáleného přístupu uživatele v místním počítači. K tomuto účelu v seznamu v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System přidejte položku DWORD LocalAccountTokenFilterPolicy hodnota 1. Chcete-li přidat položku registru typu rozhraní příkazového řádku **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Na brána Windows Firewall počítače chcete zamknout, vyberte **Povolit aplikaci nebo funkci průchod bránou Firewall**. Povolte **sdílení souborů a tiskáren** a **přístrojového vybavení správy systému Windows**. U počítačů, které patří do domény můžete nakonfigurovat nastavení brány firewall s objekt zásad skupiny.

    ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Přidejte účet, který jste vytvořili:

    - Otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
    - Na kartě **Správa účtů** klikněte na **Přidat účet**.
    - Přidejte účet, který jste vytvořili. Po přidání účtu musíte zadat pověření po povolení replikace pro počítač.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Příprava na automatické nabízených na serverech Linux

1.  Ujistěte se, že Linux počítače, který chcete zamknout podporované podle popisu v [požadavky chráněné počítače](#protected-machine-prerequisites). Ujistěte se, že síťové připojení mezi počítači Linux a server procesu.

2.  Vytvořte účet, který lze serverem proces přístup k počítači. Účet by měl být uživatel root na zdrojovém Linux serveru a se používá pouze pro instalaci připínáčku.

    - Otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
    - Na kartě **Správa účtů** klikněte na **Přidat účet**.
    - Přidejte účet, který jste vytvořili. Po přidání účtu musíte zadat pověření po povolení replikace pro počítač.

3.  Zkontrolujte, zda soubor/etc/hosts na zdrojový server Linux obsahuje položky, které místní hostname mapují IP adresy přidružené k některé síťové adaptéry.
4.  Nainstalujte nejnovější funkci openssh, funkci openssh server, openssl balíčků v počítači, který chcete replikovat.
5.  Zkontrolujte, zda SSH povolené a spuštěnou na port 22.
6.  Ověření SFTP podsystém a heslo v souboru sshd_config následujícím způsobem:

    - Přihlaste se jako kořenovou.
    - V souboru /etc/ssh/sshd_config vyhledejte řádek, který začíná **PasswordAuthentication**.
    - Zrušte komentář řádku a změňte hodnotu z **žádné** na hodnotu **Ano**.
    - Najděte řádek, který začíná **podsystém** a zrušte komentář řádku.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ruční instalace služby mobilita

Instalační programy jsou dostupné na serveru konfigurace **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

Zdrojový operační systém | Soubor Instalační služby mobilita
--- | ---
Windows Server (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 s aktualizací SP3 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Instalace služby nastavení mobilních zařízení v systému Windows Server


1. Stáhnout a spustit relevantní instalačního programu.
2. V **než začnete** vyberte **mobilita služby**.

    ![Nastavení mobilních zařízení služby](./media/site-recovery-vmware-to-azure/mobility3.png)

3. **Konfigurace serveru** podrobnosti zadejte IP adresu konfiguračního serveru a heslo vygenerované při spuštění instalačního programu jednotné. Načtete heslo opětovným spuštěním: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** na konfigurační server.

    ![Nastavení mobilních zařízení služby](./media/site-recovery-vmware-to-azure/mobility6.png)

4. **Instalace** umístění ponechejte výchozí nastavení a klikněte na tlačítko **Další** zahájíte instalaci.
5. V **Průběhu instalace** sledovat instalaci a po zobrazení výzvy restartovat počítač. Po instalaci služby může trvat asi 15 minut stav a aktualizovat na portálu.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Instalace služby nastavení mobilních zařízení v systému Windows server pomocí příkazovém řádku

1. Zkopírujte instalační program do místní složky (například C:\Temp) na serveru, který chcete zamknout. Instalační program se nachází na konfigurační Server v rámci **\home\svsystems\pushinstallsvc\repository [umístění instalace]**. Balíček pro operační systémy Windows bude mít podobný Microsoft ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe název
2. **Přejmenujte** soubor MobilitySvcInstaller.exe
3. Spusťte tento příkaz extrahovat instalační služba MSI </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Syntaxe úplné příkazového řádku

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametry**

- **/Role:** Povinný. Určuje, zda měli byste mít nainstalované službu nastavení mobilních zařízení. Vstupní hodnoty Agent | MasterTarget
- **/InstallLocation:** Povinný. Určuje, kde k instalaci služby.
- **/PassphraseFilePath:** Povinný. Konfigurace serveru heslo.
- **/LogFilePath:** Povinný. Umístění, kde by měl vytvořen instalačních souborů protokolu.



#### <a name="uninstall-mobility-service-manually"></a>Ruční odinstalace služby mobilita

Nastavení mobilních zařízení služby lze odinstalovat pomocí programu přidat odebrat z ovládacích panelů nebo pomocí příkazového řádku.

Příkaz Odinstalovat se službou nastavení mobilních zařízení v okně příkazového řádku

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Instalace mobilita služby na serveru Linux pomocí příkazového řádku

1. Zkopírujte odpovídající vkládání archivu založený na tabulce nad do počítače Linux, který chcete replikovat.
2. Spusťte aplikaci prostředí a extrahovat ZIP vkládání archiv na místní cestu spuštěním:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Vytvoření souboru passphrase.txt místního adresáře, do kterého jste extrahovali obsah vkládání archivu. To zkopírujte heslo z C:\ProgramData\Microsoft Azure webu Recovery\private\connection.passphrase na konfigurační server a uložte ho ve passphrase.txt spuštěním *`echo <passphrase> >passphrase.txt`* v prostředí.
4. Nainstalujte službu mobilita spuštěním *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Určení vnitřní IP adresu konfiguračního serveru a ujistěte se, že je vybrán port 443. Po instalaci služby může trvat asi 15 minut stav a aktualizovat na portálu.

**Můžete taky nainstalovat z příkazového řádku**:

1. Zkopírujte heslo z C:\Program Files (x86) \InMage Systems\private\connection na konfigurační server a uložit jako "passphrase.txt" v konfigurační server. Spusťte tyto příkazy. V našem příkladu je IP adresa serveru konfigurace 104.40.75.37 a HTTPS port by měl být 443:

Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Instalace na hlavní cílovém serveru:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Povolení replikace

#### <a name="before-you-start"></a>Než začnete

Pokud jste replikace VMware virtuálních počítačích mějte na paměti následující:

- VMware VMs jsou zjištěny každých 15 minut a může trvat 15 minut či delší zobrazit na portálu po zjišťování je. Zjišťování můžete brát a 15 minut po přidání nového vCenter serveru nebo vSphere hostitel.
- Změny prostředí virtuální počítače (například instalace nástrojů VMware) může trvat a 15 minut mají být aktualizovány na portálu.
- Při posledním zjištěnou můžete vyhledat VMware VMs v oblasti **Poslední kontakt na** hostitele serveru/vSphere vCenter na zásuvné **Konfigurace servery** .
- Přidání stroje replikace, aniž byste čekali plánované zjišťování, zvýraznění konfigurační server (neklikejte na ho) a klikněte na tlačítko **Aktualizovat** .
- Pokud povolíte replikace, pokud v počítači je připraven proces serveru automaticky nainstaluje službu mobilita ho.

#### <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Pokud povolíte replikace, ve výchozím nastavení jsou replikovat všech discích na počítač. Vyloučení disků z replikace. Například nebudete chtít replikovat disků s dočasná data nebo data, která aktualizovala pokaždé, když počítači nebo restartování aplikace (například pagefile.sys nebo tempdb serveru SQL Server). Pokud chcete, aby se vyloučila disků Všimněte si, že:

- Můžete jenom vyloučit disků, které už máte službu nastavení mobilních zařízení nainstalovaný. [Ručně](#install-the-mobility-service-manually) instalovat službu mobilita budete potřebovat, protože mobilita je pouze nainstalována služba pomocí mechanismus nabízených po povolení replikace.
- Pouze základní disků můžete vyloučit z replikace. Nelze vyloučit OS nebo dynamických discích.
- Po povolení replikace nelze přidat nebo odebrat disků replikace. Pokud chcete přidat nebo vyloučení disk musíte zakázat ochrany počítače a potom ho znovu povolit.
- Pokud vyloučit disku, který má potřebné pro aplikace k ovládání po přepnutí Azure bude nutné ji vytvořit ručně v Azure tak, aby mohlo by umožnit spuštění replikovanou aplikace. Můžete taky může začlenit Azure automatizaci do plánu obnovy vytváření disku při selhání počítače.
- Zpět se nezdaří disků, které můžete ručně vytvořit v Azure. Například pokud nepovede více než tři disků a vytvoříte dva přímo v Azure všech pět se nezdaří zpět. Nelze vyloučit disků vytvořeno ručně z navrácení.

**Teď povolte následujícím způsobem**:

1. Klikněte na **Krok 2: replikovat aplikace** > **zdroje**. Poté, co jste povolili replikace poprvé budete klikněte na **+ replikovat** trezoru povolit replikace pro další počítače.
2. V zásuvné **zdroj** > **zdroj** vyberte konfigurační server.
3. Ve **počítače typ** vyberte **virtuálních počítačích** nebo **Pole fyzicky počítačích**.
4. V **vCenter/vSphere hypervisoru** vyberte vCenter server, který má na starosti hostiteli vSphere nebo vyberte hostiteli. Toto nastavení není důležité, pokud jste replikace fyzických počítačů.
5. Vyberte server obrázku. Pokud jste nevytvořili nějaké další proces servery půjde název konfigurační server. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. V **cílové** vyberte předplatné trezoru a v **příspěvku selhání nasazení modelu** vyberte model (Správa classic nebo zdroje), který chcete použít v Azure po překlopení.
7. Vyberte požadovaný účet Azure úložiště, které budete používat pro replikace data. Všimněte si, že:

    - Můžete vybrat premium nebo standardní ukládání do účtu. Pokud vyberete účet premium musíte zadat účet další standardní úložiště pro probíhající replikace protokoly. Účty musí být ve stejné oblasti jako služby Recovery trezoru.
    - Pokud chcete používat účet jiné úložiště než těch, kterým jste můžete [vytvořit](#set-up-an-azure-storage-account). Vytvoření úložiště účtu pomocí modelu ARM klikněte na **vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu můžete udělat modul [Azure portálu](../storage/storage-create-storage-account-classic-portal.md).

8. Vyberte Azure síťových připojení a podsítě, ke kterému Azure VMs připojí, když jste nespředený nahoru po překlopení. Síť musí být ve stejné oblasti jako služby Recovery trezoru. Vyberte **konfigurovat nyní u vybraných počítačů** použít nastavení sítě do všech počítačů, které můžete vybrat ochranu. Vyberte **Konfigurovat později** vyberte Azure sítě jednotlivé počítače. Pokud nemáte v síti musíte [ho vytvořit](#set-up-an-azure-network). Pokud chcete vytvořit síť pomocí modelu ARM klikněte na **vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu můžete udělat modul [Azure portálu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V případě potřeby vyberte podsítě. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. Ve **virtuálních počítačích** > **Vyberte virtuálních počítačích** klikněte na a vyberte každý počítač chcete replikovat. Můžete zvolit pouze počítačích, pro které lze replikace povoleno. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. V **dialogovém okně Vlastnosti** > **Konfigurovat vlastnosti**vyberte účet, který použije server obrázku tak, aby automaticky nainstalovat službu nastavení mobilních zařízení v počítači. Ve výchozím nastavení jsou replikovat všech discích. Klikněte na **Všechny disků** a zrušte disků, které nechcete replikovat. Klikněte na **OK**. Můžete nastavit další vlastnosti později.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. V **dialogovém okně nastavení replikace** > **Konfigurovat nastavení replikace** zkontrolujte, jestli je vybraná zásady správného replikace. Můžete změnit nastavení zásad replikace v **dialogovém okně Nastavení** > **replikace zásady** > název zásady > **Upravit nastavení**. Změny platit pro zásady se použije na replikace tak i u nových počítačích.

12. Povolení **více OM konzistence** , pokud chcete shromažďovat počítačích do skupiny replikace a zadejte název skupiny. Klikněte na **OK**. Všimněte si, že:

    - V replikace replikace seskupit a sdílejí pád konzistentní a konzistentní aplikace obnovení body při jejich převzít.
    - Doporučujeme, aby se shromažďování VMs a fyzických serverů tak, aby odpovídaly svého pracovního vytížení. Povolení více OM konzistence může ovlivnit výkon pracovního vytížení a by měl použít pouze v případě počítačích se systémem stejného pracovního vytížení a musíte konzistence.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Zaškrtněte políčko **Povolit replikace**. Můžete sledovat průběh **Povolit ochranu** úlohy v **dialogovém okně Nastavení** > **úlohy** > **Úlohy obnovení webů**. Když v počítači spuštěná úloha **Dokončení ochrany** je připraven k převzetí.

> [AZURE.NOTE] Pokud počítač je počítat instalace nabízených součást služby mobilita nainstaluje při zapnuté funkci zámek. Po komponentu nainstalovaný v počítači, spuštění úlohy ochranu a dojde k chybě. Po selhání musíte restartovat všech počítačů. Po restartování ochranu úlohy, nebude zahájen znovu a počáteční replikace.

### <a name="view-and-manage-vm-properties"></a>Zobrazit a spravovat vlastnosti OM

Doporučujeme ověřit vlastnosti zdrojového počítače. Myslete na to, že název Azure OM by měly odpovídat požadavkům [Azure virtuálního počítače](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Klikněte na **Nastavení** > **replikovaná položky** > a vyberte počítač. **Základy** zásuvné s informacemi o stavu a nastavení počítače.

2. V **dialogovém okně Vlastnosti** můžete zobrazit informace o OM replikace a překlopení.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. Ve **výpočetním a síťové** > **Výpočet vlastnosti** můžete zadat Azure OM název a cílové velikost. Název dodržovat Azure požadavky v případě potřeby změňte.
Můžete taky zobrazit a přidat informace o cílové síti, podsítě a IP adresy přiřazené k OM Azure. Poznámka:

    - Můžete nastavit cílová IP adresa. Pokud neposkytnete adresu, selhalo přes počítač bude používat DHCP. Pokud jste nastavili adresu, která není k dispozici na převzetí, nebudou fungovat záložní. Stejné cílová IP adresa lze test převzetí Pokud je k dispozici v síti převzetí test na adresu.
    - Počet síťové adaptéry je dáno velikost zadané pro cílové virtuální počítač, následujícím způsobem:

        - Pokud argument číslo síťové adaptéry zdrojového počítače menší nebo rovna hodnotě počet adaptéry povolená velikost v počítači cílového, pak cíl bude mít stejný počet adaptéry jako zdroj.
        - Pokud počet adaptéry virtuálního počítače zdroj překročí povolený Cílová velikost a pak maximální velikost cílové bude použito počet.
        - Pokud například zdrojového počítače obsahuje dva síťové adaptéry a cílová velikost počítač podporuje čtyři, cílový počítač bude mít dva adaptéry. Pokud zdrojového počítače obsahuje dva adaptéry ale podporované Cílová velikost podporuje pouze jednu cílový počítač bude mít jenom jeden adaptér.     
    - Pokud OM má více síťové adaptéry budou všechny připojí ke stejné síti.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. Na **disku** uvidíte operačního systému a disků dat na OM, který bude replikovat.


## <a name="step-7-test-the-deployment"></a>Krok 7: Testování nasazení

K otestování nasazení spuštěním testovací převzetí jednoho virtuálního počítače nebo obnovení plán, který obsahuje jeden nebo více virtuálních počítačích.


### <a name="prepare-for-failover"></a>Příprava překlopení

- Spusťte test selhání doporučujeme vytvořit novou Azure síť, který má samostatný z Azure sítě (Toto je výchozí chování v Azure vytvoříte novou síť). [Další informace](site-recovery-failover.md#run-a-test-failover) o tom, jak převzetí služeb při selhání testu.
- Nejlepší možný výkon při můžete převzít Azure získáte nainstalujte agenta Azure na zamknutém počítač. Převede spuštění rychleji a pomáhá slouží k řešení potíží. Nainstalujte agent [Linux](https://github.com/Azure/WALinuxAgent) nebo [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Plně otestovat nasazení musíte mít infrastrukturu pro replikovanou počítač tak, aby fungovaly očekávaným. Pokud chcete testovat Active Directory a DNS můžete vytvořit virtuální počítač jako řadiče domény se službou DNS a replikovat to Azure pomocí obnovení webu Azure. Přečtěte si další v [aspektech převzetí test pro služby Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Ujistěte se, jestli je spuštěný konfigurační server. Převzetí jinak se nezdaří.
- Pokud jste vyloučeny disků z replikace může potřebujete vytvoření těchto discích ručně v Azure po převzetí tak, aby aplikace očekávaným způsobem.
- Pokud chcete spustit neplánované převzetí místo selhání test mějte na paměti následující:

    - Pokud je to možné měli vypnete primární počítačích před spuštěním neplánované převzetí. Zajistíte tak, že nemáte do něj zdrojovou a otevřené počítače se systémem ve stejnou dobu. Pokud jste replikace VMware VMs můžete určit, že obnovení webu by měl snažte k vypnutí počítače zdroje. V závislosti na stav primární webu může nebo nemusí fungovat. Pokud jste replikace fyzické servery obnovení webu nenabízí možnost.
    - Když spustíte neplánované převzetí zastaví replikace dat z primární počítačů tak všechny delta data nebude dojít ke ztrátě určitého po zahájení neplánované převzetí. Kromě toho neplánované převzetí plán obnovení ho bude spustit až do dokončení, i když dojde k chybě.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Příprava přechodu na připojit k Azure VMs po překlopení

Pokud chcete připojit k VMs Azure pomocí RDP po překlopení, ujistěte se, že uděláte to takhle:

**V místním počítači před převzetí**:

- Pro přístup přes internet povolit RDP zajistit přidaná TCP a UDP pravidla pro **veřejné**a zajistit, aby se v bráně **Firewall systému Windows**RDP -> **povolených aplikací a funkcí** pro všechny profily.
- Pro přístup přes připojení k webu povolit RDP v počítači a zajistit, aby se v bráně **Firewall systému Windows**RDP -> **povolených aplikací a funkcí** pro **doménu** a **privátní** sítě.
- Nainstalujte [Azure OM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) v místním počítači.
- [Ruční instalace služby mobilita](#install-the-mobility-service-manually) na počítačích místo použití server procesu posunout službu automaticky. Důvodem je instalace nabízených pouze se stane, až počítač aktivované řešení replikace.
- Ujistěte se, že je operační systém SAN zásady nastaveno OnlineAll. [Víc se uč]( https://support.microsoft.com/kb/3031135)
- Vypnutí služby IPSec před spuštěním záložní.

**V Azure OM po převzetí služeb**:

- Přidání veřejné koncový bod pro protokol RDP (port 3389) a zadejte přihlašovací údaje pro přihlášení.
- Ujistěte se, že nemáte žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.
- Pokuste se připojit. Pokud se nemůžete spojit zkontrolujte, jestli je spuštěný OM. Další tipy pro řešení problémů v tomto [článku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Pokud chcete získat přístup Azure OM systémem Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), postupujte takto:

**V místním počítači před převzetí**:

- Ověřte, zda službu zabezpečené Shell na OM Azure aby se spouštěl automaticky při spuštění systému.
- Zkontrolujte, jestli pravidla brány firewall SSH připojení k němu.

**V Azure OM po převzetí služeb**:

- Pravidla skupiny zabezpečení sítě o nezdařeném uložení přes OM a Azure podsítí, ke kterému je připojen muset povolit SSH port pro příchozí připojení.
- Veřejné koncový bod má vytvořit za účelem povolení příchozí připojení na SSH port (port TCP 22 ve výchozím nastavení).
- Pokud OM k nim získat přístup přes připojení virtuální privátní sítě (VPN sítěmi nebo Express směrování) potom klienta mohou sloužit přímo připojit přes SSH bude v angličtině.

**V Azure Windows/Linux OM po převzetí služeb**:

Pokud máte skupinu zabezpečení sítě přidružené virtuálního počítače nebo podsítě, ke kterému patří počítače, ujistěte, že skupiny zabezpečení sítě má odchozího pravidla umožňuje protokolu HTTP/HTTPS. Ujistěte se, že DNS sítě, do které virtuálního počítače je Začínáme se nepodařilo delší než správně nakonfigurovaný také. Ještě záložní může časový limit s chyba: "PreFailoverWorkflow úkol, který WaitForScriptExecutionTask vypršení časového limitu". Porozumět tomu podrobně, najdete pod odkazy v části o obnovení [sledování a Průvodce pro řešení](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Spusťte test selhání

1. Převzetí jednoho počítače, v **dialogovém okně Nastavení** > **Replikovat položky**, klikněte OM > **+ Test převzetí** ikonu.

    ![Převzetí test](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Překlopení obnovení plán, v **dialogovém okně Nastavení** > jednotného zasílání zpráv**Obnovení plány jednotného zasílání zpráv**, klikněte pravým tlačítkem myši plán > **Převzetí testu**. Vytvoření obnovení plánování, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).

3. V **Testu převzetí** vyberte Azure sítě, ke kterému bude připojený Azure VMs, když dojde k selhání.
4. Klepnutím na **OK** spusťte záložní. Sledování průběhu kliknutím na OM zobrazíte její vlastnosti nebo **Převzetí Test** projektu do pole Název trezoru > **Nastavení** > **úlohy** > **úloh obnovení webu**.
5. Dosáhne záložní stav **testování dokončeno** , postupujte takto:

    1. Zobrazení virtuálního počítače otevřené Azure portálu. Ověřte úspěšně spustí virtuální počítač.
    2. Pokud jste nastavili přístup virtuálních počítačích z místní síti můžete spustit připojení ke vzdálené ploše virtuálního počítače.
    3. Kliknutím na tlačítko Dokončit je **Dokončeno otestovat** .

        ![Převzetí test](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Kliknutím na **poznámky** můžete zaznamenat a uložit všechny připomínky přidružený k převzetí testu.
    5. Klikněte na **dokončení testu převzetí** automaticky vyčištění testovacím prostředí. Po dokončení mapování převzetí test se zobrazí stav **Dokončeno** .
    6.  V této fázi budou odstraněny všechny prvky nebo VMs vytvářeny automaticky službami obnovení webu při selhání test. Další prvky, které jste vytvořili pro překlopení test se neodstraní.

    > [AZURE.NOTE] Pokud test přepojení trvají delší než dva týdny dokončení silou.


6. Po dokončení záložní je také třeba neuvidíte otevřené Azure počítače se zobrazí v portálu Azure > **virtuálních počítačích**. Nezapomeňte, že OM je vhodné velikosti a připojeného k příslušné sítě a, jestli je spuštěný.
7. Pokud jste [připraveni připojení po převzetí](#prepare-to-connect-to-azure-vms-after-failover) by je možné se připojit k OM Azure.

## <a name="monitor-your-deployment"></a>Sledování nasazení

Tady je, jak můžete sledovat nastavení, stavu a stavu obnovení webu nasazení:

1. Klikněte na název trezoru pro přístup k řídicím panelu **Essentials** . V tomto řídicím panelu můžete úloh obnovení webu, stav replikace, plány obnovy, stavu serveru a události.  Můžete přizpůsobit Essentials zobrazíte dlaždic a rozložení, které jsou pro vás, včetně stavu ostatních zálohování a obnovení webu trezorů nejvhodnější.<br>
![Základy](./media/site-recovery-vmware-to-azure/essentials.png)

2. V této dlaždici **stavu** můžete sledovat Web servery (VMM nebo konfigurace), ve kterých dochází problém a události vyvolané obnovení webu za posledních 24 hodin.
3. Správa a sledování replikace v **Replikovat položky**jednotného zasílání zpráv **Obnovení plány jednotného zasílání zpráv**, a dlaždice **Úlohy obnovení webů** . Procházení hierarchie do projektů v **dialogovém okně Nastavení** -> **úlohy** -> **Úlohy obnovení webů**.


## <a name="deploy-additional-process-servers"></a>Nasazení servery další obrázku

Máte-li zobrazit, nasazení za 200 počítačích zdroje či celkové denní sazby konve víc než 2 TB, musíte mít serverů další proces zpracování provozu.

Zkontrolovat [velikost doporučení pro proces servery](#size-recommendations-for-the-process-server) a potom postupujte podle těchto pokynů k nastavení serveru proces. Po nastavení serveru migrujete počítačích zdroje se používá.

### <a name="install-an-additional-process-server"></a>Instalace serveru další obrázku

1. V **dialogovém okně Nastavení** > **servery obnovení webu** klikněte na konfigurační server > **server procesu**.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. V **Typ serveru** klikněte na **server procesu (místní)**.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Stáhněte si webu obnovení Unified instalační soubor a ho spusťte a instalace serveru obrázku a zaregistrovat v trezoru.
4. V **než začnete** vyberte **Přidat další proces serverů rozšiřování nasazení**.
5. Dokončete Průvodce stejným způsobem, kdy jste [Nastavení](#step-2-set-up-the-source-environment) konfigurační server.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. **Konfigurace serveru** podrobnosti zadejte IP adresu konfiguračního serveru a heslo. Získat heslo spustit ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na konfigurační server.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrace počítačích serverem nového obrázku

1. V **dialogovém okně Nastavení** > **servery obnovení webu** klikněte na konfigurační server a potom rozbalte položku **proces servery**.

    ![Aktualizovat server obrázku](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Klikněte pravým tlačítkem myši aktuálně používaného serveru obrázku a klikněte na **přepínač**.

    ![Aktualizovat server obrázku](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. V seznamu **Vyberte cílové proces server**vyberte nový server proces, který chcete použít a potom vyberte virtuálních počítačích zpracovávající nového obrázku serveru. Klikněte na informační ikonu k získání informací o serveru. Pro vám pomůže zajistit načíst rozhodnutí, se zobrazí průměr místo potřebné k replikovat každý vybraný virtuální počítač k novému obrázku serveru. Zaškrtněte políčko Spustit replikace do nového obrázku serveru.

## <a name="vmware-account-permissions"></a>VMware účet oprávnění

Server procesu, mohli automaticky zjišťovat VMs na serveru vCenter. Provádět automatické zjišťování musíte definovat [role (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) Pokud chcete povolit obnovení webu pro přístup k serveru VMware. Poznámka: Pokud potřebujete pouze migrace VMware počítačích Azure a ne muset navrácení z Azure, můžete určit roli jen pro čtení, který je dostatečně. V následující tabulce jsou shrnuty oprávnění požadovaných role.

**Role** | **Podrobnosti** | **Oprávnění**
--- | --- | ---
Azure_Site_Recovery role | Zjišťování VMware OM |Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Přidělení místa procházet úložiště, zhoršeným úrovně soubor operace., soubor odebrat, aktualizace souborů virtuálního počítače -> úložiště<br/><br/>Síť -> přiřadit sítě<br/><br/>Zdroje -> přiřadit virtuální počítač fondu zdrojů, migrace vypnutý virtuálního počítače, migrace zapnutý virtuálního počítače<br/><br/>Úkoly -> Vytvořit úkol, aktualizace úkolu<br/><br/>Konfigurace -> virtuálního počítače<br/><br/>Virtuální počítač -> interakce -> odpověď otázku, připojení zařízení?, konfigurace CD-ROM, konfigurovat disketa média, vypněte Power na, nainstalujte si VMware nástroje<br/><br/>Virtuální počítač -> zásob -> Unregister vytvoření rejstříku<br/><br/>Virtuální počítač -> Provisioning -> stáhnout povolit virtuálního počítače, souborů virtuálního počítače povolit odeslat<br/><br/>Virtuální počítač -> snímky -> Odstranit snímky
role uživatele vCenter | VMware OM zjišťování/převzetí bez ukončení zdroje OM | Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Objekt Datacentrem –> rozšíření podřízené objekt role = jen pro čtení <br/><br/>Uživatel se přiřadí úrovni datacentra a tedy má přístup pro všechny objekty v datacentra.  Pokud chcete omezit přístup, přiřadíte roli **bez přístupu** k objektu **rozšíření dítě** k podřízeným objektům (vSphere tabulkami hosts, datastores, VMs a sítě).
role uživatele vCenter | Překlopení a překlopení zpět | Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Objekt Datacentra – rozšíření podřízené objekt role = Azure_Site_Recovery<br/><br/>Uživatel se přiřadí úrovni datacentra a tedy má přístup pro všechny objekty v datacentra.  Pokud chcete omezit přístup, přiřadíte roli **přístup** s **rozšíření pro podřízený objekt** podřízeného objektu (vSphere tabulkami hosts, datastores, VMs a sítě).  
## <a name="next-steps"></a>Další kroky

- [Další informace](site-recovery-failover.md) o různých typů překlopení.
- [Další informace o navrácení](site-recovery-failback-azure-to-vmware.md) si přenést svůj selhalo přes počítače se systémem v Azure zpátky do místního prostředí.

## <a name="third-party-software-notices-and-information"></a>Oznámení o třetích stran software a informací

Co nedělat překlad nebo Localize

Software a firmware spuštěné v produktu společnosti Microsoft nebo služby je založená na nebo zahrne materiál z projektů uvedených pod ním (společně "výrobců kódu").  Společnost Microsoft se není původní autor výrobců kódu.  Původní autorských práv a licence, pod kterým společnost Microsoft tyto výrobců kódu, jsou nastavené níže uvedenými.

Informace v části A týká komponent výrobců kódu z níže uvedených projektů. Tyto licence a informace jsou k dispozici pouze pro informaci.  Tento kód třetích stran je právě relicensed vám společnost Microsoft ve skupinovém rámečku licenčních podmínek softwaru společnosti Microsoft pro Microsoft produktů nebo služeb.  

Informace v části B se týká součásti výrobců kódu, které budou k dispozici vám společnost Microsoft ve skupinovém rámečku původní licenčních podmínek.

Celý soubor najdete na webu služby [Stažení softwaru společnosti Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všemi právy není výslovně udělena, zda implicitně, odvozeně nebo jinak.
