<properties
    pageTitle="Nainstalovat Symantec Endpoint Protection OM | Microsoft Azure"
    description="Zjistěte, jak nainstalovat a nakonfigurovat rozšíření zabezpečení Symantec Endpoint Protection na novém nebo stávajícím Azure virtuálního počítače vytvořená pomocí klasické nasazení modelu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Jak nainstalovat a nakonfigurovat aplikaci Symantec Endpoint Protection na Windows OM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Tento článek popisuje, jak nainstalovat a nakonfigurovat klienta Symantec Endpoint Protection počítače existující virtuální (OM) serverem Windows. Toto je celé klienta, který obsahuje služby, jako je antivirová ochrana před spywarem, bránu firewall a zabránění průnik. Klient nainstalovaný jako rozšíření zabezpečení pomocí agenta OM.

Pokud máte stávajícímu předplatnému z Symantec pro místní řešení, můžete ji použijete k ochraně Azure virtuálních počítačích. Pokud si nejste zákazníka ještě, můžete registraci zkušební předplatné. Další informace o tomto řešení, najdete v článku [Symantec Endpoint Protection na společnosti Microsoft Azure platformu][Symantec]. Tato stránka obsahuje taky odkazy na informace o správě licencí a pokyny k instalaci klienta, pokud už jste zákazník Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Nainstalovat aplikaci Symantec Endpoint Protection na existující OM

Než začnete, musí být splněny následující:

- Modul Azure Powershellu, verze 0.8.2 nebo novější, na počítači v práci. Můžete zjistit verzi Azure Powershellu, které jste nainstalovali s **Get-modul azure | formátovat tabulku verze** příkaz. Pokyny a odkaz na nejnovější verzi, najdete v článku [jak instalace a konfigurace prostředí PowerShell Azure][PS]. Přihlaste se k Azure předplatného pomocí `Add-AzureAccount`.

- Agent OM spuštěné v počítači virtuální Azure.

Nejdřív zkontrolujte, že Agent OM už nainstalovaný v počítači virtuální. Zadejte název služby cloudu a název virtuálního počítače a příkazového řádku na úrovni správce Azure PowerShell spusťte následující příkazy. Nahrazení všechno, co do uvozovek, včetně < a > znaky.

> [AZURE.TIP] Pokud neznáte Cloudová služba a virtuální názvy, spusťte **Get-AzureVM** seznam názvů pro všechny virtuálních počítačích v aktuálního předplatného.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Pokud příkaz **zápisu hostitele** zobrazí **True**, Agent OM nainstalovaný. Pokud se zobrazí **Nepravda**, najdete v článku pokyny a odkaz na stažení v Azure blogu publikovat [OM Agent a rozšíření – část 2][Agent].

Pokud Agent OM je nainstalovaný, spusťte tyto příkazy k instalaci agenta Symantec Endpoint Protection.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Ověření, že přípona zabezpečení Symantec instalaci a aktuální:

1.  Přihlaste se do virtuálního počítače. Pokyny najdete v tématu [jak se přihlásit k serveru virtuálního počítače systém Windows][Logon].
2.  Windows Server 2008 R2, klikněte na **Start > Symantec Endpoint Protection**. Pro Windows Server 2012 nebo Windows Server 2012 R2, na obrazovce start zadejte **Symantec**a klikněte na **Symantec Endpoint Protection**.
3.  Na kartě **Stav** okna **Stav Symantec Endpoint Protection** použití aktualizací nebo restartujte v případě potřeby.

## <a name="additional-resources"></a>Další zdroje informací

[Jak se přihlásit do virtuálního počítače se systémem Windows Server][Logon]

[Rozšíření Azure OM a funkce][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493