<properties 
    pageTitle="Chránit vaše HLS obsahu s Apple FairPlay a/nebo Microsoft PlayReady | Microsoft Azure" 
    description="Toto téma obsahuje přehled a ukazuje, jak dynamicky obsahu HTTP Live datových proudů (HLS) s Apple FairPlay šifrování databáze pomocí Azure Media Services. Také ukazuje, jak používat službu Media Services licenci doručení předvádění FairPlay licence klientů." 
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

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Chránit vaše HLS obsahu s Apple FairPlay a/nebo Microsoft PlayReady

Azure Media Services umožňují dynamicky zašifrovat obsah HTTP Live datových proudů (HLS) v následujících formátech:  

- **Vymazat klíč AES 128 obálky** 

    Celý blok musí být zašifrovaný použití režimu **AES 128 CBC** . Dešifrování proudu podporuje iOS a přehrávač OSX nativně. Další informace najdete v [tomto článku](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Jednotlivé vzorky videa a zvuku jsou šifrované použití režimu **AES 128 CBC** . **Přenos FairPlay** (Snímků /) je integrovaný v operačních systémech zařízení s nativní podporu pro iOS a Apple Televizi. Safari na OS X umožňuje snímků / pomocí rozhraní podpory zašifrovaných rozšíření médií (EME).
- **Microsoft PlayReady**

Následující obrázek znázorňuje **HLS + FairPlay a/nebo PlayReady dynamické šifrování** pracovního postupu.

![Chránit pomocí FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Toto téma ukazuje, jak dynamicky HLS obsahu s Apple FairPlay šifrování databáze pomocí Azure Media Services. Také ukazuje, jak používat službu Media Services licenci doručení předvádění FairPlay licence klientů.

>[AZURE.NOTE] Pokud chcete zašifrovat obsah HLS s PlayReady, potřebujete vytvořit běžné klíč obsahu a přidružit vaší materiálů. Potřebujete konfigurovat obsahu klávesu se tak mohli ověřovat zásad způsobem popsaným v tématu [použití PlayReady dynamické běžné šifrování](media-services-protect-with-drm.md) .

    
## <a name="requirements-and-considerations"></a>Požadavky a co byste měli zvážit

- Toto jsou potřeba při použití AMS předvádění HLS zašifrovaných FairPlay a poskytovat FairPlay licence.

    - Účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Účet Media Services. Vytvoření účtu Media Services, najdete v tématu [Vytvoření účet](media-services-portal-create-account.md).
    - Zaregistrujte si aplikaci [vývoj Apple](https://developer.apple.com/).
    - Apple vyžaduje vlastník obsahu získat [balíček pro nasazení](https://developer.apple.com/contact/fps/). Uveďte vyžádání že už implementovaná KSM (klíč zabezpečení modulu) s Azure Media Services a že žádáte konečný balíček snímků /. Nastane pokyny v okně konečný balíček snímků / generovat certifikační a získat dotaz, který budete používat ke konfiguraci FairPlay. 

    - Azure Media Services .NET SDK verze **3.6.0** nebo novější.

- Na straně klíčové doručení AMS musíte nastavit následující věci:
    - **Aplikace certifikátu (AC)** : soubor .pfx obsahující privátním klíčem. Tento soubor je vytvořen zákazníkem a šifrované heslem ke stejnému zákazníkovi. 
        
        Pokud zákazník nakonfiguruje klíčové doručení zásad, musíte zadat heslo a .pfx ve formátu ve formátu Base 64.

        Následující kroky popisují, jak generování certifikátu pfx pro FairPlay.
        
        1. Instalace OpenSSL z https://slproweb.com/products/Win32OpenSSL.html
        
            Přejděte do složky, kde jsou FairPlay certifikát a jiné soubory doručen Apple.
        
        2. Příkaz Převést cer pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-informovat der – v fairplay.cer-se fairplay out.pem
        
        3. Přepínač příkazového řádku převést pem pfx soukromým klíčem (heslo pro soubor pfx potom žádá OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 – export – se fairplay out.pfx-inkey privatekey.pem – v fairplay out.pem - passin file:privatekey-pem-pass.txt 
        
    - **Aplikace certifikátu heslo** - heslo zákazníka pro vytváření soubor .pfx.
    - **Aplikace certifikátu heslo ID** – zákazník musí nahrát podobný způsob odeslání další klávesy pro AMS a použití **ContentKeyType.FairPlayPfxPassword** výčet hodnoty heslo. Výsledkem je uvidí AMS id, které jde potřebují použít uvnitř možnost zásad klíčové doručení.
    - **iv** : 16 bajtů náhodné hodnoty, musí odpovídat iv do zásad doručení materiálů. Zákazník vygeneruje IV a vloží ho z obou míst: materiálů zásady a klíčem doručení zásad možnost doručení. 
    - **Zadání** - ASK (klávesu Application tajná) přijetí při generování certifikační portálu Apple vývojář. Každý týmu vývoje dostanou jedinečné ASK. Uložte kopii POLOŽIT a uložte ho do bezpečné místo. Je třeba nakonfigurovat ASK jako FairPlayAsk na pozdější Azure Media Services. 
    -  **Požádejte ID** – pochází když zákazníka nahraje ASk do AMS. Zákazník musí nahrát ASk použití **ContentKeyType.FairPlayASk** výčet hodnoty. Jako výsledek je vrácena AMS id a toto co bude použito při nastavování možnost zásad klíčové doručení.

- Na straně klienta snímků / musí nastavit následující věci:
    - **Aplikace certifikátu (AC)** -.cer/.der soubor obsahující veřejným klíčem, který OS využívá šifrování některé datové. AMS je potřeba vědět, protože je potřeba přehrávačem. Služby klíčové doručení dešifruje pomocí odpovídající privátním klíčem.

- Přehrávání FairPlay šifrované toku musíte získat skutečné ASK první a pak vytvořte certifikát skutečné. Tento proces vytvoří všechny části 3:

    -  .DER, 
    -  .pfx a 
    -  hesla .pfx.
 
- Klienti, které podporují HLS pomocí šifrování **AES 128 CBC** : zařízení s iOS OS X, televizního Apple Safari.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Postup při konfiguraci FairPlay dynamické licence a šifrování služeb doručení

Následují obecné kroky, které je třeba provést při ochraně svém majetku s FairPlay pomocí služba pro doručování licenci Media Services a také pomocí dynamického šifrování.

1. Vytvoření aktivum a nahrajte soubory do majetku. 
1. Kódování materiálů obsahující soubor adaptivní přenosová nastavený MP4.
1. Vytvoření obsahu klíče a přidružit zakódovaný materiálů.  
1. Konfigurace zásad obsahu klávesu se tak mohli ověřovat. Při vytváření zásad obsahu klíčové ověření, potřebujete zadejte následující údaje: 
    
    - Metoda doručení (v tomto případě FairPlay), 
    - Konfigurace možností zásad FairPlay. Podrobnosti o tom, jak konfigurovat FairPlay najdete v tématu ConfigureFairPlayPolicyOptions() metoda v následující ukázce.
    
        >[AZURE.NOTE] Obvykle byste měli chcete konfigurovat FairPlay popsané možnosti zásad jenom jednou a od máte jenom jednu sadu certifikace a ASK.
-omezení (otevřít nebo tokenu) – a informace specifické pro typ klíčové dodání, který definuje, jak klávesu doručených klientovi. 
    
2. Konfigurace zásad doručení materiálů. Konfigurace zásad doručení zahrnuje: 

    - doručení protocol (HLS) 
    - Typ dynamické šifrování (běžné CBC šifrování) 
    - Adresa URL pořízení licenci. 
    
    >[AZURE.NOTE]Pokud chcete dodat toku, musí být zašifrovaný pomocí FairPlay + jiné DRM, budete muset konfigurace zásad samostatné doručení 
    >
    >- Jeden IAssetDeliveryPolicy nakonfigurovat CENC (PlayReady + WideVine) a hladký s PlayReady přerušované čáry. 
    >- Jiné IAssetDeliveryPolicy konfigurace FairPlay pro HLS

1. Vytvoření OnDemand locator získat streamování adresy URL.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Klíčové doručení FairPlay pomocí přehrávače/klientské aplikace

Zákazníci může vyvíjet aplikace přehrávače pomocí iOS SDK. Aby bylo možné přehrávání obsahu z FairPlay zákazníci mít k provedení protokolu exchange licenci. Protokol licenci exchange není nastavil Apple. Je každý aplikace tom, jak poslat klíčové doručení žádosti. Služby klíčové doručení AMS FairPlay očekává SPC bude doplněný jako zprávu zakódovaný příspěvek www-form-url následujícím způsobem: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Playeru, které nejsou podporovány FairPlay přehrávání mimo pole. Zákazníci třeba opatřit přehrávačem ukázka z Apple vývojář účtu zobrazíte FairPlay přehrávání na MAC OSX. 
 
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


##<a name="net-example"></a>Příklad .NET


Následující příklad znázorňuje funkci, která byla zavedená v Azure Media Services SDK pro .net – verze 3.6.0 (možnost používání Azure Media Services pro doručování obsahu zašifrovaných FairPlay). Příkaz Nuget balíčku byla použita k instalaci balíčku:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Vytvoření projektu konzoly.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Další kroky: Media Services naučné stezky

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
