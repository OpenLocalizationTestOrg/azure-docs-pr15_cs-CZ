<properties
   pageTitle="Omezení ve Správci služby struktury clusteru zdroje | Microsoft Azure"
   description="Informace o konfiguraci omezením podle struktury clusteru zdroje správcem služeb."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Omezení chování ze struktury clusteru zdroje Správce služeb
I když jste nakonfigurovali Správce prostředků clusteru správně, můžete získat přerušení clusteru. Například může být souběžné uzel nebo poruch domény k chybám – co byste měli příčiny, ke kterým došlo během upgradu? Správce prostředků pokusí zajistili optimální řešení všechno, ale v časy takto můžete chtít zvažte backstop, takže samotný cluster možnost stabilizovat (uzly, které se chystáte pocházet zpět proveďte, stav sítě retušovat sami, nenasadí opravený bitů). Pomoct s těchto typů situací, Správce služeb struktury clusteru zdroje zahrnout několika omezením. Všimněte si, že tyto omezením jsou velmi vyloučit a obecně není určená k použití pokud došlo některé opatrní matematické Hotovo kolem množství paralelní práce, kterou lze provést skutečně clusteru, stejně jako v časté musí odpovídat na tyto seřadí z (ahem) neplánované makroskopické konfigurace události (ZTJ: "Velmi chybná dnů").

Obecně doporučujeme vyhnout velmi špatnou dnů pomocí dalších možností (jako pravidelné kód aktualizace a chybovým overscheduling clusteru na začátku) místo omezení svůj cluster kvůli kterým ho pomocí zdroje při opravíte samotné). Omezením mají výchozí hodnoty, které jsme nalezení prostřednictvím prostředí být ok výchozí hodnoty, ale měli byste pravděpodobně podívejte a ladění je očekávaná aktuální zatížení. Během omezení není příliš nebo načtení clusteru je nejvhodnější, které můžete určit, že jsou případech, které (můžete je napravit), dokud bude trvat déle stabilizovat místo, kam budete muset mít několik omezením na místě, i když to znamená: clusteru.

##<a name="configuring-the-throttles"></a>Konfigurace omezením
Omezením, které jsou zahrnuty ve výchozím nastavení jsou:

-   GlobalMovementThrottleThreshold – řídí celkový počet pohyby clusteru nad určitou dobu (rozumí GlobalMovementThrottleCountingInterval hodnotu v sekundách)
-   MovementPerPartitionThrottleThreshold – řídí celkový počet pohyby pro každý oddíl služby nad určitou dobu (MovementPerPartitionThrottleCountingInterval, hodnotu v sekundách)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Mějte na paměti, že ve většině případů jsme viděli zákazníky pomocí těchto omezením, které už uběhlo protože byly už v prostředí zdroje omezena pouze (jako je omezené šířka pásma do jednotlivých uzlech nebo disků, které nebyly až požadavky paralelní otevřené vytvoří jsou umístěné na nich) které mysleli tyto operace nebude úspěšná nebo měl by pomalé přesto.  V těchto situacích byli zákazníci pohodlnému předpokladu, že budou byly potenciálně rozšíření množství času, který by clusteru kontaktovat stabilní státu, včetně vědět, který by mohl skončily spuštěn v dolním celkové spolehlivost a byly omezena.

## <a name="next-steps"></a>Další kroky
- Najdete informace o tom správce prostředků clusteru spravuje a zůstatky náklad v clusteru podívejte se na článek o [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Správce prostředků clusteru má spoustu možností pro popis clusteru. Zjistit další informace o jejich najdete v tématech tohoto článku na [popisující služby struktury obrázku](service-fabric-cluster-resource-manager-cluster-description.md)
