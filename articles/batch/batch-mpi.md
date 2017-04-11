<properties
    pageTitle="Spuštění aplikace MPI v Azure dávka s více instancí úkoly | Microsoft Azure"
    description="Zjistěte, jak provádět rozhraní předávání zpráv (MPI) aplikací pomocí více instanci typu úkolu v Azure dávku."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Použití více instancí úkoly pro spuštění aplikace rozhraní předávání zpráv (MPI) v Azure dávku

Více instancí úkoly aby bylo možné spouštět Azure dávkový úkol na více uzlů výpočetním současně. Tyto úkoly povolit výkonných výpočetních scénáře jako rozhraní předávání zpráv (MPI) aplikací v listu. V tomto článku se naučíte, jak provést více instancí úkolů pomocí [Dávku .NET] [ api_net] knihovny.

>[AZURE.NOTE] Příklady v tomto článku dávku .NET MS-MPI, a potom Windows výpočet uzlech, koncepty úkolu více instancí zde popsané jsou pro jiné platformy a technologií pro server (Python a Intel MPI uzlech Linux, například).

## <a name="multi-instance-task-overview"></a>Přehled úkolů více instanci

V listu, je každý úkol obvykle uzlu jedné výpočetní – odeslání více úkolů do projektu, ale službu dávku plánuje každému úkolu pro spuštění na uzel. Však nakonfigurováním úkolu **Nastavení více instance**zjistit dávku místo vytvoření jeden primární úkol a několik dílčích úkolů, potom provedených ve více uzlech.

![Přehled úkolů více instanci][1]

Po odeslání úkol s více instancí nastavení úlohy dávku provádí několik kroků jedinečné k úkolům více instance:

1. Služba dávku vytváří jeden **primární** a několik **dílčích úkolů** na základě více instancí nastavení. Celkový počet úkolů (primární plus všechny dílčí úkoly) odpovídá počtu **instance** (uzly výpočetním) určíte v nastavení více instance.
1. List jednu uzlů výpočetním označí jako **hlavní**a naplánovaný primární provést v předloze. Naplánuje dílčí úkoly na zbytek uzly výpočetním přidělit úkol více instancí jeden dílčích úkolů na uzel provést.
1. Hlavní stránku předlohy a všechny dílčí úkoly Stáhnout **běžné zdrojů soubory** , které určíte v nastavení více instance.
1. Po běžné zdrojů souborů po stažení hlavní a dílčí úkoly provedení **příkazu koordinaci** určíte v nastavení více instance. Příkaz koordinaci se obvykle používá k přípravě uzly k provedení úkolu. Může se jednat o spuštění služby na pozadí (jako je [Microsoft MPI][msmpi_msdn]společnosti `smpd.exe`) a ověřit, že uzlech jsou připravené na zpracování zpráv mezi uzel.
1. Primární úkolu je prováděn **příkaz aplikace** hlavní uzel *Po* úspěšném dokončení příkazu koordinaci tak, že hlavní stránku předlohy a všechny dílčí úkoly. Příkaz aplikace je příkazového řádku samotný úkol s více instancemi a je provedena pouhým primární úkolu. V [MS-MPI][msmpi_msdn]-na základě řešení, toto je, kde můžete provést MPI s podporou aplikace pomocí `mpiexec.exe`.

> [AZURE.NOTE] Když se funkčně liší, "více instancí úkol" není typu jedinečné úkolu jako [StartTask] [ net_starttask] nebo [JobPreparationTask][net_jobprep]. Více instancí jde jednoduše standardní úkol dávku ([CloudTask] [ net_task] v dávku .NET) nakonfigurovali u více instancí nastavení. V tomto článku se označují to jako **více instancí úkolu**.

## <a name="requirements-for-multi-instance-tasks"></a>Požadavky pro více instancí úkoly

Více instancí úkoly vyžadují fond s **komunikaci mezi uzly povoleno**a **spuštění souběžné úkolu zakázané**. Pokud se při pokusu o spuštění více instancí úkolu do fondu s internodium komunikace zakázané nebo s *maxTasksPerNode* hodnotu větší než 1, bude nikdy úkol – zůstane donekonečna udržovat ve stavu "aktivní". Tento fragment kódu ukazuje vytvoření fondu, pomocí dávku .NET knihovny.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Více instancí úkoly navíc lze spustit *pouze* v uzlů v **fondů vytvořené po 14 2015 prosinec**.

### <a name="use-a-starttask-to-install-mpi"></a>Použít StartTask nainstalovat MPI

Ke spuštění MPI aplikací ve více instancí úkolu, musíte nejdřív nainstalovat výpočetním uzlů v fondu implementaci MPI ([MS-MPI nebo Intel MPI, například). Toto je včas použít [StartTask][net_starttask], který spustí pokaždé, když uzel spojí fond nebo se nerestartuje. Tento fragment kódu vytvoří StartTask určující instalační balíček MS-MPI jako [soubor prostředku][net_resourcefile]. Zahájení úkolu příkazového řádku se spustí po stažení souboru prostředku na uzel. Přepínač příkazového řádku v tomto případě provede instalace bezobslužného MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Vzdálené přímý přístup do paměti (RDMA)

Při výběr [podporou RDMA velikost](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) například A9 uzlech výpočetním ve fondu dávku aplikace MPI můžete využít jeho Azure výkonných maximum, minimum latence vzdálené přímé paměti aplikace access (RDMA) sítě.

Vyhledejte velikosti zadaný jako "RDMA může" v těchto článcích:

* **CloudServiceConfiguration** fondů

  * [Velikost Cloudovým službám](../cloud-services/cloud-services-sizes-specs.md) (Pouze Windows)

* **VirtualMachineConfiguration** fondů

  * [Velikost virtuálních počítačích v Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Velikost virtuálních počítačích v Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Umožní využít výhod RDMA na [Linux výpočet uzly](batch-linux-nodes.md)je nutné použít **Intel MPI** na uzlů. Další informace o CloudServiceConfiguration a VirtualMachineConfiguration fondů naleznete v části [fondu](batch-api-basics.md#Pool) přehled funkcí dávku.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Vytvoří úkol s více instancemi s dávku .NET

Teď můžeme jste nichž se uplatní fondu požadavky a instalační balíček MPI, vytvoření úkolu více instancí. V této fragment vytvoříme standardní [CloudTask][net_task], nastavte jeho [MultiInstanceSettings] [ net_multiinstance_prop] vlastnost. Jak jsme zmínili dříve, není úkol s více instanci typu různých úkolů, ale standardní úkolu dávku nakonfigurováno více instancí nastavení.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primární úkolů a dílčích úkolů

Při vytváření více instancí nastavení pro daný úkol zadáte počtu uzlů výpočetním, které jsou k provedení úkolu. Po odeslání úkolů projektu služby dávku vytvoří jeden **primární** úkol a dost **dílčí úkoly** , které dohromady podle počtu uzlů, které jste zadali.

Tyto úkoly přiřazené id celé číslo v rozsahu 0 *numberOfInstances* - 1. Úkol s id 0 je primární a všechny ostatní ID jsou dílčí úkoly. Pokud vytváříte následující nastavení více instancí pro daný úkol primární úkol má id 0 a dílčí úkoly byste měli ID 1 až 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Hlavní uzel

Po odeslání více instancí úkolu službu dávku jeden z uzlů výpočetním označí jako "předlohy" uzel a provést na uzel předlohy naplánovaný primární. Dílčí úkoly jsou naplánované na zbytek uzly přidělit úkol s více instancemi provést.

## <a name="coordination-command"></a>Příkaz koordinaci

**Koordinaci příkaz** spustit pomocí obou hlavní a dílčí úkoly.

Vyvolání příkazu koordinaci blokuje – dávku příkazu aplikace nespustí, dokud příkazu koordinaci vrátil úspěšně pro všechny dílčí úkoly. Příkaz koordinaci by měl proto spuštění služby na všechny požadované pozadí, ověřte, jestli jsou připravené k použití a zavřete. Tento příkaz koordinaci řešení pomocí MS-MPI verze 7 spustí služba SMPD na uzel, například pak ukončí:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Poznámka: použití `start` v tomto příkazu koordinaci. To je nutné, protože `smpd.exe` aplikace nevrátí ihned po spuštění. Bez použití [start] [ cmd_start] tento příkaz koordinaci nezobrazí jako výsledek hledání a proto by blokovat příkazu aplikace spuštění příkazu.

## <a name="application-command"></a>Příkaz aplikace

Jakmile primární úkolu a všechny dílčí úkoly hotové spuštění příkazu koordinaci, více instancí úkolu příkazového řádku spouštět tak, že primární úkolů *pouze*. Jsme to volání **aplikace příkaz** odlišit od příkazu koordinaci.

Pro MS-MPI aplikace použijte příkaz aplikace pro spuštění aplikace MPI s podporou s `mpiexec.exe`. Tady je příklad příkaz aplikace pro řešení pomocí MS-MPI verze 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Protože MS-MPI `mpiexec.exe` používá `CCP_NODES` proměnné podle výchozí (viz [proměnné](#environment-variables)) v příkladu aplikace příkazového řádku nad nezahrnuje ho.

## <a name="environment-variables"></a>Proměnné

Dávkové vytvoří několik [proměnné] [ msdn_env_var] specifické pro více instancí úkoly ve výpočetním uzlech přidělit úkol více instancí. Příkazové řádky koordinaci a aplikace můžete odkázat tyto proměnné jako skripty a programy, které budou spustit.

Následující proměnné vytvořené službou dávka pro použití více instancí úkolů:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Úplné informace o těchto a jiných dávku výpočetních uzel proměnné prostředí, včetně jejich obsah a viditelnost najdete v článku [Výpočet uzel proměnné][msdn_env_var].

>[AZURE.TIP] Ukázka kódu dávku Linux MPI obsahuje příklady několika z těchto prostředí proměnných použití. [Koordinaci cmd] [ coord_cmd_example] flám skript běžných aplikací a soubory ke stažení vstupní soubory z Azure úložiště umožňuje sdílet systém souborů NFS na uzel předlohy a konfiguruje ostatní uzly přidělit úkol s více instancemi jako NFS klienty.

## <a name="resource-files"></a>Zdrojové soubory

Existují dvě sady souborů prostředků k zamyšlení více instancí úkolů: **běžné zdrojů soubory** , stáhněte si *všechny* úkoly (obě primární a dílčí úkoly) a zadané **souborů prostředků** pro více instancí úkolů sebe, který *jenom primární* úkol ke stažení.

V dialogovém okně Nastavení více instancí úkolu můžete zadat jeden nebo víc **souborů běžné zdrojů** . Stáhnou se tyto běžné zdrojů soubory z [Azure úložiště](./../storage/storage-introduction.md) v jednotlivých uzlech **sdíleném adresáři úkolu** primární a všechny dílčí úkoly. Dostanete sdíleném adresáři úkolů z aplikace a koordinaci příkaz řádků pomocí `AZ_BATCH_TASK_SHARED_DIR` proměnné. `AZ_BATCH_TASK_SHARED_DIR` Cesta je stejné pro všechny uzly přidělit úkol s více instancemi, tedy můžete sdílet jednu koordinaci příkaz mezi hlavní stránku předlohy a všechny dílčí úkoly. Dávkové "Nesdílet" v adresáři znamená vzdáleného přístupu, ale můžete ho použít jako připojené nebo sdílet čárky jak jsme zmínili dříve v tipu v proměnné.

Stahování souborů prostředků, zadané pro samotný úkol více instancí úkolu pracovního adresáře `AZ_BATCH_TASK_WORKING_DIR`, ve výchozím nastavení. Jak je uvedeno, na rozdíl od běžné zdrojů soubory jenom primární úkolu je možné stáhnout zdroje soubory určené pro samotný úkol více instancí.

> [AZURE.IMPORTANT] Vždy používat proměnné prostředí `AZ_BATCH_TASK_SHARED_DIR` a `AZ_BATCH_TASK_WORKING_DIR` odkázat na tyto adresáře do příkazového řádku. Odeslání není k vytvoření cesty ručně.

## <a name="task-lifetime"></a>Životnost úkolu

Životnost ovládacích prvků primární úkolu dobu trvání úkolu celou více instanci. Při ukončení hlavní, všechny dílčí úkoly ukončen. Ukončovacího primární je ukončovacího úkolu a proto slouží k určení úspěšně nebo neúspěšně úkolu pro účely opakovat.

Pokud některý z dílčích úkolů, ukončení s kódem zpáteční nenulového například celý více instancí úkolu se nezdaří. Více instancí úkolu je pak ukončen a opakované až limit opakovat.

Při odstranění úkolu více instancí službou dávku odstraní taky hlavní stránku předlohy a všechny dílčí úkoly. Všechny dílčích úkolů adresářů a svoje soubory se odstraní z výpočetním uzlů, stejně jako u standardní úkolu.

[TaskConstraints] [ net_taskconstraints] pro daný úkol více instance, například [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock]a [RetentionTime] [ net_taskconstraint_retention] vlastnosti, se uplatní, jako jsou standardní úkolu a platí pro primární a všechny dílčí úkoly. Ale pokud změníte [RetentionTime] [ net_taskconstraint_retention] vlastnost po přidání více instancí úkolu do projektu, tato změna se použije jen u primární úkolu. Všechny dílčí úkoly dál používat původní [RetentionTime][net_taskconstraint_retention].

Seznam posledních úkolu výpočetní uzly odráží id dílčího úkolu v případě poslední úkol byl součástí více instancí úkolu.

## <a name="obtain-information-about-subtasks"></a>Získat informace o dílčích úkolů

Získat informace o dílčí úkoly pomocí knihovnu dávku .NET volání [CloudTask.ListSubtasks] [ net_task_listsubtasks] metody. Tento způsob vrátí informace o všech dílčích úkolů a informace o výpočetní uzly, spouštět úkoly. Z těchto informací můžete zjistit každý dílčí kořenového adresáře, id fondu, jeho aktuálním stavu, kód ukončení a další. Tyto informace lze použít v kombinaci s [PoolOperations.GetNodeFile] [ poolops_getnodefile] způsob, jak získat soubory dílčích úkolů. Poznámka: Tato metoda nevrátí informace o primární úkolu (id 0).

> [AZURE.NOTE] Není uvedeno jinak, metody dávku .NET pracovat na více instancí [CloudTask] [ net_task] samotné vyrovnat *jenom* primární úkolu. Třeba když voláte [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metoda na úkol s více instancemi vrátí jenom soubory primární úloh.

Následující fragment kódu ukazuje, jak získat informace o dílčích úkolů, stejně jako požádat uzly, na kterých budou provedeny obsah souboru.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Ukázka kódu

[MultiInstanceTasks] [ github_mpi] ukázka kódu na GitHub demonstruje použití více instancí úkolu ke spuštění [MS-MPI] [ msmpi_msdn] aplikace v listu výpočet uzlů. Postupujte podle kroků v tématu [Příprava](#preparation) a [spuštění](#execution) spustit vzorku.

### <a name="preparation"></a>Příprava

1. Podle prvních dvou kroků v tématu [jak kompilaci a spustit jednoduchý program MS-MPI][msmpi_howto]. To vyhovuje prerequesites podle následujících pokynů.
1. Vytvořit *verzi systému [MPIHelloWorld] * [ helloworld_proj] ukázkový MPI program. Toto je program, který spustí uzlech výpočetních více instancí úkolu.
1. Vytvoření souboru zip obsahujícího `MPIHelloWorld.exe` (který jste integrované kroku 2) a `MSMpiSetup.exe` (který jste stáhli kroku 1). Tento soubor zip nahrajete jako balíčku aplikace v dalším kroku.
1. Použití [Azure portál] [ portal] vytvořte dávku [aplikace](batch-application-packages.md) s názvem "MPIHelloWorld" a určit zip soubor, který jste vytvořili v předchozím kroku jako verze "1.0" balíček aplikace. Další informace naleznete v tématu [nahrání a spravovat aplikace](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Vytvořit *verzi systému* `MPIHelloWorld.exe` tak, že nemusíte zahrnout všechny další závislosti (například `msvcp140d.dll` nebo `vcruntime140d.dll`) balíčku aplikace.

### <a name="execution"></a>Spuštění

1. Stažení [azure dávku vzorky] [ github_samples_zip] z GitHub.
1. Otevřete MultiInstanceTasks **řešení** ve Visual Studiu 2015. `MultiInstanceTasks.sln` Řešení soubor uložený v:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Zadejte svoje list a úložiště přihlašovací údaje účtu v `AccountSettings.settings` **Microsoft.Azure.Batch.Samples.Common** projektu.
1. **Vytvoření a spuštění** řešení MultiInstanceTasks provést MPI přehrajte aplikace na výpočet uzlů v fond dávku.
1. *Volitelné*: použití [Azure portál] [ portal] nebo [Průzkumníka dávku] [ batch_explorer] zkoumat ukázkové fondu, projektu a úkolu ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") před odstranit zdroje.

>[AZURE.TIP] Můžete si stáhnout [Visual Studio komunity] [ visual_studio] zdarma, pokud nemáte aplikaci Visual Studio.

Výstup z `MultiInstanceTasks.exe` je podobná této:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Další kroky

- Blog Microsoft HPC & Azure dávku týmu popisuje [MPI podpory Linux v Azure listu][blog_mpi_linux]a obsahuje informace o použití [OpenFOAM] [ openfoam] s listem. Vyhledání Python ukázky [OpenFOAM příkladu na GitHub][github_mpi].

- Zjistěte, jak [vytvářet skupiny Linux výpočetním uzlech](batch-linux-nodes.md) pro použití v Azure dávku MPI řešení.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Přehled více instanci"
