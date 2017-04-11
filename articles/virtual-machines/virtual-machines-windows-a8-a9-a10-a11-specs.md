<properties
 pageTitle="Informace o náročné VMs s Windows | Microsoft Azure"
 description="Získat základní informace a důležité informace týkající se používání Azure velikosti náročné H řady a A8 A9, A10 a A11 pro Windows VMs a cloudové služby"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informace o H řady a náročné VMs A řady

Tady je základní informace a několik tipů pro používání novější Azure řady H a starší A8, A9, A10 a A11 instance, nazývaný také *náročné* instance. Tento článek se zaměřuje na pomocí těchto případů pro Windows VMs. Tento článek je také k dispozici pro [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Přístup k síti RDMA

Můžete vytvořit clusterů instancí podporou RDMA systému Windows Server a nasazení jeden z podporovaných implementace MPI umožní využít výhod Azure RDMA sítě. Tento latencí minimum, maximum výkon sítě je rezervován MPI pouze pro provoz.

* **Operační systém**
    * **Virtuálních počítačích** – Windows Server 2012 R2, Windows Server 2012
    * **Cloudovým službám** – Windows serveru 2012 R2, Windows Server 2012, operačního systému Windows Server 2008 R2 hosta rodinu

* **MPI** - Microsoft MPI ([MS-MPI) 2012 R2 nebo novější, Intel MPI knihovny 5.x

Podporované implementace MPI pomocí rozhraní Microsoft sítě přímé komunikaci mezi instancí. Naleznete v tématu [Nastavení Windows RDMA obrázku s HPC Pack spouštění aplikací MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) a [používání více instancí úkoly spouštění aplikací rozhraní předávání zpráv (MPI) v Azure dávce](../batch/batch-mpi.md) možnosti nasazení a kroky konfigurace vzorku.


>[AZURE.NOTE]Na podporou RDMA náročné VMs musí být přidán koncovku HpcVmDrivers k VMs instalace ovladačů zařízení sítě systému Windows, které jsou potřebné pro RDMA připojení. Ve většině nasazení se automaticky přidá příponu HpcVmDrivers. Pokud potřebujete přidat rozšíření najdete v článku [Správa OM přípony](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Co zvážit při HPC Pack a Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), bezplatné clusteru HPC společnosti Microsoft a řešení úlohy správy, není nutné použít náročné instance systému Windows Server. Je ale jednou z možností vytvoříte ve výpočetním clusteru v Azure ke spuštění aplikace MPI serveru s Windows a dalších HPC úloh. HPC Pack 2012 R2 a novějších verzích obsahují prostředí runtime pro MS-MPI, můžete použít síť Azure RDMA nasazené na podporou RDMA VMs.

Další informace a seznamy úkolů pomocí náročné instancí služby HPC Pack v systému Windows Server najdete v článku [Nastavení Windows RDMA obrázku s HPC Pack spouštění aplikací MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Další kroky

* Podrobnosti o dostupnosti a ceny velikostí náročné najdete v tématu [virtuálních počítačích ceny](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) a [ceny Cloudovým službám](https://azure.microsoft.com/pricing/details/cloud-services/).

* Kapacitu úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

* Začít s nasazením a náročné instance pomocí HPC Pack v systému Windows, najdete v článku [Nastavení Windows RDMA obrázku s HPC Pack spouštění aplikací MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Informace o používání instance A8 a A9 spouštění aplikací MPI s listem Azure najdete v tématu [použití více instancí úkoly spouštění aplikací rozhraní předávání zpráv (MPI) v Azure dávku](../batch/batch-mpi.md).
