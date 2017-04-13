<properties
   pageTitle="Skript Powershellu pro nasazení clusteru Linux HPC | Microsoft Azure"
   description="Spustit skript Powershellu pro nasazení clusteru Linux HPC Pack v Azure virtuálních počítačích"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Vytvoření Linux výkonných výpočetních obrázku (HPC) se skriptem HPC Pack IaaS nasazení

Spusťte nasazení HPC Pack IaaS skript Powershellu pro nasazení celého clusteru HPC pro Linux úloh v Azure virtuálních počítačích. Připojení ke službě Active Directory hlavy uzlu systémem Windows Server a Microsoft HPC Pack a výpočetním uzlů spuštěné jednoho z Linux rozdělení podporované balíkem HPC se skládá clusteru. Pokud chcete nasadit HPC Pack obrázku v pracovního vytížení Azure pro systém Windows, v tématu [Vytvoření clusteru Windows HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Taky můžete šablonu správce prostředků Azure nasazení HPC Pack obrázku. Příklad naleznete v tématu [Vytvoření HPC obrázku s Linux výpočet uzlů](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Příklad konfiguračního souboru

Následující konfiguračního souboru vytvoří nový řadiče domény a struktury domény a nasadí HPC Pack clusteru, který má 1 hlavy uzel s místní databází a 10 uzly výpočetním Linux. Cloudových služeb vytvářet přímo v umístění jihovýchodní Asie. Uzly výpočetním Linux vytvořené v 2 cloudovými službami a 2 úložiště účty (tedy _MyLnxCN 0001_ k _MyLnxCN 0005_ _MyLnxCNService01_ a _mylnxstorage01_) a _MyLnxCN 0006_ _MyLnxCN 0010_ _MyLnxCNService02_ a _mylnxstorage02_. Uzly výpočetním vytvořené z bitové OpenLogic CentOS verze 7.0 Linux. 

Nahraďte vlastní hodnoty název předplatného a název účtu a služby.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Řešení potíží

* **Chyba "VNet neexistuje"** – Pokud spustíte skript pro nasazení HPC Pack IaaS nasazení více clusterů v Azure současně ve skupinovém rámečku jedno předplatné jeden nebo více nasazení se nemusí podařit, zobrazí se chyba "VNet *VNet\_název* neexistuje".
Pokud k této chybě dochází, opětovným spuštěním skript pro selhání nasazení.

* **Problém přístup k Internetu z Azure virtuální sítě** – Pokud jste vytvořili HPC Pack obrázku s nového řadiče domény pomocí skriptu nasazení nebo ručně zvýšení úrovně hlavy uzlu OM řadiče domény, může docházet k problémům VMs v Azure virtuální síťové připojení k Internetu. Tato situace může nastat, pokud serveru DNS pro předávání automaticky nakonfigurovaný na doménovém a tento server DNS pro předávání nerozpozná správně.

    K tomuto problému, přihlaste se k řadiče domény a buď odebrat nastavení předávání nebo konfigurace serveru DNS platné předávání. K tomuto účelu ve Správci serveru, klikněte na **Nástroje** >
    **DNS** otevřít Správce DNS a potom poklikejte na **předávání**.
    
## <a name="next-steps"></a>Další kroky

* Najdete v článku [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md) informace o podporovaných distribuce Linux přesunutí dat a odeslání úlohy HPC Pack clusteru s Linux výpočet uzlů.
* Kurzy, které slouží k vytvoření clusteru a spustit pracovní zátěž Linux HPC skript najdete v článku:
    * [Spuštění NAMD s Microsoft HPC Pack na Linux výpočet uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Spuštění OpenFOAM s Microsoft HPC Pack na Linux výpočet uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Spuštění HVĚZDA-CCM + s Microsoft HPC Pack na Linux výpočet uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
