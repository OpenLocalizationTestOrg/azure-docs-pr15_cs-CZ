<properties
    pageTitle="Azure pravidla brány firewall úrovni serveru SQL databáze pomocí rozhraní REST API | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API


> [AZURE.SELECTOR]
- [Základní informace](sql-database-firewall-configure.md)
- [Azure portálu](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [ROZHRANÍ REST API](sql-database-configure-firewall-settings-rest.md)


Databázi Microsoft Azure SQL pomocí pravidla brány firewall umožňuje připojení k serverům a databází. Nastavení serveru a databáze úrovně brány firewall pro předlohy nebo databáze uživatelů můžete definovat v Azure databázovým serverem SQL selektivně umožňuje přístup k databázi.

> [AZURE.IMPORTANT] Pokud chcete, aby aplikace z Azure připojení k databázovému serveru, musíte povolit Azure připojení. Další informace o pravidlech bránu firewall a povolení připojení z Azure najdete v článku [Firewall databáze SQL Azure](sql-database-firewall-configure.md). Pokud vytváříte připojení uvnitř hranici Azure cloudu, bude pravděpodobně otevřít některé další porty TCP. Další informace najdete v tématu **V12 SQL databáze: mimo a uvnitř** část [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Správa pravidel brány firewall úrovni serveru pomocí rozhraní REST API
1. Správa pravidel brány firewall prostřednictvím rozhraní REST API musí ověřit. Informace najdete v tématu [Průvodce pro vývojáře se tak mohli ověřovat pomocí rozhraní API Azure správce prostředků](../resource-manager-api-authentication.md).
2. Pravidla úrovni serveru můžete vytvořit, aktualizované nebo odstranit pomocí rozhraní REST API

    Vytvoření nebo aktualizace pravidla serveru Brána firewall, spusťte metodu umístění následujícími způsoby:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Žádost o textu

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Pokud chcete odebrat existující pravidla serveru Brána firewall, spusťte metodu odstranit následujícími způsoby:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Správa pravidel brány firewall pomocí rozhraní REST API

* [Vytvoření nebo aktualizace pravidlo brány Firewall](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Odstranění pravidla brány Firewall](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Získání pravidlo brány Firewall](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Seznam všech pravidel brány Firewall](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Další kroky

Jak na článek o tom, jak jazyce Transact-SQL umožňuje vytvářet pravidla serveru a databáze úrovně brány firewall najdete v článku [pravidla serveru a databáze úrovně brány firewall konfigurace databáze SQL Azure pomocí T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Pro jak články o vytváření pravidel brány firewall úrovni serveru pomocí jiné metody, přečtěte si článek: 

- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)

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

 
