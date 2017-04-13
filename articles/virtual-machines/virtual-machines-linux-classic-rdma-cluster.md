<properties
 pageTitle="Shluk Linux RDMA spouštění aplikací MPI | Microsoft Azure"
 description="Vytvoření clusteru Linux velikosti H16r, H16mr, A8 nebo A9 VMs použití sítě Azure RDMA pro spuštění aplikace MPI"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Nastavení spuštění aplikace MPI Linux RDMA obrázku


Zjistěte, jak nastavit clusteru Linux RDMA v Azure s [H řady nebo náročné VMs A řadu](virtual-machines-linux-a8-a9-a10-a11-specs.md) spouštění aplikací paralelní rozhraní předávání zpráv (MPI). Tento článek obsahuje kroky pro přípravu obrázku Linux HPC spuštění Intel MPI clusteru. Pak můžete nasadit clusteru VMs pomocí tento obrázek a jednu velikostí podporou RDMA OM Azure (aktuálně H16r, H16mr, A8 nebo A9). Použití clusteru pro spuštění aplikace MPI efektivně komunikovat s nízkou latencí, vysoký výkon sítě podle vzdálené přímý přístup do paměti technologii (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Možnosti nasazení obrázku

Tady jsou metody můžete použít k vytvoření clusteru Linux RDMA s nebo bez Plánovač úloh.


* **Rozhraní příkazového řádku azure skripty** – jak je znázorněno na pozdější v tomto článku pomocí [rozhraní Azure příkazového řádku](../xplat-cli-install.md) (rozhraní příkazového řádku) skriptu nasazení clusteru podporou RDMA VMs. Uzlů rozhraní příkazového řádku v režimu služba Správa sériově vytvoří v modelu klasické nasazení, takže nasazení mnoho uzlů výpočetním může trvat několik minut. Povolit připojení k síti RDMA při použití klasické nasazení modelu, nasazení VMs ve stejném cloudové služby.

* **Správce prostředků azure šablony** - můžete také nasazení modelu správce prostředků pro nasazení clusteru podporou RDMA VMs, ke kterému je připojen k síti RDMA. Můžete [vytvořit vlastní šablonu](../resource-group-authoring-templates.md), nebo [Azure rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/) pro šablony přispěla společností Microsoft nebo komunity nasazení řešení se má vrátit. Správce prostředků šablony můžete umožňují rychlé a spolehlivé nasazení clusteru Linux. Povolit připojení k síti RDMA při použití nasazení modelu správce prostředků, nasazení VMs v sadě stejné dostupnosti.

* **HPC Pack** – vytvoření clusteru Microsoft HPC Pack v Azure a přidejte podporou RDMA uzly výpočetním spuštěné podporované Linux rozdělení pro přístup k síti RDMA. V tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Ukázka kroků nasazení klasické modelu

Podle těchto kroků předvedení použití rozhraní příkazového řádku Azure nasadit virtuálního SUSE Linux Enterprise Server (SLES) 12 SP1 HPC počítače z Azure Marketplace, přizpůsobit a vytvořit vlastní obrázek OM. Pak můžete obrázek skriptu nasazení clusteru podporou RDMA VMs. 

>[AZURE.TIP]Podobným způsobem nasazení clusteru podporou RDMA VMs založené na jiných podporované HPC obrázky v Azure Marketplace. Některé kroky může mírně, jak je uvedeno. Například Intel MPI zahrnuté se konfigurovat jenom některé z těchto obrázky. A pokud nasadíte SLES 12 HPC OM místo SLES 12 SP1 HPC OM, budete muset aktualizovat ovladače RDMA. Podrobnosti najdete v tématu [o A8 A9, A10 a A11 náročné instance](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Zjistit předpoklady pro

*   **Klientský počítač** - budete potřebovat Mac, Linux nebo klienta se systémem Windows počítač komunikovat Azure. Tento postup předpokládá, že používáte klienta Linux.

*   **Azure předplatné** – Pokud nemáte předplatné, můžete vytvořit [účet zase uvolnit](https://azure.microsoft.com/free/) v jenom pár minut. U větších clusterů zvažte systému průběžného financování předplatné nebo další možnosti nákupu. 

*   **Dostupnost velikost OM** – aktuálně následující formáty instance jsou RDMA může: H16r, H16mr, A8 a A9. Zkontrolujte [dostupnost podle regionů produktů](https://azure.microsoft.com/regions/services/) dostupnost v Azure oblastí. 

*   **Kvóta jádra** - může být nutné zvětšit kvóty jádra nasazení clusteru náročné VMs. Například potřebujete alespoň 128 jádra Pokud si chcete nasadit 8 A9 VMs podle pokynů v tomto článku. Vaše předplatné může taky omezení počtu jádra nainstalované řady určitých OM velikost, včetně H řady. Požádat o kvótu zvýšit, [Otevřete žádost o konverzaci online Zákaznická podpora](../azure-supportability/how-to-create-azure-support-request.md) zdarma. 

*   **Azure rozhraní příkazového řádku** - [instalace](../xplat-cli-install.md) Azure rozhraní příkazového řádku a [připojení k předplatnému Azure](../xplat-cli-connect.md) z klientského počítače.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Krok 1. Zřízení OM HPC 12 SP1 SLES

Po přihlášení do Azure pomocí rozhraní příkazového řádku Azure, spusťte `azure config list` potvrďte, že výstup ukazuje služba Správa režimu. Pokud není, nastavte režim spuštěním tento příkaz:


    azure config mode asm


Zadejte seznam všech předplatných, které můžete použít:


    azure account list

Aktuální aktivní předplatné je označen `Current` nastavena na `true`. Není-li toto předplatné tu, kterou chcete použít k vytvoření clusteru, nastavte příslušný předplatné Id jako aktivní předplatné:

    azure account set <subscription-Id>

Zobrazení veřejně dostupný SLES 12 SP1 HPC obrázků v Azure, spustit podobně jako tento příkaz, za předpokladu, že vaše prostředí prostředí podporuje **grep**:


    azure vm image list | grep "suse.*hpc"

Teď zřízení podporou RDMA OM s obrázkem SLES 12 SP1 HPC spuštěním podobně jako tento příkaz:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

kde

* Velikost (A9 v tomto příkladu) rovná jedné podporou RDMA OM velikostí.

* Externí číslo portu SSH (22 v tomto příkladu je výchozí SSH) je libovolné platné číslo portu. Vnitřní číslo portu SSH nastavenou 22.

* Nové cloudové služby je vytvořit v oblasti Azure určený podle místa. Zadejte umístění, ve kterém je k dispozici velikost OM, které zvolíte.

* SLES 12 SP1 obrázek aktuálně lze název `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` nebo `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` podporu Priorita SUSE (další poplatky.).

### <a name="step-2-customize-the-vm"></a>Krok 2. Přizpůsobení OM

Po dokončení OM zřizování SSH angličtině pomocí OM externí IP adresy (nebo název DNS) a externí port číslo, které se nakonfigurované a upravit ho. Podrobné informace o připojení najdete v článku [jak přihlášení k virtuálního počítače systém Linux](virtual-machines-linux-mac-create-ssh-keys.md). Zastupování v příkazy uživatele, kterého jste nakonfigurovali ve OM, pokud kořenové přístup je potřebná k dokončení kroku.

>[AZURE.IMPORTANT]Microsoft Azure neposkytuje kořenové přístup k Linux VMs. Pokud chcete získat přístup pro správu když připojení jako uživatel, bude v angličtině, spusťte příkazy pomocí `sudo`.

* **Aktualizace** – pomocí **zypper**instalovat aktualizace. Můžete taky nainstalovat NFS nástroje. 

    >[AZURE.IMPORTANT]V SLES 12 SP1 HPC virtuálního počítače doporučujeme, aby se nevztahují jádra aktualizacích, které mohou způsobit problémy s Linux RDMA ovladače.

* **Procesor Intel MPI** - dokončit instalaci Intel MPI na SLES 12 SP1 HPC OM spuštěním následujícího příkazu:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Uzamčení paměti** – kódy pro MPI uzamknout paměť k dispozici pro RDMA, přidat nebo změnit následující nastavení v souboru /etc/security/limits.conf. (Potřebujete kořenové přístup k úpravám tohoto souboru.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Pro účely testování můžete vytvořit také memlock neomezené. Příklad: `<User or group name>    hard    memlock unlimited`. Další informace najdete v tématu [Známé nejlepší způsoby nastavení velikosti paměti uzamčený](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **Klávesové zkratky SSH pro SLES VMs** - generovat SSH klávesy se šipkami stanovit zabezpečení pro svůj uživatelský účet mezi uzly výpočetním clusteru SLES při spuštění úlohy MPI. (Pokud jste nainstalovali na základě CentOS HPC OM, nepostupujte podle tohoto kroku. V tématu pokyny dále v tomto článku nastavit passwordless SSH vztah důvěryhodnosti mezi uzly clusteru po zachytit a nasazení clusteru.) 

    Spusťte tento příkaz Vytvořit SSH klíče. Po zobrazení výzvy k zadání vstupních hodnot, stiskněte klávesu Enter k vygenerování klíčů ve výchozím umístění nechci nastavovat přístupové heslo.

        ssh-keygen

    Veřejný klíč připojení k souboru authorized_keys známé veřejné klíče.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    V adresáři ~/.ssh upravit nebo vytvořit soubor "konfigurace". Zadejte rozsah IP adres privátní sítě, který chcete použít v Azure (10.32.0.0/16 v tomto příkladu):

        host 10.32.0.*
        StrictHostKeyChecking no

    Můžete taky seznam privátní sítě IP adresu každého OM svůj cluster následujícím způsobem:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfigurace `StrictHostKeyChecking no` můžete vytvořit riziko zabezpečení, pokud není zadán konkrétní IP adresa nebo oblast.

* **Aplikace** – nainstalujte aplikace, budete potřebovat v tomto OM nebo před musíte napřed zachytit provést další úpravy.

### <a name="step-3-capture-the-image"></a>Krok 3. Zachycení obrázku

Zachyťte obrázek, nejdřív spusťte tento příkaz v OM Linux. Tento příkaz deprovisions OM, ale zachová uživatelských účtů a SSH klíčů, které jste vytvořili.

```
sudo waagent -deprovision
```

V klientském počítači spusťte následující příkazy rozhraní příkazového řádku Azure zachyťte obrázek. Podrobnosti najdete v části [Jak sbírat klasické Linux virtuálního počítače jako obrázek](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Po spuštění tyto příkazy pro použití se zaznamenává OM obrázek a OM odstraněna. Nyní máte k dispozici vlastní obrázek chtít nasazení clusteru.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Krok 4. Nasazení obrázku s obrázkem

Upravte následující flám skript s příslušnými hodnotami ve vašem prostředí a spuštění z klientského počítače. Protože Azure nasadí VMs sériově v modelu klasické nasazení, bude několik minut nasazení 8 A9 VMs doporučovány tento skript.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Co zvážit při CentOS HPC obrázku

Pokud chcete nastavit clusteru na základě jedné obrázků na základě CentOS HPC v Azure Marketplace místo SLES 12 pro HPC, postupujte podle obecných kroků v předchozí části. Poznámka: následující rozdíly při zřizování a konfigurace OM:

1. Procesor Intel MPI je už nainstalovaná na OM zřízení od založených na CentOS HPC obrázek. 

2. Nastavení zámku paměti jste už přidali OM /etc/security/limits.conf souboru.

2. Negeneruje SSH klávesám OM zřizujete pro zachycení. Místo toho doporučujeme, abyste nastavení ověřování na základě uživatele po nasadit cluser. Následující části.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Nastavení passwordless SSH zabezpečení na clusteru

Na základě CentOS HPC clusteru, existují dva způsoby pro vztah důvěryhodnosti mezi uzly výpočetním: ověřování na základě Host (hostitel) a ověřování na základě uživatele. Ověřování na základě hostitele je mimo rozsah v tomto článku a obecně musí provést prostřednictvím skriptu rozšíření během nasazení. Ověřování na základě uživatele je užitečné pro vytvoření zabezpečení po nasazení a vyžaduje generování a sdílení klíčů SSH mezi uzly výpočetním clusteru. Tento způsob je známý jako passwordless SSH přihlášení a je nutný při spuštění úlohy MPI. 

Ukázka skriptu přispěla od komunity je dostupná na [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) povolit uživateli snadný ověřování na základě CentOS HPC obrázku. Stáhněte si a použijte tento skript pomocí následujících kroků. Můžete také upravit tento skript nebo použít jiné metody stanovit passwordless SSH ověřování mezi uzly výpočetním clusteru.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Spuštění skriptu, je potřeba vědět předponu IP adres podsítí. Pokud potřebujete předponu spuštěním následujícího příkazu k některé z uzlů. Výstup by měl vypadat nějak 10.1.3.5 a předpona je 10.1.3 část.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Teď spustit skript, pomocí tří parametrů: běžné uživatelské jméno v jednotlivých uzlech výpočetním, společné heslo pro daného uživatele v uzly výpočetním a podsítě předponu, která vrátil příkaz předchozí.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Tento skript dělá toto:

* Vytvoří složku na hostitele uzel s názvem .ssh, který je potřebný pro passwordless přihlášení. 
* Vytvoří soubor konfigurace v adresáři .ssh nastaví passwordless přihlášení chcete povolit přihlášení z libovolného uzlu clusteru. 
* Vytvoří soubory obsahující uzel jména a adresy IP uzel pro všechny uzly clusteru. Tyto soubory zůstávají po spuštění skriptu měli kdykoliv při ruce. 
* Vytvoří veřejné a privátní klíče pár pro každý uzel obrázku včetně uzel Host (hostitel) a vytvoří v souboru authorized_keys položky.

>[AZURE.WARNING]Spustit tento skript můžete vytvořit riziko zabezpečení. Ujistěte se, že není distribuovaná veřejné důležitých informací v ~/.ssh.


## <a name="configure-intel-mpi"></a>Konfigurace Intel MPI

Spustit MPI aplikace na Azure Linux RDMA, musíte nakonfigurovat některé proměnné specifické pro Intel MPI. Tady je ukázka flám skript, který konfigurace proměnné potřebné ke spuštění aplikace. Změna cesty mpivars.sh v případě potřeby instalace Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Formát souboru hostitele je následujícím způsobem. Přidáte jeden řádek pro každý uzel clusteru. Určení vyžadovaného soukromé IP adres z VNet definované dříve, nikoli názvy DNS. Například pro dva hosts s IP adresy 10.32.0.1 a 10.32.0.2 soubor obsahuje následující:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Spuštění MPI základní clusteru dvěma uzly

Pokud jste to ještě neudělali, nejdřív nastavte prostředí pro Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Jednoduchý MPI příkaz Spustit

Jednoduchý MPI příkaz spusťte na jeden z uzlů výpočetním zobrazíte, že MPI se instaluje správně a můžete komunikaci mezi nejméně, že dvě výpočet uzlů. Příkaz **mpirun** spustí příkaz **hostname** na dvou uzlů.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Výstup by měl seznam jmen všech uzlů, které předáváte jako vstup pro `-hosts`. Příkaz **mpirun** s dvěma uzly vrátí například výstup podobně jako tento:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Spuštění MPI srovnávacích

Příkaz Intel MPI spustí pingpong srovnávacích ověřte konfiguraci clusteru a připojení k síti RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Clusteru práce s dvěma uzly byste měli vidět výstup podobně jako tento. V síti Azure RDMA očekávají, že latence na nebo pod 3 mikrosekundách pro zprávu s velikostí 512 bajtů.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Další kroky

* Zkuste nasazení a spouštění aplikací Linux MPI clusteru Linux.

* Přečtěte si článek [Intel MPI knihovny si přečtěte následující dokumentaci](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) pro pokyny na Intel MPI.

* Zkuste [šablony rychlý úvod](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) k vytvoření clusteru Intel Lustre pomocí obrázku na základě CentOS HPC. Podrobnosti najdete v tématu tento [příspěvek blogu](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
