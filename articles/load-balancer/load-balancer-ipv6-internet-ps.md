<properties
    pageTitle="Vytvoření internetové Vyrovnávání zatížení, IPv6 pomocí Powershellu pro správce prostředků | Microsoft Azure"
    description="Naučte se vytvářet internetové Vyrovnávání zatížení, IPv6 pomocí prostředí PowerShell pro správce zdrojů"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="protokol IPv6 Vyrovnávání zatížení azure, duální zásobníku, veřejnou ip, nativní protokol ipv6, mobilní telefon, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Začít vytvářet internetové Vyrovnávání zatížení, IPv6 pomocí prostředí PowerShell pro správce zdrojů

> [AZURE.SELECTOR]
- [Prostředí PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure rozhraní příkazového řádku](./load-balancer-ipv6-internet-cli.md)
- [Šablony](./load-balancer-ipv6-internet-template.md)

Vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy-4 při instalaci (TCP, UDP). Vyrovnávání zatížení zajišťuje dostupnost distribuce příchozí komunikaci mezi správný služby instancí služby cloudu nebo virtuálních počítačích v sadě Vyrovnávání zatížení. Azure Vyrovnávání zatížení můžete zobrazit tyto služby ve více portů a více IP adres.

## <a name="example-deployment-scenario"></a>Příklad scénáře

Následující obrázek znázorňuje řešení pro vyrovnávání zatížení nasazena v tomto článku.

![Scénář Vyrovnávání zatížení](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

V tomto scénáři vytvoří následující Azure zdroje:

- Vyrovnávání zatížení internetového IPv4 a IPv6 veřejnou IP adresu
- dvě načíst vyrovnávání pravidla přiřadit veřejné virtuální privátní koncové body
- Dostupnost hodnotu, která obsahuje dva VMs
- dva virtuálních počítačích (VMs)
- virtuální sítě rozhraní pro každou OM s IPv4 a IPv6 adresa přiražená

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Nasazení řešení pomocí prostředí PowerShell Azure

Podle těchto kroků ukazují, jak vytvořit internetové Vyrovnávání zatížení správce prostředků Azure pomocí prostředí PowerShell. Pomocí Správce prostředků Azure jednotlivé zdroje se vytvoří a nakonfigurovali jednotlivě, pak vložit můžete vytvořit zdroj.

Abyste mohli nasadit služby Vyrovnávání zatížení, vytváření a konfigurace následující objekty:

- Konfigurace front-end IP - obsahuje veřejnou IP adres pro příchozí v síti.
- Fond back-end adres – stránka obsahuje síťová rozhraní (NIC) virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení.
- Vyrovnávání zatížení pravidla - obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení s portem ve fondu back-end adres.
- Příchozí pravidla překladu síťových adres – obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres.
- Sond - obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálních počítačích ve fondu back-end adres.

Další informace najdete v článku [Správce prostředků Azure podpory pro vyrovnávání zatížení](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Nastavení prostředí PowerShell lze pomocí Správce zdrojů

Zkontrolujte, jestli že máte nejnovější výrobní verzi modulu Azure správce prostředků pro PowerShell.

1. Přihlaste se do Azure

        Login-AzureRmAccount

    Zadejte svoje přihlašovací údaje po zobrazení výzvy.

2. Kontrola předplatná pro účet

        Get-AzureRmSubscription

3. Zvolte, které předplatné Azure používat.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Vytvoření skupina zdroje (Přeskočit tento krok použití existující skupiny zdrojů)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Vytváření virtuálních sítí a veřejnou IP adresu pro fondu front-end IP

1. Vytvořte virtuální sítě s podsítě.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Vytvoření Azure veřejnou IP adresu zdroje (PIP) pro fondu front-end IP adres.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Vyrovnávání zatížení používá název domény veřejnou IP jako předpony pro jeho plně kvalifikovaný název domény. V tomto příkladu certifikátu se *lbnrpipv4.westus.cloudapp.azure.com* *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Vytvoření konfigurace předřazený IP a fondu adres Back-End

1. Vytvoření konfigurace front-end adresu, která používá veřejnou IP adresy, kterou jste vytvořili.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Vytvoření fondy back-end adres.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Vytvoření pravidel LB, pravidla překladu síťových adres, zkušební a vyrovnávání zatížení

Tento příklad vytvoří následující položky:

- pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 443 port 4443
- pravidlo Vyrovnávání zatížení zůstatek všechny příchozí přenosy na portu 80 portu 80 adresy ve fondu back-end.
- pravidlo Vyrovnávání zatížení umožňující RDP připojení k VMs na port 3389.
- zkušební pravidlo pro kontrolu stavu na stránku s názvem *HealthProbe.aspx* nebo službu na portu 8080
- Vyrovnávání zatížení, který používá tyto objekty

1. Vytváření pravidel překladu síťových adres.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Vytvořte zkušební stavu. Konfigurace zkušební dvěma způsoby:

    Zkušební HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    nebo zkušební TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    V tomto příkladu budeme používat sond TCP.

3. Vytvoření pravidla Vyrovnávání zatížení.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Vytvoření Vyrovnávání zatížení pomocí dříve vytvořené objekty.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Vytvoření nic pro VMs back-end

1. Pokud potřebujete virtuální sítě a virtuální podsítě, kde je potřeba vytvořit nic.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Vytvoření konfigurace IP a nic pro VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Vytváření virtuálních počítačích a přiřadit nově vytvořený nic

Další informace o vytváření virtuálního počítače najdete v tématu [vytvořit a nakonfigurovat Windows virtuálního počítače s správce zdrojů a Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Vytvořte účet nastavit dostupnost a úložiště

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Vytvoření každého OM a přiřazení předchozího vytvořili nic

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
