<properties 
    pageTitle="Služby Bus a Python s AMQP 1.0 | Microsoft Azure"
    description="Použití služby Bus z Python s AMQP."
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

# <a name="using-service-bus-from-python-with-amqp-10"></a>Pomocí služby Bus z Python AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Kanálem Python je vazbu jazyk Python kanálem-c; To znamená kanálem Python implementovaná jako obal kolem modul implementovaná v C.

## <a name="download-the-proton-client-library"></a>Stáhnout knihovnu kanálem klienta

Kanálem-C a jeho přidružený vazby (včetně Python) si můžete stáhnout z [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Stahování je v podobě zdrojového kódu. Pokud chcete vytvořit kód, postupujte podle pokynů obsažená v balíček stažený.

Všimněte si, že při psaní tohoto textu SSL podporu kanálem-C je dostupný jenom u operační systémy Linux. Protože Bus služby Azure vyžaduje protokol SSL, kanálem-C (a jazyk vazeb) pouze lze přistupovat k služby Bus z Linux v současné době. Práce povolit kanálem-C SSL ve Windows je probíhá tak zaškrtnutí zpět nedochází k aktualizacím.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Práce s fronty, témata a předplatná z Python Bus služby

Následující kód ukazuje, jak odesílání a přijímání zpráv ze služby Bus zpráv entity.

### <a name="send-messages-using-proton-python"></a>Odeslání zprávy používající kanálem Python

Následující kód ukazuje, jak odeslat zprávu o Bus služeb zasílání zpráv entity.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Přijímání zpráv pomocí kanálem Python

Následující kód znázorňuje dostanete zprávu od služby Bus zpráv entity.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Zasílání zpráv mezi .NET a kanálem Python

### <a name="application-properties"></a>Vlastnosti aplikace

#### <a name="proton-python-to-service-bus-net-apis"></a>Kanálem Python služby Bus rozhraní API .NET

Zprávy kanálem Python podporují vlastnosti aplikace z následujících typů: **int** **dlouhý**, **plovoucí**, **uuid**, **logická hodnota**, **řetězec**. Následující kód Python ukazuje, jak nastavit vlastnosti na zprávu pomocí jednotlivých typů vlastností.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

V rozhraní API služeb Bus .NET vlastnosti aplikace zprávy přenesou v kolekci **Vlastnosti** [BrokeredMessage][]. Následující kód ukazuje, jak číst vlastnosti aplikace zprávy přijaté od Python klienta.

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

V následující tabulce mapuje typy vlastností Python vlastnost typy .NET.

| Typ vlastnosti Python | Typ vlastnosti .NET |
|----------------------|--------------------|
| Funkce INT                  | Funkce INT                |
| uvolnit                | dvojité             |
| dlouhé                 | Int64              |
| UUID                 | identifikátor GUID               |
| Logická hodnota                 | Logická hodnota               |
| řetězec               | řetězec             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Služba Bus rozhraní API .NET kanálem Python

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
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Následující kód Python ukazuje, jak číst vlastnosti aplikace zprávy přijaté od klienta služby Bus .NET.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

V následující tabulce mapování vlastností typy .NET k těmto typům vlastnost Python.

| Typ vlastnosti .NET | Typ vlastnosti Python | Poznámky                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | Funkce INT                  | -                                                                                                                                                                     |
| SByte              | Funkce INT                  | -                                                                                                                                                                     |
| znak               | znak                 | Kanálem Python třídy                                                                                                                                                 |
| krátké              | Funkce INT                  | -                                                                                                                                                                     |
| ushort             | Funkce INT                  | -                                                                                                                                                                     |
| Funkce INT                | Funkce INT                  | -                                                                                                                                                                     |
| uint               | Funkce INT                  | -                                                                                                                                                                     |
| dlouhé               | Funkce INT                  | -                                                                                                                                                                     |
| ulong              | dlouhé                 | Kanálem Python třídy                                                                                                                                                 |
| uvolnit              | uvolnit                | -                                                                                                                                                                     |
| dvojité             | uvolnit                | -                                                                                                                                                                     |
| Decimal            | Řetězec               | Desetinné není aktuálně podporován s kanálem.                                                                                                                     |
| Logická hodnota               | Logická hodnota                 | -                                                                                                                                                                     |
| Identifikátor GUID               | UUID                 | Kanálem Python třídy                                                                                                                                                 |
| řetězec             | řetězec               | -                                                                                                                                                                     |
| Data a času           | časové razítko            | Kanálem Python třídy                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks namapovat na typ AMQP:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Časového rozpětí           | DescribedType        | Timespan.Ticks namapovat na typ AMQP:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| Identifikátor URI                | DescribedType        | Uri.AbsoluteUri namapovat na typ AMQP:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standardní vlastnosti:

V následující tabulce ukazují mapování mezi vlastnosti standardní zprávy kanálem Python a vlastnosti [BrokeredMessage][] standardní zprávy.

#### <a name="proton-python-to-service-bus-net-apis"></a>Kanálem Python služby Bus rozhraní API .NET

| Kanálem Python        | Služba Bus .NET         | Poznámky                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| trvalé              | není k dispozici                      | Služba Bus podporuje pouze trvalé zprávy.               |
| priority (priorita)             | není k dispozici                      | Služba Bus podporuje pouze prioritu podstatu sdělení.      |
| Hodnota TTL                  | Message.TimeToLive       | Převod kanálem Python TTL je definována v milisekundách. |
| první\_nabyvatel      | není k dispozici                      | -                                                           |
| doručení\_počet      | není k dispozici                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| Uživatel\_id             | není k dispozici                      | -                                                           |
| Adresa              | Message.To               | -                                                           |
| předmět              | Message.Label            | -                                                           |
| Odpovědět\_k            | Message.ReplyTo          | -                                                           |
| Analytický nástroj Korelace\_id      | Message.CorrelationID    | -                                                           |
| obsah\_typ        | Message.ContentType      | -                                                           |
| obsah\_kódování    | není k dispozici                      | -                                                           |
| vypršení platnosti\_času         | není k dispozici                      | -                                                           |
| vytvoření\_času       | není k dispozici                      | -                                                           |
| Skupina\_id            | Message.SessionId        | -                                                           |
| Skupina\_sekvence      | není k dispozici                      | -                                                           |
| Odpovědět\_k\_skupiny\_id | Message.ReplyToSessionId | -                                                           |
| Formát               | není k dispozici                      | -                                                           |

| Služba Bus .NET        | Kanálem                       | Poznámky                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| Typ obsahu             | Message.Content\_typ        | -                                                           |
| CorrelationId           | Message.Correlation\_id      | -                                                           |
| EnqueuedTimeUtc         | není k dispozici                          | -                                                           |
| Popisek                   | Message.Subject              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| ReplyTo                 | Message.Reply\_k            | -                                                           |
| ReplyToSessionId        | Message.Reply\_k\_skupiny\_id | -                                                           |
| ScheduledEnqueueTimeUtc | není k dispozici                          | -                                                           |
| ID relace               | Message.Group\_id            | -                                                           |
| TimeToLive              | Message.TTL                  | Převod kanálem Python TTL je definována v milisekundách. |
| K                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Další kroky

Jste připravení na další informace? Získáte následující odkazy:

- [AMQP Bus služby základní informace]
- [AMQP v Bus služby systému Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP v Bus služby systému Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[AMQP Bus služby základní informace]: service-bus-amqp-overview.md
