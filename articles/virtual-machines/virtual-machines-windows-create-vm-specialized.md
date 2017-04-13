<properties
    pageTitle="Vytvoření kopie OM Windows | Microsoft Azure"
    description="Naučte se vytvořit kopii vaše specializované Azure OM s Windows, v modelu nasazení Správce prostředků."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Vytvoření virtuálního počítače z specializované virtuální pevný disk

Vytvoření nového OM připojením specializované virtuálního pevného disku jako s operačním systémem disk pomocí Powershellu. Speciální virtuálního pevného disku udržuje uživatelských účtů, aplikací a dalších dat stavu ze svého původního OM. 

Pokud chcete vytvořit virtuálního počítače z generalized virtuálního pevného disku, najdete v článku [Vytvoření OM z generalized obrázku virtuální pevný disk](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Vytvoření podsítě a vNet

Vytvoření vNet a podsítí [virtuální sítě](../virtual-network/virtual-networks-overview.md).

1. Vytvoření podsítě. Tento příklad vytvoří podsítě s názvem **mySubNet**ve skupině zdroje **myResourceGroup**a nastaví předponu adresy podsítě **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Vytvořte vNet. Tento příklad nastaví virtuální síťový název **myVnetName**, umístění, které chcete **Západní USA**a předponu adresy virtuální sítě pro **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Vytvořit veřejnou IP adresu a NIC

Povolit komunikaci s virtuálního počítače v virtuální sítě, musíte na [veřejnou IP adresu](../virtual-network/virtual-network-ip-addresses-overview-arm.md) a rozhraní sítě.

1. Vytvoření veřejné adresy IP. V tomto příkladu veřejnou IP adresu název nastavenou **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Vytvoření síťovou V tomto příkladu názvu NIC nastavenou **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Vytvoření skupiny zabezpečení sítě a pravidlo RDP

Abyste mohli přihlásit k vaší OM pomocí RDP, musíte mít zabezpečení pravidlo, které vám umožní RDP na port 3389. Protože virtuálního pevného disku pro nové OM byla vytvořená z existující specializované OM, po vytvoření OM je můžete použít existující účet z počítače virtuální zdroj, který měli oprávnění k přihlášení pomocí RDP.

V tomto příkladu nastaví název NSG **myNsg** a název pravidla RDP **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Další informace o pravidlech NSG a koncové body najdete v tématu [otevření portů angličtině v Azure pomocí Powershellu](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Vytvoření konfigurace OM

Nastavení konfigurace OM připojit zkopírovaný virtuální pevný disk jako s operačním systémem virtuální pevný disk.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Pokud máte disků dat, které je potřeba se připojit k OM, je třeba přidat také následující: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Operační systém disku URL a data vypadat nějak takhle: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. To můžete najít na portálu procházení cílového úložiště kontejneru, kliknutím na operačním systému nebo dat virtuálního pevného disku, který se zkopíroval a kopírování obsahu adresu URL.


## <a name="create-the-vm"></a>Vytvoření OM

Vytvoření OM pomocí konfigurace, které právě vytvořili.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Pokud tento příkaz podaří, zobrazí se výstup takto:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Zkontrolujte, že byl vytvořen OM 
 
Měli byste vidět nově vytvořený OM buď v [Azure portál](https://portal.azure.com), klikněte v části **Procházet** > **virtuálních počítačích**, nebo můžete použít následující příkazy Powershellu:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky

Přihlašovat do nových virtuálního počítače, přejděte na OM na [portál](https://portal.azure.com), klikněte na **Připojit**a otevřete soubor vzdálené plochy RDP. Přihlaste se k počítači nové virtuální pomocí přihlašovací údaje účtu virtuálního počítače původní. Další informace najdete v tématu [jak připojit a přihlaste se k Azure virtuální počítač s Windows](virtual-machines-windows-connect-logon.md).







