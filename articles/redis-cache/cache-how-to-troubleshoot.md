<properties 
    pageTitle="Jak řešit problémy s Azure Redis mezipaměti | Microsoft Azure" 
    description="Zjistěte, jak vyřešit běžné problémy s Azure Redis mezipaměti." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Jak řešit problémy s Azure Redis mezipaměti

Tento článek obsahuje pokyny k řešení potíží s těchto kategorií Azure Redis mezipaměti problémů.

-   [Poradce při potížích straně klienta](#client-side-troubleshooting) – Tato část obsahuje pokyny identifikace a řešení problémů způsobeno aplikací připojení do mezipaměti Redis Azure.
-   [Poradce při potížích straně serveru](#server-side-troubleshooting) – Tato část obsahuje Rady, na identifikace a řešení problémů způsobil na straně serveru Azure Redis mezipaměti.
-   [Výjimky vypršení časového limitu StackExchange.Redis](#stackexchangeredis-timeout-exceptions) – Tato část obsahuje informace o řešení potíží při použití StackExchange.Redis klienta.


>[AZURE.NOTE] Několik kroků v této příručce obsahuje pokyny spusťte Redis příkazy a sledování různých měřítka. Další informace a pokyny najdete v článcích v části [Další informace](#additional-information) .

## <a name="client-side-troubleshooting"></a>Řešení potíží straně klienta


Tato část popisuje Poradce při potížích probíhajících z důvodu podmínky na klientské aplikace.

-   [Paměť tlaku na straně klienta](#memory-pressure-on-the-client)
-   [Požadavků provoz](#burst-of-traffic)
-   [Vysoký klienta využití procesoru](#high-client-cpu-usage)
-   [Překročení šířky pásma straně klienta](#client-side-bandwidth-exceeded)
-   [Velikost velké žádostí a odpovědí](#large-requestresponse-size)
-   [Co se stalo s mými daty v Redis](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Paměť tlaku na straně klienta

#### <a name="problem"></a>Problém

Tlaku paměti v klientském počítači vede k všechny typy problémy s výkonem, které lze zpozdit zpracování dat, která byla odeslána instancí Redis bez zpoždění. Po narazí paměti tlaku systému obvykle používá k datům stránky z fyzické paměti virtuální paměť, která je na disku. Tato *stránka chybující* způsobí, že systém výrazně zpomalit.

#### <a name="measurement"></a>Měření 

1.  Využití paměti počítače, abyste měli jistotu, že nepřekročí dostupnou pamětí sledovat. 
2.  Sledování `Page Faults/Sec` čítače. Většina systémů bude jste některé chyby stránky i během normální operace Dívejte se, jak to pro vrcholy pole v čítač stránky poruch výkonu, které odpovídají časové limity.

#### <a name="resolution"></a>Rozlišení

Upgrade vašeho klienta ke klientovi větší velikost OM s víc paměti nebo dostanete do svého vzorce využití paměti zmenšit consuption paměti.


### <a name="burst-of-traffic"></a>Požadavků provoz

#### <a name="problem"></a>Problém

Roztržení provoz společně s špatné `ThreadPool` nastavení může vést k zpožděním při zpracování dat odeslané serverem Redis, ale ještě nejste spotřebované množství na straně klienta.

#### <a name="measurement"></a>Měření 

Sledování jak vaše `ThreadPool` statistiky mění v čase použití kódu [takto](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Taky můžete prohlédnout `TimeoutException` zprávu od StackExchange.Redis. Tady je příklad:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Ve výše uvedené zprávě existuje několik věcí, které jsou zajímavé:

 1. Všimněte si, že v `IOCP` oddílu a `WORKER` oddílu máte `Busy` hodnota, která je větší než `Min` hodnotu. To znamená, že vaše `ThreadPool` nastavení potřebujete upravit.
 2. Můžete se taky podívat `in: 64221`. Tento údaj označuje, že 64211 bajtů přijali ve vrstvě socket jádra, ale nebyly dosud čtení aplikací (například StackExchange.Redis). Obvykle to znamená, že aplikace není načítání dat ze sítě rychle server je odesláním.

#### <a name="resolution"></a>Rozlišení

Konfigurace [Nastavení fondu podprocesů](https://gist.github.com/JonCole/e65411214030f0d823cb) a ujistěte se, že bude vlákna fondu podle požadavků scénářů rychle škálování.


### <a name="high-client-cpu-usage"></a>Vysoký klienta využití procesoru

#### <a name="problem"></a>Problém

Vysoké využití procesoru na straně klienta je údajem systému nemůžete zachovávat požádal k provedení práce. To znamená, že klienta se nemusí podařit, zpracuje odpověď z Redis včas, i když je Redis velmi rychle odeslat odpověď.

#### <a name="measurement"></a>Měření

Využití procesoru široké systém prostřednictvím portálu Azure nebo přidružené čítače sledujte. Dávejte pozor, abyste sledovat procesoru *obrázku* vzhledem k tomu jednoho obrázku můžete nízké využití procesoru ve stejnou dobu, kterou lze celého systému procesoru vysoká. Sledujte vrcholy pole v využití procesoru, které odpovídají časové limity. Důsledku vysoká procesoru, můžete taky vidět Vysoká `in: XXX` hodnoty v `TimeoutException` chybovými zprávami podle popisu v části [požadavků přenosy](#burst-of-traffic) .

>[AZURE.NOTE] StackExchange.Redis 1.1.603, včetně později `local-cpu` metrických v `TimeoutException` chybové zprávy. Zajištění používáte nejnovější verzi [StackExchange.Redis NuGet balíčku](https://www.nuget.org/packages/StackExchange.Redis/). Existuje chyby neustále pevně v kód, který nechcete, aby robustnější na časové limity nejnovější verze je důležité.

#### <a name="resolution"></a>Rozlišení

Upgrade na větší velikost OM s více kapacity CPU nebo prošetřit příčinu vzroste využití procesoru. 



### <a name="client-side-bandwidth-exceeded"></a>Překročení šířky pásma straně klienta

#### <a name="problem"></a>Problém

Jiné velké klientské počítače jste omezení na kolik šířka pásma máte k dispozici. Pokud klienta překročí dostupné šířky pásma, pak dat nebudou zpracovány na straně klienta rychle ho odesílá na server. Může dojít k časové limity.

#### <a name="measurement"></a>Měření

Sledujte, jak změnit využití šířky pásma v čase pomocí kódu [je následující](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Všimněte si, že tento kód nemusí úspěšně spustit v některých prostředích s omezenými oprávněními (třeba weby Azure).

#### <a name="resolution"></a>Rozlišení 

Zvětšení velikosti OM klienta nebo zmenšete šířku pásma.


### <a name="large-requestresponse-size"></a>Velikost velké žádostí a odpovědí

#### <a name="problem"></a>Problém

Velké žádostí a odpovědí způsobí vypršení časového limitu. Předpokládejme například, svůj časový limit klientem hodnotu 1 druhé. Vaše aplikace požaduje dva klíče (například "A" a "B") ve stejnou dobu (pomocí stejného fyzické síťového připojení). Většina klientů podporují "Pipelining" požadavků, tak, aby se obě "A" a "B" se odešle žádost o na drátěný na server jeden po druhém bez nutnosti čekání odpovědi. Server odešle odpovědi zpět ve stejném pořadí. Odpověď "A" je velký dostatečně ho vyžadovat značné množství většina časový limit pro následující požadavky. 

Následující příklad ukazuje tento scénář. V tomto scénáři žádost o "A" a "B" se odesílají rychlé, serveru spustí rychle odesílat odpovědi "A" a "B", ale kvůli doba přenosu dat, "B" mrtvém bodě za jiné žádosti a časový limit i v případě, že server odpověděl rychle.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Měření

To je obtížná změřit. V podstatě musíte nástroje kód klienta ke sledování velkého žádosti o schůzku a odpovědi. 

#### <a name="resolution"></a>Rozlišení

1.  Redis je optimalizována pro velký počet malé hodnot, namísto několik velké. Preferovaný řešením je rozdělit dat do aplikace související nižšími hodnotami. Co je rozsah hodnot ideální velikost redis zobrazit [? Je 100KB moc velký?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) příspěvek podrobnosti kolem proč doporučují se nižšími hodnotami.
2.  Zvětšení velikosti OM (pro klienta a serveru mezipaměť Redis), chcete-li získat vyšší možnosti šířky pásma snížit čas přenos dat pro větší odpovědi. Nezapomeňte, že začíná větší šířku pásma v jenom server nebo jenom v klientovi nemusí být dostatečně. Změřte využití šířky pásma a porovnat s funkcí velikost OM Teď máte.
3.  Zvýšení počtu `ConnectionMultiplexer` objekty můžete kruhového požadavky a použití různých připojení.


### <a name="what-happened-to-my-data-in-redis"></a>Co se stalo s mými daty v Redis

#### <a name="problem"></a>Problém

Mám očekávat pro některé dat je v mé instanci Azure Redis mezipaměti, ale neměli zdá být k dispozici.

##### <a name="resolution"></a>Rozlišení

V tématu [Co se stalo s mými daty v Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) možné příčiny a řešení.


## <a name="server-side-troubleshooting"></a>Řešení potíží straně serveru

Tato část popisuje Poradce při potížích probíhajících z důvodu podmínky na serveru mezipaměti.

-   [Paměť tlaku na serveru](#memory-pressure-on-the-server)
-   [Vysoké využití procesoru / serveru načíst](#high-cpu-usage-server-load)
-   [Překročení šířky pásma straně serveru](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Paměť tlaku na serveru

#### <a name="problem"></a>Problém

Paměť tlaku na straně serveru vede k všechny typy problémy s výkonem, které lze zpozdit zpracování žádostí o. Po narazí paměti tlaku systému obvykle používá k datům stránky z fyzické paměti virtuální paměť, která je na disku. Tato *stránka chybující* způsobí, že systém výrazně zpomalit. Existuje několik možných příčin tohoto tlaku paměti: 

1.  Vyplnění mezipaměti plnou kapacitu s daty. 
2.  Redis vidí vysoké paměti fragmentace - nejčastěji způsobená ukládání velké objekty (Redis je optimalizována pro malé objekty - najdete v článku [Co je rozsah hodnot ideální velikost redis? Je 100KB moc velký?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) příspěvek podrobnosti). 

#### <a name="measurement"></a>Měření

Redis poskytuje dva metriky, které vám může pomoct zjistit tento problém. První z nich je `used_memory` a druhý je `used_memory_rss`. [Tyto metriky](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) jsou dostupné v portálu Azure nebo pomocí příkazu [Redis informace](http://redis.io/commands/info) .

#### <a name="resolution"></a>Rozlišení

Existuje několik možných změny provedené pomáhají zajistit spotřebu paměti pořádku:

1. [Konfigurace zásad paměti](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) a nastavit dobu platnosti pro klíče. Všimněte si, že to nemusí být dostatečné, pokud máte fragmentace.
2. [Konfigurace hodnotu vyhrazená maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , který je dostatečně velký k tomu pro fragmentace paměti.
3. Rozdělte velké režim cached objekty do menší související objekty.
4. [Měřítko](cache-how-to-scale.md) větší velikost mezipaměti.
5. Pokud používáte [premium mezipaměti s podporou clusterů Redis](cache-how-to-premium-clustering.md) můžete [zvýšit počet shards](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Vysoké využití procesoru / serveru načíst

#### <a name="problem"></a>Problém

Vysoké využití procesoru může znamenat, že na straně klienta může selhat zpracuje odpověď z Redis včas, i když je Redis velmi rychle odeslat odpověď.

#### <a name="measurement"></a>Měření

Využití procesoru široké systém prostřednictvím portálu Azure nebo přidružené čítače sledujte. Dávejte pozor, abyste sledovat procesoru *obrázku* vzhledem k tomu jednoho obrázku můžete nízké využití procesoru ve stejnou dobu, kterou lze celého systému procesoru vysoká. Sledujte vrcholy pole v využití procesoru, které odpovídají časové limity.

#### <a name="resolution"></a>Rozlišení

[Měřítko](cache-how-to-scale.md) větší mezipaměť úroveň s více kapacity CPU nebo prošetřit příčinu vzroste využití procesoru. 

### <a name="server-side-bandwidth-exceeded"></a>Překročení šířky pásma straně serveru

#### <a name="problem"></a>Problém

Různé velikosti mezipaměti instance mají omezení na kolik šířka pásma máte k dispozici. Pokud server překročí dostupné šířky pásma, pak dat se neodesílají klientovi tak rychle. Může dojít k časové limity.

#### <a name="measurement"></a>Měření

Můžete sledovat `Cache Read` míru, což je množství dat přečíst z mezipaměti v megabajtech (MB/ne) sekundu během určeného časového intervalu vytváření sestav. Tato hodnota odpovídá šířka pásma používané mezipaměti. Pokud chcete nastavit upozornění u omezení šířky pásma sítě straně serveru, můžete vytvořit pomocí to `Cache Read` čítač. Porovnejte čtení s hodnotami v [této tabulce](cache-faq.md#cache-performance) pro omezení pozorovanými šířky pásma pro různé mezipaměť ceny úrovní a velikosti.

#### <a name="resolution"></a>Rozlišení

Pokud jste konzistentní poblíž pozorovanými maximální šířky pásma pro ceny velikost osy a mezipaměti, zvažte [měřítka](cache-how-to-scale.md) ceny osy nebo velikost, která má větší šířku pásma sítě, použitím hodnot v [této tabulce](cache-faq.md#cache-performance) jako vodítko.


## <a name="stackexchangeredis-timeout-exceptions"></a>Výjimky StackExchange.Redis vypršení časového limitu

StackExchange.Redis používá konfiguraci nastavení pojmenované `synctimeout` pro synchronní operace, které má výchozí hodnotu 1 000 ms. Pokud synchronní volání není dokončit odletu času, vyvolá klientovi StackExchange.Redis chyby vypršení časového limitu podobně jako v následujícím příkladu.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Tato chybová zpráva obsahuje metriky, které vám můžou pomoct přejděte příčiny a možná řešení problému. Následující tabulka obsahuje podrobnosti o metriky chybová zpráva.

| Zpráva míru chyb | Podrobnosti                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| INST       | V posledním časový úsek: 0 příkazy byl vydán                                                                                                                                                                                              |
| Správce        | Provádění správce socket `socket.select` což znamená, že je s dotazem, OS označíte socket, který má něco udělat; v podstatě: čtenáře nečte aktivně ze sítě protože nemá představit je něco, co chcete udělat |
| fronty      | Existuje 73 celkové v průběhu operace                                                                                                                                                                                                        |
| qu         | 6 v průběhu operace jsou ve frontě neodeslaných a nebyly dosud vloženy do odchozí sítě                                                                                                                                                           |
| QS         | 67 he v průběhu operace odeslaných na server ale odpověď ještě není k dispozici. Může být odpověď `Not yet sent by the server` nebo`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 v průběhu operace zobrazila odpovědi, ale nebyly dosud byl dokument označen jako úplný kvůli čekání na dokončení opakovat                                                                                                                                      |
| WR         | Existuje bajtů/activewriters aktivní zápis (což znamená, že nejsou ignorovány 6 neodeslaných požadavky)                                                                                                                                                   |
| v         | Existuje žádné aktivní čteček a nula bajtů jsou k dispozici Čtěte dál NIC bajty/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Postup zkoumání

1. Jako doporučený postup Ujistěte se, jsou připojení při použití klienta StackExchange.Redis pomocí následujícího vzorce.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Další informace najdete v tématu [připojení do mezipaměti pomocí StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Zajištění mezipaměť Redis Azure a klientské aplikace ve stejné oblasti v Azure. Například se může být získání časové limity při mezipaměť je východoasijských USA, ale je klient západní USA a žádost není dokončena v rámci `synctimeout` intervalu nebo může být začíná časové limity při ladění z počítače místní rozvoj. 

    Důrazně doporučujeme mít mezipaměti a v klientovi ve stejné oblasti Azure. Pokud máte situace obsahující hovory mezi oblast, je třeba nastavit `synctimeout` interval na hodnotu vyšší než výchozí interval 1000 ms s využitím `synctimeout` vlastnost v připojovacím řetězci. V následujícím příkladu je ukázka StackExchange.Redis mezipaměti připojení řetězec s `synctimeout` z 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Zajištění používáte nejnovější verzi [StackExchange.Redis NuGet balíčku](https://www.nuget.org/packages/StackExchange.Redis/). Existuje chyby neustále pevně v kód, který nechcete, aby robustnější na časové limity nejnovější verze je důležité.

4. Pokud jsou požadavky na získání svázané tak, že omezení šířky pásma serveru nebo klienta, bude trvat déle jejich provedení a následné způsobí vypršení časového limitu. Pokud svůj časový limit je kvůli šířka pásma na serveru najdete v tématu [překročení šířky pásma straně serveru](#server-side-bandwidth-exceeded). Pokud svůj časový limit je kvůli klienta šířka pásma najdete v tématu [překročení šířky pásma pro klienta straně](#client-side-bandwidth-exceeded).

6. Jsou se zobrazuje procesoru ovládací prvek vázaný na serveru nebo v klientském počítači?
    -   Zaškrtněte, pokud jste musejí začíná zachovávat procesoru na svému klientovi, což může způsobit žádost zpracována v `synctimeout` interval, což způsobuje časový limit. Přesunutí do větší velikost klienta nebo distribuce načíst vám může k tomu. 
    -   Zkontrolujte, zda se zobrazují procesoru vázaný na serveru sledováním `CPU` [míru výkonu mezipaměti](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Žádosti o chodit při Redis procesoru vázaná mohou způsobit tyto požadavky na časový limit. Při řešení to můžete rozdělit zatížení mezi více shards v mezipaměti premium nebo upgradovat na větší nebo ceny vrstvě. Další informace najdete v tématu [Překročení šířky pásma straně serveru](#server-side-bandwidth-exceeded).

7. Jsou příkazy trvá dlouho zpracování na serveru? Dlouhodobě spuštěné příkazy, jejichž dokončení trvá dlouho serveru redis způsobí vypršení časového limitu. Pár příkladů dlouho spuštěné příkazy jsou `mget` s velkým počtem klávesy `keys *` nebo špatně napsané lua skriptů. Můžete připojit k instanci aplikace mezipaměti Redis Azure pomocí klienta redis rozhraní příkazového řádku nebo použití [Redis konzoly](cache-configure.md#redis-console) a spusťte příkaz [SlowLog](http://redis.io/commands/slowlog) zjistit, zda jsou požadavky trvá déle než obvykle. Redis Server a StackExchange.Redis jsou optimalizovaná pro mnoho malé požadavků spíše než méně velké požadavky. Rozdělení data do menší bloků může zajistit zlepšení co tady. 

    Informace o připojení na SSL mezipaměti Redis Azure koncový bod pomocí rozhraní redis příkazového řádku a stunnel najdete v tématu příspěvek blogu [Oznamující ASP.NET relace poskytovatele stavu Redis verzi Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Další informace najdete v tématu [SlowLog](http://redis.io/commands/slowlog).

8. Velkém zatížení serveru Redis způsobí vypršení časového limitu. Můžete sledovat serveru načíst sledováním `Redis Server Load` [míru výkonu mezipaměti](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Zatížení serveru 100 (maximální hodnota) označuje, že redis server byl zaneprázdněn, bez doby nečinnosti zpracování žádostí o. Pokud některé požadavky zabírají všechny funkce serveru zobrazíte spuštění příkazu SlowLog podle popisu v předchozí odstavec. Další informace najdete v tématu [vysoké procesoru při využití / serveru načíst](#high-cpu-usage-server-load).

9. Byl na straně klienta, který může být příčinou blip sítě jiné události? Zkontrolovat na straně klienta (web, role pracovních nebo Iaas OM) došlo zvláštní události jako měřítka počet instancí klienta nahoru nebo dolů, zda nasazení novou verzi klienta nebo Automatické měřítko aktivované? V naší testování, které jsme našli, může způsobit automatické měřítko nebo změna velikosti up/Page down odchozí síťové připojení můžete ztratit několik sekund. StackExchange.Redis kód je pružné pro tyto události a obnovení. Během této doby opětovné připojení můžete všechny žádosti o ve frontě časový limit.

10. Byl tam velké žádosti před několika malé požadavky mezipaměti Redis vypršel časový limit? Parametr `qs` v článku chyba se zpráva, kolik požadavků byly odeslány z klienta serveru, ale ještě zpracovány odpověď. Tato hodnota můžete zachovat Kvetoucí, protože StackExchange.Redis používá jediný připojení TCP a může číst jenom jedna odpověď po druhém. I když je první operace vypršení časového limitu, jej neukončí data odesílaného ze serveru a ostatní požadavky jsou blokovány až do dokončení, které jsou příčinou časové limity. Řešením je omezit možnost časové limity zajistit, aby mezipaměť dostatečně velký k tomu u svého pracovního vytížení a rozdělení velké hodnoty do menší bloky. Jiné řešení, je použít sadu `ConnectionMultiplexer` objekty ve vašem klientovi a zvolte alespoň načtený `ConnectionMultiplexer` při odesílání novou žádost o. To měli zabránit, aby jeden vypršení časového limitu příčinou ostatní požadavky na taky časový limit.

11. Pokud používáte `RedisSessionStateprovider`, zajistit, aby nastavili časový limit opakovat správně. `retrytimeoutInMilliseconds`by měla být vyšší než `operationTimeoutinMilliseonds`, jinak napoprvé dojde. V následujícím příkladu `retrytimeoutInMilliseconds` je nastavený na 3000. Další informace najdete v tématu [Poskytovatele stavu relace ASP.NET pro Azure Redis mezipaměť](cache-aspnet-session-state-provider.md) a [jak používat parametry konfigurace poskytovatele stavu relace a poskytovatele výstupní mezipaměti](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Kontrola spotřebu paměti na server Azure Redis mezipaměti [sledováním](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` a `Used Memory`. Pokud zásadu vyřazení na místě, spustí Redis jeho vyloučení klávesy se šipkami kdy `Used_Memory` dosáhne velikost mezipaměti. V ideálním případě `Used Memory RSS` by měla být jenom mírně vyšší než `Used memory`. Velké rozdíl znamená, že fragmentace paměti (interní a externí. Když `Used Memory RSS` je menší než `Used Memory`, znamená to, že část mezipaměti byla vyměnit operační systém. V takovém případě můžete očekávat, že některé významné čekacích dob. Protože Redis nemá publikum nemůže ovládat způsob jeho přiřazení jsou namapované stránek paměti, vysoké `Used Memory RSS` je často výsledek zásobníku využití paměti. Po Redis uvolnění paměti, paměť se dostali zpátky přidělování a přidělování může nebo nemusí vrátit paměť systému. Doména může obsahovat rozdíl mezi `Used Memory` hodnota a paměti spotřebu vykázání operačního systému. Může to být z toho důvodu, byl použit paměti a vydané Redis, ale ne vzhledem k tomu zpět k systému. Zmírnit problémům s pamětí provedením následujících kroků.
    -   Upgrade mezipaměti pro větší velikost tak, aby se nepoužívá s desktopovým omezení paměti v systému.
    -   Nastavení doby vypršení platnosti klíčů tak, aby včasným vyloučení starší hodnoty.
    -   Sledování na `used_memory_rss` mezipaměti míru. Když tuto hodnotu blíží velikosti mezipaměti, budete pravděpodobně zahájení zobrazují problémy s výkonem. Distribuce data napříč několika shards, pokud používáte premium mezipaměti, nebo upgrade na větší velikost mezipaměti.

    Další informace najdete v tématu [Paměti tlaku na serveru](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Další informace

-   [K čemu nabízení Redis mezipaměti a velikost mám použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Jak lze testu výkonnosti a otestujte výkon Moje mezipaměti?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Spouštění Redis příkazy](cache-faq.md#how-can-i-run-redis-commands)
-   [Jak sledovat Azure Redis mezipaměti](cache-how-to-monitor.md)