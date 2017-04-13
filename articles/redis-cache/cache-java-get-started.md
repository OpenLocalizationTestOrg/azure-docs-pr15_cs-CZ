<properties
   pageTitle="Jak mezipaměti Redis Azure pomocí jazyka Java | Microsoft Azure"
    description="Začínáme s Azure Redis mezipaměti pomocí jazyka Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Jak mezipaměti Redis Azure pomocí jazyka Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [TECHNOLOGIE ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis mezipaměti nabízí přístup k vyhrazené Redis mezipaměti, spravuje Microsoft. Mezipaměť přístupný z libovolné aplikace v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s mezipaměti Redis Azure pomocí jazyka Java.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[Jedis](https://github.com/xetorthio/jedis) - Java klienta pro Redis

Tento kurz používá Jedis, ale můžete použít libovolný Java klient uvedený v části [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Vytvořit mezipaměť Redis na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Získat klíče název a přístup Host (hostitel)

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Povolení koncového bodu protokol SSL

V některých klientech Redis nemusí podporovat SSL a ve výchozím nastavení [není SSL port není k dispozici pro nové instance Azure Redis mezipaměti](cache-configure.md#access-ports). Při psaní tohoto textu nepodporuje klientovi [Jedis](https://github.com/xetorthio/jedis) SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání něco do mezipaměti a načíst je

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Další kroky

- [Povolit diagnostiky mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , budete moct [Sledování](https://msdn.microsoft.com/library/azure/dn763945.aspx) stavu mezipaměť.
- Přečtěte si oficiální [Redis si přečtěte následující dokumentaci](http://redis.io/documentation).

