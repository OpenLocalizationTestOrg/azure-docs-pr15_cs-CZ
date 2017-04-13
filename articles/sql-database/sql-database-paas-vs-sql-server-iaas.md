<properties
    pageTitle="Databáze SQL (PaaS) a SQL Server v cloudu na VMs (IaaS) | Microsoft Azure"
    description="Zjistěte, kterou možnost SQL Server cloudu vešel aplikace: databáze SQL Azure (PaaS) nebo SQL Server v cloudu na virtuálních počítačích Azure."
    services="sql-database, virtual-machines"
    keywords="SQL Server cloudu, SQL Server v cloudu, PaaS databáze cloudu SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Zvolte obláčkem možnost SQL Server: databáze SQL Azure (PaaS) nebo SQL Server na Azure VMs (IaaS)

Azure má dvě možnosti pro hostování úloh v Microsoft Azure SQL serveru:

* [Databáze SQL Azure](https://azure.microsoft.com/services/sql-database/): databáze A SQL nativní do cloudu, nazývaný také platformy jako databázi služby (PaaS) nebo databáze jako služba (DBaaS), která je optimalizovaná pro vývoj aplikací software jako služby (SaaS). Nabízí kompatibilní se většina funkcí SQL serveru. Další informace o PaaS najdete v tématu [Co je PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server nainstalovaný a hostovaný v cloudu v systému Windows Server virtuálních počítačích (VMs) se systémem Azure, nazývaný také infrastrukturu jako služba (IaaS).
SQL Server na virtuálních počítačích Azure je optimalizována pro přenesení existujících aplikací systému SQL Server. Všechny verze a edicemi systému SQL Server jsou k dispozici. Nabízí 100 % kompatibilní se serverem SQL Server umožňuje hostovat tolik databáze jako potřebné a provádění křížově databázové transakce. Nabízí úplné řízení na serveru SQL Server a Windows.

Zjistěte, jak jednotlivé možnosti zapadá do platformu Microsoft dat a pomoc s odpovídajícími správné možnost vašim požadavkům pro firmy. Jestli určit jejich prioritu úspor náklady nebo minimální správy před všechno, co dalšího Tento článek vám může pomoci rozhodnout, jaký přístup poskytuje proti firmy požadavky, které že vám nejvíc.


## <a name="microsofts-data-platform"></a>Platformu data společnosti Microsoft

První důležité pochopit všech diskusí a místní databáze systému SQL Server Azure reprodukujte používané vše. Společnosti Microsoft datovou platformu využívá technologii serveru SQL Server a díky kterému je dostupný různých fyzické místního počítače a soukromé cloudu prostředí, prostředí hostovanou soukromé cloudu třetích stran a veřejné cloudu. SQL Server na virtuálních mchines Azure umožňuje potřebám jedinečný a různorodého firmy pomocí kombinací místním prostředím a hostující v cloudu nasazení při používání stejnou sadu serverovými produkty, vývojového nástroje a odborných informací v těchto prostředích.

   ![Cloudové možnosti SQL Server: SQL server na IaaS nebo SaaS SQL databáze v cloudu.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak je vidět v diagramu, můžete jednotlivých nabídek rozdělení úroveň správu, ke kterým máte prostřednictvím infrastruktury (na osy X) tak stupeň náklady efektivity dosáhnout úrovně sloučení databáze a automatizace (místní s osou Y).

Při návrhu aplikace, čtyři základní možnosti jsou k dispozici pro publikování na serveru SQL Server část aplikace:

- SQL Server na nevirtuálním fyzických počítačů
- SQL Server ve počítače místních virtualizované (soukromé cloudu)
- SQL Server ve počítače Azure virtuální (veřejné cloudu společnosti Microsoft)
- Databáze SQL Azure (veřejné cloudu společnosti Microsoft)

V následujících částech informace o serveru SQL Server ve veřejných cloudu společnosti Microsoft: databáze SQL Azure a SQL Server na Azure VMs. Kromě toho můžete prozkoumání místnosti schůzky běžné obchodní motivators pro určení, kterou možnost je nejvhodnější pro aplikace.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Bližší pohled na databázi SQL Azure a SQL Server na Azure VMs

**Databáze SQL Azure** je relační databáze – jako služba (DBaaS) hostované v Azure cloudu, které spadají do kategorie odvětví *Software jako-Service (SaaS)* a *Platformy jako-Service (PaaS)*. [Databáze SQL](sql-database-technical-overview.md) jsou založeny na standardizovaným hardware a software, který je vlastní hostovaný a spravovaný společností Microsoft. S SQL databáze můžete vytvořit přímo ve službě pomocí zabudovaných funkcí a funkcí. Při použití databáze SQL, který systému průběžného financování pomocí možnosti Zobrazit nahoru či oddálení pro větší power bez přerušení.

**SQL Server na virtuálních počítačích Azure (VMs)** spadají do kategorie odvětví *Infrastruktury jako-Service (IaaS)* a umožňuje spuštění systému SQL Server do virtuálního počítače v cloudu. Podobně jako k SQL databázi, je založená na standardizovaným hardware, který je vlastní hostovaný a spravovaný společností Microsoft. Pokud chcete použít SQL Server na virtuálního počítače, můžete buď platební – jako je cestu pro SQL Server licenci už součástí obrázku SQL Server nebo snadno použít stávající licence. Můžete taky snadno měřítko up/Page down a pozastavit nebo životopis OM podle potřeby.

Obecně jsou tyto dvě možnosti SQL optimalizovaná pro jiné účely:

- **Databáze SQL** optimalizován omezit celkové náklady na minimum pro vytváření a správu mnoho databází. Vzhledem k tomu, že není nutné spravovat virtuálních počítačích, operačního systému nebo software pro databázový snižuje náklady průběžné správě. Není nutné spravovat upgradů, dostupnost nebo [zálohy](sql-database-automated-backups.md). Obecně databáze SQL Azure může výrazně zvýšit počtu databází spravuje typu single IT nebo vývoj zdroje.
- **SQL Server spuštěna Azure VMs** je optimalizována pro přenesení existujících aplikací na Azure nebo rozšíření existujících místní aplikací do cloudu v hybridních nasazeních. Kromě toho můžete SQL Server ve počítače virtuální vyvíjet a otestovat tradiční aplikací systému SQL Server. Se serverem SQL Server na Azure VMs máte úplná administrátorská práva přes vyhrazenou instanci systému SQL Server a OM cloudové. Je ideální volby při organizace už má k dispozici ke správě virtuálních počítačích zdroje IT. Tyto funkce umožňují vytvářet vysoce přizpůsobený systém věnovat zvláštní výkon a dostupnost požadavky aplikace.

Následující tabulka shrnuje hlavní vlastnosti databáze a SQL serveru SQL Server na Azure VMs:

|       | Databáze SQL | SQL Server na Azure virtuálního počítače|
| -------------- | ------------ | ---------------------- |
| **Je ideální pro:** | Navržený cloudu aplikací, které mají čas omezení vývoj a marketing. |Existující aplikace, které vyžadují rychlé migrace do cloudu s minimálními změny. Rychlé vývoj a testování scénáře, kdy nechcete koupit místní testovacím serveru SQL Server hardwaru. |
|| Družstev, která potřebujete předdefinované dostupnost, havárie obnovení a upgrade pro databázi. |Týmy, které můžete konfigurovat a spravovat dostupnost havárie obnovení a opravy pro systém SQL Server. Některé za předpokladu, že automatické funkce výrazně zjednodušit takto. |
||Družstev, která nechcete, aby ke správě operačního systému a nastavení.| Pokud budete potřebovat prostředí přizpůsobený s oprávněními úplné správce.|
||Databáze až 1 TB nebo větší databází, které se dají [Rozdělit vodorovně nebo svisle](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) pomocí škálování vzorku.|Instance serveru SQL Server s až 64 TB úložiště. Instanci podporují tolik databáze podle potřeby. |
||[Vytváření Software jako-(SaaS) aplikace služeb](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migrace a vytváření organizace a hybridní aplikací.|
|||||
|**Zdroje:**|Nechcete, aby využívat IT materiály pro konfiguraci a správu infrastruktury základní, ale zaměřte se na aplikační vrstvě.|Máte několik IT zdrojů pro konfigurace a správy. Některé za předpokladu, že automatické funkce výrazně zjednodušit takto.|
|**Celkové náklady na vlastnictví:**|Eliminuje náklady na hardware a snižuje správní náklady.|Eliminuje náklady na hardware.|
|**Nepřerušený provoz:**|Kromě předdefinované odolnost proti chybám infrastruktury funkce databáze SQL Azure obsahuje funkce, třeba [Automatické zálohování](sql-database-automated-backups.md), [Obnovení v okamžiku](sql-database-recovery-using-backups.md#point-in-time-restore), [Geo obnovit](sql-database-recovery-using-backups.md#geo-restore)a [Aktivní Geo replikace](sql-database-geo-replication-overview.md) zvětšíte nepřerušený. Další informace najdete v tématu [Přehled kontinuitu firmy databáze SQL](sql-database-business-continuity.md).|SQL Server na Azure VMs umožňuje nastavení vysoké dostupnosti a havárie obnovovací řešení pro konkrétní potřeby vaší databáze. Proto můžete mít systém vysoce optimalizovaného pro aplikaci. Můžete otestovat a spusťte převzetí služeb při selhání za vás v případě potřeby. Další informace najdete v tématu [dostupnost a obnovení pro systém SQL Server na virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybridní cloudu:**|Aplikace místní přístup k datům v databázi SQL Azure.|S Azure VMs serveru SQL Server můžete mít aplikace, které se spouštějí částečně v cloudu a částečně místní. Můžete třeba rozšířit v místní síti a Active Directory Domain do cloudu pomocí [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md). Kromě toho můžete mohou být uloženy místní datových souborů v úložišti Azure pomocí [SQL Server datových souborů v Azure](http://msdn.microsoft.com/library/dn385720.aspx). Další informace najdete v tématu [Úvod do cloudu hybridní 2014 SQL serveru](http://msdn.microsoft.com/library/dn606154.aspx).|
||Podporuje [transakční replikace serveru SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako odběratele replikovat data.|Plně podporuje [transakční replikace SQL serveru](https://msdn.microsoft.com/library/mt589530.aspx), [Skupiny dostupnosti AlwaysOn](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), Integration Services a protokolu replikovat data. Navíc jsou plně podporovány tradiční zálohy systému SQL Server|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Obchodní podnětů pro výběr databáze SQL Azure nebo SQL Server na Azure VMs

### <a name="cost"></a>Pole náklady

Ať už jste při spuštění, který je strapped pro finanční nebo týmu ve založení společnosti, který pracuje v části omezení těsné rozpočtu, omezené prostředků je často primární ovladač při rozhodování o tom, jak hostitelem vašich databází. V tomto oddílu informace o stránce fakturace a licence základní úkoly v Azure s ohledem na následujících dvou možností relační databáze: databáze a SQL serveru SQL Server na Azure VMs. Můžete taky informace o výpočtech aplikace celkové náklady.

#### <a name="billing-and-licensing-basics"></a>Základní informace o licencování a fakturace

**Databáze SQL** prodeje zákazníkům jako služba, not s operátorem licenci.  [SQL Server na Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) prodává se však započítávány licence, kterou platíte za minutu. Pokud máte stávající licence, můžete také ho.  

V současné době **SQL databáze** je dostupná v několika úrovní služby, které jsou vám účtovat poplatky hodinové pevné sazbou podle vrstvy služeb a úroveň výkonu, které zvolíte. Kromě toho je faktura pro odchozí přenosy dat Internet na běžná [přenosu dat sazby](https://azure.microsoft.com/pricing/details/data-transfers/). Služba vrstvách Basic, Standard a Premium slouží k dosažení předvídatelná výkonu víceúrovňového výkon podle požadavků Špička aplikace. Můžete změnit mezi – úrovně služeb a výkonu úrovně podle potřeb vaší aplikace paletami výkon. Pokud databáze obsahuje objemné transakční a potřeb podporuje mnoho uživatelů najednou, doporučujeme vrstvy služeb Premium. Nejnovější informace o aktuálním podporované službě úrovní najdete v článku [Úrovní služby databáze SQL Azure](sql-database-service-tiers.md). Můžete taky vytvořit [fondů pružná databáze](sql-database-elastic-pool.md) pro sdílení zdrojů výkonu mezi instancemi databáze.

S **Databáze SQL**software pro databázový je automaticky nakonfigurované opravené a upgradovat společností Microsoft, který snižuje správy náklady. Kromě toho jeho [předdefinovaný zálohování](sql-database-automated-backups.md) funkce pomáhají dosáhnout úspor nákladů, zejména pokud máte velký počet databází.

V systému **SQL Server na Azure VMs**můžete použít libovolný platformy-za předpokladu, že obrázek SQL Server (zahrnující licenci) nebo přenést licenci SQL Server. Všechny podporované verze serveru SQL Server (2008 R2, 2012, 2014, 2016) a edice (Vývojář Express, Web, standardní, organizace) jsou k dispozici. Kromě toho přenést svůj-vlastní-licence (BYOL) obrázků jsou k dispozici. Je-li pomocí Azure obrázky, provozní náklady závisí na velikosti OM a edice systému SQL Server zvolíte. Bez ohledu na velikost OM nebo verzi serveru SQL Server platíte za minutu licencování náklady SQL Server a Windows Server, spolu s náklady úložišti Azure disků OM. Možnost za minutu umožňuje SQL Server pro spolupráci prostřednictvím dlouhou, jak potřebujete, aniž byste nákupu sčítání SQL Server licence. Když aktivujete oulooku SQL Server Azure, vám bude účtovaná za systému Windows Server a náklady na úložiště. Další informace o správě licencí přenést e vlastníte najdete v článku [Mobilita licence až Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Výpočet nákladů celkové aplikace

Při spuštění pomocí platformy cloudu náklady spuštění aplikace zahrnuje vývoj a správu nákladů a náklady veřejné cloudu platformy služby.

Tady je výpočtu podrobné nákladů aplikace spuštěna v databázi SQL a SQL Server Azure VMs:

**Při použití databáze SQL Azure:**

*Celkové náklady na použití = náklady vysoce minimalizované správy software development náklady + náklady SQL databáze služby*

**Při použití systému SQL Server na Azure VMs:**

*Celkové náklady na použití = vysoce minimalizované software development náklady správy náklady + SQL Server a Windows Server licencování náklady + Azure úložiště nákladů*

Další informace o cenách najdete v následujících zdrojích:

- [Cena za databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/)
- [Virtuální počítač ceny](https://azure.microsoft.com/pricing/details/virtual-machines/) pro [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) a [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure ceny Kalkulačka](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Existuje malou funkce na serveru SQL Server, které nejsou použitelné nebo nejsou dostupné u databáze SQL. Další informace najdete v článku [omezení SQL databáze obecné a pokyny](sql-database-general-limitations.md) a [informace o SQL databáze Transact-SQL](sql-database-transact-sql-information.md) . Pokud přesunujete existujícího řešení SQL Server do cloudu, přečtěte si článek [Migrace databáze SQL serveru k databázi SQL Azure](sql-database-cloud-migrate.md). Při migraci existující aplikaci SQL Server místní k SQL databázi, zvažte aktualizace aplikace umožní využít výhod možností, které cloudových služeb nabídky. Například zvažte pomocí [Webové aplikace služby Azure](https://azure.microsoft.com/services/app-service/web/) nebo [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) hostovat vrstvě aplikačních zvětšíte výhod náklady.

### <a name="administration"></a>Správa

Pro mnoho firmy rozhodnutí o přechodu do cloudové služby je stejně jako mnohem o odstranění složitost správy ZobrazitFormulář.aspx náklady. **Databáze SQL**spravuje Microsoft základním hardwaru. Microsoft automaticky zreplikuje všechna data k poskytování dostupnost, nakonfiguruje aktualizuje software pro databázový, má na starosti Vyrovnávání zatížení a znamená průhledné selhání při selhání serveru. Můžete dál spravovat databáze, ale již nepotřebujete ke správě databázový stroj, serveru operačního systému nebo hardwaru.  Položky, které můžete dál spravovat příkladem databází a přihlášení, index a optimalizace dotazu a sestavy auditování a zabezpečení.

V systému **SQL Server na Azure VMs**máte úplnou kontrolu nad operačního systému a konfigurace instance serveru SQL Server. S OM je to na můžete se rozhodnout, kdy se má aktualizovat/upgradovat operační systém a software pro databázový a kdy instalovat další software například Anti-virus. Některé automatické funkce jsou k dispozici výrazně zjednodušit dostupnost oprav, zálohování a maximum. Kromě toho můžete určit velikost OM, počet disků a jejich konfigurace úložiště. Azure umožňuje podle potřeby změňte velikost virtuálního počítače. Informace najdete v tématu [virtuální počítač a velikosti cloudové služby Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Smlouva o úrovni služeb (SLA)

Pro mnoho IT oddělení schůzku dobu povinnosti z smlouva úrovni služeb (SLA) je nejvyšší prioritu. V tomto oddílu se podíváme na SLA platí pro každou databázi hostingu možnost.

Úrovně služby základní **Databáze SQL** , Standard a Premium poskytuje společnost Microsoft dostupnost SLA 99,99 %. Nejnovější informace najdete v tématu [Smlouvách úroveň](https://azure.microsoft.com/support/legal/sla/sql-database/). Nejnovější informace o – úrovně služeb SQL databáze a podporované firemní plány kontinuitu najdete v článku [– Úrovně služeb](sql-database-service-tiers.md).

**SQL Server spuštěna Azure VMs**poskytuje společnost Microsoft dostupnost SLA 99.95 %, který popisuje právě virtuálního počítače. Tento SLA netýká procesy (třeba SQL Server) spuštěné v OM a vyžaduje hostovat nejméně dvě instance OM sady dostupnosti. Nejnovější informace najdete v tématu [OM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Databáze vysoké dostupnosti (HA) v rámci VMs nastavte jednu z možností podporované dostupnost na serveru SQL Server, například [AlwaysOn dostupnost skupiny](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Použití možnosti podporované dostupnost nenabízí možnost Další SLA, ale můžete dosáhnout > dostupnosti databáze 99,99 %.

### <a name="market"></a>Čas na trh

**Databáze SQL** je správné řešení získáte cloudu navržený, když je považován za kritický produktivita a rychlé čas market. S programové obchodní podobné funkce je ideální pro tvůrci cloudu a vývojáři snižuje potřebné pro správu podkladových operačního systému a databáze. Například můžete [Rozhraní REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) a [Rutinách Powershellu](http://msdn.microsoft.com/library/azure/dn546726.aspx) pro automatizaci a správu pro správu operace tisíců databází. Funkce, jako jsou [Pružná fondů databáze](sql-database-elastic-pool.md) umožňují zaměřit na vrstvy aplikace a k předání vašeho řešení trhu rychleji.

**SQL Server spuštěna Azure VMs** je ideální Pokud existující nebo nové aplikace vyžadují velké databáze, vzájemně souvisejících databáze nebo přístup ke všem funkcím v SQL Server nebo Windows. Je taky dobré přizpůsobení, když chcete migrovat stávající místní aplikací a databází Azure jako-je. Vzhledem k tomu, že nepotřebujete ke změně prezentace, aplikace a data vrstvy, ušetříte čas a rozpočet na rearchitecting existujícího řešení. Místo toho se můžete zaměřit na migraci všech řešení Azure a při provádění některých optimalizaci výkonu, které může být nutné platformu Azure. Další informace najdete v tématu [Výkon doporučené postupy pro systém SQL Server na virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Souhrn

Tento článek prozkoumat databáze a SQL serveru SQL Server na virtuálních počítačích Azure (VMs) a popsány běžné motivators firmy, které by mohly ovlivnit vaše rozhodnutí. Následuje přehled návrhy trocha zamyšlení nad prezentacemi:

Vyberte **databázi Azure SQL** , pokud:

- Vytváření nové aplikace cloudového využít úspor náklady a zadejte optimalizace výkonu cloudových služeb. Tento přístup výhody plně spravovaných cloudové služby, pomůže malá počáteční čas na trhu a umožňují dlouhodobé optimalizace náklady.

- Chcete-li v aplikaci Microsoft provádět s databází aplikace společná operace správy a vyžadovat při tom silnější dostupnost rozsahu pro databáze.



**SQL Server na Azure VMs** vyberte, pokud:

- Máte existující místní aplikace, které chcete migrovat nebo rozšíření do cloudu, nebo pokud chcete vytvořit podnikových aplikací větší než 1 TB. Tento přístup poskytuje tu výhodu kompatibility SQL 100 %, například velké databáze kapacitu, úplnou kontrolu nad SQL serveru a Windows a zabezpečit tunneling do místního nasazení. Tento postup slouží k minimalizaci nákladů na vývoj a změny stávajících aplikací.

- Máte existující zdroje IT a můžete nakonec vlastní instalace oprav, zálohování a dostupnost databáze. Oznámení, že některé funkce automatického výrazně zjednodušuje tyto operace. 


## <a name="next-steps"></a>Další kroky
- Najdete v článku [databáze SQL kurz: vytvoření SQL databáze v minutách pomocí portálu Azure](sql-database-get-started.md) začít pracovat s databáze SQL.
- V tématu [databáze SQL ceny] (https://azure.microsoft.com/pricing/details/sql-database/).
- Najdete v článku [poskytnutí virtuálního serveru SQL Server počítače v Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) začít pracovat se serverem SQL Server na Azure VMs.
- V tématu [serveru SQL Server Azure počítače virtuální: cesta výuky](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
