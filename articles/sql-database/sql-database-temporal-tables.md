<properties
   pageTitle="Začínáme s časový tabulek v databázi Azure SQL | Microsoft Azure"
   description="Zjistěte, jak začít s pomocí časový tabulek v databázi SQL Azure."
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
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Začínáme s časový tabulek v databázi Azure SQL

Časový tabulky jsou novou funkci programovacích databáze SQL Azure, která vám umožní sledovat a analyzovat celou historii změn ve vašich datech bez nutnosti vlastního kódu. Časový tabulek uchovávat data úzce související s čas kontextová tak, aby uložené skutečnosti můžete interpretováno jako platný pouze v určitém období. Tato vlastnost časový tabulky umožňuje efektivně založená na čase analýzy a dosáhnout toho, aby přehledy z dat vývoj.

##<a name="temporal-scenario"></a>Časový scénářů

Tento článek ukazuje kroky pro využití časový tabulek ve scénáři aplikace. Předpokládejme, že chcete sledovat činnosti uživatelů na novém webu vytvořené od začátku nebo na stávající web, který chcete rozšířit s analýzy činnosti uživatelů. V tomto příkladu zjednodušené jsme předpokládá, že počet navštívených webových stránek během určitého časového období je indikátor, musí být zaznamenávání a sledovat v databázi webu, který je hostovaný ve databáze SQL Azure. Cílem analýzu historie činnosti uživatelů je získat vstupů změně návrhu webu a poskytnout lepší pro návštěvníci.

Model databáze v tomto scénáři je velmi jednoduché – míru činnosti uživatelů představuje s polem jednotlivé celočíselné **PageVisited**zachycené společně s základních informací v profilu uživatele. Kromě toho pro analýzu čas založený, by uchováváte řadu řádků pro jednotlivé uživatele, kde každý řádek představuje počet stránek určitému uživateli navštívili za určité období počet minut.

![Schéma](./media/sql-database-temporal-tables/AzureTemporal1.png)

Naštěstí nepotřebujete umístit všechny plánování řízené úsilí aplikace udržovat tento informace o aktivitě. Časový tabulky se tento proces se automatický režim-pojmenování úplné pružnost při návrhům webů a delší dobu zaměřit na analýzu dat, vlastní. Jediné je potřeba udělat, je ověřit, jestli mají **WebSiteInfo** tabulky nakonfigurovaný jako [časový verzí systému](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Přesný postup pro využití časový tabulek v tomto scénáři jsou uvedeny níže.

##<a name="step-1-configure-tables-as-temporal"></a>Krok 1: Konfigurace tabulky jako dočasná

Podle toho, jestli jsou počáteční vývoj nebo upgradu existující aplikace bude vytvářet dočasná tabulky nebo upravit existující přidáním dočasná atributy. Obecně případu, nefunguje může být kombinaci následujících dvou možností. Proveďte tyto akce pomocí [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) SSDT () nebo jiných nástrojů vývoj jazyce Transact-SQL.


> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Vytvoření nové tabulky

Umožňuje položky kontextové nabídky "Novou verzí systému tabulku" v prohlížeči objektů SSMS otevřete editor dotazů se skriptem šablony dočasná tabulka a potom pomocí "Zadejte hodnoty pro parametry šablony" (Ctrl + Shift + M) k naplnění šablony:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

V SSDT vybrali šablonu "Časový tabulky (systém verzí)" při přidávání nové položky do databáze project. Který bude otevřete Návrhář tabulky a umožňují snadno určit rozložení tabulky:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Můžete také vytvořit tabulku časový zadáním příslušným jazyce Transact-SQL přímo, jak je vidět v následujícím příkladu. Všimněte si, že povinné elementy každé časový tabulky definici období a klauzule SYSTEM_VERSIONING s odkazem na jiné uživatele tabulky, které se uloží historickými řádku verze:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Při vytváření verzí systému časový tabulky se automaticky vytvoří doprovodné tabulce historie s nastavením výchozích. Výchozí historie tabulka obsahuje skupinový index B strom ve sloupcích období (účelu začátek) s stránky komprese povolena. Toto nastavení je optimální pro většinu scénáře, ve kterých se používají časový tabulek, zejména u [dat auditování](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

V tomto případě jsme cíl analýza založená na čase trendu přes delší historie dat a sadami dat zvětšit tak, aby volba úložiště k tabulce historie skupinový columnstore index. Skupinový columnstore poskytuje vynikajícím komprese a výkon u analytického dotazů. Časový tabulek pružnost konfigurovat indexy na tabulky aktuální a časový úplně nezávisle na sobě. 

**Poznámka**: Columnstore indexy jsou dostupné jenom v vrstvy služeb premium.

Tento skript ukazuje, jak lze změnit výchozí index v tabulce historie na skupinový columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Časový tabulky představují v prohlížeči objektů ikonou specifické pro snazší identifikaci během své tabulce historie se zobrazí jako podřízeného uzlu.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Příkaz ALTER existující tabulky časový

Pojďme vysvětlovat alternativní scénáři, ve kterém WebsiteUserInfo tabulce už existuje, ale nebyla navržené tak, aby historii změn. V takovém případě můžete jednoduše rozšířit existující tabulky osvobozením časový, jak je vidět v následujícím příkladu:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Krok 2: Spuštění svého pracovního vytížení pravidelně

Hlavní výhodou dočasná tabulky je to, že nepotřebujete, pokud chcete změnit nebo upravit svůj web žádným způsobem k provedení sledování změn. Po vytvoření dočasná tabulek pokaždé, když provést úpravy svých dat transparentně přetrvávají předchozí verze řádku. 

Abyste mohli využít sledování situaci, zejména automatických změn, Pojďme jenom aktualizovat sloupec **PagesVisited** pokaždé, když uživatel po ukončení za své relace na webu:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Je důležité si všimněte si, že aktualizační dotaz není potřeba vědět přesný čas výskytu aktuální operace ani způsob zachová historických dat pro další analýzu. Obě aspekty jsou automaticky uskutečněných jednotlivými databáze SQL Azure. Následující obrázek znázorňuje, jak je při každé aktualizaci generování data historie.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Krok 3: Analýza historických dat

Teď Jestliže je povolena Správa verzí časový systému, analýza historickými dat je jediným dotazu od sebe. V tomto článku se nabízíme několik příkladů tuto adresu analýzy scénářů - zjistěte si všechny podrobnosti, prozkoumejte zavádí se klauzule [SYSTEM_TIME pro](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) různé možnosti.

Pokud chcete zobrazit horní 10 uživatelů seřazené podle počtu navštívených webových stránek roce za hodinu před, spusťte tento dotaz:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Můžete snadno upravit tento dotaz analyzovat návštěvy webu k den před měsícem nebo v minulosti kdykoli chcete.

Abyste mohli provést základní statistické analýzy na předchozí den, použijte v následujícím příkladu:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Vyhledat aktivity určitým uživatelem v době, použijte obsažené v klauzuli:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafické vizualizace je zejména pohodlný časový dotazů, jak můžete zobrazit trendy a použití vzorce v intuitivní způsobem velmi snadno:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Dlouhodobým schématu tabulky

Obvykle se potřebujete změnit schématu dočasná tabulka při práci vývoj aplikací. K těmto spusťte běžná ALTER TABLE příkazy a databáze SQL Azure řádně podporovat rozšíří změny v tabulce historie. Tento skript ukazuje, jak můžete přidat další atribut pro účely sledování:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Když je aktivní vaše pracovní zátěž podobně můžete změnit definice sloupce:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Nakonec můžete odebrat sloupce, které už nepotřebujete.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Můžete taky pomocí nejnovější [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) můžete změnit schématu dočasná tabulka se připojíte k databázi (online režimu) nebo jako součást projektu databáze (offline režimu).

##<a name="controlling-retention-of-historical-data"></a>Řízení uchovávání informací historických dat

Verzí systému časový tabulky, se může tabulce historie zvýšit běžná tabulkami velikost databáze více než. Tabulky velkých a někdy Kvetoucí historie, může být problém obě kvůli čistého úložiště náklady stejně jako uložení výkonu daň z časový dotazu. Vývoj zásady uchovávání informací pro správu dat v tabulce historie proto, je důležitým aspektem plánování a správa životního cyklu každé časový tabulky. Pomocí databáze SQL Azure máte následujících přístupů pro správu historických dat v tabulce časový:

- [Vytvoření oddílů tabulky](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Vlastní vyčištění skriptu](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Další kroky

Podrobné informace o časový tabulky najdete v článku [si přečtěte následující dokumentaci MSDN](https://msdn.microsoft.com/library/dn935015.aspx).
Navštivte Channel 9 poslouchat [reálnou zákazníka časový implementace úspěch textu](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a podívejte se na [live časový ukázku](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
