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

## <a name="backup"></a>Zálohování

| Zálohování | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
|Zálohování|    Nelze provést operaci, dokud OM už existuje. -Vypnout ochrana virtuální počítač bez odstranění záložních dat. Další informace na http://go.microsoft.com/fwlink/?LinkId=808124   |Tím se stane, když se odstraní primární OM, ale zásady zálohování stále můžete vyhledávat OM k zálohování. Chcete-li opravit tuto chybu: <ol><li> Znovu vytvořit virtuální počítač se stejným názvem a stejný název skupiny prostředků [název služby cloudu],<br>(NEBO)</li><li> Vypnout chrání virtuálního počítače pomocí hesla nebo bez odstraňovat záložní data. [Další informace](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Zálohování|Nelze komunikovat s agentem OM snímek stavu. -Zajistěte OM přístup k Internetu. Agent OM aktualizoval taky, jak je uvedeno v Průvodci Poradce při potížích http://go.microsoft.com/fwlink/?LinkId=800034 |Pokud došlo k potížím s agentem OM nebo se sdílením Azure infrastruktury je blokován nějak vyvolá tato chyba. Přečtěte si, že informace o ladění nahoru OM snímku problémy.<br> Pokud agent OM nezpůsobuje všech problémů, restartujte OM. V některých případech nesprávném stavu OM může způsobovat problémy s a restartování OM obnoví tento "nevyhovujícím stavu"|
|Zálohování|    Obnovení služby rozšíření se nezdařila. – Zkontrolujte, že je k dispozici v počítači virtuální nejnovější agent virtuálního počítače a se službou agent. Opakovat zálohování a pokud se nezdaří, kontaktujte podporu od Microsoftu.|   Tato chyba se vyvolá, když OM agent není aktuální. V nápovědě "Aktualizace OM Agent" oddíl níž chcete aktualizovat agenta OM.|
|Zálohování |Virtuální počítač neexistuje. -Přesvědčte, že existuje virtuální počítače, nebo vyberte jiný virtuální počítač. | Tím se stane, když se odstraní primární OM, ale zásady zálohování stále můžete vyhledávat OM k zálohování. Chcete-li opravit tuto chybu: <ol><li> Znovu vytvořit virtuální počítač se stejným názvem a stejný název skupiny prostředků [název služby cloudu],<br>(NEBO)<br></li><li>Vypnout ochrana virtuální počítač bez odstranění záložních dat. [Další informace](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Zálohování |Spuštění příkazu se nezdařila. – U této položky právě probíhá další operace. Počkejte na dokončení předchozích a opakujte |Existující zálohování a obnovení úlohu OM běží a novou úlohu nemůže začít, je spuštěná existujícího projektu.|
| Zálohování | Kopírování VHD ze záložní trezoru vypršel časový limit - akci opakujte za několik minut. Pokud potíže potrvají, obraťte se na Microsoft Support. | K tomu dojde, pokud nejsou příliš mnoho zkopírovala. Zkontrolujte, jestli máte menší než 16 disků data. |
| Zálohování | Zálohování došlo k interní chybě: akci opakujte za několik minut. Pokud potíže potrvají, obraťte se na Microsoft Support | Tato chyba se může objevit 2 důvodů: <ol><li> Při přístupu k OM úložiště je přechodná problém. Zkontrolujte [Stav Azure](https://azure.microsoft.com/en-us/status/) a podívejte se, pokud je jakékoli probíhající otázky týkající se do výpočetního/úložiště/sítě v oblasti. Je nám prosím zmírnit opakovat problém záložní příspěvek. <li>Odstranil původní OM, a proto nelze přijata zálohování. Zachovat záložních dat pro odstraněné OM, ale zastavit záložní chyby, odemknout OM a zvolením možnosti zachovat data. Tím se zastaví plán zálohování a také opakované chybové zprávy. |
| Zálohování | Se nepodařilo nainstalovat služby Azure Recovery přípona s vybranou položkou - Agent OM je předpoklad rozšíření služby Azure obnovení. Nainstalujte agenta Azure OM a znovu spustit operaci registrace | <ol> <li>Zaškrtněte, pokud agent OM nainstalován správně. <li>Ujistěte se, že je správně nastavený příznak na konfigurace OM.</ol> [Přečtěte si další](#validating-vm-agent-installation) informace o instalaci agenta OM a jak ověřit instalaci agenta OM. |
| Zálohování | Rozšíření Instalace se nezdařila, zobrazí se chyba "COM + se nepodařilo mluvit koordinátorovi Microsoft Distributed transakce | Většinou to znamená, že není spuštěná služba COM +. Kontaktujte podporu od Microsoftu nápovědu k řešení tohoto problému. |
| Zálohování | Snímek se nezdařil Chyba operace VSS "disku je uzamčený BitLocker Drive šifrování. Je třeba odemknout tuto jednotku pomocí ovládacích panelů. | Vypnutí nástroje BitLocker pro všechny jednotky OM a sledujte, pokud je VSS problém vyřešit |
| Zálohování | Virtuálních počítačích s virtuální pevných discích uložených v úložišti Premium nejsou podporované pro zálohování | Žádná |
| Zálohování | Azure virtuálního počítače nebyl nalezen. | Tím se stane, když se odstraní primární OM, ale zásady zálohování stále můžete vyhledávat OM k zálohování. Chcete-li opravit tuto chybu: <ol><li>Znovu vytvořit virtuální počítač se stejným názvem a stejný název skupiny prostředků [název služby cloudu], <br>(NEBO) <li> Zakázání ochrany pro tento OM tak, aby se nevytvoří úloh zálohování </ol> |
| Zálohování | Agent virtuální počítač není k dispozici v počítači virtuální – nainstalujte povinný před požadavek OM agent a znovu spustit operaci. | [Přečtěte si další](#vm-agent) informace o instalaci agenta OM a jak ověřit instalaci agenta OM. |

## <a name="jobs"></a>Úlohy

| Operace | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Zrušení úlohy | Zrušení není podporována pro tento typ práce: Počkejte, až se dokončí úloha. | Žádná |
| Zrušení úlohy | Úlohy není v vzdálené stavu: Počkejte, až se dokončí úloha. <br>NEBO<br> Vybraného úkolu není možné zrušit stav: Počkejte na dokončení úlohy.| Ve všech pravděpodobnost téměř dokončení úlohy; Počkejte, až se dokončí úloha |
| Zrušení úlohy | Úlohu nelze zrušit, protože není v průběhu - zrušení je podporována pouze pro projekty, které jsou v průběhu. Zrušte pokus o v průběhu projektu. | K tomu dojde kvůli přechodně stavu. Chvíli počkejte a zkuste operaci zrušit |
| Zrušení úlohy | Zrušíte - se nepodařilo Počkejte, dokud dokončení projektu. | Žádná |


## <a name="restore"></a>Obnovení
| Operace | Podrobnosti o chybě | Řešení: |
| -------- | -------- | -------|
| Obnovení | Obnovení skončil cloudu vnitřní chyba | <ol><li>Cloudové služby, do kterého chcete obnovit nakonfigurovaný s nastavením DNS. Můžete zkontrolovat <br>$deployment = get-AzureDeployment - ServiceName "Název_služby"-úsek "Výrobní" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Pokud existuje adresa nakonfigurovaná, to znamená, že jsou nakonfigurované tak nastavení DNS.<br> <li>Cloudové služby, na kterou se snažíte obnovit nakonfigurována ReservedIP a existující VMs do cloudové služby jsou přestal stavu.<br>Můžete zkontrolovat, že do cloudové služby rezervoval IP pomocí těchto rutin prostředí powershell:<br>$deployment = get-AzureDeployment - ServiceName "název_služby"-Zabránění spuštění dat. úsek "Výrobní" $ ReservedIPName <br><li>Snažíte se obnovit virtuální stroje s následující zvláštní síťové konfigurace ve stejné cloudové služby. <br>-Virtuálních počítačích v části Konfigurace služby Vyrovnávání zatížení (interní a externí)<br>-Virtuálních počítačích s více rezervovaná IP adresy<br>-Virtuálních počítačích s více nic<br>Vyberte nový cloudové služby v uživatelském rozhraní nebo naleznete [obnovení aspektech](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) VMs s konfigurací zvláštní sítě</ol> |
| Obnovení | Vybrané jméno DNS již používá – zadejte jiný název DNS a zkuste to znovu. | Název DNS odkazuje na název cloudové služby (obvykle končící znakem. cloudapp.net). To musí být jedinečný. Pokud k této chybě dojde, musíte zvolit jiný název OM během obnovení. <br><br> Všimněte si, že tato chyba se zobrazuje jenom uživatelům Azure portálu. Obnovení prostřednictvím Powershellu proběhne úspěšně, protože pouze obnoví disků a nevytvoří OM. Chyba se před vytvoření OM explicitně vy po obnovení disku. |
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

## <a name="troubleshoot-vm-snapshot-issues"></a>Poradce při potížích snímek OM
Zálohování OM závisí na vystavení příkaz snímek pro základní úložiště. Nemají přístup k ukládání nebo zpoždění spuštění úkolu snímku může selhat zálohování. Následující mohou způsobit selhání úlohy snímek.

1. Přístup k síti k základnímu úložišti je blokován pomocí NSG<br>
   Další informace o tom, jak [Povolit přístup k síti](backup-azure-vms-prepare.md#2-network-connectivity) k základnímu úložišti pomocí některého z povolení programů z IP adresy nebo prostřednictvím proxy serveru.
2.  VMs pomocí zálohování serveru Sql Server nakonfigurovaný může způsobovat zpoždění úkolu snímku <br>
    Ve výchozím nastavení OM zálohujte problémy VSS úplné zálohy systému Windows VMs. Na VMs, které jsou servery Sql a Sql Server nakonfigurovaný zálohování to může způsobit zpoždění spuštění snímek. Nastavte následující klíč registru, pokud jste zaznamenali záložní selhání kvůli problémům se snímek.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  Stav OM nesprávně protože OM vypnutí RDP.  <br>
    Pokud jste ukončili virtuálního počítače v RDP, přejděte prosím vrátit se změnami zpět na portálu OM stav se projeví správně. V opačném případě vypněte OM portálu možnost "Vypnutí" na řídicím panelu OM.
4.  Pokud více než čtyři OM sdílet stejné cloudové služby, nakonfigurujte více záložní zásad připravení záložní doby tak bez, nejvýše čtyři zálohy OM spuštění ve stejnou dobu. Zkuste rozprostřete pracovní doby hodiny od sebe mezi zásady záložní spuštění. 
5.  OM běží na vysoké procesoru a paměti.<br>
    Pokud virtuální počítač běží na vysoké procesoru usage(>90%) nebo paměti, snímek úkolu ve frontě, posunout a nakonec získá vypršení časového limitu. Zkuste zálohování na vyžádání v těchto situacích.

<br>

## <a name="networking"></a>Sítě
Stejně jako všechna rozšíření zálohování rozšíření potřebují mít přístup k veřejnému Internetu pro práci. Nemají přístup k Internetu veřejné se může projevit různými způsoby:

- Instalace rozšíření může selhat
- Může selhat zálohování (ve tvaru diskové snímek)
- Zobrazení stavu zálohování může selhat

Byly kloubové nutnosti řešení veřejné internetové adresy [tady](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Bude muset zkontrolujte konfigurace DNS pro VNET a ujistěte se, že URI Azure může být přeložena.

Překlad po dokončení správně přístup k IP adresy Azure také musí být k dispozici. Odblokovat přístup k Azure infrastruktury, postupujte jedním z těchto kroků:

1. Povolených Azure datacentra rozsahy IP adres.
    - Získání seznamu [Azure datacentra IP adresy](https://www.microsoft.com/download/details.aspx?id=41653) být povolené.
    - Blokování IP adresy pomocí rutinu [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) . Spusťte tuto rutinu v rámci OM Azure v okno zvýšenými oprávněními prostředí PowerShell (Spustit jako správce).
    - Přidání pravidla do NSG (Pokud máte v místě) umožňuje přístup k IP adresy.
2. Vytvořením cesty pro přenos HTTP plovoucí dlaždice
    - Pokud máte některá omezení síti na místě (síť skupinu zabezpečení, například) nasazení serveru HTTP proxy serveru směrovat přenosy. Kroky pro nasazení serveru Proxy pro HTTP naleznete [v tomto poli](backup-azure-vms-prepare.md#2-network-connectivity).
    - Přidání pravidla do NSG (Pokud máte v místě) povolíte přístup k Internetu z Proxy pro HTTP.

>[AZURE.NOTE] DHCP musí být povolený uvnitř hosty pro zálohování OM IaaS pracovat.  Pokud potřebujete statickou IP soukromé, nastavte ho prostřednictvím platformy. Možnost DHCP uvnitř OM vlevo používat.
Zobrazte další informace o [Nastavení statické IP interní soukromé](../virtual-network/virtual-networks-reserved-private-ip.md).
