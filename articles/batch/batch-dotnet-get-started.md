<properties
    pageTitle="Kurz – Začínáme s knihovnou Azure dávku .NET | Microsoft Azure"
    description="Přečtěte si základní koncepty dávku Azure a jak se dají pro službu dávka s příklad scénáře."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Začínáme s knihovnou Azure dávka pro .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Naučte se základy [Azure dávku] [ azure_batch] a [.NET dávku] [ net_api] knihovny v tomto článku jak probereme Tato ukázka C# aplikaci krok za krokem. Podíváme na jak tato ukázková aplikace využívá službu dávku zpracuje paralelní pracovní zátěž v cloudu, a jak pracuje s [Azure úložiště](../storage/storage-introduction.md) souborů pracovní a načítání. Dozvíte běžné techniky pracovního postupu aplikace dávku. Budete taky získat základní principy hlavní komponenty listu, třeba projekty, úkoly, skupiny a výpočet uzlů.

![Pracovní postup řešení dávku (basic)][11]<br/>

## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto článku se předpokládá být praxi C# a Visual Studia. Dále předpokládá, že se vám povede požadavkům účtu vytváření, které jsou uvedeny níže Azure a dávku a úložiště služby.

### <a name="accounts"></a>Účty

- **Azure účtu**: Pokud ještě nemáte předplatné Azure, [Vytvořit bezplatný účet Azure][azure_free_account].
- **Dávkové účtu**: Pokud máte předplatné Azure, [vytvořte účet Azure dávku](batch-account-create-portal.md).
- **Úložiště účtu**: Přečtěte si článek [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Dávkové v současné době podporuje *pouze* typ účtu úložiště **univerzální** podle pokynů v kroku #5 [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

Musí mít **Visual Studio 2015** pro vytvoření ukázkové projektu. Bezplatná a zkušební verze aplikace Visual Studio můžete najít v [základní informace o produktech Visual Studio 2015][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Ukázka *DotNetTutorial* kódu

[DotNetTutorial] [ github_dotnettutorial] vzorek tvoří jeden z mnoha ukázky zjištěným v [azure dávku vzorky] [ github_samples] úložiště na GitHub. Po kliknutí na tlačítko **Stáhnout ZIP** na domovskou stránku úložiště nebo po kliknutí na [azure dávku vzorky master.zip] můžete si stáhnout vzorku[ github_samples_zip] přímý odkaz. Jakmile jste extrahovaných obsah souboru ZIP, bude hledat řešení v následující složce:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure dávku Explorer (volitelné)

[Průzkumník dávku Azure] [ github_batchexplorer] je bezplatná nástroj, který je součástí [azure dávku vzorky] [ github_samples] úložiště na GitHub. Když není potřebná k dokončení tohoto kurzu, může být užitečné při vytvoření a ladění dávku řešení.

## <a name="dotnettutorial-sample-project-overview"></a>Přehled projektu ukázkové DotNetTutorial

Ukázka kódu *DotNetTutorial* je Visual Studio 2015 řešení, které obsahuje dva projekty: **DotNetTutorial** a **TaskApplication**.

- **DotNetTutorial** je klientská aplikace, který spolupracuje s dávku a úložiště služby na provést paralelní pracovní zátěž výpočet uzly (virtuálních počítačích). DotNetTutorial běží na místním počítači.

- **TaskApplication** je program, který běží ve výpočetním uzlech Azure k provedení skutečné práce. Ve vzorku `TaskApplication.exe` analyzuje text v souboru stahují z Azure úložiště (vstupní soubor). Potom vytvoří textového souboru (ve výstupním souboru) se seznamem začátek tři slova, která se zobrazí ve vstupním souboru. Po vytvoření výstupní soubor, TaskApplication, odešlou se soubor Azure úložiště. To díky kterému je dostupný ke klientské aplikaci ke stažení. TaskApplication běží paralelní více uzlů výpočetním ve službě dávku.

Následující obrázek znázorňuje primární operace, které provádí klientské aplikace, *DotNetTutorial*a aplikace, která je prováděných úloh *TaskApplication*. Základní pracovní postup je typické pro mnoho výpočetním řešení, které jsou vytvořené s listem. Během neuvádí všechny funkce dostupné ve službě dávku, téměř každý dávku scénář zahrnuje podobné procesů.

![Příkladem dávku pracovního postupu][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) Vytvoření **kontejnery** v úložišti objektů Blob Azure.<br/>
[**Krok 2.**](#step-2-upload-task-application-and-data-files) Nahrajte úkolu a zadávání souborů aplikace kontejnery.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Vytvoření dávky **fondu**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Při přihlášení ke fondu fondu **StartTask** stáhne úkolu binární soubory (TaskApplication) uzlů.<br/>
[**Krok 4.**](#step-4-create-batch-job) Vytvoření dávky **úlohy**.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Přidání **úkolů** do projektu.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Úkoly jsou naplánované na uzly provést.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Každý úkol stáhne jeho zadávání dat z Azure úložiště a pak spuštění, nebude zahájen.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Sledování úkolů.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jak dokončení úkolů, uložení jejich výstupní data k základnímu úložišti Azure.<br/>
[**Krok 7.**](#step-7-download-task-output) Stáhněte si úkolu výstup z úložiště.

Jak je uvedeno, nemusí být vždy dávku řešení provede tyto přesné kroky a může zahrnovat mnoho dalších, ale ukázková aplikace *DotNetTutorial* ukazuje běžné procesy součástí dávku řešení.

## <a name="build-the-dotnettutorial-sample-project"></a>Vytvoření projektu ukázkové *DotNetTutorial*

Před mohli úspěšně spustit vzorku, je nutné zadat přihlašovací údaje účtu dávku a úložiště v aplikaci project *DotNetTutorial* `Program.cs` soubor. Pokud jste tak dosud neučinili, otevřete řešení ve Visual Studiu dvojím kliknutím `DotNetTutorial.sln` soubor řešení. Nebo otevřete ve Visual Studiu pomocí **Soubor > Otevřít > projektu/řešení** nabídky.

Otevřít `Program.cs` v rámci projektu *DotNetTutorial* . Jak je uvedeno v horní části souboru zadejte svoje přihlašovací údaje:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Výše uvedené, aktuálně zadejte přihlašovací údaje účtu úložiště **univerzální** v úložišti Azure. Aplikace dávku použít úložiště objektů blob v rámci účtu úložiště **univerzální** . Není zadejte přihlašovací údaje pro úložiště účet, který byl vytvořen tak, že vyberete typ účtu *úložiště objektů Blob* .

List a úložiště účtu přihlašovacích údajů do účtu zásuvné každé služby, můžete najít v [Azure portál][azure_portal]:

![Dávkové přihlašovací údaje na portálu][9]
![úložiště pověření v portálu][10]<br/>

Teď jste aktualizovali projektu pomocí svých přihlašovacích údajů, klikněte pravým tlačítkem myši na řešení v Průzkumníku řešení a klikněte na tlačítko **Sestavit řešení**. Pokud se zobrazí výzva, potvrďte obnovení všech balíčků NuGet.

> [AZURE.TIP] Pokud nejsou automaticky obnoví balíčků NuGet nebo pokud uvidíte chyby o selhání obnovíte balíčků, zajištění [Správce balíčků NuGet] [ nuget_packagemgr] nainstalovaný. Potom povolte stáhnout chybějící balíčků. V tématu [Povolení balíčku obnovení při vytváření] [ nuget_restore] povolit Stáhnout balíček.

V následujících částech rozdělí ukázková aplikace do kroky, které provede zpracuje úlohu ve službě dávku jsme diskutovat o tyto kroky podrobností. Budeme rádi, když pracujete jak prostřednictvím ve zbývající části tohoto článku od ne každý řádek kód v ukázce pojednává v nápovědě k otevření řešení ve Visual Studiu.

Přejděte do horní části `MainAsync` metoda v aplikaci project *DotNetTutorial* `Program.cs` soubor, který chcete začít od kroku 1. Každý krok pod pak zhruba spočívá v průběh volání metody v `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Krok 1: Vytvoření úložiště kontejnery

![Vytvoření kontejnery v úložišti Azure][1]
<br/>

Dávkové podporuje předdefinované interakce s úložištěm Azure. Kontejnery ve vašem účtu úložiště poskytne soubory potřeby tak, že úkoly, které běží ve vašem účtu dávku. Kontejnery taky poskytují místo pro ukládání výstupu data, která vytvoří úkoly. První věc, kterou *DotNetTutorial* klientské aplikace je vytvoření tři kontejnery v [Úložišti objektů Blob Azure](../storage/storage-introduction.md):

- **aplikace**: Tento kontejner se uloží aplikaci spusťte úkoly i některé z jejích závislostí, například DLL.
- **zadávání**: úkoly stáhne datové soubory zpracuje ze *vstupní* kontejner.
- **výstup**: při úkoly dokončit zpracování vstupní souboru, budou do kontejneru *výstup* nahraje výsledky.

Abyste mohli pracovat s účtem úložiště a vytvořit kontejnery, doporučujeme používat [Azure úložiště klienta knihovny pro .NET][net_api_storage]. Vytvoříme odkaz na účet s [CloudStorageAccount][net_cloudstorageaccount]a od vytvořit [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Používáme `blobClient` odkaz v aplikaci a předávají jako parametr několika postupů. Příklad je v bloku kódu, který následuje výše uvedených možností, kde název `CreateContainerIfNotExistAsync` možnosti vytvořit skutečně kontejnery.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Po vytvoření kontejnery aplikaci teď nemůžete nahrávat soubory, které se použije úkoly.

> [AZURE.TIP] [Použití úložiště objektů Blob z .NET](../storage/storage-dotnet-how-to-use-blobs.md) poskytuje dobrý přehled o práci s objekty BLOB Azure úložiště kontejnery. By měl být v horní části seznamu čtení, jak začít pracovat s listem.

## <a name="step-2-upload-task-application-and-data-files"></a>Krok 2: Odeslání úkolu aplikace a datových souborů

![Odeslání aplikace úkolu a vstupní (data) souborů pro kontejnery][2]
<br/>

V operaci nahrát soubor *DotNetTutorial* definováno kolekce cesty k souboru **aplikace** a **zadávání** platných v místním počítači. Potom se odešlou se tyto soubory kontejnery, které jste vytvořili v předchozím kroku.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Existují dva způsoby v `Program.cs` , které jsou součástí proces nahrávání:

- `UploadFilesToContainerAsync`: Tento způsob vrátí kolekci [ResourceFile] [ net_resourcefile] objektů (popsané níže) a interně volání `UploadFileToContainerAsync` nahrát každý soubor, který je předaná parametr *filePaths* .
- `UploadFileToContainerAsync`: Toto je metodu, která ve skutečnosti provádí nahrávání souboru a vytvoří [ResourceFile] [ net_resourcefile] objekty. Po odeslání souboru, získá sdílený přístup podpis (přidružení zabezpečení) souboru a vrací ResourceFile objekt, který ho představuje. Sdílené přístup, který podpisy jsou také popsána níže.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ net_resourcefile] obsahuje úkoly v listu s adresou URL k souboru v úložišti Azure, která se stahuje do výpočetního uzel před spuštěním tohoto úkolu. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] vlastnost určuje úplnou adresu URL soubor uložených v úložišti Azure. Adresu URL mohou také obsahovat podpis sdílený přístup (přidružení zabezpečení), který obsahuje zabezpečený přístup k souboru. U většiny typů úkolů v rámci dávku .NET obsahuje *ResourceFiles* vlastnost, včetně:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Ukázková aplikace DotNetTutorial nepoužívá JobPreparationTask nebo JobReleaseTask typů úkolů, ale můžete další informace o nich v [Spustit přípravu a dokončení úlohy v listu Azure výpočet uzlů](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Sdílený přístup k podpisu (přidružení zabezpečení)

Sdílený přístup podpisy jsou řetězce, které – pokud jako součást adresy URL, poskytnutí zabezpečeného přístupu k kontejnerů a objektů BLOB v úložišti Azure. DotNetTutorial používá objektů blob a kontejneru sdílené přístup k podpisu adresy URL a ukazuje, jak získat tyto řetězce podpis sdílený přístup z úložiště služby.

- **Kulatý sdílené podpisy přístup**: do fondu StartTask v DotNetTutorial používá objektů blob sdílené přístup podpisy po stažení aplikace binární soubory a soubory zadávání dat z úložiště (viz krok #3 níže). `UploadFileToContainerAsync` Metoda v prvku DotNetTutorial `Program.cs` obsahuje kód, který získá každý objektů blob sdílený přístup podpis. Stejně tak tak, že zavoláte [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Kontejner sdílené podpisy přístup**: jako každý úkol dokončí svou práci na uzel výpočetním, odešle svůj výstupní soubor do kontejneru *výstup* v úložišti Azure. K tomu, používá TaskApplication podpis kontejneru sdílený přístup, který obsahuje zápisu do kontejneru jako součást cestu při odeslání souboru. Získání kontejneru sdílený přístup podpis dokončení podobným způsobem jako při získání objektů blob sdílených podpis přístup. V DotNetTutorial, bude zjistíte, že `GetContainerSasUrl` helper metoda volá [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] k tomu nevyzve. Budete číst Další informace o tom, jak TaskApplication používá kontejneru sdílené aplikace access do podpisu v "krok 6: sledování úkolů."

> [AZURE.TIP] Podívejte se na řadu složené ze dvou částí na sdílený přístup podpisů [část 1: Principy modelu podpisu (přidružení zabezpečení) sdílený přístup](../storage/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytváření a používání podpisu sdílený přístup (přidružení zabezpečení) s úložiště objektů Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), zobrazíte další informace o poskytnutí zabezpečeného přístupu k datům ve vašem účtu úložiště.

## <a name="step-3-create-batch-pool"></a>Krok 3: Vytvoření fondu dávku

![Vytvoření fondu dávku][3]
<br/>

Dávkové **fondu** je sada výpočetním uzly (virtuálních počítačích), na kterých spustí dávku úkolů projektu.

Odešle žádost a datových souborů k tomuto účtu úložiště, *DotNetTutorial* se spustí po jeho interakci se službou dávku pomocí knihovnu dávku .NET. K tomu nevyzve [BatchClient] [ net_batchclient] nově vytvořený:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Pak fond uzlů výpočetním vytvořené v účtu dávka s volání `CreatePoolAsync`. `CreatePoolAsync`použití [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoda skutečně vytvoření fondu ve službě dávku.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Při vytváření fond s [CreatePool][net_pool_create], zadáte několik parametrů, například počtu uzlů výpočetním, [velikost uzlů](../cloud-services/cloud-services-sizes-specs.md)a na uzly operační systém. V *DotNetTutorial*používáme [CloudServiceConfiguration] [ net_cloudserviceconfiguration] můžete určit, Windows Server 2012 R2 z [Cloudovým službám](../cloud-services/cloud-services-guestos-update-matrix.md). Však zadáním [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] místo toho můžete vytvořit fondů uzly vytvořené ze služby obrázky Marketplace, což zahrnuje Windows a Linux obrázky – Další informace najdete v článku [poskytnutí Linux výpočet uzlů v Azure dávku fondů](batch-linux-nodes.md) .

> [AZURE.IMPORTANT] Vám bude účtovaná za pro využití prostředků v listu. Minimalizovat náklady, můžete snížit `targetDedicated` 1 před spuštěním vzorku.

Spolu s tyto vlastnosti pole fyzicky uzel můžete také určit [StartTask] [ net_pool_starttask] skupiny. StartTask spustí v jednotlivých uzlech a uzel spojí fondu pokaždé, když se nerestartuje uzel. StartTask je užitečné především pro instalaci aplikací ve výpočetním uzlech před plnění úkolů. Například pokud úkolů zpracování dat pomocí skriptů Python, můžete použít StartTask nainstalovat Python uzlech výpočetním.

V této aplikaci vzorku StartTask slouží ke kopírování soubory ke stažení z úložiště (které jsou určeny pomocí [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] vlastnost) z adresáře pracovní StartTask ke sdílenému adresáři, k níž *všechny* úkoly na uzel přístup. V podstatě se tím zkopíruje `TaskApplication.exe` a závislými ke sdílenému adresáři v jednotlivých uzlech jako uzel spojí fondu, aby všechny úkoly, které běží na uzel k němu přístup.

> [AZURE.TIP] Funkce **balíčků aplikací** Azure dávku poskytuje další způsob, jak bude aplikace do výpočetního uzlů v fond. Další informace najdete v článku [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md) .

Také výrazné v fragment kódu je použití dvě proměnné ve vlastnosti *Příkazový řádek* StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` a `%AZ_BATCH_NODE_SHARED_DIR%`. Jednotlivých uzlech výpočetním fond dávku automaticky obsahuje několik proměnné, které jsou specifické pro dávku. Procesu prováděný úkol má přístup k tyto proměnné.

> [AZURE.TIP] Další informace o prostředí proměnných, které jsou k dispozici ve výpočetním uzlech dávku fondu a informace o úkolu pracovních adresářích, najdete v článku [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) a [soubory a adresáře](batch-api-basics.md#files-and-directories) částech [dávku přehled funkcí pro vývojáře](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Krok 4: Vytvoření dávku

![Vytvoření dávku][4]<br/>

Dávkové **úlohy** je sada úkoly a je přidružená k fond výpočetním uzlů. Na přidružené fondu výpočetním uzly provést úkolů v projektu.

Můžete použít úlohy nejen pro organizaci a sledování úkolů v souvisejících úloh, ale také při uložení určitá omezení – například maximální runtime pro danou úlohu (a tím pádem svých úkolů) i priorita úloh vzhledem k jiné úlohy v okně dávku účet. V tomto příkladu ale projektu je přidružený pouze fondu, který byl vytvořený v kroku #3. Žádná další vlastnosti jsou nakonfigurované.

Všechny úlohy dávku souvisí s fond konkrétních. Toto přiřazení označuje, které uzlů úkolů projektu se spustí na. Můžete zadat to kritérií [CloudJob.PoolInformation] [ net_job_poolinfo] vlastnost, jak je znázorněno fragment kódu.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Teď, když byl vytvořen projektu, úkoly přidají k provedení práce.

## <a name="step-5-add-tasks-to-job"></a>Krok 5: Přidání úkolů do projektu

![Přidání úkolů do projektu][5]<br/>
*(1) úkoly se přidají do projektu, (2) naplánováno spustit uzlech a (3) úkoly stahovat soubory dat pro zpracování*

Dávku **úkolům** jsou požadované jednotlivé jednotky práce na uzly výpočetním provést. Úkol má příkazového řádku a spustit skripty nebo programů jiného, které zadáte do tohoto příkazového řádku.

Provádět skutečně práce, musí být přidán úkolů do projektu. Každý [CloudTask] [ net_task] nakonfigurovaný pomocí příkazového řádku vlastností a [ResourceFiles] [ net_task_resourcefiles] (stejně jako u do fondu StartTask), která úkol stáhne na uzel před provedením příkazového řádku automaticky. V aplikaci project *DotNetTutorial* vzorku zpracuje každý úkol pouze jeden soubor. Proto svou kolekci ResourceFiles obsahuje jeden prvek.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Při přístupu k proměnné jako `%AZ_BATCH_NODE_SHARED_DIR%` nebo spuštění aplikace nebyl nalezen na uzel `PATH`, musí být předponou úkolu příkazového řádku `cmd /c`. Bude explicitně spustit video interpreter příkaz a požádejte ho ukončit po provedení příkazu. Tento požadavek je nutný úkolů spustit aplikaci na uzel `PATH` (například *robocopy.exe* nebo *powershell.exe*) a jsou použity žádné proměnné.

V `foreach` smyčka v fragment kódu, uvidíte, že příkazového řádku pro daný úkol je vytvořen tak, aby se předávají se tři argumenty příkazového řádku pro *TaskApplication.exe*:

1. **První argument** je cestu k souboru pro zpracování. Toto je místní cesta k souboru uložených na uzel. Když ResourceFile objektu v `UploadFileToContainerAsync` nově vytvořený nad názvu souboru používal pro tuto vlastnost (jako parametr konstruktoru ResourceFile). Tento údaj označuje, že soubor můžete najít v adresáři stejný jako *TaskApplication.exe*.

2. **Druhý argument** Určuje, že horních *N* slova by se měly zapisovat do výstupní soubor. Ve vzorku to je pevně tak, aby nejvyšší tři slova jsou aby došlo k zápisu výstupní soubor.

3. **Třetí argument** je podpis sdílený přístup (přidružení zabezpečení), které poskytuje přístup pro zápis do kontejneru **výstup** v úložišti Azure. *TaskApplication.exe* používá tato adresa URL podpis sdílený přístup při odesílání ve výstupním souboru k základnímu úložišti Azure. Vyhledání kódu pro tuto `UploadFileToContainer` metoda v aplikaci project TaskApplication `Program.cs` souboru:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Sledování úkolů

![Sledování úkolů][6]<br/>
*Klientská aplikace (1) sleduje úkoly k dokončení a stav úspěšné a (2) úkoly nahrajte Výsledná data k základnímu úložišti Azure*

Po přidání úkolů do projektu jsou automaticky ve frontě a naplánovat provedení uzlech výpočetním v rámci fondu přidružený k projektu. Na základě nastavení, které zadáte, dávku úchyty pro řízení fronty všech úkolů, plánování, opakování a další poplatky správy úkolů za vás. Sledování úkolů provádění mnoha způsoby. DotNetTutorial zobrazuje jednoduchý příklad sestav jenom na dokončení a úkolů nebo úspěchu státy.

V rámci `MonitorTasks` metoda v prvku DotNetTutorial `Program.cs`, existují tři dávku .NET koncepty, které vyžadují diskuse. Níže jsou uvedeny v sestupném pořadí podle vzhled:

1. **ODATADetailLevel**: zadání [ODATADetailLevel] [ net_odatadetaillevel] v seznamu je nezbytné k zajištění výkonu aplikace dávku operace (například získání seznam úkolů projektu). Přidání [dotazu služby Azure dávku efektivní](batch-efficient-list-queries.md) do seznamu čtení Pokud nebudete chtít udělat druh stav sledování aplikací dávku.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] poskytuje dávku .NET aplikací s nástroji pro Pomocníka pro sledování úkolů v USA. V `MonitorTasks`, *DotNetTutorial* čeká všechny úkoly k dosažení [TaskState.Completed] [ net_taskstate] v rámci časový limit. Pak ukončí projektu.

3. **TerminateJobAsync**: ukončení úloh s [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (nebo blokování JobOperations.TerminateJob) této úlohy označí jako dokončený. Je nutné provést v případě, že dávka řešení používá [JobReleaseTask][net_jobreltask]. Toto je speciální typ úkolu, který je popsán v [úkolech přípravu a dokončení projektu](batch-job-prep-release.md).

`MonitorTasks` Podle *DotNetTutorial*společnosti `Program.cs` se zobrazí pod:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Krok 7: Stažení výstup úkolu

![Stáhněte si úkolu výstup z úložiště][7]<br/>

Teď dokončení úlohy výstup úkoly můžete stáhnout z Azure úložiště. Důvodem je, s volání `DownloadBlobsFromContainerAsync` v *DotNetTutorial*společnosti `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Volání `DownloadBlobsFromContainerAsync` *DotNetTutorial* aplikace určuje, že se mají stahovat soubory do svého `%TEMP%` složky. Neváhejte změnit toto umístění výstupu.

## <a name="step-8-delete-containers"></a>Krok 8: Odstranění kontejnery

Protože vám bude účtovaná za dat uložených v úložišti Azure, je vždy vhodné odebrat všechny objekty BLOB, které jsou už není potřebné pro svou dávku úloh. V prvku DotNetTutorial `Program.cs`, to lze provést pomocí tři volání metody helper `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Metoda sama pouze získá odkaz do kontejneru a potom zavolá [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Krok 9: Odstranění úlohy a fondu

V posledním kroku uživateli výzva k odstranění úlohy a fondu se vytvářely v aplikaci DotNetTutorial. I když nejste účtovaná za projektů a úkolů, *jsou* účtovaná za výpočetním uzlů. Proto doporučujeme přidělit uzly pouze v případě potřeby. Odstranění nepoužitý fondů může být součást vašeho procesu údržbu.

BatchClient [JobOperations] [ net_joboperations] a [PoolOperations] [ net_pooloperations] mít odpovídající odstranění metody, které se označují jako Pokud uživatel potvrdí odstranění:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Nezapomeňte, že vám bude účtovaná za pro využití prostředků – odstraníte nepoužívané fondů minimalizuje náklady. Také mějte na paměti, že odstraněním fond odstraníte všechny výpočetním uzlů v rámci tohoto fondu a, po které bude všechna data v jednotlivých uzlech obnovit po odstranění fondu.

## <a name="run-the-dotnettutorial-sample"></a>Spuštění *DotNetTutorial* výběru

Při spuštění aplikace budou výstup konzoly podobně jako tento. Během spuštění zaznamenáte pozastavit na `Awaiting task completion, timeout in 00:30:00...` při spuštění do fondu výpočetním uzlů. Použití [Azure portálu] [ azure_portal] sledování vašeho fondu, výpočet uzly, projektu a úkoly během a po spuštění. Použití [Azure portál] [ azure_portal] nebo [Průzkumníka úložišť Azure] [ storage_explorers] zobrazíte prostředků úložiště (kontejnery a objekty BLOB), které jsou vytvořené v aplikaci.

Typické čas spuštění je **přibližně 5 minut** při spuštění aplikace ve výchozím nastavení.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Další kroky

Neváhejte proveďte změny a *DotNetTutorial* *TaskApplication* experiment s jinou výpočetním scénáře. Například přidejte zpoždění spuštění *TaskApplication*, jako je třeba s [Thread.Sleep][net_thread_sleep]simulovat dlouho probíhajících úkoly a sledovat na portálu. Zkuste přidání více úkolů nebo úpravu počtu uzlů výpočetním. Přidání logiky zjišťovat a povolit použití existující fond čas spuštění rychlosti (*Tip*: Podívejte se na `ArticleHelpers.cs` v [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektu v [azure dávku vzorcích][github_samples]).

Teď, když znáte základní pracovní postup řešení dávku, je čas na dostanete k dalším funkcím službu dávku.

- Přečtěte si [Přehled funkcí dávka pro vývojáře](batch-api-basics.md), který doporučujeme pro všechny nové dávku uživatele.
- Zahájení na jiných dávku vývoj článcích ve **vývoj hloubkovou** [cesta výuky dávku][batch_learning_path].
- Podívejte se na různé provádění zpracování pracovního vytížení "horních N slova" pomocí dávku v [TopNWords] [ github_topnwords] vzorku.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Vytvoření kontejnery v úložišti Azure"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Odeslání aplikace úkolu a vstupní (data) souborů pro kontejnery"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Vytvoření dávky fondu"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Vytvoření dávku"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Přidání úkolů do projektu"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Stáhněte si úkolu výstup z úložiště"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Pracovní postup řešení dávku (úplné diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Přihlašovací údaje dávku portálu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Úložiště pověření v portálu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Pracovní postup řešení dávku (minimální diagram)"
