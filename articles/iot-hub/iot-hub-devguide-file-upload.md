<properties
 pageTitle="Karta Vývojář v příručce - odeslání souboru | Microsoft Azure"
 description="Azure Průvodce vývojář IoT centrální - nahrávání souborů ze zařízení k rozbočovači IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Nahrání souborů ze zařízení

## <a name="overview"></a>Základní informace

Co podrobné v [Centrální IoT koncové body] [ lnk-endpoints] článku zařízení můžete zahájit ukládání souborů odesláním oznámení pomocí zařízení webových koncového bodu (**/devices/ {deviceId} / soubory**).  Když zařízení s upozorněním IoT centrální nahrání dokončený, IoT centrální generuje oznámení o odeslání souboru, které mohou získat prostřednictvím služby webových koncového bodu (**/messages/servicebound/filenotifications**) jako zprávy.

Místo zprostředkovatelské zprávy pomocí rozbočovače IoT samotné IoT centrální místo toho funguje jako odesílatele přidružené účet Azure úložiště. Zařízení vyžaduje token úložiště z centrální IoT, která je specifická soubor, který chce nahrát zařízení. Zařízení používá URI přidružení zabezpečení nahrajte soubor do úložiště a po dokončení nahrávání zařízení pošle oznámení o absolvování IoT centrální. Centrální IoT ověřuje, zda soubor byl odeslán a potom přidá upozornění na odeslání souboru na nové webových služeb soubor oznámení zasílání koncový bod.

Před odesláním souboru k rozbočovači IoT ze zařízení, musíte nakonfigurovat vaše Centrum přidružením [Úložišti Azure] [ lnk-associate-storage] účet do ní.

Zařízení, můžete [Inicializace nahrávání] [ lnk-initialize] a [Upozornit IoT centrální] [ lnk-notify] při nahrávání dokončení. Pokud chcete, pokud zařízení s upozorněním IoT centrální dokončení nahrávání, službu generovat [oznámení][lnk-service-notification].

### <a name="when-to-use"></a>Kdy použít

Pomocí této funkce IoT rozbočovači, když budete muset nahrát soubor z zařízení ke službě back-end místo odesílání zpráv pomocí IoT rozbočovače.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Přidružit IoT centrální účet Azure úložiště

Použít funkci nahrát soubor, je nutné nejprve propojit účet Azure úložiště k rozbočovači IoT. Můžete to udělat buď přes [Azure portál][lnk-management-portal], nebo programově pomocí [zprostředkovatele prostředků IoT centrální rozhraní REST API][lnk-resource-provider-apis]. Jakmile jste přiřadili účet Azure úložiště k centrální IoT, službu vrátí URI přidružení zabezpečení na zařízení, pokud iniciuje žádost o odeslání souboru zařízení.

> [AZURE.NOTE] [Azure IoT centrální SDK] [ lnk-sdks] automaticky zpracovávat načítání URI přidružení zabezpečení, při ukládání souboru a oznamování IoT centrální nahrání dokončený.

## <a name="initialize-a-file-upload"></a>Inicializace odeslání souboru

Rozbočovač IoT obsahuje koncový bod pro zařízení s žádostí o přidružení zabezpečení URI úložištěm k nahrání souboru. Zařízení zahájí proces nahrát soubor tak, že příspěvku rozbočovači IoT na `{iot hub}.azure-devices.net/devices/{deviceId}/files` s následující JSON textu:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

Rozbočovač IoT vrátí následující zařízení používá k nahrání souboru:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Nepoužívají se: Inicializace odeslání souboru s získáte

> [AZURE.NOTE] Tato část popisuje nepoužívaných funkcí informace týkající se dostanete přidružení zabezpečení URI od IoT centrální. Použijte metodu POST jsme je popsali výše.

Centrální IoT má dva koncové body ZBÝVAJÍCÍ podporuje nahrávání souboru, jedna ku zobrazí Uniform přidružení zabezpečení pro ukládání a druhý sdělit IoT rozbočovači nahrání dokončený. Zařízení zahájí proces nahrát soubor tak, že získáte rozbočovači IoT na `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Rozbočovač vrátí přidružení zabezpečení URI konkrétní soubor nahrát a ID korelace, které má být použit po dokončení nahrávání.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Upozornit IoT centrální odeslání dokončené souboru

Zařízení je zodpovědný za nahrávání souboru pro ukládání pomocí SDK úložiště Azure. Po dokončení nahrávání zařízení rozešle centru IoT v příspěvku `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` s následující JSON textu:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Hodnota `isSuccess` je logické které představuje, zda byl úspěšně odeslán soubor. Stavový kód `statusCode` je stav nahrávání souboru k základnímu úložišti a `statusDescription` odpovídá `statusCode`.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o nahrávání souborů ze zařízení.

## <a name="file-upload-notifications"></a>Oznámení o odeslání souboru

Když zařízení odešle soubor, upozorní IoT centrální dokončení nahrávání službu volitelně vygeneruje zprávy s upozorněním, který obsahuje název a úložiště umístění souboru.

Způsobem popsaným v tématu [koncové body][lnk-endpoints], IoT Centrum poskytuje soubor oznámení o odeslání prostřednictvím služby webových koncového bodu (**/messages/servicebound/fileuploadnotifications**) jako zprávy. Přijímání sémantiku pro oznámení o odeslání souboru jsou stejné jako u zprávy cloudu zařízení a mít stejné [zprávy cyklus][lnk-lifecycle]. Každé zprávy načtené z koncového bodu oznámení nahrát soubor je záznam JSON s následujícími vlastnostmi:

| Vlastnost | Popis |
| -------- | ----------- |
| EnqueuedTimeUtc | Časové razítko označující vytvoření oznámení. |
| DeviceId | **DeviceId** zařízení, které nahrát soubor. |
| BlobUri | Identifikátor URI uloženého souboru. |
| BlobName | Název souboru nahrané. |
| LastUpdatedTime | Časové razítko označující poslední aktualizace soubor. |
| BlobSizeInBytes | Velikost uloženého souboru. |

**Příklad**. Toto je příklad těla zprávy s upozorněním nahrát soubor.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Možnosti konfigurace oznámení nahrát soubor

Každý IoT Centrum poskytuje následující možnosti konfigurace pro oznámení o odeslání souboru:

| Vlastnost | Popis | Oblast a výchozí |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Určuje, jestli oznámení o odeslání souboru jsou došlo k zápisu koncového bodu oznámení ve formě souborů. | Logická hodnota. Výchozí: PRAVDA. |
| **fileNotifications.ttlAsIso8601** | Výchozí hodnota TTL pro oznámení o odeslání souboru. | ISO_8601 interval až 48 H (minimální minutu). Výchozí hodnota: 1 hodinu. |
| **fileNotifications.lockDuration** | Uzamknout dobu fronty oznámení nahrát soubor. | 5 až 300 sekund (minimální 5 sekund). Výchozí: 60 sekund. |
| **fileNotifications.maxDeliveryCount** | Doručení maximální počet různých hodnot soubor nahrajte frontě oznámení. | 1 až 100. Výchozí: 100. |

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT centrální a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT rozbočovači MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT rozbočovači protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď jste se naučili jak nahrát soubory ze zařízení s protokolem IoT centrální, vás mohla zajímat v následujících tématech vývojář příručka:

- [Správa identity zařízení v centrální IoT][lnk-devguide-identities]
- [Řízení přístupu k centrální IoT][lnk-devguide-security]
- [Pomocí zařízení twins synchronizovat stav a konfigurace][lnk-devguide-device-twins]
- [Přímé metodu na zařízení][lnk-devguide-directmethods]
- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, vás mohla zajímat následující kurz IoT centrální:

- [Jak nahrát soubory ze zařízení do cloudu pomocí IoT centrální][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
