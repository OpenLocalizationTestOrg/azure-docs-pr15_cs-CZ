<properties
    pageTitle="Konfigurace externí posluchače skupin vždy na dostupnost | Microsoft Azure"
    description="Tento kurz vás provede kroky pro vytvoření vždy na dostupnost skupiny posluchače v Azure přístupný externě pomocí veřejné virtuální IP adresu přidruženou cloudovou službu."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurace externí posluchače skupin vždy na dostupnost v Azure

> [AZURE.SELECTOR]
- [Vnitřní posluchače](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externí posluchače](virtual-machines-windows-classic-ps-sql-ext-listener.md)

V tomto tématu se dozvíte, jak nakonfigurovat posluchače vždy na dostupnost skupiny externě přístupný na Internetu. To je možné přidružením adresy **Veřejné virtuální IP (VIP)** služby cloudu posluchače.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dostupnost skupiny mohou obsahovat repliky místní pouze pouze Azure nebo rozsahu místním prostředím a Azure u hybridních konfigurací. Azure repliky mohou být umístěny ve stejné oblasti nebo ve více oblastech pomocí několika virtuálních sítí (VNets). Následující kroky předpokládají jste už [nakonfigurovali skupiny dostupnosti](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) ale nenakonfigurovali posluchače.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Pokyny a omezení pro externí posluchače

Při nasazení pomocí adresy prsy VIP cloudové služby, mějte na paměti následující pokyny o dostupnosti posluchače skupiny v Azure:

- Skupina posluchače dostupnost je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.

- Klientská aplikace musí nacházet v jiné cloudové službě než ten, který obsahuje vaše skupina dostupnost VMs. Azure nepodporuje návratu přímé serveru s klienta a serveru ve stejném cloudové služby.

- Ve výchozím nastavení v tomto článku kroků jak nakonfigurovat jeden posluchače použijete adresu virtuální IP (VIP) služby cloudu. Je však možné rezervovat a vytvořit víc adres VIP cloudové službě. Umožňuje vytvořit více posluchače, které jsou všechny přidružené k jiné VIP postupovat podle kroků v tomto článku. Informace o tom, jak vytvořit víc adres VIP najdete v tématu [Více virtuální za cloudové služby](../load-balancer/load-balancer-multivip.md).

- Pokud vytváříte posluchače pro hybridního prostředí, může mít v místní síti připojení k veřejné sítě Internet VPN na webu s Azure virtuální sítě. V Azure podsítě je dostupná jenom pro veřejnou IP adresu odpovídajících cloudové služby posluchače dostupnost skupiny.

- Není podporována pro vytvoření externího posluchače ve stejném cloudové služby, kde máte i vnitřní posluchače pomocí interní Vyrovnávání zatížení (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>Určení přístupnosti posluchače

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tento článek se zaměřuje na vytváření posluchače, který využívá **externí Vyrovnávání zatížení**. Pokud budete potřebovat posluchače, který je k síti virtuální privátní, najdete v článku verzi tohoto článku, který obsahuje postup pro nastavení [posluchače s ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Vytvoření Vyrovnávání zatížení koncové body OM přímé serverem vratky

Externí Vyrovnávání zatížení využívá virtuální veřejné virtuální IP adresu cloudové služby, který je hostitelem vaší VMs. Tak nepotřebujete k vytváření a konfigurace služby Vyrovnávání zatížení v tomto případě.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Zkopírujte skript Powershellu pod do textového editoru a nastavte hodnotách proměnných podle vašich potřeb (výchozí hodnoty jsou uvedeny některé parametry). Všimněte si, že pokud skupiny dostupnosti zahrnuje Azure oblastí, je třeba spustit skript jednou v každé datacentra pro Cloudová služba a uzly, které se nacházejí v této datacentra.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Jakmile nastavíte proměnné zkopírujte skript v textovém editoru do relace prostředí Azure PowerShell ho spusťte. Pokud pořád zobrazuje výzvu >>, zadejte ENTER Přesvědčte se, zda je skript spuštěn.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ověřte, že je KB2854082 nainstalovaný v případě potřeby

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otevření portů brány firewall v uzly skupin dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Vytvořit skupiny posluchače dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Pro vyrovnávání zatížení externí, musíte získat veřejné virtuální IP adresu cloudové služby, který obsahuje vaše repliky. Přihlaste se k portálu Azure klasické. Přejděte do cloudové služby, který obsahuje vaše skupina dostupnost OM. Zobrazení **řídicího panelu** .

3. Poznámka: na adresu zobrazenou v části **adresa veřejného virtuální IP (VIP)**. Pokud řešení zahrnuje VNets, opakujte tento krok pro každé cloudové služby, který obsahuje OM, který je hostitelem otevřené.

4. K některé z VMs zkopírujte skript Powershellu pod do textového editoru a nastavte proměnné hodnoty, které bylo uvedeno dříve.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Jednou jste nastavili proměnných, otevřete okno zvýšenými oprávněními prostředí Windows PowerShell, a pak zkopírujte skript v textovém editoru a vložte do relaci Powershellu Azure ho spusťte. Pokud pořád zobrazuje výzvu >>, zadejte ENTER Přesvědčte se, zda je skript spuštěn.

1. Tento postup opakujte na každý OM. Tento skript konfiguruje prostředek IP adresy IP adresu cloudovou službu a nastaví ostatní parametry funkce jako port zkušební. Když zdroje IP adresu do online režimu, ho můžete pak odpovědět dotazování na portu zkušební z vyrovnávání zatížení koncového bodu vytvořili dříve v tomto kurzu.

## <a name="bring-the-listener-online"></a>Přenést posluchače online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Sledování položek

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testování posluchače skupiny dostupnosti (v rámci stejné VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testování posluchače skupiny dostupnosti (přes internet)

Abyste získali přístup posluchače z mimo virtuální sítě, musíte používat vyrovnávání zatížení externí/veřejné (popsaných v tomto tématu) místo ILB, který je dostupný pouze z ve stejném VNet. V připojovacím řetězci zadáte název služby cloudu. Například pokud jste měli do cloudové služby s názvem *mycloudservice*, příkaz sqlcmd vypadat následovně:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Na rozdíl od předchozího příkladu ověřování serveru SQL musíte použít, protože volající nelze použít ověřování windows prostřednictvím Internetu. Další informace najdete v tématu [vždy na dostupnost skupiny v Azure OM: případech připojení klientů](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Pokud chcete použít ověřování serveru SQL, ujistěte se, vytvořit stejné přihlašovací jméno na obou repliky. Další informace o řešení potíží s přihlášení se skupinami dostupnost najdete v článku [mapování přihlášení nebo použití obsažené SQL databáze uživatelům připojení k replikami a namapovat na dostupnost databází](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Pokud repliky vždy na nacházejí v různých podsítí, musíte zadat klientů **MultisubnetFailover = True** v připojovacím řetězci. Výsledkem pokusy o paralelní připojení replikami v různých podsítí. Všimněte si, že tento scénář obsahuje nasazení vždy na skupiny dostupnosti více oblastí.

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
