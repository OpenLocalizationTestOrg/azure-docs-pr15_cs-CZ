<properties
    pageTitle="Stav relace ASP.NET mezipaměti poskytovatele | Microsoft Azure"
    description="Zjistěte, jak ukládat stav relace ASP.NET použití mezipaměti Redis Azure"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Zprostředkovatel stav relace ASP.NET pro Azure Redis mezipaměti

Azure Redis mezipaměť poskytuje poskytovatele stavu relace, které můžete používat k ukládání stavu relace v mezipaměti místo v paměti nebo v databázi SQL serveru. Pokud chcete použít mezipaměti poskytovatele stavu relace, nejdřív konfigurace mezipaměť a potom i konfiguraci aplikace ASP.NET pro mezipaměti pomocí balíček Redis NuGet stavu mezipaměti relace.

Není často praktické v aplikaci reálný cloud k Neuchovávejte některý z stavu pro uživatelské relace, ale některé přístupy vliv na výkon a škálovatelnost víc než ostatní. Pokud máte pro uložení stavu, nejlepším řešením je zachovejte množství stavu a uložte ho do souborů cookie. Není-li, je to vhodné, další nejlepším řešením je pro účely stav relace ASP.NET s poskytovatelem v paměti, distribuované mezipaměti. Scénář Nejhorší řešení od výkon a škálovatelnost hlediska je používat databázi záložní poskytovatele stavu relace. Toto téma obsahuje pokyny k používání poskytovatele stavu relace ASP.NET Azure Redis mezipaměti. Informace o možnostech stavu relace najdete v článku [Možnosti stavu relace ASP.NET](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Uložení stavu relace ASP.NET v mezipaměti

Konfigurace klientské aplikace ve Visual Studiu použití balíčku Redis NuGet stavu mezipaměti relace, klikněte pravým tlačítkem myši na projekt v **Okně Průzkumník** a zvolte **Spravovat balíčků NuGet**.

![Balíčky NuGet spravovat Azure Redis mezipaměti](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

Do textového pole Hledat zadejte **RedisSessionStateProvider** vyberte z pole výsledky a klikněte na tlačítko **nainstalovat**.

>[AZURE.IMPORTANT] Pokud používáte funkci clusterů od osy premium, je nutné použít [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 nebo novějším nebo o výjimce, vyvolá se. Jedná se o změnu jazycích; Další informace najdete v článku [v2.0.0 přerušení změnit podrobnosti](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Zprostředkovatele Azure Redis mezipaměti relace stavu](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Balíček Redis relace stavu poskytovatele NuGet obsahuje závislost na balíček StackExchange.Redis.StrongName. Pokud není k dispozici v projektu balíček StackExchange.Redis.StrongName bude nainstalovaný. Všimněte si, že kromě StackExchange.Redis.StrongName balíčku silným názvem není také verzi než silným názvem StackExchange.Redis. Pokud projektu používá bez silným názvem StackExchange.Redis verze že odinstalujte, před nebo po instalaci balíček Redis NuGet poskytovatele stavu relace, jinak se budou získat konfliktům pojmenování v projektu. Další informace o těchto balíčků najdete v tématu [konfigurace .NET mezipaměti klientů](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Balíček NuGet a soubory ke stažení přidá požadované sestavení odkazu a přidá že následující přidá následující část do souboru Web.Configuration, která obsahuje požadovaná konfigurace aplikace ASP.NET používat poskytovatele stavu Redis relace mezipaměti.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

Skrytými v komentářích části jsou uvedeny příklady atributy a ukázkové nastavení pro jednotlivé atributy.

Konfigurace atributy s hodnotami z mezipaměti zásuvné na portálu Microsoft Azure a nakonfigurujte další hodnoty podle potřeby. Pokyny pro přístup k vlastnosti mezipaměti najdete v článku [Konfigurace Redis nastavení mezipaměti](cache-configure.md#configure-redis-cache-settings).

-   **Host (hostitel)** : Zadejte koncový bod mezipaměti.
-   **port** – použití port a protokol SSL nebo vaše SSL port, v závislosti na nastavení ssl.
-   **accessKey** – použití primární a sekundární klíč pro mezipaměť.
-   **ssl** – hodnotu PRAVDA, pokud chcete k zabezpečení komunikace mezipaměti/klienta se ssl. jinak false. Nezapomeňte zadat správné port.
    -   Ve výchozím nastavení pro nové mezipaměti je zakázaná port a protokol SSL. Zadejte hodnotu true pro toto nastavení použít SSL port. Další informace o povolení port není SSL naleznete v části [Přístup porty](cache-configure.md#access-ports) v tématu [Konfigurace mezipaměti](cache-configure.md) .
-   **throwOnError** – hodnotu PRAVDA, pokud chcete výjimku vyvolání v případě chyby, nebo NEPRAVDA požadovaná selhání tiše operace. Můžete vyhledat selhání kontrolou statické Microsoft.Web.Redis.RedisSessionStateProvider.LastException vlastnosti. Výchozí hodnota je PRAVDA.
-   během tohoto intervalu zadané v milisekundách jsou opakované **retryTimeoutInMilliseconds** – operacích, které se nezdaří. První opakování vyskytne po 20 milisekund a potom opakování provedeno za sekundu do retryTimeoutInMilliseconds intervalu vypršení platnosti. Ihned po tomto intervalu je operaci opakovat jednorázové konečný. Pokud se stále nezdaří, výjimce zpět volajícímu v závislosti na nastavení throwOnError. Výchozí hodnota je 0, což znamená, bez opakování.
-   **databaseId** – určuje mezipaměti databázi, kterou chcete použít pro výstup data. Pokud není zadán, je použita výchozí hodnota 0.
-   **applicationName** – klíče jsou uloženy v redis jako `{<Application Name>_<Session ID>}_Data`. Díky více aplikací pro sdílení stejného klíče. Tento parametr je volitelný a pokud nezadáte ho bude použita výchozí hodnota.
-   **connectionTimeoutInMilliseconds** – toto nastavení umožňuje nastavení connectTimeout v klientovi StackExchange.Redis přepsat. Pokud není zadáno, použije se výchozí nastavení connectTimeout 5000. Další informace najdete v tématu [StackExchange.Redis konfigurace modelu](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – toto nastavení umožňuje nastavení syncTimeout v klientovi StackExchange.Redis přepsat. Pokud není zadán, se používá ve výchozím nastavení syncTimeout 1 000. Další informace najdete v tématu [StackExchange.Redis konfigurace modelu](http://go.microsoft.com/fwlink/?LinkId=398705).

Další informace o těchto vlastností najdete v článku původní oznámení příspěvek blogu oznamující ASP.NET relace stavu pro u [Poskytovatele Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Nezapomeňte si poznámky, standardní InProc relace stavu poskytovatele v části vaší web.config.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Po provedení těchto kroků budou aplikace nakonfigurovaný na používání poskytovatele stavu Redis relace mezipaměti. Při použití stavu relace v aplikaci budou uložené v mezipaměti Redis Azure instance.

>[AZURE.NOTE] Všimněte si, že data uložená v mezipaměti musí být serializovatelné na rozdíl od data, která mohou být uloženy ve výchozím v paměti ASP.NET relaci stav poskytovatele. Při použití poskytovatele stavu relace pro Redis Ujistěte se, že jsou datové typy uložené ve stavu relace serializovatelný.

## <a name="aspnet-session-state-options"></a>Možnosti stavu relace ASP.NET

- Ve skupinovém rámečku zprostředkovatel stav relace paměti - tohoto poskytovatele ukládá stav relace v paměti. Výhodou používání tento poskytovatel je, že je jednoduché a rychlé. Nelze však měřítko Web Apps používáte v paměti poskytovatele vzhledem k tomu, že není distribuovaná.

- Stav relace serveru SQL poskytovatele - tohoto poskytovatele ukládá stav relace na serveru Sql Server. Pokud chcete zachovat stavu relace v trvalé úložiště byste měli použít tohoto poskytovatele. Je možné přizpůsobit webovou aplikaci, ale pomocí serveru Sql Server pro relace bude mít vliv na výkon na Web Appu.

- Distribuované v paměti poskytovatele stavu relace například Redis mezipaměti poskytovatele stavu relace - tohoto poskytovatele vám ty nejcennější ze obou světě. Web Appu můžou obsahovat jednoduché, rychlé a scalable poskytovatele stavu relace. Ale máte ponechat v úvahu, že tento poskytovatel ukládá stav relace v mezipaměti a aplikace má vzít v úvahu všechny vlastnosti spojené upozorňovat Distributed v mezipaměti například selhání přechodná sítě. Osvědčené postupy týkající se použití mezipaměti najdete v článku [pokyny pro ukládání do mezipaměti](../best-practices-caching.md) Microsoft Patterns a postupy [Azure cloudu aplikace návrh a implementace pokyny](https://github.com/mspnp/azure-guidance).

Další informace o stavu relace a jiné doporučené postupy v tématu [Doporučené postupy vývoj Web (stavební reálný cloudu aplikace s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Další kroky

Podívejte se na [Poskytovatele ASP.NET výstupní mezipaměti pro Azure Redis mezipaměti](cache-aspnet-output-cache-provider.md).
