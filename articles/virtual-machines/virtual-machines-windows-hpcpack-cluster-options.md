<properties
 pageTitle="Možnosti obrázku Windows HPC Pack v cloudu | Microsoft Azure"
 description="Zjistit, jaké možnosti s Microsoft HPC Pack můžete vytvořit a spravovat Windows vysoký výkon výpočetních clusterů (HPC) v Azure cloudu"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Možnosti s aktualizací Service Pack HPC můžete vytvořit a spravovat cluster Windows HPC Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tento článek se zaměřuje na možnost vytvořit HPC Pack clusterů spuštění úloh systému Windows. Existují také možnosti pro vytváření clusterů ke spuštění [pracovního vytížení Linux HPC s aktualizací Service Pack HPC](virtual-machines-linux-hpcpack-cluster-options.md).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Spuštění HPC Pack obrázku v Azure VMs

### <a name="azure-templates"></a>Azure šablony

* (Marketplace) [Shluk HPC Pack pro úloh systému Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [Shluk HPC Pack pro Excel úloh](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Rychlý úvod) [Vytvoření HPC obrázku](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Rychlý úvod) [Vytvoření HPC obrázku s obrázkem po posunutí vlastní výpočetním uzel](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure OM obrázky

* [Sada HPC v systému Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [HPC Pack výpočetní uzly v systému Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack výpočet uzel s Excelem v systému Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>Skript Powershellu pro nasazení

* [Vytvoření clusteru HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Výukové programy

* [Kurz: Začínáme s HPC Pack obrázku v Azure spusťte Excel a SOA úloh](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Ruční nasazení pomocí portálu Azure

* [Nastavení hlavního uzlu HPC Pack obrázku v Azure OM](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Správa clusteru

* [Správa uzlů výpočetním clusteru HPC Pack v Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Zvětšovat a zmenšovat zdroje Azure výpočetním clusteru HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Odeslat úlohy HPC Pack clusteru v Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Řízení projektů HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Přidání pracovního role uzlů clusteru HPC Pack


* [Shlukové Azure pracovníka instance HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)

* [Kurz: Nastavení hybridního obrázku s aktualizací Service Pack HPC v Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Přidání Azure "požadavků" uzlů hlavy uzel HPC Pack v Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integrace s Azure dávku 

* [Shlukové Azure listu s aktualizací Service Pack HPC](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Vytvoření RDMA clusterů MPI úloh

* [Nastavení Windows RDMA obrázku s aktualizací Service Pack HPC spouštění aplikací MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
