<properties
    pageTitle="Začínáme s úložiště objektů Blob Azure (objektu úložiště) pomocí .NET | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Začínáme s úložiště objektů Blob Azure pomocí .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Úložiště objektů Blob Azure je služba, která ukládá jako objekty/objektů BLOB Nestrukturovaná data v cloudu. Úložiště objektů BLOB mohou být uloženy libovolné typu text nebo binárních dat, například jako dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.

### <a name="about-this-tutorial"></a>O tomto kurzu

Tento kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře pomocí úložiště objektů Blob Azure. Scénáře nichž se uplatní zahrnují nahrávání záznam, stažení a odstranění objektů BLOB.

**Předpokládaná doba dokončete:** 45 minut

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Knihovna Azure úložiště klienta pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Správce konfigurace pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Účet Azure úložiště](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další příklady

Další příklady použití úložiště objektů Blob najdete v článku [Začínáme s úložišti objektů Blob Azure v .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Můžete stáhnout ukázková aplikace a spuštění nebo vyhledat kód na GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Přidání deklarace oboru názvů

Přidejte následující `using` příkazů do horní části `program.cs` souboru:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Analyzovat připojovacího řetězce

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Vytvoření klienta služby objektů Blob

**CloudBlobClient** předmětu umožňuje načítat kontejnery a objekty BLOB uložených v úložišti objektů Blob. Tady je jedním ze způsobů vytvoření klienta služby:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Teď jste připraveni zadat kód, který načítá data z a zapisuje data k úložišti objektů Blob.

## <a name="create-a-container"></a>Vytvoření kontejneru

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak vytvořit kontejner, pokud už neexistuje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Ve výchozím nastavení je kontejneru nové soukromé, což znamená, že je nutné zadat kód úložiště přístup ke stažení objektů BLOB tento kontejner. Pokud chcete zpřístupnit soubory v kontejneru, můžete nastavit kontejneru byl veřejný pomocí následujícího kódu:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Každý, kdo na Internetu zobrazit objektů BLOB v kontejneru veřejné, ale můžete upravit nebo odstranit jenom v případě, že máte přístupová klávesa odpovídající účet nebo podpis sdílený přístup.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Úložiště objektů Blob Azure podporuje objektů BLOB bloku a objekty BLOB stránky.  Ve většině případů objektů blob blok je doporučený typ, který chcete použít.

K nahrání souboru do bloku objektů blob, odkazovat kontejneru, můžete získat odkaz objektů blob blokovat. Až budete mít objektů blob odkaz, můžete všechny toku dat do ní nahrajete tak, že zavoláte metodu **UploadFromStream** . Tuto operaci vytvoří objektů blob, pokud ho neměli dříve neexistuje nebo přepsat, pokud existují.

Následující příklad ukazuje, jak nahrát objektů blob do kontejneru a předpokládá, že kontejneru byl vytvořen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam objektů BLOB v kontejneru, nejprve získáte odkaz kontejner. Potom můžete pomocí metody **ListBlobs** kontejneru k načtení objektů BLOB a/nebo adresáře v něm obsažené. Přístup k celá řada vlastností a metod pro vrácené **IListBlobItem**, musíte jej převést na **CloudBlockBlob**, **CloudPageBlob**nebo **CloudBlobDirectory** objekt.  Pokud Neznámý stav, můžete určit, ve které se jej převést kontrolu typu.  Následující kód ukazuje, jak načíst a výstupní URI každá položka v `photos` kontejner:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

Podle výše uvedených zadáte název objektů BLOB s informace o cestě k jejich jména. Tím vytvoříte strukturu virtuálního adresáře, kterou můžete uspořádat procházet stejně jako systému tradiční souborů. Všimněte si, že struktuře adresářů virtuální pouze - jsou k dispozici v úložišti objektů Blob pouze zdroje kontejnerů a objektů BLOB. Však knihovně úložiště klienta nabízí objekt **CloudBlobDirectory** virtuální adresář a zjednodušit proces práce s objekty BLOB, které jsou uspořádány tímto způsobem.

Například, zvažte následující sadu objektů BLOB blok v kontejneru s názvem `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Když voláte **ListBlobs** v kontejneru "fotografie" (jako výše uvedené ukázkové), bude vrácena hierarchický seznam. Obsahuje **CloudBlobDirectory** a **CloudBlockBlob** objekty, které představuje adresářů a objekty BLOB v kontejneru. Výsledný výstup vypadá takto:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Pokud chcete můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** **logickou hodnotu PRAVDA**. V tomto případě všech objektů blob v kontejneru vrácena jako objekt **CloudBlockBlob** . Volání **ListBlobs** vrátíte ploché seznam vypadat takto:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a výsledek bude vypadat takto:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Stáhněte si objekty BLOB

Ke stažení objektů BLOB, první načtení objektů blob odkazu a potom volat metodu **DownloadToStream** . Metoda **DownloadToStream** přenášet obsah objektů blob toku objekt, který pak můžete uchovávat místního souboru v následujícím příkladu.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Můžete taky použijte metodu **DownloadToStream** si Pokud chcete stáhnout obsah objektů blob jako textový řetězec.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Odstranění objektů BLOB

Odstranění objektů blob, nejprve získat odkaz objektů blob a potom zavolat metodu **Delete** .

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchronní seznamu objektů blob na stránkách

Pokud jsou výpis velký počet objektů BLOB nebo chcete určit počet výsledků zaručuje výpis najednou, můžete vytvořit seznam objektů blob na stránkách výsledků. Tento příklad ukazuje, jak se na stránkách asynchronní, takže spuštění blokován během čekání se vrátíte na velké sadu výsledků.

Tento příklad ukazuje ploché objektů blob záznam, ale hierarchický seznam můžete taky provedl nastavením `useFlatBlobListing` parametr metodu **ListBlobsSegmentedAsync** `false`.

Protože volá asynchronní metodu vzorku, musí být také s `async` klíčových slov a musí vrátit objekt **úkolu** . Klíčové slovo await určeným pro metodu **ListBlobsSegmentedAsync** pozastaví provádění metodu ukázkové až do dokončení úkolu seznam.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
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

## <a name="writing-to-an-append-blob"></a>Psaní objektů blob připojit

Přidat blob je nový typ objektů blob zavádí s verzí 5.x knihovnu Azure úložiště klienta pro .NET. Přidat blob je optimalizována pro připojený operací, jako je protokolování. Jako blok blob objektů blob připojit se skládá ze bloky, ale při přidání nového bloku objektů blob přidávacího je vždy připojen za účelem objektů blob. Nelze aktualizovat nebo odstranit existující bloku v objektů blob připojit. Blokovat ID pro připojený blob nejsou přístupné, stejně jako jsou blok objektů blob.

Každý blok objektů blob přidávacího může být různé velikosti až do velikosti 4 MB a objektů blob přidávacího může obsahovat maximálně 50 000 bloky. Maximální velikosti doručovaných objektů blob přidávacího tedy něco víc než 195 GB (4 MB × 50 000 bloků).

V příkladu níže vytvoří nový objektů blob připojit a připojí některá data, simulace operace jednoduché protokolování.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Další informace o rozdílech mezi tři typy objektů BLOB najdete v článku [Principy blok objektů BLOB, objekty BLOB stránky a připojit objektů BLOB](https://msdn.microsoft.com/library/azure/ee691964.aspx) .

## <a name="managing-security-for-blobs"></a>Správa zabezpečení pro objekty BLOB

Ve výchozím nastavení úložišti Azure udržuje data zabezpečené omezením přístupu k vlastníka účtu, který je k dispozici přístupových kláves účtu. Pokud potřebujete sdílet data ve vašem účtu úložiště objektů blob, je důležité k tomu nevyzve bez omezení zabezpečení účtu přístupové klávesy. Navíc dá zašifrovat objektů blob data a ujistěte se, že byla v bezpečí přejdete myší drátěný a v úložišti Azure.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Řízení přístupu k datům objektů blob

Ve výchozím nastavení je dostupné jenom vlastník účtu úložiště objektů blob dat ve vašem účtu úložiště. Ve výchozím nastavení ověřování požadavky úložiště objektů Blob vyžaduje účet přístupové klávesy. Chcete však zpřístupnit určité údaje objektů blob ostatním uživatelům. Máte dvě možnosti:

- **Anonymní přístup:** Můžete zpřístupnit kontejneru nebo jeho objektů BLOB veřejně pro anonymní přístup. Další informace najdete v tématu [Správa anonymní přístup pro čtení kontejnerů a objektů BLOB](storage-manage-access-to-resources.md) .
- **Sdílené přístup podpisy:** Klienti může poskytnout sdílený přístup podpisu a poskytující delegované přístup k prostředku ve vašem úložišti účtu s oprávněními, které zadáte a přes interval, který určíte. Další informace najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Šifrování objektů blob dat

Azure úložiště podporuje šifrování objektů blob dat na straně klienta a na serveru:

- **Klientský šifrování:** V knihovně úložiště klienta .NET podporuje šifrování dat v klientských aplikacích před nahráním k základnímu úložišti Azure a dešifrování dat při stahování klientovi. Knihovny taky podporuje integrace se službou Azure Key trezoru pro správu klíčů účtu úložiště. Další informace najdete v tématu [Klientských šifrování s .NET pro úložišti tabulek Microsoft Azure](storage-client-side-encryption.md) . Viz také [kurz: šifrování a dešifrování objektů BLOB v úložišti tabulek Microsoft Azure pomocí Azure klíč trezoru](storage-encrypt-decrypt-blobs-key-vault.md).
- **Serverový šifrování**: úložišti Azure nyní podporuje šifrování na straně serveru. V části [šifrování služby Azure úložiště dat u ostatních (verze Preview)](storage-service-encryption.md).

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základní informace o úložišti objektů Blob, tyto odkazy vedou na další informace.

### <a name="microsoft-azure-storage-explorer"></a>Průzkumník úložišť Microsoft Azure
- [Azure úložiště Explorer (míra MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná, samostatné aplikace od Microsoftu, který umožňuje vizuálně pracovat s daty Azure úložiště v systému Windows, OS X a Linux.

### <a name="blob-storage-samples"></a>Ukázky úložiště objektů BLOB

- [Začínáme s úložišti objektů Blob Azure v .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Odkaz úložiště objektů BLOB

- [Knihovna úložiště klienta pro referenci .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Odkaz rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Koncepční vodítka

- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
- [Začínáme s úložiště souborů pro .NET](storage-dotnet-how-to-use-files.md)
- [Jak pomocí WebJobs SDK úložišti objektů blob Azure](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
