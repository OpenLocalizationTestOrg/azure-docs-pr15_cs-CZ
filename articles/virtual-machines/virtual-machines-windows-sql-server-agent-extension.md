<properties
    pageTitle="Rozšíření agenta SQL serveru pro SQL Server VMs (Správce zdrojů) | Microsoft Azure"
    description="Toto téma popisuje, jak spravovat agent rozšíření serveru SQL Server, které umožňuje automatizovat konkrétní úlohy správy systému SQL Server. Jedná se o automatické zálohování automatické opravy a integrace trezoru klíč Azure. Toto téma používá režim nasazení Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>Rozšíření agenta SQL serveru pro SQL Server VMs (Správce zdrojů)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasický](virtual-machines-windows-classic-sql-server-agent-extension.md)

Rozšíření agenta IaaS SQL Server (SQLIaaSExtension) je spuštěn Azure virtuálních počítačích k automatizaci úkolů správy. Toto téma obsahuje přehled o nepodporuje příponu, stejně jako pokyny k instalaci, stav a odebrání služby.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu. Klasická verzi tohoto článku zobrazíte najdete v článku [Rozšíření agenta SQL serveru pro SQL Server VMs klasické](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Podporované služby

Rozšíření agenta IaaS SQL Server podporuje následující úlohy správy:

| Funkce správy | Popis |
|---------------------|-------------------------------|
| **SQL automatické zálohování** | Umožňuje automatizovat plánování zálohování pro všechny databáze pro výchozí instanci systému SQL Server v OM. Další informace najdete v tématu [Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure (Správce zdrojů)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automatické opravy** | Konfiguruje údržby během níž aktualizace vaší OM může proběhnout, abyste neměli aktualizace špičce pro vaše pracovní zátěž. Další informace najdete v tématu [Automatické opravy pro systém SQL Server ve virtuálních počítačích Azure (Správce zdrojů)](virtual-machines-windows-sql-automated-patching.md).|
| **Integrace Azure klíčové trezoru** | Umožňuje automaticky nainstalujte a nakonfigurujte Azure klíč trezoru na vaše OM SQL serveru. Další informace najdete v tématu [Konfigurace Azure klíč trezoru integrace pro systém SQL Server na Azure VMs (Správce zdrojů)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Zjistit předpoklady pro

Požadavky na vaše OM používat rozšíření agenta IaaS Server SQL:

**Operační systém**:

- Windows Server 2012
- Windows serveru 2012 R2

**Verze SQL serveru**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure Powershellu**:

- [Stáhněte si a konfigurace nejnovější příkazy Powershellu Azure](../powershell-install-configure.md)

## <a name="installation"></a>Instalace

Rozšíření agenta SQL Server IaaS se instaluje automaticky když zřizujete jeden z Galerie obrázků virtuálního počítače SQL serveru.

Pokud vytvoříte virtuálního počítače s operačním systémem Windows Server, můžete nainstalovat rozšíření ručně pomocí rutiny prostředí PowerShell **Set-AzureVMSqlServerExtension** . Následující příkaz například nainstaluje rozšíření na jen s operačním systémem OM serveru Windows a pojmenuje ji "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Při aktualizaci na nejnovější verzi rozšíření agenta IaaS SQL, po restartování počítače virtuální po aktualizaci rozšíření.

>[AZURE.NOTE] Pokud jste nainstalovali SQL Server IaaS Agent rozšíření ručně OM serveru Windows, musíte pomocí a správa jeho funkcí pomocí prostředí PowerShell příkazů. Rozhraní portálu je k dispozici pouze pro obrázky v galerii SQL serveru.

## <a name="status"></a>Stav

Jedním ze způsobů k ověření, že je nainstalovaný rozšíření je zobrazení stavu agent v portálu Azure. Vyberte **všechna nastavení** v zásuvné virtuálního počítače a potom klepněte na **rozšíření**. Měli byste vidět koncovku **SQLIaaSExtension** uvedené.

![Rozšíření IaaS agenta serveru SQL Azure portálu](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Taky můžete použít rutinu **Get-AzureVMSqlServerExtension** Azure Powershellu.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Příkaz předchozí potvrdí agent je nainstalovaný a poskytuje informace obecné informace o stavu. Můžete taky získat konkrétních informací o stavu o automatické zálohování a oprav pomocí následujících příkazů.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Odebrání   

Na portálu Azure můžete odinstalovat rozšíření kliknutím na tři tečky na zásuvné **rozšíření** vlastnosti virtuálního počítače. Klepněte na tlačítko **Odstranit**.

![Odinstalace rozšíření IaaS agenta serveru SQL Azure portálu](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Taky můžete použít rutinu Powershellu **AzureRmVMSqlServerExtension odebrat** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Další kroky

Začněte pomocí jedné ze služby podporované rozšíření. Další informace najdete v tématech odkaz v části [Podporované služby](#supported-services) tohoto článku.

Další informace o tom, jak SQL Server na virtuálních počítačích Azure najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
