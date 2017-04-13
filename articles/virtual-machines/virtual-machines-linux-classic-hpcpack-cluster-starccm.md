<properties
 pageTitle="Spuštění HVĚZDA-CCM + s aktualizací Service Pack HPC na Linux VMs | Microsoft Azure"
 description="Nasazení Microsoft HPC Pack clusteru v Azure a spuštění HVĚZDA-CCM + úlohy na více Linux výpočet uzly přes síť RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Spuštění HVĚZDA-CCM + s Microsoft HPC Pack na Linux RDMA cluster v Azure
Tento článek popisuje, jak pro nasazení Microsoft HPC Pack clusteru v Azure a spustit [HVĚZDA CD adapco-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) úlohy uzlech více Linux výpočetním, které jsou propojeny InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack poskytuje funkce ke spuštění různých rozsáhlé HPC a paralelní aplikací, včetně aplikací MPI na clusterů virtuálních počítačích Microsoft Azure. HPC podporuje také spuštěné aplikace Linux HPC na Linux výpočetní uzly VMs nasazené v clusteru HPC Pack. Úvod k používání Linux výpočet uzly s aktualizací Service Pack HPC najdete v tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Nastavení obrázku HPC Pack
Stáhnout skripty nasazení HPC Pack IaaS z webu [Služby Stažení softwaru](https://www.microsoft.com/en-us/download/details.aspx?id=44949) a extrahovat místně.

Azure Powershellu je požadována. Pokud na místním počítači není nakonfigurován Powershellu, přečtěte si článek [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

Při psaní tohoto textu Linux obrázky z webu Azure Marketplace (obsahující InfiniBand ovladačů pro Azure) platí pro SLES 12, CentOS 6.5 a CentOS 7.1. Tento článek je založena na použití SLES 12. K získání jména všech Linux obrázky, které podporují HPC v Tržiště, spuštěním následujícího příkazu Powershellu:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Výstup seznamy umístění, ve kterém jsou k dispozici tyto obrázky a názvem obrázku (**ImageName**) se nemusí používat v šabloně nasazení později.

Před nasazením clusteru, budete muset vytvořit soubor HPC Pack nasazení šablony. Protože jsme budete při zacílení na malé clusteru hlavního uzlu bude řadiče domény a hostovat místní databázi SQL.

Následující šablonu nasazení hlavního uzlu, vytvoříte soubor XML s názvem **MyCluster.xml**a nahradit hodnoty **SubscriptionId** **StorageAccount**, **umístění**, **VMName**a **Název_služby** s vašimi změnami.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Zahájení vytváření hlavou uzel spuštěním příkazu Powershellu v příkazovém řádku:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Po 20 až 30 minut by měl být hlavou uzel připraven. Můžete připojit k němu z portálu Microsoft Azure kliknutím na ikonu **Připojit** virtuálního počítače.

Nakonec bude pravděpodobně nutné opravit server DNS pro předávání. K tomu, spusťte Správce DNS.

1.  Klikněte pravým tlačítkem na název serveru ve Správci DNS, vyberte možnost **Vlastnosti**a potom klikněte na kartu **předávání** .

2.  Klikněte na tlačítko **Upravit** odebrat všechny předávání a potom klikněte na **OK**.

3.  Ujistěte se, že je zaškrtnuté políčko **použít kořenové servery, pokud jsou k dispozici žádné předávání** a klikněte na tlačítko **OK**.

## <a name="set-up-linux-compute-nodes"></a>Nastavení Linux výpočet uzlů
Nasazení uzly výpočetním Linux pomocí stejné nasazení šabloně, která se použila k vytvoření hlavního uzlu.

Zkopírujte soubor **MyCluster.xml** ze svého místního počítače do hlavního uzlu a aktualizujte značku **NodeCount** počtu uzlů, které chcete nasadit (< = 20). Dávejte mít dost dostupné jádra v Azure kvótu, protože pokaždé A9 bude využívat 16 jádra předplatné. Pokud chcete použít další VMs ve stejném rozpočtu, můžete místo A9 použít A8 instance (8 jádra).

Na uzel hlavy zkopírujte skripty HPC Pack IaaS nasazení.

Na příkazovém řádku spusťte následující příkazy Powershellu Azure:

1.  Spusťte **Přidat AzureAccount** připojit k předplatnému Azure.

2.  Pokud máte víc předplatných, spusťte **Get-AzureSubscription** uvést.

3.  Nastavení výchozího předplatného spuštěním **Vyberte AzureSubscription - SubscriptionName xxxx-výchozí** příkaz.

4.  Spuštění **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** zahájíte nasazení Linux výpočet uzlů.

    ![Nasazení vedoucí uzel v akci][hndeploy]

Otevření nástroje pro správce clusteru HPC Pack. Po několika minutách Linux výpočetním uzly pravidelně se zobrazí v seznamu uzlů výpočetním clusteru. S režimem klasické nasazení IaaS VMs vzniká postupně. Takže pokud počtu uzlů je důležité, pak zobrazuje všechny nasazený může trvat značné množství času.

![Linux uzlů v Správce clusteru HPC Pack][clustermanager]

Teď, když všechny uzly zařídit i v clusteru, existují další infrastrukturu nastavení.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Nastavení Azure sdílené složky pro Windows a Linux uzel
Pomocí služby Azure souboru pro ukládání skripty balíčků aplikací a datové soubory. Azure soubor poskytuje možnosti CIFS nad úložiště objektů Blob Azure jako trvalý úložiště. Mějte na paměti, že není nejčastěji scalable řešení, ale je nejjednodušší a nevyžaduje vyhrazené VMs.

Vytvoření sdílené Azure podle pokynů uvedených v článku [Začínáme s úložiště souborů Azure v systému Windows](..\storage\storage-dotnet-how-to-use-files.md).

Zachovat název účtu úložiště jako **saname**názvu sdílení souboru jako **název_sdílené_položky**a klíč účtu úložiště jako **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Připojit Azure sdílené složky na uzel hlavy
Otevřete příkazový řádek se zvýšenými oprávněními a spusťte tento příkaz Uložit přihlašovací údaje v místním počítači trezoru:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Připojte Azure sdílené složky, spusťte:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Připojit Azure sdílené složky na Linux výpočetním uzlů
Jeden užitečným nástrojem, které jsou součástí sady HPC je nástroj clusrun. Stejný příkaz současně pracovat na sadu výpočetním uzly můžete tento nástroj příkazového řádku. V našem případě umožňuje připojit Azure sdílené složky a zachovat jeho i po restartování počítače.
Na příkazovém řádku na uzel hlavy spusťte následující příkazy.

Vytvoření adresáře připojení:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Chcete-li připojit Azure sdílené složky:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Chcete-li uchovat sdílet připojení:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Instalace HVĚZDA-CCM +
Azure OM instance A8 a A9 poskytují InfiniBand podporu a RDMA možnosti. Ovladače jádra podporující nástroj tyto funkce jsou k dispozici pro Windows Server 2012 R2, SUSE 12, CentOS 6.5 a CentOS 7.1 obrázky v Azure Marketplace. Microsoft MPI a Intel MPI (verze 5.x) se dvěma MPI knihoven, které podporují těchto ovladače v Azure.

HVĚZDA CD adapco-CCM + uvolněte 11.x a novější kombinovaný s verzí Intel MPI 5.x, aby InfiniBand podpora služby Azure je však započítávány.

Získání Linux64 HVĚZDA-balíček CCM + z [disku CD adapco portálu](https://steve.cd-adapco.com). V našem případě jsme použili verze 11.02.010 ve smíšených přesnosti.

Na uzel hlavy v **/hpcdata** Azure sdílené položky, vytvořte skript prostředí s názvem **setupstarccm.sh** s následující obsah. Tento skript se spustí v jednotlivých uzlech výpočetním nastavit HVĚZDA-CCM + místně.

#### <a name="sample-setupstarcmsh-script"></a>Ukázka skriptu setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nyní nastavit HVĚZDA-CCM + na všechny Linux výpočet uzly, otevřete příkazový řádek se zvýšenými oprávněními a spusťte tento příkaz:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Je spuštěná příkazu můžete sledovat využití procesoru pomocí tepelné mapy Správce clusteru. Po pár minut stačí by měl správně nastavit.

## <a name="run-star-ccm-jobs"></a>Spuštění HVĚZDA-CCM + úlohy
HPC Pack se používá k jeho možnosti Plánovač úloh spustit HVĚZDA-CCM + úlohy. K tomu, potřebujeme podporu několik skripty, které slouží ke spuštění úlohy a HVĚZDA-CCM +. Zadávání dat bude k dispozici ve sdílené složce Azure první pro zjednodušení.

Tento skript Powershellu slouží k fronty HVĚZDY-CCM + projektu. Trvá tři argumenty:

*  Název modelu

*  Počtu uzlů má být použita

*  Počet jádra v jednotlivých uzlech má být použita

Protože HVĚZDA-CCM + můžete vyplnit šířku pásma paměti je vhodnější používání méně jádra za výpočetním uzly a přidávání nových uzlů. Přesné číslo jádra na uzel závisí na procesorů a rychlosti propojení.

Uzly přiřazených výhradně pro daný úkol a nebude možné sdílet s ostatními úlohami. Úlohy není spuštěná jako MPI úlohu přímo. Skript prostředí **runstarccm.sh** začnou Spouštěči MPI.

Zadávání modelu a skript **runstarccm.sh** jsou uloženy v dialogu sdílet **/hpcdata** , která byla dříve připojili.

Soubory protokolu jsou s názvem s ID úlohy a jsou uloženy v **/hpcdata sdílení**, spolu s HVĚZDY-CCM + výstupní soubory.


#### <a name="sample-submitstarccmjobps1-script"></a>Ukázka skriptu SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Nahrazení **runner.java** upřednostňované hvězdičkou-CCM + Java modelu Spouštěč a kód protokolování.

#### <a name="sample-runstarccmsh-script"></a>Ukázka skriptu runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

V testovacím jsme použili token licenci Power na vyžádání. Pro tento token potřeba nejdřív nastavit proměnnou prostředí **$CDLMD_LICENSE_FILE** **1999@flex.cd-adapco.com** a klíč v **- podkey** možnost příkazového řádku.

Po pár inicializace extrahuje skript – z **$CCP_NODES_CORES** proměnné prostředí nastavené HPC Pack – seznam uzlů vytvářet hostfile používající Spouštěči MPI. Tento hostfile bude obsahovat seznamu výpočetním uzel názvy, které se používají pro danou úlohu názvů na řádku.

Formát **$CCP_NODES_CORES** používá se následující toto:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Kde je:

* `<Number of nodes>`je počtu uzlů přidělit tuto úlohu.

* `<Name of node_n_...>`stejný název jednotlivých uzlech přidělit tuto úlohu.

* `<Cores of node_n_...>`představuje počet jádra na uzel přidělit tuto úlohu.

Počet jádra (**$NBCORES**) je vypočítána také podle počtu uzlů (**$NBNODES**) a počet jejích jádra na uzel (zadané jako parametr **$NBCORESPERNODE**):

Možnosti MPI ty, které se používají s Intel MPI na Azure jsou:

*   `-mpi intel`Chcete-li zadat Intel MPI.

*   `-fabric UDAPL`použití Azure InfiniBand slovesa.

*   `-cpubind bandwidth,v`Pokud chcete optimalizovat šířky pásma pro MPI hvězdičkou-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Chcete-li pracovat s Azure InfiniBand MPI Intel a nastavit požadovaný počet jádra na uzel.

*   `-batch`Spusťte HVĚZDA-CCM + v režimu dávka s žádné uživatelské rozhraní.


Nakonec spuštění úlohy, zkontrolujte, že uzly jsou zařídit i a online ve Správci obrázku. Na příkazovém řádku prostředí PowerShell spusťte takto:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Ukončení uzlů
Později po s testy, můžete provádět následující příkazy Powershellu Pack HPC zastavení a spuštění uzly:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Další kroky
Spusťte jiných pracovního vytížení Linux. Například najdete v článku:

* [Spuštění NAMD s Microsoft HPC Pack na Linux výpočet uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Spuštění OpenFOAM s Microsoft HPC Pack clusteru Linux RDMA v Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
