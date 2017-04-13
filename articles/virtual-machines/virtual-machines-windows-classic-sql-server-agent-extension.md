<properties
    pageTitle="Rozšíření agenta SQL serveru pro SQL Server VMs (klasický) | Microsoft Azure"
    description="Toto téma popisuje, jak spravovat agent rozšíření serveru SQL Server, které umožňuje automatizovat konkrétní úlohy správy systému SQL Server. Jedná se o automatické zálohování automatické opravy a integrace trezoru klíč Azure. Toto téma používá režim klasické nasazení."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>Rozšíření agenta SQL serveru pro SQL Server VMs (klasický)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasický](virtual-machines-windows-classic-sql-server-agent-extension.md)

Rozšíření agenta IaaS SQL Server (SQLIaaSAgent) je spuštěn Azure virtuálních počítačích k automatizaci úkolů správy. Toto téma obsahuje přehled o nepodporuje příponu, stejně jako pokyny k instalaci, stav a odebrání služby.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků verzi tohoto článku zobrazíte najdete v článku [Rozšíření agenta SQL serveru pro SQL Server VMs správce prostředků](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Podporované služby

Rozšíření agenta IaaS SQL Server podporuje následující úlohy správy:

| Funkce správy | Popis |
|---------------------|-------------------------------|
| **SQL automatické zálohování** | Umožňuje automatizovat plánování zálohování pro všechny databáze pro výchozí instanci systému SQL Server v OM. Další informace najdete v tématu [Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure (klasický)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automatické opravy** | Konfiguruje údržby během níž aktualizace vaší OM může proběhnout, abyste neměli aktualizace špičce pro vaše pracovní zátěž. Další informace najdete v tématu [Automatické opravy pro systém SQL Server ve virtuálních počítačích Azure (klasický)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Integrace Azure klíčové trezoru** | Umožňuje automaticky nainstalujte a nakonfigurujte Azure klíč trezoru na vaše OM SQL serveru. Další informace najdete v tématu [Konfigurace Azure klíč trezoru integrace pro systém SQL Server na Azure VMs (klasický)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Zjistit předpoklady pro

Požadavky na vaše OM používat rozšíření agenta IaaS Server SQL:

### <a name="operating-system"></a>Operační systém:

- Windows Server 2012
- Windows serveru 2012 R2

### <a name="sql-server-versions"></a>SQL Server verze:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure Powershellu:

[Stáhněte si a konfigurace nejnovější příkazy Powershellu Azure](../powershell-install-configure.md).

Spusťte Windows PowerShell a připojte ji k předplatnému Azure pomocí příkazu **Přidat AzureAccount** .

    Add-AzureAccount

Pokud máte víc předplatných, pomocí kláves **Vyberte AzureSubscription** vyberte předplatné, které obsahuje cílový klasické OM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

V tomto okamžiku můžete získat seznam klasické virtuálních počítačích a jejich jména přidružené služby pomocí příkazu **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Instalace

Pro klasické VMs musíte se pomocí prostředí PowerShell nainstalovat SQL Server IaaS Agent linky a konfigurace přidružené služby. Pomocí rutiny prostředí PowerShell **Set-AzureVMSqlServerExtension** nainstalujte rozšíření. Následující příkaz například nainstaluje rozšíření OM serveru Windows (klasický) a pojmenuje ji "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Při aktualizaci na nejnovější verzi rozšíření agenta IaaS SQL, po restartování počítače virtuální po aktualizaci rozšíření.

>[AZURE.NOTE] Klasický virtuálních počítačích nemají možnost instalace a konfigurace rozšíření agenta IaaS SQL pomocí portálu.

## <a name="status"></a>Stav

Jedním ze způsobů k ověření, že je nainstalovaný rozšíření je zobrazení stavu agent v portálu Azure. Vyberte **všechna nastavení** v zásuvné virtuálního počítače a potom klepněte na **rozšíření**. Měli byste vidět koncovku **SQLIaaSAgent** uvedené.

![Rozšíření IaaS agenta serveru SQL Azure portálu](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Taky můžete použít rutinu **Get-AzureVMSqlServerExtension** Azure Powershellu.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Odebrání   

Na portálu Azure můžete odinstalovat rozšíření kliknutím na tři tečky na zásuvné **rozšíření** vlastnosti virtuálního počítače. Klepněte na tlačítko **Odstranit**.

![Odinstalace rozšíření IaaS agenta serveru SQL Azure portálu](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Taky můžete použít rutinu Powershellu **AzureVMSqlServerExtension odebrat** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Další kroky

Začněte pomocí jedné ze služby podporované rozšíření. Další informace najdete v tématech odkaz v části [Podporované služby](#supported-services) tohoto článku.

Další informace o tom, jak SQL Server na virtuálních počítačích Azure najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
