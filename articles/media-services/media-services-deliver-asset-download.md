<properties 
    pageTitle="Stáhněte si multimediálních materiálů" 
    description="Zjistěte, jak stáhnout aktiv s vaším počítačem. Ukázky napsané v jazyce C# a použití Media Services SDK .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Postup: předvádění aktivum stažením

Toto téma popisuje možnosti pro doručování multimediálních materiálů nahráli Media Services. Předvádění Media Services obsah v mnoha případech aplikace. Můžete si stáhnout multimediálních materiálů nebo k nim pomocí locator. Obsah média můžete odeslat do jiné aplikace nebo do jiného poskytovatele. Pro lepší výkon a škálovatelnost: můžete také doručení obsahu pomocí sítě doručování obsahu (CDN).

Tento příklad ukazuje, jak stáhnout multimediálních materiálů z Media Services do místního počítače. Kód vyhledá úlohy přidružené k účtu služby Media podle ID úlohy a přístup k jejich odběru **OutputMediaAssets** (což je sada jeden nebo více výstup multimediálních materiálů, která je výsledkem spuštění úlohy). Tento příklad ukazuje, jak stáhnout výstup multimediálních materiálů z úlohy, ale můžete použít stejný přístup ke stažení Další prostředky.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Viz taky 

[Předvádění streamování obsahu](media-services-deliver-streaming-content.md)

