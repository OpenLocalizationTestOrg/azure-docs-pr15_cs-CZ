<properties
    pageTitle="Databáze SQL výkon a možnosti: služby úrovní | Microsoft Azure"
    description="Porovnání databáze SQL výkon a obchodní funkcí pokračování ze služby úrovní vyrovnání náklady a možností při změně velikosti."
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
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Možnosti SQL databáze a výkonu: porozumět tomu, co je k dispozici v každé vrstvy služeb

[Databáze SQL Azure](sql-database-technical-overview.md) nabízí tři úrovně služby víceúrovňového výkonu zpracovávání různých úloh. Každou úroveň výkonu obsahuje sady rostoucí zdrojů navrženy pro stále vyšší výkon. Můžete spravovat každou databázi ve své vlastní [vrstvy služeb](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) s úroveň výkonu. Můžete spravovat více databází v [pružná fondu](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) s sdílených sadu prostředků. Materiály k dispozici pro samostatné databáze jsou vyjádřeny z hlediska jednotek databázové transakce (DTUs) a pružná fondů z hlediska pružná DTUs nebo eDTUs. Další informace o DTUs a eDTUs najdete v tématu [Co je DTU](sql-database-what-is-a-dtu.md). 

V obou případech zahrnout úrovní služby **základní** **Standardní**a **Premium**. Možnosti databáze v těchto úrovní se podobají pružná fondů i samostatné databází, ale v úvahu další pro pružná fondy. Tento článek obsahuje podrobných dat ze služby úrovní pružná fondů i samostatné databází.

## <a name="service-tiers-and-database-options"></a>Služba úrovní a možnosti databáze
Základní, standardní, a úrovní služby Premium mít provozu SLA 99,99 % nabízejí předvídatelná výkonu, možnosti kontinuitu flexibilní firmy, funkce zabezpečení a hodinové fakturace. V následující tabulce jsou uvedeny příklady nejlepší úrovní vhodný pro jiné aplikace úloh.

| Vrstvy služeb | Cíl úloh |
|---|---|
| **Základní** | Nejvhodnější pro malé databáze podpůrné obvykle jediné aktivní operace v daném okamžiku. Jako příklad lze uvést databáze použité pro vývoj nebo testování třetí strana nebo drobným často nepoužíváte aplikací. |
| **Standardní** | Přejít na možnost u většiny aplikací cloudu podporu více souběžné dotazů. Jako příklad lze uvést pracovní skupina nebo webové aplikace. |
| **Premium** | Navržený pro transakční velkého množství, podpůrné mnoho uživatelů najednou, které vyžadují nejvyšší úroveň kontinuitu funkcí business. Příklady databází podpůrné aplikace jsou kritické. |

>[AZURE.NOTE] Web pro podnikatele edice jsou už nepoužívá. Pokud chcete pokračovat v používání edice Web a Business, přečtěte si [Nejčastější dotazy týkající se Západ slunce](https://azure.microsoft.com/pricing/details/sql-database/web-business/) .

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Samostatné databáze služby úrovní a výkonu úrovně
Samostatné, pro databáze existuje několik úrovní výkonu v rámci každé vrstvy služeb. Máte možnost zvolte požadovanou úroveň, která odpovídá vaší vytížení. Pokud potřebujete zobrazit nahoru nebo dolů, můžete snadno změnit úrovně databáze. Podrobnosti najdete v článku [Změna úrovní služby databáze a výkonu úrovně](sql-database-scale-up.md) .

Ukazatele výkonu zde uvedené platí pro databáze vytvořené pomocí [V12 databáze SQL](sql-database-v12-whats-new.md). Bez ohledu na počet databází hostované databáze je načtena zaručené sadu prostředků a nemá vliv očekávaného výkonu vlastnosti databáze.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Podrobné vysvětlení všechny řádky v této tabulce úrovní služby najdete v článku [Možnosti osy služby a omezení](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Pružná fondu služby úrovní a výkon eDTUs
Kromě vytváření a změny velikosti samostatnou databázi, máte taky možnost správy více databází v rámci [pružná fondu](sql-database-elastic-pool.md). Všechny databáze ve fondu pružná sdílejí jednu sadu zdrojů. Ukazatele výkonu se měří *Pružná jednotek databázové transakce* (eDTUs). Jak s databázemi samostatného fondů se tři úrovně služby: **základní** **Standardní**a **Premium**. Pro fondy tyto tři služby úrovní stále definovat limity celkový výkon a několik funkcí.

Skupiny umožňují databáze pro sdílení a používání zdrojů DTU aniž by musel přiřazení výkonu konkrétní úrovně na každou databázi ve fondu. Například samostatnou databázi ve standardní fondu můžete přejít pomocí 0 eDTUs eDTU maximální databáze, které jste nastavili při konfiguraci fondu. Fondů povolit více databázím s odlišnými pracovního vytížení efektivně používat dostupných eDTU zdrojů do fondu celý. Další informace najdete v článku [aspektech cena a výkonu pružná fondu](sql-database-elastic-pool-guidance.md) .

Následující tabulka popisuje charakteristických vlastností fondu služby úrovní.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Každou databázi v rámci fondu taky vyhovuje vlastnosti databáze samostatného této osy. Například fondu základní má omezení pro maximální počet relací za fondu 4800 28800, ale jednotlivé databáze v rámci základní fondu může obsahovat databáze maximálně 300 relace.

## <a name="choosing-a-service-tier"></a>Výběr vrstvy služeb

Rozhodovat o vrstvy služeb, začněte tím, zjištění, jestli má být samostatnou databázi nebo být součástí pružná fondu databáze. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Výběr vrstvy služeb pro danou databázi samostatný

Rozhodnout o vrstvy služeb pro danou databázi samostatného nejdřív zjištění databázových funkcí, které je potřeba zvolit edition databáze SQL:

- Velikost databáze (2 GB maximální pro základní, maximálně 250 GB pro Standard a 500 až 1 TB maximální Premium – v závislosti na úrovni výkonu)
- Doba uchovávání informací záložní databáze (7 dnů Basic 35 dnů standardu a 35 dnů Premium)

Po zkontrolování edition databáze SQL, jste připraveni k určení úrovně výkonu pro databázi (počet DTUs). Můžete uhodnout a potom [Měřítko nahoru nebo dolů dynamicky](sql-database-scale-up.md) podle skutečné práce. Můžete taky [DTU kalkulačky](http://dtucalculator.azurewebsites.net/) pro aproximaci počtu počet DTUs potřeby. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Volba vrstvy služeb pro fond pružná databáze.

Rozhodovat o vrstvy služeb pro fond pružná databázi, spusťte určením databázových funkcí, které je potřeba zvolit vrstvy služeb fondu.

- Velikost databáze (2 GB pro Basic, 250 GB pro Standard a 500 pro Premium)
- Doba uchovávání informací záložní databáze (7 dnů Basic 35 dnů standardu a 35 dnů Premium)
- Počet databází na fondu (400 Basic 400 standardu a 50 Premium)
- Maximální úložiště na fondu (117 GB Basic 1200 standardu a 750 Premium)

Po zkontrolování vrstvy služeb fondu, jste připraveni k určení úrovně výkonu fondu (eDTUs). Můžete odhad a potom [škálování nebo dynamicky Neomezovat](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) na základě skutečné práce. [Kalkulačka DTU](http://dtucalculator.azurewebsites.net/) můžete použít pro aproximaci počtu počet DTUs potřebné pro každou databázi do fondu vám pomohou zjistit horní mez pro fondu.

## <a name="next-steps"></a>Další kroky
- Další informace o ceny pro tyto úrovní na [Ceny databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/).
- Další podrobnosti o [pružná fondů](sql-database-elastic-pool-guidance.md) a [cena a výkonu aspektech pro pružná fondů](sql-database-elastic-pool-guidance.md).
- Zjistěte, jak [sledování, spravovat a změna velikosti pružná fondů](sql-database-elastic-pool-manage-portal.md) a [Sledování výkonu samostatného databází](sql-database-single-database-monitor.md).
- Teď byste vědět o úrovní databáze SQL, je vyzkoušíte s [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) a zjistěte, [jak vytvoření první databáze SQL](sql-database-get-started.md).

## <a name="additional-resources"></a>Další zdroje informací

- [Provedeních pro více klienta SaaS aplikace databáze SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy videokurzu na možností pružná databáze v databázi SQL Azure](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
