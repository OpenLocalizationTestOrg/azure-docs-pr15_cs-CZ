<properties 
    pageTitle="Konfigurace obsahu klíč zásady se tak mohli ověřovat pomocí portálu Azure | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat zásad povolení obsahu klíče." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Konfigurace zásad obsahu klíčové povolení
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Základní informace

Microsoft Azure Media Services umožňuje poskytovat MPEG-POMLČKU hladký přenos a HTTP-Live – datových proudů (HLS) datových proudů chráněné pomocí šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS umožňuje poskytovat zašifrovaných Widevine DRM datových proudů přerušované čáry. PlayReady a Widevine jsou šifrovány za specifikaci běžné šifrování (CENC ISO/IEC 23001-7).

Media Services také poskytuje **Služba pro doručování klíč/licence** ze kterého můžete získat klientů klíči AES nebo PlayReady/Widevine licence k přehrání šifrované obsahu.

Toto téma ukazuje, jak používat portál Azure konfigurace zásad obsahu klíčové ověření. Klíč lze později dynamicky zašifrovat obsah. Poznámka aktuálně můžete zašifrovat následujících datových proudů formáty: HLS MPEG POMLČKU a přitom zajistit hladký přenos. Nelze zašifrovat konfigurace streaming format nebo postupné stahování.

Požaduje-li přehrávač toku, která je nastavená jako dynamicky zašifrovat, Media Services pomocí nakonfigurované klíče dynamicky zašifrovat obsah pomocí šifrování AES nebo DRM. Dešifrovat proudu přehrávač bude požádat klávesu služby klíčové doručení. Služba rozhodnout, zda je uživatel oprávnění k získání klíče, vyhodnotí se tak mohli ověřovat zásad, které jste zadali pro klávesu.


Pokud budete chtít mít více obsahu klíčů nebo jej zadejte adresu URL **Služba pro doručování klíč/licenci** než služby klíčové doručení Media Services, použijte Media Services .NET SDK rozhraní REST API.

[Konfigurace obsahu zásad klíč se tak mohli ověřovat pomocí Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurace obsahu zásad klíč se tak mohli ověřovat pomocí Media Services REST API](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Některé aspekty, které platí:

- Abyste mohli používat dynamické balení a dynamické šifrování, musíte mít aspoň jednu streamování rezervovaná jednotku. Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).
- Vaše materiálů musí obsahovat sadu adaptivní bitrate MP4s nebo Adaptivní bitrate hladký přenos souborů. Další informace najdete v tématu [kódovat aktivum](media-services-encode-asset.md).
- Služba pro doručování klíč ukládání do mezipaměti ContentKeyAuthorizationPolicy a jeho souvisejících objektů (Možnosti zásad a omezení) 15 minut.  Když vytvoříte ContentKeyAuthorizationPolicy a zadat použití "Token" omezení, potom otestovat a potom aktualizovat zásady "Otevřít" omezení, bude trvat zhruba 15 minut před zásady kombinace kláves vymění "Otevřít" verzi zásady.


##<a name="how-to-configure-the-key-authorization-policy"></a>Jak: Konfigurace zásad klíčové povolení

Konfigurace zásad klíčové se tak mohli ověřovat, vyberte stránku **Ochranu obsahu** .

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové povolení mohou být **otevřete** **tokenu**nebo omezení se tak mohli ověřovat **IP adres** (**IP** možné konfigurovat pomocí ZBÝVAJÍCÍ nebo .NET SDK).

###<a name="open-restriction"></a>Otevřít omezení

Omezení **otevřete** znamená, že systému dodá klávesu všem uživatelům, kteří žádá klíče. Toto omezení může být užitečné pro účely testování.

![Funkce OpenPolicy][open_policy]

###<a name="token-restriction"></a>Tokenu omezení

Zvolit tokenu s omezeným přístupem zásad, stiskněte tlačítko **TOKENU** .

**Tokenu** omezení zásad musí opatřeny token webová **Služba tokenů zabezpečení** (Služba tokenů zabezpečení). Media Services podporuje tokeny **Jednoduché tokeny Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) a formátem **JSON Web tokenu** (JWT). Informace najdete v tématu [JWT tokenu ověřování](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services neposkytuje **Zabezpečené tokenu služby**. Můžete vytvořit vlastní STS nebo využít Microsoft Azure ACS tokenů problému. STS musí být nakonfigurované pro vytvoření token podepsané zadaný deklarace spolu s problém, které jste zadali v konfiguraci tokenu omezení. Služba klíčové doručení Media Services vrátí šifrovací klíč klientovi pokud platí tokenu a nároky v tokenu odpovídají nakonfigurován pro klávesu obsahu. Další informace najdete v tématu [Použití Azure ACS k tokenů problému](http://mingfeiy.com/acs-with-key-services).

Při konfiguraci **TOKEN** omezení zásad, musíte nastavit hodnoty pro **ověření**, **Vystavitel** spolu s **cílové skupiny**. Ověření primární klíč obsahuje klíč, který byl podepsán tokenu, které vystavitel služba zabezpečené tokenu problémy tokenu. Cílové skupiny (někdy se jí říká obor) popisuje záměr tokenu nebo zdroje tokenu povolí přístup k. Služby klíčové doručení Media Services ověří, že tyto hodnoty v tokenu odpovídat hodnotám na šabloně.

###<a name="playready"></a>PlayReady

Ochrana obsahu pomocí **PlayReady**, jedna z možností, které bude nutné zadat na vaše povolení zásady při XML řetězec, který definuje šabloně PlayReady licenci. Ve výchozím nastavení je nastaven následující zásady:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Klikněte na tlačítko **Importovat zásady xml** a poskytují různé XML, která odpovídá schématu XML definované [tady](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

