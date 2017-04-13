<properties
    pageTitle="Správa mezipaměti Azure Redis s Azure Powershellu | Microsoft Azure"
    description="Informace o provádění úkolů správy pro Azure Redis mezipaměti pomocí prostředí PowerShell Azure."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Správa mezipaměti Azure Redis Azure prostředí PowerShell

> [AZURE.SELECTOR]
- [Prostředí PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure rozhraní příkazového řádku](cache-manage-cli.md)

Toto téma ukazuje, jak provádět běžné úkoly, jako vytvořit, aktualizovat a zmenšit instance Azure Redis mezipaměti, jak obnovit přístupových kláves z verze a zobrazení informací o vaší mezipaměti. Úplný seznam rutiny prostředí PowerShell mezipaměti Redis Azure najdete v článku [rutin Azure Redis mezipaměti](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][klasický modelu](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud jste již nainstalovali Azure PowerShell, musíte mít Azure PowerShell verze 1.0.0 nebo novější. Můžete zjistit verzi Azure Powershellu, který jste nainstalovali pomocí tohoto příkazu na příkazovém řádku prostředí PowerShell Azure.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Nejdřív se musíte přihlásit k Azure pomocí tohoto příkazu.

    Login-AzureRmAccount

V dialogovém okně přihlašovací Microsoft Azure zadejte e-mailovou adresu účtu Azure a jeho heslo.

Další Pokud máte víc předplatných Azure, musíte nastavit Azure předplatné. Pokud chcete zobrazit seznam aktuálního předplatného, tento příkaz spusťte.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Chcete-li předplatné, spusťte tento příkaz. V následujícím příkladu je v poli Název předplatného `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Než budete moct použít prostředí Windows PowerShell pomocí Správce prostředků Azure, musí být splněny následující:

- Prostředí Windows PowerShell, verze 3.0 nebo 4.0. Verze Windows Powershellu najdete zadejte:`$PSVersionTable` a ověřte přínosu `PSVersion` 3.0 nebo 4.0. Nainstalovat kompatibilní verzi, najdete v tématu [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Zobrazíte podrobnou nápovědu pro rutina, které vidíte v tomto kurzu získáte pomocí rutiny Get-Help.

    Get-Help <cmdlet-name> -Detailed

Příklad získat nápovědu k `New-AzureRmRedisCache` rutina, typ:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Jak se připojit ke cloudové Government Azure nebo cloudového Čína Azure

Ve výchozím nastavení Azure prostředí je `AzureCloud`, která představuje instance globální Azure cloudu. Připojit k jiné instanci, můžete `Add-AzureRmAccount` příkazu s `-Environment` nebo -`EnvironmentName` přepínač příkazového řádku s požadované prostředí nebo název prostředí.

Chcete-li zobrazit seznam dostupných prostředí, spusťte `Get-AzureRmEnvironment` rutiny.

### <a name="to-connect-to-the-azure-government-cloud"></a>Připojení ke cloudu pro státní správu Azure

Připojení ke cloudu pro státní správu Azure, můžete jedním z následujících příkazů.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

nebo

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Vytvořit mezipaměť v cloudu pro státní správu Azure, můžete jedním z následujících umístění.

-   USGov Virginie
-   USGov Iowa

Další informace o Azure Government Cloud najdete v článku [Microsoft Azure pro státní správu](https://azure.microsoft.com/features/gov/) a [Microsoft Azure Government vývojář Průvodce](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Připojení ke cloudové Čína Azure

Připojení ke cloudu Čína Azure, můžete jedním z následujících příkazů.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

nebo

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Vytvořit mezipaměť v cloudu Čína Azure, můžete jedním z následujících umístění.

-   Čína východ
-   Severní Čína

Další informace o cloudu Čína Azure najdete v článku [AzureChinaCloud Azure provozovaný společností 21Vianet v Číně](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Vlastnosti použité pro PowerShell mezipaměti Redis Azure

Následující tabulka obsahuje vlastnosti a popisy parametrů běžně používaných při vytváření a správa vašich instancí mezipaměti Redis Azure pomocí prostředí PowerShell Azure.

| Parametr          | Popis                                                                                                                                                                                                        | Výchozí  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Jméno               | Název mezipaměti                                                                                                                                                                                                  |          |
| Umístění           | Umístění mezipaměti                                                                                                                                                                                              |          |
| ResourceGroupName  | Název skupiny prostředků ve kterém chcete vytvořit mezipaměti                                                                                                                                                                   |          |
| Velikost               | Velikost mezipaměti. Platné hodnoty jsou: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Počet shards vytvořili při vytváření premium mezipaměti s podporou clusterů. Platné hodnoty jsou: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Určuje SKU mezipaměti. Platné hodnoty jsou: základní, standardní, Premium                                                                                                                                         | Standardní |
| RedisConfiguration | Určuje Redis nastavení. Další informace o jednotlivých nastaveních najdete v následující tabulce [RedisConfiguration vlastnosti](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Označuje, zda je povolen port a protokol SSL.                                                                                                                                                                     | NEPRAVDA    |
| MaxMemoryPolicy    | Byly změněny tento parametr: místo toho použít RedisConfiguration.                                                                                                                                              |          |
| StaticIP           | Hostování mezipaměť v VNET Určuje jedinečný IP adresu v podsítě mezipaměti. Pokud není zadán, jedna z podsítě zvolili za vás.                                                                                                                     |          |
| Podsítě             | Když hostingu mezipaměť v VNET, určuje název podsítě, ve kterém chcete nasadit do mezipaměti.                                                                                                                  |          |
| VirtualNetwork     | Hostování mezipaměť v VNET, určuje číslo ID zdroje VNET ve kterém chcete nasadit mezipaměti.                                                                                                             |          |
| Typ klíče            | Určuje, které přístupové klávesy k opětovnému vytvoření při obnovení přístupových kláves z verze. Platné hodnoty jsou: primárního, sekundárního |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>Vlastnosti RedisConfiguration

| Vlastnost                      | Popis                                                                                                          | Ceny úrovní       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| RDB podporou zálohování            | Zda je povoleno [Redis trvání dat](cache-how-to-premium-persistence.md)                                     | Pouze Premium        |
| RDB úložiště připojovacího řetězce | Připojovací řetězec na účtu úložiště [Redis trvání dat](cache-how-to-premium-persistence.md)       | Pouze Premium        |
| RDB zálohování četnosti          | Zálohování frekvence [Redis trvání dat](cache-how-to-premium-persistence.md)                               | Pouze Premium        |
| vyhrazená maxmemory            | Konfiguruje [pamětí vyhrazené](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro procesy není mezipaměti | Standardní a Premium |
| maxmemory zásad              | Konfiguruje [vyřazení zásad](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro ukládání do mezipaměti           | Všechny úrovně ceny   |
| oznámení událostí keyspace        | Konfiguruje [keyspace oznámení](cache-configure.md#keyspace-notifications-advanced-settings)                     | Standardní a Premium |
| hash max ziplist položky      | Konfiguruje [Optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy          | Standardní a Premium |
| hash max ziplist hodnota        | Konfiguruje [Optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy          | Standardní a Premium |
| nastavení max intset položky        | Konfiguruje [Optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy          | Standardní a Premium |
| zset max ziplist položky      | Konfiguruje [Optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy          | Standardní a Premium |
| zset max ziplist hodnota        | Konfiguruje [Optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy          | Standardní a Premium |
| databáze                     | Konfiguruje počet databází. Tato vlastnost je možné konfigurovat pouze při vytvoření mezipaměti.                          | Standardní a Premium |

## <a name="to-create-a-redis-cache"></a>Chcete-li vytvořit mezipaměť Redis

Nové instance Azure Redis mezipaměti vytvořené pomocí rutinu [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] Při prvním vytvoření mezipaměti Redis v předplatného pomocí portálu Azure portálu zaregistruje `Microsoft.Cache` názvů pro tuto předplatné. Pokud budete chtít vytvořit první mezipaměť Redis v předplatného pomocí prostředí PowerShell, musíte nejdřív zaregistrovat tento obor názvů pomocí následujícího příkazu; jinak rutiny jako `New-AzureRmRedisCache` a `Get-AzureRmRedisCache` nezdaří.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Chcete-li zobrazit seznam dostupných parametrů a jejich popisů `New-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Pokud chcete vytvořit mezipaměť s výchozími parametry, spusťte tento příkaz.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, a `Location` jsou požadované parametry, ale zbytek jsou volitelné a mají výchozí hodnoty. Spuštění příkazu předchozí vytváří instanci standardní SKU Azure Redis mezipaměti s zadaný název, umístění a pole Skupina zdroje, která je minimálně 1 GB velikosti s portem-SSL zakázané.

Chcete-li vytvořit mezipaměť premium, určete velikost P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), nebo P4 (53 GB - 530 GB). Chcete-li povolit clusterů, zadat počet shard pomocí `ShardCount` parametr. Následující příklad vytvoří mezipaměti premium P1 s 3 shards. Mezipaměť premium P1 6 GB velikost, a od jsme zadané tři shards celková velikost je 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

K zadání hodnot `RedisConfiguration` parametr, uzavřete hodnoty do `{}` jako klíč/hodnota dvojice jako `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Následující příklad vytvoří standardní mezipaměti 1 GB s `allkeys-random` maxmemory zásad keyspace oznámení o a nakonfigurována `KEA`. Další informace najdete v tématu [Keyspace oznámení (Upřesnit)](cache-configure.md#keyspace-notifications-advanced-settings) a [Maxmemory zásady a vyhrazená maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Konfigurace nastavení při vytváření mezipaměti databáze

`databases` Možné konfigurovat nastavení pouze během vytváření mezipaměti. Následující příklad vytvoří premium P3 mezipaměti (26 GB) s 48 databáze využívající rutinu [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Další informace o `databases` vlastnost, najdete v článku [Konfigurace serveru výchozí Azure Redis mezipaměti](cache-configure.md#default-redis-server-configuration). Další informace o vytváření mezipaměti pomocí rutinu [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) najdete v předchozí části, [Vytvoření Redis mezipaměti](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Chcete-li aktualizovat Redis mezipaměti

Azure instance Redis mezipaměti se automaticky aktualizují pomocí rutinu [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `Set-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

`Set-AzureRmRedisCache` Rutina slouží k aktualizaci vlastností jako `Size`, `Sku`, `EnableNonSslPort`a `RedisConfiguration` hodnoty. 

Následující příkaz Aktualizovat maxmemory zásad pro Redis mezipaměti s názvem myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Chcete-li změnit velikost mezipaměti Redis

`Set-AzureRmRedisCache`lze změnit velikost mezipaměti Azure Redis instance, kdy `Size`, `Sku`, nebo `ShardCount` měnit vlastnosti. 

>[AZURE.NOTE]Změna velikosti mezipaměti pomocí prostředí PowerShell podléhá stejné omezení a pokyny jako proměnné velikosti mezipaměti z portálu Microsoft Azure. Je možné přizpůsobit pro různé ceny osy s následujícími omezeními.
>
>-  Na vyšší úroveň ceny nelze změnit velikost na dolním cenách osy.
>    -    Nemůžete změnit velikost z mezipaměti **Premium** dolů **Standardní** nebo **základní** mezipaměti.
>    -    Nemůžete změnit velikost ze **Standardní** mezipaměti dolů **základní** mezipaměti.
>-  Je možné přizpůsobit z **základní** mezipaměti pro **Standardní** mezipaměti, ale nemůžete změnit velikost ve stejnou dobu. Pokud potřebujete jinou velikost, můžete udělat následující změny měřítka operaci do požadované velikosti.
>-  Z **základní** mezipaměti nelze změnit velikost přímo do mezipaměti **Premium** . Musí změnit velikost od **základní** **Standardní** změny měřítka najednou a od **Standardní** **Premium** v operaci následující změny měřítka.
>-  Nelze změnit velikost ze větší velikost dolů **C0 (250 MB)** velikost.
>
>Další informace najdete v tématu [jak měřítko Azure Redis mezipaměti](cache-how-to-scale.md).

Následující příklad ukazuje, jak změnit velikost mezipaměti s názvem `myCache` do mezipaměti 2,5 GB. Všimněte si, že tento příkaz použít pro základní nebo standardní mezipaměti.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Po vydané tento příkaz je vrácena stav mezipaměti (podobné k voláte `Get-AzureRmRedisCache`). Všimněte si, že `ProvisioningState` je `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Po dokončení změny velikosti operace `ProvisioningState` se změní na `Succeeded`. Pokud potřebujete udělat následující změny měřítka operace, jako je změna z základní na standardní a změna velikosti, musíte počkat, až do dokončení předchozích operace nebo dojít k chybě podobně jako tento.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Chcete-li získat informace o Redis mezipaměti

Můžete získat informace o mezipaměti pomocí rutiny [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) .

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `Get-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

K vrácení informací o všech mezipaměti v aktuálního předplatného, spusťte `Get-AzureRmRedisCache` bez parametrů.

    Get-AzureRmRedisCache

K vrácení informací o všech mezipaměti v určité skupiny zdrojů, spusťte `Get-AzureRmRedisCache` s `ResourceGroupName` parametr.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

K vrácení informací o konkrétní mezipaměti, spusťte `Get-AzureRmRedisCache` s `Name` parametrů obsahující název mezipaměti a `ResourceGroupName` parametr s skupina zdroje obsahující mezipaměti.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>K načtení přístupových kláves Redis mezipaměti

K načtení přístupových kláves pro mezipaměť, můžete použít rutinu [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Chcete-li zobrazit seznam dostupných parametrů a jejich popisů `Get-AzureRmRedisCacheKey`, spusťte tento příkaz.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

K načtení klíče mezipaměť, zavolejte `Get-AzureRmRedisCacheKey` rutina a předejte název mezipaměť název, který obsahuje mezipaměti skupina zdroje.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Chcete-li obnovit přístupových kláves z verze pro mezipaměť Redis

Chcete-li obnovit přístupových kláves pro mezipaměť, můžete použít rutinu [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `New-AzureRmRedisCacheKey`, spusťte tento příkaz.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
K opětovnému vytvoření primární a sekundární klíč pro mezipaměť, zavolejte `New-AzureRmRedisCacheKey` rutina předat do pole Název pole Skupina zdroje a zadat buď `Primary` nebo `Secondary` pro `KeyType` parametr. V následujícím příkladu se obnoví sekundární přístupová klávesa mezipaměti.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Chcete-li odstranit Redis mezipaměti

Pokud chcete odstranit Redis mezipaměti, získáte pomocí rutiny [AzureRmRedisCache odebrat](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `Remove-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

V následujícím příkladu s názvem mezipaměti `myCache` se odebere.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>K importu Redis mezipaměti

Import dat do instance mezipaměti Redis Azure pomocí `Import-AzureRmRedisCache` rutiny.

>[AZURE.IMPORTANT] Import nebo Export je dostupný jenom u [vrstvy premium](cache-premium-tier-intro.md) mezipamětí. Další informace o importu a exportu najdete v tématu [Import a Export dat do mezipaměti Redis Azure](cache-how-to-import-export-data.md).

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `Import-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Následující příkaz k importu dat z objektů blob nastavil přidružení zabezpečení uri do mezipaměti Redis Azure.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Chcete-li exportovat Redis mezipaměti

Export dat z instance mezipaměti Redis Azure pomocí `Export-AzureRmRedisCache` rutiny.

>[AZURE.IMPORTANT] Import nebo Export je dostupný jenom u [vrstvy premium](cache-premium-tier-intro.md) mezipamětí. Další informace o importu a exportu najdete v tématu [Import a Export dat do mezipaměti Redis Azure](cache-how-to-import-export-data.md).

Chcete-li zobrazit seznam dostupných parametrů a jejich popisů `Export-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Tento příkaz: Exportuje dat z Azure Redis mezipaměti instance do kontejneru nastavil uri přidružení zabezpečení.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Restartujte Redis mezipaměti

Restartujte počítač používá instance mezipaměti Redis Azure `Reset-AzureRmRedisCache` rutiny.

>[AZURE.IMPORTANT] Restartujte počítač je dostupný jenom u [vrstvy premium](cache-premium-tier-intro.md) mezipaměti. Další informace o restartování mezipaměti najdete v článku [Správa mezipaměti – restartujte](cache-administration.md#reboot).

Chcete-li zobrazit seznam dostupných parametrů a jejich popisy `Reset-AzureRmRedisCache`, spusťte tento příkaz.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Tento příkaz restartuje uzlech zadaný mezipaměti.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Další kroky

Další informace o použití prostředí Windows PowerShell s Azure, naleznete v následujících zdrojích:

- [Azure Redis mezipaměti rutina si přečtěte následující dokumentaci na webu MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Rutiny pro správce prostředků Azure](http://go.microsoft.com/fwlink/?LinkID=394765): Naučte se používat rutin v modulu AzureResourceManager.
- [Používání zdrojů skupiny pro správu Azure prostředků](../resource-group-template-deploy-portal.md): Naučte se vytvářet a spravovat skupiny zdrojů v portálu Azure.
- [Azure blogu](http://blogs.msdn.com/windowsazure): Další informace o nových funkcích v Azure.
- [Blog prostředí Windows PowerShell](http://blogs.msdn.com/powershell): Další informace o nových funkcích v prostředí Windows PowerShell.
- ["Přece, skriptování hoch!" Blog](http://blogs.technet.com/b/heyscriptingguy/): získání praktických tipy a triky z komunity prostředí Windows PowerShell.
