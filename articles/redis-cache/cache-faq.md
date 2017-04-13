<properties 
    pageTitle="Azure Redis mezipaměti nejčastější dotazy týkající se | Microsoft Azure" 
    description="Přečtěte si odpovědi na běžné otázky, vzorků a osvědčené postupy pro Azure Redis mezipaměti" 
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
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure Redis mezipaměti časté otázky

Přečtěte si odpovědi na běžné otázky, vzorků a osvědčené postupy pro Azure Redis mezipaměti. 


## <a name="what-if-my-question-isnt-answered-here"></a>Co když Moje byste nenašli odpověď otázku tady?

Pokud tady není uvedené svou otázku, dejte nám prosím vědět a my vám najít odpověď.

-   Můžete odeslat dotaz [Disqus vlákna](#comments) na konci v tomto článku a zapojit s týmem Azure mezipaměti a ostatní členové komunity o tohoto článku.
-   Kontaktovat na širší publikum, můžete odeslat dotaz na [Fórum MSDN Azure mezipaměti](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) a zapojit s týmem Azure mezipaměti a ostatní členové komunity.
-   Pokud chcete vytvořit žádost o funkci, můžete odeslat žádosti a nápady [Azure Redis hlasové mezipaměti uživatele](https://feedback.azure.com/forums/169382-cache).
-   Můžete taky poslat e-mailu nám v [Azure mezipaměti externí názory](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Azure základy Redis mezipaměti

Nejčastější dotazy v této části najdete některé základní informace o Azure Redis mezipaměti.

-    [Co je Azure Redis mezipaměť?](#what-is-azure-redis-cache)
-    [Jak můžu začít s mezipamětí Redis Azure?](#how-can-i-get-started-with-azure-redis-cache)

Nejčastější dotazy týkající se následující průvodní základní koncepty a dotazy týkající se Azure Redis mezipaměti a odpovědi v jednom z v dalších částech Nejčastější dotazy týkající se.

-   [K čemu nabízení Redis mezipaměti a velikost mám použít?](#what-redis-cache-offering-and-size-should-i-use)
-   [K čemu Redis mezipaměti klienti můžou pomocí?](#what-redis-cache-clients-can-i-use)
-   [Existuje místní emulátor pro mezipaměti Redis Azure?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Jak můžu sledovat výkon Moje mezipaměti a stavu?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Plánování časté otázky

-   [K čemu nabízení Redis mezipaměti a velikost mám použít?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure Redis mezipaměti výkonu](#azure-redis-cache-performance)
-   [Jaké oblasti měli můžu vyhledat Moje mezipaměti?](#in-what-region-should-i-locate-my-cache)
-   [Jak mám fakturované mezipaměti Redis Azure?](#how-am-i-billed-for-azure-redis-cache)
-   [Mám používat Azure Redis mezipaměti se Azure Government Cloud nebo cloudového Čína Azure?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Nejčastější dotazy týkající se vývoje

-   [Co možnosti konfigurace StackExchange.Redis dělat?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [K čemu Redis mezipaměti klienti můžou pomocí?](#what-redis-cache-clients-can-i-use)
-   [Existuje místní emulátor pro mezipaměti Redis Azure?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Spouštění Redis příkazy](#how-can-i-run-redis-commands)
-   [Proč nemá Azure Redis mezipaměti MSDN knihovny tříd třeba některé další služby Azure?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Můžete použít Azure Redis mezipaměti jako mezipaměti relace PHP?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Nejčastější dotazy týkající se zabezpečení

-   [Kdy si mám povolit port protokol SSL pro připojení k Redis?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Nejčastější dotazy týkající se výrobní

-   [Jaké jsou některé osvědčené postupy výrobní?](#what-are-some-production-best-practices)
-   [Jaké jsou některé důležité informace o při použití běžnými příkazy Redis?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Jak lze testu výkonnosti a otestujte výkon Moje mezipaměti?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Důležité podrobnosti o fondu podprocesů LOGLINTREND](#important-details-about-threadpool-growth)
-   [Povolení globálního katalogu serveru při použití StackExchange.Redis získat další výkon na straně klienta](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Sledování a odstraňování potíží časté otázky

Nejčastější dotazy v této části vysvětlovat běžné sledování a řešení potíží s dotazy. Další informace o sledování a odstraňování potíží instance Azure Redis mezipaměti najdete v článku [jak sledovat Azure Redis mezipaměti](cache-how-to-monitor.md) a [jak řešit problémy s Azure Redis mezipaměti](cache-how-to-troubleshoot.md).

-   [Jak můžu sledovat výkon Moje mezipaměti a stavu?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Mezipaměť Diagnostika úložiště účtu nastavení změnit, co se stalo](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Proč je povolené diagnostických nástrojů pro některé nové mezipaměti, ale ne ostatní?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Proč se mi zobrazují časové limity?](#why-am-i-seeing-timeouts)
-   [Proč je můj klienta to od ní odpojilo z mezipaměti?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Nejčastější dotazy týkající se předchozí nabízení mezipaměti

-   [Které Azure mezipaměti nabízení je pro mě nejlepší?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Co je Azure Redis mezipaměť?

Azure Redis mezipaměti vychází z oblíbených otevřít zdroji [Redis mezipaměti](http://redis.io). Umožňuje přístup k zabezpečení, vyhrazené Redis mezipaměti, spravovaný společností Microsoft a přístupné z libovolné aplikace v rámci Azure. Podrobnější informace naleznete na stránce product [Mezipaměti Redis Azure](https://azure.microsoft.com/services/cache/) na Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Jak můžu začít s mezipamětí Redis Azure?

Existuje několik způsobů, jak začít s Azure Redis mezipaměti.

-    Podívejte se na jednu z našich kurzů k dispozici pro [.NET](cache-dotnet-how-to-use-azure-redis-cache.md) [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)a [Python](cache-python-get-started.md). 
-    Podívejte se na [tom, jak vytvořit vysoký výkon aplikací pomocí Microsoft Azure Redis mezipaměť](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    Můžete se podívat klienta si přečtěte následující dokumentaci pro klienty, které odpovídají jazyka vývoj projektu pro informace o použití Redis. Existuje řada Redis klientů, které můžete používat s Azure Redis mezipaměti. Seznam Redis klientech najdete v tématu [http://redis.io/clients](http://redis.io/clients).


Pokud ještě nemáte účet Azure, máte tyto možnosti:

-    [Otevřete účet Azure zdarma](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Potřebujete přeplatky, které lze použít k vyzkoušení placené služby Azure. I když přeplatky používají, můžete mít účet a použití funkcí a bezplatné služby Azure.
-    [Aktivace Visual Studio účastnická výhod](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Vaše předplatné MSDN vám přeplatky každý měsíc, využívající služby placené Azure.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>K čemu nabízení Redis mezipaměti a velikost mám použít?
Každý Azure Redis mezipaměti poskytuje různé úrovně **velikost**, **šířku pásma**, **dostupnost**a **SLA** možnosti.

Následují co zvážit při výběru nabízení mezipaměti.

-   **Paměť**: základní a standardní úrovní nabízejí 250 MB – 53 GB. Vrstvy Premium nabízí 530 GB s větší dostupnost [na žádost](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Další informace najdete v tématu [Azure Redis mezipaměti ceny](https://azure.microsoft.com/pricing/details/cache/).
-   **Výkon sítě**: Pokud máte úlohu, které si žádá vysoký výkon osy Premium nabízí větší šířku pásma ve srovnání s standardní nebo Basic. V rámci každé osy větší velikost mezipaměti mít taky větší šířku pásma z důvodu podkladového OM, který je hostitelem mezipaměti. Najdete v [následující tabulce](#cache-performance) Další informace.
-   **Výkon**: Premium osy nabízí maximální dostupné výkon. Pokud server mezipaměti nebo klient dosáhne omezení šířky pásma, zobrazí se časové limity na straně klienta. Další informace najdete v následující tabulce.
-   **Vysoký dostupnost/SLA**: Azure Redis mezipaměti zaručuje, že standardní/Premium mezipaměti bude k dispozici aspoň 99,9 % od času. Další informace o náš SLA, najdete v článku [Azure Redis mezipaměti cena za](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). SLA se vztahuje pouze připojení k koncové body mezipaměti. SLA nezabývá ochrana před ztrátou dat. Doporučujeme používat funkci Redis dat trvalé ve vrstvě Premium zvětšíte odolnost před ztrátou dat.
-   **Redis trvání dat**: Premium osy umožňuje uchovávat data v mezipaměti v úložišti Azure účet. V mezipaměti Basic/standardní všechna data uložená jenom v paměti. V případě podkladového infrastruktury problémy tam je možné možnou ztrátu dat. Doporučujeme používat funkci Redis dat trvalé ve vrstvě Premium zvětšíte odolnost před ztrátou dat. Azure Redis mezipaměti nabízí RDB a AOF (už brzo) možnosti Redis trvalé. Další informace najdete v tématu [jak nakonfigurovat trvalé pro mezipaměť Redis Azure Premium](cache-how-to-premium-persistence.md).
-   **Redis clusteru**: vytvoření ukládá větší než 53 GB nebo chcete shard dat ve více uzlů Redis, můžete použít Redis clusterů, která je dostupná ve vrstvě Premium. Každý uzel se skládá z pár primární/otevřené mezipaměti pro dostupnost. Další informace najdete v tématu [jak nakonfigurovat clusterů pro mezipaměť Redis Azure Premium](cache-how-to-premium-clustering.md).
-   **Rozšířené zabezpečení a izolace sítě**: nasazení Azure virtuální síti (VNET) poskytuje lepší zabezpečení a izolace pro vaše Azure Redis mezipaměti, jakož i podsítí, zásady řízení přístupu a další funkce pro další omezit přístup. Další informace najdete v tématu [jak nakonfigurovat virtuální sítě podpory pro mezipaměť Redis Premium Azure](cache-how-to-premium-vnet.md).
-   **Konfigurace Redis**: V Standard a Premium úrovně, můžete nakonfigurovat Redis Keyspace oznámení.
-   **Maximální počet připojení klientů**: Premium osy nabízí maximální počet klienti, které se můžete připojit k Redis s vyšším číslem připojení pro větší velikost mezipaměti. [Sdělte v nápovědě k ceny stránku Podrobnosti](https://azure.microsoft.com/pricing/details/cache/).
-   **Vyhrazené základní Redis serveru**: ve vrstvě Premium libovolné velikosti mezipaměti nainstalovaný vyhrazené základní pro Redis. V Basic/standardu úrovní C1 velikost a výše vyhrazené základní Redis serveru.
-   **Redis je jedním podprocesem** tak s více než dvě jádra Microsoft neposkytuje žádnou další výhody přes s jenom dvě jádra, ale OM větších obvykle obsahují větší šířku pásma než menší velikosti. Pokud server mezipaměti nebo klient dosáhne omezení šířky pásma, získáte časové limity na straně klienta.
-   **Zvýšení výkonu**: mezipaměti ve vrstvě Premium zavedení na hardware, který má rychlejší procesorů a poskytuje lepší výkon ve srovnání s Basic nebo standardní osy. Premium osy mezipaměti mít vyšší výkon a nižší čekacích dob.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure Redis mezipaměti výkonu

Následující tabulka zobrazuje zjištěné při testování různých velikostech standardní hodnoty maximální šířky pásma a Premium ukládá pomocí `redis-benchmark.exe` z Iaas OM proti koncový bod Azure Redis mezipaměti. Všimněte si, že tyto hodnoty nemusí a že bez SLA těchto čísel, ale měli typické. Měli byste načíst otestovat vlastní aplikaci určit velikost správné mezipaměti aplikace.

Z této tabulky jsme upoutat k závěrům.

-   Výkon mezipaměti, které mají stejnou velikost je vyšší ve vrstvě Premium ve srovnání s standardní osy. Výkon P1 je s 6 mezipaměti GB 140K RPS ve srovnání s 49 K pro C3.
-   S Redis clusterů výkon lineárně zvyšuje jako zvýšení počtu shards (uzly) v clusteru. Pokud vytvoříte P4 clusteru 10 shards, pak dostupné výkon je například 250K * 10 = 2,5 milionů RPS.
-   Výkon pro větší velikostí klíče je vyšší ve vrstvě Premium ve srovnání s standardní osy.

| Ceny osy             | Velikost   | Procesor vzorky | Dostupné šířky pásma                                    | 1 kilobajtů klíč                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standardní mezipaměti velikosti** |        |           | **MB sec (Mb/ne) / megabajtů za sekundu (MB/ne)** | **Požadavky na sekundu (RPS)**            |
| C0                       | 250 MB | Sdílené    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2.5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1 000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Velikost mezipaměti Premium**  |        | **Procesor jádra za shard**  |                                         | **Požadavky za sekundu (RPS), za shard** |
| P1                       | 6 GB   | 2         | 1 000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | částku 250 000 jedné                                   |


Pokyny ke stažení nástroje Redis jako `redis-benchmark.exe`, najdete v článku [Jak lze spustit Redis příkazy?](#cache-commands) oddílu.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>Jaké oblasti měli můžu vyhledat Moje mezipaměti?

Pro dosažení nejlepších výsledků a nejnižší latence vyhledejte mezipaměť Redis Azure ve stejné oblasti jako klientská aplikace mezipaměti.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Jak mám fakturované mezipaměti Redis Azure?

Azure ceny Redis mezipaměti je [tady](https://azure.microsoft.com/pricing/details/cache/). Ceny stránka obsahuje ceny jako hodinovou sazbu. Na základě za minutu od doby vytvořenou mezipaměti až do mezipaměti odstraněna vám účtovat poplatky mezipaměti. Zastavení nebo pozastavení fakturace mezipaměť i žádným způsobem.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Mám používat Azure Redis mezipaměti se Azure Government Cloud nebo cloudového Čína Azure?

Ano, Azure Redis mezipaměti je k dispozici v cloudu pro státní správu Azure a Azure Čína cloudu. Poznámka: adresy URL pro přístup k a Správa mezipaměti Redis Azure se liší v cloudu pro státní správu Azure Azure Čína cloudu ve srovnání s Azure veřejné cloudu. Další informace o aspektech při použití Azure Redis mezipaměti v cloudu pro státní správu Azure a Azure Čína cloudu najdete v článku [Azure Government databází – Azure Redis mezipaměti](../azure-government/documentation-government-services-database.md#azure-redis-cache) a [Azure Čína Cloud - Azure Redis mezipaměti](https://www.azure.cn/documentation/services/redis-cache/).

Informace o použití mezipaměti Redis Azure pomocí prostředí PowerShell v cloudu pro státní správu Azure a Azure Čína cloudu najdete v článku [jak se připojit ke cloudu pro státní správu Azure nebo cloudového Čína Azure](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Co možnosti konfigurace StackExchange.Redis dělat?

StackExchange.Redis má mnoho možností. Tato část pojednává o některých běžných nastavení. Podrobnější informace o možnostech StackExchange.Redis najdete v tématu [Konfigurace StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Popis|Doporučení
---|---|---
AbortOnConnectFail|Nastavit na hodnotu true, připojení nebude při připojení po selhání sítě.|Nastavit na hodnotu false a zpřístupněte StackExchange.Redis připojení automaticky.
ConnectRetry|Počet opakování pokusy o připojení během počáteční připojení.| Viz následující poznámky pokyny. |
ConnectTimeout|Časový limit v ms pro připojení operace.| Viz následující poznámky pokyny. |

Ve většině případů jsou výchozí hodnoty klienta dostatečné. Můžete optimalizovat možnosti podle svého pracovního vytížení.

-   **Opakování**
    -   Obecné pokyny pro ConnectRetry a ConnectTimeout je selhání rychlé a opakovat znovu. To je založený na vaší pracovního vytížení a množství času na výpočet průměru ho bude váš klient příkaz Redis a dostanete odpověď.
    -   Informujte StackExchange.Redis automatické připojení namísto Kontrola stavu připojení a znovu připojit sami. **Nepoužívejte pomocí vlastnosti ConnectionMultiplexer.IsConnected**.
    -   Snowballing - někdy může nastat problému, kdy se opakování a tím snowballs a nikdy obnoví. Měli byste zvážit v tomto případě, že pomocí algoritmu exponenciální zdvojnásobení opakovat podle popisu v [Opakovat obecné pokyny](best-practices-retry-general.md) publikovat ve skupině Microsoft Patterns a postupy.
-   **Hodnoty časového limitu**
    -   Zvažte svého pracovního vytížení a podle toho nastavit hodnoty. Pokud ukládáte velké hodnoty, nastavte časový limit na hodnotu vyšší.
    -   Nastavení `AbortOnConnectFail` false a nechat StackExchange.Redis znovu za vás.
    -   Použijte jednu ConnectionMultiplexer instanci aplikace. LazyConnection slouží k vytvoření jedna instance, který vám vrátil vlastnosti připojení, jak je vidět v okně [připojit k mezipaměti pomocí ConnectionMultiplexer předmětu](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Nastavit `ConnectionMultiplexer.ClientName` vlastností jedinečný název instance aplikace pro účely diagnostiky.
    -   Použití více `ConnectionMultiplexer` instance pro vlastní úloh.
    -   Pokud máte odlišnými zatížení v aplikaci můžete začít sledovat tento model. Příklad:
    -   Můžete mít jednu multiplexor týkající se velké klíče.
    -   Můžete mít jednu multiplexor týkající se malé klíče.
    -   Můžete nastavit jiné hodnoty pro časové limity připojení a opakujte logiky pro každou ConnectionMultiplexer, který používáte.
    -   Nastavit `ClientName` vlastnost jednotlivých multiplexor Nápověda k diagnostice.
    -   To vede k více Modernější latence za `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>K čemu Redis mezipaměti klienti můžou pomocí?

Jednou z skvělých možností Redis je, že existuje řada klientů podpora spousty jazyků různých vývoj. Aktuální seznam klientů najdete v tématu [Redis klienty](http://redis.io/clients). Podrobné pokyny, které zahrnuje několik různých jazycích a klientů vás [naučí používat Azure Redis mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md) a klikněte na požadovaný jazyk ze přepínač jazyků na začátku tohoto článku.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Existuje místní emulátor pro mezipaměti Redis Azure?

Neexistuje žádný místní emulátor pro Azure Redis mezipaměti, ale běžet MSOpenTech verzi redis server.exe [Redis nástroje příkazového řádku](https://github.com/MSOpenTech/redis/releases/) v místním počítači a připojení k němu seznámení podobným se můžete setkat emulátoru místní mezipaměti, jak je vidět v následujícím příkladu.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Volitelně můžete nakonfigurovat souboru [redis.conf](http://redis.io/topics/config) lépe podle [výchozího nastavení mezipaměti](cache-configure.md#default-redis-server-configuration) pro vašeho online Azure Redis mezipaměť podle potřeby.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Spouštění Redis příkazy

Můžete použít jeden z příkazů uvedený v části [Redis příkazy](http://redis.io/commands#) s výjimkou příkazy jsou uvedené u [Redis příkazy nejsou podporované v Azure Redis mezipaměti](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Ke spuštění Redis příkazů máte několik možností.

-   Pokud máte standardní nebo Premium mezipaměti, můžete spustit Redis příkazy pomocí [Redis konzoly](cache-configure.md#redis-console). To umožňuje zabezpečené spusťte Redis příkazy Azure portálu.
-   Můžete taky použít nástroje Redis příkazového řádku. Je používat, proveďte následující kroky.
-   Stáhněte si [Redis nástroje příkazového řádku](https://github.com/MSOpenTech/redis/releases/).
-   Připojení k mezipaměti pomocí `redis-cli.exe`. Předat mezipaměti koncový bod pomocí, na které -h přejít a stisknutím klávesy – jak je vidět v následujícím příkladu.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Poznámka nástroje příkazového řádku Redis něm něco nefunguje s portem SSL, ale můžete použít nástroj jako `stunnel` zabezpečené připojení nástroje SSL port podle pokynů v příspěvku na blogu [Oznamující ASP.NET relace poskytovatele stavu Redis verzi Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Proč nemá Azure Redis mezipaměti MSDN knihovny tříd třeba některé další služby Azure?

Microsoft Azure Redis mezipaměti vychází z oblíbených otevřít zdroj Redis mezipaměti a dají se otevřít tak, že širokou škálu [Redis klientů](http://redis.io/clients) které jsou dostupné pro mnoho jazyky. Každý klient obsahuje vlastní rozhraní API, která volá instance mezipaměti Redis pomocí [Redis příkazy](http://redis.io/commands).

Protože každého klienta se liší, je na webu MSDN; ne jeden odkaz centralizované třídy Místo toho každého klienta udržuje vlastní si přečtěte následující dokumentaci odkaz. Kromě toho si přečtěte následující dokumentaci odkaz existuje několik výukové programy pro znázorňující, jak začít s Azure Redis mezipaměti pomocí různých jazycích a klienti mezipaměti. Přístup k těchto kurzech získáte informace [o použití Azure Redis mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md) a klikněte na požadovaný jazyk ze přepínač jazyk na začátku tohoto článku.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Můžete použít Azure Redis mezipaměti jako mezipaměti relace PHP?

Ano, jako mezipaměti relace PHP použít Azure Redis mezipaměti, zadat připojovací řetězec do mezipaměti Redis Azure instance v `session.save_path`.

>[AZURE.IMPORTANT] Pokud chcete použít Azure Redis mezipaměti jako mezipaměti relace PHP, musíte URL kódovat klíč zabezpečení používá pro připojení do mezipaměti, jak je vidět v následujícím příkladu.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Nejsou-li klávesu překódován jako adresa URL, může se zobrazit výjimku podobná této:`Failed to parse session.save_path`

Další informace o používání Redis mezipaměti jako mezipaměti PHP relace s klientem PhpRedis najdete v tématu [rutiny PHP relace](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Kdy si mám povolit port protokol SSL pro připojení k Redis?

Redis server nepodporuje SSL mimo pole, ale znamená Azure Redis mezipaměti. Pokud se připojujete k Azure Redis mezipaměti a klientem podporuje protokol SSL, jako je StackExchange.Redis, byste měli použít protokol SSL.

Všimněte si, že je ve výchozím nastavení pro nové instance Azure Redis mezipaměti zakázaná port a protokol SSL. Pokud váš klient nepodporuje SSL, musíte povolit port není SSL podle pokynů v části [přístup porty](cache-configure.md#access-ports) v článku [Konfigurace mezipaměti v Azure Redis mezipaměti](cache-configure.md) .

Například redis nástroje `redis-cli` nefungují s portem SSL, ale můžete použít nástroj jako `stunnel` zabezpečené připojení nástroje SSL port podle pokynů v příspěvku na blogu [Oznamující ASP.NET relace poskytovatele stavu Redis verzi Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Pokyny pro stažení nástroje Redis, najdete v článku [Jak lze spustit Redis příkazy?](#cache-commands) oddíl.



### <a name="what-are-some-production-best-practices"></a>Jaké jsou některé osvědčené postupy výrobní?

-   [Doporučené postupy StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Konfigurace a koncepty](#configuration-and-concepts)
-   [Testování výkonu](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Doporučené postupy StackExchange.Redis

-   Nastavení `AbortConnect` NEPRAVDA, nechte ConnectionMultiplexer připojení automaticky. [Podrobnosti najdete v části tady](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Opakované použití ConnectionMultiplexer - nevytvářejte nový název pro každý požadavek. `Lazy<ConnectionMultiplexer>` Vzorek [ukazuje tato část](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) důrazně doporučujeme.
-   Redis funguje nejlépe s nižšími hodnotami, takže zvažte dělení větší data do několika kláves. V [to Redis diskuse](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)je považován za "velké" 100 kb. Přečtěte si [Tento článek](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) příklad problém, který může být způsobený velké hodnoty.
-   Konfigurace [Nastavení fondu podprocesů](#important-details-about-threadpool-growth) tak, aby se zabránilo časové limity.
-   Použijte aspoň connectTimeout výchozí 5 sekund. To by poskytnout StackExchange.Redis dostatek času znovu vytvořit připojení pro nečekané blip sítě.
-   Uvědomte si výkonu náklady spojené s různých operacích, které používáte. Například `KEYS` příkaz je operaci O(n) a je nutno. Na [webu redis.io](http://redis.io/commands/) obsahuje podrobnosti kolem složitost času pro každou akci, která podporuje. Klikněte na jednotlivé příkazy zobrazíte složitost pro každou akci.

#### <a name="configuration-and-concepts"></a>Konfigurace a koncepty

-   Použijte standardní nebo vrstvy Premium pro systémy výroby. Základní osy je systém jeden uzel s žádné replikace dat a bez SLA. Taky pomocí alespoň C1 mezipaměti. C0 mezipaměti jsou skutečně určeny jednoduché zařízením nebo zkoušení scénáře.
-   Myslete na to, že je Redis úložiště dat **V paměti** . V [tomto článku](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , abyste si byli vědomi scénáře, které může dojít ke ztrátě dat.
-   Vývoj systému tak, aby mohl zpracovávat připojení blips [kvůli opravy a překlopení](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Testování výkonu

-   Začněte tím, že pomocí `redis-benchmark.exe` zjistěte, možná výkon před zápisem vlastní výkon testů. Poznámka: Tento redis srovnávacích nepodporuje SSL, proto bude mít [Povolit port Non-SSL portálu Azure](cache-configure.md#access-ports) před spustíte test. Příklady naleznete v tématu [Jak můžu testu výkonnosti a otestujte výkon Moje mezipaměti?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Klient OM pro testování by měl být na stejnou oblast jako instanci mezipaměti Redis.
-   Doporučujeme používat Dv2 OM řady pro vašeho klienta a budou mít lepší hardware vám umožní dosažení nejlepších výsledků.
-   Zkontrolujte, jestli váš klient OM zvolíte obsahuje aspoň tolik výpočetních a šířky pásma funkce jako mezipaměť testujete. 
-   Povolte VRSS v klientském počítači, pokud používáte Windows. [Podrobnosti najdete v části tady](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium osy Redis instance bude mít lepší sítě latence a výkon protože spuštěných pro lepší hardware pro vytížení procesoru a sítě.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Jaké jsou některé důležité informace o při použití běžnými příkazy Redis?

-   Není vhodné spouštět některé Redis příkazy, které trvat dlouho dokončete bez vysvětlení vlivu tyto příkazy.
    -   Například v není možné spustit příkazu [klíče](http://redis.io/commands/keys) výrobní jako může trvat dlouho vrátit podle počtu klíče. Redis je jedním podprocesem server a zpracuje příkazy jeden po druhém. Pokud jste ještě další příkazy po KLÍČŮ, nedojde k jejich zpracování dokud Redis nezpracuje příkazu klíče. Na [webu redis.io](http://redis.io/commands/) obsahuje podrobnosti kolem složitost času pro každou akci, která podporuje. Klikněte na jednotlivé příkazy zobrazíte složitost pro každou akci.
-   Velikosti klíče - mám použít malé klíč/hodnoty nebo velký klíč /? Obecně záleží na scénáře. Pokud váš scénář vyžaduje větší klíče pak můžete upravit ConnectionTimeout a opakování hodnot a upravte logiky opakovat. Z hlediska serveru Redis jsou nižšími hodnotami pozorovanými mít lepší výkon.
-   To znamenat vyšší hodnoty nelze uložit v Redis; je třeba znát následující skutečnosti. Čekacích dob budou vyšší. Pokud jste ještě jednu sadu dat, která je větší a je menší, můžete použít několika instancích spuštěných ConnectionMultiplexer, konfigurace jednotlivých s jinou sadu časový limit a opakování hodnot, podle popisu v předchozí části [co možnosti konfigurace StackExchange.Redis dělat](#cache-configuration) .



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Jak lze testu výkonnosti a otestujte výkon Moje mezipaměti?

-   [Povolení diagnostiky mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , budete moct [Sledování](cache-how-to-monitor.md) stavu mezipaměť. Metriky můžete zobrazit v portálu Azure a můžete [Stáhnout a zkontrolujte](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) pomocí nástroje podle svého výběru.
-   Můžete redis benchmark.exe načíst test Redis serveru.
-   Zajistěte, aby načítání testování klienta a mezipaměť Redis ve stejné oblasti.
-   Pomocí redis cli.exe a sledovat mezipaměti pomocí příkazu informace.
-   Pokud vaše zatížení způsobuje vysoké paměti fragmentace pak můžete by měl škálování větší velikost mezipaměti.
-   Pokyny pro stažení nástroje Redis, najdete v článku [Jak lze spustit Redis příkazy?](#cache-commands) oddíl.

Následující obrázek je příkladem použití redis benchmark.exe. Přesné výsledky spusťte tento příkaz z OM ve stejné oblasti jako mezipaměť.

-   Použití 1 datové k žádosti o propojené kanály nastavit test

    redis benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t SET - n 1000000 -d 1 024 - P 50
    
-   Test propojené kanály získat požadavků využívajících 1 datové k. 
    Poznámka: Spuštění sadu testování výše uvedeným nejdřív k naplnění mezipaměti
    
    redis benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t GET -n 1000000 - d 1 024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Důležité podrobnosti o fondu podprocesů LOGLINTREND

Fondu podprocesů CLR má dva typy podprocesů - "Pracovníka" a "V/v Port pro dokončení" (označovaná taky jako IOCP) vláknech. 

-   Pracovní vlákna se používají pro činnosti, jako zpracování `Task.Run(…)` nebo `ThreadPool.QueueUserWorkItem(…)` metody. Tyto vlákna také využívají různými součástmi v CLR práce potřebujete-li dojde na podprocesem na pozadí.
-   Vlákna IOCP se používají důvody asynchronní vstupů/výstupů chování (například čtení ze sítě). 

Fondu vlákna obsahuje nové pracovní vlákna nebo podprocesů dokončení vstupu a výstupu služba (bez jakékoli omezení), dokud nedosáhnete nastavení "Minimální" u jednotlivých typů podprocesu. Ve výchozím nastavení minimální počet vláken nastavenou počet procesorů v systému. 

Až počet stávající (zaneprázdněná) podprocesy přístupů se "" minimální počet, fondu podprocesů omezení vyjadřující se vloží nové podprocesy jeden vlákna za 500 milisekund. To znamená, že pokud systému získá shluk práce by musel zřetězení IOCP, budou zpracovány, které fungují velmi rychle. Ale pokud požadavků práce je větší než nastavení nakonfigurované "Minimální", bude některé zpoždění zpracování některé úkoly fondu podprocesů čeká na jednu z těchto akcí problémy.

1. Existující vlákno uvolní zpracovat práce.
1. Žádné existující vlákna bude zdarma 500ms, tak, aby se vytvoří nový podproces.

V podstatě znamená to, že po větší než Min vláknech počet zaneprázdněná budete pravděpodobně platíte zpoždění 500ms dříve, než je pomocí aplikace v síti. Je důležité si všimněte si, že při zřetězení existující zůstane nečinnosti po dobu delší než 15 sekund (podle můžu nezapomeňte), bude vyčistí a tento cyklus růstu a redukce můžete zopakovat.

Pokud podíváme na příklad chybové zprávy z StackExchange.Redis (vytvořit 1.0.450 nebo novější), zobrazí se, že teď vytiskne fondu podprocesů statistiky (IOCP a pracovní podrobnější informace viz níže).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Ve výše uvedeném příkladu můžou vidět, že IOCP podprocesu jsou 6 zaneprázdněná podprocesů a konfigurace systému umožňuje 4 minimální vláknech. V tomto případě klienta by pravděpodobně zobrazila dvou zpoždění 500 ms protože 6 > 4.

Všimněte si, že můžete StackExchange.Redis přístupů časové limity Pokud LOGLINTREND IOCP či PRACOVNÍKA podprocesů získá omezena.

### <a name="recommendation"></a>Doporučení

Možnost tyto informace, důrazně doporučujeme, aby zákazníci nastavte minimální konfigurace hodnotu pro vláknech IOCP a pracovní něco větší než nebo rovná hodnotě. Jsme nelze vodítko one-size-fits-all na co tato hodnota musí být hodnota vpravo pro jednu žádost proto bude moc vysoké nebo nízké pro jiné aplikace. Toto nastavení mohou ovlivnit výkon jiné části složitých aplikací, takže jednotlivé zákazníky potřebuje k úpravě toto nastavení pro své potřeby. Je dobré počáteční se 200 nebo 300, otestujte a doladit podle potřeby.

Jak nakonfigurovat toto nastavení:

-   Technologie ASP.NET ["minIoThreads" nastavení][] v části použít `<processModel>` konfigurace prvek web.config. Pokud používáte uvnitř Azure weby, toto nastavení používají, nebude vystaven pomocí možnosti konfigurace. Však můžete pořád by měla nastavovaná programově (viz níže) z způsobu Application_Start v global.asax.cs.

> **Důležitá poznámka:** hodnoty uvedené v této konfigurace element je nastavení *na základní* . Například, pokud máte 4 core počítače a chcete nastavení minIOThreads je 200 za běhu, použijete `<processModel minIoThreads="50"/>`.

-   Mimo ASP.NET použijte [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) ROZHRANÍ API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Povolení globálního katalogu serveru při použití StackExchange.Redis získat další výkon na straně klienta

Povolení globálního katalogu serveru můžete optimalizovat klienta a poskytnout lepší výkon a výkon při používání StackExchange.Redis. Další informace o serveru ° c a jak jej povolit naleznete v následujících článcích.

-   [Chcete-li povolit ° c serveru](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Základy uvolnění paměti](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Uvolnění paměti a výkon](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Jak můžu sledovat výkon Moje mezipaměti a stavu?

Microsoft Azure Redis mezipaměti instance můžete sledovat [Azure portálu](https://portal.azure.com). Můžete zobrazit metriky, připnout metriky grafy se Startboard, přizpůsobení rozsah dat a časů sledování grafy, přidání a odebrání metriky grafy a nastavení upozornění, když jsou splněné určité podmínky. Další informace najdete v tématu [Sledování Azure Redis mezipaměti](cache-how-to-monitor.md).

**Podpora + Poradce při potížích s** část zásuvné Redis mezipaměti **Nastavení** obsahuje také několik nástroje pro sledování a řešení potíží s vaší mezipaměti. 

-   **Poradce při potížích s** informacemi o běžné problémy a strategie pro jejich řešení.
-   **Protokolů auditování** obsahuje informace o akce provádějí mezipaměť. Můžete také filtrování rozbalte toto zobrazení zahrnout další zdroje informací.
-   **Stav zdroje** sleduje zdroj a zjistíte, jestli je spuštěný podle očekávání. Další informace o stavu služby Azure prostředku najdete v článku [Přehled stavu Azure prostředků](../resource-health/resource-health-overview.md).
-   **Nová žádost o podporu** poskytuje možnosti otevřete žádost o podporu pro mezipaměť.

Tyto nástroje umožňují sledovat stav instancí Azure Redis mezipaměti a vám pomůžou se správou mezipaměti aplikace. Další informace najdete v tématu [podporu a odstraňování problémů s nastavením](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Mezipaměť Diagnostika úložiště účtu nastavení změnit, co se stalo

Mezipaměti ve stejné oblasti a předplatné sdílet stejné úložiště nastavení diagnostických nástrojů a pokud je konfigurace změněné (Diagnostika povolit nebo zakázat nebo změna účtu úložiště) platí pro všechny mezipaměti předplatné, které jsou v dané oblasti. Pokud jste changed nastavení diagnostických nástrojů pro mezipaměť, zkontrolujte, pokud jste changed diagnostické nastavení pro jiné mezipaměti ve stejném předplatné a oblast. Jedním ze způsobů ke kontrole je zobrazíte v protokolech auditování mezipaměť pro `Write DiagnosticSettings` události. Další informace o práci s protokolů auditování najdete v článku [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) a [auditování operace s správce prostředků](../resource-group-audit.md). Další informace o sledování událostí Azure Redis mezipaměti najdete v článku [operace a upozornění](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Proč je povolené diagnostických nástrojů pro některé nové mezipaměti, ale ne ostatní?

Mezipaměti ve stejné oblasti a předplatné sdílet stejnou úložiště nastavení diagnostických nástrojů. Je-li vytvořit novou mezipaměť ve stejné oblasti a předplatné jako jiné mezipaměti, který má povolené diagnostických nástrojů diagnostiky povolili na novou mezipaměť pomocí stejné nastavení.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Proč se mi zobrazují časové limity?

Časové limity dojít v klienta, který můžete použít ke komunikaci s Redis. Převážně Redis server nepodporuje vypršení časového limitu. Když příkazu se odesílá na server Redis, příkaz je ve frontě a Redis serveru postupně vyzvedne příkazu a spustí jej. Ale klienta můžete časový limit během tohoto procesu a pokud má výjimku je umocněné na straně volání. Další informace o problémech, časový limit najdete v článku [řešení potíží straně klienta](cache-how-to-troubleshoot.md#client-side-troubleshooting) a [StackExchange.Redis vypršení časového limitu výjimky](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Proč je můj klienta to od ní odpojilo z mezipaměti?

Následující seznam uvádí některé běžné důvod, proč se odpojit mezipaměti.

-   Příčiny straně klienta
    -   Klientská aplikace byla znovu nasadit.
    -   Klientská aplikace provedena operace změny měřítka.
        -   V případě Cloudovým službám a webové aplikace může být příčinou automatické měřítko.
    -   Sociální sítě vrstvy na straně klienta se měnit.
    -   Přechodné chybě v klientovi nebo v síti uzlech mezi klientem a serveru.
    -   Byly dosažení mezní šířky pásma.
    -   Procesor vázaná operace trvala příliš dlouho k dokončení.
-   Příčiny straně serveru
    -   Standardní mezipaměti nabízející služby Azure Redis mezipaměti, které iniciuje před selháním z primární uzel sekundární uzel.
    -   Azure byl opravy instance, kdy byla nasazena mezipaměti
        -   Může to být Redis serveru aktualizací nebo obecné OM údržbu.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Které Azure mezipaměti nabízení je pro mě nejlepší?

>[AZURE.IMPORTANT]Podle loňského roku [oznámení](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)budou do důchodu 30 listopad 2016 služby Azure spravovaných služby mezipaměti a Azure v roli mezipaměti. Náš doporučení, je použít [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/). Informace o migraci najdete v tématu [Migrace z mezipaměti služba spravovaných Azure Redis mezipaměti](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis mezipaměti
Azure Redis mezipaměti je všeobecně dostupná v jejich velikosti až 53 GB a je dostupné SLA 99,9 %. Nové [osy premium](cache-premium-tier-intro.md) nabízí velikosti až 530 GB a podporu pro clusterů VNET a uchovávání s SLA 99,9 %.

Azure Redis mezipaměti umožňuje zákazníci použít zabezpečené, vyhrazené Redis mezipaměti, spravuje Microsoft. Pomocí této nabídce si můžete využít sadu bohaté funkcí a ekosystému poskytovanou Redis, spolehlivé hostingu a sledování od Microsoftu.

Na rozdíl od tradiční mezipaměti, které se zabývají pouze klíč dvojice je Redis Oblíbené pro jeho vysoce performant datové typy. Redis taky podporuje atomová operace na tyto typy, jako je připojení k řetězec; přičítání v algoritmus hash; předání do seznamu. Výpočet sadu průniku, union a rozdíl; nebo předání člen s nejvyšší hodnocení seřazené sady. Další funkce zahrnují podporu transakce pub/sub, Lua skriptování klávesy s omezenou time-to-live a konfigurace nastavení, abyste měli Redis více podobají tradiční mezipaměti.

Jiné klíčové poměr ke Redis úspěchu je správný, zářivý otevřít zdroj ekosystému integrované kolem něj. Se projeví v různého Redis klientů k dispozici ve více jazycích. Umožňuje používat téměř libovolném zátěží na projektu, který by vytvoříte uvnitř Azure. 

Další informace o začínáte pracovat s Azure Redis mezipaměti najdete v článku [jak použití Azure Redis mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md) a [si přečtěte následující dokumentaci Azure Redis mezipaměti](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Služba spravovaných mezipaměti
[Spravovat mezipaměti služba je nastavena na vyřadit 30 listopad 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>V roli mezipaměti
[V roli mezipaměti nastavenou vyřadit 30 listopad 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





[nastavení konfigurace "minIoThreads"]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
