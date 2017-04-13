<properties
    pageTitle="Jak používat Azure Redis mezipaměti s Node.js | Microsoft Azure"
    description="Začínáme s Azure Redis mezipaměti pomocí Node.js a node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Jak pomocí Node.js Azure Redis mezipaměti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [TECHNOLOGIE ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis mezipaměť vám umožňuje přístup k zabezpečení, vyhrazené Redis mezipaměť, spravuje Microsoft. Mezipaměť přístupný z libovolné aplikace v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s mezipaměti Redis Azure pomocí Node.js. Jiný příklad použití pomocí Node.js Azure Redis mezipaměti najdete v článku [Vytvoření aplikace Node.js konverzace s Socket.IO na webu Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Nainstalujte [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Tento kurz používá [node_redis](https://github.com/mranney/node_redis). Příklady použití jiných klientech Node.js naleznete v dokumentaci jednotlivé pro klienty Node.js uvedený v části [Node.js Redis klienty](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Vytvořit mezipaměť Redis na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Získat klíče název a přístup Host (hostitel)

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Připojení k mezipaměti bezpečně protokolem SSL

Nejnovější buildy [node_redis](https://github.com/mranney/node_redis) podporují pro připojení do mezipaměti Redis Azure protokolem SSL. Následující příklad ukazuje, jak se připojit k mezipaměti Redis Azure pomocí SSL koncového bodu 6380. Nahrazení `<name>` s názvem mezipaměť a `<key>` buď kód primární a sekundární jako popisu v předchozí části [získat hostitele název a přístup klíče](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání něco do mezipaměti a načíst je

Následující příklad ukazuje, jak připojit k instanci Azure Redis mezipaměti a uložit a načíst položky z mezipaměti. Další příklady použití Redis s klientem [node_redis](https://github.com/mranney/node_redis) naleznete v tématu [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Výstup:

    OK
    value


## <a name="next-steps"></a>Další kroky

- [Povolení diagnostiky mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , budete moct [Sledování](cache-how-to-monitor.md) stavu mezipaměť.
- Přečtěte si oficiální [Redis si přečtěte následující dokumentaci](http://redis.io/documentation).



