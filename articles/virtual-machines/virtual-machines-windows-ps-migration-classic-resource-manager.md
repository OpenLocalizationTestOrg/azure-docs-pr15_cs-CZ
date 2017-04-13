<properties
    pageTitle="Migrace zdrojů správci pomocí prostředí PowerShell | Microsoft Azure"
    description="Tento článek provede migrace podporované platformy IaaS zdrojů z klasického správci zdrojů Azure pomocí prostředí PowerShell Azure příkazů"
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
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migrace zdrojů IaaS od klasického správci zdrojů Azure pomocí prostředí PowerShell Azure

Tento postup ukazují, jak používat příkazy Powershellu Azure migrace infrastruktury jako zdroje služby (IaaS) z modelu klasické nasazení modelu nasazení Správce prostředků Azure. 

Pokud chcete, můžete taky migrovat zdroje pomocí [rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Základní informace o podporovaných migrace scénáře najdete v tématu [podporované platformy migrace IaaS zdrojů z klasické správci zdrojů Azure](virtual-machines-windows-migration-classic-resource-manager.md). 
- Podrobné pokyny a migrace návod najdete v tématu [technické hloubkové postupy na podporované platformy migrace z klasické správci zdrojů Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Krok 1: Plánování migrace

Tady je několik doporučené postupy, které doporučujeme jak zjistit migrujete IaaS zdroje z klasické správci zdrojů:

- Přečtěte si projděte [podporované a nepodporované funkce a konfigurace](virtual-machines-windows-migration-classic-resource-manager.md). Pokud máte virtuálních počítačích, které používají nepodporované konfigurace nebo funkce, doporučujeme čekat funkce a konfigurace podpory bude upřesněno. Můžete taky vyhovuje vašim potřebám, odebrat tuto funkci nebo přesunete mimo této konfigurace umožňující migrace.
-   Pokud máte automatizované skripty, které nasazení infrastrukturu a aplikací dnes, pokusu o vytvoření podobné nastavení testu pomocí tyto skripty pro migraci. Můžete také nastavit ukázkové prostředí pomocí portálu Azure.

>[AZURE.IMPORTANT]Virtuální sítě brány (Web Server, Azure ExpressRoute, aplikace brány, čárky webu) aktuálně nepodporuje pro migraci z klasického správci zdrojů. Pokud chcete migrovat síť klasické virtuální bránu, odeberte brány před spuštěním operace potvrzení přesunout síti (krok připravit umožnit spuštění aniž by odstranil brány). Po dokončení migrace se znovu připojíte brány v Azure správce.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Krok 2: Nainstalujte nejnovější verzi Azure PowerShell

Existují dva hlavní možnosti instalace Azure PowerShell: [Galerie prostředí PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) nebo [Webové platformy (WebPI)](http://aka.ms/webpi-azps). WebPI obdrží měsíční aktualizace. Galerie prostředí PowerShell získávání aktualizací trvale. Tento článek vychází z Azure PowerShell verze 2.1.0.

Pokyny k instalaci zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Krok 3: Nastavení předplatného a zaregistrovat migrace

Nejdřív začněte příkazovém řádku prostředí PowerShell. Migrace, budete muset nastavit prostředí pro obě classic a správce prostředků.

Přihlaste se ke svému účtu pro model správce prostředků.

```powershell
    Login-AzureRmAccount
```

Zjištění dostupné předplatných pomocí tento příkaz:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Nastavte Azure předplatné pro aktuální relaci. Tento příklad: Nastaví výchozí název předplatného na **Moje předplatné Azure**. Nahraďte název předplatného příklad vlastní. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Registrace je jednorázový kroku, ale musí ho udělat jednou před pokusem o migraci. Bez registrace, se zobrazí tato chybová zpráva: 

>   *BadRequest: Předplatné není registrovaný migraci.* 

Zaregistrovat se zprostředkovatelem zdroje migrace pomocí tento příkaz:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Počkejte prosím pět minut, než registrace k dokončení. Pomocí následujícího příkazu můžete zkontrolovat stav schválení:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Ujistěte se, že je RegistrationState `Registered` před pokračováním. 

Teď se přihlaste ke svému účtu pro klasické model.

```powershell
    Add-AzureAccount
```

Zjištění dostupné předplatných pomocí tento příkaz:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Nastavte Azure předplatné pro aktuální relaci. V tomto příkladu: Nastaví výchozí předplatné na **Moje předplatné Azure**. Nahraďte název předplatného příklad vlastní. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Krok 4: Ujistěte se, že máte dost jádra Azure správce prostředků virtuálního počítače v oblasti Azure aktuální nasazení nebo VNET

Pomocí následujícího příkazu Powershellu zkontrolovat aktuální počet jádra, ke kterým máte ve Správci zdrojů Azure. Další informace o kvótách základní, najdete v článku [limity a správce prostředků Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

V tomto příkladu kontroly dostupnosti v oblastí **Západní USA** . Nahraďte název oblasti příklad vlastní. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Krok 5: Spuštění příkazů, které chcete migrovat IaaS prostředků.

>[AZURE.NOTE] Jsou všechny operace zde popsané idempotent. Pokud máte problém než s nepodporovanou funkcí nebo Chyba konfigurace, doporučujeme, abyste opakovat připravit, zrušit nebo potvrdit operace. Platformu pokusí akci.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migrace virtuálních počítačích v cloudové službě (ale ne v virtuální síť)

Vytvořte požadovaný seznam cloudovým službám pomocí následujícího příkazu a potom vyberte cloudové služby, kterou chcete migrovat. Pokud VMs ve službě cloud jsou v síti virtuální nebo pokud mají webu nebo pracovního role, příkaz vrátí chybovou zprávu.

```powershell
    Get-AzureService | ft Servicename
```

Zjištění názvu nasazení ke cloudové službě. V tomto příkladu je název služby **Moje služby**. Nahraďte název služby příklad názvu služby. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Připravte se na virtuálních počítačích ve službě cloud migraci. Máte dvě možnosti můžete vybírat.

* **Možnost 1. Migrace VMs k vytvoření platformy virtuální síti**

    Nejdřív ověření, zda můžete migrovat cloudové služby, pomocí následujících příkazů:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Příkaz předchozí zobrazí upozornění a chyby, které blokují migrace. Pokud bude ověření úspěšné, pak můžete přesunout krok **připravit** :

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Možnost 2. Migrace do existující virtuální sítě v modelu nasazení Správce prostředků**

    Tento příklad nastaví na název skupiny zdroje **myResourceGroup**, názvu virtuální sítě **myVirtualNetwork** a názvu podsítě **mySubNet**. Nahraďte název v příkladu názvů vlastní zdrojů.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Nejdřív ověření, zda můžete migrovat cloudové služby, pomocí následujícího příkazu:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Příkaz předchozí zobrazí upozornění a chyby, které blokují migrace. Pokud bude ověření úspěšné, vy můžete pokračovat v následujícím kroku připravit:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Po úspěšném připravit operace s některou z předchozí možnosti dotaz Stav migrace VMs. Ujistěte se, že pocházejí v `Prepared` stavu.

V tomto příkladu nastaví název OM **myVM**. Nahraďte název příklad názvu OM.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Kontrola konfigurace připravené zdrojů pomocí prostředí PowerShell nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte tento příkaz:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migrace virtuálních počítačích v síti virtuální

Migrace virtuálních počítačích v síti virtuální migrace sítě. Virtuálních počítačích automaticky migrace se sítí. Vyberte virtuální sítě, ke které chcete migrovat. 

V tomto příkladu nastaví virtuální síťový název **myVnet**. Nahraďte název virtuální sítě příklad vlastní. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Pokud virtuální sítě obsahuje webu nebo pracovního role nebo VMs s nepodporované konfigurace, se zobrazí chybová zpráva funkce ověření.

Nejdřív ověření, zda pomocí následujícího příkazu můžete migrovat virtuální sítě:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Příkaz předchozí zobrazí upozornění a chyby, které blokují migrace. Pokud bude ověření úspěšné, vy můžete pokračovat v následujícím kroku připravit:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Kontrola konfigurace pro připravené virtuálních počítačích pomocí prostředí PowerShell Azure nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte tento příkaz:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migrace účet úložiště

Jakmile dokončíte migraci virtuálních počítačích, doporučujeme, abyste přenést účty úložiště.

Příprava každý účet úložiště migraci pomocí následujícího příkazu. V tomto příkladu je název účtu úložiště **myStorageAccount**. Příklad jména nahraďte název účtu úložiště. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Kontrola konfigurace účtu připravené úložiště pomocí prostředí PowerShell Azure nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte tento příkaz:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Další kroky

- Další informace o migraci najdete v tématu [podporované platformy migrace IaaS zdrojů z klasické správci zdrojů Azure](virtual-machines-windows-migration-classic-resource-manager.md).
- Migrace zdrojů další síťové zdroje správci pomocí prostředí PowerShell Azure, podobným způsobem s [Přesunout AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Přesunout AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)a [AzureRouteTable přesunout](https://msdn.microsoft.com/library/mt786718.aspx).
- Skripty otevřít zdroje, které můžete použít k migraci Azure zdrojů z klasické správci zdrojů najdete v článku [komunity nástroje pro migraci do Správce prostředků Azure migrace](virtual-machines-windows-migration-scripts.md)
