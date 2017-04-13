<properties
    pageTitle="Automatické opravy pro SQL Server VMs (klasický) | Microsoft Azure"
    description="Tento článek vysvětluje funkci Automatické opravy pro SQL Server virtuálních počítačích spuštěné v Azure použití režimu klasické nasazení."
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

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatické opravy pro systém SQL Server ve virtuálních počítačích Azure (klasický)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-automated-patching.md)
- [Klasický](virtual-machines-windows-classic-sql-automated-patching.md)

Automatické oprav zavádí údržby pro Azure virtuálního počítače serverem SQL Server. Automatické aktualizace lze nainstalovat jenom během této údržby. Pro systém SQL Server zajistíte tak, že aktualizace systému a všechny přidružené restartování objevit na nejlepší možné doby pro databázi. Automatické oprav závisí na [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků verzi tohoto článku naleznete v tématu [Automatické opravy pro systém SQL Server v Azure virtuálních počítačích správce](virtual-machines-windows-sql-automated-patching.md).

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

- [Nainstalujte nejnovější příkazy Powershellu Azure](../powershell-install-configure.md).

**Rozšíření IaaS serveru SQL**:

- [Instalace SQL serveru IaaS rozšíření](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které můžete konfigurovat pro automatické opravy. Pro klasické VMs musíte pomocí prostředí PowerShell tato nastavení konfigurujete takhle.

|Nastavení|Možné hodnoty|Popis|
|---|---|---|
|**Automatické opravy**|Povolit nebo zakázat (zakázáno)|Zapne nebo vypne automatické opravy pro Azure virtuální počítač.|
|**Plán údržby**|Každý den, pondělí, úterý, středa, čtvrtek, pátek, sobota, neděle|Plán pro stažení a instalace aktualizací systému Windows, SQL Server a Microsoft virtuálního počítače.|
|**Údržby start hodinu**|0 až 24|Místní čas aktualizace virtuální počítač.|
|**Doba trvání okno údržby**|30 180|Počet minut povolen dokončete stažení a instalace aktualizací.|
|**Oprava kategorie**|Důležité|Kategorie aktualizace si stáhněte a nainstalujte.|

## <a name="configuration-with-powershell"></a>Konfigurace se prostředí PowerShell

V následujícím příkladu prostředí PowerShell slouží k nastavení automatické opravy na existující OM Server SQL. Příkaz **Nový AzureVMSqlServerAutoPatchingConfig** nakonfiguruje nové okno údržbu pro automatické aktualizace.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Založené na tomto příkladu, následující tabulka popisuje praktických efekt v cíli Azure OM:

|Parametr|Efekt|
|---|---|
|**DayOfWeek**|Každý čtvrtek nainstalovány opravy.|
|**MaintenanceWindowStartingHour**|Počáteční aktualizuje na 11:00 dop.|
|**MaintenanceWindowsDuration**|Opravy musí být nainstalovaný 120 minut. Podle času spuštění, musí postupovat podle 1:00 odp.|
|**PatchCategory**|Nastavení možná pouze pro tento parametr je "Důležité".|

Může trvat několik minut nainstalujte a nakonfigurujte IaaS Agent SQL serveru.

Pokud chcete zakázat automatické opravy chyb, spusťte stejné skriptu bez povolit parametru - AzureVMSqlServerAutoPatchingConfig nový. S instalací, může trvat několik minut zakázat automatické opravy chyb.

## <a name="next-steps"></a>Další kroky

Informace o dalších dostupné automatizaci úkolů najdete v článku [Rozšíření agenta IaaS SQL serveru](virtual-machines-windows-classic-sql-server-agent-extension.md).

Podrobnosti o tom, jak na Azure VMs SQL serveru najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
