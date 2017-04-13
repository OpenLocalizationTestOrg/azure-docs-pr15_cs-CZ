<properties
 pageTitle="Spustit OpenFOAM s aktualizací Service Pack HPC Linux VMs | Microsoft Azure"
 description="Nasazení Microsoft HPC Pack clusteru v Azure a spusťte OpenFOAM úlohy ve více uzlech výpočetním Linux přes síť RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Spuštění OpenFoam s Microsoft HPC Pack clusteru Linux RDMA v Azure

Tento článek popisuje jedním ze způsobů spuštění OpenFoam Azure virtuálních počítačích. Tady můžete nasadit Microsoft HPC Pack clusteru s Linux výpočetním uzlů v Azure a spustit [OpenFoam](http://openfoam.com/) práce s Intel MPI. Můžete provádět podporou RDMA VMs Azure uzly výpočetním tak, aby uzly výpočetním komunikovat v síti Azure RDMA. Další možnosti pro spuštění OpenFoam v Azure zahrnout plně nakonfigurovaném komerční obrázky dostupné Marketplace, jako je třeba jeho UberCloud [OpenFoam 2.3 na CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)a spuštěním v [Azure listu](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (pro operace otevřete pole a manipulace) je balíček software otevřít zdroj výpočetní dutá dynamics (CFD), který je používán široce inženýrské a jiné vědecké v komerční a vzdělávací organizace. Obsahuje nástroje pro meshing, zejména snappyHexMesh, parallelized mesher složité geometrie CAD a odstranění před a po zpracování. Skoro všech procesů spustit paralelně uživatelům umožňuje využít výhod hardwaru k dispozici.  

Microsoft HPC Pack poskytuje funkce ke spuštění ve velkém měřítku HPC a paralelní aplikací, včetně aplikací MPI na clusterů virtuálních počítačích Microsoft Azure. HPC podporuje také pracovního Linux HPC aplikací na Linux výpočet uzel VMs nasazení v HPC Pack obrázku. Úvod k používání Linux výpočetním uzly s HPC Pack naleznete v tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

>[AZURE.NOTE] Tento článek ukazuje, jak provádět Linux MPI úlohu s aktualizací Service Pack HPC. Předpokládá, že máte některé znalosti s Linux systému správy a MPI úloh spuštěna clusterů Linux. Pokud používáte verzích MPI a OpenFOAM liší od těch, které jsou uvedené v tomto článku, pravděpodobně změnit některé kroky instalace a konfigurace. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

*   **HPC Pack obrázku s podporou RDMA Linux výpočet uzly** – nasazení HPC Pack obrázku s velikost A8, A9, H16r nebo H16rm Linux výpočetním uzlů pomocí [šablony správce prostředků Azure](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript Powershellu Azure](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Naleznete v tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md) požadavky a postup pro jednu z těchto možností. Pokud vyberete možnost nasazení skript Powershellu, přečtěte si článek ukázkový soubor konfigurace v ukázkové soubory na konci tohoto článku. Pomocí této konfigurace nasazení založené na Azure HPC Pack cluster obsahuje velikost hlavního uzlu A8 Windows serveru 2012 R2 a 2 velikost A8 SUSE Linux Enterprise Server 12 výpočetním uzlů. Nahradit hodnoty odpovídající názvy předplatného a přihlašovacích údajů. 

    **Další co je třeba vědět**

    *   Linux RDMA sítě požadavky v Azure najdete v článku [o H řady a náročné VMs A řadu](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Pokud použijete možnost nasazení skript Powershellu, nasazení všechny uzly výpočetním Linux v rámci jedné cloudové služby RDMA síťové připojení používají.

    *   Po zavedení uzly Linux připojte pomocí SSH provádět další úkoly správy. Najděte podrobné informace o připojení SSH pro každý Linux OM Azure portálu.  
        
*   **Procesor Intel MPI** - dělat OpenFOAM SLES 12 HPC výpočet uzlů v Azure, musíte si nainstalovat modul runtime Intel MPI knihovny 5 z [webu Intel.com](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 je předinstalovaný v na základě CentOS HPC obrázky).  Později v případě potřeby nainstalujte doplněk MPI uzly výpočetním Linux. Příprava na tento krok po registraci s Intel, kliknutím na odkaz v e-mailové potvrzení související webovou stránku. Zkopírujte odkaz ke stažení souboru .tgz pro příslušný verzi Intel MPI. Tento článek je založena na Intel MPI verze 5.0.3.048.

*   **OpenFOAM zdroj Pack** – stažení softwaru OpenFOAM zdroj Pack Linux z [webu OpenFOAM Foundation](http://openfoam.org/download/2-3-1-source/). Tento článek je založena na zdrojovém balíku verzi 2.3.1, k dispozici ke stažení jako OpenFOAM 2.3.1.tgz. Postupujte podle pokynů dál v tomto článku rozbalit a kompilace OpenFOAM uzly výpočetním Linux.

*   **EnSight** (volitelné) - zobrazte výsledky OpenFOAM simulace, stáhněte a nainstalujte aplikaci [EnSight](https://www.ceisoftware.com/download/) vizualizace a analýzy. Informace o licencování a stahování se v EnSight webu.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Nastavení vzájemného vztahu důvěryhodnosti mezi uzly výpočetním

Spuštění úlohy křížově uzel ve více uzlech Linux vyžaduje uzly s informacemi o důvěryhodnosti sobě (pomocí **rsh** nebo **ssh**). Při vytváření clusteru HPC Pack s skript pro nasazení Microsoft HPC Pack IaaS skript automaticky nastaví trvalé vzájemné zabezpečení pro účtu správce, který určíte. Uživatelé s oprávněními správce, které vytvoříte v doméně clusteru můžete nastavit dočasné vzájemné vztah důvěryhodnosti mezi uzly, když je přidělit úlohy a destroy relaci po dokončení projektu. Abyste mohli vytvořit zabezpečení pro každého uživatele, zadejte dvojici RSA klíč clusteru používající HPC Pack pro vztah důvěryhodnosti.

### <a name="generate-an-rsa-key-pair"></a>Generování dvojici RSA klíč

Není těžké si generovat dvojici RSA klíč, která obsahuje veřejným klíčem a privátním klíčem spuštěním příkazu **ssh-keygen** Linux.

1.  Přihlaste se k počítači Linux.

2.  Spusťte tento příkaz:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Stisknutím klávesy **Enter** použít výchozí nastavení, dokud nebude dokončena příkazu. Není třeba zadávat heslo. Po zobrazení výzvy k zadání hesla, stačí stisknout **Enter**.

    ![Generování dvojici RSA klíč][keygen]

3.  Změnit adresář ~/.ssh adresář. Soukromý klíč uložený ve id_rsa a veřejný klíč id_rsa.pub.

    ![Veřejné a privátní klíče][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Přidání klíčových pár clusteru HPC Pack
1.  Zkontrolujte připojení ke vzdálené ploše hlavy uzel s účtem správce HPC Pack (správce, které jste nastavili při spuštění skript pro nasazení).

2. Umožňuje vytvořit uživatelský účet domény v doméně služby Active Directory clusteru standardní postupy systému Windows Server. Například nástrojem uživatel služby Active Directory a počítačů na uzel hlavy. Příklady v tomto článku se předpokládá, že vytvoříte uživatel domény s názvem hpclab\hpcuser.

3.  Vytvoření do souboru nazvaného C:\cred.xml a zkopírujte data klíče RSA do ní. Ukázkový soubor cred.xml je na konci tohoto článku.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Otevřete příkazový řádek a zadejte tento příkaz Nastavit přihlašovací údaje data pro hpclab\hpcuser účtu. Použít parametr **extendeddata** předat název souboru C:\cred.xml, který jste vytvořili pro data klíče.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Tento příkaz úspěšném bez výstupu. Po nastavení přihlašovacích údajů pro uživatelské účty, které potřebujete k spuštění úlohy, uložte soubor cred.xml na zabezpečeném místě nebo ji odstranit.

5.  Generovaného klíče pár RSA k některé z uzly Linux nezapomeňte po dokončení práce s nimi odstranit klíče. Pokud HPC Pack najde existující id_rsa soubor nebo soubor id_rsa.pub, není nastavte si vzájemné zabezpečení.

>[AZURE.IMPORTANT] Nedoporučujeme spuštění úlohy Linux jako správce clusteru sdílené clusteru, protože spuštěná úloha odeslaný správce v části účet kořenové uzlech Linux. Však odeslaný uživatelem, kteří nejsou správci dlouhodobě spuštěná úloha v části místní uživatelský účet Linux se stejným názvem jako uživatel projektu. V tomto případě HPC Pack nastaví vzájemné zabezpečení pro tohoto uživatele Linux mezi uzly přidělit projektu. Můžete nastavit uživatele Linux ručně uzlech Linux před spuštěním úlohy nebo HPC Pack uživateli automaticky vytvoří po odeslání projektu. Pokud HPC Pack vytvoří uživatele, HPC Pack jej odstraní, až se dokončí úloha. Snížit hrozeb zabezpečení, odebere HPC Pack klávesy po dokončení projektu.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Nastavení sdílení souborů pro Linux uzly

Nyní nastavení standardní SMB sdílet ve složce na uzel hlavy. Umožňuje uzly Linux pro přístup k souborům aplikace s běžné cestou připojte ke sdílené složce uzlech Linux. Pokud chcete, můžete použít jinou možnost, třeba sdílení souborů Azure – pro mnoho scénáře - nebo sdílet NFS pro sdílení souborů. Viz sdílení informace a podrobné pokyny v [začít pracovat s Linux uzly výpočetním clusteru Pack HPC v Azure](virtual-machines-linux-classic-hpcpack-cluster.md)souborů.

1.  Vytvoření složky na uzel hlavy a sdílejte ho můžete všem nastavením oprávnění pro čtení i zápis. Například sdílet C:\OpenFOAM na uzel hlavy jako \\ \\SUSE12RDMA HN\OpenFOAM. *SUSE12RDMA kola HN* následuje název hostitele hlavou uzel.

2.  Otevřete okno prostředí Windows PowerShell a následující příkazy:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Příkaz první vytvoří složku s názvem /openfoam ve všech uzlech ve skupině LinuxNodes. Druhý příkaz připojí //SUSE12RDMA-HN/OpenFOAM sdílené složky v jednotlivých uzlech Linux s dir_mode a file_mode bitů nastavit, 777. *Uživatelské jméno* a *heslo* v příkazu by měl být přihlašovací údaje uživatele na uzel hlavou.

>[AZURE.NOTE]"\`" Symbol příkaz v druhém je symbol řídicí pro PowerShell. "\`," znamená "," (znak čárky) je součástí příkazu.

## <a name="install-mpi-and-openfoam"></a>Instalace MPI a OpenFOAM

Provádět úlohy MPI OpenFOAM RDMA síti se systémem budete muset kompilaci OpenFOAM s knihovnami Intel MPI. 

Nejdřív provádět několik **clusrun** příkazy k instalaci knihoven Intel MPI (pokud už není nainstalovaná) a OpenFOAM uzly Linux. Použití hlavního uzlu sdílení dříve nakonfigurované tak, aby sdílení instalačních souborů mezi uzly Linux.

>[AZURE.IMPORTANT]Tyto kroky kompilace a instalaci příkladů. Potřebujete znalosti správy systému Linux zajistit správně nainstalovanou závislá kompilátoru a knihovny. Možná budete muset změnit některé proměnné nebo jiná nastavení verze Intel MPI a OpenFOAM. Podrobnosti najdete v tématu [Intel MPI knihovny Průvodce instalací Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) a [Instalace sady zdroj OpenFOAM](http://openfoam.org/download/2-3-1-source/) ve vašem prostředí.


### <a name="install-intel-mpi"></a>Nainstalovat doplněk MPI

Uložení staženého instalačního balíčku MPI Intel (l_mpi_p_5.0.3.048.tgz v tomto příkladu) v C:\OpenFoam na uzel hlavy tak, aby uzly Linux můžete k tomuto souboru přístup z /openfoam. Spusťte **clusrun** nainstalovat doplněk MPI knihovny v jednotlivých uzlech Linux.

1.  Následující příkazy Kopírovat instalační balíček a extrahujte do /opt/intel v jednotlivých uzlech.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Pokud chcete nainstalovat doplněk MPI knihovny tiše, pomocí souboru silent.cfg. Příklad můžete najít v ukázkové soubory na konci tohoto článku. Umístěte tento soubor /openfoam sdílenou složku. Podrobnosti o souboru silent.cfg najdete v tématu [Intel MPI knihovny Průvodce instalací Linux - používaného](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Ujistěte se, že uložením souboru silent.cfg jako textový soubor s Linux konce řádků, (pouze LF, ne CR LF). Tento krok zajistí, že správně běží v jednotlivých uzlech Linux.

3.  Nainstalujte doplněk MPI knihovny v pasivní režimu.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Konfigurace MPI

Testování, měli byste přidat následující řádky do /etc/security/limits.conf jednotlivých uzlech Linux:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Po aktualizaci souboru limits.conf restartujte uzly Linux. Například použijte příkaz **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Po restartování počítače, ujistěte se, že na sdílenou složku připojen jako /openfoam.

### <a name="compile-and-install-openfoam"></a>Kompilace a nainstalujte OpenFOAM

Uložte stažený instalační balíček pro zdroj Pack OpenFOAM (OpenFOAM-2.3.1.tgz v tomto příkladu) do C:\OpenFoam na uzel hlavy uzly Linux můžete k tomuto souboru přístup z /openfoam. Spusťte **clusrun** příkazy sestavíte OpenFOAM v jednotlivých uzlech Linux.


1.  Vytvoření složky /opt/OpenFOAM v jednotlivých uzlech Linux, zkopírujte zdrojový balíček do této složky a extrahovat tam.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Kompilace OpenFOAM s knihovnou MPI Intel, musíte nejdřív nastavit některé proměnné Intel MPI a OpenFOAM. Pomocí skriptu flám s názvem settings.sh nastavení proměnných. Příklad můžete najít v ukázkové soubory na konci tohoto článku. Umístěte tento soubor (uloží spolu se konce řádků Linux) /openfoam sdílenou složku. Tento soubor obsahuje taky nastavení pro MPI a OpenFOAM runtimes využívající později OpenFOAM úlohu spustíte.

3. Nainstalujte závislá balíčků potřebné pro zpracování OpenFOAM. V závislosti na vaší distribuční Linux může musíte nejdřív přidání úložiště. Spusťte **clusrun** příkazy podobná této:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    V případě potřeby SSH jednotlivých uzlech Linux spustit příkazy k potvrzení, že bezchybnou funkčnost.

4.  Spusťte tento příkaz sestavíte OpenFOAM. Proces kompilace trvá některé dobu a vygeneruje velké množství informací protokolu standardní výstup, proto použijte možnost **/ interleaved** zobrazit výstup interleaved.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]"\`" Symbol nebo u příkazu je symbol řídicí pro PowerShell. "\`&" znamená, že "&" je součástí příkazu.

## <a name="prepare-to-run-an-openfoam-job"></a>Příprava ke spuštění úlohy OpenFOAM

Příprava teď spustit MPI úlohu s názvem sloshingTank3D, což je jedno vzorků OpenFoam, na dvou uzlů Linux. 

### <a name="set-up-the-runtime-environment"></a>Nastavení prostředí runtime

Nastavení prostředí runtime MPI a OpenFOAM uzlech Linux, spusťte tento příkaz v okně prostředí Windows PowerShell na uzel hlavy. (Tento příkaz je platný pro SUSE Linux pouze.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Příprava ukázkových dat

Použijte příkaz sdílet hlavního uzlu, která odrážejí dříve konfigurované ke sdílení souborů mezi uzly Linux (připojen jako /openfoam).

1.  SSH do jedné ze svého Linux výpočet uzlů.

2.  Pokud jste to ještě neudělali, spusťte tento příkaz nastavit prostředí runtime OpenFOAM.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Zkopírujte ukázková sloshingTank3D ke sdílené složce a přejděte k němu.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Při použití výchozí parametry v tomto příkladu, může to trvat desítky minut spustíte tak můžete chtít změnit některé parametry snažíme usnadnit jeho rychleji. Jednu volbu simple je můžete změnit čas krok proměnné deltaT a writeInterval v souboru systému/controlDict. Tento soubor obsahuje všechny zadávání dat řízení času a čtení a zápis dat řešení. Je třeba změnit hodnotu deltaT z 0,05 násobek 0,5 a hodnotu writeInterval z 0,05 násobek 0,5.

    ![Úprava kroku proměnných][step_variables]

5.  Zadejte požadované hodnoty pro proměnné v souboru systému/decomposeParDict. V tomto příkladu je dva Linux uzly každý s 8 jádra, takže nastavte numberOfSubdomains 16 a n hierarchicalCoeffs k (1 1 16), což znamená, že spustit OpenFOAM souběžně s 16 procesy. Další informace najdete v tématu [OpenFOAM uživatelská příručka: 3.4 spuštění aplikace paralelně](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Rozložit procesů][decompose]

6.  Spusťte následující příkazy z adresáře sloshingTank3D Příprava ukázkových dat.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Na uzel hlavy byste měli vidět, že ukázkové datové soubory se zkopírují do C:\OpenFoam\sloshingTank3D. (C:\OpenFoam je sdílená složka na uzel hlavou.)

    ![Datové soubory na uzel hlavy][data_files]

### <a name="host-file-for-mpirun"></a>Soubor Host pro mpirun

V tomto kroku vytvoříte soubor hostitele (seznam výpočetním uzlů), který příkaz **mpirun** používá.

1.  Na jeden z uzlů Linux vytvořte do souboru nazvaného hostfile v části /openfoam, abyste tento soubor zastižení /openfoam/hostfile ve všech uzlech Linux.

2.  Napište názvy uzlů Linux do tohoto souboru. V tomto příkladu soubor obsahuje tyto názvy:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Můžete taky vytvořit tento soubor na C:\OpenFoam\hostfile na uzel hlavy. Pokud vyberete tuto možnost, uložte ho jako textový soubor s Linux konce řádků (pouze LF, ne CR LF). Zajistíte tak, že správně běží v jednotlivých uzlech Linux.

    **Flám skript obálky**

    Pokud máte hodně uzly Linux a chcete práce dělat jenom některé z nich, není vhodné použít pevné hostitele souboru, protože neznáte uzlů, které se mají přidělit práce. V tomto případě psaní skriptů Obálka flám pro **mpirun** automaticky vytvořit soubor Host (hostitel). Můžete vyhledat příklad flám skript obálky s názvem hpcimpirun.sh na konci tohoto článku a uložit jako /openfoam/hpcimpirun.sh. Tento příklad skript dělá toto:

    1.  Nastaví proměnné **mpirun**a některé parametry příkazového sčítání úlohu MPI spustíte přes síť RDMA. V tomto případě nastaví následující proměnné:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = odstřižení v2 ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Vytvoří soubor hostitele podle prostředí proměnná $CCP_NODES_CORES, který je nastaven hlavy uzel HPC při aktivaci projektu.

        Formát $CCP_NODES_CORES používá se následující toto:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        kde

        * `<Number of nodes>`-počtu uzlů přidělit tuto úlohu.  
        
        * `<Name of node_n_...>`– název jednotlivých uzlech přidělit tuto úlohu.
        
        * `<Cores of node_n_...>`-počet jádra na uzel přidělit tuto úlohu.

        Například pokud úlohy vyžaduje dva uzly spustit, $CCP_NODES_CORES je podobný
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Příkaz **mpirun** volá a přidá dvěma parametry příkazového řádku.

        * `--hostfile <hostfilepath>: <hostfilepath>`-cestu k souboru hostitele skript vytvoří

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-Proměnná prostředí nastavil hlavního uzlu HPC Pack, který ukládá počet celkové jádra přidělit tuto úlohu. V tomto případě určuje počet procesů pro **mpirun**.


## <a name="submit-an-openfoam-job"></a>Odeslání OpenFOAM úlohy

Nyní je možné odeslat úlohy v Správce clusteru HPC. Potřebujete předat hpcimpirun.sh skriptu na příkazovém řádku pro některé úkoly projektu.

1. Připojení k hlavního uzlu obrázku a spusťte Správce clusteru HPC.

2. **Správa zdrojů**, zajistit, aby uzly výpočetním Linux do stavu **Online** . Pokud nejsou, vyberte je a klikněte na **Přepnout do režimu Online**.

3.  V části **Správa úloh**klikněte na **Nový projekt**.

4.  Zadejte název projektu, například _sloshingTank3D_.

    ![Podrobnosti projektu][job_details]

5.  V **projektu zdrojů**vyberte typ zdroje jako"" a nastavte minimální 2. Konfigurace spustí úlohu uzlech dvou Linux, z nichž každá obsahuje osm jádra v tomto příkladu.

    ![Práce zdrojů][job_resources]

6. V levém navigačním panelu klikněte na **Upravit úkoly** a potom klikněte na tlačítko **Přidat** pro přidání úkolu do projektu. Přidání čtyř úkolů do projektu s následujícími příkazového řádku a nastavení.

    >[AZURE.NOTE]Spuštění `source /openfoam/settings.sh` nastaví runtime prostředí OpenFOAM a MPI tak všechny následující úkoly volá před příkazem OpenFOAM.

    *   **Úkol 1**. Spusťte **decomposePar** generovat datových souborů pro spuštění **interDyMFoam** souběžně.
    
        *   K úkolu přiřadit jeden uzel

        *   **Přepínač příkazového řádku** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Pracovní adresář** - / openfoam/sloshingTank3D
        
        Viz následující obrázek. Konfigurace zbývající úkoly podobně.

        ![Podrobnosti úkolu 1][task_details1]

    *   **Úkol 2**. Spusťte **interDyMFoam** paralelně pro výpočet vzorku.

        *   K úkolu přiřadit dvou uzlů

        *   **Přepínač příkazového řádku** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Pracovní adresář** - / openfoam/sloshingTank3D

    *   **Úkol 3**. Spusťte **reconstructPar** výše uvedené množiny adresářů času z adresáře každý processor_N_ sloučit do jedné sady.

        *   K úkolu přiřadit jeden uzel

        *   **Přepínač příkazového řádku** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Pracovní adresář** - / openfoam/sloshingTank3D

    *   **Úkol 4**. Spusťte **foamToEnsight** paralelně soubory OpenFOAM výsledek převést do formátu EnSight a uskutečňovat EnSight soubory v adresáři Ensight v adresáři změnu.

        *   K úkolu přiřadit dvou uzlů

        *   **Přepínač příkazového řádku** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Pracovní adresář** - / openfoam/sloshingTank3D

6.  Přidání závislosti k těmto úkolům ve vzestupném pořadí úkolů.

    ![Závislosti mezi úkoly][task_dependencies]

7.  Kliknutím na možnost **Odeslat** tuto úlohu.

    Ve výchozím nastavení odešle HPC Pack jako aktuální přihlášený uživatelský účet. Po klepnutí na tlačítko **Odeslat**, může se zobrazit dialogové okno s výzvou k zadání uživatelského jména a hesla.

    ![Přihlašovací údaje projektu][creds]

    Za určitých podmínek HPC Pack si pamatuje, informace o uživateli před při zadávání a nemá toto dialogové okno zobrazit. Chcete-li znovu zobrazit Pack HPC, zadejte tento příkaz příkazového řádku a odešlete projektu.

    ```
    hpccred delcreds
    ```

8.  Úlohy přenese z desítek minut několik hodin podle parametrů, které jste nastavili pro vzorku. Do tepelné mapy se zobrazí úlohy spuštěné v jednotlivých uzlech Linux. 

    ![Tepelná mapa][heat_map]

    V jednotlivých uzlech osm procesy jsou spuštěné.

    ![Linux procesů][linux_processes]

9.  Po dokončení projektu úlohy výsledky hledání ve složkách pod C:\OpenFoam\sloshingTank3D a souborů na C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Zobrazit výsledky v EnSight

Pokud chcete umožňuje [EnSight](https://www.ceisoftware.com/) vizualizace a analýza výsledků OpenFOAM úlohy. Další informace o vizualizace a animace v EnSight najdete v článku tato [videa Průvodce](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Po nainstalování EnSight na uzel hlavou, spusťte ho.

2.  Otevřete C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Zobrazí nádrži v prohlížeči.

    ![Nádrži v EnSight][tank]

3.  Vytvoření **Isosurface** z **internalMesh**a pak zvolte proměnné **alpha_water**.

    ![Vytvoření isosurface][isosurface]

4.  Nastavení barvy pro **Isosurface_part** vytvořili v předchozím kroku. Například nastavte ji na vodu modrá.

    ![Úprava isosurface barvy][isosurface_color]

5.  **Hlasitost Iso** z **zdi** můžete vytvořit výběrem **zdi** v **části** panelu a klikněte na tlačítko **Isosurfaces** na panelu nástrojů.

6.  V dialogovém okně vyberte **Typ** jako **Isovolume** a nastavte Min **Isovolume oblast** násobek 0,5. Pokud chcete vytvořit isovolume, klikněte na **vytvořit s vybraných částí**.

7.  Nastavení barvy pro **Iso_volume_part** vytvořili v předchozím kroku. Například nastavte hloubkové vodu modrá.

8.  Nastavení barvy **zdí**. Například nastavte průhledné bílé.

9. Nyní klikněte na tlačítko **Přehrát** a zobrazte výsledky simulace.

    ![Výsledek nádrži][tank_result]

## <a name="sample-files"></a>Ukázkové soubory

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Konfigurační soubor XML ukázka obrázku nasazení skript Powershellu

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Ukázkový soubor cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Ukázkový soubor silent.cfg nainstalovat MPI

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Ukázka skriptu settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Ukázka skriptu hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
