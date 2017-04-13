<properties
    pageTitle="Databáze SQL: Co je DTU? | Microsoft Azure"
    description="Principy jaké Azure SQL databáze aplikace jednotku transakce je."
    keywords="Možnosti databáze, výkon databáze"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Vysvětlení jednotek databázové transakce (DTUs) a pružná jednotek databázové transakce (eDTUs)

Tento článek vysvětluje jednotek databázové transakce (DTUs) a pružná jednotek databázové transakce (eDTUs) a co se stane při stisknutí maximální DTUs nebo eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Jaké jsou databázové transakce jednotky (DTUs)

DTU je měrnou jednotku prostředky, které se musí být k dispozici k databázi Azure SQL samostatného úrovni konkrétní výkonu v rámci [vrstvy služeb samostatně databáze](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). DTU vyjadřuje prolnutí procesoru, paměti a údaje v/v operací a vstupy a výstupy protokolu transakce na poměr určena pracovní zátěž srovnávacích OLTP navržený typická pro reálný OLTP úloh. Velikosti DTUs zvýšením úrovně výkonu databáze rovná velikosti sady zdroje k dispozici pro tuto databázi. Například Premium P11 databáze pomocí 1750 DTUs poskytuje 350 x další DTU výpočet power než základní databáze pomocí 5 DTUs. Metodologie za pracovní zátěž srovnávacích OLTP požívaný k určení prolnutí DTU, najdete v tématu [Přehled srovnávacích databáze SQL](sql-database-benchmark-overview.md).

![Úvod k databázi SQL: jedna databáze DTUs osy a úroveň](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Můžete [změnit – úrovně služeb](sql-database-scale-up.md) kdykoli s minimálními prostoje aplikaci (obvykle průměru ve skupinovém rámečku čtyř sekund). Pro mnoho firmy a aplikací je možné vytvořit databází a vyžádané výkon jedné databáze nahoru nebo dolů je nestačí, zejména pokud vzorce použití jsou relativně předvídatelná. Ale pokud máte vzorce neočekávané použití ho je těžké ke správě nákladů a obchodní model. V tomto scénáři použít pružná fond s počtem eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Jaké jsou pružná jednotek databázové transakce (eDTUs)

EDTU je měrnou jednotku množiny zdrojů (DTUs), které mohou být sdíleny mezi sadu databáze na serveru Azure SQL - s názvem [pružná fondu](sql-database-elastic-pool.png). Pružná fondů poskytují jednoduché nákladů efektivní řešení pro správu výkonem pro více databázím, které mají široce různé a vzorce neočekávané použití. V tématu [pružná fondů a úrovní služby](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) Další informace.

![Úvod k databázi SQL: eDTUs osy a úroveň](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Fond je uveden sadu počet eDTUs za cenu nastavení. V rámci fondu jednotlivé databáze jsou věnovat flexibilitu automatické měřítko v rámci nastavení parametrů. Při velkém zatížení databázi používat více eDTUs plnit služba. Používání databází ve skupinovém rámečku light zatížení menší a databází bez zatížení používání žádné eDTUs. Zřízení materiály pro celou skupinu a ne pro jeden databáze zjednodušuje úlohy správy. Plus mají předvídatelná rozpočtu skupiny.

Další eDTUs lze přidat do existující fond prostoje žádné databáze nebo žádný vliv na databáze ve fondu pružná. Podobně případnému navíc eDTUs jsou už jsou je možné odebrat ze existující fond kdykoli v čase. Můžete přidat nebo jejich odečtení databáze do fondu. Pokud databáze je předvídatelností v části využití prostředků, ho můžete přesunout.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Jak lze zjistit počet DTUs potřeby tak, že Můj pracovní zátěž?

Pokud hledáte migrace existující místní nebo SQL Server virtuálního počítače zátěží na projektu k databázi SQL Azure, můžete použít [DTU kalkulačky](http://dtucalculator.azurewebsites.net/) pro aproximaci počtu počet DTUs potřeby. Pro existující vytížení databáze SQL Azure můžete použít [SQL databáze dotazu výkonu přehled](sql-database-query-performance.md) k pochopení využití prostředků vaší databáze (DTUs) získat hlubší přehled o tom, jak optimalizovat vaše pracovní zátěž. Můžete taky [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV získat informace o zdroji spotřebu poslední hodinu. Můžete taky zobrazení katalogu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) můžete taky zpracovávat zobrazíte stejná data pro posledních 14 dní, i když v dolním věrností průměry pět minut.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Jak poznám, pokud se přínos pružná fondu zdrojů?

Skupiny jsou vhodné pro velký počet databází vzorky konkrétní využití. Pro danou databázi tento způsob je které je určeno argumenty nízké využití průměru s relativně časté využití. Databáze SQL automaticky vyhodnocuje používání historickými zdrojů databází na existující databáze SQL serveru a doporučuje konfigurace odpovídající fondu Azure portálu. Další informace najdete v tématu [Při fondu pružná databáze bude použito?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Co se stane, když přístupů Moje maximální DTUs

Výkon úrovně jsou kalibrován a řídit poskytovat potřeby prostředky ke spuštění databáze vytížení až maximální limity pro úroveň osy/výkonu vybrané služby povolené. Pokud váš pracovní zátěž je zasažení omezení v jednom z/procesoru/dat vstupu a výstupu/protokolu v omezení, se nepřestanou objevovat zdrojům maximální povolenou úroveň, ale budete pravděpodobně zobrazit vyšší počet abnormálních pro dotazy zpoždění. Tyto limity za následek všechny chyby, ale raději zpomalení pracovní zátěž, pokud zpomalení změní tak, že dotazů začínají časování špatných. Pokud jsou zasažení limity relací/požadavky maximální povolený souběžné uživatele (pracovní vlákna), podívejte se explicitní chyby. V tématu [limity prostředků databáze SQL Azure](sql-database-resource-limits.md) informace o omezení prostředků než procesoru, paměti, data vstupu a výstupu a transakční protokol vstupu a výstupu.

## <a name="next-steps"></a>Další kroky

- Informace o DTUs a eDTUs k dispozici pro samostatné databází a pružná fondů najdete v tématu [vrstvy služeb](sql-database-service-tiers.md) .
- V tématu [limity prostředků databáze SQL Azure](sql-database-resource-limits.md) informace o omezení prostředků než procesoru, paměti, data vstupu a výstupu a transakční protokol vstupu a výstupu.
- V tématu [Přehled výkonu dotazu SQL databáze](sql-database-query-performance.md) pochopit spotřebu (DTUs).
- V tématu [Přehled srovnávacích databáze SQL](sql-database-benchmark-overview.md) pochopit metodologie za pracovní zátěž srovnávacích OLTP požívaný k určení DTU prolnutí.