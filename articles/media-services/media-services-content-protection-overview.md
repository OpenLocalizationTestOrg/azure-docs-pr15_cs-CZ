<properties 
    pageTitle="Ochrana obsahu přehled | Microsoft Azure" 
    description="Tento článek poskytuje přehled ochranu obsahu s Media Services." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Ochrana obsahu přehled


Microsoft Azure Media Services umožňuje zabezpečená médií od okamžiku, kdy ponechá počítače pomocí úložiště, zpracování a doručení. Media Services umožňuje poskytovat live a na vyžádání obsahu dynamicky zašifrovaných šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování) nebo na jakékoli hlavní DRMs: Microsoft PlayReady Google Widevine a Apple FairPlay. Media Services také poskytuje služby pro doručování AES klíče a DRM (PlayReady Widevine a FairPlay) licence oprávněnými klientům. 

Následující obrázek ukazuje ochranu obsahu pracovních postupů, které podporuje AMS. 

![Chránit pomocí PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Abyste mohli používat dynamické šifrování, musíte nejprve získat nejméně jednu jednotku streamování rezervovaná na streamování koncový bod, ze kterého chcete vysílat šifrovaného obsahu.

Toto téma vysvětluje [Koncepty a terminologie](media-services-content-protection-overview.md) pro Principy ochranu obsahu s AMS důležité. Téma také obsahuje [odkazy](media-services-content-protection-overview.md#common-scenarios) na témata, která ukazují, jak dosáhnout ochranu obsahu úkoly. 

##<a name="dynamic-encryption"></a>Dynamické šifrování

Microsoft Azure Media Services umožňuje poskytovat obsah dynamicky zašifrovaných AES zrušte zaškrtnutí políčka klíče nebo DRM šifrování: Microsoft PlayReady Google Widevine a Apple FairPlay.

V současnosti dá zašifrovat následujících datových proudů formátů: HLS MPEG POMLČKU a přitom zajistit hladký přenos. Nelze zašifrovat konfigurace streaming format nebo postupné stahování.

Pokud chcete pro Media Services šifrování aktivum, potřebujete přidružení šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) s vaší materiálů a taky nakonfigurovat se tak mohli ověřovat zásady pro klávesu.

Budete potřebovat pro nastavení zásad majetku doručení. Pokud chcete vysílat úložiště šifrované materiálů, ujistěte se, můžete určit, jak budete chtít poskytovat pomocí konfigurace zásad doručení materiálů.

Požádání proudu přehrávačem Media Services pomocí zadaného klíče dynamicky zašifrovat obsah pomocí AES zrušte zaškrtnutí políčka klíče nebo DRM šifrování. Dešifrovat proudu přehrávač bude požádat klávesu služby klíčové doručení. Služba rozhodnout, zda je uživatel oprávnění k získání klíče, vyhodnotí se tak mohli ověřovat zásad, které jste zadali pro klávesu.

>[AZURE.NOTE]Umožní využít výhod dynamické šifrování musíte nejprve získat nejméně jednu jednotku datových proudů na vyžádání streamování koncového bodu, ze které budete chtít doručení šifrované obsahu. Další informace najdete v tématu [jak měřítko Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Šifrování úložiště

Použití úložiště šifrování postup pro šifrování vymazat obsah místně pomocí šifrování AES 256 a pak ho nahrajte do úložiště Azure kde je uložena zašifrovat zbývající. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Případ použití primární šifrování úložiště je, když chcete zabezpečit vstupní mediální soubory kvalitní silné šifrování u ostatních na disku.

K provedení úložiště šifrované materiálů, musíte nakonfigurovat majetku doručení zásad, aby Media Services věděli, jak budete chtít doručení obsahu. Než vaše materiálů můžete streamují, serveru datových proudů odebere šifrování úložiště a přenáší datové proudy obsahu pomocí zásad dodací (například AES běžné šifrování a bez šifrování).

## <a name="common-encryption-cenc"></a>Běžné šifrování (CENC)

Běžné šifrování se používá při šifrování obsahu s PlayReady nebo / a Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Pomocí cbcs aapl šifrování

Cbcs aapl se používá při šifrování obsahu s FairPlay.

## <a name="envelope-encryption"></a>Šifrování obálky 

Tuto možnost použijte, pokud chcete ochranu obsahu vymazat klíčem AES 128. Pokud budete potřebovat bezpečnější možnosti, zvolte jednu z DRMs uvedené v tomto tématu. 

##<a name="licenses-and-keys-delivery-service"></a>Služba pro doručování licence a klíče

Media Services poskytuje služby pro doručování DRM (PlayReady Widevine, FairPlay) licence a AES zrušte klávesy se šipkami oprávněnými klienty. Můžete [portálu Azure](media-services-portal-protect-content.md)a rozhraní REST API, Media Services SDK pro .NET pro nastavení ověřování a povolení zásady pro licence a klíče.

##<a name="token-restriction"></a>Tokenu omezení

Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření nebo token omezení. Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny jednoduché Web tokeny (SWT) a formátem JSON Web tokenu (JWT). Media Services neposkytuje zabezpečené tokenu služby. Můžete vytvořit vlastní STS nebo využít Microsoft Azure ACS tokenů problému. STS musí být nakonfigurované pro vytvoření token podepsané zadaný deklarace spolu s problém, které jste zadali v konfiguraci tokenu omezení. Služba klíčové doručení Media Services vrátí požadované klíč (nebo licenci) klienta, pokud platí tokenu a deklarací tokenu se shodují můžou být nakonfigurované pro klíč (nebo licence).

Při konfiguraci tokenu omezení zásad, je třeba určit ověření primární klíč, Vystavitel a parametrech cílové skupiny. Ověření primární klíč obsahuje klíč, který byl podepsán tokenu, které vystavitel služba zabezpečené tokenu problémy tokenu. Cílové skupiny (někdy se jí říká obor) popisuje záměr tokenu nebo zdroje tokenu povolí přístup k. Služby klíčové doručení Media Services ověří, že tyto hodnoty v tokenu odpovídat hodnotám na šabloně.

##<a name="streaming-urls"></a>Streamování adresy URL

Pokud vaše materiálů zašifrované s více než jeden DRM, měli byste použít značku šifrování streamování adresy URL: (formát = "m3u8-aapl" šifrování = "xxx").

Platí následující omezení:

- Lze zadat jenom nula nebo jeden typ šifrování.
- Typ šifrování nemá být zadán v adrese url-li pouze jeden šifrování byl použitý materiálů.
- Typ šifrování je malá a velká písmena.
- Lze zadat následující typy šifrování:  
    - **cenc**: běžné šifrování (Playready nebo Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: šifrování AES obálky.

##<a name="common-scenarios"></a>Obvyklé scénáře

V následujících tématech ukazují, jak Ochrana obsahu v úložišti, poskytovat dynamicky šifrované streamování média, pomocí služba pro doručování AMS klíč/licence

- [Chránit pomocí AES](media-services-protect-with-aes128.md) 
- [Chránit pomocí PlayReady a/nebo Widevine](media-services-protect-with-drm.md)
- [Vaše HLS obsahu chráněného s Apple FairPlay a/nebo PlayReady můžete vysílat datovými proudy](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Další scénáře

- [Integrace služby Azure PlayReady licenci s encryptor/streamování serveru](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [Použití castLabs předvádění DRM licence Azure Media Services](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Související odkazy

[Oznámení o PlayReady jako dynamické šifrování AES s Azure Media Services a služby](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licenci doručení cena za vysvětlení](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Ladění AES šifrované tok v Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Tokenu authenitcation JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrace Azure Media Services OWIN MVC na základě aplikace pomocí služby Azure Active Directory a omezení doručování obsahu klíčové založené na deklaraci JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Použití Azure ACS k tokenů problému](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
