<properties 
    pageTitle="Oddíly fronty a témata | Microsoft Azure"
    description="Popisuje, jak oddíl služby Bus fronty a témata pomocí zprostředkovatelů více zpráv."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Rozdělený fronty a témata

Azure Bus služby používá více zprostředkovatelů zpráv pro zpracování zpráv a více zpráv ukládají pro ukládání zpráv. Normální fronty nebo tématu je uskutečněných jednotlivými zprostředkovatele jednu zprávu a uloženým v úložišti jeden zasílání zpráv. Služba Bus zároveň povolíte fronty nebo tématech na rozdělený napříč několika zprostředkovatelů zprávy a zpráv ukládají. To znamená, že celkový výkon rozdělený fronty nebo tématu už omezený jazykem výkon zprostředkovatele podstatu sdělení nebo zpráv úložiště. Kromě toho dočasné výpadku zpráv úložiště nezobrazuje rozdělený fronty nebo tématu není k dispozici. Rozdělený fronty a témata může obsahovat všechny pokročilé funkce Bus služby, jako jsou podpory pro transakce a relace.

Podrobné informace o interní Bus služby naleznete v tématu [Architektura Bus služby][] .

## <a name="how-it-works"></a>Jak to funguje

Jednotlivé oddíly fronty nebo tématu se skládá z více neúplné. Každou část se ukládají v úložišti různých zasílání zpráv a uskutečněných jednotlivými zprostředkovatele odlišnou zprávu. Po odeslání zprávy do oddílů fronty nebo tématu služby Bus přiřadí zprávu jednu fragmenty. Výběr probíhá náhodně Bus služby nebo pomocí kódu Product key oddíl můžete zadat odesílatele.

Pokud klient požaduje zpráva z oddílů fronty nebo z předplatného rozdělený téma dotazy služby Bus všechny neúplné pro zprávy, pak vrátí první zprávu, která pochází z jednotlivých zpráv ukládají příjemci. Služba Bus mezipaměti druhý zprávy a vrátí je Jakmile obdrží další přijímat požadavky. Přijímání klientský počítač není vědět rozdělení; chování klientů webových rozdělený fronty nebo tématu (například číst, vyplnit, pozdržet nedoručených zpráv, prefetching) je stejná jako chování běžná entity.

Neexistuje žádná další náklady při odesílání zprávy nebo přijímat zprávy z oddílů fronty nebo tématu.

## <a name="enable-partitioning"></a>Povolení rozdělení

Pomocí služby rozdělený fronty a témata Bus služby Azure, použít SDK Azure verze 2.2 nebo novější, nebo zadat `api-version=2013-10` vaše protokolu HTTP žádosti.

Můžete vytvořit služby Bus fronty a témata o velikosti 1, 2, 3, 4 a 5 GB (výchozí hodnota je 1 GB). Pomocí rozdělení povoleno, vytvoří služby Bus 16 oddíly pro každou GB zadáte. Jako například pokud vytváříte fronta, v níž je 5 GB velikosti 16 oddílů maximální velikost fronty stane (5 \* 16) = 80 GB. Zobrazí se maximální velikosti doručovaných rozdělený fronty nebo tématu pohledem na vstupu na [Azure portálu][].

Existuje několik způsobů, jak vytvořit rozdělený fronty nebo tématu. Když vytvoříte fronty nebo tématu z aplikace, můžete povolit rozdělení pro fronty nebo tématu respektive nastavením vlastnosti [QueueDescription.EnablePartitioning][] nebo [TopicDescription.EnablePartitioning][] **logickou hodnotu PRAVDA**. Tyto, musíte nastavit vlastnosti v době fronty nebo téma se vytvoří. Není možné změnit tyto vlastnosti na existující fronty nebo tématu. Příklad:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Případně můžete vytvořit rozdělený fronty nebo tématu ve Visual Studiu nebo [Azure portálu][]. Při vytváření nové fronty nebo tématu na portálu nastavte možnost **Povolit oddílů** v zásuvné **Obecná nastavení** fronty nebo tématu okno **Nastavení** na **hodnotu true**. Ve Visual Studiu zaškrtněte políčko **Povolit oddílů** v dialogovém okně **Nové fronty** nebo **Nového tématu** .

## <a name="use-of-partition-keys"></a>Použití oddíl klíčů

Pokud je zpráva byla zařazena do fronty do oddílů fronty nebo téma, Bus služby hledá stavu klíč oddílu. Pokud nenalezne, vybere fragment založené na tuto klávesu. Pokud klíč oddílu nenajde, vybere fragment podle interní algoritmus.

### <a name="using-a-partition-key"></a>Pomocí oddílu

Některé scénáře, například relace nebo transakce, je nutné zprávy uložená v konkrétní fragmentu. Všechny podobnému sledu vyžaduje použití klíč oddílu. Všechny zprávy, které používají stejnou klíč oddílu přiřazené stejné fragmentu. Pokud fragment je dočasně nedostupný, služby Bus vrátí chybu.

V závislosti scénář odlišnou zprávu vlastnosti slouží jako klíč oddíl:

**ID relace**: Pokud zpráva obsahuje sadu vlastností [BrokeredMessage.SessionId][] a pak Bus služby používá tato vlastnost jako klíč oddílu. Tímto způsobem všech zpráv, které patří k relacím jsou uskutečněných jednotlivými stejné zprostředkovatele zprávy. Díky služby Bus zajistit zprávy řazením a konzistence stavy relace.

**PartitionKey**: Pokud zpráva obsahuje vlastnost [BrokeredMessage.PartitionKey][] , ale ne [BrokeredMessage.SessionId][] nastavení vlastnosti a pak Bus služby používá vlastnost [PartitionKey][] jako klíč oddílu. Pokud zpráva obsahuje [ID relace][] a [PartitionKey][] sadu vlastností, musí být oba vlastnosti shodné. Pokud je vlastnost [PartitionKey][] nastavena na jinou hodnotu než vlastnost [ID relace][] , služby Bus vrátí výjimku **InvalidOperationException** . Vlastnost [PartitionKey][] bude použito odesílatele pošle vědět transakční zprávy bez relace. Klíč oddílu zaručuje, že jsou všechny zprávy, které jsou odeslané v rámci transakce uskutečněných jednotlivými stejné zpráv zprostředkovatele.

**MessageId**: fronty nebo tématu má vlastnost [QueueDescription.RequiresDuplicateDetection][] nastavena na **hodnotu true** a nejsou nastavte vlastnosti [BrokeredMessage.SessionId][] nebo [BrokeredMessage.PartitionKey][] , pak vlastnost [BrokeredMessage.MessageId][] slouží jako klíč oddílu. (Vezměte v úvahu, že rozhraní Microsoft .NET a AMQP knihoven automaticky přiřadit ID zpráv, pokud odesílající aplikace nemá.) V tomto případě všechny kopie stejná zpráva zpracovávají stejným zprostředkovatele zprávy. Díky Bus služby ke zjištění a odstranění duplicitních zpráv. Pokud vlastnost [QueueDescription.RequiresDuplicateDetection][] není nastavena na **hodnotu true**, Bus služby v úvahu vlastnost [MessageId][] jako klíč oddílu.

### <a name="not-using-a-partition-key"></a>Nepoužíváte klíč oddílu

Při nepřítomnosti klíč oddílu Service Bus distribuuje zprávy způsobem kruhového pro všechny neúplné rozdělený fronty nebo tématu. Pokud ve zvoleném fragmentu není k dispozici, služby Bus zprávu přiřadí různých fragmentu. Tímto způsobem operace Odeslat mu bez ohledu na dočasná nedostupnost úložištěm zasílání zpráv.

Dát služby Bus dostatek času na enqueue zprávu do jiné fragment [MessagingFactorySettings.OperationTimeout][] hodnotu určenou klienta, který je zpráva odeslána musí být větší než 15 sekund. Je vhodné nastavit vlastnost [OperationTimeout][] výchozí hodnotu 60 sekund.

Všimněte si, že klíč oddílu "PinY" zprávu, že mají zvláštní fragmentu. Pokud zasílání zpráv úložiště, který obsahuje tento fragment není k dispozici, služby Bus vrátí chybu. V nepřítomnosti klíč oddílu služby Bus můžete vybrat jiné fragment a operace proběhla úspěšně. Proto doporučujeme, aby nezadáte klíč oddílu není potřeba.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Rozšířené témata: pomocí oddílů entity transakce

Zprávy, které jsou odeslány jako součást transakce zadejte klíč oddílu. To může být jeden z následujících vlastností: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]nebo [BrokeredMessage.MessageId][]. Všechny zprávy, které jsou odeslané jako součást stejné transakce zadejte stejné klíč oddílu. Při pokusu o odeslání zprávy bez klíč oddílu v rámci transakce služby Bus vrátí výjimku **InvalidOperationException** . Pokud budete chtít odeslat více zpráv v rámci stejné transakce, které mají Key jiný oddíl, služby Bus vrátí výjimku **InvalidOperationException** . Příklad:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Pokud některé z vlastností sloužící jako klíč oddílu, služby Bus PinY zprávu na konkrétní část. K tomuto chování dochází, zda se používá transakce. Doporučujeme, aby nezadáte klíč oddílu Pokud není nutné zadávat.

## <a name="using-sessions-with-partitioned-entities"></a>Pomocí oddílů entity relací

Pokud chcete poslat zprávu transakční podporující relace tématu nebo fronty, musíte mít zprávu nastavení vlastnosti [BrokeredMessage.SessionId][] . V případě vlastnost [BrokeredMessage.PartitionKey][] i se musí shodovat s vlastnost [ID relace][] . Pokud se liší služby Bus vrátí výjimku **InvalidOperationException** .

Na rozdíl od běžná (bez oddílů) fronty nebo témata není možné použít jeden transakce odeslat více zpráv do různých relace. Pokud pokus o, vrátí funkce Bus služby výjimku **InvalidOperationException **. Příklad:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Předávání zpráv automatické s oddíly osobami

Azure Bus služby podporuje přesměrování automatického zpráv od, Komu nebo mezi oddíly entity. Povolení automatického předávání zpráv, nastavte vlastnost [QueueDescription.ForwardTo][] na zdrojové fronty nebo předplatného. Pokud se zpráva Určuje klíč oddílu ([ID relace][], [PartitionKey][]nebo [MessageId][]), tento oddíl používá se entity cíl.

## <a name="considerations-and-guidelines"></a>Důležité informace a pokyny

- **Funkce Vysoký konzistence**: Pokud entita používá funkce, například relace, duplicit nebo explicitní řízení rozdělení klíč, pak zpráv operace budou vždycky přesměrované do určité neúplné. Pokud je některý z neúplné vysoký provoz nebo základní úložiště nefunkční, tyto operace selhat a omezenou dostupnosti. Celkově konzistenci je pořád podstatně vyšší než bez oddílů entity; jen podmnožinu přenosy dochází problémech se dočtete víc na rozdíl od všechny přenosy.
- **Správa**: na všechny neúplné entity musíte provádět operace například vytvořit, aktualizovat a odstranit. Pokud nefunkční každou část může vést k chybám pro tyto operace. Pro operaci získat informace, jako je zpráva spočítá musí být agregovaný ze všech neúplné. Při chybná každou část stav dostupnosti entity hlášené jako omezené.
- **Nízká hlasitost zprávy scénáře**: U scénářů, zvlášť když pomocí protokolu HTTP, budete muset provést více operace příjmu za účelem získání všechny zprávy. Příjem žádosti front-end provádí operace přijmout všechny neúplné a ukládá všechny odpovědi dostali. Požadavku na pozdější přijetí na stejné připojení by využít tento ukládání do mezipaměti a přijímání čekacích dob bude nižší. Pokud máte víc připojení nebo jak pomocí protokolu HTTP, která však vytvoří nové připojení pro každý požadavek. Jako takové není nijak zaručené, která by má ve stejném uzlu. Pokud jsou všechny existující zprávy jsou uzamčené a uložené v jiné front-end, operace přijímání vrátí **hodnotu null**. Nakonec vypršení platnosti zprávy a můžete přijímat, je znovu. Udržování HTTP se doporučuje.
- **Procházet/náhled zpráv**: PeekBatch vždy nevrací počet určený ve [Vlastnosti MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)zpráv. Existují dva běžné důvody. Jeden z důvodů je agregované velikost kolekci zpráv překračuje maximální velikosti doručovaných 256KB. Jiný důvod, proč je, že pokud fronty nebo tématu [EnablePartitioning vlastnost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) nastavena na **hodnotu true**, oddíl nemusí dost zprávy dokončete požadovaný počet zpráv. Obecně Pokud chce aplikace zobrazí určitý počet zpráv, ho potřeba zavolat na [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) opakovaně, dokud nenarazí určeným počtem zprávy nebo žádné další zprávy na prohlížení. Další informace včetně ukázek kódu, najdete v článku [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) nebo [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Nejnovější přidanými funkcemi

- Přidání nebo odebrání pravidlo teď podporuje s oddíly entity. Liší od bez oddílů entity, tyto operace nejsou podporovány ve skupinovém rámečku transakce. 
- AMQP teď podporuje pro odesílání a přijímání zpráv do a z oddílů entity.
- AMQP teď podporuje pro tyto operace: [ [Dávku odeslat](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Přijímání dávku](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [přijímání tak, že pořadové číslo](https://msdn.microsoft.com/library/azure/hh330765.aspx), [prohlížet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), obnovení Lock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Plán zprávy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Zrušit naplánované zprávu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Přidat pravidlo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Odebrat pravidlo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Relace obnovení Lock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Nastavit stav relace](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Stav relace Get](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Náhled zpráv relace](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)a [Umožňuje zobrazit výčet relace](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Rozdělený entity omezení

Aktuálně služby Bus ukládá tato omezení na rozdělený fronty a témata:

-   Rozdělený fronty a témata nepodporují odesílání zpráv, které patří do různých relacích v jedné transakci.
-   Služba Bus aktuálně umožňuje až 100 rozdělený fronty nebo témata obor. Jednotlivé oddíly fronty nebo tématu spočítá směrem kvóty 10 000 entit na názvů (netýká osy Premium).

## <a name="next-steps"></a>Další kroky

Přečtěte si diskuzi o [AMQP 1.0 podpora služby Bus oddíly fronty a témata][] Další informace o rozdělení zpráv entity. 

  [Služba Bus architektura]: service-bus-architecture.md
  [Azure portálu]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [ID relace]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Podpora AMQP 1.0 fronty služby Bus oddíly a témata]: service-bus-partitioned-queues-and-topics-amqp-overview.md
