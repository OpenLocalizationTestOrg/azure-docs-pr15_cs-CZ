<properties
    pageTitle="Příprava mapování sítě pro ochranu pro Hyper-V virtuálního počítače s VMM v obnovení webu Azure | Microsoft Azure"
    description="Nastavte si síť mapování pro Hyper-V virtuální počítačů z místního datacentra Azure nebo vedlejší webu."
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


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Příprava mapování sítě pro ochranu pro Hyper-V virtuálního počítače s VMM v obnovení webu Azure

Obnovení Azure webu přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery.

Tento článek popisuje mapování sítě, který pomáhá optimální konfigurovat nastavení sítě při práci v obnovení webu replikovat Hyper-V virtuálních počítačích umístěn v VMM mračnech mezi dvěma datacentrech místní nebo mezi místním datacentra a Azure. Poznámka: Pokud jste replikace Hyper-V VMs bez obláčkem VMM nebo replikace VMware VMs nebo pole fyzicky servery, není v tomto článku důležité.

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.


## <a name="overview"></a>Základní informace

Mapování sítě se používá při obnovení webu Azure po nasazení Azure nebo vedlejší datacentra replikovat virtuálních počítačích Hyper-V použití Hyper-V otevřené nebo SAN replikace.

- **Replikace Hyper-V virtuálních počítačích v VMM mračnech mezi dvěma místní datacentrech**– sítě mapy mapování mezi OM sítí na serveru VMM zdroje a sítěmi OM na serveru VMM cílové provádět následující akce:

    - **Připojit virtuálních počítačích po převzetí**– zaručuje, že virtuálních počítačích bude připojen k odpovídající sítě po převzetí. Otevřené virtuální počítač bude připojen k síti cílové, který je namapovat zdrojová síť.
    - **Místo otevřené virtuálních počítačích na serverech hostitele**– optimálně umístit na serverech hostitele Hyper-V otevřené virtuálních počítačích. Otevřené virtuálních počítačích bude umístěn na hostitele, kteří mají přístup k mapovaným OM sítě.
    - **Mapování sítě**– Pokud nemáte konfigurovat mapování sítě, replikovanou virtuálních počítačích nebudete připojení k žádné OM sítě po překlopení.

- **Replikace Hyper-V virtuálních počítačích v místním VMM cloudu Azure**– sítě mapy mapování mezi OM sítí na zdrojovém VMM serveru a cílového webu Azure sítí provádět následující akce:
    - **Připojení virtuálních počítačích po převzetí**, které převzetí ve stejné síti se můžete připojit k sobě, bez ohledu na to, jaký plán obnovení spadají do všech počítačů.
    - **Brána sítě**– Pokud brána sítě nastavenou na cílovém Azure sítě, virtuálních počítačích můžete připojit k jiné místní virtuálních počítačích.
    - **Mapování sítě**– Pokud nemáte konfigurovat mapování sítě, jenom virtuálních počítačích této selhání se stejným plánem obnovení budou moct připojit k sobě navzájem po selhání přes k Azure.


## <a name="network-mapping-example"></a>Příklad mapování sítě

Mezi OM sítí na dvě VMM servery nebo na jednom serveru VMM Pokud dvou webů jsou spravovány na stejný server je možné konfigurovat mapování sítě. Při mapování správné konfiguraci a replikace zapnuté, virtuálního počítače na hlavním pracovišti bude připojený k síti a jeho otevřené v cílovém umístění bude připojený k jeho síťovou.

Pokud sítí nastavili správně v VMM, když vyberete cílové OM síti během mapování sítě, shluky VMM zdroje, které používají zdrojová OM síť bude zobrazovat spolu s sítě OM dostupné cílové týkající se cílové shluky, které se používají pro ochranu.

Tady je příklad ke znázornění tento postup. Podívejme se organizace s dvou místech v New Yorku a Chicago.

**Umístění** | **VMM serveru** | **OM sítí** | **Namapované**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | Namapované VMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Nejsou namapované
Chicago | VMM Chicago| VMNetwork1 Chicago | Namapované VMNetwork1 NewYork
 | | VMNetwork2 Chicago | Nejsou namapované

V tomto příkladu:

- Po vytvoření virtuálního počítače otevřené pro virtuální počítač, který je připojen k VMNetwork1 NewYork bude připojen k VMNetwork1 Chicago.
- Po vytvoření virtuálního počítače otevřené pro VMNetwork2 NewYork nebo VMNetwork2 Chicago nesmí být připojeni k jakákoli síť.

Tady je VMM mračnech jsou nastavení v našem příkladu organizace a logické sítě přidružené k shluky.

### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu

**Chráněný cloudu** | **Ochrana cloudu** | **Logická síť (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NEDEF</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>NEDEF</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logické funkce a OM nastavení sítě

**Umístění** | **Logická síť** | **Přidružená OM sítě**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

### <a name="target-networks"></a>Cíl sítí

Na základě tohoto nastavení, když vyberete cílové OM síti, následující tabulka zobrazuje možnosti, které mají být k dispozici.

**Vyberte** | **Chráněný cloudu** | **Ochrana cloudu** | **Cílové síti k dispozici**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | K dispozici
 | GoldCloud1 | GoldCloud2 | K dispozici
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Není k dispozici
 | GoldCloud1 | GoldCloud2 | K dispozici



## <a name="multiple-subnets"></a>Více podsítí

Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě nachází virtuálního počítače zdroje, bude možné virtuální počítač otevřené připojení k této cílové podsítě po překlopení. Pokud žádná cílové podsítě s odpovídajícím názvem virtuální počítač připojen k první podsítě v síti.


### <a name="failback"></a>Překlopení zpět

Co se stane v případě navrácení (obráceném replikace) zobrazíte Předpokládejme, že je s tímto nastavením VMNetwork1-Chicago namapované VMNetwork1 NewYork.


**Virtuální počítač** | **Připojení k síti OM**
---|---
VM1 | VMNetwork1 sítě
VM2 (otevřené VM1) | VMNetwork1 Chicago

Pomocí těchto nastavení pojďme si rozebrat, co se stane několika možné scénáře.

**Scénář** | **Výsledek**
---|---
Stejně jako v síti vlastností OM 2 po překlopení. | OM 1 zůstane připojení k síti zdroje.
Vlastnosti OM 2 se změní po převzetí a je odpojený. | Je odpojený OM-1.
Vlastnosti OM 2 se změní po převzetí a je připojen k VMNetwork2 Chicago. | Pokud není namapované VMNetwork2 Chicago, bude ukončena OM-1.
Dojde ke změně sítí mapování VMNetwork1 Chicago. | OM 1 bude připojen k síti teď namapované VMNetwork1 Chicago.


## <a name="next-steps"></a>Další kroky

Teď, když máte lepší znalost mapování sítě, [Začínáme s nasazením obnovení webu](site-recovery-best-practices.md).
