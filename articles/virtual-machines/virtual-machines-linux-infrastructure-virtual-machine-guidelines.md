<properties
    pageTitle="Pokyny pro virtuálních počítačích Linux | Microsoft Azure"
    description="Další informace o klíčových návrh a implementace pokyny pro nasazení Linux virtuálních počítačích do Azure"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Pokyny pro virtuálních počítačích

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na principy plánování kroky pro vytváření a správu virtuálních počítačích (VMs) v prostředí Azure.

## <a name="implementation-guidelines-for-vms"></a>Implementace pokyny pro VMs
Rozhodnutí:

- Kolik VMs požadujete pro různé úrovně aplikací a součástí infrastrukturu?
- K čemu prostředky procesoru a paměti musí každý OM přístup a jaké jsou požadavky na úložiště?

Úkoly:

- Definování pracovního vytížení pro aplikace a prostředky, které vyžadují VMs.
- Zarovnání požadavky zdroje pro každý OM typu odpovídající OM velikost a úložiště.
- Definujte skupiny prostředků pro různé úrovně a komponent infrastrukturu.
- Definování OM konvence.
- Vytvořte svůj VMs pomocí Azure rozhraní příkazového řádku, webového portálu, nebo se šablonami správce prostředků.

## <a name="virtual-machines"></a>Virtuálních počítačích

Jednou z hlavních částí Azure prostředí je pravděpodobně VMs. Je to, kde spuštění aplikace, databáze, ověřovacích služeb, atd.

Je důležité pochopit [OM různě](virtual-machines-linux-sizes.md) správně velikost prostředí z výkon a nákladům. Pokud vaše VMs není dost jádra procesoru a paměti, bez ohledu na to, jak je navržený a vyvinuté utrpí výkon aplikace. Navrhované úloh pro každé řadě OM jako výchozí bod podívat při se rozhodnete velikost, ve které OM můžete u jednotlivých součástí infrastrukturu. Můžete [změnit velikost OM](virtual-machines-linux-change-vm-size.md) po nasazení.

Úložiště přehraje klíčovou roli OM výkonu. Můžete použít standardní úložiště, používající běžná otáčející disků nebo Premium úložiště pro vysokou vstupu a výstupu pracovního vytížení a výkonu ve špičce, využívající SSD discích. Jako s velikostí OM nějaké jsou nákladů tipů k výběru střední úložiště. Přečtěte si [článek pokyny infrastruktury úložiště](virtual-machines-linux-infrastructure-storage-solutions-guidelines.md) pochopit, jak navrhnout odpovídající úložiště pro optimální výkon vaší VMs.


## <a name="resource-groups"></a>Skupiny zdrojů
Součásti například VMs jsou logicky seskupené pro snadnější správu a údržbu pomocí [Azure skupiny zdrojů](../azure-resource-manager/resource-group-overview.md). Pomocí skupin zdrojů můžete vytvářet, spravovat a sledovat všechny zdroje, které tvoří danou aplikaci. Můžete také implementovat [řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md) udělujete přístup uživatelům v rámci týmu pouze prostředky, které potřebují. Plánujte skupiny zdroje a přiřazení rolí dobu trvat. Skutečně návrh a implementace skupiny zdrojů, takže nezapomeňte v [článku Pokyny k používání skupin zdroje](virtual-machines-linux-infrastructure-resource-groups-guidelines.md) Pokud chcete zjistit, jak nejlépe vytvoření vaší VMs různé způsoby.


## <a name="templates"></a>Šablony 
Je možné vytvářet šablony definované deklarativní JSON soubory, vytvořte svůj VMs. Šablony obvykle vytváří také požadované úložiště, sítí, síťových rozhraní, IP adres atd spolu s VMs sami. Šablony můžete použít k vytvoření konzistentního reprodukovat prostředích pro vývoj a testování účely snadno replikovat provozním prostředí a naopak. Další informace o [vytváření a používání šablon](../azure-resource-manager/resource-group-overview.md#template-deployment) pochopit, jak je můžete používat k vytvoření a nasazení vaší VMs.


## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 