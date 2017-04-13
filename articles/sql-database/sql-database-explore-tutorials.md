<properties
   pageTitle="Prozkoumání výukové programy pro databáze Azure SQL | Microsoft Azure"
   description="Další informace o databáze SQL funkcím a možnostem"
   keywords=""
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Prozkoumání výukové programy pro databáze Azure SQL

Těchto odkazů můžete přejít na přehled jednotlivé oblasti uvedených funkcí a jednoduché krok zahájení kurzu pro každou oblast. Omezené řešení Snadné spuštění, které demonstrují použití databáze SQL v úplné řešení založené na reálné situace najdete v článku [Azure SQL řešení Snadné spuštění databáze](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Použití SQL Server Management Studio

V těchto kurzech se dozvíte o používání SQL Server Management Studio a spravovat dotazy databáze SQL Azure.


> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Kurz  | Popis  |
|---|---|---|
| [Připojení k databázi SQL Azure pomocí přihlášení hlavní úrovni serveru](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| V tomto kurzu se naučíte, jak se připojit k databázi SQL Azure pomocí přihlášení hlavní úrovni serveru.|
| [Připojení k databázi SQL Azure jako uživatel](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | V tomto kurzu se naučíte připojení k databázi Azure SQL pomocí úrovni databáze uživatelského účtu.|
||||

## <a name="elastic-pools"></a>Pružná fondů

V těchto kurzech se dozvíte o používání [pružná fondů](sql-database-elastic-pool.md) ke správě cíle výkonu k více databázím, které mají široce odlišnými a neočekávané vzorce použití.

| Kurz  | Popis  |
|---|---|---|
| [Vytvoření fondu pružná](sql-database-elastic-pool-create-portal.md) | V tomto kurzu se naučíte, jak vytvořit scalable fondu databáze Azure SQL. |
| [Sledování pružná databáze](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | V tomto kurzu se naučíte, jak sledovat jednotlivé pružná databáze pro potenciální problémy. |
| [Přidání upozornění do fondu zdrojů](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | V tomto kurzu se naučíte, jak přidat pravidla na zdroje, které odesílají e-maily lidi nebo upozornění řetězců pro koncové body URL při využití prahovou hodnotu, která nastavíte narazí zdroje. |
| [Přesunutí databáze do fondu pružná](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | V tomto kurzu se naučíte, jak se přesuňte do fondu pružná databáze. |
| [Přesunutí databáze mimo fond pružné](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | V tomto kurzu se naučíte, jak přesunout databáze z fondu pružná. |
| [Změna nastavení výkonu fond](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | V tomto kurzu se naučíte, jak upravit limity výkon a úložiště pro fond. |
||||

## <a name="elastic-database-jobs"></a>Pružná databáze projektů

V těchto kurzech se dozvíte o používání [úloh pružná databází](sql-database-elastic-jobs-overview.md).

| Kurz  | Popis  |
|---|---|---|
| [Začínáme s pružné databázové nástroje](sql-database-elastic-scale-get-started.md) | V tomto kurzu se naučíte, jak používat funkce pružná databázové nástroje pomocí jednoduchého sharded aplikace. |
| [Začínáme s pružné úlohy databáze SQL Azure](sql-database-elastic-jobs-getting-started.md)  | V tomto kurzu se naučíte, jak na tom, jak vytvářet a spravovat úkolů, které Správa skupiny související databází.  | 
||||

## <a name="elastic-queries"></a>Pružná dotazů

V těchto kurzech se dozvíte o tom, jak [pružná dotazů](sql-database-elastic-query-overview.md). 

| Kurz  | Popis  |
|---|---|---|
| [Dotazování napříč vodorovně rozdělené databáze (sharded))](sql-database-elastic-query-getting-started.md) | V tomto kurzu se naučíte, jak vytvářet sestavy z všechny databáze v vodorovně rozdělené databáze (sharded) pomocí [pružné dotazu](sql-database-elastic-query-overview.md) |
| [Dotazování napříč svisle rozdělené databáze)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | V tomto kurzu se naučíte, jak vytvářet sestavy z všechny databáze v svisle databáze pomocí [pružná dotazu](sql-database-elastic-query-overview.md) |
| [Migrace existující databáze do škálování](sql-database-elastic-convert-to-use-elastic-tools.md)| V tomto kurzu se naučíte vodorovně měřítko (shard) Azure SQL databáze. |
||||

## <a name="performance-optimization"></a>Optimalizace výkonu

V těchto kurzech se dozvíte o optimalizaci [výkonu z jedné databáze](sql-database-performance-guidance.md). Optimalizace výkonu více databázím, najdete v článku [pružná fondů](#elastic-pools).

| Kurz  | Popis  |
|---|---|---|
| [Změna úrovně služby osy a výkonu databáze](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | V tomto kurzu se naučíte, jak můžete změnit velikost nebo Neomezovat výkonu databázi Azure SQL pomocí služby úrovní. |
| [Přehled výkonu SQL databáze Advisor dotazu](sql-database-performance.md#performance-overview) | V tomto kurzu se naučíte, jak otevřít a použít SQL databáze Advisor dotazu výkonu Přehled.|
| [Doporučení pro optimální výkon Advisor databáze SQL](sql-database-advisor.md#viewing-recommendations) | V tomto kurzu se naučíte, jak zobrazit a použít doporučení pro optimální výkon Advisor databáze SQL. |
| [Kontrola horní procesoru jinými dotazů](sql-database-query-performance.md#review-top-cpu-consuming-queries)| V tomto kurzu se naučíte, jak používat SQL databáze Advisor dotazu výkonu přehled Chcete-li zobrazit nejvyšší procesoru jinými dotazů.|
| [Zobrazení podrobností jednotlivé dotazy](sql-database-query-performance.md#viewing-individual-query-details)| V tomto kurzu se naučíte, jak používat SQL databáze Advisor dotazu výkonu Přehled zobrazíte podrobnosti o výkonu jednotlivé dotazu.|
||||

## <a name="sql-database-migration-and-archive"></a>Migrace databáze SQL a archivace 

V těchto kurzech se dozvíte o [migraci existující databáze SQL serveru k databázi SQL Azure](sql-database-cloud-migrate.md).

| Kurz  | Popis  |
|---|---|---|
| [Zjišťování problémů s kompatibilitou pomocí SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | V tomto kurzu se naučíte, jak používat SQL Server Data Tools for Visual Studio zjistit kompatibilitu databáze SQL Azure. |
| [Řešení problémů s kompatibilitou pomocí SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | V tomto kurzu se naučíte, jak pomocí SQL Server Data Tools for Visual Studio opravit problémy s kompatibilitou databáze SQL Azure. |
| [Určení kompatibility databáze SQL pomocí SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | V tomto kurzu se naučíte, jak používat nástroj příkazového řádku SQLPackage.exe zjistit kompatibilitu databáze SQL Azure.|
| [Určení kompatibility databáze SQL pomocí SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |V tomto kurzu se naučíte, jak používat SQL Server Management Studio zjistit kompatibilitu databáze SQL Azure.|
| [Migrace databáze SQL serveru k SQL databázi pomocí nasazení databázi Průvodce databáze Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | V tomto kurzu se dozvíte, jak migrovat kompatibilní databáze SQL serveru k databázi SQL Azure pomocí databázi nasazení průvodce databáze Microsoft Azure SQL Server Management Studio.
| [Export do BACPAC soubor pomocí SSMS databáze SQL serveru](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | V tomto kurzu se dozvíte, jak exportovat do souboru BACPAC pomocí Průvodce aplikace exportovat osy dat v SQL Server Management Studio kompatibilní databáze SQL serveru.|
| [Export do BACPAC soubor pomocí SqlPackage databáze SQL serveru](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | V tomto kurzu se dozvíte, jak exportovat do souboru BACPAC pomocí nástroje příkazového řádku SQLPackage.exe kompatibilní databáze SQL serveru.|
| [Import souboru BACPAC do databáze SQL Azure pomocí SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | V tomto kurzu se dozvíte, jak importovat databáze do databáze SQL Azure ze souboru BACPAC pomocí Průvodce aplikace exportovat osy dat v SQL Server Management Studio. |
| [Import souboru BACPAC do databáze SQL Azure pomocí SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | V tomto kurzu se dozvíte, jak importovat databáze do databáze SQL Azure ze souboru BACPAC pomocí nástroje příkazového řádku SQLPackage. |
| [Import souboru BACPAC do databáze SQL Azure pomocí portálu Azure](sql-database-import.md) | V tomto kurzu se dozvíte, jak importovat databáze do databáze SQL Azure z BACPAC soubor, který je uložený v Azure objektů blob Azure portálu.|
| [Import souboru BACPAC do databáze SQL Azure pomocí prostředí PowerShell](sql-database-import-powershell.md) | V tomto kurzu se dozvíte, jak importovat databáze do databáze SQL Azure ze souboru BACPAC pomocí Powershellu.|
| [Archivace databáze Azure SQL pomocí portálu Azure](sql-database-export.md#export-your-database) | V tomto kurzu se naučíte, jak chcete archivovat databáze Azure SQL do souboru BACPAC pomocí portálu Azure. |
| [Archivace databázi Azure SQL pomocí prostředí PowerShell](sql-database-export-powershell.md) | V tomto kurzu se naučíte, jak chcete archivovat databáze Azure SQL do souboru BACPAC pomocí Powershellu. |
| [Zkopírujte databázi Azure SQL pomocí portálu Azure](sql-database-copy.md#copy-your-sql-database) | V tomto kurzu se naučíte, jak zkopírovat databáze Azure SQL pomocí portálu Azure. |
| [Zkopírujte databázi Azure SQL pomocí prostředí PowerShell](sql-database-copy-powershell#copy-your-sql-database) | V tomto kurzu se naučíte, jak zkopírovat databáze Azure SQL pomocí Powershellu. |
| [Zkopírujte databázi Azure SQL pomocí jazyce Transact-SQL](sql-database-copy-transact-sql.md#copy-your-sql-database) | V tomto kurzu se naučíte, jak zkopírovat databázi Azure SQL pomocí jazyce Transact-SQL. |
||||

##<a name="develop"></a>Můžete vyvíjet

V těchto kurzech se dozvíte o [Vývojové databáze SQL](sql-database-develop-overview.md) a použití [knihoven připojení](sql-database-libraries.md).

| Kurz  | Popis  |
|---|---|---|
| [Připojení k databázi SQL pomocí .NET (C#)](sql-database-develop-dotnet-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí C#. |
| [Připojení k databázi SQL pomocí jazyka Java](sql-database-develop-java-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí jazyka Java. |
| [Připojení k databázi SQL pomocí Node.js](sql-database-develop-nodejs-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí Node.js. |
| [Připojení k databázi SQL pomocí PHP](sql-database-develop-php-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí PHP. |
| [Připojení k databázi SQL pomocí Python](sql-database-develop-python-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí Python. |
| [Připojení k databázi SQL pomocí skutečné](sql-database-develop-ruby-simple.md) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí skutečné. |
||||
 
## <a name="database-access"></a>Přístup k databázi

V těchto kurzech se dozvíte o [vytváření a Správa uživatelů a přihlášení](sql-database-manage-logins.md).

| Kurz  | Popis  |
|---|---|---|
| [Vytvoření pravidla serveru Brána firewall databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)  | V tomto kurzu se naučíte, jak nakonfigurovat bránu firewall úrovni serveru databáze SQL Azure portálu.  |
| [Vytvořit pravidlo databáze brána firewall na jazyce Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | V tomto kurzu se naučíte vytvořit pravidlo databáze brána firewall na jazyce Transact-SQL.|
| [Správa pravidel brány firewall úrovni serveru pomocí jazyce Transact-SQL](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | V tomto kurzu se dozvíte, jak spravovat připojení databáze SQL Azure brána úrovni serveru pomocí jazyce Transact-SQL.|
| [Správa pravidel brány firewall úrovni serveru pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | V tomto kurzu se dozvíte, jak spravovat připojení databáze SQL Azure brána úrovni serveru pomocí Powershellu.|
| [Správa pravidel brány firewall úrovni serveru pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | V tomto kurzu se naučíte spravovat připojení databáze SQL Azure brána úrovni serveru pomocí rozhraní API obnovit.|
| [Připojení k databázi SQL Azure pomocí přihlášení hlavní úrovni serveru](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| V tomto kurzu se naučíte, jak se připojit k databázi SQL Azure pomocí přihlášení hlavní úrovni serveru.|
| [Udělení přístupu k databázi k přihlášení] (sql-database-manage-logins.md#granting-database-access-to-a-login() | V tomto kurzu se naučíte, jak chcete udělit přístup k databázi na úrovni serveru přihlášení.|
| [Vytvoření nové databáze uživatelem, který používá SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) | V tomto kurzu se naučíte, jak vytvořit nového uživatele databáze v existující databázi pomocí SSMS.|
| [Udělení oprávnění db_owner uživatele nové databáze](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | V tomto kurzu se naučíte, jak udělit oprávnění db_owner existující databáze.|
| [Připojení k databázi SQL Azure jako uživatel](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | V tomto kurzu se naučíte, jak se připojit k databázi Azure SQL pomocí úrovni databáze uživatelský účet.|
||||


## <a name="data-security"></a>Zabezpečení dat.

V těchto kurzech se dozvíte o [zabezpečení dat z databáze SQL Azure](sql-database-security.md). 

| Kurz  | Popis  |
|---|---|---|
| [Povolení zjišťování hrozbou, že databáze pomocí portálu Azure](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | V tomto kurzu se naučíte, jak nastavit zjišťování hrozbou, že na portálu Azure databáze.|
| [Zabezpečené citlivá data uisng vždy zašifrovaných](sql-database-always-encrypted-azure-key-vault.md) | V tomto kurzu použijete průvodce vždy zašifrovaných zajistit citlivá data v databázi Azure SQL.|
| [Zabezpečené senstive dat pomocí šifrování průhledné dat](https://msdn.microsoft.com/library/dn948096.aspx)| V tomto kurzu se naučíte, jak používat průhledné šifrování zabezpečit senstive data.|
| [Šifrování sloupec dat](https://msdn.microsoft.com/library/ms179331.aspx)| V tomto kurzu se naučíte, jak k šifrování sloupec dat pomocí jazyce Transact-SQL.|
| [Nastavení maskování dynamických dat](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | V tomto kurzu se naučíte, jak nastavit maskování dynamických dat databáze Azure SQL. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Nepřerušený provoz a škálování dotazu

V těchto kurzech se dozvíte o pomocí [Geo obnovení a aktivní Geo replikace](sql-database-business-continuity.md) reccover z chyby, nepřerušený a škálování dotazu.

| Kurz  | Popis  |
|---|---|---|
| [Obnovení databáze SQL Azure předchozí bod pomocí portálu Azure](sql-database-point-in-time-restore-portal.md)| V tomto kurzu se naučíte, jak obnovit předchozí v čase pomocí portálu Azure.|
| [Obnovení databáze SQL Azure předchozího bodu pomocí prostředí PowerShell](sql-database-point-in-time-restore-powershell.md) | V tomto kurzu se naučíte, jak obnovit předchozí v čase pomocí prostředí PowerShell|
| [Obnovení odstraněné databáze Azure SQL pomocí portálu Azure](sql-database-restore-deleted-database-portal.md) | V tomto kurzu se naučíte, jak obnovit odstraněné databáze pomocí portálu Azure.|
| [Obnovení odstraněné databáze Azure SQL pomocí Powershellu](sql-database-restore-deleted-database-powershell.md) | V tomto kurzu se naučíte, jak obnovit odstraněné databáze pomocí Powershellu.|
| [Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-portal.md)| V tomto kurzu se naučíte, jak nakonfigurovat aktivní Geo replikace pomocí portálu Azure.|
| [Konfigurace Geo replikace databáze SQL Azure pomocí prostředí PowerShell](sql-database-geo-replication-powershell.md)| V tomto kurzu se naučíte, jak nakonfigurovat aktivní Geo replikace pomocí Powershellu.|
| [Konfigurace Geo replikace databáze SQL Azure pomocí jazyce Transact-SQL](sql-database-geo-replication-transact-sql.md)| V tomto kurzu se naučíte, jak nakonfigurovat aktivní Geo replikace pomocí jazyce Transact-SQL.|
| [Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-failover-portal.md) | V tomto kurzu se naučíte, jak převezme replikovat geo sekundární otevřené, pomocí portálu Azure.|
| [Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí prostředí PowerShell](sql-database-geo-replication-failover-powershell.md) | V tomto kurzu se naučíte, jak převezme replikovat geo sekundární otevřené, pomocí Powershellu.|
| [Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí jazyce Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | V tomto kurzu se naučíte, jak převezme replikovat geo sekundární otevřené, pomocí jazyce Transact-SQL.|
||||

## <a name="data-sync"></a>Synchronizace dat

V tomto kurzu se dozvíte o [Synchronizovat Data](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Kurz  | Popis  |
|---|---|---| 
| [Začínáme se synchronizací dat Azure SQL (verze Preview)](sql-database-get-started-sql-data-sync.md)  | V tomto kurzu se naučíte základy synchronizace dat Azure SQL pomocí portálu klasické Azure. |
||||

## <a name="next-steps"></a>Další kroky

[Prozkoumání Azure SQL databáze řešení Snadné spuštění](sql-database-solution-quick-starts.md)
