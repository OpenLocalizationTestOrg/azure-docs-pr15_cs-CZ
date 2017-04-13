<properties 
    pageTitle="Použitím partnery předvádění Widevine licence Azure Media Services | Microsoft Azure" 
    description="Tento článek popisuje, jak můžete Azure Media Services (AMS) při dynamické zašifrované AMS s PlayReady a Widevine DRMs proudu. Licenci PlayReady pochází z nepodporovanou Media Services PlayReady a Widevine licenci Doručená castLabs licenční server." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Použitím partnery předvádění Widevine licence Azure Media Services

##<a name="overview"></a>Základní informace

Microsoft Azure Media Services umožňuje poskytovat chráněné pomocí Widevine DRM, který je zašifrován za specifikaci běžné šifrovací (CENC) MPEG-přerušované čáry.

Počínaje Media Services .NET SDK verze 3.5.2, Media Services umožňují konfigurovat Widevine licenci šablony a získat Widevine licence. Můžete také následující partnery AMS vám poskytovat Widevine licence: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

[CastLabs](http://castlabs.com/company/partners/azure/) umožňuje poskytovat Widevine licence. Další informace najdete v tématu [použití castLabs předvádění DRM licence Azure Media Services](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

[Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) umožňuje poskytovat Widevine licence. Další informace najdete v tématu [Použití Axinom předvádění DRM licence Azure Media Services](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky

[Použití PlayReady a/nebo Widevine dynamické běžné šifrování](media-services-protect-with-drm.md)

[Mingfei na blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

