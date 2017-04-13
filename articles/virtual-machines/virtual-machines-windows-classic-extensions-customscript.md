<properties
   pageTitle="Vlastní skript rozšíření OM Windows | Microsoft Azure"
   description="Automatizace úkolů konfigurace OM Azure pomocí koncovku vlastní skript ke spuštění skriptů Powershellu na remote OM systému Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Vlastní skript v článku rozšíření virtuálních počítačích Windows

Tento článek obsahuje přehled používání koncovku vlastní skript na Windows VMs pomocí rutin prostředí PowerShell Azure API správy služby Azure.

Rozšíření virtuálního počítače (OM) jsou vytvořené společností Microsoft a Důvěryhodní vydavatelé třetích stran rozšířit použitelnost funkce OM. Základní informace o rozšíření OM najdete v článku [rozšíření Azure OM a funkce](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky s využitím modelu správce prostředků](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Vlastní skript rozšíření přehled

S příponou vlastní skript pro systém Windows můžete je možné spouštět skriptů Powershellu na remote OM bez přihlášení k němu. Můžete použít tyto skripty po zajištění služby OM nebo kdykoli během životním cyklu OM bez otevření jakýkoli další. Nejběžnější případy použití pro spuštění vlastní skript rozšíření patří systém, instalace a konfigurace další software v OM po máte k dispozici.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Požadavky pro spuštění vlastní skript rozšíření

1. Instalace <a href="http://azure.microsoft.com/downloads" target="_blank">rutiny prostředí PowerShell Azure</a> verze 0.8.0 nebo novější.
2. Pokud budete potřebovat skripty ke spuštění na existující OM, zkontrolujte, že OM Agent je zapnuta OM. Pokud není nainstalovaný, postupujte podle těchto [kroků](virtual-machines-windows-classic-agents-and-extensions.md). OM vytvořeném z portálu Microsoft Azure, ve výchozím nastavení nainstalovaný OM Agent.
3. Nahrajte skriptů, které chcete spustit OM k základnímu úložišti Azure. Tyto skripty můžou pocházet z kontejneru jednoho nebo více kontejnery úložiště.
4. Skript by měl vytvořeny tak, aby položka skript, který je spuštěn příponu, spustí další skriptů.

## <a name="custom-script-extension-scenarios"></a>Vlastní skript rozšíření scénáře

### <a name="upload-files-to-the-default-container"></a>Nahrajte soubory do výchozího kontejneru

Následující příklad ukazuje, jak můžete spustit skripty v OM Pokud jsou v kontejneru úložiště výchozí účet předplatného. Do Název_kontejneru nahrajete skriptů. Výchozí úložiště účet může ověřovat pomocí **Get-AzureSubscription – výchozí** příkaz.

Následující příklad vytvoří virtuálního počítače, ale můžete taky spustit stejný scénář na existující OM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Nahrajte soubory do kontejneru jiné než výchozí úložiště

Tento scénář ukazuje, jak používat jiné než výchozí úložiště kontejneru v rámci stejného předplatného nebo v jiné předplatné pro odeslání skripty a souborů. Tento příklad ukazuje existující OM, ale stejné operace lze provést, když vytváříte virtuálního počítače.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Nahrání skripty do více kontejnery u různých úložiště účtů

  Pokud napříč několika kontejnery jsou uložené soubory skriptů, budete muset zadejte adresu URL podpisy (přidružení zabezpečení) sdílené úplný přístup k souborům spustit skripty.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Přidání vlastní skript rozšíření z portálu Microsoft Azure

Přejděte na OM <a href="https://portal.azure.com/ " target="_blank">Azure portál</a> a přidejte rozšíření zadáním soubor skriptu spustit.

  ![Určení soubor skriptu][5]


### <a name="uninstall-the-custom-script-extension"></a>Odinstalace koncovku vlastní skript

Rozšíření vlastní skript můžete odinstalovat OM pomocí následujícího příkazu.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Rozšíření vlastní skript pomocí šablony

Další informace o použití vlastní skript rozšíření šablonami správce prostředků Azure najdete v tématu [Použití vlastní skript rozšíření VMs Windows se šablonami správce prostředků Azure](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
