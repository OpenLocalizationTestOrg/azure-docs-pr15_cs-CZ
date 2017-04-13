<properties
   pageTitle="Konfigurace vždy na skupiny dostupnosti posluchačů – Microsoft Azure"
   description="Konfigurace dostupnosti skupiny posluchačů na modelu správce prostředků Azure Vyrovnávání zatížení vnitřní pomocí jedné nebo více IP adres."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Správce prostředků nakonfigurovat jeden nebo více vždy na dostupnost skupiny posluchačů – 

Toto téma ukazuje, jak provést dva kroky:

- Vytvoření interního zatížení vyrovnávání pro SQL Server dostupnost skupiny pomocí rutin prostředí PowerShell.

- Přidejte další IP adres pro vyrovnávání zatížení podporuje víc než jedné skupině dostupnost SQL serveru. 

V SQL Server je posluchače skupiny dostupnost virtuální sítě název, který klienti připojují k přístup k databázi v otevřené primární a sekundární. Na Azure virtuálních počítačích Vyrovnávání zatížení slouží k přidržení IP adresu pro posluchače. Vyrovnávání zatížení přesměrovává přenosy na instanci systému SQL Server, která přijímá požadavky na port zkušební. Ve většině případů používá skupiny dostupnosti Vyrovnávání zatížení vnitřní. Vyrovnávání Azure interní zatížení můžete hostovat jednu nebo více IP adres. Každou IP adresu používá konkrétní zkušební port. Tento dokument ukazuje, jak pomocí Powershellu přidat IP adresy do existujícího Vyrovnávání zatížení pro SQL Server dostupnost skupiny nebo vytvořit nové Vyrovnávání zatížení. 

Možnost přiřadit více IP adres pro vyrovnávání zatížení vnitřní novinkách Azure a je dostupná jenom v modelu správce prostředků. K provedení této úlohy musíte mít skupiny dostupnosti serveru SQL Server nasazený na Azure virtuálních počítačích v modelu správce prostředků. Obě virtuálních počítačích SQL serveru musí patřit k stejnou sadu dostupnosti. [Šablona aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) můžete automaticky vytvořit skupiny dostupnosti v Azure správce. Tato šablona automaticky vytvoří skupině dostupnost, včetně Vyrovnávání zatížení interní za vás. Pokud chcete, můžete [ručně nakonfigurovat skupiny dostupnosti AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Toto téma vyžaduje, že skupin dostupnost jsou již nakonfigurovány.  

Příbuzná témata, patří:

- [Konfigurace skupiny dostupnosti AlwaysOn v Azure OM (grafické)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Konfigurace připojení VNet VNet přes správce prostředků Azure a prostředí PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfigurace brány Windows Firewall

Konfigurace brány Windows Firewall přístupu SQL serveru. Je třeba konfigurovat brány firewall povolit připojení TCP porty nepoužívá instanci systému SQL Server, jakož i port používaný zkušební posluchače. Podrobné pokyny najdete v článku [Konfigurace brány Windows Firewall pro přístup k databázovému stroji](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Vytvořte pravidlo pro příchozí připojení pro port SQL serveru a portu zkušební.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Příklad skript: Vytvoření Vyrovnávání zatížení vnitřní pomocí prostředí PowerShell

> [AZURE.NOTE] Pokud jste vytvořili dostupnost skupiny pomocí [šablony Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) načíst nepotřebujete dokončení tohoto kroku. 

Tento skript Powershellu vytvoří Vyrovnávání zatížení vnitřní, nastaví pravidla pro vyrovnávání zatížení a nastaví IP adresu pro vyrovnávání zatížení. Následujícím způsobem otevřete ISE v prostředí Windows PowerShell a vložení skriptu v podokně skriptu. Použití `Login-AzureRMAccount` k přihlášení k Powershellu. Pokud máte víc předplatných Azure, `Select-AzureRmSubscription ` nastavovaná předplatné. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Příklad skriptu: Přidání IP adresy do existujícího Vyrovnávání zatížení pomocí prostředí PowerShell

Pokud chcete používat víc než jedné skupině dostupnost, pomocí prostředí PowerShell přidat další IP adresy do existujícího Vyrovnávání zatížení. Každou IP adresu vyžadují vlastní pravidla, zkušební portů a přední port pro vyrovnávání zatížení.

Front-end je port používaný aplikací pro připojení k instanci systému SQL Server. IP adresy pro různé dostupnost skupiny můžete použít stejný port front-end.

>[AZURE.NOTE] Pro skupin dostupnost serveru SQL Server vyžaduje každou IP adresu portu konkrétní zkušební. Například pokud IP adres na Vyrovnávání zatížení používá zkušební port 59999, bez dalších IP adresy na tomto Vyrovnávání zatížení port může používat zkušební 59999. 

- Informace o vyrovnávání zatížení limity najdete v článku **soukromé přední ukončit IP na Vyrovnávání zatížení** v části [Limity sítě – Správce prostředků Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Informace o dostupnosti skupiny limity najdete v článku [omezení (dostupnost skupiny)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Tento skript přidá novou IP adresu do existující Vyrovnávání zatížení. Aktualizujte proměnné ve vašem prostředí. ILB používá posluchače port pro front-end port Vyrovnávání zatížení. Tento port může být port serveru SQL Server listening na. Pro výchozí instance serveru SQL Server je to port 1433. Pravidla pro skupinu dostupnost pro vyrovnávání zatížení vyžaduje port back-end je stejná jako port front-end plovoucí IP (zpáteční přímé server).

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurace clusteru používat IP adresu Vyrovnávání zatížení 

Dalším krokem je ke konfiguraci posluchače na clusteru a přenést posluchače online. K tomu, postupujte takto: 

1. Vytvořit skupiny posluchače dostupnost na překlopení obrázku  

2. Přenést posluchače online

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Vytvořte skupinu posluchače dostupnost na překlopení obrázku

V tomto kroku přidání klientský bod překlopení obrázku s správce a použití Powershellu ke konfiguraci zdroje obrázku poslouchat port zkušební. 

1. Připojení k Azure virtuální počítač, který je hostitelem primární otevřené pomocí RDP. 

2. Otevřete správce.

3. Vyberte uzel **sítí** a poznamenejte si síťový název obrázku. Pomocí tohoto názvu v `$ClusterNetworkName` proměnných v skript Powershellu.

4. Rozbalte název obrázku a potom klikněte na položku **role**.

5. V podokně **role** klikněte pravým tlačítkem myši na název skupiny dostupnosti a pak vyberte **Přidat zdroje** > **Klientský přístupový bod**.

6. Do pole **název** vytvořit název pro tento nový posluchače, a pak dvakrát klikněte na **Další** a potom klikněte na **Dokončit**. Nezahájí posluchače nebo online zdroje v tomto okamžiku.
 
    Název nového posluchače je název sítě, využívající aplikací pro připojení k databázím ve skupině dostupnost SQL serveru.

7. Klikněte na kartu **zdroje** a potom rozbalte položku klientský přístupový bod jste právě vytvořili. Klikněte pravým tlačítkem myši IP zdroje a klikněte na vlastnosti. Zapište si název IP adresu. Použijete tento název v `$IPResourceName` proměnných v skript Powershellu.

8. V části **IP adresa** klikněte na **Statické IP adresy** a nastavte statickou IP adresu použít stejnou adresu, která jste použili při nastavování IP adresu Vyrovnávání zatížení na portálu Azure. 

9. Zakázání NetBIOS pro tuto adresu a klikněte na **OK**. Opakujte tento krok pro každý zdroj IP Pokud řešení zahrnuje víc VNets Azure. 

10. Nastavte zdroj SQL Server dostupnost skupiny závisí na IP adresu. Pravým tlačítkem myši na zdroj ve Správci clusteru, jde na kartě **zdroje** v části **Jiné zdroje**. 

11. Na uzel obrázku, který je aktuálně hostitelem primární otevřené otevřete zvýšenými ISE PowerShell a vložte následující příkazy do nový skript. Na kartě **závislosti** klikněte na název posluchače.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Aktualizace proměnných a spustit skript Powershellu pro nastavení IP adresa a port pro nové posluchače.

 >[AZURE.NOTE] Pokud jsou servery SQL v samostatných oblastech, budete muset spustit skript Powershellu dvakrát. První použití clusteru síťový název, název zdroje IP clusteru a načtěte vyrovnávání IP adresu z první skupina zdroje. Ještě jednou pomocí obrázku síťový název, název zdroje IP clusteru a načíst vyrovnávání IP adrese z druhé skupina zdroje.

Obrázku se má dostupnost skupiny posluchače zdroj.

## <a name="2-bring-the-listener-online"></a>2. přenést posluchače online

S tímto dostupnost skupiny posluchače zdrojem nakonfigurovány můžete si přenést posluchače online tak, aby aplikace můžete připojit k databázím ve skupině dostupnost s posluchače.

1. Přejděte zpět do správce. Rozbalte položku **role** a zobrazíte skupiny dostupnosti. Na kartě **zdroje** klikněte pravým tlačítkem myši na název posluchače a klikněte na **Vlastnosti**.

1. Klikněte na kartu **závislosti** . Pokud existuje více zdrojů uvedené, ověřte, zda IP adresy nebo Ne a závislosti. Klikněte na **OK**.

1. Klikněte pravým tlačítkem myši na název posluchače a klikněte na **Přepnout do režimu Online**.

1. Když posluchače je online, na kartě **zdroje** , klikněte pravým tlačítkem skupinu dostupnost a klikněte na **Vlastnosti**.

1. Vytvoření závislosti na posluchače název zdroje (nikoli na IP adresu zdrojů název). Klikněte na **OK**.

1. Spuštění SQL Server Management Studio a připojte k primární otevřené.

1. Přejděte na **Maximum AlwaysOn dostupnost** | **skupiny dostupnosti** | **posluchače skupiny dostupnosti**. 

1. Teď byste měli vidět posluchače název, který jste vytvořili v správce. Klikněte pravým tlačítkem myši na název posluchače a klikněte na **Vlastnosti**.

1. V poli **Port** zadejte číslo portu pro posluchače dostupnost skupiny pomocí $EndpointPort jste dříve použili (1433 bylo výchozí nastavení), klikněte na tlačítko **OK**.

Nyní máte skupiny dostupnosti serveru SQL Server Azure virtuálních počítačích spuštění v režimu správce prostředků. 

## <a name="test-the-connection-to-the-listener"></a>Zkontrolujte připojení k webu posluchače

Chcete-li otestovat připojení:

1. RDP k serveru SQL, který je ve stejném virtuální síť, ale nejste vlastníkem otevřené. Může to být SQL Server ve clusteru.

2. Pomocí nástroje **sqlcmd** test připojení. Například následující skript vytvoří **sqlcmd** připojení k primární otevřené prostřednictvím posluchače pomocí ověřování Windows:

    ```
    sqlmd -S <listenerName> -E
    ```

    Pokud posluchače používá port než výchozí port (1433), zadejte port v připojovacím řetězci. Například následující příkaz sqlcmd ke kterému je připojen posluchače přístavu 1435: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Připojení SQLCMD automaticky připojoval k ten instanci služby SQL Server hostuje primární otevřené. 

>[AZURE.NOTE] Zkontrolujte portu, které zadáte otevřít v bráně firewall serveru i SQL serveru. Obou serverů vyžadují pravidlo pro příchozí připojení pro port TCP, který používáte. Další informace najdete v článku [Přidat nebo upravit pravidlo bránu Firewall](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Pokyny a omezení

Poznámka: následující pokyny pro posluchače skupiny dostupnost v Azure pomocí interních služba Vyrovnávání zatížení:

- S Vyrovnávání zatížení vnitřní přístup jenom posluchače z ve stejné síti virtuální.

## <a name="for-more-information"></a>Další informace

Další informace najdete v článku [Konfigurace vždy na dostupnost seskupení v Azure OM ručně](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell

Pomocí následující rutiny prostředí PowerShell můžete vytvářet Vyrovnávání zatížení vnitřní pro Azure virtuálních počítačích.

- `New-AzureRmLoadBalancer`Vytvoří Vyrovnávání zatížení. Další informace najdete v tématu [AzureRmLoadBalancer nový](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`Vytvoří front-end IP konfigurace služby Vyrovnávání zatížení. Další informace najdete v tématu [AzureRMLoadBalancerFrontendIpConfig nový](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`vytvoří konfigurace pravidla pro vyrovnávání zatížení. Další informace najdete v tématu [AzureRmLoadBalancerRuleConfig nový](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`vytvoří konfigurace fondu adres back-end pro vyrovnávání zatížení. Další informace najdete v tématu [AzureRmLoadBalancerBackendAddressPoolConfig nový](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`Vytvoří zkušební konfigurace služby Vyrovnávání zatížení. Další informace najdete v tématu [AzureRmLoadBalancerProbeConfig nový](http://msdn.microsoft.com/library/mt603847.aspx) .

Pokud potřebujete odebrat Vyrovnávání zatížení ze skupiny Azure zdrojů, použijte `Remove-AzureRmLoadBalancer`. Další informace najdete v tématu [AzureRmLoadBalancer odebrat](http://msdn.microsoft.com/library/mt603862.aspx).
