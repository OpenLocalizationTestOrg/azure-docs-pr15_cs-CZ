<properties
    pageTitle="Dostupnost nastavit pravidla | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro nasazení dostupnost sady v Azure infrastruktury služby."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Dostupnost nastavuje pravidla

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na principy plánování kroky pro dostupnost sady a zajistit, aby vaše aplikace zůstane přístupných osobám s postižením během události plánované nebo neplánované.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementace pokyny pro dostupnost sady

Rozhodnutí:

- Kolik dostupnost sady potřebujete pro různé role a úrovní v infrastrukturu aplikace?

Úkoly:

- Určení počtu VMs v každé vrstvy aplikace, kterou požadujete.
- Určete, pokud potřebujete upravte počet poruch nebo aktualizovat domén pro použití aplikace.
- Definování výše uvedené množiny požadované dostupnost používat stejný systém názvů a co VMs nacházejí v nich. Virtuálního počítače mohou být uloženy pouze jeden dostupnost sady. 

## <a name="availability-sets"></a>Dostupnost sady

V Azure mohou být umístěny ve virtuálních počítačích (VMs) pro logické seskupení s názvem sady dostupnosti. Při vytváření VMs v rámci sady dostupnost Azure platformu distribuuje umístění tyto VMs mezi základní infrastruktury. By měly být plánované údržbě platformu Azure nebo podkladového hardwaru / infrastruktury poruch použití sady dostupnost zaručuje, zůstane alespoň jeden OM pracovního.

Osvědčený neměli aplikací umístěny na jedné OM. Dostupnost sadu, která obsahuje jediný OM nemá získat ochrany z plánované nebo neplánované událostí v Azure platformu. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) vyžaduje dva nebo více VMs v rámci dostupné umožňují rozdělení VMs prostřednictvím podkladového infrastruktury nastavení.

Základní infrastruktury v Azure rozdělen v aktualizovat domén a poruch domén. Tyto domény se definují jaké hostitelů sdílení běžné aktualizace obrázku nebo sdílet podobné fyzické infrastruktury například power a sítě. Azure automaticky distribuuje VMs v rámci dostupné nastavit mezi doménami udržovat dostupnost a odolnost proti chybám. V závislosti na velikosti aplikace a počet VMs v rámci sady dostupnost můžete upravit počet domény, kterou chcete použít. Je další informace o [správě dostupnosti a používání aktualizace a odolnost domén](virtual-machines-windows-manage-availability.md).

Při návrhu aplikace infrastruktury, měli byste naplánovat také vrstev aplikace, které používáte. Nastaví VMs skupiny, které slouží ke stejnému účelu v dostupnosti, jako jsou dostupné pro váš front-end VMs službou IIS. Vytvoření samostatného dostupnost nastavení pro svůj back-end VMs serverem SQL Server. Cílem je zajistit, aby jednotlivé součásti aplikace se po zamknutí podle sady dostupnost a aspoň jednou vždy zůstane instance pracovního.

Vyrovnávání zatížení můžete využít před jednotlivé vrstvy aplikace se dá pracovat společně sady dostupnost a zajistěte, aby vždy možné směrovat přenosy na instanci. Bez vyrovnávání zatížení vaší VMs může pokračovat v práci v celém neplánované a plánované údržby události, ale koncoví uživatelé nebudou moci vyřešit, pokud primární OM není k dispozici.


## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 