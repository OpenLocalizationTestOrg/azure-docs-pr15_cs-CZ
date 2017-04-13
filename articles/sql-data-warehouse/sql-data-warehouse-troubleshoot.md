<properties
   pageTitle="Poradce při potížích s Azure SQL datový sklad | Microsoft Azure"
   description="Poradce při potížích s Azure SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Poradce při potížích datový sklad Azure SQL

Toto téma uvádí některé z nejčastěji používaných Poradce při potížích otázky, které jsme dozvíme od zákazníků.

## <a name="connecting"></a>Připojení

| Problém                              | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Přihlášení se nezdařilo pro uživatele "Přihlášení NT AUTHORITY\ANONYMOUS". (Microsoft SQL Server, chyba: 18456) | K této chybě dochází, když uživatel AAD pokus o připojení k databázi předlohy, ale nemáte uživatele předlohy.  Problém můžete vyřešit tento problém buď zadat datový sklad SQL se chcete připojit ke v čase připojení nebo přidejte uživatele do hlavní databáze.  Viz článek [Přehled zabezpečení][] další podrobnosti.|
|Server hlavní "uživatelské_jméno" není mít přístup k databázi "předlohy" v aktuálním kontextu zabezpečení. Uživatele výchozí databázi nelze otevřít. Přihlaste se nezdařila. Přihlášení se nezdařilo pro uživatele "Uživatelské_jméno". (Microsoft SQL Server, chyba: 916) | K této chybě dochází, když uživatel AAD pokus o připojení k databázi předlohy, ale nemáte uživatele předlohy.  Problém můžete vyřešit tento problém buď zadat datový sklad SQL se chcete připojit ke v čase připojení nebo přidejte uživatele do hlavní databáze.  Viz článek [Přehled zabezpečení][] další podrobnosti.|
| Chyba CTAIP                        | K této chybě může dojít při přihlášení byl vytvořen hlavní databáze SQL serveru, ale ne v databázi SQL datový sklad.  Pokud se zobrazí tato chyba, podívejte se na článek [Přehled zabezpečení][] .  Tento článek vysvětluje, jak vytvořit vytvořte přihlášení a uživatele o předlohy a vytvoření uživatele v databázi SQL datový sklad.|
| Blokována bránou Firewall                |Azure SQL databáze jsou chráněny server a databázi úrovně bránách firewall zajistit pouze známé IP adresy mít přístup k databázi. Tyto brány firewall jsou zabezpečené ve výchozím nastavení, což znamená, že je nutné výslovně povolit a IP adresu nebo rozsah adres, abyste se mohli připojit.  Konfigurace brány firewall pro přístup, postupujte podle kroků v [přístup brány firewall konfigurace serveru pro svoji IP klienta][] v [Provisioning pokyny][].|
| Nelze se připojit pomocí nástroje pro ovladač | SQL datový sklad doporučuje používat [SSMS][], [SSDT Visual Studio 2015][]nebo [sqlcmd][] k vytvoření dotazu vaše data. Podrobné informace o ovladače a připojení k SQL datový sklad podívejte se na články [ovladačů SQL Azure datový sklad][] a [připojit k datový sklad SQL Azure][] .|


## <a name="tools"></a>Nástroje

| Problém                              | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Průzkumník objektů Visual Studio chybí AAD uživatelů | Toto je známý problém.  Jako alternativu zobrazte uživatele [sys.database_principals][].  V tématu [ověření Azure SQL datový sklad][] Další informace o použití služby Azure Active Directory s SQL datový sklad.|

## <a name="performance"></a>Výkon

|  Problém                             | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Řešení potíží s výkonem dotazu  | Pokud se pokoušíte řešení problémů s konkrétní dotazu, začněte s [naučit, jak sledovat svoje dotazy][].|
| Plány a výkonu dotazu nízká často je výsledkem chybějící statistiky   | Nejběžnější příčiny špatné výkonu je chybějící statistik o tabulkách.  Zobrazit [statistiku tabulky Údržba] [ Statistics] podrobné informace o tom, jak vytvořit Statistika a proč jsou důležité pro výkonu.|
| Nízké souběžné / ve frontě dotazů   | Princip [správy pracovního vytížení][] je důležité, abyste mohli pochopit, jak zůstatek přidělování paměti s souběžné.|
| Jak implementovat doporučené postupy    | Nejlepší umístěte zahájíte další způsoby, jak zlepšit výkonu dotazu je článku [osvědčené postupy SQL datový sklad][] .|
| Jak zvýšit výkon při změně měřítka  | Někdy řešení ke zlepšení výkonu je jednoduše přidat že více výpočetního power do dotazů roztažením [Datový sklad SQL][].|
| Výkonu dotazu nízká důsledku index špatné kvality | Dotazy některých případech můžete zpomalení z důvodu [špatné columnstore index kvalitu][].  V tomto článku najdete další informace a jak se [znovu vytvořit indexy zlepšit kvalitu segment][].|

## <a name="system-management"></a>Správa systému

|  Problém                             | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Zpráva 40847: Nelze provést operaci, protože server překročí povolený jednotku databázové transakce kvóty 45000. | Snižte [DWU][] databáze aplikace, který se pokoušíte vytvořit nebo [žádost o zvýšení kvóty][].|
| Zkoumání využití místa    | Viz [Tabulka velikosti][] pochopit místa využívání systému.|
| Informace o správě tabulky          | V tématu [Přehled tabulky] [ Overview] článku Další informace o správě tabulkách.  Tento článek obsahuje odkazy na podrobnější témata jako [tabulky datových typů][Data types], [Distribuce tabulky][Distribute], [indexování tabulky][Index], [rozdělení tabulky][Partition], [statistiky tabulek Údržba] [ Statistics] a [dočasné tabulky][Temporary].|

## <a name="polybase"></a>Polybase

|  Problém                             | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Chyba UTF-8                        |  Aktuálně PolyBase podporuje pouze načítání datové soubory, které byly kódování UTF-8.  Pokyny k řešení problémů spojených toto omezení naleznete v tématu [práce kolem požadovaného PolyBase UTF-8][] .|
| Selhání načítání z důvodu velké řádky   | Podpora velké řádku momentálně není k dispozici pro Polybase.  To znamená, že pokud tabulka obsahuje VARCHAR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX), externí tabulky se nedají použít k načtení dat.  Zátěž velké řádky je v současné době podporuje jen pomocí Azure Data Factory (plus pár BCP), Azure toku analýzy, SSIS, BCP nebo třídy .NET SQLBulkCopy. PolyBase podpory pro velké řádky se přidají v budoucích verzích.|
| selhání načítání BCP tabulky s datovým typem MAX | Je známý problém, který vyžaduje, aby VARCHAR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX) umístit na konec tabulky v některých scénářích.  Zkuste maximální počet sloupců na konec tabulky.|

## <a name="differences-from-sql-database"></a>Rozdíly v databázi SQL

|  Problém                             | Rozlišení                                      |
| :----------------------------------| :---------------------------------------------- |
| Nepodporované funkce pro databáze SQL  | V tématu [nepodporované funkce tabulky][].|
| Nepodporované typy dat databáze SQL  | V tématu [nepodporovaných datových typů][].|
| DELETE a UPDATE omezení      | V tématu [Možnosti aktualizace][], [Odstranění řešení][] a [Pomocí CTAS informace k alternativním řešením nepodporované syntaxe aktualizovat a odstranit][].  |
| Příkaz Sloučit nepodporuje   | V tématu [SLOUČENÍ řešení][].|
| Uložená procedura omezení       | Zobrazit [uložené procedury omezení][] bychom si vysvětlit některé z omezení uložené procedury.|
| Funkce definované uživatelem nepodporují příkaz SELECT | Toto je aktuální omezení naše funkce definované uživatelem.  Syntaxe, která je podporujeme naleznete v tématu [Vytvoření funkce][] .   |

## <a name="next-steps"></a>Další kroky

Pokud jste se nepodařilo najít řešení výše problém, tady je několik dalších zdrojů, můžete zkusit.

- [Blogy]
- [Žádost o funkci]
- [Videa]
- [KOČKA týmu blogů]
- [Vytvoření požadavek podpory můžete]
- [Fórum MSDN]
- [Fórum komunity přetečení zásobníku]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Přehled zabezpečení]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Ovladače Azure SQL datový sklad]: ./sql-data-warehouse-connection-strings.md
[Připojení k Azure SQL datový sklad]: ./sql-data-warehouse-connect-overview.md
[Vytvoření požadavek podpory můžete]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Změna měřítka datový sklad SQL]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[požádat o zvýšení kvóty]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Zjistěte, jak sledovat svoje dotazy]: ./sql-data-warehouse-manage-monitor.md
[Pokyny pro zřízení]: ./sql-data-warehouse-get-started-provision.md
[Nastavení přístupu k serveru brány firewall pro klienta IP]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Doporučené postupy SQL datový sklad]: ./sql-data-warehouse-best-practices.md
[Velikosti tabulky]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nepodporované funkce tabulky]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nepodporované datové typy]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Zlepšení kvality špatné columnstore indexu]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Opětovné sestavení indexy zlepšit kvalitu segmentu]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Správa zátěží na projektu]: ./sql-data-warehouse-develop-concurrency.md
[Použití CTAS informace k alternativním řešením nepodporované syntaxe aktualizovat a odstranit]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[AKTUALIZACE řešení]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Odstranění řešení]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[SLOUČENÍ řešení]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Uložená procedura omezení]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Ověřování datový sklad Azure SQL]: ./sql-data-warehouse-authentication.md
[Alternativní řešení požadovaného PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[Sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[VYTVOŘENÍ (FUNKCE)]: https://msdn.microsoft.com/library/mt203952.aspx
[SqlCmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[KOČKA týmu blogů]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádost o funkci]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Fórum komunity přetečení zásobníku]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

