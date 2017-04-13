<properties 
    pageTitle="Vytváření filtrů s Azure Media Services .NET SDK" 
    description="Toto téma popisuje, jak vytvářet filtry tak svému klientovi můžete používat k určité oddíly toku datového proudu. Media Services vytvoří dynamické manifesty dosáhnout výběrové streamování." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Vytváření filtrů s Azure Media Services .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [ZBÝVAJÍCÍ](media-services-rest-dynamic-manifest.md)

Začnete s 2.11 vydání, Media Services můžete definovat filtry majetku. Pravidla na straně serveru, které vám umožní zákazníky rozhodnete provádět věci, jako jsou tyto filtry: přehrávání pouze části videa (místo přehrávání celé video), nebo zadat jenom dílčí verze zvuku a videa, které zařízení zákazníka zpracovat (ne všechny verze, které jsou přidružené k majetku). Tento filtrování svém majetku dosáhnete pomocí **Dynamického Manifest**s vytvořenou požádání zákazníka ke streamování videa podle zadaného filtry.

Podrobnější informace související s filtry a dynamické manifestu najdete v tématu [Přehled dynamické manifesty](media-services-dynamic-manifest-overview.md).

Toto téma ukazuje, jak používat Media Services .NET SDK vytvářet, aktualizovat a odstraňovat filtry. 


Poznámka: Pokud aktualizujete filtru, může trvat až 2 minuty datového proudu koncový bod k aktualizaci pravidla. Pokud obsah byl doručen pomocí tohoto filtru (a uložené v proxy servery a CDN mezipaměti), aktualizace tento filtr může dojít k chybám player. Je doporučená vymazání mezipaměti po aktualizaci filtr. Pokud tato možnost není možné, zvažte použití různých filtru. 

##<a name="types-used-to-create-filters"></a>Typy používá k vytváření filtrů

Následující typy se používají při vytváření filtrů: 

- **IStreamingFilter**.  Toto je založena na následující rozhraní REST API [filtru](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Tento typ je založená na následující rozhraní REST API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Tento typ je založená na následující rozhraní REST API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** a **IFilterTrackPropertyCondition**. Tyto typy jsou založené na následujících rozhraní REST API [FilterTrackSelect a FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Vytvoření, aktualizace nebo pro čtení nebo odstranění globální filtry

Následující kód ukazuje, jak pomocí .NET můžete vytvářet, aktualizovat, přečtěte si nebo odstranit materiálů filtry.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Vytvoření, aktualizace nebo pro čtení nebo odstranění materiálů filtry

Následující kód ukazuje, jak pomocí .NET můžete vytvářet, aktualizovat, přečtěte si nebo odstranit materiálů filtry.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Vytváření datových proudů adresy URL, které pomocí filtrů

Informace o tom, jak publikovat a k předání vašeho majetku najdete v tématu [Doručování obsahu na zákazníky přehled](media-services-deliver-content-overview.md).


Následující příklady ukazují, jak přidat filtry streamování adresy URL.

**MPEG POMLČKA** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live streamování V4 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live streamování V3 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Hladký přenos**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**KONFIGURACE**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Viz taky 

[Dynamické manifesty přehled](media-services-dynamic-manifest-overview.md)
 

