<properties 
    pageTitle="Konfigurace zásad doručení majetku s .NET SDK | Microsoft Azure" 
    description="Toto téma ukazuje, jak nakonfigurovat Azure Media Services .NET SDK zásady doručení různých materiálů." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfigurace zásad doručení majetku s .NET SDK
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Základní informace

Pokud budete chtít doručení zašifrovaných prostředky, jeden z kroků v pracovním postupu doručování obsahu Media Services je konfigurace zásad doručení pro prostředky. Zásady doručení materiálů říká Media Services způsobu položka nedoručila ji: do kterých streamování protokolu má vaše materiálů dynamicky sbalí (pro třeba MPEG POMLČKU, HLS, hladký přenos nebo všechny), zda chcete dynamicky zašifrovat vaší materiálů a jak (obálky nebo běžné šifrování).

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
- Pokud máte aktivum u existující streamování locator nelze propojit nové zásady aktiva (můžete zrušit propojení existující zásady z aktiva, nebo aktualizovat zásady doručení přidružené k položce majetku).  Nejdřív musíte odebrat streamování locator, nastavit zásady a pak znovu vytvořte streamování locator.  Stejné locatorId se dají používat při obnovil streamování locator avšak měli jistotu, že, nezpůsobí problémy klientů od obsahu můžou mezipaměti původ nebo podřízené CDN.


##<a name="clear-asset-delivery-policy"></a>Zásady doručení vymazat materiálů

Podle pokynů pro **ConfigureClearAssetDeliveryPolicy** Určuje, nebudou mít vliv dynamické šifrování a poskytovat proudu v některém z následujících protokolů: MPEG POMLČKU HLS a přitom zajistit hladký přenos protokoly. Můžete chtít tuto zásadu použít na svém majetku úložiště zašifrovaných.

Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .

statická veřejné void ConfigureClearAssetDeliveryPolicy (IAsset majetku) {IAssetDeliveryPolicy zásad = _context. AssetDeliveryPolicies.Create ("Vymazat zásad" AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

materiály. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Zásady doručení DynamicCommonEncryption materiálů


Podle pokynů pro **CreateAssetDeliveryPolicy** vytvoří **AssetDeliveryPolicy** , který je nakonfigurovaný na použití dynamické běžné šifrování (**DynamicCommonEncryption**) vyhlazenými streamování protokolu (jiné protokoly, budou blokovány z datových proudů). Používá dvěma parametry: **materiálů** (materiálů, ke kterému chcete použití zásad doručení) a **IContentKey** (klíč obsahu typ **CommonEncryption** Další informace najdete v tématu: [Vytvoření obsahu klíče](media-services-dotnet-create-contentkey.md#common_contentkey)).

Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .


statická veřejné void CreateAssetDeliveryPolicy (IAsset materiálů, IContentKey klíče) {Uri acquisitionUrl = klíče. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration slovníku < AssetDeliveryPolicyConfigurationKey, řetězec > nový slovník < AssetDeliveryPolicyConfigurationKey, řetězec > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services můžete také přidat Widevine šifrování. Následující příklad ukazuje PlayReady a Widevine vás někdo přidá do zásad doručení materiálů.


    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        

        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Při šifrování s Widevine by pouze mohli poskytovat pomocí přerušované čáry. Nezapomeňte zadat přerušované čáry v protokolu doručení materiálů.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zásady doručení DynamicEnvelopeEncryption materiálů 

Podle pokynů pro **CreateAssetDeliveryPolicy** vytvoří **AssetDeliveryPolicy** , který je nakonfigurovaný na zašifrovat dynamické obálky (**DynamicEnvelopeEncryption**) hladký přenos HLS a POMLČKU protokoly (Pokud se rozhodnete není zadán některé protokoly, budou budou blokovány z datových proudů). Používá dvěma parametry: **materiálů** (materiálů, ke kterému chcete použití zásad doručení) a **IContentKey** (klíč obsahu typ **EnvelopeEncryption** Další informace najdete v tématu: [Vytvoření obsahu klíče](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Informace o jaké hodnoty, můžete použít při vytváření AssetDeliveryPolicy naleznete v části [typy používají při definování AssetDeliveryPolicy](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Typy používané při definování AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

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

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

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

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

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


