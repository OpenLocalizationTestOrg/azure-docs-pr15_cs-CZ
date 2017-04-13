<properties
    pageTitle="Zjištění nominální a emoce s Azure Media analýzy | Microsoft Azure"
    description="Toto téma ukazuje, jak zjistit ploch a emotikony s Azure Media analýzy."
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
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Zjištění nominální a emoce s Azure Media analýzy

##<a name="overview"></a>Základní informace

Procesor **Azure Media nominální detekce** médií (MP) umožňuje spočítat, sledovat pohyby a dokonce odhadnout účast v cílové skupiny a reakce prostřednictvím obličeje výrazech. Tato služba obsahuje dvě funkce: 

- **Zjišťování obličej**

    Nominální zjišťování najde a sleduje lidské ploch v rámci videa. Více ploch lze zjistit a následně sledovat pohyb jejich s uvedením času a místa metadaty vrácený v souboru JSON. Během sledování pokusí dávat konzistentní ID stejné nominální během je osoba manipulace na obrazovce, i když jsou brání nebo stručné ponechat rámce.

    >[AZURE.NOTE]Tato služba neprovádí obličeje rozpoznávání. Osoba, která ponechá rámce nebo změní brání pro příliš dlouho bude mít nové ID když se vrátí.

- **Zjišťování emoce**
    
    Zjišťování emoce je volitelný součástí procesor médií nominální zjišťování, který vrátí analýzy na více duševní atributy z ploch zjištěno, včetně štěstí, sadness, obav, anger apod. 

**Azure Media nominální detekce** MP momentálně v náhledu.

Toto téma uvádí podrobnosti o **Azure Media nominální detekce** a ukazuje, jak ho použít s Media Services SDK pro .NET.

##<a name="face-detector-input-files"></a>Obličej detekce vstupních souborů

Videosoubory. V současné době podporuje následující formáty: MP4, MOV a WMV.

##<a name="face-detector-output-files"></a>Obličej detekce výstupní soubory

Rozhraní API pro zjišťování a sledování nominální poskytuje vysokou přesností nominální umístění zjišťování a sledování, která dokáže rozpoznat až 64 lidské ploch do videa. Přední ploch poskytují nejlepších výsledků, při bočních ploch a small ploch (menší nebo rovna hodnotě 24 24 pixelů) nemusí být co nejpřesnější.

Vrátí zjištěné a sledovaných strany s připojení ke schůzce (vlevo, nahoře, šířka a výška) určující umístění ploch obrázku v pixelech, jakož i nominální identifikační číslo označující sledování to jednotlivé. Nominální Identifikačních čísel jsou chybám resetovat okolnosti při přední nominální dojde ke ztrátě nebo překryté v rámci, výsledkem je několik jednotlivců začíná přiřazeno více ID.

###<a id="output_elements"></a>Prvky ve výstupním souboru JSON

Nominální zjišťování a sledování operace výsledek výstup obsahuje metadata ze strany v daném souboru ve formátu JSON.

Nominální zjišťování a sledování JSON obsahuje následující atributy:

Element|Popis
---|---
Verze|Toto nastavení se týká verzi rozhraní API Video.
Časová osa|"Značky" za sekundu videa.
Posun|Toto je časový posun časová razítka. Verze 1.0 rozhraní API Video to bude vždy 0. V budoucích scénáře, které podporujeme tuto hodnotu může změnit.
FrameRate|Snímky sekundu videa.
Neúplné|Metadata je rozděleny až do různých segmentech s názvem neúplné. Každou část obsahuje zahájení, dobu trvání, číslo intervalu a události.
Zahájení|Počáteční čas dané první událost v "značky".
Doba trvání|Délka fragment v "značky".
Interval|Interval každá položka událostí v rámci fragment v "značky".
Události|Každou událost obsahuje ploch zjišťování a sledovat v rámci tohoto doba trvání. Je maticových řadu událostí. Pole vnější představuje jeden časový interval. Vnitřní pole se skládá z hodnot 0 a další události, které se stalo v daném okamžiku. [] Prázdné závorka znamená, že nebyly zjištěny žádné ploch.
ID| ID číselník, který je sledován. Toto číslo může dojít ke změně-li nerozpoznaná tvář. Dané osobě měli stejné ID v celém celkové video, ale nemůže být zaručena z důvodu omezení algoritmus rozpoznávání (NF pásmová atd.)
X, Y|Vpravo nahoře X a Y souřadnice nominální ohraničovacího rámečku normalizovanou měřítkem 0,0 do 1,0. <br/>-X a Y souřadnice jsou relativní do orientace na šířku vždy, takže pokud jste ještě snímku na výšku videa (nebo vzhůru nohama, v případě iOS), budete muset transponovat souřadnice podle toho.
Šířka, výška|Zadejte šířku a výšku nominální ohraničovacího pole v normalizovanou měřítka 0,0 do 1,0.
facesDetected|Nachází na konci výsledky JSON a shrnuje počet ploch algoritmu zjištěné během video. Protože ID můžete obnovit omylem-li nerozpoznaná tvář (například nominální vypnul obrazovce vzhledy pryč), toto číslo nemusí rovnat vždy true číslo ploch ve videu.

Nominální detekce pomocí fragmentace (metadata může dojít k rozdělení v založená na čase bloky a můžete si stáhnout jenom co byste měli) a segmentace (kde události, které jsou k rozdělení v případě, že se jim vrátit je příliš velký). Několik jednoduchých výpočtů můžete transformace dat. Řekněme, že zvláštní události jako začátek 6300 (značky) s časovou osou 2997 (značky/sec) a framerate 29,97 (snímky/sec), potom:

- Zahájení a časová osa = 2.1 sekund
- Sekundy x (Framerate/časová osa) = 63 snímků

Níže je jednoduchý příklad extrahování JSON do jednoho formát rámce nominální zjišťování a sledování:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Obličej zjišťování vstupní a výstupní příklad

###<a name="input-video"></a>Zadávání video

[Zadávání Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfigurace úkolu (Předvolby)

Při vytváření úkolu s **Azure Media nominální detekce**, je nutné zadat přednastavení konfigurace. Následující předvolba konfigurace je určené pro zjišťování nominální.

    {"version":"1.0"}

###<a name="json-output"></a>Výstup JSON

Následující příklad JSON výstup byla zkrácena.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Zjišťování emoce vstupní a výstupní příklad


###<a name="input-video"></a>Zadávání video

[Zadávání Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfigurace úkolu (Předvolby)

Při vytváření úkolu s **Azure Media nominální detekce**, je nutné zadat přednastavení konfigurace. Následující předvolby konfigurace určuje vytvořit JSON podle zjišťování emoce.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Popisy atributů

Název atributu|Popis
---|---
Režim|Ploch: Pouze obličej zjišťování <br/>AggregateEmotion: Zpáteční průměr emoce hodnoty pro všechny ploch v rámečku.
AggregateEmotionWindowMs|Použijte, pokud vybrán AggregateEmotion režim. Určuje délku video slouží k výsledkem jednotlivé agregace, v milisekundách.
AggregateEmotionIntervalMs|Použijte, pokud vybrán AggregateEmotion režim. Určuje, jak často souhrnné výsledky.

####<a name="aggregate-defaults"></a>Výchozí agregace

Níže jsou vám doporučené hodnoty agregační okno a interval nastavení. AggregateEmotionWindowMs by měl být delší než AggregateEmotionIntervalMs.

   |Výchozí hodnoty (ne)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|o 0,5.|2|0,25
AggregateEmotionIntervalMs|o 0,5.|1|0,25

###<a name="json-output"></a>Výstup JSON

JSON výstup pro agregační emoce (zkrácen):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Omezení

- Podporované formáty videa vstupní patří MP4, MOV a WMV.
- Velikost rozsahu zjišťovat nominální je 24 24 k 2048 × 2048 pixelů. Ploch mimo tato oblast nebude zjišťování.
- Pro každé video ploch vrácené maximálně 64.
- Některé plochy nemusí zjišťování kvůli technické stránky; například obrovské nominální úhlu (vedoucí pozice) a velké Pásmová propust. Přední a poblíž úplná strany mít nejlepších výsledků.


## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoří úlohu s nominální zjišťování úkolu založenou na konfigurační soubor, který obsahuje následující předvolby json. 
                    
        {
            "version": "1.0"
        }

1. Stažení souborů JSON výstupu. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)