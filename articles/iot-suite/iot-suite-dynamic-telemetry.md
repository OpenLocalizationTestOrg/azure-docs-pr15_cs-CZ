<properties
    pageTitle="Použití dynamického telemetrie | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak používat dynamické telemetrie s vzdálené monitorovací předkonfigurované řešení."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Použití dynamického telemetrie s vzdálené monitorovací předkonfigurované řešení

## <a name="introduction"></a>Úvod

Dynamické telemetrie umožňuje vizualizovat všechny telemetrie posílat vzdálené monitorovací předkonfigurované řešení. Simulovaný zařízení, která nasazení s předem řešení odeslat teploty a vlhkosti telemetrie, které můžete vizualizace na řídicím panelu. Pokud přizpůsobení existujícího simulovaný zařízení, vytvářet nové simulovaný zařízení nebo připojit fyzických zařízení předkonfigurované řešení můžete poslat hodnotám telemetrie například externí teplota, ot nebo rychlost větru. Můžete pak vizualizace tento další telemetrie na řídicím panelu.

Tento kurz používá jednoduché simulovaný zařízení Node.js, který můžete snadno upravit experiment s dynamické telemetrie.

Tento kurz budete potřebovat:

- Aktivní Azure předplatné. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure][lnk_free_trial].
- [Node.js] [ lnk-node] verze 0.12.x nebo novější.

Tento kurz v operačním systému, například Windows nebo Linux, kde můžete nainstalovat Node.js.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Konfigurace Node.js simulovaný zařízení

1. Na vzdálené sledování řídicím panelu klikněte na tlačítko **+ Přidat zařízení** a pak přidejte vlastní zařízení. Poznamenejte si rozbočovače IoT hostname, id zařízení a klíče zařízení. Musíte je dál v tomto kurzu při přípravě remote_monitoring.js zařízení klientské aplikaci.

2. Zajistit, aby tato verze Node.js 0.12.x nebo později nainstalovaný v počítači vývoj. Spuštění `node --version` příkazového řádku nebo v prostředí Kontrola verze. Informace o používání odpovědi balíčku nainstalovat Node.js Linux najdete v tématu [Instalace Node.js přes Správce balíčků][node-linux].

3. Při instalaci Node.js klonovat nejnovější verzi [azure iot SDK] [ lnk-github-repo] úložiště do počítače vývoj. Vždy používejte větvi **předlohy** v nejnovější verzi knihoven a ukázky.

4. V místní kopii [azure iot SDK] [ lnk-github-repo] úložiště, kopírování následujících dvou souborů ve složce uzel/zařízení nebo vzorků na vyprázdnit složku v počítači vývoj:

  - Packages.JSON
  - remote_monitoring.js

5. Otevřete soubor remote_monitoring.js a hledejte následující proměnné definice:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Nahraďte **[IoT Centrum zařízení připojovací řetězec]** zařízení připojovací řetězec. Použití hodnoty pro centrální IoT hostname, id zařízení a klíče zařízení, která vyrobila poznamenat v kroku 1. Připojovací řetězec zařízení má v tomto formátu:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Pokud IoT centrální hostname je **contoso** a zařízení id je **mydevice**, připojovací řetězec vypadá takto:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Uložte soubor. Spusťte následující příkazy v prostředí nebo příkazového řádku do složky, která obsahuje tyto soubory nainstalovat potřebné balíčků a znovu spusťte ukázková aplikace:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Sledujte dynamické telemetrie v akci

Řídicí panel zobrazí teploty a vlhkosti telemetrie z existujících simulovaný zařízeních:

![Výchozího řídicího panelu][image1]

Pokud vyberete Node.js simulovaný zařízení, které jste spustili v předchozí části, se zobrazí teploty, vlhkost a externí teploty telemetrie:

![Přidání externích teplotní do řídicího panelu][image2]

Vzdálené monitorovací řešení automaticky rozpozná typ telemetrie další externí teploty a přidá ji do grafu na řídicím panelu.

## <a name="add-a-telemetry-type"></a>Přidání typu telemetrie

Dalším krokem je nahrazení telemetrie generovaných Node.js simulovaný zařízení s novou sadu hodnot:

1. Vypnout simulovaný zařízení Node.js zadáním **Kombinaci kláves Ctrl + C** do příkazového řádku nebo prostředí.

2. V souboru remote_monitoring.js zobrazí se základní datových hodnot pro existující teplotu, vlhkost a externí teploty telemetrie. Přidání základních dat hodnotu **ot** následujícím způsobem:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Simulovaný zařízení Node.js používá funkci **generateRandomIncrement** v souboru remote_monitoring.js přidáte náhodné přírůstek základní datové hodnoty. Náhodně hodnotu **ot** přidáním řádek kódu po existující randomizations:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Přidejte novou hodnotu ot JSON datové, které zařízení odešle IoT centrální:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Spusťte simulovaný zařízení Node.js pomocí následujícího příkazu:

    ```
    node remote_monitoring.js
    ```

6. Sledujte nové ot telemetrie typ, která se zobrazí v grafu v řídicím panelu:

![Přidání ot do řídicího panelu][image3]

> [AZURE.NOTE] Budete muset zakázání a povolení Node.js zařízení na stránce **zařízení** na řídicím panelu změny se projeví okamžitě.

## <a name="customize-the-dashboard-display"></a>Přizpůsobení zobrazení řídicího panelu

**Informace o zařízení** zprávy můžete zahrnout metadata telemetrie odeslané zařízení k rozbočovači IoT. Tato metadata můžete určit, které zařízení odešle typy telemetrie. Upravte hodnotu **deviceMetaData** v souboru remote_monitoring.js zahrnout definici **Telemetrie** sledovat definici **Příkazy** . Následující fragment kódu znázorňuje definici **Příkazy** (je potřeba přidat `,` po definici **Příkazy** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Vzdálené sledování řešení používá porovnávání a umožňuje porovnávat definice metadat s daty v telemetrie proudu.

Přidání definici **Telemetrie** uvedeno v předchozím fragment kódu nezmění chování řídicího panelu. Metadata však mohou také obsahovat atribut **DisplayName** přizpůsobit zobrazení na řídicím panelu. Aktualizujte definici metadat **Telemetrie** uvedeno v následující ukázce:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Následující obrázek ukazuje, jak tuto změnu upravuje legendě grafu na řídicím panelu:

![Přizpůsobení legendy grafu][image4]

> [AZURE.NOTE] Budete muset zakázání a povolení Node.js zařízení na stránce **zařízení** na řídicím panelu změny se projeví okamžitě.

## <a name="filter-the-telemetry-types"></a>Filtrování typy telemetrie

Ve výchozím nastavení zobrazuje grafu na řídicím panelu všech datových řad v telemetrie proudu. **Informace o zařízení** metadat umožňuje potlačí zobrazení konkrétních telemetrie typů v diagramu. 

Chcete-li zobrazit pouze teploty a vlhkosti telemetrie grafu, vynechte **ExternalTemperature** z metadat **Telemetrie** **Informace o zařízení** následujícím způsobem:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Teplotní venkovní** už zobrazí graf:

![Filtrování telemetrie na řídicím panelu][image5]

Tato změna ovlivní pouze zobrazení grafu. Datové hodnoty **ExternalTemperature** se pořád ukládají a k dispozici pro zpracování back-end.

> [AZURE.NOTE] Budete muset zakázání a povolení Node.js zařízení na stránce **zařízení** na řídicím panelu změny se projeví okamžitě.

## <a name="handle-errors"></a>Obsloužení chyb

Tok dat v diagramu zobrazíte jeho **Typ** v metadatech **Informace o zařízení** musí odpovídat datový typ hodnoty telemetrie. Například pokud metadata Určuje, že je **Typ** dat vlhkost **int** **dvojité** nachází v toku telemetrie pak telemetrie vlhkost nezobrazuje v diagramu. Hodnoty **vlhkost** však pořád uložený a k dispozici pro zpracování back-end.

## <a name="next-steps"></a>Další kroky

Teď jste viděli, jak používat dynamické telemetrie, se můžou dozvědět víc o používání informací o zařízení předkonfigurované řešení: [zařízení informace metadata v vzdáleného sledování předkonfigurované řešení][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
