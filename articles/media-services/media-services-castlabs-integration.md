<properties 
    pageTitle="Použití castLabs předvádění Widevine licence Azure Media Services | Microsoft Azure" 
    description="Tento článek popisuje, jak můžete Azure Media Services (AMS) při dynamické zašifrované AMS s PlayReady a Widevine DRMs proudu. Licenci PlayReady pochází z nepodporovanou Media Services PlayReady a Widevine licenci Doručená castLabs licenční server." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Použití castLabs předvádění Widevine licence Azure Media Services

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Základní informace

Tento článek popisuje, jak můžete Azure Media Services (AMS) při dynamické zašifrované AMS s PlayReady a Widevine DRMs proudu. Licenci PlayReady pochází z nepodporovanou Media Services PlayReady a Widevine licenci Doručená **castLabs** licenční server.

Pokud chcete přehrávání proudů chráněny CENC (PlayReady a/nebo Widevine) můžete použít [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Další informace najdete v článku [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) .

Následující diagram ukazuje nejvyšší úrovně Azure Media Services a Architektura integrace castLabs.

![integrace](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Typické systém nastavení

- Mediální obsah uložený ve AMS.
- Klíč ID obsahu klíčů uložené v castLabs a AMS.
- castLabs a AMS mají tokenu ověřování integrované. Následující části se zabývají tokeny ověřování. 
- Když požadavků klientů proudu video, obsah dynamicky zašifrovaných **Běžné šifrování** (CENC) a dynamicky sbalí AMS a přitom zajistit hladký přenos přerušované čáry. Jsme také předvádění PlayReady M2TS základní toku šifrování protokolu streamování HLS.
- Licenční PlayReady je načtená z kontingenčního seznamu AMS nepodporovanou a Widevine licenci je načtená z kontingenčního seznamu castLabs licenční server. 
- Přehrávač automaticky určí, které licence vzdáleně podle funkci platformy klienta. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Ověřování tokenu generování usnadňující licence

CastLabs a AMS podporují tokenu formátu JWT (JSON Web tokenu), který používá k ověření licence. 

###<a name="jwt-token-in-ams"></a>JWT token v AMS 

Následující tabulka popisuje JWT token v AMS. 

Vydavatel|Řetězec Vystavitel z vybraného zabezpečené tokenu Service (Služba tokenů zabezpečení)
---|---
Cílové skupiny|Cílové skupiny řetězec z použitých Služba tokenů zabezpečení
Nároky|Sadu deklarací
Neplatí před|Zahájení platnosti tokenu
Vypršení platnosti|Konec platnosti tokenu
SigningCredentials|Klíč, který je sdílen PlayReady licenční Server castLabs licenční Server a STS, může to být symetrickou nebo asymetrické klíče.

###<a name="jwt-token-in-castlabs"></a>JWT token v castLabs

Následující tabulka popisuje JWT token v castLabs. 

Jméno|Popis
---|---
optData|JSON řetězec obsahující informace o uživateli. 
CRT|Řetězec formátu JSON s informacemi o materiálů, jeho licenční práva informace a přehrávání.
IAT|Aktuální datum a čas v epocha.
jti|Jedinečný identifikátor o tento token (každého tokenu lze použít pouze jednou v systému castLabs).

##<a name="sample-solution-set-up"></a>Ukázka řešení nastavení 

[Ukázka řešení](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) se skládá ze dvou projektů:

-   Z konzoly aplikace, který slouží k nastavení omezení DRM aktivum již požití PlayReady a Widevine.
-   Webová aplikace ruce, tokenů, které by se měly považovat za velmi zjednodušené verzi Služba tokenů zabezpečení.


Použití aplikace konzoly:

1.  Změna app.config nastavit přihlašovací údaje AMS, castLabs přihlašovací údaje, STS konfigurace a sdílený klíč.
2.  Nahrajte aktivum do AMS.
3.  Získání UUID z nahraný materiálů a změňte řádek 32 v souboru Program.cs:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Použití Id_položky pro pojmenování majetku v systému castLabs (řádek 44 v souboru Program.cs).

    Je třeba nastavit Id_položky **castLabs**; musí být jedinečný alfanumerický řetězec.

5.  Spuštění programu.


Pokud chcete použít webové aplikace (Služba tokenů zabezpečení):

1.  Změna web.config obchodníka castlabs nastavení ID, konfiguraci Služba tokenů zabezpečení a sdíleného klíče.
2.  Nasazení Azure weby.
3.  Přejděte na web.

##<a name="playing-back-a-video"></a>Přehrávání videa

Přehrávání videa zašifrovaných běžné šifrování (PlayReady a/nebo Widevine) můžete [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Při spuštění aplikace konzoly, jsou s ozvěnou obsahu ID klíče a adresu URL Manifest.

1.  Otevřete novou kartu a spuštění vaše Služba tokenů zabezpečení: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Přejděte na [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Vložte adresu URL datového proudu.
4.  Klepnutím na zaškrtávací políčko **Upřesnit možnosti** .
5.  V rozevíracím seznamu **Zámek** zaškrtněte PlayReady a/nebo Widevine.
6.  Vložte token, kterou jste získali od svého STS do textového pole Token. 
    
    Nemusí serveru castLab licence "nosný =" předpona před tokenu. Proto před odesláním tokenu, odeberte.
7.  Aktualizujte player.
8.  By měl přehrávání videa.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
