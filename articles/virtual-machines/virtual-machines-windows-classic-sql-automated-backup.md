<properties
    pageTitle="Automatické zálohování pro SQL Server virtuálních počítačích (klasický) | Microsoft Azure"
    description="Tento článek vysvětluje funkci automatického zálohování SQL serveru ve virtuálních počítačích Azure pomocí Správce prostředků. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure (klasický)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-automated-backup.md)
- [Klasický](virtual-machines-windows-classic-sql-automated-backup.md)

Automatické zálohování automaticky nakonfiguruje [Spravovaných zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající i nový databáze na OM Azure systém SQL Server 2014 Standard nebo Enterprise. Umožňuje konfigurace zálohování standardní databázi, které využívají trvalé úložišti objektů blob Azure. Automatické zálohování závisí na [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků verzi tohoto článku naleznete v tématu [Automatické zálohování pro systém SQL Server v Azure virtuálních počítačích správce](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Použití automatického zálohování, zvažte následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows serveru 2012 R2

**Verze/verzi serveru SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 nepodporuje ještě pro automatické zálohování.

**Konfigurace databáze**:

- Cílové databáze musí používat model úplné obnovení.

**Azure Powershellu**:

- [Nainstalujte nejnovější příkazy Powershellu Azure](../powershell-install-configure.md).

**Rozšíření IaaS serveru SQL**:

- [Instalace SQL serveru IaaS rozšíření](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které lze nastavit automatické zálohování. Pro klasické VMs musíte pomocí prostředí PowerShell tato nastavení konfigurujete takhle.

|Nastavení|Oblast (výchozí)|Popis|
|---|---|---|
|**Automatické zálohování**|Povolit nebo zakázat (zakázáno)|Zapne nebo vypne automatické zálohování pro Azure OM systém SQL Server 2014 Standard nebo Enterprise.|
|**Doba uchovávání informací**|1 – 30 dní (30 dní)|Počet dnů uchovávání zálohy.|
|**Úložiště účtu**|Azure úložiště (účtu úložiště vytvořené pro zadané OM)|Účet Azure úložiště chcete použít pro ukládání automatické záložních souborů v úložišti objektů blob. Kontejner se vytvoří v tomto umístění pro uložení všech záložních souborů. Pojmenování záložní soubor obsahuje datum, čas a název počítače.|
|**Šifrování**|Povolit nebo zakázat (zakázáno)|Povolí nebo zakáže šifrování. Při zapnuté funkci šifrování, certifikáty sloužící k obnovení zálohování najdete v zadané úložiště účtu ve stejném kontejneru automaticbackup pomocí stejného konvence. Pokud se změní heslo nový certifikát vytvořený pomocí toto heslo, ale zůstanou starý certifikát obnovit předchozí zálohy.|
|**Heslo**|Heslo text (žádné)|Heslo pro šifrovacího klíče. Jenom povinné, pokud je povoleno šifrování. Abyste mohli obnovit šifrované zálohování, musíte mít správné heslo a související certifikát, který byl použit v době, kdy vytvoření zálohy.|

## <a name="configuration-with-powershell"></a>Konfigurace se prostředí PowerShell

V následujícím příkladu prostředí PowerShell automatického zálohování nakonfigurovaný pro existující 2014 OM SQL serveru. Příkaz **Nový AzureVMSqlServerAutoBackupConfig** konfiguruje nastavení automatického zálohování pro ukládání záložní kopie v okně účet Azure úložiště určené proměnnou $storageaccount. Tyto zálohy se zachovají 10 dnů. Příkaz **Nastavení AzureVMSqlServerExtension** aktualizuje zadaný OM Azure pomocí těchto nastavení.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Může trvat několik minut nainstalujte a nakonfigurujte IaaS Agent SQL serveru.

Povolit šifrování, upravte předchozí skript k předání parametr EnableEncryption spolu s heslo (zabezpečené řetězec) pro parametr CertificatePassword. Tento skript umožňuje nastavení automatického zálohování v předchozím příkladu a přidá šifrování.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Zákaz automatického zálohování, následujícím způsobem stejné bez **-Povolení** parametr **AzureVMSqlServerAutoBackupConfig nový**. S instalací, může trvat několik minut zakázání automatického zálohování.

>[AZURE.NOTE] Zakázání a odinstalace SQL Server IaaS Agent neodeberete dříve konfigurované nastavení spravovaných zálohování. Automatické zálohování zakažte před zakázání nebo odinstalace IaaS Agent SQL serveru.

## <a name="next-steps"></a>Další kroky

Automatické zálohování konfiguruje Azure VMs spravovaných zálohování. To je důležité kontroloval [dokumentace k zálohování spravovaných](https://msdn.microsoft.com/library/dn449496.aspx) abyste pochopili důsledky a chování.

Vyhledání dalších zálohování a obnovení pokyny pro systém SQL Server na Azure VMs v tématu: [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších dostupné automatizaci úkolů najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-classic-sql-server-agent-extension.md).

Podrobnosti o tom, jak na Azure VMs SQL serveru najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
