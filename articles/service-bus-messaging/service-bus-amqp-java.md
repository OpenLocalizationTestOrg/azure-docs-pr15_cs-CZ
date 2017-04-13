<properties 
    pageTitle="Služby Bus a Java s AMQP 1.0 | Microsoft Azure"
    description="Pomocí služby Bus z Java AMQP"
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="use-service-bus-from-java-with-amqp-10"></a>Pomocí služby Bus z Java AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Služba zprávu jazyka Java (JMS) je standardní rozhraní API pro práci s middleware orientované zprávy platformy Java. Microsoft Azure Bus služby otestování s AMQP 1.0 na základě knihovny klienta JMS vyvinutý Apache Qpid projektu. Tuto knihovnu podporuje úplné 1.1 API JMS a lze použít pro všechny AMQP 1.0 Služba kompatibilní s zasílání zpráv. Tento scénář je podporované taky v [Bus pro Windows Server služby](https://msdn.microsoft.com/library/dn282144.aspx) (místní služba Bus). Další informace najdete v tématu [AMQP v Bus služby systému Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Stáhnout knihovnu Apache Qpid AMQP 1.0 JMS klienta

Informace o stahování nejnovější verzi knihovnu Apache Qpid JMS AMQP 1.0 klienta Navštěvujte blog o [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Následující čtyři SKLENICE soubory z archivu rozdělení Apache Qpid JMS AMQP 1.0 musíte cesty tříd jazyka Java přidat při vytváření a spouštění aplikací JMS pomocí služby Bus:

-   geronimo jms\_1.1\_.jar specifikace-[verze]

-   qpid-amqp-1-0-Client-[Version].JAR

-   qpid-amqp-1-0-Client-jms-[Version].JAR

-   qpid-amqp-1-0-Common-[Version].JAR

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Práce s fronty, témata a předplatné z JMS Bus služby

### <a name="java-naming-and-directory-interface-jndi"></a>Pojmenování Java a rozhraní služby Directory (JNDI)

JMS slouží k vytváření oddělení logické názvy a pole fyzicky Java pojmenovávání a adresáře rozhraní (JNDI). Dva typy objektů JMS jsou převáděny JNDI: **ConnectionFactory** a **cíl**. JNDI používá poskytovatele modelu, do kterého můžete připojit různých adresářové služby pro zpracování úkolů rozlišení název. Knihovna Apache Qpid JMS AMQP 1.0 součástí jednoduché vlastnosti souborové JNDI poskytovatele nakonfigurovaný pomocí textového souboru.

Zprostředkovatele JNDI Qpid vlastnosti souboru se konfigurují vlastnosti souboru v tomto formátu:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Konfigurace připojení factory

Položka slouží k definování **ConnectionFactory** JNDI zprostředkovatel Qpid vlastnosti souboru je v tomto formátu:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Kde `[jndi\_name]` a `[ConnectionURL]` rozumí:

| Jméno            | Význam                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Logický název factory připojení. Tento název vyřeší v aplikaci Java pomocí JNDI `IntialContext.lookup()` metody. |   |   |   |   |
| `[ConnectionURL]` | Adresa URL knihovnu JMS informace potřebné k AMQP zprostředkovatele.                                                      |   |   |   |   |

Formát připojení adresa URL vypadá takto:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Kde `[namespace]`, `[username]`, a `[password]` rozumí:

| Jméno          | Význam                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Obor názvů služby Bus získané [Azure portálu][].                      |   |   |   |   |
| `[username]`  | Přidružení služby Bus zabezpečení názvu klíče získané [Azure portálu][].                    |   |   |   |   |
| `[password]`  | Zakódovaný URL forma klávesu přidružení služby Bus zabezpečení získané [Azure portálu][]. |   |   |   |   |

> [AZURE.NOTE] Je třeba adresu URL kódovat heslo ručně. Užitečné kódování nástroj adresa URL je k dispozici v [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Například pokud informace získáte v portálu vypadá takto:

| Namespace:   | test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Název vydavatele: | RootManageSharedAccessKey                                        |
| Vydavatel klíč:  | ABCDEFG |

Pak k definování **ConnectionFactory** objekt s názvem `SBCONNECTIONFACTORY`, řetězec konfigurace by vypadal takto:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Konfigurace cíle

Položka, která definuje cíl JNDI zprostředkovatel Qpid vlastnosti souboru je v tomto formátu:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Kde `[jndi\_name]` a `[physical\_name]` rozumí:

| Jméno              | Význam                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Logický název cíle. Toto je název vyřeší v aplikaci Java pomocí JNDI `IntialContext.lookup()` metody. |
| `[physical\name]` | Název entity Bus služeb, do kterého aplikace odeslání nebo přijetí zprávy.                                                  |

Poznámka:

- `[physical\name]` Hodnota může být služba Bus fronty nebo tématu.
- Při příjmu ze služby Bus téma předplatného, název pole fyzicky podle JNDI by měl být na název tématu. Vytvoření trvalé předplatné v kódu aplikace JMS je k dispozici na název předplatného.
- Také je možné považovat za JMS fronty předplatné téma Bus služby. Existuje několik výhod tento přístup: téhož kódu příjemce se dá použít pro fronty a téma předplatné a všechny informace o adrese (názvy předplatného a téma) externalized v vlastnosti souboru.
- Zachází předplatné téma služby Bus jako JMS fronty, zadaná ve vlastnosti souboru by měla být ve formuláři: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Pokud chcete definovat logické cíl JMS s názvem "TÉMA" mapy služby Bus téma s názvem "téma1", hodnota zadaná do souboru vlastností vypadat následovně:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Odeslání zprávy používající JMS

Následující kód ukazuje, jak odeslat zprávu o na téma Bus služby. Předpokládá se, že `SBCONNECTIONFACTORY` a `TOPIC` jsou definované v souboru konfigurace **servicebus.properties** podle popisu v předchozí části.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Přijímání zpráv pomocí JMS

Následující kód ukazuje `how` zpráva ze služby Bus téma předplatného. Předpokládá se, že `SBCONNECTIONFACTORY` a TÉMA jsou definované v souboru konfigurace **servicebus.properties** podle popisu v předchozí části. Je taky předpokládá, že je v poli Název předplatného `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Pokyny pro vytváření robustních aplikací

Specifikace JMS definuje, jak smlouvy výjimce metody rozhraní API a kód aplikace napsat zpracovávání tyto výjimky. Tady jsou některé další body vzít v úvahu ohledně výjimek:

-   Zaregistrujte **ExceptionListener** JMS připojení s protokolem **connection.setExceptionListener**. Díky klienta být odesláno oznámení problému asynchronní. Toto oznámení je velmi důležitým pro připojení, která pouze spotřebovávat zprávy, jak byste měli žádné další seznámíte selže jejich připojení. Pokud se problém s podkladového AMQP připojení, relace nebo odkaz, je místo toho **ExceptionListener** . V takovém případě by měl program aplikace znovu vytvořit **Připojení JMS**, **relace**, **MessageProducer** a objekty **MessageConsumer** od začátku.

-   Ověřte, že zpráva byla úspěšně odeslána z **MessageProducer** entity Bus služby, zajistit, aby byla nakonfigurována aplikace **qpid.sync\_publikovat** sadu vlastností systému. Můžete to udělat spuštěním programu s **-Dqpid.sync\_publikovat = true** samostatný nastavený na příkazovém řádku při spuštění aplikace. Nastavení této možnosti nakonfiguruje knihovny, aby se nevrací z hovor odeslat potvrzení byla přijata která zpráva byla přijata tak, že služba Bus, dokud. V případě problémů v průběhu operace Odeslat **JMSException** aktivovaná. Existují dva možných příčin: 
    1. Pokud k potížím dochází kvůli služby Bus odmítnutí konkrétní odeslání zprávy, bude **MessageRejectedException** výjimce aktivovaná. Tato chyba je přechodně, nebo kvůli problémům s zprávu. Doporučené kurzu akce je, aby několik pokusí opakování operace s některé zpět vypnout použití logických operátorů. Pokud potíže potrvají, by měl zobrazí se chybová zpráva přihlášen místně opuštěné zprávu. Je nutné znovu vytvořit **Připojení JMS**, **relace**a **MessageProducer** objektů v takovém případě. 
    2. Pokud k potížím dochází kvůli služby Bus zavření AMQP odkaz, bude výjimku **InvalidDestinationException** aktivovaná. To může být kvůli dočasné potíže nebo vzhledem k odstranění entity zprávu. V obou případech objekty **JMS připojení**, **relace**a **MessageProducer** znovu vytvořit. Pokud byl přechodná chybový stav, pak operaci postupně budou úspěšně. Pokud entitu byl odstraněn, bude selhání trvalé.

## <a name="messaging-between-net-and-jms"></a>Zasílání zpráv mezi .NET a JMS

### <a name="message-bodies"></a>Zpráv

JMS definuje pět různé typy zpráv: **BytesMessage** **MapMessage**, **ObjectMessage**, **StreamMessage**a **TextMessage**. Rozhraní API služeb Bus .NET s typem podstatu sdělení [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS služby Bus rozhraní API .NET

Následující části ukazují, jak používat zprávy od každého z JMS typů zprávu z .NET. Příklad **ObjectMessage** nebyl však započítávány, tak text **ObjectMessage** obsahuje serializovatelný objekt v programovacího jazyka Java, který není interpretable aplikací .NET.

##### <a name="bytesmessage"></a>BytesMessage

Následující kód ukazuje, jak používat textu objektu **BytesMessage** pomocí rozhraní API služeb Bus .NET.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Následující kód ukazuje, jak používat textu objektu **MapMessage** pomocí rozhraní API služeb Bus .NET. Tento kód prochází prvky mapu zobrazující název-hodnota jednotlivé elementy.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Následující kód ukazuje, jak používat textu objektu **StreamMessage** pomocí rozhraní API služeb Bus .NET. Tento kód obsahuje seznam všech položek z proudu spolu s jejich typy.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Následující kód ukazuje, jak používat textu objektu **TextMessage** pomocí rozhraní API služeb Bus .NET. Tento příklad zobrazuje textový řetězec obsažený v textu zprávy.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Služba Bus rozhraní API .NET JMS

Následující části ukazují, jak aplikace .NET můžete vytvořit zprávu, která je obdrželi JMS ve všech různé typy zpráv JMS. Příklad **ObjectMessage** nebyl však započítávány, tak text **ObjectMessage** obsahuje serializovatelný objekt v programovacího jazyka Java, který není interpretable aplikací .NET.

##### <a name="bytesmessage"></a>BytesMessage

Následující kód ukazuje, jak vytvořit objekt [BrokeredMessage][] ve .NET, které se vztahují JMS klienta jako **BytesMessage**.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Následující kód ukazuje, jak vytvořit objekt [BrokeredMessage][] ve .NET, které se vztahují JMS klienta jako **StreamMessage**.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Následující kód ukazuje, jak používat textu **TextMessage** pomocí rozhraní API služeb Bus .NET. Tento příklad zobrazuje textový řetězec obsažený v textu zprávy.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Vlastnosti aplikace

####<a name="jms-to-service-bus-net-apis"></a>JMS služby Bus rozhraní API .NET

Zprávy JMS podporují vlastnosti aplikace z následujících typů: **logický** **bajt**, **Krátký**, **int**, **dlouhý**, **plovoucí**, **dvojité**a **řetězec**. Následující kód jazyka Java ukazuje, jak nastavit vlastnosti na zprávu pomocí jednotlivých typů vlastností.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

V rozhraní API služeb Bus .NET vlastnosti aplikace zprávy přenesou v kolekci **Vlastnosti** [BrokeredMessage][]. Následující kód ukazuje, jak číst vlastnosti aplikace zprávy přijaté od JMS klienta.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

V následující tabulce jsou uvedeny mapování vlastností typů JMS k těmto typům vlastnost .NET.

| Typ vlastnosti JMS | Typ vlastnosti .NET |
|-------------------|--------------------|
| Bajt              | SByte              |
| Celé číslo           | Funkce INT                |
| Plovoucí             | uvolnit              |
| Dvojité            | dvojité             |
| Logická hodnota           | Logická hodnota               |
| Řetězec            | řetězec             |

Typ [BrokeredMessage][] podporuje vlastnosti aplikace z následujících typů: **bajt**, **sbyte**, **znak**, **krátké**, **ushort**, **int**, **uint**, **dlouhý**, **ulong**, **plovoucí**, **dvojité**, **desetinné**, **logická hodnota**, **Guid**, **řetězec**, **Uri**, **data a času**, **DateTimeOffset**a **časový interval**. Následující kód .NET ukazuje, jak nastavit vlastnosti objektu [BrokeredMessage][] s některou z těchto typů vlastností.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Následující kód jazyka Java ukazuje, jak číst vlastnosti aplikace zprávy přijaté od klienta služby Bus .NET.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Následující tabulka zobrazuje mapování vlastností typů .NET k těmto typům vlastnost JMS.

| Typ vlastnosti .NET | Typ vlastnosti JMS | Poznámky                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | UnsignedByte      | -                                                                                                                                                                      |
| SByte              | Bajt              | -                                                                                                                                                                     |
| znak               | Znak         | -                                                                                                                                                                     |
| krátké              | Krátké             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| Funkce INT                | Celé číslo           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| dlouhé               | Dlouhé              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| uvolnit              | Plovoucí             | -                                                                                                                                                                     |
| dvojité             | Dvojité            | -                                                                                                                                                                     |
| Decimal            | BigDecimal        | -                                                                                                                                                                     |
| Logická hodnota               | Logická hodnota           | -                                                                                                                                                                     |
| Identifikátor GUID               | UUID              | -                                                                                                                                                                     |
| řetězec             | Řetězec            | -                                                                                                                                                                     |
| Data a času           | Datum              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks namapovat na typ AMQP:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Časového rozpětí           | DescribedType     | Timespan.Ticks namapovat na typ AMQP:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| Identifikátor URI                | DescribedType     | Uri.AbsoluteUri namapovat na typ AMQP:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Standardní záhlaví

V následujících tabulkách zobrazit, jak standardní záhlaví JMS a standardní vlastnosti [BrokeredMessage][] jsou namapované pomocí AMQP 1.0.

#### <a name="jms-to-service-bus-net-apis"></a>JMS služby Bus rozhraní API .NET

| JMS              | Služba Bus .NET               | Poznámky                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Aktuálně dostupné        | Služba Bus podporuje pouze trvalé zprávy. například DeliveryMode.PERSISTENT, bez ohledu na to, co je určen.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Zpráva. TimeToLive            | Převod                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Ve výchozím nastavení je JMSMessageID kódovaný v binárním formátu ve zprávě AMQP. Po přijetí binární id zpráv knihovnu klienta .NET převede na číselné řetězce na základě hodnot unicode bajtů. Pokud chcete přepnout knihovnu JMS řetězec zprávy ID, připojte "binární messageid = false" řetězci s parametry dotazu JNDI ConnectionURL. Příklad: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binární messageid = false ". |
| JMSPriority      | Aktuálně dostupné        | Služba Bus nepodporuje priority zprávy.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Aktuálně dostupné        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Zpráva. ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Převod                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms typu"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Služba Bus rozhraní API .NET JMS

| Služba Bus .NET        | JMS              | Poznámky                   |
|-------------------------|------------------|-------------------------|
| Typ obsahu             | -                  | Aktuálně dostupné |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Převod              |
| Popisek                   | není k dispozici              | Aktuálně dostupné |
| MessageId               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | není k dispozici              | Aktuálně dostupné |
| ScheduledEnqueueTimeUtc | není k dispozici              | Aktuálně dostupné |
| ID relace               | není k dispozici              | Aktuálně dostupné |
| TimeToLive              | JMSExpiration    | Převod              |
| K                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Nepodporované funkce a omezení

Při používání JMS přes AMQP 1.0 s služby Bus existují následující omezení:

-   Pouze s jedním **MessageProducer** nebo **MessageConsumer** je povolených pro jednoho relace. Pokud chcete vytvořit více **MessageProducer** nebo **MessageConsumer** objektů v aplikaci, vytvořte vyhrazené relací pro každý z nich.

-   Stále přepočítávané téma předplatné aktuálně nepodporuje.

-   Objekty **MessageSelector** nejsou podporované.

-   Dočasné určení; například **TemporaryQueue** nebo **TemporaryTopic**nepodporuje doplňky, spolu s **QueueRequestor** a **TopicRequestor** rozhraní API, která je použít.

-   Nejsou podporovány transakční relace.

-   Distribuované transakce nejsou podporované.

## <a name="next-steps"></a>Další kroky

Jste připravení na další informace? Získáte následující odkazy:

- [AMQP Bus služby základní informace]
- [AMQP v Bus služby systému Windows Server]

[AMQP v Bus služby systému Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[AMQP Bus služby základní informace]: service-bus-amqp-overview.md
[Azure portálu]: https://portal.azure.com
