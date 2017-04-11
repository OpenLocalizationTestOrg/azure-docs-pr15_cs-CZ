<properties
    pageTitle="Funkce přípravu a vyčištění v listu | Microsoft Azure"
    description="Použití úroveň úlohy Příprava úkoly Minimalizovat přenos dat Azure dávku výpočet uzly a uvolnit úkolů pro vyčištění uzel v okamžiku dokončení projektu."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Spuštění přípravu a dokončení úlohy na Azure dávku výpočet uzlů

 Dávku Azure často vyžaduje některé způsob instalace před jeho úkoly zpracují a po úlohy správy po dokončení úkolů. Možná budete muset stáhnout běžných úkolů zadávání dat do výpočetního uzlů nebo odešlete úkolu výstupní data k základnímu úložišti Azure, až se dokončí úloha. Úkoly **projektu přípravu** a **uvolněním projektu** můžete provádět tyto operace.

## <a name="what-are-job-preparation-and-release-tasks"></a>Co jsou Příprava úlohy a uvolněním úkoly?

Před spuštěním úkolů projektu přípravu úlohu spustí uzlech výpočetním naplánováno aspoň jeden úkol. Po dokončení projektu vydání úlohu běží ve všech uzlech fondu spouštět aspoň jeden úkol. Stejně jako normální dávkové úkoly, můžete určit příkazového řádku pro uplatnit při spuštění úlohy příprava nebo uvolnění úkolu.

Příprava a vydání úlohy nabízejí známé funkce úkolu dávku třeba stahování souboru ([souborů prostředků][net_job_prep_resourcefiles]), vyšší spuštění vlastní proměnné, doba trvání maximální spuštění, počet opakování a doba uchovávání informací soubor.

V následujících částech najdete informace o použití [JobPreparationTask] [ net_job_prep] a [JobReleaseTask] [ net_job_release] třídy nacházejí v [Dávku .NET] [ api_net] knihovny.

> [AZURE.TIP] Příprava a vydání úlohy jsou užitečné na "sdílí fond" prostředí, na která fond uzlů výpočetním potrvají mezi spuštění úlohy a používá mnoho úloh.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Kdy použít Příprava úlohy a uvolněním úkoly

Příprava projektu a vydání úlohy se přesně hodí následujících situacích:

**Stažení dat běžných úkolů**

Dávkové úlohy často vyžadují společnou sadu dat jako vstup pro úkoly v projektu. Denní výpočtů analýzy rizika market dat je například specifické pro úlohu, ještě společná pro všechny úkoly v projektu. Tato data market často několik gigabajty ve skupinovém rámečku velikost se mají stahovat do jednotlivých uzlech výpočetním jenom jednou tak, aby ji mohli použít jakýkoli úkol, který běží na uzel. **Příprava úlohu** použijte ke stažení tato data do jednotlivých uzlech před provádění úlohy je další úkoly.

**Odstranění výstupu projektu a úkolu**

V prostředí "sdílí fond", pokud nejsou do fondu výpočetním uzly odebraným mezi projekty, budete muset odstranit data projektu mezi se spustí. Možná budete muset ušetřit místo na disku v jednotlivých uzlech nebo vyhovují zásady zabezpečení vaší organizace. Umožňuje odstranit data, která byla stáhnout Příprava úlohu nebo generovaného při provádění úloh **vydání úlohu** .

**Uchovávání informací protokolu**

Můžete chtít ponechat kopii protokoly, které mohou generovat úkolů nebo možná selhat soubory výpisu generovaných selhalo aplikacemi. Umožňuje **vydání úlohu** v těchto případech komprimovat a nahrajte tato data do [Úložiště Azure] [ azure_storage] účtu.

>[AZURE.TIP] Další způsob, jak zachovat protokoly a jiné projektu a úkolu výstupní data, je použít knihovnu [Azure dávkový soubor konvence](batch-task-output.md) .

## <a name="job-preparation-task"></a>Příprava úlohu

Před provádění úkolů projektu spustí dávku Příprava úlohu v jednotlivých uzlech výpočetních, který je plánován spuštění úlohy. Ve výchozím nastavení služby dávku čeká Příprava úlohu dokončen před spuštěním úlohy naplánované na uzel provést. Však můžete nakonfigurovat službu tak, aby čekat. Pokud restartování uzel opětovném spuštění Příprava úlohu, ale můžete zakážete tím taky toto chování.

Příprava úlohu bude spuštěn pouze v uzlech, které je naplánováno spuštění úlohy. V případě, že uzel není přiřazen úkol nebude nepotřebných spuštění Příprava úkolu. Může dojít při počet úkolů v projektu je menší než počtu uzlů v fond. Platí také při [spuštění souběžné úkolu](batch-parallel-node-tasks.md) zapnuté, které opustí některé uzly nečinnosti, pokud je počet úkolů nižší než celkový počet možných souběžné úkolů. Spuštěním není Příprava úlohu nedělá uzlech, můžete méně peníze utratit poplatky za data přenos.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] se liší od [CloudPool.StartTask] [ pool_starttask] v JobPreparationTask spustí na začátku jednotlivé úlohy, že StartTask spustí pouze v případě, že výpočetní uzly nejdřív spojí fond nebo restartuje.

## <a name="job-release-task"></a>Uvolnění úlohu

Jakmile úlohy se označí jako dokončený, vydání úlohu spouštět v jednotlivých uzlech ve fondu spouštět aspoň jeden úkol. Úlohy se označit jako dokončený zadáním žádost o ukončení. Službu dávku potom nastaví stav úlohy na *ukončení*ukončí úlohy aktivní nebo pracovního přidružený k projektu a spustí vydání úlohu. Projektu přejde do stavu *dokončení* .

> [AZURE.NOTE] Odstranění úlohy se spustí také vydání úlohu. Pokud ale úlohy už byl ukončen, úkol uvedení není spustit podruhé úlohu novější odstranili.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Funkce vývojové a uvolněním úkolů pomocí dávku .NET

Použití Příprava úlohu, přiřazení [JobPreparationTask] [ net_job_prep] objektu, který má vaše úloha [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] vlastnost. Podobně inicializace [JobReleaseTask] [ net_job_release] a přiřadit ji někomu vaše úloha [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] vlastnost nastavovaná vydání úlohu.

V tomto fragment kódu `myBatchClient` instancí [BatchClient][net_batch_client], a `myPool` je existující fond v rámci dávku účtu.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Jak jsme zmínili dříve, uvolnění úkolu se provádí úlohu ukončit nebo odstranit. Ukončit úlohu s [JobOperations.TerminateJobAsync][net_job_terminate]. Odstranění úlohy s [JobOperations.DeleteJobAsync][net_job_delete]. Obvykle ukončit nebo odstranění úlohy po dokončení úkolů, případně dosáhl časový limit, který jste definovali.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Ukázka kódu na GitHub

Viz Příprava úlohy a uvolněním úkoly v akci, najdete v tématech [JobPrepRelease] [ job_prep_release_sample] ukázkový projekt na GitHub. Tato aplikace konzoly dělá toto:

1. Vytvoří fond s dvěma "small" uzlů.
2. Vytvoří úlohu s Příprava projektu, vydání a standardní úkoly.
3. Spustí Příprava úlohu, který nejdřív zapisuje ID uzlu k textovému souboru v adresáři "sdílené" na uzel.
4. Spustí úkolu v jednotlivých uzlech, který data zapisuje jeho pole číslo ID úkolu na stejné textový soubor.
5. Po dokončení všech úkolů (nebo dosažení časový limit), vytiskne obsah souboru text jednotlivých uzlech ke konzole.
6. Po dokončení projektu se spustí vydání úlohu soubor odstranit ze skupiny.
6. Vytiskne kódy ukončení úloh přípravu a verze projektu pro jednotlivé uzly dni spouštět.
7. Spuštění pozastaví umožňuje potvrzení odstranění úlohy a/nebo fond.

Výstup z ukázkové aplikace je podobná této:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Z důvodu proměnné vytvoření a spuštění dobu uzlů v nový fond (některé uzly připraveni úkolů před ostatními) můžete vidět různé výstupu. Konkrétně protože úkoly dokončit rychle, jednu do fondu uzlů může provedení všech úkolů projektu. V takovém případě zjistíte, že příprava úlohy a vydání úkoly není k dispozici pro uzel spouštět žádné úkoly.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Kontrola přípravu úlohy a vydání úkoly v portálu Azure

Při spuštění aplikace vzorku, můžete použít [Azure portál] [ portal] zobrazte vlastnosti projektu a jeho úkoly nebo dokonce sdílené text budou moct soubor stáhnout upraveném úkolů projektu.

Následující snímek obrazovky znázorňuje **Příprava úkoly zásuvné** Azure portálu po spuštění aplikace. Přejděte do vlastnosti *JobPrepReleaseSampleJob* poté, co jste dokončili úkolů (ale před odstraněním úlohy a fondu) a klikněte na **Příprava** nebo **vydání úkoly** zobrazit jejich vlastnosti.

![Příprava vlastnosti úlohy Azure portálu][1]

## <a name="next-steps"></a>Další kroky

### <a name="application-packages"></a>Balíčků aplikací

Kromě Příprava úlohu můžete také funkci [balíčků aplikací](batch-application-packages.md) listu Příprava výpočetním uzlech pro provádění úkolů. Tato funkce je užitečné především pro nasazení aplikace, které není nutné zadávat spuštění instalačního programu, aplikace, které obsahují větší množství souborů (100 +) nebo aplikace, které vyžadují ovládací prvek verze.

### <a name="installing-applications-and-staging-data"></a>Instalace aplikací a přípravu dat

Tento příspěvek ve fóru MSDN přehled několika způsoby uzly pro spuštění úkoly:

[Instalace aplikací a přípravu dat na listu vypočítat uzlů][forum_post]

Který napsal jednu členové týmu Azure dávku, popisuje několik technik, které můžete použít pro nasazení aplikace a data pro výpočet uzlů.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
