<properties 
    pageTitle="Jak používat Azure Redis mezipaměti | Microsoft Azure" 
    description="Zjistěte, jak zlepšit výkon Azure aplikací s mezipamětí Redis Azure" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Jak používat Azure Redis mezipaměti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [TECHNOLOGIE ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Tato příručka, uvidíte, jak začít používat **Azure Redis mezipaměti**. Microsoft Azure Redis mezipaměti vychází z oblíbených otevřít zdroj Redis mezipaměti. Umožňuje přístup k zabezpečení, vyhrazené Redis mezipaměti, spravuje Microsoft. Mezipaměť vytvořené pomocí Azure Redis mezipaměti přístupný z libovolné aplikace v rámci Microsoft Azure.

Microsoft Azure Redis mezipaměti je dostupná v těchto úrovně:

-   **Základní** – jeden uzel. Více velikosti až 53 GB.
-   **Standardní** – dvěma uzly primární/otevřené. Více velikosti až 53 GB. 99,9 SLA %.
-   **Premium** – dvěma uzly primární/otevřené s až 10 shards. Více velikosti z 6 GB 530 GB (kontaktujte nás Další). Všechny funkce standardní osy a další včetně podpora [Redis clusteru](cache-how-to-premium-clustering.md) [Redis trvalé](cache-how-to-premium-persistence.md)a [Azure virtuální sítě](cache-how-to-premium-vnet.md). 99,9 SLA %.

Jednotlivé vrstvy se liší z hlediska funkcí a ceny. Další informace o cenách najdete v článku [Podrobnosti ceny mezipaměti][].

Tato příručka, uvidíte, jak používat klienta [StackExchange.Redis][] pomocí C\# kód. Scénáře nichž se uplatní zahrnují **vytváření a konfigurace mezipaměť** **Konfigurace klientů mezipaměti**a **přidávání a odebírání objektů z mezipaměti**. Další informace o použití Azure Redis mezipaměti naleznete v části [Další kroky][] . Podrobný kurz sestavování ASP.NET MVC web app s Redis mezipaměti najdete v článku [jak vytvářet Web App s Redis mezipaměti](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Začínáme s Azure Redis mezipaměti

Začínáme s Azure Redis mezipaměti je snadné. Na začátku zřízení a konfigurace mezipaměti. Pak klienti mezipaměti nakonfigurovat tak, aby se můžou dostat k mezipaměti. Po konfiguraci klientů mezipaměti můžete začít pracovat s nimi.

-   [Vytvoření mezipaměti][]
-   [Konfigurace klientů mezipaměti][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Vytvořit mezipaměť

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Přístup k mezipaměť za ni se vytvoří

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Další informace o konfiguraci mezipaměti najdete v článku [jak nakonfigurovat Azure Redis mezipaměti](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfigurace klientů mezipaměti

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Po projektu klienta nakonfigurovaný pro ukládání do mezipaměti, můžete použít technik popsaných v následujících částech pro práci s mezipaměť.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Práce s mezipaměti

Postup v této části popisují, jak provádět běžné úlohy s mezipamětí.

-   [Připojení k mezipaměti][]
-   [Přidání a načíst objekty z mezipaměti][]
-   [Práce s objekty .NET v mezipaměti](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Připojení k mezipaměti

Chcete-li programově pracovat s mezipamětí, musíte odkaz do mezipaměti. Přidejte následující text na začátek všech souborů, ze které chcete použít StackExchange.Redis klienta pro přístup k mezipaměti Redis Azure.

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis klientských vyžaduje .NET Framework 4 nebo novější.

Připojení k mezipaměť Redis Azure spravován `ConnectionMultiplexer` předmětu. Tato třída slouží ke sdílené a znovu použít v celém klientské aplikace a není potřeba vytvořit na základě za operace. 

Připojení ke mezipaměti Redis Azure a být vrácen instanci připojené `ConnectionMultiplexer`, zavolejte statickou `Connect` metoda a průchodu v mezipaměti koncového bodu a klíč jako v následujícím příkladu. Pomocí klávesy generovaných z portálu Microsoft Azure jako parametr heslo.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Upozornění: Nikdy úložiště pověření v zdrojového kódu. Aby tato ukázka jednoduché, můžu mi je zobrazíte v zdrojového kódu. Informace o ukládání přihlašovacích údajů najdete v článku [jak řetězce aplikace a připojení řetězce práce][] .

Pokud nechcete použít protokol SSL, buď nastavit `ssl=false` nebo vynechat `ssl` parametr.

>[AZURE.NOTE] Ve výchozím nastavení pro nové mezipaměti je zakázaná port a protokol SSL. Pokyny o povolení port není SSL najdete v tématu [Porty přístup](cache-configure.md#access-ports).

Jedním ze způsobů sdílení `ConnectionMultiplexer` instance aplikace má mít statickou vlastnost, která vrací připojeného instanci, podobně jako v následujícím příkladu. To umožňuje vláken inicializace pouze jedinou připojení `ConnectionMultiplexer` instance. V těchto příkladech `abortConnect` je nastavena na hodnotu NEPRAVDA, což znamená, že volání proběhne úspěšně i v případě, že není vytvořit připojení do mezipaměti Redis Azure. Jeden klíčové funkce `ConnectionMultiplexer` je, že se automaticky obnoví připojení do mezipaměti jednou problém sítě nebo jiné příčiny jsou přeložena.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Další informace o možnostech konfigurace rozšířeného připojení najdete v článku [StackExchange.Redis konfigurace modelu][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Po připojení, vrátit odkaz na databázi mezipaměti redis tak, že zavoláte `ConnectionMultiplexer.GetDatabase` metody. Objekt vrácených `GetDatabase` metoda je lehké předávací objektu a nemusí být uloženy.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Teď když víte, jak se můžete připojit k instanci Azure Redis mezipaměti a vrátit odkaz na databázi mezipaměti, Podívejme se při práci s mezipamětí.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Přidání a načíst objekty z mezipaměti

Položky můžete uložené v a načtená z kontingenčního seznamu mezipaměť pomocí `StringSet` a `StringGet` metody.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis ukládá většina dat jako Redis řetězce, ale tyto řetězce může obsahovat mnoho typů dat, včetně sériové binární data, která lze použít při ukládání .NET objekty v mezipaměti.

Při volání `StringGet`, pokud existuje objektu, bude vrácena, a pokud ne, `null` je vrácena. V takovém případě můžete načtení hodnoty ze zdroje požadovaná data a uložte ho do mezipaměti pro pozdější použití. Jedná se o jako vzorek odložené mezipaměti.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Chcete-li zadat vypršení platnosti položky v mezipaměti, použijte `TimeSpan` parametr `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Práce s objekty .NET v mezipaměti

Azure Redis mezipaměti můžete mezipaměti objektů .NET, stejně jako základní datové typy, ale před objektu .NET můžou mezipaměti musí serializovat. Odpovídá vývojář aplikace a poskytuje vývojář flexibilitu při volbě serializátoru.

Jednoduchý serializovat objekty je možné používat `JsonConvert` serializace metody v [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) a serializovaci do a z JSON. Následující příklad ukazuje get a sadu pomocí `Employee` instance objektu.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy, tyto odkazy vedou na další informace o Azure Redis mezipaměti.

-   Rezervovat zprostředkovatele ASP.NET Azure Redis mezipaměti.
    -   [Stav relace Redis Azure poskytovatele](cache-aspnet-session-state-provider.md)
    -   [Azure Redis poskytovatele mezipaměti ASP.NET výstupní mezipaměti](cache-aspnet-output-cache-provider.md)
-   [Povolení diagnostiky mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , budete moct [Sledování](cache-how-to-monitor.md) stavu mezipaměť. Metriky můžete zobrazit v portálu Azure a můžete [Stáhnout a zkontrolujte](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) pomocí nástroje podle svého výběru.
-   Podívejte se na [mezipaměti StackExchange.Redis si přečtěte následující dokumentaci klienta][].
    -   Azure Redis mezipaměti můžete k nim získat přístup z mnoha Redis klienty a vývoj jazyky. Další informace najdete v tématu [http://redis.io/clients][].
-   Azure Redis mezipaměti lze také pomocí služby třetích stran a nástrojů, jako jsou Redsmin a Redis správce plochy.
    -   Další informace o Redsmin najdete v tématu [Jak získat připojovací řetězec Azure Redis a používat je s Redsmin][].
    -   Přístup a zkontrolovat, jestli vaše data v mezipaměti Redis Azure pomocí grafického rozhraní pomocí [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-   V dokumentaci k [redis][] a další informace o [redis datových typů][] a [15 minut Úvod k Redis datové typy][].



<!-- INTRA-TOPIC LINKS -->
[Další kroky]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Vytvoření mezipaměti]: #create-cache
[Configure the cache]: #enable-caching
[Konfigurace klientů mezipaměti]: #NuGet
[Working with Caches]: #working-with-caches
[Připojení k mezipaměti]: #connect-to-cache
[Přidání a načíst objekty z mezipaměti]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Jak získat připojovací řetězec Azure Redis a používat ho s Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Model konfigurace StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Podrobnosti o cenách do mezipaměti]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[Mezipaměť StackExchange.Redis si přečtěte následující dokumentaci klienta]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis datové typy]: http://redis.io/topics/data-types
[patnáct minut Úvod k datovým typům Redis]: http://redis.io/topics/data-types-intro

[Jak funguje aplikace řetězce a připojovací řetězec]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


