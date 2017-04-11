<properties
   pageTitle="Vytvoření Vyrovnávání zatížení internetového ve Správci zdrojů pomocí prostředí PowerShell | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení internetového ve Správci zdrojů pomocí prostředí PowerShell"
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

# <a name="get-started"></a>Vytváření Vyrovnávání zatížení internetového ve Správci zdrojů pomocí prostředí PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete také [Naučte se vytvářet Vyrovnávání zatížení internetového pomocí klasické nasazení modelu](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Nasazení řešení pomocí prostředí PowerShell Azure

Následující postupy popisují, jak vytvořit Vyrovnávání zatížení internetového pomocí Správce prostředků Azure pomocí prostředí PowerShell. S Azure správce prostředků, jednotlivé zdroje je vytvořili a nakonfigurovat jednotlivě a vložíte můžete vytvořit Vyrovnávání zatížení.

Musíte vytvořit a nakonfigurovat následující objekty, které chcete nasadit Vyrovnávání zatížení:

- Konfigurace front-end IP: obsahuje veřejné adresy IP (PIP) pro příchozí v síti.
- Fond back-end adres: obsahuje virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení sítě rozhraní (NIC).
- Vyrovnávání zatížení pravidla: obsahuje pravidla, která mapování veřejné portu na Vyrovnávání zatížení s portem ve fondu back-end adres.
- Příchozí pravidla překladu síťových adres: obsahuje pravidla, která mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres.
- Sond: obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálního počítače z fondu back-end adresu.

Další informace najdete v článku [Správce prostředků Azure podpory pro vyrovnávání zatížení](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Nastavení prostředí PowerShell lze pomocí Správce zdrojů

Zkontrolujte, jestli že máte nejnovější výrobní verzi modulu Azure správce prostředků pro PowerShell:

1. Přihlaste se k Azure.

        Login-AzureRmAccount

    Zadejte svoje přihlašovací údaje po zobrazení výzvy.

2. Zaškrtněte políčko předplatná pro účet.

        Get-AzureRmSubscription

3. Zvolte, které předplatné Azure používat.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Vytvoření skupiny prostředků. (Tento krok přeskočit, pokud používáte existující skupiny zdrojů.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Vytváření virtuálních sítí a veřejnou IP adresu pro fondu front-end IP

1. Vytvoření podsítě a virtuální sítě.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Vytvoření Azure veřejné prostředek adresy IP, s názvem **PublicIP**pro použití v fondu front-end IP s názvem DNS **loadbalancernrp.westus.cloudapp.azure.com**. Následující příkaz používá typ statické rozdělení.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Vyrovnávání zatížení používá název domény veřejnou IP jako předpony pro jeho plně kvalifikovaný název domény. Tím se liší od klasické nasazení modelu, který používá cloudové služby Vyrovnávání zatížení plně kvalifikovaný název domény.
    >V tomto příkladu je FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Vytvoření fondu front-end IP a fond adresu back-end

1. Vytvoření fondu front-end IP s názvem **LB Frontend** používající **PublicIp** zdroje.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Vytvoření fondu back-end adres s názvem **LB back-end**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Vytvoření pravidel překladu síťových adres, pravidla Vyrovnávání zatížení, zkušební a vyrovnávání zatížení

Tento příklad vytvoří následující položky:

- Pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 3441 port 3389
- Pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 3442 port 3389
- Zkušební pravidlo pro kontrolu stavu na stránku s názvem **HealthProbe.aspx**
- Pravidlo Vyrovnávání zatížení vyrovnání všechny příchozí přenosy na portu 80 portu 80 adresy ve fondu back-end
- Vyrovnávání zatížení, který používá tyto objekty

Pomocí těchto kroků:

1. Vytváření pravidel překladu síťových adres.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Vytvořte zkušební stavu. Konfigurace zkušební dvěma způsoby:

    Zkušební HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    Zkušební TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Vytvoření pravidla Vyrovnávání zatížení.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Vytvoření Vyrovnávání zatížení pomocí dříve vytvořené objekty.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Vytvoření nic

Vytvoření síťová rozhraní (nebo upravit existující) a přiřadit je k překladu síťových adres pravidla pravidla Vyrovnávání zatížení a sond:

1. Pokud potřebujete virtuální sítě a virtuální podsítě, kde je potřeba vytvořit nic.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Vytvoření NIC s názvem **lb nic1 být**a přidružení k první pravidlo překladu síťových adres a fondu první (a pouze) back-end adresy.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Vytvoření NIC s názvem **lb nic2 být**a přidružit druhé pravidlo překladu síťových adres a fondu první (a pouze) back-end adresy.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Podívejte se nic.

        $backendnic1

    Očekávaný výstup:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Použití `Add-AzureRmVMNetworkInterface` rutiny, aby nic přiřadit různé VMs.

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytváření virtuálních počítačů a přiřazování NIC, najdete v článku [Vytvoření OM Azure pomocí Powershellu](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Přidání rozhraní sítě vyrovnávání zatížení

1. Načtení Vyrovnávání zatížení z Azure.

    Načtení zdroje Vyrovnávání zatížení do proměnné (Pokud jste, který ještě neudělali) Proměnná se nazývá **$lb**. Použijte stejný jména ze zdroje Vyrovnávání zatížení, který jste vytvořili.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Konfigurace zatížení back-end proměnné.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Načtení rozhraní již byly vytvořeny sítě do proměnné. Název proměnné je **$nic**. Název rozhraní sítě je stejný z předchozího příkladu.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Změna konfigurace back-end v rozhraní sítě.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Uložení objekt rozhraní sítě.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Po rozhraní sítě se přidá do fondu back-end Vyrovnávání zatížení, začne přijímání na základě pravidel Vyrovnávání zatížení pro daný zdroj Vyrovnávání zatížení v síti.

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávající Vyrovnávání zatížení

1. Pomocí vyrovnávání zatížení z předchozího příkladu objekt Vyrovnávání zatížení proměnné **$slb** pomocí přiřaďte `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. V následujícím příkladu přidejte překladu síťových adres pravidlo pro příchozí připojení – pomocí fondu back-end – existující Vyrovnávání zatížení port 81 ve fondu front-end a port 8181.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Nová konfigurace ušetří `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Odebrání Vyrovnávání zatížení

Použití příkazu `Remove-AzureLoadBalancer` odstranit při vyrovnávání zatížení dříve vytvořené s názvem **NRP LB** ve skupině zdroje s názvem **NRP RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Můžete použít volitelný přepínač **– vyšší** Chcete-li předejít vyzývat k odstranění.

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
