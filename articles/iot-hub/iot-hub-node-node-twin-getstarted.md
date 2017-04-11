<properties
    pageTitle="Začínáme s twins | Microsoft Azure"
    description="Tento kurz se dozvíte, jak používat twins"
    services="iot-hub"
    documentationCenter="node"
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

# <a name="tutorial-get-started-with-device-twins-preview"></a>Kurz: Začínáme s zařízení twins (verze preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

* **AddTagsAndQuery.js**, Node.js aplikace chtěli při spuštění z back-end, která přidá značky a dotazy twins zařízení.
* **TwinSimulatedDevice.js**, Node.js aplikace, která napodobuje zařízení připojené k rozbočovači IoT s zařízení identit dříve vytvořili a sestav stavu připojení.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, můžete použít k vytváření zařízení a back-end aplikací.

Tento kurz je potřeba k provedení těchto věcí:

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Vytvoření aplikace služby

V této části vytvoříte aplikace konzoly Node.js, které umístění metadata dvojitých přidružené **myDeviceId**. Ho pak dotazů twins uložené v centru výběr zařízení umístěn v USA a sloupci vykazování mobilní připojení.

1. Vytvořte novou prázdnou složku s názvem **addtagsandqueryapp**. Ve složce **addtagsandqueryapp** vytvoření nového souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **addtagsandqueryapp** spusťte tento příkaz nainstalovat **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **AddTagsAndQuery.js** ve složce **addtagsandqueryapp** .

4. Přidejte následující kód k souboru **AddTagsAndQuery.js** a nahradit **{služby připojovací řetězec}** zástupným připojovací řetězec, který jste zkopírovali jste vytvořili vaše centrum:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    Objekt **registru** poskytuje všechny metody povinné chcete provést interakci s twins zařízení ze služby. Předchozí kód nejdřív inicializuje **registru** objekt a pak načte dvojitých **myDeviceId**a nakonec aktualizuje její značky informace požadované místo.

    Po aktualizaci značky volá funkci **queryTwins** .

7. Na konci **AddTagsAndQuery.js** implementovat funkci **queryTwins** přidáte následující kód:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Předchozí kód provede dvou dotazů: pouze twins zařízení zařízení najdete v podniku **Redmond43** vybere první a druhé také upravuje dotazu vyberte zařízení, které jsou také připojené prostřednictvím mobilní sítě.

    Všimněte si, že předchozího kódu, při vytváření objekt **dotazu** určuje maximální počet vrácených dokumentů. Objekt **dotaz** obsahuje **hasMoreResults** boolean vlastnost, která můžete volat metody **nextAsTwin** tisknutím k načtení všechny výsledky. Metoda s názvem **Další** je k dispozici pro výsledky, které nejsou zařízení twins například výsledky agregace dotazů.

8. Spusťte aplikaci:

        node AddTagsAndQuery.js

    Měli byste vidět jednoho zařízení ve výsledcích dotazu s žádostí o všech zařízeních nachází v **Redmond43** a žádná dotazu s omezením výsledky k zařízení, které používají mobilní síť.

    ![][1]

V následující části Vytvoření aplikace zařízení se ohlásí informace o připojení a mění výsledků dotazu v předchozí části.

## <a name="create-the-device-app"></a>Vytvoření aplikace zařízení

V této části vytvoříte Node.js konzoly aplikace, která se připojí k centrální jako **myDeviceId**a pak aktualizujete jeho dvojitých vykázaného vlastnosti s informacemi, že je připojený prostřednictvím mobilní sítě.

> [AZURE.NOTE] V tomto okamžiku jsou dostupné pouze z zařízení připojujících se k rozbočovači IoT pomocí protokolu MQTT twins zařízení. Získáte [podporují MQTT] [ lnk-devguide-mqtt] článku najdete pokyny, jak chcete převést existující zařízení aplikaci používat MQTT.

1. Vytvořte novou prázdnou složku s názvem **reportconnectivity**. Ve složce **reportconnectivity** vytvoření nového souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **reportconnectivity** spusťte tento příkaz k instalaci **azure iot zařízení**a **azure iot zařízení mqtt** balíčku:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **ReportConnectivity.js** ve složce **reportconnectivity** .

4. Přidejte následující kód k souboru **ReportConnectivity.js** a nahradit **{zařízení připojovací řetězec}** zástupným připojovací řetězec, který jste zkopírovali jste vytvořili identitě **myDeviceId** zařízení:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Objekt **klienta** poskytuje všechny metody, kterou požadujete Pokud chcete provést interakci s twins zařízení ze zařízení. Kód předchozí po inicializuje objekt **klientského** načte dvojitých **myDeviceId** a aktualizace vlastností nahlášeného s informacemi o připojení.

5. Spusťte aplikaci zařízení

        node ReportConnectivity.js

    Zobrazí se zpráva `twin state reported`.

6. Teď zařízení uvedeny informace o připojení, by se měly v obou dotazů. Přejděte zpátky ve složce **addtagsandqueryapp** a znovu spustíte dotazy:

        node AddTagsAndQuery.js

    Tento čas **myDeviceId** by se měly v obou výsledků dotazu.

    ![][3]

## <a name="next-steps"></a>Další kroky
V tomto kurzu nakonfigurované rozbočovači nové IoT Azure portálu a vytvoří zařízení identit v registru identity centrální. Přidá zařízení metadata jako značky z aplikace back-end a napsali simulovaný zařízení aplikaci a informace o připojení zařízení sestavu v dvojitých zařízení. Můžete taky naučili k vytvoření dotazu tyto informace pomocí centra IoT SQL profesionálové dotazovací jazyk.

Použijte v následujících zdrojích informací se dozvíte, jak:

- odeslání telemetrie ze zařízení s [začít pracovat s IoT centrální] [ lnk-iothub-getstarted] kurzu
- Konfigurace zařízení pomocí [použít požadované vlastnosti konfigurace zařízení] v dvojitých požadované vlastnosti[ lnk-twin-how-to-configure] kurzu
- ovládací prvek zařízení interaktivně (například Zapnutí ventilace pomocí funkčně uživatelem řízené aplikace), [použijte přímé metody] [ lnk-methods-tutorial] kurz.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md