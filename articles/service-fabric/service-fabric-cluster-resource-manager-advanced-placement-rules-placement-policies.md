<properties
   pageTitle="Správce prostředků služby struktury clusteru – umístění zásady | Microsoft Azure"
   description="Základní informace o dalších umístění zásady a pravidla pro struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Umístění zásad pro služby struktury
Existuje mnoha různých dalších pravidel, které může skončily caring o Pokud rozložený svůj cluster struktury služby na zeměpisné vzdálenosti řekněte více datacentrech nebo Azure oblastí, nebo pokud prostředí zahrnuje více oblastí geopolitické ovládacího prvku (nebo některé další případ, kdy budete mít právní nebo této zásadě hranice zajímavého nebo souvisejících vzdálenosti mít vliv skutečné výkonu/latence). Většina z nich lze nastavit pomocí vlastnosti uzlu a umístění omezení, ale některé jsou složitější. Aby věci jednodušší nabízíme tyto další commmands. Stejně jako se jiných omezení umístění zásady umístění lze nastavit na základě instanci služby za názvem.

## <a name="specifying-invalid-domains"></a>Zadání neplatných domén
Zásady umístění InvalidDomain vám umožní určit, že určité domény poruch nejsou platná: pro tento pracovní zátěž. Tuto zásadu zajistíte, že konkrétní službu nikdy spustí v určité oblasti, jako je například geopolitické nebo firemní zásady důvodů. Přes různé zásady může být zadán neplatný víc domén.

![Příklad neplatné domény][Image1]

Kód:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Powershellu:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Určení požadované domény
Zásady umístění požadované domény vyžaduje, aby všechny stavové repliky nebo instance příslušnosti služby pro službu účastní zadaný domény. Víc domén požadované může být zadán přes různé zásady.

![Příklad požadované domény][Image2]

Kód:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

Powershellu:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Určení preferovaného domény pro primární repliky
Primární doména upřednostňované je ovládací prvek zajímavé umožňuje výběr poruch domény, ve kterém primární má nacházet, pokud je možné provést. Pokud všechno, co je správný primární skončí v této domény. Domény nebo selhání primární otevřené nebo vypnutí z nějakého důvodu, že hlavní se přesunou do jiného umístění. Není-li toto umístění v preferované doménu, pak pokud je to možné správce prostředků obrázku se jí povytáhnout upřednostňované doménu. Přirozeným toto nastavení pouze bude výstižně popisovat stavové služby. Tuto zásadu je nejužitečnější v clusterů, které jsou rozložené přes Azure oblasti nebo více datacentrech. V těchto situacích používáte všechna umístění pro redundance, ale raději blokovat primární repliky v určitém umístění za účelem zajištění nižší latence pro operace, které přejděte na primární (zapíše a taky ve výchozím nastavení všechny operace čtení se hodit primární).

![Preferovaný primární domény a překlopení][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Powershellu:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Vyžadování repliky rozdělí mezi všech domén a zákaz balení
Jiné zásad, které můžete určit, je vyžadují repliky vždy rozdělí mezi dostupné poruch domény. K tomu dojde ve výchozím nastavení ve většině případů, kde je clusteru správný, ale jsou degenerate případy, kde repliky pro daný oddíl může být dočasně skončily zabalit do jednoho poruch nebo upgrade domény. Například Řekněme, že, i když clusteru má 9 uzlů v 3 poruch domény (0, 1 a 2), bude mít služba 3 repliky, uzly použité pro tyto repliky poruch domén 1 a 2 se a kvůli problémům s kapacitu k žádné další uzlů v těchto domén byly platné. Pokud služby struktury bylo potřeba vytvářet náhrady tyto repliky, správce prostředků clusteru musel zapište je do domény poruch 0, ale, která vytvoří situace, kdy porušení omezení poruch domény. Taky zvyšuje pravděpodobnost, že sadu celé otevřené může dojít ke ztrátě (FD 0 došlo k permananently ztratilo). (Další informace o omezení a omezení priority obecně, podívejte se na [Toto téma](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Pokud vůbec ukázali jsme stavu upozornění jako "vyrovnávání zatížení zjistil omezení pro tento otevřené: struktury: /<some service name> sekundární oddíl <some partition ID> porušuje omezení: FaultDomain" jste klikněte na tuto podmínku nebo nějak ho. Obvykle jsou přechodné těchto situací (uzly není zůstat dolů dlouhé nebo pokud přehrávají a potřebujeme vytvářet nahrazení tam jsou jiné uzly správnou odolnost domény, které jsou platné), ale existují některé úloh, které byste raději obchodu dostupnost pro riziko ztráty svých ostatními. Jsme umí zadáním zásada "RequireDomainDistribution", která zajistí, že jsou v tu samou doménu poruch nebo upgrade někdy povoleny žádné dvě repliky ze stejného oddílu.

Některé pracovního vytížení dají přednost cílové počtu replik (kopií stavu) vůbec časy (ale před selháním celkové domény a víte, že jsou obvykle obnovit stav místní), že ostatní byste raději prostoje dříve, než rizika pochybnosti správnosti a dataloss. Vzhledem k tomu, že většina výrobní úloh spustit s více než 3 replikami, ve výchozím nastavení je nevyžadují rozdělení domény a aby vyrovnávání a převzetí případy zpracovávat obvykle to i v případě, která znamená, že dočasně doménu má více replikách zabalit do ní.

Kód:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

Powershellu:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Teď by bylo možné použít tyto konfigurace služby v obrázku, který nebyl rozložený zeměpisně? Zkontrolujte, zda budete moci! Ale není k dispozici skvělé důvod, proč příliš – zejména konfigurace požadované neplatné a upřednostňované domény je vyhnout, pokud používáte skutečně geograficky rozložený clusteru - nemá smysl všechny zkusit vynutit danou úlohu běží v jedné regálů nebo raději některé segmentu místní obrázku nad jiný nejedná různé typy hardware nebo pracovní zátěž segmentace děje , a případy můžete přistupovat přes omezení normální umístění.

## <a name="next-steps"></a>Další kroky
- Pro další informace o dalších možností konfigurace služeb podívejte se na téma na ostatní clusteru Správce konfigurace k dispozici [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
