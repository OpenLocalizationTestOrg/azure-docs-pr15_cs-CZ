<properties
    pageTitle="Azure SQL databáze a výkonu pro jednu databáze | Microsoft Azure"
    description="V tomto článku můžete určit, které vrstvy služeb pro aplikace. Doporučuje také způsoby, jak optimalizovat aplikace k optimálnímu využití databáze SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL databáze a výkonu pro jednu databáze

Databáze SQL Azure nabízí tři [úrovně služby](sql-database-service-tiers.md): Basic, Standard a Premium. Jednotlivé vrstvy služeb sporná, i když izoluje prostředky, můžete použít databázi SQL a zaručuje předvídatelná výkonu pro danou úroveň služby. V tomto článku nabízíme pokyny, které vám můžou pomoct zvolte vrstvy služeb pro aplikaci. Také probereme tato způsoby vyladit aplikace k maximálnímu z databáze SQL Azure.

>[AZURE.NOTE] Tento článek se zaměřuje na výkon pokyny pro jeden databáze v databázi SQL Azure. Výkon související s fondů pružná databáze najdete v článku [aspektech cena a výkonu pro fondy pružná databáze](sql-database-elastic-pool-guidance.md). Osobní zprávu, ale, že je spoustu ladění doporučení v tomto článku vyrovnat databází ve fondu pružná databáze a získat podobné výhody výkonu.

Toto jsou tři úrovně služby databáze SQL Azure, které můžete vybírat z (měří se v databázi výkon jednotek nebo [DTUs](sql-database-what-is-a-dtu.md)výkon:

- **Základní**. Na služby základní osy nabídky dobrý výkon předvídatelnost na každou databázi, která je hodinu přes hodinu. V databázi základní dostatečné prostředky podpory dobrý výkon v malých databázi, která neobsahuje více souběžně prováděné žádosti.
- **Standardní**. Vrstvy standardní služeb nabízí Vylepšený výkon předvídatelnost a vyvolá panelu pro databáze, které mají víc souběžně prováděné žádosti, jako jsou pracovní skupiny a webových aplikací. Když zvolíte databázi standardní služba osy, změně velikosti databázové aplikace založené na předvídatelná výkonu minute přes minuty.
- **Premium**. Vrstvy služeb Premium poskytuje předvídatelná výkon druhý přes druhé na každou databázi Premium. Když zvolíte vrstvy služeb Premium, změně velikosti databázové aplikace založené na načíst Špička pro tuto databázi. Plán odebere případy, ve které výkonu odchylka mohou způsobit malé dotazů trvat déle než obvykle latence řetězcových operací. Tento model výrazně zjednodušuje ověření cyklů vývoj a produktu pro aplikace, které potřebujete zdůraznit silných údajů o potřeb zdrojů ve špičce, výkon odchylka nebo latence dotazu.

U každé vrstvy služeb nastavit úroveň výkonu tak, abyste měli možnost platí pouze pro kapacitu, které potřebujete. Můžete [Upravit kapacitu](sql-database-scale-up.md), nahoru nebo dolů, jako úlohu změny. Například pokud váš pracovní zátěž databáze nastavená dostatečně vysoká nákupní období zpátky do školy, může zvýšit úroveň výkonu pro databázi pro nastavovat čas červenec prostřednictvím dne. Můžete zmenšit ho má na konci období ve špičce. Můžete minimalizovat platíte optimalizace cloudu prostředí pro hodnotu sezónnosti vašim potřebám. Tento model také vhodný pro software cyklů verze produktu. Test tým může přidělit kapacitu při testování se spustí a potom uvolněte této funkci po dokončení testování. V žádosti o modelu kapacita platíte kapacity potřebný a vyhněte se výdaje na vyhrazené prostředky, které můžete použít jen zřídka.

## <a name="why-service-tiers"></a>Proč služby úrovní?

I když každé pracovní zátěž databáze se může lišit, účelem – úrovně služeb je byli účastníci výkonu na různých úrovních výkonu. Zákazníkům s požadavky na zdroj ve velkém měřítku databáze, můžete pracovat v více vyhrazené výpočetního prostředí.

### <a name="common-service-tier-use-cases"></a>Případy použití běžné vrstvy služeb

#### <a name="basic"></a>Základní

- **Právě začínáte s databáze SQL Azure**. Aplikace, které jsou ve vývoji často, nemusíte vysoký výkon úrovně. Základní databáze jsou ideální pro vývoj databází okamžiku minimální cena.
- **Máte databáze pomocí jednoho uživatele**. Aplikace, které jednoho uživatele přidružit databáze obvykle nemají vysoké požadavky souběžné a výkonu. Tyto aplikace jsou kandidáty pro služby základní osy.

#### <a name="standard"></a>Standardní

- **Databáze obsahuje více souběžně prováděné žádosti**. Aplikace, které služby více uživatelů najednou obvykle třeba vyšší výkon úrovně. Weby, které dostanete střední přenosy nebo oddělení aplikace, které vyžadují další zdroje jsou například správnými kandidáty pro vrstvy standardní služeb.

#### <a name="premium"></a>Premium

Premium služby osy použití většinou nastala některá z těchto vlastností:

- **Vysoký Špička načíst**. Aplikaci, která vyžaduje spoustu procesoru, paměti nebo vstupní/výstupu dokončete jeho operace vyžaduje úroveň vyhrazené, vysoký výkon. Databázová operace známá pro používání několika jádra procesoru delší dobu je například jako kandidát osy služby Premium.
- **Mnoho souběžně prováděné žádosti**. Některé databáze aplikace mnoho souběžně prováděné žádosti o služby například pokud podávání web, který má vysoké objemem. Základní a standardní – úrovně služeb omezení počtu souběžně prováděné žádosti na jeden databáze. Aplikace, které vyžadují víc připojení musel výběr velikosti odpovídající rezervace zpracovávání maximální počet potřebné požadavků.
- **Nízké latence**. Některé aplikace nutné zajistit odpověď z databáze minimální včas. Pokud konkrétní uložená procedura nazývá jako součást operace širší zákazníka, bude pravděpodobně nutné povinné mít výnosu z této volání v milisekundách maximálně 20, 99 % času. Tento typ aplikace využívá vrstvy služeb Premium, abyste měli jistotu, neexistuje požadované výpočetní výkon.

Úroveň služby, které potřebujete pro databázi SQL závisí na Špička zatížení požadavky pro každou dimenzi zdroje. Některé aplikace používat důvodu částka jednoho zdroje, ale mají významné požadavky pro ostatní zdroje.

## <a name="service-tier-capabilities-and-limits"></a>Možnosti osy služby a omezení
Každou úroveň osy a výkonu služeb souvisí s různých limity a ukazatele výkonu. Tato tabulka popisuje tyto vlastnosti pro jednu databázi.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

V následujících částech jsou další informace o tom, jak zobrazit související tato omezení.

### <a name="maximum-in-memory-oltp-storage"></a>Maximální OLTP v paměti úložiště

Zobrazení **sys.dm_db_resource_stats** slouží ke sledování použití Azure v paměti úložiště. Další informace o sledování najdete v článku [OLTP monitoru v paměti úložiště](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Azure v paměti online zpracování (OLTP) Náhled transakcí je podporována pouze pro jeden databáze. Nemůže ho používat v databázích do skupin pružná databáze.

### <a name="maximum-concurrent-requests"></a>Maximální souběžně prováděné žádosti

Pokud chcete zobrazit počet souběžně prováděné žádosti, spusťte tento dotaz jazyce Transact-SQL na databázi SQL:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Analyzovat pracovní zátěž místní databáze SQL serveru, upravte tento dotaz chcete filtrovat podle určité databázi, že kterou chcete analyzovat. Pokud máte místní databázi s názvem databáze, tento jazyce Transact-SQL dotaz vrací počet souběžně prováděné žádosti z takové databáze:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Toto je právě snímek na jednom místě v čase. Získat lepší znalost pracovního vytížení a souběžně prováděné žádosti o požadavky, musíte shromáždit mnoho ukázky určitou dobu.

### <a name="maximum-concurrent-logins"></a>Maximální souběžné přihlášení

Můžete analyzovat uživatele a aplikace průběhem určitou představu četnost přihlášení. Můžete taky spustit skutečný zatížení v testovacím prostředí, abyste měli jistotu, že nejsou zasažení to nebo jiných omezení probereme tato témata v tomto článku. Není k dispozici jednoho dotazu nebo Správa dynamických zobrazení (DMV), které se zobrazí, souběžné, že spočítá přihlášení nebo historie.

Používáte-li více klientů stejný připojovací řetězec, službu ověří každý přihlášení. Pokud 10 uživatelů současně připojení k databázi pomocí stejné uživatelské jméno a heslo, bude 10 souběžné přihlášení. Toto omezení platí jenom pro doby trvání ověřování a přihlášení. Pokud 10 uživatelům připojení k databázi postupně, počet souběžné přihlášení nikdy bude větší než 1.

>[AZURE.NOTE] V současné době toto omezení, nebudou mít vliv na databáze ve fondů pružné databáze.

### <a name="maximum-sessions"></a>Maximální počet relací

Můžete zobrazit počet aktuální aktivní relace, spusťte tento dotaz jazyce Transact-SQL na databázi SQL:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Pokud při analýze pracovní zátěž místního serveru SQL Server, upravte dotaz zaměřit se na konkrétní databáze. Tento dotaz vám pomůže určit možné relace úrazovém databázi chcete-li instalovat jeho přesunutí k databázi SQL Azure.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Znovu tyto dotazy vrátit počet v daném okamžiku. Budete-li shromažďovat více ukázek určitou dobu, budete mít nejlepší znalost relaci použít.

Analýza SQL databáze můžete získat historických statistiky na relace. Dotaz **sys.resource_stats**a použijte sloupec **active_session_count** . Naleznete v části Další informace o použití tohoto zobrazení.

## <a name="monitor-resource-use"></a>Sledování využití prostředků
Dvě zobrazení vám mohou pomoci sledování využití prostředků pro databázi SQL vzhledem k jeho vrstvy služeb:

- [Sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>Sys.dm_db_resource_stats
V každé SQL databáze můžete použít zobrazení [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) . **Sys.dm_db_resource_stats** zobrazení: Zobrazí posledních prostředků zdroj dat relativní vrstvy služeb. Průměrná procentuální hodnotu pro využití procesoru, data vstupu a výstupu, zápisy protokolu a paměti zaznamenány každých 15 sekund a zachovány pro 1 hodinu.

Protože toto zobrazení nabízí odstupňovaný pohled na využití prostředků, použijte **sys.dm_db_resource_stats** první pro analýzu aktuálního stavu nebo Poradce při potížích. Tento dotaz příkladem využití prostředků průměr a maximální pro aktuální databázi přes poslední hodinu:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Další dotazy najdete v článku příklady v [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>Sys.resource_stats

Zobrazení [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) v **hlavní** databázi má další informace, které vám můžou pomoct sledovat výkon databázi SQL úrovni jeho konkrétní službu osy a výkonu. Jsou data shromážděná každých 5 minut a trvá asi 35 dnů. Tohle zobrazení je užitečné pro analýzu dlouhodobé historie databázi SQL použití zdroje.

Následující diagram ukazuje procesoru využití prostředků pro danou databázi Premium úrovní výkonu P2 pro každou hodinu v týdnu. Tento graf začíná v pondělí zobrazuje 5 pracovní dny a potom se zobrazí víkendu, když mnohem méně se stane s aplikace.

![Využití prostředků databáze SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Z dat, má tato databáze právě zatížení procesoru Špička jenom víc než 50 procent využití procesoru relativní úroveň výkonu P2 (poledne úterý). Pokud je procesoru dominantní faktor v profilu zdrojů aplikace, pak se můžete rozhodnout, že je P2 úroveň správné výkonu zaručit, že pracovní zátěž vždy vešel. Pokud se myslíte, že aplikace postupem času zvětšovat, je vhodné mít vyrovnávací paměť dalších zdrojů, aby aplikace není vůbec dosáhla limit úroveň výkonu. Je-li zvýšit úroveň výkonu, můžete se tím vyhnout zákazníka viditelné chyby, které může dojít, pokud databázi nemá dost power zpracovávat požadavky efektivně, zejména v prostředí latence citlivé. Příklad je databáze, která podporuje aplikace, která vykreslí webových stránek na základě výsledků volání databáze.

Všimněte si, že jiné typy aplikací může interpretovat stejného grafu jinak. Například pokud aplikace se snaží zpracuje mzdových dat každý den a obsahuje jednom grafu, tohoto typu "dávku" modelu může udělat správně výkonu úrovni P1. Úroveň výkonu P1 má 100 DTUs ve srovnání s 200 DTUs na úrovni výkonu P2. Úroveň výkonu P1 poskytuje polovinu výkon úroveň P2 výkonu. Ano 50 procento využití procesoru v P2 rovná využití procesoru 100 % rovná v P1. Pokud není aplikace vypršení časového limitu, může se stát užitečné, když úlohy trvá 2 hodin nebo 2,5 hodin dokončete, pokud jej získá zpracovat dnes. Aplikace v této kategorii pravděpodobně můžete použít úroveň P1 výkonu. Můžete využít výhod skutečnosti, že jsou dobu během dne, kdy je využití prostředků malá písmena, tak, aby všechny "velký ve špičce" může přepadového do jedné žlaby později. Úroveň výkonu P1 může být vhodné pro tento typ aplikace (a uložit peníze), dokud úlohy ho mohl mít hotový dokončen včas každý den.

Zpřístupňuje databáze SQL Azure spotřebované množství informace o zdroji na každou databázi aktivní v zobrazení **sys.resource_stats** **hlavní** databází v každém serveru. Data v tabulce je agregované intervalů 5 minut. Data s úrovněmi služby základní, Standard a Premium, může trvat víc než 5 minut zobrazit v tabulce, aby tato data jsou zvýšíte jeho přínos pro analýzu historie a ne v reálném čase analýzy. Zobrazení **sys.resource_stats** zobrazíte historii nedávných databáze a k ověření, zda rezervace, které jste se rozhodli Doručená výkon, která že se má v případě potřeby dotazu.

>[AZURE.NOTE] Musíte být připojeni k databázi **předlohy** logické databáze SQL serveru k vytvoření dotazu **sys.resource_stats** v následujících příkladech.

Tento příklad ukazuje, jak se zobrazí data v tomto zobrazení:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Zobrazení sys.resource_stats katalogu](./media/sql-database-performance-guidance/sys_resource_stats.png)

Následující příklad ukazuje různé způsoby, které můžete použít zobrazení katalogu **sys.resource_stats** získat informace o používání zdrojů v databázi SQL:

1. Hledání v uplynulém týdnu zdroje určenou userdb1 databáze, můžete spustit tento dotaz:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Pokud chcete zjistit, jak dobře vaše pracovní zátěž vešel úroveň výkonu, budete muset přecházet na podrobnější každý aspekt metriky zdroje: procesoru, čtení, zápisy, počet zaměstnanců a počet relací. Tady je upravený dotaz pomocí **sys.resource_stats** vykazování průměr a maximální hodnoty tyto metriky zdroje:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Tyto informace o průměr a maximální hodnoty metriky jednotlivé zdroje můžete posuďte, chod vaše pracovní zátěž zapadá do úroveň výkonu, které jste se rozhodli. Obvykle nadprůměrných hodnot z **sys.resource_stats** umožňují dobré směrný plán můžete před Cílová velikost. Je třeba primární měření zařízení. Příklad používáte vrstvy standardní služeb k úrovni S2 výkonu. Průměr určenou procesoru a vstupu a výstupu čtení procent zápisy jsou nižší než 40 procent, průměrný počet zaměstnanců je nižší než 50 a průměrný počet relací je nižší než 200. Vaše pracovní zátěž může zapadají do celkového konceptu úroveň S1 výkonu. Není těžké si podívejte, jestli databázi vešel v rámci pracovníka a relace. Zobrazíte, zda databáze zapadá do nižší výkon pokud jde o využití procesoru, bude číst a zápisy, dělení čísla DTU nižší výkon DTU počtem aktuální úroveň výkonu a potom vynásobte výsledek hodnotou 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Výsledek je relativní výkonu rozdíl mezi dvěma výkonu úrovní v procentech. Pokud využití prostředků nepřekročila tuto hodnotu, může vaše pracovní zátěž zapadají do celkového konceptu nižší výkon. Ale budete muset podívejte se na všechny rozsahy hodnot používání zdrojů a zjistit tak, že procento, jak často má pracovní zátěž databáze zapadají do celkového konceptu nižší výkon. Následující dotaz výstupy přizpůsobením procento dimenze zdroje založené na mezní 40 procent vypočtené v tomto příkladu:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Podle úrovně cíle vaší databáze služby (SLO), můžete se rozhodnout, jestli vaše pracovní zátěž zapadá do nižší výkon. Pokud vaše pracovní zátěž databáze SLO je 99,9 procent a předchozí dotaz vrátí hodnoty větší než 99,9 procent pro všechny tři zdroje dimenze, vaše pracovní zátěž pravděpodobně se vejde do nižší výkon.

    Prohlížíte přizpůsobením procento taky vám přehled o tom, jestli budete muset přesunout na vyšší úrovni vyšší výkon odpovídá vaší SLO. Například userdb1 ukazuje následující využití procesoru pro minulého týdne:

  	| Průměrná procento využití procesoru | Maximální procento využití procesoru |
  	|---|---|
  	| 24.5 | 100,00 |

    Průměr CPU je o automatickém omezení úroveň výkonu, který bude vyhovovat úroveň výkonu databáze. Ale maximální hodnota zobrazuje databázi dosažení mezní úroveň výkonu. Potřebujete přejdete na vyšší úrovni vyšší výkon? Budete muset podívejte se na postup opakovaně pokoušeli pracovní zátěž dosáhne ze 100 % a porovnejte je databáze vytížení SLO.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Pokud tento dotaz vrátí chybovou hodnotu menší než 99,9 procenta z některého z rozměry tři zdroje píšete buď na vyšší úrovni vyšší výkon nebo pomocí optimalizace pro aplikace snížit zatížení databáze SQL.

4. Tento cvičení také byly použity plánované pracovní zátěž zvýšení v budoucnu.

## <a name="tune-your-application"></a>Optimalizace aplikace

V tradiční místního SQL serveru je proces plánování počáteční kapacity často oddělit od spuštění aplikace v výrobní proces. Hardwarová a product licence jsou zakoupené nejdřív a ladění výkonu probíhá později. Pokud používáte databázi SQL Azure, je vhodné interweave proces spuštění aplikace a ladění ho. S modelem plateb kapacity na vyžádání můžete doladit aplikace pomocí minimální prostředky potřebné teď místo overprovisioning na hardware podle pokusů plánů budoucí růst aplikace, které často nejsou správné. Někteří uživatelé můžou rozhodnete ladění aplikace a místo toho zvolit možnost overprovision prostředcích. Tento postup může být vhodné, pokud nechcete změnit klíčové aplikaci zaneprázdněné období. Ale optimalizace aplikaci můžete minimalizovat požadavků na zdroje a nižší měsíční faktury při používání služeb úrovní v databázi SQL Azure.

### <a name="application-characteristics"></a>Vlastnosti aplikace

Přestože databáze SQL Azure služby úrovní slouží ke zlepšení výkonu stability a předvídatelnost aplikace, můžete některé osvědčené postupy ladění aplikace lépe využít zdroje na úrovni výkonu. Existovat mnoho aplikací výrazné zvýšení výkonu jednoduše přepnutím do vyšší úrovně výkonu nebo vrstvy služeb některé aplikace je zapotřebí další výhody do vyšší úrovně služby. Zvýšení výkonu zvažte optimalizace pro další aplikace pro aplikace, které mají tyto vlastnosti:

- **Aplikace, které mají snížení výkonu z důvodu "chatty" chování**. Chatty aplikace provést nadbytečné dat aplikace access operace, které jsou závislé na latence sítě. Možná budete muset změnit tyto typy aplikací chcete-li zmenšit počet operací dat přístup k databázi SQL. Například může zlepšení výkonu aplikace pomocí techniky jako dávky ad-hoc dotazů nebo přesunutí dotazy pro uložené procedury. Další informace najdete v tématu [dávku dotazů](#batch-queries).
- **Databáze se náročné pracovní zátěž, která nepodporuje celý jednoho počítače**. Databáze, které překročení zdroje nejvyšší úroveň výkonu Premium můžou využívat rozšiřování pracovní zátěž. Další informace najdete v tématu [sharding křížově databáze](#cross-database-sharding) a [funkční rozdělení](#functional-partitioning).
- **Aplikace, které máte nepřesnými dotazy**. Aplikace, zejména v vrstvy přístupu k datům, která špatně správně zapnuli dotazů nemusí využít do vyšší úrovně výkonu. Jedná se o dotazy, které nemají klauzuli WHERE, chybí indexy nebo zastaralé statistiky. Tyto aplikace využívat standard query ladění výkonu postupy. Další informace najdete v tématu [chybějící indexy](#missing-indexes) a [dotaz optimalizace a psaní](#query-tuning-and-hinting).
- **Aplikace, které obsahují nepřesnými data přístup k návrhu**. Aplikace, které mají inherentní přístup souběžné problémům s daty, třeba vzájemné zablokování, nemusí vztahovat do vyšší úrovně výkonu. Zmenšete výměny zpráv v databázi SQL Azure pomocí ukládání dat na straně klienta se služby Azure ukládání do mezipaměti nebo jiné technologii ukládání do mezipaměti. V tématu [ukládání do mezipaměti aplikace osy](#application-tier-caching).

## <a name="tuning-techniques"></a>Optimalizace postupy
V tomto oddílu se podíváme na způsoby, pomocí kterých můžete doladit databáze SQL Azure získat optimálního výkonu aplikace a spuštění na nejnižší úrovni možný výkon. Některé z těchto postupů podle tradiční SQL Server optimalizace doporučené postupy, ale ostatní jsou specifické pro databázi SQL Azure. V některých případech můžete zkoumat spotřebované materiály pro danou databázi zobrazíte oblasti k další ladění a rozšíření tradiční SQL Server technik pro práci v databázi SQL Azure.

### <a name="azure-portal-tools"></a>Azure portálu nástrojů
Dva nástroje na portálu Azure, které vám můžou pomoct analyzovat a vyřešit problémy s výkonem se databázi SQL najdete:

- [Přehled výkonu dotazu](sql-database-query-performance.md)
- [Konference Advisor databáze SQL](sql-database-advisor.md)

Portál Azure obsahuje další informace o obou těchto nástrojích a jejich použití. Efektivní Diagnostika a oprava problémů, doporučujeme Nejdřív zkuste nástroje na portálu Azure. Doporučujeme použít ruční optimalizace postupů, které pak probereme chybějících indexy a ladění dotazu v speciálních případů.

### <a name="missing-indexes"></a>Chybějící indexy
Běžné potíže v výkon databáze OLTP se vztahuje k návrhu fyzické databáze. Schémata databáze jsou často navržený a vyřízeny testováním ve velkém měřítku (nebo v zatížení objemu dat). Bohužel plnění plánu dotazu může přijatelná na malé měřítku ale značně snížit v části objemů dat úroveň výroby. Nejběžnější zdroj tohoto problému je chybějící odpovídající indexy uspokojili filtry a dalšími omezeními v dotazu. Často chybějící manifesty indexy jako tabulku kontroloval může postačovat indexu vyhledávání.

V tomto příkladu je definována plán vybraný dotaz prohledávání při bude postačovat hledání:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Plán dotazu s chybějící indexy](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Databáze SQL Azure vám pomůže najít a opravit běžné chybějící indexovat podmínky. Podívejte se na kompilace dotazu, ve kterých by indexu značně snížit odhad nákladů ke spuštění dotazu DMVs, které jsou integrované v databázi SQL Azure. Při provádění dotazu databáze SQL sleduje intervalu každý plán dotazu se spustí a sleduje odhadovaná mezera mezi plán spouštění dotazů a imagined kde existoval tento index. Můžete tyto DMVs rychle uhodnout, které změny návrhu fyzické databáze může zlepšit celkové náklady pracovní zátěž pro databázi a jeho skutečnou zátěží na projektu.

Tento dotaz můžete použít k vyhodnocení potenciální chybějící indexy:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

V tomto příkladu dotaz výsledkem této návrhu:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Po vytvoření otevřela, že stejný příkaz SELECT vyskladnění na jiný plán, který používá seek místo prohledávání a provede plánu mnohem efektivněji:

![Plán dotazu s opraveným indexy](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Klíčové přehled je omezenější než vyhrazeného serverového počítače vstupu a výstupu objemu sdílené, komoditou systému. Minimalizace nepotřebných vstupu a výstupu do maximálně využít výhod možností systému v DTU každou úroveň výkonu úrovní služby Azure SQL databáze je premium. Odpovídající pole fyzicky databázi volby můžete výrazně zlepšit zpoždění pro jednotlivé dotazy, zlepšit výkon souběžně prováděné žádosti zpracování jednotkové měřítko a minimalizovat náklady potřebných k tomu dotaz. Další informace o chybějící index DMVs najdete v tématu [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Optimalizace dotazu a psaní
Optimalizace dotazu v databázi SQL Azure je podobný tradiční optimalizace dotazů SQL serveru. Většina doporučené postupy pro optimalizaci dotazů a principy důvody, proč omezení modelu pro nástroj pro optimalizaci dotazu také použít k databázi SQL Azure. Pokud doladit dotazů v databázi SQL Azure, můžete získat další výhody snížení požadavky agregační zdroje. Aplikace pak můžou spustit za méně než naladěného ekvivalent, protože ho mohlo by umožnit spuštění nižší úrovně výkonu.

Jako příklad, který není ve společné SQL Server a který platí také k databázi SQL Azure jak optimalizace dotazů "sniffs" parametry. Během kompilace tento nástroj pro optimalizaci dotazu vyhodnotí aktuální hodnotu parametru a zjistit, zda může vygenerovat více optimální plán dotazů. I když tento strategie často může vést k plánu dotazu, který je mnohem vyšší než plán kompilovaný bez hodnoty známé parametrů, aktuálně funguje imperfectly obě v SQL Server a databázi SQL Azure. Někdy není podvržený parametr a někdy podvržený parametr ale vygenerovaných plánu je nepřesnými pro celou sadu hodnoty parametrů v úlohu. Microsoft obsahuje pokyny k dotazu (směrnice), takže můžete zadat záměr více záměrně a přepíše výchozí chování sledování toku dat parametr. Často Pokud používáte tipů, můžete opravit případy, ve kterých je výchozí chování databázi SQL Azure SQL Server nebo nedokonalé pro určitého zákazníka úlohu.

Následující příklad ukazuje, jak zpracování dotazů můžete vygenerovat plán, který je nepřesnými výkonu a požadavků na zdroje. V tomto příkladě se zobrazí také, že používáte tip dotazu, můžete zmenšit spuštění dotazu čas a požadavků na zdroje pro databázi SQL:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Kód nastavení vytvoří tabulku, která obsahuje zkosená rozdělení data. Plán optimální dotazů se liší podle které zablokováno. Bohužel plán ukládání do mezipaměti chování nemá vždy překompilujte dotaz založený na nejběžnější hodnotu parametru. Ano je možné pro plán nepřesnými do mezipaměti a použít k více hodnot, i když na jiný plán může být lepší volbou plán průměrně. Vytvoří plán dotazů dvou uložené procedury, které jsou stejné, s tím rozdílem, že jeden tip zvláštní dotazu.

**Příklad, část 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Příklad, část 2**

(Doporučujeme počkejte aspoň 10 minut, než začnete část 2 v příkladu tak, aby výsledky se liší v výsledné telemetrickými daty.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Každou část v tomto příkladě se pokusí spustit parametry vložení údajů 1 000 časy (Generovat dostatečné zatížení používat datové sady test). Jakmile je spuštěn uložené procedury, zpracování dotazů kontroluje hodnota parametru, které je do postup během jeho první kompilace (parametr "sledování toku dat"). Procesor ukládá výslednou plán a používá pro pozdější vyvolání, i když hodnota parametru se liší. Ve všech případech nemusí použit optimální plán. Někdy potřebujete průvodce tento nástroj pro optimalizaci vyberte plán, který je lepší Průměrná velikost písmen, nikoli zvláštní případ ze kdy nejdřív kompilovaný dotaz. V tomto příkladu generuje původní plán "prohledávání" plán, který bude číst všechny řádky zobrazíte každou hodnotu, která odpovídá parametr:

![Dotaz optimalizace pomocí plán naskenovaný obrázek](./media/sql-database-performance-guidance/query_tuning_1.png)

Protože jsme postup spouštět pomocí hodnota 1, výsledný plán optimální pro hodnotu 1 ale byl nepřesnými pro všechny ostatní hodnoty v tabulce. Výsledek pravděpodobně vám nevyhovuje by kdybyste každý plán vyberte náhodně, protože plánu provádí pomaleji a používá více zdrojů.

Pokud spustíte test s `SET STATISTICS IO` nastavena na `ON`, logické prohledávání v tomto příkladu práci na pozadí. Uvidíte, že jsou 1,148 čtení provedenou plán (což je neefektivní, zda je průměrná případ vrátíte jen jeden řádek):

![Optimalizace pomocí logické prohledávání dotazu](./media/sql-database-performance-guidance/query_tuning_2.png)

Druhá část příkladu používá tip dotazu zjistit tento nástroj pro optimalizaci během procesu kompilace používat určité hodnoty. V tomto případě vynutí zpracování dotazů Ignorovat hodnoty, které je jako parametr, a místo aby stylově zapadly `UNKNOWN`. Odkazuje na hodnotu, která obsahuje průměr četnosti v tabulce (ignoruje zkosení). Na základě hledání plán, který je rychlejší a průměrně než plánu využívá méně zdrojů, v části 1 v tomto příkladu je výsledné plán:

![Optimalizace dotazu pomocí tip dotazu](./media/sql-database-performance-guidance/query_tuning_3.png)

Si můžete prohlédnout výsledek v tabulce **sys.resource_stats** (je prodleva od doby, že spustíte test a kdy naplní data v tabulce). Tento příklad, část 1 spouštět během časového intervalu 22:25:00 a část 2 spouštět v 22:35:00. Všimněte si, že starší časového intervalu používá více zdrojů v této časového intervalu než pozdější (z důvodu zvýšení efektivity plán).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Optimalizace příklady výsledků dotazu](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Hlasitost v tomto příkladu je sice úmyslně malé, může být efekt nepřesnými parametry podstatné, zejména v případě velkých databází. Rozdíl v krajní případech může být mezi sekund pro rychlé případy a hodin pomalé případy.

Můžete zkoumat **sys.resource_stats** chcete zjistit, zda je zdroj u test používá zdroje více nebo méně než jiné test. Při porovnávání data oddělená časování testů tak, že nejsou ve stejném okně 5 minut **sys.resource_stats** zobrazení. Cílem výkon je minimalizovat celkovou kapacitu zdroje použité a ne minimalizovat zdrojů pole Špička. Obecně vzato optimalizace část kódu pro latence také snižuje využití prostředků. Ujistěte se, že jsou potřebné změny, které uděláte aplikace a že změny nejsou negativně ovlivnit prostředí zákazníka pro někoho, kdo může používat pokyny k dotazu v aplikaci.

Pokud úlohu sadu opakující se dotazů, často dává smysl zachycení a ověřit optimality své volby plánu, protože jednotky jednotka velikost minimální zdroje potřebná k hostování databáze. Po jeho ověřením občas prozkoumat plány týkající se ujistěte se, že budou ještě sníženou kvalitu. Další informace o [Pokyny k dotazu (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Sharding křížově databáze
Protože databáze SQL Azure získáte na hardware komoditou omezení kapacity pro jednu databázi jsou nižší než pro tradiční místní instalací SQL serveru. Některé zákazníky pomocí sharding operací databázi rozděleno více databázím, když operace nehodí uvnitř limity jednu databázi v databázi SQL Azure. Většina zákazníkům, kteří používají sharding postupy v databázi SQL Azure rozdělení související data v jedné dimenzi na více databází. Tento postup musíte porozumět tomu, že aplikace OLTP často provádět transakce, které platí pouze pro jeden řádek nebo pro malou skupinu řádků ve schématu.

>[AZURE.NOTE] Databáze SQL nyní obsahuje knihovnu kvůli usnadnění sharding. Další informace najdete v tématu [Přehled knihovny klienta pružná databáze](sql-database-elastic-database-client-library.md).

Například pokud databáze obsahuje jméno zákazníka, pořadí a Rozpis objednávek (třeba tradiční příklad databázi Northwind, který se dodává se serverem SQL Server), může rozdělíte tato data do více databázím seskupením zákazníků s související objednávky a podrobné informace o pořadí. Můžete zaručit, že zákazníka dat zůstává v jedné databáze. Aplikace by rozdělit různé zákazníky do databáze, efektivně šíření načíst přes více databází. S sharding nejen zákazníci se vyhnout maximální limit velikosti databáze, ale databáze SQL Azure taky můžete zpracování úloh, které jsou podstatně vyšší než omezení úrovní různých výkonu, dokud každou jednotlivé databázi zapadá do jeho DTU.

Přestože databáze sharding nedojde k omezení kapacita agregační zdroje řešení, je velmi efektivní na podporu obrovské řešení, která jsou umístěny nad více databází. Každou databázi mohlo by umožnit spuštění na úrovni různých výkonu pro podporu obrovské a "efektivní" databáze s požadavky na vysoké prostředky.

### <a name="functional-partitioning"></a>Funkční rozdělení
SQL Server uživatelé často kombinovat mnoho funkcí v jedné databáze. Pokud aplikace logiky se spravovat zásoby úložiště, například tuto databázi pravděpodobně spojené se skladovými sledování nákupní objednávky, uložené procedury a indexovaných nebo materializované zobrazení, které Správa konci měsíce sestav použití logických operátorů. Tento postup usnadňuje ke správě databáze pro operací, jako je zálohování, ale taky vyžaduje hardware pro zpracování ve špičce načíst přes všech funkcí aplikace jeho velikost.

Pokud používáte škálování architektura v databázi SQL Azure, je vhodné rozdělit různé funkce aplikace do jiné databáze. Pomocí tohoto postupu jednotlivých aplikací měřítko nezávisle na sobě. Aplikace se stane Vytíženější (a zvyšuje zatížení databáze), správce zvolte nezávisle na výkon úrovně jednotlivých funkcí v aplikaci. Na hranici tato architektura aplikace může být větší než jednoho komoditou počítače můžete zpracovat, protože načíst rozšířit mezi více počítačů.

### <a name="batch-queries"></a>Dávkové dotazů
U aplikací, které přístup k datům pomocí velkých objemů časté, ad hoc dotazování značné množství doba odezvy je stráveného síťová komunikace mezi vrstvy aplikace a úroveň databáze SQL Azure. I když aplikace a databáze SQL Azure jsou ve stejném datovém centru, sítě latenci mezi těmito dvěma může zvětšit tak, že velký počet datových operací access. Zmenšit síť cest zaokrouhlit pro access operace s daty, zvažte použití možnosti buď dávkové ad hoc dotazů a jejich kompilace jako uložené procedury. Pokud jste dávka ad hoc dotazů, můžete poslat víc dotazů jako jeden velký list v jedné ze služební cesty k databázi SQL Azure. Pokud je ad hoc dotazů v uložená procedura, můžete dosáhnout stejný výsledek jako kdybyste je dávky. Pomocí uložené procedury také umožňuje výhodu zvětšení šanci ukládání do mezipaměti plány dotazu v databázi SQL Azure, abyste mohli znovu použít uložená procedura.

Některé aplikace jsou náročné zápisu. Někdy se můžete zmenšit celkové zatížení vstupu a výstupu databáze vzhledem k tomu, jak dávkové zápisy společně. Často to je jednoduchá – stačí použití explicitní transakce namísto automatického Potvrdit transakce v uložené procedury a ad hoc listy. Hodnocení různých metod, které můžete použít najdete v článku [Batching technik pro databáze SQL aplikace v Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimentovat s vlastní pracovní zátěž dávky najít správné modelu. Ujistěte se, pokud chcete zjistit, že modelu pravděpodobně mírně odlišnou konzistence záruky. Nalezení správné pracovní zátěž, který slouží k minimalizaci využití prostředků vyžaduje hledání požadovanou kombinaci konzistenci a výkonu střídání.

### <a name="application-tier-caching"></a>Ukládání do mezipaměti vrstva aplikace
Některé databázové aplikace mohou mít pracovního vytížení těžké pro čtení. Ukládání do mezipaměti vrstvy můžou omezit zatížení databáze a může potenciálně snížit úroveň výkonu je potřebný kvůli podpoře databáze pomocí databáze SQL Azure. S [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/), pokud máte úlohu čtení těžké si můžete přečíst data jednou (nebo jednou jednotlivé vrstvy aplikace počítače, podle toho, jak je nakonfigurované) a pak uložit tato data mimo databázi SQL. Toto je způsob, jak zmenšit zatížení databáze (procesoru a číst vstupu a výstupu), ale nejsou vliv na konzistence, protože dat přečíst z mezipaměti může být nejsou synchronizovány s daty v databázi. V mnoha aplikacích je přijatelné určitou úroveň nekonzistence, který neplatí pro všechny úlohami. Měli byste plně pochopit požadavky aplikace před implementace efektivní strategii mezipaměti vrstva aplikace.

## <a name="next-steps"></a>Další kroky

- Podrobnosti o – úrovně služeb najdete v článku [Možnosti SQL databáze a výkon](sql-database-service-tiers.md)
- Další informace o fondů pružná databáze najdete v tématu [Co je Azure pružná databáze fondu?](sql-database-elastic-pool.md)
- Informace týkající se výkonu a fondů pružná databáze najdete v článku [kdy je vhodné vzít v úvahu fondu pružná databáze](sql-database-elastic-pool-guidance.md)
