<properties
   pageTitle="Jak se označuje virtuálního počítače Linux | Microsoft Azure"
   description="Informace o označování virtuálního počítače Linux vytvořené v Azure pomocí nasazení modelu správce prostředků."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Jak se označuje Linux virtuálního počítače v Azure

Tento článek popisuje různé způsoby označení Linux virtuálního počítače v Azure prostřednictvím nasazení modelu správce prostředků. Značky jsou páry uživatelem definovaných klíč/hodnota, která mohou být umístěny přímo na zdroj nebo skupina zdroje. Azure v současné době podporuje až 15 značky na zdroj a pole Skupina zdroje. Značky může být umístěny na zdroj při vytváření nebo přidali do existujícího zdroje. Dejte pozor, značky nejsou podporované pro zdroje vytvořený prostřednictvím pouze model nasazení Správce prostředků.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Označování s Azure rozhraní příkazového řádku

Zahájení, [Nainstalujte a nakonfigurujte Azure rozhraní příkazového řádku](../xplat-cli-azure-resource-manager.md) a zkontrolujte, že jsou v režimu správce prostředků (`azure config mode arm`).

Zobrazit všechny vlastnosti pro dané virtuální počítač, včetně značek, pomocí tohoto příkazu:

        azure vm show -g MyResourceGroup -n MyTestVM

Pokud chcete přidat novou značku OM prostřednictvím rozhraní příkazového řádku Azure, můžete použít `azure vm set` příkazu spolu s parametrem značku **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Chcete-li odebrat všechny značky, můžete použít parametr **– T** v `azure vm set` příkaz.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Teď, podívejte se na našeho materiály Azure rozhraní příkazového řádku a portálu jsme použili značky, Podívejme se na podrobnosti používání zobrazíte značky na portálu fakturace.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Další kroky

* Další informace o označování Azure materiály, najdete v článku [Přehled Správce prostředků Azure][] a [Pomocí značek k uspořádání Azure zdroje][].
* Se pokud chcete podívat, jak můžete pomocí značek Správa použití Azure materiály, najdete v článku [Principy faktuře Azure][] a [získat podstatu využití prostředků Microsoft Azure][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Přehled Správce zdrojů]: ../azure-resource-manager/resource-group-overview.md
[Používání značek k uspořádání svých prostředcích Azure]: ../resource-group-using-tags.md
[Principy faktuře Azure]: ../billing/billing-understand-your-bill.md
[Získat další informace na využití prostředků Microsoft Azure]: ../billing-usage-rate-card-overview.md
