<properties
   pageTitle="Architektura spolehlivé služby | Microsoft Azure"
   description="Základní informace o architektura spolehlivé služby pro stavové a příslušnosti služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architektura stavové a příslušnosti spolehlivé služeb

Spolehlivé služby Azure služby struktury může být stavové nebo příslušnosti. Každý typ služby běží v rámci konkrétní architektury. Tyto architektury jsou popsané v tomto článku.
V tématu [Přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) Další informace o rozdílech mezi stavové a příslušnosti služby.

## <a name="stateful-reliable-services"></a>Stavová spolehlivé služby

### <a name="architecture-of-a-stateful-service"></a>Architektura stavová služba
![Diagram architektury stavové služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stavová spolehlivé služby

Stavová služba spolehlivé můžete jsou odvozeny od StatefulService nebo StatefulServiceBase třídy. Obě tyto základní třídy jsou poskytovány struktury služby. Tyto funkce poskytují různé úrovně podpory a odběru služby stavové rozhraní pro aplikaci služby struktury – a chcete tohoto programu zúčastnit jako službu v rámci služby struktury clusteru.

StatefulService pochází z StatefulServiceBase. StatefulServiceBase nabízí služby větší flexibilitu, ale vyžaduje další znalost interní struktury služby.
Podívejte se na [spolehlivé služby základní informace](service-fabric-reliable-services-introduction.md) a [spolehlivé služby Upřesnit použití](service-fabric-reliable-services-advanced-usage.md) Další informace o konkrétní psaní služby pomocí tříd StatefulService a StatefulServiceBase.

Obě základní třídy spravovat životnost a rolí implementaci služby. Implementaci služby může přepsat virtuální metody buď základní třídy Pokud implementaci služby má práce v těchto fázích životním cyklu implementaci služby – Pokud chcete vytvořit objekt posluchače komunikace. Všimněte si, že i když implementaci služby může implementovat vlastní objekt posluchače komunikace vystavení ICommunicationListener, v diagramu nahoře, komunikace posluchače prováděn struktury služby – implementaci služby používá komunikace posluchače, který provádí struktury služby.

Stavová spolehlivé služba používá správci spolehlivé stavu umožní využít výhod spolehlivé kolekcí. Spolehlivé kolekce jsou místní datovými strukturami, které jsou velmi dostupné ve službě –, která je, jsou vždy dostupné, bez ohledu na to převzetí služeb při selhání služby. Každý typ spolehlivé kolekce prováděn poskytovatele spolehlivé stavu.
Další informace o spolehlivé kolekcí najdete v článku [Přehled spolehlivé kolekcí](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Správci spolehlivé stavu a poskytovatelích stavu

Správci spolehlivé stavu je objekt, který má na starosti poskytovatelích spolehlivé stavu. Obsahuje funkce, které k vytváření, odstraňování, umožňuje zobrazit výčet a zajištění poskytovatelích spolehlivé stavu trvalých a snadno dostupné. Instanci poskytovatele spolehlivé stavu představuje instanci trvalých a snadno dostupné datových struktur, například slovník nebo fronty.

Každý poskytovatele spolehlivé stavu poskytuje rozhraní, které používá stavová služba Pokud chcete provést interakci s poskytovatele spolehlivé stavu. Použít například IReliableDictionary rozhraní spolehlivé slovník, zatímco IReliableQueue slouží jako rozhraní s spolehlivé fronta. Všech poskytovatelů spolehlivé stavu implementovat rozhraní IReliableState.

Správci spolehlivé stavu má rozhraní s názvem IReliableStateManager, což umožňuje přístup k němu stavové služby. Rozhraní poskytovatelům spolehlivé stát se vracejí prostřednictvím IReliableStateManager.

Správci spolehlivé stavu používá Plug-inu architekturu tak, aby nových typů spolehlivé kolekcí zapojit dynamicky.

Spolehlivé slovník a spolehlivé fronty jsou založeny na provádění výkonných, verzí rozdílné úložiště.

### <a name="transactional-replicator"></a>Transakční replikace

Součást transakční replikace je zajistí, že stav služby (to znamená stavu ve Správci spolehlivé stavu a spolehlivé kolekcí) odpovídá mezi ostatními službou. Také zaručuje, že je zachován stavu u protokolu. Rozhraní spolehlivé stavu správce s transakční replikace prostřednictvím mechanismus soukromé.

Transakční replikace používá síťový protokol pro komunikaci stavu s ostatními replikami instance služby tak, aby ostatními informace o aktuálním stavu.

Transakční replikace používá protokol k zachování informace o stavu, aby informace o stavu odolává obrázku nebo dojde k chybě uzel. Rozhraní pro protokol spočívá ve využití mechanismus soukromé.

### <a name="log"></a>Log

Součást protokolu poskytuje výkonné trvalé úložiště, který může být optimalizovaný pro zápis otáčejícího nebo pevných látek discích.  Návrh protokol je pro trvalé úložiště (tedy pevných discích) místní uzly spuštěné stavová služba. To umožňuje zhoršeným čekacích dob a s vysokou, ve srovnání s vzdálené trvalý úložiště, která není místní na uzel.

Součást protokolu používá víc souborů protokolu. Existuje uzel celé sdílený soubor protokolu s ostatními použít jak dát jí tak latence nejnižších a nejvyšších výkon pro ukládání data stavu. Ve výchozím nastavení protokolu sdílené umístěn v adresáři služby struktury uzel práce ale může nakonfigurovat taky umístění na jiné místo v ideálním případě na disku rezervovaná protokolu pouze sdílené. Každého otevřené pro službu má taky vyhrazený protokol a protokol vyhrazené je umístěn v adresáři služby práce. Neexistuje žádný mechanismus konfigurace protokol vyhrazené umístění na jiné místo.

Protokol sdílené je přechodová oblast pro informace o stavu otevřené, zatímco vyhrazené protokol je cíli, kde je zachován. V tomto návrhu informace o stavu nejdřív aby došlo k zápisu sdílený soubor protokolu a pomalu přesune do vyhrazené protokolu na pozadí. Tímto způsobem budou mít zápisu do protokolu sdílené latence nejnižších a nejvyšších výkon, který umožňuje služby urychlit průběh.

Čtení a zapíše do protokolu sdílené dělají prostřednictvím přímé vstupu a výstupu do předběžně přidělené místa na disku souboru sdíleného protokolu. Umožňuje optimálně používat místa na disku s vyhrazené protokoly vyhrazené protokol vytvořen jako soubor sparse NTFS. Všimněte si, že to vám umožní overprovisioning místa na disku a operační systém zobrazí vyhrazené protokoly pomocí mnohem víc místa na disku než skutečně používat.

Kromě rozhraním minimální režimu uživatele do protokolu protokol uváděný jako ovladač jádra. Při spuštěné jako ovladač jádra protokol umožníte nejvyšší výkon ke všem službám, které používá.

Další informace o konfiguraci protokol najdete v článku [Konfigurace stavové spolehlivé služby](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Příslušnosti spolehlivé služby

### <a name="architecture-of-a-stateless-service"></a>Architektura příslušnosti služby
![Diagram architektury příslušnosti služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Příslušnosti spolehlivé služby

Implementace příslušnosti služby jsou odvozeny od třídy StatelessService nebo StatelessServiceBase. StatelessServiceBase předmětu umožňuje větší flexibilitu než StatelessService předmětu.
Obě základní třídy spravovat životnost a rolí služby.

Implementaci služby může přepsat virtuální metody buď základní třídy Pokud službu má pracovní postup v těchto fázích životního cyklu služby – Pokud chcete vytvořit objekt posluchače komunikace. Všimněte si, že i když službu může implementovat vlastní objekt posluchače komunikace vystavení ICommunicationListener, v diagramu nahoře, posluchače komunikace prováděn služby struktury plnění služby používá komunikace posluchače, který provádí struktury služby.

Podívejte se na [spolehlivé služby základní informace](service-fabric-reliable-services-introduction.md) a [spolehlivé služby Upřesnit použití](service-fabric-reliable-services-advanced-usage.md) Další informace o konkrétní psaní služby pomocí tříd StatelessService a StatelessServiceBase.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Další informace o služby struktury najdete tady:

[Spolehlivé služby základní informace](service-fabric-reliable-services-introduction.md)

[Rychlý start](service-fabric-reliable-services-quick-start.md)

[Spolehlivé kolekce přehled](service-fabric-reliable-services-reliable-collections.md)

[Spolehlivé služby rozšířené možnosti použití](service-fabric-reliable-services-advanced-usage.md)

[Konfigurace spolehlivé služby](service-fabric-reliable-services-configuration.md)  
