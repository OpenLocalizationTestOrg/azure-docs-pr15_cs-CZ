<properties
    pageTitle="Jak používat Azure Redis mezipaměti s Python | Microsoft Azure"
    description="Začínáme s Azure Redis mezipaměti pomocí Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Jak pomocí Python Azure Redis mezipaměti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [TECHNOLOGIE ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Toto téma ukazuje, jak začít s mezipamětí Redis Azure pomocí Python.


## <a name="prerequisites"></a>Zjistit předpoklady pro

Nainstalujte [redis kopírovat](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Vytvořit mezipaměť Redis na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Získat klíče název a přístup Host (hostitel)

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Povolení koncového bodu protokol SSL

V některých klientech Redis nemusí podporovat SSL a ve výchozím nastavení [není SSL port není k dispozici pro nové instance Azure Redis mezipaměti](cache-configure.md#access-ports). Při psaní tohoto textu nepodporuje klientovi [redis kopírovat](https://github.com/andymccurdy/redis-py) SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání něco do mezipaměti a načíst je


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Nahrazení `<name>` s názvem vaší mezipaměti a `key` s přístupové klávesy.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
