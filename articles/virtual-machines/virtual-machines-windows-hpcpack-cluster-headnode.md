<properties
 pageTitle="Vytvoření hlavního uzlu HPC Pack v Azure OM | Microsoft Azure"
 description="Informace o vytvoření hlavního uzlu Microsoft HPC Pack v Azure OM pomocí portálu Azure a nasazení modelu správce prostředků."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Vytvoření hlavního uzlu HPC Pack obrázku v Azure OM s obrázkem Marketplace


Umožňuje vytvořit hlavní uzel clusteru HPC na [Microsoft HPC Pack virtuálního počítače obrázek](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) z webu Azure Marketplace a portálu Azure. Tento obrázek HPC Pack OM vychází z Windows serveru 2012 R2 Datacentra s HPC 2012 R2 aktualizací SP3 předinstalované. Použijte tento hlavy uzel pro doklad o nasazení koncept sady HPC v Azure. Poté můžete přidat uzly výpočetním clusteru spuštění HPC úloh.



>[AZURE.TIP]Nasazení celého clusteru HPC Pack v Azure, která obsahuje hlavního uzlu a výpočetním uzly, doporučujeme vám použít automatický způsob. Možnosti zahrnují [skript pro nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) a šablon správce prostředků [clusteru HPC Pack pro úloh systému Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) . Další šablony naleznete v tématu [Možnosti obrázku HPC Pack v Azure](virtual-machines-windows-hpcpack-cluster-options.md) . 


## <a name="planning-considerations"></a>Plánování co byste měli zvážit

Jak je znázorněno na následujícím obrázku, nasadíte hlava uzel HPC Pack v doméně služby Active Directory v Azure virtuální sítě.

![Hlavy uzel HPC Pack][headnode]

* **Active Directory domain** - HPC Pack hlavního uzlu musí být připojen k doméně služby Active Directory v Azure před zahájením služby HPC OM. Jak je vidět v tomto článku najdete doklad o nasazení koncept, můžete převést OM vytvoříte pro hlavy uzel jako řadiče domény před spuštěním služby HPC. Další možností je nasazení samostatné domény řadiče domény a struktury v Azure, ke kterému se připojujete hlavního uzlu OM.

* **Azure virtuální sítě** – při použití nasazení modelu správce prostředků pro nasazení hlavního uzlu, zadejte nebo vytvořte Azure virtuální sítě. Virtuální sítě použijte, pokud je potřeba se připojit hlavního uzlu do existující domény Active Directory. Budete potřebovat přidejte do něj později výpočetní uzly VMs clusteru.

    
## <a name="steps-to-create-the-head-node"></a>Postup vytvoření hlavy uzel

Následují hlavních kroků používat portál Azure pomocí Správce prostředků nasazení modelu vytvářet Azure OM pro hlavy uzel HPC Pack. 


1. Pokud chcete vytvořit nové doménové služby Active Directory s samostatné domény řadiče VMs v Azure, je jednou z možností použít [šablonu správce prostředků](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Pro jednoduché doklad o nasazení koncept je v pořádku tento krok vynechat a konfigurace hlavního uzlu OM sama jako řadiče domény. Tato možnost je popsané dál v tomto článku.
    
2. Na [HPC Pack 2012 R2 na stránce Windows serveru 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) v Azure Marketplace klikněte na **vytvořit virtuální počítač**. 

3. Na portálu na stránce **HPC Pack 2012 R2 v systému Windows Server 2012 R2** vyberte model nasazení **Správce prostředků** a potom klikněte na **vytvořit**.

    ![Obrázek HPC Pack][marketplace]

4. Pomocí portálu ke konfiguraci nastavení a vytvoření OM. Pokud jste novými uživateli Azure, postupujte podle kurzu [Vytvoření virtuálního počítače Windows Azure portálu](virtual-machines-windows-hero-tutorial.md). Pro doklad o nasazení koncept obvykle jsou přijatelné výchozí nebo doporučené nastavení.

    >[AZURE.NOTE]Pokud se chcete připojit hlavního uzlu do existující domény Active Directory v Azure, zkontrolujte, že zadáte virtuální sítě pro tuto doménu při vytváření OM.
       
4. Až vytvoříte OM a systémem OM se [připojit k OM](virtual-machines-windows-connect-logon.md) pomocí vzdálené plochy. 

5. Spojení OM a doménové domény nebo vytvořte strukturu domény na OM samotné.

    * Pokud jste vytvořili v Azure virtuální sítě s existující struktuře domény OM, spojení OM a struktuře pomocí standardní nástrojů pro správce serveru nebo prostředí Windows PowerShell. Restartujte.

    * Pokud jste vytvořili OM v síti nové virtuální (bez existující struktuře domény), potom zvýšit úroveň OM jako řadiče domény. Nainstalujte a nakonfigurujte roli Active Directory Domain Services na uzel hlavy pomocí standardní kroků. Podrobnosti najdete v tématu [Instalace nových Windows serveru 2012 Active Directory strukturu](https://technet.microsoft.com/library/jj574166.aspx).

5. Až OM běží a je připojen k struktuře služby Active Directory, spusťte je HPC Pack následujícím způsobem:

    na. Připojení k hlavního uzlu OM pomocí doménovým účtem, který je členem skupiny místních správců. Třeba použijte účet správce, které jste nastavili při vytvoření hlavního uzlu OM.

    b. Výchozí konfigurace hlavního uzlu spustit jako správce prostředí Windows PowerShell a zadejte tento příkaz:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Může trvat několik minut HPC Pack služby, které chcete spustit.

    Možnosti konfigurace další hlavy uzel zadejte `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Další kroky

* Nyní můžete pracovat s hlavního uzlu svůj cluster HPC Pack. Například spusťte Správce clusteru HPC a úplný [Seznam úkolů nasazení](https://technet.microsoft.com/library/jj884141.aspx).
* Pokud chcete zvýšit clusteru výpočet kapacity na vyžádání doplněk do cloudové služby [Azure požadavků uzlů](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) . 

* Spusťte test úlohu na clusteru. Příklad najdete v článku HPC Pack [příručku Začínáme](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
