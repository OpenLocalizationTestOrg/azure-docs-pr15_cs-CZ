<properties
    pageTitle="Databáze SQL vyvíjet přehled | Microsoft Azure"
    description="Informace o připojení k dispozici knihoven a osvědčené postupy pro připojení k databázi SQL aplikace."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>Přehled vývoje databáze SQL
Tento článek provede základní důležité informace, které může vývojář měli znát při psaní kódu pro připojení k databázi SQL Azure.

## <a name="language-and-platform"></a>Jazyk, tak i platformu
Ukázky nejsou k dispozici pro různé programovacím jazyce a platformách. Můžete najít odkazy na ukázky na: 

* Další informace: [knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Omezení prostředků
Databáze SQL Azure spravuje prostředky k dispozici pro databázi pomocí dva různé mechanismy: zásady správného řízení zdrojů a prosazování omezení.

* Další informace: [omezení zdrojů databáze SQL Azure](sql-database-resource-limits.md)

## <a name="security"></a>Zabezpečení
Databáze SQL Azure obsahuje prostředky pro omezení přístupu, ochrana dat a sledování aktivity v databázi SQL.

* Další informace: [zabezpečení databáze SQL](sql-database-security.md)

## <a name="authentication"></a>Ověřování
* Databáze SQL Azure podporuje SQL Server ověřování uživatelů a přihlášení, i uživatelům [Azure Active Directory authentication](sql-database-aad-authentication.md) i přihlášení.
* Budete muset zadat určitou databázi, namísto příkazu *hlavní* databáze.
* Přepnutí na jinou databázi nelze použít příkaz jazyce Transact-SQL **pomocí myDatabaseName;** SQL databázi.
* Další informace: [zabezpečení databáze SQL: Správa zabezpečení databáze aplikace access a přihlašovací](sql-database-manage-logins.md)

## <a name="resiliency"></a>Odolnost proti chybám
Pokud přechodná chybě dojde při připojení k databázi SQL, by měl kódu volání opakovat.  Doporučujeme, opakujte logiky zdvojnásobení použití logických operátorů, tak, aby ho není přebytek nadbytek databáze SQL s více klientů opakování současně.

* Kód ukázky: ukázek kódu, které ilustrují opakování použití logických operátorů, naleznete v tématu vzorků pro daný jazyk podle svého výběru na: [knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md)
* Další informace: [chybové zprávy pro databáze SQL klientských programů](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Správa připojení
* Logiky připojení klientů přepište výchozí časový limit je 30 sekund.  Výchozí interval je příliš krátká pro připojení, které jsou závislé na Internetu.
* Pokud používáte [fond připojení](http://msdn.microsoft.com/library/8xx3tyca.aspx), ujistěte se, pokud chcete ukončit připojení rychlých aplikace není aktivně s ním pracovat a není Příprava znovu použít.

## <a name="network-considerations"></a>Co byste měli zvážit sítě
* Na počítači, který je hostitelem klientským programem zajistěte, aby že brány umožňuje odchozí komunikaci TCP na port 1433.  Další informace: [Konfigurace brány firewall databáze SQL Azure](sql-database-configure-firewall-settings.md)
* Pokud klientským programem připojuje k SQL databázi V12 v průběhu svému klientovi na Azure virtuálního počítače (OM), musíte otevřít určitých oblastí port v OM. Další informace: [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
* Připojení klientů k V12 databáze SQL Azure někdy používat proxy server a interaktivně pracovat přímo s databází. Porty než 1433 stane důležité. Další informace: [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Sharding dat pomocí pružná škály
Pružná měřítko zjednodušuje měřítku (a). 

* [Provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md)
* [Začínáme s náhledem pružná měřítko databáze Azure SQL](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Další kroky

Prozkoumejte všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/).
