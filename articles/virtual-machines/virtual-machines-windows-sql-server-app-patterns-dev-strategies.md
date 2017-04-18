<properties
    pageTitle="SQL Server aplikace vzorky VMs | Microsoft Azure"
    description="Tento článek popisuje vzorků aplikace pro systém SQL Server na Azure VMs. Poskytuje tvůrci řešení a vývojáři základ architektura dobré aplikací a návrh."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Vzorky aplikace a vývoj strategie pro SQL Server ve virtuálních počítačích Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Shrnutí:
Zjištění, které aplikace vzorku nebo vzorků pro použití aplikace na serveru SQL v Azure prostředí je důležité návrh rozhodnutí a vyžaduje plné znalost SQL serveru a jednotlivé součásti infrastruktury Azure fungování společně. S SQL serverem infrastruktury služby Azure můžete snadno migrovat, spravovat a sledovat existující aplikace systému SQL Server integrované – v systému Windows Server na virtuálních počítačích v Azure.

Hledání v tomto článku je stanovit tvůrci řešení a vývojáři základ architektura dobré aplikací a návrh, která můžete sledovat při přenesení existujících aplikací na Azure stejně jako v Azure vedle aplikací.

Pro každý vzorek aplikace zjistíte scénáři místní, jeho odpovídajících podporou cloudové řešení a související technické doporučení. Kromě toho článek pojednává o Azure specifické vývoj strategie tak, aby vaše aplikace můžete navrhovat správně. Z důvodu mnoha vzorky možné aplikace doporučuje se, že tvůrci a vývojáři měli zvolit nejvhodnější vzor pro svých aplikací a uživatelé.

**Technické přispěvatelům:** Roman Leoš Vargas sleďů Madhan Arumugam Ramakrishnan

**Technické recenzentů:** Corey Sanders Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani vám Stefanova situace Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Úvod

Můžete vyvíjet mnoho typů vrstvy n aplikací tím, že odděluje komponent vrstvy jiné aplikaci na různých počítačích stejně jako samostatný součásti. Například umístíte klientské aplikace a obchodní pravidla součástí jeden počítač, front-end web osy a komponenty osy dat aplikace access v jiném počítači a back-end databáze osy v jiném počítači. Tento typ strukturování pomáhá izolovat jednotlivé vrstvy od sebe. Pokud změníte, kde se berou data, nemusíte měnit klienta nebo webové aplikace, ale jenom komponenty osy dat aplikace access.

Typické *n složek* aplikace obsahuje osy prezentace, osy business a osy dat:


| Vrstvy              | Popis                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Prezentace** | *Prezentace osy* (osy web, front-end osy) je vrstvy, do kterého uživatelé pracují s aplikací.                                                                      |
| **Business**     | *Úroveň business* (střední vrstvy) příslušné vrstvy úroveň prezentaci, osy dat pomocí vzájemně komunikovat a zahrnuje základní funkce systému. |
| **Data**         | *Osy dat* je v podstatě serveru, na kterém jsou uložena data aplikace (například serveru SQL Server).                                                             |

Aplikace vrstvy popisují logické seskupení funkcí a součástí aplikace. zatímco úrovní popisují fyzické distribuční funkce a složek na samostatné fyzické servery, počítačů, sítí nebo vzdáleném umístění. Vrstvy aplikace může být umístěny na stejném počítači fyzické (na stejné úrovni) nebo může být průběhu různých počítačích (n vrstvy) a součásti v každé vrstvy komunikovat s součástí jiných vrstvy prostřednictvím dobře definovaný rozhraní. Si můžete představit úrovně časové období jako fyzické rozdělení vzorů například oddělených dvě, tři úrovně a n osy. **2 osy aplikace vzorek** obsahuje dvě úrovně aplikace: použití a databázový server. Přímé komunikaci se stane mezi aplikační server a databázovém serveru. Aplikační server uchovává součásti web osy a obchodní osy. **Aplikace 3 osy vzorek**jsou tři úrovně aplikace: webové serveru, serveru aplikace, který obsahuje obchodní logiky osy a/nebo komponenty Accessu firmy osy dat a databázovém serveru. Komunikace mezi webového serveru a databázovém serveru se stane myší aplikační server. Podrobné informace o aplikaci vrstvy a úrovní v [Příručce Architektura aplikace Microsoft](https://msdn.microsoft.com/library/ff650706.aspx).

Před zahájením tohoto článku pro čtení, měli byste mít znalosti o základních koncepcí SQL serveru a Azure. Informace najdete v tématu [SQL Server knihy Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md) a [Azure.com](https://azure.microsoft.com/).

Tento článek popisuje několik vzorků aplikace, které může být vhodné pro jednoduché aplikace, jakož i velmi složitých podnikových aplikací. Než s podrobným popisem jednotlivých vzorku, doporučujeme, že byste si měli přečíst úložiště služby dostupné údaje v Azure, například [Azure úložiště](../storage/storage-introduction.md), [Databáze SQL Azure](../sql-database/sql-database-technical-overview.md)a [SQL Server ve Azure virtuálního počítače](virtual-machines-windows-sql-server-iaas-overview.md). Abyste měli ty nejcennější návrh rozhodnutí pro aplikace a vysvětlení kdy použít které datové úložiště služby jasně.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Zvolte serveru SQL Server Azure virtuální počítač, pokud:

- Potřebujete ovládací prvek na serveru SQL Server a Windows. Například může obsahovat verze serveru SQL Server, jinak hotfix, konfiguraci výkonnosti atd.

- Potřebujete úplnou kompatibility s místním SQL serveru a chcete přesunout stávající aplikací Azure jako-je.

- Chcete-li využít možnosti Azure prostředí, ale databáze SQL Azure nepodporuje všechny funkce, které aplikace vyžaduje. Může se jednat o následujících oblastí:

    - **Velikost databáze**: V době Tento článek byl aktualizován podporuje databáze SQL databázi až 1 TB data. Pokud vaše aplikace vyžaduje víc než 1 TB dat a nechcete, aby implementovat vlastní sharding řešení, doporučujeme použít SQL Server ve Azure virtuálního počítače. Nejnovější informace najdete v tématu [Měřítko, Azure SQL databáze](https://msdn.microsoft.com/library/azure/dn495641.aspx) a [Azure SQL databáze služby úrovní a výkonu úrovně](../sql-database/sql-database-service-tiers.md).
    - **Dodržováním ustanovení zákona HIPAA**: zdravotní péče zákazníci a (nezávisle na Software ISV) může zvolte [SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md) místo [Databáze SQL Azure](../sql-database/sql-database-technical-overview.md) , protože SQL Server ve Azure virtuálního počítače se vztahuje tak, že HIPAA firmy přidružení dohodu (BÁ). Informace o dodržování předpisů najdete v tématu [Microsoft Azure Trust Center: dodržování předpisů](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Funkce úrovni instance**: V současné době databáze SQL nepodporuje funkce, které live mimo databáze (například propojené servery Agent úlohy, FileStream zprostředkovatel služeb, atd.). Další informace najdete v článku [pokyny databáze SQL Azure](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>úrovně 1 (jednoduchý): jeden počítač virtuální

V tomto vzoru aplikace nasazení aplikace systému SQL Server a databázi do samostatného virtuálního počítače v Azure. Stejném počítači virtuální obsahuje klienta/webové aplikace, komponenty business, vrstvy přístupu k datům a databázovém serveru. Prezentaci, obchodní nebo přístupový kód dat jsou logicky oddělené ale fyzicky nacházejí ve počítače v jednom serveru. Většina zákazníků začínat této aplikace vzorku a potom rozšiřování přidáním více web role nebo virtuálních počítačích k jejich systému.

Tento způsob aplikace je užitečné situacích:

- Umožňuje provádět jednoduché migrovat na Azure platformu k vyhodnocení zda platformu odpovídá požadavky aplikace nebo ne.

- Chcete mít všechny úrovně aplikace použitý ve stejném počítači virtuální v centru Azure dat zmenšit latenci mezi úrovní.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

Následující diagram ukazuje jednoduchou místní scénář a nasazení řešení jeho cloudu povolené v jednom počítači virtuální v Azure.

![1 osy aplikace vzorek](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Nasazení vrstvy business (obchodní logiky a datových součásti) ve stejné pole fyzicky vrstvě jako vrstvy prezentace můžete zvýšení výkonu aplikace nepoužíváte musí samostatné osy kvůli pochybnosti škálovatelnost nebo zabezpečení.

Protože je velmi běžné vzorek začínat, mohou být užitečné následujícím článku na migraci přesunutí dat do vašeho OM serveru SQL: [Migrace databáze SQL Server na OM Azure](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 – osy (jednoduchý): více virtuálních počítačích

V tomto vzoru aplikace nasazení aplikace 3 osy v Azure tak, že zaškrtnete jednotlivé vrstvy aplikace v jiném počítači virtuální. To nabízí flexibilní prostředí snadno měřítko zdola nahoru a škálování scénářích. Když v jednom počítači virtuální obsahuje klienta/webové aplikace, druhý hostuje komponenty business a druhý hostuje databázový server.

Tento způsob aplikace je užitečné situacích:

- Chcete provedení migrace složité databázových aplikací na virtuálních počítačích Azure.

- Chcete, aby jiné aplikace úrovní hostování v různých oblastech. Například může sdílení databáze, které jsou používaný více oblastí pro účely vykazování.

- Chcete přesunout podnikových aplikací z místních virtualizované platformy na virtuálních počítačích Azure. Podrobné informace o podnikových aplikací naleznete v tématu [Co je podniková aplikace](https://msdn.microsoft.com/library/aa267045.aspx).

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

Následující diagram ukazuje, jak můžete provádět jednoduché aplikace 3 osy v Azure tak, že zaškrtnete jednotlivé vrstvy aplikace v jiném počítači virtuální.

![aplikace 3 osy vzorek](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

V tomto vzoru aplikace obsahuje jednotlivé vrstvy pouze v jednom počítači virtuální (OM). Pokud máte víc VMs v Azure, doporučujeme nastavení virtuální sítě. [Virtuální sítě Azure](../virtual-network/virtual-networks-overview.md) vytvoří hranici důvěryhodných zabezpečení a také umožňuje VMs mezi sebou komunikovat soukromé IP adresu. Kromě toho vždy ujistěte se, že všech připojení k Internetu pouze přejděte vrstva prezentace. Při této aplikace vzorku, spravovat pravidla skupiny zabezpečení sítě pro řízení přístupu. Další informace najdete v tématu [Povolit externí přístup k vaší OM pomocí portálu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

V diagramu lze Internet protokoly TCP UDP, HTTP a HTTPS.

>[AZURE.NOTE] Nastavení virtuální sítě v Azure je zdarma. Ale vám bude účtovaná za Brána VPN, který se připojuje k místní. Tento poplatek se podle počtu delším připojení zřizování a k dispozici.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>úrovně 2 a 3 vrstvy během prezentace úroveň škálování

V tomto vzoru aplikace nasadíte úroveň 2 nebo 3 úroveň databázové aplikace na virtuálních počítačích Azure tak, že zaškrtnete jednotlivé vrstvy aplikace v jiném počítači virtuální. Kromě toho můžete rozšiřování osy prezentace z důvodu lepší objemu příchozí žádosti klienta.

Tento způsob aplikace je užitečné situacích:

- Chcete přesunout podnikových aplikací z místních virtualizované platformy na virtuálních počítačích Azure.

- Chcete-li rozšiřování osy prezentace z důvodu lepší objemu příchozí žádosti klienta.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

- Chcete-li vlastní prostředí infrastruktura, který můžete přizpůsobit nahoru a dolů na vyžádání.

Následující diagram ukazuje, jak můžete umístit úrovní aplikace více virtuálních počítačích v Azure tak, že rozšiřování osy prezentace z důvodu lepší objemu příchozí žádosti klienta. Jak je vidět v diagramu, je zodpovědný za distribuce přenosy napříč několika virtuálních počítačích a také zjištění, jaký webový server pro připojení k vyrovnávání zatížení Azure. S několika instancích webových serverů za vyrovnávání zatížení zaručuje dostupnost osy prezentace.

![Aplikace vzorek – měřítko osy prezentace se](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Doporučené postupy pro úroveň 2, 3 osy nebo n úroveň vzorky, které mají víc VMs v jedné úrovně

Je vhodné umístěte virtuálních počítačích, které patří na stejné úrovni ve stejné služby cloudu a ve stejném dostupnosti nastavení. Například zaškrtnout sadu webových serverů **CloudService1** a **AvailabilitySet1** a sadu databázových serverů v **CloudService2** a **AvailabilitySet2**. Dostupné v Azure umožňuje umístěte uzly dostupnost do samostatných poruch domén a upgrade domény.

Můžete využít více OM instancí vrstva, budete muset konfigurace služby Vyrovnávání zatížení Azure mezi aplikací úrovní. Konfigurace služby Vyrovnávání zatížení v každé vrstvy, vytvoření koncového bodu Vyrovnávání zatížení na jednotlivé vrstvy VMs samostatně. Pro konkrétní vrstvy nejprve vytvořte VMs ve stejném cloudové služby. Zajistíte tak, že mají stejné veřejné virtuální IP adresu. Dále vytvořte koncový bod k některé z virtuálních počítačích u této osy. Přiřaďte stejný koncový bod k virtuálních počítačích u této osy pro vyrovnávání zatížení. Vytvořením sady Vyrovnávání zatížení distribuce přenosy napříč několika virtuálních počítačích a umožňují Vyrovnávání zatížení rozhodnout, jaký uzel a připojte se při selhání uzlu OM back-end. Například s několika instancích webových serverů za vyrovnávání zatížení zaručuje, dostupnost osy prezentace.

Osvědčený Ujistěte se vždy, že všech připojení k Internetu nejdřív přejděte vrstva prezentace. Vrstvy prezentace zpřístupňuje obchodní osy a potom osy firmy přistupuje osy dat. Další informace o tom, jak povolit přístup k vrstvě prezentace najdete v článku [Povolit externí přístup k vaší OM pomocí portálu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Všimněte si, že vyrovnávání zatížení v Azure funguje podobně jako načíst vyrovnávání v místním prostředí. Další informace najdete v tématu [služby Azure infrastruktury služby Vyrovnávání zatížení](virtual-machines-windows-load-balance.md).

Kromě toho doporučujeme, aby se nastavení privátní sítě pro virtuálních počítačích pomocí Azure virtuální sítě. Díky tomu, aby si mezi sebou komunikovat soukromé IP adresu. Další informace najdete v tématu [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>úrovně 2 a 3 vrstvy s business úroveň škálování

V tomto vzoru aplikace nasadíte úroveň 2 nebo 3 úroveň databázové aplikace na virtuálních počítačích Azure tak, že zaškrtnete jednotlivé vrstvy aplikace v jiném počítači virtuální. Kromě toho můžete distribuovat aplikace komponenty serveru pro více virtuálních počítačích kvůli složitosti aplikace.

Tento způsob aplikace je užitečné situacích:

- Chcete přesunout podnikových aplikací z místních virtualizované platformy na virtuálních počítačích Azure.

- Chcete-li distribuovat aplikace komponenty serveru pro více virtuálních počítačích kvůli složitosti aplikace.

- Chcete-li přesunout obchodní logiky těžké místní LOB (řádek obchodní) aplikací na virtuálních počítačích Azure. Jsou aplikace LOB množiny důležitou počítačové aplikace, které jsou zásadní spuštění licenci enterprise, například účetní lidských zdrojů (h), mzdy, řízení a plánování aplikace zdrojů.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

- Chcete-li vlastní prostředí infrastruktura, který můžete přizpůsobit nahoru a dolů na vyžádání.

Následující diagram ukazuje scénáři místní a jeho cloudu povolené řešení. V tomto scénáři umístěte úrovní aplikace ve více virtuálních počítačích v Azure tak, že rozšiřování osy firmy obsahuje obchodní logiky osy a komponenty Accessu data. Jak je vidět v diagramu, je zodpovědný za distribuce přenosy napříč několika virtuálních počítačích a také zjištění, jaký webový server pro připojení k vyrovnávání zatížení Azure. S několika instancí aplikace serverů za vyrovnávání zatížení zaručuje dostupnost firmy osy. Další informace najdete v tématu [Doporučené postupy pro úrovně 2, 3 nebo vzorek n složek aplikace, které mají víc virtuálních počítačích v jedné úrovni](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Aplikace vzorek firmy osy měřítko](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>úrovně 2 a 3 vrstvy s prezentace a obchodní úrovní škálování a HADR

V tomto vzoru aplikace nasadíte úroveň 2 nebo 3 úroveň databázové aplikace na virtuálních počítačích Azure pomocí distribuce prezentace osy (webový server) a komponenty firmy osy (Aplikační server) pro víc virtuálních počítačích. Kromě toho implementovat vysoké dostupnosti a havárie obnovení řešení pro databáze v Azure virtuálních počítačích.

Tento způsob aplikace je užitečné situacích:

- Chcete přesunout podnikových aplikací z virtualizovaném platformách místní Azure implementací dostupnost SQL serveru a možnosti obnovení havárie.

- Chcete-li rozšiřování osy prezentace a úroveň business z důvodu lepší objemu příchozí žádosti klienta a složitosti aplikace.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

- Chcete-li vlastní prostředí infrastruktura, který můžete přizpůsobit nahoru a dolů na vyžádání.

Následující diagram ukazuje scénáři místní a jeho cloudu povolené řešení. V tomto scénáři rozšiřování osy prezentace a obchodní osy součástí více virtuálních počítačích v Azure. Kromě toho byste implementovat vysoké dostupnosti a havárie obnovení (HADR) technik pro databáze systému SQL Server Azure.

Spuštění více kopií aplikace v různých VMs zkontrolujte, že jste žádosti o napříč nimi Vyrovnávání zatížení. Pokud máte víc virtuálních počítačích, budete muset zkontrolujte, že všechny VMs přístupných osobám s postižením a pracovního na jednom místě v čase. Pokud jste nakonfigurovali, Vyrovnávání zatížení, Vyrovnávání zatížení Azure sleduje zdraví VMs a směrovat tak, aby příchozí hovory přesměrují do správný funkční uzly OM správně. Informace o tom, jak nastavit vyrovnávání zatížení virtuálních počítačích najdete v tématu [Azure infrastruktury služby Vyrovnávání zatížení](virtual-machines-windows-load-balance.md). Dostupnost úrovní prezentace a business s několika instancích web a aplikace serverů za vyrovnávání zatížení zaručuje.

![Dostupnost škálování a vysoký](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Doporučené postupy pro aplikaci vzorků vyžadující SQL HADR

Při nastavení serveru SQL Server dostupnost a řešení obnovení havárie ve virtuálních počítačích Azure nastavení virtuální síť pro vaše virtuálních počítačích pomocí [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) je povinný.  Virtuálních počítačích v rámci virtuální sítě bude mít stabilní soukromé IP adresu i po výpadek služeb, tedy vyhnout se aktualizaci dobu potřebnou pro služeb překládá názvy DNS název. Virtuální sítě navíc umožňuje rozšíření místních sítě Azure a vytvoří hranici důvěryhodných zabezpečení. Pokud aplikace má omezení podnikové domény (například ověřování systému Windows, služby Active Directory), nastavte si [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) je například potřebné.

Většina zákazníci, kteří na Azure běží výrobní kód, jsou uchovávání primárních a sekundárních repliky v Azure.

Podrobné informace a výukové programy pro na dostupnost a technik pro obnovení havárie najdete v tématu [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>úrovně 2 a 3 oddělených pomocí Azure VMs a Cloudovým službám

V tomto vzoru aplikace nasadit úroveň 2 nebo 3 úroveň aplikaci Azure pomocí [Služby Azure Cloud Services](../cloud-services/cloud-services-choose-me.md#tellmecs) (web pracovního rolí a - platformy jako služba (PaaS)) a [Azure virtuálních počítačích](virtual-machines-windows-about.md) (infrastruktury služby (IaaS)). Pomocí [Služby Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) pro prezentaci osy/firmy osy a SQL Server ve [virtuálních počítačích Azure](virtual-machines-windows-about.md) osy dat je užitečné pro většinu spuštěné v Azure. Důvodem je, že máte výpočetním instance spuštěna Cloudovým službám poskytuje Snadná správa nasazení, sledování a škálování.

Cloudovým službám Azure udržuje infrastrukturu pro vás provede údržbu, opravy operačních systémech a snaží obnovení ze služby a hardware k chybám. Pokud aplikace potřebuje škálování, a opakovaně dělá sám ruční škálování možnosti jsou dostupné pro váš projekt služby cloudu zvýšení nebo snížení počtu instancí nebo virtuálních počítačích, které se používají v aplikaci. Kromě toho můžete Visual Studio v místním nasazení aplikace do cloudové služby projektu v Azure.

Souhrn nechcete vlastní rozsáhlé úroveň úkoly správy pro prezentaci/firmy a aplikace není nutná žádná složité konfigurace software nebo operační systém, použijte Azure Cloud Services. Pokud databáze SQL Azure nepodporuje všechny funkce verze, kterou hledáte, pomocí serveru SQL Server v Azure virtuálního počítače osy dat. Spuštění aplikace v Azure Cloudovým službám a ukládání dat v Azure virtuálních počítačích kombinuje výhody obě služby. Podrobné porovnání naleznete v části v tomto tématu na [Comparing vývoj strategie v Azure](#comparing-web-development-strategies-in-azure).

V tomto vzoru aplikace osy prezentace obsahuje role web, která komponentu Cloudovým službám běží v prostředí Azure spuštění a je přizpůsobená k programování webové aplikace, jak je podporován aplikací služby IIS a ASP.NET. Vrstvy business nebo back-end obsahuje role pracovníka, která komponentu Cloudovým službám běží v prostředí Azure spuštění a je užitečné pro obecný vývoj a může provádět pozadí zpracování pro web roli. Vrstvy databáze je umístěn v počítači virtuálního serveru SQL Server v Azure. Komunikace mezi osy prezentace a osy databáze se stane přímo nebo prostřednictvím osy business – součásti pracovního role.

Tento způsob aplikace je užitečné situacích:

- Chcete přesunout podnikových aplikací z virtualizovaném platformách místní Azure implementací dostupnost SQL serveru a možnosti obnovení havárie.

- Chcete-li vlastní prostředí infrastruktura, který můžete přizpůsobit nahoru a dolů na vyžádání.

- Databáze SQL Azure nepodporuje všechny funkce, které potřebuje vaše aplikace nebo databáze.

- Chcete-li provést namáhání testování pro různé úrovně pracovní zátěž, ale ve stejnou dobu nechcete vlastníte a udržovat mnoha fyzické počítačů vždy.

Následující diagram ukazuje scénáři místní a jeho cloudu povolené řešení. V tomto scénáři umístěte osy prezentace v web role, obchodní osy v jednotlivých rolích pracovníka ale osy dat ve virtuálních počítačích v Azure. Spuštění více kopií prezentace osy v různých webových role zaručuje načíst požadavků na vyrovnávání mezi nimi. Při kombinování Azure Cloud Services s Azure virtuálních počítačích doporučujeme, aby nastavíte [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) taky. [Virtuální sítě Azure](../virtual-network/virtual-networks-overview.md)může mít stabilní a trvalý soukromé IP adresy ve stejném cloudové službě v cloudu. Po definování virtuální sítě pro virtuálních počítačích a cloudových služeb, budou začínat, komunikaci mezi sebou přes soukromé IP adresu. Kromě toho s virtuálních počítačích a Azure webové či pracovníka role ve stejné [Virtuální sítě Azure](../virtual-network/virtual-networks-overview.md) poskytuje zhoršeným latence a líp zabezpečená připojení. Další informace najdete v tématu [Co je Cloudová služba](../cloud-services/cloud-services-choose-me.md).

Jak je vidět v diagramu, Vyrovnávání zatížení Azure rozdělí přenosy více virtuálních počítačích a také určí, jaký webový server nebo aplikační server pro připojení k. S několika instancích web a aplikace serverů za vyrovnávání zatížení zaručuje dostupnost osy prezentace a obchodní osy. Další informace najdete v tématu [Doporučené postupy pro aplikaci vzorků vyžadující SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Aplikace vzorků Cloudovým službám](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Další způsob, jak implementovat této aplikace vzorku, je použít role konsolidované web, který obsahuje prezentace osy a komponenty firmy vrstvy, jak je znázorněno na následujícím obrázku. Tento způsob aplikace je užitečné pro aplikace, které vyžadují stavové návrh. Protože Azure poskytuje příslušnosti výpočetním uzly na web a pracovní role, doporučujeme, abyste implementace logiky pro ukládání stavu relace pomocí jedné z následujících technologií: [Ukládání do mezipaměti Azure](https://azure.microsoft.com/documentation/services/redis-cache/), [Úložiště tabulek Azure](../storage/storage-dotnet-how-to-use-tables.md) nebo [Databáze SQL Azure](../sql-database/sql-database-technical-overview.md).

![Aplikace vzorků Cloudovým službám](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Vzorek Azure VMs databáze Azure SQL a služba Azure aplikací (webové aplikace)

Primární cíle vzorku tato aplikace je předvedení sloučit Azure infrastruktury jako součásti služby (IaaS) s Azure platformy jako služby součástí (PaaS) řešení. Tento způsob se zaměřuje na databázi SQL Azure pro ukládání relačních dat. Neobsahuje SQL serveru v Azure virtuální počítač, který je součástí Azure infrastruktury jako nabídky služeb.

V tomto vzoru aplikace nasadit aplikaci databáze Azure umístěním úrovní prezentace a business ve stejném počítači virtuální a přístup do databáze v databázi SQL Azure (databáze SQL) servery. Implementace osy prezentace pomocí tradiční založené na službě IIS webová řešení. Nebo můžete implementovat spojenou prezentaci a úroveň firmy pomocí [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Tento způsob aplikace je užitečné situacích:

- Už máte existující databáze SQL server nakonfigurovaný v Azure a chcete-li rychle testovat aplikace.

- Chcete testovat funkce Azure prostředí.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Vaše obchodní logiky a data komponenty Accessu můžou být obsaženy v rámci webové aplikace.

Následující diagram ukazuje scénáři místní a jeho cloudu povolené řešení. V tomto scénáři umístěte úrovní aplikace v jednom počítači virtuální Azure a přístup data v databázi SQL Azure.

![Smíšené aplikace vzorek](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Pokud se rozhodnete provádět kombinované web a vrstva aplikace pomocí Azure Web Apps, doporučujeme ponechat osy vícevrstvé nebo aplikace jako dynamické knihovny (DLL) v rámci webové aplikace.

Kromě toho zkontrolujte doporučení uvedených v části [Comparing web vývoj strategie v Azure](#comparing-web-development-strategies-in-azure) na konci tohoto článku Další informace o programování postupy.

## <a name="n-tier-hybrid-application-pattern"></a>Vzorek N-osy hybridní aplikace

V n osy hybridní aplikace vzorek implementace aplikace ve více úrovní distributed mezi místním prostředím a Azure. Proto vytvářet flexibilní a opakovaně použitelný hybridní systému, který můžete upravit nebo přidat konkrétní vrstvu beze změny ostatních vrstev. Rozšířit podnikové síti do cloudu, můžete pomocí služby [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) .

Tento způsob aplikace hybridní je užitečné situacích:

- Chcete-li vytvářet aplikace, které spustit částečně v cloudu a částečně místní.

- Chcete-li migrace některé nebo všechny prvky existující místní aplikace do cloudu.

- Chcete-li přesunout podnikových aplikací z místních virtualizované platformách Azure.

- Chcete-li vlastní prostředí infrastruktura, který můžete přizpůsobit nahoru a dolů na vyžádání.

- Chcete rychle zřízení vývoj a testovacím prostředí pro krátká časová období.

- Chcete nákladů efektivní způsob, jak provést zálohování pro podnikové databáze aplikace.

Následující diagram ukazuje vzorek aplikace hybridní n osy, který se nachází mezi místním prostředím a Azure. Jak je vidět v diagramu, infrastruktura místní zahrnuje řadiče domény [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) podporuje uživatele a tak mohli ověřovat. Všimněte si, že diagram znázorňuje situace, kdy některé části funkcí osy dat live v datovém centru místní že některé části funkcí osy dat bloky v Azure. Podle potřeb vaší aplikace můžete provádět několik ostatních případech hybridní. Například si může Poznámkový blok osy prezentace a úroveň firmy místním prostředí, ale osy dat v Azure.

![Vzorek N složek aplikace](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

V Azure můžete použít služby Active Directory jako samostatný adresář cloudu pro vaši organizaci nebo existující na adresářová služba Active Directory taky můžete integrovat s [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Jak je vidět v diagramu, komponenty vrstvy firmy lze získat přístup k více zdrojům dat, jako je třeba k [Serveru SQL Server Azure](virtual-machines-windows-sql-server-iaas-overview.md) pomocí soukromé vnitřní IP adresy, chcete-li místního serveru SQL prostřednictvím [Virtuální sítě Azure](../virtual-network/virtual-networks-overview.md)nebo [Databáze SQL](../sql-database/sql-database-technical-overview.md) pomocí technologie zprostředkovatel dat .NET Framework. V tomto diagramu databáze SQL Azure je služba úložiště volitelná data.

Ve vzorku n osy hybridní aplikace můžete provádět následující pracovního postupu v uvedeném pořadí:

1. Určení enterprise databázové aplikace, které je potřeba přesunout nahoru do cloudu pomocí [Microsoft hodnocení a nástrojů plánování (mapa)](http://microsoft.com/map). Sada nástrojů MAPY shromažďuje zásob a výkonu data z počítače, které jsou zvažovat pro virtualizaci a doporučení týkající se kapacita poznámek a plánování hodnocení.

1. Plán zdrojů a konfigurace potřebná v Azure informacemi o platformě, například úložiště účty a virtuálních počítačích.

1. Nastavení připojení k síti mezi podnikové síti místní a [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md). Pokud chcete nastavit propojení podnikové síti místním prostředím a virtuálního počítače v Azure, použijte jeden z následujících dvou způsobů:

    1. Spojení mezi místním prostředím a Azure prostřednictvím veřejné koncové body v počítači virtuální v Azure. Tato metoda poskytuje jednoduché instalačního programu a umožňuje použít ověřování serveru SQL Server ve virtuálních počítači. Kromě toho nastavte pravidla skupiny zabezpečení vaší sítě řídit veřejné přenos bude v angličtině. Další informace najdete v tématu [Povolit externí přístup k vaší OM pomocí portálu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Vytvoření připojení mezi místním prostředím a Azure prostřednictvím tunelem Azure virtuální privátní sítě (VPN). Tento způsob umožňuje rozšíření zásady domény do virtuálního počítače v Azure. Kromě toho můžete nastavit pravidla brány firewall a pomocí ověřování Windows ve počítače virtuální. V současné době Azure podporuje připojení VPN čárky webu a zabezpečené VPN na webu:

        - S zabezpečené připojení k webu můžete vytvořit připojení k síti mezi místní sítě a virtuální sítě v Azure. Je vhodné pro připojení prostředí místních dat centra Azure.

        - S zabezpečené připojení čárky webu můžete vytvořit připojení k síti mezi síti virtuální v Azure a jednotlivé počítače se systémem kdekoli. Doporučujeme převážně vývoj a testování jako referenci.

    Informace o tom, jak připojit k serveru SQL Server Azure najdete v článku [připojení k SQL serveru virtuálního počítače na Azure](virtual-machines-windows-classic-sql-connect.md).

1. Nastavte si naplánovaných úloh a upozornění, které obecnějším údajům místních dat do virtuálního počítače disk v Azure. Další informace najdete v tématu [SQL Server zálohování a obnovení se službou úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148.aspx) a [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md).

1. Podle potřeby aplikace implementace jednu z následujících tří obvyklé scénáře:

    1. Na databázovém serveru v Azure můžete chránit váš webový server, aplikační server a malá a velká písmena data, že zachovat citlivá data na pracovišti.

    1. Zachovat webového serveru a aplikační server místní vzhledem k tomu databázový server ve virtuálních počítači v Azure.

    1. Můžete ponechat databázový server, webového serveru a aplikační server místní že si Poznámkový blok virtuálních počítačích v Azure repliky databáze. Toto nastavení umožňuje místní webových serverů nebo aplikacích pro vytváření sestav pro přístup k databázi replikám v Azure. Proto můžete dosáhnout posunout níž pracovní zátěž v místní databázi. Doporučujeme, abyste tento scénář pro těžké implementovat číst pracovního vytížení a vývojové účely. Informace týkající se vytváření repliky databáze v Azure najdete v tématu skupiny dostupnosti AlwaysOn na [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Porovnání web vývoj strategie v Azure

Implementaci a nasazení vícevrstvého aplikaci SQL Server Azure, můžete použít jednu z následujících dvou programovacího metod:

- Nastavte si tradiční webového serveru (IIS - Internetové informační služby) v Azure a přístup k databázi serveru SQL Server ve virtuálních počítačích Azure.

- Implementaci a nasazení do cloudové služby Azure. Zkontrolujte, jestli mají přístup k této cloudové služby databáze na serveru SQL Server v Azure virtuálních počítačích. Do cloudové služby může obsahovat více web a pracovní role.

Následující tabulka obsahuje porovnání vývoj tradiční webu s Azure cloudovými službami a Azure Web Apps s ohledem na serveru SQL Server ve virtuálních počítačích Azure. Tabulka obsahuje Azure Web Apps, jako je možné použít SQL Server v Azure OM jako zdroje dat pro Azure Web Apps prostřednictvím jeho veřejné virtuální IP address or nebo název DNS.

||Vývoj tradiční webu ve virtuálních počítačích Azure|Cloudové služby Azure|S Azure Web Apps pro hostování webů|
|---|---|---|---|
|**Aplikací migrací z místního**|Existující aplikace jako-je.|Aplikace potřebují web a pracovní role.|Existující aplikace jako-se ale hodí pro samostatné webových aplikací a webové služby, které vyžadují snadné škálovatelnost.|
|**Vývojové a nasazení**|Visual Studio WebMatrix, vizuální Web Developer, WebDeploy, FTP, TFS, Správce služby IIS, Powershellu.|Visual Studio Azure SDK, TFS, Powershellu. Každý cloudové služby má dvě prostředí, ke kterým můžete nasadit služby balíčku a konfigurace: pracovní a výroby. Pracovní prostředí ji otestujte před převést na výrobní nástroje můžete nasazovat do cloudové služby.|Visual Studio WebMatrix, vizuální Web Developer, FTP, libovolná, BitBucket, CodePlex, Dropboxu, GitHub, Mercurial, TFS, Web nasadit, Powershellu.|
|**Správa a nastavení**|Zodpovídáte za úkoly správy na aplikaci, data, pravidla brány firewall, virtuální sítě a operační systém.|Zodpovídáte za úkoly správy na aplikaci, data, pravidla brány firewall a virtuální sítě.|Zodpovídáte za úkoly správy na aplikace a pouze data.|
|**Dostupnost a obnovení (HADR)**|Doporučujeme umístit virtuálních počítačích v sadě stejné dostupnost a ve stejné cloudové služby. Zachování svého VMs ve stejném dostupnost sada umožňuje Azure umístění uzlů dostupnost do samostatných poruch domén a upgrade domény. Podobně zachování svého VMs ve stejném cloudové služby umožňuje Vyrovnávání zatížení a VMs můžete komunikovat přímo k sobě navzájem místní síti v Azure datacentra.<br/><br/>Zodpovídáte za provádění dostupnost a havárie zotavení řešení pro systém SQL Server ve virtuálních počítačích Azure Chcete-li předejít všechny výpadek služeb. Podporované HADR technologií najdete v článku [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Zodpovídáte za zálohování dat a aplikace.<br/><br/>Když na hostitelském počítači v datovém centru selže kvůli problémům s hardwarem k Azure přesouvat virtuálních počítačích. Kromě toho je možné plánované odstávky vaší OM při aktualizaci na hostitelském počítači zabezpečení nebo softwaru aktualizace. Proto doporučujeme udržovat aspoň dva VMs v každé vrstvy aplikace pro zajištění nepřetržitý dostupnosti. Azure neposkytuje SLA pro jeden počítač virtuální. Další informace najdete v tématu [příručku Azure odolnost proti chybám](../resiliency/resiliency-technical-guidance.md).|Azure spravuje chyby vrácené základní software hardware nebo operační systém. Doporučujeme implementace několika instancích webu nebo pracovního role zajistit dostupnost aplikace. Informace najdete v tématu [cloudové služby, virtuálních počítačích a virtuální sítě úroveň smlouvu o poskytování služeb](http://www.microsoft.com/download/details.aspx?id=38427) a [obnovení havárie dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Zodpovídáte za zálohování dat a aplikace.<br/><br/>U databází umístěných v databázi SQL serveru v Azure OM zodpovídáte za implementace vysoké řešení obnovení dostupnost a havárie Chcete-li předejít všechny výpadek služeb. Podporované HDAR technologií najdete v článku dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure.<br/><br/>**Databáze SQL serveru odrážející strukturu**: pomocí služby Azure Cloud Services (webu nebo pracovního role). SQL Server VMs a projekt služby cloudu může být ve stejné síti virtuální Azure. Nejsou-li OM SQL Server ve stejné virtuální síti, je potřeba vytvořit Alias SQL serveru směrovat komunikace instanci systému SQL Server. Kromě toho alias jméno se musí shodovat název serveru SQL Server.|Dostupnost pochází z Azure pracovníka role, úložišti objektů blob Azure a databáze SQL Azure. Například úložišti Azure udržuje tři kopie všech objektů blob tabulce a data fronty. V daném čase, databáze SQL Azure pořád tři kopie dat spuštěná – jeden primární otevřené a dvě sekundární repliky. Další informace najdete v tématu [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/) a [Databáze SQL Azure](../sql-database/sql-database-technical-overview.md).<br/><br/>Při použití SQL serveru v Azure OM jako zdroje dat pro Azure Web Apps, mějte na paměti, že Azure Web Apps nepodporuje Azure virtuální sítě. Jinými slovy musíte všechna připojení z Azure webových aplikací pro SQL Server VMs v Azure absolvovat veřejné koncové body virtuálních počítačích. To může způsobit určitá omezení pro dostupnost a havárie zotavení scénáře. Například klientské aplikace na Web Apps Azure připojení k SQL serveru OM s odrážející strukturu databáze nebude moct připojit k serveru daného nové primární odrážející strukturu databáze vyžaduje nastavení Azure virtuální sítě mezi VMs hostitele SQL serveru v Azure. Proto pomocí **Odrážející strukturu databázi serveru SQL** Azure Web Apps není aktuálně podporován.<br/><br/>**SQL Server AlwaysOn dostupnost skupin**: můžete vytvořit skupiny dostupnosti AlwaysOn při použití Azure Web Apps v SQL Server VMs v Azure. Ale musíte nakonfigurovat AlwaysOn dostupnost skupiny posluchače komunikace směrovat primární otevřené prostřednictvím veřejné Vyrovnávání zatížení koncové body.|
|**Křížové místní připojení**|Můžete se připojit k místní Azure virtuální sítě.|Můžete se připojit k místní Azure virtuální sítě.|Je podporován Azure virtuální sítě. Další informace najdete v tématu [Integrace virtuální sítě webové aplikace](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Škálovatelnost:**|Zvětšení velikosti virtuálního počítače nebo přidáním více disků neexistuje měřítko zdola nahoru. Další informace o velikosti virtuálního počítače najdete v článku [Virtuálního počítače velikosti Azure](virtual-machines-windows-sizes.md).<br/><br/>**Databázový Server**: škálování je k dispozici prostřednictvím technik rozdělení databáze a SQL Server AlwaysOn dostupnost skupiny.<br/><br/>Hodně čtení úloh můžete [Skupiny dostupnosti AlwaysOn](https://msdn.microsoft.com/library/hh510230.aspx) na více sekundární uzly i replikace SQL serveru.<br/><br/>Hodně zápisu úloh můžete mezi servery fyzické poskytnout aplikaci škálování implementovat vodorovné rozdělení data.<br/><br/>Kromě toho můžete implementovat škálování pomocí [Serveru SQL Server Data závislá směrování](https://technet.microsoft.com/library/cc966448.aspx). S dat závislá směrování (DDR), musíte k provedení dělení mechanismus v klientské aplikaci, obvykle ve vrstvě osy firmy směrování požadavky databáze do více uzlů SQL serveru. Vrstvy firmy obsahuje mapování jak data oddíly a který uzel s daty.<br/><br/>Změna velikosti spuštěné virtuálních počítačích. Další informace najdete v tématu [jak měřítko aplikace](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Poznámka**: funkci **Automatické měřítko** v Azure umožňuje automaticky zvětšit nebo zmenšit virtuálních počítačích, které se používají v aplikaci. Tato funkce zaručuje, že koncovým problém se netýká negativně obdobích Špička a VMs jsou vypnou poptávka je nízká. Je vhodné, že nenastavíte automatické měřítko možnost cloudové službě pokud obsahuje VMs SQL serveru. Důvodem je, že funkce Automatické měřítko umožňuje Azure zapnout virtuálního počítače po vyšší než některé mezní využití procesoru v této OM a vypněte virtuálního počítače při využití procesoru vzroste nižší než ji. Funkci Automatické měřítko je užitečné pro příslušnosti aplikací, jako je webových serverů, kde můžete všechny OM spravovat pracovní zátěž bez všechny odkazy na předchozí stav. Funkce Automatické měřítko však není užitečný pro stavové aplikacemi, třeba SQL Server, kde jenom jedna instance umožňuje psaní do databáze.|Pomocí několika role web a pracovní neexistuje měřítko zdola nahoru. Další informace o velikosti virtuálního počítače web rolí a kolegy najdete v článku [Konfigurace velikosti Cloudovým službám](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Při používání **Cloudové služby**, můžete definovat několik rolí distribuce zpracování a také dosažení flexibilní měřítka aplikace. Každý cloudové služby obsahuje jeden nebo více web rolí a/nebo pracovníka role, oba objekty mají vlastní soubory aplikace a konfigurace. Můžete měřítko nakreslenými do cloudové služby zvýšením počet instancí role (virtuálních počítačích) používaný pro roli a měřítko nohama do cloudové služby zmenšením počet instancí role. Podrobné informace najdete v tématu [Azure spuštění modely](../cloud-services/cloud-services-choose-me.md).<br/><br/>Škálování je k dispozici prostřednictvím předdefinované Azure dostupnost podpory prostřednictvím [cloudové služby, virtuálních počítačích a virtuální sítě úroveň smlouvu o poskytování služeb](http://www.microsoft.com/download/details.aspx?id=38427) a vyrovnávání zatížení.<br/><br/>Vícevrstvé aplikace doporučujeme připojení webové či pracovníka role aplikace na databázový server VMs prostřednictvím virtuální sítě Azure. Kromě toho Azure poskytuje Vyrovnávání zatížení pro VMs ve stejném cloudové služby, šíření koncových uživatelů mezi nimi. Virtuálních počítačích připojení tímto způsobem můžete komunikovat přímo k sobě navzájem místní síti v Azure datacentra.<br/><br/>Můžete nastavit **Automatické měřítko** na portálu Azure klasické, jakož i plánování doby. Další informace najdete v tématu [jak měřítko aplikace](../cloud-services/cloud-services-how-to-scale.md).|**Změna velikosti nahoru a dolů**: je můžete zvětšit nebo zmenšit velikost instance (OM) vyhrazená pro váš web.<br/><br/>Měřítko se: můžete přidat další rezervovaná instancí (VMs) pro váš web.<br/><br/>Můžete nastavit **Automatické měřítko** na portálu, jakož i plánování doby. Další informace najdete v tématu [jak měřítko Web Apps](../app-service-web/web-sites-scale.md).|

Další informace o výběru mezi tyto programovací metody, najdete v článku [Azure Web Apps, cloudovými službami a VMs: volba aplikace](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Další kroky

Další informace o aplikaci SQL Server Azure virtuálních počítačích najdete v článku [SQL Server na virtuálních počítačích přehled Azure](virtual-machines-windows-sql-server-iaas-overview.md).
