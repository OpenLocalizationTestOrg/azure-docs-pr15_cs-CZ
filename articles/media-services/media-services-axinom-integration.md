<properties 
    pageTitle="Použití Axinom předvádění Widevine licence Azure Media Services | Microsoft Azure" 
    description="Tento článek popisuje, jak můžete Azure Media Services (AMS) při dynamické zašifrované AMS s PlayReady a Widevine DRMs proudu. Licenci PlayReady pochází z nepodporovanou Media Services PlayReady a Widevine licenci Doručená Axinom licenční server." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Použití Axinom předvádění Widevine licence Azure Media Services  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Základní informace

Azure Media Services (AMS) přidala dynamické ochranu Google Widevine (podrobnosti najdete v části [Mingfei na blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) ). Kromě toho Azure Media Player (AMP) také přidal Widevine podpory (podrobnosti najdete v části [dokumentu AMP](http://amp.azure.net/libs/amp/latest/docs/) ). Moderní prohlížeče vybaven MSE a EME je hlavní úspěch v proudů POMLČKU chráněny CENC s více-native-DRM (PlayReady a Widevine).

Počínaje Media Services .NET SDK verze 3.5.2, Media Services umožňují konfigurovat Widevine licenci šablony a získat Widevine licence. Můžete také následující partnery AMS vám poskytovat Widevine licence: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Tento článek popisuje, jak integrovat a otestovat Widevine nepodporovanou spravuje Axinom. Konkrétně zahrnuje:  

- Konfigurace dynamické běžné šifrování s více DRM (PlayReady a Widevine) s odpovídající získání adresy URL licence;
- Generování JWT token za účelem splnění licenční požadavky serveru;
- Vývoj aplikace Azure Media Playeru, která zpracovává získání licence s JWT tokenu ověřování;

Úplný systém a tok obsah, který klíčů, klíčové ID, klíče počáteční hodnota, JTW token a jeho deklarací lze nejlépe popsat pomocí následující diagram.

![Přerušované čáry a CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Ochrana obsahu

Konfigurace dynamické protection a klíče doručení zásad, najdete v článku Mingfei na blogu: [jak nakonfigurovat Widevine balení s Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dynamické ochranu CENC s více DRM můžete nakonfigurovat pro POMLČKU datových proudů s obě následující akce:

1. Ochrana PlayReady MS Edge a IE11, může mít omezení tokenu se tak mohli ověřovat. Token s omezeným přístupem zásad musí být doprovázeny token Vystavitel tak, že zabezpečené tokenu Service (Služba tokenů zabezpečení), například Azure Active Directory;
1. Widevine ochranu Chrome, ho můžete požádat tokenu ověřování s token vydán jiné Služba tokenů zabezpečení. 

Přečtěte si téma [Tokenu generování JWT](media-services-axinom-integration.md#jwt-token-generation) části Proč nelze použít Azure Active Directory jako služba tokenů zabezpečení pro jeho Axinom Widevine licenční server.

###<a name="considerations"></a>Co byste měli zvážit

1. Je nutné použít Axinom zadané klíčové počáteční hodnota (8888000000000000000000000000000000000000) a vygenerovaných nebo vybrané základní ID Generovat klíč obsahu pro konfiguraci služby klíčové doručení. Axinom nepodporovanou vydá všechny licence obsahující obsahu klíče založené na se stejné počáteční hodnota klíčů, která platí pro testování a výroby.
1. Widevine licenci získání adresy URL testování: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). Nastavit informace HTTP a HTTS jsou povolené.

##<a name="azure-media-player-preparation"></a>Příprava Azure Media Player

AMP v1.4.0 podporuje přehrávání AMS obsahu, který je dynamicky součástí PlayReady a Widevine DRM.
Pokud server licenci Widevine nevyžaduje tokenu ověřování, je nic další, že musíte udělat testování POMLČKU obsahu chráněny Widevine. Příklad obsahuje týmu AMP jednoduchého [vzorku](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), kde vidíte fungoval okraje a IE11 s PlayReady a Chrome s Widevine.
Licenční server Widevine poskytovanou Axinom vyžaduje ověření tokenu JWT. JWT token musí odeslaný s žádostí o licenci prostřednictvím protokolu HTTP záhlaví "X AxDRM zpráv". K tomuto účelu potřebujete přidat následující JavaScriptu na webovou stránku hostingu AMP před nastavením zdroje:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Zbytek AMP kód je standardní rozhraní API AMP jako dokument AMP [tady](http://amp.azure.net/libs/amp/latest/docs/).

Všimněte si, že nahoře JavaScriptu pro nastavení vlastních se tak mohli ověřovat záhlaví pořád přístup krátkodobá před zástupce vydání dlouhodobou přístup AMP.

##<a name="jwt-token-generation"></a>JWT tokenu generování

Axinom Widevine nepodporovanou testování vyžaduje ověření tokenu JWT. Kromě toho pohledávek tokenu JWT Reprodukujte typu složité objektu místo základní datový typ.

Azure AD můžete bohužel JWT tokeny pouze problémů základní typy. Podobně .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler a JwtPayload) pouze umožňuje zadat typ složité objektu jako deklarované. Deklarace jsou pořád serializovány jako řetězec. Proto jsme nelze použít některou z těchto dvou pro generování token JWT pro požadavek Widevine licence.


Jan Sheehan [JWT Nuget balíčku](https://www.nuget.org/packages/JWT) vyhovuje potřebám tak budeme použití tohoto Nuget balíčku.

Tady je kód letiště generování token JWT s potřebné deklarací podle potřeby serverem Axinom Widevine licenci pro účely testování:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine licenční server

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Co byste měli zvážit

1.  I když vyžaduje služba pro doručování licence AMS PlayReady "nosný =" před token ověřování, Axinom Widevine nepodporovanou nepoužívá ho.
2.  Axinom komunikace se používá jako podpisový klíč. Všimněte si, že klávesu hex řetězec, ale musí být považuje za řadu bajtů není řetězec při kódování. Dosahuje se metodou ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Načtení ID klíče

Jste si všimli, že v kódu pro generování JWT tokenu, klíčové ID není potřeba. Protože základní JWT tokenu třeba provozu před načítání AMP přehrávače ID, musí načíst za účelem vytvoření JWT token.

Kurzu, který existuje více možností, jak k získání stiskněte a podržte klávesu ID. Například jednu mohou být uloženy klíče ID společně s metadat obsahu v databázi. Nebo můžete načíst klíče ID ze souboru MPD POMLČKU (médií prezentace popis). Následující kód se k nim.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Souhrn

Pomocí nejnovější doplněk Widevine podpory v Azure Media Services ochranu obsahu a Azure Media Playeru budou moct implementace streamování POMLČKU + více-native-DRM (PlayReady + Widevine) službě licenci obou PlayReady v AMS Widevine licenci server a od Axinom pro následující moderní prohlížeče:

- Chrome
- Microsoft Edge ve Windows 10
- IE 11 ve Windows 8.1 a Windows 10
- Firefox (Desktopová aplikace) a Safari na Mac (ne iOS) jsou taky podporované prostřednictvím programu Silverlight a adresu URL s Azure Media Player

Zkrácené řešení využití Axinom Widevine nepodporovanou jsou podle následujících parametrů. S výjimkou klíč ID, zbytek parametry podle Axinom na základě jejich nastavení serveru Widevine.


Parametr|Jak se používá
---|---
Komunikace klíče ID|Musí být vložen jako hodnota deklarace "com_key_id" JWT tokenu (viz téma [v této](media-services-axinom-integration.md#jwt-token-generation) části).
Klíč komunikace|Musí být použity jako podpisový klíč JWT token (viz téma [v této](media-services-axinom-integration.md#jwt-token-generation) části).
Klíčové počáteční hodnota|Musí se používat ke generování obsahu klíče daného obsahem klíče ID (viz téma [v této](media-services-axinom-integration.md#content-protection) části).
Získání URL Widevine licence|Musíte použít v konfigurace zásad doručení materiály pro POMLČKU streamování (viz [Tento](media-services-axinom-integration.md#content-protection) oddíl).
ID obsahu klíče|Musí být součástí přínosu nároku zprávy uplatnění JWT token (viz téma [v této](media-services-axinom-integration.md#jwt-token-generation) části). 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potvrzení 

Rádi potvrdit sledování lidí dobré, kdo přispěla k vytvoření tohoto dokumentu: Kristjan Jõgi Axinom, Jan Mingfei a Amitu Rajput.
