<properties
   pageTitle="Jak zobrazit struktury služby Azure entity agregované stavu | Microsoft Azure"
   description="Popisuje, jak dotazu, zobrazení a vyhodnocování agregované stavu struktury služby Azure entity prostřednictvím stavu dotazy a obecné."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Zobrazení sestav stavu služby struktury
Azure struktury služby uvádí [stavu modelu](service-fabric-health-introduction.md) , který zahrnuje na systém, který můžete sestav součásti a watchdogs místní podmínky, které jsou sledování stavu entity. [Stav uložení](service-fabric-health-introduction.md#health-store) sloučí všechna data stavu a určuje, zda se správný entity.

Odhlášení z pole clusteru je vyplněn sestav stavu odeslaný součásti systému. Další informace na [sestavy stavu systému použít k řešení](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Služba struktury obsahuje více z následujících způsobů agregované stavu entity:

- [Služba struktury Explorer](service-fabric-visualizing-your-cluster.md) a další vizualizace nástroje

- Stav dotazů (pomocí Powershellu, API nebo REST)

- Obecné dotazy to zpáteční seznam osob, které mají stav jako jednu z vlastností (pomocí Powershellu, API nebo REST)

Ukazují tyto možnosti, Pojďme pomocí místní clusteru pět uzlů. Vedle položky **struktury: / systém** aplikace (který existuje mimo pole) nasazený některé aplikace. Jedna z těchto aplikací **struktury: / WordCount**. Tato aplikace obsahuje stavová služba nakonfigurována sedm repliky. Protože jsou pouze pět uzly, součásti systému zobrazit upozornění, že tento oddíl je menší než počet cílové.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Stav v Průzkumníku struktury služby
Služba struktury Explorer nabízí vizuální zobrazení clusteru. Na následujícím obrázku můžete vidět, který:

- Aplikace **struktury: / WordCount** je červené (v chyba), protože obsahuje chybu události vykázaného **MyWatchdog** vlastnosti **dostupnosti**.

- Jednu ze svých služeb, **struktury: / WordCount/WordCountService** žlutý (v upozornění). Protože službu nakonfigurovaný s sedm replikami, budou nelze umístit, jsou pouze pět uzlů. I když není vidět tady, není oddílu service žlutá kvůli sestava systému. Žlutá oddíl spustí žlutá služby.

- Clusteru je červená kvůli červené aplikace.

Hodnocení používá výchozí zásady z obrázku manifestu a manifestu aplikace. Jsou vždy zásady a není tolerovat všechny chyby.

Zobrazení obrázku v Průzkumníkovi struktury služby:

![Zobrazení obrázku v Průzkumníkovi struktury služby.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Další informace o [Explorer struktury služby](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Stav dotazů
Služba struktury zpřístupňuje stavu dotazů pro všechny podporované [typy entit](service-fabric-health-introduction.md#health-entities-and-hierarchy). Lze k nim prostřednictvím rozhraní API (metody se nachází na **FabricClient.HealthManager**), rutiny prostředí PowerShell a ZBÝVAJÍCÍ. Tyto dotazy vrácení dokončení stavu informací o entitu: agregovaná stavu, události stavu entitu, podřízené stavu státy (je-li k dispozici) a chybná hodnocení při entitu není v pořádku.

> [AZURE.NOTE] Po úplném naplnění v úložišti stavu, vrátí se stav entity. Entita musí být aktivní (není odstraněna) a mít systém sestavy. Jeho nadřazeného entity v řetězci hierarchie musí mít taky systémové sestavy. Pokud některé z těchto podmínek není splněno, dotazy stavu vracet výjimce znázorňující, proč není vrácena entity.

Dotazy stavu musí předat identifikátor entity, který závisí na typu entity. Dotazy přijmout parametry zásad volitelné stavu. Pokud jsou zadán žádný stav zásad, [zásady stavu](service-fabric-health-introduction.md#health-policies) z manifest obrázku nebo aplikace používají pro hodnocení. Dotazy také přijímat filtry pro vložení jenom částečné děti a události – ty, které dodržovat zadanými filtry.

> [AZURE.NOTE] Výstup filtrů na straně serveru, takže odpovědi zmenšení zprávy. Doporučujeme, můžete pomocí filtrů výstup omezit data vrácená namísto použití filtrů na straně klienta.

Stav entity obsahuje:

- Agregovaná stavu entity. Výpočet tak, že úložišti stav na základě sestav stavu entitu, podřízené stavu USA (je-li k dispozici) a zásady stavu. Další informace o [stavu hodnocení entity](service-fabric-health-introduction.md#entity-health-evaluation).  

- Události stavu entity.

- V kolekci stavů stav všech podřízených položek, které můžou mít děti entit. Stav stavu obsahují identifikátory entita a agregovaná stavu. Zobrazíte dokončení stavu pro podřízený volání stavu dotazu pro typ entity podřízené a předejte v podřízené identifikátor.

- Nefunkční hodnocení odkazujícími na sestavu, spouštěný stav entitu, pokud není správný.

## <a name="get-cluster-health"></a>Získat stav obrázku
Vrátí stavu clusteru entita a obsahuje státy stav aplikací a uzly (děti clusteru). Zadání vstupních hodnot:

- [Volitelné] Zásady stavu clusteru používá k vyhodnocení uzly a události obrázku.

- [Volitelné] Aplikace stavu zásad mapování, zásady stavu použít k přepsání seznamu zásady použití.

- [Volitelné] Filtry pro události, uzly a aplikace, které určí položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události uzly a aplikací slouží k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Pokud chcete získat stav obrázku, vytvořte `FabricClient` a volat metodu [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) na jeho **HealthManager**.

Následující volání získá stav obrázku:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Následující kód získá stavu clusteru prostřednictvím zásad stavu vlastního obrázku a filtry uzly a aplikací. Vytvoří [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), který obsahuje informace o zadávání.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav clusteru je [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Stav clusteru je pět uzly, aplikace systému a struktury: / WordCount nakonfigurované podle popisu.

Následující rutinu získá clusteru stavu pomocí výchozí zásady stavu. Souhrnné stavu je v upozornění, protože struktury: / WordCount aplikace je v upozornění. Všimněte si, jak chybná hodnocení podrobnosti na zadejte podmínky, které spouštěný agregované stavu.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Následující rutinu Powershellu získá stavu clusteru pomocí zásady vlastní aplikaci. Jsou filtrovány výsledky získat jenom chyby nebo aplikací upozornění a uzlů. Jako výsledek budou vráceny žádné uzly, jako jsou všechny správný. Pouze struktury: / WordCount aplikace použije filtr aplikací. Protože vlastní zásady určuje vzít v úvahu upozornění jako chybné pro struktury: / aplikace WordCount Vyhodnocená každá její položka jako chyby a je vlastně clusteru.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat stav obrázku s [získat žádost](https://msdn.microsoft.com/library/azure/dn707669.aspx) nebo [žádost o příspěvku](https://msdn.microsoft.com/library/azure/dn707696.aspx) , který obsahuje zásady stavu popsané v textu.

## <a name="get-node-health"></a>Získání uzel stavu
Vrátí stavu uzel entita a obsahuje události stavu vykázanému uzel. Zadání vstupních hodnot:

- [Povinné] Uzel název, který identifikuje uzel.

- [Volitelné] Obrázku stavu nastavení zásad používat k vyhodnocování stavu.

- [Volitelné] Filtry pro události, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události se používají k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Získat stav uzel prostřednictvím rozhraní API, vytvořit `FabricClient` a volat metodu [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) na jeho HealthManager.

Následující kód získá uzel stav u jména zadaný uzel:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Následující kód získá stavu uzel pro název zadaný uzel a prochází [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx)události filtr a vlastní zásady:

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav uzel je [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .
Následující rutinu získá stavu uzel pomocí výchozího stavu zásady:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Následující rutinu získá stav všech uzlů v clusteru:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat stav uzel s [získat žádost](https://msdn.microsoft.com/library/azure/dn707650.aspx) nebo [žádost o příspěvku](https://msdn.microsoft.com/library/azure/dn707665.aspx) , který obsahuje zásady stavu popsané v textu.

## <a name="get-application-health"></a>Získání aplikace stavu
Vrátí stavu entita aplikace. Jeho stav stavu nasazení aplikace a služby dětí. Zadání vstupních hodnot:

- [Povinné] Název aplikace (URI Uniform), který identifikuje aplikace.

- [Volitelné] Slouží k přepsání seznamu zásady použití zásady použití stavu.

- [Volitelné] Filtry pro události, služby a nasazení aplikace, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události, služby a nasazení aplikace slouží k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Získat stav aplikace, vytvářet `FabricClient` a volat metodu [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) na jeho HealthManager.

Následující kód získá funkčnosti aplikací pro zadaný název aplikace (URI Uniform):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Následující kód získá funkčnosti aplikací pro zadaný název aplikace (URI Uniform), pomocí filtrů a vlastní zásady zadané prostřednictvím [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav aplikace je [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Vrátí následující rutinu stavu **struktury: / WordCount** aplikace:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Následující rutinu Powershellu předává ve vlastní zásady. Filtruje taky děti a události.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat aplikaci stavu se [ZOBRAZÍ žádost o](https://msdn.microsoft.com/library/azure/dn707681.aspx) nebo [žádost příspěvku](https://msdn.microsoft.com/library/azure/dn707643.aspx) , který obsahuje zásady stavu popsané v těle.

## <a name="get-service-health"></a>Získat stav služby
Vrátí stavu služby entity. Obsahuje státy stavu oddíl. Zadání vstupních hodnot:

- [Povinné] Název služby (URI Uniform), který identifikuje službu.

- [Volitelné] Slouží k přepsání seznamu zásady použití zásady použití stavu.

- [Volitelné] Filtry pro události a oddíly, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události a oddíly se používají k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Chcete-li získat stav služby prostřednictvím rozhraní API, vytvořte `FabricClient` a volat metodu [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) na jeho HealthManager.

V následujícím příkladu uživatel dostane stavu služby s názvem zadaný služby (URI Uniform):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Následující kód získá stav služby pro zadaný název služby URI (Uniform) určující filtry a vlastní zásady prostřednictvím [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav služby je [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Následující rutinu získá stav služby pomocí výchozího stavu zásady:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Stav služby se [ZOBRAZÍ žádost o](https://msdn.microsoft.com/library/azure/dn707609.aspx) nebo [žádost příspěvku](https://msdn.microsoft.com/library/azure/dn707646.aspx) , který obsahuje zásady stavu popsané v těle, můžete získat.

## <a name="get-partition-health"></a>Získání oddíl stavu
Vrátí stavu oddíl entity. Jeho stav otevřené stavu. Zadání vstupních hodnot:

- [Povinné] Oddíl ID (GUID), který identifikuje oddílu.

- [Volitelné] Slouží k přepsání seznamu zásady použití zásady použití stavu.

- [Volitelné] Filtry pro události a repliky, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události a repliky slouží k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Oddíl stavu prostřednictvím rozhraní API získáte vytvořit `FabricClient` a volat metodu [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) na jeho HealthManager. Chcete-li volitelné parametry, vytvořte [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav oddíl je [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Následující rutinu získá stavu pro všechny oddíly **struktury: / WordCount/WordCountService** služby:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat stav oddílu se [ZOBRAZÍ žádost o](https://msdn.microsoft.com/library/azure/dn707683.aspx) nebo [žádost příspěvku](https://msdn.microsoft.com/library/azure/dn707680.aspx) , který obsahuje zásady stavu popsané v těle.

## <a name="get-replica-health"></a>Získat stav otevřené
Vrátí stavu stavová služba otevřené nebo instance příslušnosti služby. Zadání vstupních hodnot:

- [Povinné] Identifikátor GUID a otevřené ID oddílu, který identifikuje otevřené.

- [Volitelné] Aplikace stavu zásad parametrech použít k přepsání seznamu zásady použití.

- [Volitelné] Filtry pro události, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události se používají k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Chcete-li získat stav otevřené prostřednictvím rozhraní API, vytvořte `FabricClient` a volat metodu [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) na jeho HealthManager. Chcete-li upřesnit parametry, použijte [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav otevřené je [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Následující rutinu získá stavu primární otevřené pro všechny oddíly ze služby:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat stav otevřené [získat žádost o](https://msdn.microsoft.com/library/azure/dn707673.aspx) nebo [žádost o příspěvku](https://msdn.microsoft.com/library/azure/dn707641.aspx) , který obsahuje zásady stavu popsané v textu.

## <a name="get-deployed-application-health"></a>Získání stavu nasazení aplikace
Vrátí stavu aplikace nasazené na uzel entity. Obsahuje stavy nasazeném služby balíčků stavu. Zadání vstupních hodnot:

- [Povinné] Název aplikace (URI Uniform) a název uzlu (řetězec) určující nasazení aplikace.

- [Volitelné] Slouží k přepsání seznamu zásady použití zásady použití stavu.

- [Volitelné] Filtry pro události a balíčků nasazeném služeb, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události a balíčků nasazení služeb slouží k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Pokud chcete získat stav aplikace nasazené na uzel prostřednictvím rozhraní API, vytvořte `FabricClient` a volat metodu [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) na jeho HealthManager. Chcete-li volitelné parametry, použijte [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav nasazení aplikace je [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) . Pokud chcete zjistit, kde je aplikace nasazený, spusťte [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) a podívejte se na děti nasazení aplikace.

Následující rutinu získá stavu **struktury: / WordCount** nasazené na **_Node_2**aplikace.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Nasazení aplikace stavu se [ZOBRAZÍ žádost o](https://msdn.microsoft.com/library/azure/dn707644.aspx) nebo [žádost příspěvku](https://msdn.microsoft.com/library/azure/dn707688.aspx) , který obsahuje zásady stavu popsané v těle, můžete získat.

## <a name="get-deployed-service-package-health"></a>Získat stav balíčku nasazeném služby
Vrátí stavu entitu balíčku nasazeném služby. Zadání vstupních hodnot:

- [Povinné] Název aplikace (URI Uniform), název uzlu (řetězec) a název seznamu služby (řetězec) identifikující balíček nasazeném služby.

- [Volitelné] Slouží k přepsání seznamu zásady použití zásady použití stavu.

- [Volitelné] Filtry pro události, které určují položky, které jsou předmětem zájmu a má být vrácena ve výsledku (například pouze, chyby nebo upozornění i chyby). Všechny události se používají k vyhodnocování entity agregované stavu, bez ohledu na filtr.

### <a name="api"></a>ROZHRANÍ API
Získat stav balíku nasazeném služby prostřednictvím rozhraní API, vytvořit `FabricClient` a volat metodu [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) na jeho HealthManager. Chcete-li volitelné parametry, použijte [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav balíčku nasazeném služby je [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) . Kde nasazení aplikace zobrazíte spusťte [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) a podívejte se na nasazení aplikace. Které služby jsou balíčky aplikace zobrazíte podívejte děti balíčku nasazeném služby do výstupu [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

Následující rutinu získá stavu balíčku služby **WordCountServicePkg** **struktury: / WordCount** nasazené na **_Node_2**aplikace. Entita má **System.Hosting** sestav pro úspěšné balíčku služeb a aktivace vstupního bodu a úspěšné registraci typ služby.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat stav nasazeném služby balíčku s [získat žádost](https://msdn.microsoft.com/library/azure/dn707677.aspx) nebo [žádost o příspěvku](https://msdn.microsoft.com/library/azure/dn707689.aspx) , který obsahuje zásady stavu popsané v textu.

## <a name="health-chunk-queries"></a>Stav bloku dotazů
Dotazy bloku stavu můžete vrátit děti víceúrovňových obrázku (zpětně), za zadávání filtry. Podporuje pokročilé filtry, které umožňují mnohem flexibilitu vyjádřit které konkrétní děti má být vrácena, označenými jedinečný identifikátor nebo jiných identifikátor skupiny nebo stavu. Ve výchozím nastavení jsou však započítávány namísto stavu příkazy, které vždy obsahovat první úrovně děti žádné podřízené.

[Stav dotazů](service-fabric-view-entities-aggregated-health.md#health-queries) vracet pouze první úrovně děti zadaný entita na požadované filtry. Děti dětí získáte musíte volat další stavu rozhraní API pro každou entitu zájmu. Podobně stavu konkrétní entity získáte musí zavoláte jeden stavu rozhraní API pro každý požadovaný entitu. Dotaz bloku rozšířené filtrování umožňuje požádat o více položek potřebné v jednom dotazu minimalizace velikost zprávy a počet zpráv.

Dotaz bloku hodnotu, pro si můžete pořídit stavu další clusteru entity (potenciálně všechny clusteru entity počínaje požadovaný kořenový) v jednom volání. Komplexní stavu dotazu lze vyjádřit jako:

- Vraťte se pouze aplikace na chyby a pro tyto aplikace zahrnout všechny služby na upozornění | chyby. Vrácené státní zahrnout všechny oddíly.

- Vrátí jenom stavu 4 aplikací nastavil jejich jména.

- Vrátí jenom stavu aplikace typu požadovanou aplikaci.

- Vrátí všechny nasazeném entity na uzel. Vrátí všechny aplikace, všechny nasazení aplikace v určeném uzlu a všechny balíčků nasazení služeb v daném uzlu.

- Vrátí ostatními na chyby. Vrátí všechny aplikace služeb, oddíly a pouze repliky na chyby.

- Vrátí všechny aplikace. Pro zadaný služby zahrnout všechny oddíly.

V současné době dotazu bloku stavu se zobrazí pouze u obrázku entity. Vrátí bloku stav obrázku, který obsahuje:

- Clusteru agregované stavu.

- Seznamu zdravotní stav bloku uzlů, které dodržovat vstupní filtry.

- Zdravotní stav bloku seznam aplikace dodržování vstupní filtry. Každý blok aplikace zdravotní stav obsahuje seznam bloku se všemi službami, které dodržovat vstupní filtry a seznam bloku všechny nasazeném aplikace dodržování filtry. To samé platí pro děti služeb a nasazené aplikací. Tímto způsobem všechny entity clusteru mohou být potenciálně vrácena žádost v hierarchickém uspořádání.

### <a name="cluster-health-chunk-query"></a>Shluk stavu bloku dotazu
Vrátí stavu clusteru entita a obsahuje bloky stavu hierarchické stavu požadované dětí. Zadání vstupních hodnot:

- [Volitelné] Zásady stavu clusteru používá k vyhodnocení uzly a události obrázku.

- [Volitelné] Aplikace stavu zásad mapování, zásady stavu použít k přepsání seznamu zásady použití.

- [Volitelné] Filtry pro uzly a aplikace, které určí položky, které jsou předmětem zájmu a má být vrácena ve výsledku. Filtry jsou specifické pro entity/skupinu entit nebo k dispozici všechny entity na této úrovni. Seznam filtrů může obsahovat jeden obecné filtr a/nebo filtry pro specifické identifikátory jemnou entit vracená dotazem. Pokud prázdné, nejsou ve výchozím nastavení vrátí dětí.
Další informace o filtrech [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) a [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Zpětně můžou filtry aplikace zadejte Pokročilé filtry pro děti.

Výsledek bloku obsahuje podřízené položky, které dodržovat filtry.

V současné době dotazu bloku nevrací chybná hodnocení nebo události entity. Další informace získáte pomocí existujícího dotazu stavu obrázku.

### <a name="api"></a>ROZHRANÍ API
Získat bloku stav obrázku, vytvořit `FabricClient` a volat metodu [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) na jeho **HealthManager**. Můžete předat [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) popisuje zásady stavu a pokročilé filtry.

Následující kód získá clusteru stavu bloku s rozšířené filtry.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>Prostředí PowerShell
Rutina získat stav clusteru je [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Nejdřív připojte k clusteru pomocí rutiny [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) .

Následující kód získá uzly jenom v případě, že se zobrazuje chyba s výjimkou pro konkrétní uzel vždy má být vrácena.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Následující rutinu získá clusteru bloku s aplikací filtry.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Následující rutinu vrátí všechny nasazeném entity na uzel.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>ZBÝVAJÍCÍ
Můžete získat clusteru stavu bloku s [získat žádost](https://msdn.microsoft.com/library/azure/mt656722.aspx) nebo [žádost o příspěvku](https://msdn.microsoft.com/library/azure/mt656721.aspx) , který obsahuje zásady stavu a rozšířeného filtru popsané v textu.

## <a name="general-queries"></a>Obecné dotazy
Obecné dotazy vracet seznam služby struktury entit určitého typu. Se ukazuje prostřednictvím rozhraní API (pomocí metody na **FabricClient.QueryManager**), rutiny prostředí PowerShell a ZBÝVAJÍCÍ. Tyto dotazy agregovat poddotazy z více složek. Jedna z nich je na [ukládání stavu](service-fabric-health-introduction.md#health-store), který naplní agregované stavu pro každý výsledek dotazu.  

> [AZURE.NOTE] Obecné dotazy vrátí agregovanou stavu entita a neobsahují bohaté stavu data. Pokud entita není v pořádku, zpracování s dotazy stav zobrazíte všechny své informace o stavu, včetně události, podřízené stavu států a chybná hodnocení.

Obecné dotazy vrátit Neznámý stav entity, je možné, že úložišti stavu nemá úplné údaje o entity. Je také možné, že poddotazu k úložišti stavu nebyl úspěšný (třeba jste udělali chybu komunikace nebo omezena úložišti stavu). Zpracovat pomocí stavu dotazu entity. Pokud poddotazu k chybě přechodná, například problémům se sítí, opakujte tento zpracování dotazů. Ji může taky umožní další informace z obchodu stavu o proč, nebude vystaven entity.

Dotazy, které obsahují **HealthState** entit jsou:

- Seznam uzlů: vrátí seznam uzlů v obrázku (stránkování).
  - Rozhraní API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - Powershellu: Get-ServiceFabricNode
- Seznam aplikací: vrátí seznam aplikací v obrázku (stránkování).
  - Rozhraní API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - Powershellu: Get-ServiceFabricApplication
- Seznam služeb: vrátí seznam služeb v aplikaci (stránkování).
  - Rozhraní API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - Powershellu: Get-ServiceFabricService
- Seznam oddílů: vrátí seznam oddílů ve službě (stránkování).
  - Rozhraní API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - Powershellu: Get-ServiceFabricPartition
- Seznam otevřené: vrátí seznam repliky v oddílu (stránkování).
  - Rozhraní API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - Powershellu: Get-ServiceFabricReplica
- Seznam aplikací nasazena: vrátí seznam nasazeném aplikací na uzel.
  - Rozhraní API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - Powershellu: Get-ServiceFabricDeployedApplication
- Nasazení balíčku seznamem služeb: vrátí seznam služby balíčků v nasazení aplikace.
  - Rozhraní API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - Powershellu: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Některé dotazy vrátit stránkovaného výsledky. Vrácená tyto dotazy je seznam odvozeno z [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Pokud výsledky které nezapadají do zprávy, je vrácena pouze jednu stránku a ContinuationToken sledující kde výčet přerušili. Měli byste nadále zavolejte stejný dotaz a předejte tokenu pokračování z předchozího dotazu zobrazíte další výsledky.

### <a name="examples"></a>Příklady

Následující kód ní může vstoupit nefunkčních aplikací obrázku:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Následující rutinu získá podrobnosti aplikace struktury: / WordCount aplikace. Všimněte si, že stavu na upozornění.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Následující rutinu získá služeb se stavu upozornění:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Upgrade obrázku a aplikací
Během upgradu sledované obrázku a aplikace služeb struktury zkontroluje zajištění všechno zůstane funkční. Pokud entitu chybná jako vyhodnocovat pomocí zásady nakonfigurované stavu, platí upgrade zásady specifické pro upgrade k určení další akce. Upgrade může pozastavit umožňuje interakci uživatele (například řešení chybové podmínky nebo změna zásad) nebo ho může automaticky návrat na předchozí verzi dobrý.

Během upgradu *obrázku* můžete získat stavu upgradu obrázku. Stav upgradu obsahuje chybná hodnocení, které odkazují na to, co je chybná clusteru. Pokud upgrade bude vrácena kvůli stavem, stavu upgradu zapamatuje poslední chybná důvodů. Tyto informace umožňuje správcům zjistit, co je špatně po upgradu vrátit zpět nebo zastavit.

Podobně během upgradu *aplikace* všech chybná hodnocení jsou součástí stavu upgradu aplikace.

Následující příklad zobrazuje stavu upgradu aplikace pro změněné struktury: / WordCount aplikace. Sledovacích k chybě na jeden z jeho repliky. Upgrade je vrácení zpět, protože nejsou nerespektuje kontroly stavu.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Další informace o [upgradu služeb struktury aplikace](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Poradce při potížích s pomocí hodnocení stavu
Pokaždé, když je problém s clusteru nebo aplikace, podívejte se na stav obrázku nebo aplikace zjistit, proč není správné snažit. Nefunkční hodnocení poskytování údajů o co spouštěný aktuální nefunkčního stavu. Pokud potřebujete, můžete můžete přecházet na podrobnější chybná podřízených entit k identifikaci příčinou.

> [AZURE.NOTE] Nefunkční hodnocení zobrazit že první důvod, proč entitu Vyhodnocená každá její položka do aktuálního stavu. Doména může obsahovat více události, které aktivují tento stav, ale neprojevily v hodnocení. Pokud chcete získat víc informací, rozbalovat stavu entity k určení prodejce, chybná sestav v clusteru.

## <a name="next-steps"></a>Další kroky
[Použití sestav stavu systému řešit problémy s](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Přidání vlastních sestav stavu služby struktury](service-fabric-report-health.md)

[Jak sestavy a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Sledování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace služby struktury](service-fabric-application-upgrade.md)
