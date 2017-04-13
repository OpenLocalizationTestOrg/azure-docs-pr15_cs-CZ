<properties
    pageTitle="Vytvořit virtuální počítač měřítko sadu pomocí prostředí PowerShell | Microsoft Azure"
    description="Vytvořit virtuální počítač měřítko sadu pomocí prostředí PowerShell"
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
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Vytvořit sadu pomocí prostředí PowerShell Azure měřítko virtuálního počítače systému Windows

Tento postup sledovat doplnění-v –-prázdných buněk přístup k vytvoření sady měřítko Azure virtuálního počítače. V tématu [Přehled Nastaví měřítko virtuálního počítače](virtual-machine-scale-sets-overview.md) zobrazíte další informace o sady měřítko.

By měl trvá asi 30 minut kroků v tomto článku.

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalace modulu Azure PowerShell

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="step-2-create-resources"></a>Krok 2: Vytvoření zdroje

Vytvořte prostředky, které jsou potřebné pro novou sadu měřítko.

### <a name="resource-group"></a>Pole Skupina zdroje

Sada měřítko virtuální počítač musí být součástí skupiny zdrojů.

1. Získání seznamu dostupná umístění umístění zdroje:

        Get-AzureLocation | Sort Name | Select Name

2. Vyberte umístění, do kterého je pro vás nejlepší, nahraďte hodnotu **$locName** tohoto názvu umístění a pak vytvořte proměnné:

        $locName = "location name from the list, such as Central US"

3. Nahraďte hodnotu **$rgName** název, který chcete použít pro nová skupina zdrojů a pak vytvořte proměnnou: 

        $rgName = "resource group name"
        
4. Vytvoření skupiny zdrojů:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Měli byste vidět vypadá podobně jako tento příklad:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Úložiště účtu

Účet úložiště používá virtuálního počítače k ukládání disk operačního systému a diagnostiky data použitá pro změnu velikosti. Pokud je to možné, je nejvhodnější s účtem úložiště pro každou virtuální počítač vytvořené v sadě měřítko. Pokud není možné, plán pro víc než 20 VMs jednoho účtu úložiště. Příklad v tomto článku ukazuje tři účty úložiště vzniku pro tři virtuálních počítačích.

1. Nahraďte hodnotu **$saName** název účtu úložiště. Otestujte název jedinečnost. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Pokud odpověď je **PRAVDA**, je jedinečný navržený název.

3. Nahraďte hodnotu **$saType** typu vašeho účtu úložiště a pak vytvořte proměnnou:  

        $saType = "storage account type"
        
    Možné hodnoty jsou: Standard_LRS, Standard_GRS, Standard_RAGRS nebo Premium_LRS.
        
4. Vytvoření účtu:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Měli byste vidět vypadá podobně jako tento příklad:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Opakujte kroky 1 až 4 a vytvořte tři účty úložiště, třeba myst1 myst2 a myst3.

### <a name="virtual-network"></a>Virtuální sítě

Virtuální sítě je potřebný pro virtuálních počítačích v sadě měřítko.

1. Hodnota **$subnetName** nahraďte názvem, který chcete použít pro podsítě v virtuální sítě a vytvořte proměnnou: 

        $subnetName = "subnet name"
        
2. Vytvoření konfigurace podsítě:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Předpona adresy se může lišit v síti virtuální.

3. Hodnota **$netName** nahraďte názvem, který chcete použít pro virtuální sítě a pak vytvořte proměnnou: 

        $netName = "virtual network name"
        
4. Vytvořte virtuální sítě:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfigurace sady měřítko

Máte všechny zdroje, které je potřeba měřítka nastavení, a tak se Pojďme vytvořit.  

1. Nahraďte hodnotu **$ipName** název, který chcete použít pro konfiguraci IP a pak vytvořte proměnnou: 

        $ipName = "IP configuration name"
        
2. Vytvoření konfigurace IP:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Hodnota **$vmssConfig** nahraďte názvem, který chcete použít pro konfiguraci nastavení měřítko a pak vytvořte proměnnou:   

        $vmssConfig = "Scale set configuration name"
        
3. Vytvoření konfigurace sady měřítko:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Tento příklad ukazuje, že měřítkem nastavený vzniku s tři virtuálních počítačích. Další informace o kapacita měřítko sady najdete [Přehled Nastaví měřítko virtuálního počítače](virtual-machine-scale-sets-overview.md) . Tento krok je také nastavení velikosti (jako takzvaný SkuName) virtuálních počítačích v sadě. Velikost, která vám vyhovuje, hledejte na [velikosti virtuálních počítačích](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Konfigurace nastavení měřítko přidáte rozhraní konfigurace sítě:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Měli byste vidět vypadá podobně jako tento příklad:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Operační systém profilu

1. Nahraďte hodnotu **$computerName** s předponou název počítače, který chcete použít a pak vytvořte proměnnou: 

        $computerName = "computer name prefix"
        
2. Nahraďte hodnotu **$adminName** název účtu správce na virtuálních počítačích a vytvořte proměnnou:

        $adminName = "administrator account name"
        
3. Nahraďte hodnotu **$adminPassword** heslo účtu a vytvořte proměnnou:

        $adminPassword = "password for administrator accounts"
        
4. Vytvoření profilu operační systém:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Úložiště profilu

1. Hodnota **$storageProfile** nahraďte názvem, který chcete používat pro profil úložiště a pak vytvořte proměnnou:  

        $storageProfile = "storage profile name"
        
2. Vytvořte proměnné, které definují obrázku:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Informace o dalších obrázků použít, hledejte na [obrázek a vyberte Azure virtuálního počítače obrázků se prostředí Windows PowerShell a rozhraní příkazového řádku Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Nahraďte hodnotu **$vhdContainers** seznam, který obsahuje cesty, které jsou uložené virtuální pevných discích, například "https://mystorage.blob.core.windows.net/vhds" a pak vytvořte proměnnou:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Vytvoření profilu úložiště:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Nastavení měřítko virtuálního počítače

Nakonec můžete vytvořit sadu měřítko.

1. Nahraďte hodnotu **$vmssName** název sady měřítko virtuálního počítače a pak vytvořte proměnnou:

        $vmssName = "scale set name"
        
2. Vytvoření sady měřítko:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Měli byste vidět vypadá podobně jako tento příklad ukazuje úspěšné zavedení:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Krok 3: Prozkoumání zdroje

Použijte následující materiály, které můžete prozkoumat sadu měřítko virtuální počítač, který jste vytvořili:

- Azure portálu - omezené množství informací neexistuje na portálu.
- [Azure zdroje Explorer](https://resources.azure.com/) - tento nástroj je nejlepší pro prohlížení aktuální stav sadě měřítko.
- Azure PowerShell – pomocí tohoto příkazu zobrazíte informace:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Další kroky

- Správa nastavení měřítko, že jste právě vytvořili podle pokynů v [Správa virtuálních počítačích v sadě měřítko virtuálního počítače](virtual-machine-scale-sets-windows-manage.md)
- Zvažte možnost nastavit automatické změny velikosti vašeho rozsahu nastavte pomocí informací v [Nastaví automatické změny velikosti a virtuálního počítače měřítko](virtual-machine-scale-sets-autoscale-overview.md)
- Další informace o svislé měřítko kontrolou [svislé automatické měřítko sadami měřítko virtuálního počítače](virtual-machine-scale-sets-vertical-scale-reprovision.md)
