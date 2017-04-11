<properties
    pageTitle="Odesílání zpráv cloudu zařízení s IoT centrální | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak k odesílání zpráv cloudu zařízení centrální IoT Azure pomocí jazyka Java."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Kurz: Postup při odesílání zprávy cloudu zařízení s IoT centrální a Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Úvod

Azure centrální IoT je plně spravovaných služba, která pomáhá povolit spolehlivý a zabezpečené obousměrné komunikaci mezi miliony IoT zařízení a ukončení aplikace zpět. Kurz [Seznámení s IoT centrální] ukazuje, jak vytvořit rozbočovači IoT zřízení zařízení identit v něm a kód simulovaný zařízení, které odesílá zprávy zařízení do cloudu.

Tento kurz je založena na [Začínáme s IoT centrální]. To se dozvíte, jak chcete:

- Z vaší aplikace cloudu back-end odešlete zprávy cloudu zařízení jednoho zařízení pomocí IoT rozbočovače.
- Přijímání zpráv cloudu zařízení na zařízení.
- Z aplikace cloudu serverová, požádat o potvrzení o doručení (*názory*) u zpráv odeslaných z centrální IoT zařízení.

Další informace o zprávách cloudu zařízení můžete najít v [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

Na konci tohoto kurzu spusťte dvě aplikace konzoly Node.js:

* **SimulatedDevice**, upravenou verzi aplikace vytvořené v [začít pracovat s IoT centrální], která se připojí k centrální IoT volání a přijímání zpráv cloudu zařízení.
* **SendCloudToDeviceMessage**, která odešle zprávu cloudu zařízení simulovaný zařízení pomocí IoT rozbočovače a dostane jeho potvrzení doručení.

> [AZURE.NOTE] Rozbočovač IoT podporuje SDK pro mnoho platformy zařízení a jazyky (včetně C, Java a Javascript) až SDK Azure IoT zařízení. Podrobný návod k připojení zařízení tento kurz kód a obecně k rozbočovači IoT Azure naleznete v článku [Středisko pro vývojáře IoT Azure].

Tento kurz, je potřeba k provedení těchto věcí:

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Přijímání zpráv na simulovaný zařízení

V této části můžete změnit simulovaný zařízení aplikaci vytvořené v části [Začínáme s IoT centrální] do cloudu zařízení zprávy z centra IoT.

1. V textovém editoru, otevřete soubor SimulatedDevice.js.

2. Upravte funkci **connectCallback** zpracovávání zprávám odesílaným z centrální IoT. V tomto příkladu zařízení vždy vyvolá **kompletní** funkce sdělit IoT centrální nezpracuje zprávu. Novou verzi funkci **connectCallback** vypadat takto:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Pokud používáte HTTP místo MQTT nebo AMQP jako přenos, **DeviceClient** instance hledá zprávy od IoT centrální zřídka (nižší než každých 25 minut). Další informace o rozdílech mezi MQTT, AMQP a HTTP podporu a IoT centrální omezení, najdete v článku [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy cloudu zařízení

V této části Vytvoření aplikace konzoly Node.js zasílání zpráv cloudu zařízení aplikaci simulovaný zařízení. Potřebujete Id zařízení, které jste přidali v kurzu [Začínáme s IoT Centrum] zařízení. Budete potřebovat pro vaše Centrum IoT, které můžete najít v [Azure portál]připojovací řetězec.

1. Vytvoření vyprázdnit složku s názvem **sendcloudtodevicemessage**. Ve složce **sendcloudtodevicemessage** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **sendcloudtodevicemessage** spusťte tento příkaz nainstalovat **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. V textovém editoru, vytvoření nového souboru **SendCloudToDeviceMessage.js** ve složce **sendcloudtodevicemessage** .

4. Přidejte následující `require` příkazy na začátku **SendCloudToDeviceMessage.js** souboru:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Přidejte následující kód **SendCloudToDeviceMessage.js** soubor. Nahraďte zástupný symbol hodnotu připojovacího řetězce připojovací řetězec, který jste vytvořili v kurzu [Začínáme s IoT centrální] rozbočovače IoT. Nahrazení zástupných zařízení cílové id zařízení zařízení, které jste přidali v kurzu [Začínáme s IoT centrální] :

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Přidejte následující funkce pro tisk výsledky operace ke konzole:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Přidejte následující funkce pro tisk doručení zprávy názory ke konzole:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Přidejte následující kód odeslat zprávu o zařízení a zpracování zprávy zpětnou vazbu, pokud zařízení potvrdí zpráva cloudu zařízení:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Uložte a zavřete soubor **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. V příkazového řádku ve složce **simulateddevice** spusťte tento příkaz předat telemetrie k rozbočovači IoT a přijímat zprávy cloudu zařízení:

    ```
    node SimulatedDevice.js 
    ```

    ![Spusťte aplikaci simulovaný zařízení][img-simulated-device]

2. Příkazového řádku ve složce **sendcloudtodevicemessage** spusťte tento příkaz Odeslat zprávu cloudu zařízení a počkejte k získání názorů potvrzení:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Spusťte aplikaci a c2d příkaz Odeslat][img-send-command]

    > [AZURE.NOTE] V jednoduchosti saké tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení), jako navrhované v článku MSDN [Přechodná zpracování chyby].

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte odesílání a přijímání zpráv cloudu zařízení. 

Příklady úplné řešení začátku do konce, které používají IoT centrální najdete [Azure IoT sadu].

Další informace o vytváření řešení s IoT centrální, najdete v článku [IoT centrální vývojář Průvodce].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Začínáme s IoT centrální]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Příručka IoT centrální vývojář]: iot-hub-devguide.md
[Středisko pro vývojáře IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portálu]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/