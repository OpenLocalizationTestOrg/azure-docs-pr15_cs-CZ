<properties 
    pageTitle="Publikování obsahu Azure Media Services pomocí .NET" 
    description="Naučte se vytvářet locator, který slouží k vytváření datových proudů adresy URL. Ukázky napsané v jazyce C# a použití Media Services SDK .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Publikování obsahu Azure Media Services pomocí .NET
 
> [AZURE.SELECTOR]
- [ZBÝVAJÍCÍ](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portál](media-services-portal-publish.md)

##<a name="overview"></a>Základní informace

Můžete vysílat datovými proudy adaptivní bitrate MP4 nastavil vytváření datových proudů locator OnDemand a vytváření datových proudů adresy URL. [Kódování aktivum](media-services-encode-asset.md) téma ukazuje, jak kódování do adaptivní bitrate, nastavený MP4. 

>[AZURE.NOTE]Pokud obsahu musí být zašifrovaný, nakonfigurujte materiálů doručení zásad (popsanou v [tomto](media-services-dotnet-configure-asset-delivery-policy.md) tématu) před vytvořením locator. 

Můžete taky OnDemand streamování locator vytvářet URL odkazujícími na MP4 soubory, které se dají postupně stáhnout.  

Toto téma ukazuje, jak vytvořit OnDemand streamování locator při publikovat svůj materiálů a vytvořit hladké, MPEG POMLČKU a HLS streamování adresy URL. Zobrazí také žádanou vytvářet adresy URL postupné stahování. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Vytvoření OnDemand streamování locator

K vytvoření OnDemand streamování locator a získání adresy URL je potřeba postupujte takto:

   1. Pokud je zašifrován obsah, definujte zásady přístupu.
   2. Vytvoření OnDemand streamování locator.
   3. Pokud budete chtít vysílat, potřebujete streamování seznamu souboru (.ism) v majetek. 
        
    Pokud budete chtít postupně stáhnout, potřebujete názvy souborů MP4 v majetek.  
   4. Vytvoření adresy URL k seznamu souboru nebo souborům MP4. 
   

###<a name="use-media-services-net-sdk"></a>Použití Media Services .NET SDK 

Vytváření datových proudů adresy URL 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Výstupy kód:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Vysílat datovými proudy obsah přes připojení SSL. K tomuto účelu zkontrolujte, že streamování adresy URL začínat HTTPS. 

Vytvoření postupné stahování adresy URL 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Výstupy kód:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Použití Media Services .NET SDK rozšíření

Následující kód volá .NET SDK rozšíření umožňující vytvořit locator a generovat hladký přenos, HLS a adresy URL MPEG-POMLČKU adaptivní datového proudu.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky

[Stáhněte si aktiv](media-services-deliver-asset-download.md)
[Konfigurace zásad doručení materiálů](media-services-dotnet-configure-asset-delivery-policy.md)
