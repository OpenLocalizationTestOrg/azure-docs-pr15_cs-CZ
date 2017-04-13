<properties
   pageTitle="Řešení potíží s aplikací upgrady | Microsoft Azure"
   description="Tento článek popisuje některé běžné problémy kolem upgrade aplikace služby struktury a jejich řešení."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Řešení potíží s aplikací upgrady

Tento článek popisuje některé běžné problémy kolem upgrade aplikace struktury služby Azure a jejich řešení.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Poradce při potížích s upgrade aplikace, která selhala

Při upgradu se nezdaří, obsahuje výstup příkazu **Get-ServiceFabricApplicationUpgrade** dodatečné informace pro ladění chybu.  Následující seznam určuje, jak můžete použít další informace:

1. Určete typ chyby.
2. Určení důvod chyby.
3. Vyčlenění jednu nebo více složek selhání pro další vyšetřování.

Tyto informace jsou k dispozici při služby struktury zjistí selhání bez ohledu na to, zda je **FailureAction** vrátit zpět nebo pozastavit upgradu.

### <a name="identify-the-failure-type"></a>Určit typ chyby

V seznamu výstup **Get-ServiceFabricApplicationUpgrade**identifikuje **FailureTimestampUtc** časovým razítkem (UTC) niž zjistil chybu upgradu služeb struktury a **FailureAction** spuštěná. **FailureReason** identifikuje nějaká tři možné příčiny uceleném chyby:

1. UpgradeDomainTimeout - označuje, že určité upgradu domény dlouhou dobu dokončení a **UpgradeDomainTimeout** vypršela.
2. OverallUpgradeTimeout - označuje, že celkové upgrade dlouhou dobu dokončení a **UpgradeTimeout** vypršela.
3. HealthCheck - označuje, že po upgradu doménu aktualizace aplikace zůstala chybná podle zásady zadaný stavu a **HealthCheckRetryTimeout** , kterým vypršela platnost.

Tyto položky pouze objeví v výstup při upgradu dochází k chybě a spustí vrácení zpět. Další informace se zobrazí v závislosti na typu chyby.

### <a name="investigate-upgrade-timeouts"></a>Prozkoumejte upgradu časové limity

Upgradujte časový limit, které problémy se službou dostupnost nejčastěji příčinou selhání. Výstup níže je typické inovací místo, kam služby repliky nebo instance nepodařilo spustit v nové verzi kódu. Pole **UpgradeDomainProgressAtFailure** zaznamenává snímek libovolné pole Čekání na upgrade práce v době selhání.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

V tomto příkladu upgradu Nepodařilo se upgradu doménou *MYUD1* a bylo zablokované dva oddíly (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* a *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Oddíly byly zasekne, protože modul runtime se nepodařilo včasné cílových uzlů *Uzel1* a *Uzel4*primární repliky (*WaitForPrimaryPlacement*).

Příkaz **Get-ServiceFabricNode** mohou sloužit k ověření, že tyto dva uzly skutečně upgradu domény *MYUD1*. *UpgradePhase* říká *PostUpgradeSafetyCheck*, což znamená probíhající tento bezpečnostní kontroly po dokončení všech uzlů v upgradu domény upgradu. Tyto informace odkazy na potenciální problémy s novou verzi kód aplikace. Nejčastější problémy jsou chyby služby otevřít nebo podpory k cestám primární kód.

*UpgradePhase* *PreUpgradeSafetyCheck* znamená, že došlo k problémy Příprava upgradu domény před bylo provedeno. Nejčastější problémy jsou v tomto případě chyby služby v zavřít nebo snížení úrovně z primární kód cest.

Aktuální **UpgradeState** je *RollingBackCompleted*, takže původní upgrade musí byly provedeny s vrácení zpět **FailureAction**, která automaticky vrátit zpět upgrade po selhání. Pokud s ruční **FailureAction**proběhlo původní upgradu, pak upgrade by místo toho být v pozastaveném stavu umožňuje živé ladění rohu aplikace.

### <a name="investigate-health-check-failures"></a>Prozkoumejte stavu Kontrola chyb

Stav Kontrola chyb můžete spouštěný kliknutím různé problémy, které může dojít po dokončení všech uzlů v doméně upgradu upgradu a prochází všechny kontroly zabezpečení. Výstup níže je typické upgradu selhání kvůli kontroly selhalo stavu. Pole **UnhealthyEvaluations** zaznamenává snímek kontroly stavu, které se nepovedlo při upgradu podle určité [zásady stavu](service-fabric-health-introduction.md).

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Zjišťování chyb Kontrola stavu nejdřív vyžaduje pochopení modelu stavu služby struktury. Ale i bez těchto hloubkovou pochopení, můžete vidíme, že jsou dvě služby chybná: *struktury: / DemoApp/Svc3* a *struktury: / DemoApp/Svc2*, spolu s sestav stavu chyby ("InjectedFault" v tomto případě). V tomto příkladu mimo čtyř služby jsou chybná, což je pod výchozí cíl 0 % chybná (*MaxPercentUnhealthyServices*).

Upgrade se pozastaví po selhání zadáním **FailureAction** příručky při spuštění upgradu. Tohoto režimu umožňuje zkoumat systému živé ve stavu selhání před provedením další akce.

### <a name="recover-from-a-suspended-upgrade"></a>Obnovení ze pozastavené upgrade

Vrácení **FailureAction**je žádné obnovení potřeby od upgradu automaticky vrátí po selhalo. S ruční **FailureAction**existuje několik možnosti obnovení:

1. Ruční spuštění vrácení zpět
2. Pokračujte zbytek upgrade ručně
3. Pokračujte v upgradu sledované

Příkaz **Spustit ServiceFabricApplicationRollback** lze kdykoli spustit vracení aplikace. Jakmile příkazu vrátí úspěšně, žádost o odvolání transakce registrované v systému a spustí krátce poté.

Můžete použít příkaz **Životopisu ServiceFabricApplicationUpgrade** ručně, pokračujte zbytek upgrade jedné upgradu domény v čase. V tomto režimu pouze bezpečnosti ověřování systému. Žádné další kontroly stavu provádí. Tento příkaz můžete použít jenom v případě *UpgradeState* zobrazuje *RollingForwardPending*, což znamená, že aktuální upgradu domény dokončení upgradu ale další ještě nezačala (na zjištění stavu úkolů).

Příkaz **Update ServiceFabricApplicationUpgrade** mohou sloužit k životopisu sledované upgrade obou zabezpečení a stavu zkontroluje provádí.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Upgrade pokračuje z upgradu domény které poslední pozastavené a použití stejné upgrade parametry a zásady stavu jako před. V případě potřeby všem upgradu parametry a zásady stavu zobrazují v předchozím výstup lze změnit v jednom příkazu po upgradu návratu. V tomto příkladu upgrade pokračuje v režimu sledované s parametry a zásady stavu beze změny.

## <a name="further-troubleshooting"></a>Další řešení potíží

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Služba struktury není následující zásady zadaný stavu

Možná příčina 1:

Struktury služeb překládá všechny procent na skutečného počtu entity (například repliky, oddíly a služby) pro vyhodnocení stavu a vždy zaokrouhluje nahoru na celé entity. Například pokud maximální _MaxPercentUnhealthyReplicasPerPartition_ je 21 % a jsou pět kopie, pak služby struktury umožňuje až dva chybná repliky (Tento is,'Math.Ceiling (5\*0.21)). Proto zásady stavu stanovit příslušným způsobem.

Možná příčina 2:

Zásady stavu byla vyplněná z hlediska procent celkové služeb a instance nejsou specifické služby. Příklad před upgrade, pokud má čtyři aplikace služby instance A, B, C a D, kde je služba D nefunkční, ale s malé vliv na aplikace. Chcete ignorovat známé chybná služba D během upgradu a nastavte parametr *MaxPercentUnhealthyServices* 25 %, za předpokladu, že jenom A, B a C musí být správný.

Však během upgradu, D se může stát správný během C změní chybná. Upgrade by pořád úspěšné, protože jsou chybná pouze 25 % služeb. Však může vést k neočekávané chyby kvůli C je neočekávaně chybná místo D. V tomto případě je možné modelovat D jako typu jinou službu od A, B a C. Protože zásady stavu byla vyplněná za typ služby, mezní hodnoty různých chybná procento lze použít pro různé služby. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Můžu nezadali zásad stavu pro upgrade aplikace, ale upgrade stále nedaří pro některé časové limity relací, nikdy určených

Nejsou-li zásady stavu upgradu žádost, se odeberou z *ApplicationManifest.xml* aktuální verze aplikace. Například pokud upgradujete z verze 1.0 verze 2.0, zásady použití stavu podle verze 1.0 X aplikace používají. Pokud chcete zásadu různých stavu pro upgrade, zásady musí být zadán jako součást rozhraní API volání upgrade aplikace. Zásady zadaný jako součást rozhraní API hovor platí pouze během upgradu. Po dokončení upgradu se používají zásady podle *ApplicationManifest.xml* .

### <a name="incorrect-time-outs-are-specified"></a>Byla vyplněná nesprávné časové limity relací

Můžete mít zajímalo o situace, kdy časové limity relací nastavená pro používání nekonzistentně. Například může mít *UpgradeTimeout* , která je menší než *UpgradeDomainTimeout*. Odpověď se, že je vrácena chyba. Chyby se vracejí *UpgradeDomainTimeout* je menší než součet *HealthCheckWaitDuration* a *HealthCheckRetryTimeout*, zda je-li *UpgradeDomainTimeout* menší než součet *HealthCheckWaitDuration* a *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Moje aktualizace trvá příliš dlouho.

Čas pro upgrade dokončete závisí na kontroly stavu a časové limity relací zadali. Limit relace a kontroly stavu, závisí na doby kopírování, nasazení a stabilizaci aplikace. Je příliš agresivní s časové limity relací může znamenat více selhalo upgradů, proto doporučujeme konzervativně počínaje delší časové limity relací.

Tady je rychlý Toto téma připomenout na interakce časové limity relací s upgradem časy:

Aktualizuje pro upgrade domény nelze dokončit rychleji než *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Chyba při upgradu nelze provést rychleji než *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Upgrade času pro upgrade doménu je omezen *UpgradeDomainTimeout*.  Pokud *HealthCheckRetryTimeout* *HealthCheckStableDuration* jsou i nenulového a stavu aplikace pořád přechodu na jiný sebou poslalo, pak upgrade nakonec vyprší její časový limit na *UpgradeDomainTimeout*. *UpgradeDomainTimeout* počítá po upgradu pro aktuální upgradu doménu, nebude zahájen.

## <a name="next-steps"></a>Další kroky

[Upgrade vaší aplikace pomocí aplikace Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí aplikace Visual Studio.

[Upgrade vaší aplikace pomocí Powershellu](service-fabric-application-upgrade-tutorial-powershell.md) vás provede aplikace upgradovat pomocí prostředí PowerShell.

Řídit, jak aplikace aktualizuje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Proveďte upgrade aplikace kompatibilní naučit používat [Serializaci dat](service-fabric-application-upgrade-data-serialization.md).

Naučte se používat rozšířené funkce při upgradu aplikace odkazem na [Témata, Upřesnit](service-fabric-application-upgrade-advanced.md).

Řešení běžných problémů v aplikaci inovací odkazem kroků v tématu [Řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
 
