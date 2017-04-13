<properties 
    pageTitle="Jak sledovat Azure Redis mezipaměti | Microsoft Azure" 
    description="Zjistěte, jak sledovat výkon a stavu instance mezipaměti Redis Azure" 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Jak sledovat Azure Redis mezipaměti

Azure Redis mezipaměti nabízí několik možností sledování instance mezipaměti. Můžete zobrazit metriky, připnout metriky grafy se Startboard, přizpůsobení rozsah dat a časů sledování grafy, přidání a odebrání metriky grafy a nastavení upozornění, když jsou splněné určité podmínky. Tyto nástroje umožňují sledovat stav instancí Azure Redis mezipaměti a vám pomůžou se správou mezipaměti aplikace.

Pokud jsou povoleny diagnostiky mezipaměti, metriky pro instance Azure Redis mezipaměti shromažďují zhruba každých 30 sekund a uložené, mohou být zobrazena v grafech metriky a vyhodnocen pravidly upozornění.

Metriky mezipaměti byly shromážděny pomocí Redis příkaz [informace](http://redis.io/commands/info) . Další informace o různých informace hodnoty pro každý mezipaměti míru najdete v tématu [dostupné metriky a vytváření sestav funkce intervalů](#available-metrics-and-reporting-intervals).

Chcete-li zobrazit metriky mezipaměti, [přejděte](cache-configure.md#configure-redis-cache-settings) do mezipaměti instance [Azure portálu](https://portal.azure.com). Metriky pro instance Azure Redis mezipaměti jsou dostupné na zásuvné **Redis metriky** .

![Redis metriky][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Pokud tato zpráva se zobrazí v zásuvné **Redis metriky** , postupujte podle kroků uvedených v části [Povolit mezipaměti diagnostiky](#enable-cache-diagnostics) povolte diagnostiky mezipaměti.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Zásuvné **Redis metriky** má **Sledování** grafů, které se zobrazí metriky mezipaměti. Každý graf můžete přizpůsobit přidáním nebo odebráním metriky a změnit interval vytváření sestav. Pro prohlížení a konfigurace operace a výstrahy, má zásuvné **Redis mezipaměti** oddílem **operace** zobrazující mezipaměti **události** a **upozornění pravidla**.

## <a name="enable-cache-diagnostics"></a>Povolení diagnostiky mezipaměti

Azure Redis mezipaměť poskytuje možnost diagnostiky data uložená v účtu úložiště, abyste mohli používat libovolné nástroje, které chcete přístup a zpracování dat přímo. Aby mezipaměti diagnostiky shromažďované ukládají a zobrazují na portálu Azure účet úložiště musí být nakonfigurované. Mezipaměti ve stejné oblasti a předplatné sdílet stejný účet ukládání diagnostických nástrojů a dojde ke změně konfigurace platí pro všechny mezipaměti předplatné, které jsou v dané oblasti.

Povolení a konfigurace diagnostiky mezipaměti, přejděte na zásuvné **Redis mezipaměti** pro instanci mezipaměti. Pokud ještě není povoleno diagnostiky, se zobrazí zpráva místo diagnostických nástrojů grafu.

![Povolení diagnostiky mezipaměti][redis-cache-enable-diagnostics]

Klikněte na zprávu a po zobrazení zásuvné **míru** klikněte na **Diagnostika nastavení** můžete povolit a nakonfigurovat diagnostiky nastavení instanci služby mezipaměti.

![Nastavení diagnostických nástrojů][redis-cache-diagnostic-settings]

![Konfigurace diagnostických nástrojů][redis-cache-configure-diagnostics]

Klikněte **na** tlačítka povolit diagnostiky mezipaměti a zobrazit konfigurace diagnostiky.

Klikněte na šipku vpravo od **Úložiště účtu** vyberte účet úložiště pro uložení dat diagnostiky. Nejlepších výsledků dosáhnete vyberte účet úložiště ve stejné oblasti jako mezipaměť.

Jakmile je diagnostický nakonfigurováno, klikněte na **Uložit** uložte konfiguraci. Všimněte si, že ho může chvíli trvat, změny se projeví.

>[AZURE.IMPORTANT] Mezipaměti ve stejné oblasti a předplatné sdílet stejnou úložiště nastavení diagnostických nástrojů a po konfiguraci změněné (Diagnostika povolit nebo zakázat nebo změna účtu úložiště) platí pro všechny mezipaměti předplatné, které jsou v dané oblasti.

Zobrazení uložené metriky, zkontrolujte tabulky ve vašem účtu úložiště s názvy, které začínají `WADMetrics`. Další informace o přístupu k uložené metriky mimo portálu Azure najdete v článku ukázkových [dat aplikace Access Redis mezipaměti sledování](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Pouze metriky, které jsou uložené na účtu vybrané úložiště jsou zobrazeny v portálu Azure. Pokud změníte účty úložiště, data v okně účet dříve konfigurované úložiště zůstávají k dispozici ke stažení, ale není zobrazeno Azure portálu.  

## <a name="available-metrics-and-reporting-intervals"></a>K dispozici metriky a vytváření sestav funkce intervalů

Metriky mezipaměti jsou uvedeny pomocí několika podřízenosti intervalech, včetně **minulých hodiny**, **dnes**, **minulého týdne**a **vlastní**. **Metriky** zásuvné pro každý metriky graf zobrazuje průměr, minimální a maximální hodnoty pro každou míru v grafu a některé metriky zobrazit součet pro vytváření sestav interval. 

Každý míru obsahuje dvě verze. Jednu míru opatření výkonu pro celou mezipaměť a mezipaměti, které používají [clusterů](cache-how-to-premium-clustering.md), druhý verzi míru, který obsahuje `(Shard 0-9)` výkonu míry název pro jeden shard v mezipaměti. Pokud mezipaměti má 4 shards, například `Cache Hits` je celková částka přístupů pro celou mezipaměť a `Cache Hits (Shard 3)` je právě přístupů pro tuto shard mezipaměti.

>[AZURE.NOTE] I v případě mezipaměti je nečinný s žádné připojené aktivní klientské aplikace, může se zobrazit některé mezipaměti činnost, třeba připojení klientů, využití paměti a provádí operace. Tato akce je normální během operace instance Azure Redis mezipaměti.

| Metriky            | Popis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Přístupy do mezipaměti        | Počet úspěšných klíčové hledání během určeného časového intervalu vytváření sestav. To mapuje `keyspace_hits` z Redis [informace](http://redis.io/commands/info) příkaz.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Neúspěšné přístupy do mezipaměti      | Počet neúspěšných klíčové hledání během určeného časového intervalu vytváření sestav. To mapuje `keyspace_misses` od příkazu Redis informace. Neúspěšné přístupy do mezipaměti není znamenat, že je problém s mezipamětí. Třeba při použití mezipaměti odložené programovací vzorku, aplikace vypadá první v mezipaměti pro položky. Pokud položku není (mezipaměti Nenalezeno), na položku načtené z databáze a přidá do mezipaměti na příště. Neúspěšné přístupy do mezipaměti jsou běžné chování mezipaměti odložené programovací vzorku. Pokud počet neúspěšných mezipaměti přístupů je vyšší než byste čekali, podívejte se na logiky aplikace, který naplní a bude číst z mezipaměti. Pokud je vyloučení položek z mezipaměti kvůli paměti tlaku a pak můžou být některé Neúspěšné přístupy do mezipaměti, ale lepší metriky ke sledování tlaku paměti bude `Used Memory` nebo `Evicted Keys`. |
| Připojení klientů | Počet připojení klientů do mezipaměti během určeného časového intervalu vytváření sestav. To mapuje `connected_clients` od příkazu Redis informace. Po vypršení [omezení připojení](cache-configure.md#default-redis-server-configuration) se nepovede následující pokusy o připojení do mezipaměti. Všimněte si, že i když není aktivní klientské aplikaci se pořád pravděpodobně několika instancí připojení klientů kvůli vnitřní procesy a připojení.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Odstraněný klíče      | Počet položek vyloučení z mezipaměti během zadaného podřízenosti intervalu termín splnění `maxmemory` limit. To mapuje `evicted_keys` od příkazu Redis informace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Vypršela platnost klíče      | Počet položek, kterým vypršela platnost z mezipaměti během určeného časového intervalu vytváření sestav. Tato hodnota mapuje `expired_keys` příkazem Redis informace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Získá              | Počet operací získat z mezipaměti během určeného časového intervalu vytváření sestav. Tato hodnota je součet z následujících hodnot z informace Redis příkaz všechny: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, a `cmdstat_getrange`a odpovídá součtu přístupy do mezipaměti a neúspěšných přístupů během vytváření sestav intervalu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis zatížení serveru | Procento cyklů, ve kterých Redis serveru teď na něčem pracuje zpracování a není čekání nedělá zpráv. Pokud čítač dosáhne 100 znamená serveru Redis překročil ceiling výkonu a procesor se nedají zpracovat pracovat některou rychleji. Pokud se vám zobrazuje velkém zatížení Redis serveru uvidíte vypršení časového limitu výjimky v klientovi. V tomto případě je třeba zvážit škálování nebo rozdělení data do několika mezipamětí.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Sady              | Počet operací sadu do mezipaměti během určeného časového intervalu sestav. Tato hodnota je součtem následující hodnoty z informace Redis příkaz všechny: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, a `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Celkové operace  | Celkový počet příkazy zpracovány serverem mezipaměti během určeného časového intervalu vytváření sestav. Tato hodnota mapuje `total_commands_processed` příkazem Redis informace. Všimněte si, že při Azure Redis mezipaměti se používá čistě pro pub/sub bude žádné metriky pro `Cache Hits`, `Cache Misses`, `Gets`, nebo `Sets`, ale bude `Total Operations` metriky, která odrážejí použití mezipaměti pro operace pub/sub.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Použité paměti       | Velikost mezipaměti pro dvojice klíč/hodnota v mezipaměti v Megabajtech používaný během určeného časového intervalu vytváření sestav. Tato hodnota mapuje `used_memory` od příkazu Redis informace. Nezahrnuje metadata nebo fragmentace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Použité paměti RSS   | Velikost mezipaměti používaný v Megabajtech během zadaného podřízenosti interval, včetně fragmentace a metadata. Tato hodnota mapuje `used_memory_rss` příkazem Redis informace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PROCESOR               | Využití procesoru server Azure Redis mezipaměti jako procento během určeného časového intervalu vytváření sestav. Tato hodnota mapy operační systém `\Processor(_Total)\% Processor Time` čítače.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Čtení mezipaměti        | Množství dat přečíst z mezipaměti v megabajtech (MB/ne) sekundu během určeného časového intervalu vytváření sestav. Tato hodnota je odvozeno z síťových karet, které podporují virtuální počítač, který je hostitelem mezipaměti a není Redis konkrétní. **Tato hodnota odpovídá šířka pásma používané mezipaměti. Pokud chcete nastavit upozornění u omezení šířky pásma sítě straně serveru, vytvořte ho pomocí `Cache Read` čítač. Podívejte se [v této tabulce](cache-faq.md#cache-performance) pro omezení pozorovanými šířky pásma pro různé mezipaměť ceny úrovní a velikosti.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Zápis mezipaměti       | Množství dat zapsat do mezipaměti v megabajtech (MB/ne) sekundu během zadaného vykazování interval. Tato hodnota je odvozeno z síťových karet, které podporují virtuální počítač, který je hostitelem mezipaměti a není Redis konkrétní. Tato hodnota odpovídá šířka pásma dat odeslaných z klienta do mezipaměti.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Jak zobrazit metriky a přizpůsobení grafu

Zobrazení přehledu metriky pro mezipaměť na zásuvné **Redis metriky** . Pro přístup k **Redis metriky** zásuvné zvolte **všechna nastavení** > **Redis metriky**.

![Redis metriky][redis-cache-redis-metrics]


Zásuvné **Redis metriky** obsahuje následující grafy.

| Redis metriky grafu | Zobrazené metriky     |
|------------------|-------------------|
| Čtení mezipaměti a mezipaměti psaní | Čtení mezipaměti |
|                            | Zápis mezipaměti |
| Připojení klientů      | Připojení klientů |
| Přístupů a neúspěšných přístupů  | Přístupy do mezipaměti        |
|                  | Neúspěšné přístupy do mezipaměti      |
| Celková příkazy   | Celkové operace  |
| Získá a sady    | Získá              |
|                  | Sady              |
| Využití procesoru         | PROCESOR           |
| Využití paměti      | Použité paměti   |
|                   | Použité paměti RSS |
| Redis zatížení serveru | Zatížení serveru   |
| Počet klíč | Celkový počet klíčů |
|           | Odstraněný klíče |
|           | Vypršela platnost klíče |


Podrobnější zobrazení metriky na určitý graf a můžete přizpůsobit graf klikněte na požadovaný graf z zásuvné **Redis metriky** zobrazíte zásuvné **míru** pro tento graf.

![Metriky zásuvné][redis-cache-metric-blade]

Všechna oznámení, které se nastavují metrice zobrazením grafu jsou uvedeny v dolní části zásuvné **míru** pro tento graf.

Pokud chcete přidat nebo odebrat metriky nebo můžete změnit interval sestav, zvolte **Upravit graf**.

Přidání nebo odebrání metriky z grafu, zaškrtněte políčko vedle názvu metriky. Pokud chcete změnit interval vytváření sestav, klikněte na požadovaný interval. Pokud chcete změnit **typ grafu**, klikněte na požadovaný styl. Po provedení požadované změny, klikněte na **Uložit**. 

![Úprava grafu][redis-cache-edit-chart]

Když kliknete na **Uložit** změny potrvají, dokud nebude opustíte zásuvné **míru** . Při přechodu zpět později, zobrazí se původní metrické a časový rozsah znovu. Další informace o úpravách grafy najdete v článku [metriky služby Monitor](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Metriky určitého časového období v diagramu zobrazíte ukazatele myši nad jeden z určitým pruhům nebo ukazatele na graf, který odpovídá požadovanou dobu a metriky pro tento interval zobrazují.

![Zobrazení podrobností o grafu][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Jak sledovat premium mezipaměti s clusterů

Premium mezipaměti, které mají [clusterů](cache-how-to-premium-clustering.md) povoleným může obsahovat maximálně 10 shards. Každý shard obsahuje vlastní metriky a tyto metriky agregován stanovit metriky mezipaměti jako celek. Každý míru obsahuje dvě verze. Jednu míru opatření výkon pro celou mezipaměť a druhý verzi míru, který obsahuje `(Shard 0-9)` výkonu míry název pro jeden shard v mezipaměti. Pokud mezipaměti obsahuje 3 shards, například `Cache Hits` je celková částka přístupů pro celou mezipaměť a `Cache Hits (Shard 2)` je právě přístupů pro tuto shard mezipaměti.

Každý sledování graf zobrazuje metriky nejvyšší úrovně mezipaměti spolu s metrikou pro každou shard mezipaměti.

![Sledování][redis-cache-premium-monitor]

Ukazatele myši přes datové body zobrazí podrobnosti o příslušném bodu v čase. 

![Sledování][redis-cache-premium-point-summary]

Vyšší hodnoty jsou obvykle souhrnné hodnoty pro ukládání do mezipaměti, zatímco nižšími hodnotami jednotlivé metriky shard. Všimněte si, že v tomto příkladu jsou tři shards a přístupy do mezipaměti rovnoměrně přes shards.

![Sledování][redis-cache-premium-point-shard]

Chcete-li zobrazit více podrobností klikněte na graf, chcete-li zobrazit rozšířené zobrazení na zásuvné **míru** .

![Sledování][redis-cache-premium-chart-detail]

Ve výchozím nastavení obsahuje každý graf čítače nejvyšší úrovně mezipaměti, jakož i výkonnosti pro jednotlivé shards. Můžete přizpůsobit tyto na zásuvné **Upravit graf** .

![Sledování][redis-cache-premium-edit]

Další informace o dostupných výkonnosti najdete v článku [dostupné metriky a vytváření sestav funkce intervalů](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Operace a oznámení

Oddíl **operací** na zásuvné **Redis mezipaměti** má **událostí** a **pravidla výstrah** oddílů.

![Oeprations][redis-cache-operations-events]

Pokud chcete zobrazit seznam nedávných operací mezipaměti, klikněte na graf **události** zobrazíte zásuvné **události** . Operace příkladem načítání a přístupové klávesy a aktivace a rozlišení upozornění pravidel. Další informace o každé události klikněte na požadovanou událost v zásuvné **události** .

Podrobnosti o událostech najdete v článku [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md).

V části **pravidla výstrah** zobrazí počet upozornění pro instanci mezipaměti. Pravidlo výstrahy umožňuje sledovat instanci mezipaměti a určitou metrických hodnotu dosažení mezní definovaná v pravidlu dostávat e-mailu. 

Upozornění pravidla jsou vyhodnoceny zhruba každých pět minut a po aktivaci pravidla výstrahy se odešle nakonfigurované upozornění. Aktivace upozornění pravidla a oznámení se nezobrazí okamžitě; doména může obsahovat zpoždění několik minut před aktivaci pravidla výstrahy a oznámení odeslané.

Upozornění pravidel můžete zobrazit a nastavit z zásuvné **míru** pro konkrétní sledování graf nebo z zásuvné **upozornění pravidla** .

Upozornění pravidla mají následující vlastnosti.

| Vlastnost pravidlo výstrahy                 | Popis                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zdroje                            | Zdroj vyhodnocení pravidlo výstrahy. Při vytváření pravidla výstrahy z mezipaměti Redis, mezipaměť je daný zdroj.                                                                                                                                                                                                                                                                                                                                                  |
| Jméno                                | Jméno, které jednoznačně identifikuje upozornění pravidla v rámci aktuální instance mezipaměti.                                                                                                                                                                                                                                                                                                                                                                                       |
| Popis                         | Volitelný popis pravidla výstrahy.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metriky                              | Metriky sledovat pravidlem upozornění. Seznam metriky mezipaměti najdete v článku dostupné metriky a vytváření sestav funkce intervalů.                                                                                                                                                                                                                                                                                                                                             |
| Podmínky                           | Operátor podmínky pravidlo výstrahy. Je možné volby: větší než, větší než nebo rovno, méně než je menší než nebo rovno                                                                                                                                                                                                                                                                                                                             |
| Mezní hodnota                           | Hodnota použitá k porovnání s metrikou pomocí operátoru nastavil vlastnost podmínka. V závislosti míru tuto hodnotu může být v bajtech/sekundu bajtů, % nebo count.                                                                                                                                                                                                                                                                                     |
| Období                              | Určuje období, po kterou je průměrná hodnota hodnoty metriky použít k porovnání pravidlo výstrahy. Například pokud období nad poslední hodiny, průměrnou hodnotu míru za předchozí hodiny interval slouží k porovnání. Pokud chcete potvrzení o je mezní hodnota došlo ke splnění kvůli zásobníku činnost, kratší období je vhodné. Pokud chcete být upozorňováni je trvalý aktivity nad prahové hodnoty, použijte delšího období. |
| E-mailové služby a dalších správců | V případě hodnoty true, Správce služeb a dalších správce jsou poslali e-mail upozornění při aktivaci.                                                                                                                                                                                                                                                                                                                                                                    |
| Další správce e-mailu      | Volitelné e-mailovou adresu pro další správce chcete být upozorňováni aktivaci na upozornění.                                                                                                                                                                                                                                                                                                                                                                    |

Jedinou prezentujícímu se odešle na aktivaci pravidla výstrahy. Jakmile překročení u pravidla typu a prezentujícímu se odešle, není znovu vyhodnocen pravidlo, dokud metriky spadá pod je mezní hodnota. Pokud metriky následně větší než mezní hodnota, znovu aktivovat upozornění a odešle oznámení o nové.

Všechna upozornění pravidla pro instanci mezipaměti zobrazíte kliknutím na část **upozornění pravidla** na zásuvné **Redis mezipaměti** . Chcete-li zobrazit pouze upozornění, které používají určité metriky pravidla, přejděte na zásuvné **míru** pro graf, který obsahuje tento míru.

![Upozornění pravidel][redis-cache-alert-rules]

Přidat pravidlo výstrahy, klikněte na **Přidat oznámení** z zásuvné **metrický** nebo zásuvné **upozornění pravidla** . 

Zadání kritérií požadované pravidla do zásuvné pravidlo **Přidat upozornění** a klikněte na **OK**. 

![Přidání pravidla výstrahy][redis-cache-add-alert]

>[AZURE.NOTE] Vytvoření pravidla výstrahy kliknutím **Přidat upozornění** od zásuvné **míru** pouze metriky zobrazené v grafu v této zásuvné zobrazí v rozevíracím seznamu **míru** . Po vytvoření pravidla výstrahy kliknutím **Přidat upozornění** od zásuvné **pravidla výstrah** všechny metriky mezipaměti jsou dostupné v rozevíracím seznamu **míru** .

Po uložení pravidlo upozornění se zobrazí na zásuvné **pravidla výstrah** i na zásuvné **míru** pro všechny grafy, které se zobrazí míru použité v pravidle výstrahy. Chcete-li upravit pravidlo výstrahy, klikněte na název pravidla výstrahy zobrazíte zásuvné **Upravit pravidlo** . Z zásuvné **Upravit pravidlo** můžete upravit vlastnosti pravidla, odstranit nebo zakázání výstrah pravidla nebo znovu povolit pravidla Pokud dříve zakázané.

>[AZURE.NOTE] Všechny změny, které uděláte vlastností pravidla může chvíli trvat, než se projeví na zásuvné **upozornění pravidla** nebo zásuvné **míru** .

Po aktivaci pravidla výstrahy e-mailu se neodesílají v závislosti na konfiguraci upozornění pravidla a upozornění, zobrazí se ikona v části **pravidla výstrah** na zásuvné **Redis mezipaměti** .

Pravidlo výstrahy se považuje za vyřešit při upozornění podmínka už vyhodnotí jako PRAVDA. Po přeložena podmínce upozornění pravidla ikonu výstrahy nahrazen příkazem značka zaškrtnutí. Podrobnosti o oznámení aktivací a řešení klikněte na část **události** na zásuvné **Redis mezipaměti** zobrazíte události na zásuvné **události** .

Další informace o upozornění v Azure najdete v tématu [přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Metriky na zásuvné Redis mezipaměti

Zásuvné **Redis mezipaměti** zobrazí následující kategorie metriky.

-   [Sledování grafy](#monitoring-charts)
-   [Použití grafů](#usage-charts)

### <a name="monitoring-charts"></a>Sledování grafy

V části **Sledování** má ** **narazí neúspěšných přístupů do, dostane, nastaví**, **připojení**a grafy **Celkové příkazy** **.

![Sledování grafů][redis-cache-monitoring-part]

**Sledování** grafy zobrazují následující metriky.

| Sledování grafu | Metriky mezipaměti     |
|------------------|-------------------|
| Přístupů a neúspěšných přístupů  | Přístupy do mezipaměti        |
|                  | Neúspěšné přístupy do mezipaměti      |
| Získá a sady    | Získá              |
|                  | Sady              |
| Připojení      | Připojení klientů |
| Celková příkazy   | Celkové operace  |

Informace o zobrazení metriky a přizpůsobení jednotlivé grafy v této části naleznete v části následující [zobrazení metriky a přizpůsobení metriky grafy](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Použití grafů

V části **použití** má **Redis zatížení serveru**, **Využití paměti**, **Šířka pásma sítě**a grafy **Využití procesoru** a taky zobrazí **ceny osy** instance mezipaměti.

![Použití grafů][redis-cache-usage-part]

Zobrazení **osy ceny** mezipaměti ceny úroveň a může být slouží k [Měřítko](cache-how-to-scale.md) mezipaměti různých ceny osy.

**Použití** grafy zobrazují následující metriky.

| Použití grafu       | Metriky mezipaměti |
|-------------------|---------------|
| Redis zatížení serveru | Zatížení serveru   |
| Využití paměti      | Použité paměti   |
| Šířka pásma | Zápis mezipaměti   |
| Využití procesoru         | PROCESOR           |

Informace o zobrazení metriky a přizpůsobení jednotlivých grafech v této části naleznete v části následující [zobrazení metriky a přizpůsobení metriky grafů](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



