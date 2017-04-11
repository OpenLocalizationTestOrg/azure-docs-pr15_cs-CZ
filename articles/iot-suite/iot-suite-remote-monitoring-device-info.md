<properties
 pageTitle="Metadata zařízení informace ve vzdáleném sledování řešení | Microsoft Azure"
 description="Popis Azure IoT předem vzdálené sledování řešení a jeho architektura."
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
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metadata zařízení informace ve vzdáleném sledování předkonfigurované řešení

Sadu Azure IoT vzdálené sledování předkonfigurované řešení ukazuje přístup pro správu zařízení metadata. Tento článek shrnuje přístup, který má toto řešení mohli bychom si vysvětlit:

- Jaká zařízení metadata ukládá řešení.
- Jak řešení spravuje metadata zařízení.

## <a name="context"></a>Kontext

Vzdálené sledování předkonfigurované řešení používá [Azure IoT centrální] [ lnk-iot-hub] povolit zařízení odeslání dat do cloudu. Rozbočovač IoT obsahuje [zařízení identity registru] [ lnk-identity-registry] k řízení přístupu k centrální IoT. Registru IoT Centrum zařízení identity je nezávislý vzdáleného sledování řešení *zařízení registru* obsahujícího zařízení informace metadata. Vzdálené sledování řešení používá [DocumentDB] [ lnk-docdb] databáze implementovat své zařízení registru pro ukládání informací metadat zařízení. [Microsoft Azure IoT odkaz architektura] [ lnk-ref-arch] popisuje role registru zařízení v typické IoT řešení.

> [AZURE.NOTE] Vzdálené monitorovací předkonfigurované řešení pořád registru identity zařízení synchronizovat s registru zařízení. Oba používají stejné id zařízení identifikují každého zařízení připojené k rozbočovači IoT.

[Náhled správy zařízení IoT centrální] [ lnk-dm-preview] přidá funkce centrální IoT, které jsou podobné funkce správy přístupových zařízení popisované v tomto článku. Vzdálené monitorovací řešení v současné době používá pouze obecně dostupných funkcí (GA) v centrální IoT.

## <a name="device-information-metadata"></a>Metadata informace zařízení

Zařízení informace metadat JSON dokument uložený v databázi zařízení registru DocumentDB obsahuje následující strukturu:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: samotném zařízení zapíše těchto vlastností a zařízení je autority pro data. Další vlastnosti zařízení příklad zahrnují výrobce, model číslo a pořadové číslo. 
- **DeviceID**: id jedinečné zařízení. Tato hodnota je v registru IoT Centrum zařízení identity stejné.
- **HubEnabledState**: Stav zařízení v centrální IoT. Tato hodnota původně nastavena na **hodnotu null** dokud nejdřív připojení zařízení. Na portálu řešení hodnotu **null** tvaru zařízení jsou "registrovaných, ale není k dispozici."
- **CreatedTime**: čas vytvoření zařízení.
- **DeviceState**: stav hlášených zařízením.
- **UpdatedTime**: čas zařízení naposledy aktualizovaly portálu řešení.
- **SystemProperties**: na portálu řešení zapíše vlastnosti systému a zařízení nezná tyto vlastnosti. Vlastnost systému příkladu je **ICCID** Pokud řešení oprávnění s a připojení ke službě správu zařízení s podporou SIM.
- **Příkazy**: seznam příkazů zařízení podporuje. Zařízení poskytuje tyto informace zobrazily řešení.
- **CommandHistory**: seznam příkazů odeslaný vzdálené monitorovací řešení zařízení a stav příkazy.
- **IsSimulatedDevice**: příznaku, který identifikuje zařízení jako simulovaný.
- **ID**: Jedinečný identifikátor DocumentDB pro tento dokument zařízení.

> [AZURE.NOTE] Informace o zařízení mohou také obsahovat metadat popisuje telemetrie, které zařízení odešle IoT centrální. Vzdálené sledování řešení používá tato metadata telemetrie můžete přizpůsobit způsob zobrazení na řídicím panelu [telemetrie dynamické][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Cyklus

Při prvním vytvoření zařízení na portálu řešení, řešení vytvoří položku ve své zařízení registru dříve viz. Většina informací je zpočátku prázdná a **HubEnabledState** je nastavena na **hodnotu null**. V tomto okamžiku řešení také vytvoří položku zařízení registru identity zařízení, který generuje klávesy, které zařízení používají k ověření IoT centrální.

Pokud zařízení nejdřív připojí k řešení, odešle informace zprávu zařízení. Tato zpráva informace zařízení obsahuje zařízení vlastnosti výrobce zařízení, číslo modelu a pořadové číslo. Zařízení informativní zprávu obsahuje taky seznam příkazů, které zařízení podporuje včetně informací o všechny parametry příkazového. Při řešení obdrží tuto zprávu, ten se aktualizuje metadata informace zařízení v registru zařízení.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Prohlížení a úpravy informací o zařízení v portálu řešení

V seznamu zařízení na portálu řešení se zobrazí následující vlastnosti zařízení ve sloupcovém grafu: **Stav** **DeviceId**, **výrobce**, **Model číslo**, **pořadové číslo**, **firmwaru**, **platformy**, **procesor**a **Nainstalovaný RAM**. Vlastnosti zařízení **šířky** a **délky** jednotka umístění v mapy Bing na řídicím panelu. 

![Seznam zařízení][img-device-list]

Pokud kliknete na tlačítko **Upravit** v podokně **Podrobností zařízení** na portálu řešení úpravou všechny tyto vlastnosti. Úprava těchto vlastností aktualizuje záznam pro zařízení v databázi DocumentDB. Ale pokud zařízení odešle zprávu informace aktualizované zařízení, přepíše všechny změny provedené v portálu řešení. **DeviceId**, **Hostname**, **HubEnabledState**, **CreatedTime**, **DeviceState**a **UpdatedTime** vlastnosti na portálu řešení nelze upravovat, protože pouze zařízení má oprávnění prostřednictvím těchto vlastností.

![Upravit zařízení][img-device-edit]

Pomocí portálu řešení zařízení odebrat z vašeho řešení. Při odebrání zařízení řešení odebere metadata zařízení informace z registru zařízení řešení a odstraní položku zařízení v registru identity IoT Centrum zařízení. Před jejím odebráním zařízení, musíte ho zakázat.

![Odebrat zařízení][img-device-remove]

## <a name="device-information-message-processing"></a>Zpracování zprávy informace zařízení

Zařízení informace posílané zařízení se liší od telemetrie zprávy. Zařízení informace zprávy zahrnují informace, například vlastnosti zařízení, příkazy, které můžete odpovědět zařízení a všechny příkaz historie. Rozbočovač IoT samotné nezná metadata obsažené v zařízení informativní zprávu a procesů zprávu stejným způsobem, zpracuje všechny zprávy zařízení do cloudu. Ve vzdáleném sledování řešení [Azure toku analýzy] [ lnk-stream-analytics] projektu (ASA) bude číst zprávy z centrální IoT. Analýzy toku **DeviceInfo** úlohy filtry pro zprávy, které obsahují **"Vztah mezi typem objektu": "DeviceInfo"** a předá ho instanci **EventProcessorHost** Host (hostitel), která běží ve web projektu. Použití logických operátorů instance **EventProcessorHost** používá identifikátor zařízení najít záznam DocumentDB pro konkrétní zařízení a aktualizujte záznam. Záznam registru zařízení nyní obsahuje informace, například vlastnosti zařízení, příkazy a příkaz historie.

> [AZURE.NOTE] Informativní zprávu zařízení je standardní zprávy zařízení do cloudu. Řešení rozlišuje mezi zařízení informace a telemetrie zpráv pomocí ASA dotazů.

## <a name="example-device-information-records"></a>Příklad zařízení informace záznamů

Vzdálené sledování předkonfigurované řešení používá dva typy záznamů zařízení informace: záznamy pro simulovaný zařízení nasazený s položkami pro vlastní zařízení připojíte k řešení a řešení.

### <a name="simulated-device"></a>Simulovaný zařízení

Následující příklad ukazuje záznam JSON zařízení informace pro simulovaný zařízení. Tento záznam je nastavena pro **UpdatedTime**, což znamená, že zařízení odeslal zprávu **DeviceInfo** IoT centrální hodnota. Záznam zahrnuje některé běžné vlastnosti zařízení, definuje šesti příkazy simulovaný zařízení podporují příznaku **IsSimulatedDevice** nastavil **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Vlastní zařízení

Následující příklad ukazuje záznam JSON zařízení informace pro vlastní zařízení a obsahuje sadu **IsSimulatedDevice** příznak na **hodnotu 0**. Uvidíte, že tato vlastní zařízení podporuje dva příkazy a, že na portálu řešení odeslal příkaz **SetTemperature** zařízení:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Na následujícím obrázku je zpráva JSON **DeviceInfo** zařízení odeslané aktualizovat informace metadat zařízení:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Další kroky

Teď budete mít výukové, jak je možné upravit předkonfigurované řešení, můžete prozkoumat některých dalších funkcí a možností řešení IoT sadu automaticky předem nakonfigurovaná:

- [Přehled prediktivní údržby automaticky předem nakonfigurovaná řešení][lnk-predictive-overview]
- [Nejčastější dotazy IoT sadu][lnk-faq]
- [Zabezpečení IoT od základu nahoru][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
