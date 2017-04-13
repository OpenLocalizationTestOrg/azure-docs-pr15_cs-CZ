<properties
   pageTitle="Příručku: obnovení z místní selhání v Azure | Microsoft Azure"
   description="Článek na principy a navrhování pružné, vysoce dostupné, chybám aplikací, stejně jako plánování havárie obnovení zaměření na místní selhání v Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Azure odolnost proti chybám příručku: obnovení z místní selhání v Azure

Existují dva hlavní hrozeb dostupnosti aplikace:

* Chyba při zařízení, třeba jednotky a servery
* Vyčerpání kritické zdroje, jako je třeba výpočetním podmínky zatížení ve špičce

Azure poskytuje kombinací řízení zdrojů pružnost, Vyrovnávání zatížení a rozdělení povolit dostupnost za těchto podmínek. Některé z těchto funkcí pro všechny služby Azure provádí automaticky. V některých případech však vývojář aplikace musíte udělat některé další práce využívat.

##<a name="cloud-services"></a>Cloud Services

Azure Cloudovým službám tvoří kolekce jedné nebo více webu nebo pracovního rolí. Nejméně jedna instance role poběží současně. Konfigurace určuje instance. Role instance jsou sledovány a spravovat prostřednictvím komponenty s názvem řadiče struktury. Správce struktury rozpozná a odpoví na software a hardware selhání automaticky.

Všechny instance role běží ve vlastním virtuálního počítače (OM) a informuje uživatele o s jeho struktury řadiče prostřednictvím agenta Host. Agent hosta shromažďuje zdrojů a uzel metriky, včetně použití OM, stav, protokoly, používání zdrojů, výjimky a selhání podmínky. Správce struktury vyhledá agenta Host, která dokáže nahradit intervalech a restartuje OM, pokud agent hosta přestane odpovídat. V případě chyby hardwaru řadiče přidružené struktury přesune všechny instance problémového role nový uzel hardware a konfiguraci sítě chcete směrovat přenosy v síti tam.

Tyto funkce využívat, vývojáři zajistí, že všechny služby role Neuchovávejte stavu na instancí role. Místo toho všechna data trvalý měli k nim získat přístup z trvalé úložiště, jako je úložišti Azure nebo databáze SQL Azure. Díky žádné role pro zpracování žádostí o. Také znamená to, že role instance můžete přejít kdykoli bez vytvoření nekonzistencím v přechodná nebo trvalý stav služby.

Požadavek na uložení stavu externě k rolím má několik důsledky. To znamená, například, že všechny změny v související tabulce úložišti Azure by měl být změněn v jedné transakce entity skupiny, pokud je to možné. Vždy není samozřejmě umožňuje provádět všechny změny v jedné transakce. Musíte věnovat zvláštní pozornost zajistit, že role instance selhání nezpůsobují problémy při přerušení dlouho probíhajících operace, které zahrnují dva nebo více aktualizace trvalý stav služby. Pokud další roli pokusí opakovat tyto operace, by měl odhadnout a zpracovat případy, kdy práce částečně dokončený.

Zvažte například služba oddíly dat mezi více stores. Pokud roli pracovníka přejde během je přemístění shard, nemusí dokončení přemístění shard. Nebo přemístění obsažená je od jeho zahájení podle různých pracovních role potenciálně příčinou osamocené dat nebo poškození dat. Případným problémům dlouho probíhajících operace musí být jednu nebo obě z následujících akcí:

 * *Idempotent*: opakující bez vedlejší efekty. Aby idempotent dlouhodobá operace měli stejného účinku bez ohledu na to, kolik času spuštění, i když se přeruší během provádění.
 * *Postupně restartování*: moct pokračovat od nejnovějšího bodu selhání. Jako postupně restartování, dlouhodobá operace by měl být tvořeny posloupnost menší atomová operací. Také měl by zaznamenat průběh v trvalé úložiště, tak, aby každý další vyvolání vyzvedne, kde jeho předchůdce zastaveno.

Nakonec všechny operace dlouho probíhajících by měl vyvolat opakovaně, dokud se nezdaří. Například zřizovací operace může být umístěny ve frontě Azure a potom odebrány z fronty podle rolí pracovníka pouze v případě neproběhne úspěšně. Uvolnění paměti může být nutné vyčistit data, která přerušena operace vytvořit.

###<a name="elasticity"></a>Pružnost

Počáteční číslo instancí spuštěných pro každou roli, je určený v konfiguraci každou roli. Správci mají původně konfigurovat jednotlivé role pro práci s dvěma nebo víc instancí podle očekávaných načítání. Ale můžete snadno velikosti role instancí nebo dolů jako použití vzorků změnit. Můžete provést ruční na portálu Azure nebo proces automatizovat pomocí prostředí Windows PowerShell, rozhraní API správy služby nebo nástroje třetích stran. Další informace najdete v tématu [jak automatické měřítko aplikace](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Rozdělení

Správce Azure struktury používá dva typy oddílů:

* *Aktualizace domény* se používá k upgradu instancí služby role ve skupinách. Azure nasadí instancí služby do víc domén aktualizace. K aktualizaci na místě řadiče struktury přináší dolů všechny instance v doméně jedna aktualizace aktualizuje je a potom restartuje teprve pak přejděte na další doménu aktualizace. Tento přístup zabrání celý služba není k dispozici během procesu aktualizace.
* *Poruchy domény* definuje možná místa selhání hardwaru nebo k síti. Pro všechny roli, která obsahuje více než jedna instance struktury řadiče zaručuje, instance jsou rozvržena víc domén poruch zabráníte přerušení služby selhání izolace hardwaru. Poruchy domény se řídí všechny vystavení serveru a selhání clusteru.

[Azure smlouva úrovni služeb (SLA)](https://azure.microsoft.com/support/legal/sla/) zaručuje, že až dva nebo víc instancí role web nasazených na různých poruch a upgrade domény, budou mít externí připojení aspoň 99.95 procent času. Na rozdíl od aktualizace domény nejde žádným způsobem řídit počet poruch domén. Azure automaticky přidělí poruch domén a distribuuje role instance mezi nimi. AT nejnižších první dvě instance jednotlivých rolích jsou umístěná v různých poruch a upgrade domény zajistit, že všechny role s nejméně dvě instance bude vyhovovat SLA. To představuje v následujícím diagramu.

![Zjednodušené zobrazení izolace domény poruch](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Vyrovnávání zatížení

Všechny příchozí data k roli web prochází Vyrovnávání zatížení příslušnosti, který distribuuje požadavků klientů mezi instancemi role. Jednotlivé role instance nemají veřejnou IP adresy a ostatní vás přímo s možností zadání z Internetu. Role web jsou příslušnosti tak, aby všechny žádosti klienta mohou být směrovány na všechny instance role. Událost nastavit jako [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) je aktivována každých 15 sekund. Pomocí této funkce můžete označit zda roli chtít přijímat vysílání, nebo zda je zaneprázdněné a vzít mimo Vyrovnávání zatížení otočení.

##<a name="virtual-machines"></a>Virtuálních počítačích

Azure virtuálních počítačích se liší od platformy, jak službu (PaaS) výpočet role ve více směrech vzhledem k dostupnost. V některých případech musíte udělat další práce zajistit dostupnost.

###<a name="disk-durability"></a>Životnost disku

Na rozdíl od PaaS role instance dat uložených v jednotkách virtuálního počítače při trvalý i přemístění virtuální počítač. Azure virtuálních počítačích pomocí disků OM, které jsou jako objekty BLOB v úložišti Azure. Z důvodu vlastnosti dostupnost úložišti Azure data uložená v jednotkách virtuálního počítače neexistuje také vysoce.

Všimněte si, že jednotka D (v systému Windows VMs) výjimkou tohoto pravidla. Jednotka D je skutečně fyzické úložiště na regálů serveru, který je hostitelem OM a její data budou ztraceny OM je z koše. Jednotka D je určená pro dočasné uložení. V Linux Azure "obvykle" (ale ne vždy) zpřístupňuje místní disk dočasné jako /dev/sdb blok zařízení. Často připojen Linux agentem Azure s /mnt/resource nebo /mnt připojovací body (konfigurovatelné /etc/waagent.conf).

###<a name="partitioning"></a>Rozdělení

Azure nativně rozumí úrovní v aplikaci PaaS (role webového a pracovní roli) a tedy můžete správně distribuovat přes poruch a aktualizace domény. Naopak úrovní v infrastrukturu jako aplikace služby (IaaS) je nutné ručně definovat sadami dostupnosti. Dostupnost sady jsou potřeba pro SLA v části IaaS.

![Dostupnost nastaví pro Azure virtuálních počítačích](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

Na předchozím obrázku přiřazené osy Internetové informační služby (IIS), (které funguje jako vrstva webové aplikace) a úroveň SQL (která funguje jako osy dat) k dispozici různé sady. Zajistíte, že všechny výskyty jednotlivé vrstvy tím hardwaru redundance distribuce virtuálních počítačích mezi poruch doménami a že nejsou celý úrovní přijata během aktualizace.

###<a name="load-balancing"></a>Vyrovnávání zatížení

Pokud VMs by měla být přenosy rozvržena je, třeba seskupit VMs aplikace a načíst vyrovnané přes konkrétní TCP a UDP koncového bodu. Další informace najdete v tématu [Vyrovnávání zatížení virtuálních počítačích](../virtual-machines/virtual-machines-linux-load-balance.md). Pokud VMs příjem vstupního z jiného zdroje (například fronty mechanismus), Vyrovnávání zatížení není potřeba. Vyrovnávání zatížení používá kontrolu základní zdraví a zjistit, zda mají být přenosy odesílány na uzel. Také je možné vytvořit vlastní sond implementovat specifické pro aplikaci stav metriky, které zjistit, zda OM mají dostávat přenosy.

##<a name="storage"></a>Úložiště

Azure úložiště je služba trvalá data podle směrného plánu pro Azure. Poskytuje objektů blob, tabulky, fronty a místa na disku OM. Používá kombinace replikace a řízení zdrojů k poskytování dostupnost v rámci jedné datacentra. Dostupnost úložišti Azure SLA zaručuje, že aspoň 99,9 procent času:

* Správně formátované požadavky na Přidat, aktualizovat, číst nebo odstranění dat bude úspěšně a správně zpracovat.
* Účty úložiště bude mít připojení k Internetu brány.

###<a name="replication"></a>Replikace

Azure úložiště usnadňuje dat životnosti udržovat více kopií všechna data na jiných jednotkách přes podsystémy nezávislým fyzické úložiště v oblasti. Synchronní replikace dat a všechny kopie jsou potvrzené před potvrzeno zápisu. Azure úložiště odpovídá důrazně, což znamená, že je tak, aby odrážely posledních zápisy zaručena čtení. Kromě toho kopií dat jsou průběžně zkontrolovány a rozpoznávání a oprava bit hniloby, často přehlíženým hrozbou integrity uložená data.

Služby využívat replikace jenom pomocí Azure úložiště. Vývojář služba nevyžaduje další práce k obnovení ze selhání místní.

###<a name="resource-management"></a>Správa zdrojů

Úložiště účtů vytvořených po květen 2014 můžete zvětšit až 500 TB (předchozí maximální hodnota je 200 TB). Pokud je potřeba další místa, musí používat více účtů úložiště navržený aplikací.

###<a name="virtual-machine-disks"></a>Virtuální počítač disků

Virtuální počítač disku je uložená jako objektů blob stránky v úložišti Azure pojmenování ho stejné vlastnosti životnosti a škálovatelnost jako úložiště objektů Blob. Tento návrh zajišťuje data na disku počítače virtuální trvalé, i v případě, že dochází k chybě serveru OM a OM restartování na jiném serveru.

##<a name="database"></a>Databáze

###<a name="sql-database"></a>Databáze SQL

Databáze SQL Azure poskytuje databázi jako služba. Umožňuje aplikacím rychle vytvořit, vložte data do a relační databáze dotazu. Poskytuje mnoho známé SQL serveru a funkce, při abstracting zátěž hardwarová konfigurace, opravy a odolnost proti chybám.

>[AZURE.NOTE] Databáze SQL Azure neposkytuje přímé funkce dostupná se serverem SQL Server. Má určené ke splnění jinou sadu požadavky – takový, který obsahuje jedinečné vyhovující cloudové aplikace (pružná měřítko, databáze jako služba snížit náklady na údržbu a tak dál). Další informace najdete v tématu [Zvolte obláčkem možnost SQL Server: databáze SQL Azure (PaaS) nebo SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replikace

Databáze SQL Azure nabízí předdefinované odolnost proti chybám selhání úrovni uzlů. Všechny zápisy do databáze se automaticky replikovat do dvou nebo více uzlů pozadí prostřednictvím technika potvrdit kvora. (Primární a sekundární alespoň jeden musí potvrdit, že aktivity je došlo k zápisu transakční protokol před transakce se považuje za úspěšné a vrátí.) V případě selhání uzlu databázi automaticky přejde do jedné ze sekundárního repliky. To způsobí, že přerušení přechodná připojení pro klientské aplikace. Z tohoto důvodu musí všechny klienty databáze SQL Azure implementovat některé formy zpracování přechodná připojení. Další informace najdete v tématu [znovu doporučení týkající se konkrétních služeb](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Správa zdrojů

Každou databázi po vytvoření nastaven omezení horní velikosti. Je momentálně neexistuje maximální velikosti 1 TB (velikost lišit omezení na základě vaší vrstvy služeb, najdete v tématu [– úrovně služeb a výkonu úrovně databáze SQL Azure](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Pokud databáze narazí limit velikosti horní, odmítne dalších příkazů Vložit nebo aktualizovat. (Nemusí dotazy a odstraňování dat je možné, že.)

V databázi databáze SQL Azure pomocí sítě přidávání a používání zdrojů. Však místo řadiči struktury používala kruhová topologie zjišťování chyb. Všechny otevřené v clusteru má dvě sousední a odpovídá pro zjišťování, když půjde. Když otevřené přejde, sousední aktivace agenta konfigurace znovu vytvořit na jiném počítači. Omezení Engine by vám měly zajistit, aby logické serveru není použít příliš mnoho prostředků na počítač nebo překročení omezení fyzické počítače.

###<a name="elasticity"></a>Pružnost

Pokud aplikace vyžaduje více než limit 1 TB databáze, musíte implementovat škálování přístup. Změníte měřítko s databáze SQL Azure ručně rozdělení, nazývaný také sharding, dat ve více databázím SQL. Tento přístup škálování poskytuje možnost dosažení téměř lineární náklady LOGLINTREND s měřítkem. Pružná nárůst či kapacita na vyžádání můžete roste, pomocí dílčí náklady v případě potřeby vzhledem k tomu, že databáze je faktura podle průměru skutečnou velikostí používá denně, nikoli na základě maximální možné velikosti.

##<a name="sql-server-on-virtual-machines"></a>SQL Server na virtuálních počítačích

Při instalaci systému SQL Server (verze 2014 nebo novější) na virtuálních počítačích Azure, můžete využít funkce tradiční dostupnost SQL serveru. Tyto funkce patří skupiny dostupnosti AlwaysOn a odrážející strukturu databáze. Všimněte si, že Azure VMs, ukládání a sítě mít různou charakteristikou provozní než místního, nevirtuálním IT infrastrukturu. Úspěšné provádění vysoké dostupnost/havárie obnovení řešení (HA/DR) SQL serveru v Azure vyžaduje pochopit tyto rozdíly a řešení tak, aby pokryly je Navrhněte.

###<a name="high-availability-nodes-in-an-availability-set"></a>Vysoké dostupnosti uzlů v sadě dostupnosti

Po implementaci řešení vysoké dostupnosti v Azure můžete dostupnost nastavení v Azure umístění uzlů vysoké dostupnosti do samostatných poruch domén a upgrade domény. Aby bylo jasné, je sadu dostupnost Azure koncept. Je doporučený postup, který byste měli podle pokynů, abyste měli jistotu, že databáze skutečně vysoká, jsou k dispozici ať používáte skupiny dostupnosti AlwaysOn odrážející strukturu databáze nebo něco jiného. Pokud nemáte vyzkoušejte tento doporučený postup, nejspíš vás bude za false předpokladu, že vysoce neexistuje systému. Ale ve skutečnosti uzly můžete všechny současné selhání protože probíhají umístit na tu samou doménu poruch v oblasti Azure.

Toto doporučení není podle potřeby s dodací protokolu. Jako funkce obnovení havárie Ujistěte se, zda serverech běží v samostatných oblastech Azure. Definicí jsou tyto oblasti samostatné poruch domény.

Pro Azure cloudové služby VMs nasazených pomocí portálu klasické být stejnou sadu dostupnost třeba nasadíte je ve stejném cloudové služby. Nasazení prostřednictvím Azure správce (aktuální portál) VMs nemají toto omezení. Pro klasické portál nasazený VMs v Azure cloudové služby, pouze uzly ve stejném cloudové služby se může účastnit stejnou sadu dostupnosti. Kromě toho VMs služby cloudu by měl být ve stejné síti virtuální k zajištění jejich udržovat jejich IP adresy i po retušování služby. To je zabráněno přerušení aktualizace DNS.

###<a name="azure-only-high-availability-solutions"></a>Jen Azure: řešení vysoké dostupnosti

Můžete máte vysoké dostupnosti řešení pro databáze SQL serveru v Azure pomocí skupiny dostupnosti AlwaysOn nebo odrážející strukturu databáze.

Následující diagram ukazuje architektura skupiny dostupnost AlwaysOn spuštěné v Azure virtuálních počítačích. Tento diagram pořízení z podrobný článek Toto téma [dostupnost a obnovení pro systém SQL Server na virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Skupiny dostupnosti AlwaysOn v Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Můžete taky automaticky zřizujete skupiny dostupnosti AlwaysOn nasazení začátku do konce na Azure VMs pomocí šablony AlwaysOn v portálu Azure. Další informace najdete v tématu [SQL Server AlwaysOn nabízející v galerii portálu Microsoft Azure](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Následující diagram ukazuje použití databáze odrážející strukturu na virtuálních počítačích Azure. Pořízení taky z hloubkovou tématu [dostupnost a obnovení pro systém SQL Server na virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Databáze odrážející strukturu v Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Obě architektury vyžadují řadiče domény. S odrážející strukturu databáze, je možné použít certifikáty serveru eliminuje potřebu řadiče domény.

##<a name="other-azure-platform-services"></a>Další služby Azure platformy

Aplikace založené na Azure využít platformu možnosti obnovit z místní selhání. V některých případech můžete zlepšit dostupnost pro konkrétní nefunguje nějaké konkrétní akce.

###<a name="service-bus"></a>Služba Bus

Zmírnění proti dočasné výpadku Bus služby Azure, zvažte vytvoření trvalé fronty klienta. To mechanismus alternativní, místní úložiště dočasně používá k ukládání zpráv, které nelze přidat do fronty Bus služby. Postup v případě dočasně uložené zprávy po obnovení službu můžete se rozhodnout, aplikace. Další informace najdete v tématu [osvědčené postupy pro zvýšení výkonu pomocí služby Bus brokered zasílání zpráv](../service-bus-messaging/service-bus-performance-improvements.md) a [Služby Bus (havárie obnovení)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>HDInsight

Ve výchozím nastavení v úložišti objektů Blob Azure uložení dat, který máte přidružený k Azure HDInsight. Azure úložiště určuje vysoké dostupnosti a životnosti vlastnosti úložiště objektů Blob. Zpracování více uzly, který máte přidružený k Hadoop MapReduce úlohy dojde na přechodná Hadoop Distributed soubor systému (HDFS), který máte k dispozici při HDInsight potřebuje. Výsledky úlohy MapReduce také ukládají ve výchozím nastavení v úložišti objektů Blob Azure tak, aby zpracovaných dat je trvalé a vysoce dostupné po poskytování zrušeno Hadoop obrázku. Další informace najdete v tématu [HDInsight (havárie obnovení)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Seznamy úkolů pro místní selhání

###<a name="cloud-services"></a>Cloud Services

  1. Přečtěte si část Cloudovým službám tohoto dokumentu.
  2. Nakonfigurujte alespoň dvě instance pro každou roli.
  3. Zachování stavu v trvalé úložiště, ne na role instance.
  4. Událost StatusCheck správně zpracujte.
  5. Zalomení související změny v transakce, pokud je to možné.
  6. Ověřte, zda úkoly role pracovníka idempotent a restartování.
  7. Dál vyvolat operace, dokud se nezdaří.
  8. Zvažte strategie neobsahovaly text.

###<a name="virtual-machines"></a>Virtuálních počítačích

  1. Přečtěte si část virtuálních počítačích tohoto dokumentu.
  2. Nepoužívejte jednotka D pro trvalé úložiště.
  3. Skupina stroje ve vrstvě služby do sady dostupnosti.
  4. Konfigurace služby Vyrovnávání zatížení a volitelné sond.

###<a name="storage"></a>Úložiště

  1. Přečtěte si část úložiště v tomto dokumentu.
  2. Používejte více účtů úložiště dat nebo šířku pásma překročí kvóty.

###<a name="sql-database"></a>Databáze SQL

  1. Přečtěte si část SQL databáze v tomto dokumentu.
  2. Implementace obsloužení chyb přechodná zásadu opakovat.
  3. Pomocí rozdělení/sharding jako strategii škálování.

###<a name="sql-server-on-virtual-machines"></a>SQL Server na virtuálních počítačích

  1. Prohlédněte si SQL Server na virtuálních počítačích část tohoto dokumentu.
  2. Postupujte podle předchozích doporučení pro virtuálních počítačích.
  3. Použití funkce dostupnost SQL serveru, jako jsou AlwaysOn.

###<a name="service-bus"></a>Služba Bus

  1. Přečtěte si část služby Bus tohoto dokumentu.
  2. Zvažte vytvoření trvalé fronty klientských jako zálohu.

###<a name="hdinsight"></a>HDInsight

  1. Přečtěte si část HDInsight tohoto dokumentu.
  2. Žádné další dostupnost kroky jsou potřebné pro místní selhání.

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady zaměření na [příručku Azure odolnost proti chybám](./resiliency-technical-guidance.md). Další článek v této řadě je [obnovení z přerušení služby celé oblasti](./resiliency-technical-guidance-recovery-loss-azure-region.md).
