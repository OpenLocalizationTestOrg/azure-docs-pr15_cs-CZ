<properties
   pageTitle="Zabezpečení databáze aplikace SQL datový sklad | Microsoft Azure"
   description="Tipy pro zabezpečení databáze v Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Zabezpečení databáze aplikace SQL datový sklad

> [AZURE.SELECTOR]
- [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
- [Ověřování](sql-data-warehouse-authentication.md)
- [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
- [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Tento článek provede základní informace o zabezpečení databáze SQL Azure datový sklad. Zejména v tomto článku vám pomůžou začít pracovat s prostředky pro omezení přístupu, ochrana dat a sledování aktivity v databázi.

## <a name="connection-security"></a>Zabezpečení připojení

Zabezpečení připojení odkazuje na tom, jak omezit a zabezpečené připojení k databázi pomocí pravidel bránu firewall a šifrování připojení.

Pravidla brány firewall používají server a databázi odmítnout pokusy o připojení z IP adresy, které nebyly explicitně povolené. Aby umožňoval připojení z aplikace nebo klientský počítač veřejnou IP adresu, musíte nejdřív vytvořit pravidlo brány firewall na úrovni serveru pomocí portálu Azure, rozhraní REST API nebo Powershellu. Osvědčený měli byste omezit povolené přes bránu firewall serveru co nejvíc rozsahy IP adres.  Přístup k Azure SQL datový sklad z místního počítače, zajistěte, aby že bránu firewall na síť a místním počítači umožňuje odchozí komunikaci na TCP port 1433.  Další informace najdete v tématu [firewall databáze SQL Azure][], [sp_set_firewall_rule][]a [sp_set_database_firewall_rule][].

Ve výchozím nastavení jsou šifrované připojení k SQL datový sklad.  Změna nastavení připojení k zakázání šifrování jsou ignorovány.

## <a name="authentication"></a>Ověřování

Ověřování odkazuje na tom, jak prokázat vaši totožnost při připojení k databázi. Ověřování serveru SQL Server SQL datový sklad aktuálně podporuje se uživatelské jméno a heslo, jakož i Azure Active Directory. 

Po vytvoření logické serveru databáze zadaná přihlašovací "správce serveru" pomocí jména a hesla. Pomocí těchto přihlašovacích údajů, může ověřovat všechny databáze na serveru jako vlastník databáze nebo "dbo" pomocí ověřování serveru SQL Server.

Však jako doporučený postup organizace uživatelé by měli použít jiný účet ověření. Tímto způsobem můžete omezit oprávnění udělena aplikace a snížení rizik škodlivým aktivity v případě kód aplikace nebude ohroženo útok vkládání SQL. 

SQL Server ověřený uživatel vytvoříte připojení k databázi **předlohy** na server s přihlašovací jméno správce serveru a vytvořte nové přihlašovací server.  Kromě toho je vhodné vytvořit uživatele v hlavní databázi uživatelům Azure SQL datový sklad. Vytvoření uživatele předlohy umožňuje uživateli přihlásit pomocí nástroje jako SSMS bez zadání názvu databáze.  Umožňuje také používat Průzkumník objektů zobrazíte všechny databáze na serveru SQL server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Potom připojení k **databázi SQL datový sklad** můžete použít přihlašovací jméno správce serveru a vytvoření uživatele databáze podle přihlašovací server, který jste právě vytvořili.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Pokud uživatel bude udělat další operací, jako je vytvoření přihlášení nebo vytvoříte nové databáze, budou se taky muset přidělovat `Loginmanager` a `dbmanager` role v hlavní databázi. Další informace o těchto rolích další a ověřování k databázi SQL najdete v článku [Správa databází a přihlášení v databázi SQL Azure][].  Další informace o Azure AD pro SQL datový sklad najdete v tématu [připojení k SQL datový sklad tak, že pomocí Azure Active Directory, ověřování][].


## <a name="authorization"></a>Povolení

Povolení odkazuje co můžete dělat v databázi SQL Azure datový sklad a to se řídí členství svůj uživatelský účet role a oprávnění. Osvědčený udělte uživatelům alespoň oprávnění. Azure SQL datový sklad usnadňuje to správa rolí v T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Účet správce serveru, ke kterému se připojujete patří do skupiny db_owner, který má oprávnění Hotovo v databázi. Uložte pro nasazení upgradů schématu a dalších operací správy tento účet. Pomocí účtu "ApplicationUser" omezenější oprávnění připojit z aplikace do databáze s nejnižšími oprávněními potřeby tak, že aplikace.

Způsoby dále omezit, co můžete dělat uživatel s databáze SQL Azure:

- Podrobného [oprávnění][] umožňují řídit operací, které můžete dělat v jednotlivých sloupců tabulek, zobrazení, postupů a jiných objektů v databázi. Použití podrobného oprávnění většina ovládací prvek a udělit minimální potřebná oprávnění. Systém podrobného oprávnění je trochu složitější a vyžaduje některé studie efektivně používat.
- [Role databáze][] než db_datareader a db_datawriter lze vytvářet výkonnější aplikace uživatelské účty nebo méně výkonných správy účty. Předdefinované pevné databázové role poskytují snadný způsob, jak chcete udělit oprávnění, ale můžete mít za následek udělení oprávnění více než je potřeba.
- [Uložené procedury][] mohou sloužit k omezit akce, které můžou brát databázi.

Správa databází a logické servery z portálu Microsoft Azure klasické nebo pomocí rozhraní API Správce prostředků Azure řídí přiřazování rolí portálu uživatelský účet. Další informace o tomto tématu najdete v tématu [řízení přístupu na základě rolí v portálu Azure][].

## <a name="encryption"></a>Šifrování

Azure SQL datový sklad průhledné dat šifrování (TDE) pomáhá chránit před hrozbou, že škodlivý aktivity tak, že provedete v reálném čase šifrování a dešifrování data na ostatních.  Při šifrování databáze jsou šifrovány přidružené zálohování a souborů protokolu transakce bez nutnosti změny aplikace. TDE šifruje úložiště pro celou databázi pomocí symetrickou klíče s názvem šifrovací klíč databáze. Databáze SQL databáze šifrovací klíč chráněn certifikát předdefinované serveru. Certifikát předdefinované serveru je jedinečné pro každý databáze SQL server. Microsoft automaticky: otočí tato poukázky minimálně každých 90 dní. Šifrování algoritmus používaný datový sklad SQL je AES 256. Obecný popis TDE najdete v článku [Šifrování průhlednosti][].

Šifrování databáze pomocí [Portálu Azure] [ Encryption with Portal] nebo [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Další kroky

Další informace o připojení k SQL datový sklad s různými protokoly najdete v článku [připojení k SQL datový sklad][].

<!--Image references-->

<!--Article references-->
[Připojení k SQL datový sklad]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Připojení k SQL datový sklad pomocí ověřování služby Azure Active Directory]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Firewall databáze SQL Azure]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Role databáze]: https://msdn.microsoft.com/library/ms189121.aspx
[Správa databází a přihlášení v databázi SQL Azure]: https://msdn.microsoft.com/library/ee336235.aspx
[Oprávnění]: https://msdn.microsoft.com/library/ms191291.aspx
[Uložené procedury]: https://msdn.microsoft.com/library/ms190782.aspx
[Šifrování průhledné dat]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Řízení přístupu na základě rolí v portálu Azure]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
