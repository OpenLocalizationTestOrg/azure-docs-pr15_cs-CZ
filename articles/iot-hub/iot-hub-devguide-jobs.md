<properties
 pageTitle="Příručka pro vývojáře – úlohy | Microsoft Azure"
 description="Azure Průvodce vývojář IoT centrální - plánování úlohy na spustit na několika zařízeních připojené k vaše Centrum"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Naplánovat úlohy na několika zařízeních (verze preview)

## <a name="overview"></a>Základní informace

Popsané v článcích předchozí Azure IoT centrální umožňuje počet stavebních bloků ([dvojitých vlastností a zařízení značky] [ lnk-twin-devguide] a [metody cloudu zařízení][lnk-dev-methods]).  Obvykle IoT back-end aplikace umožňují Správce zařízení a operátorů, aktualizovat a pracovat s IoT zařízení hromadně a plánované najednou.  Úlohy zapouzdřují provádění aktualizací dvojitých zařízení a metody C2D vůči sadě zařízení po jednom plánu.  Operátor třeba byste použili back-end aplikace, která by zahájení a sledovat úlohy restartujte sadu zařízení při vytváření 43 a podstava prostorového grafu 3 v době, která by pak nebyl skutečnosti operace budovy.

### <a name="when-to-use"></a>Kdy použít

Zvažte použití úlohy kdy: řešení zpět ukončit potřeb k plánování a sledování průběhu některou z následujících akcí se sadou zařízení:

- Aktualizovat vlastnosti dvojitých požadované zařízení
- Aktualizace zařízení dvojitých značky
- Volat C2D metody

## <a name="job-lifecycle"></a>Životního cyklu projektu

Úlohy zahájil: Po řešení back-end se spravují IoT centrální.  Zahájení projektu pomocí webových služeb URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) a dotaz průběhu provádění úloh prostřednictvím služby webových URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Zahájenou úlohy dotazování na úlohy umožní aplikaci back-end aktualizovat stav provádění úloh.

> [AZURE.NOTE] Po zahájení projektu názvy vlastností a hodnoty může obsahovat pouze US-ASCII tisknutelné alfanumerický, s výjimkou těch následující nastavení: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o používání úlohy.

## <a name="jobs-to-execute-direct-methods"></a>Úlohy na spustit přímé metody

Následuje podrobnosti žádost HTTP 1.1 pro spuštění [Přímá metoda] [ lnk-dev-methods] na několika zařízeních pomocí úlohy:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Úlohy aktualizovat vlastnosti dvojitých zařízení

Následující obrázek je podrobnosti žádost HTTP 1.1 pro aktualizaci vlastností dvojitých zařízení pomocí úlohy:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Dotazování na průběh úloh

Následuje podrobnosti žádost HTTP 1.1 [dotazování na úlohy][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
ContinuationToken pochází z odpovědi.  

## <a name="jobs-properties"></a>Vlastnosti úlohy

Následuje seznam vlastností a odpovídající popis, které lze použít při dotazu pro projekty nebo úlohy výsledky.

| Vlastnost | Popis |
| -------------- | -----------------|
| **jobId** | Aplikace podle ID pro daný úkol. |
| **čas spuštění** | Aplikace k dispozici čas zahájení (formátu ISO-8601) pro daný úkol. |
| **čas_ukončení** | Rozbočovač IoT podle data (formátu ISO-8601) pro dokončení projektu. Platný pouze po úlohy dosáhne "dokončené" stavu. | 
| **Typ** | Typy úloh: |
| | **scheduledUpdateTwin**: úlohy použít k aktualizaci sadu dvojitých požadované vlastnosti nebo značky. |
| | **scheduledDeviceMethod**: úlohy použitou k vyvolání metodu zařízení se sadou dvojitých. |
| **Stav** | Aktuální stav úlohy. Možné hodnoty stavu: |
| | **pole Čekání na** : Naplánování a čekající na vybrané službou projektu. |
| | **plánované** : naplánovaný čas v budoucnu. |
| | **spuštění** : aktivní projektu. |
| | **zrušeno** : zrušil projektu. |
| | **se nezdařilo** : úlohy se nezdařila. |
| | **dokončení** : dokončení projektu. |
| **deviceJobStatistics** | Statistika projektu spuštění. |

Během náhledu objekt deviceJobStatistics neexistuje až po dokončení projektu.

| Vlastnost | Popis |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Počet zařízení v projektu. |
| **deviceJobStatistics.failedCount** | Počet zařízení, kde úloha se nezdařila. |
| **deviceJobStatistics.succeededCount** | Počet zařízení místo, kam bylo úspěšné projektu. |
| **deviceJobStatistics.runningCount** | Počet zařízení, která je aktuálně spuštěných projektu. |
| **deviceJobStatistics.pendingCount** | Počet zařízení, které čekají úlohu. |


### <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT rozbočovači] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro spuštěnou aplikaci a správa operace.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT centrální a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT centrální MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT centrální protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, vás mohla zajímat následující kurz IoT rozbočovači:

- [Plán a vysílání úlohy][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
