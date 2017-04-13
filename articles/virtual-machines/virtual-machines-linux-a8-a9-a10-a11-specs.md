<properties
 pageTitle="Informace o náročné VMs s Linux | Microsoft Azure"
 description="Získat základní informace a důležité informace týkající se používání náročné velikosti H řady a A8 A9, A10 a A11 pro Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informace o H řady a náročné VMs A řady 

Tady je základní informace a několik tipů pro používání novější Azure řady H a starší A8, A9, A10 a A11 velikosti, nazývaný také *náročné* instance. Tento článek se zaměřuje na pomocí těchto velikostí pro Linux VMs. Tento článek je také k dispozici pro [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Přístup k síti RDMA

Můžete vytvořit clusterů, spustit jednu z možností podporovaná Linux HPC rozdělení a podporované implementaci MPI umožní využít výhod sítě Azure RDMA podporou RDMA VMs Linux. Naleznete v tématu [nastavení clusteru Linux RDMA spouštění aplikací MPI](virtual-machines-linux-classic-rdma-cluster.md) možnosti nasazení a kroky konfigurace vzorku.

* **Distribuce** – je nutné nasadit VMs ze služby obrázky podporou RDMA SUSE Linux Enterprise Server (SLES) nebo na základě OpenLogic CentOS HPC v Azure Marketplace. Pouze na následujících obrázcích Marketplace dál nebude podporovat potřebné ovladače Linux RDMA:

    * SLES 12 SP1 HPC, SLES 12 SP1 HPC (Premium)
    
    * SLES 12 HPC, SLES 12 HPC (Premium)
    
    * Na základě centOS 7.1 HPC
    
    * Na základě centOS 6.5 HPC
    
    >[AZURE.NOTE]H řady VMs doporučujeme buď SP1 12 SLES HPC obrázku nebo CentOS 7.1 HPC obrázků.
    >
    >Na základě CentOS HPC obrázky budou zakázány aktualizace jádra v souboru konfigurace **yum** . Je to proto jsou Linux RDMA ovladače distributed jako balíček ot a aktualizace ovladačů nemusí fungovat v případě jádra se aktualizuje.

* **MPI** - Intel MPI knihovny 5.x

    V závislosti na Tržiště obrázku se rozhodnete, samostatná licence, instalace, nebo konfigurace Intel MPI může být potřeba následujícím způsobem: 
    
    * **SLES 12 SP1 pro HPC obrázek** – instalace balíčků Intel MPI distribuované na OM spuštěním následujícího příkazu:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **12 SLES pro obrázek HPC** - samostatně zaregistrujte si stáhněte a nainstalujte doplněk MPI. V tématu [Průvodce instalací Intel MPI knihovny](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Obrázky na základě centOS HPC** - Intel MPI 5.1 je už nainstalovaná.  

    Konfigurace další systémové je potřeba spustit MPI úlohy na skupinový VMs. Například clusteru VMs, budete muset vytvořit vztah důvěryhodnosti mezi uzly výpočetním. Typické nastavení najdete v článku [nastavení clusteru Linux RDMA spouštění aplikací MPI](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Co zvážit při HPC Pack a Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), bezplatné clusteru HPC společnosti Microsoft a řešení úlohy správy obsahuje jednu možnost použít instance náročné s Linux. Nejnovější verze HPC Pack 2012 R2 podpory Linux rozdělení dělat výpočet uzly nasazenou v Azure VMs spravuje hlavního uzlu systému Windows Server. S podporou RDMA Linux výpočetním uzly systém Intel MPI HPC Pack plánování a pořádání Linux MPI aplikace, které přístup k síti RDMA. Pokud chcete začít pracovat, najdete v článku [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Aspekty topologie sítě

* Na RDMA s podporou VMs Linux v Azure Eth1 vyhrazená pro RDMA v síti. Změna nastavení Eth1 nebo všechny informace v souboru konfigurace odkazující na této sítě. Eth0 je rezervován pro běžná Azure v síti.

* V Azure nepodporuje IP přes InfiniBand (i). Je podporována pouze RDMA přes i.

## <a name="rdma-driver-updates-for-sles-12"></a>Aktualizace ovladačů RDMA SLES 12

Po vytvoření OM podle obrázku SLES 12 HPC může potřebujete aktualizovat ovladače RDMA na VMs RDMA připojení k síti. 

>[AZURE.IMPORTANT]Tento krok je **potřeba** pro SLES 12 HPC OM nasazení ve všech oblastech Azure. 
>Tento krok není potřeba při nasazování SLES 12 SP1 HPC, na základě CentOS 7.1 HPC nebo na základě CentOS 6.5 HPC OM. 

Před aktualizací ovladače vypnout všechny procesy **zypper** nebo procesů, které zamknout databází repo SUSE v OM. V opačném případě ovladače nemusí aktualizovat správně.  

Aktualizujte ovladače Linux RDMA na každý OM, spusťte jednu z následujících sad příkazů Azure rozhraní příkazového řádku z klientského počítače.

**V klasickém nasazení modelu zřízení SLES 12 HPC OM**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 HPC OM zřízení v modelu nasazení Správce prostředků**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Může trvat delší dobu instalaci ovladačů a na příkaz vrátí bez výstupu. Po instalaci aktualizace vaší OM restartuje a by měl být připraven k použití v několik minut.

### <a name="sample-script-for-driver-updates"></a>Ukázka skriptu pro ovladač aktualizace

Pokud máte clusteru SLES 12 HPC VMs, je aktualizovat ovladač skriptu na všech uzlech clusteru. Například následující skript aktualizuje ovladače v 8 uzel obrázku.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Další kroky

* Podrobnosti o dostupnosti a ceny velikostí náročné najdete v tématu [ceny virtuálních počítačích](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Kapacitu úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

* Na začátku zavádění a používání náročné velikosti s RDMA na Linux najdete v článku [nastavení clusteru Linux RDMA spouštění aplikací MPI](virtual-machines-linux-classic-rdma-cluster.md).


