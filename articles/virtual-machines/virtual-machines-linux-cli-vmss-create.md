<properties
    pageTitle="Co je měřítko OM nastavuje? | Microsoft Azure"
    description="Informace o OM měřítko sady."
    keywords="Nastaví virtuálního počítače měřítko Linux virtuálního počítače" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Co je měřítko virtuálního počítače nastavuje?

Virtuální počítač měřítko sady umožňují řídit více VMs jako sady. Na vysoké úrovni měřítko sad mají následující výhody a nevýhody:

V oblasti IT:

1. Dostupnost. Každá měřítko vloží její VMs do dostupnost sada se 5 poruch domény (FDs) a 5 aktualizace domény (UDs) zajištění dostupnosti (Pokud hledáte informace o FDs a UDs, najdete v článku [OM dostupnosti](./virtual-machines-linux-manage-availability.md)). 
2. Snadno integrace se službou Azure Vyrovnávání zatížení a aplikace brány.
3. Snadno integrace se službou Azure automatické měřítko.
4. Zjednodušené nasazení, Správa a vyčištění VMs.
5. Podpora běžné Windows a Linux provedeních, jakož i vlastní obrázky.

Nevýhody:

1. Nelze se připojit disků dat na OM instance v sadě měřítko. Místo toho používají úložiště objektů Blob, Azure soubory, tabulek Azure nebo jiné řešení úložiště.

## <a name="quick-create-using-azure-cli"></a>Rychlé vytváření pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Další kroky

Obecné informace podívejte se na [hlavní cílovou stránku pro měřítko sady](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Víc si přečtěte následující dokumentaci najdete v článku [si přečtěte následující dokumentaci hlavní stránku měřítko nastaví](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Správce prostředků šablony pomocí sad měřítko, například hledat "vmss" v [Azure rychlý úvod šablony github repo](https://github.com/Azure/azure-quickstart-templates).

