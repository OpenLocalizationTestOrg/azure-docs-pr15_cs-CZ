<properties
    pageTitle="Závislosti mezi úkoly v Azure dávce | Microsoft Azure"
    description="Vytváření úkolů, které jsou závislé na úspěšném dokončení dalších úlohách pro zpracování MapReduce styl a podobně jako velký data pracovního vytížení v Azure listu."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Závislosti mezi úkoly v Azure listu

Funkce závislostí úkolů Azure listu je přesně hodí, pokud chcete zpracovat:

- Styl MapReduce úloh v cloudu.
- Úlohy, jejichž úkoly zpracování dat může být vyjádřena řízené acyklický graf (DAG).
- Libovolný jiný projekt, ve kterém podřízenými úkoly, závisí na výstup vnější úkoly.

Závislosti mezi úkoly dávku umožňují vytvářet úkoly, které je naplánováno spuštění ve výpočetním uzlech až po úspěšném dokončení jeden či více úkolů. Můžete třeba vytvořit projektu, který každý snímek 3D videa s samostatné, paralelní úkoly se vykreslují pomocí. Poslední úkol – "sloučení úkol" – sloučení vykreslená snímky do dokončení film až po všechny snímky úspěšně vykreslena.

Můžete vytvářet úkoly, které jsou závislé na jiných úkolech v relaci typu 1: 1 nebo n. Můžete taky vytvořit závislost oblast kde daný úkol závisí na úspěšném dokončení skupinu úkolů v určitém rozsahu ID úkolu. Můžete kombinovat tyto tři základní scénáře umožňující vytvořit relace n: n.

## <a name="task-dependencies-with-batch-net"></a>Závislosti mezi úkoly s dávku .NET

V tomto článku probereme tato jak nakonfigurovat závislostí pomocí [Dávku .NET] [ net_msdn] knihovny. Nejdřív ukážeme si, jak [Povolit závislosti mezi úkoly](#enable-task-dependencies) na projektech a potom ukazují, jak [nakonfigurovat se závislostmi úkolu](#create-dependent-tasks). Nakonec probereme tato témata [závislost scénáře](#dependency-scenarios) , který podporuje dávku.

## <a name="enable-task-dependencies"></a>Povolení závislosti mezi úkoly

Použít závislostí úkolů v aplikaci dávku, musíte nejdřív upozornit službu dávku používané závislostí úkolů projektu. V dávku .NET ji povolit na [CloudJob] [ net_cloudjob] nastavením jeho [UsesTaskDependencies] [ net_usestaskdependencies] vlastnost `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

V předchozím fragment kódu "batchClient" je instancí [BatchClient] [ net_batchclient] předmětu.

## <a name="create-dependent-tasks"></a>Vytvoření závislé úkoly

Pokud chcete vytvořit úkol, které jsou závislé na úspěšném dokončení jeden či více úkolů, sdělíte dávku, že úkol "závisí na" jinými úkoly. .NET dávku nakonfigurovat [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] vlastnost s instanci [TaskDependencies] [ net_taskdependencies] třídy:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Tento fragment kódu vytvoří úkol s ID květin"", které bude možné naplánovat dělat výpočetní uzly až po úspěšně dokončené úkoly s ID "Srážky" a "Ne".

 > [AZURE.NOTE] Úkol je považován za udělat, když je ve stavu **dokončení** a její **ukončení kód** `0`. V .NET dávku, znamená to, že [CloudTask][net_cloudtask]. [Stav] [net_taskstate] vlastnost přínosu `Completed` a CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] vlastnosti hodnota je `0`.

## <a name="dependency-scenarios"></a>Závislost typu scénáře

Existují tři scénáře základních úkolů závislost použité v Azure dávku: 1, 1 n a pole číslo ID úkolu v rozsahu závislost. Tyto můžete kombinovat poskytnout scénáři čtvrtý-n.

 Scénář&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Příklad | |
 :-------------------: | ------------------- | -------------------
 [1: 1](#one-to-one) | *taskB* závisí na *taskA* <p/> *taskB* nebude naplánované pro spuštění, dokud *taskA* byla úspěšně dokončena | ![Diagram: závislosti mezi úkoly typu 1: 1][1]
 [1 n](#one-to-many) | *taskC* závisí na *taskA* a *taskB* <p/> *taskC* nebude naplánované pro spuštění, dokud *taskA* a *taskB* mít byla úspěšně dokončena | ![Diagram: závislosti mezi úkoly na více][2]
 [Rozsah ID úkolu](#task-id-range) | *taskD* závisí na oblasti úkoly <p/> *taskD* nebude naplánované pro spuštění, dokud úspěšně dokončené úkoly s ID *1* až *10* | ![Diagram: Závislostí id oblasti][3]

>[AZURE.TIP] Vytvoříte **- n** relace, jako je místo, kam úkoly C, D, E a F každý, závisí na úkolech A a B. To je užitečné například v paralelizován předzpracování scénáře kde podřízenými úkoly, závisí na výstup více nadřazeného úkolů.

### <a name="one-to-one"></a>1: 1

Pokud chcete vytvořit úkol, který obsahuje závislosti na úspěšném dokončení jeden úkol, zadat ID jednoho úkolu se [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] statický metoda po naplnění [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>1 n

Pokud chcete vytvořit úkol, který obsahuje závislosti na úspěšném dokončení více úkolů, zadáte kolekce úkolu ID [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] statické metody po naplnění [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Rozsah ID úkolu

Pokud chcete vytvořit úkol, který obsahuje závislost na úspěšném dokončení okruhem úkoly jejichž ID spadají do oblasti, zadáte první a poslední úlohy ID v rozsahu [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] statické metody po naplnění [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Při použití rozsahů ID úkolu pro závislosti mezi úkoly, ID úkolu v oblasti *musí* být řetězcové vyjádření hodnot celé číslo. Kromě toho každý úkol v oblasti musí být úspěšně dokončena pro závislý úkol naplánovaný pro spuštění.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Ukázka kódu

[TaskDependencies] [ github_taskdependencies] ukázkový projekt je jednou z [Azure dávku ukázky] [ github_samples] na GitHub. Toto řešení Visual Studio 2015 ukazuje, jak povolit závislosti mezi úkoly v projektu, vytvořte úkoly, které jsou závislé na jiných úkolů a provedení těchto úkolů na fond výpočetním uzlů.

## <a name="next-steps"></a>Další kroky

### <a name="application-deployment"></a>Nasazení aplikace

Funkce [balíčků aplikací](batch-application-packages.md) dávku poskytuje snadný způsob, jak současně nasazení a verze aplikace, které úkolů na provést výpočet uzlů.

### <a name="installing-applications-and-staging-data"></a>Instalace aplikací a přípravu dat

Podívejte se na [instalace aplikace a pracovní data v listu výpočet uzly] [ forum_post] příspěvek ve fóru Azure dávku základní informace o různých metod Příprava uzly úlohy. Který napsal jednu členové týmu Azure dávku, tento příspěvek je dobré základy na různé způsoby, jak dostat soubory (včetně aplikací a úlohu vstupní data) do výpočetního uzlů.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: závislost typu 1: 1"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: závislost na více"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: závislostí id oblasti"
