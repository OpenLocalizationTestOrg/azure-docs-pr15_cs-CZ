<properties
   pageTitle="Dostupnost služby struktury služeb | Microsoft Azure"
   description="Popisuje zjišťování chyb, převzetí a obnovení služby"
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

# <a name="availability-of-service-fabric-services"></a>Dostupnost služby struktury služeb
Služby Azure služby struktury mohou být stavové nebo příslušnosti. Tento článek vám bude radit základní informace o způsobu struktury služba udržuje stavu služby v případě chyby.

## <a name="availability-of-service-fabric-stateless-services"></a>Dostupnost služby struktury příslušnosti služeb
Příslušnosti služby je služba aplikace, která neobsahuje žádné [místní trvalý stav](service-fabric-concepts-state.md).

Vytvoření příslušnosti služby vyžaduje definování instance počet, která označuje počet výskytů příslušnosti služba, která by měla běžet clusteru. Toto je počet kopií logiky aplikace, který bude vytvořena clusteru. Zvýšení počtu výskytů je doporučený způsob škálování příslušnosti služby.

Když chybu je zjištěno na všechny instance příslušnosti služby, je vytvořena nová instance na některé další měl uzel clusteru.

## <a name="availability-of-service-fabric-stateful-services"></a>Dostupnost služeb stavová služba struktury
Stavová služba obsahuje některé stavy spojené s ním. V služby struktury stavová služba modelovat jako sady repliky. Každého otevřené je instancí kód služby, která obsahuje kopii stavu. Operace pro čtení a zápis provádí u jednoho otevřené (označované jako primární). Změny uvést z zápisy jsou operace *replikovat* do více replikami (označované jako aktivní druhotné). Kombinace primární a aktivní sekundární repliky je sada otevřené služby.

Může obsahovat pouze jednu primární otevřené údržbu čtení a zápis požadavků, ale může existovat více aktivní sekundární repliky. Počet aktivní sekundární repliky je konfigurovat a vyšší počet repliky nevadí vám větší počet souběžné software a selhání hardwaru.

V případě chyby (Pokud primární otevřené havaruje) služby struktury zajišťuje jednu aktivní sekundární repliky nové primární otevřené. Tento aktivní sekundární otevřené už má aktualizovanou verzi stavu (přes *replikace*) a můžete pokračovat ve zpracování dalších pro čtení a zápis.

Tento koncepci – otevřené primární nebo aktivní sekundární – jmenoval roli otevřené.

### <a name="replica-roles"></a>Role otevřené
Role otevřené slouží ke správě životního cyklu stavu spravován této otevřené. Žádosti o čtení otevřené, jehož roli je základní služby. Také služeb požadavků na zápis tak, že aktualizace stavu a replikace změny do aktivní druhotné množiny jeho otevřené. Role aktivní sekundární je zobrazit změny stavu, které má replikovat primární otevřené a aktualizovat jeho zobrazení stavu.

>[AZURE.NOTE] Vyšší úrovně programovací modely například [spolehlivé účastníky framework](service-fabric-reliable-actors-introduction.md) abstraktní pryč koncepci otevřené role od vývojáře.

## <a name="next-steps"></a>Další kroky

Další informace o služby struktury koncepty najdete v těchto článcích:

- [Škálovatelnost služby struktury služeb](service-fabric-concepts-scalability.md)

- [Rozdělení služby struktury služby](service-fabric-concepts-partitioning.md)

- [Definování a správě stavu](service-fabric-concepts-state.md)
