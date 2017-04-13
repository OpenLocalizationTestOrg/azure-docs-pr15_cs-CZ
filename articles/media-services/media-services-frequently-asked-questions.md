<properties 
    pageTitle="Nejčastější dotazy | Microsoft Azure" 
    description="Nejčastější dotazy" 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Nejčastější dotazy

##<a name="general-ams-faqs"></a>Obecné AMS časté otázky

Otázka: jak můžete změnit velikost indexování?

Odpověď: rezervovaná jednotky jsou stejné kódování a indexování úkolů. Postupujte podle pokynů o [tom, jak měřítko kódování rezervovaná jednotky](media-services-scale-media-processing-overview.md). **Poznámka:** indexování výkon nebyl ovlivněn rezervovaná typ jednotky.

Otázka: můžu nahráli kódování a publikovaných na video. Co byste měli být důvod, proč videa se nepřehrává při pokusu o můžete vysílat datovými proudy ho?

Odpověď: jedna nejčastější příčiny je, že nebudete mít nejméně jednu rezervovaná streamování jednotku přidělit na streamování koncový bod, ze kterého chcete přehrávání.  Postupujte podle pokynů o [tom, jak měřítko streamování rezervovaná jednotky](media-services-portal-scale-streaming-endpoints.md).

Otázka: můžu skládání na živou toku?

A: skládání na živou proudy aktuálně nenabízí v Azure Media Services, tak by bylo nutné předem vytvořte v počítači.

Otázka: můžu používat Azure CDN s Live streamování?

A: Media Services podporuje integrace se službou Azure CDN (Další informace najdete v článku [jak spravovat streamování koncové body v účtu služby médií](media-services-portal-manage-streaming-endpoints.md)).  Můžete použít Live streamování s CDN. Azure Media Services poskytuje vyhlazenými výstupy datových proudů, HLS a MPEG-přerušované čáry. Tyto formáty pomocí protokolu HTTP pro přenos dat a získání výhody HTTP ukládání do mezipaměti. V živé datových proudů skutečné video nebo zvuk dat je rozdělen do neúplné a tento jednotlivé fragmenty získat uložené v CDN. Pouze data musí být aktualizovány jsou seznamu data. CDN aktualizuje pravidelně seznamu data.

Otázka: znamená Azure Media services podporují ukládání obrázků?

Odpověď: Pokud hledáte jenom ukládat ve formátu JPEG nebo PNG obrázky, máte zachovat odkazy v úložišti objektů Blob Azure. Neexistuje žádný přínos vložení Pokud chcete ponechat stále přidružené Video nebo zvuk prostředky ve vašem účtu Media Services. Nebo pokud bude pravděpodobně nutné použít obrázek jako překrytí v videa encoder. Media Encoder standardní podporuje obrázky překrývajícího horní části videa, a co seznamy JPEG a PNG jako podporovaná vstupní formáty. Další informace najdete v tématu [Vytvoření překrytí](media-services-custom-mes-presets-with-dotnet.md#overlay).

Otázka: Jak můžu zkopírovat aktiv z jednoho účtu Media Services do jiného.

Odpověď: Pokud chcete zkopírovat aktiv z jednoho účtu Media Services do jiného pomocí .NET, použijte metodu rozšíření [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) k dispozici v [Azure Media Services .NET SDK rozšíření](https://github.com/Azure/azure-sdk-for-media-services-extensions/) úložiště. Další informace najdete v tématu [podprocesu Fórum komunity](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) .

Otázka: co jsou podporované znaky pro pojmenování souborů při práci s AMS?

A: Media Services použije hodnotu vlastnost IAssetFile.Name při vytváření adresy URL pro streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu heslo percent-encoding není povolená. Hodnoty **název** vlastnosti nemůže obsahovat žádný z následujících [znaků procent kódování vyhrazené](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Navíc může být pouze jednu "." pro přípona názvu souboru.


Otázka: jak se připojit pomocí ZBÝVAJÍCÍ?

A: Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md). 


Otázka: jak můžete otočit videa během procesu kódování.

A: [Media Encoder standardní](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje otočení tak, že úhlu 90/180/270 stupňů. Výchozí chování je "Automaticky", kde pokusí ke zjištění metadata otočení v prostoru v souboru příchozí MOV nebo MP4 a nahradit ho. Obsahují následující prvek **zdroje** na jeden z json předvolby definovaný [tady](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
