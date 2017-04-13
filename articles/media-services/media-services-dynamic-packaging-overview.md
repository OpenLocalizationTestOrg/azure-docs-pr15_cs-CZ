<properties
    pageTitle="Přehled dynamické balení | Microsoft Azure"
    description="Téma poskytuje a základní informace o dynamické obalu."
    authors="Juliako"
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
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynamické balení

##<a name="overview"></a>Základní informace

Ochrana obsahu formáty k různým klient technologií a Microsoft Azure Media Services mohou sloužit k předvádění mnoho zdroj formáty multimediálních souborů, vysílání formátů médií (například iOS XBOX, Silverlight, Windows 8). Tyto klienty pochopit různé protokoly, například iOS vyžaduje formátu V4 HTTP Live datových proudů (HLS) a Silverlightu a Xbox vyžadují hladký přenos. Pokud máte sadu adaptivní bitrate (s víc bitrate) MP4 soubory (ISO Base médií 14496 12) nebo sadu hladký přenos souborů adaptivní bitrate, které chcete vytisknout klientům pochopit MPEG ČÁRKU, HLS nebo hladký přenos, měli využívat výhod dynamické zabalení Media Services.

Dynamické balení, které bude nutné stačí vytvořit materiálů, který obsahuje sadu adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos souborů. Potom podle zadaného formátu v pozvánce manifestu nebo neúplné na vyžádání datových proudů serveru zajistí budete dostávat proudu v protokol, který jste zvolili. Jako výsledek stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta.

Na následujícím obrázku vidíte tradiční kódování a statické balení pracovního postupu.

![Statická kódování](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Na následujícím obrázku vidíte dynamický balení pracovního postupu.

![Dynamické kódování](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Umožní využít výhod dynamické balení musíte nejprve získat nejméně jednu jednotku datových proudů na vyžádání pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [jak měřítko Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Běžné scénáře

1. Nahrání vstupní souboru (s názvem souboru mezzanine). Například H.264 MP4 nebo WMV (pro seznam [Formátů nepodporuje standardní Media Encoder](media-services-media-encoder-standard-formats.md)najdete v článku podporované formáty.

1. Kódování souboru mezzanine k sadám adaptivní bitrate H.264 MP4.

1. Publikujte materiálů, která obsahuje adaptivní přenosová MP4 nastavit tak, že vytvoříte na vyžádání Locator.

1. Vytváření datových proudů adresy URL a přístup k obsahu můžete vysílat datovými proudy.


##<a name="preparing-assets-for-dynamic-streaming"></a>Příprava prostředky pro dynamické streamování

Příprava vaší materiálů dynamické streamování máte dvě možnosti:

1. [Nahrání souboru předlohy](media-services-dotnet-upload-files.md).
2. [Použití Media Encoder standardní encoder k vytvoření sady adaptivní bitrate H.264 MP4](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Toku obsahu](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Formáty, které nejsou podporovány dynamické balení

Dynamické balení nejsou podporovány následující formáty souborů zdroje.

- Zvuk Dolby digital mp4 soubory.
- Zvuk Dolby digital vyhlazenými soubory.

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
