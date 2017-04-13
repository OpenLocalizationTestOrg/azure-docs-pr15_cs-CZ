<properties
    pageTitle="Replikace Hyper-V s obnovení webu Azure | Microsoft Azure"
    description="Princip technické koncepty, které vám pomohou úspěšně instalace, konfigurace a správa obnovení webu Azure pomocí tohoto článku."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Replikace Hyper-V s obnovení webu Azure

Tento článek popisuje technické koncepty, které vám pomohou úspěšně konfigurace a správa Hyper-V webů nebo web System Center virtuálního počítače Manager (VMM) Azure ochrany pomocí obnovení webu Azure.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Nastavení prostředí zdroj replikace mezi místním prostředím a Azure

V rámci nastavení havárie obnovení mezi místním prostředím a Azure nezapomeňte si stáhněte a nainstalujte poskytovatel obnovení webu Azure na serveru VMM. Nainstalujte agenta služby Azure obnovení v každém Hyper-V hostiteli.

![Nasazení serveru VMM replikace mezi místním prostředím a Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Nastavení prostředí zdroje v infrastrukturu Hyper-V spravovaných je podobný nastavení prostředí zdroje v spravovaných infrastrukturu VMM. Pouze rozdíl se vypočítá následujícím nainstalované poskytovatele a agent na hostiteli Hyper-V samotné. V prostředí VMM poskytovatel je nainstalovaný na serveru VMM a je nainstalovaný agent pro Hyper-V hostitele (v případě replikace Azure).

## <a name="workflows"></a>Pracovní postupy

### <a name="enable-protection"></a>Povolení ochrany
Po zamknutí virtuálního počítače z portálu Azure nebo místní zahájení úlohy obnovení webu s názvem **Zamknout** . Můžete ho sledovat na kartě **projekty** .

!["Povolit ochranu" úlohy v seznamu projektů](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

**Povolte ochranu** úlohu hledá požadavky před [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) metody. Tento způsob vytvoří replikace Azure pomocí vstupy, které jsou nakonfigurovány během zámek.

**Povolte ochranu** úlohu začíná počáteční replikace místní vyvoláním [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metody. Tento způsob pošle virtuálních disků virtuálního počítače Azure.

![Podrobnosti projektu "Povolit ochranu"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Dokončení ochrany počítače virtuální
Při aktivaci počáteční replikace, se považuje [Hyper-V OM snímek](https://technet.microsoft.com/library/dd560637.aspx) . Virtuální pevných discích jsou zpracovaných postupně do všech discích odeslány do Azure. To obvykle trvá určitou dobu dokončení, na základě velikost disku a šířku pásma. Optimalizace využití sítě, najdete v tématu [Správa místních využití šířky pásma sítě Azure zámek](https://support.microsoft.com/kb/3056159).

Po dokončení počáteční replikace úlohy **Dokončit ochrany počítače virtuální** konfiguruje nastavení sítě a po replikace. Počáteční replikace v průběhu:

- Všechny změny disků sledují. 
- Uvolněte úložiště je spotřebované množství snímek a soubory otevřené Hyper-V protokolu (HRL).

Po skončení počáteční replikace snímek Hyper-V OM odstraněna. Výsledkem této odstranění slučování dat změn po počáteční replikace nadřazený disk.

!["Dokončení ochrany počítače virtuální" úlohy v seznamu projektů](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Replikace delta
Sledování replikace otevřené pro Hyper-V, který je součástí modul replikace Hyper-V otevřené, sleduje změny virtuální pevný disk jako Hyper-V otevřené (*.hrl) protokoly. Soubory HRL jsou ve stejném adresáři jako přidružené discích.

Každý disku, který je nakonfigurovaný replikace má přidružený soubor HRL. Po dokončení počáteční replikace tento protokol odeslaný do úložiště účet zákazníka. Po protokol cestě k Azure se změnami primární sledují v jiném souboru protokolu ve stejném adresáři.

Během počáteční replikace nebo delta replikace můžete sledovat stav OM replikace v zobrazení OM podle [Sledování stavu replikace virtuálního počítače](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Synchronizace
Virtuální počítač je označen pro synchronizace při delta replikace selže i úplný počáteční replikace je náročné šířka pásma nebo čas. Třeba když velikost souboru HRL hromádek až 50 procent celkové velikosti disku, virtuálního počítače je označen pro synchronizace. Synchronizace minimalizuje množství dat odeslaných výpočetních součty virtuálního počítače disků zdrojové a cílové a posílání pouze rozdílu v síti.

Po dokončení synchronizace by měl pokračování normální delta replikace. Synchronizace můžete obnovit, dojde k výpadku sítě nebo jiné výpadku.

Ve výchozím nastavení automaticky naplánované synchronizace nakonfigurovaný tak, aby dojít mimo pracovní doby. Pokud virtuální počítač potřebuje ruční synchronizace, vyberte virtuální počítač z portálu Microsoft a klikněte na **synchronizovat**.

![Ruční synchronizace](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Synchronizace používá bloků algoritmus pevnou blok rozdělení zdrojové a cílové soubory do pevné bloky. Součty pro každý blok se vytvářejí a pak ve srovnání s zjistit, jaký bloky ze zdroje musí být použity byste do cíle.

### <a name="retry-logic"></a>Opakování použití logických operátorů
Existuje logika předdefinované opakování replikace chyby. Tato logika je možné rozdělit do dvou kategorií:

| Kategorie                  | Scénáře                                    |
|---------------------------|----------------------------------------------|
| Obnovit bez chyb     | Žádné opakování pokusu o odeslání. Stav replikace virtuálního počítače je **kritický**a požaduje zásah správce. Jako příklad lze uvést: <ul><li>Nefunkční řetězec virtuální pevný disk</li><li>Neplatný stav otevřené virtuálního počítače</li><li>Chyba ověřování sítě</li><li>Povolení chyby</li><li>Virtuální počítač, který není nalezen, v případě samostatný Hyper-V server</li></ul>|
| Zotavitelná chyba         | Opakování dojít každé interval replikace pomocí exponenciální zpět nepracovní, zvyšuje interval opakování od začátku prvním pokusu (1, 2, 4, 8, 10 minut). Pokud se chyba potrvají, opakujte každých 30 minut. Jako příklad lze uvést: <ul><li>Chyba sítě</li><li>Volného místa na disku</li><li>Nedostatku paměti</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Virtuální počítač Hyper-V protection a obnovení životního cyklu

![Virtuální počítač Hyper-V protection a obnovení životního cyklu](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Ostatní odkazy

- [Sledování a odstraňování případných problémů ochranu VMware VMM, Hyper-V a fyzické weby](./site-recovery-monitoring-and-troubleshooting.md)
- [Zastihnout for Microsoft Support](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Běžné chyby při obnovení webu Azure a jejich řešení](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
