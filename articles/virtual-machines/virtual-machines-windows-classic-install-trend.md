<properties
    pageTitle="Instalace trendu malých důkladné zabezpečení na virtuálního počítače | Microsoft Azure"
    description="Tento článek popisuje, jak nainstalovat a nakonfigurovat Trend Micro zabezpečení OM vytvořená pomocí klasické nasazení modelu v Azure."
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


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Jak nainstalovat a nakonfigurovat Trend Micro důkladné zabezpečení jako služba ve Windows OM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

V tomto článku se dozvíte, jak nainstalovat a nakonfigurovat Trend Micro důkladné zabezpečení jako službu v nové nebo existující virtuálního počítače (OM) serverem Windows. Důkladné zabezpečení jako služba obsahuje ochrana proti malwaru, bránu firewall, průnik zabránění systému a sledování integrita.

Klient je instalována jako rozšíření zabezpečení prostřednictvím agenta OM. Na novém počítači virtuální nainstalujete agenta OM spolu s hloubkové Agent zabezpečení. Na existující počítač virtuální, který nemá agenta OM budete muset stáhnout a nainstalovat ho nejdřív. Tento článek popisuje, jak situací.

Pokud máte existující předplatné Trend Micro pro místní řešení, můžete se můžete pomoci chrá Azure virtuálních počítačích. Pokud si nejste zákazníka ještě, můžete registraci zkušební předplatné. Další informace o tomto řešení najdete v článku Trend Micro blogu [Microsoft Azure OM Agent rozšíření pro důkladné zabezpečení](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Instalace agenta důkladné zabezpečení na nové OM

[Azure klasické portál](http://manage.windowsazure.com) umožňuje instalaci Agent OM a rozšíření Trend Micro zabezpečení při použití možnosti **Z Galerie** vytvořit virtuální počítač. Pokud vytváříte jednoho virtuálního počítače, na portálu je snadný způsob, jak přidat Zámek z Trend Micro.

Tuto možnost **Z Galerie** otevřete průvodce, který vám pomůže nastavit virtuální počítač. Poslední stránka průvodce umožňuje instalaci OM Agent a rozšíření Trend Micro zabezpečení. Obecné pokyny najdete v tématu [Vytvoření virtuálního počítače s Windows Azure klasické portálu](virtual-machines-windows-classic-tutorial.md). Až se dostanete na poslední stránce průvodce, postupujte takto:

1.  V části **Agent OM**zaškrtněte **Instalace Agent OM**.

2.  V části **Zabezpečení rozšíření**zaškrtněte **Trend Micro důkladné zabezpečení Agent**.

    ![Instalace Agent OM a Agent důkladné zabezpečení](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Zaškrtněte políčko Vytvořit virtuální počítač.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Instalace agenta důkladné zabezpečení na existující OM

Pokud chcete nainstalovat agent existující OM, musíte:

- Modul Azure Powershellu, verze 0.8.2 nebo novější, nainstalovaných v počítači. Můžete zjistit verzi Azure PowerShell, který jste nainstalovali pomocí **Get-modul azure | formátovat tabulku verze** příkaz. Pokyny a odkaz na nejnovější verzi najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md). Přihlaste se k Azure předplatného pomocí `Add-AzureAccount`.

- Agent OM nainstalovaných v počítači virtuální cílové.

Nejprve ověřte, že je Agent OM už nainstalovaný. Zadejte název služby cloudu a název virtuálního počítače a příkazového řádku na úrovni správce Azure PowerShell spusťte následující příkazy. Nahrazení všechno, co do uvozovek, včetně < a > znaky.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Pokud neznáte Cloudová služba a název počítače virtuální, spusťte **Get-AzureVM** zobrazíte tyto informace pro virtuálních počítačích v aktuálního předplatného.

Pokud příkaz **zápisu-Host (hostitel)** vrátí **hodnotu PRAVDA**, je nainstalovaný agenta OM. Vrátí **hodnotu False**, najdete v článku pokyny a odkaz na stažení v Azure blogu publikovat [OM Agent a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Pokud Agent OM je nainstalovaný, spusťte tyto příkazy.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Další kroky

Trvá několik minut agent spouštění při hned po instalaci používat. Po, musíte aktivovat důkladné zabezpečení na virtuálního počítače, můžete spravovat pomocí důkladné zabezpečení správce. Přečtěte si následující další pokyny:

- LINTREND článek o toto řešení [Instant-On cloudu zabezpečení pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- [Ukázka skriptu prostředí Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) pro konfiguraci virtuálního počítače
- [Pokyny](http://go.microsoft.com/fwlink/?LinkId=404099) k výběru

## <a name="additional-resources"></a>Další zdroje informací

[Jak se přihlásit do virtuálního počítače spuštěný Windows Server]

[Azure OM rozšíření a funkce]


<!--Link references-->
[Jak se přihlásit do virtuálního počítače spuštěný Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Azure OM rozšíření a funkce]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
