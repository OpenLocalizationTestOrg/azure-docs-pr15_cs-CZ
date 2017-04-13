<properties
   pageTitle="Otevřete porty angličtině pomocí portálu Azure | Microsoft Azure"
   description="Zjistěte, jak otevřít port / vytvoření koncového bodu vaší Windows v angličtině pomocí nasazení modelu zdroje správce na portálu Azure"
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

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Otevření porty do virtuálního počítače v Azure pomocí portálu Azure
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Rychlé příkazy
Můžete také [pomocí těchto kroků pomocí prostředí PowerShell Azure](virtual-machines-windows-nsg-quickstart-powershell.md).

Vytvoření skupiny zabezpečení sítě. Vyberte skupinu zdroje na portálu, klikněte na **Přidat**, vyhledejte a vyberte "Sítí skupiny zabezpečení":

![Přidání skupiny zabezpečení sítě](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Zadejte název skupiny zabezpečení sítě, vyberte nebo vytvořit skupinu zdrojů a vyberte umístění. Klikněte na **vytvořit** po:

![Vytvoření skupiny zabezpečení sítě](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Vyberte novou skupinu zabezpečení sítě. Vyberte zabezpečení příchozí pravidla a potom klikněte na tlačítko **Přidat** k vytvoření pravidla:

![Přidat pravidlo pro příchozí připojení](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Zadejte název nové pravidlo. Ve výchozím nastavení je již zadali port 80. Tento zásuvné je místo, kam by změníte zdroje, protokol a určení při přidávání pravidla nehledá do skupiny zabezpečení sítě. Klikněte na tlačítko **OK** pravidlo vytvoříte takhle:

![Vytvořit pravidlo pro příchozí připojení](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Posledním krokem je skupina zabezpečení sítě přidružit podsítě nebo rozhraní určité sítě. Skupina zabezpečení sítě Pojďme přidružit podsítě. Vyberte 'Podsítí' a potom klikněte na **přidružit**:

![Skupiny zabezpečení sítě přidružit podsítě](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Vyberte virtuální sítě a pak vyberte příslušnou podsítí:

![Přidružení skupinu zabezpečení sítě virtuální sítě](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Teď jste vytvořili skupinu zabezpečení sítě vytvořili pravidlo pro příchozí připojení, který umožňuje přenosy na portu 80 a přidružený k podsítě. VMs, ke kterým se připojujete k této podsítě jsou dostupné na portu 80.


## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Rychlé příkazy umožňují získat do začátků s provoz vaší v angličtině. Mnoho skvělé funkcí a granularity pro řízení přístupu k prostředkům představují skupiny zabezpečení sítě. Další informace o [vytváření skupiny zabezpečení sítě a ACL pravidla tady](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Jako součást Správce prostředků Azure šablony můžete definovat skupiny zabezpečení sítě a ACL pravidla. Přečtěte si další informace o [vytváření skupin zabezpečení sítě se šablonami](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Pokud budete potřebovat použít předávání portů externí jedinečné přiřadit interní port na vaše OM, použijte při vyrovnávání zatížení a pravidla překládání adres (NAT). Můžete třeba vystavit port TCP 8080 externě a máte přenosy přesměrují do portu TCP 80 na virtuálního počítače. Se dozvíte o [vytváření Vyrovnávání zatížení internetového](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili jednoduché pravidlo umožňuje přenosy protokolu HTTP. Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:

- [Azure Přehled Správce zdrojů](../azure-resource-manager/resource-group-overview.md)
- [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Přehled Správce prostředků pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md)
