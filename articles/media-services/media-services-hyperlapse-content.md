<properties
    pageTitle="Hyperlapse mediální soubory s Azure Media Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse vytvoří hladký zrušení čas videa z první osoba nebo akce kamery obsah. Toto téma ukazuje, jak používat médií indexování."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse mediální soubory s Azure Media Hyperlapse

Azure Media Hyperlapse je média procesor (MP), která vytvoří hladký zrušení čas videa z první osoba nebo akce kamery obsah.  Cloudové na stejné úrovni [Microsoft Research plochy Hyperlapse Pro](http://aka.ms/hyperlapse)a telefonních Hyperlapse Mobile Microsoft Hyperlapse pro službu Azure Media Services využívá rozsáhlé měřítka platformu Azure Media Services médií zpracování vodorovně měřítko a paralelní hromadně Hyperlapse zpracování.

>[AZURE.IMPORTANT]Microsoft Hyperlapse je navržený pro práci nejlépe na první osoba obsahu s přesunutí fotoaparátu.  Přestože záběru fotoaparátu pořád můžete nadále pracovat, výkon a kvalitu Azure Media Hyperlapse médií procesor nelze zaručit neobsahují další typy obsahu.  Další informace o Microsoft Hyperlapse pro službu Azure Media Services a některá videa příklad, podívejte se [úvodní do blogu](http://aka.ms/azurehyperlapseblog) z veřejné náhled.

Azure Media Hyperlapse úlohy používá jako vstup materiálů souboru MP4, MOV nebo WMV spolu s konfigurační soubor, který určuje, které snímky video by měl být zrušení čas a jaké rychlosti (například první 10 000 snímky na 2 x).  Výstup je stabilizované a zrušení čas verze vstupní video.

V nejnovějších aktualizacích Azure Media Hyperlapse najdete v článku [blogy Media Services](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse aktivum

Musíte nejdřív nahrajte požadované vstupní soubor Azure Media Services.  Další informace o souvisejících se nahrávání a správa obsahu koncepty, přečtěte si [článek Správa obsahu](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Konfigurace předvolby pro Hyperlapse

Jakmile je váš obsah ve vašem účtu Media Services, musíte vytvořit přednastavení konfigurace.  Následující tabulka uvádí pole zadané uživatelem:

 Pole | Popis
-------|-------------
StartFrame|Snímek, na kterém má začít zpracování Microsoft Hyperlapse.
NumFrames|Počet snímků pro zpracování
Rychlost|Faktory s k dosažení vyššího vstupní video.

Následujícím obrázku je příklad shoduje konfiguračního souboru XML a JSON:

**Příkaz předvolba XML:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**Příkaz předvolba JSON:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse s AMS .NET SDK

Podle pokynů pro odesílání mediální soubor jako aktivum a vytvoří úlohu s procesorem Azure Media Hyperlapse média.

> [AZURE.NOTE] Měli byste mít už CloudMediaContext v rozsahu s názvem "kontextu" pro tento kód pro práci.  Další informace o tomto najdete v [článku Správa obsahu](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Argument řetězec "hyperConfig" očekává se shoduje konfigurace přednastavené JSON nebo XML, jak jsme je popsali výše.

statické logická hodnota RunHyperlapseJob (vstup řetězce, výstupní řetězec, řetězec hyperConfig) {a / vytvořte majetek s vstupní soubor IAsset materiálů = kontextu. Prostředky. CreateAssetAndUploadSingleFile (vstup "Moje Hyperlapse vstup" AssetCreationOptions.None);

Uchopte výskyty Azure Media Hyperlapse MP IMediaProcessor mp = kontextu. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Vytvoření projektu s úloha IJob Hyperlapse = kontextu. Úlohy. Vytvořit (String.Format (vstup "Hyperlapse {0}")).

Pokud (String.IsNullOrEmpty(hyperConfig)) {/ / konfigurace nesmí být prázdné zpáteční NEPRAVDA;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = projektu. Tasks.AddNew ("Hyperlapse úkolu", mp, hyperConfig, TaskOptions.None) hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse výstup" AssetCreationOptions.None);


úlohy. Submit();

Vytvoření průběh tisk a dotazování úkoly úkolu progressPrintTask = nové Task(() = > {

IJob jobQuery = hodnota null. Proveďte {var progressContext = kontextu; jobQuery = progressContext.Jobs. Kde (j = > j.Id == projektu. ID). First(); Console.WriteLine (řetězec. Formát ("\t \t {1} {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Zobrazí se průběhu)); Thread.Sleep(10000); } během (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Podporované typy souborů

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
