<properties
    pageTitle="Vytvoření plány obnovy | Microsoft Azure" 
    description="Vytvoření plány obnovy s obnovení webu Azure převzít a obnovit skupin virtuálních počítačích a fyzické servery." 
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

# <a name="create-recovery-plans"></a>Vytvoření plány obnovy

Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md).


## <a name="overview"></a>Základní informace

Tento článek obsahuje informace o vytváření a přizpůsobení obnovení plány. 

Obnovení plány obsahovat více uspořádaných skupin, které obsahují virtuálních počítačích nebo pole fyzicky servery, které chcete překlopení sobě. Obnovení plány postupujte takto:

- Definujte skupiny stroje, které převzít a společně otevřou.
- Model závislostí mezi počítačích tak, že je seskupování ve skupině plán obnovení. Pro příklad, pokud chcete převzít a vyvoláte dané aplikace by skupinu virtuálních počítačích aplikace ve stejné skupině plán obnovení.
- Automatizace a rozšíření překlopení. Test plánované, nebo neplánované převzetí možné spouštět na plán obnovení. Můžete přizpůsobit plány obnovy s skripty, Azure automatické nebo ruční akce.

Obnovení plány se zobrazí na **Plány obnovy** na portálu obnovení webu.


V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="before-you-start"></a>Než začnete

Poznámka:

- Plán obnovy neměli kombinovat VMs s jednoho a více síťové adaptéry. Je to proto společným tyto VMs nejsou povolené v Azure cloudové službě.
- Obnovení plány skripty a ruční akce můžete rozšířit. Poznámka:
    - Psaní skriptů pomocí Windows Powershellu.
    - Zajistit, že skripty používat zkuste skutečné bloky tak, aby výjimky fungují řádně. Pokud výjimka ve skriptu se zastaví a úkol je zobrazen, která se nezdařila.  Pokud dojde k chybě, všechny zbylé části skript se nespustí. Pokud k tomu dojde při spuštění neplánované převzetí, zůstanou v plánu obnovy. V takovém případě při spuštění plánované přepojení plánu obnovy přestanou se vám. V takovém případě opravte skript, ujistěte se, že ho očekávaným způsobem a pak znova spusťte plánu obnovy.
    - Příkaz zápisu hostitele nefunguje v plánu skript obnovení a skript se nezdaří. Pokud chcete vytvořit výstup, vytvořit skript proxy, které se zase spustí hlavní skriptu a ujistěte se, že všechny je výstup pomocí >> příkaz.
    - Skript vyprší její časový limit Pokud 600 vyvolané nevrátí.
    - Pokud všechno, co je aby došlo k zápisu STDERR, bude skript zařadit jako neúspěšný. Zobrazí se tyto informace v podrobnostech spuštění skriptu.
    - Pokud používáte VMM ve vašem nasazení Všimněte si, že:

        - V kontextu účet služby VMM spustit skripty v plánu obnovení. Ujistěte se, že tento účet nemá oprávnění ke čtení vzdálené sdílené složky, ve kterém je umístěn skript a otestujte skript spouštět na úrovni VMM služby účet oprávnění.
        - Rutiny pro VMM doručované v modulu Windows PowerShell. Modul VMM Windows PowerShell nainstalovaný při instalaci konzole VMM. Modul VMM lze načíst do skript pomocí následujícího příkazu ve skriptu: modul Import-název virtualmachinemanager. [Získání dalších podrobností](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Zajistěte, že máte alespoň jeden server knihovny v VMM nasazení. Ve výchozím nastavení je cesta ke sdílené složce knihovny pro VMM server místně v VMM server s název složky MSCVMMLibrary.
        - Pokud cesta sdílet knihovny vzdálený (nebo místní ale není sdílené s MSCVMMLibrary a konfigurovat sdílet následujícím způsobem (pomocí \\libserver2.contoso.com\share\ jako příklad):
            - Otevřete Editor registru a přejděte na web Recovery\Registration HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure.
            -  Hodnotu ScriptLibraryPath upravit a umístěte hodnotu jako \\libserver2.contoso.com\share\. zadejte úplnou plně kvalifikovaný název domény. Zadejte oprávnění ke sdílení umístění.
            -  Ujistěte se, otestujte skript k uživatelskému účtu, který má stejná oprávnění jako účet služby VMM zajistit bezchybnou samostatné testované skripty stejným způsobem se budou v plánech obnovení. Na serveru VMM nastavení zásad spuštění obejít takto:
                -  Spusťte konzolu 64bitová verze Windows PowerShell pomocí zvýšenými oprávněními.
                -  Typ: **Set-zásady spouštění obejít**. [Získání dalších podrobností](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

Způsob, ve kterém můžete vytvořit plán obnovení závisí na nasazení obnovení webu.

- **Replikace VMs VMware a fyzické servery**– vytvoření plánu a přidejte replikace skupiny, které obsahují virtuálních počítačích a fyzické servery obnovení plán.
- **Replikace Hyper-V VMs (v VMM mračnech)**– vytvoření plánu a přidat chráněné virtuálních počítačích Hyper-V cloudu VMM obnovení plán.
- **Replikace Hyper-V VMs (ale ne v VMM mračnech)**– vytvoření plánu a přidejte chráněné Hyper-V virtuálních počítačích ze skupiny ochranu obnovení plán.
- **Replikace SAN**– vytvoření plánu a přidejte skupinu replikace, která obsahuje virtuálních počítačích obnovení plán. Skupiny replikace místo vyberete konkrétní virtuálních počítačích protože všechny virtuálních počítačích ve skupině replikace musí společně selhání (dojde k selhání ve vrstvě úložiště nejdřív).


Vytvoření plánu pro obnovení následujícím způsobem:

1. Klikněte na kartu **Obnovení plány jednotného zasílání zpráv** > **Vytvořit plán obnovení**.
Zadejte název plánu obnovy a zdrojové a cílové. Zdrojovému serveru musí mít virtuálních počítačích, kteří mají povolenou převzetí a obnovení.

    - Pokud jste replikace z VMM do VMM vyberte **Typ zdroje** > **VMM**a servery VMM zdrojové a cílové. Klikněte na tlačítko **Hyper-V** zobrazíte shluky, které jsou nakonfigurovaný na použití pro Hyper-V otevřené. 
    - Pokud jste replikace z VMM do VMM pomocí SAN vyberte **Typ zdroje** > **VMM**a servery VMM zdrojové a cílové. Klikněte na tlačítko **SAN** zobrazíte mračnech nakonfigurované pro SAN replikace.
    - Pokud jste replikace z VMM na Azure vyberte **Typ zdroje** > **VMM**.  Vyberte zdroj VMM serveru a **Azure** jako cíl.
    - Pokud jste replikace z webu pro Hyper-V vyberte **Typ zdroje** > **Hyper-V webu**. Vyberte požadovaný web jako zdroj a **Azure **jako cíl.
    - Pokud jste replikace z VMware nebo pole fyzicky místního serveru na Azure, vyberte konfigurace serveru jako zdroje a **Azure** jako cíl

2. V seznamu **Vyberte virtuálních počítačích** vyberte na virtuálních počítačích (nebo replikace skupinu), který chcete přidat do výchozí skupiny (skupiny 1) v plánu obnovení.

## <a name="customize-recovery-plans"></a>Přizpůsobení plány obnovy

Po přidání chráněné virtuálních počítačích nebo skupiny replikace do výchozí skupiny obnovení plán a vytvoření plánu, že je možné upravit:

- **Přidání nové skupiny**– můžete přidat další obnovení plán skupiny. V takovém pořadí, ve kterém je přidáte mají být číslovány skupin, do kterých můžete přidat. Můžete přidat až sedm skupiny. Můžete přidat víc počítačů nebo skupiny replikace na tyto nové skupiny. Všimněte si, že virtuálního počítače nebo skupiny replikace můžete jenom být součástí jedné obnovení plán skupiny.
- **Přidání skriptu **– můžete přidat skripty, které před datem start_date nebo po obnovení Skupina plánu. Po přidání skriptu přidá nové sady akcí pro skupinu. Například vytvořit sadu před postup pro skupiny 1 s názvem: skupiny 1: před kroky. Všechny předběžné kroky uvedené v této sadě. Všimněte si, že můžete jenom přidat skriptu na primární webu máte VMM server nasazení.
- **Přidání ručního akce**– můžete přidat ručně akcí před datem start_date nebo po obnovení plán skupiny. Při spuštění plánu obnovy zastaví v bodě, pro niž jste vložili ručně akce a dialogovým oknem vás vyzve k zadání, aby byl dokončen ruční akce.

## <a name="extend-recovery-plans-with-scripts"></a>Rozšíření plány obnovy pomocí skriptů

Skript můžete přidat do svého plánu obnovení:

- Pokud máte VMM zdrojového webu můžete vytvořit skript na serveru VMM a zahrnout do plánu obnovení.
- Pokud jste replikace na Azure Azure automatizaci runbooks můžete integrovat do plánu obnovení

### <a name="create-a-vmm-script"></a>Vytvořit skript VMM


Vytvořte skript následujícím způsobem:

1. Vytvoření nové složky ve sdílené složce knihovny, například \<VMMServerName > \MSSCVMMLibrary\RPScripts. Přenesení zdroji a cílového webu VMM servery.
2. Vytvořit skript (například RPScript) a zkontrolujte, že to funguje očekávaným způsobem.
3. Vložením skriptu do požadovaného umístění \<VMMServerName > \MSSCVMMLibrary na serverech VMM zdrojové a cílové.

### <a name="create-an-azure-automation-runbook"></a>Vytvoření postupu runbook Azure automatizaci

Rozšíření plánu obnovení spuštěním postupu runbook Azure automatizaci jako součást plánu. [Přečtěte si další](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Přidání vlastní nastavení obnovení plán

1. Otevřete obnovení plán, který chcete upravit.
2. Kliknutím vložíte virtuálních počítačích nebo nové skupiny.
3. Přidání skriptu nebo ruční akce klikněte na libovolnou položku v seznamu **Krok** a potom na **skript** nebo **Ruční akce**. Určete, zda chcete přidejte skript nebo akci před nebo za vybrané položky. Příkazová tlačítka **Nahoru** a **Dolů** umožňuje nastavit pozici skript nahoru nebo dolů.
4. Pokud přidáváte VMM skript, vyberte **převzetí VMM skriptu**a v do **Cesta skriptu** zadejte relativní cestu ke sdílené složce. Ano, našem příkladu, kde je umístěný na příkaz Sdílet \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, zadejte cestu: \RPScripts\RPScript.PS1.
5. Pokud přidáváte Azure automatické spuštění knihy, zadejte **Účtu automatizaci Azure** , ve kterém se nachází postupu runbook a vyberte příslušný **Azure postupu Runbook skriptu**.
5. Proveďte selhání obnovení plán, který zajistí, že skript funguje očekávaným způsobem.


## <a name="next-steps"></a>Další kroky

Různé druhy převzetí služeb při selhání možné spouštět na plán obnovení, včetně selhání test Kontrola prostředí a plánované nebo neplánované převzetí služeb při selhání. [Další informace](site-recovery-failover.md).


 
