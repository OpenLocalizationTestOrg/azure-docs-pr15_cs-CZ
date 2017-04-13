<properties
   pageTitle="Nasazení VMs NIC více pomocí Powershellu v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se nasadit VMs NIC více pomocí Powershellu v modelu klasické nasazení"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Nasazení více NIC VMs (klasické) pomocí prostředí PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Můžete vytvořit virtuálních počítačích (VMs) v Azure a připojit víc sítí rozhraní (NIC) na jednotlivé vaší VMs. Více nic povolit oddělení typy přenosů přes nic. Například jeden NIC může komunikovat s Internet, zatímco jiné informuje uživatele o jenom s interních zdrojů nejste připojení k Internetu. Možnost oddělit sítě přenosy napříč několika nic je potřebný pro mnoho sítě virtuální spotřebiče, například aplikace doručení a sítě WAN optimalizační řešení.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

V současné době nejde máte VMs s jednoho NIC a VMs s více nic ve stejném cloudové služby. Proto budete muset implementovat servery back-end v jiné cloudové službě než a jiné součásti v scénáře. Postupem použít do cloudové služby s názvem *IaaSStory* na hlavní zdroje a *IaaSStory back-end* serverů back-end.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před nasazením servery back-end, budete muset nasazení hlavní cloudovou službu s všechny potřebné prostředky pro tento scénář. Minimálně je potřeba vytvořit virtuální sítě s podsítí pro back-end. Navštivte [vytvořit virtuální sítě pomocí prostředí PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) se dozvíte, jak nasadit virtuální sítě.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Nasazení back-end VMs

VMs back-end, závisí na vytváření odkazů níže.

- **Back-end podsítě**. Databázový server bude být součástí samostatné podsítě oddělit přenosy. Skript dole předpokládá, že tento podsítě existovat v vnet s názvem *WTestVnet*.
- **Účet úložiště pro data discích**. Pro lepší výkon disků dat na serverech databáze bude technologii plné stavu jednotka (SSD), který vyžaduje účet úložiště premium. Zkontrolujte Azure umístění nasadíte podporuje premium úložiště.
- **Nastavte dostupnost**. Všechny databáze servery se přidají do jednoho dostupnost nastavit, a zkontrolujte, že alespoň jedno z VMs nahoru a systémem údržbě.

### <a name="step-1---start-your-script"></a>Krok 1: zahájení skriptu

Můžete si stáhnout celou skript Powershellu použitý [v tomto poli](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Postupujte podle pokynů a změnit skriptu pro práci v prostředí.

1. Změňte hodnoty pod proměnných založených na existující skupiny zdrojů nasazený nad [předpokladů](#Prerequisites).

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Změňte hodnoty proměnných dole na základě hodnot, které chcete použít pro nasazení back-end.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 – Vytvoření potřebné prostředky pro vaše VMs

Je potřeba vytvořit nové Cloudová služba a účet úložiště disků data pro všechny VMs. Budete potřebovat k určení obrázek a místního účtu správce pro VMs. Pokud chcete vytvořit tyto materiály, proveďte následující kroky.

1. Vytvoření nového cloudové služby.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Vytvoření nového účtu úložiště premium.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Nastavení účtu úložiště vytvořená nad aktuálním účtu úložiště u předplatného.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Vyberte obrázek OM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Nastavte přihlašovací údaje účtu místního správce.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Krok 3 – vytvoření VMs

Je potřeba použít smyčku vytvořit tolik VMs jako a vytvořte potřebné nic a VMs v obraze. Pokud chcete vytvořit nic a VMs, proveďte následující kroky.

1. Začněte `for` opakovat opakovat příkazy k vytvoření virtuálního počítače a dvě nic podle potřeby několikrát podle hodnoty `$numberOfVMs` proměnné.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Vytvoření `VMConfig` objekty obrázek, velikost a dostupnost pro OM.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Poskytování OM jako Windows OM.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Nastavení výchozího NIC a přiřadit ji statické IP adresy.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Přidejte druhý NIC pro každou OM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Vytvoření disků dat pro každou OM.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Vytvoření každého OM a smyčku.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Krok 4 – spuštění skriptu

Teď, když jste si stáhli a změnit skript podle vašich potřeb, o mu skript vytvořit back-end databáze VMs s více nic.

1. Uložte skript a spusťte z příkazového **prostředí PowerShell** nebo **ISE Powershellu**. Zobrazí se výstupu počáteční a jak je ukázáno v následujícím příkladu.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Vyplňte informace potřebné ve výzvě k zadání přihlašovacích údajů a klikněte na **OK**. Zobrazí se výstupu dole.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
