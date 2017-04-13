<properties
    pageTitle="Nastavení koncových bodů na klasické OM Linux | Microsoft Azure"
    description="Zjistěte, jak pomocí nastavení koncové body Linux OM Azure klasické portálu umožňuje komunikace s Linux virtuálního počítače v Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Jak nastavit koncové body počítače Linux klasické virtuální v Azure

Všechny Linux virtuálních počítačích, které vytvoříte v Azure pomocí klasické nasazení modelu můžete automaticky komunikovat kanálu privátní sítě s jiných virtuálních počítačích ve stejném cloudové služby nebo virtuální sítě. Však počítačů v Internetu nebo jiných virtuální sítích vyžadují koncové body tak, aby příchozí síťové směrovaly do virtuálního počítače. Tento článek je také k dispozici pro [virtuálních počítačích Windows](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Správce prostředků** nasazení modelu koncové body se konfigurují **Sítě skupin zabezpečení (NSGs)**. Další informace najdete v tématu [porty počáteční a koncové body] (virtuální počítačích linux-nsg-quickstart.md).

Při vytváření virtuálních počítačů Linux Azure klasické portálu koncový bod pro zabezpečené prostředí (SSH) se obvykle vytvoří automaticky. Při vytváření virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Další kroky

* Koncový bod OM můžete taky vytvořit pomocí [rozhraní Azure příkazového řádku](../virtual-machines-command-line-tools.md). **Vytvoření koncového bodu azure OM** příkaz spusťte.

* Pokud jste vytvořili v modelu nasazení Správce prostředků virtuálního počítače, můžete Azure rozhraní příkazového řádku v režimu správce prostředků k [Vytvoření skupiny zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-cli.md) přenosy řízení bude v angličtině.