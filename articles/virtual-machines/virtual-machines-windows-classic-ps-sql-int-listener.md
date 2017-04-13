<properties
    pageTitle="Nakonfigurovat posluchače ILB pro vždy o skupinách dostupnost | Microsoft Azure"
    description="Tento kurz využívá zdroje vytvořená pomocí klasické nasazení modelu a vytvoří vždy na dostupnost skupiny posluchače v Azure pomocí interní zatížení vyrovnávání (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurace posluchače ILB skupin vždy na dostupnost v Azure

> [AZURE.SELECTOR]
- [Vnitřní posluchače](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externí posluchače](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Základní informace

Toto téma ukazuje, jak nakonfigurovat posluchače vždy na dostupnost skupiny pomocí **Interní Vyrovnávání zatížení (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Nakonfigurovat posluchače ILB dostupnost skupiny vždy na v modelu správce prostředků, přečtěte si téma [Konfigurace interní zatížení vyrovnávání skupiny dostupnost vždy na v Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Dostupnost skupiny mohou obsahovat repliky místní pouze pouze Azure nebo rozsahu místním prostředím a Azure u hybridních konfigurací. Azure repliky mohou být umístěny ve stejné oblasti nebo ve více oblastech pomocí několika virtuálních sítí (VNets). Následující kroky předpokládají jste už [nakonfigurovali skupiny dostupnosti](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) ale nenakonfigurovali posluchače.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Pokyny a omezení pro interní posluchače
Poznámka: následující pokyny pro posluchače skupiny dostupnost v Azure pomocí ILB:

- Skupina posluchače dostupnost je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.

- Jedinou skupinu posluchače interní dostupnost je podporována za cloudové služby, protože posluchače nakonfigurovaný tak, aby ILB a existuje pouze jeden ILB za cloudové služby. Je však možné vytvořit více externí posluchače. Další informace najdete v tématu [Konfigurace externího posluchače pro vždy na dostupnost skupin v Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Není podporována pro vytvoření interního posluchače ve stejném cloudové služby, kde máte i externí posluchače pomocí veřejných VIP cloudové služby.

## <a name="determine-the-accessibility-of-the-listener"></a>Určení přístupnosti posluchače

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tento článek se zaměřuje na vytváření posluchače, který používá **Interní Vyrovnávání zatížení (ILB)**. Pokud potřebujete posluchače veřejné/externí, najdete v článku verzi tohoto článku, který obsahuje postup pro nastavení [externího posluchače](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Vytvoření Vyrovnávání zatížení koncové body OM přímé serverem vratky

Pro ILB je nutné nejprve vytvořit Vyrovnávání zatížení vnitřní. Důvodem je skriptu dole.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Pro **ILB**byste měli přiřadit statické IP adresy. Nejdřív zkontrolujte aktuální konfigurace VNet spuštěním následujícího příkazu:

        (Get-AzureVNetConfig).XMLConfiguration

1. Zapište si název **podsítě** podsítě obsahující VMs hostujících replik. Tím se použije v parametru **$SubnetName** ve skriptu.

1. Poznamenejte si název **VirtualNetworkSite** a počáteční **AddressPrefix** podsítě obsahující VMs hostujících replik. Vyhledejte dostupnou IP adresu předávání obou hodnot do příkazu **Test AzureStaticVNetIP** a Prozkoumáním **AvailableAddresses**. Pokud byla VNet s názvem *MyVNet* a měli adresu podsítě oblast, kterou začátek *172.16.0.128*, tento příkaz by adres k dispozici:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Vyberte si jednu z dostupných adres a použít v parametru **$ILBStaticIP** tento skript.

3. Zkopírujte skript Powershellu pod do textového editoru a nastavte hodnotách proměnných podle vašich potřeb (Všimněte si, že jste obdrželi výchozích hodnot pro některé parametry). Všimněte si, že existující nasazení, které používají spřažení skupiny nemůžete přidat ILB. Další informace o požadavcích ILB najdete v článku [Interní Vyrovnávání zatížení](../load-balancer/load-balancer-internal-overview.md). Také pokud skupiny dostupnosti zahrnuje Azure oblastí, je třeba spustit skript jednou v každé datacentra pro Cloudová služba a uzly, které se nacházejí v této datacentra.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Jakmile nastavíte proměnné zkopírujte skript v textovém editoru do relace prostředí Azure PowerShell ho spusťte. Pokud pořád zobrazuje výzvu >>, zadejte ENTER Přesvědčte se, zda je skript spuštěn. Poznámka:

>[AZURE.NOTE] Azure klasické portál interní Vyrovnávání zatížení v současné době nepodporuje, takže neuvidíte ILB nebo koncové body v portálu Azure klasické. **Get-AzureEndpoint** však vrátí vnitřní IP adresu, pokud Vyrovnávání zatížení běží v něm. V ostatních případech vrátí hodnotu null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ověřte, že je KB2854082 nainstalovaný v případě potřeby

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otevření portů brány firewall v uzly skupin dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Vytvořit skupiny posluchače dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Pro ILB je nutné použít IP adresu z vnitřní zatížení vyrovnávání (ILB) vytvořili. Použijte tento skript tuto IP adresu v prostředí PowerShell získat.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. K některé z VMs zkopírujte skript Powershellu pro váš operační systém do textového editoru a nastavte proměnné hodnoty, které bylo uvedeno dříve.

    Pro Windows Server 2012 nebo vyšší použijte tento skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Windows Server 2008 R2 použijte tento skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Jednou jste nastavili proměnných, otevřete okno zvýšenými oprávněními prostředí Windows PowerShell, a pak zkopírujte skript v textovém editoru a vložte do relaci Powershellu Azure ho spusťte. Pokud pořád zobrazuje výzvu >>, zadejte ENTER Přesvědčte se, zda je skript spuštěn.

2. Tento postup opakujte na každý OM. Tento skript konfiguruje prostředek IP adresy IP adresu cloudovou službu a nastaví ostatní parametry funkce jako port zkušební. Když zdroje IP adresu do online režimu, ho můžete pak odpovědět dotazování na portu zkušební z vyrovnávání zatížení koncového bodu vytvořili dříve v tomto kurzu.

## <a name="bring-the-listener-online"></a>Přenést posluchače online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Sledování položek

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testování posluchače skupiny dostupnosti (v rámci stejné VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
