<properties
    pageTitle="Zjištění pohyby s Azure Media analýzy | Microsoft Azure"
    description="Procesor Azure Media pohybu detekce médií (MP) umožňuje efektivní určit oddíly úrok v opačném případě dlouhé a bezproblémové video."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Zjištění pohyby s Azure Media analýzy

##<a name="overview"></a>Základní informace

Procesor **Azure Media pohybu detekce** médií (MP) umožňuje efektivní určit oddíly úrok v opačném případě dlouhé a bezproblémové video. Zjišťování pohybu lze na udělat střih statické kamery identifikují oddíly videa, které se vyskytuje pohybu. Vygeneruje JSON soubor obsahující metadatům časová razítka a ohraničující oblasti, kdy došlo k události.

Určené k zabezpečení videa kanálů, tento technologie je možné rozdělit pohybu do příslušné událostí a falešně pozitivní jako jsou stíny a změny osvětlení. Umožňuje generovat výstrah zabezpečení z fotoaparátu kanálů bez nevyžádané pošty nekonečné důležité události, a při tom extrahovat chvíli úrok z velmi dlouhý sledování videa.

**Azure Media pohybu detekce** MP momentálně v náhledu.

Toto téma uvádí podrobnosti o **Azure Media pohybu detekce** a ukazuje, jak ho použít s Media Services SDK pro .NET


##<a name="motion-detector-input-files"></a>Vstupní soubory detekce pohybu

Videosoubory. V současné době podporuje následující formáty: MP4, MOV a WMV.

##<a name="task-configuration-preset"></a>Konfigurace úkolu (Předvolby)

Při vytváření úkolu s **Azure Media pohybu detekce**, je nutné zadat přednastavení konfigurace. 

###<a name="parameters"></a>Parametry

Můžete použít následující parametry:

Jméno|Možnosti|Popis|Výchozí
---|---|---|---
sensitivityLevel|Řetězec: "nízká", "střední", "vysoká"|Sady vykázaného úroveň utajení, které pohyby. Upravte tak, aby upravit počtu falešně pozitivních výsledků.|"medium"
frameSamplingValue|Kladné celé číslo|Nastaví frekvenci, na které se spustí algoritmus. každý snímek roven 1, 2 znamená, že každý snímek 2 atd.|1
detectLightChange|Logická: "true", "false"|Určuje, zda light změny jsou uvedena v seznamu výsledků|"False"
mergeTimeThreshold|Křížky a čas: Hh: mm:<br/>Příklad: 00:00:03|Určuje časového intervalu mezi pohybu události, kde kombinovat a hlášené jako 1 2 události.|00:00:00
detectionZones|Matice zjišťování oblastí:<br/>– Zóna zjišťování je matice 3 nebo více míst<br/>-Je bod x a y souřadnice od 0 na 1.|Popisuje seznam tvaru zjišťování oblastí se nemusí používat.<br/>Výsledky se vykázat zóně jako ID, pomocí první z nich je "identifikátor": 0|Jedné zóně, který se zabývá celý snímek.

###<a name="json-example"></a>Příklad JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Pohybu detekce výstupní soubory

Úlohy duplicit pohybu vrací souboru JSON výstup materiálů, který popisuje upozornění pohybu a jejich kategorie v rámci video. Soubor bude obsahovat informace o čas a doba trvání pohybu zjištěnou na video.

Rozhraní API detekce pohybu poskytuje indikátory Jakmile dostanete objekty pohybu v pevné pozadím videa (například sledování videa). Detekce pohybu školení snížit upozornění NEPRAVDA, například osvětlení a změny stín. Aktuální omezení algoritmů zahrnutím noční zrakem videa, Poloprůhledný objekty a malé objekty.

###<a id="output_elements"></a>Prvky formátu JSON výstupní soubor

>[AZURE.NOTE]V nejnovější verzi formátu JSON výstup změnil a představují změně rozdělení pro některé zákazníky.

Následující tabulka popisuje prvky ve výstupním souboru JSON.

Element|Popis
---|---
Verze|Toto nastavení se týká verzi rozhraní API Video. Aktuální verze není 2.
Časová osa|"Značky" za sekundu videa.
Posun|Časový posun časová razítka v "značky". Verze 1.0 rozhraní API Video to bude vždy 0. V budoucích scénáře, které podporujeme tuto hodnotu může změnit.
FrameRate|Snímky sekundu videa.
Šířka, výška|Odkazuje na šířku a výšku v pixelech.
Zahájení|Zahájení časovým razítkem "značky".
Doba trvání|Délka události v "značky".
Interval|Interval každá položka událost, "značky".
Události|Každou část události obsahuje pohybu detekovaný v rámci tohoto doba trvání.
Typ|Aktuální verze to je vždy "2" pro obecný pohybu. To umožňuje popisek Video rozhraní API flexibilitu zařadit do kategorií pohybu v budoucích verzích.
RegionID|Jak je vysvětleno, to bude vždy 0 v této verzi. Štítek poskytuje rozhraní API Video flexibilitu najít pohybu v různých oblastech v budoucích verzích.
Oblasti|Odkazuje na oblast ve svém videu, kde zajímavého pohybu. <br/><br/>-"id" představuje oblasti oblast – v této verzi je jediný ID 0. <br/>-"typ" představuje obrazce oblasti zajímavého pro pohybu. V současné době podporuje "Obdélník" a "mnohoúhelník".<br/> Pokud jste zadali "Obdélník", oblasti má rozměry v X, Y, šířku a výšku. Souřadnice X a Y představují souřadnice XY levém horním rohu oblasti normalizovanou měřítka 0,0 do 1,0. Zadejte šířku a výšku představují velikost oblasti v normalizovanou měřítka 0,0 do 1,0. V aktuální verzi X, Y, šířku a výšku vždy pevných na 0, 0 a 1, 1. <br/>Pokud jste zadali "mnohoúhelník", oblasti má rozměrů v bodech. <br/>
Neúplné|Metadata je rozděleny až do různých segmentech s názvem neúplné. Každou část obsahuje zahájení, dobu trvání, číslo intervalu a události. Fragment se žádné události znamená, že během byl zjištěn bez pohybu, který počáteční čas a doba trvání.
Hranaté závorky]|Každá závorka představuje jeden interval události. Prázdné závorky pro tento interval znamená, že žádný pohybu byl zjištěn.
umístění|Tento nové položky ve skupinovém rámečku události Registrar umístění, kdy došlo k pohybu. Toto je konkrétnější než zjišťování zóny.

Následující obrázek je příklad JSON výstupu

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Omezení

- Podporované formáty videa vstupní patří MP4, MOV a WMV.
- Zjišťování pohybu je optimalizována pro stojící pozadí videa. Algoritmus se zaměřuje na snížení upozornění NEPRAVDA, jako jsou změny osvětlení a stínů.
- Některé pohybu nemusí zjišťování kvůli technické stránky; například noční zrakem videa Poloprůhledný objekty a malé objekty.


## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoří úlohu s úkolem zjišťování videa pohybu podle konfigurační soubor, který obsahuje následující předvolby json. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Stažení souborů JSON výstupu. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services pohybu detekce blogu](https://azure.microsoft.com/blog/motion-detector-update/)

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
