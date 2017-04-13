<properties
    pageTitle="Šifrování PlayReady a/nebo Widevine dynamické běžné | Microsoft Azure"
    description="Microsoft Azure Media Services umožňuje poskytovat MPEG-POMLČKU hladký přenos a Http-Live – datových proudů (HLS) datových proudů chráněné pomocí Microsoft PlayReady DRM. Taky můžete doručení zašifrovaných Widevine DRM přerušované čáry. Toto téma ukazuje, jak dynamicky zašifrovat pomocí PlayReady a Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Použití PlayReady a/nebo Widevine dynamické běžné šifrování

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services umožňuje poskytovat MPEG-POMLČKU hladký přenos a HTTP-Live – datových proudů (HLS) datových proudů chráněné pomocí [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Také umožňuje poskytovat šifrované datových proudů POMLČKU s Widevine DRM licence. PlayReady a Widevine jsou šifrovány za specifikaci běžné šifrování (CENC ISO/IEC 23001-7). Můžete [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje číslem verze 3.5.1) nebo rozhraní REST API pro nastavení vaší AssetDeliveryConfiguration používat Widevine.

Media Services tato služba pro doručování PlayReady a Widevine DRM licence. Media Services také poskytuje rozhraní API, která umožňují konfigurovat práva a omezení, která chcete pro PlayReady nebo Widevine DRM runtime k jejímu vynucení při přehrávání uživatele chráněný obsah. Uživatel požádá o DRM chráněné obsah, aplikační Playeru požádat o licence ze služby AMS licenci. Službu licenci AMS vydá licence přehrávač Pokud je povoleno. Licenci PlayReady nebo Widevine obsahuje dešifrování klávesy, která lze přehrávačem klienta dešifrování a datového proudu obsahu.


Můžete také následující partnery AMS vám poskytovat Widevine licence: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Další informace najdete v tématu: integrace se službou [Axinom](media-services-axinom-integration.md) a [castLabs](media-services-castlabs-integration.md).

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření nebo token omezení. Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny [Jednoduché tokeny Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a formátem [JSON Web tokenu](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Další informace najdete v tématu Konfigurace zásad obsahu klávesu se tak mohli ověřovat.

Umožní využít výhod dynamické šifrování, musíte mít materiálů, který obsahuje sadu více bitrate MP4 souborů nebo více bitrate hladký přenos zdroje. Budete taky muset konfigurace zásad doručení položka (popsané dál v tomto tématu). Potom podle formátu zadaný streamování serverem datových proudů na vyžádání zajistí doručení proudu v protokol, který jste zvolili. Jako výsledek stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající HTTP odpověď založená na každý požadavek klienta.

Toto téma mohly být užitečné pro vývojáře, které fungují v žádostech o splnit médií chráněné s více DRMs, například PlayReady a Widevine. Téma ukazuje, jak službu PlayReady licenci doručení zásady se tak mohli ověřovat tak, aby jenom oprávněnými klientů může odeslal PlayReady nebo Widevine licencí. Také ukazuje, jak přes dynamické šifrování šifrování s PlayReady nebo Widevine DRM přerušované čáry.

>[AZURE.NOTE]Chcete-li začít používat dynamické šifrování, musíte nejprve získat alespoň jednu jednotku měřítko (označovaná taky jako datových proudů jednotky). Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Stáhnout ukázkový

Můžete si stáhnout ukázkový popisované v tomto článku z [tady](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfigurace dynamické běžné šifrování a DRM licenčních doručení služeb

Následují obecné kroky, které je třeba provést při ochraně svém majetku s PlayReady pomocí služba pro doručování licenci Media Services a také pomocí dynamického šifrování.

1. Vytvoření aktivum a nahrajte soubory do majetku.
1. Kódování materiálů obsahující soubor adaptivní přenosová nastavený MP4.
1. Vytvoření obsahu klíče a přidružit zakódovaný materiálů. V Media Services obsahu klíč obsahuje majetku šifrovacího klíče.
1. Konfigurace zásad obsahu klávesu se tak mohli ověřovat. Zásady obsahu klíčové autorizace musí být nakonfigurované tak, že jste a došlo ke splnění klientem klávesu obsahu, aby nedoručila ji do klienta.

Při vytváření zásad obsahu klíčové povolení, budete muset zadat následující: metoda (PlayReady nebo Widevine), omezení doručování (otevřít nebo tokenu) a informace specifické pro typ klíčové dodání, který definuje, jak klávesu doručených klientovi ([PlayReady](media-services-playready-license-template-overview.md) nebo [Widevine](media-services-widevine-license-template-overview.md) šablony licence).
1. Konfigurace zásad doručení majetku. Konfigurace zásad doručení zahrnuje: protokol doručení (například MPEG POMLČKU, HLS, konfigurace, hladký přenos nebo všechny), zadejte dynamické šifrování (například běžné šifrování) PlayReady nebo Widevine licenci získání adresy URL.

Můžete použít jiné zásady pro každý protokol na stejné materiálů. Například můžete použít PlayReady šifrování hladký/POMLČKU a AES obálku HLS. Všechny protokoly, které nejsou podle zásad doručení (například přidáte jeden zásady pouze HLS protokol) z datových proudů, budou blokovány. Výjimkou je, pokud máte definovány vůbec žádné zásady doručení materiálů. Pak v vymazat budou povoleny všechny protokoly.
1. Vytvoření OnDemand locator abyste mohli získávat streamování adresy URL.

Dokončení příkladu .NET na konci tématu bude hledat.

Následující obrázek ukazuje pracovního postupu, jsme je popsali výše. Tady tokenu slouží k ověření.

![Chránit pomocí PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Zbývající část tohoto tématu poskytuje podrobné vysvětlení, příklady a odkazů na témata, která ukazují, jak dosáhnout úkoly jsme je popsali výše.

##<a name="current-limitations"></a>Aktuální omezení

Je-li přidat nebo aktualizovat zásady doručení materiálů, musíte odstranit přidružené locator (pokud existuje) a vytvořit nové locator.

Omezení při šifrování s Widevine s Azure Media Services: v současné době nejsou podporované několika kláves obsahu.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Vytvoření aktivum a nahrajte soubory do majetku

Abyste mohli spravovat, kódování a streamování videa, musíte nejdřív nahrajte obsahu do Microsoft Azure Media Services. Po nahrání obsahu je bezpečně uložený v cloudu pro další zpracování a přenos.

Podrobné informace najdete v tématu [Nahrajte soubory do účtu Media Services](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Kódování materiálů obsahující soubor adaptivní přenosová nastavený MP4

Pomocí dynamického šifrování, které bude nutné stačí vytvořit materiálů, který obsahuje sadu více bitrate MP4 souborů nebo více bitrate hladký přenos zdroje. Pak podle zadaného formátu v žádosti o manifest a fragment serverem datových proudů na vyžádání zajistí, že dostanete proudu protokol, který jste zvolili. Jako výsledek stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta. Další informace najdete v tématu [Dynamický balení přehled](media-services-dynamic-packaging-overview.md) .

Další informace o tom, jak kódování postup [kódovat majetku pomocí Media Encoder standardní](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Vytvoření obsahu klíče a přidružit zakódovaný materiálů

V Media Services obsahu klíč obsahuje klávesy, která chcete šifrovat aktivum s.

Podrobné informace najdete v tématu [Vytvoření obsahu klíč](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Konfigurace zásad se tak mohli ověřovat klíč obsahu

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové autorizace musí být nakonfigurované tak, že jste a splněná klientem (player), aby klávesu nedoručila ji do klienta. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření nebo token omezení.

Podrobné informace najdete v tématu [Konfigurace zásad povolení obsahu klíče](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení materiálů 

Konfigurace zásad doručení pro vaše materiálů. Některé věci, které zahrnuje konfigurace zásad doručení materiálů:

- DRM licence získání adresy URL. 
- Protokol doručení materiálů (například MPEG POMLČKU, HLS, konfigurace, hladký přenos nebo všechny). 
- Typ dynamické šifrování (v tomto případě běžné šifrování). 

Podrobné informace najdete v tématu [Konfigurace zásad doručení materiálů ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Vytvoření OnDemand streamování locator abyste mohli získávat streamování adresy URL

Je třeba váš uživateli poskytnout streamování adresy URL pro hladký, POMLČKY nebo HLS.

>[AZURE.NOTE]Je-li přidat nebo aktualizovat svůj materiálů doručení zásad, musíte odstranit existující locator (pokud existuje) a vytvořit nové locator.

Pokyny, jak publikovat aktivum a vytváření datových proudů adresy URL najdete v článku [vytvoření datových proudů adresy URL](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Získání test tokenu

Potřebujete test tokenu podle tokenu omezení použitý pro zásady klíčové ověřování.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

    
[Přehrávač AMS](http://amsplayer.azurewebsites.net/azuremediaplayer.html) slouží k testování vašeho proudu.

##<a id="example"></a>Příklad


Následující příklad znázorňuje funkci, která byla zavedená v Azure Media Services SDK pro .net – verze 3.5.2 (konkrétně umožňuje definovat šablonu Widevine licence a požádat o licenci Widevine z Azure Media Services). Příkaz Nuget balíčku byla použita k instalaci balíčku:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Vytvoření nového projektu konzoly.
1. Pomocí NuGet nainstalovat a přidejte Azure Media Services .NET SDK.
2. Přidání další odkazy: System.Configuration.
2. Přidání konfiguračního souboru, který obsahuje název účtu a důležité informace:
    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu: [Konfigurace streamování koncové body](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Kód v souboru Program.cs přepsat kód zobrazený v této části.
    
    Zkontrolujte, že aktualizace proměnné pro složek, kde se nacházejí vstupní souborů.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
                    Console.ReadLine();
                }
        
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
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
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
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
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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
            }
        }


## <a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Viz taky

[CENC s více DRM a řízení přístupu](media-services-cenc-with-multidrm-access-control.md)

[Konfigurace Widevine balení s AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Licenční Google Widevine oznamující doručení služby Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
