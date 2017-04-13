<properties
    pageTitle="Plánování kapacity chránících virtuálních počítačích a fyzické servery v obnovení webu Azure | Microsoft Azure"
    description="Obnovení Azure webu souřadnic replikace, převzetí a obnovení virtuálních počítačích a fyzické servery nachází na místním Azure nebo vedlejší místního serveru." 
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
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Plánování kapacity chránících virtuálních počítačích a fyzické servery v obnovení webu Azure

Nástroj Azure webu obnovení kapacita plánovač umožňuje zjistit vašim požadavkům kapacita pro ochranu pro Hyper-V VMs, VMware VMs a Windows/Linux fyzické servery s obnovení webu Azure.


## <a name="overview"></a>Základní informace

Použijte plánovači webu obnovení kapacitu k analýze zdrojového prostředí a pracovního vytížení a zjistit šířky pásma potřeb prostředků serveru, které budete potřebovat v umístění zdroje a prostředcích (virtuálních počítačích a úložiště atd), musíte mít v cílové umístění. 

Spustit nástroj několika způsoby:

- **Plánovací stručný**: spustit nástroj v tomto režimu a získat sítě a serveru průzkumy podle průměrný počet VMs, disků, ukládání a změna kurzu.
- **Podrobné plánování**: Spusťte nástroj v tomto režimu a uveďte podrobnosti každé pracovní zátěž úrovni OM. Analýza OM kompatibility a najděte sítě a serveru prognózy.

## <a name="before-you-start"></a>Než začnete

Před spuštěním nástroje:

1. Shromážděte informace o prostředí, včetně VMs disků za OM, úložiště na disku.
2. Určete frekvenci denní změnit (konve) pro replikovanou data. Akce:

    - Máte replikace Hyper-V VMs nakonec [Hyper-V nástroji plánování kapacity](https://www.microsoft.com/download/details.aspx?id=39057) získat změnit rychlost stáhnout. [Další informace](site-recovery-capacity-planning-for-hyper-v-replication.md) o tomto nástroji. Doporučujeme že spustit tento nástroj déle než týden k zaznamenání průměry.
    - Pokud jste replikace VMware virtuálních počítačích, umožňuje [plánování zařízení kapacity vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) zjistit konve sazbu.
    - Pokud jste replikace fyzické servery, které budete potřebovat k odhadu ručně.

## <a name="run-the-quick-planner"></a>Spuštění snadné Plánovač
1.  Stáhněte si a spuštění nástroje pro správu [Plánování kapacity obnovení webu Azure](http://aka.ms/asr-capacity-planner-excel) . Je potřeba spustit makra tak zaškrtnete toto políčko Povolit úpravy a zpřístupnění obsahu při zobrazení výzvy. 
2.  V seznamu **Vyberte typ Plánovač** vyberte **Rychlé Plánovač** ze seznamu.

    ![Začínáme](./media/site-recovery-capacity-planner/getting-started.png)

3.  V listu **Kapacita Plánovač** zadejte požadované informace. Je třeba zadat ve všech vrácených polích kruhu v červené barvě v níže snímek.

    - V seznamu **Vyberte nefunguje** zvolte **Hyper-V Azure** nebo **VMware/fyzické na Azure**.
    - V **Průměrná denní dat změnit rychlost (%)** umístění v informacích o shromažďujete [Hyper-V nástroji plánování kapacity](site-recovery-capacity-planning-for-hyper-v-replication.md) nebo [vSphere kapacita plánování zařízení](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **Komprese** pouze u komprese nabízených při replikace VMware VMs nebo pole fyzicky servery na Azure. Jsme odhadu 30 % nebo více ale můžete upravit nastavení podle potřeby. Replikace VMs Hyper-V Azure komprese můžete například Riverbed zařízení jiných výrobců. 
    -  V **Uchovávání informací vstupů** určete, jak dlouho by měl být zachován repliky. Pokud jste replikace VMware nebo pole fyzicky servery vstupní hodnotu ve dnech. Pokud jste replikace Hyper-V zadejte čas v hodinách.
    -  **Počet hodin, ve kterých počáteční replikace dávky virtuálních počítačích měli dokončit** a **počet virtuálních počítačích výrobního počáteční replikace** zadávání nastavení, která se používají pro výpočet požadavků počáteční replikace.  Při nasazení obnovení webu by měl nahrané celý počáteční uvedenou množinu dat. 

    ![Zadávání](./media/site-recovery-capacity-planner/inputs.png)

2.  Po, kterou jste do pole hodnoty pro zdrojového prostředí, zobrazeného výstup zahrnuje:

    - **Šířka pásma povinné replikace delta** (MB/sec). Šířka pásma delta replikace je vypočítána na průměrná rychlost denní změnit data:
    - **Šířka pásma požadované počáteční replikace** (MB/sec). Šířka pásma pro počáteční replikace je vypočítána na počáteční replikace hodnoty, kterou chcete vložit. 
    - **Úložiště podle (GB)** je celkové Azure úložiště povinné.
    - **Celková procesorů účtů standardní úložiště** je vypočítána na velikost jednotky procesorů 8 K účtů celkové úložiště standardní základě.  Rychlé plánovači vypočítané čísla na základě zdrojových VMs disků a denní dat změňte rychlost. Podrobné plánovači, které čísla se vypočítá podle celkový počet VMs, které jsou namapované na standardní Azure VMs a data změňte rychlost na tyto VMs. 
    - **Počet standardní úložiště účtů** poskytuje celkový počet standardní úložiště účty potřebné k ochraně VMs. Všimněte si, že účet standardní úložiště můžou obsahovat až 20000 procesorů přes všechny VMs standardní úložiště a maximální 500 procesorů podporované na disku. 
    - **Počet objektů blob disků povinné** představuje počet disků vytvořené Azure úložný prostor.
    - **Počet premium úložiště účtů povinné** obsahuje celkový počet účty úložiště premium potřebné k ochraně VMs. Všimněte si, že zdroj OM s vysokou procesorů (vyšší než 20000) vyžaduje účet úložiště premium. Účet úložiště premium můžou obsahovat až 80000 procesorů.
    - **Celkové procesorů úložný prostor premium** se vypočítá založené na 256 K procesorů jednotku velikosti úložiště účtů celkové premium.  Rychlé plánovači vypočítané čísla na základě zdrojových VMs disků a denní dat změňte rychlost. Pro podrobné plánování číslo je vypočítána na základě celkový počet VMs, které jsou namapované na premium Azure OM (Pošta a GS řad) a data změnit rychlost na tyto VMs. 
    - **Počet konfigurace servery povinné** zobrazuje kolik konfigurace servery jsou potřeba pro nasazení (1)
    - **Počet další proces servery povinné** ukazuje, zda jsou vyžadovány kromě serveru proces, který je nakonfigurovaný na konfigurační server ve výchozím nastavení další proces servery.
    - **100 % další úložiště ve zdroji** ukazuje, jestli je v umístění zdroje vyžaduje další úložiště.
            
    ![Výstup](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Spuštění podrobné plánování


1.  Stáhněte si a spuštění nástroje pro správu [Plánování kapacity obnovení webu Azure](http://aka.ms/asr-capacity-planner-excel) . Je potřeba spustit makra tak zaškrtnete toto políčko Povolit úpravy a zpřístupnění obsahu při zobrazení výzvy. 
2.  V seznamu **Vyberte typ Plánovač** vyberte **Podrobné plánování** ze seznamu.

    ![Začínáme](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  V listu **Kvalifikace zátěží na projektu** zadejte požadované informace. Je třeba zadat ve všech polích označené.

    - V **jádra procesoru** určete celkový počet jádra na zdrojový server.
    - V **přidělování paměti v Megabajtech** specifikovaly velikost RAM zdrojovému serveru. 
    - **Číslem nic** určete počet síťové adaptéry na zdrojový server. 
    -  V **celkové (GB)** zadejte celková velikost úložiště OM. Například pokud zdrojovému serveru obsahuje 3 disků s 500 GB a potom celková velikost je 1 500 GB.
    -  V **části disků připojeny počet** zadejte celkový počet disků zdrojovému serveru.
    -  Využití **kapacity disku** zadejte průměr využití.
    -  V **denně změnit rychlost (%)** zadejte jména že denní dat změna frekvence zdrojovému serveru.
    -  Velikost **Mapování Azure** zadejte velikost OM Azure, který chcete propojit. Pokud nechcete provést ručně klikněte na**Výpočet VMs IaaS**. Všimněte si, že pokud vstupní ručního nastavení a potom klikněte na výpočet VMs IaaS ruční nastavení může přepsat, protože proces výpočetním automaticky identifikuje nejvhodnější velikosti OM Azure.

    ![Pracovní zátěž kvalifikace](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Pokud kliknete na **Výpočet VMs IaaS** tady je akce:

    - Ověřuje povinné vstupy.
    - Vypočítá procesorů a navrhne nejlepší OM Azure aize odpovídající pro každou VMs je oprávněn pro replikace Azure. Pokud odpovídající velikost Azure OM nelze zjistit, že dojde k chybě. Pokud počet disků připojených v 65 chybovou je příklad problémy od nejvyšší velikost je Azure OM 64.
    - Navrhne úložiště účet, který se dá použít pro OM Azure.
    - Vypočítá celkový počet standardní úložiště účtů a premium úložiště potřebných pro pracovní zátěž. Přejděte dolů na pravé straně zobrazení typ Azure úložiště a úložiště účet, který se dá použít pro zdrojový server
    - Dokončení a řazení zbytek tabulky založené na typ požadovaný úložiště (standardní nebo premium) přiřazené virtuálního počítače a počet disků připojené. Pro všechny VMs splňující požadavky pro zálohování Azure, sloupci A (je OM kvalifikované?) zobrazí Ano. Pokud OM nemůže být až Azure chybovou zobrazený.

Sloupce AA AE jsou výstup a zadejte informace pro každou OM.

![Pracovní zátěž kvalifikace](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Příklad
Jako příklad, šest VMs s hodnotami v tabulce nástroj a nejvhodnější Azure OM a požadavky Azure úložiště.

![Pracovní zátěž kvalifikace](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- V výstup, aby příklad si všimněte těchto věcí:
    
    - První sloupec představuje ověření sloupec VMs, disků a konve.
    - Dva účty standardní úložiště a jeden premium úložiště účet jsou potřebné pro pět VMs. 
    -  VM3 nemá nárok na ochranu, protože jeden nebo více disků jsou víc než 1 TB.
    -  VM1 a VM2 můžete použít první účet standardní úložiště
    -  VM4 můžete použít druhý účet standardní úložiště.
    -  VM5 a VM6 je třeba účet úložiště premium a jak pomocí jednoho účtu.

    >[AZURE.NOTE]  Procesorů standardních a premium úložný prostor se vypočítávají na úrovni OM a ne na úrovni disku. Standardní virtuálního počítače zpracovat až 500 procesorů na disku. Pokud jsou větší než 500 procesorů disku budete potřebovat premium úložiště. Ale pokud procesorů disku jsou víc než 500, ale procesorů celkové disků OM v určených mezích standardní Azure OM podpory (OM velikost, počet disků, počet adaptéry procesoru, paměti) potom plánovači použije standardní OM a ne DS nebo GS řady. Budete muset ručně aktualizovat mapování Azure velikost buňky s odpovídající řadu Pošta nebo GS OM.

5. Až všechny podrobnosti na místě, klikněte na **Odeslat data do nástroje Plánovač** otevřete **Kapacita Plánovač** úloh jsou zvýrazněné zobrazíte, zda budou nárok na ochranu nebo ne.


### <a name="submit-data-in-the-capacity-planner"></a>Odeslání dat v Plánovači kapacity

1.  Při otevření listu **Kapacita Plánovač** je vyplněné na základě nastavení, které jste zadali. Slovo "Pracovní zátěž" se zobrazí v buňce **Infra vstupů zdroj** zobrazíte, zadávání **Pracovní zátěž kvalifikace** listu. 
2.  Pokud chcete provést změny, které budete potřebovat změnit **Pracovní zátěž kvalifikace** listu a klepněte na tlačítko Odeslat data na nástroj pro plánování.  

    ![Plánování kapacity](./media/site-recovery-capacity-planner/capacity-planner.png)


