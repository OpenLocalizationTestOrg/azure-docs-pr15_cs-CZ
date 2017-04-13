<properties
   pageTitle="Získat požadované hodnoty pro ověřování aplikace pro přístup k databázi SQL z kódu | Microsoft Azure"
   description="Vytvořte hlavní název služby pro přístup k databázi SQL z kódu."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Získat požadované hodnoty pro ověřování aplikace pro přístup k databázi SQL z kódu

Můžete vytvořit a spravovat databáze SQL z kódu zaregistrujte aplikace v doméně Azure Active Directory (AAD) v předplatného, kde byly vytvořeny Azure zdroje.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Vytvořte hlavní službu při přístupu k prostředkům z aplikace

Musíte mít nejnovější [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) nainstalovanou a spuštěnou. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

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




## <a name="see-also"></a>Viz taky

- [Vytvoření databáze SQL pomocí C#](sql-database-get-started-csharp.md)
- [Připojení k databázi SQL Azure Active Directory ověřováním](sql-database-aad-authentication.md)


