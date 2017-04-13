<properties
    pageTitle="Zkuste SQL databáze: Použití C# k vytvoření databáze SQL | Microsoft Azure"
    description="Zkuste databáze SQL k vývoji aplikací SQL a C# a vytvoření databáze SQL Azure pomocí C# pomocí knihovnu SQL databáze pro .NET."
    keywords="Zkuste sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Zkuste SQL databáze: Použití C# k vytvoření databáze SQL s knihovnou SQL databáze pro .NET


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [Prostředí PowerShell](sql-database-get-started-powershell.md)

Naučte se používat C# vytvoření databáze Azure SQL pomocí [Microsoft Azure SQL knihovnou správy pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) Tento článek popisuje, jak vytvořit jednu databázi SQL a C#. Vytvoření skupiny pružná databáze najdete v tématu [Vytvoření fondu pružná databáze](sql-database-elastic-pool-create-csharp.md).

Knihovna správy databáze SQL Azure .NET poskytuje [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md)-na základě rozhraní API, které zalamuje [Na základě správce prostředků SQL databáze REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Mnoho nových funkcí databáze SQL, jsou podporované jenom při použití [Správce prostředků Azure nasazení modelu](../azure-resource-manager/resource-group-overview.md), takže byste měli vždy používat nejnovější **knihovnou správy databáze SQL Azure pro .NET ([dokumenty](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starší [knihovny založené na modelu klasické nasazení](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podporuje pouze z důvodu zpětné kompatibility, proto doporučujeme používat novější knihovny na základě správce prostředků.

Kroky v tomto článku, je potřeba k provedení těchto věcí:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatný účet** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Visual Studia. Bezplatné kopii Visual Studiu najdete na stránce [Visual Studio stáhne](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] Tento článek vytvoří novou, prázdnou databázi SQL. Upravte metodu *CreateOrUpdateDatabase(...)* v následujícím příkladu zkopírujte databází, změnit velikost databáze, vytvořte databáze ve fondu atd. Další informace najdete v tématu [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) a [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) tříd.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Vytvoření aplikace konzoly a nainstalujte požadované knihovny

1. Spusťte aplikaci Visual Studio.
2. Klikněte na **soubor** > **nové** > **projektu**.
3. Vytvoření C# **Aplikace konzoly** a nazvěte ji: *SqlDbConsoleApp*


Pokud chcete vytvořit databázi SQL s C#, načte požadované řízení knihovny (pomocí [konzoly Správce balíčku](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klikněte na **Nástroje** > **Správce NuGet balíčků** > **Konzoly balíčku správce**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` a nainstalujte nejnovější [Knihovnou správy Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` nainstalovat [Microsoft Azure správce prostředků knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` nainstalovat [Microsoft Azure běžné Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Příklady v tomto článku synchronní formulář každý požadavek rozhraní API a blokovat až do dokončení ZBÝVAJÍCÍ zavolat na základní služby. Způsoby asynchronní k dispozici.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Vytvoření databáze SQL serveru, pravidlo brány firewall a databáze SQL - C# příklad

Následující příklad vytvoří skupina zdroje, server, pravidlo brány firewall a databázi SQL. Zobrazit, [vytvořit hlavní k přístupu k prostředkům službu](#create-a-service-principal-to-access-resources) získat `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` proměnné.

Obsah **Program.cs** nahraďte následujícím a aktualizovat `{variables}` hodnotami aplikace (nezahrnujte `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
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

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


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

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
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



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
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
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
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
Teď, když jste zkusili databáze SQL a nastavení databáze pomocí C#, jste připraveni na v následujících článcích:

- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Další zdroje informací

- [Databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Databáze třídy](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
