<properties
    pageTitle="Databáze SQL kurz: Začínáme s zabezpečení"
    description="Naučte se vytvořit uživatelské účty a spravovat databáze."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Databáze SQL kurz: vytvoření databáze SQL uživatelských účtů k přístupu a spravovat databáze


> [AZURE.SELECTOR]
- [Získání Začínáme kurz](sql-database-get-started-security.md)
- [Udělení přístupu](sql-database-manage-logins.md)

V tomto kurzu se naučíte, jak používat SQL Server Management Studio (SSMS) na:

- Přihlaste se k databázi SQL pomocí přihlášení hlavní úrovni serveru.
- Vytvoření uživatelského účtu databáze SQL.
- Udělte uživatelům databáze SQL [db_owner oprávnění](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
- Připojení k databázi SQL k uživatelskému účtu, který není hlavní úrovni serveru.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Další kroky
Teď dokončení tohoto kurzu databáze SQL a vytvořit uživatelský účet a oprávnění uživatele účtu dbo, jste připraveni na další informace o [zabezpečení databáze SQL](sql-database-manage-logins.md).


