<properties
    pageTitle="Nové nastavení databáze SQL pomocí prostředí PowerShell | Microsoft Azure"
    description="Podívejte se na vytvoření databáze SQL pomocí prostředí PowerShell. Běžné úkoly instalace databáze můžete spravovat pomocí rutin prostředí PowerShell."
    keywords="Vytvořit novou databázi sql, nastavení databáze"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Vytvoření databáze SQL a provádění běžných úloh nastavení databáze pomocí rutin prostředí PowerShell


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-get-started.md)
- [Prostředí PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Naučte se vytvářet databázi SQL pomocí rutin prostředí PowerShell. (Pro vytváření pružná databází, v tématu [Vytvoření nové fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Nastavení databáze: vytvoření pole Skupina zdroje, server a bránu firewall pravidla

Až budete mít přístup, abyste mohli spouštět rutiny proti předplatného vybrané Azure, dalším krokem je vytvoření skupina zdroje, který obsahuje server, kde bude vytvořen databáze. Další příkaz použít jakékoli platné umístění zvoleného můžete upravit. Spuštění **(Get-AzureRmLocation | Kde objekt {$_. Poskytovatelé - eq "Microsoft.Sql"}). Umístění** zobrazte seznam platná umístění.

Spusťte tento příkaz Vytvořit skupinu zdrojů:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Vytvoření serveru

SQL databáze vytvořené v databázi SQL Azure serverů. Spustit [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) k vytvoření serveru. Název serveru musí být jedinečný na všechny servery databáze SQL Azure. Pokud již není přijato název serveru, dojde k chybě. Také vhodné poznamenat, je, aby tento příkaz může trvat několik minut dokončete. Příkaz použít libovolný platný umístění zvoleného můžete upravit, ale je vhodné dát na stejném místě, které jste použili pro skupina zdroje vytvořili v předchozím kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Když spustíte tento příkaz, zobrazí se výzva k zadání uživatelského jména a hesla. Není zadejte Azure přihlašovací údaje. Místo toho zadejte uživatelské jméno a heslo pro vytvoření jako správce serveru. Skript na konci tohoto článku ukazuje, jak nastavit přihlašovací údaje serveru v kódu.

Podrobnosti o server zobrazí úspěšně vytvořený serveru.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurace brány firewall pravidla serveru umožňuje přístup k serveru

Přístup k serveru, musíte vytvořit pravidlo brány firewall. Spustit [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) příkaz nahrazení IP adresy zahájení a ukončení platné hodnoty pro počítač.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Po jeho úspěšné vytvoření zobrazovat podrobnosti pravidla bránu firewall.

Pokud chcete povolit další služby Azure k přístupu serveru, přidat pravidlo bránu firewall a nastavení StartIpAddress a EndIpAddress 0.0.0.0. Pokud chcete toto pravidlo umožňuje Azure přenos z Azure předplatné pro přístup k serveru.

Další informace najdete v tématu [Firewall databáze SQL Azure](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Vytvoření databáze SQL

Nyní máte skupina zdroje, server a bránu firewall pravidlo nakonfigurovaný tak, aby se dostanete na server.

Následující [nový AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) příkaz vytvoří databázi SQL (prázdné) ve vrstvě standardní služba s úrovní S1 výkon:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Po úspěšném vytvoření databáze se zobrazí podrobnosti o databázi.

## <a name="create-a-sql-database-powershell-script"></a>Vytvoření databáze SQL skript Powershellu

Tento skript Powershellu vytvoří databázi SQL a jeho závislé prostředky. Nahradit vše `{variables}` s hodnotami specifické pro svoje předplatné a zdroje (při nastavení hodnoty odebrat **{}** ).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Další kroky
Po vytvoření databáze SQL a nastavení provádět základní databáze, budete připraveni tyto věci:

- [Správa databáze SQL pomocí prostředí PowerShell](sql-database-manage-powershell.md)
- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Další zdroje informací

- [Rutiny databáze azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Databáze Azure SQL](https://azure.microsoft.com/documentation/services/sql-database/)
