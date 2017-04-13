<properties
   pageTitle="Životního cyklu aplikace v služby struktury | Microsoft Azure"
   description="Popisuje vývoj, nasazení, testování, upgradu, Správa a odebrání aplikací služby struktury."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Služba struktury aplikace cyklus
Jak se dalších platformách aplikace na struktury služby Azure obvykle prochází následující fáze: návrh, vývoj, testování, nasazení, upgrade, údržbu a odstranění. Služby struktury poskytuje prvotřídní podporu pro úplné verze aplikace cyklus cloudu aplikací, od vývoje přes nasazení, každodenní správu a údržba případné vyřadit z. Model služby umožňuje několik různých rolí nezávisle na sobě účastnit životním cyklu aplikace. Tento článek obsahuje přehled rozhraní API a jak je používají různé role v celém fází životního cyklu služby struktury aplikace.

## <a name="service-model-roles"></a>Role modelu služby
Role modelu služby jsou:

- **Služba developer**: vyvíjí moduly a obecný služeb, které lze znovu předpokládaného a použít ve více aplikacích stejného typu nebo různé typy. Například služba fronty lze použít při vytváření aplikace ticketing (technickou podporu) nebo aplikaci obchodování (nákupní košík).

- **Vývojář aplikace**: vytvoří aplikace integrací kolekce služeb, které splňují určité specifických požadavků nebo scénáře. "Například web na obchodování může JSON příslušnosti předřazený službu integrovat," "Přihlašovacích údajů aukce stavovou" a "Fronty stavová služba" k vytvoření auctioning řešení.

- **Správce aplikace**: rozhoduje o konfigurace aplikace (vyplnění parametry šablony konfigurace), nasazení (mapování dostupné zdroje) a kvality služby. Například správce aplikace rozhoduje o národní prostředí (v angličtině pro Českou republiku) nebo japonština pro Japonsko, například aplikace. Různé nasazení aplikace může mít různým nastavením.

- **Operátor**: nasadí aplikace založené na konfiguraci aplikace a požadavky nastavil správce aplikací. Například operátor předpisy a nasadí aplikace a zaručuje, že je spuštěná v Azure. Operátory sledovat informace o stavu a výkonu aplikace a spravovat fyzické infrastruktury podle potřeby.


## <a name="develop"></a>Můžete vyvíjet
1. *Karta Vývojář v služby* vyvíjí různé typy služeb pomocí programového modelu [Spolehlivé objekty actor](service-fabric-reliable-actors-introduction.md) nebo [Spolehlivé služby](service-fabric-reliable-services-introduction.md) .
2. *Služba developer* deklarativně Popisujte typů vyvinutý služby v seznamu soubor služby obsahující jeden nebo více balíčků kód, konfigurace a dat
3. *Vývojář aplikace* vytvoří aplikace pomocí služby různých typů.
4. *Vývojář aplikace* deklarativně popisuje typ aplikace v manifestu aplikace odkazem manifesty služby základní služeb a řádně podporovat přepsání a parametrizování různých konfigurace a nastavení nasazení služby základní.

Příklady najdete v článku [Začínáme s spolehlivé účastníky](service-fabric-reliable-actors-get-started.md) a [začít pracovat s spolehlivé služby](service-fabric-reliable-services-quick-start.md) .

## <a name="deploy"></a>Nasazení
1. *Správce aplikace* tailors typ aplikace pro určitou aplikaci nasadit služby struktury clusteru tím, že zadáte příslušné parametry **ApplicationType** element v manifestu aplikace.

2. *Operátor* , odešlou se balíček aplikace úložišti obrázek clusteru pomocí [metody **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) nebo [ **Kopírovat ServiceFabricApplicationPackage** rutiny](https://msdn.microsoft.com/library/azure/mt125905.aspx). Balíček aplikace obsahuje manifest aplikace a kolekce balíčků služeb. Služba struktury nasadí aplikací z balíčku aplikace uložených v úložišti obrázek, který může být úložiště objektů blob Azure nebo systémová služba struktury služby.

3. *Operátor* pak ustanovení typ aplikace v cílové clusteru z balíčku nahraný aplikace pomocí [metody **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register ServiceFabricApplicationType** rutina](https://msdn.microsoft.com/library/azure/mt125958.aspx)nebo [operace ZBÝVAJÍCÍ **poskytování aplikace** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Po zajištění služby aplikace, *operátor* začíná parametry zadané *Správce aplikací* pomocí [metody **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), [rutinu **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)nebo [ **Vytvořit aplikaci** ZBÝVAJÍCÍ operace](https://msdn.microsoft.com/library/azure/dn707676.aspx)aplikace.

5. Po nasazení aplikace *operátor* používá [ **CreateServiceAsync** metody](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), [rutinu **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)nebo [operace **Vytvoření služba** REST](https://msdn.microsoft.com/library/azure/dn707657.aspx) k vytvoření nové instance služby aplikace založené na typech dostupnými službami.

6. Aplikace teď běží služba struktury obrázku.

Příklady naleznete v tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Po zavedení místní rozvoj obrázku nebo obrázku test, *služby vývojář* spustí scénář testování předdefinované převzetí třídy [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) a [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) nebo [ **Vyvolat ServiceFabricFailoverTestScenario** rutiny](https://msdn.microsoft.com/library/azure/mt644783.aspx). Scénář testování převzetí zadané služby projde důležité přechody a převzetí služeb při selhání a ujistěte se, že je stále dostupné a pracovní.

2. *Karta Vývojář v služby* spustí scénář testování předdefinované chaos pomocí třídy [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) a [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) nebo [ **Vyvolat ServiceFabricChaosTestScenario** rutiny](https://msdn.microsoft.com/library/azure/mt644774.aspx). Scénář testování chaos náhodně indukuje více uzel balíček kódu a chyby otevřené do clusteru.

3. *Služby vývojář* [komunikace služby služby testů](service-fabric-testability-scenarios-service-communication.md) vytváření test scénáře, které přesunout primární repliky kolem clusteru.

Další informace najdete v článku [Úvod ke službě Analysis poruch](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Upgrade
1. *Služba vývojář* aktualizuje služby základní instance aplikace a/nebo opravy chyb a poskytuje novou verzi manifest služby.

2. *Vývojář aplikace* přepíše parameterizes nastavení konfigurace a nasazení služeb konzistentní a poskytuje novou verzi manifest aplikace. Vývojář aplikace pak zahrne nové verze manifesty služby do aplikace a poskytuje novou verzi aplikace typu balíčku aktualizované aplikace.

3. *Správce aplikace* formou novou verzi typ aplikace do cílové aplikace aktualizace příslušné parametry.

5. *Operátor* odešlou se obchodu obrázek clusteru pomocí [metody **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) nebo [ **Kopírovat ServiceFabricApplicationPackage** rutina](https://msdn.microsoft.com/library/azure/mt125905.aspx)balíčku aktualizované aplikace. Balíček aplikace obsahuje manifest aplikace a kolekce balíčků služeb.

6. *Operátor* ustanovení novou verzi aplikace v cílové clusteru pomocí [metody **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register ServiceFabricApplicationType** rutina](https://msdn.microsoft.com/library/azure/mt125958.aspx)nebo [operace ZBÝVAJÍCÍ **poskytování aplikace** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. *Operátor* aktualizuje cílové aplikace na novou verzi pomocí [metody **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), [ **Start ServiceFabricApplicationUpgrade** rutina](https://msdn.microsoft.com/library/azure/mt125975.aspx)nebo [ **Upgrade aplikace** ZBÝVAJÍCÍ operace](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. *Operátor* kontroly průběhu upgradu pomocí [metody **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), [rutiny **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)nebo [operace ZBÝVAJÍCÍ **Získat průběh Upgrade aplikace** ](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. V případě potřeby *operátor* upraví a znovu parametry aktuální upgrade aplikace pomocí [metody **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), [ **Aktualizace ServiceFabricApplicationUpgrade** rutina](https://msdn.microsoft.com/library/azure/mt126030.aspx)nebo [operace **Aktualizace aplikace upgradu** ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. V případě potřeby *operátor* vrátí aktuální upgrade aplikace pomocí [metody **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), [ **Start ServiceFabricApplicationRollback** rutina](https://msdn.microsoft.com/library/azure/mt125833.aspx)nebo [ **Vrácení aplikace upgradu** ZBÝVAJÍCÍ operace](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Služba struktury aktualizuje cílové aplikace spuštěné v clusteru bez ztráty dostupnosti základních služeb.

Viz [aplikace upgrade kurz](service-fabric-application-upgrade-tutorial.md) příklady.

## <a name="maintain"></a>Udržovat
1. Při operační systém upgradech a opravy rozhraní struktury služby Azure infrastrukturu pro zajištění dostupnosti všechny spuštěné v clusteru.

2. Při upgradech a opravy platformu služby struktury aktualizuje služby struktury samotné bez ztráty dostupnost některého z aplikace spuštěna clusteru.

3. *Správce aplikace* schválí přidávání nebo odebírání uzlů z clusteru po analýze dat využití historických kapacitu a plánované budoucí služba.

4. *Operátor* přidá a odstraní uzly nastavil *Správce aplikací*.

5. Pokud nové uzly se přidají do nebo existujících uzlů budou odebrány z clusteru, struktury služby automaticky zatížení zůstatků spuštěné aplikace na všech uzlech clusteru k dosažení optimálního výkonu.

## <a name="remove"></a>Odebrání
1. *Operátor* můžete odstranit konkrétní instanci pracovního služby v clusteru bez odebrání celou aplikaci pomocí [ **DeleteServiceAsync** metoda](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), [rutina **ServiceFabricService odebrat** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)nebo [ **Odstranit služba** REST operace](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. *Operátor* můžete taky odstranit instance aplikace a všechny její služby pomocí [metody **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), [ **Odebrat ServiceFabricApplication** rutina](https://msdn.microsoft.com/library/azure/mt125914.aspx)nebo [operace **Odstranění aplikace** ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Jakmile zastavení aplikace a služby, můžete zrušit *operátor* zřízení typ aplikace pomocí [metody **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), [ **Unregister ServiceFabricApplicationType** rutina](https://msdn.microsoft.com/library/azure/mt125885.aspx)nebo [operaci **Zrušit zřízení aplikace** ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/dn707671.aspx). Rušení vytvoření typ aplikace neodebírat balíčku aplikace z ImageStore. Balíček aplikace je třeba odebrat ručně.

4. *Operátor* odebere ImageStore pomocí [metody **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) nebo [ **Odebrat ServiceFabricApplicationPackage** rutina](https://msdn.microsoft.com/library/azure/mt163532.aspx)balíček aplikace.

Příklady naleznete v tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Další kroky

Další informace o vývoji testování a Správa aplikací služby struktury a služby, najdete v článku:

- [Spolehlivé účastníky](service-fabric-reliable-actors-introduction.md)
- [Spolehlivé služby](service-fabric-reliable-services-introduction.md)
- [Nasazení aplikace](service-fabric-deploy-remove-applications.md)
- [Upgrade aplikace](service-fabric-application-upgrade.md)
- [Testování přehled](service-fabric-testability-overview.md)
- [Ukázka životního cyklu na základě ZBÝVAJÍCÍ aplikace](service-fabric-rest-based-application-lifecycle-sample.md)
