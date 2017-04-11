<properties 
    pageTitle="Protokolování událostí rozbočovače Azure akci správy rozhraní API Azure | Microsoft Azure" 
    description="Informace o protokolování událostí do rozbočovače Azure akci správy rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Protokolování událostí rozbočovače Azure akci správy rozhraní API Azure

Azure rozbočovače událostí je služba průniku vysoce scalable data, která můžete jedí miliony události sekundu tak, aby se daly procesu a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Událost rozbočovače funguje jako "dveří přední" pro událost kanálu a po údaje rozbočovači událost, můžete transformovat a uložené kteréhokoliv poskytovatele v reálném čase analýzy nebo adaptéry dávky/úložiště. Událost rozbočovače zruší párování výrobní toku události z spotřebu z těchto skutečností tak, aby příjemci události můžete získat přístup k události vlastní plánu.

Tento článek je companion video [Integrace Azure rozhraní API Management s rozbočovače událostí](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) a popisuje, jak protokolování událostí rozhraní API správy pomocí rozbočovače události Azure.

## <a name="create-an-azure-event-hub"></a>Vytvoření rozbočovači Azure události

K vytvoření nové události centrální, přihlaste se k [Azure klasické portál](https://manage.windowsazure.com) a klikněte na **Nový**->**Aplikace služby**->**Služby Bus**->**Události centrální**->**Vytvořit**. Zadejte název události centrální, oblast, vyberte předplatné a vyberte obor názvů. Pokud jste nevytvořili obor můžete vytvořit jednu zadáním názvu do textového pole **Namespace** . Když jsou nakonfigurovány vlastnosti všechny, klikněte na **vytvořit novou událost rozbočovači** na vytvoření centra události.

![Vytvoření události centrální][create-event-hub]

Další přejděte na kartu **Konfigurovat** pro novou událost rozbočovači a vytvořte dvě **zásady sdílený přístup**. Zadejte název první z nich **odesílání** a udělit oprávnění **Odeslat** .

![Odesílání zásad][sending-policy]

Pojmenujte druhý **příjem**, dodá **Poslech** oprávnění a klikněte na tlačítko **Uložit**.

![Zásady][receiving-policy]

Každého zásadu sdílené přístupu umožňuje aplikací pro odesílání a přijímání událostí do a z centra události. Pokud chcete přistupovat ke spojení řetězců pro tyto zásady, přejděte na kartu **řídicí panel** centra událostí a klikněte na **informace o připojení**.

![Připojovací řetězec][event-hub-dashboard]

**Odesílání** připojovací řetězec se používá při protokolování událostí a připojovací řetězec **příjem** se používají při stahování události z centra události.

![Připojovací řetězec][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Vytvoření rozhraní API správy protokolování

Teď, když máte rozbočovači události, dalším krokem je pro nastavení [protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx) služby správy rozhraní API tak, aby ho protokolování událostí do centra události.

Rozhraní API správy zaznamenávající stisknutí kláves se konfigurují [Rozhraní API správy REST API](http://aka.ms/smapi). Před použitím rozhraní REST API poprvé, zkontrolujte [požadavky](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) a zajistit, že budete mít [povolený přístup k rozhraní REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Pokud chcete vytvořit protokoly, zkontrolujte umístění HTTP žádost o konverzaci pomocí následující adresu URL šablony.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Nahrazení `{your service}` s názvem instanci služby správy rozhraní API.
-   Nahrazení `{new logger name}` s požadovaný název pro nový protokolování. Tento název bude odkazovat při konfiguraci [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zásad

Přidejte následující záhlaví do žádosti.

-   Typ obsahu: aplikace/json
-   Se tak mohli ověřovat: SharedAccessSignature uid =...
    -   Pokyny k vytváření `SharedAccessSignature` najdete v článku [Azure rozhraní API správy REST API ověřování](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Zadejte požadavek textu pomocí následující šablony.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`musí být nastavený na `AzureEventHub`.
-   `description`poskytuje volitelný popis protokolování a může být řetězec nulové délky, pokud budete chtít.
-   `credentials`obsahuje `name` a `connectionString` z rozbočovače Azure události.

Pokud provedete pozvánce na schůzku, je-li protokolování vytvořena stavový kód `201 Created` je vrácena. 

>[AZURE.NOTE] Jiné možných zpáteční kódů a jejich důvodů najdete v článku [Vytvoření protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Chcete-li zobrazit jak provést další operace jako seznam, aktualizovat a odstranit, najdete v dokumentaci entity [protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurace zásad protokolu eventhubs

Po konfiguraci vašeho protokolování správy rozhraní API můžete nakonfigurovat zásady protokolu eventhubs protokolování požadované událostí. Zásady protokolu eventhubs lze použít v části příchozí zásady nebo části odchozí zásady.

Konfigurace zásad, přihlaste se k [Azure klasické portál](https://manage.windowsazure.com)přejděte služby správy rozhraní API a klikněte na **portál publisher** nebo **Spravovat** přístup k portálu aplikace publisher.

![Portál aplikace Publisher][publisher-portal]

V nabídce rozhraní API Management na levé straně klikněte na **zásad** , vyberte požadovaný produkt a rozhraní API a klikněte na **Přidat zásady**. V tomto příkladu jsme přidáváte zásadu k **Rozhraní API ozvěnu** v produktu **Neomezeno** .

![Přidání zásad][add-policy]

Umístěte kurzor `inbound` zásad oddíl a klepněte na **protokolu EventHub** zásadu, pokud chcete vložit `log-to-eventhub` údajů šablonou.

![Editor zásad][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Nahrazení `logger-id` s názvem rozhraní API správy protokolování jste nakonfigurovali v předchozím kroku.

Můžete použít libovolný výraz, který vrátí hodnotu typu string jako hodnotu `log-to-eventhub` prvek. V tomto příkladu je přihlášení k lyncu řetězec obsahující datum a čas, název služby, id požadavku, žádost o ip adresa a název operace.

Klepněte na tlačítko **Uložit** uložte aktualizovaný zásad konfigurace. Jakmile je uložen zásady je aktivní a události přihlášeni k centru určené události.

## <a name="next-steps"></a>Další kroky

-   Další informace o rozbočovače události Azure
    -   [Začínáme s rozbočovače události Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Zprávy s EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Událost rozbočovače programování Průvodce](../event-hubs/event-hubs-programming-guide.md)
-   Další informace o řízení rozhraní API a události rozbočovače integrace
    -   [Odkaz na entitu protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [odkaz na protokolu eventhub zásad](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Sledování API se správou Azure rozhraní API, rozbočovače událostí a Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Podívejte se na video návod

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






