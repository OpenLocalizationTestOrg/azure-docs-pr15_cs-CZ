<properties
    pageTitle="Připojení k databázi SQL pomocí jazyka Java JDBC ve Windows | Microsoft Azure"
    description="Představuje vzorek kód jazyka Java sloužící k připojení k databázi SQL Azure. Příklad používá JDBC a spustí v klientském počítači Windows."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Připojení k databázi SQL pomocí jazyka Java JDBC v systému Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Toto téma uvádí ukázku kódu jazyka Java využívající připojení k databázi SQL Azure. Ukázka Java závisí na Java Development Kit (JDK) verzi 1.8. V ukázce se připojuje k databázi SQL Azure pomocí ovladače JDBC.

## <a name="step-1--configure-development-environment"></a>Krok 1: Konfigurace vývojové prostředí:

[Konfigurace vývojové prostředí pro vývoj Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Vytvoření databáze SQL

Najdete v článku [Stránka Začínáme](sql-database-get-started.md) se dozvíte, jak vytvořit databázi.  

## <a name="step-3-get-connection-string"></a>Krok 3: Získání připojovacího řetězce

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Pokud používáte ovladač JTDS JDBC a pak budete muset přidat "ssl = vyžadují" na adresu URL připojení řetězce a musíte nastavit následující možnosti pro JVM "-Djsse.enableCBCProtection=false". Tuto možnost JVM zakáže oprava se zabezpečením, proto se ujistěte, že chápete riziku před nastavením tuto možnost.

## <a name="step-4-run-sample-code"></a>Krok 4: Spustit ukázkový kód

* [Ověření koncepce připojení k SQL pomocí jazyka Java](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Další kroky

* Navštivte [Středisko pro vývojáře Java](/develop/java/).
* Projděte si [Přehled vývoje databáze SQL](sql-database-develop-overview.md)
* Další informace o [Ovladač JDBC společnosti Microsoft pro systém SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Další zdroje informací 

* [Provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Prozkoumat všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/)
