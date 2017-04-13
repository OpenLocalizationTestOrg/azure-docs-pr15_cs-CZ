<properties
    pageTitle="Jak udělat úkoly správy, například resetování hesla správce | Microsoft Azure"
    description="Popisuje, jak provádět běžné úkoly správy v databázi SQL. Například resetování hesla správce, poskytnutí a odebrání přístup."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="resetování hesla správce"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Jak provádět běžné úkoly správy například resetování hesla správce v databázi SQL Azure
Toto téma pro rychlé kroky umožňuje udělit a odebrat přístup k databázi Azure SQL. Další informace naleznete v tématu:

- [Správa databází a přihlášení v databázi SQL Azure](sql-database-manage-logins.md)
- [Zabezpečení databáze SQL](sql-database-security.md)
- [Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Resetovat heslo správce pro logické server

- Na [Portálu Azure](https://portal.azure.com) klikněte na položku **SQL servery**, vyberte server ze seznamu a klikněte na **Resetovat heslo**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Se dozvíte, jak si jistí pouze oprávněnými IP adresy mohou získat přístup k serveru
- V tématu [jak: Nakonfigurujte nastavení brány firewall na databáze SQL](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Vytvoření uživatelů obsažené databáze v databázi uživatelů
- Použijte příkaz [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) a přečtěte si článek [Obsažené databáze uživatelů – díky vaši databázi Portable](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Ověřování uživatelů obsažené databáze pomocí služby Azure Active Directory
- Najdete v článku [připojení k databázi SQL pomocí ověřování služby Azure Active Directory](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Vytvoření dalších přihlášení pro vysokou oprávněními uživatele v virtuální hlavní databáze
- Pomocí příkazu [Vytvořit přihlášení](https://msdn.microsoft.com/library/ms189751.aspx) a naleznete v části Správa přihlášení [Správa databází](sql-database-manage-logins.md) a přihlášení v databázi SQL Azure podrobnější.
