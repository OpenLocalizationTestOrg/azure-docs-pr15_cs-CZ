<properties
    pageTitle="Změna velikosti Windows OM | Microsoft Azure"
    description="Změna velikosti vytvoření v modelu nasazení Správce prostředků, pomocí prostředí Powershell Azure Windows virtuálního počítače."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Změna velikosti Windows OM

Tento článek popisuje, jak změnit velikost Windows OM, vytvoření v modelu nasazení Správce prostředků pomocí prostředí Powershell Azure.

Po vytvoření virtuálního počítače (OM), můžete změnit OM nahoru nebo dolů změnou velikosti OM. V některých případech můžete nejdřív zrušit OM. Tomu může dojít, pokud není k dispozici na hardware obrázku, který je hostitelem aktuálně OM novou velikost.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Změna velikosti Windows OM nejsou v sadě dostupnosti

1. Seznam velikosti OM, které jsou k dispozici clusteru hardwaru, ve které je hostovaný OM. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Pokud je uvedená požadovanou velikost, spusťte následující příkazy změnit velikost OM. Pokud požadovanou velikost nevidíte, přejděte ke kroku 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Pokud požadovanou velikost nevidíte, spusťte následující příkazy pro zrušit OM, změnit jeho velikost a restartujte OM.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Navrácení OM vydání všechny dynamické IP adresa přiražená bude v angličtině. OS a dat disků nemá vliv. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Změna velikosti OM Windows v sadě dostupnosti

Pokud novou velikost OM sady dostupnost není k dispozici na hardware clusteru aktuálně hostingu OM, všechny VMs v sadě dostupnost potřebovat uvolnit změnit velikost OM. Také může musíte aktualizovat velikost jiných VMs dostupnosti po byl upraven jeden OM. Pokud chcete změnit velikost OM sady dostupnost, proveďte následující kroky.

1. Seznam velikosti OM, které jsou k dispozici clusteru hardwaru, ve které je hostovaný OM.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Pokud je uvedená požadovanou velikost, spusťte následující příkazy změnit velikost OM. Pokud nevidíte, přejděte ke kroku 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Pokud nenajdete požadovanou velikost, pokračujte následujícími kroky Zrušit vše VMs v sadě dostupnost, změna velikosti VMs a je znovu spustit.

4.  Ukončete všechny VMs v sadě dostupnosti.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Změna velikosti a restartujte VMs v sadě dostupnosti.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Další kroky

- Další rozšiřitelnost spusťte několika instancích spuštěných OM a rozšiřování. Další informace najdete v tématu [automaticky měřítko počítačích Windows v sadě měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



