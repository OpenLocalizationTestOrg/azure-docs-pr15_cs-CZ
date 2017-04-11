<properties 
    pageTitle="Aplikace přehledy datového modelu" 
    description="Jsou popsané vlastnosti vyexportovaný z nepřetržitý exportovat ve formátu JSON a slouží jako filtry." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Aplikace přehledy Exportovat datový Model

V této tabulce jsou uvedeny vlastnosti telemetrie odeslané z [Aplikace přehledy](app-insights-overview.md) SDK k portálu. Zobrazí se tyto vlastnosti výstup data [Nekonečný exportovat](app-insights-export-telemetry.md).
Zobrazují se také v dialogovém okně Vlastnosti filtry v [Průzkumníkovi metrické](app-insights-metrics-explorer.md) a [Diagnostiky hledání](app-insights-diagnostic-search.md).

Ukazatel je potřeba pamatovat:

* `[0]`v této tabulce znamená bodu v parametru path, kde máte vložit rejstřík; ale není vždy 0.
* Doba trvání jsou v desetiny microsecond, takže 10000000 == 1 druhé.
* Datum a čas jsou UTC a jsou uvedeny ve formátu ISO`yyyy-MM-DDThh:mm:ss.sssZ`

Existuje několik [ukázek](app-insights-export-telemetry.md#code-samples) , která ukazují, jak je používat.



## <a name="example"></a>Příklad

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontext

Všechny typy telemetrie doprovázeny oddíl kontext. Některé z těchto polí jsou přenášena se všechny datové body.



|Cesta|Typ|Poznámky|
|---|---|---|
| Context.Custom.Dimensions [0]  | objekt]  | Řetězec hodnoty klíče dvojice nastavil parametr vlastní vlastnosti. Maximální délka klíče 100, maximální délka hodnoty 1 024. Víc než 100 jedinečné hodnoty, vlastnost můžete prohledávat, ale není možné použít pro segmentace. Max 200 zkratky za ikey.  |
| Context.Custom.Metrics [0]  | objekt]  | Klíč dvojice nastavit vlastní rozměry parametr tak TrackMetrics. Maximální délka klíče 100, mohou být číselné hodnoty. |
| context.data.eventTime | řetězec | UTC |
| context.data.isSynthetic | Logická hodnota | Žádost o zdánlivě pochází z testu bot nebo web. |
| context.data.samplingRate | číslo | Procento telemetrie generovaných SDK odeslaný k portálu. Oblast 0,0 100.0.|
| Context.Device | objekt | Klientský počítač |
| Context.Device.Browser | řetězec | IE Chrome... |
| context.device.browserVersion | řetězec | Chrome 48,0... |
| context.device.deviceModel | řetězec | |
| context.device.deviceName | řetězec | |
| Context.Device.ID | řetězec | |
| Context.Device.Locale | řetězec | en GB, de-DE... |
| Context.Device.Network | řetězec | |
| context.device.oemName | řetězec | |
| context.device.osVersion | řetězec | Host (hostitel) operační systém |
| context.device.roleInstance | řetězec | ID hostitele serveru |
| context.device.roleName | řetězec | |
| Context.Device.Type | řetězec | Počítač, prohlížeče... |
| Context.Location | objekt | Odvozeno z clientip. |
| Context.location.City | řetězec | Odvozeno z clientip, pokud ho znáte  |
| Context.location.ClientIP | řetězec | Poslední Osmiúhelník je anonymních na hodnotu 0. |
| Context.location.Continent | řetězec | |
| Context.location.Country | řetězec | |
| Context.location.Province | řetězec | Kraj |
| Context.Operation.ID | řetězec | Položky, které mají stejné id operace se zobrazí jako související položky na portálu. Obvykle id žádosti. |
| Context.Operation.Name | řetězec | Adresa URL nebo žádosti o název |
| context.operation.parentId | řetězec | Umožňuje vnořené související položky. |
| Context.Session.ID | řetězec | ID skupiny operace ze stejného zdroje. Období 30 minut bez operaci označuje konec relaci. |
| context.session.isFirst | Logická hodnota | |
| context.user.accountAcquisitionDate | řetězec | |
| context.user.anonAcquisitionDate | řetězec | |
| context.user.anonId | řetězec | |
| context.user.authAcquisitionDate | řetězec | [Ověřeného uživatele](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Logická hodnota | |
| internal.data.documentVersion | řetězec | |
| internal.data.ID | řetězec | |



## <a name="events"></a>Události

Vlastní události generované [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Cesta|Typ|Poznámky|
|---|---|---|
| počet událostí [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad 4 =&gt; 25 %. |
| Název události [0] | řetězec | Název události.  Maximální délka 250. |
| Adresa url události [0] | řetězec | |
| urlData.base události [0] | řetězec | |
| urlData.host události [0] | řetězec | |

## <a name="exceptions"></a>Výjimky

Sestavy [výjimky](app-insights-asp-net-exceptions.md) na serveru a v prohlížeči. 


|Cesta|Typ|Poznámky|
|---|---|---|
| sestavení basicException [0] | řetězec | |
| počet basicException [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad 4 =&gt; 25 %. |
| exceptionGroup basicException [0] | řetězec | |
| exceptionType basicException [0] | řetězec | |řetězec | |
| failedUserCodeMethod basicException [0] | řetězec | |
| failedUserCodeAssembly basicException [0] | řetězec | |
| handledAt basicException [0] | řetězec | |
| hasFullStack basicException [0] | Logická hodnota | |
| id basicException [0] | řetězec | |
| Metoda basicException [0] | řetězec | |
| zpráva basicException [0] | řetězec | Zpráva o výjimce. Maximální délka 10 kB.|
| outerExceptionMessage basicException [0] | řetězec | |
| outerExceptionThrownAtAssembly basicException [0] | řetězec | |
| outerExceptionThrownAtMethod basicException [0] | řetězec | |
| outerExceptionType basicException [0] | řetězec | |
| outerId basicException [0] | řetězec | |
| sestavení parsedStack [0] basicException [0] | řetězec | |
| název_souboru parsedStack [0] basicException [0] | řetězec | |
| úroveň parsedStack [0] basicException [0] | celé číslo | |
| řádek parsedStack [0] basicException [0] | celé číslo | |
| Metoda parsedStack [0] basicException [0] | řetězec | |
| zásobníku basicException [0] | řetězec | Maximální délka 10k|
| typeName basicException [0] | řetězec | |



## <a name="trace-messages"></a>Sledování zpráv

Odesláno: tak, že [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)a [protokolování adaptéry](app-insights-asp-net-trace-logs.md).


|Cesta|Typ|Poznámky|
|---|---|---|
| Název_protokolovače zprávy [0] | řetězec ||
| parametry zprávy [0] | řetězec ||
| zpráva [0] nezpracovanými | řetězec | Zpráva protokolu maximální délka 10 kB. |
| severityLevel zprávy [0] | řetězec | |



## <a name="remote-dependency"></a>Vzdálené závislostí

Odeslaný TrackDependency. Slouží k sestavy výkonu a použití te000126961 [volání závislostí](app-insights-asp-net-dependencies.md) na serveru a AJAX hovory v prohlížeči.

|Cesta|Typ|Poznámky|
|---|---|---|
| asynchronní remoteDependency [0] | Logická hodnota | |
| baseName remoteDependency [0] | řetězec |  |
| commandName remoteDependency [0] | řetězec | Například "Domů/index" |
| počet remoteDependency [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad 4 =&gt; 25 %. |
| dependencyTypeName remoteDependency [0] | řetězec | NASTAVIT INFORMACE HTTP SQL... |
| durationMetric.value remoteDependency [0] | číslo | Čas od hovoru do dokončení odpověď závislostí |
| id remoteDependency [0] | řetězec | |
| Název remoteDependency [0] | řetězec | Adresa URL. Maximální délka 250.|
| resultCode remoteDependency [0] | řetězec | z HTTP závislostí |
| úspěšné remoteDependency [0] | Logická hodnota | |
| Typ remoteDependency [0] | řetězec | Nastavit informace HTTP Sql... |
| Adresa url remoteDependency [0] | řetězec |  Maximální délka 2000 |
| urlData.base remoteDependency [0] | řetězec | Maximální délka 2000 |
| urlData.hashTag remoteDependency [0] | řetězec | |
| urlData.host remoteDependency [0] | řetězec | Maximální délka 200 |


## <a name="requests"></a>Požadavky

Odeslaný [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Standardní moduly slouží k doba odezvy serveru sestav, měří na serveru. 


|Cesta|Typ|Poznámky|
|---|---|---|
| počet požadavku [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad: 4 =&gt; 25 %. |
| žádost o [0] durationMetric.value | číslo | Čas požadavku na příchozí odpověď. 1e7 == hodnotami 1 |
| id požadavku [0] | řetězec | Id operace |
| Název žádost [0] | řetězec | ZÍSKÁNÍ/POST + základní adresu url.  Maximální délka 250 |
| žádost o [0] responseCode | celé číslo | Odpovědi HTTP odeslané do klienta |
| žádost o [0] úspěch | Logická hodnota | Výchozí == (responseCode &lt; 400) |
| Adresa url žádost [0] | řetězec | Bez čísel Host (hostitel) |
| žádost o [0] urlData.base | řetězec | |
| žádost o [0] urlData.hashTag | řetězec |  |
| žádost o [0] urlData.host | řetězec | |


## <a name="page-view-performance"></a>Zobrazení stránky výkonu

Odeslaným pomocí prohlížeče. Opatření čas na zpracování stránky, od uživatele zahájení žádosti a zobrazte kompletní (s vyloučením asynchronní AJAX hovorů).

Kontext hodnoty jsou zobrazeny klienta operační systém verze prohlížeče. 


|Cesta|Typ|Poznámky|
|---|---|---|
| clientProcess.value clientPerformance [0] | celé číslo | Čas od konce přijímat kód HTML pro zobrazení stránky. |
| Název clientPerformance [0] | řetězec | |
| networkConnection.value clientPerformance [0] | celé číslo | Čas potřebný k připojení k síti. |
| receiveRequest.value clientPerformance [0] | celé číslo | Čas od konce posílat žádosti při odpovídání příjem HTML. |
| sendRequest.value clientPerformance [0] | celé číslo | Časové údaje věnovat poslat žádost HTTP. |
| total.value clientPerformance [0] | celé číslo | Čas spuštění pošlete žádost o zobrazení stránky. |
| Adresa url clientPerformance [0] | řetězec | Adresa URL tohoto požadavku |
| urlData.base clientPerformance [0] | řetězec | |
| urlData.hashTag clientPerformance [0] | řetězec | |
| urlData.host clientPerformance [0] | řetězec | |
| urlData.protocol clientPerformance [0] | řetězec | |

## <a name="page-views"></a>Zobrazení stránky

Odeslaný trackPageView() nebo [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Cesta|Typ|Poznámky|
|---|---|---|
| Počet zobrazení [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad 4 =&gt; 25 %. |
| zobrazení [0] durationMetric.value | celé číslo | Hodnota volitelně trackPageView() nebo po startTrackPage() - stopTrackPage(). Nejsou stejné jako clientPerformance hodnoty. |
| Název zobrazení [0] | řetězec | Název stránky.  Maximální délka 250 |
| Adresa url zobrazení [0] | řetězec | |
| zobrazení [0] urlData.base | řetězec | |
| zobrazení [0] urlData.hashTag | řetězec | |
| zobrazení [0] urlData.host | řetězec | |



## <a name="availability"></a>Dostupnost

Sestavy [dostupnost webových testů](app-insights-monitor-web-app-availability.md).

|Cesta|Typ|Poznámky|
|---|---|---|
| dostupnost [0] availabilityMetric.name | řetězec | dostupnost |
| dostupnost [0] availabilityMetric.value | číslo |1.0 nebo 0,0 |
| počet dostupnost [0] | celé číslo | 100 /[(vzorkování)](app-insights-sampling.md) . Příklad 4 =&gt; 25 %. |
| dostupnost [0] dataSizeMetric.name | řetězec | |
| dostupnost [0] dataSizeMetric.value | celé číslo | |
| dostupnost [0] durationMetric.name | řetězec | |
| dostupnost [0] durationMetric.value | číslo | Doba trvání testu. 1e7 == hodnotami 1 |
| dostupnost [0] zprávy | řetězec | Chyba při diagnostiky |
| výsledek dostupnost [0] | řetězec | Přijetí nebo vyloučení |
| dostupnost [0] runLocation | řetězec | GEO zdroj požadováno http |
| dostupnost [0] Název_testu | řetězec | |
| dostupnost [0] testRunId | řetězec | |
| dostupnost [0] testTimestamp | řetězec | |




## <a name="metrics"></a>Metriky

Generovaných TrackMetric().

Hodnotu metriky nachází v context.custom.metrics[0]

Příklad:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Informace o metriky hodnoty

Hodnoty metriky, jak v metrických sestavy a jinde, jsou uvedeny se strukturou standardní objektu. Příklad:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Aktuálně – i když to může změnit adresa v budoucnu - ve všech hodnot nahlášené z standardní moduly SDK `count==1` a pouze `name` a `value` polí jsou užitečné. Pouze případ, kdy by být různé bude, pokud jste napsali TrackMetric hovory v můžete nastavte další parametry. 

Další pole účel umožňuje metriky agregován SDK zmenšit přenosy k portálu. Například může průměrné několik po sobě jdoucí měření před odesláním každé metrických sestavy. Pak by výpočet min, maximum, směrodatné odchylky a celkové hodnoty (součtu nebo průměru) a nastavte počet počet čtení představované sestavy. 

V části tabulky nad jsme zapomněli jen zřídka používaná pole počet, minimum, maximum, stdDev a sampledValue.

Místo předem prostředku metriky můžete použít [odběr](app-insights-sampling.md) v případě potřeby snížení hlasitosti telemetrie.


### <a name="durations"></a>Doba trvání

Pokud není uvedeno jinak, jinak představují doby trvání v desetiny microsecond, tak, aby 10000000.0 znamená 1 druhé.



## <a name="see-also"></a>Viz taky

* [Přehledy aplikace](app-insights-overview.md) 
* [Nepřetržitý exportu](app-insights-export-telemetry.md)
* [Ukázky](app-insights-export-telemetry.md#code-samples)


