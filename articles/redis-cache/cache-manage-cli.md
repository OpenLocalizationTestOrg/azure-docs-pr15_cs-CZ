<properties 
    pageTitle="Jak vytvořit a spravovat Azure Redis mezipaměti pomocí rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku) | Microsoft Azure" 
    description="Zjistěte, jak nainstalovat Azure rozhraní příkazového řádku na libovolné platformy, jak se používá pro připojení k účtu Azure a jak vytvářet a spravovat Redis mezipaměti z Azure rozhraní příkazového řádku." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Jak vytvořit a spravovat Azure Redis mezipaměti pomocí rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku)

> [AZURE.SELECTOR]
- [Prostředí PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure rozhraní příkazového řádku](cache-manage-cli.md)

Rozhraní příkazového řádku Azure je skvělý způsob, jak spravovat infrastrukturu Azure z libovolné platformy. V tomto článku se dozvíte, jak vytvářet a spravovat svůj instance mezipaměti Redis Azure pomocí rozhraní příkazového řádku Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Můžete vytvořit a spravovat instance mezipaměti Redis Azure pomocí rozhraní příkazového řádku Azure, musíte nejdřív udělat následující kroky.

-   Musíte mít účet Azure. Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) v pár minut.
-   [Instalace Azure rozhraní příkazového řádku](../xplat-cli-install.md).
-   Připojení instalace Azure rozhraní příkazového řádku s osobním účtem Azure nebo se pracovní nebo školní účet Azure a přihlaste se pomocí rozhraní příkazového řádku Azure `azure login` příkaz. Pochopit rozdíl a zvolte, najdete v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-connect.md).
-   Před spuštěním některou z následujících příkazů, přepněte Azure rozhraní příkazového řádku do režimu správce prostředků spuštěním `azure config mode arm` příkaz. Další informace najdete v tématu [Nastavení režimu správce prostředků Azure](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis vlastnosti mezipaměti

Následující vlastnosti jsou při vytváření a aktualizace instance Redis mezipaměti.

| Vlastnost            | Přepínač                                  | Popis                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jméno                | -n, – název                              | Název Redis mezipaměti.                                                                                                                                                                                                                             |
| pole Skupina zdroje      | -g, – skupina zdroje                    | Název skupiny zdrojů.                                                                                                                                                                                                                          |
| umístění            | -l, – umístění                          | Umístění k vytvoření mezipaměti.                                                                                                                                                                                                                            |
| velikost                | -z, – velikost                              | Velikost mezipaměti Redis. Platné hodnoty: [C0 C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -x, – sku                               | Redis SKU. Musí mít některou z: [Základní standardní, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, – enable bez--port ssl               | Vlastnost EnableNonSslPort Redis mezipaměti. Pokud chcete povolit Port SSL nejsou pro mezipaměť přidat tohoto příznaku                                                                                                                                    |
| Redis konfigurace | -c, – konfigurace redis               | Redis konfigurace. Zadejte řetězec formátu JSON konfigurace klíčů a hodnot v tomto poli. Formát: "{" ":""," ":" "}"                                                                                                                                     |
| Redis konfigurace | -f, – soubor konfigurace redis          | Redis konfigurace. Zadejte cestu k souboru obsahující konfigurační klíče a hodnoty tady. Formát souboru položky: {"": "","": ""}                                                                                                                |
| Počet shard         | -r, – shard počet                       | Počet Shards na mezipaměti clusteru Premium s clusterů vytvořit.                                                                                                                                                                               |
| Virtuální sítě     | -v, – virtuální sítě                   | Hostování mezipaměť v VNET identifikátor přesné ARM zdroje virtuální sítě pro nasazení mezipaměti redis v. Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Typ klíče            | -t, – typ klíče                          | Typ key k obnovení. Platné hodnoty: [primárního, sekundárního]                                                                                                                                                                                             |
| StaticIP            | -p, – statické ip < statické ip >             | Hostování mezipaměť v VNET Určuje jedinečný IP adresu v podsítě mezipaměti. Pokud není zadán, jedna z podsítě zvolili za vás.                                                                                                |
| Podsítě              | t, – podsítě<subnet>                    | Když hostingu mezipaměť v VNET, určuje název podsítě, ve kterém chcete nasadit do mezipaměti.                                                                                                                                                    |
| VirtualNetwork      | -v, – virtuální sítě < virtuální síť > | Hostování mezipaměť v VNET identifikátor přesné ARM zdroje virtuální sítě pro nasazení mezipaměti redis v. Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Předplatné        | – Ne, – předplatné                      | Identifikátor předplatného.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Zobrazit všechny příkazy Redis mezipaměti

Zobrazit všechny příkazy Redis mezipaměti a jejich parametry, použít `azure rediscache -h` příkaz.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Vytvoření Redis mezipaměti

Pokud chcete vytvořit Redis mezipaměti, použijte tento příkaz:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Další informace o tento příkaz Spustit `azure rediscache create -h` příkaz.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Odstranění existující mezipaměti Redis

Pokud chcete odstranit mezipaměť Redis, použijte tento příkaz:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Další informace o tento příkaz Spustit `azure rediscache delete -h` příkaz.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Seznam všech Redis mezipaměti v rámci předplatného nebo skupina zdroje

Chcete-li zobrazit všechny Redis mezipaměti v rámci předplatného nebo skupina zdroje, použijte tento příkaz:

    azure rediscache list [options]

Další informace o tento příkaz Spustit `azure rediscache list -h` příkaz.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Zobrazit vlastnosti stávající mezipaměti Redis

Pokud chcete zobrazit vlastnosti stávající mezipaměti Redis, použijte tento příkaz:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Další informace o tento příkaz Spustit `azure rediscache show -h` příkaz.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Změna nastavení stávající mezipaměti Redis

Pokud chcete změnit nastavení stávající mezipaměti Redis, použijte tento příkaz:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Další informace o tento příkaz Spustit `azure rediscache set -h` příkaz.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Prodloužení platnosti předplatného klávesu ověřování pro stávající mezipaměti Redis

Obnovení klávesu ověřování pro stávající mezipaměti Redis, použijte tento příkaz:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Určení `Primary` nebo `Secondary` pro `key-type`.

Další informace o tento příkaz Spustit `azure rediscache renew-key -h` příkaz.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Seznam primární a sekundární klíče stávající mezipaměti Redis

Seznam zobrazující primární a sekundární klíče stávající mezipaměti Redis můžete tento příkaz:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Další informace o tento příkaz Spustit `azure rediscache list-keys -h` příkaz.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
