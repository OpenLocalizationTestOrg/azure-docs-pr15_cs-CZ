<properties
    pageTitle="Umožňuje vytvořit Video souhrnu Azure Media Video miniatury | Microsoft Azure"
    description="Grafický souhrn vám pomůže vytvářet souhrny dlouhé videa automaticky výběrem zajímavé výstřižky z zdroje videa. To je užitečné, pokud chcete zadat rychlý přehled toho, co můžete očekávat dlouhé videa."
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

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Umožňuje vytvořit Video souhrnu Azure Media Video miniatur
##<a name="overview"></a>Základní informace

Procesor **Azure Media Video miniatury** médií (MP) umožňuje vytvářet souhrn videa, které je užitečný pro zákazníky, kteří chcete zobrazit náhled souhrn dlouhého videa. Například zákazníci chtít najdete v článku stručný "souhrn videa" při najeďte myší na miniaturu. Úprava parametry **Azure Media Video miniatury** prostřednictvím přednastavení konfigurace, můžete pomocí MP výkonné snímek zjišťování a zřetězení technologii algorithmically generovat popisný subclip.  

**Azure Media Video miniaturu** MP momentálně v náhledu.

Toto téma uvádí podrobnosti o **Azure Media Video miniaturu** a ukazuje, jak ho použít s Media Services SDK pro .NET

##<a name="video-summary-example"></a>Grafické souhrnné příklad 

Tady je pár příkladů toho, co procesor médií Azure Media Video miniatury dělají:

###<a name="original-video"></a>Původní video

[Původní video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Videa miniatur výsledek

[Videa miniatur výsledek](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Konfigurace úkolu (Předvolby)

Při vytváření videa miniatur úkolu s **Azure Media Video miniatury**, je nutné zadat přednastavení konfigurace. Výše uvedené ukázkové miniatur byl vytvořený pomocí následující základní JSON konfigurace:

    {"version":"1.0"}

V současné době můžete změnit následující parametry:

Parametr|Popis
---|---
outputAudio|Určuje, zda výsledné video obsahuje zvuku. <br/>Povolené hodnoty jsou: True nebo False. Výchozí hodnota je PRAVDA.
fadeInFadeOut|Určuje, zda stmívání přechody se používají mezi samostatné pohybu miniatury.  <br/>Povolené hodnoty jsou: True nebo False.  Výchozí hodnota je PRAVDA.
maxMotionThumbnailDurationInSecs|Celé číslo, které určuje, jak dlouho bude celé výsledné video.  Výchozí závisí na původní videa doby trvání.


Následující tabulka popisuje výchozí doba trvání, kdy **maxMotionThumbnailInSecs** nepoužívá.

||||
---|---|---|---|---
Videa doba trvání|d < 3 min|3 minuty < d < 15 minut|15 minut < d < 30 minut| 30 minut < d
Miniatura doba trvání|15 sec (scény 2-3)| 30 sekund (scénami 3 až 5)|60 sec (5 až 10 scény)|90 sec (scény 10 až 15)


Následující JSON nastaví dostupné parametry.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoří úlohu s videa miniatur úkolu založenou na konfigurační soubor, který obsahuje následující předvolby json. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Ke stažení výstupní soubory. 

###<a name="net-code"></a>Kód .NET
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
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
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
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

###<a name="video-thumbnail-output"></a>Výstup video miniatur

[Výstup video miniatur](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)