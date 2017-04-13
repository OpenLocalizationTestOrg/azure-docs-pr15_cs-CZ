<properties
    pageTitle="Aktivní Geo replikace databáze Azure SQL"
    description="Aktivní Geo replikace umožňuje nastavit 4 kopie databáze v některém z Azure datacentrech."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Přehled: SQL aktivní Geo replikace databáze

Aktivní Geo replikace umožňují konfigurovat až se čtyřmi čitelné sekundární databáze na stejném nebo jiném dat centra místech (oblasti). Sekundární databáze jsou dostupné pro dotazy a převzetí v případě výpadku Centrum dat nebo připojení k databázi primární.

>[AZURE.NOTE] Aktivní Geo replikace (čitelné druhotné) je teď k dispozici pro všechny databáze ve všech vrstvách služby. V duben 2017-čitelné sekundární typ bude vyřadit a existující-čitelné databáze upgradují automaticky k čitelné druhotné.

 Konfigurace aktivní Geo replikace pomocí [Azure portál](sql-database-geo-replication-portal.md), [Powershellu](sql-database-geo-replication-powershell.md), [Jazyce Transact-SQL](sql-database-geo-replication-transact-sql.md), nebo [rozhraní REST API – vytvoření nebo aktualizace databázi](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfigurace: Azure portálu](sql-database-geo-replication-portal.md)
- [Konfigurace: prostředí PowerShell](sql-database-geo-replication-powershell.md)
- [Konfigurace: T-SQL](sql-database-geo-replication-transact-sql.md)

Pokud některé důvod svůj primární databáze se nezdaří, nebo jednoduše je potřeba vzít offline, můžete *převzetí* k některým z vašich sekundární databází. Po aktivaci převzetí na jednu vedlejší databází jiných druhotné automaticky propojen nové primární.

Je možné převezme sekundární pomocí [Azure portál](sql-database-geo-replication-failover-portal.md), [Powershellu](sql-database-geo-replication-failover-powershell.md), [Jazyce Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), [Rozhraní REST API - plánované převzetí](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)nebo [Rozhraní REST API - neplánované převzetí](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Převzetí: Azure portálu](sql-database-geo-replication-failover-portal.md)
- [Převzetí: prostředí PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Převzetí: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Po selhání zajistěte, aby že požadavky ověřování pro server a databázi nastavené na nové primární. Další informace najdete v tématu [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md).


Funkce aktivní Geo replikace používá mechanismus za účelem zálohování databáze ve stejné oblasti Microsoft Azure nebo v různých oblastech (geo redundance). Aktivní Geo replikace asynchronní zreplikuje potvrzené transakce z databáze až se čtyřmi kopie databáze na jiné servery, pomocí číst potvrzené snímek izolace (RCSI) pro izolace. Při konfiguraci aktivní Geo replikace sekundární databáze se vytvoří na zadaném serveru. Původní databáze bude primární databáze. Primární databáze asynchronní zreplikuje potvrzené transakce na každé sekundární databáze. Pouze celé transakce jsou replikovat. V libovolném bodě, sekundární databáze může být trochu za databázi primárního, sekundárního datového zaručena ukládání nemusíte vůbec starat částečné transakce. Konkrétních dat operace RPO najdete na [Nepřerušený přehled](sql-database-business-continuity.md).

Jednou z výhod primární aktivní Geo-replikace je, že nabízí řešení obnovení databáze úrovni havárie s časem nízké využití. Když umístíte sekundární databáze na serveru v jiné oblasti, přidejte maximální odolnost aplikaci. Více oblastí redundance aplikacím obnovit před trvalé ztrátou celý datacentra nebo částí datacentra způsobená natural havárie, katastrofické lidské chyby nebo škodlivým úkony. Následující obrázek ukazuje, že příklad aktivní Geo-replikace nakonfigurována primární v oblasti severní centrální US a vedlejší v oblasti Jih centrální US.

![Replikace GEO relace](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Další výhodou klíčové je, že sekundární databází číst a mohou sloužit k převzít jen pro čtení úloh třeba úlohy sestav. Pokud chcete použít sekundární databáze pro vyrovnávání zatížení, můžete ji vytvořit ve stejné oblasti jako primární. Vytváření sekundárních ve stejné oblasti, není rozšiřte odolnost aplikace na katastrofické selhání.  

Další scénáře použití aktivní Geo replikace patří:

- **Migrace databáze**: aktivní Geo replikace můžete migrovat databázi z jednoho serveru do druhého online s minimální prostoje.
- **Upgrade aplikace**: můžete vytvořit navíc sekundární jako kopii back selhání během aktualizace aplikací.

Dosáhnout skutečné nepřerušený přidání záložní databáze mezi datacentrech je jenom část řešení. Obnovení aplikace (služba) začátku do konce po Katastrofální selhání vyžaduje využívání všech součástí, které představují službu a závislé služby. Příklady tyto součásti: klientský software (například prohlížeč s vlastní JavaScript), webových front-end, skladování a DNS. Je důležité, všechny komponenty se pružné na stejné selhání a k dispozici v rámci cíle čas obnovení (RTO) aplikace. Proto musíte identifikovat všechny závislé služby a interpretaci záruky a funkcí, které poskytují. Potom se musí kroků odpovídající zajistit, aby funkce služby při selhání služby, u kterých záleží. Další informace o navrhování řešeních havárie obnovení najdete v článku [Navrhování cloudové řešení pro havárie obnovení pomocí aktivní Geo replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktivní možnosti replikace Geo
Funkce aktivní Geo replikace poskytuje následující základní možnosti:

- **Automatické asynchronní replikace**: sekundární databáze můžete vytvořit jenom přidáním do existující databáze. Sekundární lze vytvořit pouze v jiném serveru databáze SQL Azure. Po vytvoření sekundární databáze vyplněné s daty zkopírovanými z primární databáze. Tento proces se nazývá ohlašovat. Po sekundární databáze byly vytvořeny a nasadí, aktualizace primární databáze jsou asynchronní replikovat na vedlejší databáze automaticky. Asynchronní replikace znamená, že transakce jsou potvrzena primární databázi před replikovat na vedlejší databáze. 

- **Více sekundární databází**: dvě nebo více sekundární databází větší redundance a úroveň ochrany primární databáze a aplikace. Pokud existuje více sekundární databází aplikace zůstane chráněné i v případě jednu vedlejší databáze se nezdaří. Pokud je pouze jednu vedlejší databáze a dojde k chybě, aplikace vystaven vyšší rizika, dokud je vytvořena nová sekundární databáze.

- **Čitelné sekundární databáze**: aplikace můžete získat přístup k databázi vedlejší jen pro čtení operací pomocí objekty stejném nebo jiném zabezpečení použít pro přístup k databázi primární. Sekundární databází pracovat v režimu izolace snímek zajistit replikace aktualizací primární (protokolu znova se přehrávat) není posunout tak, že dotazy na sekundární.

>[AZURE.NOTE] Přehrání protokolu zpožděn sekundární databázi pokud jsou tu uvedené schématu aktualizacích doručování z primární, které vyžadují zámek schématu sekundární databázi.

- **Aktivní geo replikace pružná fondu databází**: možné konfigurovat aktivní Geo replikace databáze v každém fondu pružná databáze. Sekundární databáze může obsahovat další fond pružná databáze. Běžná, pro databáze lze sekundární pružná databáze fondu a naopak naopak, dokud – úrovně služeb jsou stejné. 

- **Úroveň, která dokáže nahradit výkonu sekundární databáze**: sekundární databázi lze vytvořit s nižší výkon než primární. Primární a sekundární databází musí mít stejný vrstvy služeb. Tuto možnost nedoporučujeme pro aplikace s vysokou databáze zápisu aktivitou protože prodlevy lepší replikace zvyšuje riziko ztráty dat podstatné po přepojení. Kromě toho po převzetí aplikace je dopad na výkon, dokud nové primární upgradovat na vyšší úrovni výkonu. Graf procento vstupu a výstupu protokolu na Azure portálu poskytuje vhodná k odhadu úroveň minimální výkonu sekundární, která je potřebná k udržení replikace načíst. Například, pokud je P6 primárního databáze (1000 DTU) a jeho protokolu vstupu a výstupu v procentech je 50 % sekundární musí být alespoň P4 (500 DTU). Můžete taky protokolu vstupu a výstupu data načtete pomocí [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) nebo [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) zobrazení databáze.  Další informace o úrovních výkonu databáze SQL najdete v článku [Možnosti SQL databáze a výkonu](sql-database-service-tiers.md). 

- **Uživatelem řízené překlopení a překlopení zpět**: sekundární databáze můžete explicitně přejít primární roli kdykoliv tak, že aplikace nebo uživatele. Během výpadku skutečné "neplánované" možnost bude použito, které okamžitě líbí, zvýší sekundární jako primární. Až selhalo primárního obnoví a znovu je k dispozici, systém automaticky označí obnoveného primárního jako sekundární a ji aktuální a mají nové primární. Z důvodu asynchronní přírodní replikace malou část dat může dojít ke ztrátě během neplánované převzetí služeb při selhání primární selže před zreplikuje posledních změn na sekundární. Při selhání primární s více druhotné systému automaticky změní konfiguraci replikace vztahy a odkazy zbývající druhotné na nově propagovanými primární bez zásahu uživatele. Po zmírnit výpadku, která způsobila záložní může být vhodné návrat aplikace oblasti primární. Aby je dostala, je třeba použít příkaz převzetí s parametrem "plánované". 

- **Přihlašovací údaje a pravidla brány firewall synchronní**: doporučujeme pomocí [pravidla databáze brány firewall](sql-database-firewall-configure.md) pro geo replikace databáze tak následujícími pravidly replikovat s databází zajistit všechny sekundární databáze stejná pravidla brány firewall jako primární. Tento přístup nemusí zákazníci ruční konfiguraci a správě pravidel brány firewall na serverech hostingu jak primárních a sekundárních databází. Podobně pomocí [obsažené databáze uživatelů](sql-database-manage-logins.md) pro přístup k datům zaručuje, že primárních a sekundárních databází vždy mít stejný uživatel tak pověření při selhání, není žádná přerušení kvůli problémy s uživatelského jména a hesla. Po přidání [Azure Active Directory](../active-directory/active-directory-whatis.md)zákazníci spravovat přístup uživatelů k primárních a sekundárních databází a odstranění potřebné pro správu přihlašovacích údajů v úplně databází.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgrade nebo přechodu primární databáze
Můžete upgradovat nebo omezit primární databáze do různých výkonu úrovně (v rámci stejné vrstvy služeb) bez odpojení sekundární databází. Při upgradu, doporučujeme upgradovat sekundární databázi nejdřív a potom upgrade primární. Při přechodu, obrátit pořadí: nejdřív omezit primární a omezit sekundární. 

Sekundární databáze musí být ve stejné vrstvy služeb jako primární, tak migrace primární databáze na jinou službu osy, musíte buď ukončit odkaz geo replikace a přejmenujte sekundární databáze nebo jednoduše přetáhnout. Potom migrace primární do nové vrstvy služeb a překonfigurovat geo replikace. Nové sekundární se vytvoří automaticky se stejnou úrovní výkonu jako primární ve výchozím nastavení.

## <a name="preventing-the-loss-of-critical-data"></a>Zabránění ztrátě důležitá data
Z důvodu vysokou latencí rozsáhlé sítě nepřetržitý kopie používá mechanismus asynchronní replikace. Asynchronní replikace zajišťuje ztrátu dat nezbytné dojde k chybě. Některé aplikace však může vyžadovat beze ztráty dat. Pokud chcete zamknout tyto důležité aktualizace, vývojář aplikace volání postup systém [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) bezprostředně za spuštěním transakce. Volání **sp_wait_for_database_copy_sync** bloky volající vlákno až do poslední potvrzené transakcí bylo replikovat na vedlejší databáze. Postup bude Počkejte, dokud všech ve frontě transakcí bylo potvrzeno sekundární databází. **sp_wait_for_database_copy_sync** má obor vymezený na konkrétní nepřetržitý Kopírovat odkaz. Všichni uživatelé s oprávněním připojení k databázi primárního upoutat tento postup.

>[AZURE.NOTE] Zpoždění způsobená volání procedur **sp_wait_for_database_copy_sync** může být významný. Zpoždění závisí na velikosti délka protokolu transakce v okamžiku, kdy a volání nevrací dokud replikovat celý protokol. Vyhněte se volání tento postup Pokud vytvářejte.

## <a name="programmatically-managing-active-geo-replication"></a>Programově Správa aktivní Geo replikace

Jak bylo popsáno dříve, aktivní Geo replikace lze spravovat také programově pomocí prostředí PowerShell Azure a rozhraní REST API. Následující tabulka popisuje sady příkazů k dispozici.

- **Rozhraní API Správce prostředků Azure a na základě rolí zabezpečení**: aktivní Geo replikace obsahuje sadu [Rozhraní API Azure správce prostředků]( https://msdn.microsoft.com/library/azure/mt163571.aspx) pro správu, včetně [rutiny prostředí PowerShell založené na Azure správce prostředků](sql-database-geo-replication-powershell.md). Tato rozhraní API vyžaduje použití skupiny zdrojů a podpora na základě rolí zabezpečení (RBAC). Další informace o tom, jak implementovat přístup rolí v tématu [Řízení přístupu Azure Role-Based](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Mnoho nových funkcí aktivní Geo-replikace podporují pouze pomocí [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) na základě [Azure SQL REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) a [rutinách Powershellu databáze SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx). (Klasické) rozhraní REST API] (https://msdn.microsoft.com/library/azure/dn505719.aspx) a [(klasické) rutiny pro správu databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn546723.aspx) podporují z důvodu zpětné kompatibility tak, aby se doporučuje pomocí rozhraní API, založené na Azure správce prostředků. 


### <a name="transact-sql"></a>Skalární funkce

|Příkaz|Popis|
|-------|-----------|
|[Příkaz ALTER databáze (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|Používání argumentu přidat sekundární na SERVER vytvoříte sekundární databázi pro stávající databáze a replikace spuštění dat|
|[Příkaz ALTER databáze (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|K přepnutí sekundární databáze, která bude primárního zahajte převzetí pomocí převzetí nebo FORCE_FAILOVER_ALLOW_DATA_LOSS
|[Příkaz ALTER databáze (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|Použití odebrat sekundární na SERVER ukončit replikace dat mezi databázi SQL a zadaná sekundární databáze.|
|[Sys.geo_replication_links (databáze SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx)|Vrátí informace o všech existujících replikační odkazy na každou databázi na serveru logické databáze SQL Azure.|
|[Sys.dm_geo_replication_link_status (databáze SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)|Získá poslední čas replikace, poslední replikace prodlevy a další informace o propojení replikace pro danou databázi SQL.|
|[Sys.dm_operation_status (databáze SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx)|Zobrazí stav pro všechny operace databáze včetně stavu replikační odkazy.|
|[sp_wait_for_database_copy_sync (databáze SQL Azure)](https://msdn.microsoft.com/library/dn467644.aspx)|způsobí, že aplikace počkejte, dokud se všechny potvrzené transakce replikovat a potvrzeno aktivní sekundární databáze.|
||||

### <a name="powershell"></a>Prostředí PowerShell

|Rutina|Popis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Získá jednu nebo více databází.|
|[Nové AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Vytvoří databázi sekundární pro stávající databázi a spustí replikace data.|
|[Nastavení AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Kombinace kláves vymění sekundární databáze, která bude primárního zahajte překlopení.|
|[Odebrat AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Ukončí replikace dat mezi databázi SQL a zadaná sekundární databáze.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Získá geo replikace propojení mezi databáze SQL Azure a skupina zdroje nebo SQL Server.|
||||

### <a name="rest-api"></a>ROZHRANÍ REST API

|ROZHRANÍ API|Popis|
|---|-----------|
|[Vytvoření nebo aktualizace databáze (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Vytvoří, aktualizace nebo obnovení primární a sekundární databáze.|
|[Získání vytvořit nebo aktualizovat stav databáze](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Vrátí stav během operace vytvoření.|
|[Nastavit sekundární databáze jako primární (plánované převzetí)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Zvýšení úrovně sekundární databáze Geo replikace partnerství osvobozením od nové primární databáze.|
|[Nastavit sekundární databáze jako primární (neplánované převzetí)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Vynucení přepojení sekundární databáze a nastavit sekundární jako primární.|
|[Získání odkazů replikace](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Získá všechny odkazy replikace pro danou databázi SQL geo replikace partnerství. Načte informace zobrazené sys.geo_replication_links katalogu.|
|[Získat odkaz replikace](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Získá konkrétní replikace odkaz pro danou databázi SQL geo replikace partnerství. Načte informace zobrazené sys.geo_replication_links katalogu.|
||||



## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md).
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md).
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md).
- Další informace o požadavky ověřování pro nový primární server a databázi, najdete v článku [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md).
