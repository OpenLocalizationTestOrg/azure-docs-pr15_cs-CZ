<properties
 pageTitle="Možnosti obrázku Linux HPC Pack v cloudu | Microsoft Azure"
 description="Zjistit, jaké možnosti s Microsoft HPC Pack můžete vytvořit a spravovat Linux výkonných výpočetních clusterů (HPC) v Azure cloudu"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Možnosti HPC Pack můžete vytvořit a spravovat clusteru HPC v Azure pro Linux úloh

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tento článek se zaměřuje na možnost použití HPC Pack pro spuštění pracovního vytížení Linux. Podívejte se také možnosti pro spuštění [pracovního vytížení Windows HPC s aktualizací Service Pack HPC](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Spuštění HPC Pack obrázku v Azure VMs

### <a name="azure-templates"></a>Azure šablony


* (Marketplace) [Shluk HPC Pack pro Linux úloh](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Rychlý úvod) [Vytvoření HPC obrázku s Linux výpočet uzlů](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>Skript Powershellu pro nasazení

* [Vytvoření clusteru Linux HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Výukové programy

* [Kurz: Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Kurz: Výpočet spustit NAMD s Microsoft HPC Pack na Linux uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Kurz: Spustit OpenFOAM s Microsoft HPC Pack clusteru Linux RDMA v Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Kurz: Spustit HVĚZDA-CCM + s Microsoft HPC Pack na Linux RDMA cluster v Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Správa clusteru

* [Odeslat úlohy HPC Pack clusteru v Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Řízení projektů HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Vytvoření RDMA clusterů MPI úloh

* [Kurz: Spustit OpenFOAM s Microsoft HPC Pack clusteru Linux RDMA v Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Nastavení spuštění aplikace MPI Linux RDMA obrázku](virtual-machines-linux-classic-rdma-cluster.md)

