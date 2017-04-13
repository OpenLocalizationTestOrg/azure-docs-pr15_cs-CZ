<properties
    pageTitle="Indexování mediální soubory s náhledem indexování 2 Azure Media | Microsoft Azure"
    description="Azure Media indexování umožňuje provádět s možností vyhledávání obsahu mediální soubory a generovat fulltextové přepis skryté titulky a klíčových slov. Toto téma ukazuje, jak používat médií indexování 2 Preview."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indexování mediální soubory s náhledem indexování 2 Azure Media

##<a name="overview"></a>Základní informace

Procesor médií **Azure Media indexování 2 Preview** (MP) umožňuje vytvořit mediální soubory a obsahu s možností vyhledávání, stejně jako generovat skrytých titulků sleduje. Porovnání s předchozí verzí [Azure Media indexování](media-services-index-content.md) **Azure Media indexování 2 Preview** provádí rychleji indexování a nabízí širší podpora jazyků. Podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština, portugalština a arabština.

**Azure Media indexování 2 Preview** MP momentálně v náhledu.

Toto téma ukazuje, jak vytvořit indexovací úlohy s **Azure Media indexování 2 Preview**.

>[AZURE.NOTE]Platí následující omezení:
>
>Indexování 2 není podporována v Číně Azure a Azure Government.
>
>Náhled se omezí na zpracování ~ 10 minut, ale je zadarmo všem zákazníkům.
>
>Indexování obsahu, nezapomeňte použít mediální soubory, které mají jasný řeči (bez hudební doprovod hluku, efekty a hiss mikrofon). Některé příklady příslušný obsah: zaznamenaných schůzek, přednášek nebo prezentace. Následující obsah nemusí být vhodné pro indexování: filmy, zobrazuje televizního cokoli s smíšených zvuk a zvukových efektů špatně zaznamenané obsah s šum na pozadí (hiss).


Toto téma uvádí podrobnosti o **Azure Media indexování 2 Preview** a ukazuje, jak ho použít s Media Services SDK pro .NET

##<a name="input-and-output-files"></a>Vstupní a výstupní soubory

###<a name="input-files"></a>Vstupní soubory

Zvukové soubory nebo videosoubory

###<a name="output-files"></a>Výstupní soubory

Indexovací úlohy můžete generovat titulků soubory v těchto formátech:  

- **Sami**
- **TTML**
- **WebVTT**

Zavřený titulek kopie soubory v těchto formátech mohou sloužit k usnadnění zvukových souborů a videosouborů pro osoby se sluchovým postižením.

##<a name="task-configuration-preset"></a>Konfigurace úkolu (Předvolby)

Při vytváření indexovací úlohy s **Azure Media indexování 2 Preview**, je nutné zadat přednastavení konfigurace.

Následující JSON nastaví dostupné parametry.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Podporované jazyky  

Azure Media indexování 2 Preview podporuje řeči text následujících jazycích (při zadávání názvu jazyka v konfiguraci úlohy, použít kód znaku 4 v závorkách viz níže):

- Angličtina [EnUs]
- Španělština [EsEs]
- Čínština [ZhCn]
- Francouzština [FrFr]
- Němčina [DeDe]
- Italština [ItIt]
- Portugalština [PtBr]
- Arabština (Egyptian) [ArEg]


## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoří úlohu s indexování úkolu založenou na konfigurační soubor, který obsahuje následující předvolby json.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Ke stažení výstupní soubory. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


## <a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)