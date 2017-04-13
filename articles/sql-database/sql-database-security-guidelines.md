<properties
   pageTitle="Pokyny pro zabezpečení databáze Azure SQL a omezení | Microsoft Azure"
   description="Informace o databázi Microsoft Azure SQL pokyny a omezení týkající se zabezpečení."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Pokyny k používání zabezpečení Azure SQL databáze a omezení

Toto téma popisuje databázi Microsoft Azure SQL pokyny a omezení týkající se zabezpečení. Při správě zabezpečení databáze SQL Azure, zvažte následující možnosti.

## <a name="access-to-the-virtual-master-database"></a>Přístup k databázi virtuální předlohy

Pouze správci obvykle, potřebujete přístup k databázi předlohy. Běžné přístup k každý uživatel databáze by měl být prostřednictvím uživatelů není správcem obsažené databáze vytvořené v jednotlivých databází. Při použití uživatelů obsažené databáze nepotřebujete vytvořte přihlášení v hlavní databázi. Další informace najdete v tématu [Obsažené databáze uživatelů – díky vaši databázi Portable](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Brána firewall

Brána firewall systému SQL Server, která má obor pro celý Server SQL Azure obvykle nakonfigurovaný prostřednictvím portálu a mají jenom přijmout IP adresy používané správci. Vytvořit pravidlo s rozsahem databáze bránu firewall, která se otevře potřebné rozsah IP adres pro každou databázi, připojení k databázi uživateli a potom použijte [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) jazyka Transact-SQL.

Služba databáze SQL Azure neexistuje pouze prostřednictvím portu TCP 1433. Pro přístup k databázi SQL ze svého počítače, ujistěte se, že brány firewall klientský počítač umožňuje odchozí komunikaci TCP na TCP port 1433. Pokud není třeba pro ostatní aplikace, blokovat příchozí připojení pomocí protokolu TCP port 1433. 

Jako součást procesu připojení připojení z Azure virtuálních počítačích přesměrováni na různých IP adresa a číslo portu jedinečné pro každou roli pracovníka. Číslo portu je v rozmezí od 11000 do 11999. Další informace o porty TCP najdete v tématu [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Ověřování

Použití služby Active Directory authentication (integrované zabezpečení) kdykoli je to možné. Informace o konfiguraci AD ověřování najdete v článku [připojení k SQL databázi tak, že pomocí Azure Active Directory, ověřování](sql-database-aad-authentication.md)a [Zvolte režim ověřování](https://msdn.microsoft.com/library/ms144284.aspx) v SQL Server knihy Online. 

Použití obsažené databázi uživatelům umožní vylepšit škálovatelnost. Další informace najdete v tématu [Obsažené databáze uživatele – díky vaši databázi Portable](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)a [Obsažené databází](https://technet.microsoft.com/library/ff929071.aspx).

Databázový stroj zavře připojení, která zůstat nečinná dobu delší než 30 minut. Připojení musí přihlášení znovu před použitím.

Neustále aktivní připojení k databázi SQL vyžadují reauthorization (provedly databázový stroj) minimálně každých 10 hodin. Databázový stroj pokusí reauthorization pomocí původně odeslané hesla a požaduje zásah uživatele. Důvodů výkonu při vytvoření nového hesla v databázi SQL připojení není nové ověření vyžadováno, i když připojení obnovit kvůli fond připojení. Tím se liší od chování místního SQL serveru. Došlo ke změně hesla od původně oprávnění připojení, musí být ukončena připojení a vytvořit nové připojení pomocí nového hesla. Uživatel s oprávněním ukončení připojení k databázi můžete explicitně ukončení připojení k databázi SQL pomocí příkazu [UKONČIT](https://msdn.microsoft.com/library/ms173730.aspx) .

## <a name="logins-and-users"></a>Přihlášení a uživatelů

Při správě přihlášení a uživatele do SQL databáze, existují omezení.


- Musíte být připojeni k databázi **předlohy** při provádění ``CREATE/ALTER/DROP DATABASE`` příkazy. – Uživatel databáze v hlavní databázi odpovídající přihlášení hlavní úrovni serveru nelze měnit nebo nezobrazí. 
- US angličtina je výchozí jazyk pro přihlášení hlavní úrovni serveru.
- Pouze správci (přihlašovací hlavní úrovni serveru nebo správce Azure AD) a členy této role databáze **dbmanager** v databázi **předlohy** mají oprávnění k provedení ``CREATE DATABASE`` a ``DROP DATABASE`` příkazy.
- Musíte být připojeni k databázi předlohy při provádění ``CREATE/ALTER/DROP LOGIN`` příkazy. Ale se nedoporučuje používat přihlášení. Použijte uživatelů obsažené databáze.
- Připojení k databázi uživateli, je nutné zadat název databáze v připojovacím řetězci.
- Přihlášení hlavní úrovni serveru a členy této role databáze **loginmanager** v **hlavní** databázi mít oprávnění k provedení pouze ``CREATE LOGIN``, ``ALTER LOGIN``, a ``DROP LOGIN`` příkazy.
- Při provádění ``CREATE/ALTER/DROP LOGIN`` a ``CREATE/ALTER/DROP DATABASE`` příkazy v aplikaci ADO.NET parametry příkazy není povolená. Další informace najdete v tématu [parametry a příkazy](https://msdn.microsoft.com/library/ms254953.aspx).
- Při provádění ``CREATE/ALTER/DROP DATABASE`` a ``CREATE/ALTER/DROP LOGIN`` příkazy, každý z těchto údajů musí být pouze následný výběr v jazyce Transact-SQL dávku. V opačném dojde k chybě. Například následující jazyce Transact-SQL zkontroluje, zda databáze existuje. Pokud existuje, ``DROP DATABASE`` údajů se nazývá odebrání databáze. Protože ``DROP DATABASE`` údajů není příkaz pouze na listu, spouštěním následujících údajů jazyce Transact-SQL má za následek chybu.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Při provádění ``CREATE USER`` výpis s ``FOR/FROM LOGIN`` možnost, musí být pouze následný výběr v jazyce Transact-SQL dávku.
- Při provádění ``ALTER USER`` výpis s ``WITH LOGIN`` možnost, musí být pouze následný výběr v jazyce Transact-SQL dávku.
- K ``CREATE/ALTER/DROP`` vyžaduje uživatele ``ALTER ANY USER`` oprávnění databázi.
- Při přidávání nebo odebírání jiný uživatel databáze nebo z této databáze roli se snaží vlastníka databáze role, může dojít k tato chyba: **uživatele nebo role "Název" neexistuje v databázi.** K této chybě dochází, protože nejsou viditelná vlastníkovi uživatele. Tento problém vyřešíte udělit vlastníka role ``VIEW DEFINITION`` oprávnění na uživatele. 

Další informace o přihlášení a uživatelů najdete v tématu [Správa databáze a přihlášení v databázi SQL Azure](sql-database-manage-logins.md).


## <a name="see-also"></a>Viz taky

[Brána Firewall databáze Azure SQL](sql-database-firewall-configure.md)

[Postup: Nakonfigurujte nastavení brány Firewall (databáze Azure SQL)](sql-database-configure-firewall-settings.md)

[Správa databází a přihlášení v databázi SQL Azure](sql-database-manage-logins.md)

[Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)
