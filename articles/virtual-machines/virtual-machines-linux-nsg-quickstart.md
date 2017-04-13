<properties
   pageTitle="Otevřete porty a protokoly koncových bodů Linux OM | Microsoft Azure"
   description="Zjistěte, jak otevřít port / vytvoření koncového bodu vaší v angličtině Linux pomocí modelu Azure zdroje Správce nasazení a rozhraní příkazového řádku Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Otevření porty a protokoly koncových bodů Linux OM v Azure
Otevřete port nebo vytvoříte koncový bod, do virtuálního počítače (OM) v Azure vytvoření filtru sítě podsítě nebo OM síťové rozhraní. Umístěte tyto filtry, které řídí příchozí a odchozí přenosy ve skupině zabezpečení síti připojených ke zdroji, která přijímá přenosy. Použijeme Běžným příkladem web přenosy na portu 80.

## <a name="quick-commands"></a>Rychlé příkazy
Vytvoření skupiny zabezpečení sítě a pravidla, které že budete potřebovat [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) nainstalovaný a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `myNetworkSecurityGroup`, a `myVnet`.

Vytvoření skupiny zabezpečení sítě, zadávání svoje vlastní názvy a umístění řádně podporovat. Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup` v `WestUS` umístění:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Přidání pravidla přenosy protokolu HTTP na serveru (nebo upravit vlastní scénáře, například SSH access nebo databáze připojení). Následující příklad vytvoří pravidla s názvem `myNetworkSecurityGroupRule` umožňující přenosy protokolu TCP na portu 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Skupina zabezpečení sítě přidružit rozhraní sítě vaší OM (NIC). Následující příklad Přidruží existující NIC s názvem `myNic` ke skupině zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Místo toho můžete propojit skupiny zabezpečení sítě s virtuální podsítě a ne právě síťového rozhraní na jeden OM. Následující příklad Přidruží existující podsítě s názvem `mySubnet` v `myVnet` virtuální sítě s skupina zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Rychlé příkazy umožňují získat do začátků s provoz vaší v angličtině. Mnoho skvělé funkcí a granularity pro řízení přístupu k prostředkům představují skupiny zabezpečení sítě. Další informace o [vytváření skupiny zabezpečení sítě a ACL pravidla tady](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Jako součást Správce prostředků Azure šablony můžete definovat skupiny zabezpečení sítě a ACL pravidla. Přečtěte si další informace o [vytváření skupin zabezpečení sítě se šablonami](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Pokud budete potřebovat použít předávání portů externí jedinečné přiřadit interní port na vaše OM, použijte při vyrovnávání zatížení a pravidla překládání adres (NAT). Můžete třeba vystavit port TCP 8080 externě a máte přenosy přesměrují do portu TCP 80 na virtuálního počítače. Se dozvíte o [vytváření Vyrovnávání zatížení internetového](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili jednoduché pravidlo umožňuje přenosy protokolu HTTP. Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:

- [Azure Přehled Správce zdrojů](../azure-resource-manager/resource-group-overview.md)
- [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Přehled Správce prostředků pro vyrovnávání zatížení](../load-balancer2    /load-balancer-arm.md)