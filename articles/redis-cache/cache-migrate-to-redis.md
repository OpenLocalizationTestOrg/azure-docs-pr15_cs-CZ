<properties 
    pageTitle="Mezipaměť migrovat do Redis | Microsoft Azure"
    description="Naučte se migrovat aplikace služba spravovaných mezipaměti do mezipaměti Redis Azure"
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
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migrace z spravovaných mezipaměti služby Azure Redis mezipaměti

Migrace aplikace, které používají služba spravovaných mezipaměti Azure do mezipaměti Redis Azure lze provést s minimálními změn v aplikaci, podle toho, funkce služba spravovaných mezipaměti používané aplikací ukládání do mezipaměti. Během rozhraní API jsou přesně stejné jsou podobné a velkou část stávající kód, který používá služba spravovaných mezipaměti pro přístup k mezipaměti lze opakovaně použít s minimálními změny. Toto téma ukazuje o možnostech potřebné konfigurace a změny aplikace migrace aplikace služba spravovaných mezipaměti používat Azure Redis mezipaměti a některé z funkcí Azure Redis mezipaměti použití implementace funkcí služba spravovaných mezipaměti mezipaměti.

## <a name="migration-steps"></a>Kroky migrace

Následující kroky nutné migrace aplikace služba spravovaných mezipaměti používat Azure Redis mezipaměti.

-   Mapování spravovaných služby mezipaměti funkce do mezipaměti Redis Azure
-   Zvolte nabízení mezipaměti
-   Vytvořit mezipaměť
-   Konfigurace klientů mezipaměti
    -   Odebrání konfiguraci služby spravovaných mezipaměti
    -   Konfigurace klienta mezipaměti použití balíčku NuGet StackExchange.Redis
-   Migrace kód služba spravovaných mezipaměti
    -   Připojení k mezipaměti pomocí ConnectionMultiplexer třídy
    -   Access základní datové typy v mezipaměti
    -   Práce s objekty .NET v mezipaměti
-   Migrace stav relace ASP.NET a ukládání výstupu do mezipaměti do mezipaměti Redis Azure 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Mapování spravovaných služby mezipaměti funkce do mezipaměti Redis Azure

Azure služba spravovaných mezipaměti Azure Redis mezipaměti jsou podobné a provedení některých jejich funkce různými způsoby. Tato část popisuje některé rozdíly a obsahuje pokyny týkající se implementace funkcí služba spravovaných mezipaměti v Azure Redis mezipaměti.

|Spravované funkce služby mezipaměti|Spravované podpora služby mezipaměti|Podpora Azure Redis mezipaměti|
|---|---|---|
|Pojmenovaná mezipaměti|Výchozí mezipaměti nakonfigurovaný a v mezipaměti Standard a Premium možné konfigurovat nabídky až devět další s názvem mezipaměti podle potřeby.|Azure mezipaměti Redis mají konfigurovatelné počet databáze (výchozí nastavení 16), které lze provádět podobnou funkci obsahují na pojmenované mezipaměti. Další informace najdete v tématu [výchozí Redis konfigurace serveru](cache-configure.md#default-redis-server-configuration).|
|Dostupnost|Poskytuje dostupnost pro položky v mezipaměti v mezipaměti nabídky Standard a Premium. Pokud položky se ztratí z důvodu se nepovede, jsou pořád dostupné záložní kopie položky v mezipaměti. Sekundární mezipaměti čtou synchronní.|Dostupnost je k dispozici v Standard a Premium nabídky mezipaměti, které mají konfigurace primární/otevřené dvou uzel (každý shard v mezipaměti Premium má pár primární/otevřené). Zápisy otevřené jsou určené asynchronní. Další informace najdete v tématu [ceny Azure Redis mezipaměti](https://azure.microsoft.com/pricing/details/cache/).|
|Oznámení|Umožňuje zobrazovat oznámení asynchronní při různých operacích mezipaměti výskytu pojmenované mezipaměti.|Klientské aplikace můžete dosáhnout podobnou funkci obsahují oznámení pomocí Redis pub/sub nebo [Keyspace oznámení](cache-configure.md#keyspace-notifications-advanced-settings) .|
|Místní mezipaměti|Uloží kopii uložené v mezipaměti objektů místně na straně klienta pro velmi rychlý přístup.|Klientské aplikace jsou potřeba k provedení této funkce použití slovníku nebo podobné datová struktura.|
|Zásady vyřazení|Žádný nebo LRU. Výchozí zásady je LRU.|Azure Redis mezipaměti podporuje následující zásady vyřazení: stálých lru allkeys lru, stálých náhodné, allkeys náhodné stálých ttl, noeviction. Výchozí zásady je stálých lru. Další informace najdete v tématu [výchozí Redis konfigurace serveru](cache-configure.md#default-redis-server-configuration).|
|Zásad vypršení platnosti|Výchozí zásady vypršení platnosti je absolutní a intervalu vypršení platnosti výchozí je deset minut. Zásady klouzavé a nikdy jsou k dispozici.|Ve výchozím nastavení se položky v mezipaměti nevyprší, ale vypršení můžete nakonfigurovat na základě zápisu za použití přetížení nastavení mezipaměti. Další informace najdete v tématu [Naučte se přidávat a obnovit objekty z mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Oblastí a označování|Oblasti jsou podskupin pro položky v mezipaměti. Oblasti také podporují poznámky položek v mezipaměti s další popisný řetězce značky. Oblasti domovské stránce podpory provádět operace vyhledávání na všechny položky se značkou v dané oblasti. Všechny položky v rámci oblasti se nacházejí v rámci jedné uzel clusteru mezipaměti.|Redis mezipaměti se skládá z jednoho uzlu (Pokud Redis clusteru je povoleno) tak, aby koncept služba spravovaných mezipaměť oblastí, nebudou mít vliv. Redis podporuje vyhledávání a zástupných operace při načítání klíče tak popisný značky můžete vložený do názvy klíčů a použitý k načtení položky později. Příklad implementace značek řešení pomocí Redis naleznete v tématu [Implementace mezipaměti označování s Redis](http://stackify.com/implementing-cache-tagging-redis/).|
|Serializace|Spravované mezipaměti podporuje NetDataContractSerializer, BinaryFormatter a používání vlastních serializers. Výchozí hodnota je NetDataContractSerializer.|Odpovídá klientské aplikace serializovat objekty .NET před vložením do mezipaměti, s výběrem serializer až vývojář aplikace klienta. Další informace a ukázkový kód najdete v článku [práce s objekty .NET v mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Emulátoru mezipaměti | Spravované mezipaměť poskytuje emulátoru místní mezipaměti. | Azure Redis mezipaměti nemá emulátoru, ale můžete [Spustit MSOpenTech sestavení redis-server.exe místně](cache-faq.md#cache-emulator) emulátoru prostředí. |

## <a name="choose-a-cache-offering"></a>Zvolte nabízení mezipaměti

Microsoft Azure Redis mezipaměti je dostupná v těchto úrovně:

-   **Základní** – jeden uzel. Více velikosti až 53 GB.
-   **Standardní** – dvěma uzly primární/otevřené. Více velikosti až 53 GB. 99,9 SLA %.
-   **Premium** – dvěma uzly primární/otevřené s až 10 shards. Více velikosti z 6 GB 530 GB (kontaktujte nás Další). Všechny funkce standardní osy a další včetně podpora [Redis clusteru](cache-how-to-premium-clustering.md) [Redis trvalé](cache-how-to-premium-persistence.md)a [Azure virtuální sítě](cache-how-to-premium-vnet.md). 99,9 SLA %.

Jednotlivé vrstvy se liší z hlediska funkcí a ceny. Funkce jsou uvedené v této příručce a další informace o cenách najdete v tématu [Podrobnosti ceny mezipaměti](https://azure.microsoft.com/pricing/details/cache/).

Výchozí bod pro migraci je vyberte požadovanou velikost odpovídající velikosti mezipaměť předchozí služba spravovaných mezipaměti a potom měřítko nahoru nebo dolů v závislosti na požadavky aplikace. Další informace o výběru doprava Nabízení Azure Redis mezipaměti, najdete v článku [Jaké nabízení Redis mezipaměti a velikost mám použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Vytvořit mezipaměť

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Konfigurace klientů mezipaměti

Po mezipaměti vytvořili a nakonfigurovali, dalším krokem je odebrání konfigurace služba spravovaných mezipaměti a přidat Přidat konfiguraci Azure Redis mezipaměti a odkazuje tak, aby mezipaměti klientů můžete získat přístup k mezipaměti.

-   Odebrání konfiguraci služby spravovaných mezipaměti
-   Konfigurace klienta mezipaměti použití balíčku NuGet StackExchange.Redis

### <a name="remove-the-managed-cache-service-configuration"></a>Odebrání konfiguraci služby spravovaných mezipaměti

Před klientské aplikace lze nakonfigurovat pro Azure Redis mezipaměti, existující konfiguraci služba spravovaných mezipaměti a odkazy na sestavení potřeba ji nejdřív odebrat odinstalováním balíček spravovaných NuGet služby mezipaměti.

Odinstalace balíčku spravovaných NuGet služby mezipaměti, klikněte pravým tlačítkem myši v **Okně Průzkumník** projektu klienta a zvolte **Spravovat balíčků NuGet**. Vyberte uzel **Instalované balíčků** a zadejte do vyhledávacího pole nainstalovaných balíčků W**indowsAzure.Caching** . Vyberte **Windows** **Azure mezipaměti** (nebo **Windows** **Azure ukládání do mezipaměti** v závislosti na verzi balíček NuGet), klikněte na **odinstalovat**a potom na tlačítko **Zavřít**.

![Odinstalace balíčku NuGet služby Azure spravovaných mezipaměti](./media/cache-migrate-to-redis/IC757666.jpg)

Odinstalování balíčku spravovaných NuGet služby mezipaměti odebere sestavení služba spravovaných mezipaměti a položky služba spravovaných mezipaměti app.config nebo web.config klientské aplikace. Protože některé vlastní nastavení nemusí odeberou, když odinstalujete balíček NuGet, otevřete web.config nebo app.config a ujistěte se, že jsou následující prvky úplně odebere.

Zkontrolujte, že je `dataCacheClients` položka se odebere z `configSections` prvek. Neodebírat celé `configSections` element. odebrat právě `dataCacheClients` údaj, pokud je k dispozici.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Zkontrolujte, že je `dataCacheClients` část se odebere. `dataCacheClients` Oddíl bude vypadat podobně jako v následujícím příkladu.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Po odebrání konfigurace služba spravovaných mezipaměti můžete nakonfigurovat klientovi mezipaměti popsané v následující části.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Konfigurace klienta mezipaměti použití balíčku NuGet StackExchange.Redis

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrace kód služba spravovaných mezipaměti

Rozhraní API pro klienta mezipaměti StackExchange.Redis je podobný služba spravovaných mezipaměti. Tato část obsahuje základní informace o rozdílech.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Připojení k mezipaměti pomocí ConnectionMultiplexer třídy

Služby spravovaných mezipaměti, byly připojení do mezipaměti uskutečněných jednotlivými `DataCacheFactory` a `DataCache` třídy. V mezipaměti Redis Azure jsou spravovány tato připojení `ConnectionMultiplexer` předmětu.

Přidejte následující pomocí příkazu na začátek všech souborů, ze kterého chcete získat přístup k mezipaměti.

    using StackExchange.Redis
                                
Pokud tento obor názvů nerozpozná, ujistěte se, že jste přidali balíček StackExchange.Redis NuGet podle [Konfigurace klientů mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Všimněte si, že StackExchange.Redis klientský počítač vyžaduje .NET Framework 4 nebo novější.

Připojení k instanci Azure Redis mezipaměti, zavolejte statickou `ConnectionMultiplexer.Connect` metoda a předejte v koncového bodu a klíče. Jedním ze způsobů sdílení `ConnectionMultiplexer` instance aplikace má mít statickou vlastnost, která vrací připojeného instanci, podobně jako v následujícím příkladu. To umožňuje vláken inicializace pouze jedinou připojení `ConnectionMultiplexer` instance. V tomto příkladu `abortConnect` je nastavena na hodnotu NEPRAVDA, což znamená, že volání proběhne úspěšně i v případě, že není vytvořit připojení do mezipaměti. Jeden klíčové funkce `ConnectionMultiplexer` je, že se automaticky obnoví připojení do mezipaměti jednou problém sítě nebo jiné příčiny jsou přeložena.

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

Koncový bod mezipaměti, klíče a porty lze získat z **Mezipaměti Redis** zásuvné instance mezipaměti. Další informace najdete v tématu [Vlastnosti Redis mezipaměti](cache-configure.md#properties).

Po připojení, vrátit odkaz na databázi mezipaměti Redis tak, že zavoláte `ConnectionMultiplexer.GetDatabase` metody. Objekt vrácených `GetDatabase` metoda je lehké předávací objektu a nemusí být uloženy.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Klient StackExchange.Redis používá `RedisKey` a `RedisValue` typů pro přístup k ukládání položek v mezipaměti. Tyto typy mapovat na nejčastěji základní jazyk typů, včetně řetězce a často nepoužívaná přímo. Redis řetězce jsou základní druh Redis hodnotu a může obsahovat mnoho typů dat, včetně sériové binární datové proudy, a zatímco typ nemusí používat přímo, ale použijete metody, které obsahují `String` v názvu. Pro většinu základní datové typy uložit a načíst položek z mezipaměti pomocí `StringSet` a `StringGet` metody, pokud ukládáte kolekce nebo jiných Redis datových typů v mezipaměti. 

`StringSet`a `StringGet` se hodně podobají mezipaměti služba spravovaných `Put` a `Get` metody jednoho hlavní rozdíl že před obnovit původní stav a získejte objekt .NET do mezipaměti musí serializovat nejdřív. 

Při volání `StringGet`, pokud existuje objektu, bude vrácena, a pokud ne, vrátí se hodnota null. V takovém případě můžete načtení hodnoty ze zdroje požadovaná data a uložte ho do mezipaměti pro pozdější použití. Jedná se o jako vzorek odložené mezipaměti.

Chcete-li zadat vypršení platnosti položky v mezipaměti, použijte `TimeSpan` parametr `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis mezipaměti můžete spolupracovat s objekty .NET, stejně jako základní datové typy, ale před objektu .NET můžou mezipaměti musí serializovat. Toto je zodpovědnost vývojář aplikace. To vám vývojář pružnost při volbě serializátoru. Další informace a ukázkový kód najdete v článku [práce s objekty .NET v mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migrace stav relace ASP.NET a ukládání výstupu do mezipaměti do mezipaměti Redis Azure

Azure Redis mezipaměti má poskytovatelů pro stav relace ASP.NET a stránky výstupu do mezipaměti. Migrace aplikaci, která používá služba spravovaných mezipaměti verzích tito poskytovatelé, nejprve odebrat existující oddíly ze svého web.config a nakonfigurujte verzí Azure Redis mezipaměti poskytovatelů. Pokyny k používání poskytovatelů Azure Redis ASP.NET mezipaměti najdete v článku [Poskytovatele stavu relace ASP.NET Azure Redis mezipaměti](cache-aspnet-session-state-provider.md) a [ASP.NET poskytovatele výstupní mezipaměti pro Azure Redis mezipaměti](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Další kroky

Prozkoumání místnosti schůzky [si přečtěte následující dokumentaci Azure Redis mezipaměti](https://azure.microsoft.com/documentation/services/cache/) pro výukové programy pro ukázky, videa a další.

