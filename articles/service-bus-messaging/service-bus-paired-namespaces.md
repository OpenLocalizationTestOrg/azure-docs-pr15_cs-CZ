<properties 
    pageTitle="Služba Bus párovaný obory názvů | Microsoft Azure"
    description="Podrobnosti o implementaci párových názvů a náklady"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Párovaný podrobnosti implementace názvů a náklady důsledky

Metoda [PairNamespaceAsync][] pomocí instanci [SendAvailabilityPairedNamespaceOptions][] provádí viditelné úkoly vaším jménem. Protože tam jsou náklady co byste měli zvážit při použití funkce, je vhodné porozumět těchto úkolů, abyste se to děje očekávané chování. Rozhraní API věnuje následující automatické chování vaším jménem:

-   Vytvoření rezervu fronty.
-   Vytvoření [MessageSender][] objekt, který pojednává fronty nebo témata.
-   Při zasílání zpráv entity nebude k dispozici, odešle příkazu ping zprávy entity při pokusu o zjistit, kdy entity znovu k dispozici
-   Volitelně vytvoří množiny "čerpadel zprávu", přesouvat zprávy z fronty rezervy do primární fronty.
-   Souřadnice kulatou/chybující [MessagingFactory][] instancí primárních a sekundárních.

Na vysoké úrovni, bude tato funkce takto: primární entity správný, žádné změny v chování proběhne. Kdy doby trvání [FailoverInterval][] uplyne a primární entita uvidí ne úspěšné po odešle nepřechodná [MessagingException][] nebo [TimeoutException][]následující chování:

1.  Odeslání jsou zakázány operace primární entita a systému pomocí příkazu ping primární entity dokud příkazu ping můžete úspěšně doručené.

2.  Náhodné rezervu fronty zaškrtnuté.

3.  Objekty [BrokeredMessage][] jsou směrovány do fronty zvolené rezervu.

1.  Když operace Odeslat do fronty zvolené rezervu nepovede, fronty pocházejí z otočení a nové fronty zaškrtnuté. Zjistěte, všem odesílatelům instance [MessagingFactory][] chyby.

Na následujících obrázcích znázornění pořadí. Nejdřív odesílatele odesílat zprávy.

![Párových obory názvů][0]

Po selhání odeslání do primární fronty odesílatele, nebude zahájen posílání zpráv do fronty náhodně vybraného rezervu. Současně spustí příkaz ping úkolu.

![Párových obory názvů][1]

Zprávy v tomto okamžiku jsou ještě ve frontě sekundární a nebyla doručena primární fronty. Jakmile bude primární fronta správný znovu, alespoň jeden by měla běžet proces Trativod. Trativod poskytuje zprávy ze všech různých frontách rezervu správné cíl entit (fronty a témata).

![Párových obory názvů][2]

Zbytek Toto téma popisuje podrobnosti o fungování těchto částí.

## <a name="creation-of-backlog-queues"></a>Vytvoření rezervu fronty

Objekt [SendAvailabilityPairedNamespaceOptions][] předán metodě [PairNamespaceAsync][] označuje počet fronty rezervu, který chcete použít. Každý rezervu pak vytvoření fronty s následujícími vlastnostmi explicitně sadu (všechny ostatní hodnoty se nastaví na výchozí nastavení [QueueDescription][] ):

| Cesta                                   | [primární názvů] / x servicebus přenosu / [index] kde [index] představuje hodnotu v [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int. MaxValue                                                                                         |
| DefaultMessageTimeToLive               | Hodnotu TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | Hodnotu TimeSpan.MaxValue                                                                                    |
| Trvání uzamčení                           | 1 minuta                                                                                             |
| EnableDeadLetteringOnMessageExpiration | PRAVDA                                                                                                 |
| EnableBatchedOperations                | PRAVDA                                                                                                 |

Například první fronty rezervu vytvořené pro s názvem názvů **contoso** `contoso/x-servicebus-transfer/0`.

Při vytváření frontách, kód nejprve zkontroluje, pokud takové fronty existuje. Pokud fronty neexistuje, je vytvořen fronty. Kód nemá vyčistit "navíc" rezervu fronty. Konkrétně-li aplikaci primární názvů **contoso** požádáni o pět nevyřízené položky fronty ale rezervu fronty s cestou `contoso/x-servicebus-transfer/7` existuje, fronty navíc rezervu stále existuje, ale není použit. Systém explicitně umožňuje navíc rezervu fronty existují, které by použít. Jakožto vlastníka názvů zodpovídáte za vyčištění všechny nepoužívané/nežádoucí rezervu fronty. Důvody tohoto rozhodnutí je, že služby Bus nelze zjistit, jaké účely existují pro všechny fronty v oboru názvů. Kromě toho pokud fronty existuje se zadaným názvem však předpokládá, že [QueueDescription][]nesplňuje vaše důvodů, proč budou vlastní ke změně výchozího chování. Žádné záruky jsou určené pro úpravy fronty rezervu příslušný kód. Zkontrolujte, že důkladně otestovat provedené změny.

## <a name="custom-messagesender"></a>Vlastní MessageSender

Při odesílání, všechny zprávy absolvovat interní [MessageSender][] objekt, který se chová obvykle při všechno funguje a přesměruje fronty rezervu při věci "přerušit." Po přijetí selhání nepřechodná, spustí se časovač. Po určité době [TimeSpan][] obsahující hodnotu [FailoverInterval][] během níž nejsou odesílány žádné úspěšné zprávy záložní provozuje. V tomto okamžiku následující nastane pro každou entitu:

- Úkol ping provede každé [PingPrimaryInterval][] zkontrolovat, zda je dostupné entity. Po úspěšném tento úkol všechny kód klienta, který používá entitu okamžitě spustí odesílání nových zpráv do primární názvů.

- Budoucí požadavky k odeslání, že stejný subjekt od jiných odesílatelů bude mít za následek [BrokeredMessage][] se odešlou lze do ve frontě rezervu. Změna odebere některé vlastnosti objektu [BrokeredMessage][] a ukládá kdekoli jinde. Následující vlastnosti jsou zrušeno a přidat v části nový alias povolení služby Bus a SDK rovnoměrně zpracování zpráv:

| Původní název vlastnosti       | Nový název vlastnosti |
|-------------------------|-------------------|
| ID relace               | x-ms-ID relace    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms cesta         |

Původní cílová cesta je také uložena v zprávu jako vlastnost s názvem x-ms-path. Tento návrh umožňuje zpráv pro entity existenci ve frontě jednoho rezervu. Vlastnosti jsou převést zpět tak, že Trativod.

Vlastní objekt [MessageSender][] může dojít k potížím při zprávy přiblíží limit 256 KB a převzetí provozuje. Vlastní objekt [MessageSender][] ukládá zprávy pro všechny fronty a témata společně fronty rezervu. Tento objekt smíchává zprávy z mnoha základní barvy společně ve frontách rezervu. Zpracování mezi mnoho klientech nebudou vědět, vzájemně Vyrovnávání zatížení, SDK náhodně použije jeden fronty rezervu pro každou [QueueClient][] nebo [TopicClient][] vytvoříte v kódu.

## <a name="pings"></a>Příkazu ping

Zpráva ping je prázdné [BrokeredMessage][] aplikace/vnd.ms-servicebus-ping a [TimeToLive][] hodnotu 1 sekunda vlastnost [typ obsahu][] . Tento příkaz ping je jedna speciální charakteristika Bus služby: nikdy doručen ping na všechny volající požádá o [BrokeredMessage][]. Proto se ukládání nemusíte vůbec starat se dozvíte, jak přijímat a ignorovat tyto zprávy. Každá entita (jedinečné fronty nebo tématu) za [MessagingFactory][] instance ke klientovi bude příkaz ping, když jsou považovány za je k dispozici. Ve výchozím nastavení děje se jednou za minutu. Ping zprávy se považuje za běžné služby Bus zprávy a může mít za následek poplatky za pásma nebo zprávy. Jakmile klienti zjistit, že systému k dispozici, zprávy vypnout.

## <a name="the-syphon"></a>Trativod

Alespoň jeden spustitelný soubor v aplikaci by měl být aktivně aplikaci Trativod. Trativod provádí typu long hlasování přijímání, která trvá 15 minut. Když máte 10 fronty rezervu všechny entity jsou k dispozici, aplikace, který je hostitelem Trativod volá operace přijímání 40 časy / hod 960 časy po dnech a 28 800 bitů časy 30 dní. Při Trativod je aktivně z rezervu přesouvání zpráv do primární fronty, dojde k každé zprávy následující poplatky (standardní velikosti zpráv a šířky pásma poplatky postupně všechny):

1.  Odešlete rezervu.

2.  Podívejte se rezervu.

3.  Odešlete primární.

4.  Podívejte se primární.

## <a name="closefault-behavior"></a>Zavřít/poruch chování

V aplikaci, která hostuje Trativod jednou chyby [MessagingFactory][] primární a sekundární nebo zavření bez partnerského je také k chybě/nezavřeli a Trativod rozpozná tento stav úkony Trativod. Pokud ostatní [MessagingFactory][] není zavřeli vyvolané 5, Trativod chybu stále otevřené [MessagingFactory][].

## <a name="next-steps"></a>Další kroky

Podrobný popis služby Bus asynchronní zpráv najdete v článku [asynchronní zpráv vzorů a dostupnost][] . 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Časového rozpětí]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Typ obsahu]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchronní zpráv vzorů a dostupnost]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
