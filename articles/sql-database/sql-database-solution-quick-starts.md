<properties
   pageTitle="Rychlé zahájení práce Azure SQL databáze řešení | Microsoft Azure"
   description="Další informace o řešení databáze Azure SQL"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Prozkoumejte Azure SQL databáze řešení Snadné spuštění

Tento článek obsahuje přehled Azure SQL řešení Snadné spuštění databáze. Tyto Snadné spuštění se nacházejí v úložišti vzorky GitHub SQL serveru a ukazují použití databáze SQL v úplné řešení založené na reálné situace. Jednoduchý podrobné výukové programy, které demonstrují použití určité funkce SQL databáze najdete v článku [výukové programy pro procházení databáze SQL Azure](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Zkuste WingTipTickets ukázku a praktické cvičení

Ukázka [WingTipTickets databáze SQL Azure](https://github.com/microsoft/wingtiptickets) a praktické cvičení ukazují databáze SQL Azure a ukázkové založené na Azure vyhledávání aplikace, která se používá k prodeji spolupracuje požadavky.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Shromažďovat a sledovat zdroje použití zásad správy informací mezi více skupin

[Řešení rychlý Start: pružná fondu telemetrie pomocí prostředí PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) poskytuje řešení pro shromažďování a používání zdrojů databáze SQL sledování napříč několika fondů v předplatné. Pokud máte velký počet databází v předplatné, je náročný sledování každý pružná fond samostatně.

Tento problém můžete vyřešit, můžete kombinovat PowerShell SQL databáze a dotazy T SQL získat informace o použití zdroje dat z více fondů a jejich databází. To vám pomůže sledovat a analýza využití prostředků mnohem efektivněji.

Rychlý Start obsahuje sadu skriptů Powershellu a dotazy T SQL spolu s si přečtěte následující dokumentaci co dělá řešení a jak implementovat ho.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Začínáme s pružná databází ve scénáři SaaS

 [Řešení rychlý Start: pružná fondu vlastní řídicí panel pro SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) poskytuje řešení pro situace Software – jako řešení (SaaS), který využívá funkce pružná databáze SQL databáze do back-end databáze efektivní a scalable stanovit SaaS aplikace.

V tomto řešení vás provede provádění do webových aplikací. Tento web appu umožňuje vizualizovat načíst vytvořený v databázi pružná zatížení generátor, která používá vlastní řídicí panel, který doplňuje portálu Azure.

Rychlý Start poskytuje generátor načítání a sledování v prohlížeči spolu s v dokumentaci k čemu slouží aplikaci a jak se používá.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Vytvoření databáze Azure SQL pomocí kódu první vývoj a Entity Framework

Video a vzorku v [Kódu první nové databáze](https://msdn.microsoft.com/data/jj193542.aspx) obsahuje úvod do kódu první vývoj, který se zaměřuje novou databázi. Tento scénář cílovou databázi, která neexistuje, ale která se vytvoří pomocí kódu první. Můžete taky scénáře vytvoří prázdnou databázi, do kterého kód první přidává nové tabulky.

Kód nejdřív umožňuje definovat modelu pomocí C# nebo Visual Basic .NET tříd. Volitelné další konfiguraci lze provést pomocí atributů tříd a vlastnosti nebo pomocí fluent rozhraní API.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integrace pružná databázové nástroje do aplikace Entity Framework

Ukázková [Knihovna klienta pružná databáze s Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) ukazuje změny, které potřebujete provést k aplikaci Entity Framework integrovat s [pružná databázové nástroje](sql-database-elastic-scale-get-started.md). Se zaměřuje na psaní [shard mapování správy](sql-database-elastic-scale-shard-map-management.md) a [směrování závislé dat](sql-database-elastic-scale-data-dependent-routing.md) pomocí Entity Framework kód první přístup.

[První kódu pro novou databázi ukázkové pro EF](http://msdn.microsoft.com/data/jj193542.aspx) slouží jako náš pracovního příklad v tomto příkladu. Ukázkový kód, který doprovází tento dokument je součástí sady nástrojů pružná databáze vzorků ve vzorcích kódu Visual Studio.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Pružná databázové nástroje integrovat řádku úrovně zabezpečení

[Víceklientské aplikací s pružná databázové nástroje a řádek úrovně zabezpečení](sql-database-elastic-tools-multi-tenant-row-level-security.md) zobrazuje změny, že budete muset tak, abyste aplikaci Entity Framework [pružná databázové nástroje](sql-database-elastic-scale-get-started.md) integrovat [řádku úrovně zabezpečení](https://msdn.microsoft.com/library/dn765131). Tento příklad ukazuje, jak používat tyto technologie společně s můžete vytvořit aplikaci vrstva vysoce scalable dat, který podporuje víceklientské shards.

Můžete to provést pomocí ADO.NET SqlClient nebo Entity Framework. Tento příklad rozšiřuje [knihovně klienta pružná databáze pomocí Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) přidáním podpory pro víceklientské shard databáze.
Vytvoří jednoduchou konzoly aplikace pro vytváření blogů a příspěvků, s čtyři klienty a dvě víceklientské shard databáze.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Vytvoření online průzkumy aplikaci Tailspin průzkumy

Tento [Tailspin průzkumy vzorové aplikace](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) je víceklientské webové aplikace, s názvem zjišťování, který umožňuje uživatelům vytvářet průzkumy online. Vzorku adresy některé klíčové pochybnosti o tom, jak správy identit uživatelů v víceklientské aplikace, včetně registrace, ověřování, povolení a rolí aplikace.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Informace o nejnovějších funkcích zabezpečení databáze SQL s aplikací ukázku část Contoso

Tato [Ukázka část Contoso aplikace](https://github.com/Microsoft/azure-sql-security-sample) hodnotí nejnovější funkce zabezpečení databáze SQL.

## <a name="next-steps"></a>Další kroky

[Prozkoumání výukové programy pro databázi SQL Azure](sql-database-explore-tutorials.md)
