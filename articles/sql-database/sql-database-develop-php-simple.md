<properties
    pageTitle="Připojení k databázi SQL pomocí PHP ve Windows | Microsoft Azure"
    description="Zobrazuje ukázka PHP programu, který se připojuje k databázi SQL Azure z klienta se systémem Windows a poskytuje odkazy na nezbytné součásti softwaru potřeby klientem."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Připojení k databázi SQL pomocí PHP v systému Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Toto téma ukazuje, jak se můžete připojit k databázi SQL Azure z klientské aplikace napsané v PHP, která poběží na Windows.

## <a name="step-1--configure-development-environment"></a>Krok 1: Konfigurace vývojové prostředí:

[Konfigurace vývojové prostředí pro vývoj PHP](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Vytvoření databáze SQL

Najdete v článku [Stránka Začínáme](sql-database-get-started.md) se dozvíte, jak vytvořit ukázkovou databázi.  Je důležité, že které sledujete průvodce k vytvoření **šablony databáze AdventureWorks**. Ukázky ukázáno v následujícím příkladu fungují jen **AdventureWorks schéma**.


## <a name="step-3-get-connection-details"></a>Krok 3: Stáhněte si podrobnosti o připojení

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Krok 4: Spustit ukázkový kód

* [Ověření koncepce připojení k SQL pomocí PHP](https://msdn.microsoft.com/library/mt720665.aspx)
* [Připojení k SQL s PHP pružným způsobem](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Další kroky

* Projděte si [Přehled vývoje databáze SQL](sql-database-develop-overview.md)
* Další informace o [Ovladač PHP společnosti Microsoft pro systém SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Další informace o použití a instalaci PHP najdete v tématu [Otevření databáze SQL serveru s PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Další zdroje informací 

* [Provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Prozkoumat všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/)
