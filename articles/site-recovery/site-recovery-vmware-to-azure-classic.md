<properties
    pageTitle="Replikace VMware virtuálních počítačích a fyzické servery na Azure s obnovení webu Azure | Microsoft Azure"
    description="Tento článek popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení místní VMware virtuálních počítačích a fyzických serverů Azure Windows nebo Linux."
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
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replikovat VMware virtuálních počítačích a fyzických serverů Azure s obnovení webu Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmware-to-azure.md)
- [Klasický portálu](site-recovery-vmware-to-azure-classic.md)
- [Klasický portál (starší verze)](site-recovery-vmware-to-azure-classic-legacy.md)


Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md).

## <a name="overview"></a>Základní informace

Tento článek popisuje postup:

- **Replikovat VMware virtuálních počítačích na Azure**– nasazení obnovení webu ke koordinaci replikace, převzetí a obnovení místní VMware virtuálních počítačích k základnímu úložišti Azure.
- **Replikovat fyzických serverů Azure**– nasazení obnovení Azure webu ke koordinaci replikace, převzetí a obnovení místních serverů fyzické Windows a Linux Azure.

>[AZURE.NOTE] Tento článek popisuje, jak replikovat na Azure. Pokud budete chtít replikovat VMware VMs nebo pole fyzicky servery Windows/Linux na vedlejší datacentru, postupujte podle pokynů v [tomto článku](site-recovery-vmware-to-vmware.md).

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="enhanced-deployment"></a>Vylepšené nasazení

Tento článek obsahuje pokyny k nasazení rozšířené v portálu klasické Azure. Doporučujeme že tuto verzi použít pro všechny nové nasazení. Pokud jste již nainstalovali pomocí dřívější starší verze doporučujeme migrovat do nové verze. Přečtěte si [Další](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) informace o migraci.

Vylepšené nasazení je hlavní aktualizace. Tady je souhrn, které jste udělali jsme vylepšení:

- **Žádná infrastruktura VMs v Azure**: Data zreplikuje přímo na účet Azure úložiště. Kromě toho replikace a převzetí je potřeba nastavit veškeré infrastruktury VMs (konfigurační server, předlohy na cílový) v případě potřeby jsme ve starší verzi nasazení.  
- **Instalace jednotné**: jedna instalace poskytuje jednoduché nastavení a škálovatelnost pro místní součásti.
- **Zabezpečené nasazení**: zašifrovaných všechny přenosy a replikace správy komunikace jsou odesílány HTTPS 443.
- **Obnovení body**: podpora pád a konzistenci aplikací obnovení odkazuje na Windows a Linux prostředí a podporuje jazyky jedním OM a více OM konzistentní konfigurace.
- **Testování převzetí**: podpora bez přerušení test převezme Azure bez vlivu na výrobní nebo pozastavení replikace.
- **Neplánované převzetí**: podpora neplánované převezme Azure rozšířené vyberete možnost vypnutí VMs automaticky před překlopení.
- **Navrácení**: integrované navrácení, který zreplikuje pouze delta změn zpátky do místního serveru.
- **vSphere 6.0**: omezenou podporu pro nasazení VMware Vsphere 6.0.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Jak obnovení webu pomáhá chránit virtuálních počítačích a fyzické servery?


- Správci VMware můžete nakonfigurovat mimo ochrany Azure firmy pracovního vytížení a spuštěné v VMware virtuálních počítačích. Správci serveru můžete replikovat fyzické místního systému Windows a Linux servery Azure.
- Obnovení webu Azure konzoly obsahuje jedno místo pro jednoduché nastavení a Správa replikace, převzetí a obnovení procesů.
- Pokud replikovat VMware virtuálních počítačích, které je spravováno serverem vCenter obnovení webu mohli zjišťovat tyto VMs. Pokud počítačích na hostiteli ESXi obnovení webu nenajde VMs na hostiteli.
- Spuštění snadno převzetí služeb při selhání z místního infrastruktury Azure a navrácení (obnovení) z Azure VMware OM servery v místní síti.
- Konfigurace obnovení plány seskupit pracovního vytížení aplikace, které jsou vrstveny ve více počítačích. Můžete převzít plány a obnovení webu poskytuje více OM konzistence tak, aby počítačích spuštění stejného pracovního vytížení lze obnovit společně do konzistentního datového bodu.


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

Scénář součásti:

- **Místní management server**: server pro správu spouští komponent obnovení webu:
    - **Konfigurace serveru**: souřadnice komunikace a spravuje dat replikace a obnovení procesů.
    - **Proces serveru**: slouží jako replikace brány. Načte data z chráněného zdroj počítačů, optimalizuje ukládání do mezipaměti, komprese a šifrování a odešle data replikace Azure úložiště. Také zpracovává nabízených instalace služby mobilita chráněné počítačích a provádí automatické zjišťování VMware VMs.
    - **Hlavní cílový server**: zpracovává replikace dat během navrácení z Azure.
    Můžete taky nasadit na serveru správy, která funguje jako proces serveru, abyste mohli zobrazit nasazení.
- **Služby mobilita**: součást nasazení počítače každý (VMware OM nebo pole fyzicky server), který chcete replikovat na Azure. Zaznamenává zápis dat v počítači a předá ho server procesu.
- **Azure**: není potřeba vytvářet všechny Azure VMs zpracovávání replikace a překlopení. Obnovení webu služby zpracovává správy dat a datové zreplikuje přímo na Azure úložiště. Replikovanou VMs Azure jsou nespředený nahoru automaticky pouze v případě, že dojde k selhání Azure. Však podle potřeby můžete selhání zpět z Azure na místní web musíte nastavit Azure OM má fungovat jako proces serveru.


Obrázek zobrazuje interakce tyto součásti.

![Architektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Obrázek 1: VMware/fyzické na Azure** (vytvořil Henryho Robalino)


## <a name="capacity-planning"></a>Plánování kapacity

Pokud plánujete kapacitu, tady je co je potřeba vzít v úvahu:

- **Zdrojového prostředí**– plánování kapacity nebo požadavky VMware infrastrukturu a zdroj počítače.
- **Server pro správu**– plánování serverech správy místních spustit komponenty obnovení webu.
- **Šířka pásma ze zdroje na cíl**-plánování šířku pásma sítě požadovanou replikace mezi zdroji a Azure

### <a name="source-environment-considerations"></a>Co byste měli zvážit zdroj prostředí

- **Maximální denně změnit rychlost**– chráněné počítače můžete použít jenom jeden proces server a serveru pro jednoho obrázku můžete obsloužení až 2 TB Změna dat během dne. 2 TB tedy maximální denní dat změní míra, která je podporovaný pro chráněné počítače.
- **Maximální výkon**– replikovanou počítače můžete patří do jednoho účtu úložiště v Azure. Standardní úložiště účtu můžete zpracování maximálně 20 000 požadavky za sekundu a doporučujeme, abyste počet procesorů v počítači zdroje 20 000. Pro příklad Pokud máte zdrojového počítače s 5 disků a každý disk generuje 120 procesorů (8K velikost) ve zdroji, bude nacházet v Azure na disku procesorů maximálně 500. Požadovanou velikost úložiště účtů povinné = zdroje celkem procesorů/20000.


### <a name="management-server-considerations"></a>Důležité informace o serveru správy

Server pro správu spouští součásti obnovení webu, kteří zpracovávat optimalizace dat, Správa a replikace. Měli být schopní zpracovat denní kapacity změnit rychlost přes všechny úlohami spuštěna chráněné počítačích a má dostatečnou šířku pásma pro nepřetržitě replikovat data k základnímu úložišti Azure. Konkrétně:

- Server proces přijímá replikace data z chráněné počítačů a optimalizuje ukládání do mezipaměti, komprese a šifrování před odesláním Azure. Server pro správu měli dostatečná zdroje provádět tyto úkoly.
- Server proces používá mezipaměti založené na disku. Doporučujeme disk samostatné mezipaměti nejméně 600 GB provádět změny dat uložených v případě kritický sítě nebo výpadku. Při nasazení můžete nakonfigurovat mezipaměti pro všechny jednotku, která obsahuje alespoň na úrovni 5 GB úložiště, ale je 600 GB minimální doporučení.
- Osvědčený doporučujeme ale, že server pro správu být umístěny ve stejné síti a LAN segmentu jako stroje, které chcete zamknout. Mohou být umístěny na jiné síti ale stroje, které chcete zamknout by měla být L3 sítě viditelnost k němu.

V následující tabulce jsou shrnuty velikost doporučení pro server pro správu.

**Server pro správu procesoru** | **Paměti** | **Velikost diskové mezipaměti** | **Změnit rychlost dat** | **Chráněný počítačích**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz) | 16 GB | 300 GB | 500 GB nebo nižší | Nasazení serveru správy pomocí těchto nastavení replikovat menší než 100 počítačích.
12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz) | 18 GB | 600 GB | 500 GB až 1 TB | Nasazení serveru správy pomocí těchto nastavení replikace mezi 100 150 počítačích.
16 vCPUs (2 sockets * 8 jádra @ 2,5 GHz) | 32 GB | 1 TB | 1 TB až 2 TB | Nasazení serveru správy pomocí těchto nastavení replikace mezi 150-200 počítačích.
Nasazení serveru jiného obrázku | | | > 2 TB | Pokud jste replikace více než 200 počítačích nebo pokud denní dat změnit rychlost větší než 2 TB nasaďte další proces servery.

Kde je:

- Každý počítač zdroj nastaven 3 disků 100 GB.
- Použijeme benchmarking úložiště 8 jednotek přidružení zabezpečení 10 kB ot RAID 10 měření diskové mezipaměti.

### <a name="network-bandwidth-from-source-to-target"></a>Šířka pásma ze zdroje na cíl
Ujistěte se, že výpočet šířky pásma, které by bylo nutné pro počáteční replikace a replikace delta pomocí [nástroje Plánovač kapacity](site-recovery-capacity-planner.md)

#### <a name="throttling-bandwidth-used-for-replication"></a>Omezení šířky pásma pro replikační

Přenosy VMware replikovat na Azure prochází server specifický proces. Můžete šířku pásma, který neexistuje replikace obnovení webu na tomto serveru takto:

1. Otevřít modulu snap-in konzoly MMC Zálohování Microsoft Azure na serveru správy hlavních nebo na serveru správy spuštěné další k dispozici proces servery. Ve výchozím nastavení zástupce, pro Microsoft Azure záloha vytvořena na ploše nebo najdete ji v: C:\Program Files\Microsoft Azure obnovení služby Agent\bin\wabadmin.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.

    ![Omezení šířky pásma](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Na kartě **Throttling** zadejte šířku pásma, kterou můžete použít k obnovení webu replikace a použitelné plánování.

    ![Omezení šířky pásma](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Volitelně můžete vytvořit také omezení pomocí Powershellu. Tady je příklad:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maximalizace využití šířky pásma
Pokud chcete zvýšit šířky pásma využít replikace obnovení webu Azure je potřeba změnit klíče registru.

Tento klíč řídí počet podprocesů na replikace disku, které se používají při replikace

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 V síti "overprovisioned" Tento klíč registru musí změnit výchozí hodnoty. Podporujeme maximálně 32.  


[Další informace](site-recovery-capacity-planner.md) o plánování podrobné kapacity.

### <a name="additional-process-servers"></a>Servery další obrázku

Pokud nepotřebujete chránit více než 200 počítače nebo je větší denní změna kurzu 2 TB můžete přidat další servery zpracovávání načíst. Zobrazit si můžete provést tyto akce:

- Zvýšení počtu servery správy. Můžete třeba chránit až 400 počítačích s dvěma servery správy.
- Přidat další proces servery a používat tyto zpracovávání přenosy místo (nebo kromě) server pro správu.

Tato tabulka popisuje scénáři, ve kterém:

- Nastavení původní server pro správu používat jako configuration server.
- Nastavení serveru další obrázku.
- Konfigurace chráněné virtuálních počítačích pro použití dalších proces serveru.
- Každý počítač chráněného zdroj nastaven tři disků 100 GB.

**Původní server pro správu**<br/><br/>(konfigurační server) | **Další proces serveru**| **Velikost diskové mezipaměti** | **Změnit rychlost dat** | **Chráněný počítačích**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 4 vCPUs (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti | 300 GB | 250 GB nebo nižší | Můžete replikovat 85 nebo menší počítačích.
8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti | 600 GB | 250 GB až 1 TB | Můžete replikovat mezi 85 150 počítačích.
12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz), 18 GB paměti | 12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz) 24 GB paměti | 1 TB | 1 TB až 2 TB | Můžete replikovat mezi 150 225 počítačích.


Způsob, ve kterém můžete změnit velikost serverech závisí na předvolby pro změnu velikosti nahoru nebo rozšiřování modelu.  Škálování nasazením několik kvalitní řízení a proces servery nebo rozšiřování nasazením další servery s méně prostředků. Příklad: v případě potřeby můžete chránit 220 počítačích může udělejte jednu z následujících akcí:

- Nakonfigurujte původní server pro správu 12vCPU, 18 GB paměti, serveru další proces s 12vCPU 24 GB paměti a konfigurace chráněné počítačů používat jenom server další obrázku.
- Nebo můžete taky můžete konfigurovat dva management (2 x 8vCPU, 16 GB paměti RAM) a dva další proces servery (1 x 8vCPU) a 4vCPU x 1 pro zpracování 135 + 85 (220) počítačích a konfigurace chráněné počítačů používat pouze servery další proces.


[Postupujte podle těchto pokynů](#deploy-additional-process-servers) k nastavení serveru další obrázku.

## <a name="before-you-start-deployment"></a>Než začnete nasazení

Tabulky Souhrn předpoklady pro nasazení tento scénář.


### <a name="azure-prerequisites"></a>Požadavky na Azure

**Předpoklady** | **Podrobnosti**
--- | ---
**Účet Azure**| Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Replikovanou data se ukládají v Azure úložiště a Azure VMs jsou nespředený nahoru, když dojde k selhání. <br/><br/>Potřebujete [Standardní geo nadbytečné úložiště účtu](../storage/storage-redundancy.md#geo-redundant-storage). Účet musí být ve stejné oblasti jako služba obnovení webu a být přidružené stejném předplatném. Všimněte si, že replikace k účtům úložiště premium není aktuálně podporován a není určená k použití.<br/><br/>Nepodporujeme přesunout úložiště účtů vytvořených pomocí [nového Azure portálu](../storage/storage-create-storage-account.md) mezi skupinami zdroje. [Přečtěte si informace o](../storage/storage-introduction.md) Azure úložiště.<br/><br/>
**Azure sítě** | Budete potřebovat Azure virtuální sítě, který Azure VMs připojí k selháním. Azure virtuální sítě musí být ve stejné oblasti jako trezoru obnovení webu.<br/><br/>Poznámka: budete potřebovat selhání zpět po přepnutí Azure VPN připojení (nebo Azure ExpressRoute) nastavení z Azure sítě k místnímu webu.


### <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

**Předpoklady** | **Podrobnosti**
--- | ---
**Server pro správu** | Potřebujete serveru Windows 2012 R2 místním systémem virtuálního počítače nebo pole fyzicky serveru. Všechny prvky obnovení webu místní nainstalovaných na tento server pro správu<br/><br/> Doporučujeme že nasazení serveru jako vysoce dostupné OM VMware. Překlopení zpět k místnímu webu z Azure je vždy k VMware VMs bez ohledu na to, jestli je selhání VMs nebo pole fyzicky servery. Pokud není konfigurace serveru správy jako VMware OM, budete muset nastavit server oddělené předlohy cílové jako VMware OM pro příjem navrácení vysílání.<br/><br/>Server by neměly být řadiče domény.<br/><br/>Server by měla být statické IP adresy.<br/><br/>Název hostitele serveru by měl být 15 znaků nebo menší.<br/><br/>Verze operačního systému; by měl být jenom angličtiny.<br/><br/>Server pro správu vyžadují připojení k Internetu.<br/><br/>Je třeba odchozí aplikace access ze serveru takto: dočasný přístup na HTTP 80 během instalace součástí obnovení webu (Pokud si chcete stáhnout MySQL); Probíhající přístup pro odchozí připojení na HTTPS 443 správy replikace; Probíhající přístup pro odchozí připojení na HTTPS 9443 přenosů replikace (Tento port lze upravit)<br/><br/> Zkontrolujte, jestli že jsou k dispozici ze serveru pro správu tyto adresy URL: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Pokud máte IP adresu brány firewall pravidla na serveru, zkontrolujte, jestli tato pravidla sdělení Azure. Musíte povolit [Rozsahy IP Azure Datacentra](https://www.microsoft.com/download/details.aspx?id=41653) a port HTTPS (443). Budete taky muset rozsahy IP adres seznamu bílé oblasti Azure předplatného a odstranění západní USA. Adresa URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") je pro její stažení MySQL.
**Host (hostitel) VMware vCenter/ESXi**: | Potřebujete jednu nebo více vMware vSphere ESX/ESXi hypervisory Správa virtuální počítače VMware systém ESX/ESXi verze 6.0, 5.5 nebo 5.1 s nejnovějšími aktualizacemi.<br/><br/> Doporučujeme že nasazení serveru vCenter VMware ke správě ESXi hosts. Ho by měl být verzí vCenter 6.0 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/>Poznámka: obnovení webu, které nejsou podporovány nové vCenter vSphere 6.0 funkcí jako křížové vCenter vMotion virtuální svazky a úložiště DRS. Podpora obnovení webů se omezí na funkce, které byly také k dispozici ve verzi 5.5.
**Chráněný stroje**: | **AZURE**<br/><br/>Stroje, které chcete zamknout by měly odpovídat s [Azure požadavky](site-recovery-best-practices.md#azure-virtual-machine-requirements) pro vytváření Azure VMs.<br><br/>Pokud se chcete připojit k Azure VMs po převzetí je potřeba povolit připojení ke vzdálené ploše na místní brána firewall.<br/><br/>Jednotlivé kapacita disku chráněné počítačích nesmí být více než 1023 GB. Virtuálního počítače můžete mít až 64 disků (tedy až 64 TB). Pokud máte disků větších než 1 TB zvažte replikace databáze ATP SQL Server vždy na stráž dat Oracle.<br/><br/>Minimální 2 GB volného místa na disku instalace pro instalaci součásti.<br/><br/>Sdílené hosta disku, které nejsou podporovány clusterů. Pokud máte na skupinový nasazení zvažte použití replikace databáze ATP SQL Server vždy na stráž dat Oracle.<br/><br/>Jednotné Extensible Firmware rozhraní (UEFI) / spouštěcí Extensible Interface(EFI) firmwaru není podporovaná.<br/><br/>Názvy by měl obsahovat mezi 1 až 63 znaků (písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslem a končit písmeno nebo číslici. Když počítači se po zamknutí Azure název lze upravit.<br/><br/>**VMware VMs**<br/><br>Budete muset nainstalovat VMware vSphere PowerCLI 6.0. na serveru správy (konfigurační server).<br/><br/>VMware VMs, které chcete zamknout měli VMware nástroje nainstalovanou a spuštěnou.<br/><br/>Pokud má zdroj OM týmové NIC se převede na jeden NIC po přepnutí Azure.<br/><br/>Pokud chráněné VMs disku iSCSI obnovení webu převede chráněné disku iSCSI OM do souboru virtuálního pevného disku při OM selhání Azure. Pokud cíle iSCSI zastižení OM Azure se připojit k cíle iSCSI a v podstatě najdete v článku dvou nebo více discích – disk virtuální pevný disk na OM Azure a disku iSCSI zdroje. V tomto případě musíte odpojit, které se objeví selhalo přes Azure OM cíle iSCSI.<br/><br/>[Další informace](#vmware-permissions-for-vcenter-access) o uživatelských oprávněních VMware, které jsou vyžadovány obnovení webu.<br/><br/> **WINDOWS SERVER počítačů (VMware OM nebo pole fyzicky server)**<br/><br/>Server by měl být podporované 64-bit operačním systémem: Windows serveru 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 s na nejnižších SP1.<br/><br/>Operační systém, měli byste mít nainstalované C:\ složky a s operačním systémem disk by měl být Windows základní disku (s operačním systémem neměli se systémem Windows dynamický.)<br/><br/>U počítačů s Windows Server 2008 R2 se musíte mít .NET Framework 3.5.1 nainstalovaný.<br/><br/>Budete muset zadat účtu správce (pouze správci místního počítače Windows) pro instalaci nabízených služeb mobilita na serverech Windows. Je-li zadané účet není doménovým účtem musíte zakázat ovládací prvek vzdáleného přístupu uživatele v místním počítači. [Další informace](#install-the-mobility-service-with-push-installation).<br/><br/>Obnovení webu podporuje VMs s diskem RDM.  Během navrácení obnovení webu opakovaně používat disku RDM Pokud původní zdrojový OM a RDM disk je k dispozici. Pokud nejsou k dispozici, během navrácení obnovení webu vytvoří nový soubor VMDK pro každý disk.<br/><br/>**LINUX POČÍTAČÍCH**<br/><br/>Budete potřebovat podporované 64bitový operační systém: červená klobouk Enterprise Linux 6,7; Centos 6.5, 6.6,6.7; Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts soubory chráněné počítačích smí obsahovat položky, které namapujte název místní hostitele IP adresy přidružené k některé síťové adaptéry. <br/><br/>Pokud chcete připojit k Azure virtuální počítači spuštěná Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), ověřit, že služba zabezpečené Shell na zamknutém počítač je nastavena na spustit automaticky při spuštění systému a že povolit pravidla brány firewall ssh připojení k ní.<br/><br/>Ochrana lze povolit pouze pro Linux počítačích s následující úložiště: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.<br/><br/>Obnovení webu podporuje VMs s diskem RDM.  Během navrácení Linux nemá obnovení webu znovu použít RDM disku. Místo toho vytvoří nový soubor VMDK pro každý odpovídající RDM disk.

Pouze Linux OM - zajistit nastavení nastavení disk.enableUUID=true v konfiguraci parametry OM v VMware. Pokud tento řádek neexistuje, přidejte ho. To je nutné zajistit konzistentní UUID VMDK tak, aby správně připojí. Navíc nezapomeňte, že bez toto nastavení používají, navrácení způsobí úplné stažení i v případě, že OM je k dispozici na prem. Přidání toto nastavení zajistí, že jsou pouze změny delta přenesených zpět během navrácení.

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru

1. Přihlaste se k [portálu Správa](https://manage.windowsazure.com/).
2. Rozbalte položku **Datové služby** > **Obnovení služby** a klikněte na **Webu obnovení trezoru**.
3. Klikněte na **vytvořit nový** > **vytvořit**.
4. Do pole **název**zadejte popisný název k identifikaci trezoru.
5. V **oblasti**vyberte zeměpisná oblast pro trezoru. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klikněte na **vytvořit trezoru**.
    ![Nové trezoru](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Zkontrolujte na stavovém řádku potvrďte, že byla úspěšně vytvořena trezoru. Trezoru se zobrazí jako **aktivní** na hlavní stránce **Obnovení služby** .

## <a name="step-2-set-up-an-azure-network"></a>Krok 2: Nastavte Azure síť

Nastavte si síť Azure tak, aby Azure VMs bude připojený k síti po převzetí a navrácení k místnímu webu mohli pracovat podle očekávání.

1. Na portálu Azure > **vytvořit virtuální síť** zadejte síťový název. IP adresy oblast a podsítě název.
2. Musíte přidat VPN/ExpressRoute k síti, pokud je třeba udělat navrácení. Virtuální privátní sítě nebo ExpressRoute lze přidat k síti i po překlopení.

[Přečtěte si další](../virtual-network/virtual-networks-overview.md) informace o Azure sítě.

> [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.

## <a name="step-3-install-the-vmware-components"></a>Krok 3: Instalace součásti VMware

Pokud chcete replikovat VMware virtuálních počítačích nainstalovat následující součásti VMware na serveru správy:

1. [Stažení](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) a instalace VMware vSphere PowerCLI 6.0.
2. Restartujte server.


## <a name="step-4-download-a-vault-registration-key"></a>Krok 4: Stáhněte si trezoru registračního klíče

1. Z řízení serveru spusťte konzolu obnovení webu v Azure. Na stránce **Služby Recovery** na trezoru a otevře se stránka rychlý Start. Představení aplikace můžete otevřít také kdykoli pomocí ikony.

    ![Ikona rychlým startem](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Na **Úvodní** stránce klikněte na **Zdroje Příprava cílové** > **Stáhnout registračního klíče**. Soubor je vytvořen automaticky. Platí pro 5 dní po vygenerování.


## <a name="step-5-install-the-management-server"></a>Krok 5: Nainstalujte server pro správu
> [AZURE.TIP] Zkontrolujte, jestli že jsou k dispozici ze serveru pro správu tyto adresy URL:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Na **Úvodní** stránce stáhněte jednotné instalační soubor na server.

2. Spuštění instalačního souboru v Průvodci nastavením obnovy jednotné webu spuštění instalačního programu.

3.  **Než začnete** vyberte **nainstalovat konfigurační server a proces server**.

    ![Než začnete](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. V **Jiných výrobců softwarová licence** klikněte na **mám přijmout** si stáhněte a nainstalujte MySQL. 

    ![Třetí = software výrobce](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. V **Registrace** Procházet a vyberte klávesu registrace, kterou jste si stáhli z trezoru.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. V **Dialogovém okně nastavení Internet** určete, jak bude poskytovatele na serveru konfigurace připojení k obnovení webu Azure přes internet.

    - Pokud se chcete připojit k proxy, která je aktuálně nastavena na počítači vyberte **připojit k existující nastavení proxy**.
    - Pokud chcete poskytovatele, který má přímé připojení vyberte **připojení přímo bez proxy**.
    - Pokud existující proxy server vyžaduje ověření a chcete použít vlastní proxy poskytovatel připojení vyberte **připojení s nastavením proxy serveru vlastní**.
        - Pokud používáte vlastní proxy, budete muset zadat adresu, portů a přihlašovací údaje
        - Pokud používáte proxy jste měli už povolili následující adresy URL:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Brána firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Zkontrolujte požadavky na** instalaci spuštěn zaškrtnutí a ujistěte se, mohlo by umožnit spuštění instalace. 

    
    ![Zjistit předpoklady pro](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Pokud se zobrazí upozornění o **globální čas synchronizace zkontrolujte** ověřte čas na systémové hodiny (nastavení**data a času** ) se stejná jako časové pásmo.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. **Konfigurace MySQL** Vytvořte přihlašovací údaje pro přihlášení k instanci server MySQL, která budou nainstalovány.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. V poli vyberte **Podrobnosti prostředí** jestli budete chtít replikovat VMware VMs. Pokud se nastavení zkontroluje nainstalované PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. V poli **Umístění instalace** vyberte místo, kam chcete nainstalovat binární soubory a ukládání do mezipaměti. Můžete vybrat jednotku, která obsahuje alespoň na úrovni 5 GB úložiště, ale doporučujeme jednotku mezipaměti s nejméně 600 GB volného místa.

    ![Umístění instalace](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. **Network** výběru určete posluchače (síťový adaptér a SSL port) ve kterém bude konfigurační server odesílat a přijímat replikace data. Můžete změnit výchozí port (9443). Kromě tento port port 443 se bude používat tak, že na webový server, který orchestrates replikace operace. pro příjem replikace není vhodné používat 443.


    ![Výběr sítě](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  V **souhrnu** zkontrolujte informace a klikněte na tlačítko **nainstalovat**. Po dokončení instalace vygeneruje se přístupové heslo. Budete potřebovat, když povolíte replikace tak zkopírujte ho a udržovat její na zabezpečeném místě.

    ![Souhrn](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  Přečtěte si informace v **souhrnu** .

    ![Souhrn](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure služby agenta obnovení proxy musí být nastavení.
>Po dokončení instalace spusťte aplikaci s názvem "Microsoft Azure obnovení služby prostředí" z nabídky Start systému Windows. V okně příkazového, které se objeví sadu následujících příkazů nastavit nastavení proxy serveru.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Spuštění instalačního programu z příkazového řádku

Můžete taky spustit Průvodce jednotné z příkazového řádku následujícím způsobem:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Kde je:

- / ServerMode: povinný. Určuje, zda instalaci nainstalovat servery konfigurace a proces nebo jenom server obrázku (použili k instalaci další proces servery). Vstupní hodnoty: CS, PS.
- InstallDrive: povinný. Určí složku nainstalovanou součásti.
- / MySQLCredFilePath. Povinný. Určuje cestu k souboru, kde jsou pověření server MySQL textu. Pokud potřebujete šablonu, kterou chcete vytvořit soubor.
- / VaultCredFilePath. Povinný. Umístění souboru trezoru přihlašovací údaje
- / EnvType. Povinný. Typ instalace. Hodnoty: VMware NonVMware
- / PSIP a /CSIP. Povinný. IP adresu serveru obrázku a konfigurace serveru.
- / PassphraseFilePath. Povinný. Umístění souboru heslo.
- / ByPassProxy. Volitelné. Určuje, že server pro správu ke kterému je připojen Azure bez na proxy server.
- / ProxySettingsFilePath. Volitelné. Určuje nastavení pro vlastní proxy (výchozí proxy serveru, který vyžaduje ověření) nebo vlastní proxy




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Krok 6: Nastavení přihlašovacích údajů pro službu vCenter

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Server procesu, mohli automaticky zjišťovat VMware VMs spravovaných serverem vCenter. Pro automatické zjišťování obnovení webu vyžaduje účet a přihlašovací údaje, které můžete získat přístup k vCenter serveru. Toto není důležité, pokud jste replikace fyzické servery.

Udělejte to takto:

1. V vCenter serveru vytvořit roli (**Azure_Site_Recovery**) na úrovni vCenter s [potřebná oprávnění](#vmware-permissions-for-vcenter-access).
2. Přiřazení **Azure_Site_Recovery** role vCenter uživateli.

    >[AZURE.NOTE] VCenter uživatelský účet, který má roli jen pro čtení můžete spustit převzetí bez ukončení chráněného zdroj počítačích. Pokud chcete vypnout těchto počítačů musíte roli Azure_Site_Recovery. Všimněte si, že pokud máte jenom migrace VMs z VMware Azure a nemusíte navrácení potom roli jen pro čtení dostatečné.

3. Chcete-li přidat účet otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
2. na kartě **Správa účtů** klikněte na **Přidat účet**.

    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. **Podrobnosti o účtu** přidejte přihlašovací údaje, které lze použít pro přístup k vCenter serveru. Všimněte si, že to může trvat delší než 15 minut pro název účtu, který se zobrazí na portálu. Okamžitou aktualizaci, klikněte na tlačítko Aktualizovat na kartě **Servery konfigurace** .

    ![Podrobnosti](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Krok 7: Přidání vCenter servery a ESXi tabulkami hosts

Pokud jste replikace VMware VMs budete muset přidat vCenter server (nebo ESXi hostitele).

1. Na **serverech** > **Konfigurace servery** zaškrtněte políčko konfigurační server > **Přidat vCenter serveru**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Přidání vCenter server nebo ESXi hostitele podrobnosti, název účtu, který jste zadali pro přístup k serveru vCenter v předchozím kroku a proces serveru, na kterém se použijí k Seznamte se s VMs VMware, který je spravováno z vCenter serveru. Poznámka: vCenter serveru nebo ESXi hostitele by měl být umístěny ve stejné síti jako serveru, na kterém je nainstalovaná server procesu.

    >[AZURE.NOTE] Pokud přidáváte vCenter serveru nebo ESXi host pomocí účtu, který nemá oprávnění správce serveru vCenter nebo Host (hostitel), zkontrolujte vCenter nebo ESXi účty oprávnění tyto povolené: datacentru, úložiště, složky, Jost, sítě, zdrojů, virtuální počítače, vSphere distribuované přepínač. Kromě toho vCenter serveru musí oprávnění zobrazení úložiště.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Po dokončení zjišťování vCenter server bude uveden na kartě **Servery konfigurace** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Krok 8: Vytvoření ochrany skupiny

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Ochranu skupiny jsou logické seskupení virtuálních počítačích nebo pole fyzicky servery, které chcete zamknout pomocí stejné nastavení zámku. Použití nastavení zámku ochranu skupiny a tato nastavení platí pro všechny virtuálních počítačích/fyzické stroje, které můžete přidat do skupiny.

1. Otevření **Položek chráněné** > **Ochranu skupiny** a klikněte na Přidat skupinu zámek.

    ![Vytvoření ochrany skupiny](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Na stránce **Zadejte nastavení ochrany skupiny** zadejte název skupiny a **od** vyberte konfigurační server, na kterém chcete vytvořit skupinu. **Cíl** je Azure.

    ![Nastavení ochrany skupiny](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Na stránce **Zadejte nastavení replikace** konfigurace replikace nastavení, která se použije pro všechny počítače ve skupině.

    ![Replikace ochrany skupiny](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Soulad s víc OM**: Pokud to zapnete vytváří sdílené aplikace konzistentní obnovení body v počítačích ve skupině zámek. Toto nastavení je nejrelevantnější při všech počítačů ve skupině zámek spuštění stejného pracovního vytížení. Všechny počítače bude obnoven stejný datový bod. Toto je k dispozici, jestli jste replikace VMware VMs nebo pole fyzicky servery systému Windows nebo Linux.
    - **Mezní hodnota operace RPO**: Nastaví operace RPO. Upozornění vygeneruje při replikace ochranu souvislá data překračuje nakonfigurované operace RPO mezní hodnota.
    - **Obnovení přejděte uchovávání informací**: Určuje okno uchovávání informací. Chráněný počítačích můžete obnovit až kamkoli v tomto okně.
    - **Aplikace konzistentní počet_plateb snímek**: Určuje, jak často se body obnovení obsahující konzistenci aplikací snímky vytvoří.

Po kliknutí na zaškrtnutí ochranu skupiny se vytvoří nahraďte názvem, který jste zadali. In addition vytvoření druhého ochranu skupiny s názvem < ochranu skupiny název navrácení). Tuto skupinu ochranu slouží navrácení k místnímu webu po přepnutí Azure. Ochranu skupiny můžete sledovat, jak jste vytvořili na stránce **Chráněné položky** .

## <a name="step-9-install-the-mobility-service"></a>Krok 9: Instalace služby mobilita

Cílem prvního kroku s povolením ochranu virtuálních počítačích a fyzických serverů je instalace služby nastavení mobilních zařízení. Můžete to udělat dvěma způsoby:

- Automaticky nabízená a nainstalujte službu na každém počítači ze serveru obrázku. Všimněte si, že při přidání stroje ochranu skupiny a už jsou vhodné verzi instalace mobilita služba nabízených nedojde.
- Automaticky nainstalujte službu metodou vaší organizace nabízených WSUS ATP Správce konfigurace pro System Center. Zkontrolujte, že server pro správu jste nastavili než to uděláte.
- Ruční instalace na každém počítači, které chcete zamknout. načit jisti, že jste nastavili server pro správu než to uděláte.


### <a name="install-the-mobility-service-with-push-installation"></a>Instalace služby nastavení mobilních zařízení s instalací připínáčku

Při přidání stroje ochranu skupiny služby mobilita automaticky posune a nainstalovaný na každém počítači serverem obrázku.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Příprava na automatické nabízená v počítačích Windows

Tady je postup, jak připravit počítačích Windows tak, aby může proces serveru automaticky nainstalována služba nastavení mobilních zařízení.

1.  Vytvořte účet, který lze serverem proces přístup k počítači. Účet by měl oprávnění správce (místní nebo název domény). Všimněte si, že těchto přihlašovacích údajů se používají pouze pro nabízených instalace služby nastavení mobilních zařízení.

    >[AZURE.NOTE] Pokud nepoužíváte účet domény musíte zakázat ovládací prvek vzdáleného přístupu uživatele v místním počítači. K tomuto účelu v seznamu v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System přidejte položku DWORD LocalAccountTokenFilterPolicy s hodnotou 1 v části. Chcete-li přidat registru položkou z rozhraní příkazového řádku otevřete příkaz nebo pomocí prostředí PowerShell zadejte **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Na brána Windows Firewall počítače chcete zamknout, vyberte **Povolit aplikaci nebo funkci průchod bránou Firewall** a povolte **sdílení souborů a tiskáren** a **Přístrojového vybavení správy systému Windows**. U počítačů, které patří do domény můžete nakonfigurovat zásadu brány firewall s objekt zásad skupiny.

    ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Přidejte účet, který jste vytvořili:

    - Otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
    - Na kartě **Správa účtů** klikněte na **Přidat účet**.
    - Přidejte účet, který jste vytvořili. Po přidání účtu musíte zadat pověření při přidání stroje ochranu skupiny.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Příprava na automatické nabízených na serverech Linux

1.  Ujistěte se, že Linux počítače, který chcete zamknout podporované podle popisu v [místním požadavky](#on-premises-prerequisites). Zkontrolujte, že mezi počítači, ve kterém chcete zamknout připojení k síti a serveru pro správu, která spustí proces serveru.

2.  Vytvořte účet, který lze serverem proces přístup k počítači. Účet by měl být uživatel root na zdrojový server Linux. Všimněte si, že těchto přihlašovacích údajů se používají pouze pro nabízených instalace služby nastavení mobilních zařízení.

    - Otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění instalace].
    - Na kartě **Správa účtů** klikněte na **Přidat účet**.
    - Přidejte účet, který jste vytvořili. Po přidání účtu musíte zadat pověření při přidání stroje ochranu skupiny.

3.  Zkontrolujte, zda soubor/etc/hosts na zdrojový server Linux obsahuje položky, které místní hostname mapují IP adresy přidružené k některé síťové adaptéry.
4.  Nainstalujte nejnovější funkci openssh, funkci openssh server, openssl balíčků v počítači, který chcete zamknout.
5.  Zkontrolujte, zda SSH povolené a spuštěnou na port 22.
6.  Ověření SFTP podsystém a heslo v souboru sshd_config následujícím způsobem:

    - Přihlaste se jako kořenovou.
    - V souboru /etc/ssh/sshd_config vyhledejte řádek, který začíná PasswordAuthentication.
    - Zrušte komentář řádku a změňte hodnotu z **žádné** na hodnotu **Ano**.
    - Najděte řádek, který začíná **podsystém** a zrušte komentář řádku.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ruční instalace služby mobilita

Instalační programy jsou dostupné v C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Zdrojový operační systém | Soubor Instalační služby mobilita
--- | ---
Windows Server (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 s aktualizací SP3 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64-bit pouze) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Ruční instalace v systému Windows server


1. Stáhnout a spustit relevantní instalačního programu.
2. V **než začnete **vyberte **mobilita služby**.

    ![Nastavení mobilních zařízení služby](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. **Konfigurace serveru** podrobnosti zadejte IP adresu serveru pro správu a heslo, vygenerované při instalaci součásti server pro správu. Načtete heslo opětovným spuštěním: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na serveru správy.

    ![Nastavení mobilních zařízení služby](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. **Instalace** umístění ponechte výchozí umístění a klikněte na tlačítko **Další** zahájíte instalaci.
5. V **Průběhu instalace** sledovat instalaci a po zobrazení výzvy restartovat počítač.

Taky můžete nainstalovat z příkazového řádku:

UnifiedAgent.exe [/ Role < Agent/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Kde je:

- / Role: povinný. Určuje, zda měli byste mít nainstalované službu nastavení mobilních zařízení.
- / InstallLocation: povinný. Určuje, kde k instalaci služby.
- / PassphraseFilePath: povinný. Určuje heslo konfigurace serveru.
- / LogFilePath: povinný. Určuje umístění souborů protokolu instalačního programu

#### <a name="uninstall-mobility-service-manually"></a>Ruční odinstalace služby mobilita

Nastavení mobilních zařízení služby lze odinstalovat pomocí programu přidat odebrat z ovládacích panelů nebo pomocí příkazového řádku.

Příkaz Odinstalovat se službou nastavení mobilních zařízení v okně příkazového řádku

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Úprava IP adresu serveru pro správu

Po spuštění průvodce můžete upravit IP adresu serveru správy následujícím způsobem:

1. Otevřete soubor hostconfig.exe (umístěný na ploše).
2. Na kartě **globální** můžete změnit IP adresu serveru pro správu.

    >[AZURE.NOTE] Pouze změňte IP adresu serveru pro správu. Číslo portu komunikaci se serverem správy musí být 443 a vlevo povolené použití HTTPS. Nesmí změnit heslo.

    ![Správa serveru IP adresa](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Ruční instalace na serveru Linux:

1. Zkopírujte odpovídající vkládání archivu založený na tabulce nad do počítače Linux, který chcete zamknout.
2. Spusťte aplikaci prostředí a extrahovat ZIP vkládání archiv na místní cestu spuštěním:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Vytvoření souboru passphrase.txt místního adresáře, do kterého jste extrahovali obsah vkládání archivu. To zkopírujte heslo z C:\ProgramData\Microsoft Azure webu Recovery\private\connection.passphrase na serveru správy a uložte ho ve passphrase.txt tokem poš *`echo <passphrase> >passphrase.txt`* v prostředí.
4. Instalace služby mobilita zadáním *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Určení vnitřní IP adresu serveru pro správu a ujistěte se, že je vybrán port 443.

**Můžete taky nainstalovat z příkazového řádku**:

1. Zkopírujte heslo z C:\Program Files (x86) \InMage Systems\private\connection na serveru správy a uložit jako "passphrase.txt" na serveru správy. Spusťte tyto příkazy. V našem příkladu je IP adresa serveru správy 104.40.75.37 a HTTPS port by měl být 443:

Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Instalace na hlavní cílovém serveru:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Krok 10: Povolit ochranu pro počítač

Povolení ochrany přidáte virtuálních počítačích a fyzické servery ke skupině zámek. Než začnete, Poznámka Pokud jste ochrana VMware virtuálních počítačích:

- VMware VMs jsou zjištěny každých 15 minut a může trvat víc než 15 minut, aby se zobrazí na portálu obnovení webu po zjišťování.
- Změny prostředí virtuální počítače (například instalace nástrojů VMware) může trvat delší než 15 minut mají být aktualizovány v obnovení webu.
- Při posledním zjištěnou můžete vyhledat VMware VMs v oblasti **Poslední kontakt na** hostitele serveru/ESXi vCenter na kartě **Servery konfigurace** .
- Pokud máte vytvořen ochranu skupiny a přidání vCenter serveru nebo ESXi hostitele po, může trvat delší než 15 minut portál obnovení webu Azure a aktualizace a odstranění virtuálních počítačích uvedena v dialogovém okně **Přidat počítače na ochranu skupiny** .
- Pokud chcete pokračovat v okamžitě přidání stroje ochranu skupiny, aniž byste čekali plánované zjišťování, zvýrazněte konfigurační server (neklikejte na ho) a klikněte na tlačítko **Aktualizovat** .

Kromě toho Všimněte si, že:

- Doporučujeme architektonické ochranu skupiny, aby odpovídaly svého pracovního vytížení. Přidávejte počítače se systémem dané aplikace do stejné skupiny.
- Po přidání stroje ke skupině zámek server procesu automaticky posune nainstaluje automaticky službu mobilita Pokud ještě není nainstalovaný Všimněte si, že musíte mít mechanismus nabízených připravit podle popisu v předchozím kroku.


Přidání stroje ochranu skupiny:

1. Klepněte na **chráněný položky** > **Ochranu skupiny** > **počítače** > Přidat počítačích. \As doporučený postup
2. **Vyberte virtuálních počítačích** se chránit VMware virtuálních počítačích, vyberte vCenter serveru, který je Správa virtuálních počítačích (neboli EXSi host, na kterém máte systém) a potom vyberte počítače.

    ![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  Ve **virtuálních počítačích vyberte** Pokud jste chránit fyzických serverů, v průvodci **Přidat fyzických počítačů** zadejte IP adresu a popisný název. Vyberte skupinu operační systém.

    ![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. V **Zadejte prostředcích cílové** vyberte úložiště účet, který používáte pro replikace a vyberte, zda má být použita nastavení pro všechny úlohami. Všimněte si, že účty úložiště premium nepodporují aktuálně.

    >[AZURE.NOTE] 1.v nepodporujeme přesunout úložiště účtů vytvořených pomocí [nového Azure portálu](../storage/storage-create-storage-account.md) mezi skupinami zdroje.                           2.[migrace úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

    ![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. **Určení** účty vyberte účet [nakonfigurovaný](#install-the-mobility-service-with-push-installation) pro použití pro automatické instalace služby nastavení mobilních zařízení.

    ![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Klikněte na značku zaškrtnutí dokončíte přidávání počítačích ochranu skupiny a spuštění počáteční replikace pro každý počítač.

    >[AZURE.NOTE] Pokud nabízená instalace připravený mobilita na počítačích, ve kterých není ho jako někdo přidá do skupin ochrany automaticky nainstalovaná služba. Po instalaci služby úlohy ochranu spustí a přestane. Po selhání musíte restartovat každý počítač, který byl službu nastavení mobilních zařízení nainstalovaný. Po restartování ochranu úlohy, nebude zahájen znovu a počáteční replikace.

Můžete sledovat stav na stránce **úlohy** .

![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Kromě toho můžete sledovat stav ochrany v **Chráněném položky** > <protection group name> > **virtuálních počítačích**. Po dokončení počáteční replikace a data synchronizovat, stav počítače změní na** chráněné**.

![Povolení ochrany](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Krok 11: Nastavení chráněné vlastnosti stroje

1. Po stroje má stav **chráněného** můžete nakonfigurovat vlastností překlopení. V podrobnostech ochranu skupiny vyberte počítače a otevřete kartu **Konfigurovat** .
2. Obnovení webu automaticky navrhne vlastnosti pro OM Azure a zjistí, že že místní síti nastavení.

    ![Nastavení vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Můžete změnit následující nastavení:

    -  **Název Azure OM**: to je název, který budou mít počítači v Azure po překlopení. Název musí vyhovovat požadavkům Azure.

    -  **Azure OM velikost**: počet síťové adaptéry je dáno velikost určit cílový virtuálního počítače. [Přečtěte si další](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) informace o velikosti a adaptéry. Všimněte si, že:

        - Když změníte velikost virtuálního počítače a uložte nastavení, při otevřete kartu **Konfigurovat** příště změní počet síťový adaptér. Počet síťové adaptéry cílové virtuálních počítačích je minimální počet síťové adaptéry na zdroj virtuálního počítače a maximální počet síťové adaptéry nepodporuje velikost virtuálního počítače zvolili.
            - Pokud argument číslo síťové adaptéry zdrojového počítače menší nebo rovna hodnotě počet adaptéry povolená velikost v počítači cílového, pak cíl bude mít stejný počet adaptéry jako zdroj.
            - Pokud počet adaptéry virtuálního počítače zdroj překročí povolený Cílová velikost a pak maximální velikost cílové bude použito počet.
            - Pokud například zdrojového počítače obsahuje dva síťové adaptéry a cílová velikost počítač podporuje čtyři, cílový počítač bude mít dva adaptéry. Pokud zdrojového počítače obsahuje dva adaptéry ale podporované Cílová velikost podporuje pouze jednu cílový počítač bude mít jenom jeden adaptér.
        - Pokud počítač virtuální více síťové adaptéry, které mají všechny adaptéry připojení k stejné Azure síti.
    - **Azure sítě**: Zadejte Azure sítě, který Azure VMs bude připojen k po překlopení. Pokud nezadáte jednu Azure VMs nesmí být připojeni k jakákoli síť. Kromě toho musíte zadat Azure síť, pokud chcete navrácení z Azure k místnímu webu. Navrácení vyžaduje připojení VPN mezi Azure sítí a v místní síti.
    - **Azure IP adresa nebo podsítě**: pro každý síťový adaptér vyberete podsítě, ke kterému OM Azure připojen. Všimněte si, že:
        - Pokud síťový adaptér zdrojového počítače nakonfigurovaný tak, aby pomocí statické IP adresy můžete určit statickou IP adresu pro OM Azure. Pokud neposkytnete statickou IP adresu se mají přidělit všechny dostupné IP adresu. Pokud není zadán cílová IP adresa, ale je už nepoužívá jiný OM v Azure převzetí nezdaří. Pokud síťový adaptér zdrojového počítače nakonfigurovaný pro použití protokolu DHCP budete mít DHCP nastavením Azure.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Krok 12: Vytvoření plánu pro obnovení a spusťte selhání

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Můžete spustit přepojení pro jeden počítač nebo selhání více virtuálních počítačích, které zvolený nebo spuštění stejného pracovního vytížení. Překlopení více počítačů najednou přidat je do plánu obnovení.

### <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

1. Na stránce **Obnovení plány jednotného zasílání zpráv** klikněte na možnost **Přidat obnovení plán** a přidejte plán obnovení. Zadejte podrobnosti pro plán a vyberte **Azure** jako cíl.

    ![Konfigurace obnovení plán](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. V **Vyberte virtuální počítač** vyberte ochranu skupiny a pak vyberte počítačích ve skupině přidat do plánu obnovení.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Můžete přizpůsobit budete chtít vytvořit skupiny a pořadí pořadí, ve kterém jsou selhání počítačích v plánu obnovení. Můžete taky přidat skripty a pokynů pro ruční akce. Skripty můžete vytvořit, ať už ručně nebo pomocí [Runbooks automatizaci Azure](site-recovery-runbook-automation.md). [Další informace](site-recovery-create-recovery-plans.md) o přizpůsobení obnovení plány.

## <a name="run-a-failover"></a>Spuštění selhání

Aby se vám nestalo přepojení Všimněte si, že:


- Ujistěte se, že běží server pro správu a k dispozici - jinak převzetí se nezdaří.
- Při spuštění neplánované převzetí poznámky, které:

    - Pokud je to možné měli vypnete primární počítačích před spuštěním neplánované převzetí. Zajistíte tak, že nemáte do něj zdrojovou a otevřené počítače se systémem ve stejnou dobu. Pokud jste replikace VMware VMs pak při spuštění neplánované převzetí můžete určit, že obnovení webu připomínat nejlepší plánování řízené úsilí k vypnutí počítače zdroje. V závislosti na stav primární webu může nebo nemusí fungovat. Pokud jste replikace fyzické servery obnovení webu nenabízí možnost.
    - Když provedete neplánované převzetí zastaví replikace dat z primární počítačů tak všechny delta data nebude dojít ke ztrátě určitého po zahájení neplánované převzetí.

- Pokud chcete připojit k počítači virtuální otevřené v Azure po překlopení, před spustit záložní a povolit připojení RDP přes bránu firewall povolte připojení ke vzdálené ploše počítače zdroje. Taky musíte tak, aby RDP veřejné koncový bod služby Azure virtuálního počítače po překlopení. Použijte tyto [Doporučené postupy](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) zajistit, aby fungovala RDP po přepojení.

>[AZURE.NOTE] Nejlepší možný výkon zobrazí, když provedete přepojení Azure, zajistit nainstalované agenta Azure v zamknutém počítače. Pomáhají při spuštění rychleji a taky pomůže v diagnostice pro nečekané problémy. Linux agent může být nalezených [tady](https://github.com/Azure/WALinuxAgent) - a Windows agent najdete [tady](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Spusťte test selhání

Spusťte test selhání tak, aby napodobily převzetí a obnovení procesů izolovaná síť, který nemá vliv provozním prostředí a běžná replikace pokračuje jako obvykle. Test převzetí zahájí ve zdroji a můžete ji spustit několika způsoby:

- **Není určení Azure sítě**: Pokud spustíte test selhání bez připojení k síti test se jednoduše zkontrolujte, jestli virtuálních počítačích spustit a zobrazí správně v Azure. Virtuálních počítačích nebudete připojení k síti Azure po překlopení.
- **Určení Azure sítě**: Tento typ převzetí zkontroluje až podle očekávání pochází celý replikace prostředí a Azure virtuálních počítačích připojeni k Zadaná síť.


1. Na stránce **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **Testovat převzetí**.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. V **Potvrďte testování převzetí** vyberte **žádný** označíte, že nechcete Azure sítě pro překlopení test, nebo vyberte podle sítě, ke kterému se test VMs připojený po překlopení. Zaškrtněte políčko Spustit záložní.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Sledovat průběh převzetí na kartě **projekty** .

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Po dokončení záložní je také třeba neuvidíte otevřené Azure počítače se zobrazí v Azure portál > **virtuálních počítačích**. Pokud chcete zahájit připojení ke vzdálené ploše angličtině Azure musíte otevřít port 3389 na koncový bod OM.

5. Jakmile jste hotovi, převzetí dosažení dokončit testování fázi, klikněte na tlačítko úplný zkušební dokončete. V poznámkách zaznamenat a uložit všechny připomínky přidružený k převzetí testu.

6. Klikněte na **dokončení testu převzetí** automaticky vyčištění testovacím prostředí. Po dokončení mapování převzetí test se zobrazí stav **Dokončeno** . Budou odstraněny všechny prvky nebo VMs vytvářeny automaticky při selhání testu. Všimněte si, že pokud test přepojení trvají delší než dva týdny dokončení silou.



### <a name="run-an-unplanned-failover"></a>Spuštění neplánované překlopení

Neplánované převzetí je spuštěná z Azure a lze provést i v případě, že hlavní web není dostupný.


1. Na stránce **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **převzetí** > **Neplánované převzetí**.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Pokud jste replikace VMware virtuálních počítačích můžete vybrat si můžete vyzkoušet a vypněte VMs místní. Toto je nejlepší dostupné a převzetí zůstanou, zda plánování řízené úsilí povede nebo ne. Pokud není úspěšné podrobnosti o chybě se zobrazí na kartě **úlohy **> **Neplánované převzetí úlohy**.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Tato možnost není dostupná, pokud jste replikace fyzické servery. Musíte si můžete vyzkoušet a vypněte můžou být ručně Pokud je to možné.

3. V **Potvrďte převzetí** ověření převzetí směru (Azure) a vyberte obnovení bod, který chcete použít pro záložní. Pokud povolíte více OM při konfiguraci vlastností replikace můžete obnovit poslední bod obnovení aplikace nebo dojde k chybě konzistentní. Můžete také vybrat **vlastní obnovení přejděte** na obnovit předchozí v čase. Zaškrtněte políčko Spustit záložní.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Počkejte na dokončení úlohy neplánované převzetí. Můžete sledovat průběh převzetí na kartě **projekty** . Všimněte si, že to i v případě chyby při neplánované selhání plánu obnovy běží až po dokončení. Je třeba také neuvidíte otevřené Azure počítače aplikace ve virtuálních počítačích v portálu Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Připojení k replikovat Azure virtuálních počítačích po překlopení

Budete moct připojit k replikovat virtuálních počítačích v Azure po převzetí tady, co budete potřebovat:

1. Připojení ke vzdálené ploše používat na primární počítač.
2. Brána Windows Firewall primární počítače umožňuje RDP.
3. Po převzetí musíte přidat RDP veřejné koncový bod pro Azure virtuální počítač.

[Přečtěte si další](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) informace o nastaveních.


## <a name="deploy-additional-process-servers"></a>Nasazení servery další obrázku

Pokud máte zobrazit nasazení za 200 počítačích zdroje či celkové denní kurz konve překračuje limit 2 TB, musíte mít servery další proces zpracování objemem. Nastavení serveru další proces Zkontrolujte požadavky na [servery další obrázku](#additional-process-servers) a postupujte podle pokynů v tomto poli nastavení serveru obrázku. Po nastavení serveru můžete nakonfigurovat počítačích zdroje se používá.

### <a name="set-up-an-additional-process-server"></a>Nastavení serveru další obrázku

Nastavení serveru další proces následujícím způsobem:

- Pomocí Průvodce jednotné konfigurace serveru správy jako server obrázku.
- Pokud si chcete spravovat replikace dat pomocí nového serveru proces musíte migrace počítače chráněné akce.

### <a name="install-the-process-server"></a>Instalace serveru obrázku

1. Na úvodní stránce jednotné instalace moct soubor stáhněte pro instalaci součásti obnovení webu. Spuštění instalačního programu.
2. V **než začnete** vyberte **Přidat další proces serverů rozšiřování nasazení**.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Dokončete Průvodce stejným způsobem, kdy jste se [nastavit](#step-5-install-the-management-server) první server pro správu.

4. **Konfigurace serveru** podrobnosti zadejte IP adresu původní server pro správu, na které jste si nainstalovali konfigurační server a heslo. Původní serveru správy spustit ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** získat heslo.

    ![Přidání obrázku serveru](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrace počítačích serverem nového obrázku

1. Otevřete **Konfigurace servery** > **serveru** > název původního serveru správy > **Podrobnosti o Server**.

    ![Aktualizovat server obrázku](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. V seznamu **Servery obrázku** klikněte na **Změnit proces Server** vedle serveru, který chcete změnit.

    ![Aktualizovat server obrázku](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. Na **Serveru proces změnit** > **Cílový proces Server** vyberte nové server pro správu a pak vyberte virtuálních počítačích zpracovávající nového obrázku serveru. Klikněte na informační ikonu k získání informací o serveru. Průměrná místo potřebné k replikovat každý vybraný virtuální počítač nový server proces se zobrazí, budete moct přijímat načíst rozhodnutí. Zaškrtněte políčko Spustit replikace do nového obrázku serveru.

    ![Aktualizovat server obrázku](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>Oprávnění VMware vCenter přístup pomocí protokolu

Server procesu, mohli automaticky zjišťovat VMs na serveru vCenter. Provádět automatické zjišťování musíte definovat role (Azure_Site_Recovery) na úrovni vCenter povolit obnovení webu pro přístup k vCenter serveru. Poznámka: Pokud potřebujete pouze migrace VMware počítačích Azure a ne muset navrácení z Azure, můžete určit roli jen pro čtení, který je dostatečně. Nastavení oprávnění podle popisu v [Krok 6: Nastavte přihlašovací údaje pro vCenter server](#step-6-set-up-credentials-for-the-vcenter-server) oprávnění role jsou shrnuté v následující tabulce.

**Role** | **Podrobnosti** | **Oprávnění**
--- | --- | ---
Azure_Site_Recovery role | Zjišťování VMware OM |Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Přidělení místa procházet úložiště, zhoršeným úrovně soubor operace., soubor odebrat, aktualizace souborů virtuálního počítače -> úložiště<br/><br/>Síť -> přiřadit sítě<br/><br/>Zdroje -> přiřadit virtuální počítač fondu zdrojů, migrace vypnutý virtuálního počítače, migrace zapnutý virtuálního počítače<br/><br/>Úkoly -> Vytvořit úkol, aktualizace úkolu<br/><br/>Konfigurace -> virtuálního počítače<br/><br/>Virtuální počítač -> interakce -> odpověď otázku, připojení zařízení?, konfigurace CD-ROM, konfigurovat disketa média, vypněte Power na, nainstalujte si VMware nástroje<br/><br/>Virtuální počítač -> zásob -> Unregister vytvoření rejstříku<br/><br/>Virtuální počítač -> Provisioning -> stáhnout povolit virtuálního počítače, souborů virtuálního počítače povolit odeslat<br/><br/>Virtuální počítač -> snímky -> Odstranit snímky
role uživatele vCenter | VMware OM zjišťování/převzetí bez ukončení zdroje OM | Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Objekt Datacentrem –> rozšíření podřízené objekt role = jen pro čtení <br/><br/>Uživatel se přiřadí úrovni datacentra a tedy má přístup pro všechny objekty v datacentra.  Pokud chcete omezit přístup, přiřadíte roli **bez přístupu** k objektu **rozšíření dítě** k podřízeným objektům (ESX tabulkami hosts, datastores, VMs a sítě).
role uživatele vCenter | Překlopení a překlopení zpět | Přiřazení tato oprávnění pro v centru serveru:<br/><br/>Objekt Datacentra – rozšíření podřízené objekt role = Azure_Site_Recovery<br/><br/>Uživatel se přiřadí úrovni datacentra a tedy má přístup pro všechny objekty v datacentra.  Pokud chcete omezit přístup, přiřadíte roli **přístup **s **rozšíření pro podřízený objekt** podřízeného objektu (ESX tabulkami hosts, datastores, VMs a sítě).  



## <a name="third-party-software-notices-and-information"></a>Oznámení o třetích stran software a informací

Co nedělat překlad nebo Localize

Software a firmware spuštěné v produktu společnosti Microsoft nebo služby je založená na nebo zahrne materiál z projektů uvedených pod ním (společně "výrobců kódu").  Společnost Microsoft se není původní autor výrobců kódu.  Původní autorských práv a licence, pod kterým společnost Microsoft tyto výrobců kódu, jsou nastavené níže uvedenými.

Informace v části A týká komponent výrobců kódu z níže uvedených projektů. Tyto licence a informace jsou k dispozici pouze pro informaci.  Tento kód třetích stran je právě relicensed vám společnost Microsoft ve skupinovém rámečku licenčních podmínek softwaru společnosti Microsoft pro Microsoft produktů nebo služeb.  

Informace v části B se týká součásti výrobců kódu, které budou k dispozici vám společnost Microsoft ve skupinovém rámečku původní licenčních podmínek.

Celý soubor najdete na webu služby [Stažení softwaru společnosti Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všemi právy není výslovně udělena, zda implicitně, odvozeně nebo jinak.

## <a name="next-steps"></a>Další kroky

[Další informace o navrácení](site-recovery-failback-azure-to-vmware-classic.md) si přenést svůj selhalo přes počítače se systémem v Azure zpátky do místního prostředí.
