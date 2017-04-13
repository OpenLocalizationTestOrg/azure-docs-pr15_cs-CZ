<properties
    pageTitle="Azure obnovení webu Podpora matice | Microsoft Azure"
    description="Je uveden souhrn podporované operační systémy a součásti pro obnovení webu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure obnovení webu Podpora matice

Tento článek shrnuje podporované operační systémy a součástí obnovení webu Azure nasazení. Seznam podporovaných součásti a požadavky je k dispozici pro každý scénáře v každém odpovídající článek nasazení a tento dokument je uveden souhrn je.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Podporované operační systémy pro virtualizaci serverů


**Zdroje** | **Cíl** | **Host (hostitel) operační systém**
---|---|---|--- 
**Pro Hyper-V hostiteli (bez VMM)** | Azure | Windows Server 2012 R2 s nejnovějšími aktualizacemi
**Pro Hyper-V hosts (plus pár VMM)** | Azure | Windows Server 2012 R2 s nejnovějšími aktualizacemi
**Pro Hyper-V hosts (plus pár VMM)** | Sekundární VMM webu | Minimálně systému Windows Server 2012 nejnovějších aktualizacích
**VMware tabulkami hosts a vCenter** | Azure | vCenter 5.5 nebo 6.0 (podpora 5,5 funkcí) <br/><br/> vSphere 6.0, 5.5 nebo 5.1 nejnovějších aktualizacích
**VMware tabulkami hosts a vCenter** | Sekundární VMware webu | vCenter 5.5 nebo 6.0 (podpora 5,5 funkcí) <br/><br/> vSphere 6.0, 5.5 nebo 5.1 nejnovějších aktualizacích

## <a name="supported-requirements-for-replicated-machines"></a>Podporované požadavky pro replikovanou počítačích

**Zdroje** | **Co je replikovat** | **Cíl** | **Host (hostitel) operační systém**
---|---|---|--- 
**VMs Hyper-V** | Všechny zátěží na projektu | Azure | Všechny hosta OS, [nepodporuje Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs musí požadavkům [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (plus pár VMM)** | Všechny zátěží na projektu | Azure | Všechny hosta OS, [nepodporuje Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs musí požadavkům [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (plus pár VMM)** | Všechny zátěží na projektu | Sekundární VMM webu | Všechny hosta OS, [nepodporuje Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Všechny pracovní zátěž se systémem Windows OM | Azure | 64bitová verze Windows serveru 2012 R2, Windows Server 2012, Windows Server 2008 R2 s na nejnižších SP1<br/><br/> VMs musí požadavkům [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Všechny pracovní zátěž spuštěna Linux OM | Azure | Červené klobouk Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Úložiště povinné: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs musí požadavkům [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Všechny pracovní zátěž se systémem Windows OM | Sekundární VMware webu | 64bitová verze Windows serveru 2012 R2, Windows Server 2012, Windows Server 2008 R2 s na nejnižších SP1
**VMware VMs** | Všechny pracovní zátěž spuštěna Linux OM | Sekundární VMware webu | Červené klobouk Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Úložiště povinné: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.
**Pole fyzicky serverů** | Všechny zátěží na projektu v systému Windows | Azure | 64bitová verze Windows serveru 2012 R2, Windows Server 2012, Windows Server 2008 s na nejnižších SP1
**Pole fyzicky serverů** | Všechny pracovní zátěž spuštěna Linux | Azure | Červené klobouk Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Úložiště povinné: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.
**Pole fyzicky serverů** | Všechny zátěží na projektu v systému Windows | Sekundární webu | 64bitová verze Windows serveru 2012 R2, Windows Server 2012, Windows Server 2008 s na nejnižších SP1
**Pole fyzicky serverů** | Všechny pracovní zátěž spuštěna Linux | Sekundární webu | Červené klobouk Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Úložiště povinné: systému (EXT3 ETX4, ReiserFS, XFS); souborů Funkce software zařízení mapování (funkce)); Hlasitost správce: (LVM2). Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované. Systém souborů ReiserFS je podporována pouze u SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Zprostředkovatel verze

**Jméno** | **Popis** | **Nejnovější verze** | **Podpora** | **Podrobnosti**
---|---|---|---| ---
**Obnovení poskytovatel Azure webu** | Souřadnice komunikace mezi místním servery a Azure/sekundární webu <br/><br/> Pokud není žádný VMM server je nainstalovaný na místní VMM, nebo Hyper-V servery | 5.1.1700 (dostupné z portálu) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Obnovení Azure webu Unified instalační program (VMware na Azure)** | Souřadnice komunikace mezi místním VMware servery a Azure <br/><br/> Nainstalována na místní VMware serverech | 9.3.4246.1 (dostupné z portálu) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Nastavení mobilních zařízení služby** | Replikace souřadnice mezi místním VMware serverů/fyzické servery a Azure/sekundární webu | NEDEF (dostupné z portálu) | Na každém VMware OM nebo pole fyzicky serveru, který chcete replikovat nainstalovaný. **Agent Microsoft Azure obnovení služby MARS)** | Replikace souřadnice mezi Hyper-V VMs a Azure<br/><br/> Nainstalována na místní Hyper-V serverech (s nebo bez VMM serveru) | 2.0.8689.0 (dostupné z portálu) | Tento agent používanou Azure zálohování a obnovení webu Azure). [Další informace] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Další kroky

[Připravte pro nasazení](site-recovery-best-practices.md)

