<properties 
    pageTitle="Přehrávání obsahu | Microsoft Azure" 
    description="Toto téma obsahuje seznam existující hráčů, slouží k přehrávání obsahu." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Přehrávání obsahu s existující přehrávače

Azure Media Services podporuje mnoho oblíbených streamování formáty, jako jsou hladký přenos Live streamování HTTP a MPEG-přerušované čáry. Toto téma odkazuje na existující hráčů, které můžete použít k testování vaší proudů.

>[AZURE.NOTE]Přehrát dynamicky sbalí nebo dynamicky šifrovaného obsahu, ujistěte se, že zobrazíte alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete dodávat obsah. Informace o měřítka streamování jednotky najdete v tématu: [postup měřítko streamování jednotky](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portálu Media Services obsahu player.

Na portálu **Azure** poskytuje obsahu player, které můžete použít k testování videa.

Klikněte na požadované video (zkontrolujte byl [publikován](media-services-portal-publish.md)) a klikněte na tlačítko **Přehrát** v dolní části na portálu.

Některé aspekty, které platí:

- **Přehrávač obsahu služby** se přehrává výchozí streamování koncového bodu. Pokud chcete přehrát z jiné než výchozí streamování koncového bodu, použijte jiný player. Například [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure Media Player

Pomocí [Azure Media Playeru](http://amsplayer.azurewebsites.net/azuremediaplayer.html) přehrávání obsahu (Vymazat nebo chráněné) v některém z následujících formátů:

- Hladký přenos
- MPEG POMLČKA
- HLS
- Postupné MP4


###<a name="flash-player"></a>Flash Playeru

####<a name="aes-encrypted-with-token"></a>Šifrování AES s tokenu

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Přehrávač Silverlight

####<a name="monitoring"></a>Sledování

[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady s tokenu

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Přehrávač přerušované čáry

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Další

Chcete-li otestovat adresy URL HLS můžete taky použít:

- **Safari** na zařízení s iOS nebo
- **3ivx HLS přehrávače** ve Windows.

##<a name="developing-video-players"></a>Vývoj videopřehrávače

Informace o tom, jak vývoj vlastních přehrávače najdete v tématu [rozvojových videopřehrávače](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
