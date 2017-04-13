<properties
   pageTitle="Jak vyvolat ztrátu dat na služby struktury | Microsoft Azure"
   description="Popisuje, jak používat ztrátou dat rozhraní api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Jak vyvolat ztrátu dat na služby

>[AZURE.WARNING] Tento dokument popisují, jak dojít ke ztrátě dat ve vašich služeb a bude použito opatrně.

## <a name="introduction"></a>Úvod
Vyvoláte ztrátou dat v oddílu vaše služba struktury tak, že volání StartPartitionDataLossAsync().  Toto rozhraní api používá k provedení práce způsobilo podmínky ztrátu dat pravděpodobnost vkládání a služby Analysis.

## <a name="using-the-fault-injection-and-analysis-service"></a>Použití pravděpodobnost vkládání a služby Analysis

Pravděpodobnost vkládání a služby Analysis aktuálně podporuje následující rozhraní API v grafu níže.  Pravé straně graf zobrazuje odpovídající rutiny prostředí PowerShell.  Získáte dokumentace msdn každý rozhraní API pro další informace o každého z nich.

|           ROZHRANÍ API C#                    |         Rutiny prostředí PowerShell                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Zahájení ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Zahájení ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Zahájení ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Přehled spuštění příkazu

Pravděpodobnost vkládání a služby Analysis používá asynchronní model, kde ho příkaz spustit jednu rozhraním API, označovány jako "Start" rozhraní API v tomto dokumentu a kontroly průběhu tento příkaz pomocí "GetProgress" rozhraní API, dokud dosáhla terminálu státu, nebo ji zrušit.
Příkaz zahájíte volání rozhraní API "Start" pro odpovídající rozhraní API.  Toto rozhraní API vrátí po pravděpodobnost vkládání a služby Analysis žádost přijal.  Však neuvádí určíte, jak má příkaz spustit, a to i v případě, že má nezačne.  Abyste mohli kontrolovat průběh příkazu, zavolejte na linku "GetProgress" rozhraní API, které odpovídá k rozhraní API "Start" dříve nazývanou.  "GetProgress" rozhraní API vrátí objekt označující aktuální stav příkazu uvnitř vlastností stavu.  Příkaz Spustit donekonečna udržovat do:

1.  Úspěšně dokončena.  Pokud jste volali "GetProgress" na ní v tomto případě, bude dokončena stav průběhu objektů.
2.  Najde kritické chyby.  Pokud jste volali "GetProgress" na ní v tomto případě, dojde k chybě stav průběhu objektů
3.  Zrušení prostřednictvím [CancelTestCommandAsync]  [ cancel] rozhraní API nebo [Ukončení ServiceFabricTestCommand]  [ cancelps] rutiny prostředí PowerShell.  Pokud jste volali "GetProgress" na ní v tomto případě, bude průběh objekt stavu zrušeno nebo ForceCancelled, podle toho, argument této rozhraní API.  Najdete v dokumentaci [CancelTestCommandAsync]  [ cancel] další podrobnosti.


## <a name="details-of-running-a-command"></a>Podrobnosti o spuštění příkazu

Ke spuštění příkazu, zavolejte na linku rozhraní API začít s očekávané argumenty.  Všechna spuštění rozhraní API mít argumentem Guid s názvem operationId.  Jste měli udržení přehledu o operationId argument od pracovní postup slouží ke sledování průběhu tohoto příkazu.  To musí být předaným "GetProgress" rozhraní API pro sledování průběhu příkazu.  OperationId musí být jedinečná.

Po úspěšném volání rozhraní API spustit, rozhraní API GetProgress by měl být s názvem ve smyčce až do dokončení vlastnosti objektu vrácené průběh stavu.  Všechny [společnosti FabricTransientException]  [ fte] a jeho OperationCanceledException by měl opakované.
Při dosažení terminálu stavu (dokončeno, chybný nebo zrušeno) na příkaz objekt vrácené průběh výsledek vlastnost bude mít další informace.  V případě stavu je dokončen, bude obsahovat Result.SelectedPartition.PartitionId id oddílu, který jste vybrali.  Result.Exception bude mít hodnotu null.  Pokud došlo k chybě stavu Result.Exception bude mít důvod, proč pravděpodobnost vkládání a služby Analysis k chybě příkazu.  Result.SelectedPartition.PartitionId bude mít id oddílu, který jste vybrali.  V některých případech nemusí příkazu dostatečně úplně pokračovat výběr oddílu.  V takovém případě ID oddílu budou 0.  Pokud je zrušena stavu, bude mít Result.Exception hodnotu null.  Jako chybný případ Result.SelectedPartition.PartitionId bude mít id oddílu, který byl vybrán, ale pokud příkazu dostatečně úplně nedošlo k tomu nevyzve, bude 0.  Taky získáte následující ukázce.

Následující ukázkový kód ukazuje, jak začít pak kontroly průběhu na příkaz dojít ke ztrátě dat do určitého oddílu.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Následující příklad ukazuje, jak používat PartitionSelector zvolit náhodné oddíl zadané služby:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Historie a zkrácení

Po příkazu dosáhl terminálu státu, jeho metadata zůstanou v pravděpodobnost vkládání a služby Analysis určitou dobu, než se odeberou šetřit tak místo.  Jestliže "GetProgress" je jen pomocí operationId příkazu poté, co už nenajdete, vrátí funkce FabricException s kód chyby KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
