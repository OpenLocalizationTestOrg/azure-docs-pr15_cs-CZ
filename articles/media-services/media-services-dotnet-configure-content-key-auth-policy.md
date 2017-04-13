<properties 
    pageTitle="Konfigurace zásad obsahu klíčové tak mohli ověřovat pomocí Media Services .NET SDK | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat zásady se tak mohli ověřovat pro klíč obsahu pomocí Media Services .NET SDK." 
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
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamické šifrování: Konfigurace zásad obsahu klíčové povolení

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Základní informace

Microsoft Azure Media Services umožňuje poskytovat MPEG-POMLČKU hladký přenos a HTTP-Live – datových proudů (HLS) datových proudů chráněné pomocí šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS umožňuje poskytovat zašifrovaných Widevine DRM datových proudů přerušované čáry. PlayReady a Widevine jsou šifrovány za specifikaci běžné šifrování (CENC ISO/IEC 23001-7).

Media Services také poskytuje **Služba pro doručování klíč/licence** ze kterého můžete získat klientů klíči AES nebo PlayReady/Widevine licence k přehrání šifrované obsahu.

Pokud chcete pro Media Services šifrování aktivum, potřebujete šifrovací klíč (**CommonEncryption** nebo **EnvelopeEncryption**) přidružit materiálů (jak je popsáno [tady](media-services-dotnet-create-contentkey.md)) a taky nakonfigurovat se tak mohli ověřovat zásady pro klíč (způsobem popsaným v tomto článku).

Požádání proudu přehrávačem Media Services pomocí zadaného klíče dynamicky zašifrovat obsah pomocí šifrování AES nebo DRM. Dešifrovat proudu přehrávač bude požádat klávesu služby klíčové doručení. Služba rozhodnout, zda je uživatel oprávnění k získání klíče, vyhodnotí se tak mohli ověřovat zásad, které jste zadali pro klávesu.

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: **otevření** nebo **tokenu** omezení. Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny **Jednoduché tokeny Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) a formátem **JSON Web tokenu** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)).

Media Services neposkytuje zabezpečené tokenu služby. Můžete vytvořit vlastní STS nebo využít Microsoft Azure ACS tokenů problému. STS musí být nakonfigurované pro vytvoření token podepsané zadaný deklarace spolu s problém, které jste zadali v konfiguraci tokenu omezení (způsobem popsaným v tomto článku). Služba klíčové doručení Media Services vrátí šifrovací klíč klientovi pokud platí tokenu a nároky v tokenu odpovídají nakonfigurován pro klávesu obsahu.

Další informace najdete v tématu

[Tokenu authenitcation JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrace Azure Media Services OWIN MVC na základě aplikace pomocí služby Azure Active Directory a omezení doručování obsahu klíčové založené na deklaraci JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Použití Azure ACS k tokenů problému](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Některé aspekty, které platí:

- Abyste mohli používat dynamické balení a dynamické šifrování, musíte mít aspoň jednu streamování rezervovaná jednotku. Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).
- Vaše materiálů musí obsahovat sadu adaptivní bitrate MP4s nebo Adaptivní bitrate hladký přenos souborů. Další informace najdete v tématu [kódovat aktivum](media-services-encode-asset.md).
- Nahrání a kódovat svém majetku možnost **AssetCreationOptions.StorageEncrypted** .
- Pokud budete chtít kláves více obsahu, které vyžadují stejné konfigurace zásad, doporučujeme vytvoření zásad jediné povolení a opakované použití s několika kláves obsahu.
- Služba pro doručování klíč ukládání do mezipaměti ContentKeyAuthorizationPolicy a jeho souvisejících objektů (Možnosti zásad a omezení) 15 minut.  Když vytvoříte ContentKeyAuthorizationPolicy a zadat použití "Token" omezení, potom otestovat a potom aktualizovat zásady "Otevřít" omezení, bude trvat zhruba 15 minut před zásady kombinace kláves vymění "Otevřít" verzi zásady.
- Je-li přidat nebo aktualizovat svůj materiálů doručení zásad, musíte odstranit existující locator (pokud existuje) a vytvořit nové locator.
- V současné době nejde šifrování konfigurace streaming format nebo postupné stahování.


##<a name="aes-128-dynamic-encryption"></a>Dynamické šifrování AES 128

###<a name="open-restriction"></a>Otevřít omezení

Otevřít omezení znamená, že systému dodá klávesu všem uživatelům, kteří žádá klíče. Toto omezení může být užitečné pro účely testování.

Následující příklad vytvoří zásad otevřené povolení a přidá ji do klávesu obsahu.

statické veřejné void AddOpenAuthorizationPolicy (IContentKey contentKey) {a / vytvořit ContentKeyAuthorizationPolicy s omezeními zobrazit dotaz otevřený / / a vytvořte zásady se tak mohli ověřovat IContentKeyAuthorizationPolicy zásady = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("otevřené povolení zásady"). Výsledek;

Seznam<ContentKeyAuthorizationPolicyRestriction> omezení = nový seznam<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Tokenu omezení

Tato část popisuje jak vytvořit zásady obsahu klíčové autorizace a přidružit klávesu obsahu. Zásady se tak mohli ověřovat popisuje, co se tak mohli ověřovat musí být splněny a zjistit, pokud uživatel oprávnění pro příjem klávesu (například slouží seznam "ověření klíč" obsahovat klíč, který byl podepsán tokenu).

Abyste mohli nakonfigurovat možnost tokenu omezení, budete muset popisují požadavky tokenu se tak mohli ověřovat pomocí XML. Konfigurace tokenu omezení XML musí splňovat následující schéma XML.

####<a id="schema"></a>Schéma tokenu omezení
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Při konfiguraci **token** omezení zásad, je nutné zadat parametry primární** klíč ověření**, **Vystavitel** a **cílové skupiny** . **Ověření primární klíč **obsahuje klíč, který byl podepsán tokenu, které **Vystavitel** služba zabezpečené tokenu problémy tokenu. **Cílové skupiny** (někdy se jí říká **obor**) popisuje záměr tokenu nebo zdroje tokenu povolí přístup k. Služby klíčové doručení Media Services ověří, že tyto hodnoty v tokenu odpovídat hodnotám na šabloně. 

Při použití **Media Services SDK pro .NET**, můžete generovat token omezení **TokenRestrictionTemplate** předmětu.
Následující příklad vytvoří zásad se tak mohli ověřovat pomocí tokenu omezení. V tomto příkladu klienta musel prezentovat token, který obsahuje: podepisování klíč (VerificationKey), vydavatele tokenu a požadovaných nároků.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Test tokenu

Pokud chcete získat token test podle tokenu omezení použitý pro zásady klíčové ověřování, postupujte takto.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dynamické PlayReady šifrování 

Media Services umožňují konfigurovat práva a omezení, která chcete pro runtime PlayReady DRM k jejímu vynucení když uživatel pokouší přehrávání chráněného obsahu. 

Ochrana obsahu pomocí PlayReady, jedna z možností, které bude nutné zadat na vaše povolení zásady při XML řetězec, který definuje [PlayReady licenci šablony](media-services-playready-license-template-overview.md). V Media Services SDK pro .NET třídy **PlayReadyLicenseResponseTemplate** a **PlayReadyLicenseTemplate** vám pomůže definovat šabloně PlayReady licenci.

[Toto téma](media-services-protect-with-drm.md) ukazuje, jak zašifrovat obsah s **PlayReady** a **Widevine**.

###<a name="open-restriction"></a>Otevřít omezení
    
Otevřít omezení znamená, že systému dodá klávesu všem uživatelům, kteří žádá klíče. Toto omezení může být užitečné pro účely testování.

Následující příklad vytvoří zásad otevřené povolení a přidá ji do klávesu obsahu.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Tokenu omezení

Abyste mohli nakonfigurovat možnost tokenu omezení, budete muset popisují požadavky tokenu se tak mohli ověřovat pomocí XML. Konfigurace tokenu omezení XML musí odpovídat schéma XML uvedeno v [této](#schema) části.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Test token podle tokenu omezení použitý pro klíčové povolení zásad získáte [v této](#test) části. 

##<a id="types"></a>Typy používané při definování ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Další krok
Teď jste nakonfigurovali zásady se tak mohli ověřovat klíč obsahu, přejděte do tématu [Konfigurace zásad doručení materiálů](media-services-dotnet-configure-asset-delivery-policy.md) .
 
