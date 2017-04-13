<properties
    pageTitle="Začínáme s objektů blob úložiště a Visual Studio připojené služby (cloudovým službám) | Microsoft Azure"
    description="Jak začít používat úložiště objektů Blob Azure v aplikaci project cloudové služby ve Visual Studiu po připojení k úložišti účtu pomocí aplikace Visual Studio připojené služby"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Začínáme s úložišti objektů Blob Azure a Visual Studio připojené služby (cloud services projektů)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak začít s úložišti objektů Blob Azure po vytvoření nebo odkazuje účet Azure úložiště pomocí dialogového okna aplikace Visual Studio **Přidat připojené služby** v projektu Visual Studio cloud services. Ukážeme vám jak přístupu a k vytváření objektů blob kontejnery a jak provádět běžné úlohy jako nahrávání, zobrazení a stahování objektů BLOB. Vzorky jsou napsané v jazyce C\# a používat [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Úložiště objektů Blob Azure je služba pro ukládání velkých objemů Nestrukturovaná data, která je přístupný z kdekoli na světě prostřednictvím protokolu HTTP nebo HTTPS. Jeden objektů blob může mít libovolnou velikost. Objekty BLOB lze například obrázků, zvukových souborů a videosouborů, neformátovaná data a dokumentů.

Stejně jako soubory live ve složkách úložiště objektů BLOB live v kontejnerech. Po vytvoření úložiště vytvořit jednu nebo více kontejnery v úložišti. Například v úložiště s názvem "Výstřižků", můžete vytvořit kontejnery v úložišti s názvem "obrázky" pro ukládání obrázků a jiného s názvem "zvuku" pro uložení zvukových souborů. Po vytvoření kontejnery jednotlivé objektů blob soubory můžete nahrávat na ně.

- Další informace o programově manipulaci s objekty BLOB najdete v článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md).
- Obecné informace o úložišti Azure najdete v článku [si přečtěte následující dokumentaci úložiště](https://azure.microsoft.com/documentation/services/storage/).
- Obecné informace o Azure Cloud Services najdete v článku [si přečtěte následující dokumentaci Cloudovým službám](https://azure.microsoft.com/documentation/services/cloud-services/).
- Další informace o programování ASP.NET aplikací najdete v tématu [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Kontejnery objektů blob přístup v kódu

Zobrazíte programově objektů blob na projektech cloudové služby, musíte přidat následující položky, pokud již nejsou prezentovat.

1. Přidejte následující deklarace oboru názvů kód do horní části jakéhokoli C# souboru ve kterém chcete programově přístup k úložišti Azure.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pokud potřebujete **CloudStorageAccount** objekt, který představuje informací o účtu úložiště. Pomocí následující kód úložiště připojovacího řetězce a informace o účtu úložiště z konfiguraci služby Azure.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Získejte objekt **CloudBlobClient** neodkazuje existující kontejner ve vašem účtu úložiště.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Získejte objekt **CloudBlobContainer** neodkazuje kontejneru konkrétní objektů blob.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Použijte všechny kód zobrazený v předchozím postupu před kód zobrazený v následujících částech.

## <a name="create-a-container-in-code"></a>Vytvoření kontejneru v kódu

> [AZURE.NOTE] Některé rozhraní API, které provádět volání se k základnímu úložišti Azure technologie ASP.NET jsou asynchronní. Další informace najdete v tématu [asynchronní programování s asynchronní a Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kód v následujícím příkladu se předpokládá, že používáte asynchronní programovací metody.

Vytvoření kontejneru ve vašem účtu úložiště, musíte udělat je přidání hovoru do **CreateIfNotExistsAsync** jako následující kód:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Soubory v kontejneru zpřístupnit všem, můžete nastavit kontejneru byl veřejný pomocí následující kód.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Každý, kdo na Internetu zobrazit objektů BLOB v kontejneru veřejné, ale můžete upravit nebo odstranit jenom v případě, že máte odpovídající přístupové klávesy.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Azure úložiště podporuje objektů BLOB bloku a objekty BLOB stránky. Ve většině případů objektů blob blok je doporučený typ, který chcete použít.

K nahrání souboru do bloku objektů blob, odkazovat kontejneru, můžete získat odkaz objektů blob blokovat. Až budete mít objektů blob odkaz, můžete všechny toku dat do ní nahrajete tak, že zavoláte metodu **UploadFromStream** . Tuto operaci objektů blob ho neměli existují dříve, nebo vytvořena přepíše, pokud existují. Následující příklad ukazuje, jak nahrát objektů blob do kontejneru a předpokládá, že kontejneru byl vytvořen.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam objektů BLOB v kontejneru, nejprve získáte odkaz kontejner. Potom můžete pomocí metody **ListBlobs** kontejneru k načtení objektů BLOB a/nebo adresáře v něm obsažené. Přístup k celá řada vlastností a metod pro vrácené **IListBlobItem**, musíte jej převést na **CloudBlockBlob**, **CloudPageBlob**nebo **CloudBlobDirectory** objekt. Pokud Neznámý stav, můžete určit, ve které se jej převést kontrolu typu. Následující kód ukazuje, jak načíst a výstupní URI jednotlivých položek v kontejneru **fotografie** :

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

Jak je vidět ve výše uvedeném příkladu, služba objektů blob obsahuje koncepci adresáře v rámci kontejnerů, i. To je a organizovat vaše objektů BLOB ve více stromové struktuře. Například zvažte následující sadu objektů BLOB blok v kontejneru s názvem **fotografie**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Při volání **ListBlobs** v kontejneru (jako v předchozím výběru) kolekci vrácenou obsahem **CloudBlobDirectory** a **CloudBlockBlob** představující adresářů a objekty BLOB obsažené na nejvyšší úrovni. Tady je výsledný výstup:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Pokud chcete můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** **logickou hodnotu PRAVDA**. Výsledkem všech objektů blob vrácené jako **CloudBlockBlob**, bez ohledu na adresář. Tady je volání **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a tady jsou výsledky:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Další informace najdete v tématu [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Stáhněte si objekty BLOB

Ke stažení objektů BLOB, první načtení objektů blob odkazu a potom volat metodu **DownloadToStream** . Metoda **DownloadToStream** přenášet obsah objektů blob toku objekt, který pak můžete uchovávat místního souboru v následujícím příkladu.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Můžete taky použijte metodu **DownloadToStream** si Pokud chcete stáhnout obsah objektů blob jako textový řetězec.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Odstranění objektů BLOB

Odstranit objektů blob, nejprve získat odkaz objektů blob a poté zavolejte metodu **Delete** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchronní seznamu objektů blob na stránkách

Pokud jsou výpis velký počet objektů BLOB nebo chcete určit počet výsledky zaručuje výpis najednou, můžete vytvořit seznam objektů blob na stránkách výsledků. Tento příklad ukazuje, jak se na stránkách asynchronní, takže spuštění blokován během čekání se vrátíte na velké sadu výsledků.

Tento příklad ukazuje ploché objektů blob záznam, ale hierarchický seznam můžete taky provedl nastavením parametru **useFlatBlobListing** metody **ListBlobsSegmentedAsync** na **hodnotu false**.

Protože volá asynchronní metodu vzorku, musí být také pomocí klíčového slova **asynchronní** a musí vrátit objektu **úlohy** . Klíčové slovo await určeným pro metodu **ListBlobsSegmentedAsync** pozastaví provádění metodu ukázkové až do dokončení úkolu seznam.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
