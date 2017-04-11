<properties
    pageTitle="Sledování API se správou Azure rozhraní API, rozbočovače událostí a Runscope"
    description="Ukázková aplikace prokázat zásad protokolu eventhub spojovací Azure rozhraní API správy, Azure události rozbočovače a Runscope HTTP protokolování a sledování"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Sledování API se správou Azure rozhraní API, rozbočovače událostí a Runscope

[Služba Správa rozhraní API](api-management-key-concepts.md) poskytuje mnoho funkcí k vylepšení zpracování HTTP žádostí o odeslané rozhraní API HTTP. Jsou však přechodná existenci požadavky a odpovědi. Zadání požadavku a prochází službu pro správu rozhraní API pro vaše back-end rozhraní API. Vaše rozhraní API zpracuje žádost a odpověď prochází zpět k rozhraní API příjemce. Službu pro správu rozhraní API pořád některé důležité statistických údajů o rozhraní API pro zobrazení v řídicím panelu aplikace Publisher portálu, ale za, aby se podrobnosti pryč.

Pomocí [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zásad](api-management-howto-policies.md) ve službě Správa rozhraní API můžete odeslat cokoli v podrobnostech z žádostí a odpovědí k [Centrální Azure události](../event-hubs/event-hubs-what-is-event-hubs.md). Existuje několik důvodů, proč můžete chtít vytvářet události ze zprávy protokolu HTTP se odešlou vaše rozhraní API. Je několik příkladů revizního aktualizace, analýzy využití, výjimce výstrahy a 3 integrací stran.   

Tento článek ukazuje, jak zachytit celé zprávy žádostí a odpovědí HTTP, odešlete rozbočovači události a potom předávání zpráv příznakem třetí straně služba, která poskytuje HTTP protokolování a monitorování služby.

## <a name="why-send-from-api-management-service"></a>Proč poslat ze služby správy rozhraní API?
Je možné psát middleware HTTP, který můžete připojit k rozhraní API HTTP rámce zaznamenávat HTTP požadavky a odpovědi a kanálu na protokolování a monitorování systémy. Nevýhodou na tento přístup je middlewaru HTTP musí být integrovaná do back-end rozhraní API a musí odpovídat platformu rozhraní API. Pokud existuje více rozhraní API každého z nich zaveďte middleware. Často důvodů Proč nelze aktualizovat back-end rozhraní API.

Pomocí služby správy rozhraní API Azure integrovat s infrastruktura protokolování poskytuje centralizované a nezávislý na platformě řešení. Je také scalable, v části kvůli možnosti [replikace geo](api-management-howto-deploy-multi-region.md) Azure rozhraní API správy.

## <a name="why-send-to-an-azure-event-hub"></a>Proč odeslat rozbočovači události Azure?
Je vhodné požádejte, proč vytvoření zásad, která je specifická pro rozbočovače události Azure? Existuje mnoho různých míst, kde se chcete přihlásit Moje žádosti. Proč není právě odeslat žádosti přímo cíli?  To je možnost. Při vytváření protokolování žádosti o služby správy rozhraní API, je však nutné zvážit zprávy protokolování bude vliv výkon rozhraní API. Postupné zvýšení zatížení máte zvýšením dostupných instancí součástí systému nebo využijete geo replikace. Krátké vrcholy pole v přenosy však mohou způsobit žádosti o výrazně zpozdí Pokud chcete snížit zatížení, začněte požadavky na infrastrukturu protokolování.

Událost rozbočovače Azure slouží k průniku velké objemy dat, s kapacitou pro vyřizování úplně vyšší počet události než počet HTTP požadavků na většině rozhraní API obrázku. Centrální události funguje jako sofistikované vyrovnávací mezi služby správy rozhraní API a infrastrukturu, která bude ukládat a zpracování zpráv. Zajistíte tím, že nebude výkon rozhraní API dojít kvůli infrastruktura protokolování.  

Jakmile se data byl předán rozbočovači událostí je zachován a počká pro událost rozbočovači spotřebitele zpracuje ji. Centrální události nezáleží na tom způsob zpracování, je právě záleží jak zajistit, že bude úspěšně doručené zprávy.     

Událost rozbočovače mají možnost toku události více skupin příjemce. Díky události, které chcete zpracovat úplně jiné systémy. Díky podpůrné mnoho scénáře integrace bez vložení sčítání zpoždění na zpracování požadavku rozhraní API v rámci služby správy rozhraní API pouze jednu událost potřeb generovat.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Zásady odeslání zpráv aplikace/http
Rozbočovači události přijme data události jako jednoduchý řetězec. Obsah tento řetězec je zcela na vás. Aby mohla zabalit žádost HTTP a odešlete ji události rozbočovače potřebujeme formátovací řetězec s informacemi o požadavku nebo odpovědi. V situacích, jako je tady pokud existuje existujícího formátu nám můžete znovu použít a potom nemusí máme psát naší analýza kód. Nejprve se považuje za pomocí [HAR](http://www.softwareishard.com/blog/har-12-spec/) odeslání HTTP žádosti o schůzku a odpovědi. Tento formát je však optimalizovaná pro ukládání ve formátu JSON na základě posloupnost požadavků HTTP. Obsahuje počet povinné elementy, které přidali nepotřebných složitost scénář předávání zpráv HTTP prostřednictvím sítě.  

Alternativní možnost byl používat `application/http` typ média podle popisu v specifikaci HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230). Tento typ média ve formátu přesně stejné používanou skutečně odesílat zprávy HTTP přes lince, ale celé zprávy může být umístění v těle další žádost HTTP. V našem případě jenom budeme používat textu jako naše zprávu odešlete rozbočovače události. Snadno, je analyzátor rozlohu v knihovnách [Microsoft ASP.NET webového rozhraní API 2.2 klienta](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , které lze analyzovat tento formát a převést na nativní `HttpRequestMessage` a `HttpResponseMessage` objekty.

Abyste mohli vytvořit tuto zprávu, kterou potřebujeme využít C# na základě [výrazů zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx) správy rozhraní API Azure. Tady je zásada, která odešle zprávu žádost HTTP rozbočovače Azure události.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Deklarace zásad
Nějaké konkrétní co je potřeba jmění zmínění o tento výraz zásady. Zásady protokolu eventhub obsahuje atribut jen protokolování id, který odkazuje na název protokolování vytvořenou v rámci služby správy rozhraní API. Podrobnosti o tom, jak nastavit protokolování událostí centrální ve službě Správa rozhraní API najdete v dokumentu [protokolování událostí, které mají rozbočovače Azure akci správy rozhraní API Azure](api-management-howto-log-event-hubs.md). Druhá atribut je volitelný parametr, který nastaví rozbočovače události, které oddílů pro ukládání zpráv v. Událost rozbočovače oddíly umožňuje povolit scalabilty a vyžadovat minimálně dva. V oddílu je pouze zaručena uspořádaných doručení zprávy. Pokud není jsme pokyn centrální události ve kterém oddílu umístit zprávu, použije algoritmus kruhového k distribuci načíst. Však mohou způsobit některé náš zprávy zpracovat mimo pořadí.  

### <a name="partitions"></a>Oddíly
Zajistit naše zprávy jsou doručeny do spotřebitele v pořadí a využít výhod možností zatížení rozdělení oddílů se rozhodli, že k odesílání zpráv žádost HTTP jeden oddíl a HTTP odpovědi na zprávy na druhý oddíl. Zajistíte tím i zatížení rozdělení a můžeme zaručit, že všechny žádosti o bude potřeba v pořadí a všechny odpovědi bude potřeba v pořadí. Je možné pro odpověď spotřebované před požadavku, ale jako to není problém jako máme odpovědi na žádosti o různých mechanismus při vzájemného vztahu a nám vědět, že žádosti o vždy předchází odpovědi.

### <a name="http-payloads"></a>Datové části HTTP
Po stavební `requestLine` jsme zkontrolujte, pokud mají zkráceny požadavku. Požadavku je zkrácena na pouze 1024. To mohlo dojít ke zvýšení, ale jsou omezené na 256KB jednotlivých událostí Centrum zpráv, je pravděpodobné, že některé zprávy HTTP orgánů bude není na šířku v jedné zprávě. Při provádění protokolování a technologie pro analýzu značné množství informací můžou pocházet z jenom řádek žádost HTTP a záhlaví. Také mnoho požadavků rozhraní API pouze vrátit malé orgánů a ztráty hodnotou informace zkrácení velké orgánů je vlastně poměrně minimální ve srovnání s snížení přenos, zpracování a úložiště náklady zachovat veškerý obsah textu. Jeden konečný upozornění o zpracování textu je potřeba předat `true` jen<string>() způsob protože jsme čtete obsah, ale byl také má back-end rozhraní API moct přečíst text. Předáním hodnotu true tento způsob doporučujeme způsobit textu paměti tak, aby mohli číst podruhé. To je důležité nějaká Pokud máte rozhraní API, které nemá nahrávání velmi velké soubory nebo používá dlouhé hlasování. V těchto případech je vhodné čtení textu vůbec.   

### <a name="http-headers"></a>Záhlaví HTTP
Nastavit informace HTTP záhlaví můžete jednoduše přenášena do formátu zprávy ve formátu pár jednoduchých klíč/hodnota. Zvolili jsme odstranit určitých citlivé polí zabezpečení, chcete-li předejít zbytečně nevrací přihlašovacími údaji. Není pravděpodobné, že rozhraní API klíče a další údaje použije pro účely analýzy. Pokud nám chcete udělat analýzu na uživatele a určitý produkt používají pak jsme z mohl `context` objektu a přidat do zprávy.     
### <a name="message-metadata"></a>Zpráva metadat
Při vytváření celou zprávu odešlete centru události, první řádek není ve skutečnosti v části `application/http` zprávy. První řádek je tvořena jestli je zpráva žádost nebo odpovědi a id zpráv, které se používá ke koordinaci požadavků na odpovědi na další metadata. Id zprávy je vytvořený s využitím jiného zásad, která vypadá takto:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Společnost Microsoft může vytvořili zprávě s žádostí o, tak, aby byl odpověď vrácena a potom jednoduše odeslaných žádostí a odpovědí jako jednu zprávu, která uložené v proměnné. Však odesílání žádostí a odpovědí nezávisle na sobě a sladit dvou pomocí id zpráv, doporučujeme vstoupit trochu větší flexibilitu velikost zprávy, možnost umožní využít výhod více oddílů, zatímco zachování pořadí zpráv a žádost se zobrazí na řídicím panelu naše protokolování dříve. Je také možné některé scénáře, kdy platnou odpověď nebyla odeslána k rozbočovači události, případně kvůli chybu zásadní žádosti o služby správy rozhraní API, ale pořád se máme záznamu žádosti o.

Zásady odeslání HTTP odpověď podobá velmi žádosti a tak konfigurace dokončení zásady vypadá takto:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

`set-variable` Zásad vytvoří hodnotu, která je přístupný i `log-to-eventhub` zásad `<inbound>` oddílu a `<outbound>` oddílu.  

## <a name="receiving-events-from-event-hubs"></a>Přijímání události z rozbočovače události
Události z Azure události rozbočovači jsou doručeny pomocí [AMQP protokol](http://www.amqp.org/). Tým Bus služby Microsoft provedli klienta knihoven dostupné usnadnit náročný události. Jedním ze dvou různých způsobů podporované, jednu probíhá *Přímé příjemce* a druhý používá `EventProcessorHost` předmětu. Příklady těchto dvou postupů najdete v [Průvodce rozbočovače programování událostí](../event-hubs/event-hubs-programming-guide.md). Je krátká verze rozdíly `Direct Consumer` umožňuje úplnou kontrolu a `EventProcessorHost` znamená některé úkoly vodoinstalaci pro je ale určitých předpokladů o tom, jak bude zpracovávat tyto události.  

### <a name="eventprocessorhost"></a>EventProcessorHost
V tomto příkladu použijeme `EventProcessorHost` pro zjednodušení, ale může není nejlepší volbou pro situaci, zejména. `EventProcessorHost`pevné provádí jak zajistit, že nemusíte bát zřetězení problémů v rámci třídy procesor zvláštní událost. V našem případě jsme se však jednoduše převod zprávu do jiného formátu a předáním podél na jinou službu pomocí metody asynchronní. Není potřeba aktualizovat sdíleného stavu a tedy riziko problémy zřetězení. Pro většinu scénářů `EventProcessorHost` pravděpodobně nejlepší varianta a je určitě jednodušší možnost.     

### <a name="ieventprocessor"></a>IEventProcessor
Centrální koncept při použití `EventProcessorHost` je vytvoření implementaci `IEventProcessor` rozhraní, která obsahuje metodu `ProcessEventAsync`. Podstatu příslušný způsob je zobrazena zde:

  asynchronní {IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) úkolu

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Seznam EventData objektů předávají do metody a jsme iteraci tento seznam. Bajtů obou metod jsou analyzovat na HttpMessage objektu a tento objekt předána instanci IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Instance obsahuje tři dat:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

`HttpMessage` Instance obsahuje `MessageId` GUID, který umožňuje cz se připojit k odpovídající odpověď HTTP a logická hodnota, která určuje, zda objekt obsahuje instanci HttpRequestMessage a HttpResponseMessage žádost HTTP. Podle prostřednictvím protokolu HTTP předmětů z `System.Net.Http`, můžu bylo možno využít `application/http` analýza kód, který je součástí `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Instance provádění předá `IHttpMessageProcessor` tedy rozhraní jsem vytvořil(a) oddělit přijímání a výklad událost z Azure události centrální a skutečné zpracování od něj.


## <a name="forwarding-the-http-message"></a>Předání zprávy HTTP
Tento příklad rozhodli, že by být zajímavé nabízená žádost HTTP přes [Runscope](http://www.runscope.com). Runscope je cloudovou službu specializující HTTP ladění, protokolování a monitorování. Mají bezplatné osy, takže není těžké si zkuste dovoloval, abychom vám najdete v článku požadavky HTTP v v reálném čase prochází naší služby správy rozhraní API.

`IHttpMessageProcessor` Implementaci podobu,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Můžu bylo možno umožní využít výhod [existující knihovny klienta pro Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , která usnadňuje nabízených `HttpRequestMessage` a `HttpResponseMessage` instance až do své služby. Pokud chcete získat přístup k rozhraní API Runscope budete potřebovat účet a rozhraní API klíč. Pokyny k získání rozhraní API klíč najdete v [Vytváření aplikací Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) záznam dění na monitoru.

## <a name="complete-sample"></a>Celá ukázková
[Zdrojový kód](https://github.com/darrelmiller/ApimEventProcessor) a testuje vzorku jsou na Github. Budete potřebovat [Služba Správa rozhraní API](api-management-get-started.md) [Rozbočovači připojeného událostí](api-management-howto-log-event-hubs.md)a [Úložiště účtu](../storage/storage-create-storage-account.md) spuštění výběru pro sebe.   

Vzorek tvoří jenom jednoduchou konzoly aplikaci, která přijímá události pocházejících z centrální události, převede do `HttpRequestMessage` a `HttpResponseMessage` objekty a předá je k rozhraní API Runscope.

V následující animovaný obrázek se zobrazí žádost o odkazy na rozhraní API v portálu pro vývojáře, aplikace konzoly zobrazující zprávu přijala, zpracovávané a přesměrování a potom žádostí a odpovědí neprojevují na inspektor Runscope přenos.

![Ukázka žádost předávané Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Souhrn
Služba Azure rozhraní API Management poskytuje ideální místo k zaznamenání přenosy protokolu HTTP jízdě z vaší rozhraní API a. Azure rozbočovače událostí je velmi scalable nízké náklady řešení pro zachycení která vysílání a podávání ho do vedlejší zpracování systémů pro protokolování, sledování a jiné sofistikované analýzy. Připojení k sledování systémy jako Runscope je jednoduchý jako několik desítek řádky kódu 3 provozu stran.

## <a name="next-steps"></a>Další kroky
-   Další informace o rozbočovače události Azure
    -   [Začínáme s rozbočovače události Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Zprávy s EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Událost rozbočovače programování Průvodce](../event-hubs/event-hubs-programming-guide.md)
-   Další informace o řízení rozhraní API a události rozbočovače integrace
    -   [Protokolování událostí rozbočovače Azure akci správy rozhraní API Azure](api-management-howto-log-event-hubs.md)
    -   [Odkaz na entitu protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [odkaz na protokolu eventhub zásad](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    