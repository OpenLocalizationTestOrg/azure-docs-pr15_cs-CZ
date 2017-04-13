<properties 
    pageTitle="Relay Service Bus vzorky přehled | Microsoft Azure"
    description="Rozdělíte do kategorií a popisuje ukázky relay Service Bus s odkazy na jednotlivé."
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
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Ukázky relay Service Bus

Ukázky relay Service Bus demonstrují hlavními funkcemi [relay Service Bus](https://azure.microsoft.com/services/service-bus/). Tento článek rozdělíte do kategorií a popisuje dostupné s odkazy na jednotlivé ukázky.

>[AZURE.NOTE] Ukázky Bus služby nejsou nainstalovány s SDK. Ukázky, získáte na [stránce ukázek Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Kromě toho je aktualizovanou sadu ukázky relay Service Bus [tady](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (k zápisu, že nejsou popisované v tomto článku).  

Zasílání zpráv výběry najdete v článku [služby Bus zpráv vzorky](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Relay Service Bus

Následující příklady ukazují, jak psát aplikace, které využívají služby pro přenos dat Bus služby.

Všimněte si, že ukázky relay vyžadují připojovací řetězec pro přístup k služby Bus názvů.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Získání připojovacího řetězce pro Bus služby Azure

1. Přihlaste se k [portálu Azure](http://portal.azure.com).

1. V levém sloupci klikněte na **Bus služby**.

1. Klikněte na název svého názvů v seznamu.

3. V zásuvné názvů klikněte na **sdílené zásady přístupu**.

4. V zásuvné **zásady přístupu sdílené** klikněte na **RootManageSharedAccessKey**.

6. Zkopírujte připojovací řetězec do schránky.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Získání připojovacího řetězce pro Bus služby systému Windows Server

1. Spusťte následující rutinu Powershellu:

    ```
    get-sbClientConfiguration
    ```

2. Zkopírovat připojovací řetězec do konfiguračního souboru pro vzorku.

## <a name="service-bus-relay"></a>Relay Service Bus

Ukázky, které demonstrují relay Service Bus.

### <a name="getting-started"></a>Začínáme

|Ukázkový název|Popis|Minimální SDK verze|Dostupnost|
|---|---|---|---|
|[Přenos zasílání zpráv: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Ukazuje, jak provádět klienta Bus služby a služby Azure. V tomto příkladu nakonfiguruje služby Bus programově. Pouze prostředí a zabezpečení informace jsou uložené v souborech konfigurace.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv ověření: Tajná sdílené](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Ukazuje, jak použít název vydavatele a Vystavitel tajná ověření s Bus služby.|1.8|Bus služby Microsoft Azure|
|[Přenos zpráv ověření: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Ukazuje, jak vystavit služby protokolu HTTP, který nevyžaduje ověřování uživatelů klienta.|1.8|Bus služby Microsoft Azure|
|[Přenos zpráv vazby: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Ukazuje, jak lze pomocí vazby **WebHttpRelayBinding** vrátit binárními daty pomocí webu modelu programování.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Přenos NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Ukazuje, jak používat **NetTcpRelayBinding** vazby.|1.8|Bus služby Microsoft Azure|

### <a name="exploring-features"></a>Představení funkcí

Ukázky, které demonstrují různé funkce relay Service Bus.

|Ukázkový název|Popis|Minimální SDK verze|Dostupnost|
|---|---|---|---|
|[Přenášené zpráv ověření: Jednoduchý WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Demonstruje použití tokenu pověření jednoduché web o ověření pomocí služby Bus. Vzorek tvoří podobně jako v příkladu ozvěnu s několik změn. V tomto příkladu konkrétně přidá chování v aplikacích ChannelFactory (klient) a ServiceHost (služba).|1.8|Bus služby Microsoft Azure|
|[Přenášené zasílání zpráv: Vyrovnávání zatížení](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Ukazuje, jak používat Microsoft Azure služby Bus ke směrování zpráv více příjemcům. Zobrazuje více instancí jednoduché služby komunikaci s klientem prostřednictvím vazeb **NetTcpRelayBinding**|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Čistý události](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Ukazuje, jak pomocí vazby **NetEventRelayBinding** na Microsoft Azure služby Bus.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Relace WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Ukazuje, jak pomocí **WS2007HttpRelayBinding** vazbu s spolehlivé relace povolené. Také ukazuje, jak můžete určit službu Bus pověření v souboru konfigurace místo programově.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: MsgSecCertificate WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Ukazuje, jak pomocí vazby **WS2007HttpRelayBinding** zabezpečení zpráv zabezpečení začátku do konce zpráv současně pořád vyžaduje ověření s služby Bus klientů.|1.8|Bus služby Microsoft Azure|
|[Přenášené zasílání zpráv: Výměny metadat](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Ukazuje, jak vystavit metadat koncový bod, který používá předávací vazbu. **MetadataExchange** je podporované v následující relay vazby: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**a **WS2007HttpRelayBinding**.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Přímé NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Ukazuje, jak nakonfigurovat **NetTcpRelayBinding** vazbu s **hybridním** režimu připojení, která nejprve vytvoří stavu připojení a pokud je to možné, kombinace kláves vymění automaticky na přímé připojení mezi klientem a služby podpory.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Uživatelské jméno MsgSec NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Ukazuje, jak používat **NetTcpRelayBinding** vazbu s zabezpečení zpráv.|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Čistý Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Ukazuje, jak vystavit a používání koncový bod služby pomocí vazby **NetOnewayRelayBinding** .|1.8|Bus služby Microsoft Azure|
|[Přenášené zpráv vazby: Jednoduchý WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Ukazuje, jak pomocí vazby **WS2007HttpRelayBinding** . Ukazuje jednoduchý služba, která používá žádné možnosti zabezpečení a nevyžaduje klienti ověřit.|1.8|Bus služby Microsoft Azure|

## <a name="next-steps"></a>Další kroky

V následujících tématech pro konceptuální přehledy Bus služby.

- [Přehled relay Service Bus](service-bus-relay-overview.md)
- [Služba Bus architektura](../service-bus-messaging/service-bus-architecture.md)
- [Základy Bus služby](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)