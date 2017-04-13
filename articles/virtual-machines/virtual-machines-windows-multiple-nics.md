<properties
   pageTitle="Vytvoření OM Windows s více nic | Microsoft Azure"
   description="Naučte se vytvářet OM Windows s více nic připojené k pomocí prostředí PowerShell Azure nebo správce prostředků šablon."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Vytváření OM Windows s více nic
Vytvoření virtuálního počítače (OM) v Azure obsahující více virtuální sítě rozhraní (NIC) připojené k němu. Běžné situace bude mít různých podsítí pro připojení front-end a back-end nebo sítě snaží o sledování nebo záložní řešení. Tento článek obsahuje rychlý příkazy k vytvoření virtuálního počítače s více nic připojené k němu. Podrobné informace, včetně jak vytvořit víc nic v rámci vlastních skriptů Powershellu, přečtěte si další informace o [nasazení více NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Různé [formáty OM](virtual-machines-windows-sizes.md) podporují různý počet nic, tak velikost vaší OM příslušným způsobem.

>[AZURE.WARNING] Při vytváření virtuálního počítače – nic nelze přidat do existující OM, je nutné připojit víc nic. Můžete [vytvořit OM založený na původní virtuální discích](virtual-machines-windows-vhd-copy.md) a vytvořit více nic nasadit OM.

## <a name="create-core-resources"></a>Vytvoření základní prostředky
Ujistěte se, že máte [nejnovější Azure PowerShell nainstalovali a nakonfigurovali](../powershell-install-configure.md). Přihlaste se k účtu Azure:

```powershell
Login-AzureRmAccount
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myVM`.

Vytvoření skupiny zdrojů. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUs` umístění:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Vytvoření účtu úložiště pro uložení vaší VMs. Následující příklad vytvoří úložiště účet s názvem `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Vytváření virtuálních sítí a podsítí
Definování dva virtuální podsítí – jeden pro front-end komunikaci a jedno pro přenos back-end. Následující příklad definuje dva podsítí s názvem `mySubnetFrontEnd` a `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Vytvořte virtuální sítě a podsítí. Následující příklad vytvoří virtuální sítě s názvem `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Vytvoření více nic
Vytvořte dva nic připojení jeden NIC front-end podsítě a jeden NIC podsítě back-end. Následující příklad vytvoří dva nic s názvem `myNic1` a `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Obvykle můžete taky vytvořit [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) nebo spravovat a distribuce přenosy přes svůj VMs [Služba Vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) . [Podrobnější OM více NIC](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) článek vás provede vytvoření skupiny zabezpečení sítě a přiřazení nic.


## <a name="create-the-virtual-machine"></a>Vytvoření virtuálního počítače
Začněte vytvářet konfiguraci OM. Každou OM velikost má omezení pro celkový počet nic přidané do virtuálního počítače. Další informace o [velikosti OM Windows](virtual-machines-windows-sizes.md). 

Nejdřív nastavit přihlašovací údaje OM `$cred` proměnné takto:

```powershell
$cred = Get-Credential
```

Následující příklad definuje OM s názvem `myVM` a používá velikost OM, který podporuje až dva nic (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Vytvoření zbytek OM konfigurace. Následující příklad vytvoří virtuálního Windows serveru 2012 R2 počítače:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Dva nic dříve vytvořené připojení:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Nakonfigurujte úložiště a virtuální disk pro nové OM:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Nakonec vytvořte virtuálního počítače:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Vytvoření více nic pomocí Správce prostředků šablon
Azure správce prostředků šablony pomocí deklarativní soubory JSON prostředí definovat. Přečtěte si [informace o nástroji Azure zdroje správce](../azure-resource-manager/resource-group-overview.md). Správce prostředků šablony poskytují způsob, jak vytvořit víc instancí zdroj během nasazení, jako jsou vytváření více nic. Chcete-li zadat počet instancí vytvořit použít *kopírování* :

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Další informace o [vytváření více instancí *zkopírováním*](../resource-group-create-multiple.md). 

Můžete taky použít `copyIndex()` přidání čísla do název zdroje, který umožňuje vytvářet `myNic1`, `MyNic2`atd. Na následujícím obrázku je příklad přidávání hodnota indexu:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Přečtěte si kompletní příklad [vytváření více nic pomocí šablon správce prostředků](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte, že kontrola [Windows OM velikostí](virtual-machines-windows-sizes.md) při pokusu o vytvoření virtuálního počítače s více nic. Věnujte pozornost maximální počet NIC podporuje každý OM velikost. 

Myslete na to, že se nedají přidat další nic existující OM, musíte vytvořit všechny nic při nasazení OM. Dávejte při plánování nasazení a ujistěte se, že máte všechny požadované připojení k síti od začátku.