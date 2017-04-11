<properties
    pageTitle="Použití reálnou zařízení s SDK brány IoT | Microsoft Azure"
    description="Azure IoT brány SDK návodu pomocí zařízení Texas nástrojů SensorTag odeslání dat do IoT centrální pomocí brány na modul výpočet Edison Intel"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT brány SDK (verze beta) – odesílání zpráv zařízení do cloudu pomocí reálnou zařízení, které používá Linux

Tento návod [Bluetooth zhoršeným energie ukázkové] [ lnk-ble-samplecode] se dozvíte, jak používat v [Azure IoT brány SDK] [ lnk-sdk] k přesměrování zařízení do cloudu telemetrie k rozbočovači IoT z fyzické zařízení a směrování příkazy z centrální IoT fyzické zařízení.

Tento návod obsahuje:

* **Architektura**: důležité architektonické informace o výběru zhoršeným energie Bluetooth.

* **Vytvoření a spuštění**: kroky potřebné k vytvoření a spuštění vzorku.

## <a name="architecture"></a>Architektura

Návod ukazuje, jak vytvořit a spustit brány IoT Intel Edison výpočet modul protokolu, které se spouští Linux. Brána je vytvořené pomocí SDK IoT brány. Příklad používá zařízení Texas nástrojů SensorTag Bluetooth minimum energie (tabulky) shromažďovat teplotní data.

Když spustíte brány je:

- Připojí k SensorTag zařízení, které používá protokol Bluetooth minimum energie (tabulky).
- Připojí k centrální IoT pomocí protokolu HTTP.
- Předá telemetrie z zařízení SensorTag IoT centrální.
- Přesměrovává příkazy z centrální IoT SensorTag zařízení.

Brány obsahuje následující moduly:

- *Modul tabulky* , které rozhraní se zařízením tabulku pro příjem teplotní data ze zařízení a pošlete příkazy k zařízení.
- *Tabulku Obláčkem, která zařízení modul* , který překládá zprávy JSON pocházejících z cloudu do tabulky pokyny pro *modul tabulku*.
- *Modul protokolování* , které protokoly všech zpráv brány.
- *Mapování identit modul* , který překládá adresy MAC zařízení tabulku a Azure IoT Centrum zařízení identity.
- *IoT centrální modul* , odešlou se telemetrickými daty rozbočovači IoT volání a přijímání zařízení příkazy rozbočovači IoT.
- *Modul tiskárny tabulky* , které interpretovat telemetrie z tabulky zařízení a vytiskne formátovaných dat ke konzole povolit řešení potíží a ladění.

### <a name="how-data-flows-through-the-gateway"></a>Jak dat prochází brány

Následující Blokový diagram znázorňuje toku kanálu telemetrie odesílání dat:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Kroky, které položku telemetrie trvá cestování z tabulky zařízení IoT centrální jsou:

1. Zařízení tabulku vygeneruje výběru teploty a odešle přes Bluetooth modulu tabulku v brány.
2. Modul tabulku obdrží vzorku a její publikování zprostředkovatele spolu s adresou MAC zařízení.
3. Modul mapování identity vyzvedne tuto zprávu a používá vnitřní tabulku překladu adresu MAC zařízení do identitu IoT Centrum zařízení (ID zařízení a klíče zařízení). Potom publikuje novou zprávu obsahující vzorová data teplotu, adresu MAC zařízení, ID zařízení a klávesu zařízení.
4. Modul IoT centrální obdrží toto nové zprávy (generované modulu mapování identity) a publikuje k rozbočovači IoT.
5. Modulu protokolování zaznamenává všechny zprávy z agenta do souboru na disku.

Následující Blokový diagram znázorňuje kanálu toku dat příkazů zařízení:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Modul IoT centrální pravidelně zjišťuje centru IoT pro nové zprávy příkaz.
2. Novou zprávu příkaz přijaté modulu IoT centrální publikuje ho zprostředkovatele.
3. Modul mapování identity vyzvedne command zpráva a jak používá vnitřní tabulku překladu ID zařízení IoT Centrum zařízení MAC adresu. Potom publikuje novou zprávu, která obsahuje adresa MAC cílového zařízení v mapování vlastností zprávy.
4. Tabulku Obláčkem, která zařízení modul vyzvedne tuto zprávu a převede ho na správné tabulku instrukce pro modul tabulku. Potom publikuje novou zprávu.
5. Modul tabulku vyzvedne tuto zprávu a provede instrukční vstupu a výstupu tak, že komunikaci se zařízením tabulku.
6. Modulu protokolování zaznamenává všechny zprávy z agenta do souboru na disku.

## <a name="prepare-your-hardware"></a>Příprava hardwaru

Tento kurz se předpokládá, že používáte [Texas nástrojů SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) zařízení připojené k vývěsce Intel Edison.

### <a name="set-up-the-edison-board"></a>Nastavení Edison vývěsky

Než začnete, nezapomeňte, že zařízení Edison připojit k bezdrátové sítě. Nastavit zařízení Edison, budete muset připojit k počítači Host (hostitel). Procesor Intel obsahuje úvodní příručky pro následující operační systémy:

- [Začínáme s vývěsky Intel Edison vývoj ve Windows 64-bit][lnk-setup-win64].
- [Začínáme s vývěsky Intel Edison vývoj ve Windows 32bitová verze][lnk-setup-win32].
- [Začínáme s vývěsky vývoj Edison Intel v systému Mac OS X][lnk-setup-osx].
- [Začínáme s Intel® Edison vývěsce na Linux][lnk-setup-linux].

Nastavení zařízení Edison a seznamte se s ním, by měl proveďte všechny kroky v těchto "Začínáme" články s výjimkou poslední krok "Zvolte integrovaném vývojovém prostředí", není pro aktuální kurz. Na konci procesu Edison nastavení máte k dispozici:

- Blikal Edison s nejnovější firmware.
- Sériové připojení ze svého hostitele Edison.
- Spusťte skript **configure_edison** nastavení hesla a povolení Wi-Fi na vaše Edison.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Povolit připojení k zařízení SensorTag z Edison vývěsky

Před spuštěním vzorku, budete muset ověřte, že Edison vývěsky můžete připojit ke SensorTag zařízení.

Nejdřív budete muset ověřte, že vaše Edison můžete připojit ke SensorTag zařízení.

1. Odblokování bluetooth v Edison a ověřte, zda je číslo verze **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Provedení příkazu **bluetoothctl** . Měli byste být prostředí interaktivní bluetooth. 

3. Zadejte příkaz **zapněte** na mocninu nahoru zařízení bluetooth. Měli byste vidět výstup:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Zůstaňte ještě v prostředí interaktivní bluetooth zadejte příkaz **prohledávání při** prohledávání zařízení bluetooth. Měli byste vidět výstup:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Zpřístupnění zařízení SensorTag konfigurací stisknutím tlačítka small (zelené LED bliká). Edison by měly objevit SensorTag zařízení:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    V tomto příkladu můžete vidět, že adresa MAC zařízení SensorTag **A0:E6:F8:B5:F6:00**.

6. Vypnutí kontroly zadáním příkazu **skenování vypnout** .
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Připojení k zařízení SensorTag pomocí adresu MAC zadáním **připojení \<adresu MAC >**. Všimněte si, že je zkrácen výstupu vzorku:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Poznámka: Můžete vytvořit seznam vlastností GATT zařízení znovu pomocí příkazu **atributy seznamu** .

8. Můžete nyní odpojení od zařízení pomocí příkazu **Odpojit** a ukončete z prostředí bluetooth pomocí příkazu **Ukončete** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Teď jste připraveni spustit ukázkové tabulky brány na zařízení s Edison.

## <a name="run-the-ble-gateway-sample"></a>Spuštění ukázkové tabulky brány

Ukázka tabulku na vaše Edison spustíte je potřeba provést tři úlohy:

- Konfigurace dvě ukázkové zařízení v centrální IoT.
- Vytvoření brány SDK IoT na zařízení s Edison.
- Konfigurace a spusťte ukázkové tabulky na zařízení s Edison.

Při psaní SDK brány IoT podporuje pouze bran, které používají tabulku moduly na Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurace dvě ukázkové zařízení v centrální IoT

- [Vytvoření rozbočovači IoT] [ lnk-create-hub] předplatné Azure byste potřebovali název rozbočovače k dokončení tohoto postupu. Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- Přidání jednoho zařízení s názvem **SensorTag_01** k rozbočovači IoT a poznamenejte si jeho id a zařízení klíče. Můžete používat [zařízení Průzkumník nebo iothub Průzkumník] [ lnk-explorer-tools] nástroje můžete přidat toto zařízení k rozbočovači IoT jste vytvořili v předchozím kroku a obnovit jeho klíče. Toto zařízení namapuje zařízení SensorTag při konfiguraci brány.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Vytvoření brány SDK IoT na zařízení s Edison

Verze **Libovolná** na Edsion nepodporuje submodules. Stáhnout zdroji úplné IoT SDK brány Edison máte dvě možnosti:

- Možnost #1: Klonovat [Azure IoT brány SDK] [ lnk-sdk] úložiště na vaše Edison a ručně klonovat úložiště pro každou submodule.
- Možnost #2: Klonovat [Azure IoT brány SDK] [ lnk-sdk] úložiště na počítači kde **Libovolná** podporuje submodules a zkopírujte úplnou úložiště s submodules do svého Edison.

Pokud vyberete možnost #2, použijte následující příkazy **Libovolná** klonovat SDK IoT brány a jeho submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Celý místní úložiště by pak zip do jednoho archiv před kopírováním Edison. Můžete použít nástroj například **pscp** , který je součástí **nátěrové** zkopírovat archiv Edison. Příklad:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Jakmile úplnou kopii IoT brány SDK úložiště na vaše Edison, je možné vytvářet pomocí následujícího příkazu ze složky, která obsahuje SDK:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Konfigurace a spustit na zařízení s Edison ukázkové tabulky

Bootstrap a spouštění vzorku, musíte nakonfigurovat jednotlivých modulech, která je účastníkem v brány. Toto nastavení je k dispozici v souboru JSON a musíte nakonfigurovat názevprojektu pět účastní. Je ukázkový soubor JSON uvedenou v úložišti s názvem **gateway_sample.json** , které lze použít jako výchozí bod pro vytváření konfigurační soubor. Tento soubor je ve složce **Ukázky/ble_gateway_hl/src** v místní kopii IoT brány SDK úložiště.

Následující části popisují, jak můžete upravit tento soubor konfigurace pro ukázkové tabulky a předpokládá, že je v úložišti IoT brány SDK **/home/root/azure-iot-gateway-sdk /** složky na svém zařízení Edison. Pokud úložiště je kdekoli jinde, byste měli upravit cesty příslušným způsobem:

#### <a name="logger-configuration"></a>Konfigurace protokolování

Za předpokladu, že úložišti brány se nachází ve složce **/home/root/azure-iot-gateway-sdk /**, nakonfigurovat modul protokolování následujícím způsobem:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Konfigurace modulu tabulku

Ukázka konfigurace pro zařízení tabulku předpokládá Texas nástrojů SensorTag zařízení. Libovolné standardní tabulku zařízení, které mohou pracovat, jako by měla práci GATT okrajové ale muset aktualizovat GATT charakteristické ID a data (zapsat pokynů). Přidání adresy MAC SensorTag zařízení: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>Modul IoT centrální

Přidejte název rozbočovače IoT. Přípona hodnotu obvykle **azure devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfigurace modulu mapování identity

Přidání adresy MAC SensorTag zařízení a zařízení Id a klíč **SensorTag_01** zařízení, které jste přidali do rozbočovače IoT:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfigurace modul tabulku tiskárny

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Konfigurace směrování

Následující konfigurace zajišťuje takto:
- Modul **protokolování** obdrží a zaznamenávat všech zpráv.
- Modul **SensorTag** odesílat zprávy **mapování** a moduly **Tabulku tiskárny** .
- Modul **mapování** odesílat zprávy modulu **IoTHub** nechat zasílat rozbočovače IoT.
- Modul **IoTHub** odesílat zprávy zpět modulu **mapování** .
- Modulu **mapování** odešle zpět do modulu **SensorTag** zprávy.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Spuštění vzorku spustíte **ble_gateway_hl** binární předávání cestu k souboru konfigurace JSON. Pokud jste použili **gateway_sample.json** soubor, na příkaz Spustit vypadat takto:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Budete muset stiskněte tlačítko malé na zařízení SensorTag zpřístupnění konfigurací před spuštěním vzorku.

Když spustíte vzorku, můžete [zařízení Průzkumník nebo iothub Průzkumník] [ lnk-explorer-tools] nástroje ke sledování zpráv brány přepošle z SensorTag zařízení.

## <a name="send-cloud-to-device-messages"></a>Odeslání zprávy cloudu zařízení

Modul tabulku taky podporuje odesílání pokynů z centra IoT Azure zařízení. [Azure IoT Centrum zařízení Průzkumníka](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) nebo [IoT centrální Průzkumníka](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) slouží k odesílání zpráv JSON, které modul brány tabulku předává zařízení tabulku. Například pokud používáte zařízení Texas nástrojů SensorTag pak můžete poslat tyto zprávy JSON zařízení od IoT rozbočovače.

- Všechny LED a odpověď obnovit (vypnout)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Konfigurace vstupu a výstupu jako "vzdáleného"

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Zapnutí červený Indikátor

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Zapnutí zelené LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Zapnutí a odpověď

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Výchozí chování pro zařízení pomocí protokolu HTTP se připojit k centrální IoT je ke kontrole každých 25 minut pro příkaz Nový. Proto odeslat několik různých příkazů potřebnosti až 25 minut zařízení pro příjem jednotlivé příkazy.

> [AZURE.NOTE] Brány také kontroluje nové příkazy při každém spuštění, můžete jej zpracuje příkazu zastavením a od brány vynutit.

## <a name="next-steps"></a>Další kroky

Pokud chcete získat pokročilejší Principy SDK IoT brány a vyzkoušet některé příklady, navštěvujte blog o následující výukové programy pro vývojáře a prostředky:

- [Azure brány IoT SDK][lnk-sdk]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
