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
* **SetDesiredConfigurationAndQuery**, aplikace konzoly .NET chtěli při spuštění z back-end, která nastavuje požadovaná konfigurace na zařízení a dotazů proces aktualizace konfigurace.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, můžete použít k vytváření zařízení a back-end aplikací.

Tento kurz je potřeba k provedení těchto věcí:

+ Microsoft Visual Studio 2015.

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

    Metodu **initConfigChange** aktualizuje nahlášeného vlastnosti objektu místní dvojitých s žádostí o aktualizaci konfigurace a stav nastaví, **čeká na vyřízení**a pak se aktualizuje v dvojitých zařízení na službu. Po úspěšném aktualizaci dvojitých napodobuje dlouho pracovního procesu, který končí spuštění **completeConfigChange**. Tento způsob aktualizuje místní dvojitých nahlášeného vlastnosti nastavení stavu na **úspěšné** a odebrání **pendingConfig** objektu. Potom aktualizuje dvojitých služby.

    Všimněte si, uložte šířky pásma, vykázaného vlastnosti jsou aktualizovány zadáním pouze vlastnosti, které chcete změnit (pojmenované **oprava** ve výše uvedeném kódu), místo nahrazení celým dokumentem.

    > [AZURE.NOTE] Tento kurz není simulovat jakékoli chování aktualizace souběžné konfigurace. Některé procesy aktualizací konfigurace možné tak, aby zahrnoval změny konfigurace cílové spuštěn aktualizace, ostatní může být potřeba je do fronty a ostatní můžou odmítnout se chybový stav. Zkontrolujte, že zvažte požadované chování pro konkrétní konfigurace obrázku a přidejte příslušné logickou před zahájením změna konfigurace.

6. Spusťte aplikaci zařízení:

        node SimulateDeviceConfiguration.js

    Zobrazí se zpráva `retrieved device twin`. Zachovat aplikace spuštěné.

## <a name="create-the-service-app"></a>Vytvoření aplikace služby

V této části vytvoříte aplikace konzoly .NET, který aktualizuje *požadované vlastnosti* v dvojitých přidružené **myDeviceId** s objektem novou konfigurace telemetrie. Pak vyhledá twins zařízení uložené v centru a zobrazuje rozdíl mezi konfigurace požadované a nahlášeného zařízení.

1. Ve Visual Studiu přidáte projekt Visual Basic klasické počítač s Windows do aktuálního řešení pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **SetDesiredConfigurationAndQuery**.

    ![Nový projekt Visual Basic klasické počítač s Windows][img-createapp]

2. V Průzkumníku klikněte pravým tlačítkem **SetDesiredConfigurationAndQuery** projektu a potom klikněte na **Spravovat balíčků Nuget**.

3. V okně **Správce balíčků Nuget** zkontrolujte, zda je zaškrtnuto políčko **Zahrnout zkušební** , vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** a nainstalujte *předprodejní* verzi **Microsoft.Azure.Devices** balíček a přijměte podmínky použití. Tento postup ke stažení, instalace a přidá odkaz na [Microsoft Azure IoT služby SDK] [ lnk-nuget-service-sdk] Nuget balíčku a jejich závislosti.

    ![Okno Nuget balíčku správce][img-servicenuget]

4. Přidejte následující `using` příkazy v horní části souboru **Program.cs** :

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Následující pole přidáte do **programu** předmětu. Nahrazení zástupného symbolu hodnoty připojovací řetězec pro centrální IoT, který jste vytvořili v předchozí části.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Přidejte následující metodu třídy **aplikace** :

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    Objekt **registru** poskytuje všechny metody povinné chcete provést interakci s twins zařízení ze služby. Kód předchozí po inicializuje objekt **registru** načte dvojitých **myDeviceId**a aktualizuje požadované vlastnosti s objektem novou konfigurace telemetrie.
    Poté každých 10 sekund ho dotazy twins uložené v centru a tiskne požadované a nahlášeného telemetrie konfigurace. V nápovědě k [Centrální IoT dotazovací jazyk] [ lnk-query] informace o propracovaných sestavy na všech svých zařízeních.

    > [AZURE.IMPORTANT] Tato aplikace dotazy IoT centrální každých 10 sekund příkladů důvodů. Použití dotazů generování sestav uživatele směřující na mnoha zařízeních a nechcete zjišťuje změny. Pokud vaše řešení v reálném čase oznámení o události zařízení pomocí [zařízení do cloudu zpráv][lnk-d2c].

7. Nakonec přidejte následující řádky metody **hlavní** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. **SimulateDeviceConfiguration.js** při spuštěné aplikaci spustit aplikaci .NET z aplikace Visual Studio pomocí **F5** a měli vidět nahlášeného konfigurace přejděte brání v **dosažení úspěchu** **čekající** na **úspěšné** znovu pomocí nových aktivní odeslat četnost pět minut, místo 24 hodin.

    > [AZURE.IMPORTANT] Existuje zpoždění až minutu mezi operaci sestavy zařízení a výsledků dotazu. To je to povolíte infrastruktury dotazu pro práci ve velkém měřítku velmi vysoké. K načtení konzistentní zobrazení jednoho dvojitých použijte metodu **getDeviceTwin** ve třídě **registru** .

## <a name="next-steps"></a>Další kroky

V tomto kurzu nastavte požadovanou konfiguraci jako *požadované vlastnosti* z back-end a napsali zařízení aplikaci ke zjištění této změny a simulovat proces aktualizace vícekrokové vykazování stavu jako *nahlášeného vlastnosti* dvojitých.

Použijte v následujících zdrojích informací se dozvíte, jak:

- odeslání telemetrie ze zařízení s [začít pracovat s IoT centrální] [ lnk-iothub-getstarted] kurzu
- Naplánování a provedení operace velkých sad zařízení najdete v článku [použití úlohy plánovat a vysílání zařízení operace] [ lnk-schedule-jobs] kurz.
- ovládací prvek zařízení interaktivně (například Zapnutí ventilace pomocí funkčně uživatelem řízené aplikace), [použijte přímé metody] [ lnk-methods-tutorial] kurz.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

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
