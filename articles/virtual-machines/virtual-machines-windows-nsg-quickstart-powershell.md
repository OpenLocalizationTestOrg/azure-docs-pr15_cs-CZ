<properties
   pageTitle="Otevřete porty angličtině pomocí prostředí PowerShell | Microsoft Azure"
   description="Zjistěte, jak otevřít port / vytvoření koncového bodu vaší Windows v angličtině použití režimu nasazení Správce Azure zdroje a Azure PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Otevření porty a protokoly koncových bodů OM v Azure pomocí prostředí PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Rychlé příkazy
Vytvoření skupiny zabezpečení sítě a ACL pravidla musíte [nejnovější verzi Azure PowerShell nainstalovaný](../powershell-install-configure.md). Můžete také [pomocí těchto kroků na Azure portálu](virtual-machines-windows-nsg-quickstart-portal.md).

Přihlaste se k účtu Azure:

```powershell
Login-AzureRmAccount
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `myNetworkSecurityGroup`, a `myVnet`.

Vytvořte pravidlo. Následující příklad vytvoří pravidla s názvem `myNetworkSecurityGroupRule` umožňující přenosy protokolu TCP na portu 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Potom vytvoření skupiny zabezpečení sítě a přiřadit HTTP pravidlo, které jste právě vytvořili následujícím způsobem. Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Teď Pojďme přiřadit podsítě skupina zabezpečení sítě. Následující příklad přiřadí existující virtuální sítě s názvem `myVnet` proměnné `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Skupina zabezpečení sítě přidružit vaší podsítě. Následující příklad přidruží podsítě s názvem `mySubnet` s skupiny zabezpečení sítě:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Nakonec aktualizace virtuální sítě, aby se změny projeví:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Rychlé příkazy umožňují získat do začátků s provoz vaší v angličtině. Mnoho skvělé funkcí a granularity pro řízení přístupu k prostředkům představují skupiny zabezpečení sítě. Další informace o [vytváření skupiny zabezpečení sítě a ACL pravidla tady](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Jako součást Správce prostředků Azure šablony můžete definovat skupiny zabezpečení sítě a ACL pravidla. Přečtěte si další informace o [vytváření skupin zabezpečení sítě se šablonami](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Pokud budete potřebovat použít předávání portů externí jedinečné přiřadit interní port na vaše OM, použijte při vyrovnávání zatížení a pravidla překládání adres (NAT). Můžete třeba vystavit port TCP 8080 externě a máte přenosy přesměrují do portu TCP 80 na virtuálního počítače. Se dozvíte o [vytváření Vyrovnávání zatížení internetového](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili jednoduché pravidlo umožňuje přenosy protokolu HTTP. Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:

- [Azure Přehled Správce zdrojů](../azure-resource-manager/resource-group-overview.md)
- [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Přehled Správce prostředků pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md)