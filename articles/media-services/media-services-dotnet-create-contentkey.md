<properties 
    pageTitle="Vytvoření ContentKeys s .NET" 
    description="Naučte se vytvářet obsahu zkratky, které poskytnutí zabezpečeného přístupu k prostředky." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>Vytvoření ContentKeys s .NET

> [AZURE.SELECTOR]
- [ZBÝVAJÍCÍ](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media Services umožňuje vytvořit a poskytují šifrované prostředky. **ContentKey** poskytuje zabezpečený přístup k s **materiálů**. 

Když vytvoříte nový majetek (například před [nahrávání souborů](media-services-dotnet-upload-files.md)), můžete zadat následující možnosti šifrování: **StorageEncrypted**, **CommonEncryptionProtected**nebo **EnvelopeEncryptionProtected**. 

Při předvádění prostředky pro klienty můžete [nakonfigurovat aktiv dynamicky šifrování](media-services-dotnet-configure-asset-delivery-policy.md) s některým z následujících dvou šifrování: **DynamicEnvelopeEncryption** nebo **DynamicCommonEncryption**.

Šifrované materiálů musí být přidružené k **ContentKey**s. Tento článek popisuje, jak při vytváření obsahu klíče.

>[AZURE.NOTE] Při vytváření nové majetku **StorageEncrypted** pomocí Media Services .NET SDK **ContentKey** automaticky vytvořené a propojené s majetku.

##<a name="contentkeytype"></a>ContentKeyType

Jedna z hodnot, že je nutné nastavit při vytvoření obsahu je klíčem klíče typu obsahu. Vyberte jednu z následujících hodnot. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Vytvořit typ obálky ContentKey

Následující fragment kódu vytvoří obsahu klíč typu šifrovací obálky. Potom přiřadí klávesu s zadaný materiálů.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

volání

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Vytvoření běžné typ ContentKey    

Následující fragment kódu vytvoří obsahu klíč běžné typ šifrování. Potom přiřadí klávesu s zadaný materiálů.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
volání

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
