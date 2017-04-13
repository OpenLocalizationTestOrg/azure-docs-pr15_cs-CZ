<properties
   pageTitle="Testování akce | Microsoft Azure"
   description="Tento článek pojednává o testování akce součástí struktury služby Microsoft Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testování akce
Aby napodobily nedůvěryhodných infrastruktury, struktury služby Azure poskytuje, vývojář způsoby tak, aby napodobily různých reálný selhání a přechody stavu. Toto jsou vystavit jako testování akce. Rozhraní API nižší úrovně, který způsobí konkrétní pravděpodobnost vkládání, přechod stavu nebo ověření, že jsou tyto akce. Zkombinováním tyto akce můžete napsat komplexní test scénáře pro své služby.

Služba struktury poskytuje že některé běžné scénáře test skládající se ze tyto akce. Důrazně doporučujeme používat tyto předdefinované scénáře, které jsou pečlivě zvolené otestovat běžné přechody stavu a selhání případy. Však akce lze vytvořit vlastní test scénáře, když budete chtít přidat pokrytí pro scénáře, které se nevztahují předdefinované scénáře ještě nebo, které jsou vlastní přizpůsobené aplikace.

Implementace C# akcí se nacházejí v sestavení System.Fabric.dll. Modul systém struktury Powershellu najdete v sestavení Microsoft.ServiceFabric.Powershell.dll. Jako součást instalace runtime modul ServiceFabric Powershellu nainstalovaný povolit pro snadnější použití.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Bezproblémové porovnání vynuceném poruch akce
Testování akce dělí do dva hlavní bloky:

* Vynuceném chyby: tyto chyby simulovat selhání jako restartování počítače a zpracování chyb. V takovém případě chyby kontextu spuštění procesu náhlému ukončení. To znamená, že žádný vyčištění státu, mohlo by umožnit spuštění před spuštění aplikace znovu.

* Bezproblémové chyby: tyto chyby simulovat bezproblémové akcí, jako se přesune otevřené a kapky spouštěný kliknutím Vyrovnávání zatížení. V takovém případě službu získá oznámení o ukončení a můžete vyčistit stav před ukončením.

Pro lepší ověřování kvality organizovat pracovní zátěž service a pracovní přitom vyvolat různých bezproblémové a vynuceném chyby. Vynuceném chyby neuplatnění scénáře, které služby náhle ukončení procesu uprostřed některé pracovního postupu. Tato funkce testuje cesta pro obnovení ihned po otevřené služby podle struktury služby. To vám pomůže otestovat konzistence dat a jestli stavu služby správně udržovat po k chybám. Další sady testu selhání (bezproblémové selhání), který replikám přesunutí tak, že služba struktury správně reakce služby. To srovnává zpracování hodnot zrušení metodu RunAsync. Služba musí vyhledat token zrušení Probíhá nastavení, stavu správně uložit a zavřete metodu RunAsync.

## <a name="testability-actions-list"></a>Seznam akcí testování

| Akce | Popis | Spravované rozhraní API | Rutiny prostředí PowerShell | Bezproblémové/vynuceném chyb |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Odebere všechny stavu test z obrázku v případě chybná vypnutí ovladač test. | CleanTestStateAsync | Odebrat ServiceFabricTestState | Nejde použít |
| InvokeDataLoss | Indukuje ztrátou dat do oddílu služby. | InvokeDataLossAsync | Vyvolání ServiceFabricPartitionDataLoss | Bezproblémové |
| InvokeQuorumLoss | Vloží oddíl daná stavová služba do ztráty kvora. | InvokeQuorumLossAsync | Vyvolání ServiceFabricQuorumLoss | Bezproblémové |
| Přesunutí primární | Slouží k přesunutí zadaný primární otevřené stavová služba zadaný uzel clusteru. | MovePrimaryAsync | Přesunutí ServiceFabricPrimaryReplica | Bezproblémové |
| Přesunutí sekundární | Slouží k přesunutí aktuální sekundární otevřené stavová služba různých clusteru. | MoveSecondaryAsync | Přesunutí ServiceFabricSecondaryReplica | Bezproblémové |
| RemoveReplica | Napodobuje selhání otevřené odebráním otevřené clusteru. Tím se zavře otevřené a bude přechod k roli "Žádný", odeberete všechny stavu clusteru. | RemoveReplicaAsync | Odebrat ServiceFabricReplica | Bezproblémové |
| RestartDeployedCodePackage | Napodobuje selhání kód balíčku proces restartováním balíček kódu nasazený na uzel v clusteru. To přeruší proces balíčku kód, který bude restartujte uživatele služby ostatními hostované daného procesu. | RestartDeployedCodePackageAsync | Restartování ServiceFabricDeployedCodePackage | Vynuceném |
| RestartNode | Napodobuje selhání služby struktury clusteru uzlu restartováním uzel. | RestartNodeAsync | Restartování ServiceFabricNode | Vynuceném |
| RestartPartition | Napodobuje situace datacentra přerušení spojení nebo obrázku přerušení spojení restartováním některých nebo všech kopie oddíl. | RestartPartitionAsync | Restartování ServiceFabricPartition | Bezproblémové |
| RestartReplica | Napodobuje selhání otevřené restartováním trvalých otevřené v clusteru, zavření otevřené a potom ho znovu otevřete. | RestartReplicaAsync | Restartování ServiceFabricReplica | Bezproblémové |
| Počáteční_uzel | Spustí uzel obrázku, který je zastaven. | StartNodeAsync | Zahájení ServiceFabricNode | Nejde použít |
| StopNode | Napodobuje selhání uzlu tak, že zastavení uzlu clusteru. Uzel zůstanou dolů, dokud se nazývá Počáteční_uzel. | StopNodeAsync | Zastavit ServiceFabricNode | Vynuceném |
| ValidateApplication | Ověří dostupnost a stav všech služeb služby struktury aplikace, obvykle po vyvolat některé poruch do systému. | ValidateApplicationAsync | Test-ServiceFabricApplication | Nejde použít |
| ValidateService | Ověří dostupnost a stav služby služby struktury obvykle po vyvolat některé poruch do systému. | ValidateServiceAsync | Test-ServiceFabricService | Nejde použít |

## <a name="running-a-testability-action-using-powershell"></a>Spuštění akce testování pomocí prostředí PowerShell

Tento kurz se dozvíte, jak chcete spustit akci testování pomocí prostředí PowerShell. Se dozvíte, jak chcete spustit akci testování proti clusteru služby Azure nebo místní clusteru (jedno pole). Microsoft.Fabric.Powershell.dll – modulu služby struktury PowerShell – instaluje automaticky při instalaci MSI struktury služby Microsoft. Modul načte se automaticky při otevření příkazovém řádku prostředí PowerShell.

Výuková segmentů:

- [Spuštění akce proti clusteru jedno pole](#run-an-action-against-a-one-box-cluster)
- [Spustí akci na hledání v Azure obrázku](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Spuštění akce proti clusteru jedno pole

Pokud chcete spustit akci testování proti místní obrázku, první připojte se k němu a otevřete příkazovém řádku prostředí PowerShell v režimu správce. Dejte nám podívejte **Restart-ServiceFabricNode** akce.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Tady je spuštěn akce **Restart-ServiceFabricNode** na uzel s názvem "Uzel1". Ukončení režimu určuje, že neměli ověřit, zda akce restart uzel skutečně úspěšně. Ukončení režimu určuje, jak je "Ověření" způsobí, že můžete ověřit, jestli opravdu bylo úspěšné restart akce. Místo přímého určení uzel jeho název, můžete ji určit pomocí klíče oddíl a druh otevřené, následujícím způsobem:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Restartování ServiceFabricNode** bude použito restartovat službu struktury uzel na clusteru. Tím se zastaví Fabric.exe proces, který bude restartujte všechny systém služby a uživatele služby replice hostovaných na uzel. Pomocí této rozhraní API testování služby pomáhá objevovat chyby podél cesty obnovení překlopení. Je dobré simulovat selhání uzlů v clusteru.

Následující obrázek ukazuje příkazu testování **Restart-ServiceFabricNode** v akci.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Výstup první **Get-ServiceFabricNode** (rutina z modulu služby struktury Powershellu) zobrazuje místní cluster má pět uzly: Node.1 Node.5. Po provedení akce testování (rutina) **Restart-ServiceFabricNode** je na uzel s názvem Node.4, vidíme, že na uzel provozu resetování.

### <a name="run-an-action-against-an-azure-cluster"></a>Spustí akci na hledání v Azure obrázku

Spuštění akce testování (pomocí Powershellu) proti Azure clusteru je podobný spuštění akce proti místní obrázku. Jediný rozdíl je, že před spuštěním akce místo připojení k místní clusteru, musíte se nejdřív připojit k Azure obrázku.

## <a name="running-a-testability-action-using-c35"></a>Spuštění akce testování portálu C# 35;

Pokud chcete spustit akci testování pomocí C#, musíte se připojit k clusteru pomocí FabricClient. Získejte parametry potřebné ke spuštění akce. Různé parametry mohou sloužit k spuštění stejné akce.
Prohlížíte RestartServiceFabricNode akce, je podle pokynů uzel (uzel jméno a ID instance uzel) v clusteru jedním ze způsobů ho spusťte.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametr vysvětlivky:

- **CompleteMode** Určuje, že režim neměli ověřte, zda akce restart skutečně úspěšně. Ukončení režimu určuje, jak je "Ověření" způsobí, že můžete ověřit, jestli opravdu bylo úspěšné restart akce.  
- **OperationTimeout** nastaví dobu operace dokončit před TimeoutException výjimce.
- **CancellationToken** umožňuje hovoru na zjištění stavu úkolů na zrušit.

Místo přímého určení uzel jeho název, můžete ji určit pomocí klíče oddíl a druh otevřené.

Další informace najdete v tématu [PartitionSelector a ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector a ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector je pomocná zveřejněným v testování a slouží k výběru konkrétní oddíl, do kterého chcete provádět operace testování. Slouží k vybrat konkrétní oddíl je-li ID oddílu známo předem. Nebo můžete zadat klíč oddílu a operaci vyřeší interně ID oddílu. Máte taky možnost výběru náhodné oddíl.

Použít tento pomocník, vytvořte PartitionSelector objektu a vyberte oddílu jedním ze způsobů Select *. Rozhraní API, je to nutné ji předejte PartitionSelector objektu. Pokud je vybrána možnost bez, ve výchozím nastavení náhodné oddíl.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector pomocníka zveřejněným v testování a slouží k výběru otevřené podle něhož chcete provádět operace testování. Lze vybrat konkrétní otevřené je-li ID otevřené známo předem. Kromě toho máte možnost výběru primárního otevřené nebo náhodné sekundární. ReplicaSelector pochází z PartitionSelector, takže budete muset vybrat otevřené a oddíl, na kterém chcete provést operaci testování.

Použít tento pomocník, vytvořte objekt ReplicaSelector a nastavte tak, jak chcete vybrat otevřené a oddílu. Můžete pak jí do rozhraní API, který vyžaduje. Pokud je vybrána možnost bez, ve výchozím nastavení náhodné otevřené a náhodné oddíl.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Další kroky

- [Testování scénáře](service-fabric-testability-scenarios.md)
- Jak testování služby
   - [Simulovat k chybám při úloh služby](service-fabric-testability-workload-tests.md)
   - [Selhání-služeb komunikace](service-fabric-testability-scenarios-service-communication.md)
