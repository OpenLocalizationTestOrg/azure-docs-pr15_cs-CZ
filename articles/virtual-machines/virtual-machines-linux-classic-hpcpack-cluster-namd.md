<properties
 pageTitle="NAMD s Microsoft HPC Pack na Linux VMs | Microsoft Azure"
 description="Nasazení Microsoft HPC Pack clusteru v Azure a spustit simulace NAMD s charmrun více Linux výpočetním uzlů"
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
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Spuštění NAMD s Microsoft HPC Pack na Linux výpočet uzlů v Azure

Tento článek popisuje jedním ze způsobů v Azure virtuálních počítačích spustit úlohu výkonných výpočetních clusterů (HPC) Linux. Tady můžete nastavení [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) clusteru v Azure s Linux výpočetním uzly a spusťte [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulace pro výpočty a vizualizovat strukturu systému velké biomolekulárních.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (pro program molekulární Dynamics Nanoscale) je paralelní molekulární dynamics balíček sice pro výkonné simulace velké biomolekulárních systémů obsahujících maximálně miliony atomů. Tyto systémy příkladem viry, struktury buňky a velké bílkoviny. NAMD měřítko stovky vzorky pro typické simulace a víc než 500 000 vzorky pro největší simulace.

* **Microsoft HPC Pack** poskytuje funkce ke spuštění ve velkém měřítku HPC a paralelní aplikací v prostředí clusterů místního počítače nebo Azure virtuálních počítačích. Původně vyvinuté jako řešení pro Windows HPC úloh, HPC Pack nyní podporuje spuštěna Linux HPC aplikací Linux výpočet uzel VMs nasazenou v HPC Pack obrázku. Úvodní informace v tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

Další možnosti pro spuštění pracovního vytížení Linux HPC v Azure najdete v tématu [technické prostředky pro list a výkonných výpočetních](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Zjistit předpoklady pro

* **HPC Pack obrázku s Linux výpočet uzly** – nasazení clusteru HPC Pack s Linux výpočetním uzlů v Azure pomocí účtu [Správce prostředků Azure šablony](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript Powershellu Azure](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Naleznete v tématu [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md) požadavky a postup pro jednu z těchto možností. Pokud vyberete možnost nasazení skript Powershellu, přečtěte si článek ukázkový soubor konfigurace v ukázkové soubory na konci tohoto článku. Tento soubor nakonfiguruje založené na Azure HPC Pack cluster obsahuje hlavou uzel Windows serveru 2012 R2 a čtyři velikost velkých 6.6 CentOS výpočetním uzlů. Upravte tento soubor v případě potřeby ve vašem prostředí.


* **Soubory software a kurz NAMD** – softwaru stáhnout NAMD Linux z webu [NAMD](http://www.ks.uiuc.edu/Research/namd/) (registrace povinné). Tento článek je založená na NAMD verze 2.10 a používá archivu [Linux x86_64 (64bitová verze Intel/AMD s Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) . Také stahovat [soubory NAMD kurzu](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Soubory ke stažení případech jde o soubory .tar a musíte nástroj extrahujte soubory na uzel hlavy obrázku. Extrahujte soubory, postupujte podle pokynů dál v tomto článku. 

* **VMD** (volitelné) – a zobrazte výsledky NAMD práce, stáhněte a nainstalujte aplikaci molekulární vizualizace [VMD](http://www.ks.uiuc.edu/Research/vmd/) v počítači podle svého výběru. Aktuální verze není 1.9.2. V tématu VMD stáhnout webu můžete začít.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Nastavení vzájemného vztahu důvěryhodnosti mezi uzly výpočetním
Spuštění úlohy křížově uzel ve více uzlech Linux vyžaduje uzly s informacemi o důvěryhodnosti sobě (pomocí **rsh** nebo **ssh**). Při vytváření clusteru HPC Pack s skript pro nasazení Microsoft HPC Pack IaaS skript automaticky nastaví trvalé vzájemné zabezpečení pro účtu správce, který určíte. Uživatelé s oprávněními správce, které vytvoříte v doméně clusteru budete muset nastavit dočasné vzájemné vztah důvěryhodnosti mezi uzly při úlohy přidělení k nim. Potom destroy relaci po dokončení projektu. Postup pro každého uživatele zadejte dvojici RSA klíč k obrázku, který HPC Pack používá k vytvoření vztahu důvěryhodnosti. Postupujte podle pokynů.

### <a name="generate-an-rsa-key-pair"></a>Generování dvojici RSA klíč
Není těžké si generovat dvojici RSA klíč, která obsahuje veřejným klíčem a privátním klíčem spuštěním příkazu **ssh-keygen** Linux.

1.  Přihlaste se k počítači Linux.

2.  Spusťte tento příkaz:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Stisknutím klávesy **Enter** použít výchozí nastavení, dokud nebude dokončena příkazu. Není třeba zadávat heslo. Po zobrazení výzvy k zadání hesla, stačí stisknout **Enter**.

    ![Generování dvojici RSA klíč][keygen]

3.  Změnit adresář ~/.ssh adresář. Soukromý klíč uložený ve id_rsa a veřejný klíč id_rsa.pub.

    ![Veřejné a privátní klíče][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Přidání klíčových pár clusteru HPC Pack
1.  [Připojit tak, že Vzdálená plocha](virtual-machines-windows-connect-logon.md) na hlavní uzel OM doména nepoužívá pověření za předpokladu, že při nasazení obrázku (například hpc\clusteradmin). Správa clusteru z hlavního uzlu.

2. Umožňuje vytvořit uživatelský účet domény v doméně služby Active Directory clusteru standardní postupy systému Windows Server. Například nástrojem uživatel služby Active Directory a počítačů na uzel hlavy. Příklady v tomto článku se předpokládá, že vytvoříte uživatel domény s názvem hpcuser v doméně hpclab (hpclab\hpcuser).

3. Přidejte uživatele domény clusteru HPC Pack jako uživatel obrázku. Pokyny najdete v tématu [Přidání nebo odebrání clusteru uživatelů](https://technet.microsoft.com/library/ff919330.aspx).

2.  Vytvoření do souboru nazvaného C:\cred.xml a zkopírujte data klíče RSA do ní. Příklad můžete najít v ukázkové soubory na konci tohoto článku.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Otevřete příkazový řádek a zadejte tento příkaz Nastavit přihlašovací údaje data pro hpclab\hpcuser účtu. Použít parametr **extendeddata** předat název, který jste vytvořili souboru C:\cred.xml dat klíče.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Tento příkaz úspěšném bez výstupu. Po nastavení přihlašovacích údajů pro uživatelské účty, které potřebujete k spuštění úlohy, uložte soubor cred.xml na zabezpečeném místě nebo ji odstranit.

5.  Generovaného klíče pár RSA k některé z uzly Linux nezapomeňte po dokončení práce s nimi odstranit klíče. HPC Pack není vzájemné zabezpečení Pokud nastavte uvedenu existující soubor id_rsa nebo id_rsa.pub soubor.

>[AZURE.IMPORTANT] Nedoporučujeme spuštění úlohy Linux jako správce clusteru sdílené clusteru, protože spuštěná úloha odeslaný správce v části účet kořenové uzlech Linux. V části místní uživatelský účet Linux dlouhodobě spuštěná úloha odeslaný uživatelem, kteří nejsou správci se stejným názvem jako uživatel projektu. V tomto případě HPC Pack nastaví vzájemné zabezpečení pro tohoto uživatele Linux na všech uzlech přidělit projektu. Můžete nastavit uživatele Linux ručně uzlech Linux před spuštěním úlohy nebo HPC Pack uživateli automaticky vytvoří po odeslání projektu. Pokud HPC Pack vytvoří uživatele, HPC Pack jej odstraní, až se dokončí úloha. Snížit riziko zabezpečení, jsou klávesy odebrány až se dokončí úloha v jednotlivých uzlech.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Nastavení sdílení souborů pro Linux uzly

Nyní nastavení sdílení souborů SMB a připojit ke sdílené složce ve všech uzlech Linux umožňuje uzly Linux pro přístup k souborům NAMD s běžné cestou. Následují kroků můžete připojit sdílené složky na uzel hlavy. Sdílené doporučujeme distribuce například CentOS 6.6, které aktuálně nepodporují služby Azure souborů. Pokud uzly Linux podporují Azure sdílené složky, vás [naučí používat úložiště souborů Azure s Linux](../storage/storage-how-to-use-files-linux.md). Další možnosti sdílení s aktualizací Service Pack HPC souborů najdete v článku [Začínáme s Linux uzly výpočetním clusteru Pack HPC v Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Vytvoření složky na uzel hlavy a sdílejte ho můžete všem nastavením oprávnění pro čtení i zápis. V tomto příkladu \\ \\CentOS66HN\Namd je název složky, kde CentOS66HN je název hostitele hlavního uzlu.

2. Vytvoření nové podsložky s názvem namd2 ve sdílené složce. V namd2 vytvořte jiný podsložku s názvem namdsample.

3. Pomocí verze systému Windows **vkládání** nebo jiný nástroj, který pracuje na .tar archivy extrahujte NAMD soubory ve složce. 
    * Extrahování archivace vkládání NAMD na \\ \\CentOS66HN\Namd\namd2.
    
    * Extrahujte soubory kurzu ve skupinovém rámečku \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Otevřete okno prostředí Windows PowerShell a následující příkazy můžete připojit ke sdílené složce na uzly Linux.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Příkaz první vytvoří složku s názvem /namd2 ve všech uzlech ve skupině LinuxNodes. Druhý příkaz připojí //CentOS66HN/Namd/namd2 sdílenou složku na složku s dir_mode a file_mode bitů nastavit, 777. *Uživatelské jméno* a *heslo* v příkazu by měl být přihlašovací údaje uživatele na uzel hlavy.

>[AZURE.NOTE]"\`" Symbol příkaz v druhém je symbol řídicí pro PowerShell. "\`," znamená "," (znak čárky) je součástí příkazu.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Vytvořit skript flám spuštění úlohy NAMD

Práce NAMD musí souboru *seznamu* pro **charmrun** k určení počtu uzlů používat při spuštění NAMD procesů. Můžete použít flám skript, který vytváří souboru seznamu a spustí **charmrun** s tímto souborem seznamu. Odešlete NAMD úlohy v HPC Správce clusteru, která volá tento skript.

V textovém editoru podle svého výběru, vytvořit skript flám ve složce /namd2 obsahující NAMD typy souborů a nazvěte ji hpccharmrun.sh. Pro rychlé ověření koncepce zkopírovat příklad hpccharmrun.sh skriptu na konci tohoto článku a přejděte na [Odeslat NAMD projektu](#submit-a-namd-job).

>[AZURE.TIP] Uložit jako textový soubor s Linux skript konce (pouze LF, ne CR LF) řádků. Zajistíte tak, že správně běží v jednotlivých uzlech Linux.

Tady jsou podrobnosti o možnostech tento skript flám. 

1.  Definování některé proměnné.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Vyhledejte uzel informace z proměnné prostředí. $NODESCORES ukládá seznam slov rozdělení z $CCP_NODES_CORES. $COUNT je velikost $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Formát proměnné CCP_NODES_CORES $ vypadá takto:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Tuto proměnnou seznamy celkový počet uzly názvy uzlů a počet jádra v jednotlivých uzlech, které mají být úkoly přiřazovány. Například pokud úlohy potřebuje 10 jádra spustit, $CCP_NODES_CORES hodnotu podobně jako:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Pokud není nastavit proměnnou CCP_NODES_CORES $, začněte **charmrun** přímo. (Pouze dojde při spuštění tento skript přímo na uzly Linux.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Nebo vytvořit soubor seznamu pro **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Spusťte **charmrun** se souborem seznamu, zpáteční stav a odeberte souboru seznamu na konci.

    {CCP_NUMCPUS} je jiné proměnné prostředí nastavil hlavou uzel HPC Pack. Uloží počet celkové jádra přidělit tuto úlohu. Ji používáme k určení počtu procesů pro charmrun.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Ukončete se stavem vrácení **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Následuje informace v seznamu soubor, který vytváří skript:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Příklad:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Odeslání NAMD úlohy

Teď jste připraveni k odeslání NAMD úlohy v Správce clusteru HPC.

1.  Připojení k hlavního uzlu obrázku a spusťte Správce clusteru HPC.

2.  V části **Správa zdrojů**zajištění uzly výpočetním Linux do stavu **Online** . Pokud nejsou, vyberte je a klikněte na **Přepnout do režimu Online**.

2.  V části **Správa úloh**klikněte na **Nový projekt**.

3.  Zadejte název projektu, například *hpccharmrun*.

    ![Nová úloha HPC][namd_job]

4.  Na stránce **Podrobnosti projektu** v seznamu **Zdrojů projektu**vyberte typ zdroje jako **uzel** a nastavte **minimální** na 3. , jsme spuštění úlohy na tři uzly Linux a jednotlivých uzlech má čtyři jádra.

    ![Práce zdrojů][job_resources]

5. V levém navigačním panelu klikněte na **Upravit úkoly** a potom klikněte na tlačítko **Přidat** pro přidání úkolu do projektu.    


6. Na stránce **podrobností úkolu a přesměrování v/v** nastavení na tyto hodnoty:

    * **Přepínač příkazového řádku** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Předchozí přepínač příkazového řádku je jediný příkaz bez konce řádků. Zalamuje zobrazit několik řádků v části **příkazového řádku**.

    * **Pracovní adresář** - /namd2

    * **Minimální** - 3

    ![Podrobnosti úkolu][task_details]

    >[AZURE.NOTE] Můžete nastavit pracovní adresář tady protože **charmrun** snaží přejděte k adresáři pracovní v jednotlivých uzlech. Pokud není nastavená pracovní adresář, HPC Pack spustí příkaz Složka náhodně názvem vytvořené v jednom uzlů Linux. To způsobí, že tato chyba v jednotlivých uzlech: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` problému můžete předejít, zadejte cestu ke složce, můžete k nim získat přístup tak, že všechny uzly jako pracovní adresář.

5.  Klikněte na tlačítko **OK** a potom kliknutím na možnost **Odeslat** tuto úlohu.

    Ve výchozím nastavení odešle HPC Pack jako aktuální přihlášený uživatelský účet. Dialogové okno vás může požádat o zadání uživatelského jména a hesla po klepnutí na tlačítko **Odeslat**.

    ![Přihlašovací údaje projektu][creds]

    Za určitých podmínek HPC Pack si pamatuje, informace o uživateli před při zadávání a nemá toto dialogové okno zobrazit. Chcete-li znovu zobrazit Pack HPC, zadejte tento příkaz příkazového řádku a odešlete projektu.

    ```command
    hpccred delcreds
    ```

6.  Úlohy trvá několik minut dokončete.

7.  Vyhledání protokolu úlohy v \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log a výstupní soubory v \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  V případě potřeby spusťte VMD zobrazíte výsledky projektu. Postup pro vizualizaci NAMD výstupní soubory (v tomto případě ubiquitin bílkovin molekuly v oblasti vodu) jsou nad rámec tohoto článku. Další informace v tématu [Kurz NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Výsledky projektu][vmd_view]

## <a name="sample-files"></a>Ukázkové soubory

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Konfigurační soubor XML ukázka obrázku nasazení skript Powershellu

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Ukázka skriptu hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
