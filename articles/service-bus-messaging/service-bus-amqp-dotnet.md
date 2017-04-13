<properties 
    pageTitle="Služby Bus s .NET a AMQP 1.0 | Microsoft Azure"
    description="Pomocí služby Bus z .NET AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Pomocí služby Bus z .NET AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Stažení služby Bus SDK

Podpora AMQP 1.0 je k dispozici v SDK Bus služby verze 2.1 nebo vyšší. Zajistíte, že máte nejnovější verzi stažením bitů služby Bus z [NuGet][].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurace aplikací .NET pro použití AMQP 1.0

Ve výchozím nastavení knihovny klienta služby Bus .NET informuje uživatele o se službou Bus služby prostřednictvím vyhrazené protokol SOAP. Použití AMQP 1.0 namísto výchozí protokol vyžaduje explicitní konfigurace Bus služby základní připojovací řetězec, popsané v následující části. Než tato změna kódu aplikace se nezmění při použití AMQP 1.0.

V současné verzi existuje několik funkcí rozhraní API, které nejsou podporovány při použití AMQP. Tyto nepodporované funkce jsou uvedené dále v části [nepodporované funkce, omezení a chování rozdíly](#unsupported-features-restrictions-and-behavioral-differences). Některá upřesňující nastavení taky s významem různých při použití AMQP.

### <a name="configuration-using-appconfig"></a>Konfigurace pomocí App.config

Je vhodné pro použití konfiguračního souboru konfigurace uložit nastavení aplikacemi. V aplikacích služby Bus můžete App.config uložit nastavení služby Bus **ConnectionString** hodnoty. Konfigurační soubor příklad vypadá takto:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Hodnota `Microsoft.ServiceBus.ConnectionString` nastavení je služba Bus připojovací řetězec, který se používá ke konfiguraci připojení služby Bus. Formát vypadá takto:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Kde `[namespace]` a `SharedAccessKey` jsou získány z [Azure portálu][]. Další informace najdete v tématu [Začínáme s fronty Bus služby][].

Pokud chcete použít AMQP, přidat připojovací řetězec s `;TransportType=Amqp`. Tento typ notation informuje klienta knihovny tak, aby služba Bus pomocí AMQP 1.0 připojení.

## <a name="message-serialization"></a>Zpráva serializace

Pokud chcete použít výchozí protokol, výchozí chování serializace knihovnu .NET klienta, je použít typ [DataContractSerializer][] serializovat instanci [BrokeredMessage][] doprava mezi knihovnu klienta a služba Bus služby. Při použití režimu přenosu AMQP knihovnu klienta se systémem AMQP typ pro serializaci [brokered zprávy][BrokeredMessage] do zprávy AMQP. Tato serializace umožňuje zprávu přijala a interpretovaný přijímání aplikací, na kterém běží potenciálně na jiné platformě, například aplikaci Java, která používá JMS rozhraní API pro přístup k Bus služby.

Při vytváření instanci [BrokeredMessage][] zajistíte objektu .NET jako parametr konstruktoru sloužit jako textu zprávy. U objektů, které lze namapovat na základní typy AMQP textu seřazeny do AMQP datové typy. Pokud objekt nelze mapovat přímo do AMQP základní typ; To znamená vlastní typ definovány aplikací, klepněte na objekt serializován pomocí [DataContractSerializer][]a sériové bajtů jsou odeslány ve zprávě AMQP dat

Usnadnit spolupráce s klienty není .NET použijte pouze typy .NET, které můžete seřazeny přímo do AMQP typů textu zprávy. Následující tabulka uvádí jenom určité typy a odpovídající mapování AMQP typ systému.

| Typ objektu .NET textu          | Typ mapovaných AMQP                          | Typ oddílu AMQP textu                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Logická hodnota                           | Logická hodnota                                   | Hodnota AMQP                                                                                                                                                |
| bajt                           | ubyte                                     | Hodnota AMQP                                                                                                                                                |
| ushort                         | ushort                                    | Hodnota AMQP                                                                                                                                                |
| uint                           | uint                                      | Hodnota AMQP                                                                                                                                                |
| ulong                          | ulong                                     | Hodnota AMQP                                                                                                                                                |
| SByte                          | bajt                                      | Hodnota AMQP                                                                                                                                                |
| krátké                          | krátké                                     | Hodnota AMQP                                                                                                                                                |
| Funkce INT                            | Funkce INT                                       | Hodnota AMQP                                                                                                                                                |
| dlouhé                           | dlouhé                                      | Hodnota AMQP                                                                                                                                                |
| uvolnit                          | uvolnit                                     | Hodnota AMQP                                                                                                                                                |
| dvojité                         | dvojité                                    | Hodnota AMQP                                                                                                                                                |
| Decimal                        | decimal128                                | Hodnota AMQP                                                                                                                                                |
| znak                           | znak                                      | Hodnota AMQP                                                                                                                                                |
| Data a času                       | časové razítko                                 | Hodnota AMQP                                                                                                                                                |
| Identifikátor GUID                           | UUID                                      | Hodnota AMQP                                                                                                                                                |
| Byte]                         | binární.                                    | Hodnota AMQP                                                                                                                                                |
| řetězec                         | řetězec                                    | Hodnota AMQP                                                                                                                                                |
| System.Collections.IList       | seznam                                      | Hodnota AMQP: položky obsažené v kolekci lze pouze ty, které jsou definované v této tabulce.                                                             |
| System.Array                   | pole                                     | Hodnota AMQP: položky obsažené v kolekci lze pouze ty, které jsou definované v této tabulce.                                                             |
| System.Collections.IDictionary | mapy                                       | Hodnota AMQP: položky obsažené v kolekci lze pouze ty, které jsou definované v této tabulce. Poznámka: podporuje pouze řetězce klíčů.                        |
| Identifikátor URI                            | Popsané řetězec (viz následující tabulka) | Hodnota AMQP                                                                                                                                                |
| DateTimeOffset                 | Popsané dlouho (viz následující tabulka)   | Hodnota AMQP                                                                                                                                                |
| Časového rozpětí                       | Popsané dlouho (viz následující)         | Hodnota AMQP                                                                                                                                                |
| Toku                         | binární.                                    | Data AMQP (může být více). Oddíly Data obsahují nezpracovanými bajtů číst z objektu proudu.                                                           |
| Jiného objektu                   | binární.                                    | Data AMQP (může být více). Obsahuje sériové binární objekt, který používá DataContractSerializer nebo serializátor poskytnutou aplikací. |

| Typ .NET      | Mapovaný AMQP popsané typ                                                                                              | Poznámky                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| Identifikátor URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Časového rozpětí       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nepodporované funkce, omezení a chování rozdíly

Při použití AMQP nejsou aktuálně podporované tyto funkce rozhraní API .NET Bus služby:

-   Transakce

-   Odeslat přenos cíl

Jsou taky některé malé rozdíly v části přizpůsobení rozhraní API služeb Bus .NET při použití AMQP ve srovnání s výchozí protokol:

-   Vlastnost [OperationTimeout][] ignorována.

-   `MessageReceiver.Receive(TimeSpan.Zero)`implementace jako `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Dokončení zpráv podle tokeny uzamknout pouze se teď dá tak, že příjemce zprávy, které původně přijaté zprávy.

## <a name="controlling-amqp-protocol-settings"></a>Určení nastavení protokolu AMQP

Rozhraní API .NET vystavit několik nastavení pro řízení chování AMQP Protocol (protokol):

-   **MessageReceiver.PrefetchCount**: ovládací prvky počáteční Dal použít ke spojení. Výchozí hodnota je 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: ovládací prvky maximální velikost rámce AMQP nabízená při vyjednávání při připojení otevřete čas. Výchozí hodnota je 65 536 bajtů.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Pokud přenosy batchable, tato hodnota Určuje zpoždění při odesílání potížemi. Ve výchozím nastavení dědí odesílatele nebo příjemce. Jednotlivé odesílatele nebo příjemce můžete přepsat výchozí nastavení, což je 20 milisekund.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: Určuje, zda AMQP připojení se vytvářejí přes připojení SSL. Výchozí hodnota je **PRAVDA**.

## <a name="next-steps"></a>Další kroky

Jste připravení na další informace? Získáte následující odkazy:

- [AMQP Bus služby základní informace]
- [Podpora AMQP 1.0 fronty služby Bus oddíly a témata]
- [AMQP v Bus služby systému Windows Server]

  [Začínáme s služby Bus fronty]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portálu]: https://portal.azure.com
[AMQP Bus služby základní informace]: service-bus-amqp-overview.md
[Podpora AMQP 1.0 fronty služby Bus oddíly a témata]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP v Bus služby systému Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
