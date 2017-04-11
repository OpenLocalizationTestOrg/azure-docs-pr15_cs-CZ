<properties
   pageTitle="Připojte zařízení pomocí Node.js | Microsoft Azure"
   description="Popisuje, jak se připojit k Azure IoT sadu automaticky předem nakonfigurovaná vzdálené sledování řešení pomocí aplikace napsané v Node.js zařízení."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Připojte zařízení vzdálené sledování předkonfigurované řešení (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Vytvoření a spuštění node.js ukázkové řešení

1. Chcete-li klonovat úložiště GitHub *Microsoft Azure IoT SDK* a nainstalujte *Microsoft Azure IoT zařízení SDK Node.js* v prostředí plochy, postupujte [připravit vývojové prostředí] [ lnk-github-prepare] pokyny.

2. V místní kopii [azure iot SDK] [ lnk-github-repo] úložiště, kopírování následujících dvou souborů ve složce uzel/zařízení nebo vzorků do složky na vašem zařízení:

  - Packages.JSON
  - remote_monitoring.js

3. Otevřete soubor remote_monitoring.js a hledejte následující proměnnou:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Nahraďte **[IoT Centrum zařízení připojovací řetězec]** zařízení připojovací řetězec. Můžete najít hodnoty pro vaše IoT Centrum hostname, id zařízení a klíče zařízení v řídicím panelu vzdálené sledování řešení. Připojovací řetězec zařízení má v tomto formátu:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Pokud IoT centrální hostname je **contoso** a zařízení id je **mydevice**, připojovací řetězec vypadá takto:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Uložte soubor. Na příkazovém řádku do složky, která obsahuje tyto soubory nainstalovat potřebné balíčky a znovu spusťte aplikaci ukázkové spusťte následující příkazy:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md