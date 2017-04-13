<properties
    pageTitle="Správa VMs pomocí Správce zdrojů a prostředí PowerShell | Microsoft Azure"
    description="Správa virtuálních počítačích pomocí Správce prostředků Azure a Powershellu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Správa virtuálních počítačích Azure pomocí Správce zdrojů a prostředí PowerShell

## <a name="install-azure-powershell"></a>Instalace modulu Azure PowerShell
 
Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="set-variables"></a>Nastavení proměnné

Všechny příkazy v článku vyžadují název Skupina zdroje, které je umístěn virtuálního počítače a název počítače virtuální ke správě. Hodnota **$rgName** nahraďte názvem, který obsahuje virtuální počítač skupina zdroje. Nahraďte hodnotu **$vmName** s názvem OM. Vytvořte proměnné.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Zobrazení informací o virtuálního počítače

Získáte informace o virtuální počítače.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Vrátí vypadá podobně jako tento příklad:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Ukončení virtuálního počítače

Ukončení spuštěného virtuálního počítače.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Se zobrazí výzva k potvrzení:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Zadejte **Y** ukončíte virtuální počítač.

Po několika minutách vrátí vypadá podobně jako tento příklad:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Spuštění virtuálního počítače

Spuštění virtuálního počítače, pokud je zastaveno.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Po několika minutách vrátí vypadá podobně jako tento příklad:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Pokud chcete ho restartovat virtuálního počítače, na kterém běží už použití **Restart-AzureRmVM** dělat dále popsané.

## <a name="restart-a-virtual-machine"></a>Restartujte virtuálního počítače

Restartujte pracovního virtuálního počítače.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Vrátí vypadá podobně jako tento příklad:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Odstranění virtuálního počítače

Odstranění virtuálního počítače.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Můžete použít **– vyšší** parametr přeskočit okně pro potvrzení.

Pokud nepoužíváte - platnost parametr, se zobrazí výzva k potvrzení:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Vrátí vypadá podobně jako tento příklad:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Aktualizace virtuálního počítače

Tento příklad ukazuje, jak aktualizovat velikost virtuální počítač.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Vrátí vypadá podobně jako tento příklad:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Seznam dostupných formátů virtuálního počítače najdete v článku [velikosti virtuálních počítačích v Azure](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Přidání dat disku do virtuálního počítače

Tento příklad ukazuje, jak přidat disk dat do existující virtuálního počítače.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Disk, které přidáte není spuštěn. Inicializace disk, můžete Přihlaste se k němu a použít Správa disků. Pokud jste si nainstalovali WinRM a certifikát v něm když jste ho vytvořili, můžete vzdálený PowerShell inicializace disku. Můžete taky použít rozšíření vlastní skript: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Soubor skriptu mohou obsahovat vypadá podobně jako tento kód inicializace disků:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Další kroky

Pokud nejsou problémy s nasazení, může vypadat při [nasazení skupina zdroje Poradce při potížích s Azure portálu](../resource-manager-troubleshoot-deployments-portal.md)
