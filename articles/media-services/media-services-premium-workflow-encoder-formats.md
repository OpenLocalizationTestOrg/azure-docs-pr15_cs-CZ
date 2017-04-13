<properties 
    pageTitle="Formáty Media Encoder Premium pracovního postupu a kodeky | Microsoft Azure" 
    description="Toto téma obsahuje přehled Media Encoder Premium pracovního postupu formáty formáty a kodeky" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Formáty Media Encoder Premium pracovního postupu a kodeky


>[AZURE.NOTE]Další otázky encoder premium e-mail mepd na Microsoft.com.
>
>Procesor médií Media Encoder Premium pracovního postupu popisované v tomto tématu není k dispozici v Číně. 

Tento dokument obsahuje seznam vstupní a výstupní soubor formáty a kodeky, které jsou podporovány veřejné verzi systému encoder **Media Encoder Premium pracovního postupu** .

[Media Encoder Premium Worflow vstupní formáty a kodeky](#input_formats)

[Media Encoder Premium Worflow výstupních formátů a kodeky](#output_formats)

**Pracovní postup Premium Media Encoder** podporuje titulků popsaná v [této](#closed_captioning) části. 


##<a id="input_formats"></a>Media Encoder Premium pracovního postupu při zadávání kodecích a formátech

V následující části jsou uvedeny kodecích a soubor formáty podporované procesoru médií předávat na vstupu.

###<a name="input-containerfile-formats"></a>Při zadávání kontejneru/formátech

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- Toky Transport MPEG-2
- MPEG-2 programové datové proudy
- MPEG-4 NEBO MP4
- Windows Media/ASF
- AVI (nekomprimované 8 bitů/10 bitů)

###<a name="input-video-codecs"></a>Zadávání Videokodeky

- AVC bit/10 – 8bitovou, až 4:2:2, včetně AVCIntra
- Avid DNxHD (v MXF)
- DVCPro/DVCProHD (v MXF)
- JPEG2000
- MPEG-2 (až 422 profilu a vysoké úrovni, včetně varianty například XDCAM XDCAM HD, XDCAM IMX, CableLabs® a D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Zadávání zvukové kodeky

- AES (SMPTE 331 M a 302 M, AES3 – 2003)
- Dolby® E
- Dolby® Digital (AC3)
- AAC (AAC-LC, AAC-HE a AAC-HEv2; až 5.1)
- MPEG vrstvy 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Media Encoder Premium pracovního postupu výstupních formátů a kodeky

V následující části jsou uvedeny kodecích a soubor formáty, které jsou podporované jako výstup z tohoto procesoru média.

###<a name="output-containerfile-formats"></a>Výstupních formátů souboru/kontejneru

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM a AS02)
- DPP (včetně AS11)
- GXF
- MPEG-4 NEBO MP4
- Windows Media/ASF
- AVI (nekomprimované 8 bitů/10 bitů)
- Hladký streamování formát souboru (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Výstup Videokodeky

- AVC (H.264; 8bitovou; až zviditelnění, úrovně 5.2; 4 K Ultra vysokém rozlišení; Uvnitř AVC)
- Avid DNxHD (v MXF)
- DVCPro/DVCProHD (v MXF)
- MPEG-2 (až 422 profilu a vysoké úrovni, včetně varianty například XDCAM XDCAM HD, XDCAM IMX, CableLabs® a D10)
- MPEG-1
- Windows Media Video/VC-1
- Vytvoření náhledů JPEG

###<a name="output-audio-codecs"></a>Výstupní zvukové kodeky

- AES (SMPTE 331 M a 302 M, AES3 – 2003)
- Dolby® Digital (AC3)
- Dolby® Digital Plus (E-AC3) až 7.1
- AAC (AAC-LC, AAC-HE a AAC-HEv2; až 5.1)
- MPEG vrstvy 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Podpora titulků

Na jedí, podporuje **Media Encoder Premium pracovního postupu** :

1. SCC souborů
1. Soubory SMPTE TT
1. CEA-608/CEA-708 – při provádění jako uživatelská data (SEI zprávy H.264 základní datové proudy, ATSC/53, SCTE20) nebo při provádění jako pomocné data MXF/GXF souborů se změnami
1. STL podnadpis souborů

Výstup jsou k dispozici následující možnosti:

1. CEA 608 k CEA 708 překladu
1. CEA-608/CEA-708 projít (vložené do zpráv SEI základní datových proudů H.264 nebo při provádění jako pomocné dat v souborech MXF)
1. SCC
1. SMPTE Načasovali Text (zdrojem CEA 608 za SMPTE RP2052, včetně vytváření souborů DFXP)
1. Soubor SRT titulků
1. Toky podnadpis DVB

Poznámka: některé z výše uvedených výstupních formátů jsou podporovány pro doručování prostřednictvím streamování v Azure Media Services.

##<a name="known-issues"></a>Známé problémy

Pokud vstupní videa neobsahuje titulky, výstup materiálů budou nadále obsahovat prázdný soubor TTML. 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
