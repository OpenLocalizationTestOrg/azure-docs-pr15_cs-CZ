<properties
   pageTitle="Škálovatelnost služby struktury služeb | Microsoft Azure"
   description="Popisuje, jak zobrazit služby struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Změna měřítka struktury služeb aplikací
Azure struktury služby usnadňuje vytváření scalable aplikací služby, oddíly a repliky Vyrovnávání zatížení ve všech uzlech clusteru. Díky použití maximální zdrojů.

Vysoký měřítko pro službu struktury aplikace můžete dosáhnout dvěma způsoby:

1. Změna měřítka na úrovni oddílu

2. Změna měřítka na úrovni název služby

## <a name="scaling-at-the-partition-level"></a>Změna měřítka na úrovni oddílu
Služba struktury podporuje rozdělení jednotlivých služeb do více menších oddílů. [Rozdělení přehled](service-fabric-concepts-partitioning.md) obsahuje informace o typy režimů oddílů, které jsou podporované. Kopie každý oddíl jsou rozšířit mezi uzly v clusteru. Zvažte služba, která používá ranged rozdělení schéma s nízkou klíč 0, vysoké klíč 99 a čtyři oddíly. Tři uzel clusteru službu může být rozložení s čtyři replikami, které sdílení zdrojů v jednotlivých uzlech, jak ukazuje tato část:

![Oddíl rozložení se třemi uzlů](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Zvýšení počtu uzlů umožní struktury služby pro využití zdroje na nové uzly některé replice přesunutím na prázdné uzlů. Služba se zvýšením počtu uzlů čtyři tři repliky provozu v jednotlivých uzlech (různých oddílů), umožňují lepší využití prostředků a výkonu.

![Oddíl rozložení s čtyři uzlů](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Změna měřítka na úrovni název služby
Instanci služby je konkrétní instanci název aplikace a název typu služby (viz [životního cyklu aplikace služby struktury](service-fabric-application-lifecycle.md)). Při vytváření službu zadejte oddíl schématu (viz [rozdělení struktury služby služby](service-fabric-concepts-partitioning.md)) se nemusí používat.

První úrovně měřítka je název služby. Vytvářet nové instance služby, s různé úrovně rozdělení jako svého starší instancí služby stanou zaneprázdněn. Díky nové příjemcům služby používat instancí služby méně zaneprázdněn, nikoli Vytíženější z nich.

Jednou z možností pro zvýšení kapacitu, jakož i rostoucí nebo poklesu počty oddíl je vytvořte novou instanci služby programu nový oddíl. Přidá složitosti, ale jako nutnosti náročný klientům vědět, kdy a jak tuto funkci používat službu jinak pojmenované.

### <a name="example-scenario-embedded-dates"></a>Příklad situace: vložené kalendářních dat
Jeden možný bude používat kalendářními daty v rámci název služby. Můžete třeba použít instanci služby s určitým názvem pro všechny zákazníky, kteří se připojili v 2013 a jiný název pro zákazníky, kteří se připojili v 2014. Toto schéma pojmenování umožňuje programově zvětšení názvy v závislosti na datum (2014 přístupy instanci služby 2014 lze vytvořit na vyžádání).

Tento přístup se však podle klientů pomocí specifické pro aplikaci informace o názvech, který je mimo rozsah znalostí struktury služby.

- *Používat stejný systém názvů*: V 2013 při přechodu aplikace live, vytvoříte službu s názvem struktury: / aplikace/service2013. V blízkosti druhé čtvrtletí 2013 vytvoříte jiné služby s názvem struktury: / aplikace/service2014. Jsou obě z těchto služeb stejného typu služby. Tento přístup svému klientovi, musí používat použití logických operátorů k vytvoření názvu odpovídající služeb na základě roku.

- *Použití vyhledávací služby*: jiný způsob je poskytovat sekundární vyhledávací služba, která lze zadat název služby pro požadované klíče. Vyhledávací služba bude možné vytvořit nové instance služby. Vyhledávací služba sama nezachová všechna data aplikace pouze data o názvy služeb, které vytvoří. Tím na základě roku výše uvedeném příkladu, klienta by první kontakt vyhledávací služby zjistěte název služby zpracování dat pro daný rok a pak použijte tento název služby k provádění aktuální operace. Výsledek první vyhledávání můžete mezipaměti.

## <a name="next-steps"></a>Další kroky

Další informace o služby struktury koncepty najdete v těchto článcích:

- [Dostupnost služby struktury služeb](service-fabric-availability-services.md)

- [Rozdělení služby struktury služby](service-fabric-concepts-partitioning.md)

- [Definování a správě stavu](service-fabric-concepts-state.md)
