<properties 
    pageTitle="Jak provést živou streamování pomocí Azure Media Services vytvořit bitrate více datových proudů s .NET | Microsoft Azure" 
    description="Tento kurz vás provede jednotlivými kroky vytvoření kanálu, který přijme živou toku jednoduchým bitrate a kódování více bitrate proudu pomocí .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Jak provést živou streamování pomocí Azure Media Services vytvořit bitrate více datových proudů s .NET

> [AZURE.SELECTOR]
- [Portál](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [ROZHRANÍ REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Základní informace

Tento kurz vás provede kroky pro vytvoření **kanálu** , který přijme živou toku jednoduchým bitrate a kódování více bitrate proudu.

Další informace týkající se kanály, kteří mají povolenou živou kódování najdete v článku [Live datových proudů pomocí Azure Media Services k vytváření bitrate více datových proudů](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Běžné situace živou streamování

Následující kroky popisují úkoly při vytváření běžné živou streamování aplikací.

>[AZURE.NOTE] Je v současné době, maximální doporučené trvání událost nastavit jako živou 8 hodin. Pokud potřebujete k spuštění kanálu po delší dobu, obraťte se na amslived na Microsoft.com.

1. Videokameru, tak připojte k počítači. Spuštění a konfigurace živou encoder místní, které můžete vytvořit jednu bitrate toku v některém z následujících protokolů: RTMP hladký přenos nebo RTP (MPEG-TS). Další informace najdete v tématu [Podpora RTMP Azure Media Services a Live kodéry](http://go.microsoft.com/fwlink/?LinkId=532824).

Tento krok mohou provést také po vytvoření kanálu.

1. Vytvoření a spuštění kanálu.

1. Načtení kanálu jedí adresu URL.

Adresa URL ingest se používá živou kodérem odešlete kanálu proudu.

1. Získat adresu URL kanálu náhled.

Pomocí této adresy URL pro ověření, že vaše kanálu správně dostává informace o živou proudu.

2. Vytvoření aktivum.
3. Pokud chcete pro materiálů dynamicky šifrované během přehrávání, postupujte takto:
1. Vytvoření obsahu klíče.
1. Konfigurace zásad obsahu klávesu se tak mohli ověřovat.
1. Konfigurace zásad doručení materiálů (používané dynamické balení a dynamické šifrování).
3. Vytvořit program a zadat použití materiálů, který jste vytvořili.
1. Publikujte materiálů přidružený k programu vytvořením OnDemand locator.

Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.

1. Jakmile budete připraveni spustit přenos a archivace, spusťte program.
2. Volitelně můžete živou encoder signál zahájíte reklamu. Inzerce je vložena do výstupu proudu.
1. Ukončení programu kdykoli budete chtít přestat datových proudů a archivace události.
1. Odstranění Program (a volitelně odstraňte majetku).

## <a name="what-youll-learn"></a>Co se dozvíte

V tomto tématu se dozvíte, jak provádět různé operace na kanály a programů, které používají Media Services .NET SDK. Protože mnoha operací jsou časově náročné .NET rozhraní API, kteří spravují dlouhodobě spuštěné jsou použity operace.

Téma ukazuje, jak provádět následující akce:

1. Vytvoření a spuštění kanálu. Rozhraní API dlouho probíhajících používají.
1. Získání kanály jedí (vstupní) koncového bodu. Tento koncový bod je třeba sdělit encoder, které můžou odesílat jednoho bitrate živou proudu.
1. Pokud potřebujete koncový bod náhled. Tento koncový bod slouží k zobrazení náhledu vašeho proudu.
1. Vytvoření aktiva, která se použije pro ukládání obsahu. Stejně, jak je vidět v tomto příkladu by měl konfigurovat zásady doručení materiálů.
1. Vytvořit program a zadat použití materiálů, která byla dříve vytvořena. Spuštění programu. Rozhraní API dlouho probíhajících používají.
1. Vytvořte locator pro majetek obsah publikování a můžete odeslat svoje klienty.
1. Zobrazení a skrytí potřeby. Spustit a zastavit inzerce. Rozhraní API dlouho probíhajících používají.
1. Vyčistěte kanálu a všechny přidružené prostředky.


##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto jsou potřebná k dokončení kurzu.

- Pro dokončení tohoto kurzu, třeba účet Azure.

Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F). Potřebujete přeplatky, které lze použít k vyzkoušení placené služby Azure. I když přeplatky používají, můžete mít účet a pomocí bezplatné služby Azure a funkce, například s funkcí webových aplikací Web Apps v aplikaci služby Azure.
- Účet Media Services. Vytvoření účtu Media Services, najdete v tématu [Vytvoření účet](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate nebo Express) nebo novější verze.
- Je nutné použít Media Services .NET SDK verze 3.2.0.0 nebo novější.
- Webkamerou a kódovací, který můžete poslat jednoho bitrate live proudu.

##<a name="considerations"></a>Co byste měli zvážit

- Je v současné době, maximální doporučené trvání událost nastavit jako živou 8 hodin. Pokud potřebujete k spuštění kanálu po delší dobu, obraťte se na amslived na Microsoft.com.
- Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.

##<a name="download-sample"></a>Stáhnout ukázkový

Získání a spusťte výběru z [tady](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Nastavení pro vývoj s Media Services SDK pro .NET

1. Vytvoření aplikace konzoly pomocí aplikace Visual Studio.
1. Přidání Media Services SDK pro .NET konzoly aplikaci použití balíčku služby NuGet médií.

##<a name="connect-to-media-services"></a>Připojení k Media Services
Osvědčený používejte konfigurační soubor uložit klávesu Media Services název a účtu.

>[AZURE.NOTE]Najít hodnoty název a klíč, přejděte na portál Azure a vyberte svůj účet. Nastavení okno na pravé straně. V okně Nastavení vyberte klíče. Klepnutím na ikonu vedle každé textové pole slouží ke kopírování hodnoty do schránky systému.

Přidání oddílu appSettings konfiguračního souboru a nastavení hodnot pro svůj Media Services název a účet klíč účtu.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Příklad

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Hledáte něco jiného?

Toto téma neobsahoval, co jste očekávali, něco chybí, zda v jiným způsobem neměli vlastním potřebám, přejděte prosím uveďte nám s vámi pomocí níže uvedených Disqus vlákna.
