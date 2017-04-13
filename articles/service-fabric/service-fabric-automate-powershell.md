<properties
    pageTitle="Automatizace Správa aplikací služby struktury pomocí prostředí PowerShell | Microsoft Azure"
    description="Nasazení upgradovat, otestujte a odebrání aplikací služby struktury pomocí prostředí PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatizace životním cyklu aplikace pomocí prostředí PowerShell

Můžete automatizovat aspektech životního [cyklu aplikace služby struktury](service-fabric-application-lifecycle.md) .  Tento článek ukazuje, jak pomocí Powershellu Automatizace běžných úkolů pro nasazení upgradu, odebrání a testování struktury služby Azure aplikací.  Spravovat a HTTP rozhraní API pro správu aplikace jsou dostupné taky. V tématu [životního cyklu aplikace](service-fabric-application-lifecycle.md) Další informace.  

## <a name="prerequisites"></a>Zjistit předpoklady pro
Přesunutí k úkolům v článku nezapomeňte:

+ Seznámení s koncepty služby struktury popsané v [technický přehled struktury služby](service-fabric-technical-overview.md).
+ [Instalace nástroje runtime SDK a](service-fabric-get-started.md)taky instalace modulu **ServiceFabric** Powershellu.
+ [Spuštění skript Powershellu povolit](service-fabric-get-started.md#enable-powershell-script-execution).
+ Spusťte místní obrázku.  Spuštění nové okno prostředí PowerShell jako správce a potom spusťte instalační skript obrázku ve složce SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Před spuštěním všechny příkazy Powershellu v tomto článku se nejprve připojte se ke místní služba struktury clusteru pomocí [**ServiceFabricCluster připojení**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Následující úkoly vyžadují v1 balíčku aplikace pro nasazení a v2 balíčku aplikace pro upgrade. Stáhněte si [ **WordCount** ukázková aplikace](http://aka.ms/servicefabricsamples) (umístěné ve vzorcích Začínáme). Vytvořte a balíček aplikace ve Visual Studiu (klikněte pravým tlačítkem myši na **WordCount** v Průzkumníku řešení, vyberte **balíček**). Zkopírujte balíček v1 v `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` k `C:\Temp\WordCount`. Kopírovat `C:\Temp\WordCount` k `C:\Temp\WordCountV2`, vytvoření balíčku aplikace v2 pro upgrade. Otevřít `C:\Temp\WordCountV2\ApplicationManifest.xml` v textovém editoru. V elementu **ApplicationManifest** změňte atribut **ApplicationTypeVersion** z "1.0.0" na "2.0.0" Aktualizovat číslo verze aplikace. Uložte soubor změněné ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Úkol: Nasazení aplikace služby struktury

Po vytvořené a balíčku aplikace (nebo stáhnout balíček aplikace), můžete nasadit aplikaci do místní clusteru struktury služby. Nasazení zahrnuje nahrání balíčku aplikace, registrace typ aplikace a vytváření instance aplikace. Nasaďte novou aplikaci clusteru pomocí pokynů uvedených v této části.

### <a name="step-1-upload-the-application-package"></a>Krok 1: Nahrání balíčku aplikace
Nahrání balíčku aplikace na úložiště obrázek vloží ho do umístění přístupné pro interní součásti struktury služby.  Balíček aplikace obsahuje potřebné manifestu aplikace služby manifesty a kód, konfigurace a balíčky dat, které chcete vytvořit aplikaci a instancí služby.  Příkaz [**Kopírovat ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) odešle balíček. Příklad:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Krok 2: Registrace typ aplikace
Typ aplikace a verzi deklarované v manifest aplikace dostupné pro použití registraci balíčku aplikace díky. Systém přečte balíček nahráli v kroku 1, ověření balíček (odpovídá spuštěn [**Test ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) místně), zpracování obsahu balíčku a zkopírujte balíček zpracovaných do polohy vnitřní systém.  Spusťte rutinu [**ServiceFabricApplicationType registru**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCount
```
Pokud chcete zobrazit všechny typy aplikací registrované clusteru, spusťte rutinu [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Krok 3: Vytvoření instance aplikace
Aplikaci můžete vytvořené pomocí všechny verze aplikace typu registrované úspěšně pomocí příkazu [**Nový ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . Název každého aplikace je deklarovat na nasazení čas a musí začínat **struktury:** schématu a být jedinečné pro každou instanci aplikace. Zadejte název aplikace a verzi aplikace typu jsou deklarovány v souboru **ApplicationManifest.xml** balíčku aplikace. Pokud jakékoli výchozí služby byly definované v aplikaci manifestu typ cílové aplikace, pak můžou být vytvořené v současné době.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Příkaz [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) vypíše všechny instance aplikace, které byly úspěšně vytvořené spolu s jejich celkový stav. Příkaz [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) obsahuje seznam všech instancí služby, které byly úspěšně vytvořené v rámci instance dané aplikace. Výchozí služby (pokud existuje) jsou uvedené.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Úkol: Upgrade služeb struktury aplikace
Můžete upgradovat dříve nasazení aplikace služby struktury s balíčku aktualizované aplikace. Tento úkol aktualizuje WordCount aplikace, která byla nasazenou v "úkolu: nasazení aplikace služby struktury." Přečtěte si prostřednictvím [služby struktury aplikace upgradu](service-fabric-application-upgrade.md) Další informace.

Jednodušší jednoduché v tomto příkladu, pouze číslo verze aplikace aktualizovaném balíček aplikace WordCountV2 vytvořené v požadavky. Další reálné situace by zahrnovala aktualizace souborů služby kódu, konfigurace nebo data a opětovné vytvoření balení aplikace reprezentovaný číslem od aktualizovanou verzi.  

### <a name="step-1-upload-the-updated-application-package"></a>Krok 1: Odeslání aktualizované aplikace balíčku
Aplikace v1 WordCount je připravená k upgradu. Pokud otevřete okno prostředí PowerShell jako správce a typ [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)vidíte že nasazený tuto verzi 1.0.0 typ WordCount aplikace.  

Nyní kopírovat balíčku aktualizované aplikace do úložiště služby struktury obrázek (balíčků aplikací uloženými podle struktury služby). Parametr **ApplicationPackagePathInImageStore** informuje struktury služby, kde najdou balíček aplikace. Následující příkaz zkopíruje balíčku aplikace na **WordCountV2** v úložišti obrázek:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Krok 2: Registrace typ aktualizované aplikace
Dalším krokem je k registraci novou verzi aplikace struktury služby, které lze provést pomocí rutiny [**ServiceFabricApplicationType registru**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Krok 3: Zahájení upgradu
Různé upgradu parametry časové limity a stavu kritéria lze použít inovací aplikace. Čtení dokumentů [aplikace upgradu parametry](service-fabric-application-upgrade-parameters.md) a [proces upgradu](service-fabric-application-upgrade.md) Další informace. Všechny služby a instance by měl být _správný_ po upgradu.  Nastavte **HealthCheckStableDuration** na 60 sekundy (tak, aby služby správný pro aspoň 20 sekundách upgrade pokračuje na další upgradu doménu).  Nastavte taky **UpgradeDomainTimeout** 1200 sekund a **UpgradeTimeout** na 3000 sekund. Nakonec nastavte **UpgradeFailureAction** k **vrácení zpět**, která vyžaduje, aby služby struktury vrátí aplikaci v předchozí verzi pokud jsou během upgradu došlo k chybám.

Upgrade aplikace můžete začít pomocí rutiny [**Start ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Všimněte si, že název aplikace stejný jako název aplikace dříve nasazeném v1.0.0 (struktury: / WordCount). Služba struktury používá tento název k identifikaci začíná upgradu která aplikace. Pokud jste nastavili časové limity relací příliš krátkým, můžete narazit chybová zpráva o časový limit, které je tento pokyn problém. V nápovědě k [Poradce při potížích s aplikací inovace](service-fabric-application-upgrade-troubleshooting.md)nebo zvětšit časové limity relací.

### <a name="step-4-check-upgrade-progress"></a>Krok 4: Kontrola průběh upgradu
Můžete sledovat průběh upgradu aplikace pomocí [Služby struktury Průzkumníka](service-fabric-visualizing-your-cluster.md)nebo můžete použít rutinu [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) :

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Za několik okamžiků rutinu [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) zobrazuje, aby byly všechny upgradu domény upgradovali (Dokončit).

## <a name="task-test-a-service-fabric-application"></a>Úkol: Otestujte aplikaci služby struktury

Psaní vysoce kvalitní služby, vývojáři chcete moct způsobit chyby nedůvěryhodných infrastruktury otestovat stability svých služeb. Služby struktury umožňuje vývojářům vyvolat poruch akce a testování služeb přítomnosti chyby pomocí scénáře testování chaos a převzetí.  [Přehled testování](service-fabric-testability-overview.md) přečíst další informace.

### <a name="step-1-run-the-chaos-test-scenario"></a>Krok 1: Spusťte test scénář chaos
Scénář testování chaos vygeneruje chyby napříč celou clusteru struktury služby. Scénáře zkomprimuje chyby obecně zobrazené v měsících nebo letech několik hodin. Kombinace prokládaném chyby s vysokou poruch sazbu vyhledá případy rohu, které by jinak zmeškané. V následujícím příkladu se spustí scénář testování chaos dobu 60 minut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Krok 2: Spuštění scénář testování překlopení
Přenesení testování scénář cílů oddíl konkrétní službu namísto celého obrázku a ponechá jiných služeb, nemá vliv. Scénáře prochází posloupnosti simulovaný chyb v služby ověření průběhu obchodní logiky. Chyba ověřování služby označuje problém, který vyžaduje další vyšetřování. Převzetí test indukuje jedinou poruch najednou, namísto scénář testování chaos, který může být nutné více chyby. Následující příklad spustí test převzetí dobu 60 minut struktury: / WordCount/WordCountService služby.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Úkol: Odebrání aplikace služby struktury
Můžete odstranit instanci nasazení aplikace, odeberte typ zřizování aplikace z clusteru a odebrání balíčku aplikace ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Krok 1: Odeberte instanci aplikace
Pokud již není potřeba instance aplikace, můžete ho odebrat trvale pomocí rutiny [**ServiceFabricApplication odebrat**](https://msdn.microsoft.com/library/azure/mt125914.aspx) . Tím taky automaticky odeberete všechny služby patřící do aplikace trvale odeberete všechny stav služby. Tuto operaci nejde vrátit zpět a nelze ji obnovit stav aplikace.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Krok 2: Unregister typ aplikace
Když už nepotřebujete konkrétní verzi aplikace typu unregister pomocí rutiny [**Unregister ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) . Registrace nepoužitý typy vydání úložného prostoru používaný balíčku aplikace v úložišti obrázek. Typ aplikace možné, dokud aplikace vytvořena u ní nebo čekající aplikace upgrady odkazování.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Krok 3: Odebrání balíčku aplikace
Po registrace typ aplikace, lze odebrat balíčku aplikace z obchodu obrázek pomocí rutiny [**ServiceFabricApplicationPackage odebrat**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Další kroky
[Služba struktury aplikace cyklus](service-fabric-application-lifecycle.md)

[Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[Upgrade aplikace](service-fabric-application-upgrade.md)

[Rutiny pro Azure struktury služby](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Rutiny pro správu testování Azure struktury služby](https://msdn.microsoft.com/library/azure/mt125844.aspx)
