<properties
 pageTitle="Začít používat správu zařízení | Microsoft Azure"
 description="Tento kurz se dozvíte, jak začít používat správu zařízení v centrální IoT Azure"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Kurz: Začněte používat správu zařízení (verze preview)

## <a name="introduction"></a>Úvod
IoT cloudové aplikace můžete použít základní prvky v centrální IoT Azure, hlavně dvojitých zařízení a přímý metody vzdáleně spustit a sledovat akce správy zařízení na zařízeních.  Tento článek obsahuje pokyny a kód pro aplikace IoT cloudu a zařízení fungování společně zahájit sledovat a restartujte počítač vzdáleného zařízení pomocí IoT centrální.

Vzdáleně začít sledovat a akce správy zařízení na vašich zařízeních z cloudového a back-end aplikace, použít základní Centrum IoT Azure například [zařízení dvojitých] [ lnk-devtwin] a [cloudu zařízení (C2D) metody][lnk-c2dmethod]. Tento kurz ukazuje, jak v aplikaci back-end a zařízení společně pracovat můžete zahájit a sledovat vzdálené zařízení restartujte počítač z centrální IoT.

Použijte metodu C2D zahájit akce správy zařízení (například restartování počítače, obnovení factory a aktualizaci firmwaru) pomocí funkčně back-end aplikace v cloudu. Zařízení je zodpovědný za:

- Zpracování požadavku metoda odesílaným z centrální IoT.
- Zahájení odpovídající zařízení určité akce na zařízení.
- Poskytování aktualizací stavu prostřednictvím dvojitých zařízení oznámeny IoT centrální vlastnosti.

Můžete do back-end aplikací v cloudu spouštění dotazů dvojitých zařízení vykazování průběhu akce správy zařízení.

Tento kurz se dozvíte, jak chcete:

- Pomocí portálu Azure můžete vytvořit rozbočovači IoT a vytvoření zařízení identit v centrální IoT.
- Vytvořte simulovaný zařízení, který má přímé metodu, což umožňuje restartujte počítač, nazývaný v cloudu.
- Vytvoření aplikace konzoly, která volá metodu restartujte počítač přímo na simulovaný zařízení přes Centrum IoT.

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

**dmpatterns_getstarted_device.js**, který se připojuje k rozbočovače IoT s zařízení identit dříve vytvořili, obdrží přímá metoda restartujte počítač, napodobuje fyzické restartujte počítač a sestavách dobu poslední restartujte počítač.

**dmpatterns_getstarted_service.js**, který volá přímé metodu na simulovaný zařízení, zobrazí odpovědi a zobrazí dvojitých aktualizované zařízení vykázaného vlastnosti.

Tento kurz, je potřeba k provedení těchto věcí:

Verze Node.js 0.12.x nebo novější <br/>  [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Node.js pro účely tohoto návodu Windows nebo Linux.

Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části Vytvoření aplikace konzoly Node.js reagující na přímé metodu nazvanou podle cloudu, který spustí simulovaný zařízení restartujte počítač a používá dvojitých zařízení vykázaného vlastností, které lze povolit zařízení dvojitých dotazy k identifikaci zařízení a kdy se naposledy restartování počítače.

1. Vytvořte novou prázdnou složku s názvem **manageddevice**.  Ve složce **manageddevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **manageddevice** , spusťte tento příkaz a nainstalujte **azure-iot-device@dtpreview** zařízení SDK balíčku a **azure-iot-device-mqtt@dtpreview** balíčku:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **dmpatterns_getstarted_device.js** ve složce **manageddevice** .

4. Přidejte následující "vyžadují" příkazy na začátku **dmpatterns_getstarted_device.js** souboru:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Přidání proměnné **connectionString** , můžete vytvořit klienta zařízení.  Nahraďte připojovací řetězec zařízení připojovací řetězec.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Přidejte následující funkce implementovat metodu přímo na zařízení

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Otevřete připojení k centrální IoT a spuštění nástroje pro sledování přímá metoda:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Uložte a zavřete soubor **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Aktivace vzdálený restart na zařízení přímé metodou 

V této části Vytvoření aplikace konzoly Node.js, který zahájí vzdálený restart na zařízení přímé metodou a používá zařízení dvojitých dotazy k nalezení posledního restartujte počítač pro toto zařízení.

1. Vytvořte novou prázdnou složku s názvem **triggerrebootondevice**.  Ve složce **triggerrebootondevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **triggerrebootondevice** , spusťte tento příkaz nainstalovat **azure-iothub@dtpreview** zařízení SDK balíčku a **azure-iot-device-mqtt@dtpreview** balíčku:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. V textovém editoru, vytvoření nového souboru **dmpatterns_getstarted_service.js** ve složce **triggerrebootondevice** .

4. Přidejte následující "vyžadují" příkazy na začátku **dmpatterns_getstarted_service.js** souboru:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Přidejte následující deklarace proměnných a nahraďte zástupný symbol hodnoty:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Přidejte následující funkce pro vyvolání metody zařízení restartujte cílové zařízení:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Přidejte následující funkce do dotazu pro zařízení a najděte posledního restartování počítače:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Přidejte následující kód volání funkce, které bude spouštět přímý metoda restartujte počítač a dotaz naposledy restartováním počítače:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Uložte a zavřete soubor **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. V příkazového řádku ve složce **manageddevice** spusťte tento příkaz zahájíte naslouchají přímá metoda restartujte počítač.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. V příkazového řádku ve složce **triggerrebootondevice** spusťte tento příkaz spustí vzdálené restartujte počítač a dotaz pro zařízení dvojitých zobrazíte poslední restartujte čas.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Zobrazí se reagují metody přímé vytištěním, zprávy

## <a name="customize-and-extend-the-device-management-actions"></a>Přizpůsobení a rozšíření zařízení řízení akce

Řešení IoT můžete rozbalit definovaný sady vzorů správy zařízení nebo povolení vlastní vzorků použitím dvojitých zařízení a C2D metoda základní. Jiné akce správy zařízení příkladů factory obnovit aktualizaci firmwaru, aktualizace, správy power, Správa sítě a připojení a šifrování.

## <a name="device-maintenance-windows"></a>Údržba zařízení windows

Obvykle nakonfigurujete zařízení k provádění akcí v době, která slouží k minimalizaci vyrušování a výpadek služeb.  Údržba zařízení windows jsou běžně používaných vzorek definovat čas, kdy by měla zařízení aktualizovat jeho konfiguraci. Řešení back-end slouží k definování a aktivace zásady na vašem zařízení, který umožňuje údržby požadované vlastnosti dvojitých zařízení. Pokud zařízení obdrží zásad okno údržby, můžete nahlášeného vlastnost dvojitých zařízení Pokud chcete vykázat stav zásady. Aplikaci back-end pak můžete zařízení dvojitých dotazů potvrzovat dodržování předpisů zařízení a každého zásadu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste použili metodu přímý aktivovat vzdálený restart na zařízení používané zařízení dvojitých vykázaného vlastnosti vykazování posledního restartujte počítač ze zařízení a dotazovaném pro zařízení dvojitých zjistit posledního restartujte počítač zařízení z cloudu.

Pokračovat v začínáte pracovat s IoT centrální a vzorků správy zařízení například vzdáleného přes air aktualizaci firmwaru, najdete tady:

[Kurz: Postup aktualizaci firmwaru][lnk-fwupdate]

Informace o prodloužení svého IoT řešení a plánovali metoda volá na několika zařízeních, podívejte se na [plán a vysílání úlohy] [ lnk-tutorial-jobs] kurz.

Pokračujte začínáte pracovat s IoT centrální najdete v článku [Začínáme s SDK brány IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx