<properties
 pageTitle="Vzdálené sledování předem řešení návodu | Microsoft Azure"
 description="Popis Azure IoT předem vzdálené sledování řešení a jeho architektura."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Vzdálené sledování návodu předkonfigurované řešení

## <a name="introduction"></a>Úvod

Vzdálené IoT sadu sledování [automaticky předem nakonfigurovaná řešení] [ lnk-preconfigured-solutions] je implementace začátku do konce monitorovací řešení pro více počítače se systémem ve vzdáleném umístění. Řešení kombinuje klíčové Azure služby a zadejte obecný implementaci business scénář a můžete ho jako výchozí bod pro vlastní implementaci. Můžete [Přizpůsobit] [ lnk-customize] řešení vlastní specifickým obchodním požadavkům.

Tento článek vás provede některými klíčové prvky vzdálené sledování řešení, které umožňují porozumět tomu, jak to funguje. Tyto znalosti vám pomůže:

- Řešení problémů s řešení.
- Plánování přizpůsobení řešení podle vlastního specifických požadavků. 
- Navržení vlastní IoT řešení používající Azure služby.

## <a name="logical-architecture"></a>Logická architektura

Následující schéma shrnuje logické součásti předkonfigurované řešení:

![Logická architektura](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Simulovaný zařízení

V předem řešení představuje simulovaný zařízení chlazení (například stavební air conditioner nebo zařízení air zpracování jednotky). Při nasazení předkonfigurované řešení taky automaticky zřízení čtyři simulovaný odesílala v [Azure WebJob][lnk-webjobs]. Simulovaný zařízení usnadňují prozkoumání chování řešení bez nutnosti nasazení fyzických zařízení. Abyste mohli nasadit skutečné fyzické zařízení, najdete v článku [Připojte zařízení pro vzdálené sledování předkonfigurované řešení] [ lnk-connect-rm] kurz.

Každý simulovaný zařízení můžete poslat IoT centrální následující typy zpráv:

| Zpráva  | Popis |
|----------|-------------|
| Při spuštění  | Při spuštění zařízení, odešle **informace o zařízení** zprávu s informacemi o sobě do back-end. Tato data obsahuje identifikátor zařízení, metadata zařízení, seznam příkazů podporuje zařízení a aktuální konfigurace zařízení. |
| Informace o stavu | Zařízení odesílá zprávu **informace o stavu** a nahlaste, jestli zařízení rozpozná stavu senzoru. |
| Telemetrie | Zařízení odesílá **telemetrie** zprávu, která zprávách simulovaný hodnoty pro teploty a vlhkosti shromážděné ze zařízení simulovaný senzorů. |


Simulovaný zařízení odeslání následující vlastnosti zařízení ve zprávě **informace o zařízení** :

| Vlastnost               |  Účel |
|------------------------|--------- |
| ID zařízení              | ID, které je k dispozici nebo přiřazeno při vytvoření zařízení v řešení. |
| Výrobce           | Výrobce zařízení |
| Číslo modelu           | Číslo modelu zařízení |
| Pořadové číslo          | Pořadové číslo zařízení |
| Firmware               | Aktuální verze firmwaru na zařízení |
| Platformy               | Architektura platformy zařízení |
| Procesor              | Procesory zařízení |
| Nainstalované RAM          | Velikost paměti RAM nainstalovaný na zařízení |
| Centrální povolený stav      | Vlastnost stavu IoT Centrum zařízení |
| Čas vytvoření           | Čas vytvoření zařízení v řešení |
| Časem aktualizace           | Čas posledního vlastnosti aktualizovaly se pro zařízení |
| Šířky               | Zeměpisná šířka umístění zařízení |
| Délky              | Umístění délky zařízení |

Simulator osazuje těchto vlastností v simulovaný zařízení s ukázkové hodnoty.  Pokaždé, když simulator inicializuje simulovaný zařízení zařízení přidal příspěvek předdefinovaných metadata IoT centrální. Všimněte si, jak to přepíše všechny aktualizace metadat na portálu zařízení.


Simulovaný zařízení můžete dělat následující příkazy odeslané na řídicím panelu řešení prostřednictvím centra IoT:

| Příkaz                | Popis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Odešle _ping_ zařízení ke kontrole, jestli že je aktivní   |
| StartTelemetry         | Spustí zařízení odesílání telemetrie                 |
| StopTelemetry          | Ukončí zařízení od konkrétních telemetrie             |
| ChangeSetPointTemp     | Změní hodnotu nastavit bod okolo kterého je generováno náhodná data |
| DiagnosticTelemetry    | Spustí simulator zařízení odeslání další telemetrie hodnotu (externalTemp) |
| ChangeDeviceState      | Změní vlastnosti rozšířené stavu pro zařízení a odešlete zprávu s informace o zařízení ze zařízení |

Příkaz potvrzení zařízení do back-end řešení je k dispozici prostřednictvím centra IoT.

## <a name="iot-hub"></a>Rozbočovač IoT

[Rozbočovač IoT] [ lnk-iothub] ingests data odeslaná ze zařízení do cloudu a díky kterému je dostupný k projektům Azure toku analýzy (ASA). Rozbočovač IoT taky rozešle příkazy zařízení jménem portálu zařízení. Jednotlivé úlohy ASA toku používá samostatné skupině spotř IoT centrální číst toku zpráv ze svých zařízeních.

## <a name="azure-stream-analytics"></a>Azure toku analýzy

Ve vzdáleném sledování řešení [Azure toku analýzy] [ lnk-asa] (ASA) odešle zařízení zprávy přijaté v centru IoT na jiné back-end komponenty pro zpracování nebo úložiště. Různé úlohy ASA funkce konkrétní na základě obsahu zprávy.

**Úlohy 1: informace o zařízení** filtruje zprávy zařízení informace z toku příchozích zpráv a odešle koncový bod centrální události. Zařízení odešle zařízení informace zprávy při spuštění a odpověď na příkaz **SendDeviceInfo** . Tuto úlohu je definována následujícím definice dotazu k identifikaci **informace o zařízení** zpráv:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Tuto úlohu odešle jeho výstup k rozbočovači událostí pro další zpracování.

**Úlohy 2: pravidla** vyhodnocuje příchozí teploty a vlhkosti telemetrie hodnoty před mezní hodnoty na zařízení. Mezní hodnoty se nastavují v editoru pravidla k dispozici v řídicím panelu řešení. Každou dvojici zařízení/hodnota je uložen podle časového razítka do objektů blob, který toku analýzy v jako **Odkaz Data**. Úlohy porovnává libovolnou neprázdnou hodnotu proti mezní nastavení zařízení. Pokud sešit překračuje ">" podmínky, uloží úlohu událost **upozornění** , která označuje, že je mezní hodnota překročení a poskytuje zařízení, hodnoty a hodnoty časové razítko. Tuto úlohu je definována následujícím definice dotazu k identifikaci telemetrie zpráv, které by mělo dojít varování:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Úlohy odesílá jeho výstup k rozbočovači událostí pro další zpracování a ukládá podrobnosti každé upozornění k úložišti objektů blob z načteno upozornění informace na řídicím panelu řešení.

**Úlohy 3: Telemetrie** pracuje na příchozí proudu telemetrie zařízení dvěma způsoby. První odešle všechny zprávy telemetrie ze zařízení k úložišti objektů blob trvalý služby dlouhodobě úložiště. Druhý vypočítá vlhkost průměrná minimální a maximální hodnoty nad posuvná okna pět minut a odesílá tyto údaje k úložišti objektů blob. Řídicí panel řešení přečte telemetrickými daty z úložiště objektů blob k naplnění grafy. Tento úkol je definována následujícím definice dotazu:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Rozbočovače události

Úlohy ASA **informace o zařízení** a **pravidla** výstupní data do události rozbočovače problémy se spolehlivým přesměrovat k **Procesor událostí** spuštění v WebJob.

## <a name="azure-storage"></a>Azure úložiště

Řešení používá úložišti objektů blob Azure uchovávat všechny telemetrie nezpracovanými a souhrnná data ze zařízení v řešení. Řídicí panel přečte telemetrickými daty z úložiště objektů blob k naplnění grafy. Zobrazit oznámení, řídicího panelu čte data z úložiště objektů blob záznam překročení telemetrie hodnoty nakonfigurováno prahové hodnoty. Úložiště objektů blob řešení taky používá k zaznamenání prahové hodnoty, které můžete nastavit v řídicím panelu.

## <a name="webjobs"></a>WebJobs

Kromě hostitelem simulátory zařízení, WebJobs v řešení také hostovat **Procesor událostí** spuštěné v Azure WebJob této úchyty zařízení informace zprávy a odpovědi na příkaz. Používá:

- Informace o zprávy zařízení aktualizace registru zařízení (uložené v databázi DocumentDB) s aktuální informace o zařízení.
- Příkaz odpovědi na zprávy Aktualizovat historii příkaz zařízení (uložené v databázi DocumentDB).

## <a name="documentdb"></a>DocumentDB

Řešení používá DocumentDB databáze pro uložení informací o zařízení připojených k řešení. Tyto informace zahrnují metadata zařízení a historie příkazy odeslána do zařízení na řídicím panelu.

## <a name="web-apps"></a>Webové aplikace

### <a name="remote-monitoring-dashboard"></a>Vzdálené sledování řídicího panelu
Tato stránka ve webové aplikaci používá PowerBI ovládací prvky javascript (viz [PowerBI vizuální prvky repo](https://www.github.com/Microsoft/PowerBI-visuals)) vizualizovat telemetrickými daty z zařízení. Řešení využívá úlohu telemetrie ASA psát telemetrickými daty pro úložiště objektů blob.


### <a name="device-administration-portal"></a>Portál Správa zařízení

Tento web appu umožňuje:

- Zřízení nového zařízení. Tato akce nastaví id jedinečné zařízení a vygeneruje klávesu ověřování. Informace o zařízení zapíše do registru identity IoT centrální a databázi DocumentDB specifické řešení.
- Spravovat vlastnosti zařízení. Tato akce obsahuje stávající vlastnosti zobrazení a aktualizace s nové vlastnosti.
- Příkazy posílat do zařízení.
- Zobrazení historie příkazů pro zařízení.
- Povolení a zakažte zařízení.

## <a name="next-steps"></a>Další kroky

Následující příspěvků na blogu TechNet najdete další podrobné informace o vzdálené monitorovací předkonfigurované řešení:

- [IoT sadu - rozšířená - vzdálené sledování](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [Sady IoT - vzdálené sledování – přidání Live a simulovaný zařízení](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Můžete pokračovat v začínáte pracovat s IoT sadu načtením v následujících článcích:

- [Připojte zařízení vzdálené sledování předkonfigurované řešení][lnk-connect-rm]
- [Oprávnění na webu azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md