<properties 
    pageTitle="Služby Bus a PHP s AMQP 1.0 | Microsoft Azure"
    description="Použití služby Bus z PHP s AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Pomocí služby Bus z PHP AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Kanálem PHP je vazbu jazyk PHP kanálem-c; To znamená kanálem PHP implementovaná jako obal kolem modul implementovaná v C.

## <a name="downloading-the-proton-client-library"></a>Stahování knihovny kanálem klienta

Kanálem-C a jeho přidružený vazby (včetně PHP) si můžete stáhnout z [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Stahování je v podobě zdrojového kódu. Pokud chcete vytvořit kód, postupujte podle pokynů obsažené v okně stažených balíček.

> [AZURE.IMPORTANT] Při psaní tohoto textu SSL podporu kanálem-C je dostupný jenom u operační systémy Linux. Protože Bus služby Azure vyžaduje protokol SSL, kanálem-C (a jazyk vazeb) pouze lze přistupovat k služby Bus z Linux v současné době. Práce povolit kanálem-C SSL ve Windows probíhá, proto zaškrtnutí zpět nedochází k aktualizacím.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Práce s fronty, témata a předplatná z PHP Bus služby

Následující kód ukazuje, jak odesílání a přijímání zpráv ze služby Bus zpráv entity.

### <a name="sending-messages-using-proton-php"></a>Odeslání zprávy používající kanálem PHP

Následující kód ukazuje, jak odeslat zprávu o Bus služeb zasílání zpráv entity.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Přijímání zpráv pomocí kanálem PHP

Následující kód znázorňuje dostanete zprávu od služby Bus zpráv entity.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Zasílání zpráv mezi .NET a kanálem PHP

### <a name="application-properties"></a>Vlastnosti aplikace

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP služby Bus rozhraní API .NET

Zprávy kanálem PHP podporují vlastnosti aplikace z následujících typů: **celé číslo**, **dvojité**, **Boolean**, **řetězce**a **objektu**. Následující kód PHP ukazuje, jak nastavit vlastnosti na zprávu pomocí jednotlivých typů vlastností.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

V rozhraní API služeb Bus .NET vlastnosti aplikace zprávy přenesou v kolekci **Vlastnosti** [BrokeredMessage][]. Následující kód ukazuje, jak číst vlastnosti aplikace zprávy přijaté od PHP klienta.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

V následující tabulce mapuje typy vlastností PHP vlastnost typy .NET.

| Typ vlastnosti PHP | Typ vlastnosti .NET |
|-------------------|--------------------|
| celé číslo           | Funkce INT                |
| dvojité            | dvojité             |
| Logická hodnota           | Logická hodnota               |
| řetězec            | řetězec             |
| objekt            | Objekt             |

#### <a name="service-bus-net-apis-to-php"></a>Služba Bus rozhraní API .NET PHP

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

Následující kód PHP ukazuje, jak číst vlastnosti aplikace zprávy přijaté od klienta služby Bus .NET.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

V následující tabulce mapování vlastností typy .NET k těmto typům vlastnost PHP.

| Typ vlastnosti .NET | Typ vlastnosti PHP | Poznámky                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | celé číslo           | -                                                                                                                                                                     |
| SByte              | celé číslo           | -                                                                                                                                                                     |
| znak               | Znak              | Kanálem PHP třídy                                                                                                                                                    |
| krátké              | celé číslo           | -                                                                                                                                                                     |
| ushort             | celé číslo           | -                                                                                                                                                                     |
| Funkce INT                | celé číslo           | -                                                                                                                                                                     |
| uint               | Celé číslo           | -                                                                                                                                                                     |
| dlouhé               | celé číslo           | -                                                                                                                                                                     |
| ulong              | celé číslo           | -                                                                                                                                                                     |
| uvolnit              | dvojité            | -                                                                                                                                                                     |
| dvojité             | dvojité            | -                                                                                                                                                                     |
| Decimal            | řetězec            | Desetinné není aktuálně podporován s kanálem.                                                                                                                     |
| Logická hodnota               | Logická hodnota           | -                                                                                                                                                                     |
| Identifikátor GUID               | UUID              | Kanálem PHP třídy                                                                                                                                                    |
| řetězec             | řetězec            | -                                                                                                                                                                     |
| Data a času           | celé číslo           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks namapovat na typ AMQP:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Časového rozpětí           | DescribedType     | Timespan.Ticks namapovat na typ AMQP:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| Identifikátor URI                | DescribedType     | Uri.AbsoluteUri namapovat na typ AMQP:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standardní vlastnosti:

V následující tabulce ukazují mapování mezi vlastnosti standardní zprávy kanálem PHP a vlastnosti [BrokeredMessage][] standardní zprávy.

| Kanálem PHP           | Služba Bus .NET         | Poznámky                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Trvalé              | není k dispozici                      | Služba Bus podporuje pouze trvalé zprávy.          |
| Priority (priorita)             | není k dispozici                      | Služba Bus podporuje pouze prioritu podstatu sdělení. |
| Hodnota TTL                  | Message.TimeToLive       | Převod kanálem PHP TTL je definována v milisekundách.   |
| první\_nabyvatel      | -                          | -                                                          |
| doručení\_počet      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| Uživatel\_id             | -                          | -                                                          |
| Adresa              | Message.To               | -                                                          |
| Předmět              | Message.Label            | -                                                          |
| Odpovědět\_k            | Message.ReplyTo          | -                                                          |
| Analytický nástroj Korelace\_id      | Message.CorrelationId    | -                                                          |
| obsah\_typ        | Message.ContentType      | -                                                          |
| obsah\_kódování    | není k dispozici                      | -                                                          |
| vypršení platnosti\_času         | Message.ExpiresAtUTC     | -                                                          |
| vytvoření\_času       | není k dispozici                      | -                                                          |
| Skupina\_id            | Message.SessionId        | -                                                          |
| Skupina\_sekvence      | -                          | -                                                          |
| Odpovědět\_k\_skupiny\_id | Message.ReplyToSessionId | -                                                          |
| Formát               | není k dispozici                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Služba Bus rozhraní API .NET kanálem PHP

| Služba Bus .NET        | Kanálem PHP                                             | Poznámky                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| Typ obsahu             | Zpráva -\>obsah\_typ                                | -                                                        |
| CorrelationId           | Zpráva -\>korelace\_id                              | -                                                        |
| EnqueuedTimeUtc         | Zpráva -\>poznámky [x a určovat, jestli-byla zařazena do fronty čas]             | -                                                        |
| Popisek                   | Zpráva -\>předmět                                      | -                                                        |
| MessageId               | Zpráva -\>id                                           | -                                                        |
| ReplyTo                 | Zpráva -\>odpověď\_k                                    | -                                                        |
| ReplyToSessionId        | Zpráva -\>odpověď\_k\_skupiny\_id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Zpráva -\>poznámky ["x-určovat, jestli naplánované enqueue-time"] | -                                                        |
| ID relace               | Zpráva -\>skupiny\_id                                    | -                                                        |
| TimeToLive              | Zpráva -\>ttl                                          | Převod kanálem PHP TTL je definována v milisekundách. |
| K                      | Zpráva -\>adresy                                      | -                                                        |

## <a name="next-steps"></a>Další kroky

Jste připravení na další informace? Získáte následující odkazy:

- [AMQP Bus služby základní informace]
- [AMQP v Bus služby systému Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP v Bus služby systému Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[AMQP Bus služby základní informace]: service-bus-amqp-overview.md
