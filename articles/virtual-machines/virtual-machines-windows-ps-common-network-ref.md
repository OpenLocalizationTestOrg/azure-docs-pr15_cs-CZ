<properties
    pageTitle="Společné síti prostředí PowerShell příkazy pro VMs | Microsoft Azure"
    description="Běžné příkazy Powershellu, abyste mohli začít vytváření virtuálních sítí a jeho přidružené prostředky pro VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Běžné příkazy Powershellu Azure sítě pro VMs

Pokud chcete vytvořit virtuální počítač, budete muset vytvořit [virtuální sítě](../virtual-network/virtual-networks-overview.md) nebo vědět o existující virtuální sítě, ve kterém můžete přidat OM. Obvykle když vytvoříte virtuálního počítače, bude potřeba zvažte vytvoření prostředků popsaných v tomto článku.

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, výběr předplatného a přihlášení k vašemu účtu.

## <a name="create-network-resources"></a>Vytvořit prostředky sítě

Úkol | Příkaz 
-------------- | -------------------------
Vytvořit podsítě konfigurace | $Podsíť1 = [New AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -název "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$Podsíť2 = New AzureRmVirtualNetworkSubnetConfig-název "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Typické síť může mít podsítí pro [internet protilehlé Vyrovnávání zatížení](../load-balancer/load-balancer-internet-overview.md) a samostatné podsítě pro [Vyrovnávání zatížení vnitřní](../load-balancer/load-balancer-internal-overview.md). |
Vytvořit virtuální sítě | $vnet = [New AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -resource_group_name"název"virtual_network_name"- ResourceGroupName"-umístění "location_name" - AddressPrefix XX. X.X.X/XX-podsítě $Podsíť1, $Podsíť2
Test pro název jedinečný domény | [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "název_domény"-umístění "location_name"<BR><BR>Zadání názvu DNS domény pro [veřejnou IP zdroje](../virtual-network/virtual-network-ip-addresses-overview-arm.md), která vytvoří mapování domainname.location.cloudapp.azure.com na veřejnou IP adresu v servery DNS Azure Správa přístupových práv. Název může obsahovat jenom písmena, číslice a spojovníky. První a poslední znak musí být písmeno nebo číslo a název domény musí být jedinečný v rámci jeho Azure umístění. Vrátí **hodnotu True** , navržený název při globálně jedinečný.
Vytvořit veřejnou IP adresu | $pip = [New AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -název_domény"název"ip_address_name"- ResourceGroupName"resource_group_name"- DomainNameLabel"-umístění "location_name" - AllocationMethod dynamické<BR><BR>Na veřejnou IP adresu používá název domény, které jste dříve testováno používá frontend konfigurace služby Vyrovnávání zatížení.
Vytvoření konfigurace IP frontend | $frontendIP = [New AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -název "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Konfigurace frontend zahrnuje veřejnou IP adresu, který jste vytvořili pro příchozí v síti.
Vytvoření fondu adres back-end | $beAddressPool = [New AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -název "backend_pool_name"<BR><BR>Poskytuje vnitřní adresy back-end Vyrovnávání zatížení, které jsou dostupné přes síťové rozhraní.
Vytvoření zkušební | $healthProbe = [Nový AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) – název "probe_name" - RequestPath "HealthProbe.aspx"-protokolu http-Port 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálních počítačích ve fondu back-end adresu.
Vytvoření pravidla Vyrovnávání zatížení | $lbRule = [New AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -HTTP název - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-Probe $healthProbe-protokolu Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Obsahuje pravidla, která přiřadit veřejné port Vyrovnávání zatížení s portem ve fondu back-end adresu.
Vytvořit pravidlo pro příchozí připojení překladu síťových adres | $inboundNATRule = [New AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -název "rule_name" - FrontendIpConfiguration $frontendIP-protokolu TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adresu.
Vytvoření Vyrovnávání zatížení | $loadBalancer = [New AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName "resource_group_name"-název "load_balancer_name"-umístění "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Probe $healthProbe
Vytvoření rozhraní sítě | $nic1 = [New AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName "resource_group_name"-název "network_interface_name"-umístění "location_name" - PrivateIpAddress XX. X.X.X-podsítě Podsíť2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Vytvoření rozhraní sítě pomocí veřejnou IP adresu a podsítě virtuální sítě, který jste vytvořili.
    
## <a name="get-information-about-network-resources"></a>Získat informace o prostředcích sítě

Úkol | Příkaz 
-------------- | -------------------------
Seznam virtuálních sítí | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Obsahuje seznam všech virtuální sítí ve skupině prostředků.
Získat informace o virtuální sítě | Get-AzureRmVirtualNetwork-resource_group_name"název"virtual_network_name"- ResourceGroupName"
Seznam podsítí v síti virtuální | Get-AzureRmVirtualNetwork-název - ResourceGroupName "virtual_network_name" "resource_group_name" & #124; Vyberte podsítí
Získání informací o podsítě | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) – název "subnet_name" - VirtualNetwork $vnet<BR><BR>Získání informací o podsítě v zadaném virtuální sítě. Hodnota $vnet představuje objekt vrácené Get-AzureRmVirtualNetwork.
Seznam IP adres | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Seznam veřejných adres IP ve skupině prostředků.
Seznam Vyrovnávání zatížení | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Obsahuje seznam všech Vyrovnávání zatížení ve skupině prostředků.
Seznam síťových rozhraní | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Seznam rozhraní sítě ve skupině prostředků.
Získat informace o rozhraní sítě | Get-AzureRmNetworkInterface-resource_group_name"název"network_interface_name"- ResourceGroupName"<BR><BR>Získání informací o rozhraní určité sítě.
Získání konfigurace IP rozhraní sítě | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) – název "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Získání informací o konfiguraci IP rozhraní určité sítě. Hodnota $nic představuje objekt vrácené Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Přidávání a používání zdrojů sítě

Úkol | Příkaz 
-------------- | -------------------------
Přidání podsítě virtuální sítě | [Přidání AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX – název "subnet_name" - VirtualNetwork $vnet<BR><BR>Přidá podsítě existující virtuální sítě. Hodnota $vnet představuje objekt vrácené Get-AzureRmVirtualNetwork.
Odstranění virtuální sítě | [Odebrat AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -resource_group_name"název"virtual_network_name"- ResourceGroupName"<BR><BR>Odebere zadaný virtuální sítě ze skupiny zdrojů.
Odstranění rozhraní sítě | [Odebrat AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -resource_group_name"název"network_interface_name"- ResourceGroupName"<BR><BR>Odebere rozhraní určité sítě ze skupiny zdrojů.
Odstranění Vyrovnávání zatížení | [Odebrat AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -resource_group_name"název"load_balancer_name"- ResourceGroupName"<BR><BR>Odebere Vyrovnávání zatížení zadaný ze skupiny zdrojů.
Odstranění veřejnou IP adresu | [Odebrat AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-resource_group_name"název"ip_address_name"- ResourceGroupName"<BR><BR>Odebere zadaný veřejnou IP adresu ze skupiny zdrojů.

## <a name="next-steps"></a>Další kroky

- Použití rozhraní sítě, že jste vytvořili při [Vytvoření virtuálního počítače](virtual-machines-windows-ps-create.md).
- Zjistěte, jak se dá [vytvořit OM s více síťových rozhraní](../virtual-network/virtual-networks-multiple-nics.md).
