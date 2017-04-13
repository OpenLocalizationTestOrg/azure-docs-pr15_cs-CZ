<properties
    pageTitle="Resetování hesla nebo konfigurace Vzdálená plocha na Windows OM | Microsoft Azure"
    description="Zjistěte, jak resetovat heslo k účtu nebo služby Vzdálená plocha na OM Windows Azure portál nebo Azure Powershellu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Jak obnovit službu Vzdálená plocha nebo jeho heslo pro přihlášení do Windows OM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Pokud se nemůžete připojit k počítači virtuální systému Windows (OM), můžete resetovat heslo místního správce nebo obnovit konfiguraci služby Vzdálená plocha. Můžete buď portálu Azure nebo číslo linky OM přístup v prostředí PowerShell Azure aby vám resetoval heslo. Pokud používáte Powershellu, ujistěte se, nejnovější prostředí PowerShell instalace modulu na počítači v práci a jste přihlášení k předplatnému Azure. Podrobný postup přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

> [AZURE.TIP] Můžete zjistit verzi, kterou jste nainstalovali pomocí prostředí PowerShell`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows VMs v modelu nasazení Správce prostředků

### <a name="azure-portal"></a>Azure portálu
Vyberte svůj OM po kliknutí na **Procházet** > **virtuálních počítačích** > *počítači se systémem Windows virtuální* > **všechna nastavení** > **resetování hesla**. Zásuvné resetování hesla se zobrazí:

![Stránka pro vytvoření nového hesla](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Zadejte uživatelské jméno a nové heslo a potom klikněte na tlačítko **Uložit**. Připojte se k vaší OM znovu.

### <a name="vmaccess-extension-and-powershell"></a>Rozšíření VMAccess a prostředí PowerShell

Zkontrolujte, jestli máte Azure PowerShell 1.0 nebo novější a jste přihlášení k účtu pomocí `Login-AzureRmAccount` rutiny.

#### <a name="reset-the-local-administrator-account-password"></a>**Resetování hesla účtu místního správce**

Správce hesel nebo uživatelské jméno můžete obnovit pomocí příkazu prostředí PowerShell [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) .

Vytvoření správce místní přihlašovací údaje účtu pomocí tento příkaz:

    $cred=Get-Credential

Pokud zadáte jiný název než aktuální účet, tento příkaz rozšíření VMAccess přejmenuje místního účtu správce přiřadí heslo k tomuto účtu a problémy události odhlášení Vzdálená plocha. Pokud je zakázané místního účtu správce koncovku VMAccess ji povolí.

Pomocí rozšíření OM aplikace access můžete nastavit nové přihlašovací údaje následujícím způsobem:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Nahrazení `myRG`, `myVM`, `myVMAccess`a umístění s hodnotami příslušných nastavení.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Obnovení konfiguraci služby Vzdálená plocha**

Můžete obnovit vzdáleného přístupu vaší OM pomocí [AzureRmVMExtension sadu](https://msdn.microsoft.com/library/mt603745.aspx) nebo [Sadu AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx)následujícím způsobem. (Nahradit `myRG`, `myVM`, `myVMAccess` a umístění s vlastními hodnotami.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Nebo:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Oba příkazy přidat nové agenta pojmenované přístup OM do virtuálního počítače. V libovolném okamžiku virtuálního počítače může obsahovat pouze jeden OM přístup agenta. Nastavení přístupu OM agent vlastností úspěšně, odebrat agent přístupu nastaveno pomocí `Remove-AzureRmVMAccessExtension` nebo `Remove-AzureRmVMExtension`. Počínaje Azure PowerShell verze 1.2.2 se můžete vyhnout tímto krokem při použití `Set-AzureRmVMExtension` s `-ForceRerun` možnost. Při použití `-ForceRerun`, zkontrolujte, že používat stejný název pro agent přístupu OM nastavená pomocí příkazu předchozí.

Pokud se stále nemůžete připojit vzdáleně do virtuálního počítače, podívejte se na další kroky vyzkoušet na [Poradce při potížích s vzdálené plochy připojení k serveru s Windows Azure virtuálního počítače](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows VMs v modelu klasické nasazení

### <a name="azure-portal"></a>Azure portálu

Pro virtuálních počítačích vytvořené pomocí klasické nasazení modelu můžete [Azure portál](https://portal.azure.com) resetovat službu Vzdálená plocha. Klikněte na: **Procházet** > **virtuálních počítačích (klasické)** > *počítači se systémem Windows virtuální* > **Obnovit vzdálené...**. Zobrazí se další stránky.

![Obnovit stránku konfigurace RDP](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Můžete taky zkusit obnovením jméno a heslo místního účtu správce. Klikněte na: **Procházet** > **virtuálních počítačích (klasické)** > *počítači se systémem Windows virtuální* > **všechna nastavení** > **resetování hesla**. Zobrazí se další stránky.

![Stránka pro vytvoření nového hesla](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Po zadání nové uživatelské jméno a heslo, klikněte na **Uložit**.

### <a name="vmaccess-extension-and-powershell"></a>Rozšíření VMAccess a prostředí PowerShell

Zkontrolujte, že Agent OM nainstalovaná v počítači virtuální. Rozšíření VMAccess nemusí nainstalovat, než budete moct použít, jakož agenta OM je k dispozici. Už instalaci agenta OM pomocí následujícího příkazu ověřte. (Nahraďte "myCloudService" a "myVM" podle názvů Cloudová služba a OM, respektive. Tyto názvy se dozvíte spuštěním `Get-AzureVM` bez parametrů.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Pokud příkaz **zápisu hostitele** zobrazí **True**, Agent OM nainstalovaný. Pokud se zobrazí **Nepravda**, najdete v článku pokyny a odkaz na stažení [OM Agent a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure v příspěvku do blogu.

Pokud jste vytvořili pomocí portálu virtuální počítač, zkontrolujte, zda `$vm.GetInstance().ProvisionGuestAgent` vrátí **hodnotu True**. V opačném případě můžete nastavit tak, pomocí tohoto příkazu:

    $vm.GetInstance().ProvisionGuestAgent = $true

Tento příkaz zabrání při provozu na příkaz **Nastavení AzureVMExtension** v dalším krokům tato chyba: "Poskytování hosta Agent musí být povolený na požadovaném objektu OM před nastavením rozšíření Access IaaS OM."

#### <a name="reset-the-local-administrator-account-password"></a>**Resetování hesla účtu místního správce**

Vytvoření přihlašovací pověření s aktuální název místní správce účtu a nové heslo a pak spusťte `Set-AzureVMAccessExtension` následujícím způsobem.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Pokud zadáte jiný název než aktuální účet, koncovku VMAccess přejmenuje místního účtu správce přiřadí heslo k tomuto účtu a Vzdálená plocha odhlašovací při potížích. Pokud je zakázané místního účtu správce koncovku VMAccess ji povolí.

Tyto příkazy také obnovit konfiguraci služby Vzdálená plocha.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Obnovení konfiguraci služby Vzdálená plocha**

Obnovte konfiguraci služby Vzdálená plocha, spusťte tento příkaz:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Rozšíření VMAccess běží dva příkazy na virtuálního počítače:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Tento příkaz umožňuje předdefinovaných skupiny Brána Firewall systému Windows umožňující vzdálené plochy příchozích, který využívá TCP port 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Tento příkaz nastaví fDenyTSConnections registru hodnotu 0, povolení připojení ke vzdálené ploše.


## <a name="next-steps"></a>Další kroky

Pokud nejste schopni resetování hesla rozšíření access Azure OM nereaguje, můžete [Obnovit Windows heslo místního offline](virtual-machines-windows-reset-local-password-without-agent.md). Tento způsob je pokročilejší proces a vyžaduje, abyste připojení virtuální pevný disk problematický OM jiné v angličtině. Postupujte podle kroků popsaných v tomto článku nejdřív a pouze pokusí metodu offline heslo resetovat jako poslední možnost.

[Azure OM rozšíření a funkce](virtual-machines-windows-extensions-features.md)

[Připojení k Azure virtuálního počítače s RDP nebo SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Poradce při potížích s připojení ke vzdálené ploše na serveru s Windows Azure virtuálního počítače](virtual-machines-windows-troubleshoot-rdp-connection.md)
