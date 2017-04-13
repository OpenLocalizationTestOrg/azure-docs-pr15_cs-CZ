<properties
 pageTitle="Nastavení Windows RDMA clusteru spouštění aplikací MPI | Microsoft Azure"
 description="Naučte se vytvářet Windows HPC Pack obrázku s velikostí H16r, H16mr, A8 nebo A9 VMs použití sítě Azure RDMA pro spuštění aplikace MPI."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Nastavení Windows RDMA obrázku s aktualizací Service Pack HPC spouštění aplikací MPI

Nastavení Windows RDMA obrázku v Azure s [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) a [H řady nebo náročné A řadu instancí](virtual-machines-windows-a8-a9-a10-a11-specs.md) spouštění aplikací paralelní rozhraní předávání zpráv (MPI). Když nastavíte podporou RDMA, systémem Windows Server uzlů v HPC Pack clusteru, MPI aplikací komunikovat efektivní zhoršeným latence vysoký výkon sítě v Azure, který je založený na technologii vzdálené přímé paměti aplikace access (RDMA).

Pokud chcete spustit MPI úloh Linux VMs, přístup k síti Azure RDMA, najdete v článku [nastavení clusteru Linux RDMA spouštění aplikací MPI](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>Možnosti nasazení clusteru HPC Pack
Microsoft HPC Pack je nástroj za předpokladu, že bezplatně vytvořit HPC clusterů místní nebo v Azure ke spuštění systému Windows nebo Linux HPC aplikací. Sada HPC obsahuje prostředí runtime pro implementaci Microsoft zprávu předá rozhraní pro Windows ([MS-MPI). Při použití s podporou RDMA instance podporované operační systém Windows Server, HPC Pack umožňuje efektivně spuštění aplikace Windows MPI přístup k síti Azure RDMA. 

Tento článek uvádí dva scénářů a odkazy na podrobné pokyny k nastavení Windows RDMA obrázku s Microsoft HPC Pack. 

* Scénář 1. Nasazení náročné pracovní role instance (PaaS)

* Scénář 2. Nasazení výpočetním uzlů v náročné VMs (IaaS)

Obecné požadavky pomocí náročné instancí služby systému Windows najdete v článku [o H řady a náročné VMs A řadu](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scénář 1. Nasazení náročné pracovní role instance (PaaS)

Z existujícího HPC Pack clusteru přidání navíc výpočetním zdrojů v Azure pracovníka role instance (Azure uzly) spuštěné v cloudové službě (PaaS). Tato funkce taky se mu říká "shlukové na Azure" z HPC Pack podporuje škála rozměrů instancí pracovního role. Při přidávání Azure uzly, stačí zadejte jeden podporou RDMA velikostí.

Následující splněno roztržení k podporou RDMA Azure instance z existující (obvykle místní) obrázku. Slouží k přidání role instancí pracovního HPC Pack hlavy uzlu nasazenou v Azure OM podobná procedury.

>[AZURE.NOTE] Kurz roztržení Azure s aktualizací Service Pack HPC najdete v článku [Nastavení hybridního obrázku s aktualizací Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Všimněte si, co byste měli zvážit v následujících krocích specifické pro podporou RDMA Azure uzlů.

![Shlukové Azure][burst]

### <a name="steps"></a>Postup

4. **Nasazení a konfiguraci hlavního uzlu HPC Pack 2012 R2**

    Stáhněte instalační balíček nejnovější HPC Pack z [Webu služby Stažení softwaru](https://www.microsoft.com/download/details.aspx?id=49922). Požadavky a pokyny pro přípravu nasazení Azure požadavků najdete v tématu [HPC Pack příručky Začínáme](https://technet.microsoft.com/library/jj884144.aspx) a [požadavků na instancí pracovního Azure pomocí Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Konfigurace certifikátu správy v Azure předplatného**

    Konfigurace certifikátu zabezpečení připojení mezi hlavního uzlu a Azure. Možnosti a postupy najdete v tématu [scénáře konfigurace certifikátu Azure Management Pack HPC](http://technet.microsoft.com/library/gg481759.aspx). Zkušební nasazení HPC Pack nainstaluje výchozí Microsoft HPC Azure správy certifikát můžete rychle nahrávat k předplatnému Azure.

6. **Vytvoření nového Cloudová služba a účet úložiště**

    Pomocí portálu Azure klasické můžete vytvářet Cloudová služba a účet úložiště pro nasazení v oblasti, kde jsou k dispozici podporou RDMA instance.

7. **Vytvoření šablony e Azure uzel**

    Použití uzel Průvodce vytvořením šablony v Správce clusteru HPC. Postup najdete v tématech [Vytvoření šablony e Azure uzel](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) sady"kroky k nasazení Azure uzly s Microsoft HPC".

    Pro počáteční testy měli byste konfigurace zásad ruční dostupnost v šabloně.

8. **Přidání uzlů clusteru**

    Použití Průvodce přidáním uzlu Správce clusteru HPC. Další informace najdete v tématu [Přidání Azure uzlů clusteru HPC Windows](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Při určování velikost uzly, vyberte jednu z podporou RDMA velikosti instance.
    
    >[AZURE.NOTE]V jednotlivých požadavků na Azure nasazení s instancí náročné HPC Pack automaticky nasadí aspoň 2 podporou RDMA instancí (například A8) jako proxy uzly kromě instancí role Azure pracovníka zadanými. Uzly proxy používat jádra, které jsou přidělené předplatné a vzniknou poplatky spolu s rolí instancí Azure pracovníka.

9. **Zahájení (poskytování) uzly a předkládat online spuštění úloh**

    Vyberte a pomocí akce **Spustit** Správce clusteru HPC. Po dokončení zřizování vyberte je a správce **Přenést Online** akce v HPC obrázku. Uzly připraveni k provádění úloh.

10. **Odeslání úlohy clusteru**

    Spuštění úlohy obrázku pomocí nástrojů odeslání úlohy HPC Pack. V tématu [Microsoft HPC Pack: úlohy správy](http://technet.microsoft.com/library/jj899585.aspx).

11. **Ukončení (identitách) uzlů**

    Po dokončení pracovního úlohy, uzly jako offline a použijte akci **Zastavit** Správce clusteru HPC.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scénář 2. Nasazení výpočetním uzlů v náročné VMs (IaaS)

V tomto scénáři nasadíte hlava uzel HPC Pack a clusteru výpočet uzlů v VMs připojen k Active Directory domain Azure virtuální sítě. HPC Pack poskytuje několik [Možnosti nasazení v Azure VMs](virtual-machines-linux-hpcpack-cluster-options.md), včetně automatické nasazení skripty a Azure rychlý úvod šablony. Jako příklad aspektech a podle následujících kroků vás pomocí [skriptu nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) většina tento proces zautomatizovat.

![Shluk v Azure VMs][iaas]



### <a name="steps"></a>Postup

1. **Vytvořte hlavní clusteru a výpočet uzel VMs spuštěním skript pro nasazení HPC Pack IaaS v klientském počítači**

    Stáhněte balíček HPC Pack IaaS nasazení skriptu z [Webu služby Stažení softwaru](https://www.microsoft.com/download/details.aspx?id=49922).

    Příprava klientského počítače, vytvořte konfigurační soubor skriptu a následujícím způsobem, najdete v tématu [Vytvoření clusteru HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Nasazení podporou RDMA výpočetním uzlů v úvahu následující skutečnosti Další:
    
    * **Virtuální sítě** - určete nový virtuální sítě v oblasti, ve kterém je k dispozici podporou RDMA velikosti instance chcete použít.

    * **Operační systém Windows Server** - podporují RDMA připojení, určete operačním systémem Windows Server 2012 R2 nebo Windows Server 2012 pro výpočetní uzly VMs.

    * **Cloudovým službám** – doporučujeme nasazení hlavy uzlů v jedné cloudové službě a výpočetním uzlů v jiné cloudové službě.

    * **Hlavy uzel velikost** – v tomto scénáři vzít v úvahu velikost aspoň A4 (velký) pro hlavy uzel.

    * **Rozšíření HpcVmDrivers** - skript pro nasazení nainstaluje Agent OM Azure a rozšíření HpcVmDrivers automaticky při nasazení velikost A8 nebo A9 výpočet uzly operačním systému Windows Server. HpcVmDrivers nainstaluje ovladače výpočetní uzly VMs tak, aby se mohli přihlásit k síti RDMA.

    * **Konfigurace sítě clusteru** – skript pro nasazení automaticky nastaví HPC Pack obrázku v topologii 5 (všech uzlů v podnikové síti). Tato topologie je potřebný pro všechna nasazení clusteru HPC Pack v VMs. Se nemění topologie sítě clusteru později.

2. **Spuštění úlohy přenesení uzly výpočetním online**

    Vyberte a správce **Přenést Online** akce v HPC obrázku. Uzly připraveni k provádění úloh.

3. **Odeslání úlohy clusteru**

    Připojení k hlavního uzlu posílat úkoly nebo nastavení místním počítači, můžete to udělat. Informace najdete v tématu [Odeslání úlohy HPC obrázku v Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Uzly jako offline a zastavit (zrušit) je**

    Po dokončení pracovního úlohy, trvat Správce clusteru HPC uzlů v offline režimu. Pak pomocí nástroje pro správu Azure vypnout.



## <a name="run-mpi-applications-on-the-cluster"></a>Spustit MPI aplikace na clusteru

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Příklad: Spuštění mpipingpong clusteru HPC Pack

Potvrďte nasazení HPC Pack podporou RDMA instancí příkaz HPC Pack **mpipingpong** na clusteru. **mpipingpong** odešle paketů dat mezi párových uzly opakovaně k výpočtu latence a měření výkon a Statistika sítích RDMA s podporou aplikace. Tento příklad ukazuje typické vzor pro spuštění MPI úlohy (v tomto případě **mpipingpong**) pomocí příkazu **mpiexec** obrázku.

V tomto příkladě se předpokládá, že jste přidali Azure uzlů v konfiguraci "požadavků na Azure" ([Scénář 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Pokud jste nainstalovali HPC Pack clusteru Azure VMs, musíte upravit syntaxe příkazu zadejte skupinu různých uzel a nastavte další proměnné prostředí tak, aby sítě směrovaly do sítě RDMA.


Spuštění mpipingpong pro clusteru:


1. Hlavy uzel nebo na správně nakonfigurovaném klientský počítač otevřete příkazový řádek.

2. K odhadu latenci mezi dvojice uzlů v Azure požadavků nasazení 4 uzlů, zadejte tento příkaz odeslání úlohy na spustit mpipingpong velikost malé paketu velkého počtu iterací:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Příkaz vrátí ID projektu, ze kterého se odešle.

    Pokud jste nainstalovali clusteru HPC Pack nasazený na Azure VMs uzel skupinu, která obsahuje výpočet uzel VMs nasazení v jedné cloudové službě a upravte příkazu **mpiexec** následujícím způsobem:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Až se dokončí úloha zobrazit výstup (v tomto případě výstup úkolu 1 úlohy), zadejte tento příkaz

    ```
    task view <JobID>.1
    ```

    kde &lt; *JobID* &gt; je ID práce, která byla odeslána.

    Výstup bude obsahovat výsledky latence podobně jako tento.

    ![Latence pong ping][pingpong1]

4. K odhadu výkon mezi dvojice uzlů Azure požadavků, zadejte tento příkaz odeslání úlohy na Spustit **mpipingpong** velikost velkých paketu malým počtem poštovních iterací:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Příkaz vrátí ID projektu, ze kterého se odešle.

    Na HPC Pack clusteru nasazený na Azure VMs upravte příkaz, jak je uvedeno v kroku 2.

5. Až se dokončí úloha zobrazit výstup (v tomto případě výstup úkolu 1 úlohy), zadejte následující příkaz:

    ```
    task view <JobID>.1
    ```

  Výstup bude obsahovat výsledky výkon podobně jako tento.

  ![Příkaz ping pong výkon][pingpong2]


### <a name="mpi-application-considerations"></a>Co byste měli zvážit MPI aplikace


Tady jsou aspektech MPI aplikacích systému s aktualizací Service Pack HPC v Azure. Některé platí pouze pro nasazení Azure uzlů (pracovní role instancí přidali v konfiguraci "požadavků na Azure").

* Role instancí pracovního v do cloudové služby jsou pravidelně reprovisioned bez předchozího upozornění tak, že Azure (například pro správu systému, nebo v případě, kdy se nezdaří instance). Pokud instance je reprovisioned ho je spuštěná úloha MPI, instance dojde ke ztrátě dat a vrátí stavu, pokud ho nejdřív nasazené, což může způsobit úlohy MPI selhání. Spustí další uzly, že použijete pro jeden projekt MPI a tím již úlohu, tím větší je pravděpodobnost, že jeden instancí bude reprovisioned při práci se systémem. Tento postup byste měli zvážit také v případě určit načítání v nasazení jako souborovém serveru.


* Spustit MPI úlohy v Azure, nemusíte použít podporou RDMA instance. Můžete použít libovolnou velikost instance podporovaný HPC Pack. Však podporou RDMA instance jsou vám doporučené spuštěním relativně rozsáhlé MPI úloh, které jsou závislé na latence a šířku pásma sítě, ke kterému je připojen uzlů. Pokud používáte ostatní formáty spustit latence a šířky pásma citlivé MPI úlohy, doporučujeme spouštět malé úloh, ve kterých běží jednoho úkolu na jenom několik uzlů.

* Nasazené na Azure instance se vztahují licenčních podmínek přidružené k aplikaci. Obraťte se na dodavatele komerční žádost o licencování nebo jiných omezení pro práci v cloudu. Ne všechny dodavatelé nabízejí systému průběžného financování licence.


* Azure instance potřebujete další nastavení uzly místní přístup, jejich počet a servery licencí. Povolit Azure uzly pro přístup k serveru místní licence, například můžete nakonfigurovat webu webu Azure virtuální sítě.


* Ke spuštění MPI aplikací v Azure instance, zaregistrujte každé aplikaci MPI s Brána Firewall systému Windows na instancí spuštěním příkazu **hpcfwutil** . Díky MPI sdělení proběhnout portu, které se přiřadí dynamické bránou firewall.

    >[AZURE.NOTE] Pro požadavků na Azure nasazení můžete taky nakonfigurovat příkaz výjimky brány firewall pro spouští automaticky u všech nové uzlů Azure, které se přidají do vašeho obrázku. Po spuštění příkazu **hpcfwutil** a zkontrolujte, že aplikace pracuje, přidejte tento příkaz spuštění skriptu pro Azure uzlů. Další informace najdete v tématu [použití spouštěcího skriptu pro Azure uzlů](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack používá proměnnou CCP_MPI_NETMASK clusteru prostředí Pokud chcete zadat rozsah přijatelné adres MPI komunikace. Spuštění v HPC Pack 2012 R2, proměnnou CCP_MPI_NETMASK clusteru prostředí ovlivní pouze MPI komunikace mezi uzly výpočetním clusteru doméně (buď místně nebo v Azure VMs). Bude proměnná ignorována uzly přidané v shluk Azure konfiguraci.


* Úlohy MPI nejde spustit přes Azure instancí nasazené v jiné cloudové služby (například požadavků na Azure nasazení s jinou uzel šablony nebo uzly výpočetním Azure OM nasazenou v několika cloudovým službám). Pokud máte víc nasazení Azure uzel zahájené šablonami různých uzel projektu MPI spuštěním na pouze jednu sadu Azure uzlů.


* Když přidáte Azure uzly svůj cluster a přenést online, Plánovač úloh HPC okamžitě pokusí o spuštění úlohy v jednotlivých uzlech. Pokud pouze část vaše pracovní zátěž mohlo by umožnit spuštění na Azure, zkontrolujte aktualizace nebo vytvoření šablony úlohy definujte, co typy úloh mohlo by umožnit spuštění na Azure. Třeba zajistit úlohy odeslaný šablony úlohy pouze bezchybnou Azure uzlech vlastnost uzel skupiny přidat do šablony úlohy a vyberte AzureNodes jako požadovanou hodnotu. Vytvořit vlastní skupiny pro uzly Azure, získáte pomocí rutiny Powershellu HPC HpcGroup přidat.


## <a name="next-steps"></a>Další kroky

* Jako alternativu k použití HPC Pack vyvíjet se službou Azure dávku spustit MPI aplikace na spravované fondů výpočetním uzlů v Azure. V tématu [použití více instancí úkoly spouštění aplikací rozhraní předávání zpráv (MPI) v Azure listu](../batch/batch-mpi.md).

* Pokud chcete spustit Linux MPI aplikace, které přístup k síti Azure RDMA, najdete v článku [nastavení clusteru Linux RDMA spouštění aplikací MPI](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png