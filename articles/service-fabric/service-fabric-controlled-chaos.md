<properties
   pageTitle="Vyvolat Chaos v služby struktury clusterů | Microsoft Azure"
   description="Použití pravděpodobnost vkládání a rozhraní API služeb Analysis clusteru ke správě Chaos clusteru."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Vyvolat řízené Chaos v služby struktury clusterů
Rozsáhlé distribuované systémy, jako jsou podstatě nedůvěryhodných infrastruktury cloudu. Azure struktury služby umožňuje vývojářům psát spolehlivé služby nad infrastrukturu nedůvěryhodných. Psaní nabídkou služeb, vývojáři chcete moct způsobit chyby proti takové nedůvěryhodných infrastruktury otestovat stability svých služeb.

Pravděpodobnost vkládání a služby Analysis obrázku (označovaná taky jako služba Analysis poruch) umožňuje vývojářům vyvolat poruch akce, které chcete testovat služby. Však cílových simulovaný chyby pomohou pouze zatím. Aby testování dál, můžete použít Chaos.

Chaos napodobuje nepřetržitý, prokládaném chyb (bezproblémové i vynuceném) v celém clusteru průběhu delšího období. Po konfiguraci Chaos s rychlosti a druh chyb se spustí nebo zastaví prostřednictvím rozhraní API C# nebo prostředí PowerShell generovat chyb v clusteru a služby.

Je spuštěná Chaos vytvoří různých událostech, zachycení stavu spustit v okamžiku. Například ExecutingFaultsEvent obsahuje všechny chyby, provedených v této opakování. ValidationFailedEvent obsahuje podrobnosti o selhání nalezený během clusteru ověření. Vyvoláte rozhraní API GetChaosReportAsync zobrazíte sestavu Chaos se spustí.

## <a name="faults-induced-in-chaos"></a>Chyby způsobené v Chaos
Chaos vygeneruje chyby přes celého obrázku služby struktury a zkomprimuje chyb, které se nacházejí v měsících nebo letech do několika hodin. Kombinace prokládaném chyby s vysokou poruch sazba vyhledá případy rohu, které jsou jinak zmeškané. Tento Chaos cvičení vede k významné zlepšování kvality kód služby.

Chaos indukuje chyby z těchto kategorií:

 - Restartujte uzel
 - Restartujte balíček nasazeném kódu
 - Odebrání otevřené
 - Restartujte otevřené
 - Přesunutí primární otevřené (Konfigurovat)
 - Přesunutí sekundární otevřené (Konfigurovat)

Chaos běží ve více iterací. Každý iterace se skládá z chyby a ověření obrázku pro zadané období. Můžete nakonfigurovat čas strávený clusteru stabilizovat a pro ověření úspěšné. Pokud se nepovede nachází v ověření clusteru, Chaos vygeneruje a trvá ValidationFailedEvent s časové razítko UTC a podrobnosti chyby.

Zvažte například instanci Chaos, který je nastavený ke spouštění hodiny maximálně tři souběžné chyby. Chaos indukuje tři chyby a potom ověří stavu obrázku. Ho prochází předchozí krok, dokud nebude explicitně přestal prostřednictvím rozhraní API StopChaosAsync ani hodinovou předá. Pokud clusteru změní chybná v libovolné opakování (to znamená ho není stabilizovat v nakonfigurovaném čase), Chaos vygeneruje ValidationFailedEvent. Tato událost označuje, že něco se nepovede a může být nutné další vyšetřování.

V jeho aktuálním formuláři indukuje Chaos pouze bezpečných chyby. To znamená, že v nepřítomnosti externí chyby ztráty kvora nebo ztrátou dat nikdy nedošlo.

## <a name="important-configuration-options"></a>Možnosti důležité konfigurace
 - **TimeToRun**: celkový čas, které se spustí Chaos před dokončením úspěšně. Před spuštění dobu TimeToRun prostřednictvím rozhraní API StopChaos zastavit Chaos.
 - **MaxClusterStabilizationTimeout**: maximální počet hodin až clusteru osvobozením správný před vyhledáváním v něm znovu. Toto je zmenšit zatížení clusteru, když je obnovení. Provést jsou:
    - Pokud stavu clusteru je v pořádku.
    - Pokud je stav služby v pořádku.
    - Pokud otevřené cílové nastavte velikost nedosáhnete oddílu service
    - Existenci žádné InBuild repliky
 - **MaxConcurrentFaults**: maximální počet souběžné chyb, které jsou způsobené v každém opakování. Vyšší čísla, čím víc agresivní Chaos je. Výsledkem kombinace přechodu a složitější převzetí služeb při selhání. Chaos zaručuje, že v nepřítomnosti externí chyby nemůže žádným způsobem ztráty kvora ani ztrátou dat, bez ohledu na to, jak vysoko hodnota této konfiguraci s.
 - **EnableMoveReplicaFaults**: Zapne nebo vypne chyb, které způsobují primární a sekundární repliky přesunout. Ve výchozím nastavení jsou zakázaná tyto chyby.
 - **WaitTimeBetweenIterations**: dobu čekat mezi iteracemi, tedy po zaokrouhlit chyby a odpovídající ověření.
 - **WaitTimeBetweenFaults**: dobu mezi dvěma po sobě jdoucí chyb v iterace.

## <a name="how-to-run-chaos"></a>K tomu Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**Powershellu:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
