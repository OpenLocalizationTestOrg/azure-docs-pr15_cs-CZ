<properties
   pageTitle="Vytvoření Vyrovnávání zatížení interní pomocí prostředí PowerShell správce prostředků | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení interní pomocí prostředí PowerShell ve Správci zdrojů"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Vytvoření Vyrovnávání zatížení interní pomocí prostředí PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Následující kroky popisují, jak vytvořit Vyrovnávání zatížení interního správce prostředků Azure pomocí prostředí PowerShell. Pomocí Správce prostředků Azure položky, které chcete vytvořit Vyrovnávání zatížení vnitřní jsou nakonfigurovat jednotlivě a potom kombinaci vytvořit Vyrovnávání zatížení.

Je potřeba vytvořit a nakonfigurovat následující objekty, které chcete nasadit Vyrovnávání zatížení:

- Přední konec IP konfigurace – nastaví její konfiguraci a soukromé IP adresa příchozí v síti
- Fond adres back - nastaví její konfiguraci a rozhraní sítě, které dostanou přenosy rozloženy pocházejících z fondu front-end IP
- Vyrovnávání zatížení, pravidla - zdroj a konfiguraci místního port pro vyrovnávání zatížení.
- Sond: nakonfiguruje zkušební zdravotní stav instancí virtuálního počítače.
- Příchozí pravidla překladu síťových adres – nakonfiguruje port pravidla pro přímý přístup k instance virtuálního počítače.

Další informace o zatížení jde dosáhnout pomocí Správce Azure prostředků v [Azure správce zdrojů podpory pro vyrovnávání zatížení](load-balancer-arm.md)vyrovnávání součásti.

Konfigurace služby Vyrovnávání zatížení mezi dvěma virtuálních počítačích popisují následující postupy.


## <a name="setup-powershell-to-use-resource-manager"></a>Nastavení prostředí PowerShell lze pomocí Správce zdrojů

Ujistěte se, máte nejnovější verzi modulu Azure výrobní pro PowerShell a správně máte prostředí PowerShell nastavení pro přístup k Azure předplatné.

### <a name="step-1"></a>Krok 1

        Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Kontrola předplatná pro účet

        Get-AzureRmSubscription

Zobrazí se výzva k ověřit pomocí svých přihlašovacích údajů.<BR>

### <a name="step-3"></a>Krok 3

Zvolte, které předplatné Azure používat. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Vytvoření skupina zdroje pro vyrovnávání zatížení

### <a name="step-4"></a>Krok 4

Vytvoření nové skupiny prostředků (Přeskočit tento krok použití existující skupiny zdrojů)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Se používá jako výchozí umístění pro zdroje v dané skupině zdroje. Zkontrolujte, že všechny příkazy k vytvoření Vyrovnávání zatížení použijete stejné skupiny prostředků.

Ve výše uvedeném příkladu jsme vytvořili zdroje skupiny nazvané "NRP RG" a "Západní nám" umístění.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Vytváření virtuálních sítí a soukromých IP adres IP fondu front-end


### <a name="step-1"></a>Krok 1

Vytvoří podsítě pro virtuální sítě a přiřadí proměnné $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Vytvořte virtuální sítě:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Vytvoří virtuální sítě a přidá podsítě lb podsítě být virtuální sítě NRPVNet a přiřadí proměnné $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Vytvoření fondu IP přední konce a fondu adres back-end

Nastavení IP fondu front-end pro příchozí zatížení vyrovnávání sítě přenosy a back-end fondu adres pro příjem načíst rovnoměrně přenosy.

### <a name="step-1"></a>Krok 1

Vytvoření fondu front-end IP adresy soukromé IP 10.0.2.5 pro které bude příchozí koncový bod sítě přenosy 10.0.2.0/24 podsítě.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Krok 2

Nastavení fondu adres back-end slouží k přijímání příchozích z fondu front-end IP:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Vytvoření pravidla LB, překladu síťových adres pravidla, zkušební a služba Vyrovnávání zatížení

Po vytvoření IP fondu front-end a back-end fondu adres, je potřeba vytvořit pravidla, které bude zdroj Vyrovnávání zatížení patří:

### <a name="step-1"></a>Krok 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Výše uvedený příklad vytváří následující položky:

- Pravidlo překladu síťových adres, které všechny příchozí umožnění datových přenosů do portu 3441 budou moct port 3389.
- druhé pravidlo překladu síťových adres, které všechny příchozí umožnění datových přenosů do portu 3442 budou moct port 3389.
- pravidlo Vyrovnávání zatížení, která načte zůstatek všech příchozích na veřejné portu 80 místní port 80 ve fondu back-end adresu.
- zkušební pravidlo, které bude kontrola stavu pro cestu "HealthProbe.aspx"



### <a name="step-2"></a>Krok 2

Vytvoření Vyrovnávání zatížení sečtením všech objektů (překladu síťových adres pravidla, pravidla Vyrovnávání zatížení, zkušební konfigurace):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Vytvoření síťová rozhraní

Po vytvoření Vyrovnávání zatížení vnitřní, potřebujete definovat rozhraní sítě, která bude přijímat příchozí rozloženy v síti, překladu síťových adres pravidla a zkušební. Rozhraní sítě v tomto případě nakonfigurovaný jednotlivě a můžete přidělovat virtuálního počítače později.


### <a name="step-1"></a>Krok 1


Získání zdroje virtuální sítě a podsítě vytvořit rozhraní sítě:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Tento krok vytvoří rozhraní sítě, který patří do fondu back-end Vyrovnávání zatížení a přiřadit první pravidlo překladu síťových adres RDP pro rozhraní sítě:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Krok 2

Vytvoření druhého rozhraní sítě s názvem LB Nic2 být:

Tento krok vytvoří druhý rozhraní sítě, přiřazení do fondu back-end stejné Vyrovnávání zatížení a přidružení druhé pravidlo překladu síťových adres vytvoří RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

V konečném důsledku řádku se zobrazí následující:

    $backendnic1

Očekávaný výstup:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Krok 3

Příkazem přidat AzureRmVMNetworkInterface NIC přiřadit virtuálního počítače.

Najdete podrobné pokyny k vytvoření virtuálního počítače a přiřaďte NIC následující dokumentaci: [Vytvoření OM Azure pomocí Powershellu](../virtual-machines/virtual-machines-windows-ps-create.md).

Pokud už máte virtuálního počítače vytvořili, můžete přidat rozhraní sítě pomocí následujícího postupu:

#### <a name="step-1"></a>Krok 1

Načtení zdroje Vyrovnávání zatížení do proměnné (Pokud jste, který ještě neudělali) Proměnná použitá se nazývá $lb a použít stejné názvy z prostředku Vyrovnávání zatížení vytvořeného nad.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Krok 2

Načtení konfigurace back-end proměnné.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Krok 3

Načtení rozhraní již byly vytvořeny sítě do proměnné. Proměnná název použitý je $nic. Síťový název rozhraní používá odpovídá z příkladu výše.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Krok 4

Změna konfigurace back-end v rozhraní sítě.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Krok 5

Uložení objekt rozhraní sítě.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Po rozhraní sítě se přidá do fondu back-end Vyrovnávání zatížení, začne příjem podle pravidel pro daný zdroj Vyrovnávání zatížení vyrovnávání zatížení v síti.

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávající Vyrovnávání zatížení


### <a name="step-1"></a>Krok 1

Pomocí vyrovnávání zatížení v předchozím příkladu, přiřaďte proměnné $slb pomocí Get-AzureRmLoadBalancer objekt Vyrovnávání zatížení

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Krok 2

V následujícím příkladu přidáte nové pravidlo příchozí překladu síťových adres pomocí port 81 v přední a port 8181 pro fondu back-end existující Vyrovnávání zatížení

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Krok 3

Uložení nového konfiguraci pomocí nastavení AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Odebrání Vyrovnávání zatížení

Pomocí příkazu odebrat AzureRmLoadBalancer odstranit Vyrovnávání zatížení dříve vytvořené s názvem "NRP kg" ve skupině zdroje s názvem "NRP RG"

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Můžete použít volitelné přepnout – vyšší Chcete-li předejít vyzývat k odstranění.



## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)