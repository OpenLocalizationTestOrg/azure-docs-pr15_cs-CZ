<properties
    pageTitle="Vytvoření OM z generalized virtuálního pevného disku | Microsoft Azure"
    description="Zjistěte, jak vytvořit virtuální počítač Windows z obrázku generalized virtuální pevný disk pomocí prostředí PowerShell Azure v modelu nasazení Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Vytvoření virtuálního počítače z obrázku generalized virtuální pevný disk

Generalized obrázek virtuální pevný disk byla všech informací vaší osobní účet odebrat pomocí [Sysprep](virtual-machines-windows-generalize-vhd.md). Spuštěním Sysprep na místní OM, pak [nahrávání virtuální pevný disk na Azure](virtual-machines-windows-upload-image.md)nebo Sysprep spuštěna e existující OM Azure a potom [kopírování virtuální pevný disk](virtual-machines-windows-vhd-copy.md), můžete vytvořit generalized virtuálního pevného disku.

Pokud chcete vytvořit virtuálního počítače z specializované virtuálního pevného disku, najdete v článku [Vytvoření OM z specializované virtuálního pevného disku](virtual-machines-windows-create-vm-specialized.md).

Nejrychlejším způsobem, jak vytvořit virtuálního počítače z generalized virtuálního pevného disku, je použít [úvodní šablony] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud se chystáte používat virtuálního pevného disku nahrané z místního OM, jako jsou vytvořené pomocí Hyper-V, měli byste zkontrolovat jste postupovali podle pokynů v tématu [Příprava Windows virtuální pevný disk na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Nahrané VHD existující VHD OM Azure musí i generalized, než budete moct vytvářet virtuálního počítače tímto způsobem. Další informace najdete v tématu [použití Sysprep generalizace virtuálního počítače Windows](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Nastavit identifikátor URI virtuální pevný disk

Identifikátor URI virtuálního pevného disku používat je ve formátu: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**VHD. V tomto příkladu s názvem **myVHD** virtuálního pevného disku je v účtu úložiště **mystorageaccount** v kontejneru **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Vytvořit virtuální sítě

Vytvoření vNet a podsítí [virtuální sítě](../virtual-network/virtual-networks-overview.md).


1. Vytvoření podsítě. Následující příklad vytvoří podsítě s názvem **mySubnet** skupina zdroje **myResourceGroup** s předponou adresu **10.0.0.0/24**.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Vytvořte virtuální sítě. Následující příklad vytvoří virtuální sítě s názvem **myVnet** **Západní USA** umístění s předponou adresu **10.0.0.0/16**.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Vytvoření veřejného IP adresa a síťové rozhraní

Povolit komunikaci s virtuálního počítače v virtuální sítě, musíte na [veřejnou IP adresu](../virtual-network/virtual-network-ip-addresses-overview-arm.md) a rozhraní sítě.

1. Vytvořte veřejnou IP adresu. Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Vytvoření síťovou Tento příklad vytvoří NIC s názvem **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Vytvoření skupiny zabezpečení sítě a pravidlo RDP

Abyste mohli přihlásit k vaší OM pomocí RDP, musíte mít zabezpečení pravidlo, které vám umožní RDP na port 3389. 

Tento příklad vytvoří NSG s názvem **myNsg** obsahující pravidlo s názvem **myRdpRule** umožňující RDP přenosy přes port 3389. Další informace o NSGs najdete v tématu [otevření portů angličtině v Azure pomocí Powershellu](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Vytvořit proměnnou pro virtuální sítě

Vytvořte proměnnou pro dokončení virtuální sítě. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Vytvoření OM

Tento skript Powershellu ukazuje, jak nastavit konfigurace virtuálního počítače a nahrané OM obrázek použít jako zdroj pro novou instalaci.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Zkontrolujte, že byl vytvořen OM 

Až budete hotovi, byste měli vidět nově vytvořený OM [Azure portál](https://portal.azure.com) klikněte v části **Procházet** > **virtuálních počítačích**, nebo můžete použít následující příkazy Powershellu:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky

Správě nové virtuálního počítače s Azure Powershellu najdete v tématu [Správa virtuálních počítačích pomocí Správce prostředků Azure a Powershellu](virtual-machines-windows-ps-manage.md).


