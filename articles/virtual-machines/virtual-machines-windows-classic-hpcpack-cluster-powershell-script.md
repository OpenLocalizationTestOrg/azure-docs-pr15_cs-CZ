<properties
   pageTitle="Skript Powershellu pro nasazení clusteru Windows HPC | Microsoft Azure"
   description="Spustit skript Powershellu pro nasazení clusteru Windows HPC Pack v Azure virtuálních počítačích"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Vytvoření Windows výkonných výpočetních clusterů (HPC) clusteru se skriptem HPC Pack IaaS nasazení

Spusťte nasazení HPC Pack IaaS skript Powershellu pro nasazení celého clusteru HPC pro Windows úloh v Azure virtuálních počítačích. Clusteru je tvořen připojení ke službě Active Directory hlavy uzlu se systémem Windows Server a Microsoft HPC Pack a další okna výpočet prostředky, které zadáte. Pokud chcete nasadit clusteru HPC Pack v Azure pro Linux zatížení, v tématu [Vytvoření clusteru Linux HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Taky můžete šablonu správce prostředků Azure nasazení HPC Pack obrázku. Příklady naleznete v tématu [Vytvoření HPC obrázku](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) a [Vytvoření HPC obrázku s vlastní výpočet uzel obrázek](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Příklad konfigurační soubory

V následujících příkladech nahraďte vlastní hodnoty pro vaše předplatné Id nebo název a název účtu a služby.

### <a name="example-1"></a>Příklad 1

Následující konfiguračního souboru nasadí HPC Pack clusteru, který má hlavy uzel s místní databází a pět výpočet uzly s operačním systémem Windows serveru 2012 R2. Cloudových služeb vytvářet přímo v umístění západní USA. Hlavy uzel funguje jako domény řadiče domény struktury.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Příklad 2

Následující konfiguračního souboru nasadí HPC Pack obrázku v existující struktuře domény. Clusteru má 1 hlavy uzel s místní databází a 12 výpočet uzly s příponou BGInfo OM použitý.
Automatické instalace aktualizací systému Windows není k dispozici pro všechny VMs ve struktuře domény. Cloudových služeb vytvářet přímo v umístění jihovýchodní Asie. Uzly výpočetním vytvořené v tři cloudovými službami a tři účty úložiště: _MyHPCCN 0001_ _MyHPCCN 0005_ _MyHPCCNService01_ a _mycnstorage01_; _MyHPCCN 0006_ _MyHPCCN0010_ _MyHPCCNService02_ a _mycnstorage02_; a _MyHPCCN 0011_ _MyHPCCN 0012_ _MyHPCCNService03_ a _mycnstorage03_). Uzly výpočetním vytvořené z existující soukromé obrázek nezaznamenávají z výpočetní uzly. Automatické zvětšovat a zmenšovat služba s výchozím zvětšovat a zmenšovat intervalů.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Příklad 3

Následující konfiguračního souboru nasadí HPC Pack obrázku v existující struktuře domény. Clusteru obsahuje jeden hlavy uzel, jeden databázový server s diskem dat 500 GB, 2 zprostředkovatele uzly s operačním systémem Windows serveru 2012 R2 a pět výpočetním uzly s operačním systémem Windows serveru 2012 R2. Cloudové služby MyHPCCNService se vytvoří ve skupině spřažení *MyIBAffinityGroup*a cloudových služeb jsou vytvářeny ve skupině spřažení *MyAffinityGroup*. Hlavy uzlu jsou povoleny HPC úlohy Plánovač REST API a HPC webového portálu.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Příklad 4:

Následující konfiguračního souboru nasadí HPC Pack obrázku v existující struktuře domény. Clusteru obsahuje dva hlavní uzel s místní databází, dvě Azure uzel šablony vytvořené a tři uzly střední Azure velikost vytvořené šablony Azure uzel _AzureTemplate1_. Soubor skriptu běží na uzel hlavy po dokončení konfigurace hlavního uzlu.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Řešení potíží


* **Chyba "VNet neexistuje"** – Pokud spustíte skript pro nasazení více clusterů v Azure současně ve skupinovém rámečku jedno předplatné jeden nebo více nasazení se nemusí podařit, zobrazí se chyba "VNet *VNet\_název* neexistuje".
Pokud k této chybě dochází, spusťte skript znovu pro selhání nasazení.

* **Problém přístup k Internetu z Azure virtuální sítě** – Pokud jste vytvořili clusteru pomocí nového řadiče domény pomocí skriptu nasazení nebo ručně zvýšení úrovně hlavy uzlu OM řadiče domény, může docházet k problémům VMs připojení k Internetu. Tomuto problému může dojít, pokud serveru DNS pro předávání automaticky nakonfigurovaný na doménovém a tento server DNS pro předávání nerozpozná správně.

    K tomuto problému, přihlaste se k řadiče domény a buď odebrat nastavení předávání nebo konfigurace serveru DNS platné předávání. Ve Správci serveru toto nastavení, klikněte na **Nástroje** >
    **DNS** otevřít Správce DNS a potom poklikejte na **předávání**.

* **Problém přístupu k síti RDMA z náročné VMs** – Pokud přidáte výpočetním systému Windows Server nebo zprostředkovatel VMs uzel s podporou RDMA velikostí A8 ATP A9, může docházet k problémům tyto VMs připojení k síti aplikace RDMA. Jedna z důvodu že se vyskytuje problém, je-li přípona HpcVmDrivers nenainstaluje správně při přidání VMs clusteru. Rozšíření může například zůstal viset ve stavu instalace.

    Informace k alternativním řešením tohoto problému, nejprve zkontrolujte stav do VMs rozšíření. Rozšíření bez nainstalovaného správně, zkuste odebrat uzly z HPC obrázku a pak přidejte uzlů. Například můžete přidat výpočetní uzly VMs spuštěním skriptu přidat HpcIaaSNode.ps1 na uzel hlavy.
    
## <a name="next-steps"></a>Další kroky

* Spusťte test úlohu na clusteru. Příklad najdete v článku HPC Pack [příručku Začínáme](https://technet.microsoft.com/library/jj884144).

* Kurz skriptu nasazení obrázku a spustit pracovní zátěž HPC najdete v článku [Začínáme s HPC Pack obrázku v Azure spusťte Excel a SOA úloh](virtual-machines-windows-excel-cluster-hpcpack.md).

* Zkuste nástroje HPC Pack spustit, zastavit, přidání a odebrání uzly výpočetním clusteru, které vytvoříte. V tématu [Správa výpočet uzlů v HPC Pack obrázku v Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Nastavit odesílat úlohy clusteru z místního počítače, najdete v článku [Odeslat HPC úlohy z místního počítače HPC Pack clusteru v Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
