<properties
    pageTitle="Přehled virtuálních počítačích Windows | Microsoft Azure"
    description="Informace o vytváření a správě virtuálních počítačích Windows v Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Základní informace o virtuálních počítačích Windows v Azure

Azure virtuálních počítačích (OM) je jeden z několika typů [na vyžádání, scalable výpočetních prostředků](../app-service-web/choose-web-site-cloud-service-vm.md) nabízející Azure. Obvykle virtuálního počítače zvolíte, když potřebujete větší kontrolu nad prostředí než nabízejí jiné možnosti. Tento článek obsahuje informace o co byste měli vědět před vytvořením virtuálního počítače, jak vytvořit a jak spravovat.

Azure OM vám umožní virtualizace aniž by bylo nutné koupit a spravovat fyzické hardware, který spustí jej. Ale potřebujete ke správě OM úkoly, například konfiguraci, opravy a instalaci softwaru, který běží v něm.

Azure virtuálních počítačích lze použít různými způsoby. Příklady:

- **Vývoj a testování** – nabídka Azure VMs a rychlý a jednoduchý způsob vytvoření počítače s konkrétní konfigurace muset kód a testovat aplikaci.
- **Aplikace v cloudu** – protože služba pro aplikaci můžete kolísání, může být economic smysl ho spusťte na OM v Azure. Můžete buď zaplatit navíc VMs když potřebujete a vypnout sbalené.
- **Rozšířený datacentra** – virtuálních počítačích v Azure virtuální sítě můžete snadno připojení k síti vaší organizace.

Počet VMs používané aplikace můžete přizpůsobit nahoru a se něco jiného, co je potřeba přizpůsobit svým potřebám.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Co je potřeba vzít v úvahu před vytvořením virtuálního počítače?

Jsou vždy velké množství [navrhování](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) při vytváření se infrastrukturu aplikace v Azure. Tyto aspekty virtuálního počítače je důležité zvážit před spuštěním:

- Názvy zdrojů aplikace
- Umístění, kde jsou uloženy zdroje
- Velikost OM
- Maximální počet VMs, které lze vytvořit
- Operační systém, která poběží OM
- Konfigurace OM po spuštění
- Související materiály, které potřebuje OM

### <a name="naming"></a>Pojmenování

Virtuální počítač má [název](virtual-machines-windows-infrastructure-naming-guidelines.md) přiřazenou a má název počítače nakonfigurování jako součást operačního systému. Název virtuálního počítače může být až 15 znaků.

Pokud používáte Azure k vytvoření disk operačního systému, název počítače a název počítače virtuální jsou stejné. Pokud jste [nahrávání a použít vlastní obrázek](virtual-machines-windows-upload-image.md) , který obsahuje dříve konfigurované operačních systémů a použijte ji k vytvoření virtuálního počítače, názvy může být různé. Doporučujeme, aby při umístěte vložit ovládací prvek obrázek uděláte název počítače v operačním systému a virtuální počítač stejný název.

### <a name="locations"></a>Umístění

Všechny zdroje vytvořené v Azure jsou rozvržena více [zeměpisnými oblastmi](https://azure.microsoft.com/regions/) světa. Obvykle oblasti je místo toho **umístění** při vytváření virtuálního počítače. Pro OM umístění Určuje, kde jsou uloženy virtuální pevných discích.

V této tabulce jsou uvedeny některé způsoby můžete získat seznam dostupných umístění.

| Metoda | Popis |
| ------ | ----------- |
| Azure portálu | Vyberte umístění, ze seznamu při vytváření virtuálního počítače. |
| Azure Powershellu | Pomocí příkazu [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| ROZHRANÍ REST API | Pomocí [seznamu umístění](https://msdn.microsoft.com/library/dn790540.aspx) operace. |

### <a name="vm-size"></a>Velikost OM

[Velikost](virtual-machines-windows-sizes.md) OM, který používáte, je určený podle pracovní zátěž, kterou chcete spustit. Velikost, které zvolíte pak určuje faktory například zpracování power, paměti a úložiště kapacity. Azure nabízí celou řadu velikosti podpory více typů používá.

Azure poplatky za [každou hodinu cena](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) na základě velikosti a operační systém OM. Částečné hodin Azure poplatky jenom pro minut použít. Úložiště je cenami a Účtovaná samostatně.

### <a name="vm-limits"></a>Limity OM

Vaše předplatné má výchozí [kvóty](../azure-subscription-service-limits.md) na místě, které by mohly ovlivnit nasazení mnoho VMs plánu projektu. Aktuální omezení na základě předplatného za je 20 VMs jednotlivých oblastech. Omezení můžete vyvolané používá se zařazování požadavek podpory můžete žádosti o zvýšení.

### <a name="operating-system-disks-and-images"></a>Operační systém disků a obrázků

Virtuálních počítačích [virtuální pevných discích (VHD)](virtual-machines-windows-about-disks-vhds.md) slouží k ukládání jejich operačního systému (OS) a data. VHD slouží k snímky, které je možné z instalace operačního systému. 

Azure obsahuje mnoho [marketplace obrázky](https://azure.microsoft.com/marketplace/virtual-machines/) pomocí různých verzí a typy operačních systémech Windows Server. Tržiště obrázky jsou označeny obrázek Publisheru, nabídky, sku a verze (obvykle verze zadaný jako poslední). 

Této tabulce jsou uvedeny některé způsoby, můžete najít informace pro obrázek.

| Metoda | Popis |
| ------ | ----------- |
| Azure portálu | Hodnoty jsou automaticky jazyku za vás, když vyberete obrázek, který chcete použít. |
| Azure Powershellu | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) – umístění "umístění"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) – umístění "umístění"-vydavatel "publisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -umístění "umístění"-vydavatel "publisherName" – nabídka "offername." |
| Rozhraní REST API | [Vydavatelé seznam obrázků](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Nabízí seznam obrázků](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Obrázek seznamu skladové jednotky](https://msdn.microsoft.com/library/mt743701.aspx) |

Můžete [ukládat](virtual-machines-windows-upload-image.md) a používat vlastní obrázek a po výběru, nejsou použity název vydavatele, nabídky a skladové jednotky.

### <a name="extensions"></a>Rozšíření

OM [rozšíření](virtual-machines-windows-extensions-features.md) poskytují další funkce aplikace OM prostřednictvím konfigurace nasazení příspěvku a automatické úkoly.

Tyto běžné úkoly lze provést pomocí rozšíření:

- **Spustit vlastní skripty** – [Vlastní skript příponu](virtual-machines-windows-extensions-customscript.md) vám pomůže konfigurovat úloh v OM spuštěním skript, když máte k dispozici OM.
- **Nasazení a spravovat konfigurace** – [rozšíření prostředí PowerShell žádoucí stavu konfigurace (DSC)](virtual-machines-windows-extensions-dsc-overview.md) vám pomůže nastavit DSC na virtuálního počítače ke správě konfigurace a prostředí.
- **Shromáždit data diagnostiky** – [Azure diagnostiky rozšíření](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) vám pomůže nakonfigurovat OM sběr dat diagnostických nástrojů, který slouží ke sledování stavu aplikace.

### <a name="related-resources"></a>Související materiály

Zdroje v této tabulce jsou používány OM a potřebujete existují nebo vytvořen při OM.

| Zdroje | Povinné | Popis |
| -------- | -------- | ----------- |
| [Pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md) | Ano | OM musí být součástí skupiny zdrojů. |
| [Úložiště účtu](../storage/storage-create-storage-account.md) | Ano | OM musí úložiště účet, který chcete uložit jeho virtuální pevných discích. |
| [Virtuální sítě](../virtual-network/virtual-networks-overview.md) | Ano | OM musí být členem skupiny virtuální sítě. |
| [Veřejnou IP adresu](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Ne | OM může mít přiřazené vzdálený přístup k veřejnou IP adresu. |
| [Rozhraní sítě](../virtual-network/virtual-network-network-interface-overview.md) | Ano | OM vyžaduje rozhraní sítě komunikovat v síti. |
| [Disků dat](virtual-machines-windows-attach-disk-portal.md) | Ne | OM mohou obsahovat data disků rozbalte možnosti ukládání. |

## <a name="how-do-i-create-my-first-vm"></a>Jak vytvořit svůj první OM?

Máte několik možností pro vytváření vašeho OM. Výběr, kdy byla závisí na prostředí, který používáte. 

Tato tabulka obsahuje informace, abyste mohli začít vytváření vašeho OM.

| Metoda | Článek |
| ------ | ------- |
| Azure portálu | [Vytvoření virtuálního počítače s Windows, na portálu](virtual-machines-windows-hero-tutorial.md) |
| Šablony | [Vytvoření virtuálního počítače Windows se šablonou správce prostředků](virtual-machines-windows-ps-template.md) |
| Azure Powershellu | [Vytvoření Windows OM pomocí prostředí PowerShell](virtual-machines-windows-ps-create.md) |
| Klient SDK | [Nasazení Azure zdrojů prostřednictvím C#](virtual-machines-windows-csharp.md) |
| Rozhraní REST API | [Vytvoření nebo aktualizace virtuálního počítače](https://msdn.microsoft.com/library/mt163591.aspx) |

Ať nikdy proběhne, ale občas vyskytnou. Pokud tato situace se vám, podívejte se informace o [řešení potíží s správce prostředků nasazení](virtual-machines-windows-troubleshoot-deployment-new-vm.md)problémy s vytváření virtuálních počítačů Windows v Azure.

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Jak můžu spravovat OM, který jsem vytvořil(a)?

VMs dá ovládat nástroje portál pomocí prohlížeče, příkazového řádku s podporou pro skriptování nebo přímo prostřednictvím rozhraní API. Některé úlohy typické správy, které můžete provádět jsou získání informací o OM, přihlášení k OM, Správa dostupnosti a vytvoření zálohy.

### <a name="get-information-about-a-vm"></a>Získání informací o virtuálního počítače

Tato tabulka ukazuje některé způsoby, kterou můžete získat informace o virtuálního počítače.

| Metoda | Popis |
| ------ | ----------- |
| Azure portálu | V nabídce centrální klikněte na **virtuálních počítačích** a vyberte OM ze seznamu. Na zásuvné pro OM mít přístup na souhrnné informace, nastavení hodnot a sledování metriky. |
| Azure Powershellu | Informace o použití Powershellu ke správě VMs najdete v tématu [Správa virtuálních počítačích Azure pomocí Správce zdrojů a Powershellu](virtual-machines-windows-ps-manage.md). |
| ROZHRANÍ REST API | Pomocí operace [OM získat informace](https://msdn.microsoft.com/library/mt163682.aspx) můžete získat informace o virtuálního počítače. |
| Klient SDK | Informace o používání C# ke správě VMs najdete v tématu [Správa virtuálních počítačích Azure pomocí Správce prostředků Azure a C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Přihlaste se k OM

Použili jste na tlačítko Připojit v Azure portálu pro [zahájení relace vzdálené plochy](virtual-machines-windows-connect-logon.md). Co je někdy si nepovedlo při pokusu o připojení ke vzdálené použití. Pokud tato situace se vám, přečtěte si nápovědu v [připojení ke vzdálené ploše Poradce při potížích s Azure virtuálního počítače s Windows](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Správa dostupnosti

Je důležité si chcete porozumět tomu, jak [zajistit, aby dostupnost](virtual-machines-windows-manage-availability.md) pro aplikaci. Konfigurace zahrnuje vytvoření více VMs ověřit, jestli je spuštěný alespoň jedno.

Aby nasazení pro naše 99.95 OM úroveň smlouvách budete muset nasazení dva nebo více VMs systém vaše pracovní zátěž uvnitř [dostupnost nastavení](virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Tuto konfiguraci zajišťuje vaše VMs rozvržena víc domén poruch a nasadit do hosts s windows různé údržbu. Úplné [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje zaručené dostupnost Azure jako celek.
 
### <a name="back-up-the-vm"></a>Obecnějším údajům OM
 
[Obnovení služby trezoru](../backup/backup-introduction-to-azure-backup.md) slouží k ochraně dat a prostředky služby Azure zálohování a obnovení webu Azure. Můžete použít trezoru obnovení Services [nasazovat](../backup/backup-azure-vms-automation.md)a spravovat zálohy pro nasazení Správce prostředků VMs pomocí Powershellu. 

## <a name="next-steps"></a>Další kroky

- Pokud váš záměr pro práci s Linux VMs, podívejte se na [Azure a Linux](virtual-machines-linux-azure-overview.md).
- Další informace o pokyny kolem nastavení infrastrukturu v tomto [příkladu Azure infrastruktury návodu](virtual-machines-windows-infrastructure-example.md).
- Zkontrolujte, že používaly [Osvědčené postupy pro systém Windows OM na Azure](virtual-machines-windows-guidance-compute-single-vm.md).


