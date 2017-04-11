<properties 
    pageTitle="Událost rozbočovače často kladené otázky ke | Microsoft Azure"
    description="Nejčastější dotazy týkající se rozbočovače události."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Událost rozbočovače časté otázky

Událost rozbočovače poskytuje rozsáhlé příjmu trvalé a zpracování událostí data ze zdroje dat vysoký výkon a/nebo miliony zařízení. Při použití souběžně s služby Bus fronty a témata, událostí rozbočovače umožňuje trvalý příkazy a ovládání nasazení scénářích [Internet věcí (IoT)](https://azure.microsoft.com/services/iot-hub/) .

Tento článek popisuje ceny informace a najdete odpovědi na některé časté otázky k události rozbočovače:

## <a name="pricing-information"></a>Ceny informace

Podrobné informace o události rozbočovače ceny zobrazit [Podrobnosti o cenách rozbočovače události](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Jak se počítají události rozbočovače průniku událostí?

Každou událost odeslaná k rozbočovači události se počítá jako fakturaci zprávy. *Událost průniku* je definována jako celek data, která je menší nebo rovna hodnotě 64 KB. Události, která je menší nebo rovna hodnotě 64 KB velikosti se považuje za jeden fakturaci události. Pokud je větší než 64 KB událost, počet fakturaci událostí se vypočítá podle velikosti událostí v násobcích 64 KB. Příklad 8 KB událost odeslaná k rozbočovači události fakturované jako jednu událost, ale 96 KB zpráva odeslaná jako centrální události fakturované jako dvou událostí.

Události spotřebované množství od rozbočovači událost jako i operace správy řízení volání například kontrolních bodů, nejsou počítány jako fakturaci průniku události, ale metodu nabíhání nákladů až příspěvek jednotku výkon.

## <a name="what-are-event-hubs-throughput-units"></a>Jaké jsou události rozbočovače výkon jednotky?

Explicitně vybrat události rozbočovače výkon jednotky prostřednictvím portálu Azure nebo šablony správce zdrojů rozbočovače události. Výkon jednotky platí pro všechny události rozbočovače v názvů rozbočovače události, a každou výkon jednotku opravňuje oboru k následující možnosti:

- Až 1 MB sekundu průniku události (události odeslané do rozbočovači události), ale bez více než 1000 průniku události, Správa operace nebo ovládacího prvku rozhraní API hovorů za sekundu.

- Až 2 MB sekundu výstupní události (události spotřebované množství od rozbočovači událostí).

- Až 84 GB úložiště události (dostatečná po dobu 24 hodin uchovávání výchozí).

Událost rozbočovače výkon jednotky účtovány každou hodinu, podle maximální počet jednotek vybraný během zadané hodiny.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Jak se nevynucují události rozbočovače výkon jednotku omezení?

Pokud průniku celkový výkon nebo úroková.míra události celkové průniku přes všechny události rozbočovače v oboru překročí příspěvky agregační výkon jednotku, odesílatelů se sníží a přijímání chyb, které označují překročení kvóty průniku.

Pokud výstupní celkový výkon nebo úroková.míra výstupní celkové události přes všechny události rozbočovače v oboru překročí příspěvky agregační výkon jednotky, příjemce jsou omezena a označující překročení kvóty výstupní k chybám. Kvóty průniku a výstupním vynucuje samostatně, takže žádné odesílatele mohou způsobit události spotřebu zpomalit ani příjemce zabránit události odesílaného do rozbočovači události.

Všimněte si, že je výběr jednotky výkon nezávisle na počet oddílů rozbočovače události. Během každý oddíl nabízí maximální výkon 1 MB na druhý průniku (plus pár maximálně události 1000 sekundu) a 2 MB na druhý výstupní, je bezplatná pevné oddíly sami. Náklady je určený pro agregované výkon jednotky na všechny události rozbočovače v názvů rozbočovače události. Pomocí tohoto vzorku můžete vytvořit dost oddíly podporovat očekávané maximální načíst pro systémy, bez nabíhání všechny poplatků za jednotku výkon až načíst událostí systému skutečně vyžaduje vyšší výkon čísla, aniž byste museli měnit strukturu a architekturu vašich systémů jako načíst do systému zvyšuje.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Existuje limit počtu výkon jednotky, které mohou být vybrány?

Je výchozí kvóta 20 jednotek výkon na obor názvů. Větší kvóty zboží výkon můžete požádat o tak, že používá se zařazování požadavek podpory můžete. Za jednotku limit 20 výkon sady jsou dostupné v 20 až 100 jednotek výkon. Všimněte si, že pomocí víc než 20 jednotek výkon odstraní možnost změnit počet jednotek, výkon bez archivace požadavek podpory můžete.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Existuje nákladů pro zachování události rozbočovače událostí pro víc než 24 hodin?

Událost rozbočovače standardní osy povoluje uchovávání informací zprávy období delší než 24 hodin, než 30 dní. Pokud velikost celkový počet události uložené překročí příspěvek úložiště pro počet jednotek vybraného výkon (84 GB za jednotku výkon), velikost, která překračuje příspěvek Účtovaná rychlostí publikované úložiště objektů Blob Azure. Příspěvek na úložiště v každé jednotce výkon zahrnuje všechny úložiště náklady za období uchování 24 hodin (výchozí) i v případě jednotku výkon čerpány maximální průniku příspěvek.

## <a name="what-is-the-maximum-retention-period"></a>Co je období uchování maximální?

Uchovávání informací maximální dobu 7 dní v současné době podporuje rozbočovače standardní osy události. Poznámka: události rozbočovače nejsou určeny jako trvalé úložiště. Období uchování větší než 24 hodin jsou určené pro situace, ve které je užitečné k přehrání toku událostí do stejné systémů; Chcete-li například školení nebo zkontrolovali nový model výukové počítače na existujících datech.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Jak velikosti úložiště rozbočovače události výpočtu a Účtovaná?

Celková velikost všechny uložené události, včetně všech interní režijních záhlaví událost nebo na disku úložiště struktury v všechny události rozbočovače měří celý den. Na konci den se vypočítá velikosti úložiště ve špičce. Denní příspěvek úložiště je vypočítána na základě minimální počet jednotek výkon, které jste vybírali během dne (jednotlivé výkon jednotky poskytuje příspěvek 84 GB): Pokud na celkovou velikost překročí počítané denní příspěvek úložiště, nadbytečné úložiště je vám účtovat poplatky pomocí sazby úložiště objektů Blob Azure (sazbou **Místně nadbytečné úložiště** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Dá se pomocí jednoho připojení AMQP odesílat a přijímat z rozbočovače událostí a služby Bus fronty/témata?

Ano, pokud jsou všechny události rozbočovače, fronty a témata v oboru stejné. Jako takové můžete provádět obousměrné brokered připojení na mnoha zařízeních s subsecond čekacím efektivní a vysoce scalable způsobem.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Brokered připojení poplatky k události rozbočovače?

Odesílatelům připojení se poplatky za pouze v případě použití protokolu AMQP. Existuje žádné poplatky připojení pro odesílání událostí pomocí protokolu HTTP, bez ohledu na počet odesílání systémy nebo zařízení. Pokud máte v plánu používat AMQP (například dosáhnout efektivnější streamování událost nebo povolte nebo zakažte obousměrné komunikaci v IoT příkazy a ovládání scénáře), získáte na stránce [služby Bus ceny informace](https://azure.microsoft.com/pricing/details/service-bus/) pro informace o co představuje brokered připojení a jak se budou podle objemu dat.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Jaký je rozdíl mezi události rozbočovače základní a standardní úrovní?

Standardní rozbočovače osy události poskytuje funkce rámec toho, co je k dispozici v události rozbočovače základní a v některých konkurenční systémy. Tyto funkce patří uchovávání informací dobu, kdy je delší než 24 hodin a možnost používat jediné připojení AMQP odešlete velkého počtu zařízení s čekacích subsecond dob příkazy, jakož i posílání telemetrie z těchto zařízení do rozbočovače události. Seznam funkcí Zobrazit [Podrobnosti o cenách rozbočovače události](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Zeměpisná dostupnosti

Událost rozbočovače je k dispozici v těchto oblastech:

|GEO|Oblasti|
|---|---|
|Spojené státy americké|Centrální USA východoasijských USA, východoasijských USA 2, Jižní centrální USA, západ USA|
|Europe|Severní Europe západní Evropě|
|Asijsko-tichomořské|Východní Asie jihovýchodní Asie|
|Japonsko|Japonsko východ západ Japonsko|
|Brazílie|Brazílie jih|
|Austrálie|Austrálie Východ Austrálie jihovýchodní|

## <a name="support-and-sla"></a>Podpora a SLA

Technická podpora pro událost rozbočovače je k dispozici až [komunitních fór](https://social.msdn.microsoft.com/forums/azure/home). Podpora správy fakturace a předplatného je k dispozici zdarma.

Další informace o naše SLA, najdete na stránce [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Další kroky

Další informace o události rozbočovače, naleznete v následujících článcích:

- [Přehled rozbočovače události][].
- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].

[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
