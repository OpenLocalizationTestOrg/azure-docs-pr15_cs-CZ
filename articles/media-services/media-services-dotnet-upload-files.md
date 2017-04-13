<properties 
    pageTitle="Nahrajte soubory do účtu Media Services pomocí .NET | Microsoft Azure" 
    description="Zjistěte, jak získat mediální obsah do Media Services – vytvoření a odeslání prostředky." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Nahrajte soubory do účtu Media Services pomocí .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [ZBÝVAJÍCÍ](media-services-rest-upload-files.md)
 - [Portál](media-services-portal-upload-files.md)

V Media Services můžete nahrát (nebo jedí) digitální souborů do aktivum. Entita **materiálů** může obsahovat video, zvuk, obrázky, miniatur kolekce, text skladeb a titulků soubory (a metadata o těchto souborech.)  Po nahrání souborů obsahu je bezpečně uložený v cloudu pro další zpracování a datových proudů.

Soubory v majetek nazývají **Materiálů soubory**. **AssetFile** instance a skutečné multimediálního souboru jsou dvě samostatné objekty. AssetFile instance obsahuje metadata multimediálního souboru, když mediální soubor s obsahem skutečné mediální.

>[AZURE.NOTE]Při výběru název souboru materiálů platí následující omezení:
>
>- Media Services použije hodnotu vlastnost IAssetFile.Name při vytváření adresy URL pro streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu heslo percent-encoding není povolená. Hodnoty **název** vlastnosti nemůže obsahovat žádný z následujících [znaků procent kódování vyhrazené](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Navíc může být pouze jednu "." pro přípona názvu souboru.
>
>- Délka názvu nesmí být větší než 260 znaků.

Když vytvoříte prostředky, můžete použít následující možnosti šifrování. 

- **Žádná** – bez šifrování se používá. Toto je výchozí hodnota. Všimněte si, že použijete tuto možnost obsahu není zamčený při přenosu šifrovaná nebo u ostatních v úložišti.
Pokud budete chtít předvádění MP4 pomocí postupného stahování použijte tuto možnost. 
- **CommonEncryption** - použít tuto možnost, pokud ukládáte obsah, který už zašifrovaných a chráněny běžné šifrování nebo PlayReady DRM (například hladký přenos chráněné s PlayReady DRM).
- **EnvelopeEncrypted** – použití tuto možnost, pokud ukládáte HLS zašifrovaných AES. Všimněte si, že soubory, které musí být zakódovaný a zašifrovaných transformace správce.
- **StorageEncrypted** - šifruje místně pomocí šifrování AES 256 bit a potom odesílání, aby Azure úložiště, kde je uložena zašifrován v klidu vymazat obsah. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Případ použití primární šifrování úložiště je, když chcete zabezpečit vstupní mediální soubory kvalitní silné šifrování u ostatních na disku.

    Media Services poskytuje úložiště na disku šifrování majetku, ne přes – drátěný jako správce digitálních práv (DRM).

    -Li vaše materiálů úložiště zašifrovaných, musíte nakonfigurovat zásady doručení materiálů. Další informace najdete v článku [Konfigurace zásad doručení materiálů](media-services-dotnet-configure-asset-delivery-policy.md).

Pokud zadáte položka šifrování s některou z možností **CommonEncrypted** nebo možnost **EnvelopeEncypted** , musíte přidružit **ContentKey**vaše materiálů. Další informace najdete v tématu [Vytvoření ContentKey](media-services-dotnet-create-contentkey.md). 

Pokud zadáte položka šifrování s možností **StorageEncrypted** , Media Services SDK pro .NET vytvoří **StorateEncrypted** **ContentKey** pro vaše materiálů.


Toto téma ukazuje, jak používat Media Services .NET SDK, jakož i Media Services .NET SDK rozšíření nahrát soubory do majetku Media Services.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Nahrání jednoho souboru s Media Services .NET SDK 

Následující ukázkový kód používá .NET SDK provádět následující úkoly: 

- Vytvoří aktivum prázdné.
- Vytváří instanci AssetFile, kterému chcete přidružit k majetku.
- Vytváří instanci AccessPolicy, který definuje oprávnění a doby trvání přístup k majetku.
- Vytváří instanci Locator, která zajišťuje přístup k materiálů.
- Odešle mediální soubor do Media Services. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Uložení více souborů s Media Services .NET SDK 

Následující kód ukazuje, jak vytvořit aktivum a uložení více souborů.

Kód dělá toto:
    
-   Vytvoří aktivum prázdné metodou CreateEmptyAsset definované v předchozím kroku.
    
-   Vytváří instanci **AccessPolicy** , který definuje oprávnění a doby trvání přístup k majetku.
    
-   Vytváří instanci **Locator** , která zajišťuje přístup k materiálů.
    
-   Vytváří instanci **BlobTransferClient** . Tento typ představuje klienta, který pracuje s objekty BLOB Azure. V tomto příkladu použijeme klienta ke sledování průběhu nahrávání. 
    
-   Výčet prostřednictvím souborů v zadaném adresáři a vytváří instanci **AssetFile** pro každý soubor.
    
-   Odesílání souborů do Media Services metodou **UploadAsync** . 
    
>[AZURE.NOTE] Použijte metodu UploadAsync ověřit, že volání neblokuje a soubory jsou odeslány paralelně.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Při odesílání velkého počtu prostředky, zvažte tyto skutečnosti:

- Vytvoření nového objektu **CloudMediaContext** za vlákna. Třídy **CloudMediaContext** není podprocesů.
 
- Zvětšení NumberOfConcurrentTransfers s výchozí hodnotou 2 na hodnotu vyšší jako 5. Nastavení této vlastnosti ovlivní všechny výskyty **CloudMediaContext**. 
 
- Ponechat ParallelTransferThreadCount u výchozí hodnotu 10.
 
##<a id="ingest_in_bulk"></a>Ingesting prostředky hromadně pomocí Media Services .NET SDK 

Nahrávání souborů velké materiálů může být kritický bod při vytváření materiálů. Ingesting vybavení hromadné nebo "Hromadné Ingesting", zahrnuje oddělení materiálů vytvoření procesu nahrát. Použití hromadné ingesting přístup, vytvořte manifestu (IngestManifest), který popisuje majetek a souvisejících souborů. Potom použijte metodu nahrát podle svého výběru do kontejneru objektů blob manifestu nahrát souvisejících souborů. Microsoft Azure Media Services sleduje kontejneru objektů blob přidružené manifest. Po nahrání souboru do kontejneru objektů blob Microsoft Azure Media Services provede vytváření materiálů podle konfigurace materiálů v manifestu (IngestManifestAsset).


Vytvoření nového volání IngestManifest metodu vytvořit zveřejněné příslušným kolekci IngestManifests na CloudMediaContext. Tento způsob vytvoří nový IngestManifest seznamu název, který zadáte.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Vytvořte prostředky, které bude přidružený k hromadné IngestManifest. Konfigurace možností požadované šifrování majetku pro ingesting hromadně.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset přiřadí aktivum hromadné IngestManifest pro ingesting hromadně. Přiřadí taky AssetFiles tvořící každý materiálů. Pokud chcete vytvořit IngestManifestAsset, použijte metodu vytvořit v kontextu serveru.

Následující příklad ukazuje, přidání dva nové IngestManifestAssets, které spojení dvou prostředky dřív vytvořili hromadné jedí manifestu. Každý IngestManifestAsset taky přiřadí sadu souborů, které bude možné odeslat pro každý majetek během hromadného ingesting.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Můžete použít libovolné vysokorychlostní klientské aplikace, může nahrávání souborů materiálů do kontejneru úložiště objektů blob URI poskytovanou vlastnost **IIngestManifest.BlobStorageUriForUpload** IngestManifest. Jedna důležitá vysokorychlostní nahrát služba je [Aspera na vyžádání aplikace Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Můžete také vytvořit kód nahrát soubory prostředky, jak je vidět v následujícím příkladu.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Kód nahrávání souborů materiály pro vzorku, použité v tomto tématu se zobrazují v následujícím příkladu.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Můžete zjistit průběhu hromadné ingesting pro všechny prostředky spojené se **IngestManifest** dotazování vlastnost statistiky **IngestManifest**. Aktualizujte si informace o průběhu, je nutné použít nové **CloudMediaContext** pokaždé, když hlasování vlastnost statistiky.

Následující příklad ukazuje, dotazování IngestManifest tak, že jeho **Id**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Nahrát soubory pomocí .NET SDK rozšíření 

V příkladu níže ukazuje, jak nahrát tvořená jedním souborem pomocí .NET SDK rozšíření. V tomto případě **CreateFromFile** metoda slouží k, ale asynchronní verze je také k dispozici (**CreateFromFileAsync**). Metoda **CreateFromFile** umožňuje zadat název souboru, možnost šifrování a zpětné abyste mohli vykazovat průběh nahrát soubor.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Následující příklad volá UploadFile funkci a určuje úložiště šifrování jako možnost vytvoření materiálů.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Další krok

Teď jste nahráli aktivum Media Services, přejděte na téma [Jak získat procesor média][] .

[Jak získat procesor médií]: media-services-get-media-processor.md
 
