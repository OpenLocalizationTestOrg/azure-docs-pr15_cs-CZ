<properties
 pageTitle="Správa vypršení platnosti obsahu Azure úložiště objektů blob Azure CDN | Microsoft Azure"
 description="Informace o možnostech pro řízení time-to-live pro objekty BLOB v Azure CDN ukládání do mezipaměti."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Správa vypršení platnosti obsahu Azure úložiště objektů blob Azure CDN

> [AZURE.SELECTOR]
- [Azure webové aplikace/cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Služba Azure úložiště objektů blob](cdn-manage-expiration-of-blob-content.md)

[Služby objektů blob](../storage/storage-introduction.md#blob-storage) [Azure úložiště](../storage/storage-introduction.md) je jeden z několika typů založené na Azure integrovaný s Azure CDN.  Veškerý obsah veřejně přístupný objektů blob můžete uložené v Azure CDN, dokud uplyne time to live (TTL).  Hodnota TTL, je určený podle [záhlaví *Cache Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP z Azure úložiště.

>[AZURE.TIP] Můžete nastavit žádné TTL objektů blob.  Azure CDN v tomto případě použije automaticky výchozí hodnota TTL sedmi dnů.
>
>Další informace o tom, jak funguje Azure CDN urychlit přístup ke objektů BLOB a jiných souborů najdete v článku [Přehled CDN Azure](./cdn-overview.md).
>
>Další informace o službu Azure úložiště objektů blob najdete v tématu [Objektů Blob služby koncepty](https://msdn.microsoft.com/library/dd179376.aspx). 

Tento kurz ukazuje několik způsobů, jak můžete nastavit hodnotu TTL objektů blob v úložišti Azure.  

## <a name="azure-powershell"></a>Azure Powershellu

[Azure PowerShell](../powershell-install-configure.md) je taková nejrychlejší a nejvýkonnějších způsobů, jak spravovat služby Azure.  Použití `Get-AzureStorageBlob` rutina získat odkaz na objektů blob, nastavte `.ICloudBlob.Properties.CacheControl` vlastnost. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Můžete taky pomocí Powershellu ke [správě profilů CDN a koncové body](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Knihovna Azure úložiště klienta pro .NET

Pokud chcete nastavit pomocí .NET objektů blob TTL, použijte [Azure úložiště klienta knihovny pro .NET](../storage/storage-dotnet-how-to-use-blobs.md) nastavovaná vlastnost [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Nejsou k dispozici v [Vzorky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)mnoho dalších .NET ukázky.

## <a name="other-methods"></a>Další metody

- [Azure rozhraní příkazového řádku](../xplat-cli-install.md)

    Při odesílání objektů blob, nastavte vlastnost *cacheControl* pomocí `-p` přepnout.  V tomto příkladu nastaví hodnota TTL hodina (3600 sekund).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Vlastnost *x-ms-objektů blob mezipaměti-ovládacího prvku* výslovně nastavené na žádost o [Umístění objektů Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Umístění seznamu blokovaných adres](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)nebo [Nastavit vlastnosti objektů Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Nástroje pro správu úložiště třetích stran

    Některé nástroje pro správu úložiště Azure třetích stran umožňuje nastavit vlastnost *CacheControl* u objektů BLOB. 

## <a name="testing-the-cache-control-header"></a>Testování záhlaví *Mezipaměti ovládacího prvku*

Můžete snadno ověřit TTL vaší objektů BLOB.  Pomocí webového prohlížeče [nástrojů pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)otestujte, aby vaše objektů blob je včetně záhlaví odpověď *Mezipaměti ovládacího prvku* .  Můžete taky pomocí nástroje jako **wget**, [pošťák](https://www.getpostman.com/)nebo [Fiddler](http://www.telerik.com/fiddler) zkoumat hlavičky odpovědi.

## <a name="next-steps"></a>Další kroky

- [Přečtěte si informace o záhlaví *Mezipaměti ovládacího prvku*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Naučte se spravovat Cloudová služba obsahu v Azure CDN vypršení platnosti](./cdn-manage-expiration-of-cloud-service-content.md)

