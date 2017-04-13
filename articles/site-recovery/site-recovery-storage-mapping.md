<properties
    pageTitle="Namapujte úložiště na obnovení webu Azure pro Hyper-V virtuální počítačů mezi místním datacentrech | Microsoft Azure"
    description="Příprava úložiště mapování pro Hyper-V virtuální počítačů mezi dvěma datacentrech místní s obnovení webu Azure."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Příprava úložiště mapování pro Hyper-V virtuální počítačů mezi dvěma datacentrech místní s obnovení webu Azure


Obnovení Azure webu přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Tento článek popisuje mapování úložiště, což pomáhá uděláte optimální provádějte úložiště používáte obnovení webu replikovat Hyper-V virtuálních počítačích mezi dvěma místního VMM datacentrech.

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="overview"></a>Základní informace

Mapování úložiště je důležité, když jste replikace Hyper-V virtuálních počítačích, které máte uložené v VMM mračnech z primární datacentra k sekundární datacentru pomocí Hyper-V otevřené nebo SAN replikace takto:


- **Místních místní replikace s Hyper-V otevřené)** – Nastavení úložiště mapování tak, že mapování klasifikace úložiště na zdroj a cílového webu VMM servery provádět následující akce:

    - **Rozpoznání cílového úložiště pro otevřené virtuálních počítačích**– virtuálních počítačích bude replikovat do cílového úložiště (SMB sdílet nebo clusteru sdílené svazky (CSVs)), které zvolíte.
    - **Umístění virtuálního počítače otevřené**– úložiště mapování slouží k optimálně umístit na serverech hostitele Hyper-V otevřené virtuálních počítačích. Otevřené virtuálních počítačích bude umístěn na hosts s přístupem ke klasifikaci mapovaných úložiště.
    - **Mapování úložiště**, pokud není konfigurovat mapování úložiště, virtuálních počítačích bude replikovat na výchozí umístění úložiště určené na hostitelském serveru Hyper-V přidružené otevřené virtuálního počítače.

- **Replikace místní s SAN místní**– nastavení úložiště mapování tak, že mapování fondů matice úložiště na zdroj a cílového webu VMM servery.
    - **Určení fondu**– Určuje, které fond sekundárním úložiště obdrží replikace dat z fondu primární.
    - **Určení cílového úložiště fondů**– zaručuje, že logické ve skupině replikace zdroje jsou replikovat do fondu úložiště Namapovaná cílová podle svého výběru.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Nastavení klasifikace úložiště replikace Hyper-V

Pokud používáte Hyper-V otevřené replikovat s obnovení webu, můžete mezi klasifikace úložiště na zdrojovém a cílovém VMM servery nebo na jednom VMM serveru Pokud mapovat dvou webů jsou spravovány na stejný server VMM. Všimněte si, že:

- Když je povolený replikace správně nakonfigurované mapování, virtuální počítač virtuální pevný disk na hlavním pracovišti replikovat na úložiště v mapovaných cílové umístění.
- Úložiště klasifikace musí být k dispozici vyhledali v mračnech zdrojové a cílové skupiny Host (hostitel).
- Klasifikace nemusíte mít stejný druh úložiště. Můžete třeba namapovat klasifikace zdroje, která obsahuje sdílené položky SMB pro cílové klasifikace, která obsahuje CSVs.
- Další informace v [článku jak vytvářet klasifikace úložiště v VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Příklad

Pokud vyberete zdrojové a cílové VMM serveru při ukládání mapování klasifikace správně nakonfigurováno v VMM, zobrazí se klasifikace zdrojové a cílové. Tady je příklad úložiště souborů akcií a klasifikace v organizaci s dvou místech v New Yorku a Chicago.

**Umístění** | **VMM serveru** | **Sdílení souborů (zdrojový)** | **Zařazení (zdrojový)** | **Namapované** | **Sdílení souborů (cílový)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | GOLD | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SLOVO STŘÍBRO | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONZOVÁ | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Nejsou namapované |
| | | SILVER_TARGET | Nejsou namapované |
 | | | BRONZE_TARGET | Nejsou namapované

Můžete chtít nakonfigurovat na kartě **Server úložiště** na stránce **zdroje** portálu obnovení webu.

![Konfigurace mapování úložiště](./media/site-recovery-storage-mapping/storage-mapping1.png)

V tomto příkladu:
- Když otevřené virtuálního počítače se vytvoří všechny virtuální počítač GOLD úložný prostor (SourceShare1), bude se replikovat do úložiště GOLD_TARGET (TargetShare1).
- Po vytvoření virtuálního počítače otevřené pro všechny virtuální počítač SILVER úložný prostor (SourceShare2) bude replikovat na úložiště SILVER_TARGET (TargetShare2) a tak dále.

Skutečné sdílené složky a jejich přiřazené klasifikace VMM se zobrazí na další obrazovce snímek.

![Klasifikace úložiště v VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Úložišť více

Pokud klasifikace cílové přiřazen k více sdílené položky SMB nebo CSVs, optimální úložiště vybere se automaticky při uzamknutí virtuální počítač. Pokud není vhodné cílové úložiště je dostupná s zadaný klasifikace, výchozí úložiště zadanou v hostiteli Hyper-V slouží k umístit na otevřené virtuální pevných discích.

V následující tabulce zobrazit, jak klasifikace úložiště a svazky clusteru sdílené se nastavují v našem příkladu.

**Umístění** | **Klasifikace** | **Přidružená úložiště**
---|---|---
New York | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SLOVO STŘÍBRO | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

Tato tabulka shrnuje chování po povolení ochrany virtuálních počítačích (VM1 - VM5) v tomto příkladu prostředí.

**Virtuální počítač** | **Zdroj úložiště** | **Zdroj klasifikace** | **Mapovaný cílového úložiště**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Obě GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Obě GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SLOVO STŘÍBRO | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SLOVO STŘÍBRO |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Obě SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | NENÍ K DISPOZICI | Mapování, proto je použito výchozí umístění úložiště hostiteli Hyper-V

## <a name="next-steps"></a>Další kroky

Teď, když máte lepší znalost úložiště mapování [Příprava nasazení obnovení webu Azure](site-recovery-best-practices.md).
