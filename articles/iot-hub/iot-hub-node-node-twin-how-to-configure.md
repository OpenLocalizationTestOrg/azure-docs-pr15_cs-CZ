<properties
    pageTitle="Použití vlastností dvojitých | Microsoft Azure"
    description="Tento kurz se dozvíte, jak používat dvojitých vlastnosti"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Kurz: Požadované vlastnosti používá ke konfiguraci zařízení (verze preview)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

* **SimulateDeviceConfiguration.js**, o aplikaci simulovaný zařízení, která čeká požadovaná konfigurace aktualizace a sestav stavu proces aktualizace simulovaný konfigurace.
* **SetDesiredConfigurationAndQuery.js**, Node.js aplikace chtěli při spuštění ze serverové databázi, která nastavuje požadovaná konfigurace na zařízení a dotazech proces aktualizace konfigurace.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, můžete použít k vytváření zařízení a back-end aplikací.

Tento kurz je potřeba k provedení těchto věcí:

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

Pokud jste postupovali [začít pracovat s zařízení twins] [ lnk-twin-tutorial] kurzu už máte rozbočovači povolit správu zařízení a zařízení identit s názvem **myDeviceId**; a můžete přejít k [Vytvoření aplikaci simulovaný zařízení] [ lnk-how-to-configure-createapp] oddíl.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části vytvoříte aplikace konzoly Node.js, ke kterému je připojen k rozbočovači jako **myDeviceId**, čeká na požadovaná konfigurace aktualizace a sestavy aktualizace o procesu aktualizace simulovaný konfigurace.

1. Vytvořte novou prázdnou složku s názvem **simulatedeviceconfiguration**. Ve složce **simulatedeviceconfiguration** vytvoření nového souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **simulatedeviceconfiguration** spusťte tento příkaz nainstalovat **azure iot zařízení**a **azure iot zařízení mqtt** balíček:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **SimulateDeviceConfiguration.js** ve složce **simulatedeviceconfiguration** .

4. Přidejte následující kód k souboru **SimulateDeviceConfiguration.js** a nahradit **{zařízení připojovací řetězec}** zástupným připojovací řetězec, který jste zkopírovali jste vytvořili identitě **myDeviceId** zařízení:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Objekt **klienta** poskytuje všechny metody povinné chcete provést interakci s twins zařízení ze zařízení. Kód předchozí po inicializuje objekt **klientského** načte dvojitých **myDeviceId**a připojí rutinu k aktualizaci na požadované vlastnosti. Obslužná rutina ověří, že je žádost o konverzaci skutečné konfigurace změnit tak, že porovnáte configIds a potom vyvolá metodu, která spustí změna konfigurace.

    Všimněte si, že za účelem zjednodušení předchozí kód používá výchozím pevně úvodní konfiguraci. Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.
    
    > [AZURE.IMPORTANT] Události změny požadované vlastnosti jsou vždy jednou emitovány při připojení zařízení, zkontrolujte, že zkontrolujte, zda je skutečná změnu v požadované vlastnosti před provedením všech akcí.

5. Přidejte následující metody před `client.open()` vyvolání:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Metodu **initConfigChange** aktualizuje nahlášeného vlastnosti objektu místní dvojitých s žádostí o aktualizaci konfigurace a stav nastaví, **čeká na vyřízení**a pak se aktualizuje v dvojitých zařízení na službu. Po úspěšném aktualizaci dvojitých napodobuje dlouho pracovního procesu, který končí spuštění **completeConfigChange**. Tento způsob aktualizuje místní dvojitých nahlášeného vlastnosti nastavení stavu na **úspěšné** a odebráním **pendingConfig** objektu. Potom aktualizuje dvojitých na služby.

    Všimněte si, uložte šířky pásma, vykázaného vlastnosti jsou aktualizovány zadáním pouze vlastnosti, které chcete změnit (pojmenované **oprava** ve výše uvedeném kódu), místo nahrazení celým dokumentem.

    > [AZURE.NOTE] Tento kurz není simulovat jakékoli chování aktualizace souběžné konfigurace. Některé procesy aktualizací konfigurace možné tak, aby zahrnoval změny konfigurace cílové spuštěn aktualizace, ostatní může být potřeba je do fronty a ostatní můžou odmítnout se chybový stav. Zkontrolujte, že zvažte požadované chování pro konkrétní konfigurace obrázku a přidejte příslušné logickou před zahájením změna konfigurace.

6. Spusťte aplikaci zařízení:

        node SimulateDeviceConfiguration.js

    Zobrazí se zpráva `retrieved device twin`. Zachovat aplikace spuštěné.

## <a name="create-the-service-app"></a>Vytvoření aplikace služby

V této části vytvoříte aplikace konzoly Node.js, který aktualizuje *požadované vlastnosti* v dvojitých přidružené **myDeviceId** s objektem novou konfigurace telemetrie. Pak vyhledá twins zařízení uložené v centru a zobrazuje rozdíl mezi konfigurace požadované a nahlášeného zařízení.

1. Vytvořte novou prázdnou složku s názvem **setdesiredandqueryapp**. Ve složce **setdesiredandqueryapp** vytvoření nového souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **setdesiredandqueryapp** spusťte tento příkaz nainstalovat **azure iothub** :

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. V textovém editoru, vytvoření nového souboru **SetDesiredAndQuery.js** ve složce **addtagsandqueryapp** .

4. Přidejte následující kód k souboru **SetDesiredAndQuery.js** a nahradit **{služby připojovací řetězec}** zástupným připojovací řetězec, který jste zkopírovali jste vytvořili vaše centrum:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Objekt **registru** poskytuje všechny metody povinné chcete provést interakci s twins zařízení ze služby. Kód předchozí po inicializuje objekt **registru** načte dvojitých **myDeviceId**a aktualizuje nový objekt konfigurace telemetrie požadované vlastnosti. Poté hovorů události funkce **queryTwins** 10 sekund.

    > [AZURE.IMPORTANT] Tato aplikace dotazy IoT centrální každých 10 sekund příkladů důvodů. Použití dotazů generování sestav uživatele směřující na mnoha zařízeních a nechcete zjišťuje změny. Pokud vaše řešení v reálném čase oznámení o události zařízení pomocí [zařízení do cloudu zpráv][lnk-d2c].

5. Přidat následující kód právě před `registry.getDeviceTwin()` vyvolání implementovat funkci **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Předchozí kód dotazy twins uložené v centru a vytiskne konfigurace požadované a nahlášeného telemetrie. V nápovědě k [Centrální IoT dotazovací jazyk] [ lnk-query] informace o propracovaných sestavy na všech svých zařízeních.


8. **SimulateDeviceConfiguration.js** při spuštěné aplikaci spusťte aplikaci:

        node SetDesiredAndQuery.js 5m

    Měli byste vidět hlášení konfigurace změnu brání v **dosažení úspěchu** **čekající** na **úspěšné** znovu pomocí nových aktivní odeslat četnost pět minut, místo 24 hodin.

    > [AZURE.IMPORTANT] Existuje zpoždění až minutu mezi operaci sestavy zařízení a výsledků dotazu. To je to povolíte infrastruktury dotazu pro práci ve velkém měřítku velmi vysoké. K načtení konzistentní zobrazení jednoho dvojitých použijte metodu **getDeviceTwin** ve třídě **registru** .

## <a name="next-steps"></a>Další kroky

V tomto kurzu nastavte požadovanou konfiguraci jako *požadované vlastnosti* z aplikace back-end a napsali simulovaný zařízení aplikaci ke zjištění, změna a simulovat proces aktualizace vícekrokové vykazování stavu jako *vykázaného vlastnosti* dvojitých.

Použijte v následujících zdrojích informací se dozvíte, jak:

- odeslání telemetrie ze zařízení s [začít pracovat s IoT centrální] [ lnk-iothub-getstarted] kurzu
- Naplánování a provedení operace velkých sad zařízení najdete v článku [použití úlohy plánovat a vysílání zařízení operace] [ lnk-schedule-jobs] kurz.
- ovládací prvek zařízení interaktivně (například Zapnutí ventilace pomocí funkčně uživatelem řízené aplikace), [použijte přímé metody] [ lnk-methods-tutorial] kurz.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
