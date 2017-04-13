<properties
   pageTitle="Nasazení VMs NIC více pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se nasadit VMs NIC více pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Nasazení více NIC VMs (klasický) pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Můžete vytvořit virtuálních počítačích (VMs) v Azure a připojit víc sítí rozhraní (NIC) na jednotlivé vaší VMs. Více nic povolit oddělení typy přenosů přes nic. Například jeden NIC může komunikovat s Internet, zatímco jiné informuje uživatele o jenom s interních zdrojů nejste připojení k Internetu. Možnost oddělit sítě přenosy napříč několika nic je potřebný pro mnoho sítě virtuální spotřebiče, například aplikace doručení a sítě WAN optimalizační řešení.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

V současné době nejde máte VMs s jednoho NIC a VMs s více nic ve stejném cloudové služby. Proto budete muset implementovat servery back-end v jiné cloudové službě než a jiné součásti v scénáře. Postupem použít do cloudové služby s názvem *IaaSStory* na hlavní zdroje a *IaaSStory back-end* serverů back-end.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před nasazením servery back-end, budete muset nasazení hlavní cloudovou službu s všechny potřebné prostředky pro tento scénář. Minimálně je potřeba vytvořit virtuální sítě s podsítí pro back-end. Navštivte [vytvořit virtuální sítě pomocí rozhraní příkazového řádku Azure](virtual-networks-create-vnet-classic-cli.md) se dozvíte, jak nasadit virtuální sítě.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Nasazení back-end VMs

VMs back-end, závisí na vytváření odkazů níže.

- **Účet úložiště pro data discích**. Pro lepší výkon disků dat na serverech databáze bude technologii plné stavu jednotka (SSD), který vyžaduje účet úložiště premium. Zkontrolujte Azure umístění nasadíte podporuje premium úložiště.
- **Nic**. Každý OM bude mít dvě nic, jedno pro přístup k databázi a druhý pro správu.
- **Nastavte dostupnost**. Všechny databáze servery se přidají do jednoho dostupnost nastavit, a zkontrolujte, že alespoň jedno z VMs nahoru a systémem údržbě.

### <a name="step-1---start-your-script"></a>Krok 1: zahájení skriptu

Můžete si stáhnout celou flám skript použitý [v tomto poli](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Postupujte podle pokynů a změnit skriptu pro práci v prostředí.

1. Změňte hodnoty pod proměnných založených na existující skupiny zdrojů nasazený nad [předpokladů](#Prerequisites).

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Změňte hodnoty proměnných dole na základě hodnot, které chcete použít pro nasazení back-end.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 – Vytvoření potřebné prostředky pro vaše VMs

1. Vytvoření nového Cloudová služba pro všechny VMs back-end. Všimněte si použití `$backendCSName` proměnné pro skupinu název zdroje a `$location` oblasti Azure.

        azure service create --serviceName $backendCSName \
            --location $location

2. Vytvoření účtu úložiště premium disků OS a data pro použití v váš VMs.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Krok 3 – vytvoření VMs s více nic

1. Zahájení smyčka k vytvoření více VMs na základě `numberOfVMs` proměnné.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Pro každý OM zadejte název a IP adresu každého ze dvou nic.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Vytvořte OM. Všimněte si využití `--nic-config` parametr, obsahuje seznam všech nic se jménem, podsítě a IP adresu.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Pro každý OM vytvořte dvou dat discích.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Krok 4 – spuštění skriptu

Teď, když jste si stáhli a změnit skript podle vašich potřeb, spusťte skript, který chcete vytvořit back-end databáze VMs s více nic.

1. Uložte skript a spusťte z terminálu **flám** . Zobrazí se výstupu počáteční a jak je ukázáno v následujícím příkladu.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Po několika minutách bude spuštění ukončena a zobrazí se zbytek výstup jak je ukázáno v následujícím příkladu.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
