<properties
    pageTitle="Pokyny pro skupiny zdrojů | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro nasazení služby Azure infrastruktury skupiny zdrojů."
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

# <a name="azure-resource-group-guidelines"></a>Pokyny pro skupinu Azure zdroje

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na vysvětlení logicky vytvoření prostředí a seskupení všech součástí skupiny zdrojů.


## <a name="implementation-guidelines-for-resource-groups"></a>Implementace pokyny pro skupiny zdrojů

Rozhodnutí:

- Chystáte vytvoření skupiny zdrojů základní součásti infrastruktury nebo nasazení dokončení aplikace?
- Potřebujete omezit přístup ke skupinám zdrojů pomocí řízení přístupu na základě rolí?

Úkoly:

- Co základní součásti infrastruktury a snaží skupiny zdrojů je třeba definujte.
- Prohlédněte si jak implementovat správce prostředků šablony konzistentní a reprodukovat nasazení.
- Definování rolích přístupu uživatele potřebujete pro řízení přístupu k skupiny zdrojů.
- Vytvoření skupiny zdrojů používat stejný systém názvů. Můžete použít rozhraní příkazového řádku Azure nebo portál.


## <a name="resource-groups"></a>Skupiny zdrojů

V Azure logické seskupení souvisejících zdroje, jako jsou úložiště účty, virtuální sítí a virtuálních počítačích (VMs) nasadit, Správa a udržovat jako jedna entita. Tento přístup usnadňuje a zachovat přitom všechny související materiály z hlediska Správa nasazení aplikace nebo jinými uživateli udělit přístup do příslušné skupiny prostředků. Komplexnější Principy skupiny zdrojů, si můžete přečíst [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Klíčové funkce skupinám zdrojů je možnost vytvořit prostředí pomocí JSON souboru, který deklaruje úložiště, sítě, a výpočet zdroje. Můžete také definovat jakékoli konfigurace chcete-li použít nebo související vlastních skriptů. Pomocí těchto JSON šablony vytvoříte konzistentní a reprodukovat nasazení aplikace. Tento postup vám umožní sestavit prostředí ve vývoji a použít tuto stejnou šablonu k vytvoření provozní nasazení, nebo naopak. Lépe pochopit informace o používání šablon, přečtěte si [šablonu návod](../resource-manager-template-walkthrough.md) , který vás provede jednotlivými kroky vytvoření šablony JSON.

Dvěma způsoby různých, kterými můžete při návrhu prostředí se skupinami zdrojů:

- Skupiny prostředků pro každou nasazení aplikace, který kombinuje úložiště účty, virtuální sítí a podsítí VMs, načíst vyrovnávání atd.
- Centralizované skupiny zdrojů, které obsahují základní virtuální sítě a podsítí nebo úložiště účtů. Používání aplikací budou do vlastní skupiny zdrojů, které obsahovat pouze VMs, Vyrovnávání zatížení sítě rozhraní, atd.

Jak přizpůsobit, vytváření centralizované skupin zdrojů virtuální sítě a podsítí usnadňuje vytváření více místní síťového připojení mezi pro hybridní konektivity. Alternativní přístup je určený pro jednotlivé aplikace vlastní virtuální síť vyžaduje konfiguraci a údržba. [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md) umožňují podrobného pomocí řízení přístupu k skupiny zdrojů. Produkčních aplikacích můžete určit uživatele, kteří mohou získat tyto informace nebo pro základní infrastruktury prostředky můžete omezit jenom infrastruktury inženýrů a pracovat s nimi. Vaše vlastníci aplikace pouze mít přístup k součásti aplikace v rámci jejich pole Skupina zdroje a ne základní Azure infrastruktury prostředí. Při návrhu prostředí vezměte v úvahu uživatelů, které vyžadují přístup ke zdroji a příslušným způsobem návrhu skupiny zdrojů. 


## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 