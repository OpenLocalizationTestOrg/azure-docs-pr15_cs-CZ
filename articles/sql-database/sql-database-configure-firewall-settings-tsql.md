<properties
    pageTitle="Azure databáze SQL server a databázi úrovně pravidel pomocí T-SQL | Microsoft Azure"
    description="Informace o konfiguraci brány firewall pro IP adresy, které přístup k databázím Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Konfigurace pravidel serveru a databáze úrovně brány firewall databáze SQL Azure pomocí T-SQL


> [AZURE.SELECTOR]
- [Základní informace](sql-database-firewall-configure.md)
- [Azure portálu](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [ROZHRANÍ REST API](sql-database-configure-firewall-settings-rest.md)


Databázi Microsoft Azure SQL pomocí pravidla brány firewall umožňuje připojení k serverům a databází. Nastavení serveru a databáze úrovně brány firewall pro předlohy nebo databáze uživatelů můžete definovat v databázi SQL Azure serveru selektivně umožňuje přístup k databázi.

> [AZURE.IMPORTANT] Pokud chcete, aby aplikace z Azure připojení k databázovému serveru, musíte povolit Azure připojení. Další informace o pravidlech bránu firewall a povolení připojení z Azure najdete v článku [Firewall databáze SQL Azure](sql-database-firewall-configure.md). Pokud vytváříte připojení uvnitř hranici Azure cloudu, bude pravděpodobně otevřít některé další porty TCP. Další informace najdete v tématu **V12 SQL databáze: mimo a uvnitř** část [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Pravidla serveru Brána firewall

Jenom na úrovni serveru hlavní přihlášení Azure Active Directory správce nebo můžete vytvořit pravidlo brány firewall úrovni serveru pomocí jazyce Transact-SQL.

1. Spusťte okno s dotazem a připojení k databázi virtuální předlohy pomocí SQL Server Management Studio.
2. Pravidla brány firewall úrovni serveru můžete vybraná, vytvořili, aktualizovat nebo odstraní ze v okně dotazu.
3. Chcete-li vytvořit nebo aktualizovat pravidla serveru Brána firewall, spusťte `sp_set_firewall_rule` uložené procedury. Následující příklad umožňuje rozsah IP adresy na serveru Contoso.<br/>Začněte tím, že zobrazují, jaká pravidla již existují.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Potom přidáte pravidlo brány firewall.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Odstranění pravidla serveru Brána firewall, proveďte postup sp_delete_firewall_rule uložený. Následující příklad odstraní pravidla s názvem ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Další informace o těchto uložené procedury najdete v tématu [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) a [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Pravidla brány firewall úrovni databáze

Pouze databáze uživatel s oprávněním **ovládacího PRVKU** v databázi (například vlastníka databáze) můžete vytvořit pravidlo databáze brána firewall.

1. Po vytvoření bránu firewall úrovni serveru pro svoji IP adresu, spusťte okno s dotazem prostřednictvím portálu Classic nebo SQL Server Management Studio.
2. Připojení k databázi, u kterého chcete vytvořit pravidlo databáze brána firewall.

    Chcete-li vytvořit novou nebo aktualizovat pravidlo existující databáze brána firewall, spusťte `sp_set_database_firewall_rule` uložené procedury. Následující příklad vytvoří nové pravidlo brány firewall s názvem ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Chcete-li odstranit existující databáze brána firewall pravidlo, spusťte `sp_delete_database_firewall_rule` uložené procedury. Následující příklad odstraní pravidla s názvem ContosoFirewallRule.
`
   SPOUŠTĚNÍ sp_delete_database_firewall_rule @name = N'ContosoFirewallRule "

Další informace o těchto uložené procedury najdete v tématu [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) a [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Další kroky

Pro jak články o vytváření pravidel brány firewall úrovni serveru pomocí jiné metody, přečtěte si článek: 

- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytváření databáze najdete v článku [vytvoření databáze SQL Azure portálu minut](sql-database-get-started.md).
Nápověda ke službě připojení k databázi Azure SQL z otevřít zdroj nebo aplikací jiných výrobců najdete v článku [klienta rychlého ukázky k SQL databázi](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Jak přejít do databáze, najdete v tématu [Správa databáze aplikace access a přihlašovací zabezpečení](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Další zdroje informací

- [Zabezpečení databáze](sql-database-security.md)
- [Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)
