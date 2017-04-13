<properties
 pageTitle="Přidání požadavků uzlů clusteru HPC Pack | Microsoft Azure"
 description="Naučte se rozbalte HPC Pack cluster v Azure okamžitou přidáním instancí pracovního role spuštěných v cloudové službě"
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
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Přidání na vyžádání "požadavků" uzlů clusteru HPC Pack v Azure



Pokud jste nastavili clusteru [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) v Azure, můžete způsob, jak rychle změnit velikost obrázku kapacita nahoru nebo dolů, bez zachování sadu předkonfigurované výpočetní uzly VMs. Tento článek ukazuje, jak přidat na vyžádání "požadavků" uzly (kolegy role instancí spuštěných ve službě cloud) jako zdroje výpočetním hlavy uzel v Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Požadavků uzlů][burst]

Kroky v tomto článku vám pomůže přidat Azure uzly rychle do cloudové HPC Pack hlavy uzel OM pro ověření koncepce nasazení nebo testu. Nejvyšší úrovně se postupuje stejně jako při jeho "roztržení Azure" přidáte cloudu výpočet schopnost clusteru HPC Pack místní. Kurz najdete v článku [Nastavení hybridním výpočet obrázku s Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Podrobné pokyny a co byste měli zvážit výrobní nasazení najdete v tématu [Shlukové Azure s Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Zjistit předpoklady pro

* **Hlavy uzel HPC Pack nasazenou v Azure OM** – můžete samostatný hlavy uzel OM nebo takový, který je součástí větší obrázku. Vytvoření samostatného hlavního uzlu najdete v tématu [nasazení uzlu HPC Pack vedoucího v Azure OM](virtual-machines-windows-hpcpack-cluster-headnode.md). Automatické možnosti nasazení clusteru HPC Pack najdete v tématu [Možnosti můžete vytvořit a spravovat cluster Windows HPC Azure s Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Pokud použijete [skript pro nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) k vytvoření clusteru v Azure, můžete zahrnout Azure požadavků uzlů v automatické nasazení. Příklady v tomto článku.

* **Azure předplatného** – přidání Azure uzlů, můžete vybrat stejný předplatného použít k nasazení hlavního uzlu OM, nebo jiné předplatné (nebo předplatná).

* **Kvóta jádra** - může být nutné zvětšit kvóty jádra, zejména pokud se rozhodnete pro nasazení několik Azure uzlů s vícejádrových velikosti. Pokud chcete zvýšit kvóty, [Otevřete žádost o konverzaci online Zákaznická podpora](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Krok 1: Vytvoření Cloudová služba a úložiště účet Azure uzlů

Pomocí portálu Azure klasické nebo odpovídající nástroje pro nastavení v následujících zdrojích, které jsou potřebné pro nasazení uzly Azure:

* Nové Azure cloudové služby
* Nový účet Azure úložiště

>[AZURE.NOTE] Není znovu použít existující cloudové služby předplatného. 

**Co byste měli zvážit**

* Konfigurace samostatné Cloudová služba pro každou Azure uzel šablonu, kterou chcete vytvořit. Však může použít stejný účet úložiště pro více uzel šablon.

* Doporučujeme, abyste ve stejné oblasti Azure vyhledejte Cloudová služba a účtu úložiště pro nasazení.




## <a name="step-2-configure-an-azure-management-certificate"></a>Krok 2: Nakonfigurujte Azure Správa certifikátů

Přidání Azure uzlů jako pro využití prostředků, potřebujete certifikát správy na hlavní uzel a uložit, odpovídající certifikát Azure předplatné používané pro nasazení.

V tomto scénáři můžete použít **Výchozí HPC Azure správy certifikát** , který HPC Pack nainstaluje a automaticky nakonfiguruje na uzel hlavy. Tento certifikát je užitečné pro testování účely a ověření koncepce nasazení. Pokud chcete použít tento certifikát, nahrajte 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack soubor z hlavního uzlu OM k předplatnému. Pokud chcete odeslat certifikát [Azure klasické portál](https://manage.windowsazure.com), klikněte na **Nastavení** > **Osvědčení o řízení**.

Další možnosti pro nastavení správy certifikát najdete v článku [scénáře konfigurace certifikátu správy Azure Azure shlukové nasazení](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Krok 3: Nasazení Azure uzly clusteru



Postup přidání a začněte Azure uzlů v tomto scénáři jsou obvykle kroky s hlavního uzlu místní. Další informace najdete v následujících částech [kroků nasazení Azure uzly s Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Vytvoření šablony e Azure uzel

* Přidání clusteru Windows HPC Azure uzlů

* Zahájení uzly (poskytování) Azure

Po přidání a začněte uzly jsou připravena k použití pro spuštění úlohy obrázku.

Pokud při nasazení Azure uzly docházet k problémům, přečtěte si článek [Poradce při potížích s nasazení z Azure uzly s Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Další kroky

* Použití velikosti náročné instance uzlů požadavků, najdete v článku Co byste měli zvážit v [o H řady a náročné VMs A řadu](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Pokud chcete automaticky zvětšit nebo zmenšit Azure výpočetních prostředků podle pracovní zátěž obrázku, přečtěte si článek [automaticky zvětšovat a zmenšovat zdroje Azure výpočetním clusteru HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
