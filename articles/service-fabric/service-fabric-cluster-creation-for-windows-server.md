<properties
   pageTitle="Vytváření a Správa clusteru struktury služby Azure samostatného | Microsoft Azure"
   description="Vytváření a správa struktury služby Azure obrázku na všechny počítače (fyzické nebo virtuální) serverem Windows, ať už jde o místní nebo v libovolné cloudu."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Vytváření a Správa clusteru se systémem Windows Server

Struktury služby Azure slouží k vytvoření struktury služby clusterů na virtuálních počítačích nebo počítače se systémem Windows Server. To znamená, že můžete nasazení a spuštění služby struktury aplikací v prostředí, který obsahuje sadu propojené počítačích Windows Server, budete ji místně nebo s kteréhokoliv poskytovatele cloudu. Služba struktury obsahuje instalační balíček k vytvoření struktury služby clusterů s názvem balíček samostatného systému Windows Server.

Tento článek vás provede kroků pro vytvoření clusteru pomocí samostatného balíčku pro službu struktury místně, když ho můžete snadno upraví pro ostatní prostředí například jiných poskytovatelů cloudu.

>[AZURE.NOTE] Tento balíček systému Windows Server samostatného může obsahovat funkce, které jsou v současnosti ve verzi preview a nejsou podporované pro komerční použití. Seznam funkcí, které jsou v náhledu najdete v tématu "funkce součástí tohoto balíčku." Můžete taky [Stažení kopie této smlouvy](http://go.microsoft.com/fwlink/?LinkID=733084) .


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Získání podpory pro balíček samostatného struktury služby

- Zeptejte se komunity o balíček samostatné struktury služby systému Windows Server ve [fóru struktury služby Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Otevřete lístek [Professional podpora služby struktury](http://support.microsoft.com/oas/default.aspx?prid=16146 ).  Další informace o [Podpoře Professional od Microsoftu](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Stáhnout balíček samostatné struktury služby


[Stáhnout balíček samostatného pro službu struktury pro Windows Server 2012 R2 a novější](http://go.microsoft.com/fwlink/?LinkId=730690), která se nazývá Microsoft.Azure.ServiceFabric.WindowsServer. &lt;verze&gt;ZIP.


V okně Stáhnout balíček bude hledat následující soubory:

|**Název souboru**|**Krátký popis**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|CAB soubor, který obsahuje binární soubory, které jsou nasazených na jednotlivé počítače v clusteru.|
|ClusterConfig.Unsecure.DevCluster.json|Shluk konfigurace ukázkový soubor, který obsahuje nastavení pro nezabezpečenou, tři uzly, jednom počítači (nebo virtuálního počítače) vývoj clusteru služby, včetně informací pro každý uzel clusteru. |
|ClusterConfig.Unsecure.MultiMachine.json|Shluk konfigurace ukázkový soubor, který obsahuje nastavení pro nezabezpečenou, více počítače (nebo virtuálního počítače) obrázku, včetně informací pro každý počítač clusteru.|
|ClusterConfig.Windows.DevCluster.json|Shluk konfigurace ukázkový soubor, který obsahuje všechna nastavení pro zabezpečené, tři uzly jednom počítači (nebo virtuálního počítače) vývoj clusteru, včetně informace o jednotlivých uzlech, který je v clusteru. Clusteru je zabezpečená pomocí [Windows identity](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.json|Shluk konfigurace ukázkový soubor, který obsahuje všechna nastavení zabezpečení, více počítače (nebo virtuálního počítače) clusteru pomocí zabezpečení systému Windows, včetně informací pro každý počítač, který je v zabezpečené obrázku. Clusteru je zabezpečená pomocí [Windows identity](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.x509.DevCluster.json|Shluk konfigurace ukázkový soubor, který obsahuje všechna nastavení pro zabezpečené, tři uzly jednom počítači (nebo virtuálního počítače) vývoj clusteru, včetně informace o jednotlivých uzlech clusteru. Clusteru je zabezpečená pomocí x509 certifikáty.|
|ClusterConfig.x509.MultiMachine.json|Shluk konfigurace ukázkový soubor, který obsahuje všechna nastavení pro zabezpečené, více počítače (nebo virtuálního počítače) clusteru, včetně informací pro každý uzel v zabezpečené obrázku. Clusteru je zabezpečená pomocí x509 certifikáty.|
|EULA_ENU.txt|Licenční podmínky pro použití struktury služby Microsoft Azure samostatný systému Windows Server balíček. Teď můžete [Stáhnout kopii smlouvu EULA](http://go.microsoft.com/fwlink/?LinkID=733084) .|
|Readme.txt|Odkaz na poznámky k verzi a pokyny k základním instalaci. Tvoří podmnožinu pokyny v tomto dokumentu.|
|CreateServiceFabricCluster.ps1|Skript Powershellu, který vytváří clusteru pomocí nastavení v ClusterConfig.json.|
|RemoveServiceFabricCluster.ps1|Skript Powershellu, která odebere clusteru pomocí nastavení v ClusterConfig.json.|
|ThirdPartyNotice.rtf |Oznámení o třetích stran software, který je v okně balíček.|
|AddNode.ps1|Skript Powershellu pro přidání uzel s existujícím nasazené obrázku.|
|RemoveNode.ps1|Skript Powershellu pro odebrání uzlu z existující nasazené obrázku.|
|CleanFabric.ps1|Skript Powershellu pro čištění samostatně instalace služby struktury vypnout aktuální počítač. Odeberte předchozí instalace MSI pomocí vlastní přidružené uninstallers.|
|TestConfiguration.ps1|Skript Powershellu pro analýzu infrastruktury určené v Cluster.json.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Plánování a příprava nasazením obrázku
Než vytvoříte svůj cluster, proveďte následující kroky.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Krok 1: Plánování infrastrukturu obrázku
Chystáte se vytvoření clusteru struktury služby na počítačích, kterou vlastníte, abyste se mohli rozhodnout, jaké druhy selhání má cluster zachován. Například je třeba oddělit power čar nebo připojení k Internetu předán tyto počítače? Kromě toho zvažte fyzické zabezpečení těchto počítačů. Kde se nacházejí strojů a kdo potřebuje přístup k nim? Po provedení těchto rozhodnutí můžete logicky namapovat počítačích různé domény poruch (viz krok 4). Plánování výrobní clusterů infrastruktury je složitější než clusterů testu.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Krok 2: Příprava počítače plnit požadavky
Požadavky pro každý počítač, který chcete přidat do clusteru:

- Aspoň 16 GB paměti RAM, je vhodné.
- Je vhodné aspoň 40 GB volného místa na disku.
- Doporučuje 4 core nebo větší procesoru.
- Připojení k síti zabezpečené nebo sítí pro všechny počítače.
- Windows Server 2012 R2 nebo Windows Server 2012 (musíte mít [KB2858668](https://support.microsoft.com/kb/2858668) nainstalovaný).
- [.NET framework 4.5.1 nebo vyšší](https://www.microsoft.com/download/details.aspx?id=40773), úplné nainstalovat.
- [Prostředí Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- [Služba RemoteRegistry](https://technet.microsoft.com/library/cc754820) by měla běžet na všech počítačích.

Správce clusteru nasazení a konfiguraci clusteru musíte mít [oprávnění správce](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) ve všech počítačích. Nelze nainstalovat Service struktury řadiče domény.

### <a name="step-3-determine-the-initial-cluster-size"></a>Krok 3: Určete velikost počáteční obrázku
Každý uzel v samostatné služby struktury clusteru má struktury služby runtime nasazeném a je členem clusteru. V typické provozní nasazení je jeden uzel každou instanci OS (fyzické nebo virtuální). Velikost obrázku, je určený podle potřeb uživatele. Ale musíte mít minimální velikost clusteru tři uzlů (strojů nebo virtuálních počítačích).
Pro účely vývoj může mít více uzel na daný počítač. V provozním prostředí služby struktury podporuje pouze jeden uzel za fyzické nebo virtuálního počítače.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Krok 4: Určení počtu domén poruch a upgrade domén
*Poruchy domény* (FD) je fyzická jednotka selhání a přímo souvisí s fyzické infrastruktury v datacentrech. Poruchy domény se skládá součástí hardwaru (počítačů, přepínače, sítě a další), která sdílejí selhání v jednom místě. Sice 1:1 mapování mezi poruch domény a stojany volně mluvit každý regálů považovat za poruch domény. Při rozhodování, uzlů v clusteru, důrazně doporučujeme uzly rozdělí mezi nejméně tři poruch domény.

Pokud zadáte FDs v ClusterConfig.json, můžete název každého FD. Služba struktury podporuje hierarchické FDs tak, aby odrážela topologii infrastruktury v nich.  Například následující FDs platí:

- "faultDomain": "fd: / Room1/Rack1/POCITAC1"
- "faultDomain": "fd: / FD1"
- "faultDomain": "fd: / Room1/Rack1/PDU1/M1"


*Upgrade domény* (UD) je logické jednotka uzlů. Během aktualizace služby struktury orchestrated (upgrade aplikace nebo upgradu na obrázku) stačí UD přesměrováni upgradu během uzlů v jiných UDs zůstávají k dispozici a bude předávat žádosti. Upgrady firmwaru pomocí počítače respektovat UDs, takže musíte udělat je jeden počítače najednou.

Nejjednodušší způsob, jak myslete koncepce je zvažte FDs jako celek neplánované selhání a UDs jako celek plánovanou údržbu.

Pokud zadáte UDs v ClusterConfig.json, můžete název každého UD. Například následující názvy platí:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Modré"

Podrobnější informace o upgradu domény a poruch domény najdete v článku [popisující služby struktury obrázku](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Krok 5: Stáhněte balíček samostatné struktury služby systému Windows Server
[Stáhnout balíček samostatné struktury služby systému Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) a rozbalte balíček, nasazení počítač, který není součástí clusteru, nebo na jeden z počítače, které mají být součástí svůj cluster. Lze přejmenovat složku rozbaleny `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Vytvořte svůj cluster

Po projít kroky plánování a příprava, jste připraveni k vytvoření clusteru.

### <a name="step-1-modify-cluster-configuration"></a>Krok 1: Změna konfigurace obrázku
Clusteru je popsaná v ClusterConfig.json. Podrobnosti o oddíly v tomto souboru najdete v článku [Konfigurace nastavení samostatného Windows obrázku](service-fabric-cluster-manifest.md).
Otevřete jednu ze souborů ClusterConfig.json z balíčku, který jste stáhli a změnit následující nastavení:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Konfigurace nastavení**|**Popis**|
|-----------------------|--------------------------|
|**NodeTypes**|Typy uzlů umožňují oddělení cluster uzly do různých skupin. Clusteru, musíte mít alespoň jeden NodeType. Všechny uzly ve skupině mají následující společné vlastnosti: <br> **Název** – to je název typu uzel. <br>**Porty koncového bodu** - Toto jsou různé pojmenované koncové body (porty), které jsou přidružené k tento typ uzlu. Můžete použít jiné číslo portu, které jste právě a chtěli byste, dokud nedošlo ke konfliktu s něco v tomto manifestu ještě nejsou nepoužívá jakékoli jiné aplikace spuštěné v počítači/OM. <br> **Umístění vlastnosti** – tyto popisují vlastnosti pro tento typ uzel, který slouží jako umístění omezení služeb systému nebo služby. Tyto vlastnosti jsou definované uživatelem klíč/dvojice, které jsou zdrojem metadata navíc pro daný uzel. Příklady vlastnosti uzlu bude, zda má uzel pevném disku nebo grafickou kartu, počet vřetena pevného disku, jádra a dalších vlastností pole fyzicky. <br> **Kapacity** - uzel kapacity definovat název a množství určitého zdroje s konkrétním uzlu umožňující spotřebu. Uzel může například definovat, že má kapacitu metriky s názvem "MemoryInMb" a že nejsou 2048 MB k dispozici ve výchozím nastavení. Tyto objemy slouží za běhu zajistit, aby služby, které vyžadují určitého množství prostředků jsou umístěná v jednotlivých uzlech, které mají k dispozici v požadované množství prostředků.<br>**IsPrimary** – Pokud máte víc než jeden NodeType definované zajistit, že je pouze jeden je nastavena na primární hodnotu *PRAVDA*, což je místo, kam systémové služby spuštění. Všechny ostatní typy uzel by měla být nastavena na hodnotu *false*|
|**Uzly**|Toto jsou podrobnosti o všech uzlech, které jsou součástí clusteru (uzel typ, název uzlu, IP adresu, poruch domény a upgrade doménu uzel). Strojů chcete clusteru vytvořit na se musí být uvedené spolu s jejich IP adres. <br> Pokud použití stejné adresy IP pro všechny uzly pak jedno pole obrázku je vytvořen, které můžete použít pro účely testování. Nepoužívejte clusterů jedno pole pro nasazení pracovního vytížení výroby.|


### <a name="step-2-run-the-testconfiguration-script"></a>Krok 2: Spuštění TestConfiguration skriptu

Skript TestConfiguration testy infrastruktury podle cluster.json, abyste měli jistotu, že potřebná oprávnění přiřazené strojů připojeni k sobě navzájem a další atributy se definují tak, aby mohli úspěšné zavedení. Je v podstatě mini Analyzátoru osvědčených postupů. Budeme dál přidávat další ověření tento nástroj v čase snažíme usnadnit jeho robustnější.

Tento skript poběží v počítači, který má přístup správce do všech počítačů, které jsou označeny jako uzlů v konfiguračního souboru obrázku. Počítač, který se spustí tento skript na nemá být součástí clusteru.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Krok 3: Následujícím způsobem vytvořit obrázku
Po změny konfigurace obrázku v dokumentu JSON a přidá všechny informace uzel, spusťte vytváření clusteru skript Powershellu *CreateServiceFabricCluster.ps1* ve složce balíčku a předat cestu k souboru konfigurace JSON. Po dokončení této, přijmete smlouvu EULA.

Tento skript poběží v počítači, který má přístup správce do všech počítačů, které jsou označeny jako uzlů v konfiguračního souboru obrázku. Počítač, který se spustí tento skript na nemá být součástí clusteru.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Nejsou k dispozici místně na OM/počítač, který spustil Powershellu CreateServiceFabricCluster na protokoly nasazení. Jsou v podsložce s názvem DeploymentTraces ve složce místo, kam jste spustili příkazu Powershellu. Pokud chcete zobrazit, pokud byla struktury služby správně nasazena k počítači, nainstalované soubory můžete najít v adresáři C:\ProgramData a procesy FabricHost.exe a Fabric.exe viděli spuštěné ve Správci úloh.

### <a name="step-4-connect-to-the-cluster"></a>Krok 4: Připojte se k němu

Připojení k zabezpečeného clusteru, najdete v článku [struktury služby připojení k zabezpečené obrázku](service-fabric-connect-to-secure-cluster.md).

Připojení k nezabezpečené clusteru, spusťte tento příkaz Powershellu:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Krok 5: Vyvoláte Explorer struktury služby

Teď se můžete připojit k obrázku v Průzkumníkovi struktury služby buď přímo z jednoho z počítače s http://localhost:19080/Explorer/index.html nebo vzdáleně se http://<*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Přidání a odebrání uzlů

Můžete přidat nebo odebrat uzly do samostatného služby struktury clusteru změní vaše obchodní potřeby. Podrobný postup najdete v článku [Přidání nebo odebrání uzlů do samostatného obrázku struktury služby](service-fabric-cluster-windows-server-add-remove-nodes.md) .


## <a name="remove-a-cluster"></a>Odebrání clusteru

Odebrat clusteru, spusťte tento skript Powershellu *RemoveServiceFabricCluster.ps1* ve složce balíčku a předat cestu k souboru konfigurace JSON. Volitelně můžete zadat do umístění protokolu odstranění.

Tento skript poběží v počítači, který má přístup správce do všech počítačů, které jsou označeny jako uzlů v konfiguračního souboru obrázku. Počítač, který se spustí tento skript na nemá být součástí clusteru.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemetrickými daty shromažďují a jak vyjádření výslovného nesouhlasu ho

Ve výchozím produktu shromažďuje telemetrie o využití služeb struktury zlepšit produktu. Analyzátoru osvědčených postupů, které se spouští jako součást kontroly nastavení pro připojení k [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Pokud není dostupný, nastavení se nezdaří, pokud vyjádření výslovného nesouhlasu telemetrie.

  1. Profilace telemetrie se snaží nahrajete následujícími údaji [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) jednou každý den. Je nejlepší dostupné upload a nemá žádný vliv na funkci obrázku. Telemetrie odeslaný jenom ze skupiny, které se spouští záložní primární správce. Žádné uzly rozešlete telemetrie.

  2. Telemetrie se skládá z následujících akcí:

-            Počet služeb
-            Počet ServiceTypes
-            Počet aplikací
-            Počet ApplicationUpgrades
-            Počet FailoverUnits
-            Počet InBuildFailoverUnits
-            Počet UnhealthyFailoverUnits
-            Počet repliky
-            Počet InBuildReplicas
-            Počet StandByReplicas
-            Počet OfflineReplicas
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Počet uzlů
-            IsContextComplete: True nebo False
-            ClusterId: Toto je GUID náhodně vygeneruje pro každou obrázku
-            ServiceFabricVersion
-             IP adresa virtuálního počítače nebo počítače, ze kterého je nahráli telemetrie


Jak zakázat telemetrie, přidejte následující text a *Vlastnosti* v souboru obrázku config: *enableTelemetry: false*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>Funkce zahrnuté v tomto balíčku

Žádná.

>[AZURE.NOTE] S novým [GA verzi samostatného obrázku v systému Windows Server (verze 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), můžete upgradovat svůj cluster budoucích verzích, ať už ručně nebo automaticky. Vzhledem k tomu, že tato funkce není k dispozici ve verzích náhled, musíte pro vytvoření clusteru pomocí verze GA a migraci dat a aplikace z náhled obrázku. Zůstaňte sestavenou podrobné informace o této funkci.


## <a name="next-steps"></a>Další kroky
- [Konfigurace nastavení pro samostatné Windows obrázku](service-fabric-cluster-manifest.md)
- [Přidání nebo odebrání uzly clusteru struktury služeb samostatně](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Vytvoření samostatného služby struktury clusteru se systémem Windows Azure VMs](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí X509 certifikátů](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
