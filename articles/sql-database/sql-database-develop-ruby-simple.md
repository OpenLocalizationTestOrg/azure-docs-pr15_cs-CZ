<properties
    pageTitle="Připojení k databázi SQL pomocí skutečné | Microsoft Azure"
    description="Zobrazit ukázku skutečné kódu spuštění připojení k databázi SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="andrela"/>


# <a name="connect-to-sql-database-by-using-ruby"></a>Připojení k databázi SQL pomocí skutečné 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Toto téma ukazuje, jak se můžete připojit a dotaz databázi SQL Azure pomocí skutečné. Tento příklad lze spustit z Windows, se systémem Ubuntu Linux nebo Mac platformách.

## <a name="step-1-configure-development-environment"></a>Krok 1: Konfigurace vývojové prostředí:

[Požadavky pro použití ovladače skutečné TinyTDS pro systém SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Vytvoření databáze SQL

Najdete v článku [Stránka Začínáme](sql-database-get-started.md) se dozvíte, jak vytvořit ukázkovou databázi.  Je důležité, že které sledujete průvodce k vytvoření **šablony databáze AdventureWorks**. Ukázky ukázáno v následujícím příkladu fungují jen **AdventureWorks schéma**.

## <a name="step-3-get-connection-details"></a>Krok 3: Stáhněte si podrobnosti o připojení

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Krok 4: Spustit ukázkový kód

[Ověření koncepce připojení k SQL pomocí skutečné](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Další kroky

* Projděte si [Přehled vývoje databáze SQL](sql-database-develop-overview.md)
* Další informace o [Ovladač skutečné společnosti Microsoft pro systém SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>Další zdroje informací 

* [Provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Prozkoumat všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/)
