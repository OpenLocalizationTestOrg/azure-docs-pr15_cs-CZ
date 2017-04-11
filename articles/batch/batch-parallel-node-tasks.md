<properties
    pageTitle="Maximalizace dávku uzel pomocí paralelní úlohy | Microsoft Azure"
    description="Zvýšení efektivity a nižší náklady pomocí méně výpočetním uzly a zprovoznění souběžné úkolů v jednotlivých uzlech ve fondu dávku Azure"
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

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Jak dosáhnout maximální využití prostředků výpočetním Azure dávku s souběžné uzlů úkolů

Opětovným spuštěním více úkolů současně v jednotlivých uzlech výpočetním v Azure dávku fondu, může maximalizovat využití prostředků v menší počtu uzlů v fondu. U některých úloh můžete výsledkem kratší pracovních hodin a levnější.

Během některé scénáře využívat přidělíte všechny zdroje na uzel jeden úkol, několik situací využívat povolení více úkolů sdílení tyto materiály:

 - **Minimalizace přenos dat** , pokud jsou úkoly moct sdílet data. V tomto případě může výrazně omezit poplatky za data přenos zkopírováním sdílených dat na menší počtu uzlů a provádění úkolů současně v jednotlivých uzlech. Týká zejména pokud data, která chcete zkopírovat do jednotlivých uzlech musí přenášena mezi zeměpisné oblasti.

 - **Využití paměti Maximizing** při vyžadovat velké množství paměti, ale jenom obdobích krátké počet minut a potom v proměnné časy během provádění úkolů. Je možné použít méně, ale větší, výpočetní uzly s víc paměti efektivně zpracovat takové vrcholy pole. Tyto uzly byste měli více úkolů paralelně spuštěných v jednotlivých uzlech, ale každý úkol by využít na uzly bohaté paměť ve stejnou dobu.

 - **Zmírnění uzel číslo omezení** při komunikaci mezi uzly požaduje v rámci fondu. V současné době fondů nakonfigurován pro komunikaci mezi uzly se omezuje 50 výpočetním uzlů. Pokud každý uzel do fondu je moct spouštět úkolů současně, můžete větší počet úkolů současně spouštět.

 - **Replikace místního výpočet obrázku**, například při prvním přesunu prostředí výpočetním Azure. Pokud vaše aktuální místní řešení spustilo více úkolů na uzel výpočetním, můžete zvýšit maximální počet úkolů uzel jak lépe zrcadlově tuto konfiguraci.

## <a name="example-scenario"></a>Příklad scénáře

Jako příklad ke znázornění výhody spuštění paralelní úkolu Řekněme, že aplikace úkolu obsahuje požadavky procesoru a paměti tak, aby [Standardní\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) uzly jsou dostatečné. Ale k dokončení projektu v požadovaném čase, je třeba 1 000 tyto uzly.

Místo použití standardní\_D1 uzly 1 core procesoru, můžete použít [Standardní\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) uzlů, které mají 16 jádra a povolte spuštění paralelní úkolu. Proto lze použít *16 časy méně uzly* – místo 1 000 uzly, pouze 63 by bylo potřeba. Kromě toho pokud aplikace velké soubory nebo referenčních dat jsou potřeba pro jednotlivých uzlech, pracovní doba trvání efektivity znovu lepší a od data zkopírována do pouze 16 uzlů.

## <a name="enable-parallel-task-execution"></a>Povolit spuštění paralelní úkolu

Konfigurace výpočetním uzly pro spuštění paralelní úkolu na úrovni fondu. S knihovnou dávku .NET nastavit [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost při vytvoření fondu. Pokud používáte rozhraní REST API dávku, nastavte [maxTasksPerNode] [ rest_addpool] prvek v hlavním textu žádosti o při vytváření fondu.

Azure dávku umožňuje nastavit maximální počet uzlů úkolů až čtyřikrát (4 x) počet uzel jádra. Například pokud fondu nakonfigurovaný s uzly velikost "Velké" (čtyři jádra), potom `maxTasksPerNode` možná nastavil 16. Podrobnosti počtu jádra pro jednotlivá pole velikostí uzel najdete v tématu [velikosti Cloudovým službám](../cloud-services/cloud-services-sizes-specs.md). Další informace o limitech služby najdete v tématu [kvót a limity pro službu Azure dávku](batch-quota-limit.md).

> [AZURE.TIP] Je potřeba vzít v úvahu `maxTasksPerNode` hodnota při vytváření [vzorce automatické měřítko] [ enable_autoscaling] fondu. Například vzorec, který bude vyhodnocen `$RunningTasks` může výrazně ovlivněna zvýšení počet uzlů úkolů. Další informace najdete v článku [automaticky měřítko výpočet uzlů v fondu dávku Azure](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Rozdělení úkolů

Když výpočetním uzlů v fond můžete provádět úkoly současně, je důležité můžete určit, jak chcete úkoly, které mají být rozvržena uzlů v fondu.

Pomocí [CloudPool.TaskSchedulingPolicy] [ task_schedule] vlastnictví, můžete určit, že úkoly přiřazeny rovnoměrně na všech uzlech ve fondu ("rozšíření"). Nebo můžete určit, co nejvíce úkolů by měl být přiřazen jednotlivých uzlech před úkoly přiřazovány jiné uzel fondu ("balení").

Jako příklad toho, jak tuto funkci je důležité zvážit fondu [Standardní\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) uzly (v předchozím příkladu), u kterých je nakonfigurována [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] přínosu 16. Pokud [CloudPool.TaskSchedulingPolicy] [ task_schedule] nakonfigurována [ComputeNodeFillType] [ fill_type] *Pack*by maximalizovat použití všechny 16 jádra jednotlivých uzlech a povolit [fondu neobsahovaly text](batch-automatic-scaling.md) do vyřadit nepoužitý uzly z fondu (uzly bez přiřazeny žádné úkoly). Slouží k minimalizaci využití prostředků a ukládá peníze.

## <a name="batch-net-example"></a>Příklad .NET dávku

Tento [Dávku .NET] [ api_net] fragment kódu rozhraní API zobrazuje žádost o vytvořit skupinu, která obsahuje čtyři velké uzly maximální počet uzel čtyři úkolů. Určuje plánování zásad, který naplní jednotlivých uzlech s úkoly před přiřazení úkolů jiného uzel fondu úkolů. Další informace o přidávání fondů pomocí rozhraní API .NET dávku najdete v tématu [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Příklad ZBÝVAJÍCÍ dávku

[ZBÝVAJÍCÍ dávku] [ api_rest] rozhraní API úryvek ukazuje žádost o vytvořit skupinu, která obsahuje dva velké uzly maximální počet uzel čtyři úkolů. Další informace o přidávání fondů pomocí rozhraní REST API, přečtěte si článek [Přidání fond s klientem][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Můžete nastavit `maxTasksPerNode` prvek a [MaxTasksPerComputeNode] [ maxtasks_net] vlastnost pouze na údaje o času vytvoření fondu. Nelze změnit po fond již existuje.

## <a name="code-sample"></a>Ukázka kódu

[ParallelNodeTasks] [ parallel_tasks_sample] projektu na GitHub ukazuje použití [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost.

Tato aplikace konzoly C# používá [Dávku .NET] [ api_net] knihovny vytvoření fondu s jeden nebo více výpočetním uzlů. Spustí, která dokáže nahradit počet úkolů v těchto uzlech, aby napodobily proměnné načíst. Výstup z aplikace určuje, které uzly spouštět každý úkol. Aplikace také poskytuje přehled parametrů úlohy a doby trvání. Souhrnné část výstup dva různé výskytů ukázková aplikace se zobrazí pod.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

První spuštění aplikace ukazuje, že s načítání fondu a ve výchozím nastavení jeden úkol pro každého uzel že projektu trvá delší než 30 minut.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Druhý spuštění zobrazuje ukázka významný pokles pracovní doby trvání. Je to proto fondu nakonfigurovanou se čtyřmi úkoly na uzel, který umožňuje spuštění paralelní úkolů k dokončení projektu téměř čtvrtletí od času.

> [AZURE.NOTE] Doby trvání úlohy v souhrnech výše nezahrnujte fondu údaje o času vytvoření. Každá z výše uvedených úloh byla odeslána dříve vytvořené fondů jehož výpočetním uzly byly do stavu *nečinnosti* na čas odeslání.

## <a name="next-steps"></a>Další kroky

### <a name="batch-explorer-heat-map"></a>Dávkové Explorer tepelné mapy

[Průzkumník dávku Azure][batch_explorer], jedna z Azure dávku [ukázkové aplikace][github_samples], obsahuje *Tepelné mapy* funkci, která obsahuje vizualizace provádění úkolů. Když máte provádění [ParallelTasks] [ parallel_tasks_sample] ukázková aplikace, můžete použít funkci tepelné mapy snadno vizualizovat plnění paralelní úkolů v jednotlivých uzlech.

![Dávkové Explorer tepelné mapy][1]

*Dávkové Explorer tepelná Mapa znázorňující fond čtyři uzlů s jednotlivých uzlech zrovna provádí čtyři úkoly*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
