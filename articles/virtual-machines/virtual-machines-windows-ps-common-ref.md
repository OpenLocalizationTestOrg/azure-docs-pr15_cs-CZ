<properties 
   pageTitle="Běžné příkazy Powershellu pro VMs | Microsoft Azure"
   description="Běžnými příkazy Powershellu vám usnadní zahájení práce vytváření a správě VMs v Azure v systému Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Běžnými příkazy Powershellu pro vytváření a správu VMs

Tento článek popisuje některé příkazy Powershellu Azure, které slouží k vytváření a správa virtuálních počítačích předplatné Azure.  Podrobnější nápovědu k určité přepínače příkazového řádku a možnosti můžete **Získat nápovědu** *příkaz*.

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="create-a-vm"></a>Vytvoření virtuálního počítače

Úkol | Příkaz
-------------- | -------------------------
Vytvoření konfigurace OM | $vm = vm_size" [New AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName"vm_name"- VMSize"<BR></BR><BR></BR>Konfigurace OM lze definovat nebo aktualizovat nastavení OM. Konfigurace je inicializován s názvem OM a jeho [velikost](virtual-machines-windows-sizes.md).
Přidání konfigurace nastavení | $vm = [Set AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - OM $vm – Windows – název_počítače "název_počítače"-pověření $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Nastavení operační systém, včetně [přihlašovacích údajů](https://technet.microsoft.com/library/hh849815.aspx) se přidají do konfigurační objekt, který jste vytvořili pomocí AzureRmVMConfig nový.
Přidat síťové rozhraní | $vm = [Přidat AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - OM $vm – Id $nic. ID<BR></BR><BR></BR>Virtuálního počítače musí být v [síti rozhraní](virtual-machines-windows-ps-create.md) komunikovat v síti virtuální. Můžete taky [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) načíst existující objekt rozhraní sítě.
Určení obrázku platformy | $vm = publisher_name" [Set AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - OM $vm - PublisherName"-nabízejí "publisher_offer" - SKU "product_sku"-"nejnovější" verze<BR></BR><BR></BR>[Obrázek informace](virtual-machines-windows-cli-ps-findimage.md) se přidají do konfigurační objekt, který jste vytvořili pomocí AzureRmVMConfig nový. Objekt vrátil příkaz se používá pouze při nastavování OS disku použít obrázek platformu.
Nastavení OS disku použít obrázek platformy | $vm = [Set AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - OM $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"název"disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Název disk operačního systému a jeho umístění v [úložišti](../storage/storage-powershell-guide-full.md) je přidán do konfigurační objekt, který jste vytvořili.
Nastavení OS disk, aby používal generalized obrázek | $vm = set AzureRmVMOSDisk - OM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"název"disk_name"- SourceImageUri. {guid} VHD"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage – Windows<BR></BR><BR></BR>Název disku operační systém, umístění zdroje obrázku a místa disku v [úložišti](../storage/storage-powershell-guide-full.md) se přidá k objektu konfigurace.
Nastavení OS disk, aby používal specializované obrázek | $vm = set AzureRmVMOSDisk - OM $vm-http://mystore1.blob.core.windows.net/vhds/"název"name_of_disk"- VhdUri" - CreateOption připojit – Windows
Vytvoření virtuálního počítače | [Nový AzureRmVM]() - ResourceGroupName "resource_group_name"-umístění "location_name" - OM $vm<BR></BR><BR></BR>Všechny zdroje jsou vytvářeny ve [pole Skupina zdroje](../powershell-azure-resource-manager.md). OM musí vytvořené ve stejném [umístění](https://msdn.microsoft.com/library/azure/dn495177.aspx) jako skupina zdroje. Před spusťte tento příkaz spusťte nový AzureRmVMConfig AzureRmVMOperatingSystem nastavení, nastavení AzureRmVMSourceImage, přidat AzureRmVMNetworkInterface a nastavení AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>Získání informací o VMs

Úkol | Příkaz
-------------- | -------------------------
Seznam VMs v předplatné| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Seznam VMs ve skupině zdroje | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Seznam skupin zdrojů ve vašem předplatném, použijte [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Získání informací o virtuálního počítače | Get-AzureRmVM - ResourceGroupName "resource_group_name"-název "vm_name"

## <a name="manage-vms"></a>Správa VMs

Úkol | Příkaz
-------------- | -------------------------
Zahájení virtuálního počítače | [Zahájení AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-název "vm_name"
Ukončení virtuálního počítače | [Zastavit AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-název "vm_name"
Restartujte pracovního OM | [Restart AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-název "vm_name"
Odstranění virtuálního počítače | [Odebrat AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-název "vm_name"
Generalize virtuálního počítače | [Nastavení AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup – název "vm_name"-Generalized<BR></BR><BR></BR>Spusťte tento příkaz před spuštěním AzureRmVMImage uložit.
Zachycení virtuálního počítače | [Uložit AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-cestu "C:\filepath\filename.json"<BR></BR><BR></BR>Virtuální počítač musí být [vypnout a generalized](virtual-machines-windows-generalize-vhd.md) použít k vytvoření obrázku. Dříve než spustíte tento příkaz spusťte AzureRmVm sadu.
Aktualizace virtuálního počítače | [Aktualizace AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - OM $vm<BR></BR><BR></BR>Získání aktuální konfigurace OM pomocí Get-AzureRmVM, změňte nastavení na požadovaném objektu OM a spusťte tento příkaz.
Přidání dat disku do virtuálního počítače | [Přidat AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - OM $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"název"disk_name"- VhdUri" - logické jednotky # – ukládání do mezipaměti pouze - DiskSizeinGB # - CreateOption prázdné<BR></BR><BR></BR>Pomocí Get-AzureRmVM OM objektu. Zadejte požadovanou velikost logické jednotky a velikost disku. Spuštění aktualizace AzureRmVM změny se projeví konfigurace bude v angličtině. Disk, které přidáte není spuštěn. Informace o inicializace disků, jak byly přidány najdete v článku [Správa virtuálních počítačích Azure pomocí Správce zdrojů a Powershellu](virtual-machines-windows-ps-manage.md).
Odebrání disku data z virtuálního počítače | [Odebrat AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - OM $vm – název "disk_name"<BR></BR><BR></BR>Pomocí Get-AzureRmVM OM objektu. Spuštění aktualizace AzureRmVM změny se projeví konfigurace bude v angličtině.
Přidání rozšíření do virtuálního počítače | [Set AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-vm_name"umístění"azure_location"- VMName"-název "extension_name"-Publisheru "publisher_name"-typu "extension_type" - TypeHandlerVersion "#. #"-nastavení $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Spusťte tento příkaz příslušné [informace o konfiguraci](virtual-machines-windows-extensions-configuration-samples.md) pro rozšíření, které chcete nainstalovat.
Odebrání příponu OM | [Odebrat AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-vm_name"název"extension_name"- VMName"

## <a name="next-steps"></a>Další kroky

- V tématu základní kroky při vytváření virtuálních počítačů vytvořit [Windows OM pomocí Správce zdrojů a Powershellu](virtual-machines-windows-ps-create.md).

