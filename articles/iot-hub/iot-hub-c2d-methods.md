<properties
 pageTitle="Použití přímé metody | Microsoft Azure"
 description="Tento kurz se dozvíte, jak používat přímé metody"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Kurz: Použít přímé metody

## <a name="introduction"></a>Úvod

Azure rozbočovači IoT je plně spravovaných služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a aplikace serverová. Předchozí výukové programy pro ([začít pracovat s IoT centrální] a [posílání zpráv cloudu zařízení s IoT centrální]) ilustrují základní zařízení do cloudu a cloudu zařízení zpráv funkce centrální IoT. Rozbočovač IoT dále poskytuje možnost volat není spotřeby metody na zařízeních z cloudu. Metody představují požadavek odpověď interakce s podobným pozvání na HTTP zařízením úspěšné nebo nepodařilo okamžitě (po uživatelem zadaný časový), aby uživatel vědět stav hovor. [Přímé metodu na zařízení] [ lnk-devguide-methods] popisuje způsobů podrobněji a nabízí pokyny, kdy je vhodné použít metody versus cloudu zařízení zprávy.

Tento kurz se dozvíte, jak chcete:

- Pomocí portálu Azure můžete vytvořit rozbočovači IoT a vytvoření zařízení identit v centrální IoT.
- Vytvořte simulovaný zařízení, který má přímé metodu nazývanou v cloudu.
- Vytvoření konzoly aplikace, která volá přímé metodu na simulovaný zařízení pomocí rozbočovače IoT.

> [AZURE.NOTE] V tomto okamžiku jsou dostupné pouze z zařízení připojujících se k rozbočovači IoT pomocí protokolu MQTT přímé metody. Získáte [podporují MQTT] [ lnk-devguide-mqtt] článku najdete pokyny, jak chcete převést existující zařízení aplikaci používat MQTT.

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

* **CallMethodOnDevice.js**, který volá metodu na simulovaný zařízení a zobrazí odpověď.
* **SimulatedDevice.js**, připojí k rozbočovači IoT se zařízení identit dříve vytvořili, který odpovídá metodu s názvem v cloudu.

> [AZURE.NOTE] Článek [IoT rozbočovači SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, které můžete použít k vytvoření obou spouštět aplikace na zařízeních a vašeho řešení back-end.

Tento kurz, je potřeba k provedení těchto věcí:

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části Vytvoření aplikace konzoly Node.js reagující na metodu nazvanou podle cloudu.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **simulateddevice** spusťte tento příkaz nainstalovat zabalit zařízení SDK **azure iot zařízení** a **azure iot zařízení mqtt** balíčku:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **SimulatedDevice.js** ve složce **simulateddevice** .

4. Přidejte následující `require` příkazy na začátku **SimulatedDevice.js** souboru:

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Přidání proměnné **connectionString** a použijete k vytváření klientských zařízení. **{Zařízení připojovací řetězec}** nahraďte připojovací řetězec vytvořeného v části *vytvoření identity zařízení* :

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Přidejte následující funkce implementovat metodu na zařízení:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Otevřít připojení k centrální IoT a začněte inicializace posluchače metodu:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Uložte a zavřete soubor **SimulatedDevice.js** .

> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například opakování připojení), jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Volání metody na zařízení

V této části Vytvoření aplikace konzoly Node.js, která volá metodu na simulovaný zařízení a pak zobrazuje odpověď.

1. Vytvořte novou prázdnou složku s názvem **callmethodondevice**. Ve složce **callmethodondevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **callmethodondevice** spusťte tento příkaz nainstalovat **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. V textovém editoru, vytvořte **CallMethodOnDevice.js** soubor ve složce **callmethodondevice** .

4. Přidejte následující `require` příkazy na začátku **CallMethodOnDevice.js** souboru:

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Přidejte následující proměnné deklaraci a nahraďte připojovací řetězec pro vaše Centrum IoT hodnotu zástupného textu:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Vytvoření klienta otevřete připojení k centrální IoT.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Přidejte následující funkce pro vyvolání metodu zařízení a vytisknout odpověď zařízení ke konzole:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Uložte a zavřete soubor **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Příkazového řádku ve složce **simulateddevice** spusťte tento příkaz začít poslouchat pro volání metody z rozbočovače IoT:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Příkazového řádku ve složce **callmethodondevice** spusťte tento příkaz zahájíte sledování rozbočovače IoT:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Zobrazí se zařízení reagovat na požadovanou metodu vytištěním se zpráva a aplikace, která s názvem způsob zobrazení odpovědi ze zařízení:

    ![][9]
    
## <a name="next-steps"></a>Další kroky

V tomto kurzu nakonfigurované rozbočovači nové IoT Azure portálu a vytvoří zařízení identit v registru identity centrální. K povolení aplikace simulovaný zařízení reagovat metody vyvolané cloudu použít tuto identitu zařízení. Vytvořili jste také aplikace, která vyvolá na zařízení a zobrazí odpovědi ze zařízení. 

Pokračujte začínáte pracovat s IoT rozbočovači a prozkoumání ostatních případech IoT naleznete v tématu:

- [Začínáme s IoT centrální]
- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Informace o prodloužení svého IoT řešení a plánovali metoda volá na několika zařízeních, podívejte se na [plán a vysílání úlohy] [ lnk-tutorial-jobs] kurz.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Odesílání zpráv cloudu zařízení s IoT centrální]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s IoT centrální]: iot-hub-node-node-getstarted.md
