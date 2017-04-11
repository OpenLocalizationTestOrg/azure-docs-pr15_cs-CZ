<properties
    pageTitle="Azure Glosář - Azure slovník | Microsoft Azure"
    description="Použijte Azure Glosář k pochopení cloudu terminologie platformu Azure. Toto krátké Azure slovníku poskytuje definice pro společné cloudu termíny pro Azure."
    keywords="Azure slovníku terminologie cloudu, Azure glosář, terminologie definice, cloudu podmínky"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Glosář Microsoft Azure: slovník cloudu terminologie platformu Azure

Glosář Microsoft Azure je krátké slovník cloudu terminologie pro platformu Azure.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Vyhledání definice služeb a jinými termíny cloudu

* Definice Azure služeb a jejich AWS protějšky najdete v článku [Microsoft Azure a Amazon webové služby](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Obecné odvětví cloudu termínů najdete v článku [cloudu výpočetních podmínky](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Azure glosář s odkazy na dvou výše uvedených poskytuje taxonomie začátku do konce Azure a odvětví cloudu.  

## <a name="azure-glossary-list"></a>Seznam Azure Glosář


### <a name="account"></a>účet  
Pracovní nebo školní nebo osobního účtu Microsoft, který slouží k přístupu a správa Azure předplatného.  
Viz taky [jak Azure předplatná souvisí s Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>nastavení dostupnosti  
Sbírka virtuálních počítačích spravovaných společně poskytnout aplikaci redundance a spolehlivosti. Použití sady dostupnost zajistí během buď plánované nebo neplánované údržbě alespoň jeden virtuálního počítače k dispozici.  
Viz taky [Spravovat dostupnost virtuálních počítačích Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) nebo [Správa dostupnosti Linux virtuálních počítačích](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Model Azure klasické nasazení  
Jedna ze dvou [modelů nasazení](resource-manager-deployment-model.md) slouží k nasazení zdrojů v Azure (nový model je správce prostředků Azure). Některé Azure zdroje mohou být nasazeny v jedné modelu nebo jiné, zatímco ostatní mohli nasazenou v obou modelů. Pokyny pro jednotlivé Azure zdroje podrobnosti, které model(s) zdroje můžete nasadit s.


### <a name="cli"></a>Azure rozhraní příkazového řádku (rozhraní příkazového řádku)  
[Rozhraní příkazového řádku](xplat-cli-install.md) , které lze použít ke správě Azure služeb z Windows, OSX a počítačů Linux.


### <a name="powershell"></a>Azure Powershellu  
[Rozhraní příkazového řádku](powershell-install-configure.md) pro správu služby Azure přes příkazového řádku z počítače s logem Windows. Některé služby nebo funkce služby dá se ovládat pouze pomocí prostředí PowerShell nebo rozhraní příkazového řádku. Pokyny pro podrobnosti každé konkrétního Azure zdroje, které model(s) zdroje můžete nasadit s.   
Viz taky [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md)


### <a name="arm-model"></a>Azure správce prostředků nasazení modelu  
Jedna ze dvou [modelů nasazení](resource-manager-deployment-model.md) slouží k nasazení zdrojů v Microsoft Azure (druhý je modelu klasické nasazení). Některé Azure zdroje mohou být nasazeny v jedné modelu nebo jiné, zatímco ostatní může být nasazené v obou modelech. Pokyny pro jednotlivé Azure zdroje podrobnosti, které model(s) zdroje můžete nasadit s.


### <a name="fault-domain"></a>poruchy domény  
Kolekce virtuálních počítačích v sadě dostupnost, případně selhat ve stejnou dobu. Příklad je skupina strojů regálů, majících společné zdroje a síťové vypínač. V Azure, což jsou virtuálních počítačích v sadě dostupnost automaticky oddělené přes víc domén poruch.  
Viz taky [Spravovat dostupnost virtuálních počítačích Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) nebo [Správa dostupnosti Linux virtuálních počítačích](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>GEO  
Definovaný hranice sídlo data, která obvykle obsahuje dva nebo více oblastí. Hranice může být v rámci nebo za vnitrostátní ohraničení a jsou ovlivněny nařízení daně. Každý geo obsahuje aspoň jednu oblast. Příklady geos se Asijsko-tichomořské Japonsko. Taky se mu říká *zeměpisná oblast*.  
Viz taky [Azure oblastí](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>replikace GEO  
Proces automaticky replikace obsah, jako je objektů BLOB, tabulky a fronty v rámci místního dvojice.  
Viz také [Aktivní Geo replikace databáze Azure SQL](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>Obrázek  
Soubor, který obsahuje operačního systému a konfigurace aplikace, které můžete použít k vytvoření libovolný počet virtuálních počítačích. V Azure existují dva typy obrázků: OM obrázek a obrázek s operačním systémem. Obrázek OM obsahuje operačního systému a všech discích připojené k virtuálního počítače při vytvoření obrázku. Obrázek s operačním systémem obsahuje pouze generalized operačním systémem s konfigurací disku žádná data.  
Viz taky [obrázek a vyberte obrázky virtuálního počítače Windows v Azure pomocí prostředí PowerShell nebo rozhraní příkazového řádku](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>omezení  
Počet zdrojů, které se dají vytvářet nebo srovnávacích výkonu, které lze dosáhnout. Limity obvykle souvisí s předplatná, služby a nabídky.  
Viz taky [Azure předplatné a omezení služby, kvót a omezení](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>Vyrovnávání zatížení  
Prostředek, který distribuuje příchozí komunikaci mezi počítače v síti. V Azure Vyrovnávání zatížení distribuuje umožnění datových přenosů do virtuálních počítačích definované v sadě Vyrovnávání zatížení. [Služba Vyrovnávání zatížení](./load-balancer/load-balancer-overview.md) může být internetového nebo může být vnitřní.  


### <a name="offer"></a>nabídka  
Ceny, přeplatků a souvisejících termínů pro předplatné Azure.  
V tématu [Azure nabízejí stránku s podrobnostmi o](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>portál  
Zabezpečeného webového portálu slouží k nasazením a správou Azure služby.  Existují dva portály: [Azure portál](http://portal.azure.com/) a [klasické portálu](http://manage.windowsazure.com/). Některých služeb jsou k dispozici v obou portály, zatímco ostatní jsou k dispozici pouze v jednom nebo druhému. [Graf Azure portálu dostupnost](https://azure.microsoft.com/features/azure-portal/availability/) seznamy služby, které jsou k dispozici v které portálu.  


### <a name="region"></a>oblast  
Oblast v rámci geo znamená kříž vnitrostátní ohraničení, který obsahuje jeden nebo více datacentrech. Ceny, místní služeb a typů nabídky jsou umístěné na úrovni oblast. Oblast je obvykle spárované s jiné oblasti, který může být až několik jinam, stovky mil tvoří pár místní. Místní dvojice mohou sloužit jako mechanismus pro obnovení havárie a dostupnost scénáře. Taky nazývá obecně *umístění*.  
Viz taky [Azure oblastí](best-practices-availability-paired-regions.md)


### <a name="resource"></a>zdroje  
Položky, která je součástí vašeho Azure řešení. Každé služby Azure umožňují nasazení různé typy zdrojů, jako jsou databáze nebo virtuálních počítačích.   
Viz taky [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>pole Skupina zdroje  
Kontejner v správce obsahujícího související materiály pro aplikaci. Skupina zdroje můžete zahrnout všechny zdroje pro aplikaci nebo pouze zdroje, které jsou logicky seskupené. Můžete se rozhodnout, jak chcete agregovat skupinám zdroje založené na co je nejsmysluplnější pro vaši organizaci.  
Viz taky [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Správce prostředků šablony  
JSON soubor, který deklarativně definuje jeden nebo více Azure zdrojů a definující závislostí mezi nasazeném zdroje. Šablona mohou sloužit k nasazení zdroje konzistentní a opakovaně.  
Viz také [šablony pro vytváření správce prostředků Azure](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>zprostředkovatele prostředků  
Služba, která poskytuje zdroje můžete nasazovat a spravovat pomocí Správce prostředků. Každý poskytovatel zdrojů nabízí operace pro práci s prostředky, které používají. Zprostředkovatelé zdroje můžete k nim získat přístup prostřednictvím portálu Azure, Azure PowerShell a několik programovacím SDK.  
Viz taky [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>role  
Význam: řízení přístupu, přiřazené uživatelům, skupiny a služeb. Role mohou provádět tyto vytváření, Správa a Čtěte dál Azure zdroje.  
Viz taky [RBAC: předdefinované role](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>Smlouva o úrovni služeb (SLA)  
Smlouva popisující závazky společnosti Microsoft pro provozu a připojení. Konkrétní SLA má každá Azure služba.  
Viz taky [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>úložiště účtu  
Úložiště účet, který umožňuje přístup ke službám objektů Blob Azure, fronty, tabulky a souboru v úložišti Azure. Účtu úložiště poskytuje jedinečných názvů pro datové objekty Azure úložiště.  
Viz taky [účty adresářové služby Azure o úložiště](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>předplatné  
Zákazníků přísudku se Microsoft, která umožní získat Azure services. Předplatné ceny a souvisejících termínů upravuje nabídky pro předplatné. V tématu [smlouvou Microsoft Online předplatného](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Viz taky [jak Azure předplatná souvisí s Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>značka  
Indexování termín, který umožňuje zařadit do kategorií zdroje podle vašim požadavkům pro řízení a fakturace. Značky můžete použít, když máte složité kolekce skupiny zdrojů a zdroje informací a potřebujete vizualizace aktiva způsobem, jako je nejsmysluplnější. Můžete například označení prostředky, které slouží podobné role ve vaší organizaci nebo patří stejné oddělení.  
Viz taky [pomocí značek k uspořádání Azure prostředků.](resource-group-using-tags.md)


### <a name="update-domain"></a>Aktualizace domény  
Kolekce virtuálních počítačích v sadě dostupnost, které mají být aktualizovány ve stejnou dobu. Virtuálních počítačích v tu samou doménu aktualizace jsou plánované údržbě společně restartovat. Azure nerestartuje víc než jednu doménu aktualizace najednou. Bývá označovaná taky jako upgradu domény.  
Viz taky [Spravovat dostupnost virtuálních počítačích Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) nebo [Správa dostupnosti Linux virtuálních počítačích](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuální počítač  
Software implementace fyzických počítač, na kterém běží operační systém. Více virtuálních počítačích je možné současně spouštět na stejné hardwaru. V Azure, což jsou k dispozici v různých velikostech virtuálních počítačích.  
Viz taky [si přečtěte následující dokumentaci virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>rozšíření virtuálního počítače  
Zdroj implementující chování nebo funkce, které buď jiných programů, práce nebo poskytnout možnost, kterou chcete provést interakci s pracovního počítače. Třeba byste mohli resetování nebo změna hodnoty vzdáleného přístupu v Azure virtuálního počítače přes koncovku OM přístup.  
Viz taky [o rozšíření virtuálního počítače a funkcí (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) nebo [rozšíření virtuálního počítače a funkcí (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtuální sítě  
Síť, která zajišťuje propojení mezi Azure zdroje, které je izolovaný od ostatních Azure klienti. Můžete být připojeni jiných Azure virtuální sítích pomocí [Brána VPN Azure](./vpn-gateway/vpn-gateway-about-vpngateways.md) a místních sítí [více možností](./vpn-gateway/vpn-gateway-cross-premises-options.md). Plně můžete určit blok adresy IP, nastavení DNS, zásady zabezpečení a směrování tabulek v rámci této sítě.  
Viz taky [Přehled virtuální sítě](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Viz taky**  
- [Začínáme s Azure](https://azure.microsoft.com/get-started/)
- [Centrum zdrojů cloudu](https://azure.microsoft.com/resources/)  
- [Azure pro podnikové aplikaci](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure ve vaší datacentru](https://azure.microsoft.com/overview/business-apps-on-azure/) 





