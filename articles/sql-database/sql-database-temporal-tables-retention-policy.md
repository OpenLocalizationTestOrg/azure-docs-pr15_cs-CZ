<properties
   pageTitle="Správa historických dat v časový tabulek s zásady uchovávání informací | Microsoft Azure"
   description="Informace o použití zásad uchovávání informací časový k synchronizaci historickými dat ve skupinovém rámečku ovládací prvek."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Správa historických dat v časový tabulek s zásady uchovávání informací

Časový tabulky mohou zvýšit velikost databáze více než normální tabulek, zejména v případě, že uchovávat historických dat delší dobu. Zásady uchovávání informací pro historických dat, je důležitým aspektem plánování a správa životního cyklu každé dočasná tabulka. Časový tabulek v databázi SQL Azure jsou součástí mechanismus snadno použitelné uchovávání informací, který umožňuje provedení této úlohy.

Uchovávání informací časový historie může být nakonfigurováno na úrovni jednotlivých tabulek, což umožňuje uživatelům vytvářet flexibilní stárnutí zásady. Použití časový uchovávání informací je jednoduchá: vyžaduje pouze jeden parametr nastavovaná při změně schématu nebo vytvoření tabulky.

Po definování zásad uchovávání informací databáze SQL Azure začnou kontroly pravidelně, pokud je potřeba udělat historických řádky, které se dá použít vyčištění automatické dat. Identifikace odpovídajících řádků a jejich odebrání z tabulky historie transparentně, spadat do pozadí úkolů, které je naplánováno a spusťte systém. Podmínka věku pro řádky tabulky historie je zaškrtnuto políčko podle sloupce představující konci SYSTEM_TIME období. Pokud doba uchovávání informací, například je nastaveno šest měsíců, řádky tabulky použít vyčištění splňovat následující podmínky:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

V předchozím příkladu jsme předpokládá, že sloupec **ValidTo** odpovídá konci období SYSTEM_TIME.

##<a name="how-to-configure-retention-policy"></a>Jak nakonfigurovat zásady uchovávání informací?

Před konfigurace zásad uchovávání informací pro tabulku časový zkontrolujte nejdřív, jestli časový historických uchovávání informací je povolený *na úrovni databáze*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Databáze příznak **is_temporal_history_retention_enabled** je ve výchozím nastavení zapnuto, ale uživatelé mohou měnit příkazem Vlastnosti databáze. Automaticky se nastaví na vypnuto, pokud po [přejděte v době obnovení](sql-database-point-in-time-restore-portal.md) operaci. Povolit vyčištění časový historie uchovávání informací pro databázi, spusťte následující příkaz:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Uchovávání informací pro dočasná tabulky můžete nakonfigurovat, i když **is_temporal_history_retention_enabled** je vypnuto, pokud ale automatické vyčištění starých řádky nebudete spustil v takovém případě.


Zásady uchovávání informací při vytváření tabulky nakonfigurovaný zadáním hodnoty parametru HISTORY_RETENTION_PERIOD:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Databáze SQL Azure vám umožní určit období uchování pomocí různých časových jednotek: DNY, TÝDNY, měsíce a roky. Pokud HISTORY_RETENTION_PERIOD vynechán, předpokládá se NEKONEČNÉ uchovávání informací. Můžete také NEKONEČNÉ klíčové slovo explicitně.

V některých případech je vhodné konfigurace uchovávání informací po vytvoření tabulky nebo změnit dříve nakonfigurováno hodnotu. V takovém případě použijte příkaz ALTER TABLE:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Nastavení SYSTEM_VERSIONING vypněte *nezachová* hodnota období uchovávání informací. Nastavení SYSTEM_VERSIONING ON bez HISTORY_RETENTION_PERIOD výslovně zadaný výsledky v období placení NEKONEČNÉ uchovávání informací.

Chcete-li zobrazit aktuální stav zásady uchovávání informací, použijte následující dotaz, který propojuje dočasná uchovávání informací příznaku oprávnění na úrovni databáze s období uchování pro jednotlivé tabulky:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Jak se odstraní databáze SQL starý řádků?

Postup vyčištění závisí na index rozložení tabulce historie. Je důležité si všimněte si tuto *pouze historie tabulky s seskupený index (B stromu nebo columnstore) můžete mít zásady uchovávání informací konečných nakonfigurované*. Vytvoření úkolu pozadí provádět vymazání starých údajů pro všechny časový tabulky s období uchování omezené.
Vyčištění logiky pro index skupinový rowstore (B – strom) neodstraní starých řádek v menší bloky (maximálně 10 kB) minimalizace tlaku na protokolu databáze a podsystém vstupu a výstupu. I když vyčištění logiky využívá požadovaný B stromu index, pořadí odstraněné položky starší než období uchování nemůže být zaručena pevně řádků. Proto *Neprovádět všechny závislosti na pořadí vyčištění v aplikacích*.

Vyčištění úkolu pro skupinový columnstore odebere celý [řádek skupiny](https://msdn.microsoft.com/library/gg492088.aspx) najednou (obvykle obsahují 1 milion řádků každý), což je velmi efektivní, zejména po vygenerování historickými dat vysoké tempem.

![Skupinový columnstore uchovávání informací](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Komprese pracovníků dat a něco v ní vyčištění efektivně uchovávání informací skupinový columnstore index perfektní volbou scénářích Pokud váš pracovní zátěž rychle vygeneruje vysoké částky historických dat. Tímto způsobem je typické pro náročné [transakční zpracování úloh, které používají časový tabulky](https://msdn.microsoft.com/library/mt631669.aspx) pro sledování změn a auditování analýze trendů a požití IoT data.

##<a name="index-considerations"></a>Co byste měli zvážit indexu

Vyčištění úkolu pro tabulky s rowstore seskupený index vyžaduje index začínat sloupce odpovídající konci období SYSTEM_TIME. Pokud tyto index neexistuje, nebude moct konfigurace období uchování konečných:

*Zpráva 13765, 16 úroveň stavu 1 <br> </br> nastavení období uchování konečných nezdařilo na verzí systému dočasná tabulka "temporalstagetestdb.dbo.WebsiteUserInfo" tabulce historie "temporalstagetestdb.dbo.WebsiteUserInfoHistory" neobsahuje požadovaný skupinový index. Zvažte vytvoření skupinový columnstore nebo B stromu index od sloupce, který odpovídá konec SYSTEM_TIME období v tabulce historie.*

Je důležité si všimněte si, že má výchozí tabulku historie již vytvořené databáze SQL Azure skupinový index, která je kompatibilní se standardem pro zásady uchovávání informací. Pokud se pokusíte odstranit tento index v tabulce s období uchování konečných, operace nezdaří se tato chyba:

*Zpráva 13766, 16 úroveň stavu 1 <br> </br> nelze odstranit seskupený index "WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory", protože ho používá pro automatického vymazání starých data. Zvažte nastavení HISTORY_RETENTION_PERIOD neomezené na odpovídající verzí systému dočasná tabulka v případě potřeby vložte tento index.*

Vyčištění na skupinový columnstore index funguje optimálně vložili historických řádky vzestupně (uspořádaná před koncem období sloupce), který je vždy případ, kdy tabulce historie vyplněné výhradně tak, že mechanismus SYSTEM_VERSIONIOING. Pokud řádků v tabulce historie se seřazenými podle konci období ve sloupci (který může být případ, pokud migrovat stávající historickými data), je třeba znovu vytvořit index skupinový columnstore horní index rowstore B strom, který je správně objednali k dosažení optimálního výkonu.

Vyhněte se opětovné vytvoření skupinový columnstore index v tabulce historie tečkou konečných uchovávání informací, protože může změnit řazení ve skupině řádku přirozeným stanoví operaci systému správy verzí. Pokud budete muset znovu vytvořit index skupinový columnstore v tabulce historie, můžete to udělejte tak, že znovu vytvoříte nad kompatibilní se standardem index B stromu zachování řazení v rowgroups potřebné pro vyčištění běžná dat. Stejný přístup je třeba při vytváření dočasná tabulka s existující tabulky historie, která obsahuje skupinový indexu sloupce bez zaručené data objednávky:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Pokud období uchování konečných nakonfigurovaný k tabulce historie s skupinový columnstore index, nelze vytvořit další neseskupené B stromu indexy v této tabulce:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Pokus o spuštění nad údajů se nepovede se tato chyba:

*Zpráva 13772, 16 úroveň stavu 1 <br> </br> nelze vytvořit neseskupené index v tabulce časový historie "WebsiteUserInfoHistory" protože má období uchování konečných a skupinový columnstore index definované.*

##<a name="querying-tables-with-retention-policy"></a>Dotazování tabulky se zásady uchovávání informací

Všechny dotazy v tabulce časový automaticky odfiltrovat historických řádků odpovídajících zásady uchovávání informací konečných, aby nedocházelo k výsledkům neočekávaným a konzistentní, protože starých řádky odstraněním úkolu vyčištění *kdykoli v čase a libovolný popořádku*.

Následující obrázek znázorňuje plán dotazů jednoduchého dotazu:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Plán dotazu zahrnuje další filtrem na konci období ve sloupci (ValidTo) operátor "Skupinový Index skenování" v tabulce historie (zvýrazněným). Tento příklad předpokládá tohoto období uchování 1 měsíc byl nastaven na WebsiteUserInfo tabulky.

![Filtr dotazu uchovávání informací](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Ale pokud dotaz tabulky historie přímo, může se zobrazit řádky, které jsou starších než zadaný uchovávání informací období, ale bez složených výsledků dotazu opakující. Následující obrázek znázorňuje dotazu plán spuštění dotazu v tabulce historie bez dalších použitými filtry:

![Dotazování historie bez filtru uchovávání informací](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Není spolehnout obchodní logiky čtení tabulky historie za období uchování jako nekonzistentní nebo neočekávané výsledky se může zobrazit. Doporučujeme použít časový dotazy s pro SYSTEM_TIME klauzulí pro analýzu dat v tabulkách časový.

##<a name="point-in-time-restore-considerations"></a>Přejděte v době obnovení informace

Při vytváření nové databáze obnovením [existující databázi k určitému bodu v čase](sql-database-point-in-time-restore-portal.md)má časový uchovávání informací zakázané na úrovni databáze. (příznak**is_temporal_history_retention_enabled** nastavena na vypnuto). Tato funkce umožňuje zkoumat všechny řádky historických po obnovení, bez obav, že starých řádky odeberou se získat k vytvoření dotazu je. Můžete ji použijete k *Zkontrolovat historickými dat za období uchování nakonfigurované*.

Řekněme, že časový tabulka obsahuje období uchování jeden měsíc. Pokud databáze byla vytvořená v vrstvy Premium služeb, bude moct vytvořit kopii databáze se stav databáze nahoru na 35 dnů zpět v minulosti. Které efektivně umožní analýze historickými řádky, které jsou až 65 dny dotazem v tabulce historie přímo.

Pokud chcete aktivovat vyčištění časový uchovávání informací, spusťte následující příkaz Transact-SQL od místa v době obnovení:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Další kroky

Naučte se používat časový tabulek v aplikacích najdete v tématech [Začínáte pracovat s časový tabulek v databázi SQL Azure](sql-database-temporal-tables.md).

Navštivte Channel 9 poslouchat [reálnou zákazníka časový implementaci úspěch textu](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a podívejte se na [live časový ukázku](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Podrobné informace o časový tabulek přečtěte [si přečtěte následující dokumentaci MSDN](https://msdn.microsoft.com/library/dn935015.aspx).
