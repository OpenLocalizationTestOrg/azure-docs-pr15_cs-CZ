<properties
   pageTitle="Typy uzlů služby struktury a OM měřítko sady | Microsoft Azure"
   description="Popisuje, jak typy uzlů struktury služby se vztahují k OM měřítko sady a jak chcete vzdálené připojit k instanci OM měřítko vybrána položka nebo clusteru."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Vztah mezi typy uzlů služby struktury a sady měřítko virtuálního počítače

Virtuální počítač měřítko sady jsou výpočet Azure zdrojů můžete nasazovat a spravovat kolekce virtuálních počítačích jako sady. Všechny typy uzel, která je definovaná v clusteru služby struktury nastavenou jako samostatný OM měřítko sadu. Každý typ uzlu lze nastavit nebo dolů nezávisle na sobě, mají různé sady porty otevřít a může obsahovat jinou kapacitu metriky.

Následující snímek obrazovky znázorňuje obrázku, který má dva typy uzel: FrontEnd a back-end.  Každý typ uzel má pět uzlů.

![Snímek obrazovky s obrázku, který má dva typy uzel][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Mapování nastavte měřítko OM instance uzlů

Jak je vidět nad instance nastavte měřítko OM spustit z instance 0 a poté přejde nahoru. Číslování se projeví v názvech. Například BackEnd_0 je instance 0 sady back-end OM měřítko. Má pět instance s názvem BackEnd_0 BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4 v této konkrétní OM měřítko sady.

Při změně měřítka nastaveny měřítko OM se vytvoří nové instance. Nastavte měřítko OM instance názvu nové bude obvykle OM měřítko pojmenovat + další instanci číslo. V našem příkladu je BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Mapování OM měřítko nastavit každý uzel typ/OM měřítko sadu Vyrovnávání zatížení

Pokud jste nainstalovali svůj cluster z portálu Microsoft nebo jste použili Ukázková šablona správce prostředků, který jsme za předpokladu, když se zobrazí seznam všech zdrojů v části Skupina zdroje uvidíte Vyrovnávání zatížení u jednotlivých typů OM měřítko vybrána položka nebo uzel.

Název by nějak: **LB -&lt;NodeType název&gt;**. Například LB-sfcluster4doc-0, jak ukazuje tento obrázek:


![Zdroje informací][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Vzdáleného připojení k instanci OM měřítko vybrána položka nebo clusteru
Všechny typy uzel, která je definovaná v clusteru je nastavit jako samostatný OM měřítko sadu.  To znamená, že můžete diagramů s měřítky typy uzlů nahoru nebo dolů nezávisle na sobě a půjde z různých OM skladové jednotky. Na rozdíl od jedna instance VMs instance nastavte měřítko OM nezobrazí virtuální IP adresu vlastní. To může být trochu náročné když hledáte IP adresy a portu, které vám pomohou vzdáleného připojení konkrétní instanci.

Tady je postup, můžete k Seznamte se s nimi.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Krok 1: Zjištění, virtuální IP adresa zadejte uzel a potom překladu síťových adres příchozí pravidla pro RDP

Abyste mohli získávat, musíte získat příchozí překladu síťových adres pravidla, které byly definované jako součást definici zdroje pro **Microsoft.Network/loadBalancers**.

Na portálu přejděte na zásuvné Vyrovnávání zatížení a potom **na nastavení**.

![LBBlade][LBBlade]


Ve skupinovém rámečku **Nastavení**klikněte na **překladu síťových adres příchozí pravidla**. Tento nyní vám nabídne IP adresa a portu, které vám pomohou vzdáleného připojení k jeho první instanci OM měřítko vybrána položka. Následující obrázek je **104.42.106.156** a **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Krok 2: Vyhledání mimo portu, který slouží k vzdáleného připojení k určité nastavte měřítko OM instance/uzel

Dříve v tomto dokumentu můžu kontakt o mapování instance nastavte měřítko OM do uzlů. Použijeme, zjistit přesné číslo portu.

Porty přiřazených ve vzestupném pořadí instance OM měřítko vybrána položka. tak, aby v tomto příkladu typu uzel FrontEnd porty všech pět instancí následující. Teď je potřeba udělat stejný mapování pro instanci aplikace OM měřítko vybrána položka.

|**Nastavte měřítko OM instanci**|**Port**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Krok 3: Vzdáleného připojení konkrétní instanci nastavte měřítko OM

V následující snímek obrazovky použiji připojení ke vzdálené ploše pro připojení k FrontEnd_1:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Jak změnit rozsah hodnot port RDP

### <a name="before-cluster-deployment"></a>Před nasazením obrázku

Pokud nastavujete clusteru pomocí šablony pro správce prostředků, můžete v **inboundNatPools**určit oblast.

Přejděte na definici zdroje pro **Microsoft.Network/loadBalancers**. V části, vyhledejte popis **inboundNatPools**.  Nahrazení hodnot *frontendPortRangeStart* a *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Po zavedení obrázku
Toto je trochu složitější a může mít za následek VMs začíná recykloval. Nyní máte k nastavení nové hodnot pomocí prostředí PowerShell Azure. Ujistěte se, že je v počítači nainstalovaný Azure PowerShell 1.0 nebo novější. Pokud jste to před neudělali, můžu důrazně doporučujeme postupujte podle kroků uvedených v [instalace a konfigurace prostředí PowerShell Azure.](../powershell-install-configure.md)

Přihlaste se k účtu Azure. Když se tento příkaz prostředí PowerShell nepovede z nějakého důvodu, můžete měli zkontrolovat, zda Azure PowerShell správně.

```
Login-AzureRmAccount
```

Spusťte následující získat podrobné informace o vyrovnávání zatížení a zobrazit hodnoty pro popis **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Teď hodnoty, které chcete nastavit *frontendPortRangeEnd* a *frontendPortRangeStart* .

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Další kroky

- [Základní informace o funkci "Nasazení kamkoli" a porovnání s Azure Správa přístupových práv clusterů](service-fabric-deploy-anywhere.md)
- [Zabezpečení obrázku](service-fabric-cluster-security.md)
- [Služba struktury SDK a Začínáme](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
