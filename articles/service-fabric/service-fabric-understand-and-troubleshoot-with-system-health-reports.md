<properties
   pageTitle="Poradce při potížích s sestav stavu systému | Microsoft Azure"
   description="Popisuje sestav stavu byl poslán struktury služby Azure součásti a jejich použití pro řešení potíží obrázku nebo problémy s aplikací."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Použití sestav stavu systému řešit problémy s

Azure služby struktury součásti Sestava mimo pole u všech entit clusteru. [Stav uložení](service-fabric-health-introduction.md#health-store) vytvoří a odstraní entity založený na systému sestavy. Také uspořádává je v hierarchii, který popisuje interakce entity.

> [AZURE.NOTE] O principech týkající se stavu, přečtěte si další informace v [modelu stavu služby struktury](service-fabric-health-introduction.md).

Sestavy stavu systému poskytují přehled o obrázku a funkcí aplikace a příznak problémy prostřednictvím stavu. U aplikací a služeb sestavy stavu systému zkontrolujte, že entity jsou implementovaná se chová správně ze služby struktury perspektivu. V sestavách nenabízejí všechny sledování stavu obchodní logiky služby nebo detekce zablokování procesů. Data stavu informace specifické pro jejich použití logických operátorů lze rozšířit uživatelské služby.

> [AZURE.NOTE] Sestavy stavu watchdogs jsou viditelné jen *Po* vytvoření součásti systému entity. Při odstranění entita úložišti stavu automaticky odstraní všechny sestavy stavu s ním spojené. Platí totéž i při vytvoření nové instance entitu (například se vytvoří novou instanci služby otevřené). Všechny sestavy přidružený k staré instanci jsou odstraněné a vyčistí z úložiště.

Součást systému sestavy jsou označeny zdroj, který začíná "**systém.**" Předpona. Watchdogs nelze použít stejnou předponu pro příslušného zdroje jsou odmítnuté sestavy s při použití neplatných parametrů.
Podívejme se na některé systém sestavy chcete porozumět tomu, jaký vyvolá a jak můžete vyřešit tak možné problémy, které představují.

> [AZURE.NOTE] Služba struktury i nadále sestavy můžete přidávat na předpoklady potřebné zlepšení viditelnosti do co je nového v obrázku a aplikace.

## <a name="cluster-system-health-reports"></a>Sestavy stavu systému obrázku
Automaticky se vytvoří entity stavu obrázku v úložišti stavu. Pokud všechno funguje správně, nemá sestavy systému.

### <a name="neighborhood-loss"></a>Ztráta prostředí
**System.Federation** hlásí chybu, když zjistí ztráty prostředí. Sestava je z jednotlivých uzlech a uzel ID je součástí název vlastnosti. Pokud jeden prostředí dojde ke ztrátě v celé struktury služby vyzvánět, můžete obvykle očekávat dvou událostí (obě strany sestavy mezera). Pokud další sousedství se ztratí, je potřeba další události.

Sestava Určuje časový limit globální zapůjčení jako time to live. Sestava je dotaz každé polovině TTL dobu podmínka zůstává aktivní. Událost se automaticky odeberou při vypršením jeho platnosti. Odeberte, když platnost chování zaručuje, že sestavy je vyčistí z obchodu stavu správně, i v případě vytváření sestav uzel dolů.

- **ID zdroje**: System.Federation
- **Vlastnost**: začíná **Okolní počítače** a obsahuje informace uzel
- **Další kroky**: Zjistěte, proč servery jsou ztraceny (například zkontrolovat komunikace mezi uzly clusteru).

## <a name="node-system-health-reports"></a>Sestavy stavu systému uzel
**System.FM**, což představuje službu převzetí správce, je oprávnění, která spravuje informace o uzlech. Každý uzel by měla být jedné sestavy z System.FM s jeho stavu. Při stavu uzel se odebere (viz [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)), budou odebrány uzel entity.

### <a name="node-updown"></a>Uzel up/Page down
System.FM sestavy jako OK při uzel spojí vyzvánění (je do začátků). Hlásí chybu, pokud na uzel vyplouvající vyzvánět (je dolů, buď pro upgrade nebo jednoduše protože se nezdařila). Hierarchie stavu vytvořeného pomocí úložišti stavu provádět akce se nasazeném entity se System.FM uzel sestavy. Považuje uzel virtuální nadřazené všechny nasazeném entity. Nasazeném entity na uzel jsou vystaven prostřednictvím dotazů, jestliže uzel jako až tak, že System.FM s stejné instanci jako instance přidružené entity. Když System.FM hlásí, že na uzel dolů nebo nerestartuje (nové instance), úložišti stavu nasazeném entity, které mohou být uloženy pouze na uzel dolů nebo na předchozí instance uzel automaticky vyčistí.

- **ID zdroje**: System.FM
- **Vlastnost**: stavu
- **Další kroky**: Pokud je uzel dolů k upgradu, ho měli přichází zpět po provedla upgrade. V tomto případě stavu by měl být po přechodu na jiný zpátky na OK. Pokud se vraťte na uzel nebo dojde k chybě, musí problém další vyšetřování.

Následující příklad ukazuje událost System.FM s stavu tohoto OK pro uzel:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Vypršení platnosti certifikátu
**System.FabricNode** sestavy upozornění, když certifikáty používané uzel se v blízkosti vypršení platnosti. Existují tři certifikátů na uzel: **Certificate_cluster** **Certificate_server**a **Certificate_default_client**. Aspoň dva týdny po vypršení sestavy stavu je v pořádku. Po vypršení do dvou týdnů je typ sestavy upozornění. Hodnota TTL těchto událostí je nekonečné a odeberou se při uzel ponechá clusteru.

- **ID zdroje**: System.FabricNode
- **Vlastnost**: začíná **certifikát** a obsahuje informace o typ certifikátu
- **Další kroky**: aktualizovat certifikáty, pokud jsou v blízkosti vypršení platnosti.

### <a name="load-capacity-violation"></a>Načtení porušení kapacity
Služby Vyrovnávání zatížení struktury sestavy upozornění zjistí porušení kapacita uzel.

 - **ID zdroje**: System.PLB
 - **Vlastnost**: začíná **kapacity**
 - **Další kroky**: Zkontrolujte podle metriky a zobrazit aktuální kapacita na uzel.

## <a name="application-system-health-reports"></a>Použití sestav stavu systému
**System.CM**, představující službu Správce clusteru je oprávnění, která spravuje informace o aplikaci.

### <a name="state"></a>Stav
System.CM hlásí OK aplikace se vytvořila nebo aktualizovala. Informuje úložišti stavu po odstranění aplikace tak, aby bylo možné odebrat z úložiště.

- **ID zdroje**: System.CM
- **Vlastnost**: stavu
- **Další kroky**: Pokud vytvořil aplikace by měl obsahovat sestava stavu Správce clusteru. Zkontrolujte, zda stavu aplikace zadáním dotazu (například rutiny Powershellu * *Get-ServiceFabricApplication ApplicationName *applicationName***).

Následující příklad ukazuje událost stavu na **struktury: / WordCount** aplikace:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Sestavy stavu systému služby
**System.FM**, což představuje službu převzetí správce, je autorita, která spravuje informace o službách.

### <a name="state"></a>Stav
System.FM hlásí OK po vytvoření službu. Slouží k odstranění entitu z obchodu stavu služby po odstranění.

- **ID zdroje**: System.FM
- **Vlastnost**: stavu

Následující příklad ukazuje událost stavu služby **struktury: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Narušení Neumístěná repliky
**System.PLB** sestavy upozornění, pokud nemůžu najít umístění pro jeden nebo více repliky služby. Po vypršení platnosti jsou odebrány sestavy.

- **ID zdroje**: System.FM
- **Vlastnost**: stavu
- **Další kroky**: Kontrola omezení služby a stav aktuální umístění.

Následující příklad ukazuje porušení pro službu nakonfigurovaný s 7 cílové replikami obrázku s 5 uzly:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Sestavy stavu systému oddíl
**System.FM**, což představuje službu převzetí správce, je oprávnění, která spravuje informací o oddílech služby.

### <a name="state"></a>Stav
System.FM hlásí OK až oddílu byl vytvořen a bude funkční. Slouží k odstranění entitu z obchodu stavu při odstranění oddílu.

Pokud oddíl je nižší než minimální otevřené počet, hlásí chybu. Pokud oddíl není nižší než minimální otevřené počet, ale je nižší než počet otevřené cílové, sestavy upozornění. Pokud oddíl je ztráty kvora, System.FM hlásí chybu.

Další důležité události zahrnout upozornění při změně konfigurace trvá déle než očekáváte a při sestavování trvá déle než obvykle. Očekávané doby pro vytvoření a konfigurace jsou konfigurovatelné podle scénáře služby. Sestavení trvá déle než pro službu s malou státu, například pokud službu má terabajt stavu, jako jsou databáze SQL.

- **ID zdroje**: System.FM
- **Vlastnost**: stavu
- **Další kroky**: Pokud stavu není OK, je možné, že ještě několik replik vytvořili, otevřít nebo propagované primární a sekundární správně. V mnoha případech je příčinou služby Chyba při provádění otevřít nebo změnit roli.

Následující příklad ukazuje oddíl pořádku:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Následující příklad ukazuje stavu oddíl, který je menší než počet otevřené cílové. Dalším krokem je uslyšíte popis oddílu, který ukazuje, jak je nastavený: **MinReplicaSetSize** dvě a **TargetReplicaSetSize** je sedm. Získání počtu uzlů v clusteru: pět. Takže v tomto případě dvě repliky nelze umístit.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Narušení omezení otevřené
**System.PLB** sestavy upozornění, pokud zjistí narušení omezení otevřené a nelze umístit kopie oddílu.

- **ID zdroje**: System.PLB
- **Vlastnost**: začíná **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Sestavy stavu systému otevřené
**System.RA**, představující součásti agent konfigurace je oprávnění pro stav otevřené.

### <a name="state"></a>Stav
**System.RA** hlásí OK po vytvoření otevřené.

- **ID zdroje**: System.RA
- **Vlastnost**: stavu

Následující příklad ukazuje správný otevřené:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Otevřít stav otevřené
Popis této sestavy stavu obsahuje počáteční čas (UTC), při volání rozhraní API.

**System.RA** ohlásí upozornění, že pokud otevřít otevřené trvá déle než nakonfigurované období (výchozí: 30 minut). Pokud rozhraní API ovlivňuje dostupnost služby, sestavy vydán rychleji (na Konfigurovat interval, výchozí hodnota je 30 sekund). Čas měří obsahuje dobu nutnou k replikace otevřít a služby otevřít. Vlastnost se změní na OK, pokud dokončí otevřít.

- **ID zdroje**: System.RA
- **Vlastnost**: **ReplicaOpenStatus**
- **Další kroky**: Pokud stavu není OK, zjistěte, proč trvá déle než obvykle otevřít otevřené.

### <a name="slow-service-api-call"></a>Volání rozhraní API pomalé služby
Sestavu **System.RAP** a **System.Replicator** upozornění-li zavolat na kód služby uživatele trvá déle než nakonfigurované čas. Po dokončení hovoru je zrušeno upozornění.

- **ID zdroje**: System.RAP nebo System.Replicator
- **Vlastnost**: název pomalé rozhraní API. Popis najdete další informace o době, kdy byla rozhraní API na zjištění stavu úkolů.
- **Další kroky**: Zjistěte, proč trvá déle než obvykle hovor.

Následující příklad ukazuje oddíl v ztráty kvora a vyšetřování kroky Hotovo a zjistit, proč. Jedna z replik tak, abyste se jeho stav obsahuje stavu upozornění. Zobrazuje operace služby trvá déle než obvykle události vykázaného System.RAP. Po přijetí tyto informace, dalším krokem je a podívejte se na kód služby prošetřit tam. Pro tento případ **RunAsync** provádění stavová služba výjimku neošetřené. Replik jsou recyklace, takže vidíte nemusí libovolnou replikou ve stavu upozornění. Opakovat začíná stavu a vyhledejte rozdíly v otevřené ID. V některých případech pokusy vám může vodítko.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Při spuštění vadná aplikace v části ladění diagnostických událostí windows zobrazovat výjimka ze RunAsync:

![Visual Studio 2015 diagnostických událostí: RunAsync Chyba instalace v struktury: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostických událostí: RunAsync Chyba instalace v **struktury: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Zaneprázdněn úplné
**System.Replicator** sestavy upozornění, pokud je už plná zaneprázdněn. Na hlavní to obvykle způsobeno tím minimálně jednu vedlejší repliky jsou pomalé k potvrzení operace. V sekundární to obvykle se stane, když služba je pomalá provádět způsobů. Upozornění zaškrtnuté, když už je už plná fronty.

- **ID zdroje**: System.Replicator
- **Vlastnost**: **PrimaryReplicationQueueStatus** nebo **SecondaryReplicationQueueStatus**v závislosti na otevřené rolí

### <a name="slow-naming-operations"></a>Pomalé pojmenování operace

Když operace Naming trvá déle než přijatelné **System.NamingService** sestavy stavu na primární otevřené. Příklady operací Naming jsou [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) nebo [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Další metody najdete v části FabricClient, například v části [metody správy služby](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) nebo [metody správy vlastnost](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Služba Naming překládá názvy služby na místo v clusteru a uživatelům umožňuje spravovat názvy služeb a vlastnosti. Je služba struktury oddíly zachován služby. Jeden z oddílů představuje vlastník autorita obsahuje metadata služby struktury názvy a services. Názvy služeb struktury jsou namapované do různých oddílů označované jako vlastníka oddíly, tedy extensible službu. Přečtěte si další informace o [službě Naming](service-fabric-architecture.md).

Pokud operace Naming trvá déle než obvykle, operace označena se sestavou upozornění na *primární otevřené Naming služby oddílu, který slouží operaci*. Pokud úspěšném upozornění zaškrtnuté. Pokud dokončení operace s chybou sestava stavu obsahuje podrobnosti o chybě.

- **ID zdroje**: System.NamingService
- **Vlastnost**: začíná předpona **Duration_** a identifikuje operaci pomalé a struktury služby název, na kterém se používá operaci. Například pokud vytvoření služby na název struktury: / aplikace/Moje_služba trvá moc dlouho, že je vlastnost Duration_AOCreateService.fabric:/MyApp/MyService. AO odkazy na roli oddílu Naming pro tento název a provoz.
- **Další kroky**: Proč se Naming nezdaří zaškrtnutí. Každá operace může obsahovat jinou příčin. Odstraňte například služba může zarazí na uzel, protože hostitele aplikace pořád k selhání uzlu kvůli chybu uživatele v kódu služby.

Následující příklad zobrazuje operaci služby vytvořit. Operace překročila nakonfigurované doby trvání. AO opakování a odešle práce na Ne. Ne dokončení poslední operace s časový limit. V tomto případě je stejné replikační primární AO a žádné role.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>Sestavy stavu systému DeployedApplication
**System.Hosting** je autorita na nasazeném entity.

### <a name="activation"></a>Aktivace
System.Hosting hlásí OK při aplikace úspěšně aktivovaný na uzel. V opačném hlásí chybu.

- **ID zdroje**: System.Hosting
- **Vlastnost**: aktivace, včetně zavedení verze
- **Další kroky**:-li aplikaci chybná, zjistěte, proč se nepodařilo aktivaci.

Následující příklad ukazuje úspěšnou aktivaci:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Ke stažení
**System.Hosting** hlásí chybu, pokud se nezdaří Stáhnout balíček aplikace.

- **ID zdroje**: System.Hosting
- **Vlastnost**: * *Stáhnout:*RolloutVersion***
- **Další kroky**: Zjistěte, proč se nepodařilo stahování na uzel.

## <a name="deployedservicepackage-system-health-reports"></a>Sestavy stavu systému DeployedServicePackage
**System.Hosting** je autorita na nasazeném entity.

### <a name="service-package-activation"></a>Aktivace balíčku služby
System.Hosting sestavy jako OK pokud aktivace balíčku služby na uzel proběhne úspěšně. V opačném hlásí chybu.

- **ID zdroje**: System.Hosting
- **Vlastnost**: Aktivace
- **Další kroky**: Zjistěte, proč aktivace se nezdařila.

### <a name="code-package-activation"></a>Aktivace balíčku kód
**System.Hosting** sestavy jako OK pro každý kód balíček Pokud aktivaci proběhne úspěšně. Pokud se aktivace nezdaří, sestavy upozornění konfigurace. Pokud **CodePackage** nepodaří aktivace nebo ukončí zobrazí se chybová zpráva větší než nakonfigurované **CodePackageHealthErrorThreshold**, který je hostitelem hlásí chybu. Balíček služby obsahuje více balíčků kód, formuláře s vyúčtováním aktivace zpráva pro každou z nich.

- **ID zdroje**: System.Hosting
- **Vlastnost**: využívá předponu **CodePackageActivation** a obsahují název balíčku kód a vstupní bod jako * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/vstupní bod* ** (například **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Registrace typ služby
**System.Hosting** sestavy jako OK pokud typ služby registrované úspěšně. Ho hlásí chybu, pokud registrace nebyla provedena v čase (jak nakonfigurovat pomocí **ServiceTypeRegistrationTimeout**). Pokud je typu služby registrace ze skupiny, totiž zavřel modul runtime. Hostingu sestavy upozornění.

- **ID zdroje**: System.Hosting
- **Vlastnost**: využívá tuto předponu **ServiceTypeRegistration** a obsahují název typu služby (například **ServiceTypeRegistration:FileStoreServiceType**)

Následující příklad ukazuje balíček správný nasazení služeb:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Ke stažení
**System.Hosting** hlásí chybu, pokud se nezdaří stažení balíčku služby.

- **ID zdroje**: System.Hosting
- **Vlastnost**: * *Stáhnout:*RolloutVersion***
- **Další kroky**: Zjistěte, proč se nepodařilo stahování na uzel.

### <a name="upgrade-validation"></a>Ověření upgradu
**System.Hosting** hlásí chybu, pokud neúspěšné ověření během upgradu nebo upgrade selhání na uzel.

- **ID zdroje**: System.Hosting
- **Vlastnost**: používá předponu **FabricUpgradeValidation** a obsahuje verze upgrade
- **Popis**: odkazuje na došlo k chybě

## <a name="next-steps"></a>Další kroky
[Zobrazení sestav stavu služby struktury](service-fabric-view-entities-aggregated-health.md)

[Jak sestavy a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Sledování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace služby struktury](service-fabric-application-upgrade.md)
