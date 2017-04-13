<properties 
    pageTitle="Kódování aktivum s Media Encoder standardní pomocí .NET | Microsoft Azure" 
    description="Toto téma ukazuje, jak používat .NET kódování majetku pomocí Media Encoder Strandard." 
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
    ms.date="09/19/2016"
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Kódování aktivum s Media Encoder standardní pomocí .NET

Kódování úlohy jsou jednou nejběžnější zpracování v Media Services. Vytvoření kódování úlohy převést mediální soubory z jednoho kódování do jiného. Při kódování, můžete použít předdefinované Media Encoder Media Services. Můžete také encoder poskytovanou partnerem Media Services; třetí strany kodéry jsou dostupné prostřednictvím webu Azure Marketplace. 

Toto téma ukazuje, jak používat .NET kódování svém majetku s Media Encoder standardní (MES). Media Encoder standardní se konfigurují nějaká encoder předvolby popsané [tady](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Doporučuje se vždy kódovat mezzanine souborů do adaptivní bitrate MP4 nastavení a následný převod sadu na požadovaný formát pomocí [Dynamického obalu](media-services-dynamic-packaging-overview.md). Umožní využít výhod dynamické balení musíte nejprve získat nejméně jednu jednotku datových proudů na vyžádání pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [jak měřítko Media Services](media-services-portal-manage-streaming-endpoints.md).

-Li výstup materiálů úložiště zašifrovaných, musíte nakonfigurovat zásady doručení materiálů. Další informace najdete v článku [Konfigurace zásad doručení materiálů](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MES vytvoří výstupní soubor s názvem, který obsahuje prvních 32 znaky názvu souboru zadávání. Název podle zadaných v souboru přednastavené. Například "NázevSouboru": "{Basename} – {Index} {rozšíření}". {Basename} nahrazuje prvních 32 znaky názvu souboru zadávání.

###<a name="mes-formats"></a>Formáty MES

[Formáty a kodeky](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Předvolby MES

Media Encoder standardní se konfigurují nějaká encoder předvolby popsané [tady](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Vstupní a výstupní metadat

Při kódování vstupní aktiva (nebo prostředky) pomocí MES dostanete aktivum výstup na úspěšné dokončení tohoto kódovat úkolu. Výstup materiálů obsahuje video, zvuk, miniatury, manifestu atd založené na předvolby kódování, který používáte.

Výstup materiálů obsahuje také soubor s metadaty o zadávání materiálů. Název souboru XML metadat má v tomto formátu: < asset_id > _metadata.xml (například 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), kde < asset_id > hodnotu Id_položky vstupní majetku. Schéma tato vstupní metadata XML je popsán [v tomto poli](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Výstup materiálů obsahuje také soubor s metadata výstup materiálů. Název souboru XML metadat má v tomto formátu: < source_file_name > _manifest.xml (například BigBuckBunny_manifest.xml). Schéma tato výstup metadata XML je popsán [v tomto poli](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Pokud chcete prozkoumat jednu ze dvou souborů metadata, můžete vytvořit locator přidružení zabezpečení a stáhněte tak soubor do místního počítače. Příklad můžete najít o tom, jak vytvořit locator přidružení zabezpečení a stáhněte si soubor pomocí Media Services .NET SDK rozšíření.

##<a name="download-sample"></a>Stáhnout ukázkový

Získání a spusťte výběru z [tady](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Příklad

Následující příklad využívá Media Services .NET SDK provádět následující úkoly:

- Vytvoření kódování úlohy.
- Získáte odkaz na Media Encoder standardní encoder.
- Zadat použití "H264 více Bitrate 720p" přednastavené. Zobrazí se všechny položky [v tomto poli](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Můžete také zkoumat schéma, do které musí splňovat tyto přednastavené [tady](https://msdn.microsoft.com/library/mt269962.aspx) tématu.
- Přidání jednoho kódování úkolu do projektu. 
- Zadejte vstupní aktiva zakódovat.
- Vytvoření aktivum výstup, který bude obsahovat zakódovaný materiálů.
- Přidání obslužné rutiny události kontroly průběhu projektu.
- Odeslání projektu.
        
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
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky 

[Jak vygenerovat miniaturu pomocí Media Encoder standardní s .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Přehled kódování Media Services](media-services-encode-asset.md)
