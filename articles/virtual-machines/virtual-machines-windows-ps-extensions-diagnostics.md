<properties
    pageTitle="Použití Powershellu ke povolit Azure Diagnostika v virtuálního počítače s Windows | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Zjistěte, jak pomocí Powershellu povolit Azure diagnostiky virtuálního počítače s Windows"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Použití Powershellu ke povolit Azure Diagnostika v virtuálního počítače s Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure diagnostiky je možnost v Azure, která umožňuje shromažďování diagnostických informací o nasazení aplikace. Rozšíření diagnostiky umožňuje shromáždit data diagnostiky jako protokoly aplikace nebo výkonnosti Azure virtuální počítače (OM), který se systémem Windows. Tento článek popisuje, jak používat Windows PowerShell pro povolení koncovku diagnostických nástrojů pro virtuálního počítače. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) pro požadavcích pro tento článek.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Povolení rozšíření diagnostických nástrojů použijete nasazení modelu správce prostředků

Rozšíření diagnostiky můžete povolit při vytvoření OM Windows prostřednictvím nasazení modelu správce prostředků Azure přidáním konfigurace rozšíření šabloně správce prostředků. V tématu [Vytvoření Windows virtuálního počítače s sledování a diagnostice pomocí šablony správce prostředků Azure](virtual-machines-windows-extensions-diagnostics-template.md).

Povolit na Diagnostika příponu existující OM, který byl vytvořený prostřednictvím nasazení modelu správce prostředků, můžete použít rutinu prostředí PowerShell [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) , jak je ukázáno v následujícím příkladu.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* je cesta k souboru, který obsahuje konfigurace diagnostiky ve formátu XML, jak je uvedeno v [ukázkové](#sample-diagnostics-configuration) následující.  

Pokud soubor konfigurace diagnostiky určuje prvek **StorageAccount** s název účtu úložiště, pak skript *Sady AzureRMVMDiagnosticsExtension* automaticky nastaví koncovku diagnostických nástrojů na odeslat diagnostické data k tomuto účtu úložiště. Tento postup vyžaduje účet úložiště musí být v rámci stejného předplatného jako OM.

Pokud žádná **StorageAccount** zadaná v konfiguraci diagnostických nástrojů, musíte v parametru *StorageAccountName* předat rutiny. Pokud zadaný parametr *StorageAccountName* budou rutinu vždy používat úložiště účet, který je určené parametrem a ne ten, který není zadán v souboru konfigurace diagnostiky.

Pokud účet úložiště Diagnostika v jiné předplatné z OM, musíte explicitně v parametrech *StorageAccountName* a *StorageAccountKey* předat rutiny. Při účet úložiště diagnostiky je ve stejném předplatném rutinu můžete automaticky dotazu a nastavit hodnoty klíče při aktivaci koncovku Diagnostika není potřeba parametr *StorageAccountKey* . Však-li účet úložiště Diagnostika v jiné předplatné, pak rutinu nebudete moct automaticky získat klávesu a budete muset explicitně zadat klíč prostřednictvím parametr *StorageAccountKey* .  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Po rozšíření diagnostiky je zapnuta virtuálního počítače, získáte pomocí rutiny [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) aktuální nastavení.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Rutiny vrátí *PublicSettings*, která obsahuje konfigurace XML ve formátu kódováním Base 64. Číst XML, budete muset dekódovat.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Rutina [Odebrat AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) mohou sloužit k odebrání OM koncovku diagnostických nástrojů.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Povolení rozšíření diagnostických nástrojů použijete modelu klasické nasazení

Povolení rozšíření diagnostiky na OM, který vytvoříte pomocí klasické nasazení modelu můžete použít rutinu [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Následující příklad ukazuje, jak vytvořit nový OM prostřednictvím klasické nasazení modelu s příponou diagnostiky povolené.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Povolení rozšíření diagnostiky na existující OM, který byl vytvořený prostřednictvím klasické nasazení modelu, napřed pomocí rutiny [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) získání konfigurace OM. Aktualizujte konfigurace OM příponu diagnostiky pomocí rutiny [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Nakonec použití aktualizovaná konfigurace bude v angličtině pomocí [AzureVM aktualizace](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Konfigurace diagnostiky ukázka

Následující kód XML se dá použít pro veřejné konfigurace diagnostiky pomocí výše uvedených skriptů. Tato ukázka konfigurace přenese výkonnosti různých úložiště na Diagnostika účet spolu s chybami z aplikace, zabezpečení a systém kanálů v protokolu událostí systému Windows a všechny chyby v protokolech diagnostiky infrastruktury.

Konfigurace, musí být aktualizuje tak, aby patřit následující úkoly:

- Atribut *resourceID* elementu **metriky** , musí být aktualizovány číslo ID zdroje pro OM.
    - Číslo ID zdroje můžete vytvořen pomocí následujícího vzorce: "/ předplatná / {*ID předplatného pro předplatné s OM*} /resourceGroups/ {*název resourcegroup OM*} / providers/Microsoft.Compute/virtualMachines/ {*Název OM*}".
    - Například pokud ID předplatného pro předplatné, kde je spuštěný OM je **11111111-1111-1111-1111-111111111111**, název skupiny zdrojů Skupina zdroje je **MyResourceGroup**a název OM **MyWindowsVM**, pak hodnota *resourceID* by být:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Další informace o tom, metriky vygenerují podle konfigurace čítačů a metriky výkonu najdete v tématu [Azure diagnostiky metriky tabulky v úložišti](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- **StorageAccount** element musí být aktualizovány název účtu úložiště diagnostických nástrojů.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Další kroky
- Další informace o použití funkce Azure Diagnostika a jinými postupy k řešení problémů s najdete v článku [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnostika konfigurace schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) vysvětluje různé možnosti konfigurace XML pro diagnostiku rozšíření.
