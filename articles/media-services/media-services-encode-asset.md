<properties 
    pageTitle="Přehled a porovnání Azure na službu médií kodéry | Microsoft Azure" 
    description="Toto téma obsahuje přehled a porovnání Azure na službu médií kodéry." 
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

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Přehled a porovnání Azure na službu médií kodéry

##<a name="encoding-overview"></a>Kódování přehled

Azure Media Services obsahuje více možností pro šifrování médií v cloudu.

Když začnete s Media Services, je důležité pochopit rozdíl mezi formáty kodecích a soubor.
Kodeky jsou software implementující algoritmů kompresi/dekompresi vzhledem k tomu formátech souborů jsou kontejnery, které obsahují komprimované video.

Media Services poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 a přitom zajistit hladký přenos zakódovaný úprav textu v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) aniž by bylo nutné znovu balíček do těchto streamování formáty.

Umožní využít výhod [dynamické balení](media-services-dynamic-packaging-overview.md)potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos (kódování kroky jsou prokázáno dál v tomto kurzu).
- Potřebujete alespoň jednu jednotku streamování na vyžádání pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [jak okamžitou streamování rezervovaná jednotkách](media-services-portal-manage-streaming-endpoints.md).

Media Services podporuje následující na službu kodéry, které jsou popsané v tomto článku:

- [Standardní Media Encoder](media-services-encode-asset.md#media-encoder-standard)
- [Pracovní postup Premium Media Encoder](media-services-encode-asset.md#media-encoder-premium-workflow)

Tento článek obsahuje stručný přehled na vyžádání kodéry média a obsahuje odkazy na články poskytující podrobnější informace. V tématu také poskytuje srovnání kodéry.

Všimněte si, že ve výchozím nastavení každý účet Media Services můžete mít jednoho aktivního úkolu kódování po druhém. Kódování jednotky, které umožňují více kódování úkoly používajícího souběžně pro každou kódování rezervovaná jednotku, zakoupených můžete rezervovat. Informace najdete v tématu [Měřítko kódování jednotky](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Standardní Media Encoder

###<a name="how-to-use"></a>Jak používat

[Jak se Media Encoder Standard kódování](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Formáty

[Formáty a kodeky](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Předvolby

Media Encoder standardní se konfigurují nějaká encoder předvolby popsané [tady](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Vstupní a výstupní metadat

Zadávání metadat kodéry je popsán [v tomto poli](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Metadata výstup kodéry je popsán [v tomto poli](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Generování miniatur

Informace najdete v tématu [jak generovat miniatury pomocí Media Encoder standardní](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Střih videa (omezení)

Informace najdete v tématu [se střihu videoklipů pomocí Media Encoder standardní](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Vytvoření překrytí

Informace najdete v článku [jak vytvářet překrytí pomocí Media Encoder standardní](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Viz taky

[Blog Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Pracovní postup Premium Media Encoder

###<a name="overview"></a>Základní informace

[Úvodní informace o Premium kódování v Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Jak používat

Media Encoder Premium pracovního postupu pomocí složitých pracovních postupů. Pracovní postup soubory nelze vytvořit a aktualizované pomocí nástroje pro [Návrháři pracovního postupu](media-services-workflow-designer.md) .

[Jak používat Premium kódování v Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Známé problémy

Pokud vstupní videa neobsahuje titulky, výstup materiálů budou nadále obsahovat prázdný soubor TTML. 


##<a id="compare_encoders"></a>Porovnání kodéry

###<a id="billing"></a>Fakturace metr používaný každý encoder

Název procesor média|Použít ceny|Poznámky
---|---|---
**Standardní Media Encoder** |ENCODER|Kódování úkoly strhne příslušná podle velikosti výstup materiálů v GB sazbou zadané [tady][1], ve sloupci ENCODER.
**Pracovní postup Premium Media Encoder** |PREMIUM ENCODER|Kódování úkoly strhne příslušná podle velikosti výstup materiálů v GB sazbou zadané [tady][1], ve sloupci PREMIUM ENCODER.


V této části porovnává funkce kódování **Media Encoder standardní** a **Media Encoder Premium pracovního postupu**.


###<a name="input-containerfile-formats"></a>Při zadávání kontejneru/formátech

Při zadávání kontejneru/formátech|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
Adobe® Flash® F4V           |Ano|Ano
MXF/SMPTE 377M              |Ano|Ano
GXF                         |Ano|Ano
Toky Transport MPEG-2    |Ano|Ano
MPEG-2 programové datové proudy      |Ano|Ano
MPEG-4 NEBO MP4                  |Ano|Ano
Windows Media/ASF           |Ano|Ano
AVI (nekomprimované 8 bitů/10 bitů)|Ano|Ano
3GPP/3GPP2                  |Ano|Ne
Hladký streamování formát souboru (PIFF 1.3)|Ano|Ne
[Microsoft Digital videa Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ano|Ne
Matroska/WBEM               |Ano|Ne
QuickTime (.mov) |Ano|Ne

###<a name="input-video-codecs"></a>Zadávání Videokodeky

Zadávání Videokodeky|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
AVC bit/10 – 8bitovou, až 4:2:2, včetně AVCIntra   |8 bit 4:2:0 až 4:2:2|Ano
Avid DNxHD (v MXF)                                 |Ano|Ano
DVCPro/DVCProHD (v MXF)                            |Ano|Ano
JPEG2000                                            |Ano|Ano
MPEG-2 (až 422 profilu a vysoké úrovni, včetně varianty například XDCAM XDCAM HD, XDCAM IMX, CableLabs® a D10)|Až 422 profilu|Ano
MPEG-1                                              |Ano|Ano
Windows Media Video/VC-1                            |Ano|Ano
Canopus Centrála/HQX                                      |Ne|Ne
MPEG-4 při instalaci část 2                                       |Ano|Ne
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ano|Ne
Apple ProRes 422    |Ano|Ne
Apple ProRes 422 LT |Ano|Ne
Apple ProRes 422 Centrála |Ano|Ne
Apple ProRes Proxy|Ano|Ne
Apple ProRes 4444 |Ano|Ne
Apple ProRes 4444 XQ |Ano|Ne

###<a name="input-audio-codecs"></a>Zadávání zvukové kodeky

Zadávání zvukové kodeky|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
AES (SMPTE 331 M a 302 M, AES3 – 2003)        |Ne|Ano
Dolby® E                                    |Ne|Ano
Dolby® Digital (AC3)                        |Ne|Ano
Digitální Dolby® Plus (E-AC3)                 |Ne|Ano
AAC (AAC-LC, AAC-HE a AAC-HEv2; až 5.1)|Ano|Ano
MPEG vrstvy 2|Ano|Ano
MP3 (MPEG-1 Audio Layer 3)|Ano|Ano
Windows Media Audio|Ano|Ano
WAV/PCM|Ano|Ano
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ano|Ne
[Díle](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ano|Ne
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ano|Ne


###<a name="output-containerfile-formats"></a>Výstupních formátů souboru/kontejneru

Výstupních formátů souboru/kontejneru|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
Adobe® Flash® F4V|Ne|Ano
MXF (OP1a, XDCAM a AS02)|Ne|Ano
DPP (včetně AS11)|Ne|Ano
GXF|Ne|Ano
MPEG-4 NEBO MP4|Ano|Ano
MPEG-TS|Ano|Ano
Windows Media/ASF|Ne|Ano
AVI (nekomprimované 8 bitů/10 bitů)|Ne|Ano
Hladký streamování formát souboru (PIFF 1.3)|Ne|Ano

###<a name="output-video-codecs"></a>Výstup Videokodeky

Výstup Videokodeky|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
AVC (H.264; 8bitovou; až zviditelnění, úrovně 5.2; 4 K Ultra vysokém rozlišení; Uvnitř AVC)|Pouze 8 bit 4:2:0|Ano
Avid DNxHD (v MXF)|Ne|Ano
MPEG-2 (až 422 profilu a vysoké úrovni, včetně varianty například XDCAM XDCAM HD, XDCAM IMX, CableLabs® a D10)|Ne|Ano
MPEG-1|Ne|Ano
Windows Media Video/VC-1|Ne|Ano
Vytvoření náhledů JPEG|Ne|Ano

###<a name="output-audio-codecs"></a>Výstupní zvukové kodeky

Výstupní zvukové kodeky|Standardní Media Encoder|Pracovní postup Premium Media Encoder
---|---|---
AES (SMPTE 331 M a 302 M, AES3 – 2003)|Ne|Ano
Dolby® Digital (AC3)|Ne|Ano
Dolby® Digital Plus (E-AC3) až 7.1|Ne|Ano
AAC (AAC-LC, AAC-HE a AAC-HEv2; až 5.1)|Ano|Ano
MPEG vrstvy 2|Ne|Ano
MP3 (MPEG-1 Audio Layer 3)|Ne|Ano
Windows Media Audio|Ne|Ano


##<a name="error-codes"></a>Kódy chyb  

Následující tabulka uvádí kódy chyb, které by mohly být vrácen v případě, že došlo k chybě při kódování provádění úkolů.  Podrobnosti o chybách v kódu .NET, použijte [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) předmětu. Podrobnosti o chybách v kódu REST, použijte rozhraní REST API [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) .

ErrorDetail.Code|Možné příčiny chyb
-----|-----------------------
Neznámý| Neznámá chyba při provádění úkolu
ErrorDownloadingInputAssetMalformedContent|Kategorie chyb, které bude pokrývat chyby při stahování vstupní materiálů například názvy chybná souborů, soubory nulové délky, nesprávné formátů a tak dál.
ErrorDownloadingInputAssetServiceFailure|Kategorie chyby, které bude pokrývat problémy na straně služby – příklad sítě nebo úložiště chyby při stahování.
ErrorParsingConfiguration|Kategorie chyb kde úkolů <see cref="MediaTask.PrivateData"/> (konfigurace) není platná, například konfigurace není platný systém přednastavené nebo obsahuje neplatné XML.
ErrorExecutingTaskMalformedContent|Kategorie chyby při provádění úloh, kde problémy uvnitř vstupní mediální soubory způsobit selhání.
ErrorExecutingTaskUnsupportedFormat|Kategorie chyb kde procesor media se nedají zpracovat soubory k dispozici – prostě formátování médií není podporovaná nebo neodpovídá konfiguraci. Například se pokusíte vytvořit jenom zvukový výstup z materiálů, který obsahuje pouze video
ErrorProcessingTask|Kategorie jiné chyby, které procesor médií narazí během zpracování úkolu, které nesouvisí s obsahem.
ErrorUploadingOutputAsset|Kategorie chyby při ukládání výstupu materiálů
ErrorCancelingTask|Kategorie chyb pokrýval k chybám při pokusu o zrušení úkolu
TransientError|Kategorie chyb pokrýval přechodná problémy (např. dočasné problémy se sítí s úložištěm Azure)


Potřebujete pomoc od týmu služeb **Media Services** otevřete [podporují lístek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Související články

- [Provádět pokročilé kódování přizpůsobením Media Encoder standardní předvolby](media-services-custom-mes-presets-with-dotnet.md)
- [Kvót a omezení](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
