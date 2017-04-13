<properties
    pageTitle="Začínáme s objektů blob úložiště a Visual Studio připojené služby (ASP.NET 5) | Microsoft Azure"
    description="Jak začít používat úložiště objektů Blob Azure v projektu Visual Studio ASP.NET 5 po vytvoření úložiště účtu pomocí aplikace Visual Studio připojené služby"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Začínáme s objektů Blob Azure úložiště a Visual Studio připojené služby (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Základní informace

Tento článek popisuje, jak začít používat úložiště objektů Blob Azure ve Visual Studiu, když jste vytvořili nebo odkazuje účet Azure úložiště v aplikaci project ASP.NET 5 pomocí dialogového okna aplikace Visual Studio přidat připojené služby.

Úložiště objektů Blob Azure je služba pro ukládání velkých objemů Nestrukturovaná data, která je přístupný z kdekoli na světě prostřednictvím protokolu HTTP nebo HTTPS. Jeden objektů blob může mít libovolnou velikost. Objekty BLOB lze například obrázků, zvukových souborů a videosouborů, neformátovaná data a dokumentů. Tento článek popisuje, jak začít s úložiště objektů blob po vytvoření účet Azure úložiště pomocí dialogového okna aplikace Visual Studio **Přidat připojené služby** ASP.NET 5 projektu.

Stejně jako soubory live ve složkách úložiště objektů BLOB live v kontejnerech. Po vytvoření úložiště vytvořit jednu nebo více kontejnery v úložišti. Například v úložiště s názvem "Výstřižků", můžete vytvořit kontejnery v úložišti s názvem "obrázky" pro ukládání obrázků a jiného s názvem "zvuku" pro uložení zvukových souborů. Po vytvoření kontejnery jednotlivé objektů blob soubory můžete nahrávat na ně. Další informace o programově manipulaci s objekty BLOB najdete v článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Kontejnery objektů blob přístup v kódu

Zobrazíte programově objektů blob na projektech ASP.NET 5, musíte přidat následující položky, pokud již nejsou prezentovat.

1. Přidejte následující deklarace oboru názvů kód do horní části jakéhokoli C# souboru ve kterém chcete přistupovat programově Azure úložiště.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Pokud potřebujete **CloudStorageAccount** objekt, který představuje informací o účtu úložiště. Pomocí následující kód úložiště připojovacího řetězce a informace o účtu úložiště z konfiguraci služby Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Poznámka:** Pomocí výše uvedených kód před kód v následujících částech.


3. Pomocí objektu **CloudBlobClient** **CloudBlobContainer** odkaz na existující kontejner ve vašem účtu úložiště.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Vytvoření kontejneru v kódu

K vytvoření kontejneru ve vašem účtu úložiště můžete taky použít **CloudBlobClient** . Je třeba udělat stačí přidáte volání **CreateIfNotExistsAsync** jako následující kód:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Poznámka:** Rozhraní API provádějící volání Azure úložiště v ASP.NET 5 jsou asynchronní. Další informace najdete v tématu [asynchronní programování s asynchronní a Await](http://msdn.microsoft.com/library/hh191443.aspx) . Následující kód předpokládá, že použily asynchronní programovací metody.

Soubory v kontejneru zpřístupnit všem, můžete nastavit kontejneru byl veřejný pomocí následující kód.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

K nahrání souboru objektů blob do kontejneru, odkazovat kontejneru, můžete získat odkaz objektů blob. Až budete mít objektů blob odkaz, můžete všechny toku dat do ní nahrajete tak, že zavoláte metodu **UploadFromStreamAsync** . Tuto operaci vytvoří objektů blob, pokud ještě není nebo přepíše, pokud existují. Následující příklad ukazuje, jak nahrát objektů blob do kontejneru a předpokládá, že kontejneru byl vytvořen.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru
Seznam objektů BLOB v kontejneru, nejprve získáte odkaz kontejner. Potom můžete zavolat metoda **ListBlobsSegmentedAsync** kontejneru k načtení objektů BLOB a/nebo adresáře v něm obsažené. Přístup k celá řada vlastností a metod pro vrácené **IListBlobItem**, musíte jej převést na **CloudBlockBlob**, **CloudPageBlob**nebo **CloudBlobDirectory** objekt. Pokud neznáte typ objektů blob, můžete určit, ve které se jej převést kontrolu typu. Následující kód ukazuje, jak načíst a výstup URI jednotlivých položek v kontejneru.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
            }

            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob pageBlob = (CloudPageBlob)item;

                Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
            }

            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory directory = (CloudBlobDirectory)item;

                Console.WriteLine("Directory: {0}", directory.Uri);
            }
        }
    } while (token != null);

Způsoby ostatní zobrazit obsah kontejneru objektů blob. Další informace najdete v článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Stáhněte si objektů blob
Stažení objektů blob, nejprve získat odkaz na objekt blob a volat metodu **DownloadToStreamAsync** . Metoda **DownloadToStreamAsync** přenášet obsah objektů blob toku objekt, který pak můžete uložit jako soubor místní v následujícím příkladu.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Existují další způsoby, jak ukládat jako soubory objektů BLOB. Další informace najdete v článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Odstranění objektů blob
Odstranit objektů blob, nejprve získat odkaz na objekt blob a potom zavolat metodu **DeleteAsync** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
