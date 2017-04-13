<properties
   pageTitle="Výhody použití Azure hybridní server okno | Microsoft Azure"
   description="Naučte se Maximalizace výhod Windows Server Software Assurance přenést místní licence Azure"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Výhody používání Azure hybridní v systému Windows Server

Zákazníkům, kteří používají systému Windows Server s Software Assurance můžete přenést vaší místní licencí serveru Windows Azure a spustit systému Windows Server VMs v Azure omezená platbou. Výhody použití hybridního Azure umožňuje běží Windows Server VMs v Azure a pouze získat vám účtovat poplatky pro kurz základní výpočetním. Další informace najdete v článku [výhody použití hybridního Azure licencování stránky](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Tento článek vysvětluje, jak nasazení systému Windows Server VMs v Azure používat tuto výhodu licencí.

> [AZURE.NOTE] K instalaci systému Windows Server VMs využití výhody použití hybridního Azure nelze použít obrázky z Azure Marketplace. Je třeba nasadit vaší VMs pomocí prostředí PowerShell nebo správce prostředků šablon správně zaregistrujte svůj VMs jako nárok na základní výpočetním diskontní sazba.

## <a name="pre-requisites"></a>Předpoklady
Abyste mohli využít Azure hybridní použití výhoda pro Windows Server VMs v Azure existuje několik předpoklady:

- Instalace modulu Azure PowerShell
- Virtuální pevný disk s serveru Windows nahráli k základnímu úložišti Azure

### <a name="install-azure-powershell"></a>Instalace modulu Azure PowerShell
Zkontrolujte, že jste [nainstalovali a nakonfigurovali nejnovější Azure Powershellu](../powershell-install-configure.md). I když se chystáte nasazení vaší VMs pomocí šablon správce prostředků, potřebujete nainstalovaný Nahrajte svůj virtuálního pevného disku serveru Windows Azure PowerShell (viz podle následujících pokynů).

### <a name="upload-a-windows-server-vhd"></a>Nahrání systému Windows Server virtuální pevný disk

Abyste mohli nasadit OM serveru Windows v Azure, musíte nejdřív vytvořit virtuálního pevného disku, který obsahuje základní sestavení systému Windows Server. Tento virtuální pevný disk musí před odesláním Azure řádně podporovat připravit pomocí Sysprep. Můžete [Číst další informace o požadavcích virtuálního pevného disku a Sysprep obrázku](./virtual-machines-windows-upload-image.md) a [Sysprep podpora role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Před spuštěním Sysprep obecnějším údajům OM. Po předem připravených vaší virtuálního pevného disku, nahrajte virtuální pevný disk pomocí účtu Azure úložiště `Add-AzureRmVhd` rutina takto:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL serveru SharePoint Server a Dynamics můžete taky využít Software Assurance licence. Potřebujete Příprava obrázek systému Windows Server instalaci součástí aplikace a poskytování licenční klíče příslušným způsobem a následným odesílá obrázek disku Azure. Odpovídající dokumentaci pro spuštění programu Sysprep s aplikací, jako je [Důležité informace o instalaci SQL serveru pomocí Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) nebo [Vytvoření obrázku SharePoint serveru 2016 odkaz (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Můžete taky Další informace o [nahrávání virtuální pevný disk Azure procesu](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Tento článek se zaměřuje na nasazení systému Windows Server VMs. Můžete taky nasadíte VMs klienta Windows stejným způsobem. V následujících příkladech nahradíte `Server` s `Client` řádně podporovat.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>OM pomocí prostředí PowerShell rychlého nasazení
Když nasadíte vaší systému Windows Server OM prostřednictvím Powershellu, které mají další parametr, pro `-LicenseType`. Až budete mít vaše virtuální pevný disk nahráli Azure, vytvoříte novou OM pomocí `New-AzureRmVM` a zadejte licencování následujícím způsobem:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Můžete [Číst další podrobné informace o nasazení OM v Azure pomocí prostředí PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) dole nebo přečtěte si podrobnější příručku na různé kroky k [Vytvoření Windows OM pomocí Správce zdrojů a Powershellu](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Nasazení OM prostřednictvím Správce prostředků
V rámci šablony správce prostředků další parametr pro `licenseType` lze zadat. Další informace o [vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md). Až budete mít vaše virtuální pevný disk nahráli Azure, upravte, správce prostředků šablonu jako součást poskytovatele výpočetním zahrnout typu licence a nasazení šablony jako obvykle:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Ověřte, jestli že vaše OM používá licencování výhod
Po zavedení vaší OM prostřednictvím buď prostředí PowerShell nebo správce prostředků nasazení metody, ověřte typu licence s `Get-AzureRmVM` následujícím způsobem:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Výstup vypadá přibližně takto:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

To porovnání s následující OM nasazeny bez licencování výhod použití hybridního Azure, například OM nasazený přímo z Galerie Azure:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Podrobný postup prostředí PowerShell

Následující podrobné prostředí PowerShell kroků plné nasazení virtuálního počítače. Přečtěte si další kontext skutečné rutiny a různých složek vytvořených v [vytvořit Windows OM pomocí Správce zdrojů a Powershellu](./virtual-machines-windows-ps-create.md). Přecházet mezi vytváření pole Skupina zdroje, účtu úložiště a virtuální sítě, a pak definovat vaše OM a nakonec vytvořte svůj OM.
 
Nejdřív bezpečně získat přihlašovací údaje; zadejte umístění a název skupiny zdroje:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Vytvoření veřejné IP:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definování vaší podsítě NIC a VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Název vaší OM a vytvořte OM konfigurace:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definování váš operační systém:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Přidání svého NIC bude v angličtině:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definování úložiště účet, který chcete použít:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Nahrání souboru řádně počítat a připojte k OM pro použití:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Nakonec vytvořte svůj OM a definovat typ licencování využívat výhod použití hybridního Azure:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Další kroky

Další informace o [licencování výhod použití hybridního Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Další informace o [použití šablon správce prostředků](../azure-resource-manager/resource-group-overview.md).
