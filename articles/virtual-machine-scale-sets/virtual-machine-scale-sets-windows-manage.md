<properties
    pageTitle="Správa VMs v sadě měřítko virtuálního počítače | Microsoft Azure"
    description="Správa virtuálních počítačích ve počítače virtuální měřítku sadu pomocí prostředí PowerShell Azure."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Správa virtuálních počítačích v sadě měřítko virtuálního počítače

Ke správě virtuálních počítačích v sadě měřítko virtuálního počítače použijte úkoly v tomto článku.

Většinu úkolů, které se týkají správy virtuálního počítače v sadě měřítko vyžadují vědět ID instance počítače, který chcete spravovat. [Průzkumník zdroje Azure](https://resources.azure.com) slouží k vyhledání ID instance virtuálního počítače v sadě měřítko. Pomocí Průzkumníka zdroje taky ověřit stav úkolů, které můžete dokončit.

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="display-information-about-a-scale-set"></a>Zobrazit informace o sadě měřítko

Obecné informace o sadu měřítko, což je označovány jako zobrazení instanci můžete získat. Nebo můžete získejte podrobnější informace, jako jsou informace o zdrojích v sadě měřítko.

Nahraďte uvozovkách hodnoty název nebo skupina zdroje a měřítko nastavení a potom zadejte příkaz:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Vrátí přibližně takto:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Nahraďte uvozovkách hodnoty název zdroje skupiny a měřítko sady. Nahrazení *#* identifikátorem instance virtuální počítač, který chcete získat informace a potom ho spusťte:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Vrátí vypadá podobně jako tento příklad:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Spuštění virtuálního počítače v sadě měřítko

Nahraďte uvozovkách hodnoty název zdroje skupiny a měřítko sady. Nahrazení *#* identifikátorem virtuální počítač, který chcete spustit a pak ho spusťte:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku zdroje vidíme, že je stav instance **spuštěna**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Zahájení virtuálních počítačích v měřítku nastavit tak, že nepoužíváte parametr – ID instance.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Ukončení virtuálního počítače v sadě měřítko

Nahraďte uvozovkách hodnoty název zdroje skupiny a měřítko sady. Nahrazení *#* identifikátorem virtuální počítač, který chcete ukončit a znovu ho spusťte:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku zdroje jsme můžete vidět, stav instance **odebrána**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Zastavit virtuálního počítače a Ne zrušit, použijte parametr – StayProvisioned. Ukončení virtuálních počítačích v sadě tak, že nepoužíváte parametr – ID instance.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Restartujte počítač virtuální v sadě měřítko

Nahraďte názvem skupiny zdrojů a nastavte měřítko hodnoty v uvozovkách. Nahrazení *#* identifikátorem virtuální počítač, který chcete znovu spustit a pak ho spusťte:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Můžete restartovat virtuálních počítačích v sadě tak, že nepoužíváte parametr – ID instance.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Odebrání sady měřítko virtuálního počítače

Nahraďte názvem skupiny zdrojů a nastavte měřítko hodnoty v uvozovkách. Nahrazení *#* identifikátorem virtuální počítač, který chcete odebrat a pak ho spusťte:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Nastavení měřítko virtuálního počítače v celém dokumentu můžete odebrat tak, že nepoužíváte parametr – ID instance.

## <a name="change-the-capacity-of-a-scale-set"></a>Změna objemu sady měřítko

Můžete přidat nebo odebrat virtuálních počítačích změnou kapacita sadu. Pokud potřebujete sadu měřítko, který chcete změnit, nastavit kapacitu odpovídá vašim požadavkům na a potom aktualizujte měřítko sada se nové kapacity. V příkazech nahraďte názvem skupiny zdrojů a nastavte měřítko hodnoty v uvozovkách.

  $vmss = get-AzureRmVmss - ResourceGroupName "název skupiny prostředků" - VMScaleSetName "název sady měřítko" $vmss.sku.capacity = 5 aktualizace AzureRmVmss - ResourceGroupName "název skupiny prostředků"-název "měřítko název sady" - VirtualMachineScaleSet $vmss 

Pokud odstraňujete virtuálních počítačích ze sady měřítko, virtuálních počítačích s nejvyšší ID odeberou se nejprve.
