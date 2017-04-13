<properties
    pageTitle="Přehled dotazů pružná databáze Azure SQL databáze | Microsoft Azure"
    description="Základní informace o funkci pružná dotazu"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL databáze pružná databázi dotazu přehled (verze preview)

Funkce dotazu pružná databáze (ve verze preview) umožňuje spuštění dotazu jazyce Transact-SQL, který přesahuje více databází v databázi SQL Azure (SQLDB). Umožňuje provádět křížově databázových dotazů pro přístup k vzdálené tabulek a připojení nástrojů Microsoft a třetích stran (Excel PowerBI, Tableau, atd.) k vytvoření dotazu přes úrovní dat s více databází. Pomocí této funkce můžete rozšiřování dotazů úrovní velkých dat v SQL databázi a vizualizovat výsledky v sestavách business intelligence (BI).

## <a name="documentation"></a>Si přečtěte následující dokumentaci

* [Začínáme s více databázových dotazů](sql-database-elastic-query-getting-started-vertical.md)
* [Sestava v cloudu rozšířených databází](sql-database-elastic-query-getting-started.md)
* [Dotaz na sharded cloudu databáze (vodorovně oddíly)](sql-database-elastic-query-horizontal-partitioning.md)
* [Dotaz na cloud databáze s různými schématy (svisle oddíly)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_spustit \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Proč používat pružná dotazy?

**Databáze Azure SQL**

Dotaz na databáze Azure SQL úplně v T-SQL. Tato funkce umožňuje jen pro čtení dotazování na vzdálená databáze. To poskytuje možnost pro aktuální místní zákazníci SQL serveru k migraci aplikací pomocí jména tři a čtyř částečný nebo odkazovaného serveru do SQL databáze.

**Dostupné ve standardní vrstvě** Pružná dotazu je podporována u osy standardní výkonu kromě osy výkonu Premium. Naleznete v části na náhled omezení pod o omezeních výkonu pro nižších úrovní výkonu.

**Použít pro vzdálená databáze**

Pružná dotazů teď můžete řídit parametry SQL Vzdálená databáze pro spuštění.

**Spuštění uložené procedury**

Spuštění vzdáleného uložená procedura volání nebo pomocí funkce remote [sp\_spustit \_vzdálené](https://msdn.microsoft.com/library/mt703714).

**Flexibilitu**

Externí tabulky se pružná dotazu může teď označovat vzdálené tabulek s jiné schéma nebo název tabulky.

## <a name="elastic-database-query-scenarios"></a>Scénáře dotazu pružná databáze

Cílem je usnadnit dotazu scénáře, které více databázím přispívat řádků do jednoho výsledku celkové. Dotaz se může buď skládat uživatel nebo aplikace přímo nebo nepřímo prostřednictvím nástroje, které jsou připojené k databázi. To je užitečné při vytváření sestav pomocí komerční nástroje pro integraci BI nebo data, nebo všechny aplikace, které nelze změnit. Pomocí pružná dotazu můžete dotaz na několik databází používat známé prostředí připojení SQL serveru v nástrojích jako je Excel, PowerBI, Tableau nebo Cognos.
Pružná dotaz umožňuje snadný přístup k celé kolekci databáze pomocí dotazů vydán SQL Server Management Studio nebo Visual Studio a usnadňuje křížově databáze dotazování z Entity Framework nebo jiná ORM prostředí. Obrázek 1 zobrazuje situace existující aplikaci cloud (které využívá [knihovny klienta pružná databáze](sql-database-elastic-database-client-library.md)) je založena na osy rozšířených dat, kde pružná dotazu se používá pro vytváření sestav křížově databáze.

**Obrázek 1** Pružná databázového dotazu použít u datových diagramů s měřítky osy

![Pružná databázového dotazu použít u datových diagramů s měřítky osy][1]

Scénáře zákazníka pružná dotazu jsou charakteristické podle těchto topologií:

* **Svislá osa rozdělení – křížově databázových dotazů** (Topologie 1): data rozdělit svisle mezi řady databází ve vrstvě data. Různé sady tabulek jsou obvykle, umístěny na jiné databáze. Která znamená, že schéma různých na jiné databáze. Například jsou všechny tabulky zásob na jednu databázi a všechny související účetní tabulky ve druhém databázi. Běžné případy použití v tomto topologii vyžaduje k vytvoření dotazu přes nebo sestavíte sestavy mezi tabulkami v několika databází.
* **Vodorovné rozdělení – Sharding** (Topologie 2): Data rozdělen je vodorovně k distribuci řádky přes škálovanou se data osy. Tento přístup je u všech zúčastněných databází shodné schéma. Tento přístup je zkratka "sharding". Sharding nemůže být použita a spravovaných (1) pružná databázi pomocí nástroje knihovny nebo (2) vlastní sharding. Pružná dotaz používá k dotazu nebo kompilaci sestav v mnoha shards.

> [AZURE.NOTE] Pružná databázových dotazů je nejvhodnější pro občas podřízenosti scénáře, kde můžete provádět většinu zpracování u osy dat. Hodně sestav úloh nebo skladovými scénáře dat s více složitých dotazů taky zvažte [Datový sklad SQL Azure](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Pružná topologie dotazu databází

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologie 1: Svislé rozdělení – databáze Křížové dotazy

Chcete-li začít kódování najdete v článku [Začínáme s více databázových dotazů (svislá dělení)](sql-database-elastic-query-getting-started-vertical.md).

Pružná dotazu lze provádět data umístěná v databázi SQLDB k dispozici další SQLDB databáze. To umožňuje vytvářet dotazy z jedné databáze odkazující na tabulky v jiných Vzdálená databáze SQLDB. Cílem prvního kroku je k definování externího zdroje dat pro každou vzdálené databázi. Externí zdroj dat je definována v místní databázi, ze kterého chcete získat přístup k tabulkám nachází na vzdálené databázi. Je nutné vzdálené databázi žádné změny. Typické svislé rozdělení scénáře kde různých databázích používají různé schémata pružná dotazů lze provádět běžné případy použití například přístup k datům odkaz a dotazování křížově databáze.

**Referenční data**: 1 topologie slouží ke správě dat odkaz. Na obrázku dvěma tabulkami (T1 a T2) s daty odkaz zachovány ve vyhrazené databázi. Použití pružná dotazu, můžete teď vyvolat tabulek T1 a T2 vzdáleně z jiné databáze, jak je vidět na obrázku. Použití topologie 1, pokud odkaz tabulky jsou menší nebo vzdálené dotazů do tabulky odkaz mít výběrové predikáty.

**Obrázek 2** Svislý rozdělení – použití dat pružná odkaz do dotazu

![Svislý rozdělení – použití dat pružná odkaz do dotazu][3]

**Dotazování křížově databáze**: ohebné vyhledá případy použití povolit, které vyžadují dotazování napříč několika SQLDB databází. Obrázek 3 zobrazuje čtyři jiné databáze: CRM zásob, HR a produkty. Dotazy provedených v jednom z databáze potřebovat přístup do jedné nebo všech dalších databází. Použití pružná dotazu, můžete nakonfigurovat databáze pro tento případ spuštěním pár jednoduchých příkazů DDL jednotlivých hodnot čtyři databáze. Po tomto jednorázové konfiguraci je jednoduchá – stačí odkazující na místní tabulky v dotazech T SQL nebo nástrojích BI přístup ke vzdálené tabulky. Tento přístup je vhodné, pokud vzdálené dotazy nevrací velké výsledky.

**Obrázek 3** Svislý rozdělení – použití různých databází ohebné do dotazu

![Svislý rozdělení – použití různých databází ohebné do dotazu][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologie 2: Vodorovné rozdělení – sharding

Použití pružná dotazu k provedení podřízenosti úkoly přes sharded, tedy, vodorovně oddíly, osy dat vyžaduje služby [mapy shard pružná databáze](sql-database-elastic-scale-shard-map-management.md) představující databází osy dat. Obvykle se používá pouze jeden shard mapy v tomto scénáři a vyhrazené databáze pomocí pružná dotazů slouží jako vstupní bod pro vytváření sestav dotazů. Pouze vyhrazené databázi potřebuje přístup shard mapu. Obrázek 4 znázorňuje tento topologie a jeho konfiguraci s mapou databáze a shard pružná dotazu. Databáze ve vrstvě dat může být libovolné verzi databáze SQL Azure nebo edition. Další informace o knihovně klienta pružná databáze a vytváření shard map najdete v článku [Shard mapování správy](sql-database-elastic-scale-shard-map-management.md).

**Obrázek 4** Vodorovné rozdělení – pomocí pružná dotazu pro vytváření sestav přes úrovní sharded dat

![Vodorovné rozdělení – pomocí pružná dotazu pro vytváření sestav přes úrovní sharded dat][5]

> [AZURE.NOTE] Vyhrazené pružná databázi databáze dotazu musí být v12 databáze SQL databáze. Neplatí žádná omezení shards sami.

Chcete-li začít kódování najdete v článku [Začínáme s pružná databázových dotazů pro vodorovné rozdělení (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Provádění pružná databázových dotazů

V následujících částech jsou uvedeny kroky implementovat pružná dotazu svislých a vodorovných rozdělení scénářích. Také odkazovat na podrobnější si přečtěte následující dokumentaci pro různé rozdělení scénáře.

### <a name="vertical-partitioning---cross-database-queries"></a>Svislý rozdělení - databáze Křížové dotazy

Následující kroky konfigurace pružná databázových dotazů svislé rozdělení scénáře, které vyžadují přístup k tabulce uložené ve vzdáleném SQLDB databází na stejné schéma:

*    [Vytvořte hlavní klíč](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Vytvoření databáze omezené pověření](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Externí zdroj dat vytvořit/rozevírací](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **RDBMS**
*    [Externí tabulka vytvořit/rozevírací](https://msdn.microsoft.com/library/dn935021.aspx) tabulka

Po spuštění příkazů DDL, se dostanete vzdálené tabulky "tabulka" jako kdyby to byl místní tabulky. Databáze SQL Azure automaticky otevře připojení ke vzdálené databázi, zpracuje vaši žádost o vzdálená databáze a vrátí výsledek.
Další informace o kroky potřebné pro svislé rozdělení scénář najdete v [pružná dotazu pro svislé rozdělení](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Vodorovné rozdělení - sharding

Následující kroky konfigurace pružná databázových dotazů pro vodorovné rozdělení scénáře, které vyžadují přístup k sadě tabulky, které máte uložené na (obvykle) několik vzdálené SQLDB databáze:

*    [Vytvořte hlavní klíč](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Vytvoření databáze omezené pověření](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Vytvoření [mapy shard](sql-database-elastic-scale-shard-map-management.md) představující vaší osy dat pomocí klienta knihovny pružná databáze.   
*    [Externí zdroj dat vytvořit/rozevírací](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **SHARD_MAP_MANAGER**
*    [Externí tabulka vytvořit/rozevírací](https://msdn.microsoft.com/library/dn935021.aspx) tabulka

Po provedení těchto kroků, máte přístup k tabulce ve vodorovném směru rozdělený "tabulka" jako kdyby to byl místní tabulky. Databáze SQL Azure automaticky otevře více paralelní připojení ke vzdálené databáze tabulky fyzicky uloženými zpracovávat požadavky na vzdálená databáze a vrátí výsledek.
Další informace o kroky potřebné pro vodorovné rozdělení scénář najdete v [pružná dotazu pro vodorovné rozdělení](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>Dotazování T-SQL
Po definování zdroje externích dat a externí tabulkách můžete připojení k databázím, kde jste definovali externí tabulkách běžná připojovací řetězec SQL serveru. Můžete spustit příkazy T SQL přes externí tabulkách připojení s omezeními níže uvedených pokynů. Další informace a příklady T-SQL dotazů můžete najít v tématech si přečtěte následující dokumentaci pro [dělení vodorovné](sql-database-elastic-query-horizontal-partitioning.md) a [Svislé rozdělení](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Připojení pro nástroje
Běžná řetězce připojení SQL serveru můžete použít pro aplikace a nástroje pro integraci BI nebo datového připojení k databázím, které mají externí tabulky. Ujistěte se, že SQL Server je podporované jako zdroje dat pro nástroj. Po připojení v nápovědě k databázi pružná dotazu a externí tabulek z takové databáze úplně stejně, jako by se jakékoli jiné databáze SQL serveru, které připojíte k vaší nástrojem.

> [AZURE.IMPORTANT] Ověření pomocí služby Azure Active Directory s pružná dotazů není aktuálně podporován.

## <a name="cost"></a>Pole náklady

Pružná dotazu se zahrne do náklady databáze SQL Azure. Nezapomeňte, že podporovaných topologií kde Vzdálená databáze jsou dostupné v centru jiná data než koncový bod pružná dotazu, ale výstupní data z Vzdálená databáze bude účtovaná běžná [Azure sazby](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Náhled omezení
* Spuštěním první pružná dotazu může trvat několik minut u osy standardní výkonu. Tentokrát je nezbytné k načtení funkce pružná dotazu. výkon při načítání se vylepšuje s vyšší výkon úrovní.
* Skriptování externím zdrojům dat nebo externí tabulek z SSMS nebo SSDT není zatím podporované.
* Import nebo Export pro SQL databáze dosud nepodporuje externí zdroje dat a externí tabulky. Pokud budete potřebovat Import nebo Export, umístěte tyto objekty před exportem a pak je znovu vytvořte před importem.
* Pružná databázových dotazů aktuálně podporuje pouze jen pro čtení přístup k externím tabulek. Ale můžete plnou funkčnost T-SQL na databázi kde je definována v externí tabulce. To může být užitečné, například uchovávat dočasné výsledků, například, vyberte < column_list > do < local_table > nebo definovat uložené procedury v databázi pružná dotazů, které odkazují na externí tabulky.
* S výjimkou nvarchar(max) LOB typy nejsou podporovány v externí tabulce definice. Jako alternativu můžete vytvořit zobrazení vzdálené databázi, která přetypuje typ LOB do nvarchar(max), definování externí tabulce zobrazení místo základní tabulka a v dotazech jej převést zpátky na původní typ LOB.
* Sloupec statistiky přes externí tabulky aktuálně nepodporuje. Tabulkami Statistika jsou podporované, ale je potřeba vytvořit ručně.

## <a name="feedback"></a>Zpětné vazby
Zkontrolujte nasdílet názory na prostředí pružná dotazů s abychom Disqus níže, fóra MSDN nebo na Stackoverflow. Jsme zájem o všechny typy myslíte o službu (vady, hrubé okraje, mezery funkce).

## <a name="more-information"></a>Další informace

Další informace o databáze Křížové dotazy a svislé rozdělení scénáře najdete v následujících dokumentů:

* [Databáze Křížové dotazy a svislé rozdělení přehled](sql-database-elastic-query-vertical-partitioning.md)
* Vyzkoušejte naše podrobné kurzu, který plná konkrétní příklad spuštěné v minutách: [Začínáme s více databázových dotazů (svislá dělení)](sql-database-elastic-query-getting-started-vertical.md).


Další informace o vodorovné rozdělení sharding příklady použití neexistuje tady:

* [Vodorovné rozdělení a sharding – přehled](sql-database-elastic-query-horizontal-partitioning.md)
* Vyzkoušejte naše podrobné kurzu, který plná konkrétní příklad spuštěné v minutách: [Začínáme s pružná databázových dotazů pro vodorovné rozdělení (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
