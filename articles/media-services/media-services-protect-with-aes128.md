<properties
    pageTitle="Použití dynamického šifrování AES 128 a přihlašovacích údajů klíčové doručení | Microsoft Azure"
    description="Microsoft Azure Media Services umožňuje poskytovat obsah zašifrovaných AES 128bitové šifrování klíče. Media Services také poskytuje klíč doručení služba, která poskytuje šifrovacího klíče oprávnění uživatelům. Toto téma ukazuje, jak dynamicky šifrování AES 128 a používat službu klíčové doručení."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Použití dynamického šifrování AES 128 a přihlašovacích údajů klíčové doručení

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Základní informace

>[AZURE.NOTE] V tématu [Toto](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video a získejte přehled o tom, jak chránit vaše médium s obsahu šifrování AES.

Microsoft Azure Media Services umožňuje poskytovat Http Live – Streaming (HLS) a přitom zajistit hladký datových proudů šifrovaná pomocí šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování). Media Services také poskytuje klíč doručení služba, která poskytuje šifrovacího klíče oprávnění uživatelům. Pokud chcete pro Media Services šifrování aktivum, potřebujete šifrovací klíč přidružit majetek a taky nakonfigurovat zásady se tak mohli ověřovat klíče. Požádání proudu přehrávačem Media Services pomocí zadaného klíče dynamicky zašifrovat obsah pomocí šifrování AES. Dešifrovat proudu přehrávač bude požádat klávesu služby klíčové doručení. Služba rozhodnout, zda je uživatel oprávnění k získání klíče, vyhodnotí se tak mohli ověřovat zásad, které jste zadali pro klávesu.

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření nebo token omezení. Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny [Jednoduché tokeny Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a formátem [JSON Web tokenu](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Další informace najdete v tématu [Konfigurace zásad obsahu klávesu se tak mohli ověřovat](media-services-protect-with-aes128.md#configure_key_auth_policy).

Umožní využít výhod dynamické šifrování, musíte mít materiálů, který obsahuje sadu více bitrate MP4 souborů nebo více bitrate hladký přenos zdroje. Budete potřebovat pro nastavení zásad doručení položka (popsané dál v tomto tématu). Potom podle formátu zadaný streamování serverem datových proudů na vyžádání zajistí doručení proudu v protokol, který jste zvolili. Jako výsledek stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta.

Toto téma mohly být užitečné pro vývojáře, které fungují v žádostech o splnit chráněná média. Téma ukazuje, jak službu klíčové doručení zásady se tak mohli ověřovat tak, aby jenom oprávněnými klientů může odeslal šifrovacího klíče. Také ukazuje, jak používat dynamické šifrování.

>[AZURE.NOTE]Chcete-li začít používat dynamické šifrování, musíte nejprve získat alespoň jednu jednotku měřítko (označovaná taky jako datových proudů jednotky). Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dynamické šifrování AES 128 a pracovní postup služby klíčové doručení

Následují obecné kroky, které je třeba provést při šifrování svém majetku s pomocí služby klíčové doručení Media Services a také pomocí dynamického šifrování AES.

1. [Vytvoření aktivum a nahrajte soubory do majetku](media-services-protect-with-aes128.md#create_asset).
1. [Kódovat materiálů obsahující soubor adaptivní přenosová nastavený MP4](media-services-protect-with-aes128.md#encode_asset).
1. [Vytvoření obsahu klíče a spojit s kódováním materiálů](media-services-protect-with-aes128.md#create_contentkey). V Media Services obsahu klíč obsahuje majetku šifrovacího klíče.
1. [Konfigurace zásad obsahu klávesu se tak mohli ověřovat](media-services-protect-with-aes128.md#configure_key_auth_policy). Zásady obsahu klíčové autorizace musí být nakonfigurované tak, že jste a došlo ke splnění klientem klávesu obsahu, aby nedoručila ji do klienta.
1. [Konfigurace zásad doručení majetku](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfigurace zásad doručení zahrnuje: získání adresy URL a inicializace vektorové IV klíčových (AES 128 vyžaduje stejné IV uvede se při šifrování a dešifrování), protokol doručení (například MPEG POMLČKU, HLS, konfigurace, hladký přenos nebo všechny), zadejte dynamické šifrování (například obálky nebo bez dynamických šifrování).

Můžete použít jiné zásady pro každý protokol na stejné materiálů. Například můžete použít PlayReady šifrování hladký/POMLČKU a AES obálku HLS. Všechny protokoly, které nejsou podle zásad doručení (například přidáte jeden zásady pouze HLS protokol) z datových proudů, budou blokovány. Výjimkou je, pokud máte definovány vůbec žádné zásady doručení materiálů. Pak v vymazat budou povoleny všechny protokoly.

1. [Vytvoření OnDemand locator](media-services-protect-with-aes128.md#create_locator) abyste mohli získávat streamování adresy URL.

Téma také ukazuje, [jak se klientské aplikaci můžete požádat o klíči ze služby klíčové doručení](media-services-protect-with-aes128.md#client_request).

Kompletní.NET [Příklad](media-services-protect-with-aes128.md#example) na konci tématu bude hledat.

Následující obrázek ukazuje pracovního postupu, jsme je popsali výše. Tady tokenu slouží k ověření.

![Chránit pomocí AES 128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Zbývající část tohoto tématu poskytuje podrobné vysvětlení, příklady a odkazů na témata, která ukazují, jak dosáhnout úkoly jsme je popsali výše.

##<a name="current-limitations"></a>Aktuální omezení

Je-li přidat nebo aktualizovat svůj materiálů doručení zásad, musíte odstranit existující locator (pokud existuje) a vytvořit nové locator.

##<a id="create_asset"></a>Vytvoření aktivum a nahrajte soubory do majetku

Abyste mohli spravovat, kódování a streamování videa, musíte nejdřív nahrajte obsahu do Microsoft Azure Media Services. Po nahrání obsahu je bezpečně uložený v cloudu pro další zpracování a přenos. 

Podrobné informace najdete v tématu [Nahrajte soubory do účtu Media Services](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Kódování materiálů obsahující soubor adaptivní přenosová nastavený MP4

Pomocí dynamického šifrování, které bude nutné stačí vytvořit materiálů, který obsahuje sadu více bitrate MP4 souborů nebo více bitrate hladký přenos zdroje. Potom podle zadaného formátu v pozvánce manifestu nebo neúplné na vyžádání datových proudů serveru zajistí budete dostávat proudu v protokol, který jste zvolili. Jako výsledek stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta. Další informace najdete v tématu [Dynamický balení přehled](media-services-dynamic-packaging-overview.md) .

Další informace o tom, jak kódování postup [kódovat majetku pomocí Media Encoder standardní](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Vytvoření obsahu klíče a přidružit zakódovaný materiálů

V Media Services obsahu klíč obsahuje klávesy, která chcete šifrovat aktivum s.

Podrobné informace najdete v tématu [Vytvoření obsahu klíč](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Konfigurace zásad se tak mohli ověřovat klíč obsahu

Media Services podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíčové požadavky. Zásady obsahu klíčové autorizace musí být nakonfigurované tak, že jste a splněná klientem (player), aby klávesu nedoručila ji do klienta. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření, token omezení nebo IP omezení.

Podrobné informace najdete v tématu [Konfigurace zásad povolení obsahu klíče](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení materiálů 

Konfigurace zásad doručení pro vaše materiálů. Některé věci, které zahrnuje konfigurace zásad doručení materiálů:

- Adresa URL pořízení klíče. 
- Inicializace vektoru (IV) pro účely šifrování obálky. AES 128 vyžaduje stejné IV uvede se při šifrování a dešifrování. 
- Protokol doručení materiálů (například MPEG POMLČKU, HLS, konfigurace, hladký přenos nebo všechny).
- Typ dynamické šifrování (například AES Obálka) nebo bez dynamických šifrování. 

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

##<a id="client_request"></a>Jak lze klientem požádat klíč služby klíčové doručení?

V předchozím kroku vytvořen adresa URL odkazující na soubor seznamu. Klient musí extrahovat potřebných informací z datových proudů seznamu souborů před uskutečněním žádost o službu klíčové doručení.

###<a name="manifest-files"></a>Seznamu souborů

Klient je potřeba extrahovat adresu URL (obsahující také klíč obsahu Id (dítě)) hodnoty ze soubor. Klient bude pak se pokusíte získejte šifrovacího klíče ze služby klíčové doručení. Klient nutné, aby extrahovat IV hodnota a použití dešifrování proudu. Následující úryvek <Protection> prvek manifest hladký přenos.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

V případě HLS manifest kořenové rozdělené segmentu soubory. 

Je například manifest kořenové: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) a obsahuje seznam názvů souborů segment.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Pokud dokument otevřete jednu segmentu souborů v textovém editoru (například http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), by měl obsahovat #EXT-X-klíč, což znamená, že soubor je zašifrován.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Žádost o klávesu s služby klíčové doručení

Následující kód ukazuje, jak pošlete žádost o službu klíčové doručení Media Services pomocí klíčových oznámení o doručení identifikátor Uri (, který byl extrahovaných z manifestu) a token (v tomto tématu není mluvit o tom, jak získat jednoduchý Web tokenů z Služba tokenů zabezpečení).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Příklad

1. Vytvoření nového projektu konzoly.
1. Pomocí NuGet nainstalovat a přidejte Azure Media Services .NET SDK rozšíření. Při instalaci tohoto balíčku, také nainstaluje Media Services .NET SDK a přidá všechny ostatní požadované závislosti.
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

1. Kód v souboru Program.cs přepsat kód zobrazený v této části.
    
    Zkontrolujte, že aktualizace proměnné pro složek, kde se nacházejí vstupní souborů.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
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
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
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
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
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
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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
            }
        }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
