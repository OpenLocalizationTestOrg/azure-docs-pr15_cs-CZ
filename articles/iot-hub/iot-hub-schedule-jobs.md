<properties
 pageTitle="Jak naplánovat úlohy | Microsoft Azure"
 description="Tento kurz se dozvíte, jak naplánovat úlohy"
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

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Kurz: Plánování a vysílání úloh (verze preview)

## <a name="introduction"></a>Úvod
Azure centrální IoT je plně spravovaných služba, která umožňuje aplikace back-end můžete vytvořit a sledovat úloh, které plánování a aktualizovat miliony zařízení.  Práce se dá použít pro následující akce:

- Aktualizovat vlastnosti dvojitých požadované zařízení
- Aktualizace zařízení dvojitých značky
- Volat metody cloudu zařízení

Úlohy entity zalamuje jednu z následujících akcí a ke sledování průběhu provádění vůči sadě zařízení, která je definovaná v dvojitých dotazu.  Například pomocí úlohy aplikace back-end může vyvolat metodu restartujte počítač a tabletů 10 000, nastavil dvojitých dotazu a plánování později.  Tato aplikace pak můžete sledovat průběh postupně každou z těchto zařízení dostávat a spustit metodu restartujte počítač.

Další informace o každé z těchto možností v těchto článcích:

- Dvojité zařízení a vlastnosti: [Začínáme s dvojitých] [ lnk-get-started-twin] a [kurzu: použití vlastností dvojitých][lnk-twin-props]
- Metod cloudu zařízení: [Vývojář Průvodce - přímé metody] [ lnk-dev-methods] a [kurz: C2D metody][lnk-c2d-methods]

Tento kurz se dozvíte, jak chcete:

- Vytvoření simulovaný zařízení, které má přímé metodu, která umožňuje lockDoor nazývanou může být aplikací zpět ukončit.
- Vytvoření konzoly aplikace, která volá metodu přímé lockDoor na simulovaný zařízení pomocí úlohy a dvojitých požadované vlastnosti pomocí úlohy zařízení se aktualizuje.

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

**simDevice.js**, která se připojí k rozbočovače IoT s zařízení identit volání a přijímání přímá metoda lockDoor.

**scheduleJobService.js**, která volá přímé metodu na simulovaný zařízení a aktualizovat vlastnosti disired dvojitých pomocí úlohy.

Tento kurz, je potřeba k provedení těchto věcí:

Verze Node.js 0.12.x nebo novější <br/>  [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Node.js pro účely tohoto návodu Windows nebo Linux.

Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části Vytvoření aplikace konzoly Node.js reagující na přímé metodu nazvanou podle cloudu, který spustí simulovaný zařízení restartujte počítač a používá dvojitých zařízení vykázaného vlastností, které lze povolit zařízení dvojitých dotazy k identifikaci zařízení a kdy se naposledy restartování počítače.

1. Vytvořte novou prázdnou složku s názvem **simDevice**.  Ve složce **simDevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **simDevice** spusťte tento příkaz nainstalovat zabalit zařízení SDK **azure iot zařízení** a **azure iot zařízení mqtt** balíčku:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **simDevice.js** ve složce **simDevice** .

4. Přidejte následující "vyžadují" příkazy na začátku **simDevice.js** souboru:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Přidání proměnné **connectionString** , můžete vytvořit klienta zařízení.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Přidejte následující funkce pro zpracování metodu lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Přidejte následující kód pro registraci rutiny pro metodu lockDoor.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Uložte a zavřete soubor **simDevice.js** .

> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Naplánovat úlohy volání přímé metody a aktualizace dvojitých vlastnosti

V této části Vytvoření aplikace konzoly Node.js, které iniciuje vzdálené lockDoor na zařízení přímé metodou a aktualizovat dvojitých vlastnosti.

1. Vytvořte novou prázdnou složku s názvem **scheduleJobService**.  Ve složce **scheduleJobService** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **scheduleJobService** spusťte tento příkaz k instalaci **azure iothub** zařízení SDK balíčku a **azure iot zařízení mqtt** balíčku:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. V textovém editoru, vytvoření nového souboru **scheduleJobService.js** ve složce **scheduleJobService** .

4. Přidejte následující "vyžadují" příkazy na začátku **dmpatterns_gscheduleJobServiceetstarted_service.js** souboru:

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Přidejte následující deklarace proměnných a nahraďte zástupný symbol hodnoty:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Přidejte následující funkce, která se bude používat sledování provádění úlohy:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Přidejte následující kód pro plánování úlohy, volající metodu zařízení:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Přidejte následující kód naplánovat úlohy pro aktualizaci dvojitých:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Uložte a zavřete soubor **scheduleJobService.js** .

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. V příkazového řádku ve složce **simDevice** spusťte tento příkaz zahájíte naslouchají přímá metoda restartujte počítač.

    ```
    node simDevice.js
    ```

2. V příkazového řádku ve složce **scheduleJobService** spusťte tento příkaz spustí vzdálené restartujte počítač a dotaz pro zařízení dvojitých zobrazíte poslední restartujte čas.

    ```
    node scheduleJobService.js
    ```

3. Zobrazí se výstup zařízení a back-end aplikace.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste použili úlohy naplánování přímé metodu pro zařízení a aktualizovat vlastnosti dvojitých zařízení.

Pokračovat v začínáte pracovat s IoT centrální a vzorků správy zařízení například vzdáleného přes air aktualizaci firmwaru, najdete tady:

[Kurz: Postup aktualizaci firmwaru][lnk-fwupdate]

Pokračujte začínáte pracovat s IoT centrální najdete v článku [Začínáme s SDK brány IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx