<properties
 pageTitle="Import a export IoT Centrum zařízení identit | Microsoft Azure"
 description="Principy a .NET výstřižky hromadné správy identit zařízení IoT centrální kódu"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Hromadně správy identit IoT Centrum zařízení

Každý IoT centrální má registru identity zařízení, které slouží k vytváření zdrojů na zařízení v služby, jako je fronta obsahující letu zprávy cloudu zařízení. Registru identity zařízení můžete také pomocí řízení přístupu k koncové body přístupným zařízení. Tento článek popisuje, jak k importu a exportu zařízení identit hromadně do a z registru identity zařízení.

Import a export operace dojde v souvislosti s *projekty* , které umožňují spouštět operace služby hromadné proti rozbočovači IoT.

Třídy **RegistryManager** obsahuje **ExportDevicesAsync** a **ImportDevicesAsync** metody, které používají framework **projektu** . Těchto postupů můžete exportovat, importovat a synchronizovat rozsahu k IoT Centrum zařízení registru.

## <a name="what-are-jobs"></a>Jaké jsou úlohy?

Operace registru identity zařízení **úlohy** systém při operaci:

*  Má potenciálně spuštění dlouho ve srovnání s standardní runtime operace nebo
*  Vrátí velkého množství dat uživateli.

V těchto případech místo jedné rozhraní API hovoru čekání nebo blokování na výsledek operace vytvoří operace asynchronní **projektu** pro daný rozbočovač IoT. Operace klikněte hned vrátí **JobProperties** objekt.

Následující C# fragment kódu ukazuje, jak vytvořit úlohu exportovat:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Pak můžete třídy **RegistryManager** k vytvoření dotazu stav **úlohy** pomocí vrácená **JobProperties** metadata.

Následující C# fragment kódu ukazuje, jak hlasování každých pět sekund zobrazíte, když úlohy dokončí:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Export zařízení

Použijte metodu **ExportDevicesAsync** exportovat jako celek k IoT Centrum zařízení registru do kontejneru [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/) objektů blob používání [Sdílené podpis přístup](https://msdn.microsoft.com/library/ee395415.aspx).

Tento způsob umožňuje vytvořit spolehlivé zálohy vašich zařízení v kontejneru objektů blob, který určíte.

Metoda **ExportDevicesAsync** vyžaduje dva parametry:

*  *Řetězec* , který obsahuje identifikátor URI objektů blob kontejner. Tento identifikátor URI musí obsahovat token přidružení zabezpečení, které poskytuje přístup pro zápis do kontejneru. Úlohy vytvoří blok objektů blob v tomto kontejneru k ukládání sériové exportovat data zařízení. Přidružení zabezpečení token musí obsahovat tato oprávnění:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  *Boolean* označující, pokud chcete vyloučit ověřovací klíče exportovat data. Pokud **Nepravda**, ověřování aktualizace jsou zahrnuty ve exportovat výstup; v opačném klíče exportován jako **hodnotu null**.

Následující C# fragment kódu ukazuje, jak exportovat úlohy, která zahrnuje zařízení ověřovací klíče ve exportovat data zahájení a potom hlasování pro dokončení:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Úlohy ukládá jeho výstup v kontejneru ujednaných objektů blob jako objektů blob blok s názvem **devices.txt**. Výstupní data se skládá ze JSON serializován zařízení dat jednoho zařízení na řádek.

Následující příklad ukazuje výstupní data:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Pokud potřebujete přístup k těmto datům v kódu, můžete snadno rekonstruovat tato data pomocí třídy **ExportImportDevice** . Následující C# fragment kódu ukazuje, jak číst informací o zařízení, která byla dříve exportovat do bloku objektů blob:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Můžete taky metodu **GetDevicesAsync** třídy **RegistryManager** vzdáleně seznam svých zařízeních. Tento přístup má však pevné zakončení 1000 na počet objektů zařízení, které se vracejí. Případ použití očekávané pro metodu **GetDevicesAsync** je scénářích vývoj na podporu ladění a se nedoporučuje výrobní úloh.

## <a name="import-devices"></a>Import zařízení

Metoda **ImportDevicesAsync** ve třídě **RegistryManager** umožňuje provádět operace import a synchronizace hromadné IoT Centrum zařízení registru. Podobně jako metodu **ExportDevicesAsync** používá metodu **ImportDevicesAsync** framework **projektu** .

Dávejte metodou **ImportDevicesAsync** , protože kromě vytváření nových zařízení v registru identity zařízení, můžete taky aktualizovat a odstranit existující zařízení.

> [AZURE.WARNING]  Operace importu nelze vzít zpět. Vždycky si zazálohujte stávajících datech metodou **ExportDevicesAsync** na jiný kontejner objektů blob před provádět hromadné změny registru identity svého zařízení.

Metoda **ImportDevicesAsync** se zadávají dva parametry:

*  *Řetězec* , který obsahuje identifikátor URI kontejneru [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/) objektů blob používat jako *zadávání* do projektu. Tento identifikátor URI musí obsahovat token přidružení zabezpečení, které poskytuje přístup pro čtení na kontejner. Tento kontejner musí obsahovat objektů blob s názvem **devices.txt** , který obsahuje data sériové zařízení, která chcete importovat do registru identity zařízení. Import dat musí obsahovat informace o zařízení ve stejné formátu JSON používající **ExportImportDevice** úkoly při vytváření objektů blob **devices.txt** . Přidružení zabezpečení token musí obsahovat tato oprávnění:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *Řetězec* , který obsahuje identifikátor URI kontejneru [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/) objektů blob jako *výstup* projektu použít. Úlohy vytvoří blok objektů blob v tomto kontejneru obsahují libovolné chyby informace z importu dokončení **projektu**. Přidružení zabezpečení token musí obsahovat tato oprávnění:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Dvěma parametry může odkazovat na stejném kontejneru objektů blob. Samostatné parametry jednoduše povolit mít větší kontrolu nad daty kontejneru výstup vyžaduje další oprávnění.

Následující C# fragment kódu ukazuje, jak zahajte import projektu:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Nastavení importu

Použijte metodu **ImportDevicesAsync** dělat tyto operace hromadné v registru identity zařízení:

-   Hromadné registrace nového zařízení
-   Hromadné odstranění existující zařízení
-   Hromadné změny stavu (povolit nebo zakázat zařízení)
-   Hromadné přiřazování nové klíče ověřování zařízení
-   Hromadně automatického obnovení zařízení ověřování kláves

Můžete provádět libovolnou kombinací předchozí operace v rámci jedné **ImportDevicesAsync** hovoru. Můžete třeba zaregistrovat nová zařízení a odstranit nebo aktualizovat existující zařízení ve stejnou dobu. Při použití společně s metodu **ExportDevicesAsync** úplně z můžete migrovat všech svých zařízeních IoT středovém do jiného.

Použijte vlastnost volitelné **režimem importu** v dialogu import dat serializace u každého zařízení řídit proces za zařízení pro import. Vlastnost **režimem importu** obsahuje následující možnosti:

| režimem importu |  Popis |
| -------- | ----------- |
| **createOrUpdate** | Pokud zařízení se zadaným **id**neexistuje, je nově registrovaná. <br/>Pokud je zařízení už existuje, dojde k přepsání existující informace zadané vstupní data bez ohledu na hodnotu **ETag** . |
| **Vytvoření** | Pokud zařízení se zadaným **id**neexistuje, je nově registrovaná. <br/>Pokud je zařízení už existuje, chyba je aby došlo k zápisu soubor protokolu. |
| **aktualizace** | Pokud se zadaným **id**již existuje zařízení, dojde k přepsání existující informace zadané vstupní data bez ohledu na hodnotu **ETag** . <br/>Pokud je zařízení neexistuje, chyba je aby došlo k zápisu soubor protokolu. |
| **updateIfMatchETag** | Pokud se zadaným **id**již existuje zařízení, existující informace dojde k přepsání zadané vstupní data jenom v případě, že je shody **ETag** . <br/>Pokud je zařízení neexistuje, chyba je aby došlo k zápisu soubor protokolu. <br/>Pokud existuje **ETag** neshodu, chyba je aby došlo k zápisu soubor protokolu. |
| **createOrUpdateIfMatchETag** | Pokud zařízení se zadaným **id**neexistuje, je nově registrovaná. <br/>Pokud je zařízení už existuje, existující informace dojde k přepsání zadané vstupní data jenom v případě, že je shody **ETag** . <br/>Pokud existuje **ETag** neshodu, chyba je aby došlo k zápisu soubor protokolu. |
| **odstranění** | Pokud se zadaným **id**již existuje zařízení, odstraní se bez ohledu na hodnotu **ETag** . <br/>Pokud je zařízení neexistuje, chyba je aby došlo k zápisu soubor protokolu. |
| **deleteIfMatchETag** | Pokud se zadaným **id**již existuje zařízení, odstraní se jenom v případě, že je shody **ETag** . Pokud je zařízení neexistuje, chyba je aby došlo k zápisu soubor protokolu. <br/>Pokud existuje ETag neshodu, chyba je aby došlo k zápisu soubor protokolu. |

> [AZURE.NOTE] Pokud data serializace není výslovně definovat **režimem importu** příznaku pro zařízení, ve výchozím nastavení **createOrUpdate** během operace importu.

## <a name="import-devices-example--bulk-device-provisioning"></a>Import zařízení příklad – hromadně zřizování zařízení 

Následující příklad kódu C# ukazuje, jak generovat více identit zařízení, která:

- Obsahují ověřování klíče.
- Napište informace, které zařízení objektů blob blok.
- Naimportujte zařízení registru identity zařízení.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Import zařízení příklad – hromadné odstranění

Následující příklad kódu ukazuje, jak odstranit zařízení jste přidali pomocí výše uvedeném příkladu:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Získání kontejneru URI přidružení zabezpečení


Následující příklad kódu znázorňuje generovat [URI přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-2.md) pro čtení, Zapsat a oprávnění pro kontejneru objektů blob odstraňování:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Další kroky

V tomto článku se naučíte v rozbočovači IoT provádět hromadné operace proti registru identity zařízení. Tyto odkazy vedou na další informace o správě Azure IoT centrální:

- [Použití metriky][lnk-metrics]
- [Operace sledování][lnk-monitor]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md