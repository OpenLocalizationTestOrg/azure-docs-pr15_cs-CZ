<properties
   pageTitle="Odolnost proti chybám příručku pro obnovení poškozením dat nebo nechtěným odstraněním | Microsoft Azure"
   description="Článek na vědět, jak obnovit před poškozením dat dat nebo nechtěného odstranění dat do a navrhování pružné s vysokou dostupností odolnost proti chybám aplikací i plánování havárie obnovení"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure odolnost proti chybám příručku: obnovení z poškození dat nebo náhodného odstranění

Část robustní obchodního plánu kontinuitu má plán, pokud vaše data získá poškozený nebo omylem odstranili. Následující obrázek je informace o obnovení po poškození dat nebo omylem odstranili z důvodu chyby aplikací nebo chyby operátora.

##<a name="virtual-machines"></a>Virtuálních počítačích

K ochraně svého Azure virtuálních počítačích (někdy se jí říká VMs infrastruktury jako služby) z chyby aplikací nebo náhodného odstranění, použijte [Azure zálohování](https://azure.microsoft.com/services/backup/). Azure zálohování umožňuje vytváření záloh, které budou konzistentní na více disků OM. Kromě toho trezoru zálohování replikovat různých oblastí k poskytnutí obnovení před ztrátou oblast.

##<a name="storage"></a>Úložiště

Všimněte si, že zatímco úložišti Azure poskytuje odolnost proti chybám dat pomocí automatického repliky, to nezabrání vaše aplikace kód (nebo vývojáři/uživatelé) v poškození dat pomocí nechtěné ani před nechtěným odstranění, aktualizace a tak dál. Udržování dat věrností při aplikace nebo uživatel chyby vyžadují složitější postupy, například kopírování dat na sekundárním úložiště místo s protokolu auditování. Vývojáři můžou využívat objektů blob [funkce snímku](https://msdn.microsoft.com/library/azure/ee691971.aspx), což může vést k snímky v okamžiku jen pro čtení obsahu objektů blob. Slouží jako základ pro data věrností řešení pro objekty BLOB Azure úložiště.

###<a name="blob-and-table-storage-backup"></a>Kulatý a zálohování úložiště tabulky

Během objektů BLOB a tabulek jsou velmi trvalé, představují vždy aktuální stav data. Obnovení z nežádoucích úpravy nebo odstranění dat může vyžadovat obnovování dat na předchozí stav. Toho můžete dosáhnout využijete poskytované Azure k ukládání a zachování kopie v okamžiku.

Pro objekty BLOB Azure lze provést pomocí [funkce snímek objektů blob](https://msdn.microsoft.com/library/ee691971.aspx)zálohy v okamžiku. Pro každý snímek jsou pouze účtovaná za úložiště potřebných pro uložení rozdíly v objektů blob od posledního snímku stavu. Snímky jsou závislé na existenci původní objektů blob, které jsou založeny na, takže kopírování jiné objektů blob nebo dokonce jiný účet úložiště je vhodné. Zajistíte tím, že záložních dat je správně chráněn před nechtěným odstraněním. U tabulek Azure můžete si vytvořit v okamžiku kopie do jiné tabulky nebo objekty BLOB Azure. Podrobnější pokyny a příklady zálohování úrovni aplikace tabulek a objekty BLOB najdete tady:

  * [Ochrana tabulkách proti chybám aplikace](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Ochrana vaší objektů BLOB proti chybám aplikace](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Databáze

Databáze SQL Azure existuje několik možností [nepřerušený](../sql-database/sql-database-business-continuity.md) (zálohování, obnovení). Databáze můžete zkopírovat pomocí funkce [Kopie databáze](../sql-database/sql-database-copy.md) nebo [Export](../sql-database/sql-database-export.md) a [Import](https://msdn.microsoft.com/library/hh710052.aspx) souboru bacpac SQL Server. Kopie databáze obsahuje využití transakce stejné výsledky, ale nikoli bacpac (prostřednictvím služby import nebo export). Obě tyto možnosti fungují jako na základě fronty služby v datovém centru a jejich nenabízejí aktuálně SLA čas ukončení.

>[AZURE.NOTE]Kopírování a import nebo export možnosti databáze umístěte výrazněji zatížení na zdrojové databáze. Budou mohou být příčinou konflikty prostředků nebo omezení události.

###<a name="sql-database-backup"></a>Zálohování databáze SQL

Zálohování v okamžiku pro databázi Microsoft Azure SQL se zadáváním [kopírování databázi Azure SQL](../sql-database/sql-database-copy.md). Tímto příkazem můžete vytvořit kopii databáze využití transakce konzistentní na stejné logické databázový server nebo k jinému serveru. V obou případech kopie databáze je plně funkční a nezávislou zdrojové databáze. Kopií, které vytvoříte představuje možnosti obnovení v daném okamžiku. Stav databáze můžete obnovit úplně přejmenováním novou databázi název zdrojové databáze. Můžete taky můžete obnovit dílčí sadu dat z nové databáze pomocí dotazů jazyce Transact-SQL. Další podrobnosti o databáze SQL najdete v tématu [Přehled nepřerušený s databáze SQL Azure](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server na virtuálních počítačích zálohování

Pro systém SQL Server Azure infrastrukturu použité jako virtuálních počítačích služby (někdy označovány jako IaaS nebo IaaS VMs), dvěma způsoby: tradiční zálohování a dodávky protokolu. Použití tradiční zálohování umožňují obnovit k určitému bodu v čase, ale proces obnovení je pomalé. Obnovení tradiční zálohování vyžaduje počínaje počáteční úplné zálohování a použitím libovolné zálohy provedené po který. Druhou možností je třeba nakonfigurovat protokol dodací relace zpozdit obnovení záloh protokolu (například podle dvě hodiny). Díky tomu za tímto oknem se přizpůsobuje chyby na primární.

##<a name="other-azure-platform-services"></a>Další služby Azure platformy

Některé služby Azure platformu obsahují informace v databázi SQL Azure nebo uživatelem řízené úložiště účet. Pokud účet nebo úložiště zdroje se odstraní nebo poškozený, to se službou způsobit vážně chyby. V těchto případech je důležité ji Udržovat zálohy, které vám umožní znovu vytvořit tyto materiály, pokud jste odstranili nebo poškozený.

Azure weby a služby Azure mobilní telefon musíte zálohování a udržovat přidružené databáze. Pro službu Azure Media a virtuálních počítačích musíte údržby přidružený účet Azure úložiště a všechny zdroje v tomto účtu. Například pro virtuálních počítačích musí obecnějším údajům a správě disků OM v úložišti objektů blob Azure.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Seznamy úkolů pro poškození dat nebo náhodného odstranění

##<a name="virtual-machines-checklist"></a>Kontrolní seznam virtuálních počítačích

  1. Přečtěte si část virtuálních počítačích tohoto dokumentu.
  2. Obecnějším údajům a udržovat disků OM s Azure záložní (nebo zálohy systému pomocí úložišti objektů blob Azure a virtuální pevný disk snímky).

##<a name="storage-checklist"></a>Kontrolní seznam úložiště

  1. Přečtěte si část úložiště v tomto dokumentu.
  2. Pravidelně obecnějším údajům prostředků kritické úložiště.
  3. Zvažte použití funkci snímek pro objekty BLOB.

##<a name="database-checklist"></a>Kontrolní seznam databáze

  1. Přečtěte si část databáze v tomto dokumentu.
  2. Vytvoření zálohy v okamžiku pomocí příkazu kopie databáze.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server na virtuálních počítačích zálohování kontrolního seznamu

  1. Prohlédněte si SQL Server na virtuálních počítačích zálohování část tohoto dokumentu.
  2. Použít tradiční zálohování a obnovení postupy.
  3. Vytvořte opožděnou protokol dodací relace.

##<a name="web-apps-checklist"></a>Kontrolní seznam webové aplikace

  1. Obecnějším údajům a udržovat přidružené databáze, pokud existuje.

##<a name="media-services-checklist"></a>Kontrolní seznam Media Services

  1. Obecnějším údajům a spravovat zdroje přidružené úložiště.

##<a name="more-information"></a>Další informace

Další informace o funkcích zálohování a obnovení v Azure najdete v článku [úložiště, zálohování a obnovení scénáře](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady zaměření na [příručku Azure odolnost proti chybám](./resiliency-technical-guidance.md). Pokud hledáte další odolnost proti chybám havárie obnovení a dostupnost zdroje, najdete v Azure odolnost proti chybám příručku [Další zdroje informací](./resiliency-technical-guidance.md#additional-resources).