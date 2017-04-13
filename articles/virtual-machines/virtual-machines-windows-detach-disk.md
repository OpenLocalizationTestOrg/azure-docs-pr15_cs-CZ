<properties
    pageTitle="Odpojení disk dat z Windows OM | Microsoft Azure"
    description="Zjistěte, jak odpojení disk dat od virtuálního počítače v Azure pomocí nasazení modelu správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Jak se odpojit disk dat z Windows virtuálního počítače


Pokud už nepotřebujete dat disku, který je připojen k virtuálního počítače, můžete ho snadno odpojit. Odebere z počítače virtuální disk, ale neodebere z úložiště. 

> [AZURE.WARNING] Pokud odpojení disk neodstraníte automaticky. Pokud máte předplatné k základnímu úložišti Premium, vám budou zasílané uskutečňovat úložiště poplatky za disku. Další informace naleznete v [ceny](../storage/storage-premium-storage.md#pricing-and-billing)a fakturaci při použití Premium úložiště. 

Pokud chcete znovu použít existující data na disku, můžete ho znovu na stejném počítači virtuální nebo nějaký jiný.  


## <a name="detach-a-data-disk-using-the-portal"></a>Odpojení disk dat na portálu

1. V centru portálu vyberte **virtuálních počítačích**.

2. Vyberte virtuální počítač, který obsahuje data disku, který chcete odpojit a pak klikněte na **všechna nastavení**.

3. V **Nastavení** zásuvné vyberte **disků**.

4. V zásuvné **disků** vyberte dat disku, který chcete odpojit.

5. V zásuvné disku dat klikněte na **Odebrat**.


    ![Snímek obrazovky s tlačítkem odpojit.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Disk zůstane v úložiště, ale je už není připojen k virtuálního počítače.


## <a name="detach-a-data-disk-using-powershell"></a>Odpojení disk dat pomocí prostředí PowerShell

V tomto příkladu získá první příkaz virtuálního počítače s názvem **MyVM07** ve skupině prostředků **RG11** pomocí rutiny Get-AzureRmVM. Příkaz Uložit virtuální počítač **$VirtualMachine** proměnné. 

Druhý příkaz odebere disk dat s názvem DataDisk3 z virtuální počítač. 

Příkaz konečný aktualizovat stav počítače virtuální dokončete proces odebrání disku data.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Další informace najdete v tématu [Odebrat AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Další kroky

Pokud chcete znovu použít datový disk, můžete to udělat jenom [připojit k jiné OM](virtual-machines-windows-attach-disk-portal.md)
