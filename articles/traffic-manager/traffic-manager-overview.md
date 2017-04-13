<properties
    pageTitle="Co je správce přenosy | Microsoft Azure"
    description="Tento článek vám pomůže porozumět tomu, co je přenosy správcem a zda je směrování Volba správné přenosy aplikace"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Informace o nástroji Správce přenosy

Správce Microsoft Azure přenosy umožňuje řídit rozdělení provozu uživatele pro koncové body služby v jiném datacentrech. Koncové body služby podporované správcem přenosy obsahuje Azure VMs Web Apps a cloudových služeb. Můžete taky přenosy správce s externím, není Azure koncové body.

Přenosy správce používá systému DNS (Domain Name) směrování požadavků klientů nejvhodnější koncového bodu na základě [metody přenosu směrování](traffic-manager-routing-methods.md) a stavu koncové body. Přenosy správce obsahuje velké množství metod směrování přenosu podle potřeb jinou aplikací, [Sledování](traffic-manager-monitoring.md)stavu koncového bodu a automatické převzetí. Přenosy Manager je pružné nepovede, včetně selhání celou Azure oblast.

## <a name="traffic-manager-benefits"></a>Výhody provoz správce

Přenosy správce vám mohou pomoci:

- **Zlepšit dostupnost kritické aplikací**

    Přenosy správce poskytuje dostupnost pro aplikace sledováním koncových bodů a poskytují automatické selhání při havaruje koncový bod.

- **Zlepšení rychlostí reakce výkonné aplikací**

    Azure umožňuje spuštění cloudovým službám nebo weby v datacentrech nacházejí po celém světě. Přenosy správce zlepšuje dobu odezvy aplikací přesměrováním umožnění datových přenosů do koncový bod s nejnižší latence sítě pro klienta.

- **Údržbě služby bez výpadek služeb**

    Můžete provádět operace plánované údržby na používání aplikací bez výpadek služeb. Správce provoz směrovat tak, aby umožnění datových přenosů do alternativního koncové body probíhá údržbu.

- **Kombinování místním prostředím a cloudové aplikace**

    Přenosy správce podporuje externí, není Azure koncové body umožňuje používat s hybridním cloudu a místní nasazení, včetně "požadavků na cloudu," "migrace do cloudu," a "převzetí cloudu" scénáře.

- **Distribuce přenosy složitých nasazení**

    Používání [vnořených profilů přenosy správce](traffic-manager-nested-profiles.md), metod směrování přenosu kombinací vytvářet složité a flexibilním pravidla pro podporu potřeb větší, složitější nasazení.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Další kroky

- Další informace o tom, [Jak funguje přenosy správce](traffic-manager-how-traffic-manager-works.md).

- Zjistěte, jak se dají pomocí [Správce přenosy koncový bod sledování](traffic-manager-monitoring.md)aplikací s vysokou dostupností.

- Další informace o [směrování přenosu metody](traffic-manager-routing-methods.md) podporované správcem přenosy.

- [Vytvoření profilu přenosy správce](traffic-manager-manage-profiles.md).

