<properties
    pageTitle="Poskytovatele mezipaměti ASP.NET výstupní mezipaměti"
    description="Naučte se mezipaměť výstupu stránky ASP.NET použití mezipaměti Redis Azure"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Poskytovatele ASP.NET výstupní mezipaměti pro Azure Redis mezipaměti

Poskytovatele Redis výstupní mezipaměti je mechanismus mimo proces úložiště pro výstupní mezipaměti data. Tato data jsou určeny pro úplné odpovědi protokolu HTTP (stránka ukládání výstupu do mezipaměti). Zprostředkovatel zapojení do nového výstupní mezipaměti poskytovatele rozšiřitelnost bodu, která byla zavedená v ASP.NET 4.

Pomocí poskytovatele Redis výstupní mezipaměti, nejdřív konfigurace mezipaměť a potom i konfiguraci aplikace ASP.NET pomocí balíček Redis NuGet poskytovatele výstupní mezipaměti. Toto téma obsahuje pokyny ke konfiguraci aplikace pomocí poskytovatele Redis výstupní mezipaměti. Další informace o vytváření a konfigurace instance Azure Redis mezipaměti najdete v článku [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Ukládání výstupu stránky ASP.NET v mezipaměti

Konfigurace klientské aplikace ve Visual Studiu použití balíčku Redis NuGet poskytovatele výstupní mezipaměti, klikněte pravým tlačítkem myši na projekt v **Okně Průzkumník** a zvolte **Spravovat balíčků NuGet**.

![Balíčky NuGet spravovat Azure Redis mezipaměti](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Do textového pole Hledat zadejte **RedisOutputCacheProvider** vyberte z pole výsledky a klikněte na tlačítko **nainstalovat**.

![Azure Redis poskytovatele výstupní mezipaměti mezipaměti](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Balíček Redis výstupní mezipaměti poskytovatele NuGet obsahuje závislost na balíček StackExchange.Redis.StrongName. Pokud není k dispozici v projektu balíček StackExchange.Redis.StrongName bude nainstalovaný. Všimněte si, že kromě StackExchange.Redis.StrongName balíčku silným názvem není také verzi než silným názvem StackExchange.Redis. Pokud váš projekt používá bez silným názvem StackExchange.Redis verze že odinstalujte, před nebo po instalaci balíček Redis NuGet poskytovatele výstupní mezipaměti, jinak se budou získat konfliktům pojmenování v projektu. Další informace o těchto balíčků najdete v tématu [konfigurace .NET mezipaměti klientů](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Balíček NuGet stáhne a přidá požadované sestavení odkazy a v následující části se přidá do souboru Web.Configuration, která obsahuje požadovaná konfigurace aplikace ASP.NET používat poskytovatele Redis výstupní mezipaměti.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Skrytými v komentářích části jsou uvedeny příklady atributy a ukázkové nastavení pro jednotlivé atributy.

Konfigurace atributy s hodnotami z mezipaměti zásuvné na portálu Microsoft Azure a nakonfigurujte další hodnoty podle potřeby. Pokyny pro přístup k vlastnosti mezipaměti najdete v článku [Konfigurace Redis nastavení mezipaměti](cache-configure.md#configure-redis-cache-settings).

-   **Host (hostitel)** : Zadejte koncový bod mezipaměti.
-   **port** – použití port a protokol SSL nebo vaše SSL port, v závislosti na nastavení ssl.
-   **accessKey** – použití primární a sekundární klíč pro mezipaměť.
-   **ssl** – hodnotu PRAVDA, pokud chcete k zabezpečení komunikace mezipaměti/klienta se ssl. jinak false. Nezapomeňte zadat správné port.
    -   Ve výchozím nastavení pro nové mezipaměti je zakázaná port a protokol SSL. Zadejte hodnotu true pro toto nastavení použít SSL port. Další informace o povolení port není SSL naleznete v části [Přístup porty](cache-configure.md#access-ports) v tématu [Konfigurace mezipaměti](cache-configure.md) .
-   **databaseId** – zadané databázi, ke které použít pro výstup data v mezipaměti. Pokud není zadán, je použita výchozí hodnota 0.
-   **applicationName** – klíče jsou uloženy v redis jako <AppName>_<SessionId>_Data. Díky více aplikací pro sdílení stejného klíče. Tento parametr je volitelný a pokud nezadáte ho bude použita výchozí hodnota.
-   **connectionTimeoutInMilliseconds** – toto nastavení umožňuje nastavení connectTimeout v klientovi StackExchange.Redis přepsat. Pokud není zadáno, použije se výchozí nastavení connectTimeout 5000. Další informace najdete v tématu [StackExchange.Redis konfigurace modelu](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – toto nastavení umožňuje nastavení syncTimeout v klientovi StackExchange.Redis přepsat. Pokud není zadán, se používá ve výchozím nastavení syncTimeout 1 000. Další informace najdete v tématu [StackExchange.Redis konfigurace modelu](http://go.microsoft.com/fwlink/?LinkId=398705).

Přidejte směrnici OutputCache každou stránku, u kterého chcete výstupu do mezipaměti.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

V tomto příkladu se data uložená v mezipaměti stránky v mezipaměti dobu 60 sekund a jiné verze stránky mezipaměti pro každý parametr kombinaci. Další informace o směrnice OutputCache najdete v tématu [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Po provedení těchto kroků budou aplikace je nakonfigurovaný na používání poskytovatele Redis výstupní mezipaměti.

## <a name="next-steps"></a>Další kroky

Podívejte se na [Poskytovatele stavu relace ASP.NET Azure Redis mezipaměti](cache-aspnet-session-state-provider.md).
