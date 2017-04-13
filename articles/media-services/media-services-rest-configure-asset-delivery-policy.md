<properties 
    pageTitle="Konfigurace zásad doručení materiálů pomocí Media Services REST API | Microsoft Azure" 
    description="Toto téma ukazuje, jak nakonfigurovat různých materiálů doručení zásady použití Media Services REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Konfigurace zásad doručení materiálů

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Pokud budete chtít předvádění dynamicky šifrované prostředky, jeden z kroků v pracovním postupu doručování obsahu Media Services konfiguruje doručení zásad pro prostředky. Zásady doručení materiálů říká Media Services způsobu položka nedoručila ji: do kterých streamování protokolu má vaše materiálů dynamicky sbalí (pro třeba MPEG POMLČKU, HLS, hladký přenos nebo všechny), zda chcete dynamicky zašifrovat vaší materiálů a jak (obálky nebo běžné šifrování).

Toto téma popisuje důvod, proč a postupem vytvoření a konfigurace zásad doručení materiálů.

>[AZURE.NOTE]Abyste mohli používat dynamické balení a dynamické šifrování, musíte mít aspoň jednu jednotku měřítko (označovaná taky jako streamování jednotky). Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).
>
>Vaše materiálů musí obsahovat soubor adaptivní bitrate MP4s nebo Adaptivní bitrate hladký přenos souborů.

Můžete použít různé zásady ke stejnému majetku. Například může použijete PlayReady šifrování hladký přenos a AES obálky šifrování MPEG POMLČKU a HLS. Všechny protokoly, které nejsou podle zásad doručení (například přidáte jeden zásady pouze HLS protokol) z datových proudů, budou blokovány. Výjimkou je, pokud máte definovány vůbec žádné zásady doručení materiálů. Pak v vymazat budou povoleny všechny protokoly.

Pokud chcete při ukládání šifrovaných materiálů, musíte nakonfigurovat zásady majetku doručení. Před vaší materiálů můžete streamují, serveru datových proudů odebere šifrování úložiště a přenáší datové proudy obsahu pomocí zásad zadaný doručení. Například předvádění vaší materiálů zašifrované pomocí šifrování AES (Advanced Standard) obálky šifrovací klíč, nastavte typ zásad **DynamicEnvelopeEncryption**. Odebrat úložiště šifrování a materiálů v Vymazat můžete vysílat datovými proudy, nastavte typ zásad pro **NoDynamicEncryption**. Příklady, které ukazují, jak nakonfigurovat tyto typy zásad podle.

V závislosti na konfiguraci zásad doručení materiálů by budete moci dynamicky balíček dynamicky šifrování a následující streamování protokoly můžete vysílat datovými proudy: hladký přenos HLS, MPEG POMLČKU a konfigurace datových proudů.

V následujícím seznamu vidíte formáty používat ke streamování hladký HLS, přerušované čáry a konfigurace.

Hladký přenos:

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG POMLČKA

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

KONFIGURACE

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Pokyny, jak publikovat aktivum a vytváření datových proudů adresy URL najdete v článku [vytvoření datových proudů adresy URL](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Co byste měli zvážit

- Nelze odstranit AssetDeliveryPolicy přidružené aktivum locator OnDemand (streamování) existuje pro majetek. Doporučujeme odebrat zásady majetku před odstraněním zásady.
- Streamování locator nejde vytvořit šifrované majetku úložiště žádné materiálů doručení zásady nastavená.  Není-li majetku úložiště zašifrovaných, systému vám umožní vytvořit locator a vysílat aktiva v vymazat bez zásad doručení materiálů.
- Máte několik zásad doručení materiálů přidružených k jedné materiálů, ale můžete zadat pouze jeden způsob zpracování dané AssetDeliveryProtocol.  Znamená, když se pokusíte propojit dvě zásady dodání, které určují AssetDeliveryProtocol.SmoothStreaming protokol, který bude mít za následek chybu protože systému nebudou vědět, které jeden se má použít, pokud klient požaduje hladký přenos.
- Pokud máte aktivum u existující streamování locator nelze propojit nové zásady aktiva, zrušte tak propojení existující zásady z majetku nebo aktualizovat zásadu doručení přidružené k položce majetku.  Nejdřív musíte odebrat streamování locator, nastavit zásady a pak znovu vytvořte streamování locator.  Stejné locatorId se dají používat při obnovil streamování locator avšak měli jistotu, že, nezpůsobí problémy klientů od obsahu můžou mezipaměti původ nebo podřízené CDN.

>[AZURE.NOTE] Když pracujete s Media Services REST API, platí následující omezení:
>
>Při přístupu k entit v Media Services, musíte nastavit konkrétní záhlaví pole a hodnoty v žádostech HTTP. Další informace najdete v tématu [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).

>Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Zásady doručení vymazat materiálů

###<a id="create_asset_delivery_policy"></a>Vytvoření zásad doručení materiálů
Následující žádost HTTP vytvoří zásadu doručení materiálů, která určuje, nebudou mít vliv dynamické šifrování a poskytovat proudu v některém z následujících protokolů: MPEG POMLČKU HLS a přitom zajistit hladký přenos protokoly. 

Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .   


Žádost o:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Odpověď:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Odkaz materiálů zásadami doručení materiálů

Následující žádost HTTP propojí zadaný materiálů zásadu doručení materiálů.

Žádost o:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Odpověď:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zásady doručení DynamicEnvelopeEncryption materiálů 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Vytvoření obsahu klíče typu EnvelopeEncryption a odkaz na majetku

Při určování zásad doručení DynamicEnvelopeEncryption, budete muset zkontrolujte, že odkaz vaší materiálů obsahu s klávesou CTRL typu EnvelopeEncryption. Další informace najdete v tématu: [Vytvoření obsahu klíče](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Získání URL doručení

Získání adresy URL doručení metoda zadaný doručení obsahu klíče vytvořili v předchozím kroku. Klientský počítač používá vrácené adresy URL k žádosti o AES klíč nebo licenci PlayReady v pořadí přehrávání chráněného obsahu.

Zadejte adresu URL zobrazíte v hlavním textu žádosti HTTP o. Pokud jsou ochrana obsahu pomocí PlayReady, požádat o Media Services PlayReady licenci získání adresy URL, pomocí 1 pro keyDeliveryType: {"keyDeliveryType": 1}. Pokud jsou ochrana obsahu pomocí šifrování obálky, požádat o klíčové získání adresy URL zadáním 2 pro keyDeliveryType: {"keyDeliveryType": 2}.

Žádost o:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Odpověď:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Vytvoření zásad doručení materiálů

Následující žádost HTTP vytvoří **AssetDeliveryPolicy** , který je nakonfigurovaný na použití dynamického obálky šifrování (**DynamicEnvelopeEncryption**) protokolu **HLS** (v tomto příkladu jiné protokoly, budou blokovány z datových proudů). 


Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .   

Žádost o:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Odpověď:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Odkaz materiálů zásadami doručení materiálů

Zobrazit [odkaz materiálů zásadami doručení materiálů](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Zásady doručení DynamicCommonEncryption materiálů 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Vytvoření obsahu klíče typu CommonEncryption a odkaz na majetku

Při určování zásad doručení DynamicCommonEncryption, budete muset zkontrolujte, že odkaz vaší materiálů obsahu s klávesou CTRL typu CommonEncryption. Další informace najdete v tématu: [Vytvoření obsahu klíče](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Získání URL doručení

Získání adresy URL doručení Metoda doručení PlayReady obsahu klíče vytvořili v předchozím kroku. Klient používá vrácené URL požádat o licenci PlayReady v pořadí přehrávání chráněného obsahu. Další informace najdete v tématu [Získání adresy URL doručení](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Vytvoření zásad doručení materiálů

Následující žádost HTTP vytvoří **AssetDeliveryPolicy** , který je nakonfigurovaný na použití dynamické běžné šifrování (**DynamicCommonEncryption**) protokolu **Hladký přenos** (v tomto příkladu jiné protokoly, budou blokovány z datových proudů). 

Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .   


Žádost o:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Pokud chcete ochranu obsahu pomocí Widevine DRM, aktualizujte AssetDeliveryConfiguration hodnoty pro použití WidevineLicenseAcquisitionUrl (který má hodnotu 7) a zadejte adresu URL služba pro doručování licence. Pomocí následujících partnery AMS můžete dodat Widevine licence: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Příklad: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Při šifrování s Widevine by pouze mohli poskytovat pomocí přerušované čáry. Nezapomeňte zadat POMLČKU (2) v protokolu doručení materiálů.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Odkaz materiálů zásadami doručení materiálů

Zobrazit [odkaz materiálů zásadami doručení materiálů](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Typy používané při definování AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
