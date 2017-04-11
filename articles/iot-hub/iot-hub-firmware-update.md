<properties
 pageTitle="Jak se to aktualizaci firmwaru | Microsoft Azure"
 description="Tento kurz se dozvíte, jak provádět aktualizaci firmwaru"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Kurz: Postup firmwaru aktualizace (verze preview)

## <a name="introduction"></a>Úvod
V [začít používat správu zařízení] [ lnk-dm-getstarted] kurzu jste viděli, jak používat [zařízení dvojitých] [ lnk-devtwin] a [cloudu zařízení (C2D) metody] [ lnk-c2dmethod] základní vzdáleně Restartujte zařízení. Tento kurz využívá stejnou základní Centrum IoT obsahuje pokyny a se dozvíte, jak se to aktualizaci firmwaru simulovaný začátku do konce.  Tento způsob se používá k výběru zařízení Intel Edison v aktualizaci provádění firmware.

Tento kurz se dozvíte, jak chcete:

- Vytvoření aplikace konzoly, volající metodu přímé firmwareUpdate na simulovaný zařízení přes Centrum IoT.
- Vytvoření simulovaný zařízení, které používá přímé způsob firmwareUpdate, které prochází vícestupňové proces, který čeká na stáhnout obrázek firmwaru, stáhne obrázek firmware a nakonec platí ář firmware obrázek.  Během spouštění každé fázi zařízení používá dvojitých vykázaného vlastnosti aktualizovat průběh.

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

**dmpatterns_fwupdate_service.js**, která volá metodu přímo na simulovaný zařízení, se zobrazí odpovědi a pravidelně vykázaného aplikace (každé 500ms) zobrazí dvojitých aktualizované zařízení vlastnosti.

**dmpatterns_fwupdate_device.js**, který se připojuje k rozbočovače IoT s identitě zařízení dříve vytvořili, obdrží přímý metodu firmwareUpdate, spustí více stavy procesem tak, aby napodobily včetně aktualizaci firmwaru: čeká na stažení obrázek, stahování nový obrázek a nakonec použití obrázku.


Tento kurz, je potřeba k provedení těchto věcí:

Verze Node.js 0.12.x nebo novější <br/>  [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Node.js pro účely tohoto návodu Windows nebo Linux.

Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

Postupujte podle [začít používat správu zařízení](iot-hub-device-management-get-started.md) článek vytváření rozbočovače IoT a získat připojovací řetězec.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části Vytvoření aplikace konzoly Node.js reagující na přímé metodu nazvanou podle cloudu, který spustí aktualizaci firmwaru simulovaný zařízení a používá dvojitých zařízení vykázaného vlastností, které lze povolit zařízení dvojitých dotazy k identifikaci zařízení a kdy se naposledy restartování počítače.

1. Vytvořte novou prázdnou složku s názvem **manageddevice**.  Ve složce **manageddevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **manageddevice** , spusťte tento příkaz a nainstalujte **azure-iot-device@dtpreview** zařízení SDK balíčku a **azure-iot-device-mqtt@dtpreview** balíčku:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **dmpatterns_fwupdate_device.js** ve složce **manageddevice** .

4. Přidejte následující "vyžadují" příkazy na začátku **dmpatterns_fwupdate_device.js** souboru:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Přidání proměnné **connectionString** , můžete vytvořit klienta zařízení.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Přidejte následující funkce, které se použijí k aktualizaci dvojitých vykázaného vlastnosti

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Přidejte následující funkce, které bude Simulovat stahování a použití firmware obrázku.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Přidejte následující funkce, které bude aktualizovat stav aktualizaci firmwaru prostřednictvím dvojitých nahlášený vlastnosti čekající na stáhnout.  Obvykle zařízení informováni dostupné aktualizace a správce definované zásad příčiny zařízení spusťte stahování a použití aktualizace.  Toto je místo, kam chcete spustit logickou povolit tuto zásadu.  Pro zjednodušení jste zpoždění 4 sekund jsme řízení ke stažení obrázek firmware. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Přidejte následující funkce, které bude aktualizovat stav aktualizaci firmwaru prostřednictvím dvojitých nahlášený vlastnosti stahování firmwaru obrázku.  Sleduje pomocí simulace stažení firmwaru a nakonec se aktualizuje stav aktualizace firmwaru informovat úspěch stahování nebo chyby.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Přidejte následující funkce, které bude aktualizovat stav aktualizaci firmwaru prostřednictvím dvojitých nahlášený vlastnosti použití obrázek firmware.  Sleduje pomocí simulace použití firmware obrázku a nakonec aktualizuje stav aktualizaci firmwaru sdělovat použít úspěch nebo selhání.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Přidejte následující functoin, které zpracovat metodu firmwareUpdate a zahájí proces aktualizace vícestupňové firmware.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Nakonec přidáte následující kód, který se připojuje k centrální IoT jako zařízení 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Aktivace aktualizaci firmwaru vzdálené na zařízení přímé metodou 

V této části vytvoříte aplikace konzoly Node.js, který zahájí aktualizaci firmwaru vzdálené na zařízení přímé metodou a používá zařízení dvojitých dotazů pravidelně získat stav aktualizace aktivní firmwaru na toto zařízení.


1. Vytvořte novou prázdnou složku s názvem **triggerfwupdateondevice**.  Ve složce **triggerfwupdateondevice** vytvoření souboru package.json pomocí následujícího příkazu příkazového řádku.  Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```
    
2. Vaše příkazového řádku ve složce **triggerfwupdateondevice** spusťte tento příkaz nainstalovat zabalit zařízení SDK **azure iothub** a **azure iot zařízení mqtt** balíčku:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. V textovém editoru, vytvoření nového souboru **dmpatterns_getstarted_service.js** ve složce **triggerfwupdateondevice** .

4. Přidejte následující "vyžadují" příkazy na začátku **dmpatterns_getstarted_service.js** souboru:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Přidejte následující deklarace proměnných a nahraďte zástupný symbol hodnoty:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Přidejte následující funkce Najít a zobrazení hodnotu firmwareUpdate vykázaného vlastnost.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Přidejte následující funkce pro vyvolání metody firmwareUpdate restartujte cílové zařízení:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Nakonec přidat následující funkce kódu pro začátek firmware aktualizovat pořadí a pravidelně zobrazení dvojitých hlášených vlastnosti:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Uložte a zavřete soubor **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. V příkazového řádku ve složce **manageddevice** spusťte tento příkaz zahájíte naslouchají přímá metoda restartujte počítač.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. V příkazového řádku ve složce **triggerfwupdateondevice** spusťte tento příkaz spustí vzdálené restartujte počítač a dotaz pro zařízení dvojitých zobrazíte poslední restartujte čas.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Zobrazí se reagují metody přímý tak, že vytištění zprávy

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste přímá metoda aktivace aktualizaci firmwaru vzdálené na zařízení a pravidelně používá uvedena dvojitých vlastnosti pochopit průběhu firmware aktualizovat proces.  

Informace o prodloužení svého IoT řešení a plánovali metoda volá na několika zařízeních, podívejte se na [plán a vysílání úlohy] [ lnk-tutorial-jobs] kurz.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
