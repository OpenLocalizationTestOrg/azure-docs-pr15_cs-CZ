<properties
    pageTitle="Přehled: nástroje pro správu databáze SQL | Microsoft Azure"
    description="Srovnání nástroje a možnosti pro správu databáze SQL Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Přehled: nástroje pro správu databáze SQL

Toto téma popisuje a porovnává nástroje a možnosti pro správu databáze Azure SQL, takže si můžete vybrat správný nástroj pro úkoly, máte ve firmě a můžete. Když zvolíte správný nástroj závisí na kolik databází spravujete, úkol a jak často se provádí úkolu.

## <a name="azure-portal"></a>Azure portálu

[Azure portál](https://portal.azure.com) je webové aplikace založené na místo, kam můžete vytvářet, aktualizovat a odstranit databáze a logické servery a sledování databáze. Pokud teprve začínáte s Azure, Správa několik databáze nebo musíte udělat něco rychle tento nástroj doporučujeme.

Další informace o použití portálu najdete v článku [Správa databáze SQL pomocí portálu Azure](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio a SQL Server Data Tools ve Visual Studiu

SQL Server Management Studio (SSMS) a SQL Server Data nástroje (SSDT) jsou klientských nástrojích spuštěné v počítači pro správu a vývoj databáze v cloudu. Pokud jste zkušenosti s Visual Studio nebo jiná integrované vývojové prostředí (IDEs), [zkuste použít SSDT ve Visual Studiu](https://msdn.microsoft.com/library/mt204009.aspx)vývojář aplikace. Mnoho správců databáze znají SSMS, který se dá používat se databáze Azure SQL. [Stáhněte si nejnovější verzi SSMS](https://msdn.microsoft.com/library/mt238290) a vždy použít nejnovější verzi při práci s databáze SQL Azure. Další informace o správě databáze SQL Azure pomocí SSMS najdete v článku [Správa SQL databáze pomocí SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Vždy použijte nejnovější verzi [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) a [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze.


## <a name="powershell"></a>Prostředí PowerShell

Můžete Powershellu ke správě databáze a fondů pružná databáze a k automatizaci nasazení Azure zdroje. Microsoft doporučuje tento nástroj pro správu velkého počtu databází a automatizace nasazení a změny zdrojů v pracovním prostředí.

Další informace najdete v tématu [Správa databáze SQL pomocí prostředí PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Pružná databázové nástroje
Umožňuje provádět akce jako pružná databázové nástroje 

* Spuštění T-SQL skript vůči sadě databáze pomocí [pružná projektu](sql-database-elastic-jobs-overview.md)
* Přesunutí více klienta modelu databáze do modelu jednoho tenanta pomocí [Nástroje rozdělit sloučení](sql-database-elastic-scale-overview-split-and-merge.md)
* Správa databází v modelu jednoho klienta nebo více klienta modelu pomocí [pružná měřítko klienta knihovny](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Další zdroje informací

- [Azure správce prostředků](https://azure.microsoft.com/features/resource-manager/)
- [Automatizace Azure](https://azure.microsoft.com/documentation/services/automation/)
- [Azure Plánovač](https://azure.microsoft.com/documentation/services/scheduler/)