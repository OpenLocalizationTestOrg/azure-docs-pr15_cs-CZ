<properties
    pageTitle="Vytvoření fondu pružná databáze s C# | Microsoft Azure"
    description="Použijte C# databáze vývoj techniky pro vytvoření fondu scalable pružná databáze v databázi SQL Azure, sdílení zdrojů mezi mnoho databází."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Vytvoření fondu pružná databáze s C a #x 23.

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-create-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Tento článek popisuje, jak používat C# k vytvoření fondu pružná databáze Azure SQL se [Knihovny databáze SQL Azure pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Vytvoření samostatného databáze SQL najdete v tématu [použití C# k vytvoření databáze SQL s knihovnou SQL databáze pro .NET](sql-database-get-started-csharp.md).

Knihovna databáze SQL Azure pro .NET poskytuje [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md)-na základě rozhraní API, které zalamuje [Na základě správce prostředků SQL databáze REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Mnoho nových funkcí databáze SQL, jsou podporované jenom při použití [Správce prostředků Azure nasazení modelu](../azure-resource-manager/resource-group-overview.md), takže byste měli vždy používat nejnovější **knihovnou správy databáze SQL Azure pro .NET ([dokumenty](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starší [knihovny založené na modelu klasické nasazení](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podporuje pouze z důvodu zpětné kompatibility, proto doporučujeme používat novější knihovny na základě správce prostředků.

Kroky v tomto článku, je potřeba k provedení těchto věcí:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatný účet** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Visual Studia. Bezplatné kopii Visual Studiu najdete na stránce [Visual Studio stáhne](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Vytvoření aplikace konzoly a nainstalujte požadované knihovny

1. Spusťte aplikaci Visual Studio.
2. Klikněte na **soubor** > **nové** > **projektu**.
3. Vytvoření C# **Aplikace konzoly** a nazvěte ji: *SqlElasticPoolConsoleApp*


Pokud chcete vytvořit databázi SQL s C#, načte požadované řízení knihovny (pomocí [konzoly Správce balíčku](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klikněte na **Nástroje** > **Správce NuGet balíčků** > **Konzoly balíčku správce**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` nainstalovat [Microsoft Azure SQL správy knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` nainstalovat [Microsoft Azure správce prostředků knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` nainstalovat [Microsoft Azure běžné Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Příklady v tomto článku synchronní formulář každý požadavek rozhraní API a blokovat až do dokončení ZBÝVAJÍCÍ zavolat na základní služby. Způsoby asynchronní k dispozici.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Vytvoření fondu pružná databáze SQL - C# příklad

V následujícím příkladu vytvoří pole Skupina zdroje, server, pravidlo brány firewall, pružná fondu a potom vytvoří databázi SQL ve fondu. Zobrazit, [vytvořit hlavní k přístupu k prostředkům službu](#create-a-service-principal-to-access-resources) získat `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` proměnné.

Obsah **Program.cs** nahraďte následujícím a aktualizovat `{variables}` hodnotami aplikace (nezahrnujte `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```





## <a name="create-a-service-principal-to-access-resources"></a>Vytvoření služby základní při přístupu k prostředkům

Tento skript Powershellu vytvoří Active Directory (AD) aplikace a služby jistiny, potřebujeme ověření naše C# aplikace. Skript výstupy hodnoty, které potřebujeme pro v předchozím příkladu C#. Podrobné informace najdete v článku [použití Azure vytvořit hlavní k přístupu k prostředkům službu](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret


  

## <a name="next-steps"></a>Další kroky

- [Správa vašeho fondu](sql-database-elastic-pool-manage-csharp.md)
- [Vytvoření pružná úlohy](sql-database-elastic-jobs-overview.md): pružná úlohy umožňují kontrolovat libovolný počet databáze do fondu skripty T-SQL.
- [Rozšiřování s databáze SQL Azure](sql-database-elastic-scale-introduction.md): umožňuje rozšiřování pružná databázové nástroje.

## <a name="additional-resources"></a>Další zdroje informací

- [Databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Rozhraní API správy Azure zdroje](https://msdn.microsoft.com/library/azure/dn948464.aspx)
