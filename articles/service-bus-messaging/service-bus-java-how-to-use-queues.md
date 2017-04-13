<properties
    pageTitle="Použití služby Bus fronty s Java | Microsoft Azure"
    description="Naučte se používat službu Bus fronty v Azure. Ukázky napsané v Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak používat službu Bus fronty

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak používat službu Bus fronty. Vzorky psaných Java a použít v [Azure SDK jazyka Java][]. Scénáře nichž se uplatní zahrnují **vytváření fronty** **odesílání a přijímání zpráv**a **odstranění fronty**.

## <a name="what-are-service-bus-queues"></a>Co jsou služby Bus fronty?

Služba Bus fronty podporují **brokered zpráv** model komunikace. Pokud chcete použít fronty, komponent distribuované aplikace není přímo vzájemně komunikovat; Místo toho exchange zprávy prostřednictvím fronty, který slouží jako zprostředkovatel (zprostředkovatel). Zpráva výrobce (odesílatele) ruce zprávu do fronty vypnout a poté pokračuje zpracování.
Asynchronní příjemce zprávy (sluchátko) použije zprávy ve frontě a zpracuje ji. Výrobce nemá potřeba počkat, abyste mohli pokračovat ve zpracování a odeslání další zprávy odpovědi od příjemce. Fronty nabízejí **první – první mimo FIFO** doručení zprávy pro jednoho nebo více konkurenční spotřebitele. Zprávy jsou to znamená, obvykle dostali a zpracována příjemce v pořadí, ve kterém se přidala do fronty a každá zpráva přijetí a zpracování klientem pouze jedné zprávy.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Služba Bus fronty jsou univerzální technologie, která se dá použít pro celou řadu scénáře:

- Komunikace mezi web a pracovní role v vícevrstvého Azure aplikace.
- Komunikace mezi aplikacemi pro místní a Azure hostovaný aplikací v hybridní řešení.
- Komunikace mezi součástmi distribuované aplikace spuštěné místní v různých organizace nebo oddělení organizace.

Použití fronty umožňuje jednodušší rozšiřování aplikace a povolení odolnost proti chybám v architektuře.

## <a name="create-a-service-namespace"></a>Vytvoření obor názvů služby

Chcete-li v práci začít používat služby Bus fronty Azure, je nutné nejprve vytvořit obor. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci.

Vytvoření obor názvů:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Zkontrolujte, jestli že máte nainstalované [Azure SDK jazyka Java][] před vytvořením v tomto příkladu. Pokud používáte zatmění, můžete nainstalovat [Azure sada nástrojů pro Eclipse][] obsahující SDK Azure jazyka Java. **Knihovny Microsoft Azure jazyka Java** poté můžete přidat do projektu:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Přidejte následující `import` příkazů do horní části souboru Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Vytvoření fronty

Operace správy pro službu Bus fronty lze provést pomocí **ServiceBusContract** předmětu. Objekt **ServiceBusContract** je vytvořen s příslušnou konfiguraci, které je zahrnuta token přidružení zabezpečení s oprávnění ke správě ho a třídy **ServiceBusContract** je jediný bod komunikace s Azure.

Třídy **ServiceBusService** nabízí způsobů vytvořit výčet a fronty odstranit. V následujícím příkladu použití objektu **ServiceBusService** k vytvoření fronty s názvem "TestQueue" obor názvů s názvem "HowToSample":

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Způsoby na **QueueInfo** , které umožňují vlastnosti fronty vyladit (například: Chcete-li nastavit výchozí time to live (TTL) hodnotu mohou být použity pro zprávy odeslané do fronty). Následující příklad ukazuje, jak vytvořit fronty s názvem `TestQueue` s maximální velikosti doručovaných 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Všimněte si, že můžete metodu **listQueues** **ServiceBusContract** objektů zaškrtněte, pokud fronty se zadaným názvem už existuje v rámci služby názvů.

## <a name="send-messages-to-a-queue"></a>Odeslání zprávy do fronty

Odeslání zprávy do fronty Bus služby, aplikace získá **ServiceBusContract** objektu. Následující kód ukazuje, jak odeslat zprávu pro `TestQueue` fronty dřív vytvořili v `HowToSample` názvů:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Zpráva pošle a od služby Bus přijata fronty jsou instancemi tříd [BrokeredMessage][] . [BrokeredMessage][] objekty mají sadu standardní vlastnosti (například [štítky](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) slovník, který se používá k ukládání specifické vlastnosti vlastní aplikace a textu data libovolného aplikací. Aplikaci můžete nastavit textu zprávy předáním serializovatelný objektu do konstruktoru [BrokeredMessage][]a odpovídající převodník do sériového tvaru bude použita serializovat objekt. Můžete taky můžete poskytnout **java. VSTUPU A VÝSTUPU. InputStream** objektu.

Následující příklad ukazuje, jak poslat pět zkušební zprávy, které chcete `TestQueue` **MessageSender** jsme získáte v předchozí fragment kódu:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Fronty Bus služby podpory maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB v [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zprávu pozdržet ve frontě je ale zakončení na celkové velikosti zprávy uskutečňuje fronty. Velikost fronty je definována v údaje o času vytvoření, s horní mez 5 GB.

## <a name="receive-messages-from-a-queue"></a>Přijímání zpráv z fronty

Hlavní způsob přijímání zpráv z fronty je použít objekt **ServiceBusContract** . Přijatých zpráv, můžete pracovat v dvěma různými způsoby: **ReceiveAndDelete** a **PeekLock**.

Použití režimu **ReceiveAndDelete** přijímání při operaci jeden snímek – to znamená, když služby Bus obdrží žádost o přečtení zprávy ve frontě, ji označí zprávu jako právě spotřebované množství a vrátí do aplikace. Režim **ReceiveAndDelete** (což je výchozí režim) je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování.
Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** přijímat změní operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte na přijaté zprávy **Odstranit** . Při služby Bus uvidí volání **Odstranění** , bude označte zprávu jako právě spotřebované a odebrat z fronty.

Následující příklad ukazuje, jak zprávy stavu a zpracovat v režimu **PeekLock** (ne výchozí režim). V příkladu níže znamená nekonečné smyčky a zpracování zpráv, které dorazí do náš "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **unlockMessage** zavolat na přijaté zprávy (místo **deleteMessage** metoda). To způsobí služby Bus odemknout zprávy ve frontě a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené ve frontě a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že po zpracování zprávy, ale před zadáním požadavku **deleteMessage** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány aplikaci po restartování. To je někdy označovány jako **Aspoň jednou Processing**, tedy každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. Dosahuje se často, metodou **getMessageId** zprávy, která se nemění přes pokusy o doručení.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, [fronty, témata a předplatné][] Další informace najdete.

Další informace najdete v tématu [Středisko pro vývojáře Java](/develop/java/).


  [Azure SDK jazyka Java]: http://azure.microsoft.com/develop/java/
  [Azure sada nástrojů pro Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

