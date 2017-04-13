<properties
    pageTitle="Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell | Microsoft Azure"
    description="Informace o konfiguraci brány firewall pro IP adresy, které přístup k databázím Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Základní informace](sql-database-firewall-configure.md)
- [Azure portálu](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [ROZHRANÍ REST API](sql-database-configure-firewall-settings-rest.md)


Databáze SQL Azure pomocí pravidla brány firewall umožňuje připojení k serverům a databází. Nastavení serveru a databáze úrovně brány firewall pro hlavní databáze nebo databáze uživatelů můžete definovat v databázi SQL serveru selektivně umožňuje přístup k databázi.

> [AZURE.IMPORTANT] Pokud chcete, aby aplikace z Azure připojení k databázovému serveru, musíte povolit Azure připojení. Další informace o pravidlech bránu firewall a povolení připojení z Azure najdete v článku [Firewall databáze SQL Azure](sql-database-firewall-configure.md). Pokud vytváříte připojení uvnitř hranici Azure cloudu, bude pravděpodobně otevřít některé další porty TCP. Další informace najdete v tématu "V12 SQL databáze: mimo a uvnitř" část [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Vytvoření pravidla brány firewall serveru

Brána firewall úrovni serveru pravidla lze vytvářet, aktualizovat a odstranit pomocí prostředí PowerShell Azure.

Pokud chcete vytvořit nové pravidlo brány firewall úrovni serveru, spusťte [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) rutina. Následující příklad umožňuje rozsah IP adresy na serveru Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Chcete-li upravit existující pravidla serveru Brána firewall, spusťte [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) rutina. Následující příklad změn rozsahu IP adres přijatelné pravidla s názvem ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Pokud chcete odstranit existující pravidla serveru Brána firewall, spusťte [odebrat-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) rutina. Následující příklad odstraní pravidla s názvem ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Správa pravidel brány firewall pomocí prostředí PowerShell

Můžete taky použití Powershellu ke správě pravidel brány firewall. Další informace naleznete v následujících tématech:

* [Nové AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Odebrat AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Další kroky

Informace o používání jazyce Transact-SQL vytvářet pravidla serveru a databáze úrovně brány firewall, najdete v článku [pravidla serveru a databáze úrovně brány firewall konfigurace databáze SQL Azure pomocí T-SQL](sql-database-configure-firewall-settings-tsql.md).

Informace o postupu při vytváření pravidla brány firewall úrovni serveru jiným způsobem, najdete v článku:

- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytváření databáze najdete v článku [vytvoření databáze SQL Azure portálu minut](sql-database-get-started.md).
Připojení k databázi Azure SQL z otevřít zdroj nebo aplikací jiných výrobců nápovědu najdete v tématu [klienta rychlého ukázky k SQL databázi](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Jak přejít do databáze, najdete v tématu [Správa databáze aplikace access a přihlašovací zabezpečení](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Další zdroje informací

- [Zabezpečení databáze](sql-database-security.md)
- [Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
