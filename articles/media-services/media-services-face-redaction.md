<properties
    pageTitle="Obličej redigování s Azure media analýzy | Microsoft Azure"
    description="Toto téma ukazuje, jak redigovat ploch s Azure media analýzy."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Nominální redigování s Azure media analýzy

##<a name="overview"></a>Základní informace

**Azure Media Redactor** je procesor médií [Azure Media analýzy](media-services-analytics-overview.md) (MP), která nabízí redigování scalable obličej v cloudu. Nominální redigování umožňuje změnit videa k rozostření ploch vybraný osobám. Je vhodné použít službu redigování obličej v veřejné bezpečnosti a multimediální zprávy scénáře. Může trvat několik minut udělat střih, která obsahuje více ploch hodin redigovat ručně, ale k této službě procesu redigování nominální vyžadují jen pomocí několika jednoduchých kroků. Další informace najdete v tématu [Tento](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Toto téma uvádí podrobnosti o **Azure Media Redactor** a ukazuje, jak ho použít s Media Services SDK pro .NET.

**Azure Media Redactor** MP momentálně v náhledu.

## <a name="face-redaction-modes"></a>Režimy redigování obličej

Obličeje redigování funguje, zjišťování ploch v každém snímku videa a sledování nominální objektu obou dopředu a dozadu v čase, takže stejné jednotlivé můžete hranice z jiných úhlu i. Proces automatického redigování je velmi složité a ne vždy vytvářet 100 % požadované výstupní z tohoto důvodu médií analýzy obsahuje můžete pomocí několika jednoduchých způsobů, jak změnit konečný výstup.

Kromě plně automatický režim se dvěma průchodu pracovní postup, který umožňuje výběr/deaktivuje-selection nalezených ploch prostřednictvím seznam ID. Taky provést libovolný za úpravy snímku MP používá metadat souboru ve formátu JSON. Tento pracovní postup je rozdělen do **analyzovat** a **Redact** režimy. Zkombinování dvou režimech v jediném kroku, které se spouští oba úkoly v úloh. Tento režim je místo toho **kombinované**.

###<a name="combined-mode"></a>Kombinované režimu

Zajistíte zredigované mp4 automaticky bez všechny příručky pro zadávání.

Fáze|Název souboru|Poznámky
---|---|---
Zadávání materiálů|foo.bar|Video ve formátu WMV, MOV nebo MP4
Konfigurace při zadávání|Přednastavené konfiguraci úloh|{"verze": "1.0', 'možnosti': {"režimu":"kombinovanou"}}
Výstup materiálů|foo_redacted.MP4|Videa pomocí kodeků rozostření použitý

####<a name="input-example"></a>Zadávání příklad:

[zobrazení v tomto videu](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Příklad výstupu:

[zobrazení v tomto videu](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analýza režimu

Průchodu **analyzovat** pracovního postupu dvě průchodu vstup videa, která bude a vytvoří soubor JSON nominální umístění a obrázky ve formátu jpg každý zjištěné plochy.

Fáze|Název souboru|Poznámky
---|---|----
Zadávání materiálů|foo.bar|Video ve formátu WMV, MPV nebo MP4
Konfigurace při zadávání|Přednastavené konfiguraci úloh|{"verze": "1.0', 'možnosti': {"režimu":"analyzovat"}}
Výstup materiálů|foo_annotations.JSON|Pořizování poznámek data nominální umístění ve formátu JSON. Lze upravit tak, že uživateli upravit rozostření ohraničovací rámečky. Viz následující ukázce.
Výstup materiálů|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Oříznuté jpg každého detekovaný nominální, kde počet označuje labelId obličej

####<a name="output-example"></a>Příklad výstupu:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… zkrácené


###<a name="redact-mode"></a>Redigovat režimu

Druhá fáze pracovního postupu, která bude většího počtu vstupy, které musí být sloučena do jednoho materiálů.

Jedná se o seznam ID rozostření, původní video a poznámky JSON. V tomto režimu používá poznámky k použití rozostření o vstupní video.

Výstup průchodu analyzovat nezahrnuje původní video. Video musí nahrát do vstupní materiály pro daný úkol režimu Redact a vyberete jako primární soubor.

Fáze|Název souboru|Poznámky
---|---|---
Zadávání materiálů|foo.bar|Video ve formátu WMV, MPV nebo MP4. Stejné videa v kroku 1.
Zadávání materiálů|foo_annotations.JSON|Soubor metadat poznámky z první fázi, s volitelné změny.
Zadávání materiálů|foo_IDList.txt (volitelné)|Seznam nominální ID redigovat čárkami volitelné nový řádek. Pokud necháte prázdné, rozostří všechny ploch.
Konfigurace při zadávání|Přednastavené konfiguraci úloh|{"verze": "1.0', 'možnosti': {"režimu":"redigovat"}}
Výstup materiálů|foo_redacted.MP4|Videa pomocí kodeků rozostření použité založené na poznámky

####<a name="example-output"></a>Příklad výstupu

Toto je výstup IDList s ID vybrané.

[zobrazení v tomto videu](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Popisy atributů

MP redigování nabízí vysokou přesností nominální umístění zjišťování a sledování, která dokáže rozpoznat až 64 lidské ploch rámeček videa. Přední ploch poskytují nejlepších výsledků při bočních ploch jsou a small ploch (menší nebo rovna hodnotě 24 24 pixelů) náročné.

Zjištěné a sledovaných strany jsou vráceny souřadnice označující umístění ploch, jakož i tvář identifikační číslo označující sledování to jednotlivé. Nominální Identifikačních čísel jsou chybám resetovat okolnosti při přední nominální dojde ke ztrátě nebo překryté v rámci, výsledkem je několik jednotlivců začíná přiřazeno více ID.

Podrobné vysvětlení atributů tématu [zjišťování nominální a emocemi s Azure Media analýzy](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Ukázkový kód

Program ukazuje následující postup:

1. Vytvořte aktivum a uložte mediálních souborů do majetku.
1. Vytvoření úlohy pomocí nominální redigování úkolu založenou na konfigurační soubor, který obsahuje následující předvolby json. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Stahování souborů JSON výstupu. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
