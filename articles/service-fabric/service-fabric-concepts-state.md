<properties
   pageTitle="Definování a správě stavu | Microsoft Azure"
   description="Jak definovat a správě stavu služby v struktury služby"
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

# <a name="service-state"></a>Stav služby
**Stav služby** odkazuje na data, která službu vyžaduje, abyste mohli pracovat. Zahrnuje datové struktury a proměnných, které služba bude číst a zapíše do práce.

Zvažte do jednoduchých kalkulačky služby, třeba. Tuto službu zabírá dvěma čísly a vrátí jejich součet. Toto je čistě příslušnosti služba, která neobsahuje žádná data s ním spojené.

Nyní zvažte stejné kalkulačky, ale kromě výpočetního součet je také metodu pro vrácení posledního součet, který má spočítat. Tato služba je teď stavovou – obsahuje některé stavy, které zapíše do (je-li vypočítá nové součtu) a čtení z (když vrátí poslední počítanou součet).

V struktury služby Azure nazývá první služby příslušnosti služby. Druhá služba se nazývá stavová služba.

## <a name="storing-service-state"></a>Ukládání stavu služby
Stav lze externalized nebo umístěn společně s kód, který je manipulaci s stavu. Externalization státu obvykle probíhá pomocí externí databázi nebo úložiště přihlašovacích údajů. V našem příkladu kalkulačky může to být SQL databáze, ve kterém je uložen aktuální výsledek v tabulce. Každý požadavek pro výpočet součtu provede aktualizaci na tomto řádku.

Můžete taky stát umístit současně s kód, který pracuje tento kód. Stavová služba v služby struktury jsou vytvořené pomocí tohoto modelu. Služba struktury poskytuje infrastrukturu a ujistěte se, že je tento stav vysoce dostupné a odolnost proti chybám v případě se nepovede.

## <a name="next-steps"></a>Další kroky

Další informace o služby struktury koncepty najdete v těchto článcích:

- [Dostupnost služby struktury služeb](service-fabric-availability-services.md)

- [Škálovatelnost služby struktury služeb](service-fabric-concepts-scalability.md)

- [Rozdělení služby struktury služby](service-fabric-concepts-partitioning.md)
