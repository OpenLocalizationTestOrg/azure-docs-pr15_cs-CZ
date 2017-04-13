<properties
    pageTitle="Správa databáze Azure SQL pomocí prostředí PowerShell | Microsoft Azure"
    description="Azure SQL databáze Správa prostřednictvím Powershellu."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Správa databáze Azure SQL pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-manage-portal.md)
- [Skalární funkce (SSMS)](sql-database-manage-azure-ssms.md)
- [Prostředí PowerShell](sql-database-manage-powershell.md)

Toto téma ukazuje rutinách Powershellu, které slouží k provádění mnoha úkolů databáze SQL Azure. Úplný seznam najdete v článku [Rutiny databáze SQL Azure](https://msdn.microsoft.com/library/mt574084.aspx).


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření skupiny prostředků pro naše databáze SQL a související materiály Azure s rutinu [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) .

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Další informace najdete v tématu [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).
Ukázka skriptu najdete v článku [vytvoření databáze SQL skript Powershellu](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Vytvoření databáze SQL serveru

Vytvoření databáze SQL serveru s rutinu [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) . Nahraďte *server1* název svého serveru. Názvy serverů musí být jedinečný na všech serverech databáze SQL Azure. Pokud již není přijato název serveru, dojde k chybě. Tento příkaz může trvat několik minut. Skupina zdroje, musí existovat v předplatném.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Další informace najdete v tématu [Co je databáze SQL](sql-database-technical-overview.md). Ukázka skriptu najdete v článku [vytvoření databáze SQL skript Powershellu](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Vytvořit pravidlo databáze SQL serveru brány firewall

Vytvořte pravidlo brány firewall pro přístup k serveru s rutinu [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) . Spusťte tento příkaz nahrazení IP adresy zahájení a ukončení platné hodnoty pro vašeho klienta. Pole Skupina zdroje a server, musí existovat v předplatném.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Další služby Azure přístupu k serveru, vytvořit pravidlo brány firewall a nastavení `-StartIpAddress` a `-EndIpAddress` k **0.0.0.0**. Toto pravidlo zvláštní brány firewall umožňuje všechny Azure přenosy pro přístup k serveru.

Další informace najdete v tématu [Firewall databáze SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Ukázka skriptu najdete v článku [vytvoření databáze SQL skript Powershellu](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Vytvoření databáze SQL (prázdné)

Vytvoření databáze pomocí rutinu [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) . Pole Skupina zdroje a server, musí existovat v předplatném. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Další informace najdete v tématu [Co je databáze SQL](sql-database-technical-overview.md). Ukázka skriptu najdete v článku [vytvoření databáze SQL skript Powershellu](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Změna úrovně výkonu databázi SQL

Velikost databáze nahoru nebo dolů můžete použít rutinu [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) . Pole Skupina zdroje, server a databázi, musí existovat v předplatném. Nastavit `-RequestedServiceObjectiveName` jednoduché řádkování (například následující úryvek) pro základní osy. Nastavte *S0*, *S1*, *P1*, *P6*atd., podobně jako v předchozím příkladu pro ostatních vrstev.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Další informace najdete v tématu [Možnosti SQL databáze a výkonu: porozumět tomu, co je k dispozici v každé vrstvy služeb](sql-database-service-tiers.md). Ukázka skriptu najdete v článku [skript Powershellu vzorku změnit úroveň osy a výkonu služby SQL databáze](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Zkopírujte databázi SQL na stejný server

Zkopírujte databázi SQL na stejný server s rutinu [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) . Nastavit `-CopyServerName` a `-CopyResourceGroupName` stejné hodnoty jako zdroj databáze prostředků serveru a skupiny.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Další informace najdete v tématu [kopírování databáze SQL Azure](sql-database-copy.md). Ukázka skriptu v tématu [kopírování databázi SQL skript Powershellu](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Odstranit databázi SQL

Odstranění databáze SQL pomocí rutiny [AzureRmSqlDatabase odebrat](https://msdn.microsoft.com/library/azure/mt619368.aspx) . Pole Skupina zdroje, server a databázi, musí existovat v předplatném.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Odstranění databáze SQL serveru

Odstranění serveru s rutinu [AzureRmSqlServer odebrat](https://msdn.microsoft.com/library/azure/mt603488.aspx) .

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Vytváření a Správa skupin pružná databáze pomocí prostředí PowerShell

Podrobnosti o vytváření fondů pružná databáze pomocí Powershellu najdete v tématu [Vytvoření nové fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md).

Podrobnosti o správě fondů pružná databáze pomocí Powershellu najdete v tématu [sledování a správa fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Související informace

- [Rutiny pro správu databáze Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Reference pro rutiny Azure](https://msdn.microsoft.com/library/azure/dn708514.aspx)
