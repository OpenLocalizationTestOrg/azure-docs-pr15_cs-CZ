<properties 
    pageTitle="Rozšířené možnosti kódování s pracovním postupem Premium Media Encoder | Microsoft Azure" 
    description="Naučte se kódovat s pracovním postupem Media Encoder Premium. Ukázky napsané v jazyce C# a použití Media Services SDK .NET." 
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
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Rozšířené možnosti kódování s pracovním postupem Premium Media Encoder

>[AZURE.NOTE] Procesor médií Media Encoder Premium pracovního postupu popisované v tomto tématu není k dispozici v Číně.

Další otázky encoder premium e-mail mepd na Microsoft.com.

##<a name="overview"></a>Základní informace

Microsoft Azure Media Services zavádí procesor médií **Media Encoder Premium pracovního postupu** . Tento rozšířené nabídky procesor kódování možností pracovních postupů okamžitou premium. 

V následujících tématech osnovy podrobnosti související **Media Encoder Premium pracovního postupu**: 

- [Podporované formáty tak, že pracovní postup Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – popisuje kodeky a formáty souborů podporované **Media Encoder Premium pracovního postupu**.

- Části [porovnání kodéry](media-services-encode-asset.md#compare_encoders) porovnává funkce kódování **Media Encoder Premium pracovního postupu** a **Media Encoder standardní**.

Toto téma ukazuje, jak kódovat s **Media Encoder Premium pracovního postupu** pomocí .NET.

Kódování úkoly **Pracovního postupu Premium Media Encoder** vyžadují samostatné konfigurační soubor s názvem souboru pracovního postupu. Tyto soubory příponou .workflow a vytvořené pomocí nástroje [Návrhář pracovního postupu](media-services-workflow-designer.md) .

##<a name="encode"></a>Kódování

Kódování úkoly **Pracovního postupu Premium Media Encoder** vyžadují samostatné konfigurační soubor s názvem souboru pracovního postupu. Tyto soubory příponou .workflow a vytvořené pomocí nástroje [Návrhář pracovního postupu](media-services-workflow-designer.md) .


Otevřete taky výchozí pracovní postup soubory [sem](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Složka také obsahuje popis těchto souborů.

Soubory pracovního postupu potřeba nahrát ke svému účtu Media Services jako aktivum a tento majetek by měla být v předána kódování úkolu.

Následující příklad ukazuje, jak kódovat s **Pracovním postupem Media Encoder Premium**. 

Provádí následující kroky: 
 
1. Vytvoření aktivum a nahrajte soubor pracovního postupu. 
2. Vytvořte aktivum a uložte zdrojový soubor média.
3. Pokud potřebujete procesor médií "Media Encoder Premium pracovního postupu".
4. Vytvoření projektu a úkolu. 

    Ve většině případů, je prázdný řetězec konfigurace úkolu (jako v následujícím příkladu). Existují některé pokročilé scénáře (, které vyžadují, abyste k nastavení vlastností runtime dynamicky) v tomto případě umožní řetězci XML kódování úkolu. Příklady scénářů: vytváření překryvném, paralelní nebo postupné médií ve hřbetu, titulkování.
5. Přidání dvou vstupní prostředky k úkolu.
    
    na. 1 – materiálů pracovního postupu.

    b. 2nd – video materiálů.
    
    **Poznámka**: pracovní postup materiálů musí být přidán do úkol před multimediální materiály. Konfigurace řetězec pro tento úkol by měl být prázdný. 

6. Odeslání kódování úlohy.

Následujícím obrázku je dokončení příkladu. Další informace o tom, jak nastavit vývoji Media Services .NET najdete v článku [vývoj Media Services s .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
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
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Další otázky encoder premium e-mail mepd na Microsoft.com.

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
