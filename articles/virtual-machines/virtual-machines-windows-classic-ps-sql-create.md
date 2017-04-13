<properties
    pageTitle="Vytvoření SQL Server virtuálního počítače v Azure prostředí PowerShell (klasický) | Microsoft Azure"
    description="Obsahuje postup a skriptů Powershellu pro vytváření Azure OM s obrázky v galerii virtuálního počítače SQL serveru. Toto téma používá režim klasické nasazení."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Zřízení virtuálního serveru SQL Server počítače pomocí Powershellu Azure (klasický)

## <a name="overview"></a>Základní informace

Tento článek obsahuje postup pro vytvoření virtuálního počítače SQL serveru v Azure pomocí rutin prostředí PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků verzi tohoto tématu najdete v článku [poskytnutí virtuálního serveru SQL Server počítače pomocí Správce prostředků prostředí PowerShell Azure](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Instalace a konfigurace prostředí PowerShell:

1. Pokud nemáte účet Azure, navštěvujte blog o [Azure bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

2. [Stáhněte si a nainstalujte nejnovější příkazy Powershellu Azure](../powershell-install-configure.md).

3. Spusťte Windows PowerShell a připojte ji k předplatnému Azure pomocí příkazu **Přidat AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Určit cílový Azure oblast

Počítač SQL Server virtuální bude použitý ve do cloudové služby, který je umístěn určité Azure oblasti. Podle těchto kroků umožňují určit oblast, účtu úložiště a cloudové služby, která se použije pro zbytek kurzu.

1. Určení datovém centru, které chcete použít k hostování vašeho OM SQL serveru. Následující příkazy Powershellu zobrazí oblasti k dispozici podrobně s souhrnný seznam na konci.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Jakmile jste určili preferovaného umístění, nastavte proměnnou s názvem **$dcLocation** do této oblasti.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Nastavení účtu úložiště a předplatného

1. Určení Azure předplatné, které budete používat pro nový virtuální počítač.

        (Get-AzureSubscription).SubscriptionName

1. Přiřaďte cílovém Azure předplatné **$subscr** proměnné. Nastavte to jako aktuálního předplatného Azure.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Vyhledejte stávajících účtů úložiště. Tento skript zobrazuje všechny účty úložiště, která existují ve vybrané oblasti:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Pokud požadujete do nového účtu úložiště, nejprve vytvořit název účtu úložiště všechna malá písmena pomocí příkazu Nový AzureStorageAccount jako v následujícím příkladu: **New AzureStorageAccount - StorageAccountName "<storage account name>"-umístění $dcLocation**

1. Název účtu úložiště cílové přiřadíte **$staccount**. Pomocí **Nastavení AzureSubscription** nastavte předplatné a aktuální účtu úložiště.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Vyberte obrázek virtuálního počítače SQL serveru

1. Přečtěte si seznam dostupných SQL Server virtuálních počítačích obrázky z galerie. Tyto obrázky mít **ImageFamily** vlastnost, která začíná na "SQL". Následující dotaz zobrazuje dostupné obrázek řady, že máte předinstalovaný serveru SQL Server.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Jakmile hledaný řady obrázek virtuálního počítače, může to být více publikované obrázky v této skupině. Použijte tento skript najít nejnovější název obrázek publikované virtuálního počítače pro rodinu vybraného obrázku (třeba **SQL Server 2016 RTM Enterprise v systému Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Vytvoření virtuálního počítače

Nakonec vytvořte virtuálního počítače pomocí prostředí PowerShell:

1. Vytvoření do cloudové služby hostovat nové OM. Všimněte si, že je také možné místo toho použít existující cloudové služby. Vytvoření nové proměnné **$svcname** s krátký název cloudovou službu.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Zadejte název počítače virtuální a velikost. Další informace o velikosti virtuálního počítače najdete v článku [Virtuálního počítače velikosti Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Určení místního účtu správce a heslo.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Spuštěním následujícího skriptu vytvořit virtuální počítač.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Další vysvětlení a možnosti konfigurace naleznete v části **Vytvoření sady příkazů** v [Prostředí PowerShell Azure použít k vytvoření a nastavení serveru s Windows virtuálních počítačích](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Příklad skript Powershellu

Poskytuje následující skript a příklady celý skript, který vytvoří **SQL serveru 2016 RTM Enterprise v systému Windows Server 2012 R2** virtuální počítač. Pokud použijete tento skript, je nutné upravit počáteční proměnné podle předchozích kroků v tomto tématu.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Připojit se ke vzdálené ploše

1. Vytvoření. RDP souborů ve složce dokumentu aktuálního uživatele ke spuštění tyto virtuálních počítačích, abyste dokončili nastavení:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. V adresáři dokumenty spusťte soubor RDP. Spojení s správce uživatelské jméno a heslo, které dříve (například pokud vaše uživatelské jméno je VMAdmin, zadejte "\VMAdmin" jako uživatel a zadat heslo).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Dokončete konfiguraci počítače SQL serveru pro vzdálený přístup

Po přihlášení k počítači pomocí vzdálené plochy konfigurace systému SQL Server podle [kroků pro konfiguraci konektivitu serveru SQL Server Azure OM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)v.

## <a name="next-steps"></a>Další kroky

Můžete najít další pokyny pro vytváření virtuálních počítačích s prostředím PowerShell v [si přečtěte následující dokumentaci virtuálních počítačích](virtual-machines-windows-classic-create-powershell.md). Další skripty vztahující se k serveru SQL Server a Premium úložiště najdete v článku [Použití Azure Premium úložiště se serverem SQL Server na virtuálních počítačích](virtual-machines-windows-classic-sql-server-premium-storage.md).

V mnoha případech dalším krokem je migrace databáze do tohoto nového OM SQL serveru. Databáze migrace najdete v článku [Migrace databáze SQL Server na OM Azure](virtual-machines-windows-migrate-sql.md).

Pokud máte taky zájem vytváření virtuálních počítačích SQL pomocí portálu Azure, najdete v článku [zřizování SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md). Všimněte si, že kurzu, který vás provede portálu vytvoří VMs pomocí doporučené modelu správce prostředků, nikoli klasické model použitý v tomto tématu Powershellu.

Kromě tyto materiály doporučujeme, abyste si prošli [jiných témat týkajících se serverem SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
