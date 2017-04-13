<properties
    pageTitle="Automatické zálohování pro SQL Server virtuálních počítačích (Správce zdrojů) | Microsoft Azure"
    description="Tento článek vysvětluje funkci automatického zálohování SQL serveru ve virtuálních počítačích Azure pomocí Správce prostředků. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure (Správce zdrojů)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-automated-backup.md)
- [Klasický](virtual-machines-windows-classic-sql-automated-backup.md)

Automatické zálohování automaticky nakonfiguruje [Spravovaných zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající i nový databáze na OM Azure systém SQL Server 2014 Standard nebo Enterprise. Umožňuje konfigurace zálohování standardní databázi, které využívají trvalé úložišti objektů blob Azure. Automatické zálohování závisí na [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu. Klasický verzi tohoto článku naleznete v tématu [Automatické zálohování pro systém SQL Server Azure virtuálních počítačích Classic](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Použití automatického zálohování, zvažte následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows serveru 2012 R2

**Verze/verzi serveru SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Konfigurace databáze**:

- Cílové databáze musí používat model úplné obnovení

**Azure Powershellu**:

- [Nainstalujte nejnovější příkazy Powershellu Azure](../powershell-install-configure.md) , pokud máte v plánu pro nastavení automatického zálohování pomocí prostředí PowerShell.

>[AZURE.NOTE] Automatické zálohování závisí na rozšíření agenta IaaS SQL serveru. Aktuální obrázky z Galerie virtuálního počítače SQL přidejte tuto linku ve výchozím nastavení. Další informace najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které lze nastavit automatické zálohování. Postup skutečné konfigurace lišit podle toho, jestli používáte Azure portál nebo příkazy Powershellu Windows Azure.

|Nastavení|Oblast (výchozí)|Popis|
|---|---|---|
|**Automatické zálohování**|Povolit nebo zakázat (zakázáno)|Zapne nebo vypne automatické zálohování pro Azure OM systém SQL Server 2014 Standard nebo Enterprise.|
|**Doba uchovávání informací**|1 – 30 dní (30 dní)|Počet dnů uchovávání zálohy.|
|**Úložiště účtu**|Azure úložiště (účtu úložiště vytvořené pro zadané OM)|Účet Azure úložiště chcete použít pro ukládání automatické záložních souborů v úložišti objektů blob. Kontejner se vytvoří v tomto umístění pro uložení všech záložních souborů. Pojmenování záložní soubor obsahuje datum, čas a název počítače.|
|**Šifrování**|Povolit nebo zakázat (zakázáno)|Povolí nebo zakáže šifrování. Při zapnuté funkci šifrování, certifikáty sloužící k obnovení zálohování najdete v zadané úložiště účtu ve stejném kontejneru automaticbackup pomocí stejného konvence. Pokud se změní heslo nový certifikát vytvořený pomocí toto heslo, ale zůstanou starý certifikát obnovit předchozí zálohy.|
|**Heslo**|Heslo text (žádné)|Heslo pro šifrovacího klíče. Jenom povinné, pokud je povoleno šifrování. Abyste mohli obnovit šifrované zálohování, musíte mít správné heslo a související certifikát, který byl použit v době, kdy vytvoření zálohy.|

## <a name="configuration-in-the-portal"></a>Konfigurace na portálu
Na portálu Azure můžete použít pro nastavení automatického zálohování během vytváření nebo u existující VMs.

### <a name="new-vms"></a>Nové VMs
Pomocí portálu Azure pro nastavení automatického zálohování při vytváření nové 2014 virtuálního počítače serveru SQL v modelu nasazení Správce prostředků.

V **Nastavení systému SQL Server** zásuvné vyberte **Automatické zálohování**. Následující Azure portálu snímek obrazovky znázorňuje zásuvné **SQL automatického zálohování** .

![Konfigurace automatického zálohování SQL Azure portálu](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontext naleznete v tématu dokončení na [zřizování virtuálního počítače v Azure SQL serveru](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující VMs
Pro existující virtuálních počítačích SQL serveru vyberte virtuálního počítače SQL serveru. Vyberte část zásuvné **Nastavení** **Konfigurace systému SQL Server** .

![Automatické zálohy SQL pro existující VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

V zásuvné **Konfigurace systému SQL Server** klikněte na tlačítko **Upravit** v části automatické zálohy.

![Nastavení automatického zálohování SQL pro existující VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Až budete hotovi, klikněte na tlačítko **OK** v dolní části zásuvné **Konfigurace systému SQL Server** uložte provedené změny.

Chcete-li povolit automatické zálohování poprvé, nakonfiguruje Azure SQL Server IaaS Agent na pozadí. Během této doby nemusí zobrazit portálu Azure, že je nakonfigurovaný automatického zálohování. Počkejte několik minut Agent nainstalovaný, konfigurovat. Až to projeví portálu Azure nové nastavení.

>[AZURE.NOTE] Můžete taky nakonfigurovat automatické zálohy pomocí šablony. Další informace najdete v tématu [Azure rychlý úvod šablony pro automatické zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfigurace se prostředí PowerShell

Po zajištění služby SQL OM, pomocí prostředí PowerShell pro nastavení automatického zálohování.

V následujícím příkladu prostředí PowerShell automatického zálohování nakonfigurovaný pro existující 2014 OM SQL serveru. Příkaz **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** konfiguruje nastavení automatického zálohování ukládat zálohy v Azure úložiště účet spojený s virtuální počítač. Tyto zálohy se zachovají 10 dnů. Příkaz **Nastavení AzureRmVMSqlServerExtension** aktualizuje zadaný OM Azure pomocí těchto nastavení.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Může trvat několik minut nainstalujte a nakonfigurujte IaaS Agent SQL serveru.

Povolit šifrování, upravte předchozí skript k předání parametr **EnableEncryption** spolu s heslo (zabezpečené řetězec) pro parametr **CertificatePassword** . Tento skript umožňuje nastavení automatického zálohování v předchozím příkladu a přidá šifrování.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Zákaz automatického zálohování, následujícím způsobem stejné bez **-Povolení** parametr příkazu **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** . Nepřítomnosti **-Povolení** parametr signalizuje příkazu zakázání funkce. S instalací, může trvat několik minut zakázání automatického zálohování.

>[AZURE.NOTE] Odebrání SQL Server IaaS Agent neodebere dříve konfigurované nastavení automatického zálohování. Automatické zálohování zakažte před zakázání nebo odinstalace IaaS Agent SQL serveru.

## <a name="next-steps"></a>Další kroky

Automatické zálohování konfiguruje Azure VMs spravovaných zálohování. To je důležité kontroloval [dokumentace k zálohování spravovaných](https://msdn.microsoft.com/library/dn449496.aspx) abyste pochopili důsledky a chování.

Vyhledání dalších zálohování a obnovení pokyny pro systém SQL Server na Azure VMs v tématu: [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších dostupné automatizaci úkolů najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

Podrobnosti o tom, jak na Azure VMs SQL serveru najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
