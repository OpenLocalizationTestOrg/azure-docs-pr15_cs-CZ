<properties
    pageTitle="Projektu a úkolu výstupní trvalé v Azure dávce | Microsoft Azure"
    description="Naučte se používat Azure úložiště a trvalé úložiště dávku úkol a úlohy výstup povolit zobrazení tento trvalých výstup Azure portálu."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Zachování výstupu projektu a úkolu dávku Azure

Úkoly, které spouštíte dávku obvykle vytvoří výstup, který musí být uloženy a potom později načítané další úkoly v projektu, klientské aplikace, která spouštět úkoly nebo obojí. Tento výstup může být soubory vytvořené zpracování zadávání dat nebo přidružený k provádění úloh souborů protokolu. Tento článek uvádí knihovna tříd .NET, využívající technika na základě konvence uchovávat těchto úkolů výstup k úložišti objektů Blob Azure jeho zpřístupnění i po odstranění fondů, úlohy a výpočet uzlů.

Pomocí postupu v tomto článku můžete taky zobrazit výstup úkolu v **uložený výstupní soubory** a **uložené protokoly** [Azure portál][portal].

![Výstupní soubory uložené a uložené protokoly voliče portálu][1]

>[AZURE.NOTE] [Azure dávkový soubor konvence] [ nuget_package] knihovna tříd .NET popisované v tomto článku je momentálně v náhledu. Některé z funkcí popsaných v tomto poli se mohou změnit před všeobecně dostupná.

## <a name="task-output-considerations"></a>Důležité informace o úkolu výstup

Když navrhujete dávku řešení, je nutné zvážit několika faktorech související s výstupy projektu a úkolu.

* **Výpočet uzel životnosti**: výpočet uzly, jsou často přechodná, zejména u kterých je povolené automatické měřítko fondů. Výstupy úkoly spuštěné v operačním systému uzel jsou k dispozici pouze a uzel existuje pouze v rámci doby uchování souboru jste nastavili pro daný úkol. Abyste měli jistotu, že se zachová výstupu úkolu, musí úkolů proto nahrajte svoje soubory výstup trvalé úložiště, například Azure úložiště.

* **Výstup úložiště**: uchovávat data výstup úkolu do trvalé úložiště, můžete [Azure úložiště SDK](../storage/storage-dotnet-how-to-use-blobs.md) v kódu úkolu do kontejneru úložiště objektů Blob nahrát výstupu úkolu. Pokud se rozhodnete implementovat kontejner a soubor konvence, klientská aplikace nebo další úkoly v projektu můžete vyhledejte a stáhněte si tento výstup podle názvů.

* **Výstup načítání**: Pokud lze načíst výstup úkolu přímo z výpočetním uzlů v vašeho fondu nebo z Azure úložiště úkolů zachovat jeho výstup. K načítání výstupu úkolu přímo ze výpočetní uzly, musíte název souboru a jeho umístění výstupu na uzel. Pokud potrvají výstup k základnímu úložišti Azure podřízenými úkoly nebo klientské aplikace musí mít úplnou cestu k souboru v úložišti Azure stáhnout pomocí SDK úložiště Azure.

* **Zobrazení výstupu**: přejděte k úkolu dávku Azure portálu a vyberete **souborů na uzel**, zobrazí se všemi soubory přidruženého k úkolu, nejenom výstupu soubory, které vás zajímá. Soubory ve výpočetním uzlech znovu, jsou k dispozici pouze a uzel existuje pouze v rámci doby uchování souboru jste nastavili pro daný úkol. Chcete-li zobrazit výstup úkolu, který jste zachován k základnímu úložišti Azure v portálu nebo aplikaci, třeba [Průzkumníka úložišť Azure][storage_explorer], musíte znát jeho umístění a přejděte k souboru přímo.

## <a name="help-for-persisted-output"></a>Nápověda pro trvalé výstup

Vám více snadno uchovávat projektu a výstupní úkolu, má týmu dávku definované a implementovaná sadu konvence pro zadávání názvů a také .NET knihovna tříd [Azure dávkový soubor konvence] [ nuget_package] knihovny, využívající v aplikacích dávku. Kromě toho portálu Azure známa tyto konvence tak, abyste mohli snadno najít soubory, které jste uložili pomocí knihovny.

## <a name="using-the-file-conventions-library"></a>Použití knihovny konvence soubor

[Azure dávkový soubor konvence] [ nuget_package] je knihovna tříd .NET, aplikace dávku .NET můžete snadno uložit a načíst výstupy úkolu do a z Azure úložiště. Jsou určeny pro použití v úkolu a klient s kódem – v úkol kód pro zachování souborů a kód klienta seznam a načíst je. Úkoly slouží také knihovnu pro načítání výstupy nadřazeného úkoly, jako je třeba ve scénáři [závislosti mezi úkoly](batch-task-dependencies.md) .

Zajistit, že kontejnery úložiště a úkol výstupní soubory jsou s názvem podle konvence a jsou nahrát na správném místě při zachován k základnímu úložišti Azure stará knihovně smluv. Po načtení výstupy snadno najdete výstupy pro jednotlivé úlohy a úkolu ve výpisu nebo načítání výstupy podle ID a účel, takže není nutné znát názvy souborů nebo kde se vyskytuje v úložišti.

Například můžete knihovnu "seznam všechny soubory intermediate pro úkol 7" nebo "získat mnou náhledu miniatur pro úlohu *mít název mujFilm"*bez znáte názvy souborů a umístění v rámci svého účtu úložiště.

### <a name="get-the-library"></a>Získání knihovny

Můžete získat knihovny, která obsahuje nové třídy a slouží k rozšíření [CloudJob] [ net_cloudjob] a [CloudTask] [ net_cloudtask] tříd pomocí nových metod z [NuGet][nuget_package]. Můžete ho přidat do aplikace Visual Studio projektu pomocí [Správce balíčku knihovny NuGet][nuget_manager].

>[AZURE.TIP] [Zdrojový kód] můžete najít[ github_file_conventions] pro knihovnu Azure dávkový soubor konvence na GitHub v sadě Microsoft Azure SDK pro .NET úložiště.

## <a name="requirement-linked-storage-account"></a>Povinné: propojené úložiště účtu

Chcete-li obsahují výstupy trvalé úložiště pomocí knihovny konvence souborů a zobrazit je v portálu Azure, musíte [odkaz účet Azure úložiště](batch-application-packages.md#link-a-storage-account) ke svému účtu dávku. Pokud jste to ještě neudělali, propojení úložiště účtu k vašemu účtu dávku pomocí portálu Azure:

Zásuvné **dávku účet** > **Nastavení** > **Účtu úložiště** > **Účtu úložiště** (žádné) > Vybrat účet úložiště ve vašem předplatném

Podrobnější walk-through na propojení účtu úložiště najdete v článku [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md).

## <a name="persist-output"></a>Zachování výstup

Existují dva hlavní akce provádět při ukládání výstupu projektu a úkolu s knihovnou konvence soubor: vytvoření kontejneru úložiště a uložit výstupní do kontejneru.

>[AZURE.WARNING] Protože ve stejném kontejneru jsou uloženy všechny výstupy projektu a úkolu, [omezení úložiště](../storage/storage-performance-checklist.md#blobs) může být vynutit, pokud velkého počtu úkoly pokusíte zachovat soubory ve stejnou dobu.

### <a name="create-storage-container"></a>Vytvoření kontejneru úložiště

Nejprve úkolů trvalý výstup k základnímu úložišti, musíte vytvořit kontejneru úložiště objektů blob, do kterého budete uložení jeho výstup. To udělat tak, že zavoláte [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Tento způsob rozšíření trvá [CloudStorageAccount] [ net_cloudstorageaccount] objektu jako parametr a vytvoří kontejner tak, že obsah lze zjistit tak, že portálu Azure a metody načítání popsáno dále v tomto článku.

Obvykle umístěte tento kód v klientské aplikaci – aplikace, která vytvoří fondů, projekty a úkoly.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Obsahují výstupy úkolu

Teď jste připraveni kontejneru v úložišti objektů blob úkoly můžete vám ušetří výstup do kontejneru [TaskOutputStorage] [ net_taskoutputstorage] předmětu najdete v knihovně smluv soubor.

V kódu úkolu nejdřív vytvořit [TaskOutputStorage] [ net_taskoutputstorage] objektu a potom po dokončení práce úkolu volání [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] způsob, jak uložit jeho výstup k základnímu úložišti Azure.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Parametr "výstup identifikovat" rozdělíte do kategorií trvalé soubory. Existují čtyři předdefinované [TaskOutputKind] [ net_taskoutputkind] typů: "TaskOutput", "TaskPreview", "TaskLog" a "TaskIntermediate." Můžete také definovat vlastní typy Pokud mohly být užitečné v pracovním postupu.

Tyto typy výstup umožňují určit typ výstupy seznam později dotazu dávka pro trvalé výstupy daného úkolu. Jinými slovy když seznam výstupy pro daný úkol můžete filtrovat seznam na jeden z typů výstup. Například "získávat výstupu *náhledu* pro úkol *109*." Další záznam a načítání výstupy se zobrazí v [Načtení výstupu](#retrieve-output) dále v tomto článku.

>[AZURE.TIP] Typ výstup určuje Azure portálu příslušný soubor umístění: *TaskOutput*-zařazené do kategorií soubory se zobrazí v "Úkolu výstupní soubory" a *TaskLog* soubory se zobrazí v "Úkolu protokoly."

### <a name="store-job-outputs"></a>Obsahují výstupy projektu

Kromě ukládání výstupy úkolu, mohou být uloženy výstupy přidružené celý projekt. Například v úkolu sloučení úlohy vykreslování videa můžete uchovávat plně vykreslená film jako výstup projektu. Po dokončení práce klientské aplikace můžete jednoduše seznam a načíst výstupy pro daný úkol a není nutné k vytvoření dotazu jednotlivé úlohy.

Ukládání výstupu projektu tak, že zavoláte [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] metody, a zadejte [JobOutputKind] [ net_joboutputkind] a název souboru:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Při použití s TaskOutputKind pro úkol výstupy [JobOutputKind] [ net_joboutputkind] parametr zařadit do nějaké kategorie projektu je zachován soubory. Tento parametr umožňuje na pozdější dotaz (seznam) určitý typ výstupu. JobOutputKind obsahuje typy výstup a náhled a podporuje vytváření vlastní typy.

### <a name="store-task-logs"></a>Ukládání protokolů úkolu

Kromě uchování souboru, který se trvalé úložiště dokončení úkolu nebo projektu, budete potřebovat k zachování souborů, které se aktualizují při provádění úloh – protokoly nebo `stdout.txt` a `stderr.txt`, například. K tomuto účelu knihovna Azure dávkový soubor konvence poskytuje [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] metody. S [SaveTrackedAsync][net_savetrackedasync], přehled o aktualizacích do souboru na uzel (na interval, který určíte) a zachovat tyto aktualizace k základnímu úložišti Azure.

V následující fragment kódu používáme [SaveTrackedAsync] [ net_savetrackedasync] aktualizovat `stdout.txt` v úložišti Azure každých 15 sekund během provádění úkolu:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`je jednoduše zástupný symbol pro kód, který by obvykle provedení úkolu. Máte kód, který ke stažení dat z Azure úložiště a provede transformace či výpočet na něj. Které demonstrují důležitou součástí tohoto fragment jak může obtékat takové doručení s kódem v `using` blok pravidelně aktualizovat soubor s [SaveTrackedAsync][net_savetrackedasync].

`Task.Delay` Požaduje na konci tohoto `using` blok zajistit, že agent uzel nějaký čas na Vymazat obsah standardní rezervovat soubor stdout.txt na uzel (agenta uzel je program, který běží v jednotlivých uzlech z fondu a poskytuje rozhraní příkaz a řízení mezi uzel a služba dávku). Tento neprodleně je možné přijít poslední několik sekund, než výstupu. Toto zpoždění nemusí být nutné pro všechny soubory.

>[AZURE.NOTE] Po povolení soubor sledování s SaveTrackedAsync pouze *připojí* sledované souboru jsou zachován k základnímu úložišti Azure. Pomocí této metody pouze pro sledování bez otočení protokoly nebo jiných souborů, které mají být přidána, to znamená, data jenom přibude konec soubor při aktualizaci.

## <a name="retrieve-output"></a>Načtení výstupu

Při načítání trvalých výstup knihovnu Azure dávkový soubor konvence provedete úkol a úlohy zaměřené na zpracování způsobem. Pro dané můžete požádat o výstup úkolu nebo projektu bez nutnosti znát cestou v úložišti objektů blob nebo dokonce názvu souboru. Jednoduše řekněte, "Získávat výstupní soubory pro úkol *109*."

Následující fragment kódu prochází všechny úkoly projektu, vytiskne některé informace o souborech výstup pro daný úkol a stáhne jeho soubory z úložiště.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Úkol výstupy a portálu Azure

Portál Azure zobrazuje výstupy úkolu a protokolů, které jsou trvalé propojený klient úložišti Azure pomocí názvů součástí [Soubor Readme pro službu Azure dávkový soubor konvence][github_file_conventions_readme]. Můžete implementovat tyto konvence sami v jazyce e, nebo můžete použít knihovnu konvence souborů v aplikacích .NET.

### <a name="enable-portal-display"></a>Povolit portálu zobrazení

Povolit zobrazení vaše výstupy na portálu, musí splňovat tyto požadavky:

 1. [Odkaz účet Azure úložiště](#requirement-linked-storage-account) ke svému účtu dávku.
 2. Když uchování výstupy dodržovat předdefinované konvence pro kontejnery úložiště a soubory. Definice tyto konvence můžete najít v knihovně smluv souboru [README][github_file_conventions_readme]. Pokud používáte [Azure dávkový soubor konvence] [ nuget_package] splněny knihovny uchovávat vaše výstup tohoto požadavku.

### <a name="view-outputs-in-the-portal"></a>Zobrazení výstupy na portálu

Zobrazíte výstupy úkolu a protokoly Azure portálu přejděte k úkolu jejichž výstup zajímají a potom klikněte na **uložený výstupní soubory** nebo **Uložit protokoly**. Tento obrázek ukazuje **uložený výstupní soubory** pro daný úkol s ID "007":

![Zásuvné výstupy úkolu na portálu Azure][2]

## <a name="code-sample"></a>Ukázka kódu

[PersistOutputs] [ github_persistoutputs] ukázkový projekt je jednou z [Azure dávku ukázky] [ github_samples] na GitHub. Toto řešení Visual Studio 2015 ukazuje, jak používat knihovnu Azure dávkový soubor konvence pro zachování úkolu výstup trvalé úložiště. Spuštění vzorku, postupujte takto:

1. Otevřete projekt ve **Visual Studiu 2015**.
2. V aplikaci project Microsoft.Azure.Batch.Samples.Common dodejte **AccountSettings.settings** list a úložiště **pověření účtu** .
3. **Vytvoření** (ale není možné spustit) na řešení. Pokud se zobrazí výzva obnovte všechny balíčky NuGet.
4. K nahrání [balíčku aplikace](batch-application-packages.md) pro **PersistOutputsTask**použijte portál Azure. Zahrnout `PersistOutputsTask.exe` a jeho závislá sestavení v balíčku ZIP nastavení ID aplikace na "PersistOutputsTask" a verze balíček aplikace "1.0".
5. **Zahájení** Projekt (spustit) **PersistOutputs** .

## <a name="next-steps"></a>Další kroky

### <a name="application-deployment"></a>Nasazení aplikace

Funkce [balíčků aplikací](batch-application-packages.md) dávku poskytuje snadný způsob, jak současně nasazení a verze aplikace, které úkolů na provést výpočet uzlů.

### <a name="installing-applications-and-staging-data"></a>Instalace aplikací a přípravu dat

Podívejte se na [instalace aplikace a pracovní data v listu výpočet uzly] [ forum_post] příspěvek ve fóru Azure dávku základní informace o různých metod Příprava uzly pro spuštění úkoly. Který napsal jednu z Azure dávku členy týmu, tento příspěvek je dobré základy na různé způsoby, jak dostat soubory (včetně aplikací a data zadání úkolu) do výpočetního uzly a některé důležité informace pro obou metod.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Výstupní soubory uložené a uložené protokoly voliče portálu"
[2]: ./media/batch-task-output/task-output-02.png "Zásuvné výstupy úkolu na portálu Azure"