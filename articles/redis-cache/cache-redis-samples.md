<properties 
    pageTitle="Azure vzorky Redis mezipaměti | Microsoft Azure" 
    description="Naučte se používat Azure Redis mezipaměti" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure Redis mezipaměti vzorky 

Toto téma obsahuje seznam vzorky Azure Redis mezipaměti, scénáře například připojení mezipaměti, čtení a zápis dat do a z mezipaměti a pomocí zprostředkovatelů ASP.NET Redis mezipaměti. Některé vzorky ke stažení projektů, a některé obsahují podrobné pokyny a zahrnout fragmenty kódu ale nejsou připojeny ke stažení projektu.

## <a name="hello-world-samples"></a>Ahoj světa vzorky

Příklady v této části zobrazit základní informace o připojení k instanci Azure Redis mezipaměti a čtení a zápis dat do mezipaměti pomocí různých jazycích a Redis klienty.

Příklad [Vítáme](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukazuje, jak provádět různé operace mezipaměti pomocí klienta .NET [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

Tento příklad ukazuje, jak:

-   Použít různé možnosti připojení
-   Čtení a zápis objekty do a z mezipaměti pomocí synchronní a asynchronní operace
-   Vrátí hodnoty zadané kláves taky pomocí příkazů Redis MGET/MSET
-   Provádět operace transakční Redis
-   Práce se seznamy Redis a seřazené sady
-   Úložiště objektů .NET pomocí JsonConvert serializers
-   Použití sady Redis implementovat označování
-   Práce s Redis obrázku

Další informace najdete v dokumentaci k [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) na github a další příklady použití najdete v článku testování [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) .

[Jak používat Azure Redis mezipaměti s Python](cache-python-get-started.md) ukazuje, jak začít s Azure Redis mezipaměti pomocí Python a klientovi [redis kopírovat](https://github.com/andymccurdy/redis-py) .

[Práce s objekty .NET v mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) dozvíte jedním ze způsobů serializovat .NET objekty, abyste mohli psát, aby a číst z mezipaměti Redis Azure instance. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Použití Redis mezipaměti jako měřítko, základní ASP.NET SignalR

Ukázka [Použití Redis mezipaměti jako měřítko si základní technologie ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) znázorňuje, jak můžete pomocí Azure Redis mezipaměti jako základní SignalR. Další informace o základní najdete v článku [SignalR Scaleout s Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis mezipaměti zákazníka dotazu ukázka

Tento příklad ukazuje porovná výkonu mezi přístup k datům z mezipaměti a přístup k datům z trvalé úložiště. V tomto příkladu obsahuje dva projekty.

-   [Ukázka, jak můžete Redis mezipaměti zlepšíte výkon při ukládání do mezipaměti dat](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Počáteční hodnota databáze a mezipaměti pro videa](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Stav relace ASP.NET a ukládání výstupu do mezipaměti

Ukázka [Použití Azure Redis mezipaměti ukládat ASP.NET SessionState a OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) znázorňuje, jak můžete používat k ukládání ASP.NET relace a výstupní mezipaměti pomocí zprostředkovatelů SessionState a OutputCache pro Redis Azure Redis mezipaměti.

## <a name="manage-azure-redis-cache-with-maml"></a>Správa mezipaměti Azure Redis s MAML

[Správa mezipaměti Redis Azure pomocí knihovny správy Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) příklad znázorňuje, jak můžete pomocí knihovny správy Azure Správa – (Vytvořit / aktualizovat a odstranit) mezipaměť. 

## <a name="custom-monitoring-sample"></a>Vlastní sledování ukázka

Příklad [dat aplikace Access Redis mezipaměti sledování](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) znázorňuje přístupu k monitorování dat pro mezipaměť Redis Azure mimo portálu Azure.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Styl Twitter klonovat vytvořených pomocí PHP a Redis

Ukázka [Retwis](https://github.com/SyntaxC4-MSFT/retwis) je Redis Vítáme. Je minimální klonovat sociálních sítí Twitter styl vytvořených pomocí Redis a PHP pomocí klienta [Predis](https://github.com/nrk/predis) . Zdrojový kód je určená být hodně jednoduché a současně zobrazíte různé Redis datové struktury.

## <a name="bandwidth-monitor"></a>Sledování pásma

Ukázka [monitor šířky pásma](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) umožňuje sledovat šířku pásma v klientském počítači. Měření šířky pásma, spustit vzorku v klientském počítači mezipaměti, volat do mezipaměti a sledujte pásma vykázaného vzorek monitor pásma.
