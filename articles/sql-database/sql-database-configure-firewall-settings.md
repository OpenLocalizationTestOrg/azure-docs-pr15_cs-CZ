<properties
    pageTitle="Konfigurace pravidlo databáze SQL serveru Brána firewall | Microsoft Azure"
    description="Informace o konfiguraci brány firewall pro IP adresy, které mají přístup Azure SQL serveru."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Konfigurace brány firewall na úrovni serveru pravidlo databáze SQL Azure pomocí portálu Azure


> [AZURE.SELECTOR]
- [Základní informace](sql-database-firewall-configure.md)
- [Azure portálu](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [ROZHRANÍ REST API](sql-database-configure-firewall-settings-rest.md)

Azure SQL serveru používá bránu firewall pravidla povolit připojení k serverům a databází. Nastavení serveru a databáze úrovně brány firewall pro předlohy nebo databáze uživatelů můžete definovat v Azure SQL logické serveru selektivně umožňuje přístup k databázi. Toto téma popisuje pravidla serveru Brána firewall.

> [AZURE.IMPORTANT] Pokud chcete, aby aplikace z Azure na připojit k serveru Azure SQL, musíte povolit Azure připojení. Jak bránu firewall pravidel práce, najdete v tématu [jak nakonfigurovat firewall server Azure SQL \- přehled](sql-database-firewall-configure.md). Pokud vytváříte připojení uvnitř hranici Azure cloudu, bude pravděpodobně otevřít některé další porty TCP. Další informace najdete v tématu **V12 SQL databáze: mimo a uvnitř** část [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

**Doporučení:** Používejte pravidla brány firewall úrovni serveru pro správce a máte mnoho databází, které mají stejné požadavky přístup a nechcete, aby se vám konfigurace každou databázi jednotlivě. Microsoft doporučuje používat databázi brána firewall pravidla kdykoli je to možné, můžete zvýšit zabezpečení a tím obecnější databáze.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Správa existující pravidla brány firewall úrovni serveru portálu Azure

Opakujte kroky ke správě pravidel serveru Brána firewall.

- Přidání místního počítače, klikněte na Přidat klienta IP.
- Pokud chcete přidat další IP adresy, zadejte název pravidla, spusťte IP adresa a koncová IP adresa.
- Chcete-li upravit existující pravidlo, klikněte na libovolné pole v pravidle a upravte.
- Chcete-li odstranit existující pravidlo, přejděte myší na pravidlo, dokud se nezobrazí na X na konci řádku. Klikněte na X odebrat pravidlo.

Klikněte na **Uložit** změny uložte.

## <a name="next-steps"></a>Další kroky

Jak na článek o tom, jak jazyce Transact-SQL umožňuje vytvářet pravidla serveru a databáze úrovně brány firewall najdete v článku [pravidla serveru a databáze úrovně brány firewall konfigurace databáze SQL Azure pomocí T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Pro jak články o vytváření pravidel brány firewall úrovni serveru pomocí jiné metody, přečtěte si článek: 

- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytváření databáze najdete v článku [vytvoření databáze SQL Azure portálu minut](sql-database-get-started.md).
Nápověda ke službě připojení k databázi Azure SQL z otevřít zdroj nebo aplikací jiných výrobců najdete v článku [klienta rychlého ukázky k SQL databázi](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Jak přejít do databáze, najdete v tématu [Správa databáze aplikace access a přihlašovací zabezpečení](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Další zdroje informací

- [Zabezpečení databáze](sql-database-security.md)
- [Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 
