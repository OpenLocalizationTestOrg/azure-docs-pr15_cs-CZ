<properties
   pageTitle="Připojte zařízení pomocí C v systému Windows | Microsoft Azure"
   description="Popisuje, jak připojte zařízení Azure IoT sadu automaticky předem nakonfigurovaná vzdálené sledování řešení pomocí aplikace napsané v jazyce C v systému Windows."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Připojte zařízení vzdálené sledování předkonfigurované řešení (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Vytvoření řešení ukázkové C v systému Windows

Podle těchto kroků ukazují, jak pomocí aplikace Visual Studio k vytvoření klientské aplikaci napsané v jazyce C komunikující s řešením vzdálené sledování předem.

Vytvoření projektu starter ve Visual Studiu 2015 a přidejte NuGet balíčky IoT Centrum zařízení klienta:

1. Ve Visual Studiu 2015 vytvoření aplikace konzoly C pomocí šablony vizuální C++ **Konzoly aplikace typu Win32** . Zadejte název projektu **RMDevice**.

2. Na stránce **Nastavení aplikace** podle pokynů **Průvodce aplikace typu Win32**zkontrolujte, zda je vybrán **aplikace konzoly** a zrušte zaškrtnutí políčka **Předkompilovaná záhlaví** a **kontroly životního cyklu vývoje zabezpečení (SDL)**.

3. V **Okně Průzkumník**odstraňte soubory stdafx.h, targetver.h a stdafx.cpp.

4. V **Okně Průzkumník**přejmenujte soubor RMDevice.cpp RMDevice.c.

5. V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši na projektu **RMDevice** a klikněte na **Spravovat NuGet balíčků**. Klikněte na tlačítko **Procházet**, vyhledejte a nainstalujte následující balíčky NuGet do projektu:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši na projektu **RMDevice** a kliknutím na příkaz **Vlastnosti** otevřete dialogové okno **Vlastností stránky** projektu. Podrobnosti najdete v tématu [Nastavení vlastností projektu vizuální C++][lnk-c-project-properties]. 

7. Klikněte na složku **Propojovací program** a potom klikněte na stránce vlastností **vstupní** .

8. Přidání **crypt32.lib** vlastnost **Další závislosti** . Kliknutím na tlačítko **OK** a potom na **OK** znova uložte projekt nemovitostí s hodnotou.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Nastavit chování IoT Centrum zařízení

Knihoven klienta IoT centrální pomocí modelu můžete určit formát zprávy, které zařízení odešle IoT centrální a příkazy, které obdrží z centrální IoT.

1. Ve Visual Studiu otevřete soubor RMDevice.c. Nahraďte stávající `#include` příkazy pomocí následující kód:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Přidejte následující deklarace proměnných po `#include` příkazy. Nahrazení zástupného symbolu hodnot [Id zařízení] a [klíč zařízení] s hodnotami pro zařízení na vzdálené sledování řešení řídicím panelu. Umožňuje nahradit [název IoTHub] Hostname centrální IoT na řídicím panelu. Například pokud váš Hostname centrální IoT **contoso.azure devices.net**, nahraďte [webu název IoTHub] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Přidejte následující kód pro definování modelu, který umožňuje zařízení komunikovat s IoT centrální. Tento model Určuje, že zařízení odešle teplotu, externí teplotu, vlhkost a id zařízení jako telemetrie. Zařízení také rozešle metadat o zařízení IoT centrální, včetně jejich seznamu příkazy, které podporuje zařízení. Toto zařízení odpovídá příkazů **SetTemperature** a **SetHumidity**:

    ```
    // Define the Model
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

## <a name="implement-the-behavior-of-the-device"></a>Implementace chování zařízení

Teď přidejte kód implementující chování definováno v modelu.

1. Přidejte následující funkce, které provést po zaznamenání příkazy **SetTemperature** a **SetHumidity** z centrální IoT:

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

2. Přidejte následující funkce, která odešle zprávu IoT centrální:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Přidejte následující funkce, která zachytí nahoru knihovnu serializace v SDK:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Přidejte následující funkce připojení k centrální IoT, odesílání a přijímání zpráv a Odpojit z centra. Všimněte si, jak zařízení odešle metadata, včetně příkazy, které podporuje k rozbočovači IoT při připojení. Tento metadat umožňuje řešení aktualizovat stav zařízení na **systém** pro řídicí panel:

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
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

5. Následující kód pro vyvolání funkce **remote_monitoring_run** nahraďte **hlavní** funkce:

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Klikněte na **vytvořit** a potom **Sestavit řešení** k vytvoření aplikace zařízení.

7. V **Okně Průzkumník**klikněte pravým tlačítkem **RMDevice** projektu, klikněte na tlačítko **ladění**a klikněte na **spuštění nové instance** vzorku. Zobrazené v konzole zprávy jako aplikace umožňuje ukázkové telemetrie k rozbočovači IoT volání a přijímání příkazy.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx