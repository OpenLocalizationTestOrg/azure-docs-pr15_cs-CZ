<properties
    pageTitle="Poradce při potížích s Azure virtuálního počítače zálohování | Microsoft Azure"
    description="Poradce při potížích s zálohování a obnovení Azure virtuálních počítačích"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Poradce při potížích s Azure virtuálního počítače zálohování

> [AZURE.SELECTOR]
- [Obnovení služby trezoru](backup-azure-vms-troubleshoot.md)
- [Zálohování trezoru](backup-azure-vms-troubleshoot-classic.md)

Poradce při potížích chyby při zálohování Azure pomocí informace uvedené v následující tabulce.

## <a name="discovery"></a>Zjišťování

| Zálohování | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Zjišťování | Objevte nové položky - Microsoft Azure zálohování narazili a interní chybě se nepovedlo. Počkejte několik minut a potom akci opakujte. | Opakujte proces zjišťování po 15 minut.
| Zjišťování | Objevování nových položek – se nepodařilo jiné zjišťování operace probíhá. Počkejte, dokud nebude aktuální zjišťování po dokončení. | Žádná |

## <a name="register"></a>Registrace
| Zálohování | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Registrace | Počet disků dat připojených k virtuální počítač překročení podporovaný – zkontrolujte odpojení disků některá data na tomto počítači virtuální a opakujte. Azure zálohování podporuje až 16 disků dat připojených k Azure virtuálního počítače pro zálohování | Žádná |
| Registrace | Microsoft Azure zálohování došlo k interní chybě - počkejte pár minut a zkuste akci. Pokud potíže potrvají, obraťte se na Microsoft Support. | K této chybě dojít kvůli jednu z následujících Nepodporovaná konfigurace OM na Premium LRS. <br> Úložiště Premium VMs zálohovat lze pomocí obnovení služby trezoru. [Víc se uč](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Registrace | Registrace skončil časový limit operace nainstalovat Agent | Zkontrolujte, zda je podporován operační systém verze virtuálního počítače. |
| Registrace | Příkaz spuštění se nezdařila – další operace probíhá u této položky. Počkejte, dokud nebude dokončena předchozí operace | Žádná |
| Registrace | Virtuálních počítačích s virtuální pevných discích uložených v úložišti Premium nejsou podporované pro zálohování | Žádná |
| Registrace | Agent virtuální počítač není k dispozici v počítači virtuální – nainstalujte povinný před požadavek OM agent a znovu spustit operaci. | [Přečtěte si další](#vm-agent) informace o instalaci agenta OM a jak ověřit instalaci agenta OM. |

## <a name="backup"></a>Zálohování

| Zálohování | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Zálohování | Nelze komunikovat s agentem OM snímek stavu. Snímek OM dílčí úkol vypršení časového limitu. -Najdete Průvodce pro řešení potíží o tom, jak vyřešit. | Pokud došlo k potížím s agentem OM nebo se sdílením Azure infrastruktury je blokován nějak vyvolá tato chyba. Další informace o [ladění nahoru OM snímek problémy](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Pokud agent OM nezpůsobuje všech problémů, restartujte OM. V některých případech nesprávném stavu OM může způsobovat problémy s a restartování OM obnoví tento "nevyhovujícím stavu" |
| Zálohování | Zálohování došlo k interní chybě: akci opakujte za několik minut. Pokud potíže potrvají, obraťte se na Microsoft Support | Zkontrolujte, jestli není při přístupu k OM úložiště přechodná problém. Zkontrolujte [Stav Azure](https://azure.microsoft.com/en-us/status/) a podívejte se, pokud je jakékoli probíhající otázky týkající se do výpočetního/úložiště/sítě v oblasti. Je nám prosím zmírnit opakovat problém záložní příspěvek. |
| Zálohování | Nelze provést operaci, dokud OM už existuje. | Zálohování nelze provést, protože byl odstraněn OM nakonfigurován pro zálohování. Ukončení další zálohování tak, že přejdete do chráněného zobrazení položky, vyberte položku chráněné a klikněte na odemknout. Výběrem možnosti zachovat zálohování dat můžete zachovat data. Později můžete vrátit ochranu pro tento virtuální počítač kliknutím na konfigurovat ochranu ze zobrazení registrovaných položky|
| Zálohování | Se nepodařilo nainstalovat služby Azure Recovery přípona s vybranou položkou - Agent OM je předpoklad rozšíření služby Azure obnovení. Nainstalujte agenta Azure OM a znovu spustit operaci registrace | <ol> <li>Zaškrtněte, pokud agent OM nainstalován správně. <li>Ujistěte se, že je správně nastavený příznak na konfigurace OM.</ol> [Přečtěte si další](#validating-vm-agent-installation) informace o instalaci agenta OM a jak ověřit instalaci agenta OM. |
| Zálohování | Příkaz spuštění se nezdařila – další operace právě probíhá u této položky. Počkejte na dokončení předchozích a opakujte | Existující zálohování a obnovení úlohu OM běží a novou úlohu nemůže začít, je spuštěná existujícího projektu. |
| Zálohování | Rozšíření Instalace se nezdařila, zobrazí se chyba "COM + se nepodařilo mluvit koordinátorovi Microsoft Distributed transakce | Většinou to znamená, že není spuštěná služba COM +. Kontaktujte podporu od Microsoftu nápovědu k řešení tohoto problému. |
| Zálohování | Snímek se nezdařil Chyba operace VSS "disku je uzamčený BitLocker Drive šifrování. Je třeba odemknout tuto jednotku pomocí ovládacích panelů. | Vypnutí nástroje BitLocker pro všechny jednotky OM a sledujte, pokud je VSS problém vyřešit |
| Zálohování | Virtuálních počítačích s virtuální pevných discích uložených v úložišti Premium nejsou podporované pro zálohování | Žádná |
| Zálohování | Azure virtuálního počítače nebyl nalezen. | Tím se stane, když se odstraní primární OM, ale zásady zálohování stále můžete vyhledávat OM k zálohování. Chcete-li opravit tuto chybu: <ol><li>Znovu vytvořit virtuální počítač se stejným názvem a stejný název skupiny prostředků [název služby cloudu], <br>(NEBO) <li> Zakážete ochranu tento OM, takže nebude získat spouštěný pozdější zálohování. </ol> |
| Zálohování | Agent virtuální počítač není k dispozici v počítači virtuální – nainstalujte povinný před požadavek OM agent a znovu spustit operaci. | [Přečtěte si další](#vm-agent) informace o instalaci agenta OM a jak ověřit instalaci agenta OM. |

## <a name="jobs"></a>Úlohy
| Operace | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Zrušení úlohy | Zrušení není podporována pro tento typ práce: Počkejte, až se dokončí úloha. | Žádná |
| Zrušení úlohy | Úlohy není v vzdálené stavu: Počkejte, až se dokončí úloha. <br>NEBO<br> Vybraného úkolu není v vzdálené stavu: Počkejte na dokončení úlohy.| Ve všech pravděpodobnost téměř dokončení úlohy; Počkejte, až se dokončí úloha |
| Zrušení úlohy | Úlohu nelze zrušit, protože není v průběhu - zrušení je podporována pouze pro projekty, které jsou v průběhu. Zrušte pokus o v průběhu projektu. | K tomu dojde kvůli přechodně stavu. Chvíli počkejte a zkuste operaci zrušit |
| Zrušení úlohy | Zrušíte - se nepodařilo počkejte na dokončení projektu. | Žádná |


## <a name="restore"></a>Obnovení
| Operace | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Obnovení | Obnovení skončil cloudu vnitřní chyba | <ol><li>Cloudové služby, do kterého chcete obnovit nakonfigurovaný s nastavením DNS. Můžete zkontrolovat <br>$deployment = get-AzureDeployment - ServiceName "Název_služby"-úsek "Výrobní" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Pokud existuje adresa nakonfigurovaná, to znamená, že jsou nakonfigurované tak nastavení DNS.<br> <li>Cloudové služby, na kterou se snažíte obnovit nakonfigurována ReservedIP a existující VMs do cloudové služby jsou přestal stavu.<br>Můžete zkontrolovat, že do cloudové služby rezervoval IP pomocí těchto rutin prostředí powershell:<br>$deployment = get-AzureDeployment - ServiceName "název_služby"-Zabránění spuštění dat. úsek "Výrobní" $ ReservedIPName <br><li>Snažíte se obnovení virtuálního počítače pomocí následujících speciálních síťových konfigurací v stejné cloudové služby. <br>-Virtuálních počítačích v části Konfigurace služby Vyrovnávání zatížení (interní a externí)<br>-Virtuálních počítačích s více rezervovaná IP adresy<br>-Virtuálních počítačích s více nic<br>Vyberte nový cloudové služby v uživatelském rozhraní nebo naleznete [obnovení aspektech](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) VMs s konfigurací zvláštní sítě</ol> |
| Obnovení | Vybrané jméno DNS již používá – zadejte jiný název DNS a zkuste to znovu. | Název DNS odkazuje na název cloudové služby (obvykle končící znakem. cloudapp.net). To musí být jedinečný. Pokud k této chybě dojde, musíte zvolit jiný název OM během obnovení. <br><br> Tato chyba se zobrazuje jenom uživatelům Azure portálu. Obnovení prostřednictvím Powershellu úspěšná, protože pouze obnoví disků a nevytvoří OM. Chyba se před vytvoření OM explicitně vy po obnovení disku. |
| Obnovení | Konfigurace zadaný virtuální sítě není správného – zadejte konfigurace různých virtuální sítě a zkuste to znovu. | Žádná |
| Obnovení | Zadaný cloudovou službu používá rezervovaná IP, který není shodovat s nastavením počítače virtuální obnovována - určete jiné cloudové služby, který nepoužívá User nebo zvolte jiné obnovení, přejděte na Obnovit z. | Žádná |
| Obnovení | Cloudová služba dosáhl omezení počtu vstupní koncový bod: opakujte zadáním jiné cloudové službě nebo můžete použít existující koncový bod. | Žádná |
| Obnovení | Zálohování trezoru a cílové účtu úložiště jsou ve dvou různých oblastí: Zajistěte, aby byl podle obnovení účtu úložiště ve stejné oblasti Azure jako záložní trezoru. | Žádná |
| Obnovení | Účet úložiště určený pro obnovení není podporované - pouze základní/standardní úložiště účty s místně nadbytečné nebo nastavení nadbytečné replikace geo podporují. Vyberte účet podporované úložiště | Žádná |
| Obnovení | Typ účtu úložiště určeným pro obnovení není online – zkontrolujte, že účet úložiště podle obnovení je online | K tomu může dojít kvůli přechodná chyby v úložišti Azure nebo kvůli výpadku. Vyberte jiný účet úložiště. |
| Obnovení | Překročení kvóty pole Skupina zdroje – odstraňte některé skupiny zdrojů z Azure portálu nebo kontaktujte podporu Azure zvětšíte limity. | Žádná |
| Obnovení | Vybrané podsítě neexistuje – vyberte podsítě, který existuje | Žádná |


## <a name="policy"></a>Zásady
| Operace | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Vytvoření zásad | Nepodařilo se vytvořit zásady - zmenšete možností pokračujte konfigurace zásad uchovávání informací. | Žádná |


## <a name="vm-agent"></a>Agent OM

### <a name="setting-up-the-vm-agent"></a>Nastavení agenta OM
Obvykle agenta OM již je v VMs vytvořené v galerii Azure. Však virtuálních počítačích, které se migrují od místního datacentrech neměli agenta OM nainstalovaný. U těchto VMs agenta OM potřeba nainstalovat explicitně. Další informace o [instalaci na existující OM agenta OM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Pro Windows VMs:

- Stáhněte a nainstalujte [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Budete potřebovat oprávnění správce a dokončete instalaci.
- Vyznačení, že je nainstalovaný agent [Aktualizovat vlastnost OM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) .

Pro Linux VMs:

- Nainstalujte nejnovější [Linux agent](https://github.com/Azure/WALinuxAgent) z github.
- Vyznačení, že je nainstalovaný agent [Aktualizovat vlastnost OM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) .


### <a name="updating-the-vm-agent"></a>Aktualizace agenta OM
Pro Windows VMs:

- Aktualizace agenta OM je jednoduchá – stačí přeinstalace [OM Agent binární](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Však je třeba zajistit, že žádný zálohování běží době, kdy je aktualizován agenta OM.

Pro Linux VMs:

- Podle pokynů na [Aktualizace Agent OM Linux](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Ověřování instalaci OM agenta
Jak se dá zjistit verzi OM Agent na Windows VMs:

1. Přihlaste se k Azure virtuálního počítače a přejděte do složky *C:\WindowsAzure\Packages*. Nenajdete prezentovat WaAppAgent.exe soubor.
2. Klikněte pravým tlačítkem myši na soubor, přejděte na **Vlastnosti**a pak klikněte na kartu **Podrobnosti** . Pole verze produktu by měl být 2.6.1198.718 nebo vyšší





