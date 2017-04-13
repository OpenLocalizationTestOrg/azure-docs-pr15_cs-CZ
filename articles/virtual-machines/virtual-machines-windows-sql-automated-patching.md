<properties
    pageTitle="Automatické opravy pro SQL Server VMs (Správce zdrojů) | Microsoft Azure"
    description="Tento článek vysvětluje funkce Automatické opravy pro SQL Server virtuálních počítačích spuštěné v Azure pomocí Správce prostředků."
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
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatické opravy pro systém SQL Server ve virtuálních počítačích Azure (Správce zdrojů)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-automated-patching.md)
- [Klasický](virtual-machines-windows-classic-sql-automated-patching.md)

Automatické oprav zavádí údržby pro Azure virtuálního počítače serverem SQL Server. Automatické aktualizace lze nainstalovat jenom během této údržby. Pro systém SQL Server tento rescriction zaručuje, že aktualizace systému a všechny přidružené restartování objevit na nejlepší možné doby pro databázi. Automatické oprav závisí na [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu. Klasický verzi tohoto článku naleznete v tématu [Automatické opravy pro systém SQL Server Azure virtuálních počítačích Classic](virtual-machines-windows-classic-sql-automated-patching.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud chcete použít automatické opravy, zvažte následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows serveru 2012 R2

**SQL Server verze**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure Powershellu**:

- [Nainstalujte nejnovější příkazy Powershellu Azure](../powershell-install-configure.md) , pokud chcete nakonfigurovat automatické opravy pomocí prostředí PowerShell.

>[AZURE.NOTE] Automatické oprav závisí na rozšíření agenta IaaS SQL serveru. Aktuální obrázky z Galerie virtuálního počítače SQL přidejte tuto linku ve výchozím nastavení. Další informace najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které můžete konfigurovat pro automatické opravy. Postup skutečné konfigurace lišit podle toho, jestli používáte Azure portál nebo příkazy Powershellu Windows Azure.

|Nastavení|Možné hodnoty|Popis|
|---|---|---|
|**Automatické opravy**|Povolit nebo zakázat (zakázáno)|Zapne nebo vypne automatické opravy pro Azure virtuální počítač.|
|**Plán údržby**|Každý den, pondělí, úterý, středa, čtvrtek, pátek, sobota, neděle|Plán pro stažení a instalace aktualizací systému Windows, SQL Server a Microsoft virtuálního počítače.|
|**Údržby start hodinu**|0 až 24|Místní čas aktualizace virtuální počítač.|
|**Doba trvání okno údržby**|30 180|Počet minut povolen dokončete stažení a instalace aktualizací.|
|**Oprava kategorie**|Důležité|Kategorie aktualizace si stáhněte a nainstalujte.|

## <a name="configuration-in-the-portal"></a>Konfigurace na portálu
Portál Azure můžete používá ke konfiguraci automatické opravy chyb při vytváření nebo u existující VMs.

### <a name="new-vms"></a>Nové VMs
Portál Azure používá ke konfiguraci automatické opravy chyb při vytváření nové SQL Server virtuálního počítače v modelu nasazení Správce prostředků.

V **Nastavení systému SQL Server** zásuvné vyberte **Automatické opravy**. Následující Azure portálu snímek obrazovky znázorňuje zásuvné **SQL automatické opravy** .

![Automatické opravy SQL Azure portálu](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontext naleznete v tématu dokončení na [zřizování virtuálního počítače v Azure SQL serveru](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující VMs
Pro existující virtuálních počítačích SQL serveru vyberte virtuálního počítače SQL serveru. Vyberte část zásuvné **Nastavení** **Konfigurace systému SQL Server** .

![Automatické opravy SQL pro existující VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

V zásuvné **Konfigurace systému SQL Server** klikněte na tlačítko **Upravit** v automatické opravy oddíl.

![Nastavení automatické opravy SQL pro existující VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Až budete hotovi, klikněte na tlačítko **OK** v dolní části zásuvné **Konfigurace systému SQL Server** uložte provedené změny.

Chcete-li povolit automatické opravy poprvé, nakonfiguruje Azure SQL Server IaaS Agent na pozadí. Během této doby nemusí zobrazit portálu Azure, že je nakonfigurované tak automatické opravy. Počkejte několik minut Agent nainstalovaný, konfigurovat. Po, která vyjadřuje portálu Azure nové nastavení.

>[AZURE.NOTE] Můžete taky nakonfigurovat automatické opravy pomocí šablony. Další informace najdete v tématu [Azure rychlý úvod šablony pro automatické opravy](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Konfigurace se prostředí PowerShell

Po zajištění služby SQL OM, nakonfigurujte automatické opravy pomocí Powershellu.

V následujícím příkladu prostředí PowerShell slouží k nastavení automatické opravy na existující OM Server SQL. Příkaz **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** nakonfiguruje nové okno údržbu pro automatické aktualizace.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Založené na tomto příkladu, následující tabulka popisuje praktických efekt v cíli Azure OM:

|Parametr|Efekt|
|---|---|
|**DayOfWeek**|Každý čtvrtek nainstalovány opravy.|
|**MaintenanceWindowStartingHour**|Počáteční aktualizuje na 11:00 dop.|
|**MaintenanceWindowsDuration**|Opravy musí být nainstalovaný 120 minut. Podle času spuštění, musí postupovat podle 1:00 odp.|
|**PatchCategory**|Nastavení možná pouze pro tento parametr je **důležité**.|

Může trvat několik minut nainstalujte a nakonfigurujte IaaS Agent SQL serveru.

Zákaz automatické opravy, následujícím způsobem stejné bez **-Povolení** parametr **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Nepřítomnosti **-Povolení** parametr signalizuje příkazu zakázání funkce.

## <a name="next-steps"></a>Další kroky

Informace o dalších dostupné automatizaci úkolů najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-sql-server-agent-extension.md).

Podrobnosti o tom, jak na Azure VMs SQL serveru najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
