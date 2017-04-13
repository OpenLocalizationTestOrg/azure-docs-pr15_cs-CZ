<properties
   pageTitle="Přidání nebo odebrání uzly do samostatného služby struktury clusteru | Microsoft Azure"
   description="Naučte se přidat nebo odebrat uzly do struktury služby Azure clusteru fyzické nebo virtuálního počítače serverem Windows, které by mohly být místní nebo v libovolné cloudu."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Přidání nebo odebrání uzly do samostatného služby struktury clusteru se systémem Windows Server

Po [vytvoření samostatného clusteru struktury služby na počítačích Windows Server](service-fabric-cluster-creation-for-windows-server.md)potřebám vaší společnosti může změnit tak, že možná budete muset přidat nebo odebrat více uzlů svůj cluster. Tento článek obsahuje podrobné pokyny k tomuto účelu.


## <a name="add-nodes-to-your-cluster"></a>Přidání uzlů do svého obrázku

1. Příprava OM/počítač, který chcete přidat na svůj cluster podle kroků uvedených v části [připravit počítačích plnit požadavky pro nasazení obrázku](service-fabric-cluster-creation-for-windows-server.md#preparemachines) .
2. Plánování, které poruch doména a upgradu se chystáte přidat OM/počítač tak, aby.
3. Vzdálená plocha (RDP) do OM/počítač, který chcete přidat do clusteru.
4. Kopírování nebo [Stáhnout balíček samostatného pro struktury pro Windows Server služby](http://go.microsoft.com/fwlink/?LinkId=730690) pro tento OM/počítač a rozbalte balíček.
5. Prostředí Powershell spustili s oprávněními správce a přejděte do umístění balíček.
6. Spusťte *AddNode.ps1* Powershellu s parametry popisující nový uzel přidat. Následující příklad přidává nový uzel s názvem VM5, s typem NodeType0, IP adresa 182.17.34.52 do UD1 a FD1. *ExistingClusterConnectionEndPoint* je připojení koncový bod pro uzel už ve stávajícím clusteru. Pro tento koncový bod můžete IP adresu *uzel* clusteru.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Odebrání uzlů ze svého obrázku

1. V závislosti na úrovni Reliablity zvolili clusteru nemůžete odebrat první n (3/5/7/9) uzly typu primární uzel
2. Spuštění příkazu RemoveNode clusteru vývojáře není podporovaná.
2. Vzdálená plocha (RDP) do OM/počítač, který chcete odebrat z clusteru.
2. Kopírování nebo [Stáhnout balíček samostatného pro struktury služby systému Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) a rozbalte balíček pro tento OM/počítač.
3. Prostředí Powershell spustili s oprávněními správce a přejděte do umístění balíček.
4. V prostředí PowerShell spusťte *RemoveNode.ps1* . V příkladu níže odebere aktuální uzel z clusteru. *ExistingClientConnectionEndpoint* je koncový bod připojení klientů pro uzel, který bude platit clusteru. Zvolte IP adresa a port koncového bodu *jakékoli* **jiné uzel** clusteru. Tento **druhý** zase aktualizovat konfigurace clusteru uzel odebírat. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Všimněte si, že i po odebrání uzel, se může zobrazit jako právě dolů v dotazech a SFX. Toto je známý vadu a opravíme v nadcházející verzi. 


## <a name="next-steps"></a>Další kroky
- [Konfigurace nastavení pro samostatné Windows obrázku](service-fabric-cluster-manifest.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí X509 certifikátů](service-fabric-windows-cluster-x509-security.md)
- [Vytvoření samostatného služby struktury clusteru se systémem Windows Azure VMs](service-fabric-cluster-creation-with-windows-azure-vms.md)
