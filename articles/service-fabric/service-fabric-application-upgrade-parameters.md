
<properties
   pageTitle="Upgrade aplikace: upgrade parametry | Microsoft Azure"
   description="Popisuje parametry související s upgradu služeb struktury aplikace, včetně kontroly stavu provádět a zásady automaticky vrátit upgradu."
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



# <a name="application-upgrade-parameters"></a>Upgrade parametry aplikace

Tento článek popisuje různé parametry, které se vztahují během upgradu aplikace struktury služby Azure. Parametry zahrnovat jméno a verzi aplikace. Jsou knoflíky, kterými se řídí časové limity relací a kontroly stavu, které jsou použity během upgradu a určují zásad, které má být použito při upgradu se nezdaří.


<br>

| Parametr | Popis |
| --- | --- |
| ApplicationName | Název aplikace, která probíhá upgrade. Příklady: struktury: / VisualObjects, struktury: / ClusterMonitor  |
| TargetApplicationTypeVersion | Typ verzi aplikace, aby upgradu cílů. |
| FailureAction | Akci, kterou službu struktury při upgradu se nezdaří. Aplikace může vrátit zpět na verzi před aktualizace (vrácení zpět) nebo upgrade může přerušili na aktuální upgradu domény. V takovém případě se režim upgradu také změní na ručně. Povolené hodnoty jsou vrácení zpět a ručně. |
| HealthCheckWaitDurationSec | Čas (v sekundách) až po dokončení upgradu na upgrade domény před služby struktury vyhodnotí stavu aplikace. Doba trvání mohou být považovány čas, kdy by měla běžet aplikace předtím, než je možné považovat za správný. Pokud kontrola stavu projde, procesu upgradu pokračuje na další upgradu doménu.  Pokud kontrola stavu selže, struktury služby čeká interval (UpgradeHealthCheckInterval) akci Kontrola stavu znovu do dosažení HealthCheckRetryTimeout. Výchozí a doporučená hodnota je 0 sekund. |
| HealthCheckRetryTimeoutSec | Doba trvání (v sekundách), které služby struktury budou dál problémy provádět stavu vyhodnocení před deklarace upgradu, protože se nezdařila. Výchozí hodnota je 600 sekund. Doba trvání spustí po dosažení HealthCheckWaitDuration. V tomto HealthCheckRetryTimeout služby struktury provádět více kontroly stavu zdraví aplikace. Výchozí hodnota je 10 minut a by měl být přizpůsobený řádně podporovat aplikace. |
| HealthCheckStableDurationSec | Doba trvání (v sekundách) pro ověření, že aplikace je stabilní před přesunutí do další upgradu domény nebo dokončení upgradu. Doba trvání čekání se používá k uchování nerozpoznaná zdraví vpravo po provedení kontroly stavu. Výchozí hodnota je 120 sekundy a by měl být přizpůsobený řádně podporovat aplikace. |
| UpgradeDomainTimeoutSec | Maximální doba (v sekundách) pro upgrade jedné upgradu domény. Pokud je tento časový limit, upgrade zastaví a vytvářejí podle nastavení UpgradeFailureAction. Výchozí hodnota je nikdy (nekonečné) a aplikace by měl být přizpůsobený řádně podporovat. |
| UpgradeTimeout | Časový limit (v sekundách), která platí pro celou upgrade. Pokud je tento časový limit, upgrade tabulátorů a UpgradeFailureAction se při spuštění aktivují. Výchozí hodnota je nikdy (nekonečné) a aplikace by měl být přizpůsobený řádně podporovat. |
| UpgradeHealthCheckInterval | Četnost, že je zaškrtnuté políčko stavu. Tento parametr není zadán v části ClusterManager _clusteru_ _manifest_a není zadaný jako součást rutinu upgradu. Výchozí hodnota je 60 sekund.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Hodnocení stavu služby během upgradu aplikace

<br>
Kritéria hodnocení stavu jsou volitelné. V případě stavu kritéria hodnocení nejsou při spuštění upgradu, struktury služby používá zásady použití stavu podle ApplicationManifest.xml instance aplikace.


<br>

| Parametr | Popis |
| --- | --- |
| ConsiderWarningAsError | Výchozí hodnota je False. Považovat za události stavu upozornění pro aplikaci chyby při vyhodnocování stavu aplikace během upgradu. Ve výchozím nastavení služby struktury vyhodnotí upozornění stavu události neúspěchům (chyby), takže upgrade můžete pokračovat, i když jsou upozornění.   |
| MaxPercentUnhealthyDeployedApplications | Výchozí a doporučená hodnota je 0. Určuje maximální počet nasazeném aplikace (viz [část stav](service-fabric-health-introduction.md)), které můžete předcházet chybná aplikace je považován za chybná a přestane upgradu. Tento parametr určuje funkčnosti aplikací na uzel a pomáhají zjistit problémy během upgradu. Kopie aplikace obvykle potřebujete Vyrovnávání zatížení jiných uzel, které umožňuje aplikaci zobrazit správný, takže upgrade pokračovat. Zadáním striktně MaxPercentUnhealthyDeployedApplications stavu služby struktury rychle zjistit potíže s balíčku aplikace a nápovědu můžete plodiny selhání rychle upgradu. |
| MaxPercentUnhealthyServices | Výchozí a doporučená hodnota je 0. Zadejte maximální počet služeb do instance aplikace, kterou může být chybná než aplikace je považován za chybná a přestane upgradu. |
| MaxPercentUnhealthyPartitionsPerService | Výchozí a doporučená hodnota je 0. Zadejte maximální počet oddílů do služby, kterou může být chybná než službu se považuje za nebezpečné. |
| MaxPercentUnhealthyReplicasPerPartition | Výchozí a doporučená hodnota je 0. Zadejte maximální počet repliky v oddílu, který může být chybná před oddílu se považuje za nebezpečné. |
| UpgradeReplicaSetCheckTimeout | **Příslušnosti služby**– v rámci jedné upgradu domény služby struktury se snaží zajistit, že je k dispozici další instance služby. Pokud počet instancí cílové se více než jedno služby struktury čeká více než jedna instance, aby byl k dispozici až maximálního limitu. Tento časový limit je určen pomocí vlastnost UpgradeReplicaSetCheckTimeout. Pokud vypršení časového limitu služby struktury vytvářejí upgradu, bez ohledu na počet instancí služby. Pokud počet instancí cílové, struktury služby nečeká a okamžitě vytvářejí s upgradem. **Stavová služba**– v rámci jedné upgradu domény služby struktury se snaží zajistit, aby sadu otevřené hlasování. Služba struktury čeká hlasování je k dispozici až maximálního limitu (nastavil vlastnost UpgradeReplicaSetCheckTimeout). Pokud vypršení časového limitu služby struktury vytvářejí upgradu, bez ohledu na to kvora. Toto nastavení je sada jako nikdy (nekonečno) při obnovení nebo 900 sekund při vracení. |
| ForceRestart | Při aktualizaci dat balíčku a konfigurace bez aktualizace kód služby, pouze pokud je nastavena vlastnost ForceRestart restartování služby true (pravda). Po dokončení aktualizace služby struktury s upozorněním službu, nového konfiguraci nebo balíček dat je dostupný. Služba není zodpovědný za použití změn. V případě potřeby můžete restartovat službu samotné. |



<br>
<br>
Kritéria MaxPercentUnhealthyServices MaxPercentUnhealthyPartitionsPerService a MaxPercentUnhealthyReplicasPerPartition můžete zadat na typ služby pro instance aplikace. Nastavení těchto parametry za služby umožňuje aplikace má být různé služby typy zásadám různých hodnocení. Typ služby příslušnosti brány můžete mít například MaxPercentUnhealthyPartitionsPerService, které se liší od typu stavového stroje služby pro konkrétní instanci aplikace.

## <a name="next-steps"></a>Další kroky

[Upgrade vaší aplikace pomocí aplikace Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí aplikace Visual Studio.

[Upgrade vaší aplikace pomocí Powershellu](service-fabric-application-upgrade-tutorial-powershell.md) vás provede aplikace upgradovat pomocí prostředí PowerShell.

Proveďte upgrade aplikace kompatibilní naučit používat [Serializaci dat](service-fabric-application-upgrade-data-serialization.md).

Naučte se používat rozšířené funkce při upgradu aplikace odkazem na [Témata, Upřesnit](service-fabric-application-upgrade-advanced.md).

Řešení běžných problémů v aplikaci inovací odkazem kroků v tématu [Řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
 
