<properties
   pageTitle="Databáze Azure SQL vytvoří více klienta aplikace s izolace a efektivity"
   description="Zjistěte, jak databáze SQL vytvoří více klienta aplikace"
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
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Vytvoří více klienta aplikace s databáze Azure SQL pomocí izolace a efektivity

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Využít pružná fondů a vytvořte efektivnější více klienta aplikace

Pokud jste SaaS vývojáře psaní aplikace více klienta a zpracování množství zákazníků, často uděláte nevýhody v zákazníka výkonu, správu a zabezpečení. Azure SQL databáze pružná databáze fondy už musíte tuto narušení zabezpečení. Tyto fondů vám usnadní správu a sledování aplikací více klienta a získání izolace výhody jeden zákazníka na databáze. V tématu [provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Vytvoření více klienta aplikace](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatické měřítko ovládáte

Fondů automaticky měřítko výkonu a kapacity úložiště pro pružná databáze rychlé úpravy. Můžete určit výkonu přiřazená fond, přidat nebo odebrat pružná databáze jako služba a definovat výkonu pružná databází beze změny celkové náklady fondu. To znamená, že nemusíte bát, Správa použití jednotlivé databází.

[Přečtěte si dokumentaci](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Inteligentní správa prostředí

Doporučení pro změnu velikosti předdefinované včasným popsána databází, které byste měli využívat fondů. Těmito doporučeními povolit analýzu "citlivostní" pro snadné optimalizace ke splnění své cíle výkonu. Formátovaný sledování výkonu a řešení potíží s řídicí panely, abyste vizualizace historickými fondu využití.

[Přečtěte si dokumentaci](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Výkon a cena vlastním potřebám

Základní, standardní, a Premium fondů umožňují obecných spektru výkonu, ukládání a ceny možnosti. Fondy mohou obsahovat až 400 pružná databází. Pružná databáze můžete automatické měřítko až 1000 pružná databázové transakce jednotky (eDTU).

[Přečtěte si dokumentaci](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Pružná nástroje

Kromě pružná fondů jsou databáze SQL funkcí, které pomáhají spravovat činnostech přes více databází:

**Proveďte více databázových dotazů a sestav.**  
[Pružná databázových dotazů](sql-database-elastic-query-overview.md) umožňuje vytvářet dotazy nebo v sestavách všech databází v pružná fondu a přístup k vzdálené data uložená v mnoha databázích fondu najednou.

**Spusťte křížového databázové transakce.**  
[Pružná databázové transakce](sql-database-elastic-transactions-overview.md) aby bylo možné spouštět transakce, které zahrnují více databází v SQL databáze a provedení operace (tedy při zpracování finanční transakce přes databáze nebo při aktualizaci zásob v jednom databáze a objednávky).

**Provedení stejné operace na několik databází.**  
[Pružná databáze úloh](sql-database-elastic-jobs-overview.md) spustit pro správu operací, jako je opětovné sestavení indexů nebo aktualizaci schémat přes každou databázi pružná fondu.

Přejděte na domovskou stránku chcete zjistit, co dalšího databáze SQL má nabízet.
[Podívejte se na](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Další kroky

Pokud potřebujete [Azure neváhejte](https://azure.microsoft.com/get-started/) a [vytvoření první databáze SQL Azure](sql-database-get-started.md).

## <a name="additional-resources"></a>Další zdroje informací

Prozkoumejte všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/).
 
Projděte si [technický přehled SQL databáze](sql-database-technical-overview.md).  
