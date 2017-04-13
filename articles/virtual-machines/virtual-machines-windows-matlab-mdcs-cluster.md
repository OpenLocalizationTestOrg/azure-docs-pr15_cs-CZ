<properties
   pageTitle="MATLAB clusterů virtuálních počítačích | Microsoft Azure"
   description="Umožňuje vytvořit MATLAB Distributed Computing serveru clusterů spuštění svého náročné paralelní MATLAB pracovního vytížení virtuálních počítačích Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Vytvoření serveru Distributed Computing MATLAB clusterů na Azure VMs 

Umožňuje vytvořit jeden nebo více clusterů MATLAB Distributed Computing serveru spuštění svého náročné paralelní MATLAB pracovního vytížení virtuálních počítačích Microsoft Azure. Instalace softwaru serveru Distributed Computing MATLAB na OM používat jako základní obrázek a pomocí šablony Azure rychlý úvod nebo skript Powershellu Azure (dostupné na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) nasazením a správou clusteru. Po zavedení připojte se k němu spuštění svého pracovního vytížení. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Informace o MATLAB a MATLAB Distributed Computing serveru 

Platformu [MATLAB](http://www.mathworks.com/products/matlab/) je optimalizována pro řešení potíží s technickým a matematickém. MATLAB s rozsáhlé simulace a zpracování dat úkoly mohou uživatelé společnost MathWorks paralelní výpočetních produkty dosažení vyššího jejich pracovního vytížení náročné využijete výpočetních clusterů a mřížka služeb. [Paralelní výpočetních podokno úloh Souprava nástrojů](http://www.mathworks.com/products/parallel-computing/) MATLAB uživatelům umožňuje paralelní aplikací a využijte výhod procesory, využívají grafické karty a výpočet clusterů. [MATLAB Distributed Computing Server](http://www.mathworks.com/products/distriben/) umožňuje MATLAB využít mnoho počítačů ve výpočetním clusteru. 


Pomocí Azure virtuálních počítačích, můžete vytvořit serveru Distributed Computing MATLAB clusterů, které mají stejné mechanismy můžou odeslat paralelní práce jako v místním clusterů, například interaktivní projektů, dávkové úlohy, nezávisle na úkoly a komunikující úkolů. Použití Azure ve spojení s informacemi o platformě MATLAB má řadu výhod ve srovnání s zřizování a pomocí tradiční místní hardware: velikostí oblasti virtuálního počítače, vytváření clusterů na vyžádání tak platíte jenom pro využití prostředků můžete použít a možnost testování modely ve velkém měřítku.  

## <a name="prerequisites"></a>Zjistit předpoklady pro

* **Klientský počítač** -, musíte mít serveru s Windows klientský počítač komunikovat s Azure a clusteru serveru Distributed Computing MATLAB po nasazení. 

* **Azure PowerShell** – přečtěte si téma [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) nainstalovat ve vašem klientském počítači. 

* **Azure předplatné** – Pokud nemáte předplatné, můžete vytvořit [účet zase uvolnit](https://azure.microsoft.com/free/) v jenom pár minut. U větších clusterů zvažte systému průběžného financování předplatné nebo další možnosti nákupu. 

* **Kvóta jádra** - může být nutné zvětšit základní kvóty pro nasazení velké obrázku nebo víc serveru Distributed Computing MATLAB obrázku. Pokud chcete zvýšit kvóty, [Otevřete žádost o konverzaci online Zákaznická podpora](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma. 

* **MATLAB, paralelní výpočetních podokno úloh Souprava nástrojů a MATLAB Distributed Computing Server licence** - skripty se předpokládá, že [Společnost MathWorks hostované správce licencí](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) se používal pro všechny licence.  

* **Software serveru Distributed Computing MATLAB** - nainstaluje na OM, který bude sloužit jako základní obrázek OM clusteru VMs. 


## <a name="high-level-steps"></a>Vysoké úrovně kroky

Pro účely Azure virtuálních počítačích serveru Distributed Computing MATLAB clusterů, jsou požadovány následující hlavních kroků. Podrobné pokyny najdete v dokumentaci šablony rychlý úvod a skripty na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Vytvoření základní obrázek OM**  
    * Stažení a instalace serveru Distributed Computing MATLAB softwaru do této OM. 

    >[AZURE.NOTE]To může trvat několik hodin, musíte ale pouze na to, jakmile ve všech verzích MATLAB používáte.   
    
2. **Vytvořit jeden nebo více clusterů**  
    * Použijte předaném skript Powershellu nebo šablona rychlý úvod k vytvoření clusteru z základní OM obrázku.   
    * Správa clusterů pomocí předaném skript Powershellu, který umožňuje seznam, pozastavení, obnovení a odstraňte clusterů. 
 
## <a name="cluster-configurations"></a>Konfigurace clusterů 

V současné době skript vytvoření obrázku a šablon můžete vytvořit jednu topologie serveru Distributed Computing MATLAB. Pokud budete potřebovat, vytvořit jeden nebo více další clusterů, s každý cluster mají rozdílný počet pracovníka VMs pomocí různých velikostech OM a tak dále. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klienta a obrázku v Azure 

Uzel Klient MATLAB Plánovač úloh MATLAB uzel a uzly serveru Distributed Computing MATLAB "pracovníka" jsou všechny konfigurovat jako Azure VMs virtuální sítě, jak je znázorněno na následujícím obrázku. 

![Topologie obrázku](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Použití clusteru, se připojte pomocí Vzdálená plocha na uzel klienta. Uzel klient spustí klienta MATLAB. 

* Uzel klienta je sdílený soubor, který je přístupný všechny zaměstnance.

* Správce licencí hostované společnost MathWorks se používá pro kontroly licenci pro všechny MATLAB software. 

* Ve výchozím nastavení jeden pracovní serveru Distributed Computing MATLAB za základní se vytvoří na pracovní VMs, ale můžete použít jiné číslo. 


## <a name="use-an-azure-based-cluster"></a>Použití obrázku založené na Azure 

Jako s další typy serveru Distributed Computing MATLAB clusterů, budete muset Správce clusteru profilu v klientovi MATLAB (na straně klienta OM) umožňuje vytvořit profil Plánovač úloh MATLAB obrázku.

![Správce profilů obrázku](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Další kroky

* Podrobné informace o nasazením a správou serveru Distributed Computing MATLAB clusterů Azure najdete v článku úložišti [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) obsahující šablony a skriptů. 

* Přejděte na [webu společnost MathWorks](http://www.mathworks.com/) podrobné dokumentaci MATLAB a serveru Distributed Computing MATLAB.
