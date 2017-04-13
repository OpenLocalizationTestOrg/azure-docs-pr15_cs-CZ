<properties 
    pageTitle="Můžete vyvíjet aplikace přehrávače videa" 
    description="Téma obsahuje odkazy na přehrávače rámce a moduly plug-in pro využívající vyvinout vlastní klientské aplikace, které můžete používat datových proudů médií od Media Services." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Můžete vyvíjet aplikace přehrávače videa

##<a name="overview"></a>Základní informace

Azure Media Services poskytuje nástroje, které potřebujete k vytvoření dynamických klientských přehrávače aplikací pro většinu platformy, včetně: iOS zařízení zařízení s Androidem, Windows, Windows Phone, Xbox a Set-top polí. Toto téma obsahuje také odkazy SDK a přehrávače rozhraními, využívající vyvinout vlastní klientské aplikace, které můžete používat datové proudy médií z Azure Media Services.

##<a name="azure-media-player"></a>Azure Media Player

[Azure Media Playeru](http://aka.ms/ampinfo) je web přehrávače videa předdefinovaných přehrávání média obsah z Microsoft Azure Media Services na nejrůznějších zařízení a prohlížeče. Azure Media Playeru využívá oborovými standardy, například HTML5, médií zdroj přípony (MSE) a zašifrovaných rozšíření médií (EME) obohacený adaptivní streamování prostředí. Když standardy jsou k dispozici na zařízení nebo v prohlížeči, Azure Media Playeru nepoužije Flashe a Silverlightu jako základní technologii. Bez ohledu na technologii přehrávání používá bude mít vývojáři jednotné JavaScript rozhraní pro přístup k rozhraní API. To umožňuje prohlížení službou Azure Media Services obsahu přehrát přes celou oblast zařízení a prohlížeče bez navíc úsilí.

Microsoft Azure Media Services umožňuje obsah, který podávané s POMLČKU hladký přenos a HLS streamování formáty přehrávání obsahu. Azure Media Playeru bere v úvahu tyto různé formáty a automaticky se přehraje nejlepší odkaz možnostem platformy/prohlížeče. Microsoft Azure Media Services umožňuje, aby dynamické šifrování aktiv pomocí šifrování PlayReady nebo obálky šifrování AES-128bitové. Azure Media Playeru umožňuje dešifrování PlayReady a AES 128 bit šifrovaného obsahu při konfiguraci řádně podporovat. 

Další informace:

- [Azure Media Player](http://aka.ms/ampinfo)
- [Si přečtěte následující dokumentaci Azure Media Player](http://aka.ms/ampdocs) 
- [Azure Media Playeru začíná začít blogu](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Registrace k upozorňovat na nové zprávy s nejnovějšími z Azure Media Player](http://aka.ms/ampsignup)
- [Přidání nových žádostí o funkci, nápady a názory](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Další nástroje pro vytváření aplikací přehrávače

Můžete taky používáte některou z následujících SDK:

- [Hladce streamování klienta SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Hladký streamování pro Windows Store](media-services-build-smooth-streaming-apps.md)
- [Platformu Microsoft médií: Framework přehrávače](http://playerframework.codeplex.com/) 
- [HTML5 Dokumentace Framework přehrávače](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft hladký přenos modul plug-in pro OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Licencování vyhlazenými streamování klienta Microsoft® přenos Kit](http://aka.ms/sspk) 
- [Vývoj videa aplikací XBOX](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Reklamní

Azure Media Services podporuje vkládání ad prostřednictvím platformy Windows Media: přehrávače rámce. Přehrávač rámce s podporou ad jsou k dispozici pro zařízení s Windows 8, Silverlight, Windows Phone 8 a iOS. Každý framework přehrávače obsahuje ukázkový kód, který se dozvíte, jak implementovat aplikace player. Existují tři různé druhy reklam, které se dají vložit do média:

Lineární – celého snímku reklam, které pozastavení hlavní videa

Nelineárních problémů – překryvném reklam, které se zobrazí při přehrávání hlavní videa, obvykle logo nebo jiný statický obrázek umístěná v rámci přehrávače

Průvodce vyhledáváním – reklam, které se zobrazují mimo přehrávač

Služby Active Directory mohou být umístěny v jakémkoli okamžiku hlavní video časová osa. Přehrávač musí řekněte přehrávaných ad a které reklam přehrát. Důvodem je pomocí sady standardní založeném na jazyce XML soubory: Video Ad služby šablony (VAST), více Ad stop (VMAP) ve digitální Video, šablony abstraktní klasifikace na médií (STOŽÁRŮ) a digitální Video přehrávače Ad rozhraní definice (VPAID). VELKÉ soubory zadejte, jaké reklam k zobrazení. Soubory VMAP určit, kdy má přehrát různé služby Active Directory a obsahují OBROVSKÉ XML. Další způsob, jak posloupnost reklam, které mohou také obsahovat OBROVSKÉ XML jsou STOŽÁRŮ soubory. Soubory VPAID definovat rozhraní mezi přehrávače videa a ad nebo ad server. Další informace najdete v tématu [Vložení služby Active Directory](https://msdn.microsoft.com/library/dn387398.aspx).

Informace o podpoře skryté titulky a služby Active Directory v Live streamování videa najdete v tématu [podporované titulky a standardy vložení Ad](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky

[Vložení MPEG-POMLČKU adaptivní streamování videa v aplikaci HTML5 s DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js úložiště](https://github.com/Dash-Industry-Forum/dash.js)
 
