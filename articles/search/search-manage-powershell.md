<properties 
    pageTitle="Spravovat vyhledávání Azure pomocí skriptů Powershellu | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu" 
    description="Správa služby Azure vyhledávání pomocí skriptů Powershellu. Vytvoření nebo aktualizace Azure vyhledávací služby a správa Azure hledání správy klíčů" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Správa služby Azure vyhledávání pomocí prostředí PowerShell
> [AZURE.SELECTOR]
- [Portál](search-manage.md)
- [Prostředí PowerShell](search-manage-powershell.md)
- [ROZHRANÍ REST API](search-get-started-management-api.md)

Toto téma popisuje prostředí PowerShell příkazy k provádění mnoha úkolů správy služby Azure vyhledávání. Jsme provede jednotlivými vytváření vyhledávací služby, změny měřítka a správu své rozhraní API klíče.
Tyto příkazy paralelní správy možnosti dostupné v [Azure hledání správy REST API](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Zjistit předpoklady pro
 
- Musí mít Azure PowerShell 1.0 nebo vyšší. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).
- Musíte být přihlášení k předplatnému Azure v prostředí PowerShell podle níže uvedeného popisu.

Nejdřív musíte přihlášení k Azure pomocí tohoto příkazu:

    Login-AzureRmAccount

V dialogovém okně Microsoft Azure přihlášení zadejte e-mailovou adresu účtu Azure a jeho heslo.

Můžete taky můžete [interaktivně služby základní přihlášení](../resource-group-authenticate-service-principal.md).

Pokud máte víc předplatných Azure, musíte nastavit Azure předplatné. Pokud chcete zobrazit seznam aktuálního předplatného, tento příkaz spusťte.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Chcete-li předplatné, spusťte tento příkaz. V následujícím příkladu je v poli Název předplatného `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Příkazy, které vám pomůžou začít

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Další kroky
    
Teď, když se vytvoří služby, může trvat další kroky: vytvořit [index](search-what-is-an-index.md), [dotaz index](search-query-overview.md)a nakonec vytvářet a spravovat vlastní aplikaci Vyhledávací služby používá Azure hledání.

- [Vytvoření indexu vyhledávání Azure na portálu Azure](search-create-index-portal.md)

- [Dotaz indexu vyhledávání Azure pomocí Průzkumníka hledání na portálu Azure](search-explorer.md)

- [Nastavení indexování k načtení dat z jiných služeb](search-indexer-overview.md)

- [Použití vyhledávání Azure v .NET](search-howto-dotnet-sdk.md)

- [Analýza provozu Azure hledání](search-traffic-analytics.md)
