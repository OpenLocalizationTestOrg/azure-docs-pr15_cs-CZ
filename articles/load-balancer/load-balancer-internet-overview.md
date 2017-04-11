
<properties
   pageTitle="Internetové přehled Vyrovnávání zatížení | Microsoft Azure "
   description="Přehled pro internetové služby Vyrovnávání zatížení a jeho funkcí. Jak funguje Vyrovnávání zatížení pro Azure pomocí virtuálních počítačích a cloudovým službám."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Přehled Vyrovnávání zatížení vystaveného internetového

Vyrovnávání zatížení Azure mapy veřejnou IP adresu a číslo portu příchozích na privátní IP adresa a port číslo virtuálního počítače a naopak pro přenos dat odpověď z virtuální počítač. Vyrovnávání zatížení pravidla umožňují distribuovat specifické typy přenosů mezi více virtuálních počítačích nebo služeb. Například můžete rozšířit načíst web žádost o přenosu mezi více webových serverů nebo web role.

Pro cloudové služby, který obsahuje výskyty role webu nebo pracovního role můžete definovat veřejné koncového bodu v souboru definice (.csdef) služby.

Soubor _servicedefinition.csdef_ obsahuje konfigurace koncového bodu a obsahujících několika instancí role role nasazení webu nebo pracovního Vyrovnávání zatížení bude instalace. Způsob, jak přidat instance do cloudu nasazení je změnit počet instancí v souboru konfigurace služby (.csfg).

Následující obrázek znázorňuje Vyrovnávání zatížení endpoint pro přenos šifrované web, který je sdílen tři virtuálních počítačích pro veřejných a privátních port TCP 443. Tyto tři virtuálních počítačích se v množině Vyrovnávání zatížení.

![Příklad Vyrovnávání zatížení veřejné](./media/load-balancer-internet-overview/IC727496.png))

Obrázek 1 – Vyrovnávání zatížení koncový bod pro přenos šifrované web

Po Internetu klienti odešlou webovou stránku na veřejnou IP adresu cloudové služby na TCP port 443, Vyrovnávání zatížení Azure distribuuje požadavky mezi třemi virtuálních počítačích v sadě Vyrovnávání zatížení. Další informace o algoritmech Vyrovnávání zatížení najdete v článku [načtení stránky přehled vyrovnávání](load-balancer-overview.md#load-balancer-features).

Ve výchozím nastavení Vyrovnávání zatížení Azure distribuuje rovnoměrně několika instancích spuštěných virtuálního počítače v síti. Můžete taky nakonfigurovat relace spřažení Další informace najdete v tématu [režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Další kroky

Informace o [interní služba Vyrovnávání zatížení](load-balancer-internal-overview.md) a snadněji pochopit, které Vyrovnávání zatížení je vešel cloudu nasazení.

Můžete taky [začít vytvářet internetové Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace jaký druh [režimu rozdělení](load-balancer-distribution-mode.md) konkrétní vyrovnávání sítě přenosy GUID.

Pokud potřebujete-li zachovat spojení aktivní serverů za vyrovnávání zatížení aplikace, můžete chápete víc o [Nastavení časového limitu nečinnosti TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). Další informace o chování nečinná připojení, když používáte Vyrovnávání zatížení Azure vám pomůže.
