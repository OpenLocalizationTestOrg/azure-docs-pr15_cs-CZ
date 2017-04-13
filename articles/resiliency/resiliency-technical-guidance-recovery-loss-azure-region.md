<properties
   pageTitle="Odolnost proti chybám obnovení před ztrátou Azure oblast příručku | Microsoft Azure"
   description="Článek principy a navrhování pružné s vysokou dostupností odolnost proti chybám aplikací i plánování havárie obnovení"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure odolnost proti chybám příručku: obnovení z přerušení služby oblast

Azure je rozdělen fyzicky a logické jednotky s názvem oblastí. Oblast obsahuje jednu nebo více datacentrech v blízkosti. Při psaní tohoto textu Azure má 24 oblastí ve světě.

Za méně častých okolností je možné, že zařízení v celé oblasti, může být nedostupné, například z důvodu selhání sítě. Nebo zařízení můžete ztratit úplně, například z důvodu přírodní katastrofě. Tato část popisuje funkce Azure pro vytváření aplikací, které jsou rozvržena oblastí. Toto rozdělení pomáhá minimalizovat možnost, že chyba instalace v jednom regionu může mít vliv na jiné oblasti.

##<a name="cloud-services"></a>Cloud services

###<a name="resource-management"></a>Správa zdrojů

Vytvořením samostatné Cloudová služba v jednotlivých oblastech cílové a publikování balíček pro nasazení na každý cloudové služby můžete distribuovat výpočetním instance různých oblastí. Však, že distribuce přenosy přes cloudovým službám v různých oblastech musí implementovaná tak, že vývojář aplikace nebo pomocí služby správy přenosy.

Určení počtu instance náhradní role pro nasazení předem havárie obnovení je důležitým aspektem plánování kapacity. S úplné sekundární nasazení zajistí kapacitu už dostupné v případě potřeby; však to efektivně zdvojnásobuje náklady. Běžné vzorek je malý, sekundární nasazení, dostatečně velký ke spouštění kritické služeb. Toto malé sekundární nasazení je vhodné, obě rezervujte kapacita a testování konfigurace sekundárního prostředí.

>[AZURE.NOTE]Kvóta předplatné není záruky kapacity. Kvóty je jednoduše úvěr. Chcete-li zajistit kapacitu, je třeba definovat požadovaný počet role v modelu služby a musí být nasazené role.

###<a name="load-balancing"></a>Vyrovnávání zatížení

Vyrovnávání zatížení přenosů přes oblastí vyžaduje řešení pro správu přenosy. Azure poskytuje [Azure přenosy správce](https://azure.microsoft.com/services/traffic-manager/). Můžete taky využít služeb třetí strany, které jsou zdrojem podobné přenosy možnosti správy.

###<a name="strategies"></a>Strategie

Mnoho alternativní strategií jsou k dispozici pro implementaci distribuované výpočetním různých oblastí. Tyto musí být ušitý na míru funkcím specifickým obchodním požadavky a okolností aplikace. Na vysoké úrovni můžete přístupy rozdělit na následující kategorie:

  * __Přeinstalujte na havárie__: V tomto přístupu aplikace je znovu nasadit přímo v době katastrofě. To je vhodné pro nekritické aplikace, které nevyžadují zaručené obnovení doleva.

  * __Teplé náhradní (aktivní nebo pasivní)__: sekundární hostovanou službu se vytvoří v alternativní oblasti a role jsou používaný k zajištění minimální kapacity; však role, se nezobrazí přenosy výroby. Tento přístup je užitečný pro aplikace, které nebyly cílem distribuování přenosy v oblasti.

  * __Aktivní náhradní (aktivní nebo aktivní)__: aplikace je určen pro příjem výrobní zatížení ve více oblastech. Cloudových služeb v jednotlivých oblastech může být nakonfigurované pro vyšší výkon než požadováno pro účely obnovení havárie. Můžete taky může měřítko cloudových služeb out podle potřeby v době havárie a překlopení. Tento postup vyžaduje, který nabízí podstatně vyšší investice v návrhu aplikace, ale má spoustu značných výhod. Jedná se o obnovení zhoršeným a zaručené čas nepřetržitý testování všech obnovení umístění a efektivní využití kapacity.

Úplné informace o distribuované návrh je mimo rozsah tohoto dokumentu. Další informace najdete v tématu [obnovení havárie a dostupnost pro Azure aplikace](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtuálních počítačích

Obnovení infrastruktury služby (IaaS) virtuálních počítačích (VMs) je podobný platformy službu (PaaS) výpočet obnovení v mnoha ohledem. Jsou důležité rozdíly, ale tím, že IaaS OM se skládá z OM a OM disku.

  * __Pomocí nástroje Zálohování Azure k vytváření záloh křížového oblasti, které jsou aplikace konzistentní__.
  [Zálohování Azure](https://azure.microsoft.com/services/backup/) umožňuje zákazníkům vytváření záloh konzistentní aplikace na více disků OM a podporu replikace zálohy napříč oblastí. Lze provést pomocí příkazu geo replikace záložní trezoru při vytváření. Všimněte si, že replikace zálohování trezoru musí být nakonfigurované při vytváření. Nejde nastavit později. Pokud dojde ke ztrátě oblast, Microsoft zpřístupníte zálohy zákazníkům. Zákazníci budou moct obnovit na některý z jejich body nakonfigurované obnovení.

  * __Oddělení disku data z disku operační systém__. Důležitá poznámka pro IaaS VMs je disk operačního systému nemůžete změnit bez opětovné vytvoření OM. Toto není problém, pokud strategie obnovení přeinstalujte po havárie. Pokud používáte teplé náhradní přístup k rezervaci kapacity však může být problém. K provedení správně, musíte mít disku správný operační systém používaný primárních a sekundárních umístění a musí být aplikace data uložena do samostatné složky. Pokud je to možné použijte standardní operační systém konfigurace poskytovanou na obou míst. Po selhání musí pak připojíte jednotku dat svůj stávající VMs IaaS v sekundární řadiče domény. Přetažením AzCopy snímky discích dat na vzdálený server.

  * __Mějte na paměti potenciálních problémů konzistence po geo selhání více OM discích__. OM disků implementovaná jako objekty BLOB Azure úložiště a mít stejné charakteristikami geo replikace. Pokud není použit [Zálohování Azure](https://azure.microsoft.com/services/backup/) , existuje žádné záruky konzistence disků, protože geo replikace asynchronní a zreplikuje nezávisle na sobě. Jednotlivé OM disků jsou zaručena konzistentní chybu stavu po geo selhání, ale není konzistentní na discích. V některých případech (například v případě disku prokládání) to může způsobit problémy.

##<a name="storage"></a>Úložiště

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Obnovení pomocí Geo nadbytečné úložiště objektů blob, tabulky, fronty a místa na disku OM

V Azure objektů BLOB, tabulek, dotazů a OM disků jsou všechny geo replikovat ve výchozím nastavení. To je uvedená jako Geo nadbytečné úložiště (GRS). GRS zreplikuje datový úložiště na párových datacentra stovky mil od sebe v rámci konkrétní zeměpisnou oblast. GRS umožňuje další životnost v případě, že se selhání hlavní datacentra. Ovládací prvky Microsoft selháním a převzetí se omezí na hlavní havárie, ve kterých původního primární umístění se považuje za obnovit v rozumné časový úsek. V některých případech to může být několik dnů. Obvykle replikace dat objevit během pár minut, i když interval synchronizace není zatím vztahuje konsolidovaná smlouvy o úrovni služeb.

V případě geo selhání, budou bez změny způsob přístupu k účtu (klíč adresy URL a účtu se nezmění). Úložiště účet, ale bude v jiné oblasti po překlopení. To, můžou mít vliv aplikace, které vyžadují místní spřažen účtem úložiště. I pro services a aplikace, které není nutné zadávat účet úložiště ve stejném datacentru latenci mezi datacentra a šířky pásma náklady mohou být přesvědčivých důvodů, proč dočasně přesunout přenosy k oblasti překlopení. Faktor může do celkového strategii obnovení havárie.

Kromě automatické selhání poskytovanou GRS Azure zavádí služba, která umožňuje přístup pro čtení ke kopírování dat v umístění sekundárním úložiště. Je místo toho možnost Geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS).

Další informace o ukládání GRS a GRS Vzdálená pomoc v tématu [úložišti Azure replikace](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Replikace GEO oblast mapování:

Je důležité vědět, kde jsou vaše data geo replikovanou, pokud chcete vědět, kde nasazení ostatních instancí dat, která vyžadují místní spřažen úložišti. V následující tabulce jsou uvedeny párování primárních a sekundárních umístění:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Replikace GEO ceny:

Replikace GEO je součástí aktuální ceny pro Azure úložiště. Je místo toho možnost Geo nadbytečné úložiště (GRS). Pokud nechcete, aby vaše data replikovat geo můžete zakázat geo replikace pro váš účet. Tento postup se nazývá místně nadbytečné úložiště a účtovaná za cenu diskontního ve srovnání s GRS.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Zjištění, pokud došlo k geo překlopení

Dojde k selhání geo, bude účtován [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/). Aplikace můžete používat automatické prostředky zjištění toho ale sledováním geo oblast pro svůj účet úložiště. Lze použít ke spouštění jiných obnovení operací, jako je aktivace pro využití prostředků v oblasti geo kde svoje úložiště přesune do. Provedením dotazu pro tuto ze služby správy rozhraní API, pomocí [Získat vlastnosti účtu úložiště](https://msdn.microsoft.com/library/ee460802.aspx). Odpovídající vlastnosti jsou:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>OM disků a geo překlopení

Jak je popsáno v části na discích OM, jsou bez záruky dat konzistence disků OM po přepojení. Zajistit správnost zálohy záložní výrobek například správce ochranu dat má být použit k zálohování a obnovení dat aplikace.

##<a name="database"></a>Databáze

###<a name="sql-database"></a>Databáze SQL

Databáze SQL Azure poskytuje dva typy zotavení: obnovení Geo a aktivní Geo replikace.

####<a name="geo-restore"></a>Obnovení GEO

[Obnovení GEO](../sql-database/sql-database-recovery-using-backups.md#geo-restore) je také k dispozici u Basic, Standard a Premium databází. Výchozí možnost poskytuje při databázi není dostupný kvůli incident v oblasti, kde je hostovaný databáze. Podobně jako u obnovit v okamžiku, Geo obnovení vychází zálohy databáze v geo nadbytečné Azure úložiště. Obnoví ze záložní kopie replikovat geo a tedy pružné výpadků úložiště v oblasti primární. Další podrobnosti najdete v tématu [obnovení databáze SQL Azure nebo převzetí na sekundární](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktivní Geo replikace

[Aktivní Geo replikace](../sql-database/sql-database-geo-replication-overview.md) je dostupný u všech úrovní databáze. Je určený pro aplikace, které mají víc agresivní obnovení požadavky než můžou nabízet Geo obnovit. Použití aktivní Geo replikace, můžete vytvořit až se čtyřmi čitelné druhotné na serverech v různých oblastech. Zahájení konverzace převezme jednotlivých druhotné. Kromě toho aktivní Geo replikace lze podporují scénáře upgradu nebo přemístění aplikací, stejně jako načíst Vyrovnávání zatížení jen pro čtení. Další informace najdete v tématu [Konfigurace Geo replikace](../sql-database/sql-database-geo-replication-portal.md) a k [předání sekundární databáze](../sql-database/sql-database-geo-replication-failover-portal.md). Podívejte se do [návrhové aplikace pro obnovení havárie cloudu pomocí aktivní Geo replikace databáze SQL](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [postupné inovace Správa aplikací cloudu pomocí SQL databáze aktivní Geo replikace](../sql-database/sql-database-manage-application-rolling-upgrade.md) podrobné informace o tom, jak návrh a implementace aplikace a aplikace upgrade bez výpadek služeb.

###<a name="sql-server-on-virtual-machines"></a>SQL Server na virtuálních počítačích

Různé možnosti jsou k dispozici pro využití a dostupnost pro SQL Server 2012 (a novější) spuštění v Azure virtuálních počítačích. Další informace najdete v tématu [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Další služby Azure platformy

Při pokusu o spuštění Cloudová služba ve více oblastech Azure, je nutné zvážit důsledky u každé závislosti mezi úkoly. V následujících částech pokyny specifických pro službu předpokládá, že je nutné použít stejné služby Azure v alternativní Azure datacentra. Tento postup představuje konfigurace a replikace dat úkoly.

>[AZURE.NOTE]V některých případech tyto kroky vám může pomoci zmírnit výpadku specifických pro službu spíše než celý datacentra událost. Z pohledu aplikace může být výpadku specifických pro službu právě jako omezení a vyžadují dočasně migrace službu na alternativní Azure oblast.

###<a name="service-bus"></a>Služba Bus

Azure Bus služby používá jedinečný obor zahrnující není Azure oblastí. Požadavek na první je vlastně instalaci názvů bus potřebné služeb v oblasti alternativní. Můžou ale nastat také aspektech na životnost ve frontě zpráv. Existuje několik strategie pro replikovat zprávy přes Azure oblastí. Podrobnosti o těchto strategií replikace a další strategie obnovení havárie najdete v tématu [Doporučené postupy pro izolační aplikace proti služby Bus výpadků a havárie](../service-bus-messaging/service-bus-outages-disasters.md). Další úvahy dostupnosti najdete v článku [Služby Bus (dostupnosti)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>Aplikace služby

Migrace aplikaci Azure aplikaci služby, jako jsou webové aplikace nebo mobilní aplikace na vedlejší Azure oblast, musí mít záložní kopii webu k dispozici pro publikování. Pokud výpadku nezahrnuje celý Azure datacentru, je možné použít FTP ke stažení poslední záložní kopii obsah webu. Vytvořte nové aplikace v oblasti alternativní pokud dříve dokončení rezervujte kapacity. Publikování na web na novou oblast a proveďte nezbytné kroky konfigurace změny. Tyto změny může obsahovat řetězců připojení k databázi nebo jiná nastavení příslušnou oblast. V případě potřeby přidat certifikát SSL na web a nastavit záznam DNS CNAME tak, aby vlastní název domény odkazuje na adresa URL redeployed Azure Web App.

###<a name="hdinsight"></a>HDInsight

Data spojená se HDInsight uložený ve výchozím nastavení v úložišti objektů Blob Azure. HDInsight vyžaduje, aby Hadoop clusteru zpracování MapReduce úlohy musí být spoluvytváření umístěné ve stejné oblasti úložiště účtu, který obsahuje data probíhá analýza. Za předpokladu, že používáte funkci geo replikace dostupné k základnímu úložišti Azure, máte přístup k datům v oblasti sekundární kde Pokud z nějakého důvodu oblasti primární už není dostupná replikovat data. Vytvoření nového clusteru Hadoop v oblasti, kam má replikovat a pokračovat ve zpracování ho data. Další úvahy dostupnosti najdete v článku [HDInsight (dostupnosti)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>Vytváření sestav SQL

V současné době obnovení před ztrátou Azure oblast vyžaduje více SQL Reporting instancí v různých oblastech Azure. Tyto SQL Reporting instance měli přístup k stejných dat a tato data by měla být vlastní obnovení plánování v případě selhání. Můžete taky spravovat externí záložní kopie souboru RDL pro každou sestavu.

###<a name="media-services"></a>Mediální služby

Azure Media Services má jiný obnovení přístupu k kódování a přenos. Přenos bývá více kritické během výpadku místní. Příprava na to, byste si měli účet Media Services ve dvou různých Azure oblastí. Zakódovaný obsahu musí být umístěny v obou oblastí. Během se nepovede můžete je přesměrovávat streamování přenosy na alternativní oblast. Kódování lze provádět jakékoli Azure oblasti. Pokud kódování citlivé na čas, například během zpracování živou události, musí být připravené posílat úkoly k alternativní datacentru během k chybám.

###<a name="virtual-network"></a>Virtuální sítě

Konfigurace soubory obsahují nejrychlejším způsobem, jak vytvořit virtuální síť v alternativní Azure oblasti. Po konfiguraci virtuální sítě v primární Azure oblasti [exportovat virtuální síťová nastavení](../virtual-network/virtual-networks-create-vnet-classic-portal.md) pro aktuální síť do souboru konfigurace sítě. V případě výpadku v oblasti primární, [obnovit virtuální sítě](../virtual-network/virtual-networks-create-vnet-classic-portal.md) z uložené konfiguračního souboru. Nakonfigurujte jiné cloudové služby, virtuálních počítačích nebo více místní nastavení pro práci s novou virtuální síť.

##<a name="checklists-for-disaster-recovery"></a>Seznamy úkolů pro havárie obnovení

##<a name="cloud-services-checklist"></a>Kontrolní seznam služby cloudu

  1. Přečtěte si část Cloudovým službám tohoto dokumentu.
  2. Vytvoření strategii obnovení havárie více oblastí.
  3. Princip střídání v rezervaci kapacity alternativní oblastí.
  4. Použití přenosy směrování nástroje, například Azure přenosy správce.

##<a name="virtual-machines-checklist"></a>Kontrolní seznam virtuálních počítačích

  1. Přečtěte si část virtuálních počítačích tohoto dokumentu.
  2. [Zálohování Azure](https://azure.microsoft.com/services/backup/) slouží k vytvoření konzistentního zálohování aplikace různých oblastí.

##<a name="storage-checklist"></a>Kontrolní seznam úložiště

  1. Přečtěte si část úložiště v tomto dokumentu.
  2. Nezakazujte geo replikace úložiště zdrojů.
  3. Princip alternativní oblast geo replikace v případě překlopení.
  4. Vytvoření vlastní záložní strategie pro uživatelem řízené převzetí strategie.

##<a name="sql-database-checklist"></a>Kontrolní seznam databáze SQL

  1. Přečtěte si část SQL databáze v tomto dokumentu.
  2. Podle potřeby můžete použijte [Geo obnovení](../sql-database/sql-database-recovery-using-backups.md#geo-restore) nebo [Geo replikace](../sql-database/sql-database-geo-replication-overview.md) .

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server na virtuálních počítačích kontrolního seznamu

  1. Prohlédněte si SQL Server na virtuálních počítačích část tohoto dokumentu.
  2. Použití více oblastí AlwaysOn dostupnost skupiny nebo odrážející strukturu databáze.
  3. Můžete také použít zálohování a obnovení do úložiště objektů blob.

##<a name="service-bus-checklist"></a>Kontrolní seznam Bus služby

  1. Přečtěte si část služby Bus tohoto dokumentu.
  2. Konfigurace služby Bus názvů na alternativní oblast.
  3. Zvažte vlastní replikace strategie pro zprávy různých oblastí.

##<a name="app-service-checklist"></a>Kontrolní seznam aplikace služby

  1. Přečtěte si část aplikaci služby v tomto dokumentu.
  2. Udržujte zálohy webu mimo primární oblast.
  3. Při částečné výpadku pokus o načtení aktuální web s FTP.
  4. Plánování nasadit na web nové nebo existující web v alternativní oblasti.
  5. Plánování změny v konfiguraci aplikace a záznamy DNS CNAME.

##<a name="hdinsight-checklist"></a>Kontrolní seznam HDInsight

  1. Přečtěte si část HDInsight tohoto dokumentu.
  2. Vytvoření nového clusteru Hadoop v oblasti s replikovanou daty.

##<a name="sql-reporting-checklist"></a>Vytváření sestav SQL kontrolního seznamu

  1. Přečtěte si část SQL Reporting tohoto dokumentu.
  2. Udržujte alternativní SQL Reporting instance v jiné oblasti.
  3. Udržujte samostatná plánu replikovat do cíle do této oblasti.

##<a name="media-services-checklist"></a>Kontrolní seznam Media Services

  1. Přečtěte si část Media Services v tomto dokumentu.
  2. Vytvoření účtu Media Services v alternativní oblasti.
  3. Kódování stejný obsah v obou oblastí pro podporu streamování překlopení.
  4. Odešlete kódování úlohy alternativní oblasti v případě přerušení služby.

##<a name="virtual-network-checklist"></a>Virtuální kontrolního seznamu sítí

  1. Přečtěte si část virtuální sítě tohoto dokumentu.
  2. Použití exportovány virtuální síťová nastavení pro ji znovu vytvořit v jiné oblasti.

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady zaměření na [příručku Azure odolnost proti chybám](./resiliency-technical-guidance.md). Další článek v této řadě se zaměřuje na [obnovení z místního datacentra na Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
