<properties
   pageTitle="Režim analýzy selhání | Microsoft Azure"
   description="Pokyny k provádění analýzy režimu selhání cloudové řešení založené na Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Pokyny pro Azure odolnost proti chybám: analýzy selhání režimu

Režim analýzy selhání (FMA) je proces pro vytváření odolnost proti chybám do systému, tak, že identifikují míst možné selhání systému. FMA by měly být součástí fází architektura a návrhů, mohli vytvářet zotavení do systému od začátku.

Tady je obecný postup vést FMA: 

1. Určení všechny prvky systému. Zahrnout externí závislostí, jako je třeba Zprostředkovatelé identit jiní, služby třetích stran a tak dál.   

2. Pro jednotlivé součásti popsána potenciální chyby, ke kterým může dojít. Jednotlivé součásti může mít víc než jeden selhání režim. Například měli byste zvážit čtení selhání a zapsat selhání samostatně, dopad a možná bude jiné.

3. Ohodnocení jednotlivých režimech selhání podle celkového rizika. Uvažte následující faktory:  

    - Co je pravděpodobnost chyby. Je poměrně běžné? Extrememly méně častých? Nemusíte přesné čísel. slouží k uspořádání prioritu.

    - Co je dopad na aplikaci z hlediska dostupnost, ztrátou dat, peněžní náklady a narušením business? 

4. Pro každý režim poruchy zjistit, jak odpoví a obnovení aplikace. Zvažte poměry složitostí nákladů a aplikace.   

Jako výchozí bod pro FMA procesu Tento článek obsahuje katalog potenciální režimy selhání a jejich mitigations. Katalog uspořádaný podle technologii nebo služby Azure plus kategorii obecné návrhu úrovni aplikace. Katalog není vyčerpávající, ale obsahuje řadu základních Azure služby. 


## <a name="app-service"></a>Aplikace služby

### <a name="app-service-app-shuts-down"></a>Vypnutí aplikace aplikaci služby.

**Zjišťování**. Možných příčin:

- Očekávané vypnutí
    
    - Operátor vypne aplikace. například na portálu Azure.

    - Aplikaci byl uvolnit, protože byl nečinný. (Pouze v případě `Always On` je zakázáno.)

- Neočekávané vypnutí

    - Způsobí zhroucení aplikace.

    - Instanci aplikace služby OM nebude k dispozici.


Protokolování Application_End zachytí domény vypnutí aplikace (měkké proces dojde k chybě) a je jediný způsob, jak zachytit domény vypnutí aplikace.

**Obnovení**

- Pokud by měla být vypnutí, použijte událost vypnutí aplikace neukončí. Například v ASP.NET, použít `Application_End` metody.

- Pokud je aplikace uvolnění během nečinnosti, se automaticky nerestartuje dalším požadavku. Však je možné za "studený start" nákladů. 

- Chcete-li zabránit aplikaci vyložení během nečinnosti, povolit `Always On` nastavení ve web appu. V tématu [Konfigurace web apps v aplikaci služby Azure][app-service-configure].

- Pokud chcete zakázat vypnutím aplikace operátor, nastavte zámek zdroje s `ReadOnly` úroveň. [Uzamčení]tématech pomocí Správce prostředků Azure[rm-locks].

- Pokud dojde k chybě aplikace nebo an, chyby OM služby aplikace nebude k dispozici, aplikaci služby automaticky restartování aplikace. 


**Diagnostika**. Aplikace protokoly a webový server. V tématu [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Určitému uživateli opakovaně vykonává chybná žádosti nebo přetížení systému. 

**Zjišťování**. Ověřování uživatelů a zahrnout ID uživatele protokoly aplikace.

**Obnovení**

- Použití [Správy rozhraní API Azure] [ api-management] do žádosti o omezení od uživatele. Zobrazit [Upřesňující žádost o omezení službou Azure rozhraní API Management][api-management-throttling]

- Blokovat uživatele.

**Diagnostika**. Požadavky na všechny ověřování.


### <a name="a-bad-update-was-deployed"></a>Chybné aktualizace nasazené.

**Zjišťování**. Monitorování stavu aplikace portálu Azure (viz [Sledování Azure webové aplikace výkon][app-insights-web-apps]) nebo provedení [Sledování vzorek stavu koncový bod][health-endpoint-monitoring-pattern].

**Obnovení**. Použití více [nasazení sloty] [ app-service-slots] a návrat k poslední známý nasazení. Další informace najdete v tématu [základní webové aplikace odkaz architektura][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>Ověření OpenID připojení (OIDC) se nezdaří.

**Zjišťování**. Možných způsobů poruch patří:

1. Azure AD není k dispozici nebo není dostupný kvůli potížím se sítí. Přesměrování koncový bod ověření úspěšné a OIDC middleware výjimku.
2. Azure AD klienta neexistuje. Přesměrování koncový bod ověřování vrátí chybu HTTP a OIDC middleware výjimku.
3. Uživatel nemůže ověřit. Žádné strategii zjišťování je nutné; Azure AD úchyty pro přihlášení k chybám.

**Obnovení**

1. Zachycení neošetřené výjimky z middleware.
2. Zpracování `AuthenticationFailed` události.
3. Přesměrujte uživatele na stránku s chybou.
4. Uživatel opakování.


## <a name="azure-search"></a>Azure hledání

### <a name="writing-data-to-azure-search-fails"></a>Zápis dat do hledání Azure se nezdaří.

**Zjišťování**. Zachycení `Microsoft.Rest.Azure.CloudException` chyby.

**Obnovení**

[Hledání .NET SDK] [ search-sdk] ztracená po přechodná k chybám. Požadované výjimky vyvolané klienta SDK zacházet jako chybné nepřechodná.

Výchozí zásady opakovat používá exponenciální back vypnout. Použití různých opakovat zásad, zavolejte `SetRetryPolicy` na `SearchIndexClient` nebo `SearchServiceClient` předmětu. Další informace najdete v tématu [Automatické opakování][auto-rest-client-retry].

**Diagnostika**. Použití [analýzy přenosy hledání][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Čtení dat z Azure vyhledávání se nezdaří.

**Zjišťování**. Zachycení `Microsoft.Rest.Azure.CloudException` chyby.

**Obnovení**

[Hledání .NET SDK] [ search-sdk] ztracená po přechodná k chybám. Požadované výjimky vyvolané klienta SDK zacházet jako chybné nepřechodná.

Výchozí zásady opakovat používá exponenciální back vypnout. Použití různých opakovat zásad, zavolejte `SetRetryPolicy` na `SearchIndexClient` nebo `SearchServiceClient` předmětu. Další informace najdete v tématu [Automatické opakování][auto-rest-client-retry].

**Diagnostika**. Použití [analýzy přenosy hledání][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Čtení nebo psaní na uzel se nezdaří.

**Zjišťování**. Zachycení výjimky. Pro .NET klienty, obvykle se bude `System.Web.HttpException`. Jiný klient možná bude potřeba dalších typů výjimku.  Další informace najdete v tématu [zpracování chyb Cassandra Hotovo doprava](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Obnovení**

- Každý [Klient Cassandra](https://wiki.apache.org/cassandra/ClientOptions) obsahuje vlastní zásady opakování a možnosti. Další informace najdete v tématu [zpracování chyb Cassandra správně prováděný][cassandra-error-handling].
- Pomocí datových uzlů rozvržena domény poruch podporující regálů nasazení.
- Nasazení více oblastí s místní kvorum konzistence. Pokud dojde k chybě nepřechodná, převzetí do jiné oblasti.

**Diagnostika**. Protokoly aplikace


## <a name="cloud-service"></a>Cloudová služba

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Role webu nebo pracovního se neočekávaně vypíná.

**Zjišťování**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] vyvolání události.

**Obnovení**. Přepsat [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] metoda řádně vyčištění. Další informace najdete v tématu [Vpravo způsob zpracování události OnStop Azure] [ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Čtení dat z DocumentDB se nezdaří.

**Zjišťování**. Zachycení `System.Net.Http.HttpRequestException` nebo `Microsoft.Azure.Documents.DocumentClientException`. 

**Obnovení**

- V SDK ztracená neúspěšné pokusy. Pokud chcete nastavit počet opakování a maximální prodleva nakonfigurovat `ConnectionPolicy.RetryOptions`. Výjimky vyvolá klienta, jsou za opakovat zásad nebo nejsou přechodná chyby. 

- Pokud DocumentDB omezení klienta, vrátí chybu HTTP 429. Zkontrolujte stav kód `DocumentClientException`. Pokud se zobrazují chyby 429 konzistentní, zvažte zvýšíte výkon hodnotu kolekci DocumentDB.

- Replikace databáze DocumentDB dvě nebo více oblastí. Ostatními jsou čitelný. Pomocí klienta SDK, zadejte `PreferredLocations` parametr. Toto je seznam pořadí Azure oblastí. Všechny operace čtení odešle první oblasti k dispozici v seznamu. Pokud žádost nezdaří, klient se bude snažit jiné oblasti v seznamu polí v pořadí. Další informace najdete v tématu [rozvojových s více oblastí DocumentDB účty][docdb-multi-region].

**Diagnostika**. Přihlaste se všechny chyby na straně klienta.


### <a name="writing-data-to-documentdb-fails"></a>Zápis dat do DocumentDB dojde k chybě.

**Zjišťování**. Zachycení `System.Net.Http.HttpRequestException` nebo `Microsoft.Azure.Documents.DocumentClientException`. 

**Obnovení**

- V SDK ztracená neúspěšné pokusy. Pokud chcete nastavit počet opakování a maximální prodleva nakonfigurovat `ConnectionPolicy.RetryOptions`. Výjimky vyvolá klienta, jsou za opakovat zásad nebo nejsou přechodná chyby. 

- Pokud DocumentDB omezení klienta, vrátí chybu HTTP 429. Zkontrolujte stav kód `DocumentClientException`. Pokud se zobrazují chyby 429 konzistentní, zvažte zvýšíte výkon hodnotu kolekci DocumentDB.

- Replikace databáze DocumentDB dvě nebo více oblastí. Když oblasti primární nepovede, jiné oblasti jeho úroveň se zvýší psát. Přepojení můžete spustit taky ručně. SDK nepodporuje automatické zjišťování a hodnocení směrování, takže kód aplikace bude dál fungovat i po přepojení. Období převzetí (obvykle minut) budou zápisu mít vyšší latence jako SDK najde novou oblast zápisu. Další informace najdete v tématu [rozvojových s více oblastí DocumentDB účty][docdb-multi-region].

- Jako základní formulář přetrvávají dokument, který chcete záložní fronty a zpracování fronty později.

**Diagnostika**. Přihlaste se všechny chyby na straně klienta.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Čtení dat z Elasticsearch se nezdaří.

**Zjišťování**. Zachycení odpovídající výjimky pro konkrétní [Elasticsearch klienta] [ elasticsearch-client] použitý. 

**Obnovení**

- Použití mechanismus opakovat. Každý klient obsahuje vlastní zásady opakovat. 

- Nasazení více uzlů Elasticsearch a použití replikace dostupnost.

Další informace najdete v tématu [Spuštění Elasticsearch na Azure][elasticsearch-azure].

**Diagnostika**. Použití nástroje pro sledování pro Elasticsearch nebo protokolovat všechny chyby na straně klienta se datové. Naleznete v části "Sledování" v [Spuštěný Elasticsearch na Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Zápis dat do Elasticsearch dojde k chybě.

**Zjišťování**. Zachycení odpovídající výjimky pro konkrétní [Elasticsearch klienta] [ elasticsearch-client] použitý.  

**Obnovení**

- Použití mechanismus opakovat. Každý klient obsahuje vlastní zásady opakovat. 
 
- Pokud aplikace nevadí vám úrovně omezená konzistence, zvažte psaní s `write_consistency` nastavení `quorum`.

Další informace najdete v tématu [Spuštění Elasticsearch na Azure][elasticsearch-azure].

**Diagnostika**. Použití nástroje pro sledování pro Elasticsearch nebo protokolovat všechny chyby na straně klienta se datové. Naleznete v části "Sledování" v [Spuštěný Elasticsearch na Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Úložiště fronty

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Napsání zprávy do fronty Azure úložiště dojde k chybě konzistentní.

**Zjišťování**. Po *N* novými pokusy o aktualizaci, se zápis stále nezdaří.

**Obnovení**

- Uložení dat v místní mezipaměti a předávání zápisy k základnímu úložišti později, až bude službu dostupný.
- Vytvoření sekundární fronty a zapisovat do fronty Pokud primární fronty není k dispozici.

**Diagnostika**. Použití [metriky úložišť][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikace se nedají zpracovat určitou zprávu ve frontě. 

**Zjišťování**. Specifické pro aplikaci. Například zpráva obsahuje neplatná data nebo obchodní logiky neúspěšné z nějakého důvodu. 

**Obnovení**

Přesunutí zprávy do samostatných fronty. Spusťte samostatný proces zkoumat zprávy ve frontě.

Zvažte použití zasílání zpráv Bus služby Azure fronty, která poskytuje [doručena] [ sb-dead-letter-queue] funkcí k tomuto účelu.

Poznámka: Pokud používáte úložiště fronty se WebJobs, WebJobs SDK poskytuje předdefinované poškozená zpráva zpracování. Informace [o použití úložiště Azure fronty s WebJobs SDK][sb-poison-message].

**Diagnostika**. Použití aplikace protokolování.


## <a name="redis-cache"></a>Redis mezipaměti

### <a name="reading-from-the-cache-fails"></a>Čtení z mezipaměti se nezdaří.

**Zjišťování**. Zachycení `StackExchange.Redis.RedisConnectionException`.

**Obnovení**

1. Opakování na přechodná k chybám. Azure mezipaměti Redis podporuje předdefinované opakovat až najdete v článku [pokyny opakovat Redis mezipaměti][redis-retry].
2. Nepřechodná k chybám považovat za paní mezipaměti a přejdou na původní zdroj dat.

**Diagnostika**. Použití [Redis mezipaměti diagnostiky][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Psaní do mezipaměti se nezdaří.

**Zjišťování**. Zachycení `StackExchange.Redis.RedisConnectionException`.

**Obnovení**

1. Opakování na přechodná k chybám. Azure mezipaměti Redis podporuje předdefinované opakovat až najdete v článku [pokyny opakovat Redis mezipaměti][redis-retry].
2. Pokud je chyba nepřechodná, ignorujte ji a nechat ostatní transakce zapisovat do mezipaměti později.

**Diagnostika**. Použití [Redis mezipaměti diagnostiky][redis-monitor].


## <a name="sql-database"></a>Databáze SQL

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Nemůže připojit k databázi v oblasti primární.

**Zjišťování**. Připojení se nezdaří.

**Obnovení**

Předpoklady: Databáze musí být nakonfigurované pro aktivní geo replikace. V tématu [SQL databáze aktivní Geo replikace][sql-db-replication].

- Dotazy přečtěte si z sekundární otevřené. 
- Vloží a aktualizace ručně převzít sekundární otevřené. V tématu [Zahájit plánované nebo neplánované selhání databáze SQL Azure][sql-db-failover].

Otevřené používá jiný připojovací řetězec, takže byste potřebovali aktualizovat připojovací řetězec v aplikaci.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Klient spustí mimo připojení ve fondu připojení.

**Zjišťování**. Zachycení `System.InvalidOperationException` chyby. 

**Obnovení**

- Opakujte.
- Jako plán na omezení rizik izolujte fondy připojení pro každý případu použití tak, aby jeden případu použití nelze dominujte všechna připojení.
- Zvětšení sdružení maximální připojení.

**Diagnostika**. Aplikace se přihlásí.


### <a name="database-connection-limit-is-reached"></a>Připojení databáze je limitu. 

**Zjišťování**. Databáze SQL Azure omezuje počet souběžné pracovníků, přihlášení a relace. Limity závisí na vrstvy služeb. Další informace najdete v tématu [limity prostředků databáze SQL Azure][sql-db-limits].

Zjistit tyto chyby zachytit `System.Data.SqlClient.SqlException` a zkontrolujte hodnotu `SqlException.Number` kódy chyb SQL. Seznam kódů relevantní chyb najdete v tématu [kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů][sql-db-errors].

**Obnovení**. Tyto chyby jsou považovány za přechodná, takže opakování může problém vyřešit. Pokud konzistentní přístupů tyto chyby, zvažte měřítka databáze.

**Diagnostika**. – [Sys.event_log] [ sys.event_log] dotaz vrací připojení k databázi úspěšné, chyby připojení a zablokování.

- Vytvoření [pravidla výstrahy] [ azure-alerts] pro se nezdařilo připojení.

- Povolení [auditování databáze SQL] [ sql-db-audit] a vyhledat selhalo přihlášení.


## <a name="service-bus-messaging"></a>Služby Bus zasílání zpráv

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Čtení zprávy z fronty služby Bus se nezdaří.

**Zjišťování**. Zachycení výjimky z klienta SDK. Základní třídy pro službu Bus výjimky je [MessagingException][sb-messagingexception-class]. Pokud je chyba přechodná, `IsTransient` vlastnost na hodnotu true. 

Další informace najdete v tématu [služby Bus zpráv výjimky][sb-messaging-exceptions].

**Obnovení**

1. Opakování na přechodná k chybám. V tématu [Služby Bus opakovat pokyny][sb-retry].

2. Zprávy, které nelze doručit všechny příjemce jsou umístěny v *doručena*. Pomocí této fronty najdete v článku zprávy, které se nepodařilo přijmout. Neexistuje žádný automatické vyčištění fronty doručena. Zprávy zůstávají, dokud je explicitně načíst. V tématu [Přehled služby Bus doručena fronty][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Napsání zprávy služby Bus fronty nezdaří.

**Zjišťování**. Zachycení výjimky z klienta SDK. Základní třídy pro službu Bus výjimky je [MessagingException][sb-messagingexception-class]. Pokud je chyba přechodná, `IsTransient` vlastnost na hodnotu true. 

Další informace najdete v tématu [služby Bus zpráv výjimky][sb-messaging-exceptions].

**Obnovení**

1. Klienta služby Bus ztracená po přechodná chyby. Ve výchozím nastavení používá exponenciální back vypnout. Po maximální počet opakování nebo maximální časový limit klienta výjimku. Další informace najdete v tématu [Služby Bus opakovat pokyny][sb-retry].

2. Pokud překročení kvóty fronty klienta vyvolá [QuotaExceededException][QuotaExceededException]. Zpráva o výjimce vám bude radit další podrobnosti. Vyprázdnit některé zprávy ve frontě akci a zvažte možnost vyhnout pokračování opakování při překročení kvóty vzorek okruh dělení. Nezapomeňte, že vlastnost [BrokeredMessage.TimeToLive] není nastavený příliš velký. 

3. V oblasti, odolnost proti chybám můžete vylepšit pomocí [oddílů fronty nebo témata][sb-partition]. Bez oddílů fronty nebo tématu přiřazen jeden zpráv úložiště. Pokud toto zpráv úložiště není k dispozici, všechny operace na danou fronty nebo téma se nezdaří. Rozdělený fronty nebo tématu rozdělený napříč několika zpráv ukládají. 

4. Další odolnosti vytvořit dvěma obory Bus služby v různých oblastech a replikovat zprávy. Můžete použít aktivní replikace nebo pasivní replikace.

    - Aktivní replikace: klient odešle každé zprávy obou dotazů. Příjemce sleduje na obou dotazů. Označení zprávy s jedinečného identifikátoru, takže klienta můžete zrušit duplicitní zprávy.

    - Trpný replikace: klient odešle zprávu do jednoho fronty. Pokud dojde k chybě, přejde klienta do fronty. Příjemce sleduje na obou dotazů. Tento přístup snižuje počet duplicitní zprávy, které jsou odeslány. Příjemce musí však pořád zpracovat duplicitní zprávy.

    Další informace najdete v tématu [GeoReplication vzorku] [ sb-georeplication-sample] a [osvědčené postupy pro izolační aplikace proti služby Bus výpadků a havárie][sb-outages].


### <a name="duplicate-message"></a>Duplicitní zprávu.

**Zjišťování**. Podívat se `MessageId` a `DeliveryCount` vlastnosti zprávy.

**Obnovení**

- Pokud je to možné návrh vaší zprávě zpracování idempotent. V opačném obsahují zprávy ID zpráv, které jsou již zpracovány a zkontrolujte ID před zpracování zprávy.

- Povolení duplicit, vytvořením fronty s `RequiresDuplicateDetection` nastavena na hodnotu true. Při tomto nastavení služby Bus automaticky odstraní libovolnou zprávu, která je se stejným `MessageId` jako předchozí zprávu.  Poznámka:

    -  Zabrání duplicitní zprávy umístit do fronty. Nezabráníte mu příjemce z více než jednou zpracování stejnou zprávu.

    - Duplicit má časového intervalu. Je-li duplicitní odeslána za toto okno, nebude zjišťování.

**Diagnostika**. Protokolovat duplicitní zprávy.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikace se nedají zpracovat určitou zprávu ve frontě. 

**Zjišťování**. Specifické pro aplikaci. Například zpráva obsahuje neplatná data nebo obchodní logiky neúspěšné z nějakého důvodu. 

**Obnovení**

Existují dva způsoby selhání vzít v úvahu. 

- Příjemce zjistí selhání. V tomto případě přesunete do fronty doručena. Později spusťte samostatný proces zkoumat zprávy ve frontě doručena.

- Příjemce selže uprostřed zpracování zprávy &mdash; například kvůli neošetřené výjimce. Zpracovat tento případ, můžete `PeekLock` režimu. V tomto režimu když skončí platnost zámek, zprávu zpřístupní jiných příjemce. Pokud zpráva překročila počet maximální doručení nebo time-to-live, přesune se automaticky zprávu do fronty doručena.

Další informace najdete v tématu [Přehled služby Bus doručena fronty][sb-dead-letter-queue].

**Diagnostika**. Když aplikace přesune zprávu do fronty doručena, Zapsat událost protokoly aplikace.


## <a name="service-fabric"></a>Služba struktury

### <a name="a-request-to-a-service-fails"></a>Žádost o službu se nezdaří.

**Zjišťování**. Služba vrátí chybu.

**Obnovení**

- Vyhledejte na proxy server znovu (`ServiceProxy` nebo `ActorProxy`) a znovu volat metodu service/actor. 

- **Stavová služba**. Zalomit operace spolehlivé kolekcí v transakci. Pokud dojde k chybě, budou transakce vrátit zpět. Pozvánce na schůzku, pokud doplněné z fronty, budou zpracovány znovu. 
 
- **Příslušnosti služby**. Pokud službu potrvají dat do externích úložiště, všechny operace potřeba idempotent.


**Diagnostika**. Protokol aplikace

### <a name="service-fabric-node-is-shut-down"></a>Služba struktury uzel je vypnutý.

**Zjišťování**. Zrušení token předána služby `RunAsync` metody. Služba struktury zruší úkol před ukončením uzel.

**Obnovení**. Zrušení token použijte ke zjištění vypnutí. Zrušení požádá služby struktury dokončení práci a ukončete `RunAsync` co nejdříve.

**Diagnostika**. Protokoly aplikace


## <a name="storage"></a>Úložiště

### <a name="writing-data-to-azure-storage-fails"></a>Zápis dat do úložiště Azure dojde k chybě

**Zjišťování**. Klient nedostane chyby při psaní.

**Obnovení**

1. Opakujte, k obnovení ze přechodná k chybám. [Opakovat zásad] [ Storage.RetryPolicies] v klientovi SDK zpracovává to automaticky.
2. Implementace vzorek okruh dělení Chcete-li předejít jednoznačná úložiště.
3. Pokud dojde k selhání pokusů N, proveďte bezproblémové nouzového. Příklad:

    - Uložení dat v místní mezipaměti a předávání zápisy k základnímu úložišti později, až bude službu dostupný.
    - Pokud je proces zápisu v transakční obor, vyrovnání transakce.

**Diagnostika**. Použití [metriky úložišť][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Čtení dat z Azure úložiště se nezdaří.

**Zjišťování**. Klient nedostane chyby při čtení.

**Obnovení**

1. Opakujte, k obnovení ze přechodná k chybám. [Opakovat zásad] [ Storage.RetryPolicies] v klientovi SDK zpracovává to automaticky.
2. Vzdálená pomoc GRS úložiště Pokud čtení z primární koncového bodu nepovede, zkuste čtení z sekundární koncového bodu. Klient SDK zpracovat to automaticky. V tématu [úložišti Azure replikace][storage-replication].
3. Pokud je selhání pokusů *N* trvat náhradní neudělat k bez. Pokud obrázek produktu nelze načtená z kontingenčního seznamu úložiště, třeba zobrazte obecné zástupný obrázek.

**Diagnostika**. Použití [metriky úložišť][storage-metrics].


## <a name="virtual-machine"></a>Virtuální počítač

### <a name="connection-to-a-backend-vm-fails"></a>Připojení k back-end OM se nezdaří.

**Zjišťování**. Chyby připojení sítě.

**Obnovení**

- Nasazení aspoň dva back-end VMs sady dostupnost za vyrovnávání zatížení.

- Pokud přechodná připojení chyba se někdy TCP úspěšně zopakuje odeslání zprávy. 

- Implementace zásadu opakovat v aplikaci. 

- Trvalý nebo nepřechodná chyby implementace [okruh dělení] [ circuit-breaker] vzorku.

- Volání OM přesáhne limit výstupní sítě, odchozí fronty zaplní. Pokud odchozí fronta konzistentní plná, zvažte rozšiřování. 

**Diagnostika**. Protokolování událostí v omezení služeb.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>OM instance bude k dispozici nebo chybná.

**Zjišťování**. Konfigurace služby Vyrovnávání zatížení [stavu zkušební] [ lb-probe] , která označuje, zda je funkční OM instance. Zkušební zkontrolujte, jestli jsou správně reagovat kritické funkce. 

**Obnovení**. Pro každé osy aplikace umístění několika instancích spuštěných OM do stejnou sadu dostupnost a dejte Vyrovnávání zatížení před VMs. Když se zkušební stavu nepovede, Vyrovnávání zatížení ukončí odesílání nových připojení chybná instanci.

**Diagnostika**. – Použijte Vyrovnávání zatížení [protokolu analýzy][lb-monitor].
- Konfigurace systému sledování sledovat všechny koncové body sledování stavu.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operátor omylem vypne virtuálního počítače.

**Zjišťování**. NENÍ K DISPOZICI

**Obnovení**. Nastavte zámek zdroje s `ReadOnly` úroveň. [Uzamčení]tématech pomocí Správce prostředků Azure[rm-locks].

**Diagnostika**. Použití [Azure aktivity protokoly][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Nepřetržitý úlohy se zastaví době nečinnosti hostiteli Správce služeb.

**Zjišťování**. Předáte token zrušení funkci WebJob. Další informace najdete v tématu [vypnutí][web-jobs-shutdown]. 

**Obnovení**. Povolit `Always On` nastavení ve web appu. Další informace najdete v tématu [spuštění pozadí úkoly s WebJobs][web-jobs].


## <a name="application-design"></a>Návrh aplikace

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Aplikace nejde zpracovat zásobníku v příchozí žádosti.

**Zjišťování**. Závisí na aplikace. Typické příznaky:

- Na webu spustí vrácení kódy chyb 5xx HTTP.
- Závislé služby, jako je databáze nebo úložiště, začněte omezení žádosti. Vyhledejte HTTP chyby, jako jsou 429 HTTP (příliš mnoho žádosti o), v závislosti služby.
- Délka fronty HTTP roste.

**Obnovení**

- Rozšiřování případě lepší načíst. 

- Zmírnění selhání-li se vyhnout CSS selhání přerušit celou aplikaci. Omezení rizik strategie patří:

    - Implementace [Omezení vzorek] [ throttling-pattern] Chcete-li předejít jednoznačná back-end systémy.
    - Použít [na základě fronty zatížení vyrovnání] [ queue-based-load-leveling] vyrovnávací paměť žádosti a jejich zpracování odpovídající tempem.
    - Určovat priority některých klientech. Pokud má aplikace bezplatných i placených úrovní, omezení například zákazníky při bezplatné osy, ale ne placené zákazníky. [Priority (priorita) fronty]vzorek[priority-queue-pattern].

**Diagnostika**. Použití [aplikace služby diagnostické protokolování][app-service-logging]. Pomocí služby, třeba [Azure protokolu analýzy][azure-log-analytics], [Aplikace přehledy][app-insights], nebo [Nové Relic] [ new-relic] porozumět diagnostické protokoly.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Jedna z operací v pracovním postupu nebo distribuované transakce se nezdaří.

**Zjišťování**. Po *N* novými pokusy o aktualizaci, pořád dojde k chybě.

**Obnovení**

- Jako plán na omezení rizik implementace [Plánovač Agent správce] [ scheduler-agent-supervisor] vzor pro správu celé pracovního postupu. 
- Není opakovat časových limitů. Existuje zhoršeným úspěšnosti pro tuto chybu. 
- Fronta práce, abyste mohli později opakovat.

**Diagnostika**. Přihlaste se všechny operace (úspěšné a neúspěšné), včetně kompenzace akce. Pomocí ID korelace, aby mohli sledovat všechny operace v rámci stejné transakce.


### <a name="a-call-to-a-remote-service-fails"></a>Volání do vzdálené služby nezdaří.

**Zjišťování**. Kód chyby HTTP.

**Obnovení**

1. Opakování na přechodná k chybám. 
2. Pokud se nezdaří volání po *N* pokusí, náhradní akce. (Specifické pro aplikaci.)
3. Provedení [dělení okruh vzorek] [ circuit-breaker] Chcete-li předejít CSS k chybám. 

**Diagnostika**. Přihlaste se k chybám všechny vzdálené volání.


## <a name="next-steps"></a>Další kroky

Další informace o procesu FMA najdete v článku [odolnost záměrné služby cloudu] [ resilience-by-design-pdf] (ke stažení PDF).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

