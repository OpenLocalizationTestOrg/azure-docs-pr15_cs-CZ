<properties
 pageTitle="Automatické měřítko HPC Pack uzlech | Microsoft Azure"
 description="Automaticky zvětšovat a zmenšovat počtu uzlů výpočetním clusteru HPC Pack v Azure"
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
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack v Azure podle pracovní zátěž obrázku




Pokud nasadit Azure "požadavků" uzlů v HPC Pack obrázku nebo vytvoření clusteru HPC Pack v Azure VMs, je vhodné způsob, jak automaticky zvětšit nebo zmenšit počet Azure výpočetním zdroje, jako jsou uzly nebo jádra podle aktuální zatížení clusteru. Umožňuje použít Azure zdroje mnohem efektivněji a budete moci určit jejich náklady.
K tomuto účelu nastavte vlastnost clusteru HPC Pack **AutoGrowShrink**. Můžete taky spusťte skript Powershellu HPC **AzureAutoGrowShrink.ps1** , který se instaluje s aktualizací Service Pack HPC.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Také aktuálně můžete automaticky zvětšit a zmenšit HPC Pack výpočetním uzly, ve kterých se nepoužívá operační systém Windows Server.

## <a name="set-the-autogrowshrink-cluster-property"></a>Nastavte vlastnost AutoGrowShrink obrázku

### <a name="prerequisites"></a>Zjistit předpoklady pro

* **HPC Pack 2012 R2 aktualizace 2 nebo novější clusteru** - hlavy clusteru může být nasazené buď místně nebo v Azure OM. V tématu [Nastavení hybridního obrázku s aktualizací Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) začít pracovat s hlavního uzlu místní a Azure "požadavků" uzlů. Viz [skript pro nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) k rychlému nasazení clusteru HPC Pack v Azure VMs.


* **U obrázku s hlavou uzel v Azure** – Pokud použijete skript pro nasazení HPC Pack IaaS k vytvoření clusteru, **AutoGrowShrink** clusteru vlastnost povolit nastavením možnosti AutoGrowShrink v konfiguračního souboru obrázku. Další informace najdete v dokumentaci doprovodným [skript stáhnout](https://www.microsoft.com/download/details.aspx?id=44949). 

    Můžete taky povolte vlastnost clusteru **AutoGrowShrink** po nasazení clusteru pomocí prostředí PowerShell HPC příkazy popsané v následující části. Příprava na to, udělejte toto:
    1. Konfigurace certifikátu Azure správy na uzel hlavy a v Azure předplatného. Zkušební nasazení můžete použít výchozí Microsoft Azure HPC certifikátu podepsaného svým držitelem HPC Pack nainstaluje na uzel hlavy a jednoduše k předplatnému Azure nahrát tento certifikát. Možnosti a postup najdete v tématu [pokyny v knihovně TechNet](https://technet.microsoft.com/library/gg481759.aspx).
    2. Spusťte **příkaz regedit** na uzel hlavy, přejděte na HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo a přidejte novou řetězcovou hodnotu. Nastavení názvu hodnotu "Miniatura" a hodnoty data, která chcete Miniatura certifikát v kroku 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Příkazy Powershellu HPC nastavovaná vlastnost AutoGrowShrink

Toto jsou ukázková příkazy Powershellu HPC **AutoGrowShrink** a ladění jeho chování se další parametry. Dál v tomto článku úplný seznam nastavení najdete v článku [AutoGrowShrink parametry](#AutoGrowShrink-parameters) . 

Tyto příkazy spustíte zahájen HPC Powershellu hlavy clusteru jako správce.

**Chcete-li povolit vlastnost AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Jak zakázat vlastnost AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Chcete-li změnit interval zvětšit v minutách**

    Set-HpcClusterProperty –GrowInterval <interval>

**Chcete-li změnit interval zmenšit v minutách**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Pokud chcete zobrazit aktuální konfigurace AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parametry

Následují AutoGrowShrink parametrů, které můžete změnit pomocí příkazu **HpcClusterProperty sadu** .

* **EnableGrowShrink** - přepínač Povolit nebo zakázat vlastnost **AutoGrowShrink** .
* **ParamSweepTasksPerCore** - počet úkolů čištění parametrů rozšířit jeden základní. Ve výchozím nastavení je zvětšovat jeden základní jednoho úkolu. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 změní **ParamSweepTasksPerCore** **TasksPerResourceUnit**. Je založená na typ zdroje úlohy a může být uzel, socket nebo core.
    
* **GrowThreshold** - mezní hodnota ve frontě úkolů aktivovat automatické LOGLINTREND. Výchozí hodnota je 1, což znamená, že, pokud jsou 1 či více úkolů ve stavu ve frontě automaticky zvětšit uzlů.
* **GrowInterval** - Interval v minutách aktivovat automatické LOGLINTREND. Výchozí interval je 5 minut.
* **ShrinkInterval** - Interval v minutách aktivovat automatické zmenšením. Výchozí interval je 5 minut. |
* **ShrinkIdleTimes** - počet nepřetržitý kontroly se zmenší označíte uzly jsou nečinná. Výchozí hodnota je 3 časy. Třeba při **ShrinkInterval** 5 minut, HPC Pack zkontroluje, jestli uzel je nečinný každých 5 minut. Pokud uzly ve stavu nečinnosti po 3 nepřetržitý zkontroluje (15 minut), HPC Pack zmenší uzel.
* **ExtraNodesGrowRatio** – další procento uzlů rozšířit úloh rozhraní předávání zpráv (MPI). Výchozí hodnota je 1, což znamená, že roste HPC Pack uzly 1 % MPI úloh. 
* **GrowByMin** - přepnout do označuje, zda zásad automatické zvětšování vychází z minimální prostředky potřebné pro daný úkol. Výchozí hodnota je false, což znamená, že HPC Pack roste uzly úloh podle maximální prostředky potřebné pro úlohy.
* **SoaJobGrowThreshold** - mezní příchozí žádosti SOA aktivovat automatické zvětšit obrázku. Výchozí hodnota je 50000.  
    
    >[AZURE.NOTE] Tento parametr je podporován od HPC Pack 2012 R2 aktualizace 3.
    
* **SoaRequestsPerCore** -počet příchozí SOA žádostí o rozšířit jeden základní. Výchozí hodnota je 20000.  

    >[AZURE.NOTE] Tento parametr je podporován od HPC Pack 2012 R2 aktualizace 3.

### <a name="mpi-example"></a>Příklad MPI

Ve výchozím nastavení HPC Pack roste 1 % navíc uzly MPI úloh (**ExtraNodesGrowRatio** je nastavena na hodnotu 1). Důvod, proč je, že MPI může vyžadovat víc uzly a úlohy lze spustit pouze, kdy je připraven všech uzlů. Po spuštění uzly Azure občas jeden uzel delší dobu zahájení než ostatní příčinou uzlech je nečinný během čekání na uzel připravit potřebovat. Podle Kvetoucí navíc uzly HPC Pack snižuje tentokrát čekání zdroje a potenciálně uloží náklady. Pokud chcete zvýšit procento navíc uzly pro MPI úlohy (například 10 %), spuštění příkazu podobně jako

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Příklad SOA

Ve výchozím nastavení **SoaJobGrowThreshold** je nastavený na 50000 a **SoaRequestsPerCore** je nastavený na 200000. Pokud odešlete jednu SOA úlohu s 70000 požadavky, bude jeden úkol ve frontě a příchozí žádosti jsou 70000. V tomto případě roste HPC Pack 1 základní úlohy ve frontě a pro příchozí žádosti zvětšit (70000 50000) / základní 20000 = 1, takže celkem rozrůstat 2 jádra pro tento projekt SOA.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Spustit skript AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Zjistit předpoklady pro

* **HPC Pack 2012 R2 aktualizace 1 nebo novější clusteru** - **AzureAutoGrowShrink.ps1** skript se instaluje do složky Koš % CCP_HOME %. Hlavy clusteru může být nasazené buď místně nebo v Azure OM. V tématu [Nastavení hybridního obrázku s aktualizací Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) začít pracovat s hlavního uzlu místní a Azure "požadavků" uzlů. V tématu [skript pro nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) k rychlému nasazení clusteru HPC Pack v Azure VMs nebo použít [šablonu Azure rychlý úvod](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - skript aktuálně závisí na této konkrétní verzi Azure Powershellu. Pokud používáte novější verzi na uzel hlavy, bude pravděpodobně nutné přejít Azure PowerShell [verze 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) následujícím způsobem. 

* **U obrázku s Azure požadavků uzly** - následujícím způsobem v klientském počítači nainstalovanou HPC Pack nebo na uzel hlavy. Pokud systém do klientského počítače, ujistěte se, nastavit proměnnou $env: CCP_SCHEDULER správně tak, aby ukazovaly na uzel hlavou. Uzly Azure "požadavků" musí být přidanou clusteru, ale může být v stavu nasazený není.


* **Pro cluster nasazenou v Azure VMs** - následujícím způsobem na uzel hlavy OM, protože závisí na **HpcIaaSNode.ps1 spustit** a **Zastavit HpcIaaSNode.ps1** skripty, která nejsou nainstalovány. Tyto skripty navíc nevyžaduje certifikát Azure správy ani publikovat soubor nastavení (viz [Správa výpočet uzlů v HPC Pack obrázku v Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Ujistěte se, všechny výpočetní uzly VMs potřebujete jste už přidali do clusteru. Může být ve stavu Zastaveno.

### <a name="syntax"></a>Syntaxe

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parametry

 * **NodeTemplates** - názvy šablon uzel k definování rozsahu uzlů zvětšovat a zmenšovat. Pokud není zadán (výchozí hodnota je @()), všechny uzly ve skupině uzel **AzureNodes** jsou v rozsahu při **NodeType** má hodnotu AzureNodes a všechny uzly ve skupině uzel **ComputeNodes** jsou v rozsahu při **NodeType** má hodnotu ComputeNodes.

 * **JobTemplates** - názvy šablon úlohy definovat rozsah uzly rozšířit.

 * **NodeType** – typ uzel a zvětšovat a zmenšovat. Podporované hodnoty jsou:

     * **AzureNodes** – Azure PaaS (požadavků) uzlech místního nebo Azure IaaS obrázku.

     * **ComputeNodes** – pro výpočetní uzly VMs pouze v Azure IaaS obrázku.

* **NumOfQueuedJobsPerNodeToGrow** - počet frontě nutné zvětšit uzlů.

* **NumOfQueuedJobsToGrowThreshold** - prahová frontě spustit proces zvětšit.

* **NumOfActiveQueuedTasksPerNodeToGrow** - počet aktivních úkolů ve frontě nutné zvětšit uzlů. Je-li **NumOfQueuedJobsPerNodeToGrow** obsahujících hodnotu větší než 0, je tento parametr ignorován.

* **NumOfActiveQueuedTasksToGrowThreshold** - prahová aktivních úkolů ve frontě spustit proces zvětšit.

* **NumOfInitialNodesToGrow** - počáteční minimální počet uzlů rozšířit, pokud jsou všechny uzly v rozsahu **Není nasazení** nebo **Zastaveno (Deallocated)**.

* **GrowCheckIntervalMins** - interval v minutách mezi kontroly rozšířit.

* **ShrinkCheckIntervalMins** - interval v minutách mezi kontroly se zmenší.

* **ShrinkCheckIdleTimes** - počet nepřetržitý zmenšit kontroly (oddělených **ShrinkCheckIntervalMins**) označíte, že jsou nedělá uzlů.

* **UseLastConfigurations** - předchozí konfigurace uložené v souborech argumentů.

* **ArgFile**– název souboru argument použili k uložení a aktualizovat konfigurace následujícím způsobem.

* **LogFilePrefix** - předpona názvu souboru protokolu. Můžete zadat cestu. Ve výchozím nastavení je došlo k aktuálnímu adresáři pracovní zápisu protokol.

### <a name="example-1"></a>Příklad 1

Následující příklad nastaví uzly Azure požadavků nasazený s výchozí AzureNode šablonu, kterou chcete zvětšovat a zmenšovat automaticky. Pokud jsou všechny uzly původně ve stavu **Nasazený není** , jsou začali nejméně 3 uzlů. Pokud počet frontě větší než 8, spustí skript, dokud jejich číslo větší než poměr úlohy ve frontě **NumOfQueuedJobsPerNodeToGrow**uzlů. Pokud uzel zjistí, že nečinná v 3krát po sobě jdoucí nečinnosti, je zastaveno.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Příklad 2

Následující příklad nastaví Azure výpočetní uzly VMs nasazený s výchozí ComputeNode šablonu, kterou chcete zvětšovat a zmenšovat automaticky.
Úlohy nakonfigurovaný tak, že výchozí šablonu projektu definovat rozsah pracovní zátěž na clusteru. Zastavení všechny uzly jsou původně, jsou začali alespoň na úrovni 5 uzlů. Pokud počet aktivních úkolů ve frontě překročí 15, spustí skript, dokud jejich číslo větší než poměr aktivní ve frontě úkoly **NumOfActiveQueuedTasksPerNodeToGrow**uzlů. Pokud uzel zjistí, že nečinná v 10krát po sobě jdoucí nečinnosti, je zastaveno.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
