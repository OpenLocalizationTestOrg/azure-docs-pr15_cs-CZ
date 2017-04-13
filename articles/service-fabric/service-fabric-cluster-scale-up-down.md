<properties
   pageTitle="Změna velikosti obrázku služby struktury či oddálení | Microsoft Azure"
   description="Změnit velikost clusteru služby struktury či oddálení podle služba pomocí nastavení automatické měřítko pravidla pro každou sadu uzel typ/OM měřítko. Přidání nebo odebrání uzly clusteru struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Změna velikosti obrázku služby struktury či oddálení pomocí pravidel pro automatické měřítko

Virtuální počítač měřítko sady jsou Azure výpočetním prostředek, který můžete nasazovat a spravovat kolekce virtuálních počítačích jako sady. Všechny typy uzel, která je definovaná v clusteru služby struktury nastavenou jako samostatnou sadu OM měřítko. Každý typ uzlu lze nastavit v nebo se nezávisle na sobě, mají různé sady porty otevřít a může obsahovat jinou kapacitu metriky. Další informace o ho [nodetypes služby struktury](service-fabric-cluster-nodetypes.md) dokumentu. Protože struktury služby typy uzlů v svůj cluster provedeny OM měřítka nastaví na back-end, budete muset nastavit automatické měřítko pravidla pro každou sadu uzel typ/OM měřítko.

>[AZURE.NOTE] Vaše předplatné, musíte mít dost jádra přidáte nové VMs tvořící tomto obrázku. Momentálně žádné ověřování modelu, abyste se selhání nasazení čas Pokud některé z kvóty přístupů.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Zvolte typ/OM měřítko uzel nastavte měřítko

V současné době nejste schopni automatické měřítko pravidla zadat pro OM měřítko sady na portálu, takže dejte nám pomocí prostředí PowerShell Azure (1.0 +) typy Uzel seznamu a pak k nim přidat pravidla automatické měřítko.

Vytvořte požadovaný seznam nastavení OM měřítko, které tvoří svůj cluster, spusťte tyto rutiny:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Nastavit automatické měřítko pravidla pro sadu měřítko typ/OM uzlů

Pokud svůj cluster má více typů uzel, potom opakujte u jednotlivých typů/OM měřítko uzel nastaví, že chcete změnit velikost (či oddálení). Vzít v úvahu počtu uzlů, musíte mít před nastavením automatické měřítko. Minimální počet uzlů, které musíte mít pro typ primární uzel úsilím podle úrovně spolehlivosti, kterou jste zvolili. Další informace o [úrovních spolehlivost](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Zmenšení primární uzel zadejte do nižší než minimální číslo vytvářecího nestabilní obrázku nebo přenést. To může způsobit ztrátu dat pro aplikace a služby systému.

Funkci Automatické měřítko není aktuálně úsilím tak, že zatížení, které aplikace může sestavy do služby struktury. V současné době, které automatické měřítko, získáte je čistě řízeno výkon tak čítače, které jsou vyzařované každým OM velikosti sady instancí.  

Postupujte podle těchto pokynů [nastavit automatické měřítko pro každou sadu OM měřítko](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] V měřítku dolů scénář Pokud povahy uzel má úroveň životnosti Gold nebo slovo stříbro budete muset zavolat [Odebrat ServiceFabricNodeState rutina](https://msdn.microsoft.com/library/azure/mt125993.aspx) s názvem příslušný uzel.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Ruční přidání VMs sady uzlů typ/OM měřítko

Ukázka a podle pokynů [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) změnit počet zobrazených VMs v jednotlivých Nodetype. 

>[AZURE.NOTE] Přidávání VMs dlouho, takže neočekáváte dodatky, které mají být okamžité. Takže v plánu přidat kapacitu i v čase umožňující víc než 10 minut před je k dispozici pro replik kapacita OM / služby instance umístěny.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Ruční odebrání VMs ze sady primární uzel typ/OM měřítko

>[AZURE.NOTE] Systém služby struktury spustit v seznamu Typ primární uzel clusteru. Tak, aby měli nikdy vypnout nebo Neomezovat počet instancí u typů uzel menší než úrovně spolehlivosti vyžadoval. Podívejte se do [Podrobnosti o spolehlivosti úrovní tady](service-fabric-cluster-capacity.md). 

Je nutné provést následující kroky jedna OM instance po druhém. To umožňuje systémových služeb (a stavové služby) se neukončí na OM instance, kterou chcete odebrat a nové repliky vytvořené v jiné uzlů.

1. Spusťte [Zakázat ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) záměr "RemoveNode" Zakázat uzel budete chtít odebrat (nejvyšší instance tohoto typu uzel).

2. Spusťte [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , abyste měli jistotu, že na uzel má ve skutečnosti, které přešly zakázáno. Pokud ne, počkejte, až na uzel zakázané. Nelze hurry tento krok.

2. Ukázka a podle pokynů [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) změnit počet zobrazených VMs tak, že v této Nodetype. Instancí odebrány je nejvyšší OM instance. 

3. Opakujte kroky 1 až 3 v případě potřeby, ale nikdy Neomezovat počet instancí v primární uzel typy menší než vyžadoval spolehlivost osy. Podívejte se do [Podrobnosti o spolehlivosti úrovní tady](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Ruční odebrání VMs ze sady není primární uzel typ/OM měřítko

>[AZURE.NOTE] Ve stavové služby třeba počtu uzlů být vždy až zachovat dostupnost a zachovat stav služby. V velmi minimální musíte počtu uzlů rovno cílové otevřené sadu počet hodnot v oddílu nebo službu. 

Potřebujete spouštět následující kroky jedna OM instance po druhém. Tato funkce umožňuje systémových služeb (a stavové služby) se neukončí OM instance, kterou chcete odebrat a nové repliky vytvořit dalšího where.

1. Spusťte [Zakázat ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) záměr "RemoveNode" Zakázat uzel budete chtít odebrat (nejvyšší instance tohoto typu uzel).

2. Spusťte [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , abyste měli jistotu, že na uzel má ve skutečnosti, které přešly zakázáno. V opačném případě počkejte, dokud uzel zakázané. Nelze hurry tento krok.

2. Ukázka a podle pokynů [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) změnit počet zobrazených VMs tak, že v této Nodetype. Tato akce odebere teď nejvyšší OM instance. 

3. Opakujte kroky 1 až 3 v případě potřeby, ale nikdy Neomezovat počet instancí v primární uzel typy menší než vyžadoval spolehlivost osy. Podívejte se do [Podrobnosti o spolehlivosti úrovní tady](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Chování může dojít v Průzkumníku struktury služby

Když změníte velikost clusteru Explorer struktury služby budou obsahovat počtu uzlů (OM měřítko nastavené instance), které jsou součástí clusteru.  Však při změně měřítka obrázku dolů můžete uvidí instance odebrané uzel/OM zobrazené v nefunkčního stavu, pokud voláte [Odebrat ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) s názvem příslušný uzel.   

Tady je popis toto chování.

Uzly uvedená v Průzkumníku struktury služby jsou odraz jaké struktury služby systému služeb (FM konkrétně) ví o počtu uzlů clusteru měli/obsahuje. Při měřítka měřítko OM stanovené OM byl odstraněn, ale FM systémová služba pořád myslí, že na uzel (které bylo OM, který byl odstraněn) chodily zpět. Takže služby struktury Průzkumníka nadále zobrazovat uzel (když stavu může být chybu nebo neznámý).

Aby měli jistotu, že uzel zmizí při odebrání virtuálního počítače, máte dvě možnosti:

1) Vyberte úroveň životnosti Gold nebo slovo stříbro (dostupné brzy bude k dispozici) pro typy uzlů v clusteru, které vám integrace infrastruktury. Které pak automaticky odstraní uzly z našich stav služby (FM) systému při měřítka.
Podívejte se do [Podrobnosti o životnosti úrovně tady](service-fabric-cluster-capacity.md)

2) Jakmile instance OM byla změněna velikost,, budete muset volat [Odebrat ServiceFabricNodeState rutiny](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Služba struktury clusterů vyžadují počtu uzlů být v vždy při zachování dostupnost a zachovat stav - označovány jako "Správa kvora." Ano je obvykle nebezpečné vypnout všechny počítače v clusteru, pokud jste provedli [úplné zálohování váš stav](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Další kroky
Přečtěte si následující taky Další informace o plánování clusteru kapacity upgradu clusteru a rozdělení služby:

- [Plánování clusteru kapacity](service-fabric-cluster-capacity.md)
- [Upgrade obrázku](service-fabric-cluster-upgrade.md)
- [Oddíl stavové služby pro maximální měřítko](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
