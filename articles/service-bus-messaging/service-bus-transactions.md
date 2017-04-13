<properties 
    pageTitle="Služba Bus transakce | Microsoft Azure" 
    description="Základní informace o Bus služby Azure jednotlivé transakce a pošlete prostřednictvím" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Základní informace o Bus služba transakci zpracování

Tento článek popisuje možnosti transakce Bus služby Azure. Velkou část diskuse je znázorněn [Jednotlivé transakce pomocí služby Bus vzorku](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Tento článek se omezí na přehled zpracování transakcí a funkci *Odeslat* v služby Bus při výběru jednotlivé transakce širší a složitější v rozsahu.

## <a name="transactions-in-service-bus"></a>Transakce v Bus služby

[Transakce](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) skupin dvě nebo více operací společně do *spuštění obor*. Přírodní takové transakce musí zajistit, aby všechny operace patřící do dané skupině operací úspěšné nebo se nezdaří společně. V tomto ohledu transakce fungovat jako jednu jednotku, které se často označují jako *nedělitelnost*. 

Služba Bus je zprostředkovatel transakční zprávy a zajišťuje transakční integritu pro všechny operace interní před jeho ukládá zprávy. Všechny přenosy zpráv uvnitř Bus služby, například přesunutí zprávy do [doručena](service-bus-dead-letter-queues.md) nebo [Automatické předávání](service-bus-auto-forwarding.md) zpráv mezi entity, jsou transakční. Jako například pokud služby Bus přijme zprávu, ho již byly uloženy a označené pořadové číslo. Od klikněte na libovolnou zprávu převody v rámci služby Bus jsou koordinovaný operace přes entity a ani vede ke ztrátě (zdroje se mu to a cílové selže) nebo kopírování (dochází k chybě zdrojové a cílové se mu) zprávy.

Služba Bus podporuje operace seskupení u jedné zpráv entity (fronty, téma, předplatné) v rámci transakce. Například víc zpráv můžete poslat jeden fronty z v rozsahu transakce a zprávy pouze budou potvrzené ve frontě protokolu dokončení transakce úspěšně.

## <a name="operations-within-a-transaction-scope"></a>Operace v rozsahu transakce 

Operace, které lze provádět v rozsahu transakce jsou následujícím způsobem:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: odeslat, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: dokončení CompleteAsync opuštění, AbandonAsync, nedoručených zpráv, DeadletterAsync, pozdržet, DeferAsync, RenewLock, RenewLockAsync 

Příjem operace nejsou zahrnuty, protože se předpokládá, že aplikace získá zprávy použití režimu [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) uvnitř některé přijímání smyčka nebo s [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) zpětné a teprve pak otevře rozsahu transakce pro zpracování zprávy.

V rozsahu a v závislosti na celkové výsledek transakce nedojde k dispozice zprávy (úplné, opuštění doručena, pozdržet).

## <a name="transfers-and-send-via"></a>Přenosy a "Odeslat"

Aby transakční předání dat z fronty procesoru a jiné frontě podporuje služby Bus *přenosy*. V operaci přenos odesílatele nejdřív odešle zprávu "přenos fronty" a přenos fronty okamžitě přesune zprávu do zamýšleného cíle fronty pomocí stejnou implementaci robustní přenos závisející na funkci automatického předat dál. Nikdy velmi dbá na protokolu fronty přenosu tak, že je viditelná pro přenos fronty spotřebitele zprávu.

Power tuto funkci transakční zřejmé, když fronty přenos samotné je zdrojem odesílatele zprávy při zadávání. Jinými slovy, služby Bus můžete přenést zprávu do cílové fronty "pomocí" frontě přenosu při provádění úplný (nebo pozdržet, nebo doručena) operace na zprávu v atomová najednou. 

### <a name="see-it-in-code"></a>Podívejte se, jak kód

Pokud chcete nastavit převodech, vytvoříte odesílatele zprávy, který se zaměřuje cílové fronty prostřednictvím fronty přenos. Můžete bude mít taky zajištěný příjemce, který použije zprávy z této jedné fronty. Příklad:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Jednoduchý transakce potom používá tyto prvky, jako v následujícím příkladu:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Další kroky

Naleznete v následujících článcích Další informace o fronty Bus služby:

- [Zřetězení služby Bus jednotek automatické přeposílání](service-bus-auto-forwarding.md)
- [Ukázka automatického předat dál](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Jednotlivé transakce s ukázkovými Bus služby](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure fronty a služby Bus fronty porovnání](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)