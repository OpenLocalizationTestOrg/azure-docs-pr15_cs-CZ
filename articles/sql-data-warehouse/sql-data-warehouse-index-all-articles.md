<properties
    pageTitle="Všech témat služby SQL datový sklad | Microsoft Azure"
    description="Tabulka všech témat pro službu Azure s názvem datový sklad SQL, která jsou na http://azure.microsoft.com/documentation/articles/, název a popis."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Všech témat služby Azure SQL datový sklad

Toto téma obsahuje seznam všech téma, které slouží k použití přímo do služby **Datový sklad SQL** Azure. Hledat tuto webovou stránku klíčových slov pomocí **Kombinace kláves Ctrl + F**, najdete v tématech aktuální zájmu.




## <a name="new"></a>Nový

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 1 | [Zálohování SQL datový sklad](sql-data-warehouse-backups.md) | Informace o zálohování předdefinované databáze SQL datový sklad, které vám umožní obnovte Azure SQL Data Warehouse bod obnovení nebo jinou zeměpisnou oblast. |


## <a name="updated-articles-sql-data-warehouse"></a>Aktualizované články, datový sklad SQL

V této části jsou uvedeny články, které byly aktualizovány naposledy, kde aktualizace proběhla velká nebo významný. Pro každý aktualizovaný článek hrubé fragment přidané markdown text zobrazený. V rozsahu kalendářních dat **2016-08-22** **2016 10 05**aktualizovaly se tyto články.

| &nbsp; | Článek | Aktualizované text, fragment kódu | Aktualizace |
| --: | :-- | :-- | :-- |
| 2 | [Načtení dat z úložiště objektů blob Azure do SQL datový sklad (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Ke sledování bajtů a soubory vyberte r.command, s.request_id, r.status (různých input_name) jsou započteny jako číslo nbr_files, SUMA (s.bytes_processed) / 1024/1024 jako gb_processed z sys.dm_pdw_exec_requests r vnitřní spojení sys.dm_pdw_dms_external_work s na r.request_id = s.request_id WHERE r. Popisek = "CTAS: cso načíst. DimProduct "OR r. Popisek = "CTAS: cso načíst. FactOnlineSales' GROUP BY r.command s.request_id, r.status Order nbr_files desc, gb_processed desc;  | 2016 09 07 |
| 3 | [Obnovení SQL datový sklad](sql-data-warehouse-restore-database-overview.md) | **Můžete obnovit pozastaveném datový sklad?** Obnovení datový sklad pozastavené, budete muset nejdřív znovu režimu online. Po návrat do online režimu datový sklad máte sedmi dnů bodů obnovení můžete vybírat. **Obnovení do geo nadbytečné oblasti** Pokud používáte geo nadbytečné úložiště, můžete obnovit datový sklad párových datacentrem v jinou zeměpisnou oblast. Datový sklad obnovit z posledního denní zálohování. **Obnovení časové osy** Obnovením databáze do libovolného bodu obnovení během posledních sedmi dnů. Snímky spusťte každých čtyři až osm hodin a jsou dostupné pro sedmi dnů. Je-li snímek starší než 7 dnů, vypršením jeho platnosti a jeho obnovení bod už není dostupná. **Obnovení nákladů** Využití úložiště obnovená datový sklad fakturované sazbou Azure Premium úložiště. Zastavte ukazatel myši obnovená datový sklad vám bude účtovaná za úložiště sazbou Azure Premium úložiště. Má pozastavení tu výhodu, že nejste poplatků | 2016-09 – 29 |





## <a name="get-started"></a>Začínáme

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 4 | [Ověřování datový sklad Azure SQL](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) a SQL Server ověřování datový sklad SQL Azure. |
| 5 | [Doporučené postupy pro datový sklad SQL Azure](sql-data-warehouse-best-practices.md) | Doporučení a osvědčené postupy, které byste měli vědět při vyvíjet řešení pro datový sklad SQL Azure. Tyto vám umožní být úspěšný. |
| 6 | [Ovladače Azure SQL datový sklad](sql-data-warehouse-connection-strings.md) | Připojovací řetězec a ovladačů SQL datový sklad |
| 7 | [Připojení k Azure SQL datový sklad](sql-data-warehouse-connect-overview.md) | Jak najít serveru název a připojovací řetězec pro vaše k datový sklad SQL Azure |
| 8 | [Analýza dat pomocí výukové počítače Azure](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Pomocí Azure počítače učení a prediktivní počítače výukové modelu podle data uložená v Azure SQL datový sklad. |
| 9 | [Dotaz SQL Azure datový sklad (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Dotazování datový sklad SQL Azure pomocí nástroje příkazového řádku sqlcmd. |
| 10 | [Vytvoření databáze SQL datový sklad pomocí jazyce Transact-SQL TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Naučte se vytvářet datový sklad SQL Azure pomocí TSQL |
| 11 | [Jak vytvořit lístek podpora pro systém SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Jak vytvořit požadavek podpory můžete v Azure SQL datový sklad. |
| 12 | [Načtení dat s Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Zjistěte, jak pomocí načítání dat s Azure Data Factory |
| 13 | [Načtení dat s PolyBase v SQL datový sklad](sql-data-warehouse-get-started-load-with-polybase.md) | Zjistěte, co je PolyBase a jak se používá pro data skladování scénáře. |
| 14 | [Vytvořit datový sklad Azure SQL](sql-data-warehouse-get-started-provision.md) | Naučte se vytvářet datový sklad SQL Azure na portálu Azure |
| 15 | [Vytvořit datový sklad SQL pomocí prostředí PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Vytvořit datový sklad SQL pomocí prostředí PowerShell |
| 16 | [Vizualizace dat s Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Vizualizace SQL datový sklad dat s Power BI |
| 17 | [Dotaz Azure SQL datový sklad (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Dotaz SQL datový sklad s Visual Studia. |



## <a name="develop"></a>Můžete vyvíjet

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 18 | [Optimalizace transakce pro datový sklad SQL](sql-data-warehouse-develop-best-practices-transactions.md) | Doporučené postupy pro moduly na psát efektivní transakce aktualizace v Azure SQL datový sklad |
| 19 | [Souběžné a pracovní zátěž správy v SQL datový sklad](sql-data-warehouse-develop-concurrency.md) | Princip správy souběžné a pracovní zátěž v Azure SQL datový sklad k vývoji řešení. |
| 20 | [Vytvoření tabulky jako vyberte (CTAS) v SQL datový sklad](sql-data-warehouse-develop-ctas.md) | Tipy pro kódování s tabulkou vytvoření příkazu Select (CTAS) v Azure SQL datový sklad k vývoji řešení. |
| 21 | [Dynamické SQL v SQL datový sklad](sql-data-warehouse-develop-dynamic-sql.md) | Tipy pro použití dynamického příkazu SQL v Azure SQL datový sklad k vývoji řešení. |
| 22 | [Seskupit podle možnosti v SQL datový sklad](sql-data-warehouse-develop-group-by-options.md) | Tipy pro implementaci skupiny možností v Azure SQL datový sklad k vývoji řešení. |
| 23 | [Použít popisky na nástroj dotazy v SQL datový sklad](sql-data-warehouse-develop-label.md) | Tipy pro použití štítky, aby nástroje dotazy v Azure SQL datový sklad k vývoji řešení. |
| 24 | [Smyčky v SQL datový sklad](sql-data-warehouse-develop-loops.md) | Tipy pro smyčky jazyce Transact-SQL a nahrazení kurzory v Azure SQL datový sklad k vývoji řešení. |
| 25 | [V SQL Data Warehouse uložené procedury](sql-data-warehouse-develop-stored-procedures.md) | Tipy pro implementaci v Azure SQL Data Warehouse uložené procedury pro vývoj řešení. |
| 26 | [Transakce v SQL datový sklad](sql-data-warehouse-develop-transactions.md) | Tipy pro implementaci transakce v Azure SQL datový sklad k vývoji řešení. |
| 27 | [Uživatelem definované schémat v SQL datový sklad](sql-data-warehouse-develop-user-defined-schemas.md) | Tipy pro použití jazyce Transact-SQL schémat v SQL Azure datový sklad k vývoji řešení. |
| 28 | [Přiřazení proměnných v SQL datový sklad](sql-data-warehouse-develop-variable-assignment.md) | Tipy pro přiřazení proměnné jazyce Transact-SQL Azure SQL datový sklad k vývoji řešení. |
| 29 | [Zobrazení SQL datový sklad](sql-data-warehouse-develop-views.md) | Tipy pro použití zobrazení jazyce Transact-SQL v Azure SQL datový sklad k vývoji řešení. |
| 30 | [Navrhování a kódování technik pro datový sklad SQL](sql-data-warehouse-overview-develop.md) | Vývojové koncepty, navrhování, doporučení a kódování technik pro SQL datový sklad. |



## <a name="manage"></a>Správa

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 31 | [Správa výpočetního výkonu v Azure SQL datový sklad (přehled)](sql-data-warehouse-manage-compute-overview.md) | Výkon měřítko, funkce v Azure SQL datový sklad. Rozšiřování úpravou DWUs nebo pozastavit a obnovit výpočetním zdroje snížit náklady. |
| 32 | [Správa výpočetního výkonu v Azure SQL datový sklad (Azure portál)](sql-data-warehouse-manage-compute-portal.md) | Azure úkoly portálu pro správu výpočetního výkonu. Měřítko výpočet zdroje úpravou DWUs. Nebo pozastavení a obnovení výpočet nákladů na zdroje. |
| 33 | [Správa výpočetního výkonu v Azure SQL datový sklad (Powershellu)](sql-data-warehouse-manage-compute-powershell.md) | Úkoly Powershellu ke správě výpočetního výkonu. Měřítko výpočet zdroje úpravou DWUs. Nebo pozastavení a obnovení výpočet nákladů na zdroje. |
| 34 | [Správa výpočetního výkonu v Azure SQL datový sklad (REST)](sql-data-warehouse-manage-compute-rest-api.md) | Úkoly Powershellu ke správě výpočetního výkonu. Měřítko výpočet zdroje úpravou DWUs. Nebo pozastavení a obnovení výpočet nákladů na zdroje. |
| 35 | [Správa výpočetního výkonu v Azure SQL datový sklad (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Skalární funkce (T-SQL) úkoly škálování výkonu úpravou DWUs. Uložení náklady roztažením zpět v čase a pozvolným. |
| 36 | [Sledovat vaše pracovní zátěž pomocí DMVs](sql-data-warehouse-manage-monitor.md) | Zjistěte, jak sledovat vaše pracovní zátěž pomocí DMVs. |
| 37 | [Správa databází v Azure SQL datový sklad](sql-data-warehouse-overview-manage.md) | Základní informace o správě databáze SQL datový sklad. Obsahuje nástroje pro správu, DWUs a škálování výkon Poradce při potížích s výkonu dotazu, vytvoření zásad zabezpečení a obnovení databáze před poškozením dat nebo z místního výpadku. |
| 38 | [Sledování uživatele dotazů v Azure SQL datový sklad](sql-data-warehouse-overview-manage-user-queries.md) | Základní informace o co byste měli zvážit osvědčené postupy a úkoly pro sledování uživatele dotazů v Azure SQL datový sklad |
| 39 | [Obnovení SQL datový sklad](sql-data-warehouse-restore-database-overview.md) | Základní informace o možnostech obnovení databáze pro obnovení databáze v Azure SQL datový sklad. |
| 40 | [Obnovení Azure SQL datový sklad (portál)](sql-data-warehouse-restore-database-portal.md) | Azure portálu úkoly při obnovení datový sklad SQL Azure. |
| 41 | [Obnovení Azure SQL datový sklad (Powershellu)](sql-data-warehouse-restore-database-powershell.md) | Prostředí PowerShell úkoly při obnovení datový sklad SQL Azure. |
| 42 | [Obnovení Azure SQL datový sklad (REST API)](sql-data-warehouse-restore-database-rest-api.md) | Úkoly rozhraní REST API pro obnovení datový sklad SQL Azure. |



## <a name="tables-and-indexes"></a>Tabulky a indexy

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 43 | [Datové typy pro tabulky v SQL datový sklad](sql-data-warehouse-tables-data-types.md) | Video: Začínáme s datovými typy tabulek Azure SQL datový sklad. |
| 44 | [Distribuce tabulek v SQL datový sklad](sql-data-warehouse-tables-distribute.md) | Video: Začínáme s distribuce tabulek v Azure SQL datový sklad. |
| 45 | [Indexování tabulek v SQL datový sklad](sql-data-warehouse-tables-index.md) | Video: Začínáme s tabulkou indexování v Azure SQL datový sklad. |
| 46 | [Základní informace o tabulkách v SQL datový sklad](sql-data-warehouse-tables-overview.md) | Video: Začínáme s tabulkami sklad dat SQL Azure. |
| 47 | [Rozdělení tabulek v SQL datový sklad](sql-data-warehouse-tables-partition.md) | Video: Začínáme s v Azure SQL datový sklad vytvoření oddílů tabulky. |
| 48 | [Správa statistiky tabulek v SQL datový sklad](sql-data-warehouse-tables-statistics.md) | Video: Začínáme s statistiky tabulek v Azure SQL datový sklad. |
| 49 | [Dočasné tabulky v SQL datový sklad](sql-data-warehouse-tables-temporary.md) | Video: Začínáme s dočasnými tabulkami v Azure SQL datový sklad. |



## <a name="integrate"></a>Integrace

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 50 | [Pomocí datový sklad SQL Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) | Tipy pro použití Azure dat Factory (ADF) s Azure SQL datový sklad k vývoji řešení. |
| 51 | [Pomocí datový sklad SQL Azure počítače výukové](sql-data-warehouse-integrate-azure-machine-learning.md) | Kurz pro používání Azure počítače učení s Azure SQL datový sklad k vývoji řešení. |
| 52 | [Pomocí datový sklad SQL Azure toku analýzy](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tipy pro použití analýzy toku Azure s Azure SQL datový sklad k vývoji řešení. |
| 53 | [Použití Power BI s SQL datový sklad](sql-data-warehouse-integrate-power-bi.md) | Tipy pro používání Power BI s Azure SQL datový sklad k vývoji řešení. |
| 54 | [Využít jinými službami s SQL datový sklad](sql-data-warehouse-overview-integrate.md) | Nástroje a partnery řešení, která integrace s SQL datový sklad.  |



## <a name="load"></a>Načtení

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 55 | [Načtení dat z úložiště objektů blob Azure do Azure SQL datový sklad (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Zjistěte, jak pomocí načítání dat s Azure Data Factory |
| 56 | [Načtení dat z úložiště objektů blob Azure do SQL datový sklad (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Naučte se používat PolyBase k načtení dat z úložiště objektů blob Azure do SQL datový sklad. Načtení několik tabulek z ve veřejných datech do schématu Contoso maloobchodní datový sklad. |
| 57 | [Načtení dat ze serveru SQL Server do Azure SQL datový sklad (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Export dat z SQL serveru k plochému souborům, AZCopy k importu dat k úložišti objektů blob Azure a PolyBase jedí data do SQL Azure datový sklad používá bcp. |
| 58 | [Načtení dat ze serveru SQL Server do Azure SQL datový sklad (textových souborů)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Velikost malé dat používá bcp exportovat data ze serveru SQL Server textových souborů a data importovat přímo do datový sklad SQL Azure. |
| 59 | [Načtení dat ze serveru SQL Server do Azure datový sklad SSIS (SQL)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Se dozvíte, jak vytvořit balíček SQL Server Integration Services (SSIS) přesuňte data z nejrůznějších zdrojů dat do SQL datový sklad. |
| 60 | [Načtení dat s PolyBase v SQL datový sklad](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Export dat z SQL serveru k plochému souborům, AZCopy k importu dat k úložišti objektů blob Azure a PolyBase jedí data do SQL Azure datový sklad používá bcp. |
| 61 | [Příručka pro použití PolyBase v SQL datový sklad](sql-data-warehouse-load-polybase-guide.md) | Pokyny a doporučení pro použití PolyBase v SQL datový sklad scénáře. |
| 62 | [Načtení ukázkových dat do SQL datový sklad](sql-data-warehouse-load-sample-databases.md) | Načtení ukázkových dat do SQL datový sklad |
| 63 | [Načtení dat s bcp](sql-data-warehouse-load-with-bcp.md) | Zjistěte, jaké bcp je a jak se používá pro data skladování scénáře. |
| 64 | [Načtení dat do datový sklad SQL Azure](sql-data-warehouse-overview-load.md) | Další obvyklé scénáře pro data do SQL datový sklad načítání. Jedná se o pomocí PolyBase, úložišti objektů blob Azure, textových souborů a dodávky disku. Můžete taky pomocí nástrojů jiných výrobců. |



## <a name="migrate"></a>Migrace

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 65 | [Migrace kód SQL do SQL datový sklad](sql-data-warehouse-migrate-code.md) | Tipy pro migraci kód SQL Azure SQL datový sklad k vývoji řešení. |
| 66 | [Přenést Data](sql-data-warehouse-migrate-data.md) | Tipy pro migraci dat do Azure SQL datový sklad k vývoji řešení. |
| 67 | [Nástroj pro skladové přenesení dat (verze Preview)](sql-data-warehouse-migrate-migration-utility.md) | Migrace do SQL datový sklad. |
| 68 | [Migrace schématu do SQL datový sklad](sql-data-warehouse-migrate-schema.md) | Tipy pro migraci schématu Azure SQL datový sklad k vývoji řešení. |
| 69 | [Migrace řešení pro systém SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Migrace pokyny pro dosažení řešení Azure SQL datový sklad platformu. |



## <a name="partners"></a>Partneři

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 70 | [SQL datový sklad business intelligence partnery](sql-data-warehouse-partner-business-intelligence.md) | Seznam partnerů třetích stran business intelligence s řešení, které podporují SQL datový sklad. |
| 71 | [Partneři integrace dat SQL datový sklad](sql-data-warehouse-partner-data-integration.md) | Seznamy třetích stran partnery řešení integrace dat, které podporují datový sklad SQL Azure. |
| 72 | [SQL datový sklad dat správu partnerů](sql-data-warehouse-partner-data-management.md) | Seznam partnerů správy dat třetích stran s řešení, které podporují SQL datový sklad. |



## <a name="reference"></a>Odkaz

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 73 | [Odkazy na témata pro datový sklad SQL](sql-data-warehouse-overview-reference.md) | Obsahu odkazy pro SQL datový sklad. |
| 74 | [Rutiny prostředí PowerShell a REST API pro datový sklad SQL](sql-data-warehouse-reference-powershell-cmdlets.md) | Najděte horní rutiny prostředí PowerShell pro datový sklad SQL Azure včetně postup pozastavit databáze. |
| 75 | [Prvky jazyka](sql-data-warehouse-reference-tsql-language-elements.md) | Seznam odkazů na referenční obsah prvků jazyce Transact-SQL pro SQL datový sklad. |
| 76 | [Témata Transact-SQL](sql-data-warehouse-reference-tsql-statements.md) | Odkazy na referenční obsah v jazyce Transact-SQL tématu používaný datový sklad SQL. |
| 77 | [Systémová zobrazení](sql-data-warehouse-reference-tsql-system-views.md) | Odkazy na systém prohlíží obsahu pro SQL datový sklad. |



## <a name="security"></a>Zabezpečení

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 78 | [SQL datový sklad - klientů nižší úrovně podpory auditování a dynamických dat maskování](sql-data-warehouse-auditing-downlevel-clients.md) | Další informace o klientech SQL datový sklad podpora pro data auditování |
| 79 | [V Azure SQL datový sklad sestavy auditování](sql-data-warehouse-auditing-overview.md) | Začínáme s auditování v Azure SQL datový sklad |
| 80 | [Začínáme s průhledné dat šifrování (TDE) v SQL datový sklad](sql-data-warehouse-encryption-tde.md) | Šifrování průhledné dat (TDE) v SQL datový sklad |
| 81 | [Začínáme s šifrování průhledné dat (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Šifrování průhledné dat (TDE) v SQL datový sklad (T-SQL) |
| 82 | [Zabezpečení databáze aplikace SQL datový sklad](sql-data-warehouse-overview-manage-security.md) | Tipy pro zabezpečení databáze v Azure SQL datový sklad k vývoji řešení. |



## <a name="miscellaneous"></a>Různé

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 83 | [Instalace aplikace Visual Studio 2015 a SSDT SQL datový sklad](sql-data-warehouse-install-visual-studio.md) | Instalace aplikace Visual Studio a SQL Server vývojového nástroje SSDT () pro datový sklad Azure SQL |
| 84 | [Přechod na podrobnosti úložiště Premium](sql-data-warehouse-migrate-to-premium-storage.md) | Pokyny pro migraci stávajícího SQL datový sklad k základnímu úložišti premium |
| 85 | [Začínáme s hrozbou, že zjišťování](sql-data-warehouse-security-threat-detection.md) | Jak začít s hrozbou, že zjišťování |
| 86 | [Omezení SQL datový sklad kapacity](sql-data-warehouse-service-capacity-limits.md) | Maximální hodnoty u připojení, databází, tabulky nebo dotazy na SQL Data Warehouse. |
| 87 | [Poradce při potížích datový sklad Azure SQL](sql-data-warehouse-troubleshoot.md) | Poradce při potížích s Azure SQL datový sklad. |

