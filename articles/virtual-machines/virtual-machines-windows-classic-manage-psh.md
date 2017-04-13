<properties
   pageTitle="Správa virtuálních počítačích pomocí prostředí PowerShell Azure | Microsoft Azure"
   description="Další příkazy, které můžete použít k automatizaci úkolů při správě virtuálních počítačích."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Správa virtuálních počítačích pomocí prostředí PowerShell Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Mnoho úkoly se každý den si můžete spravovat svůj VMs můžete automatizovat pomocí rutin prostředí PowerShell Azure. Tento článek vám příklad příkazy pro jednodušší úkoly a odkazy na články, které ukazují příkazy pro složitější úlohy.

>[AZURE.NOTE] Pokud jste to ještě nainstalovali a nakonfigurovali Azure Powershellu, můžete získat pokyny v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Použití příkazů příklad
Budete potřebovat k nějakému textu v seznamu Příkazy nahraďte text, který je vhodné pro prostředí. < a > symboly označte text, je nutné nahradit. Když nahraďte text, odeberte symboly, ale nechat značky nabídky na místě.

## <a name="get-a-vm"></a>Získání virtuálního počítače
Toto je základní úkol, který se často používáte. Můžete získat informace o virtuálního počítače, provádění úkolů na virtuálního počítače nebo získat výstup pro uložení proměnné.

Pokud chcete získat informace o OM, spustit tento příkaz Nahradit vše v uvozovkách, včetně < a > znaky:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Pokud chcete ukládání výstupu do proměnné $vm, spusťte:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Přihlaste se k serveru s Windows OM

Spusťte tyto příkazy:

>[AZURE.NOTE] Virtuální počítač a cloudové služby názvu dostali ze zobrazení příkazu **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Ukončení virtuálního počítače

Spusťte tento příkaz:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Použijte tento parametr zachovat virtuální IP (VIP) služby cloudu v případě, že je poslední OM v téhle služby cloudu. <br><br> Pokud používáte parametr StayProvisioned, můžete pořád fakturovat pro OM.

## <a name="start-a-vm"></a>Zahájení virtuálního počítače

Spusťte tento příkaz:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Připojení dat disku
Tento úkol vyžaduje několik kroků. Nejdřív použijte *** přidat – rutiny AzureDataDisk *** můžete přidat disku $vm objektu. Potom **Aktualizace AzureVM** rutina slouží k aktualizaci konfigurace OM.

Musíte taky rozhodnout, jestli připojení nového disku nebo který obsahuje data. Nový disk na příkaz vytvoří soubor VHD a ji připojí.

Pokud chcete přidat nový disk, spusťte tento příkaz:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Pokud chcete připojit existující data disk, spusťte tento příkaz:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Připojení dat disků z existujícího souboru VHD v úložišti objektů blob, spusťte tento příkaz:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Vytvoření OM serveru s Windows

Pokud chcete vytvořit nový počítač virtuálního serveru s Windows v Azure, postupujte podle pokynů v [Pomocí prostředí PowerShell Azure k vytvoření a nastavení serveru s Windows virtuálních počítačích](virtual-machines-windows-classic-create-powershell.md). Toto téma postup vám pomůže vystavením Azure PowerShell příkaz Nastavení, která vytvoří OM serveru s Windows, které můžete automaticky předem nakonfigurovaná:

- S členství v doméně služby Active Directory.
- S další disk.
- Jako člen jejího existující Vyrovnávání zatížení nastavení.
- Pomocí statické IP adresy.
