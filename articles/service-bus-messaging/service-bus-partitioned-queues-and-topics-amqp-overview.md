<properties 
    pageTitle="AMQP 1.0 podpora služby Bus oddíly fronty a témata | Microsoft Azure" 
    description="Zjistěte, jak používat fronty Upřesnit zpráva řízení fronty Protocol (AMQP) 1.0 s služby Bus oddíly a témata." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Podpora AMQP 1.0 fronty služby Bus oddíly a témata 

Azure Bus služby nyní podporuje Upřesnit zpráva řízení fronty protokol (**AMQP**) 1.0 pro službu Bus **oddíly fronty a témata.**

**AMQP** je otevřené standardní zprávy fronty protokol, který umožňuje vývoj aplikací platformě pomocí programového jazyky. Obecné informace o AMQP podpory pro službu Bus najdete v tématu [AMQP 1.0 podpory pro službu Bus](service-bus-amqp-overview.md).

**Partitioned fronty a témat**, nazývaný také *rozdělený entity*nabízejí vyšší dostupnost, spolehlivost a výkon než normální fronty bez oddílů a témata. Další informace o rozdělený entity najdete v článku [Rozdělený entity zasílání zpráv](service-bus-partitioning.md).

Přidání AMQP 1.0 jako protokol pro komunikaci s oddíly fronty a témata umožňuje vytvářet interoperabilní aplikace, které můžete využít vyšší dostupnost, spolehlivost a v celém nabízené služby Bus oddíly entity.

Podrobné úrovni drátěný AMQP 1.0 protokol příručku, která vysvětluje, jak službu implementuje a vytvoří na technické specifikaci OASIS AMQP, najdete v článku [AMQP 1.0 Bus služby Azure a rozbočovače událost protokolu Průvodce](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Použití AMQP s oddíly fronty

Fronty jsou užitečné pro scénáře, které vyžadují časový oddělení Vyrovnávání zatížení, Vyrovnávání zatížení a volné spojování. K frontě vydavatelé odesílat zprávy do fronty a uživatelé přijímat zprávy ve frontě v situacích, kdy zpráva pouze stavu jednou. Příkladem tohoto je systému zásob, ve kterém pokladním terminály publikování dat do fronty namísto přímo do systému správy zásob. Systému řízení zásob potom spotřebovává data kdykoli ke správě burzovní doplnění. Tato možnost má několik výhod, včetně systému správy zásob nemusíte mít online vždy. Podrobné informace o frontách Bus služby najdete v článku [Vytvoření aplikacemi, které používají fronty Bus služby](service-bus-create-queues.md). 

Další oddíly fronty zvyšuje dostupnost, spolehlivost a výkon u aplikací, jako jsou tyto fronty rozdělený napříč několika zprostředkovatelů zprávy a zpráv ukládají.     

### <a name="create-partitioned-queues"></a>Vytvořit rozdělený fronty

Můžete vytvořit rozdělený fronty pomocí [Azure klasické portál][] a SDK Bus služby. Vytvořit rozdělený fronty, nastavte vlastnost [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) na **hodnotu true** [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) instance. Následující kód ukazuje, jak vytvořit rozdělený fronty pomocí SDK Bus služby. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Odesílání a přijímání zpráv pomocí AMQP

Odeslání zprávy a přijímání zpráv z oddílů fronty pomocí AMQP jako protokol ve vlastnosti [hodnotu TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) uvedeno v následující kód.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Použití AMQP s oddíly témata

Témata podobají entity fronty, ale témata můžete směrovat kopii stejná zpráva víc *předplatných*. V tématu vydavatelé odesílat zprávy na téma a uživatelé přijímat zprávy z předplatného. V pokladním scénář zásob systému terminály publikování dat na téma. Systému řízení zásob obdrží zprávy z předplatného. Kromě toho systém sledování můžete tato zpráva z jiné předplatné. Podrobné informace o služby Bus témata a předplatná najdete v článku [Vytvoření aplikace používající služby Bus témata a předplatná](service-bus-create-topics-subscriptions.md). 

Stejně jako u fronty, zvětšit další oddíly témata dostupnost, spolehlivost a výkon u aplikací, jako tato témata a jejich předplatná rozdělený napříč několika zprostředkovatelů zprávy a zpráv ukládají. 

### <a name="create-partitioned-topics"></a>Vytvořit rozdělený témata

Můžete vytvořit rozdělený téma pomocí [Azure klasické portál][] a SDK Bus služby. Pokud chcete vytvořit rozdělený téma, nastavte vlastnost [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) na **platí** v případě [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Následující kód ukazuje, jak vytvořit rozdělený téma používání SDK Bus služby.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Odesílání a přijímání zpráv pomocí AMQP

Odeslání zprávy a přijímání zpráv ze rozdělený téma předplatného pomocí AMQP jako protokol ve vlastnosti [hodnotu TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), jak je vidět v následujícím kódu.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Další kroky

Viz následující dodatečné informace zobrazíte další informace o rozdělený zpráv entity, jakož i AMQP.

*    [Rozdělený entity zasílání zpráv](service-bus-partitioning.md)
*    [OASIS Upřesnit zpráva řízení fronty Protocol (AMQP) verze 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Podpora AMQP 1.0 v Bus služby](service-bus-amqp-overview.md)
*    [AMQP 1.0 v Bus služby Azure rozbočovače událost protokolu příručka](service-bus-amqp-protocol-guide.md)
*    [Jak Java zprávy služby (JMS) rozhraní API aplikace pomocí služby Bus a AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Jak pomocí rozhraní API služeb Bus .NET AMQP 1.0](service-bus-dotnet-advanced-message-queuing.md)

[Azure klasické portálu]: http://manage.windowsazure.com
