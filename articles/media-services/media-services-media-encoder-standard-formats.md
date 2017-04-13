<properties 
    pageTitle="Media Encoder standardní formáty a kodeky" 
    description="Toto téma obsahuje přehled Media Encoder standardní formáty a kodeky." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder standardní formáty a kodeky


Tento dokument obsahuje seznam nejběžnějších import a export formátů, které můžete použít s Media Encoder standardní.


##<a name="input-containerfile-formats"></a>Při zadávání kontejneru/formátech

Formáty souborů (přípony souborů)|Podporované
---|---|---|---
FLV (kodeky H.264 a AAC) (.flv)          |Ano 
MXF (.mxf)                  |Ano 
GXF (.gxf)                  |Ano 
MPEG2-PS MPEG2 TS, 3GP (.ts PS, .3gp, .3gpp, MPG)   |Ano 
Windows Media Video (WMV) / ASF (soubor .wmv, ASF) |Ano 
AVI (nekomprimované 8 bitů/10 bitů) (AVI)|Ano 
MP4 (M4A, MP4, m4v) / ISMV (.isma, .ismv)|Ano 
[Microsoft Digital videa Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) dvr ms) |Ano 
Matroska/WBEM (.mkv)        |Ano 
Vlnovka/WAV (WAV) |Ano 
QuickTime (.mov) |Ano

>[AZURE.NOTE] Výše je seznam běžně došlo k přípony. Media Encoder standardní podporuje dalších (například: .m2ts, .mpeg2video, .qt). Pokud se pokusíte kódování souboru a zobrazí se chybová zpráva o formát nejsou podporované, přejděte prosím zpětnou vazbu [zde](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Formáty zvuku v zadávání kontejnery 

Media Encoder standardní podporuje, provádění následující formáty zvuku v zadávání kontejnery:

- MXF, GXF a QuickTime soubory, které mají zvukových stop s prokládaném stereo nebo 5.1 vzorky

nebo

- Soubory MXF, GXF a QuickTime kdy zvuk provádí jako samostatný PCM skladeb, ale kanálu mapování (stereo nebo 5.1) může být odvozeno z metadata souboru

Poznámka: podporující pro mapování explicitní/uživatelský kanálů se dozvíte v blízké budoucnosti.


##<a name="input-video-codecs"></a>Zadávání Videokodeky

Zadávání Videokodeky|Podporované
---|---|---|---
AVC bit/10 – 8bitovou, až 4:2:2, včetně AVCIntra   |8 bit 4:2:0 až 4:2:2 
Avid DNxHD (v MXF)                                 |Ano 
DVCPro/DVCProHD (v MXF)                            |Ano 
Digitální video (digitální) (v souborech AVI)                   |Ano
JPEG 2000                                           |Ano 
MPEG-2 (až 422 profilu a vysoké úrovni, včetně varianty například XDCAM XDCAM HD, XDCAM IMX, CableLabs® a D10)|Až 422 profilu 
MPEG-1                                              |Ano 
VC-1/WMV9                                           |Ano 
Canopus Centrála/HQX                                      |Ne 
MPEG-4 při instalaci část 2                                       |Ano 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ano 
YUV420 nekomprimované nebo mezzanine                   |Ano
Apple ProRes 422                                    |Ano
Apple ProRes 422 LT |Ano
Apple ProRes 422 Centrála |Ano
Apple ProRes Proxy|Ano
Apple ProRes 4444 |Ano
Apple ProRes 4444 XQ |Ano



##<a name="input-audio-codecs"></a>Zadávání zvukové kodeky

Zadávání zvukové kodeky|Podporované
---|---|---|---
AAC (AAC-LC, AAC-HE a AAC-HEv2; až 5.1)|Ano 
MPEG vrstvy 2|Ano 
MP3 (MPEG-1 Audio Layer 3)|Ano 
Windows Media Audio|Ano 
WAV/PCM|Ano 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ano 
[Díle](http://go.microsoft.com/fwlink/?LinkId=822667) |Ano 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ano 
AMR (adaptivní více kurz)|Ano
AES (SMPTE 331 M a 302 M, AES3 – 2003)        |Ne 
Dolby® E                                    |Ne 
Dolby® Digital (AC3)                        |Ne 
Digitální Dolby® Plus (E-AC3)                 |Ne 


##<a name="output-formats-and-codecs"></a>Výstupních formátů a kodeky

Následující tabulka uvádí formáty kodecích a souborů, které jsou podporované pro export.


Formát souboru|Videokodeky|Zvukový kodek
---|---|---
MP4 <br/><br/>(včetně více bitrate MP4 kontejnery) |H.264 (vysoké, hlavní a profily podle směrného plánu)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2 TS |H.264 (vysoké, hlavní a profily podle směrného plánu)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky

[Kódování obsahu na vyžádání se Azure Media Services](media-services-encode-asset.md)

[Jak se Media Encoder Standard kódování](media-services-dotnet-encode-with-media-encoder-standard.md)
