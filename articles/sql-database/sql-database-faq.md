<properties 
   pageTitle="Databáze Azure SQL časté otázky" 
   description="Odpovědi na běžné otázky zákazníci zeptejte se cloudu databází a databáze SQL Azure, systému správy relační databáze společnosti Microsoft (RDBMS) a databáze jako službu v cloudu." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Databáze SQL časté otázky

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Jak použití databáze SQL zobrazuje na faktuře? 
Databáze SQL faktury na předvídatelná hodinovou sazbu podle vrstvy služeb + úroveň výkonu pro jednu databáze nebo eDTUs fondu pružná databáze. Skutečné použití vyhodnocované a poměrně hodinové, takže vašeho účtu se můžou zobrazit zlomky za hodinu. Pokud databáze existuje 12 hodin za měsíc, najdete na faktuře Příkladem použití 0.5 dnů. Navíc – úrovně služeb + úroveň výkonu a eDTUs za fondu částky na faktuře usnadňuje zobrazíte spočítá počet dnů databáze, kterou jste použili pro jednotlivá pole v jeden měsíc.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Co když jedné databáze je aktivní menší než hodiny nebo používá vyšší vrstvy služeb pro menší než za hodinu?
Je faktura pro každou hodinu, kterou existuje databáze pomocí na nejvyšší úrovni služby + úroveň výkonu, která používají během této hodiny, bez ohledu na používání nebo jestli databázi bylo aktivní pro menší než za hodinu. Třeba když vytvoříte jednu databázi a odstraňte ji později pět minut faktuře odráží částka za hodinu jednu databázi. 

Příklady
    
- Pokud vytvoříte základní databázi a potom hned upgradovat na standardní S1, vám bude účtovaná sazbou standardní S1 za první hodiny.

- Pokud upgradujete databáze z Basic Premium na 10:00 a dokončení upgradu na 1:35 dop. Následující den vám bude účtovaná sazbou Premium počínaje 1:00 dop. 

- Je-li omezit databázi z Premium základní ve 11:00 a dokončením ve 2:15 a pak databázi bude účtováno sazbou Premium až 3:00 odp, po které bude účtováno základní sazeb.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Jak využití fondu pružná databáze zobrazuje na faktuře a co se stane, když změním eDTUs za fondu?
Pružná databáze fondu poplatky zobrazit na faktuře jako pružná DTUs (eDTUs) úsecích zobrazí pod eDTUs jeden fond na [stránce ceny](https://azure.microsoft.com/pricing/details/sql-database/). Je bezplatná za databáze pro fondy pružná databáze. Můžete se vám účtovat poplatky za každou hodinu fond existuje na nejvyšší eDTU bez ohledu na používání nebo zda fondu bylo aktivní pro menší než za hodinu. 

Příklady

- Pokud vytvoříte fond standardní pružná databáze s 200 eDTUs v 11:18:00, přidání pět databáze do fondu, vám bude účtovaná za 200 eDTUs pro celé hodiny, počínaje 11: 00 až zbývající dne.
- 2 den v 5:05 hodin 1 databáze, nebude zahájen jinými 50 eDTUs a obsahuje stabilní prostřednictvím den. Databáze 2 až 5 kolísání mezi 0 a 80 eDTUs. Během dne přidejte pět dalších databází, které používání různých eDTUs celý den. Den 2 je celý den fakturovány za 200 eDTU. 
- Den 3, ve 5: 00. Přidání jiného 15 databází. Použití databáze zvyšuje celý den do bodu, kde jste se rozhodli zvětšit eDTUs fondu 200 400 ve 8:05 Náklady na úrovni 200 eDTU projevilo až 20: 00 a zvyšují 400 eDTUs zbývající 4 hodiny. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Jak používat z Active Geo replikace ve fondu pružná databáze zobrazuje na faktuře?
Na rozdíl od jedné databáze pomocí pružná databáze [Aktivní Geo replikace](sql-database-geo-replication-overview.md) nemá přímý fakturační vliv.  Pouze vám bude účtovaná za eDTUs zřízení pro jednotlivá pole fondů (fondu primární a sekundární fondu)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Jaký použít funkci auditování má vliv na můj účet? 
Sestavy auditování je zabudované do služby SQL databáze zvyšovat náklady a je dostupná Basic, Standard a Premium databází. Však k ukládání protokolů auditování, auditování použití funkce, které účet Azure úložiště a sazby pro tabulky a fronty v úložišti Azure použít na základě velikosti protokolu auditování.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Jak najít správné služby osy a výkonu úrovně jedné databáze a fondů pružná databáze 
Jsou k dispozici je několik nástrojů. 

- Pro místní databáze pomocí [Poradce pro změnu velikosti DTU](http://dtucalculator.azurewebsites.net/) doporučit databází a DTUs povinné a vyhodnocení více databázím pro fondy pružná databáze.
- Pokud by jednu databázi výhodné před do fondu, doporučuje inteligentní modul Azure na uvidí vzorek historických použití, která zaručuje, je-li fondu pružná databáze. V tématu [sledování a správa fondu pružná databáze pomocí portálu Azure](sql-database-elastic-pool-manage-portal.md). Podrobnosti o tom, jak výpočty sebe najdete v tématu [aspektech cena a výkonu pro fond pružná databáze](sql-database-elastic-pool-guidance.md)
- Jestli potřebujete vytočit jednu databázi nahoru nebo dolů najdete [pokyny výkonu pro jednu databáze](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Jak často můžete změnit úroveň osy nebo výkonu služby jednu databázi? 
U V12 databází můžete změnit vrstvy služeb (mezi Basic standardní a Premium) nebo úroveň výkonu v rámci vrstvy služeb (například S1 s2) podle potřeby můžete. U starších verzí databází můžete změnit úroveň osy nebo výkonu služby celkem čtyřikrát 24 hodin.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Jak často můžete upravit eDTUs za fondu? 
Jak často se má.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Jak dlouho trvá úrovně služby osy nebo výkonu z jedné databáze nebo přesunutí databáze a odhlášení z fondu pružná databáze? 
Změna vrstvy služeb databáze a přesunutí a odhlášení z fondu vyžaduje databáze, kterou chcete zkopírovat na platformě jako operace pozadí. Změna vrstvy služeb může trvat několik minut až několik hodin v závislosti na velikosti databází. V obou případech databází zůstat online a dostupný během přesunutí. Podrobnosti o změně jedné databáze najdete v tématu [Změna vrstvy služeb databáze](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kdy mám použít jednu databázi porovnání pružná databáze? 
Obecně fondů pružná databáze jsou sice pro typické [software jako služby (SaaS) aplikace vzorku](sql-database-design-patterns-multi-tenancy-saas-applications.md), kde je jedna databáze na zákazníky nebo klienta. Zakoupení jednotlivé databází a overprovisioning zahájit proměnnou a vrcholu služba na každou databázi je často není náklady. Pomocí skupin spravovat byste výkon fondu a databází měřítko nahoru a dolů automaticky. 

Inteligentní modul Azure společnosti doporučuje fond pro databáze při použití vzorek vyžadoval ho. Podrobnosti najdete v tématu [databáze SQL ceny doporučení osy](sql-database-service-tier-advisor.md). Podrobné informace o výběru mezi jednotlivými a pružná databází najdete v článku [aspektech cena a výkonu pro fondy pružná databáze](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Co znamená mít až 200 % úložišti maximální zřizování databáze pro záložní úložiště? 
Zálohování úložiště je přidružený k zálohování automatické databáze, která se používají pro [bodu-v-čas-obnovení](sql-database-recovery-using-backups.md#-point-in-time-restore) a [Geo obnovit](sql-database-recovery-using-backups.md#geo-restore). Databázi Microsoft Azure SQL poskytuje až 200 % úložišti maximální zřizování databáze záložní úložiště bezplatně. Například pokud máte instanci standardní DB s velikostí zřizování DB 250 GB, máte k dispozici 500 GB úložiště zálohy bez dalších poplatků. Pokud databázi překročí zadané záložní úložiště, můžete ke snížení období uchování kontaktováním podpory Azure nebo zaplatit úložiště navíc zálohy účtováno standardní sazbou přístup pro čtení geograficky nadbytečné úložiště (Vzdálená pomoc GRS). Další informace o fakturaci Vzdálená pomoc GRS zobrazit podrobnosti ceny úložiště.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Můžu mi Přesunované z Web/firmy do nové služby úrovní, co je potřeba vědět?
Azure SQL Web a Business databáze jsou teď už nepoužívá. Vrstvách Basic, Standard, Premium a ohebné nahraďte retiring Web a obchodní databází. Jsme Další nejčastější dotazy, které vám pomohou v tomto přechodu období. [Web a Business Edition Jahoda – nejčastější dotazy](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Co je replikace očekávaného prodlevy při geo replikace databáze mezi dvěma oblastmi v rámci stejné Azure geography?  
Jsme momentálně podporují operace RPO pět sekund a prodlevy replikace byl menší než, kdy je hostovaný geo sekundární v Azure doporučené párových oblast a na stejné úrovni služby.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Co je replikace očekávaného prodlevy při vytvoření geo sekundární ve stejné oblasti jako primární databáze?  
Založené na základě zkušeností dat, není příliš rozdíl mezi uvnitř oblasti a replikace mezi oblast prodlevy při Azure doporučujeme použít párových oblast. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Pokud je mezi dvěma oblastmi selhání sítě, jak Logika opakování funguje při nastavení Geo replikace?  
Pokud je k odpojení, jsme opakovat každých 10 sekund znovu navázat připojení.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Jak zajistit, že je replikovat kritické změnit primární databázi?
Sekundární geo je asynchronní otevřené a ne pokusíme uchovat celou synchronizovat s primární. Ale nabízíme způsob, jak vynutit synchronizaci zajištění replikace kritické změny (například hesla aktualizace). Vynuceného synchronizace ovlivňuje výkonu, protože blokuje volání vlákna, dokud replikovat všechny potvrzené transakce. Podrobnosti najdete v tématu [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Jaké nástroje jsou k dispozici ke sledování prodlevy replikace mezi databáze primární a sekundární geo?
Jsme vystavit prodlevy v reálném čase replikace mezi primární databáze a geo sekundární až DMV. Podrobnosti najdete v tématu [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
