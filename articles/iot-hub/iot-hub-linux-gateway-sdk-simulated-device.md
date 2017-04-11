<properties
    pageTitle="Simulovat zařízení s SDK brány IoT | Microsoft Azure"
    description="Azure návodu IoT brány SDK ilustrují odesílání telemetrie z simulovaný zařízení, které používá SDK brány IoT Azure pomocí Linux."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT brány SDK (verze beta) – odesílání zpráv zařízení do cloudu pomocí simulovaný zařízení, které používá Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Vytvoření a spuštění výběru

Než začnete, musíte:

- [Nastavení prostředí vývoj] [ lnk-setupdevbox] pro práci s SDK na Linux.
- [Vytvoření rozbočovači IoT] [ lnk-create-hub] předplatné Azure budete potřebovat název rozbočovače k dokončení tohoto postupu. Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- Přidání dvou zařízeních rozbočovače IoT a poznamenejte si jeho ID a zařízení klíče. Můžete používat [zařízení Průzkumník nebo iothub Průzkumník] [ lnk-explorer-tools] nástroj Přidat zařízení k rozbočovači IoT jste vytvořili v předchozím kroku a získat legendou.

Vytvoření výběru:

1. Otevřete prostředí.
2. Přejděte do kořenové složky v místní kopii **azure iot brány sdk** úložiště.
3. Spusťte skript **tools/build.sh** . Tento skript používá nástroj **cmake** a vytvořte složku s názvem **Vytvoření** v kořenové složce vaší místní kopie úložišti **azure iot brány sdk** generovat soubor pravidel. Skript potom vytvoří řešení a spustí testů.

> [AZURE.NOTE]  Pokaždé, když spustíte **build.sh** skript, odstranit a pak znovu vytvořit **Vytvoření** složky v kořenové složce vaší místní kopie **azure iot brány sdk** úložiště.

Spuštění výběru:

V textovém editoru otevřete soubor **samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** v místní kopii **azure iot brány sdk** úložiště. Tento soubor konfiguruje moduly v ukázkové brány:

- Modul **IoTHub** připojuje k centrální IoT. Musíte nastavit odeslání dat do rozbočovače IoT. Konkrétně nastavte hodnotu **IoTHubName** název rozbočovače IoT a nastavte hodnotu **IoTHubSuffix** na **azure devices.net**. Nastavte hodnotu **Transport** na jednu z: "HTTP", "AMQP" nebo "MQTT". Všimněte si, že v současnosti pouze "HTTP" bude sdílet jednu TCP připojení pro všechny zprávy zařízení. Pokud má argument hodnotu "AMQP" nebo "MQTT", brány udržovat samostatná připojení TCP IoT centrální u každého zařízení.
- **Mapování** modulu mapování adresy MAC simulovaný zařízení na vaše ID IoT Centrum zařízení. Ujistěte se, že se shodují hodnoty **deviceId** ID dvě zařízení, které jste přidali do rozbočovače IoT a, aby obsahoval **deviceKey** hodnoty klíče dvě zařízení.
- Moduly **BLE1** a **BLE2** jsou simulovaný zařízení. Všimněte si, jak jejich adres MAC se shodují s v modulu **mapování** .
- Modul **protokolování** zaznamenává aktivitě brány do souboru.
- **Modul cestu** hodnoty ukázáno v následujícím příkladu se předpokládá, že se spustí výběru z kořenové složky místní kopii **azure iot brány sdk** úložiště.
- Pole **odkazy** na konci souboru JSON ke kterému je připojen moduly **BLE1** a **BLE2** modulu **mapování** a modulu **mapování** modulu **IoTHub** . Také zaručuje, že všechny zprávy jsou zaznamenány modulem **protokolování** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

Uložte provedené změny, které jste provedli konfiguračního souboru.

Spuštění výběru:

1. V prostředí přejděte do kořenové složky v místní kopii **azure iot brány sdk** úložiště.
2. Spusťte tento příkaz:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. Můžete používat [zařízení Průzkumník nebo iothub Průzkumník] [ lnk-explorer-tools] nástroje ke sledování zpráv, které IoT centrální přijímání brány.

## <a name="next-steps"></a>Další kroky

Pokud chcete získat pokročilejší Principy SDK IoT brány a vyzkoušet některé příklady, navštěvujte blog o následující výukové programy pro vývojáře a prostředky:

- [Odesílání zpráv zařízení do cloudu z skutečné zařízení s SDK IoT brány][lnk-physical-device]
- [Azure brány IoT SDK][lnk-gateway-sdk]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Zabezpečené IoT řešení od základu nahoru][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md