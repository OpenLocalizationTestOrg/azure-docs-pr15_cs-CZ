<properties
   pageTitle="Nepřerušený provoz v cloudu – databáze obnovení - databáze SQL | Microsoft Azure"
   description="Zjistěte, jak databáze SQL Azure podporuje nepřerušený provoz cloudu a obnovení databáze a pomáhá chránit důležitých cloudu spuštěné."
   keywords="Nepřerušený provoz nepřerušený provoz cloudu, havárie obnovení databáze, obnovení databáze"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Základní informace o nepřerušený s databáze SQL Azure

Přehled popisuje funkce, které stanoví nepřerušený a obnovení databáze SQL Azure. Poskytuje možnosti, doporučení a výukové programy pro obnovení z vyloučit událostí, ke kterým může dojít ke ztrátě dat nebo způsobit databáze a aplikace nedostupná. Diskuse obsahuje co dělat, když uživatel aplikace chyby ovlivňuje integrity dat, Azure oblast obsahuje výpadku nebo aplikace vyžaduje údržbu. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funkce databáze SQL, které můžete použít k poskytnutí provozní kontinuitu

Databáze SQL nabízí několik funkcí kontinuitu firmy, včetně automatické zálohy a replikace databáze nepovinné. Má každá různou charakteristikou odhadovaná obnovení času (Vložit) a možnou ztrátu dat pro poslední transakce. Jakmile pochopíte tyto možnosti můžete vybrat mezi nimi – a, ve většině případů, používáte dohromady různých scénářích. Při přípravě vašeho plánu kontinuitu firmy je potřeba porozumět maximální přijatelné dobu, po jejímž aplikace plně obnoví po vyloučit události – to je vaším cílem časového využití (RTO). Je taky potřeba porozumět maximální velikost nejnovější aktualizace dat (časový interval) nevadí vám aplikace ztráty při obnově po vyloučit události - cíle bodu obnovení (operace RPO). 

V následující tabulce porovnává vložit a operace RPO tři nejběžnější scénářích.

| Funkce |  Základní vrstvy | Standardní vrstvy  | Premium osy |
|---|---|---|---|
| Přejděte v době obnovení ze zálohy | Některé bod obnovení do 7 dnů   | Některé bod obnovení do 35 dnů  | Některé bod obnovení do 35 dnů |
GEO obnovení ze zálohy replikovat geo | Vložit < 12 hodin, operace RPO < 1h   | Vložit < 12 hodin, operace RPO < 1h   | Vložit < 12 hodin, operace RPO < 1h |
|Aktivní Geo replikace | Vložit < 30s, operace RPO < 5s s   | Vložit < 30s, operace RPO < 5s s | Vložit < 30s, operace RPO < 5s s |


### <a name="use-database-backups-to-recover-a-database"></a>Obnovení databáze pomocí zálohy databáze

Databáze SQL automaticky provádí kombinací úplné zálohování databáze týdenní, rozdílné vyhovovaly ochrana před ztrátou dat databáze každou hodinu a transakční protokol zálohování každých pět minut. Tyto zálohy jsou uložené v místně nadbytečné úložiště 35 dnů pro databáze v služby úrovní Standard a Premium a sedmi dnů pro databáze ve vrstvě služby základní - viz [– úrovně služeb](sql-database-service-tiers.md) podrobné informace o – úrovně služeb. Pokud období uchování vrstvy služeb nesplňuje vašim požadavkům pro firmy, můžete zvětšit doba uchovávání informací změnou [vrstvy služeb](sql-database-scale-up.md). Úplné a rozdílné databázi, kterou zálohy jsou i replikovat [párových datacentra](../best-practices-availability-paired-regions.md) pro ochranu před výpadku Centrum dat – najdete v článku [zálohy databáze automatické](sql-database-automated-backups.md) další podrobnosti.

Obnovení databáze z různých vyloučit události, v datovém centru i jiné datacentrem můžete tyto automatické zálohování databází. Použití automatické zálohování databází, předpokládanou dobu obnovení závisí na několika faktorech jako celkový počet databází obnovení ve stejné oblasti současně, velikost databáze, transakční protokol velikost a šířka pásma. Ve většině případů časového využití je menší než 12 hodin. Při obnově do jiné oblasti dat, možnou ztrátu dat se omezí na 1 hodinu geo nadbytečné úložiště hodinové zálohování rozdílné databáze. 

> [AZURE.IMPORTANT] Když Pokud chcete obnovit, pomocí automatické zálohy, musíte být členem role přispěvatele SQL Server nebo vlastník předplatného – najdete v článku [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Můžete obnovit pomocí portálu Azure, prostředí PowerShell nebo rozhraní REST API. Nelze použít jazyce Transact-SQL.

Pokud jako firmy kontinuitu a obnovení mechanismus použít automatické zálohy aplikace:

- Nejsou považovány za velice důležité.
- Nemá vazby SLA proto prostoje 24 hodin či delší nebude mít za následek finanční odpovědnosti.
- Má zhoršeným sazba Změna dat (zhoršeným transakce / hod) a ztráty až za hodinu změnit ztrátu přijatelného data. 
- Pole Náklady se rozlišují malá písmena. 

V případě potřeby rychlejší obnovení použijte [Aktivní Geo replikace](sql-database-geo-replication-overview.md) (popsáno dále). Pokud potřebujete mít možnost obnovení dat z období starší než 35 dní, zvažte archivace databáze pravidelně do souboru BACPAC (mu tuhle zkomprimovanou soubor obsahující schématu databáze a Spojenými daty) uložené v úložišti objektů blob Azure nebo do jiného umístění podle svého výběru. Další informace o tom, jak vytvořit archiv využití transakce konzistentní databáze najdete v článku [Vytvoření kopie databáze](sql-database-copy.md) a [Exportovat kopie databáze](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Použití aktivní Geo replikace zkrátit obnovení a omezit ztrátou dat přidružený k obnovení

Kromě použití zálohování databáze pro obnovení databáze v případě narušením firmy, můžete [Aktivní Geo replikace](sql-database-geo-replication-overview.md) ke konfiguraci databáze mít až se čtyřmi čitelné sekundární databází v oblasti podle svého výběru. Tyto sekundární databáze jsou průběžně synchronizovány s hlavní databází pomocí mechanismus asynchronní replikace. Tato funkce slouží k ochraně proti firmy narušení v případě výpadku Centrum dat nebo během upgradu aplikace. Aktivní Geo replikace lze také geograficky rozdělený uživatelům poskytovat lepší výkon dotazu pro dotazy jen pro čtení.

Pokud je hlavní databází přejde do režimu offline neočekávaně nebo je nutné brát je offline pro činnosti spojené s údržbou, můžete rychle převést sekundární primární (nazývané také selhání) a konfiguraci aplikací pro připojení k nově propagovanými primární. Plánované selhání je beze ztráty dat. S neplánované převzetí může být některé malou část ztrátou dat pro velmi poslední transakce kvůli povahy asynchronní replikace. Po selhání můžete později navrácení - podle plánu nebo datovém centru vycházejí návrat do online režimu. Ve všech případech uživatelé dojít malou část výpadek služeb a muset připojit. 

> [AZURE.IMPORTANT] Použití aktivní Geo replikace, musíte být vlastník předplatného nebo oprávnění správce serveru SQL Server. Můžete nakonfigurovat a převzetí pomocí portálu Azure, prostředí PowerShell nebo rozhraní REST API pomocí oprávnění u předplatného nebo jazyce Transact-SQL pomocí oprávnění v rámci SQL serveru.

Aktivní Geo replikace použijte, pokud vaše aplikace splňuje následující kritéria:

- Je velice důležité.
- Má smlouvy o úrovni služeb (SLA), který neumožňuje 24 hodin a víc výpadek služeb.
- Prostoj při povede finanční odpovědnosti.
- Má vysoká rychlost dat změnit vysoká a ztráty hodinu dat není přijatelná.
- Další náklady aktivní geo replikace je menší než potenciální finanční odpovědnosti a související ztrátu business.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Obnovení databáze po chybu uživatele nebo aplikace

* Nikdo je ideální! Uživatel může neúmyslně odstraníte některá data, omylem přetáhnout důležité tabulky nebo i přetáhnout celou databázi. Nebo aplikace může omylem přepsat dobré data chybného kvůli vadu aplikace. 

V tomto scénáři tyhle možnosti obnovení.

### <a name="perform-a-point-in-time-restore"></a>Obnovení v okamžiku

Automatické zálohy lze použít k obnovení kopii databáze pro známé dobré bodu v čase, za předpokladu, že čas je v období uchování databáze. Po obnovení databáze je nahraďte původní databáze obnovenou databázi a kopírovat požadovaná data z obnovená data do původní databáze. Pokud databázi používá aktivní Geo replikace, doporučujeme kopírování požadovaná data v obnovených kopírovat do původní databáze. Pokud nahradíte původní databáze obnovenou databázi, bude muset překonfigurovat a synchronizovat aktivní Geo replikace (který může trvat dlouho pro velké databáze). 

Další informace a podrobný postup pro obnovení databáze na bod v čase pomocí portálu Azure nebo pomocí Powershellu najdete v tématu [obnovení v daném okamžiku](sql-database-recovery-using-backups.md#point-in-time-restore). Se nedá obnovit pomocí jazyce Transact-SQL.

### <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze

Pokud databáze se odstraní, ale nebyla odstraněna logické server, můžete obnovit odstraněnou databázi čárky niž byl odstraněn. Obnovení záložní databáze stejný logické SQL Server ze kterého byla Odstraněná. Můžete obnovit pomocí původní název nebo zadat nový název nebo obnovení databáze.

Další informace a podrobný postup pro obnovení odstraněné databáze pomocí portálu Azure nebo pomocí Powershellu najdete v tématu [Obnovení odstraněných databáze](sql-database-recovery-using-backups.md#deleted-database-restore). Nebude možné obnovit pomocí jazyce Transact-SQL.

> [AZURE.IMPORTANT] Pokud odstranili logické serveru se nedá obnovit odstraněnou databázi. 

### <a name="import-from-a-database-archive"></a>Import z archivu databáze

Pokud mimo aktuální období uchování automatické zálohy došlo k ztrátou dat a mít byla archivace databáze, můžete [Importovat soubor archivované BACPAC](sql-database-import.md) novou databázi. V tomto okamžiku můžete nahraďte původní databáze importovaných databáze nebo kopírování požadovaná data z importovaných dat do původní databáze. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Obnovení databáze do jiné oblasti z výpadku Centrum Azure data pro oblasti

<!-- Explain this scenario -->

V několika málo Azure datacentrem může obsahovat výpadku. Při výpadek způsobí přerušení firmy, které mohou pouze trvat několik minut nebo může trvat hodin. 

- Jednou z možností je potřeba počkat, databáze pro opětovném přechodu do online po výpadku Centrum data. To funguje pro aplikace, které můžou dovolit mít offline databázi. Příklad vývojového projektu nebo bezplatnou zkušební verzi nemusíte dopracování neustále. Po datového centra výpadku, nebudou vědět, jak dlouho bude trvat výpadku, funkčnost tuto možnost, pouze pokud nepotřebujete databázi určitou dobu.
- Další možností je buď přepnutí do jiné oblasti dat používáte-li aktivní Geo replikace nebo obnovit pomocí zálohy geo záložní databáze (Geo obnovit). Převzetí trvá jen pár sekund, při obnovení systému pomocí zálohy trvá hodin.

Kdy bude akce, jak dlouho trvá obnovit a kolik ztrátou dat vzniknou v případě výpadku dat centra závisí na tom, jak se rozhodnete používat funkce kontinuitu firmy popsané výše v aplikaci. Nakonec můžete pomocí kombinace zálohování databáze a aktivní Geo replikace v závislosti na požadavcích aplikace. Diskusní pokyny k návrhu aplikace pro samostatné databází a pružná fondů použití těchto funkcí kontinuitu firmy najdete v článku [návrhové aplikace pro obnovení havárie cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Následující části obsahují poskytují přehled kroků, které obnovit pomocí zálohy databáze nebo aktivní Geo replikace. Podrobný postup včetně plánování požadavky příspěvek obnovení kroky a informace o tom, aby napodobily výpadku provádět přechod obnovení havárie najdete v tématech [Obnovit databázi SQL z výpadku](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Příprava výpadek

Bez ohledu na funkci kontinuitu firmy, které používáte, musíte:

- Identifikace a připravit cílový server, včetně pravidla serveru Brána firewall, přihlášení a oprávnění na úrovni předlohy databáze.
- Zjistěte, jak přesměrování klientů a klientské aplikace k novému serveru
- Dokument jiné závislosti, například auditování výstrahy a nastavení 
 
Pokud není plánování a příprava správně, jejich přenesením aplikace online po selhání nebo obnovení trvá déle a pravděpodobně vyžadují navíc Poradce při potížích po druhém namáhání - chybná kombinaci.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Přepnutí do replikovat geo sekundární databáze 

Pokud používáte aktivní Geo replikace jako váš mechanismus obnovení [Vynutit přepojení na sekundární replikovat geo](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Vyvolané sekundární propagované osvobozením od nové primární a je připraven k zaznamenání nového transakce zpráv a odpovídání na všechny dotazy - s jenom několik sekund, než ztráty dat pro data, která nebyly dosud replikovat. Informace o automatizaci překlopení obrázku najdete v tématu [Návrh žádost o obnovení havárie cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Když datovém centru přejde do režimu online, můžete navrácení na původní primární (nebo neuchovával).

### <a name="perform-a-geo-restore"></a>Obnovení Geo- 

Pokud používáte automatické zálohy s opakováním geo nadbytečné úložiště jako váš mechanismus obnovení [zahájit obnovení databáze pomocí Geo obnovit](sql-database-disaster-recovery.md#recover-using-geo-restore). Obnovení obvykle probíhá do 12 hodin – se ztrátou dat až jednu hodinu určena při poslední hodinové rozdílné zálohování s přijatá a replikovanou. Až do dokončení obnovení databáze nejde zaznamenat všechny transakce nebo odpověď na všechny dotazy. 

> [AZURE.NOTE] Pokud datovém centru přejde do režimu online před přepnutím aplikace obnoveného databáze, můžete jednoduše zrušit obnovení.  

### <a name="perform-post-failover--recovery-tasks"></a>Provedení publikování převzetí nebo obnovení úkoly 

Po obnovení z obou mechanismus obnovy musí provést tyto další věci před uživatelů a aplikací se zpět do začátků:

- Přesměrování klientů a klientské aplikace na nový server a obnovení databáze
- Zajistěte, aby příslušné brány firewall úrovni serveru pravidla platí na místě, aby je uživatelé mohli připojit (nebo použít [úrovni databáze bránách firewall](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Zkontrolujte, zda jsou v místě odpovídající přihlášení a oprávnění na úrovni předlohy databáze (můžete taky použít [obsažené uživatelů](https://msdn.microsoft.com/library/ff929188.aspx))
- Konfigurace auditování v případě potřeby
- Konfigurace upozornění, podle potřeby

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgrade aplikace s minimálními výpadek služeb

Někdy aplikace musí být do režimu offline z důvodu plánované údržby například upgrade aplikace. [Upgrade aplikace spravovat](sql-database-manage-application-rolling-upgrade.md) popisuje způsob použití aktivní Geo replikace povolit postupné inovace cloudu aplikaci prostoje během aktualizace a zadejte cestu obnovení v případě, že dojde k chybě. Tento článek sleduje dva různé způsoby orchestrating procesu upgradu a pojednává o výhodách a střídání jednotlivých možností.

## <a name="next-steps"></a>Další kroky

Diskusní pokyny k návrhu aplikace pro samostatné databází a pružná fondů najdete v článku [návrhové aplikace pro obnovení havárie cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






