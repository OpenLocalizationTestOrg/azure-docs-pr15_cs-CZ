<properties
   pageTitle="Migrace: Datový sklad nástroj pro přenesení | Microsoft Azure"
   description="Migrace do SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Nástroj pro skladové přenesení dat (verze Preview)

> [AZURE.SELECTOR]
- [Stáhněte si nástroj pro migraci][]

Nástroj pro migraci sklad dat je nástroj určený k migraci schéma a data ze serveru SQL Server a databázi SQL Azure SQL Azure datový sklad. V průběhu migrace schématu nástroj automaticky mapuje schématu příslušným zdrojem do cíle. Po migraci schématu nástroje poskytuje možnost přesunout data se automaticky generované skripty.

Kromě schéma a data migrace tento nástroj poskytuje možnost generování sestav kompatibility, které obsahují souhrn kompatibilitou mezi instancemi cíl a zdroje, které brání zjednodušený migrace.

## <a name="get-started"></a>Začínáme
Jako předpoklady pro instalaci budete potřebovat nástroj BCP příkazového řádku spusťte migraci skripty a Office a zobrazte sestavu kompatibility. Po spuštění spustitelný soubor, který se stáhne se výzva k přijmout standardní EULA, aby se nainstaluje nástroj.

Kromě toho Utiliy migrace spustíte byste potřebovali tu sledovat oprávnění na databázi, které hledáte migrace: vytvoření databáze změnit libovolného nebo libovolného definice zobrazení.

### <a name="launching-the-tool-and-connecting"></a>Spuštění nástroje a připojení
Spusťte nástroj kliknutím na ikony na ploše, který se objeví příspěvek nainstalovat. Po otevření nástroje, zobrazí se výzva se stránkou počáteční připojení, kde si můžete zvolit zdrojové a cílové pro nástroje pro migraci. V současné době podporujeme SQL Server a databázi SQL Azure jako zdroje a SQL datový sklad jako cíle. Po výběru tento se bude požádán, aby připojit k serveru zdroj vyplňování v poli Název serveru a ověření a potom klikněte na "Připojit".

Po ověření, se zobrazí nástroje seznamu databází, které jsou k dispozici na serveru, který jste připojeni k. Migrace můžete začít výběrem databázi, která se mají migrovat a potom kliknete na "Migrace vybrané".

## <a name="migration-report"></a>Sestavy migrace
Výběr zkontrolovat kompatibilitu databáze v nástroji vygeneruje zprávu shrnutí všechny kompatibilitou objektů v databázi požadované migrace. Širší seznam některé funkce SQL serveru, který není k dispozici v SQL datový sklad můžete najít v naší [si přečtěte následující dokumentaci migrace][]. Po generování sestavy budete moct uložit a otevřete sestavu v aplikaci Excel.

Poznámka: při generování schématu migrace většina problémy identifikovaný jako "Objekt" upraví, aby okamžité migrace tato data. Zkontrolujte změny zajistit, že nechcete provést další úpravy před použitím schéma.

## <a name="migrate-schema"></a>Migrace schématu

Po připojení, výběr migrace schématu vygeneruje skriptu migrace schématu pro vybrané tabulky. Tento skript porty strukturu tabulky, mapy kompatibilní datové typy mají víc kompatibilní formuláře a vytvoří zabezpečovací přihlašovací údaje a schéma Pokud to je označen uživatele v nastavení migrace. Tento kód můžete kontrolovat cílových instanci systému SQL datový sklad, uloží do souboru, zkopírovali do schránky nebo dokonce upravovat v řádku před provedením další akce.  

Uvedeno výše, po migraci schématu revize migrace změn, které nástroj připíše zajistit tak, že plně chápete je.  

## <a name="migrate-data"></a>Migraci dat

Kliknutím na možnost migrovat dat si můžete vygenerovat BCP skripty, které bude přesunutí dat nejdřív textových souborů na serveru, a potom přímo do vaší SQL datový sklad. Doporučujeme, abyste tento proces pro přesunutí malých krocích dat a opakování nejsou předdefinované a pokud dojde ke ztrátě připojení k síti může docházet k chybám. Chcete spustit, bude musíte mít nainstalovaný nástroj BCP a schématu pro data musí již byly vytvořeny.

Po vyplnění použití výše uvedených parametrů potřebujete jednoduše klikněte na Spustit migraci a vygeneruje sadu dva balíčky do zadaného umístění. Spusťte exportovaného souboru Pokud chcete exportovat data ze zdroje migrace do textových souborů a spustit importovaného souboru, abyste mohli importovat data do SQL datový sklad.

## <a name="next-steps"></a>Další kroky
Teď jste poštovním některá data, najdete v článku jak se [dají][].

<!--Image references-->

<!--Article references-->
[přečtěte následující dokumentaci pro migraci]: sql-data-warehouse-overview-migrate.md
[Můžete vyvíjet]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Stáhněte si nástroj pro migraci]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
