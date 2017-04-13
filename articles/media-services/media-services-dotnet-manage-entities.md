
<properties 
    pageTitle="Správa aktiv a související entity s Media Services .NET SDK" 
    description="Naučte se spravovat prostředky a související entity s Media Services SDK pro .NET" 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Správa aktiv a související entity s Media Services .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [ZBÝVAJÍCÍ](media-services-rest-manage-entities.md)


Toto téma ukazuje, jak provádět následující úkoly správy Media Services:

- Získat odkaz materiálů 
- Získání odkazu projektu 
- Seznam všech prostředky 
- Seznam úlohy a prostředky 
- Seznam všech zásady přístupu 
- Seznam všech Locator
- Výčet prostřednictvím velkých sad entity
- Odstranění aktivum 
- Odstranění úlohy 
- Odstranění zásady přístupu 

##<a name="prerequisites"></a>Zjistit předpoklady pro 

V tématu [nastavení prostředí](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Získat odkaz materiálů

Časté úkolu je odkaz na existující materiálů vstoupit Media Services. Následující příklad ukazuje, jak můžete rychle referenční materiály z kolekce prostředky na serveru objekt kontextu podle aktivum Id.
Následující příklad používá Linq dotaz na získat odkaz ke stávajícímu IAsset objektu.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Získání odkazu projektu

Při práci s zpracování úkolů v kódu Media Services často potřebujete k získání odkazu na existující úlohu podle Id. Následující příklad ukazuje, jak získat odkaz na objekt IJob z kolekce úlohy.
WarningWarning budete muset odkazovat úlohu při zahájení práce kódování dlouho probíhajících a potřebujete zkontrolovat stav úlohy v podprocesu. V případě takto až metodu vrátí z podproces, bude potřeba načíst aktualizovaná odkaz na projektu.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Seznam všech prostředky

Narůstající velikostí počet prostředky, které máte v úložišti, je vhodné seznam svém majetku. Následující příklad ukazuje, jak iteraci kolekci prostředky objektu místní server. S každý majetek příklad kódu také zapíše některé její nemovitostí s hodnotou ke konzole. Každý majetek například může obsahovat větší množství souborů médií. Příklad kódu píše všechny soubory související s každý materiálů.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Seznam úlohy a prostředky

Důležité související úkol je seznam aktiv pomocí své přidružené práce v Media Services. Následující příklad ukazuje, jak získat seznam všech objektů IJob a pak pro jednotlivé úlohy zobrazí vlastnosti o projektu, všechny související úkoly, všechny při zadávání majetek i všechny výstup prostředky. Kód v tomto příkladu může být užitečné pro mnoho dalších úkolů. Například pokud budete chtít seznam prostředky výstup z jedné nebo více kódování úloh, které jste dříve spustili, tento kód ukazuje, jak pro přístup k výstup prostředky. Když máte odkaz na aktivum výstup, lze potom doručit obsah jiní uživatelé nebo aplikace tak, že si ji stáhnete nebo adresu URL. 

Další informace o možnostech aby poskytly prostředky najdete v článku [Předvádění aktiv pomocí Media Services SDK pro .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Seznam všech zásady přístupu

V Media Services můžete definovat zásady přístupu na aktivum nebo jeho soubory. Zásady přístupu definuje oprávnění k souboru nebo aktiva (jaký typ přístupu a dobu trvání). V kódu Media Services obvykle definovat zásady přístupu vytvořením objektu IAccessPolicy a přiřadí se mu existující materiálů. Můžete vytvořit ILocator objekt, který umožňuje poskytovat přímý přístup k vybavení Media Services. Projekt aplikace Visual Studio, který je součástí řady si přečtěte následující dokumentaci obsahuje několik příkladů kódu, které ukazují, jak vytvoření a přiřazení zásady přístupu a Locator prostředky.

Následující příklad ukazuje, jak chcete-li zobrazit všechny zásady přístupu na serveru a zobrazí typ oprávnění přidružená jednotlivým. Další užitečné způsob zobrazení zásady přístupu je seznam všech objektů ILocator na serveru a pak pro každé locator můžete vytvořit seznam jeho přidružený přístupu pomocí vlastností AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Seznam všech Locator

URL je adresa URL, která obsahuje přímé cesty pro přístup k aktivum spolu s oprávněními k majetku podle definice zásady přístupu locator. Každý majetek můžete mít kolekce ILocator objektů přidružených na vlastností Locator. Kontext serveru má i kolekci Locator, která obsahuje všechny Locator.

Následující příklad uvádí všechny Locator na serveru. Pro každý locator zobrazí Id zásada související materiály a přístup. Taky zobrazí typ oprávnění, datum vypršení platnosti a úplnou cestu k majetku.

Všimněte si, že cesta URL k aktivum pouze základní adresy URL majetku. K vytvoření přímé cesty pro jednotlivé soubory, které může vyhledejte uživatele nebo aplikace, musí kódu přidat cestu konkrétním souborem cestu URL. Další informace o tomto postupu najdete v tématu [Předvádění aktiv pomocí Media Services SDK pro .NET](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Výčet prostřednictvím velkých sad entity

Při dotazování entity, je omezení na 1000 entity vrátit najednou, protože veřejné v2 ZBÝVAJÍCÍ omezení výsledků dotazu 1000 výsledky. Potřebujete provádějte přeskočit a opět převzít výčtu velkých sad entity. 
    
Následující funkce prochází všechny úlohy v zadané účtu služeb média. Media Services vrátí 1000 úlohy v kolekci úlohy. Funkce využívá přeskočit a dělat, abyste měli jistotu, že všechny úlohy se nachází (v případě máte více než 1000 úlohy ve vašem účtu).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Odstranění aktivum

Následující příklad odstraní aktivum.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Odstranění úlohy

Odstranění úlohy, zaškrtněte stav úlohy podle státu vlastnost. Úlohy, které jsou po dokončení nebo zrušení odstraněním, zatímco úlohy, které jsou v některých jiných států, například ve frontě plánované a zpracování, je nutné nejprve zrušit a potom sami odstranili.
Následující příklad ukazuje metodu pro odstranění úlohy kontrolou stavy úloh a potom odstraňování při stavu dokončení nebo zrušení. Tento kód závisí na předchozí oddíl v tomto tématu usnadňující odkaz na úlohy: získat odkaz projektu.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Odstranění zásady přístupu

Následující příklad ukazuje, jak získat odkaz na zásady přístupu na základě zásad Id a potom odstraňte zásadu.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
