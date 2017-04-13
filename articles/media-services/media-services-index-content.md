<properties
    pageTitle="Indexování mediální soubory s Azure Media indexování"
    description="Azure Media indexování umožňuje provádět s možností vyhledávání obsahu mediální soubory a generovat fulltextové přepis skryté titulky a klíčových slov. Toto téma ukazuje, jak používat médií indexování."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Indexování mediální soubory s Azure Media indexování


Azure Media indexování umožňuje provádět s možností vyhledávání obsahu mediální soubory a generovat fulltextové přepis skryté titulky a klíčových slov. Můžete zpracovat jeden mediálních souborů nebo více mediální soubory v listu.  

>[AZURE.IMPORTANT] Indexování obsahu, nezapomeňte použít mediální soubory, které mají jasný řeči (bez hudební doprovod hluku, efekty a hiss mikrofon). Některé příklady příslušný obsah: zaznamenaných schůzek, přednášek nebo prezentace. Následující obsah nemusí být vhodné pro indexování: filmy, zobrazuje televizního cokoli s smíšených zvuk a zvukových efektů špatně zaznamenané obsah s šum na pozadí (hiss).


Indexovací úlohy mohou způsobit následující výstupy:

- Zavřený titulek souborů v těchto formátech: **SAMI**, **TTML**a **WebVTT**.

    Skryté titulky soubory obsahují značku s názvem Recognizability, které skóre indexovací úlohy na základě toho, jak rozpoznatelný řeči v tomto videu zdroje.  Hodnota Recognizability můžete použít k obrazovce výstupní soubory pro zvýšení použitelnosti. Nízké skóre bude znamenat špatnou indexování výsledky z důvodu kvalitu zvuku.
- Klíčové slovo souboru (XML).
- Zvuk indexování souboru blob (AIB) pro použití se serverem SQL server.

    Další informace najdete v tématu [Použití AIB soubory s Azure Media indexování a SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


Toto téma ukazuje, jak vytvořit indexovací úlohy **Index aktivum** a **Indexovat najednou několik souborů**.

V nejnovějších aktualizacích Azure Media indexování najdete v článku [blogy Media Services](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Používání souborů konfigurace a manifestu indexování úkoly

Víc informací můžete určit indexování úkolů pomocí nástroje Konfigurace úkolu. Můžete třeba určit které metadat pro účely mediální soubor. Tato metadata slouží modulem jazyk rozbalte jeho slovník a výrazně zvyšuje přesnost rozpoznávání řeči.  Je taky možné zadejte požadované výstupní soubory.

Můžete taky zpracováváte více mediální soubory v celém dokumentu pomocí seznamu souboru.

Další informace najdete v tématu [Úkolu předvolby pro Azure Media indexování](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Index aktivum

Podle pokynů pro odesílání mediální soubor jako aktivum a vytvoří úlohu indexovat majetku.

Všimněte si, že pokud není zadán žádný konfigurační soubor, multimediálního souboru indexování s všechna výchozí nastavení.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Výstupní soubory

Ve výchozím nastavení indexovací úlohy generuje následující výstupu. Soubory, které budou uloženy v první výstup materiálů.

Pokud existuje více než jeden soubor vstupních média, indexování vygeneruje seznamu soubor pro výstupy úlohu s názvem "JobResult.txt". Pro každý vstup multimediálního souboru, výsledný AIB, SAMI, TTML, WebVTT a soubory klíčové slovo, jsou postupně lichých a s názvem použití "Aliasu".

Název souboru | Popis
----------|------------
__InputFileName.aib__ | Zvukový soubor indexování objektů blob. <br /><br /> Zvukový soubor indexování objektů Blob (AIB) je binární soubor, který lze prohledávat v Microsoft SQL server pomocí vyhledávání celý text.  Soubor AIB je výhodnější než soubory jednoduché titulek, protože obsahuje alternativy u každého slova, což mnohem lepší vyhledávání. <br/> <br/>Vyžaduje instalaci doplňku SQL indexování na počítači spuštěné Microsoft SQL server 2008 nebo novějším. Hledání AIB pomocí Microsoft SQL server celý text můžete vyhledávat přesnější výsledky hledání než hledání titulků soubory generované WAMI. Je to proto AIB obsahuje word alternativy která zvuková podobné zatímco titulků soubory, které obsahují nejvyšší slovo spolehlivosti pro každou část zvuk. Je-li hledání mluveného slova upmost důležité, doporučujeme používat AIB ve spojení s Microsoft SQL Server.<br/><br/> Pokud si Pokud chcete stáhnout doplněk, klikněte na <a href="http://aka.ms/indexersql">Azure Media indexování SQL doplňků</a>. <br/><br/>Je také možné použít jiné vyhledávací weby například Apache Lucene/Solr jednoduše indexovat videa na základě skryté titulky a soubory ve formátu XML klíčové slovo, ale výsledkem bude méně přesné výsledky hledání.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Skrytých titulků kopie souborů ve formátech SAMI, TTML a WebVTT.<br/><br/>Se mohou sloužit k usnadnění zvukových souborů a videosouborů pro osoby se sluchovým postižením.<br/><br/>Zavřené soubory titulek vložit značku s názvem <b>Recognizability</b> je které skóre indexovací úlohy podle jak rozpoznatelný rozpoznávání řeči ve zdroji videa.  Hodnota <b>Recognizability</b> můžete použít k obrazovce výstupní soubory pro zvýšení použitelnosti. Nízké skóre bude znamenat špatnou indexování výsledky z důvodu kvalitu zvuku.
__InputFileName.kw.xml<br />InputFileName.info__ |Informace o klíčových slov a soubory. <br/><br/>Klíčové slovo soubor je soubor XML, který obsahuje klíčová slova extrahuje z obsahu řeči počet_plateb a posun informace. <br/><br/>Informace o souboru je soubor ve formátu prostého textu, který obsahuje podrobné informace o jednotlivých rozpoznala termín. První řádek je speciální a obsahuje skóre Recognizability. Každý další řádek je seznam odděleného tabulátory následujícími údaji: zahájení čas čas ukončení, slovo nebo frázi, spolehlivosti. Čas jsou uvedeny v sekundách a důvěru je uveden jako číslo od 0-1. <br/><br/>Příklad řádku: "1,20 1,45 word 0.67" <br/><br/>Tyto soubory můžete umožňuje pro počet účely, jako například provádění analýzy řeči nebo zveřejněným vyhledávače, jako je Bing, Google nebo Microsoft SharePoint, abyste mediální soubory intuitivnější nebo dokonce používá k poskytování víc relevantních služby Active Directory.
__JobResult.txt__ |Výstup manifestu, prezentace pouze v případě, že indexování více souborů obsahující následující informace:<br/><br/><table border="1"><tr><th>Vstupní_soubor</th><th>Alias</th><th>MediaLength</th><th>Chyba</th></tr><tr><td>a.MP4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.MP4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.MP4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Není-li všechny soubory vstupních média indexovaných úspěšně, indexovací úlohy se nepovede s kódem chyby 4000. Další informace najdete v tématu [kódy chyb](#error_codes).

## <a name="index-multiple-files"></a>Index najednou několik souborů

Podle pokynů pro odešle více mediální soubory jako aktivum a vytvoří úlohu indexovat tyto soubory v listu.

Na seznamu soubor s příponou .lst jsou vytvořené a odesílání do majetku. Soubor obsahuje seznam všech souborů materiálů. Další informace najdete v tématu [Úkolu předvolby pro Azure Media indexování](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Částečně úspěchu projektu

Není-li všechny soubory vstupních média indexovaných úspěšně, indexovací úlohy se nepovede s kódem chyby 4000. Další informace najdete v tématu [kódy chyb](#error_codes).


Stejné výstupy (jako úspěchu úlohy) se vytvářejí. Můžete vytvořit odkaz ve výstupním souboru seznamu zjistěte vstupní soubory, které jsou selže, podle chybové hodnoty sloupce. Pro zadávání soubory, které se nepodařilo, výsledný AIB, SAMI, TTML, WebVTT a klíčových slov soubory není vygeneruje se.

### <a id="preset"></a>Úkol předvolby pro Azure Media indexování

Zpracování z Azure Media indexování můžete přizpůsobit zadáním volitelné úkolu přednastavené vedle úkolu.  V následujícím textu najdete formát xml této konfiguraci.

Jméno | Vyžadovat | Popis
----|----|---
__zadání vstupních hodnot__ | NEPRAVDA | Materiály soubor(y), které potřebujete, který chcete indexovat.</p><p>Azure Media indexování podporuje následující formáty multimediálních souborů: MP4 WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Zadání názvu souboru (s) v atributu **jméno** nebo **seznam** **vstupních** prvku (viz níže). Pokud nezadáte souborů materiálů, které funkce index, je vybráno primární soubor. Pokud soubor žádné primární materiálů první soubor v zadávání materiálů indexováno.</p><p>Explicitně zadání názvu souboru materiálů, postup:<br />`<input name="TestFile.wmv">`<br /><br />Můžete taky index více materiálů souborů v celém dokumentu (maximálně 10). Akce:<br /><br /><ol class="ordered"><li><p>Vytvoření textového souboru (seznamu soubor) a jak ho .lst rozšíření. </p></li><li><p>Přidejte seznam jmen všech materiálů soubor v zadávání materiálů k tomuto seznamu souboru. </p></li><li><p>Přidejte do aktiva (Odeslat) thanifest soubor.  </p></li><li><p>Zadejte název soubor v atributu seznamu na vstup.<br />`<input list="input.lst">`</li></ol><br /><br />Poznámka: Pokud chcete soubor přidat více než 10 soubory, se indexovací úlohy nepovede s kódem chyby 2006.
__metadata__ | NEPRAVDA | Metadata pro zadané materiálů soubory, které používají pro přizpůsobení slovník.  Užitečné Příprava indexování rozpoznatelný nestandardní slovník slova jako vlastních jmen.<br />`<metadata key="..." value="..."/>` <br /><br />Můžete zadat __hodnoty__ pro předdefinované __klíče__. Nyní jsou podporovány následující klíče:<br /><br />"název" a "Popis" – používá k přizpůsobení slovník doladit jazyk modelu pro danou úlohu a zlepšit přesnost rozpoznávání řeči.  Hodnoty inokula Internet vyhledávání pro hledání dokumentů kontextově příslušný text rozšířit interní slovník pro doby trvání úkolu indexování pomocí obsah.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__funkce__ <br /><br /> Přidá ve verzi 1.2. Je v současné době, pouze podporované funkce rozpoznávání řeči ("funkci Automatické obnovení systému").| NEPRAVDA | Funkce rozpoznávání řeči obsahuje následující klíče nastavení:<table><tr><th><p>Klíč</p></th>     <th><p>Popis</p></th><th><p>Příklad hodnoty</p></th></tr><tr><td><p>Jazyk</p></td><td><p>Přirozeného jazyka uplatnit v multimediálních souborů.</p></td><td><p>Angličtina, španělština</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>oddělené středníkem seznam popisků k obrázkům požadované výstupních formátů (pokud existuje)</p></td><td><p>ttml sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Logická příznaku určující, zda soubor AIB požaduje (pro použití s SQL serverem a zákazníka indexování IFilter).  Další informace najdete v tématu <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Použití AIB soubory s Azure Media indexování a SQL Server</a>.</p></td><td><p>PRAVDA a naopak. NEPRAVDA</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Logická příznaku určující, zda požaduje soubor XML klíčových slov.</p></td><td><p>PRAVDA a naopak. NEPRAVDA. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Logická příznaku určující, zda chcete vynutit úplné titulků (bez ohledu na spolehlivosti).  </p><p>Výchozí hodnota je false, v takovém případě slova nebo fráze, které mají méně než 50 % spolehlivosti vynechány výstupy konečný titulek a nahrazuje tři tečky (trojtečkou (...)).  Na tři tečky jsou vhodné pro řízení kvality popisků k obrázkům a auditování.</p></td><td><p>PRAVDA a naopak. NEPRAVDA. </p></td></tr></table>

### <a id="error_codes"></a>Kódy chyb

V případě chyby měli hlásit Azure Media indexování zpět jednu z následujících chyb:

Kód | Jméno | Možné příčiny
-----|------|------------------
2000 | Neplatná konfigurace | Neplatná konfigurace
2001 | Neplatné vstupní prostředky | Chybějící vstupní prostředky nebo prázdná materiálů.
2002 | Neplatný manifest | Manifest je prázdná nebo manifestu obsahuje neplatné položky.
2003 | Nepodařilo stáhnout mediálních souborů | Neplatné URL v seznamu souborů.
2004 | Nepodporované Protocol (protokol) | Protokol adresy URL media není podporován.
2005 | Nepodporovaný typ souboru | Typ souboru vstupních média není podporován.
2006 | Příliš mnoho vstupní souborů | Zadávání manifestu existuje více než 10 soubory.
3000 | Nepodařilo se dekódovat multimediálního souboru | Nepodporované médií kodek <br/>nebo<br/> Poškozený multimediálního souboru <br/>nebo<br/> Bez datového proudu zvuku v vstupních média.
4000 | Indexování dávku částečně úspěšně | Některé soubory vstupních média nelze indexovat. Další informace najdete v tématu <a href="#output_files">výstupní soubory</a>.
Další | Vnitřní chyby | Obraťte se na tým podpory. indexer@microsoft.com


## <a id="supported_languages"></a>Podporované jazyky

V současné době podporuje jazyky angličtině a španělštině. Další informace najdete v tématu [verze 1.2 vydání blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Související odkazy

[Mediální služby Azure přehled analýzy](media-services-analytics-overview.md)

[Používání souborů AIB s Azure Media indexování a SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexování mediální soubory s náhledem indexování 2 Azure Media](media-services-process-content-with-indexer2.md)
