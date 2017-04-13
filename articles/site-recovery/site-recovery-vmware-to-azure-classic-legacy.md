<properties
    pageTitle="Replikace VMware virtuálních počítačích a fyzické servery na Azure s obnovení webu Azure (starší verze) | Microsoft Azure" 
    description="Popisuje, jak replikovat místní VMs a Windows/Linux fyzických serverů Azure pomocí obnovení webu Azure v starší verze nasazení klasické portálu." 
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

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Replikace VMware virtuálních počítačích a fyzické servery na Azure s obnovení webu Azure na portálu klasické (starší verze)

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmware-to-azure.md)
- [Klasický portálu](site-recovery-vmware-to-azure-classic.md)
- [Klasický portál (starší verze)](site-recovery-vmware-to-azure-classic-legacy.md)


Vítá vás obnovení Azure webu! Tento článek popisuje starší verze nasazení replikace místní VMware virtuálních počítačích nebo pole fyzicky servery Windows/Linux Azure pomocí obnovení webu Azure klasické portálu.

## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie.

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzické servery a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundárním umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)


>[AZURE.WARNING] Tento článek obsahuje **Pokyny k starší verze**. Nebudete používat novou nasazení. Místo toho [postupujte podle těchto pokynů](site-recovery-vmware-to-azure.md) pro nasazení obnovení webu v Azure portál nebo [použijte tyto pokyny](site-recovery-vmware-to-azure-classic.md) ke konfiguraci rozšířeného nasazení klasické portálu. Pokud jste již nainstalovali způsobem popsaným v tomto článku, doporučujeme migrovat do vylepšené nasazení klasické portálu.


## <a name="migrate-to-the-enhanced-deployment"></a>Migrace do vylepšené nasazení

Tato část platí pouze pokud jste již nainstalovali obnovení webu pomocí pokynů uvedených v tomto článku.

Migrace stávajícího nasazení, který budete muset:

1. Nasaďte novou obnovení webu součástí místních webů.
2. Konfigurace přihlašovacích údajů pro automatické zjišťování VMware VMs na novém konfigurace serveru.
3. Seznamte se s VMware servery s novou konfigurační server.
3. Vytvoření nové skupiny ochranu s novou konfigurační server.


Než začnete:

- Doporučujeme nastavení údržby migraci.
- **Migrace počítačích** možnost je dostupná jenom v případě, že máte existující ochranu skupiny, které jsou vytvořené během starší verze nasazení.
- Po dokončení migračních kroků může trvat 15 minut nebo déle aktualizace její přihlašovací údaje a seznamte se s a aktualizujte virtuálních počítačích, abyste mohli přidat ke skupině zámek. Místo čekání můžete aktualizovat ručně. 

Migrace následujícím způsobem:

1. Přečtěte si o [vylepšené nasazení klasické portálu](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Seznamte se s rozšířené [Architektura](site-recovery-vmware-to-azure-classic.md#scenario-architecture)a [požadavky](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Z počítače, který jste právě replikace odinstalujte službu nastavení mobilních zařízení. Nová verze služby nainstaluje počítačích při přidávání nové ochranu skupiny.
3. Stáhněte si [trezoru registračního klíče](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) a [spusťte Průvodce jednotný vzhled](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) instalace konfigurace serveru, serveru obrázku a předlohy výběr server součástí. Další informace o [plánování kapacity](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Nastavení přihlašovacích údajů](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) , které webu obnovení umožňuje přístup k serveru VMware automaticky zjišťovat VMware VMs. Informace o [potřebná oprávnění](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Přidejte [vCenter servery nebo vSphere tabulkami hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Může trvat 15 minut další servery, které se zobrazí v portálu obnovení webu.
6. Vytvoření [nové skupiny zámek](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Může trvat až 15 minut portálu aktualizujete tak, aby se objeví virtuálních počítačích a zobrazí. Pokud nechcete, aby se má čekat, můžete zvýraznit název serveru správy (neklikejte na to) > **Aktualizovat**.
7. Novou skupinu zámku klikněte u položky **Migrace počítačích**.

    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. V **Počítačích vyberte** vyberte ochranu skupiny, kterou chcete migrovat ze a počítačích, kterou chcete migrovat.

    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. V **Konfigurovat nastavení cílové** určete, zda chcete použít stejné nastavení pro všechny počítače a vyberte server procesu a účet Azure úložiště. Pokud nemáte serveru samostatný proces to bude IP adresu serveru konfigurace.


    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migrace z úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

10. **Určení účtů**vyberte účet, že kterou jste vytvořili pro server proces pro přístup k počítači posunout novou verzi službu mobilita.

    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Obnovení webu přenese replikovanou dat Azure úložiště účtu, který jste zadali. Volitelně můžete znovu použít úložiště účtu, který jste použili v starší verze nasazení.
12. Po dokončení projektu virtuálních počítačích bude automaticky synchronizovat. Až se dokončí synchronizaci můžete odstranit virtuálních počítačích ze starší verze ochranu skupiny.
13. Po provedení migrace všech počítačích můžete odstranit starší ochranu skupiny.
14. Nezapomeňte zadat vlastnosti převzetí počítačích a nastavení Azure sítě po ukončení synchronizace.
15. Pokud máte existující plán obnovení, můžete migrovat do vylepšené nasazení s parametrem **Migrace plánování obnovení** . Bude třeba pouze provést po migraci všech chráněných počítačích. 

    ![Přidání účtu](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Po dokončení migrace budeme pokračovat v [rozšířeném článek](site-recovery-vmware-to-azure-classic.md). Zbývající části tohoto článku starší verze již nebude možné relevantní a nepotřebujete sledovat všechny další podle kroků popsaných v it **.




## <a name="what-do-i-need"></a>Co je potřeba?

Tento diagram znázorňuje součásti nasazení.

![Nové trezoru](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Tady je, co budete potřebovat:

**Součásti** | **Nasazení** | **Podrobnosti**
--- | --- | ---
**Konfigurace serveru** | Azure standardní A3 virtuálního počítače v rámci stejného předplatného jako obnovení webu. | Konfigurace serveru souřadnice komunikaci mezi chráněné počítačích, proces serveru a předlohy cílové servery v Azure. Nastavuje replikace a souřadnice obnovení v Azure selháním.
**Hlavní cílový server** | Azure virtuálního počítače – serveru Windows na základě Galerie aplikace Windows serveru 2012 R2 (Pokud chcete zamknout počítačích Windows) nebo jako Linux server podle OpenLogic CentOS 6.6 Galerie aplikace (Pokud chcete zamknout Linux počítačích).<br/><br/> Existují tři pro změnu velikosti možnosti – standardní A4, standardní D14 a standardní DS4.<br/><br/> Server je připojen k stejné Azure síti jako konfigurační server.<br/><br/> Nastavení portálu obnovení webu | Přijme a zachová replikovanou data z počítače chráněné pomocí připojených VHD vytvořené v úložišti objektů blob Azure úložiště účtu.<br/><br/> Vyberte standardní DS4 speciálně pro konfiguraci ochranu pracovního vytížení vyžadující konzistentní vysoký výkon a nízké latence pomocí účtu úložiště Premium.
**Proces serveru** | Místní virtuální nebo pole fyzicky serveru se systémem Windows serveru 2012 R2<br/><br/> Doporučujeme, abyste je umístěn na stejné síti a LAN segmentu jako stroje, které chcete zamknout, ale je mohlo by umožnit spuštění jiné síti se systémem, dokud chráněné počítačích mají L3 sítě viditelnost k němu.<br/><br/> Nastavte si a zaregistrovat k konfigurační server na portálu obnovení webu. | Chráněný počítačích odeslat data replikace místního serveru obrázku. Má diskové mezipaměti do mezipaměti replikace data, která obdrží. Slouží k provedení určité akce na tato data.<br/><br/> Data optimalizuje tak, že ukládání do mezipaměti, komprese a šifrování před odesláním k předlohy cílový server.<br/><br/> Zpracovává nabízených instalace služby nastavení mobilních zařízení.<br/><br/> Slouží k provedení automatické zjišťování VMware virtuálních počítačích.
**Místního počítače** | Místní VMware virtuálních počítačích nebo pole fyzicky servery se systémem Windows nebo Linux. | Konfigurace nastavení replikace jeden nebo více počítačů. Přes jednotlivé počítače nebo častěji, může selhat více počítačů, které jste shromažďování do plánu obnovení. 
**Nastavení mobilních zařízení služby** | Na každé virtuálního počítače nebo pole fyzicky serveru, který chcete zamknout nainstalovaný<br/><br/> Můžete nainstalovat ručně nebo posune a instaluje automaticky serverem proces při povolíte replikace pro počítač. | Služba mobilita odešle data na server proces během počáteční replikace (synchronizace). Po počítače přísnější (po dokončení synchronizace) službu mobilita zaznamenává zápisy disk v paměti a odesílá na server obrázku. Applicationconsistency pro servery systému Windows je dosáhnout pomocí aplikace VSS.
**Azure trezoru obnovení webu** | Vytvoření trezoru obnovení webu s předplatným Azure a zaregistrujte servery v trezoru. | Trezoru souřadnic a orchestrates replikace dat, převzetí a obnovení mezi místních webů a Azure.
**Replikační mechanismus** | **Přes Internet**– komunikuje a opakování data z chráněné místních serverů Azure pomocí zabezpečené SSL/TLS kanálu přes internet. Toto je výchozí možnost.<br/><br/> **Virtuální privátní sítě nebo ExpressRoute**– komunikuje a duplicitních dat mezi místní servery a Azure přes připojení VPN. Musíte nastavit VPN na webu nebo ExpressRoute připojení mezi místních webů a Azure sítí.<br/><br/> Budete vyberte, jak budete chtít replikovat během obnovení webu nasazení. Po dokončení konfigurace bez vlivu na replikace existující strojů není možné změnit mechanismus. | Ani možnost vyžaduje otevřít jakýkoli příchozí síťové v zamknutém počítačích. Všechna síťová komunikace je spuštěná z místních webů. 

## <a name="capacity-planning"></a>Plánování kapacity

Hlavní oblasti, které budete potřebovat k zamyšlení:

- **Zdrojového prostředí**– VMware infrastruktury nastavení počítače zdroje a požadavky.
- **Servery součástí**– server procesu, konfigurační server a předlohy cílový server 

### <a name="considerations-for-the-source-environment"></a>Co zvážit při zdrojového prostředí

- **Maximální velikost disku**– aktuální maximální velikosti doručovaných disku, který může být připojen virtuálního počítače je 1 TB. Maximální velikosti doručovaných zdrojový disk, replikovat tedy taky omezené na 1 TB.
- **Maximální velikost na zdroj**– maximální velikosti doručovaných jediném počítače je 31 TB (plus pár 31 disků) a pomocí D14 instance zřízení pro předlohy cílový server. 
- **Počet zdrojů na předlohy na cílový**– více počítačů zdroje mohou být chráněny pomocí jednoho předlohy cílového serveru. Počítač jeden zdroj však nelze chráněné mezi servery předlohy cílové vzhledem k tomu, jak disků replikovat, virtuálního pevného disku, který zrcadlí velikost disku je vytvořit v úložišti objektů blob Azure a přiložený jako datový disk předlohy cílový server.  
- **Maximální denně změna sazbu vztaženou na zdroj**, existují tři faktory, které je potřeba třeba zvážit při rozhodování doporučené změnit sazbou zdroje. Cíl na základě aspektů technologie dvou procesorů vyžadovaných na cílovém disku pro každou akci ve zdroji. Je to proto číst stará data a zapsat nová data dojde na cílovém disku. 
    - **Denní změnit rychlost proces server nepodporuje**– zdrojového počítače nemůže zahrnovat více serverů obrázku. Jediný proces server podporuje až 1 TB denní kurz změnit. Proto 1 TB je že maximální denní dat změnit rychlost podporované pro počítač zdroje. 
    - **Maximální výkon nepodporuje cílový disk**– konve maximální počet zdrojový disk nesmí být více než 144 GB/den (s 8 K zápisu velikost). Pro různé psaní velikosti, najdete v tabulce v části hlavní cílové výkonu a procesorů cíle. Toto číslo musí vydělí dvě vzhledem k tomu, že každý zdroj IOP vygeneruje 2 procesorů na cílovém disku. Přečtěte si o [Azure cílů škálovatelnost a výkon](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) při konfiguraci cílové účet úložiště premium.
    - **Maximální výkon úložiště účet podporuje**– zdroj nemůže zahrnovat více účtů úložiště. Vzhledem k účtu úložiště trvá maximálně 20 000 požadavky za sekundu a každý zdroj IOP vygeneruje 2 procesorů v hlavní cílový server, doporučujeme že zachovat počet procesorů přes zdroji do 10 000. Přečtěte si o [Azure cílů škálovatelnost a výkon](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) při konfiguraci zdroje pro účty úložiště premium.

### <a name="considerations-for-component-servers"></a>Důležité informace týkající se servery součástí

Tabulka 1 shrnuje velikosti virtuálního počítače pro konfiguraci a předlohy cílové servery.

**Součásti** | **Nasazeném Azure instance** | **Vzorky** | **Paměti** | **Max disků** | **Velikost disku**
--- | --- | --- | --- | --- | ---
Konfigurace serveru | Standardní A3 | 4 | 7 GB | 8 | 1023 GB
Hlavní cílový server | Standardní A4 | 8 | 14 GB | 16 | 1023 GB
 | Standardní D14 | 16 | 112 GB | 32 | 1023 GB
 | Standardní DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabulka 1**

#### <a name="process-server-considerations"></a>Důležité informace o procesu serveru

Obecně proces serveru pro změnu velikosti závisí na denní kurz změnit přes všechny chráněné úlohami.


- Potřebujete dostatečná výpočetním k provádění úloh, jako je vložený komprese a šifrování.
- Server proces používá mezipaměti založené na disku. Zkontrolujte, že je k dispozici usnadnit změny dat uložených v případě kritický sítě nebo výpadku doporučené mezipaměti místa a disku výkon. 
- Zajištění dostatečnou šířku pásma tak, aby server obrázku můžete odeslat data předlohy cílový server zajistit ochranu souvislá data. 

Tabulka 2 obsahuje přehled procesu pravidla serveru.

**Změnit rychlost dat** | **PROCESOR** | **Paměti** | **Velikost diskové mezipaměti**| **Výkon diskové mezipaměti** | **Šířka pásma průniku/výstupní**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 jádra @ 2,5 GHz) | 4 GB | 600 GB | 7 až 10 MB za sekundu | 30 MB / MB / / 21
300 až 600 GB | 8 vCPUs (2 sockets * 4 jádra @ 2,5 GHz) | 6 GB | 600 GB | 11 až 15 MB za sekundu | 60 MB / MB / / 42
600 GB až 1 TB | 12 vCPUs (2 sockets * 6 jádra @ 2,5 GHz) | 8 GB | 600 GB | 16 až 20 MB za sekundu | 100 MB / / 70 MB /
> 1 TB | Nasazení serveru jiného obrázku | | | | 

**Tabulka 2**

Kde je: 

- Průniku je stažení (intranetové mezi zdroji a proces server).
- Výstupní je nahrát (internet mezi proces serveru a předlohy cílový server). Výstupní čísla předpokládají průměr serveru komprese obrázku 30 %.
- Pro mezipaměť disku samostatný disk OS minimální 128 GB doporučujeme všech serverů obrázku.
- Pro výkon diskové mezipaměti byla použita následující úložiště pro porovnávání: 8 přidružení zabezpečení jednotky 10 ot s konfigurací RAID 10.

#### <a name="configuration-server-considerations"></a>Důležité informace o konfiguraci serveru

Každý konfigurační server podporuje až 100 zdroj počítačích s svazky 3-4 při instalaci. Pokud nasazení je větší doporučujeme že nasadíte jiné konfigurační server. Viz Tabulka 1 pro výchozí vlastnosti virtuálního počítače konfigurace serveru. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Informace o účtu předlohy cílové serveru a úložiště

Prostor úložiště pro každé předlohy na cílový obsahuje OS disku, hlasitost uchovávání informací a disků data. Jednotka uchovávání informací udržuje deníku disku změny po celou dobu okna uchovávání informací podle portálu obnovení webu.  Podívejte se do tabulka 1 pro vlastnosti virtuálního počítače předlohy cílového serveru. Tabulka 3 ukazuje použití disků A4.

**Instanci** | **OS disk** | **Uchovávání informací** | **Disků dat**
--- | --- | --- | ---
 | | **Uchovávání informací** | **Disků dat**
Standardní A4 | 1 disk (1 * 1023 GB) | 1 disk (1 * 1023 GB) | 15 disků (15 * 1023 GB)
Standardní D14 |  1 disk (1 * 1023 GB) | 1 disk (1 * 1023 GB) | 31 disků (15 * 1023 GB)
Standardní DS4 |  1 disk (1 * 1023 GB) | 1 disk (1 * 1023 GB) | 15 disků (15 * 1023 GB)

**Tabulka 3**

Plánování pro předlohy cílový server kapacity závisí na:

- Výkon Azure úložiště a omezení
    - Maximální počet vysoce využívána disků pro standardní OM osy, asi 40 (20 000/500 procesorů na disku) v jedné úložiště účtu. Přečtěte si o [cílů rozšiřitelnost pro standardní úložiště sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) a [sccounts úložiště premium](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Denní změna kurzu 
-   Uchovávání informací hlasitost úložiště.

Všimněte si, že:

- Jeden zdroj nemůže zahrnovat více účtů úložiště. Toto nastavení se projeví na disk dat, přejděte na účty úložiště vybrané při konfiguraci zámek. Operační systém a obvykle přejděte k tomuto účtu automaticky nasazeném úložiště disků uchovávání informací.
- Objem úložiště uchovávání informací povinné závisí na denní změnu sazby a spočítá počet dnů uchovávání informací. Uchovávání informací úložiště na předlohy na cílový = celkem konve ze zdroje denně * počet dnů uchovávání informací. 
- Každé předlohy na cílový obsahuje pouze jeden hlasitost uchovávání informací. V disků připojené k předlohy cílový server se sdílí hlasitost uchovávání informací. Příklad:
    - Je-li zdroj stroje s 5 disků a každý disk vygeneruje 120 procesorů (8K velikost) ve zdroji, to se převádí na 240 procesorů na disku (2 operací na disku cílové na zdroj vstupu a výstupu). 240 procesorů spadá Azure na disku procesorů maximálně 500.
    - Na objem uchovávání informací, toto je 120 * 5 = 600 procesorů a to může být lahev na krku. V tomto scénáři bude vhodné strategie přidat další disků hlasitost uchovávání informací a zahrnují ho přes, jako pruh konfigurace RAID. Protože procesorů jsou rozvržena více jednotek to bude zvýšit výkon. Počet jednotek, které budou přidány do hlasitost uchovávání informací budou následujícím způsobem:
        - Celkový počet procesorů ze zdrojového prostředí / 500
        - Celková konve denně ze zdrojového prostředí (nekomprimované) / 287 GB. 287 GB je maximální výkon nepodporuje cílový disk po dnech. Tento míru budou lišit v závislosti na velikosti zápisu s faktoru 8K vzhledem k tomu, že v tomto případě 8 argument K není thí předpokládá, že velikost zápis. Například je-li zapsat velikost 4 kB výkon bude 287/2. A je-li zapsat velikost 16 kB výkon bude 287 * 2.
- Požadovanou velikost úložiště účtů povinné = zdroje celkem procesorů/10000.


## <a name="before-you-start"></a>Než začnete

**Součásti** | **Požadavky** | **Podrobnosti**
--- | --- | --- 
**Účet Azure** | Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
**Azure úložiště** | Musíte mít účet Azure úložiště, který chcete uložit replikovanou data<br/><br/> Buď účet by měl být [Standardní Geo nadbytečné úložiště účet](../storage/storage-redundancy.md#geo-redundant-storage) nebo [Účet úložiště Premium](../storage/storage-premium-storage.md).<br/><br/> Musíte ve stejné oblasti jako služba obnovení webu Azure a být přidružené stejném předplatném. Nepodporujeme přesunout úložiště účtů vytvořených pomocí [nového Azure portálu](../storage/storage-create-storage-account.md) mezi skupinami zdroje.<br/><br/> Další informace v tématu [Úvod k úložišti tabulek Microsoft Azure](../storage/storage-introduction.md)
**Azure virtuální sítě** | Budete potřebovat Azure virtuální sítě, na kterém má být nasazené konfigurační server a předlohy cílový server. Je třeba ve stejném předplatné a oblasti jako obnovení webu Azure trezoru. Pokud chcete replikovat data přes připojení VPN nebo ExpressRoute Azure virtuální sítě musí připojení k síti místní přes VPN na webu nebo připojení k ExpressRoute.
**Azure zdroje** | Zkontrolujte, že máte dost Azure materiály pro nasazení všechny komponenty. Přečtěte si další v [Azure předplatné omezení](../azure-subscription-service-limits.md).
**Azure virtuálních počítačích** | S [Azure požadavky](site-recovery-best-practices.md)musí vyhovovat virtuálních počítačích, které chcete zamknout.<br/><br/> **Počet disků**– podporuje maximálně 31 disků na jednom chráněné serveru<br/><br/> **Velikostí disků**– jednotlivé kapacita disku nesmí být větší než 1023 GB<br/><br/> **Clusterů**– skupinový servery nejsou podporované<br/><br/> **Spuštění**– Unified Extensible Firmware rozhraní (UEFI) / spouštěcí Extensible Interface(EFI) firmwaru není podporovaná<br/><br/> **Svazky**– nejsou podporované šifrované objemů nástroje Bitlocker<br/><br/> **Názvy serverů**, názvy by měl obsahovat mezi 1 až 63 znaků (písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslem a končit písmeno nebo číslici. Když počítači se po zamknutí Azure název lze upravit.
**Konfigurace serveru** | Standardní A3 virtuálního počítače založené na Azure webu obnovení Windows serveru 2012 R2 Galerie aplikace se vytvoří ve vašem předplatném konfigurační server. Je vytvořen jako první instanci nové cloudové služby. Pokud vyberete veřejné internetové připojení typ pro konfigurační server cloudovou službu vytvořená pomocí rezervovaná veřejnou IP adresu.<br/><br/> Instalační cesta by měl být v anglické znaky.
**Hlavní cílový server** | Azure virtuálního počítače standardní A4, D14 nebo DS4.<br/><br/> Instalační cesta by měl být v anglické znaky. Například cesta by měl být **/usr/local/ASR** serveru předlohy cílové Linux.
**Proces serveru** | Proces serveru na fyzické nebo virtuálního počítače se systémem Windows Server 2012 R2 s nejnovějšími aktualizacemi nástroje můžete nasazovat. Instalace na C: /.<br/><br/> Doporučujeme že umístěte serveru na stejné síti a podsítě jako stroje, které chcete zamknout.<br/><br/> Instalace VMware vSphere rozhraní příkazového řádku 5.5.0 na server procesu. Komponentu VMware vSphere rozhraní příkazového řádku se při vyhledávání virtuálních počítačích spravuje vCenter serveru nebo virtuálních počítačích provozu v hostiteli ESXi vyžaduje na serveru obrázku.<br/><br/> Instalační cesta by měl být v anglické znaky.<br/><br/> Systém souborů odkazy není podporován.
**VMware** | Server vCenter VMware Správa vašeho hypervisory vSphere VMware. Ho by měl být verzí vCenter 5.1 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/> Nejméně jedna hypervisory vSphere obsahující VMware virtuálních počítačích, které chcete zamknout. Hypervisor by měla běžet ESX/ESXi verze 5.1 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/> VMware virtuálních počítačích měli VMware nástroje nainstalovanou a spuštěnou. 
**Počítačích Windows** | Počet požadavky mít chráněné fyzické servery nebo VMware virtuálních počítačích s Windows.<br/><br/> Podporované 64bitová verze operačního systému A: **Windows serveru 2012 R2**, **Windows Server 2012**, nebo **Windows Server 2008 R2 s na nejnižších SP1**.<br/><br/> Název hostitele, připojovací body, názvy zařízení, cestu systému Windows (například: C:\Windows) by měl být jenom v angličtině.<br/><br/> Operační systém, měli byste mít nainstalované C:\ jednotky.<br/><br/> Podporuje pouze základní discích. Dynamické disků nejsou podporované.<br/><br/> Pravidla brány firewall v zamknutém počítačích by mohly připojit k serverům cílové konfigurace a předlohy v Azure.p ><p>Budete muset zadat účtu správce (pouze správci místního počítače Windows) k instalaci nabízených služeb mobilita na serverech Windows. Je-li zadané účet není doménovým účtem musíte zakázat ovládací prvek vzdáleného přístupu uživatele v místním počítači. Stačí přidat položky registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Přidání položky registru z otevřené cmd rozhraní příkazového řádku nebo prostředí powershell a zadejte **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Další informace](https://msdn.microsoft.com/library/aa826699.aspx) o řízení přístupu.<br/><br/> Po selhání podle potřeby můžete připojit k virtuálních počítačích Windows v Azure pomocí vzdálené plochy zkontrolujte, že je povolená Vzdálená plocha na místním počítači. Pokud se nepřipojujete přes VPN, měli pravidla brány firewall povolit připojení ke vzdálené ploše přes internet.
**Linux počítačích** | Podporované 64bitový operační systém: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4 6.5 s červeným klobouk kompatibilní jádra nebo nerozbitného Enterprise jádra verze 3 (UEK3)**, **Linux Enterprise SUSE Server s aktualizací SP3 11**.<br/><br/> Pravidla brány firewall v zamknutém počítačích by měl aby mu umožnil přístup konfigurace a předlohy cílové servery v Azure.<br/><br/> / etc/hosts soubory chráněné počítačích by měl obsahovat položky, které namapujte název místní hostitele IP adresy přidružené k všechny nic <br/><br/> Pokud chcete připojit k Azure virtuální počítači spuštěná Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), ověřit, že služba zabezpečené Shell na zamknutém počítač je nastavena na spustit automaticky při spuštění systému a že povolit pravidla brány firewall ssh připojení k ní.<br/><br/> Název hostitele, připojovací body, názvy zařízení a Linux systémových cest a názvy souborů (například/atd /; USR) třeba v angličtině pouze.<br/><br/> Ochrana se dá nastavit podpora pro místního počítače s následující úložiště:-<br>Systém souborů: EXT3 ETX4, ReiserFS, XFS<br>Funkce software zařízení mapování (funkce)<br>Hlasitost správce: LVM2<br>Pole fyzicky servery s úložištěm řadiče HP CCISS nejsou podporované.
**Třetích stran** | Některé součásti nasazení v tomto scénáři, závisí na jiných výrobců software budou fungovat správně. Úplný seznam najdete v článku [o třetích stranách software a informací](#third-party)


### <a name="network-connectivity"></a>Připojení k síti

Máte dvě možnosti při konfiguraci síťové připojení mezi místních webů a Azure virtuální sítě, na které jsou součástí infrastruktury (konfigurační server, předlohy cílových serverů) nasazeny. Musíte se rozhodnout, kterou možnost připojení k síti pomocí před nasazením konfigurační server. Budete muset zvolte toto nastavení používají při nasazení. Nemůžete změnit později.

**Internetu:** Komunikace a replikace dat mezi místní servery (server procesu, chráněné počítače) a servery součástí Azure infrastruktury (konfigurační server, předlohy na cílový) se stane, prostřednictvím zabezpečené připojení SSL/TLS z místního veřejné koncové body na serverech cílové konfigurace a předlohy. (Jedinou výjimkou je připojení mezi server procesu a předlohy cílový server na portu TCP 9080, což nešifrovaném. Pouze informace o kontrole související s protokolem replikace pro nastavení replikace dojde k výměně u tohoto připojení.)

![Diagram nasazení internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**Virtuální privátní sítě**: komunikaci a replikace dat mezi místní servery (server procesu, chráněné počítače) a servery součástí Azure infrastruktury (konfigurační server, předlohy na cílový) se stane přes připojení VPN mezi místní síti a Azure virtuální sítě, na kterém jsou nasazeny konfigurační server a předlohy cílové servery. Zajistit, že v místní síti je připojen k Azure virtuální sítě ExpressRoute připojení nebo připojení VPN k webu.

![Diagram nasazení VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru

1. Přihlaste se k [portálu Správa](https://portal.azure.com).


2. Rozbalte položku **Datové služby** > **Obnovení služby** a klikněte na **Webu obnovení trezoru**.


3. Klikněte na **vytvořit nový** > **vytvořit**.

4. Do pole **název**zadejte popisný název k identifikaci trezoru.

5. V **oblasti**vyberte zeměpisná oblast pro trezoru. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Klikněte na **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Zkontrolujte na stavovém řádku potvrďte, že byla úspěšně vytvořena trezoru. Trezoru se zobrazí jako **aktivní** na hlavní stránce **Obnovení služby** .

## <a name="step-2-deploy-a-configuration-server"></a>Krok 2: Nasazení konfigurace serveru

### <a name="configure-server-settings"></a>Konfigurace nastavení serveru

1. Na stránce **Služby Recovery** na trezoru a otevře se stránka rychlý Start. Představení aplikace můžete otevřít také kdykoli pomocí ikony.

    ![Ikona rychlým startem](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. V rozevíracím seznamu vyberte **mezi místních webů s VMware/fyzické servery a Azure**.
3. V **Tématu Příprava Target(Azure) zdroje** klikněte na **Nasazení serveru**.

    ![Nasazení serveru](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. V **Nové podrobnosti o konfigurace serveru** zadejte:

    - Název serveru a přihlašovací údaje pro připojení k němu.
    - V seznamu typ připojení k síti rozevírací seznam vybrat **Veřejný Internetu** nebo **VPN**. Všimněte si, že nelze změnit toto nastavení po se použije.
    - Vyberte Azure síť, ve kterém by měl být serveru nachází. Pokud používáte nastavení sítě VPN zkontrolujte Azure sítě připojení k místní síti podle očekávání. 
    - Určení vnitřní adresy IP a podsítě přiřazené k serveru. Všimněte si, že první čtyři IP adresy v libovolné podsítě rezervováno pro interní Azure spotřebu. Použití jiných IP adresy dostupné.
    
    ![Nasazení serveru](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Po klepnutí na tlačítko **OK** standardní virtuálního počítače A3 založené na Azure webu obnovení Windows serveru 2012 R2 Galerie aplikace se vytvoří ve vašem předplatném konfigurační server. Je vytvořen jako první instanci nové cloudové služby. Pokud jste vybrali připojení přes internet cloudovou službu se vytvoří pomocí rezervovaná veřejnou IP adresu. Můžete sledovat průběh na kartě **projekty** .

    ![Sledování průběhu](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Pokud se připojujete prostřednictvím Internetu, po konfigurační server je nasazeném upozornění na veřejnou IP adresu přiřazeného k němu na stránce **virtuálních počítačích** v portálu Azure. Klikněte na **koncové body** poznamenejte si veřejné port HTTPS namapované soukromé port 443. Tento údaj později budete potřebovat při registraci předlohy cíl a proces servery konfigurační server. Konfigurace serveru nasazení se tyto koncové body:

    - HTTPS: Veřejné port slouží ke koordinaci komunikaci mezi servery součástí a Azure přes internet. Privátní port 443 slouží ke koordinaci komunikaci mezi servery součástí a Azure přes VPN.
    - Vlastní: Veřejné port slouží k navrácení nástroj komunikace přes internet. Soukromé port 9443 se používá pro komunikaci nástroj navrácení přes VPN.
    - Powershellu: Privátní port 5986
    - Vzdálená plocha: soukromé port 3389
    
    ![Koncové body OM](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Není odstranit nebo změnit číslo portu veřejné nebo soukromé všechny koncové body vytvořen během konfigurace serveru nasazení.

Konfigurační server je používaný v automaticky vytvořené Azure cloudové službě s rezervovaná IP adresu. Rezervované adresy je potřeba ověřit, že IP adresa konfigurace serveru cloudové služby zůstane po restartování počítače virtuálních počítačích (včetně konfigurace serveru) ve službě cloud. Rezervovaná veřejnou IP adresu bude potřebovat ručně nerezervované při vyřadit z provozu konfigurační server nebo budete zůstat rezervovaná. Je výchozí maximálně 20 vyhrazené veřejné adresy IP jedno předplatné. [Další informace](../virtual-network/virtual-networks-reserved-private-ip.md) o rezervovaná IP adres. 

### <a name="register-the-configuration-server-in-the-vault"></a>Registrace konfigurace serveru v trezoru

1. Na **Úvodní** stránce klikněte na **Zdroje Příprava cílové** > **Stáhnout registračního klíče**. Soubor s klíčem je vytvořen automaticky. Platí pro 5 dní po vygenerování. Zkopírujte do konfigurační server.
2. Ve **virtuálních počítačích** vyberte ze seznamu virtuálních počítačích konfigurační server. Otevřete kartu **řídicích panelů** a klikněte na **Připojit**. **Otevřít** stažený soubor RDP přihlásit konfigurační server pomocí vzdálené plochy. Pokud používáte VPN, použijte vnitřní IP adresu (adresu, kterou jste zadali při nasazení konfigurační server) pro připojení ke vzdálené ploše z místního serveru. Při prvním přihlášení Průvodce instalací konfigurační Server Azure webu obnovení spustí automaticky.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. Instalace **softwaru třetích stran** klikněte na **mám přijmout** si stáhněte a nainstalujte MySQL.

    ![Instalace MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. **Podrobnosti o Server MySQL** Vytvořte přihlašovací údaje o přihlášení instanci server MySQL.

    ![Přihlašovací údaje MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. V **Nastavení sítě Internet** zadejte, jak bude konfigurační server připojení k Internetu. Všimněte si, že:

    - Pokud chcete použít vlastní proxy by měl nastavením před instalací zprostředkovatele.
    - Když kliknete na **Další** test se spustí při kontrole připojení proxy k.
    - Pokud používáte vlastní proxy nebo výchozí proxy server vyžaduje ověření budete muset zadávat podrobnosti proxy, včetně adresy, portů a přihlašovací údaje.
    - Následující adresy URL by měl být přístupných proxy server:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Pokud máte IP založené na adrese pravidla brány firewall Zkontrolujte, že pravidla umožňují sdělení konfigurační server IP adresy podle [Rozsahy IP Azure datacentru](https://msdn.microsoft.com/library/azure/dn175718.aspx) a HTTPS (443) Protocol (protokol) nastavení. Je třeba bílé seznam rozsahů IP Azure oblasti, kterou chcete použít a aby západní USA.

    ![Registrace proxy serveru](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. V **Nastavení poskytovatele chybové zprávy lokalizace** zadejte jazyk, který chcete zobrazit chybové zprávy.

    ![Chybová zpráva registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. V **Azure webu obnovení registrace** Procházet a vyberte klíčové soubor, který jste zkopírovali na server.

    ![Registrace klíčové souborů](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Na stránce dokončení průvodce vyberte tyto možnosti:

    - Vyberte **Spustit dialogovém okně Správa účtu** můžete určit, že by měla po dokončení Průvodce otevře dialogové okno Spravovat účty.
    - Výběrem možnosti **vytvořit ikony na ploše pro Cspsconfigtool** přidat zástupce na ploše na konfigurační server, takže můžete kdykoli otevřít dialogové okno **Spravovat účty** aniž by musel znovu spusťte průvodce.

    ![Dokončení registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Klikněte na **Dokončit** dokončete průvodce. Vygeneruje se přístupové heslo. Zkopírujte do na zabezpečeném místě. Musíte je ověřování a zaregistrovat obrázku a předlohy cílové servery systému konfigurační server. Slouží také k zajištění integrity kanálu při komunikaci server configuration. Obnovit heslo, ale pak budete muset znovu zaregistrujte předlohy cílové a zpracovat serverů pomocí nové heslo.

    ![Heslo](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Po registraci konfigurační server zobrazí na stránce **Konfigurace servery** v trezoru.

### <a name="set-up-and-manage-accounts"></a>Nastavení a Správa účtů

Při nasazení obnovení webu vyžaduje přihlašovací údaje pro následující akce:

- Účet VMware thatSite obnovení moct automatické zjišťování VMs vCenter servery nebo vSphere tabulkami hosts. 
- Když přidáte počítačů pro ochranu, tak, aby obnovení webu můžete nainstalovat službu mobilita na nich.

Po zaregistrujete konfigurační server můžete otevřít dialogové okno **Spravovat účty** k přidání a Správa účtů, které má být použita pro tyto akce. Existuje několik způsobů, jak to udělat:

- Otevřete na zástupce, kterého jste připravovaném vytvořili k zobrazení dialogového okna na poslední stránce nastavení pro konfigurační server (cspsconfigtool).
- Otevření dialogového okna na dokončení konfigurace serveru instalace.

1. **Správa** účtů klikněte na **Přidat účet**. Můžete také upravit a odstranit existující účty.

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. V **Podrobnosti o účtu** zadejte název účtu použít v Azure a přihlašovací údaje (domény a uživatelské jméno). 

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Připojení k serveru konfigurace 

Připojení k serveru konfigurace dvěma způsoby:

- Připojení prostřednictvím sítě VPN webu webu nebo ExpressRoute připojení
- Přes internet 

Všimněte si, že:

- Připojení k Internetu koncové body virtuální počítač používá ve spojení s veřejné virtuální IP adresu serveru.
- Připojení k síti VPN používá interní IP adresu serveru společně s soukromé porty koncového bodu.
- Je jednorázové rozhodnutí se rozhodnout, jestli chcete připojení (ovládací prvek a replikace dat) z místních serverů různých servery součástí (konfigurační server, předlohy na cílový) spuštěné v Azure přes připojení VPN nebo internet. Není možné změnit toto nastavení později. Pokud neuděláte musíte přeinstalujte scénáře a reprotect počítače.  


## <a name="step-3-deploy-the-master-target-server"></a>Krok 3: Nasazení serveru předlohy cíl

1. Klikněte na **Příprava zdroje Target(Azure)** > **nasazení předlohy cílový server**.
2. Zadejte podrobnosti předlohy cílové serveru a přihlašovací údaje. Ve stejné síti Azure jako konfigurační server má být nasazené na server. Po kliknutí dokončete Azure virtuálního počítače se vytvoří pomocí Galerie aplikace Windows nebo Linux.

    ![Nastavení serveru cíle](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Všimněte si, že první čtyři IP adresy v libovolné podsítě rezervováno pro interní Azure spotřebu. Určení jiné IP adresy dostupné.

>[AZURE.NOTE] Při konfiguraci ochranu úloh, které vyžaduje hostovat vstupu a výstupu náročné úloh pomocí [Účtu úložiště Premium](../storage/storage-premium-storage.md)konzistentní vysoký výkon vstupu a výstupu a nízké latence, vyberte standardní DS4.


3. Windows předlohy na cílový vytvořené pomocí těchto koncové body OM. Nezapomeňte vytvořené veřejné koncové body jenom v případě připojení přes internet.

    - Vlastní: Veřejné port používají server procesu odeslání replikace dat přes internet. Soukromé port 9443 slouží serverem proces poslat data replikace předlohy cílový server přes VPN.
    - Vlastní 1: Veřejné port, který serverem proces poslat metadat přes internet. Soukromé port 9080 slouží serverem proces poslat metadat předlohy cílový server přes VPN.
    - Powershellu: Privátní port 5986
    - Vzdálená plocha: soukromé port 3389

4. Linux předlohy na cílový OM se vytvoří pomocí těchto koncové body. Poznámka: vytvořené veřejné koncové body jenom v případě, že jste připojeni přes internet.

    - Vlastní: Veřejné port používají server procesu odeslání replikace dat přes internet. Soukromé port 9443 slouží serverem proces poslat data replikace předlohy cílový server přes VPN.
    - Vlastní 1: Veřejné port používá server procesu odeslat metadata přes internet. Privátní port 9080 server proces používá poslat metadat předlohy cílový server přes VPN
    - SSH: Privátní port 22

    >[AZURE.WARNING] Není odstranit nebo změnit číslo portu veřejné nebo soukromé některého z koncové body vytvořen během nasazení serveru předlohy cílové.

5. Ve **virtuálních počítačích** počkat, až virtuálního počítače začít.

    - Pokud je poznámku systému Windows server dolů vzdálené plochy podrobnosti.
    - Pokud je Linux server a jste připojení prostřednictvím sítě VPN mějte na paměti interní IP adresu virtuálního počítače. Pokud se připojujete prostřednictvím Internetu Poznámka veřejnou IP adresu.

6.  Přihlaste se k serveru k dokončení instalace a zaregistrujte s konfigurační server. 
7.  Pokud používáte Windows:

    1. Zahájení konverzace připojení ke vzdálené ploše virtuálního počítače. Při prvním přihlášení skript se spustí v okně Powershellu. Nechejte jej. Po dokončení nástroje Konfigurace hostitele Agent se automaticky otevře registrace serveru.
    2. Konfigurace **hostitele Agent** zadejte interní IP adresu serveru a port 443. Můžete interní adresu a soukromé port 443 i když nejste připojení přes VPN protože virtuální počítač připojen ke stejné síti Azure jako konfigurační server. Nechte **Použití HTTPS** povolené. Zadejte heslo pro konfigurace serveru, na kterém bylo uvedeno dříve. Kliknutím na **OK** zaregistrujte serveru. Možnosti překladu síťových adres můžete ignorovat. Nejsou použity.
    3. Pokud požadovanou jednotku odhadovaná uchovávání informací je víc než 1 TB můžete nakonfigurovat pomocí virtuální disk a [prostor úložiště](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) hlasitost uchovávání informací (R:)
    
    ![Hlavní cílové systému Windows server](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Pokud používáte Linux:
    1. Ujistěte se, že máte nainstalovanou nejnovější Linux Integration Services (seznam) před instalací předlohy cílovém serveru nainstalovaná. Najděte nejnovější verzi seznam spolu s pokyny k instalaci [tady](https://www.microsoft.com/download/details.aspx?id=46842). Po instalaci seznam restartujte počítač.
    2. **Příprava cílového (Azure)** zdrojů klikněte na **Stáhnout a nainstalovat další software (jenom pro Linux předlohy cílový Server)**. Zkopírujte soubor stažený vkládání do virtuálního počítače pomocí sftp klienta. Můžete taky můžete přihlásit k serveru předlohy cílové nasazeném linux a použít *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* ke stažení na soubor.
    2. Přihlaste se k serveru pomocí klienta zabezpečené prostředí. Pokud jste připojení k síti Azure přes VPN použijte vnitřní IP adresu. Jinak použijte externí IP adresy a veřejné koncový bod SSH.
    3. Extrahujte soubory z instalačního programu algoritmem gzip spuštěním: **cílový – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6 – 64***
    ![Linux předlohy cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Ujistěte se, že jste v adresáři, do kterého jste extrahovali obsah souboru vkládání.
    5. Kopírování konfigurace serveru heslo místního souboru pomocí příkazu * *ozvěnu* `<passphrase>` * > passphrase.txt**
    6. Spuštění příkazu "**sudo. / instalace -t obou - a hostovat -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 – ne y - c https -P passphrase.txt**".

    ![Registrace cílové serveru](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Počkejte několik minut (10 – 15) a na stránce Kontrola, zda předlohy cílový server uvedeny při registraci v **servery** > kartu **Server podrobnosti o** **Konfiguraci servery** . Pokud používáte Linux a nezaregistroval hostiteli nástrojů konfigurace znova spusťte z /usr/local/ASR/Vx/bin/hostconfigcli. Musíte nastavit přístupová oprávnění spuštěním chmod jako kořenovou.

    ![Ověření cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Může trvat až 15 minut po dokončení pro předlohy cílový server uvedena v portálu registrace. Hned, klepnutím na tlačítko **Aktualizovat** na stránce **Konfigurace servery** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Krok 4: Nasazení místního serveru obrázku

Než začnete doporučujeme nakonfigurovat statickou IP adresu serveru obrázku tak, aby ho má zaručena trvalý po restartování počítače.

1. Klikněte na tlačítko rychlý Start > **Instalace serveru proces místní** > **stažení a instalace serveru obrázku**.

    ![Instalace server procesu](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Zkopírujte soubor stažený zip na serveru, na kterém budete chtít nainstalovat server obrázku. Soubor zip obsahuje dva instalačních souborů:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Rozbalení archivu a zkopírujte instalačních souborů na místo na serveru.
4. Spustit **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** instalační soubor a postupujte podle pokynů. Nainstalujete potřebné pro nasazení součásti třetích stran.
5. Spusťte **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Na stránce **Režimu serveru** vyberte **Server procesu**.
7. Na stránce **Podrobnosti prostředí** postupujte takto:


    - Pokud chcete zamknout VMware virtuálních počítačích, klepněte na **Ano**
    - Pokud chcete zamknout fyzické servery a tedy, nemusíte VMware vCLI nainstalovat na server proces. Klikněte na **Ne** a pokračovat.

8. Při instalaci VMware vCLI, mějte na paměti následující:

    - **Je podporována pouze VMware vSphere rozhraní příkazového řádku 5.5.0**. Server procesu nefunguje s jinými verzemi nebo aktualizace vSphere rozhraní příkazového řádku.
    - Stáhněte si vSphere rozhraní příkazového řádku 5.5.0 z [zde.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Pokud jste si nainstalovali vSphere rozhraní příkazového řádku těsně před zahájením instalace proces serveru a nastavení nenajde, počkejte, až pět minut před pokusem o nastavení. Zajistíte tím, že všechny proměnné potřebné pro zjišťování rozhraní příkazového řádku vSphere inicializovány správně.

9.  V **NIC výběrem Server procesu** vyberte síťový adaptér server obrázku použít.

    ![Vyberte adaptér](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Konfigurace serveru podrobnosti**:

    - Pro IP adresa a portu Pokud se připojujete přes VPN zadejte interní IP adresu konfigurační server a port 443. Zadejte jinak veřejné virtuální IP adresa a mapovaných veřejné koncový bod HTTP.
    - Zadejte heslo konfigurační server.
    - Pokud chcete vypnout ověřování, když použijete automatické nabízených instalace služby zrušte **ověření mobilita služby software podpis** . Ověření podpisu vyžaduje připojení k Internetu ze serveru obrázku.
    - Klikněte na tlačítko **Další**.

    ![Registrace konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. **Vyberte instalace** jednotky vyberte jednotku mezipaměti. Proces serveru musí jednotku mezipaměti s nejméně 600 GB volného místa. Klikněte na **nainstalovat**. 

    ![Registrace konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Všimněte si, že bude potřeba restartovat službu dokončete instalaci. **Konfigurace**serveru > **Server podrobnosti o** zkontrolujte, jestli serveru obrázku se objeví a je úspěšně zaregistrovaná v trezoru.

>[AZURE.NOTE]Může trvat až 15 minut po dokončení procesu server zobrazují, zatímco zařazený do kategorie konfigurační server registrace. Okamžitou aktualizaci aktualizace konfigurační server kliknutím na tlačítko Aktualizovat v dolní části na stránce konfigurace serveru
 
![Ověřování serveru obrázku](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Pokud jste se vlastně zakážete ověření podpisu pro službu mobilita při registraci proces server, můžete to provést později takto:

1. Přihlaste se k serveru obrázku jako správce a otevřete soubor C:\pushinstallsvc\pushinstaller.conf pro úpravy. V části **[PushInstaller.transport]** přidejte následující řádek: **SignatureVerificationChecks = "0"**. Uložte a zavřete soubor.
2. Restartujte InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Krok 5: Obnovení webu aktualizace součásti

Součásti webu obnovení se automaticky aktualizují od času. Existenci nových aktualizacích nainstalujte je v následujícím pořadí:

1. Konfigurace serveru
2. Proces serveru
3. Hlavní cílový server
4. Nástroj navrácení (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Získání a instalace aktualizací


1. Aktualizace pro konfiguraci, proces a servery předlohy cílové můžete získat z obnovení webu **řídicího panelu**. Pro instalaci Linux extrahujte soubory z instalačního programu algoritmem gzip a spuštění příkazu "sudo. / instalace" k instalaci aktualizace.
2. [Stáhněte si](http://go.microsoft.com/fwlink/?LinkID=533813) nejnovější aktualizaci pro tool(vContinuum) navrácení.
3. Pokud používáte virtuálních počítačích nebo pole fyzicky servery, které už máte službu nastavení mobilních zařízení nainstalovaný, můžete získat aktualizace pro službu následujícím způsobem:

    - **Možnost 1**: Stáhněte si aktualizace:
        - [Windows Server (64-bit pouze)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (64-bit pouze)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (64-bit pouze)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server s aktualizací SP3 (64-bit pouze)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Po aktualizaci server procesu aktualizovanou verzi službu mobilita bude možné ve složce C:\pushinstallsvc\repository na serveru obrázku.
    - **Možnost 2**: Pokud máte stroje s starší verzí systému službu nastavení mobilních zařízení nainstalovaný, můžete automaticky aktualizovat služby nastavení mobilních zařízení v počítači z portálu pro správu.

        1. Zajistit, aby server procesu aktualizoval.
        2. Ujistěte se, že chráněné počítač splňuje [požadavky](#install-the-mobility-service-automatically) na automaticky předání službu mobilita tak, aby aktualizace funguje očekávaným způsobem.
        2. Vyberte skupinu pro ochranu, zvýrazněte chráněné počítače a klikněte na možnost **aktualizace mobilita přihlašovacích**. Kliknutím na toto tlačítko neexistuje pouze při na novější verzi službu nastavení mobilních zařízení. 

            ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Vyberte účty zadejte účet správce, který chcete použít k aktualizaci službu mobilita na zamknutém serveru. Klikněte na tlačítko OK a počkejte na dokončení aktivované úlohy.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Krok 6: Přidání vCenter servery nebo vSphere tabulkami hosts

1. Klikněte na položku **servery** > **Konfigurace serverů** > konfigurační server >**Přidat vCenter serveru** přidání vCenter serveru nebo vSphere hostitele.

    ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Zadejte podrobnosti pro server nebo Host (hostitel) a vyberte server proces, který se použijí k nalezení.

    - Pokud není na výchozí port 443 vCenter serveru zadejte číslo portu, na kterém běží vCenter server.
    - Proces serveru musí být ve stejné síti jako hostitel serveru/vSphere vCenter a měli VMware vSphere rozhraní příkazového řádku 5.5.0 nainstalovaný.

    ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Po dokončení zjišťování vCenter serveru uvedené v části Podrobnosti o konfiguraci serveru.

    ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Pokud používáte účet není správcem přidáte serveru nebo Host (hostitel), zkontrolujte, jestli že má účet následující oprávnění:

    - účty vCenter by měl mít datacentru, úložiště, složky, Host (hostitel), sítě, zdroje, úložiště zobrazení, virtuálního počítače a oprávnění Distributed přepnout vSphere povolené.
    - vSphere hostitele účty by měl mít datacentru, úložiště, složky, Host (hostitel), sítě, zdrojů, virtuálního počítače a oprávnění Distributed přepnout vSphere povoleno



## <a name="step-7-create-a-protection-group"></a>Krok 7: Vytvoření ochrany skupiny

1. Otevření **Položek chráněné** > **Ochranu skupiny** > **vytvořit ochranu skupiny**.

    ![Vytvoření ochrany skupiny](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Na stránce **Zadejte nastavení ochrany skupiny** zadejte název skupiny a vyberte konfigurační server, na kterém chcete vytvořit skupinu.

    ![Nastavení ochrany skupiny](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Na stránce **Zadejte nastavení replikace** konfigurace replikace nastavení, která se použije pro všechny počítače ve skupině.

    ![Replikace ochrany skupiny](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Nastavení:
    - **Soulad s víc OM**: Pokud to zapnete vytváří sdílené aplikace konzistentní obnovení body v počítačích ve skupině zámek. Toto nastavení je nejrelevantnější při všech počítačů ve skupině zámek spuštění stejného pracovního vytížení. Všechny počítače bude obnoven stejný datový bod. Dostupné pouze pro servery systému Windows.
    - **Mezní hodnota operace RPO**: upozornění vygeneruje se při replikace ochranu souvislá data operace RPO překračuje nakonfigurované operace RPO mezní hodnota.
    - **Obnovení přejděte uchovávání informací**: Určuje okno uchovávání informací. Chráněný počítačích můžete obnovit až kamkoli v tomto okně.
    - **Aplikace konzistentní počet_plateb snímek**: Určuje, jak často se body obnovení obsahující konzistenci aplikací snímky vytvoří.

Ochranu skupiny můžete sledovat, jak jste vytvořili na stránce **Chráněné položky** .

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Krok 8: Nastavení stroje, které chcete zamknout

Budete potřebovat k instalaci služby mobilita na virtuálních počítačích a fyzické servery, které chcete zamknout. Můžete to udělat dvěma způsoby:

- Automaticky nabízená a nainstalujte službu na každém počítači ze serveru obrázku.
- Ruční instalace služby. 

### <a name="install-the-mobility-service-automatically"></a>Automaticky nainstalovat službu mobilita

Při přidání stroje ochranu skupiny služby mobilita automaticky posune a nainstalovaný na každém počítači serverem obrázku. 

**Automaticky nabízených nainstalujte službu mobilita na serverech Windows:** 

1. Nainstalujte nejnovější aktualizace pro server proces podle popisu v [Krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že server obrázku je k dispozici. 
2. Ujistěte se, je připojení k síti mezi zdrojového počítače a proces serveru a ze serveru proces přístupný zdrojového počítače.  
3. Konfigurace brány Windows firewall umožňuje **sdílení souborů a tiskáren** a **Přístrojového vybavení správy systému Windows**. V části nastavení brány Windows Firewall vyberte možnost "Povolit aplikace nebo funkci průchod bránou Firewall" a vyberte aplikace, jak je znázorněno na následujícím obrázku. U počítačů, které patří do domény můžete nakonfigurovat zásadu brány firewall s objekt zásad skupiny.

    ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Účet použili k instalaci nabízených musí být v skupiny Administrators ve počítače, který chcete zamknout. Těchto přihlašovacích údajů se používají pouze pro nabízených instalace služby mobilita a budete poskytnete je přidaný do počítače ke skupině zámek.
5. Pokud zadané účet není doménovým účtem musíte zakázat ovládací prvek vzdáleného přístupu uživatele v místním počítači. Stačí přidat položky registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Přidání položky registru z otevřené cmd rozhraní příkazového řádku nebo prostředí powershell a zadejte **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Automaticky nabízených nainstalujte službu mobilita na serverech Linux:**

1. Nainstalujte nejnovější aktualizace pro server proces podle popisu v [Krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že server obrázku je k dispozici.
2. Ujistěte se, je připojení k síti mezi zdrojového počítače a proces serveru a ze serveru proces přístupný zdrojového počítače.  
3. Ujistěte se, že účet uživatele root na zdrojový server Linux.
4. Ujistěte se, že soubor/etc/hosts na zdrojový server Linux obsahuje položky, které namapujte název místní hostitele IP adresy přidružené k všechny nic.
5. Nainstalujte nejnovější funkci openssh, funkci openssh server, openssl balíčků v počítači, který chcete zamknout.
6. Zkontrolujte, zda SSH povolené a spuštěnou na port 22. 
7. Ověření SFTP podsystém a heslo v souboru sshd_config následujícím způsobem: 

    - a) přihlásit jako kořenovou.
    - b) do souboru /etc/ssh/sshd_config soubor vyhledejte řádek, který začíná **PasswordAuthentication**.
    - c) zrušte komentář řádku a změňte hodnotu z "Ne", "Ano".

        ![Nastavení mobilních zařízení Linux](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) najděte řádek, který začíná podsystém a zrušte komentář řádku.
    
        ![Linux nabízených mobilita](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Zajistěte, aby že podporované varianty zdroj počítače Linux. 
 
### <a name="install-the-mobility-service-manually"></a>Ruční instalace služby mobilita

Softwarových balíčků použili k instalaci služby nastavení mobilních zařízení jsou na serveru proces C:\pushinstallsvc\repository. Přihlášení k serveru obrázku a zkopírujte odpovídající instalační balíček do počítače zdroj podle následující tabulky:-

| Zdrojový operační systém                               | Nastavení mobilních zařízení balíček služby na serveru proces                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (64-bit pouze)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (64-bit pouze)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 s aktualizací SP3 (64-bit pouze)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (64-bit pouze)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Instalace služby mobilita ručně v systému Windows server**, postupujte takto:

1. Zkopírujte balíček **Aplikace Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** z adresář proces serveru uvedené v předchozí tabulce k počítači zdroje.
2. Nainstalujte službu mobilita spuštěním spustitelný soubor v počítači zdroje.
3. Postupujte podle pokynů instalační program.
4. Vyberte **službu mobilita** roli a klikněte na tlačítko **Další**.
    
    ![Instalace služby mobilita](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Nechte adresáři instalace jako výchozí instalační cesta a klikněte na tlačítko **nainstalovat**.
6. V **Konfiguraci Agent Host (hostitel)** zadejte IP adresu a HTTPS port serveru konfigurace.

    - Pokud se připojujete prostřednictvím Internetu zadejte veřejné virtuální IP adresa a veřejné koncový bod HTTPS jako číslo portu.
    - Pokud se připojujete přes VPN zadejte interní IP adresu a port 443. Opustit **Použití HTTPS** zaškrtnuté.

    ![Instalace služby mobilita](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Zadejte heslo konfigurace serveru a klikněte na **OK** Zaregistrujte službu nastavení mobilních zařízení s konfigurační server.

**Spuštění z příkazového řádku:**

1. Kopírování heslo z CX do souboru "C:\connection.passphrase" na serveru a spusťte tento příkaz. V našem příkladu CX i 104.40.75.37 a HTTPS port je 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Instalace služby mobilita ručně na serveru Linux**:

1. Zkopírujte odpovídající vkládání archivu podle předchozí tabulce, ze serveru obrázku do počítače zdroje.
2. Spusťte aplikaci prostředí a extrahujte ZIP vkládání archiv na místní cestu spuštěním`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Vytvoření souboru passphrase.txt místního adresáře, do kterého jste extrahovali obsah archivu vkládání zadáním *`echo <passphrase> >passphrase.txt`* z prostředí.
4. Instalace služby mobilita zadáním *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Zadejte IP adresu a portu:

    - Pokud se připojujete konfigurační server přes internet zadejte konfigurace virtuální veřejné IP adresu serveru a veřejné koncový bod HTTPS v `<IP address>` a `<port>`.
    - Pokud se připojujete přes připojení VPN zadejte interní IP adresy a 443.

**Pokud chcete spustit z příkazového řádku**:

1. Kopírování heslo z CX do souboru "passphrase.txt" na serveru a spusťte tento příkaz. V našem příkladu CX i 104.40.75.37 a HTTPS port je 62519:

Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Instalace na cílovém serveru:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Po přidání stroje ke skupině zámek právě spuštěné upozornění příslušné verzi službu mobilita potom instalace nabízená vynechán.


## <a name="step-9-enable-protection"></a>Krok 9: Povolit ochranu

Povolení ochrany přidáte virtuálních počítačích a fyzické servery ke skupině zámek. Než začnete, Všimněte si, že:

- Virtuálních počítačích jsou zjištěny každých 15 minut a může trvat až 15 minut, aby se v obnovení webu Azure po zjišťování.
- Změny prostředí virtuální počítače (například instalace nástrojů VMware) můžete taky trvat až 15 minut aktualizují v obnovení webu.
- Můžete se změnami zjištěnou posledního pole **Poslední kontakt na** hostitele serveru/ESXi vCenter na stránce **Konfigurace servery** .
- Pokud máte vytvořen ochranu skupiny a přidání vCenter serveru nebo ESXi hostitele poté trvá patnáct minut portál obnovení webu Azure a aktualizace a odstranění virtuálních počítačích uvedena v dialogovém okně **Přidat počítače na ochranu skupiny** .
- Pokud chcete pokračovat v okamžitě přidání stroje ochranu skupiny, aniž byste čekali plánované zjišťování, zvýrazněte konfigurační server (neklikejte na ho) a klikněte na tlačítko **Aktualizovat** .
- Když přidáte virtuálních počítačích nebo pole fyzicky počítačích ochranu skupiny, serveru proces automaticky posune nainstaluje automaticky službu mobilita na zdrojovém serveru it bez nainstalovaného už
- Pro automatické nabízená mechanismus pracovat zkontrolujte, jestli jste nastavíte počítače chráněné popsané v předchozím kroku.

Přidání stroje následujícím způsobem:

1. Klepněte na **chráněný položky** > **Ochranu skupiny** > **počítačích** > **Přidat počítačích**. Osvědčený doporučujeme, aby měli ochranu skupiny tak, že přidáte počítače se systémem dané aplikace do stejné skupiny odrážet svého pracovního vytížení.
2. Ve **virtuálních počítačích vyberte** Pokud jste chránit fyzických serverů, v průvodci **Přidat fyzických počítačů** zadejte IP adresu a popisný název. Vyberte skupinu operační systém.

    ![Přidání V okraji ve středu serveru](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. **Vyberte virtuálních počítačích** se chránit VMware virtuálních počítačích, vyberte vCenter serveru, který je Správa virtuálních počítačích (neboli EXSi host, na kterém máte systém) a potom vyberte počítače.

    ![Přidání V okraji ve středu serveru](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. V **Zadejte prostředcích cílové** vyberte předlohy cílové servery a úložiště pomocí replikace a vyberte, zda má být použita nastavení pro všechny úlohami. Vyberte [Účet úložiště Premium](../storage/storage-premium-storage.md) při konfiguraci ochranu úloh, které vyžaduje hostovat náročné pracovního vytížení vstupu a výstupu konzistentní vysoké ZVYŠUJÍ výkon a nízké latence. Pokud chcete používat účet Premium úložiště pracovní zátěž disku, musíte použít cílové předlohy DS řady. Disků Premium úložiště nelze použít s cílem předlohy DS řady.

    >[AZURE.NOTE] Nepodporujeme přesunout úložiště účtů vytvořených pomocí [nového Azure portálu](../storage/storage-create-storage-account.md) mezi skupinami zdroje.

    ![vCenter serveru](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. **Zadejte** účty vyberte účet, který chcete použít pro instalaci služby mobilita na zamknutém počítačích. Přihlašovací údaje účtu jsou potřebné pro automatické instalace služby nastavení mobilních zařízení. Pokud vám nejdou vybrat účtu Ujistěte se, že můžete nastavit jeden podle popisu v části Krok 2. Všimněte si, že tento účet nemají přístup k Azure. Pro Windows server účtu by měl oprávnění správce na zdrojovém serveru. Linux účet musí být kořenové.

    ![Přihlašovací údaje Linux](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Klikněte na značku zaškrtnutí dokončíte přidávání počítačích ochranu skupiny a spuštění počáteční replikace pro každý počítač. Můžete sledovat stav na stránce **úlohy** .

    ![Přidání V okraji ve středu serveru](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Kromě toho můžete sledovat stav ochrany kliknutím **Chráněné položky** > název ochranu skupiny > **virtuálních počítačích** . Po dokončení počáteční replikace a strojů synchronizují dat se zobrazí stav **chráněného** .

    ![Virtuální počítač úlohy](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Nastavení chráněné počítače vlastností

1. Po stroje má stav **chráněného** můžete nakonfigurovat vlastností překlopení. V podrobnostech ochranu skupiny vyberte počítače a otevřete kartu **Konfigurovat** .
2. Můžete změnit název, který budou mít počítači v Azure po převzetí a velikost Azure virtuálního počítače. Můžete také vybrat Azure sítě, ke které se v počítači připojený po překlopení.

    > [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.

    ![Nastavení vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Všimněte si, že:

- Název počítače, Azure musí vyhovovat požadavkům Azure.
- Ve výchozím nastavení nejsou replikovanou virtuálních počítačích v Azure připojení k síti Azure. Pokud chcete, aby replikovanou virtuálních počítačích komunikovat nezapomeňte nastavit stejné Azure síti, je.
- Když změníte hlasitost na VMware virtuálního počítače nebo pole fyzicky serveru přejde do stavu kritických. Pokud potřebujete změnit velikost, postupujte takto:

    - a) změňte nastavení velikosti.
    - b) na kartě **virtuálních počítačích** vyberte virtuální počítač a klikněte na tlačítko **Odebrat**.
    - c) v **Odebrat počítač virtuální** vyberte požadovanou možnost **Zakázat ochranu (slouží k obnovení procházení a množství změnit velikost)**. Tato možnost zakáže ochranu ale zachová obnovení bodů v Azure.

        ![Nastavení vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) znovu povolte ochranu virtuální počítač. Když povolíte ochranu dojde k Azure převedení dat pro změněnou velikostí hlasitost.

    

## <a name="step-10-run-a-failover"></a>Krok 10: Spuštění selhání

Aktuálně jenom spuštěním neplánované převzetí služeb při selhání chráněné VMware virtuálních počítačích a fyzické serverů. Poznámka:



- Dříve než zahájíte přepojení zajištění cílových serverů konfigurace a předlohy a správnost. Převzetí jinak se nezdaří.
- Zdroj počítačích nejsou vypnout jako součást neplánované převzetí. Provádění neplánované převzetí zastaví replikace dat pro chráněné servery. Budete muset zabránění skupině zámek počítačích a znovu přidat k začněte znova chránit počítačích po dokončení neplánované převzetí.
- Pokud chcete převzít bez ztráty dat, ujistěte se, že virtuálních počítačích primární webu jsou vypnou a zůstanou vypnuté než zahájíte záložní.

1. Na **Obnovení plány jednotného zasílání zpráv** a přidejte plán obnovení. Zadejte podrobnosti pro plán a vyberte **Azure** jako cíl.

    ![Konfigurace obnovení plán](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. V **Vyberte virtuální počítač** vyberte ochranu skupiny a pak vyberte počítačích ve skupině přidat do plánu obnovení. [Přečtěte si další](site-recovery-create-recovery-plans.md) informace o obnovení plány.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Pokud potřeby je možné upravit budete chtít vytvořit skupiny a pořadí pořadí na kterých počítačích obnovení plánu jsou selhání. Můžete taky přidat pokynů pro ruční akce a skriptů. Tyto skripty při obnovení Azure lze přidat pomocí [Runbooks automatizaci Azure](site-recovery-runbook-automation.md).

4. Na stránce **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **Neplánované převzetí**.
5. V **Potvrďte převzetí** ověření směru převzetí (Azure k) a vyberte požadovaný bod obnovení přenesou do.
6. Počkejte úlohy převzetí dokončíte a ověřte, zda záložní funguje očekávaným způsobem a úspěšně spustit replikovanou virtuálních počítačích v Azure.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Krok 11: Selhání zpátky selhání počítačích z Azure

[Další informace](site-recovery-failback-azure-to-vmware-classic-legacy.md) o tom, jak přenést svůj selhalo přes počítače se systémem Azure zpátky do místního prostředí.


## <a name="manage-your-process-servers"></a>Správa serverech obrázku

Server proces odešle data replikace předlohy cílový server v Azure a nenajde nové VMware virtuálních počítačích vCenter serveru přidat. V následujících případech můžete chtít změnit server obrázku v nasazení:

- V případě aktuální proces serveru
- Pokud vaším cílem bodu obnovení (operace RPO) roste nepřijatelná úrovni pro vaši organizaci.

V případě potřeby replikace některé nebo všechny své místní VMware virtuálních počítačích a fyzické servery můžete přesunout na jiné zpracování server. Příklad:

- **Chyba při**– Pokud server proces selže nebo není k dispozici chráněné počítačů můžete přesunout do jiného proces serveru. Metadata zdrojového počítače a otevřené počítače se přesune do nového obrázku serveru a synchronizace dat. Nový server proces bude automaticky připojit k serveru vCenter provádět automatické zjišťování. Můžete sledovat stav procesu servery na řídicím panelu obnovení webu.
- **Chcete-li upravit operace RPO Vyrovnávání zatížení**– zdokonalené můžete Vyrovnávání zatížení, můžete vybrat jiné zpracování server v portálu obnovení webu a přesunutí replikace jeden nebo více počítačů pro vyrovnávání zatížení ručně. V tomto případě metadat vybraný zdroj a otevřené strojů přesune se do nového obrázku serveru. Původní server procesu zůstane připojení k serveru: vCenter. 

### <a name="monitor-the-process-server"></a>Sledování serveru obrázku

Je-li proces server ve stavu kritických zobrazí se upozornění stav na řídicím panelu webu obnovení. Kliknete na stav a otevřete kartu událostí a pak procházet hierarchii na určité úkoly na kartě projekty. 

### <a name="modify-the-process-server-used-for-replication"></a>Úprava obrázku serveru pro replikační

1. Otevřete **servery** > **Konfigurace serverů** > konfigurační server > **Podrobnosti o Server**.
2. Klikněte na položku **Proces servery** > **Změnit proces Server** vedle položky server, který chcete upravit.

    ![Změna obrázku serveru 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. Na **Serveru proces změnit** > **Cílový proces Server** vyberte nové server, který chcete použít a potom vyberte virtuálních počítačích, které chcete replikovat do nového serveru. Klikněte na informační ikonu vedle názvu serveru pro podrobnosti volného místa a použité paměti. Místo na průměr, které budou muset replikovat každý vybraný virtuální počítač nový server proces se zobrazí, budete moct přijímat načíst rozhodnutí.

    ![Změna obrázku Server 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Klikněte na značku zaškrtnutí zahájíte replikace do nového obrázku serveru. Všimněte si, že pokud odstraníte všechny virtuálních počítačích ze serveru proces, který byl kritické ji už zobrazí kritické upozornění na řídicím panelu.


## <a name="third-party-software-notices-and-information"></a>Oznámení o třetích stran software a informací

Co nedělat překlad nebo Localize

Software a firmware spuštěné v produktu společnosti Microsoft nebo služby je založená na nebo zahrne materiál z projektů uvedených pod ním (společně "výrobců kódu").  Společnost Microsoft se není původní autor výrobců kódu.  Původní autorských práv a licence, pod kterým společnost Microsoft tyto výrobců kódu, jsou nastavené níže uvedenými.

Informace v části A týká komponent výrobců kódu z níže uvedených projektů. Tyto licence a informace jsou k dispozici pouze pro informaci.  Tento kód třetích stran je právě relicensed vám společnost Microsoft ve skupinovém rámečku licenčních podmínek softwaru společnosti Microsoft pro Microsoft produktů nebo služeb.  

Informace v části B se týká součásti výrobců kódu, které budou k dispozici vám společnost Microsoft ve skupinovém rámečku původní licenčních podmínek.

Celý soubor najdete na webu služby [Stažení softwaru společnosti Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všemi právy není výslovně udělena, zda implicitně, odvozeně nebo jinak.
