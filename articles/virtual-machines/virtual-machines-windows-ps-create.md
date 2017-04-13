<properties
    pageTitle="Vytvoření OM Azure pomocí prostředí PowerShell | Microsoft Azure"
    description="Pomocí prostředí PowerShell Azure a správce prostředků Azure snadno vytvářet nové OM spuštěný Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Vytvoření Windows OM pomocí Správce zdrojů a prostředí PowerShell

V tomto článku se dozvíte, jak můžete rychle vytvořit Azure virtuální počítač systému Windows Server a materiály potřebné pomocí [Správce zdrojů](../azure-resource-manager/resource-group-overview.md) a Powershellu. 

Všechny potřebné kroky v tomto článku jsou potřebné k vytvoření virtuálního počítače a měli trvá asi 30 minut kroků. Příklad hodnoty parametrů v seznamu Příkazy nahraďte názvy, které vyhovují prostředí.

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalace modulu Azure PowerShell

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.
        
## <a name="step-2-create-a-resource-group"></a>Krok 2: Vytvoření skupiny zdrojů

Všechny zdroje musí být součástí skupiny zdrojů, takže umožňuje vytvořit nejdřív.  

1. Získání seznamu dostupná umístění, kde se dají vytvářet zdroje.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Nastavení umístění pro zdroje. Tento příkaz nastaví umístění **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Vytvoření skupiny prostředků. Příkaz vytvoří skupina zdroje s názvem **myResourceGroup** do umístění, které nastavíte.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Krok 3: Vytvoření účtu úložiště

[Účet úložiště](../storage/storage-introduction.md) je potřeba k ukládání virtuální pevného disku, který používá virtuální počítač, který vytvoříte. Názvy účtů úložiště musí být mezi 3 a 24 znaků a může obsahovat čísla pouze malými písmeny.

1. Otestujte název účtu úložiště pro jedinečnost. Tento příkaz testuje název **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Pokud tento příkaz vrátí **hodnotu PRAVDA**, je jedinečný v rámci Azure navržený název. 
    
2. Dále vytvořte účet úložiště.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Krok 4: Vytvoření virtuální sítě

Všechny virtuálních počítačích jsou součástí [virtuální sítě](../virtual-network/virtual-networks-overview.md).

1. Vytvoření podsítě pro virtuální sítě. Příkaz vytvoří podsítě s názvem **mySubnet** předponou adresu 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Dále vytvořte virtuální sítě. Příkaz vytvoří virtuální sítě s názvem **myVnet** pomocí podsítě, kterou jste vytvořili a předponu adresy **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Krok 5: Vytvoření veřejné IP adresa a síťové rozhraní

Povolit komunikaci s virtuálního počítače v virtuální sítě, musíte na [veřejnou IP adresu](../virtual-network/virtual-network-ip-addresses-overview-arm.md) a rozhraní sítě.

1. Vytvořte veřejnou IP adresu. Příkaz vytvoří veřejnou IP adresu s názvem **myPublicIp** metodou přidělení z **dynamické**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Vytvoření rozhraní sítě. Tento příkaz vytvoří rozhraní sítě s názvem **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Krok 6: Vytvoření virtuálního počítače

Teď, když máte všechny části na místě je čas vytvoření virtuálního počítače.

1. Spusťte tento příkaz Správce jména a hesla pro virtuální počítač.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Heslo musí být v 12 123 znaků a alespoň jeden malými písmeny znak, jeden znak na velká písmena, jedno číslo a jeden speciální znak. 
        
2. Vytvoření objektu konfigurace virtuálního počítače. Příkaz vytvoří konfigurace objekt s názvem **myVmConfig** , který definuje název OM a velikost OM.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Seznam dostupných formátů virtuálního počítače najdete v článku [velikosti virtuálních počítačích v Azure](virtual-machines-windows-sizes.md) .
    
3. Konfigurace nastavení operačního systému OM. Tento příkaz nastaví název počítače, typ operačního systému a přihlašovací údaje účtu OM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definování obrázku zřízení OM. Tento příkaz definuje obrázek systému Windows Server pro účely OM. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Další informace o výběru obrázky, které chcete použít najdete v článku [obrázek a vyberte obrázky virtuálního počítače Windows v Azure pomocí prostředí PowerShell nebo rozhraní příkazového řádku](virtual-machines-windows-cli-ps-findimage.md).
        
5. Přidání rozhraní sítě, který jste vytvořili v konfiguraci.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Definujte název a umístění OM pevném disku. Virtuální pevný disk soubor uložený v kontejneru. Příkaz vytvoří disk v kontejneru s názvem **vhds/WindowsVMosDisk.vhd** úložiště účet, který jste vytvořili.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Přidejte informace disku operační systém OM konfiguraci. Nahraďte hodnotu **$diskName** název disku operační systém. Vytvořte proměnnou a přidejte informace o disku konfiguraci.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Nakonec vytvořte virtuální počítač.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Další kroky

- Pokud došlo k problémy s nasazení, dalším krokem bude visiové [nasazení skupina zdroje Poradce při potížích s Azure portálu](../resource-manager-troubleshoot-deployments-portal.md)
- Naučte se spravovat virtuální počítač, který jste vytvořili kontrolou [Spravovat virtuálních počítačích pomocí Správce prostředků Azure a Powershellu](virtual-machines-windows-ps-manage.md).
- Využití výhod použití šablony k vytvoření virtuálního počítače pomocí informace v tématu [Vytvoření virtuálního počítače Windows se šablonou správce prostředků](virtual-machines-windows-ps-template.md)
