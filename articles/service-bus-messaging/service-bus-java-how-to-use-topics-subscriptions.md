<properties
    pageTitle="Použití služby Bus témata s Java | Microsoft Azure"
    description="Naučte se používat službu Bus témata a předplatná v Azure. Ukázky jsou určena aplikace Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Používání služby Bus témata a předplatného

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tato příručka popisuje, jak používat službu Bus témata a předplatná. Vzorky psaných Java a použít v [Azure SDK jazyka Java][]. Scénáře nichž se uplatní patří **vytváření témata a předplatná**, **vytváření filtrů předplatného**, **odesílání zpráv na téma**, **přijímat zprávy z předplatného**a **Odstranění témata a předplatná**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co je služba Bus témata a předplatného?

Služba Bus témata a předplatná domovské stránce podpory *publikování a přihlášení k odběru* zpráv modelu komunikace. Pokud chcete použít, témata a předplatná, komponent distribuované aplikace není přímo vzájemně komunikovat; Místo toho exchange zprávy přes tematickým, který slouží jako zprostředkovatele.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Na rozdíl od služby Bus fronty, ve kterých je každá zpráva zpracována jednoho příjemce, témata a předplatná poskytují "-n" formu komunikace pomocí vzorek publikovat nebo přihlášení k odběru. Je možné k registraci víc předplatných na téma. Po odeslání zprávy na téma ho je pak zpřístupnili pro každého předplatného úchyt/procesu nezávisle na sobě.

Předplatné na téma podobá virtuální fronta, která přijímá kopie zpráv, které byly odeslány tématu. Volitelně můžete zaregistrovat pravidla filtru tématu na základě za předplatné umožňuje filtru nebo omezit zprávy, které mají na téma obdrží které téma předplatná.

Služba Bus témata a předplatná umožňují měřítko zpracuje velmi velké množství zpráv přes velkého počtu uživatelů a aplikací.

## <a name="create-a-service-namespace"></a>Vytvoření obor názvů služby

Chcete-li začít používat služby Bus témata a předplatná v Azure, je nutné nejprve vytvořit obor názvů služby. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci.

Vytvoření obor názvů:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Zkontrolujte, jestli že máte nainstalované [Azure SDK jazyka Java][] před vytvořením v tomto příkladu. Pokud používáte zatmění, můžete nainstalovat [Azure sada nástrojů pro Eclipse][] obsahující SDK Azure jazyka Java. **Knihovny Microsoft Azure jazyka Java** poté můžete přidat do projektu:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Přidejte následující příkazy importovat do horní části souboru Java:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Přidání knihoven Azure jazyka Java cestu Tvůrce dotazů a zahrnout do sestavení nasazení projektu.

## <a name="create-a-topic"></a>Vytvoření tematického

Operace správy služby Bus témat lze provést pomocí třídy **ServiceBusContract** . Objekt **ServiceBusContract** je vytvořen s příslušnou konfiguraci, které je zahrnuta token přidružení zabezpečení s oprávnění ke správě ho a třídy **ServiceBusContract** je jediný bod komunikace s Azure.

Třída **ServiceBusService** poskytuje metody vytvořit, zobrazit výčet a odstraňovat témata. Následující příklad ukazuje, jak objekt **ServiceBusService** mohou sloužit k vytvoření tematického s názvem `TestTopic`, s obor názvů s názvem `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Způsoby na **TopicInfo** podporující nástroj vlastnosti v tématu nastavení (například: Chcete-li nastavit výchozí time to live (TTL) hodnotu použije na zprávy poslané na téma). Následující příklad ukazuje, jak vytvořit téma s názvem `TestTopic` s maximální velikosti doručovaných 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Všimněte si, že můžete metodu **listTopics** **ServiceBusContract** objektů zaškrtněte, pokud téma se zadaným názvem už existuje v rámci služby názvů.

## <a name="create-subscriptions"></a>Vytvoření předplatného

Předplatná pro témata vzniká za **ServiceBusService** předmětu. Předplatné se s názvem a může obsahovat volitelné filtr, který omezuje sadu zpráv do fronty virtuální předplatného.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

Filtr **MatchAll** je výchozí filtr, který se používá, pokud zadaný žádný filtr při vytvoření nové předplatné. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "AllMessages" a používá jako výchozí **MatchAll** filtr.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky vytvořit filtry, které vám umožní obor, které zprávy poslané na téma má zobrazit v konkrétním tématu předplatné.

Flexibilní typ filtru nepodporuje pro předplatná je [SqlFilter][], které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Podrobné informace o výrazech, které lze použít s filtrem SQL prohlédněte si [SqlFilter.SqlExpression][] syntaxe.

Následující příklad vytvoří předplatné s názvem `HighMessages` s [SqlFilter][] objekt, který pouze vybere zprávy, které mají vlastních vlastností **MessageNumber** větší než 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Podobně následující příklad vytvoří předplatné s názvem `LowMessages` s [SqlFilter][] objekt, který pouze vybere zprávy, které mají **MessageNumber** vlastnost menším větší nebo rovnu 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Pokud je teď odeslána zpráva `TestTopic`, bude vždy zašle příjemce, nemáte předplatné `AllMessages` předplatného a selektivním doručeno příjemce, nemáte předplatné `HighMessages` a `LowMessages` předplatná (v závislosti na obsah zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy na téma

Zaslání zprávy na téma Bus služby aplikace získá **ServiceBusContract** objektu. Následující kód ukazuje, jak odeslat zprávu pro `TestTopic` téma nevytvořili v rámci `HowToSample` názvů:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Zprávy zasílané do služby Bus témata jsou instancemi tříd [BrokeredMessage][] . [BrokeredMessage][] *objekty mají sadu standardní metod (například * *setLabel* * a * *TimeToLive**), slovník, který se používá k ukládání vlastní vlastnosti specifické pro aplikaci a textu data libovolného aplikací. Aplikaci můžete nastavit textu zprávy předáním serializovatelný objektu do konstruktoru [BrokeredMessage][]a příslušné * *DataContractSerializer* * bude použita serializovat objekt. Můžete taky * *java.io.InputStream** lze zadat.

Následující příklad ukazuje, jak poslat pět zkušební zprávy, které chcete `TestTopic` **MessageSender** jsme získaný v fragment kódu.
Všimněte si, jak se mění hodnotu **MessageNumber** jednotlivých zpráv na opakování smyčky (to bude určovat, které předplatné obdrží):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Toto téma velikost je definována na údaje o času vytvoření, s horní mez 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak přijímat zprávy z předplatného

Aby se zprávy z předplatného, použijte **ServiceBusContract** objekt. Přijatých zpráv, můžete pracovat v dvěma různými způsoby: **ReceiveAndDelete** a **PeekLock**.

Použití režimu **ReceiveAndDelete** přijímání při operaci jeden snímek – to znamená, když služby Bus obdrží žádost o přečtení zprávy, ji označí zprávu jako právě spotřebované množství a vrátí do aplikace. Režim **ReceiveAndDelete** je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** přijímat změní operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte na přijaté zprávy **Odstranit** . Při služby Bus uvidí volání **Odstranění** , bude označte zprávu jako právě spotřebované a odebrat z tématu.

Následující příklad ukazuje, jak zprávy stavu a zpracovat v režimu **PeekLock** (ne výchozí režim). V příkladu níže provádí smyčky a zpracování zpráv v "HighMessages" předplatného a pak ukončí po žádné další zprávy (můžete taky může být nastavena čekat pro nové zprávy).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **unlockMessage** zavolat na přijaté zprávy (místo **deleteMessage** metoda). To způsobí služby Bus odemknout zprávy v tématu a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené v tématu a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že po zpracování zprávy, ale před zadáním požadavku **deleteMessage** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány aplikaci po restartování. To je někdy označovány jako **Aspoň jednou Processing**, tedy každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. Dosahuje se často, metodou **getMessageId** zprávy, která se nemění přes pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Primární způsob, jak odstranit témata a předplatná, je použít objekt **ServiceBusContract** . Odstranění tématu budou odstraněny také žádné předplatné, které jsou registrované s tématem. Předplatné můžete odstraní taky nezávisle na sobě.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, [fronty Bus služby, témata a předplatná][] Další informace najdete.

  [Azure SDK jazyka Java]: http://azure.microsoft.com/develop/java/
  [Azure sada nástrojů pro Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Služba Bus fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
