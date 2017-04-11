<properties
   pageTitle="Připojte zařízení pomocí C na mbed | Microsoft Azure"
   description="Popisuje, jak připojte zařízení Azure IoT sadu automaticky předem nakonfigurovaná vzdálené sledování řešení pomocí aplikace napsané v jazyce C v mbed zařízení."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Připojte zařízení vzdálené sledování předkonfigurované řešení (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Vytvoření a spuštění řešení ukázkové C

Kroky pro připojení [Mbed s podporou FRDM K64F Freescale] popisují následující pokyny[ lnk-mbed-home] zařízení vzdálené sledování řešení.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Připojte zařízení mbed do počítače síť a plochy

1. Připojení k síti prostřednictvím ethernetového kabelu mbed zařízení. Tento krok je nutné, protože ukázková aplikace vyžadují připojení k Internetu.

2. Přečtěte si článek [Začínáme s mbed] [ lnk-mbed-getstarted] připojení zařízení mbed do počítače.

3. Pokud počítače se systémem Windows, přečtěte si článek [Konfigurace počítače] [ lnk-mbed-pcconnect] můžete nakonfigurovat sériový port přístup k mbed zařízení.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Vytvořit projekt mbed a importovat ukázkový kód

1. Ve webovém prohlížeči přejděte na [Web pro vývojáře](https://developer.mbed.org/)mbed.org. Pokud ještě nemáte, podívejte se na volbu Vytvořit účet (je to zadarmo). V opačném přihlaste pomocí svých přihlašovacích údajů účtu. V pravém horním rohu stránky klepněte na **kompilátoru** . Tato akce přináší rozhraní *pracovního prostoru* .

2. Ujistěte se, že hardwarové platformy, které používáte se zobrazí v pravém horním rohu okna nebo klikněte na ikonu v pravém rohu vyberte svoji platformu hardwaru.

3. V hlavní nabídce klikněte na **importovat** . Klikněte na **Kliknutím sem** můžete importovat z adresy URL odkazu vedle logo mbed glóbusu.

    ![][6]

4. V okně místní zadejte odkaz pro https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ kód vzorku a pak klikněte na **importovat**.

    ![][7]

5. Zobrazí se v okně kompilátoru mbed, že importu tohoto projektu naimportuje také různých knihoven. Některé jsou k dispozici a zachovány týmem Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/) [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), zatímco ostatní jsou dostupné v katalogu knihoven mbed knihovny třetích stran.

    ![][8]

6. Otevřete soubor remote_monitoring\remote_monitoring.c a vyhledejte následující kód v souboru:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Nahradit [Id zařízení] a [klíč zařízení], s daty zařízení povolit ukázkový program pro připojení k centrální IoT. Použití centrální Hostname IoT nahrazení [název IoTHub] a [IoTHub příponu, tedy azure devices.net] zástupné symboly. Pokud vaše Hostname centrální IoT **contoso.azure devices.net**, **contoso** je například **hubName** a všechno, co po **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Projděte si kód

Pokud vás zajímají fungování program, tato část popisuje některé klíčové části ukázek kódu. Pokud chcete spustit kód, pokračujte [Vytvoření a spuštění programu](#buildandrun).

#### <a name="defining-the-model"></a>Definování modelu

V tomto příkladu [serializer] [ lnk-serializer] knihovny tak, aby definovat modelu, který určuje zprávy zařízení k rozbočovači IoT odesílat a přijímat z centrální IoT. V tomto příkladu definuje oboru **Contoso** **termostatu** modelu, který určuje telemetrickými daty **teploty**, **ExternalTemperature**a **vlhkosti** spolu s metadat například id zařízení, vlastnosti zařízení a příkazy, které odpovídá zařízení:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Související s definice modelu jsou definice **SetTemperature** a **SetHumidity** příkazů, které odpovídá zařízení:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Připojení modelu do knihovny

Funkce **sendMessage** a **IoTHubMessage** se často používaný kód pro posílání telemetrie ze zařízení a připojení zprávy z centrální IoT rutinám příkaz.

#### <a name="the-remotemonitoringrun-function"></a>Funkce remote_monitoring_run

**Hlavní** funkce programu vyvolá funkci **remote_monitoring_run** při spuštění aplikace spustit na zařízení chování jako centrální IoT zařízení klienta. Tato funkce **remote_monitoring_run** se skládá převážně párů vnořených funkcí:

- **platformu\_inicializace** a **platformu\_deinit** operací specifické pro platformu inicializace a vypnutí.
- **Převodník do sériového tvaru\_inicializace** a **serializer\_deinit** inicializace a zrušte inicializace serializer knihovny.
- **IoTHubClient\_vytvořit** a **IoTHubClient\_Destroy** vytvořit ty klienta, **iotHubClientHandle**, pomocí přihlašovacích údajů zařízení připojit se k centrální IoT.

V části hlavní funkce **remote_monitoring_run** program provede tyto operace pomocí úchytu **iotHubClientHandle** :

- Vytváří instanci modelu termostatu Contoso a nastaví zpětná zprávy pro dva příkazy.
- Odešle informace o zařízení, včetně příkazy, které podporuje k rozbočovače IoT serializer knihovnu. Rozbočovač obdrží tuto zprávu, změní se stav zařízení na řídicím panelu z hodnoty **pole Čekání na** **spuštění**.
- Spustí **během** smyčky, která odešle teplota, externí teploty a vlhkost hodnoty IoT centrální za sekundu.

Odkaz tady je ukázka **DeviceInfo** Komu IoT centrální při spuštění:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Odkaz tady je ukázka **Telemetrie** zprávy poslané na IoT centrální:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Odkaz tady je ukázka, který **příkaz** dostali z centrální IoT:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Vytvoření a spuštění programu

1. Klikněte na tlačítko **kompilaci** vytvářet program. Bezpečné můžete ignorovat všechna upozornění, ale pokud sestavení generuje chyby, vyřešit pokračovat.

2. Pokud sestavení není úspěšné, na webu překladač mbed vytvoří soubor Bin s názvem projektu a stáhne do místního počítače. Zkopírujte soubor Bin zařízení. Uložení souboru Bin zařízení způsobí, že zařízení restartujte a spuštění programu obsažené v Bin. Kdykoli můžete ručně restartovat program stisknutím tlačítka Obnovit na zařízení mbed.

3. Připojení k zařízení pomocí SSH klientská aplikace, například nátěrové. Můžete zjistit sériové port, který používá zařízení s kontrolou správce zařízení Windows.

    ![][11]

4. V nátěrové klikněte na typ **sériové** připojení. Obvykle připojení zařízení na 9600 přenosová, takže zadejte do pole **rychlost** 9600. Klepněte na tlačítko **Otevřít**.

5. Spuštění programu spuštění. Možná bude potřeba obnovit tabuli (stisknutím kláves CTRL + konec nebo tlačítko Obnovit na vývěsku) Pokud se program spustit automaticky při připojení.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
