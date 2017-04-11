<properties
    pageTitle="Efektivní seznamu dotazů v Azure dávku | Microsoft Azure"
    description="Zvýšení výkonu pomocí filtrování svoje dotazy týkající se dávku zdrojů, jako jsou skupiny, projekty, úkoly a výpočet uzlů."
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
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>Efektivní dotazu služby Azure dávku

Tady se dozvíte, jak zvýšit výkon aplikace Azure dávku omezením množství dat, který vám vrátil službu při dotazu projekty, úkoly a výpočet uzly s [Dávku .NET] [ api_net] knihovny.

Skoro všech aplikací dávku potřeba provést několik typ sledování nebo jiné operaci, která často dotazy službu dávku v pravidelných intervalech. Určit, zda jsou úlohy ve frontě zbývající v projektu, například musíte nejprve získat dat na každý úkol v projektu. Určit stav uzlech vašeho fondu, musíte nejprve získat data v každém uzlu fondu. Tento článek vysvětluje, jak provádět tyto dotazy nejefektivnější způsobem.

## <a name="meet-the-detaillevel"></a>Seznámení DetailLevel

Ve výrobním dávku aplikace entity jako úlohy, úkoly a výpočetním uzly číslování ve tisíců. Při požadavku na informace o těchto zdrojích, potenciálně velkého množství dat musí "křížové lince" služby dávku do aplikace na obou dotazů. Omezením počtu položek a typ informací, je vrácená dotazem můžete zvýšit rychlost dotazech a proto výkon aplikace.

Tento [Dávku .NET] [ api_net] fragment kódu rozhraní API seznamy *každý* úkol, který je přidružená k projektu, spolu s *všechny* vlastností jednotlivých úkolů:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Provedením mnohem efektivnější seznamu dotazu, ale použitím "úroveň podrobností" do dotazu. To uděláte zadáním [ODATADetailLevel] [ odata] objekt, který chcete [JobOperations.ListTasks] [ net_list_tasks] metody. Tento fragment kódu vrátí jenom ID, příkazového řádku a vlastnosti informací uzel výpočetních dokončených úkolů:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

V tomto příkladu scénáři, pokud existuje spousta úkolů v projektu, výsledky z druhého dotazu obvykle vrátí největší rychleji než první. Další informace o používání ODATADetailLevel seznam položek pomocí rozhraní API .NET dávku je zahrnuta [pod](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Doporučujeme, abyste tento je *vždy* zadejte objektu ODATADetailLevel ke svým hovorům seznamu rozhraní API .NET ověřit maximální efektivity a výkon aplikace. Zadáním úrovní podrobností pomůžete Ztlumením doby odezvy služby dávku zvýšení využití sítě a minimalizovat spotřebu paměti v klientských aplikacích.

## <a name="filter-select-and-expand"></a>Filtrování, vyberte a rozbalte

[.NET dávku] [ api_net] a [PODRŽTE dávku] [ api_rest] rozhraní API umožňují snížit obou počet položek, které se vracejí v seznamu i množství informací poskytovaných je vrácena pro jednotlivá pole. Můžete udělat zadáním **filtru**, **Vyberte**a **rozbalte řetězce** při provádění dotazů seznamu.

### <a name="filter"></a>Filtr
Filtr řetězec je výraz, který snižuje počet položek, které se vracejí. Například seznam pouze pracovního úkolů v projektu nebo seznam pouze výpočetním uzly, které budete chtít spustit úkoly.

- Řetězec filtr se skládá z jednoho nebo víc výrazů s výraz, který obsahuje název vlastnosti, operátor a hodnotu. Vlastnosti, které lze zadat jsou specifické pro jednotlivé typy entit zadáte dotaz, jako jsou operátory, které jsou podporovány pro každou vlastnost.
- Kombinací několika výrazů pomocí logické operátory `and` a `or`.
- V tomto příkladu filtrovat seznamy řetězec jen spuštění "vykreslení" úkoly: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Vyberte
Vyberte řetězec omezuje hodnoty vlastností, které vracejí pro každou položku. Zadejte seznam jmen vlastnost a jenom ty nemovitostí s hodnotou se vracejí položek ve výsledcích dotazu.

- Vyberte řetězec se skládá z hodnoty oddělené seznam jmen vlastnost. Můžete zadat některé z vlastností pro, která se dotazujete typ entity.
- Tento příklad vyberte řetězce určuje, že pouze tři vlastnost návratové hodnoty pro každý úkol: `id, state, stateTransitionTime`.

### <a name="expand"></a>Rozbalte položku
Rozbalení řetězec snižuje počet volání rozhraní API, která jsou potřebná k získání určité informace. Při použití řetězci Rozbalit lze získat další informace o jednotlivých položek pomocí jednoho volání rozhraní API. Místo první získání seznamu entit požadující informace pro každou položku v seznamu použít řetězci rozbalit získá stejné jednoho rozhraní API hovor. Méně volání rozhraní API znamená lepší výkon.

- Podobně jako vyberte řetězec, ovládací prvky řetězec rozbaleno: jestli určitých dat je součástí seznamu výsledků dotazu.
- Rozbalení řetězec je podporována pouze použijete ve výpisu úlohy, plány projektu, úkolů a skupiny. V současné době podporuje pouze statistické informace.
- Až všechny vlastnosti jsou potřeba a není zadán žádný vyberte řetězec, Rozbalit řetězec, *musíte* použít k získání informací statistiky. Pokud řetězec select se používá k získání dílčí sadu vlastností, pak `stats` může být zadán v vyberte řetězce a řetězec rozbalit nemusí být zadán.
- V tomto příkladu rozbalte řetězec Určuje, že má být vrácena statistické informace pro každou položku v seznamu: `stats`.

> [AZURE.NOTE] Při vytváření jednotlivých typech řetězec tři dotazu (filtrování, vyberte a rozbalte), musíte se ujistit, že názvy vlastností a písmena odpovídat jejich protějšky prvku rozhraní REST API. Třeba při práci s.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) předmětu, je třeba určit **Stav** místo **státu**, i když je vlastnost .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Viz tabulky pod pro mapování vlastností mezi .NET a rozhraní REST API.

### <a name="rules-for-filter-select-and-expand-strings"></a>Pravidla pro filtr, vyberte a rozbalte řetězce

- Vlastnosti názvy ve filtru, vyberte a rozbalte řetězce by měla vypadat jako se přehrávají ve [ZBÝVAJÍCÍ dávku] [ api_rest] rozhraní API – i když používáte [Dávku .NET] [ api_net] nebo některé z dalších dávku SDK.
- Vlastnost názvy malá a velká písmena, ale nemovitostí s hodnotou jsou malá a velká písmena.
- Datum a čas řetězce může být jednu ze dvou formátů a musí začínat s `DateTime`.

  - W3C – DTF formát příklad:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Příklad RFC 1123 formátu:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Logická řetězce si můžete prohlédnout `true` nebo `false`.
- Pokud není zadán neplatný vlastnost nebo operátor `400 (Bad Request)` bude výsledkem chyba.

## <a name="efficient-querying-in-batch-net"></a>Efektivní dotazování v dávku .NET

V rámci [Dávku .NET] [ api_net] rozhraní API, [ODATADetailLevel] [ odata] třídy se používá k poskytování filtr, vyberte a rozbalte řetězce seznam způsobů. Třídy ODataDetailLevel má tři řetězec veřejného vlastnosti, které můžete podle konstruktoru nebo nastavit přímo na požadovaném objektu. Potom předáte ODataDetailLevel objekt jako parametr různých operacích seznamu například [ListPools][net_list_pools], [ListJobs][net_list_jobs]a [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Omezení počtu položek, které se vracejí.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Určení hodnot vlastností, které jsou vráceny všechny požadované položky.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Získání dat u všech položek v jedné rozhraní API volání místo samostatných volání pro každou položku.

Následující fragment kódu používá rozhraní API dávku .NET pro efektivní dotazování službu dávka statistik určité skupiny fondů. V tomto scénáři má uživatel dávku testovacím i pracovním fondů. Test fondu ID jsou předponou "test" a fondu výrobní ID jsou předponou "výrobní". Úryvek je *myBatchClient* správně spuštění instancí třídy [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Instanci [ODATADetailLevel] [ odata] , který je nakonfigurovaný s vyberte a rozbalit klauzule lze také předat danou Get metody, například [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), chcete-li omezit množství dat, která je vrácena.

## <a name="batch-rest-to-net-api-mappings"></a>Dávkové REST .NET API mapování

Vlastnost názvy ve filtru, vyberte a rozbalte řetězce *musí* obsahovat jejich protějšky rozhraní REST API, jak v názvu a velikost písmen. V následujících tabulkách mapování mezi protějšky .NET a rozhraní REST API.

### <a name="mappings-for-filter-strings"></a>Mapování řetězcích filtru

- **.NET seznam metod**: všech metody .NET rozhraní API v tomto sloupci přijme [ODATADetailLevel] [ odata] objekt jako parametr.
- **ZBÝVAJÍCÍ seznamu požadavky**: každý rozhraní REST API stránka propojené v tomto sloupci obsahuje tabulku, která určuje vlastnosti a operacích, které jsou povoleny v řetězcích *filtru* . Při vytváření [ODATADetailLevel.FilterClause] budou používat tyto názvy vlastností a operace[ odata_filter] řetězec.

| Seznam metod .NET | ZBÝVAJÍCÍ seznam požadavků |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Seznam certifikátů na účtu][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Seznam souborů související s úkolem][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Seznam stav úlohy přípravu a vydání úlohy projektu][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Seznam úlohy na účtu][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Seznam souborů na uzel][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Seznam úkolů přidruženého úlohy][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Seznam plány projektu na účtu][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Seznam úlohy přidružený k plánu projektu][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Seznam výpočetním uzlů v fond][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Seznam skupin na účtu][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Mapování vyberte řetězců

- **Typy .NET dávku**: dávka .NET API typy.
- **Rozhraní REST API entity**: každou stránku v tomto sloupci obsahuje jednu nebo více tabulek, které seznam názvů vlastností rozhraní REST API pro typ. Tyto názvy vlastnost se používají při vytváření *Vyberte* řetězce. Při vytváření [ODATADetailLevel.SelectClause] použijete stejnými názvy vlastnost[ odata_select] řetězec.

| Typy .NET dávku | Entity rozhraní REST API |
|---|---|
| [Certifikát][net_cert] | [Získat informace o certifikátu][rest_get_cert] |
| [CloudJob][net_job] | [Získání informací o projektu][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Získání informací o plánu projektu][rest_get_schedule] |
| [ComputeNode][net_node] | [Získání informací o uzel][rest_get_node] |
| [CloudPool][net_pool] | [Získání informací o fond][rest_get_pool] |
| [CloudTask][net_task] | [Získat informace o úkolu][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Příklad: vytvoření řetězec filtru

Když vytvoříte řetězec filtru pro [ODATADetailLevel.FilterClause][odata_filter], použijte předchozí tabulce v části "Mapování pro filtr řetězce" najít rozhraní REST API si přečtěte následující dokumentaci stránku, která odpovídá seznamu operaci, kterou chcete provést. Filtrovat vlastností a jejich podporované operátorů najdete v tabulce první multirow na této stránce. Pokud chcete získat všechny úkoly jehož ukončovacího byl nenulová, například konkrétní řádek v [seznamu úkolů přidružené úlohy] [ rest_list_tasks] Určuje řetězec jde vlastnosti a povolenou operátorů:

| Vlastnost | Povolené operace | Typ |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Proto řetězec filtr pro seznam všech úkolů s nenulová ukončovacího budou:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Příklad: sestavit výběrový řetězec

Vytvářet [ODATADetailLevel.SelectClause][odata_select], naleznete v předchozí tabulce v části "Mapování vyberte řetězců" a přejděte na stránku rozhraní REST API, která odpovídá typu osoba, která se seznamem. Výběr vlastností a jejich podporované operátorů najdete v tabulce první multirow na této stránce. Pokud chcete získat ID a přepínač příkazového řádku pro každý úkol v seznamu, například zjistíte tyto řádky v tabulce použít na [získání informací o úkolu][rest_get_task]:

| Vlastnost | Typ | Poznámky |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Vyberte řetězec pro včetně ID a přepínač příkazového řádku do každého seznamu úkolu bude:

`id, commandLine`

## <a name="code-samples"></a>Ukázky

### <a name="efficient-list-queries-code-sample"></a>Ukázka kódu efektivně seznamu dotazů

Podívejte se na [EfficientListQueries] [ efficient_query_sample] ukázkový projekt na GitHub najdete v článku jak efektivně dotaz na seznam může ovlivnit výkon v aplikaci. Tato aplikace konzoly C# vytvoří a přidá velký počet úkolů do projektu. Potom ho volá více [JobOperations.ListTasks] [ net_list_tasks] metoda a průchodů [ODATADetailLevel] [ odata] objekty, které budou nakonfigurována různých nemovitostí s hodnotou měnit množství dat, která má být vrácena. Vytvoří výstup následujícím způsobem:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Podle uplynulý čas, můžete značně snížit doby odezvy dotaz omezením vlastností a počet položek, které se vracejí. Můžete najít to a Další ukázkové projekty v [azure dávku vzorky] [ github_samples] úložiště na GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>Ukázková knihovna a kód BatchMetrics

Kromě EfficientListQueries kód výše uvedené, můžete najít [BatchMetrics] [ batch_metrics] projektu v [azure dávku vzorky] [ github_samples] GitHub úložiště. Ukázkový projekt BatchMetrics ukazuje, jak efektivní sledování průběhu projektu dávku Azure pomocí rozhraní API dávku.

[BatchMetrics] [ batch_metrics] vzorek obsahuje .NET třídy knihovny projektu, který lze zahrnout do vašich projektů a jednoduchý příkazového řádku program výkon a ukazují použití knihovny.

Ukázková aplikace v rámci projektu znázorňuje následující operace:

1. Výběr určitých atributy chcete-li stáhnout pouze vlastnosti, které budete potřebovat
2. Filtrování doby přechodu stavu chcete-li stáhnout jenom změn od posledního dotazu

Podle pokynů se zobrazí v knihovně BatchMetrics. Vrátí ODATADetailLevel, který určuje, že jenom `id` a `state` entit, které jsou dotazovaném by měla získat vlastnosti. Také určuje, že pouze entity stavem změnil od zadaného `DateTime` parametr má být vrácena.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Další kroky

### <a name="parallel-node-tasks"></a>Paralelní uzlů úkolů

[Maximalizace dávku Azure výpočet používání zdrojů k úkolům souběžné uzel](batch-parallel-node-tasks.md) je taky Tento článek týkající se výkonu aplikace dávku. Některé typy úloh využívat provádění paralelní úlohy na větší – ale méně – výpočet uzlů. Podívejte se na [Příklad scénář](batch-parallel-node-tasks.md#example-scenario) v článku podrobné informace o těchto situace.

### <a name="batch-forum"></a>Fórum komunity dávku

[Fórum komunity dávku Azure] [ forum] na webu MSDN je ideální místo k diskutovat o dávka se a položte otázky týkající se služby. Hlavy na přes pro užitečné "rychlé" příspěvky a pošlete svoje otázky, kterým dochází při vytváření dávku řešení.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
