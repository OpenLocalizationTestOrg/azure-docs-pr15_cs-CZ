<properties
    pageTitle="K převodu textového obsahu v videosoubory digitálního textu použijte Azure Media analýzy | Microsoft Azure"
    description="Azure Media analýzy optického rozpoznávání znaků (optické rozpoznávání znaků) umožňuje převést textového obsahu v videosoubory upravovat, vyhledávání digitální text.  Umožňuje provádět automatické extrahování smysluplné metadat z videa signálu médií."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>K převodu textového obsahu v videosoubory digitálního textu použijte Azure Media analýzy 

##<a name="overview"></a>Základní informace

Pokud potřebujete extrahovat text obsah z videosoubory a generovat upravovat, vyhledávání digitální text, použijte Azure Media analýzy OCR (optické rozpoznávání znaků). Azure Media procesoru rozpozná textového obsahu je videosoubory a vygeneruje soubory ve formátu RTF pro použití. OCR umožňují automatizaci extrahování smysluplné metadat z videa signálu médií.

Při použití ve spojení s vyhledávacím webu, můžete snadno index médií podle textu a zlepšit vyhledávání obsahu. To je velmi užitečné v vysoce textové video, třeba nahrávání videa nebo snímku obrazovky prezentace prezentace. Procesor Azure OCR médií je optimalizována pro digitální text.

Procesor médií **Azure Media OCR** momentálně v náhledu.

Toto téma uvádí podrobnosti o **Azure Media OCR** a ukazuje, jak ho použít s Media Services SDK pro .NET. Další informace a příklady přečtěte si [Tento blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Vstupní soubory OCR

Videosoubory. V současné době podporuje následující formáty: MP4, MOV a WMV.

##<a name="task-configuration"></a>Konfigurace úkolu 

Konfigurace úkolu (Předvolby). Při vytváření úlohy pomocí **Azure Media OCR**, je nutné zadat konfigurace předvolby pomocí JSON nebo XML. 

###<a name="attribute-descriptions"></a>Popisy atributů

Název atributu|Popis
---|---
Jazyk|(volitelné) popisuje jazyka textu, u kterého chcete najít. Jednu z těchto věcí: automatické rozpoznání (výchozí), arabština, ChineseSimplified, ChineseTraditional, Česká dánské, holandština, angličtina, finština, francouzština, němčina, řečtina, maďarština, italština, japonština, korejština, norština, polština, portugalština, rumunština, ruština, SerbianCyrillic, SerbianLatin, slovenština, španělština, švédština, turečtina.
TextOrientation|(volitelné) popisuje orientaci textu, u kterého chcete najít.  "Doleva" znamená, že jsou začátek všechna písmena nasměruje vlevo.  Výchozí text (třeba, které můžete najít v knize) můžete volat "Nahoru" orientací.  Jednu z těchto věcí: automatické rozpoznání (výchozí), nahoru, doprava, dolů, doleva.
TimeInterval|(volitelné) popisuje četnosti.  Výchozí hodnota je za sekundu 1/2.<br/>Formátu JSON – hh. SSS (výchozí 00:00:00.500)<br/>Formát XML – základní doba trvání W3C XSD (výchozí nastavení PT0.5)
DetectRegions|(volitelné) Pole objekty DetectRegion zadáte oblasti v rámci na rámeček videa, ve kterém zjišťování text.<br/>Objekt DetectRegion je volání z následujících hodnot čtyři celé číslo:<br/>Vlevo – pixelů z levého okraje<br/>Horní – pixelů na horní okraj<br/>Šířka – šířky oblasti v pixelech<br/>Výška – výšku v pixelech oblasti

####<a name="json-preset-example"></a>Příklad přednastavené JSON
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Příklad přednastavené XML

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>Výstupní soubory OCR

Výstup procesor médií OCR je soubor JSON.

###<a name="elements-of-the-output-json-file"></a>Prvky formátu JSON výstupní soubor

Výstup videa OCR poskytuje segmentována čas data znaků, najdete ve videu.  Použít atributy například jazyk nebo orientace s tímto doplňkem na přesně text, který vás zajímá při analýze. 

Výstup obsahuje následující atributy:

Element|Popis
---|---
Časová osa|"značky" za sekundu videa
Posun|čas posun časová razítka. Verze 1.0 rozhraní API Video to bude vždy 0.
FrameRate|Snímky sekundu videa
Šířka|Šířka videa v pixelech
Výška|výšku v pixelech
Neúplné|pole založená na čase bloky video, do kterého je blokovém metadata
zahájení|spuštění fragment v "značky"
Doba trvání|Délka fragment v "značky"
interval|Interval všech událostí v rámci dané fragment
události|pole obsahující oblastí
oblast|objekt představující detekovaný slova nebo fráze
jazyk|jazyka textu detekovaný uvnitř oblasti
orientace|orientace textu detekovaný uvnitř oblasti
řádky|pole řádků textu detekovaný uvnitř oblasti
text|vlastní text

###<a name="json-output-example"></a>Příklad JSON výstupu

Následující příklad výstup obsahuje obecné informace, videa a několik videa fragmentů. V každé video fragment obsahuje všechny oblasti, která ke zjištění pomocí OCR MP s jazykem a jeho orientace textu. Oblast obsahuje také každý řádek aplikace word v této oblasti s textem na čáru, pozice na čáru a každý word informace (obsahu ve Wordu, umístění a spolehlivosti) v tomto řádku. Následujícím obrázku je příklad a umístění některých vloženého komentáře.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoří úlohy pomocí souboru konfigurace/přednastavení OCR.
1. Stažení souborů JSON výstupu. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
        {
            class Program
            {
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
                }
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
            }
        }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)
