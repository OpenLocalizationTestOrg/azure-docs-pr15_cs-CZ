<properties 
    pageTitle="Jak vytvořit médií procesor | Microsoft Azure" 
    description="Naučte se vytvářet součásti procesoru médií kódovat, převedení formátu, šifrování nebo dešifrování obsahu médií pro službu Azure Media Services. Ukázky napsané v jazyce C# a použití Media Services SDK .NET." 
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


#<a name="how-to-get-a-media-processor-instance"></a>Postup: získání instanci procesor médií

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [ZBÝVAJÍCÍ](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Základní informace

V Media Services procesor médií je součást, která zajišťuje konkrétní zpracování úkolů, například kódování formátujte převodu šifrování nebo dešifrování mediální obsah. Obvykle vytvoříte médií procesoru při vytváření úkolu kódování, šifrování nebo převodu formátu obsahu médií.

Následující tabulka obsahuje název a popis procesorech dostupné média.

Název procesor média|Popis|Další informace
---|---|---
Standardní Media Encoder|Poskytuje standardní možnosti na vyžádání kódování. |[Přehled a porovnání Azure Media kodéry služba](media-services-encode-asset.md)
Pracovní postup Premium Media Encoder|Umožňuje kódování úlohy pomocí Media Encoder Premium pracovního postupu.|[Přehled a porovnání Azure Media kodéry služba](media-services-encode-asset.md)
Indexování Azure Media| Umožňuje vytvořit mediální soubory a obsahu s možností vyhledávání, stejně jako generovat skrytých titulků skladeb a klíčových slov.|[Indexování Azure Media](media-services-index-content.md)
Azure Media Hyperlapse (verze preview)|Umožňuje hladce, "hrboly" ve svém videu s videa ustálení. Lze také dosažení vyššího obsahu do spotřební klipartu.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Odepsat
Dešifrování úložiště| Odepsat|
Azure Media Balíčkovač|Odepsat|
Azure Media Encryptor|Odepsat|

##<a name="get-media-processor"></a>Získání procesor médií

Podle pokynů pro ukazuje, jak získat instanci procesor média. Příklad kódu předpokládá využívání proměnnou úroveň modulu s názvem **_context** neodkazuje kontextu serveru podle popisu v části [jak: připojení k Media Services programově](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Další kroky

Teď když víte, jak získat instanci procesor média, přejděte na téma [jak kódovat aktivum](media-services-dotnet-encode-with-media-encoder-standard.md) , které vám ukáže, jak používat Media Encoder Standard kódování aktivum.


