<properties
    pageTitle="Vytvoření SQL Server virtuálního počítače v Azure prostředí PowerShell (Správce zdrojů) | Microsoft Azure"
    description="Obsahuje postup a skriptů Powershellu pro vytváření Azure OM s obrázky v galerii virtuálního počítače SQL serveru."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Zřízení virtuálního serveru SQL Server počítače pomocí Powershellu Azure (Správce zdrojů)

> [AZURE.SELECTOR]
- [Portál](virtual-machines-windows-portal-sql-server-provision.md)
- [Prostředí PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak vytvořit jednu Azure virtuálního počítače pomocí nasazení modelu **Správce prostředků Azure** pomocí rutin prostředí PowerShell Azure. V tomto kurzu budeme vytvářet jednoho virtuálního počítače pomocí jednoho diskovou jednotku od obrázků v galerii SQL. Vytvoříme nových poskytovatelů úložiště, sítě a výpočetním prostředky, které se použije pro virtuální počítač. Pokud máte existující poskytovatelé některého z následujících zdrojích, můžete tyto poskytovatelů místo.

V případě potřeby klasické verzi v tomto tématu najdete v článku [poskytnutí virtuálního serveru SQL Server počítače pomocí Azure prostředí PowerShell klasické](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pro účely tohoto návodu budete potřebovat:

- Účet Azure a předplatné před spuštěním. Pokud nemáte jeden zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- [Azure Powershellu)](../powershell-install-configure.md), minimální verze 1.4.0 nebo novější (Tento kurz vytvořených pomocí verze 1.5.0).
    - K načtení svoji verzi, zadejte **Get-modul Azure ListAvailable**.

## <a name="configure-your-subscription"></a>Konfigurace předplatného

Otevřete Windows PowerShell a vytvořte přístup ke svému účtu Azure spusťte následující rutinu. Zobrazí se přihlašovací obrazovka k zadání přihlašovacích údajů. Použijte stejný e-mail a heslo, které používáte pro přihlášení k portálu Azure.

    Add-AzureRmAccount

Po úspěšném přihlášení uvidíte některé informace na obrazovce, která obsahuje SubscriptionId, se kterým jste se nepřihlásili. Toto je předplatné, ve kterém prostředky pro tento kurz se vytvoří není-li změnit na jiné předplatné. Pokud máte víc SubscriptionIds, spusťte následující rutinu zobrazíte seznam všech vaší SubscriptionIds:

    Get-AzureRmSubscription

Pokud chcete změnit na jiné SubscriptionID, spusťte následující rutinu s požadované SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definování proměnných obrázek

Pro zjednodušení použitelnosti a pochopení dokončené skript z tohoto kurzu, bude Začneme tím, že definujete počtu proměnných. Změňte hodnoty parametrů najdete v článku přizpůsobit, ale mějte na paměti názvových omezení související s délky název a speciálních znaků při úpravách hodnoty zadané.

### <a name="location-and-resource-group"></a>Umístění a pole Skupina zdroje
Umožňuje definovat oblast dat a skupina zdroje, do kterého chcete vytvořit další zdroje pro virtuální počítač dvě proměnné.

Podle potřeby a pak spusťte tyto rutiny, aby inicializace tyto proměnné.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Vlastnosti úložiště

Umožňuje definovat účtu úložiště a typ úložiště pro použití v počítači virtuální následující proměnné.

Změňte podle potřeby a potom spustit následující rutinu inicializace tyto proměnné. Všimněte si, že v tomto příkladu jsme používáte [Premium úložiště](../storage/storage-premium-storage.md), která je vhodné pro výrobní úloh. Podrobnosti o tyto pokyny a další doporučení najdete v tématu [výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Vlastnosti sítě

Umožňuje definovat rozhraní sítě, metodu přidělení TCP/IP, virtuální síťový název, název virtuální podsítě, rozsah IP adres pro virtuální sítě, rozsah IP adresy podsítě a nápisem veřejné domény název pro použití v síti ve počítače virtuální následující proměnné.

Změňte podle potřeby a potom spustit následující rutinu inicializace tyto proměnné.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Vlastnosti virtuálního počítače

Pomocí následujících proměnných definovat název virtuálního počítače, název počítače, velikost virtuálního počítače a název disku operační systém virtuálního počítače.

Změňte podle potřeby a potom spustit následující rutinu inicializace tyto proměnné.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Vlastnosti obrázku

Umožňuje definovat obrázku pro virtuální počítač následující proměnné. V tomto příkladě se používá obrázek SQL serveru 2016 Enterprise.

Změňte podle potřeby a potom spustit následující rutinu inicializace tyto proměnné.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Všimněte si, že se dostanete úplný seznam SQL Server Obrázek nabídky s příkazem Get-AzureRmVMImageOffer:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

A uvidíte umožňující nabídky s příkazem Get-AzureRmVMImageSku skladové jednotky. Následující příkaz zobrazuje že nabízejí všech SKU umožňující **SQL2014SP1 WS2012R2** .

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Správce prostředků nasazení modelu je první objekt, který vytvoříte skupina zdroje. Použijeme rutinu [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) vytvořit skupinu Azure prostředků a jeho zdroje s zdroje skupinu název a umístění definované proměnné, které jste předtím inicializován.

Spusťte následující rutinu k vytvoření nové skupiny prostředků.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

Virtuální počítač vyžaduje prostředků úložiště pro disk operačního systému a serveru SQL Server data a souborů protokolu. Pro zjednodušení vytvoříme jeden disk pro obě. Můžete připojit později otevřít pomocí rutiny [Přidat Azure disku](https://msdn.microsoft.com/library/azure/dn495252.aspx) při umístit data SQL serveru a souborů na vyhrazené discích protokolu další disk. Vytvoření účtu standardní ukládání do nové skupiny prostředků a název účtu úložiště, úložiště Sku název a umístění definované pomocí proměnných, které jste předtím inicializován použijeme rutinu [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) .

Spusťte následující rutinu vytvořte nový účet úložiště.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Vytvořit prostředky sítě

Virtuální počítač vyžaduje určitý počet zdrojů sítě pro připojení k síti.

- Každý virtuální počítač vyžaduje virtuální sítě.
- Virtuální síť musí obsahovat alespoň jeden podsítě definované.
- Rozhraní sítě musí být tato pole definovány veřejné nebo soukromé IP adresu.

### <a name="create-a-virtual-network-subnet-configuration"></a>Vytvoření konfigurace podsítě virtuální sítě

Bude začneme vytvořením konfigurace podsítě pro naše virtuální sítě. Pro naše kurz vytvoříme výchozí podsítě pomocí rutinu [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) . Vytvoříme naše virtuální konfiguraci podsítě předponou podsítě jména a adresy definované pomocí proměnných, které jste předtím inicializován.

>[AZURE.NOTE] Můžete definovat další vlastnosti virtuálního konfiguraci podsítě pomocí tuto rutinu, ale, která je nad rámec tohoto kurzu.

Spusťte následující rutinu vytvořit virtuální podsítě konfiguraci.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Vytvořit virtuální sítě

Dále vytvoříme naše virtuální sítě pomocí rutinu [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . Vytvoříme naše virtuální sítě do nové skupiny prostředků, název, umístění a adresa předpona definována pomocí proměnných, které jste předtím inicializován a pomocí konfigurace podsítě, který jste definovali v předchozím kroku.

Spusťte následující rutinu vytvořit virtuální sítě.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Vytvořit veřejnou IP adresu

Teď, když máme naše virtuální sítě definované, potřebujeme konfigurace IP adresa pro připojení k virtuální počítač. Pro účely tohoto návodu vytvoříme veřejnou IP adresu pomocí dynamického IP adres pro podporu připojení k Internetu. Použijeme rutinu [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) se dá vytvořit veřejnou IP adresu skupina zdroje vytvořili prevously a s název, umístění, metoda přidělení a definované pomocí proměnných, které jste předtím inicializován Popisek názvu domény DNS.

>[AZURE.NOTE] Můžete definovat další vlastnosti na veřejnou IP adresu pomocí tuto rutinu, ale, která je nad rámec počáteční kurzu. Je možné vytvořit také soukromé adresu nebo adresu statické adresou, ale, která je také nad rámec tohoto kurzu.

Spusťte následující rutinu vytvořit veřejnou IP adresu.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Vytvoření rozhraní sítě

Teď připraveni vytvořit rozhraní sítě využívající naše virtuálního počítače. Aby mohlo vzniknout naší sítě rozhraní ve skupině prostředků vytvořili dříve a s název, umístění, podsítě a veřejnou IP adresu dříve definované použijeme rutinu [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) .

Spusťte následující rutinu aby mohlo vzniknout rozhraní sítě.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurace objekt OM

Máme úložiště a síťové zdroje definované, jsme připravení k definování pro využití prostředků pro virtuální počítač Naše výukové jsme určete velikost virtuálního počítače a různé vlastnosti operační systém, zadejte rozhraní sítě, který jsme vytvořili, definujte úložiště objektů blob a pak zadejte disk operačního systému.

### <a name="create-the-vm-object"></a>Vytvoření objektu OM

Začneme bude zadáním velikost virtuálního počítače. Pro účely tohoto návodu jsme zadáváte DS13. Vytvoření objektu, která dokáže nahradit virtuálního počítače s názvem a velikost definované pomocí proměnných, které jste předtím inicializován použijeme rutinu [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) .

Spusťte následující rutinu vytvořit virtuální počítač objekt.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Vytvoření objektu přihlašovací údaje pro podržte jméno a heslo pro pověření místního správce

Abychom mohli nastavit operační systém vlastnosti virtuálního počítače, potřebujeme zadejte pověření místního účtu správce jako bezpečné řetězec. K tomu použijeme rutinu [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) .

Spustit následující rutinu a v okně prostředí Windows PowerShell pověření žádost o zadejte jméno a heslo pro účely místního účtu správce ve počítače virtuální Windows.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Nastavení vlastností operační systém virtuálního počítače

Teď je čas nastavit vlastnosti virtuálního počítače operační systém. Nastavení typu operační systém jako Windows, vyžadují [agent virtuální počítač](virtual-machines-windows-classic-agents-and-extensions.md) můžete nainstalovat, zadat rutinu umožňuje automatické aktualizace a nastavení s názvem virtuálního počítače, název počítače a přihlašovací údaje pomocí proměnných, které jste předtím inicializován použijeme rutinu [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) .

Spusťte následující rutinu a operační systém vlastnosti virtuálního počítače.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Přidání rozhraní sítě do virtuálního počítače

Pak přidáme rozhraní sítě, který jsme vytvořili dříve virtuálního počítače. Přidání rozhraní sítě pomocí rozhraní proměnné sítě, který jste definovali dříve použijeme rutinu [AzureRmVMNetworkInterface přidat](https://msdn.microsoft.com/library/mt619351.aspx) .

Spusťte následující rutinu nastavení rozhraní sítě pro virtuálního počítače.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Nastavení umístění v úložišti objektů blob disku pro použití v virtuálního počítače

Dále budou nastavení umístění v úložišti objektů blob disku pro použití v počítači virtuální pomocí proměnných, které jste definovali dříve.

Spusťte následující rutinu nastavit umístění úložiště objektů blob.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Nastavení vlastností disku pro virtuální počítač operační systém

Dále jsme nastaví operační systém disku vlastnosti virtuálního počítače. Určete, že operačního systému virtuálního počítače pocházet z obrázku, nastavení mezipaměti číst jenom (protože na stejném disku instaluje SQL Server) a definovat název virtuálního počítače a operační systém disku definované pomocí proměnných, která byla definována dříve použijeme rutinu [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) .

Spusťte následující rutinu a operační systém disku vlastnosti virtuálního počítače.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Určení obrázek platformy virtuálního počítače

Náš poslední krok konfigurace je zadat obrázek platformu pro naše virtuální počítač. Pro naše kurz používáme nejnovější SQL serveru 2016 CTP obrázek. Použijeme rutinu [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) použít tento obrázek definované proměnné, které jste definovali dříve.

Spusťte následující rutinu můžete určit obrázek platformu pro virtuálního počítače.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Vytvoření OM SQL

Teď, když dokončíte kroky konfigurace, jste připraveni vytvořit virtuální počítač. Vytvoření virtuálního počítače pomocí proměnných, která byla definována použijeme rutinu [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) .

Spusťte následující rutinu vytvoření virtuálního počítače.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Virtuální počítač se vytvoří. Všimněte si, že účet standardní ukládání se vytvoří spouštěcí Diagnostika protože zadaný úložiště pro virtuální počítač disk je premium úložiště účet.

Teď můžete zobrazit tento počítač v portálu Azure zobrazíte [jeho veřejnou IP adresu a jeho plně kvalifikovaný název domény](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Příklad skriptu

Tento skript obsahuje celý skript Powershellu pro účely tohoto návodu. Předpokládá, že už máte vzhled Azure předplatné pro použití s příkazy **AzureRmAccount přidat** a **Vyberte AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Další kroky
Po vytvoření virtuálního počítače jste připravení se připojit k počítači virtuální pomocí RDP a nastavení připojení. Další informace najdete v tématu [připojení k SQL serveru virtuálního počítače na Azure (Správce zdrojů)](virtual-machines-windows-sql-connect.md).
