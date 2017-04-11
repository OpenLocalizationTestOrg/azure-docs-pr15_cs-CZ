<properties
    pageTitle="Azure IoT základem Node.js Začínáme | Microsoft Azure"
    description="Azure centrální IoT s Node.js začíná začali kurz. Pomocí Azure IoT centrální a Node.js SDK Microsoft Azure IoT implementovat řešení pro Internet věci."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Začínáme s Azure IoT Centrum pro Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu máte tři Node.js konzoly aplikace:

* **CreateDeviceIdentity.js**, která vytvoří zařízení identit a klíč přidružené zabezpečení připojení simulovaný zařízení.
* **ReadDeviceToCloudMessages.js**, které zobrazuje telemetrie odeslaný simulovaný zařízení.
* **SimulatedDevice.js**, který připojí k rozbočovači IoT se zařízení identit dříve vytvořili a odešle zprávu telemetrie každý druhý pomocí protokolu AMQP.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, které můžete použít k vytvoření obou spouštět aplikace na zařízeních a vašeho řešení back-end.

Tento kurz, je potřeba k provedení těchto věcí:

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Teď jste vytvořili rozbočovače IoT. Máte hostname IoT centrální a IoT centrální připojovací řetězec, který je potřeba dokončit zbytku tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření zařízení identity

V této části Vytvoření aplikace konzoly Node.js vytvářející zařízení identit v registru identity rozbočovače IoT. Zařízení nemůže připojit k centrální IoT, pokud má položka registru identity zařízení. Přečtěte si část **Zařízení Identity registru** [IoT centrální Developer Guide] [ lnk-devguide-identity] Další informace. Při spuštění této aplikace konzoly vygeneruje jedinečný zařízení ID a klávesy, která zařízení můžete použít k identifikaci k zasílání zpráv zařízení do cloudu k rozbočovači IoT.

1. Vytvořte novou prázdnou složku s názvem **createdeviceidentity**. Ve složce **createdeviceidentity** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **createdeviceidentity** spusťte tento příkaz nainstalovat SDK služby **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. V textovém editoru, vytvořte **CreateDeviceIdentity.js** soubor ve složce **createdeviceidentity** .

4. Přidejte následující `require` údajů na začátku **CreateDeviceIdentity.js** souboru:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Přidat následující kód k souboru **CreateDeviceIdentity.js** a nahraďte připojovací řetězec, který jste vytvořili v předchozí části rozbočovače IoT hodnotu zástupného textu: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Přidejte následující kód a vytvořte definici zařízení v registru identity zařízení rozbočovače IoT. Tento kód vytvoří zařízení Pokud identifikátor zařízení není k dispozici v registru, jinak vrátí klávesu existující zařízení:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Uložte a zavřete soubor **CreateDeviceIdentity.js** .

8. Pokud chcete spustit aplikaci **createdeviceidentity** , spusťte následující příkaz příkazového řádku ve složce createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Poznamenejte si **id zařízení** a **klíče zařízení**. Je třeba tyto hodnoty později při vytváření aplikace, ke kterému je připojen k rozbočovači IoT jako zařízení.

> [AZURE.NOTE] Registru identity IoT centrální pouze ukládá zařízení identit povolit zabezpečený přístup k centru. Slouží k ukládání ID zařízení a klávesy se šipkami použít jako zabezpečovací přihlašovací údaje a povolit nebo zakázat příznak, který slouží k zakázání přístupu pro jednotlivé zařízení. Pokud aplikace potřebuje k ukládání ostatních metadatech specifické pro zařízení, používejte úložiště specifické pro aplikaci. [Karta Vývojář v] příručce IoT centrální[ lnk-devguide-identity] Další informace.

## <a name="receive-device-to-cloud-messages"></a>Zprávy zařízení do cloudu

V této části Vytvoření aplikace konzoly Node.js, který bude číst zprávy zařízení do cloudu z centrální IoT. Rozbočovači IoT zpřístupňuje [Události rozbočovače][lnk-event-hubs-overview]-kompatibilní koncový bod umožňující přečtených zprávách: zařízení do cloudu. Tento kurz jednodušší jednoduché, vytvoří základní reader, který není vhodné pro nasazení vysoký výkon. [Zpracování zpráv zařízení do cloudu] [ lnk-process-d2c-tutorial] kurzu se dozvíte, jak ke zpracování zpráv ve velkém měřítku zařízení do cloudu. [Začínáme s události rozbočovače] [ lnk-eventhubs-tutorial] kurz poskytuje další informace o tom, k zpracování zpráv z rozbočovače události a platí pro koncové body kompatibilní s prohlížečem IoT centrální události centrální.

> [AZURE.NOTE] Kompatibilní s prohlížečem události centrální koncový bod pro čtení zpráv zařízení do cloudu vždy používá protokol AMQP.

1. Vytvořte novou prázdnou složku s názvem **readdevicetocloudmessages**. Ve složce **readdevicetocloudmessages** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **readdevicetocloudmessages** spusťte tento příkaz nainstalovat **azure události rozbočovače** :

    ```
    npm install azure-event-hubs --save
    ```

3. V textovém editoru, vytvořte **ReadDeviceToCloudMessages.js** soubor ve složce **readdevicetocloudmessages** .

4. Přidejte následující `require` příkazy na začátku **ReadDeviceToCloudMessages.js** souboru:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Přidejte následující proměnné deklaraci a nahraďte připojovací řetězec pro vaše Centrum IoT hodnotu zástupného textu:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Přidejte následující dvě funkce, které tisk výstupu ke konzole:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Přidejte následující kód pro vytvoření **EventHubClient**, otevřete dialogové okno připojení k rozbočovači IoT a vytvoření sluchátko pro každý oddíl. Tato aplikace používá filtru při vytváření příjemce, aby příjemce přečte pouze zprávy poslané na IoT centrální po spuštění příjemce. Tento filtr je užitečné v testovacím prostředí, ke které se zobrazí pouze aktuální sadu zpráv. V prostředí výrobní kódu měli zpracuje všechny zprávy – přečtěte si, [jak zpracování zpráv IoT Centrum zařízení do cloudu] [ lnk-process-d2c-tutorial] výuková Další informace:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Uložte a zavřete soubor **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte aplikace konzoly Node.js, který napodobuje zařízení, které odesílá zařízení do cloudu zprávy k rozbočovači IoT.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **simulateddevice** spusťte tento příkaz nainstalovat na zařízení SDK balíčky **azure iot zařízení** a **azure iot zařízení amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. V textovém editoru, vytvoření nového souboru **SimulatedDevice.js** ve složce **simulateddevice** .

4. Přidejte následující `require` příkazy na začátku **SimulatedDevice.js** souboru:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Přidání proměnné **connectionString** , můžete vytvořit klienta zařízení. Nahrazení **{youriothostname}** s názvem centrální IoT jste vytvořili v části *Vytvoření rozbočovači IoT* . **{Yourdevicekey}** nahradíte hodnoty klíče zařízení, vytvořeného v části *vytvoření identity zařízení* :

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Přidejte následující funkce Zobrazit výstup z aplikace:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Vytvoření zpětné a použijte funkci **setInterval** odešlete novou zprávu rozbočovače IoT za sekundu:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Připojení k centrální IoT otevřete a začněte odesílání zpráv:

    ```
    client.open(connectCallback);
    ```

9. Uložte a zavřete soubor **SimulatedDevice.js** .

> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].


## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Příkazového řádku ve složce **readdevicetocloudmessages** spusťte tento příkaz zahájíte sledování rozbočovače IoT:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT centrální služby klientské aplikace pro sledování zpráv zařízení do cloudu][7]

2. Příkazového řádku ve složce **simulateddevice** spusťte tento příkaz začněte posílat rozbočovače IoT telemetrickými daty:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT Centrum zařízení klientské aplikace k odesílání zpráv zařízení do cloudu][8]

3. **Použití** dlaždice [Azure portál] [ lnk-portal] zobrazuje počet odeslaných k Centru zpráv:

    ![Azure portálu použití dlaždic znázorňující počet odeslaných zpráv k rozbočovači IoT][43]

## <a name="next-steps"></a>Další kroky

V tomto kurzu nakonfigurované rozbočovači nové IoT Azure portálu a vytvoří zařízení identit v registru identity centrální. K povolení aplikace simulovaný zařízení k odesílání zpráv zařízení do cloudu k rozbočovači použít tuto identitu zařízení. Aplikaci, která se zobrazí na zprávy přijaté centru taky vytvořili. 

Pokračujte začínáte pracovat s IoT centrální a prozkoumání ostatních případech IoT naleznete v tématu:

- [Připojení zařízení][lnk-connect-device]
- [Začínáme používat správu zařízení][lnk-device-management]
- [Začínáme s SDK IoT brány][lnk-gateway-SDK]

Informace o prodloužení IoT řešení a zpracování zpráv zařízení do cloudu ve velkém měřítku, najdete v článku [zpracování zpráv zařízení do cloudu] [ lnk-process-d2c-tutorial] kurz.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
