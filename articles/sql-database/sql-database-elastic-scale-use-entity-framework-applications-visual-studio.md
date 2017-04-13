<properties 
    pageTitle="Pružná databáze klienta knihovny pomocí Entity Framework | Microsoft Azure" 
    description="Použít knihovnu pružná databáze klienta ale Entity Framework pro kódování databází" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Pružná databáze klienta do knihovny Entity Framework 
 
V tomto dokumentu zobrazí změny v aplikaci Entity Framework, které jsou potřebné pro integraci s [pružná databázové nástroje](sql-database-elastic-scale-introduction.md). Se zaměřuje na psaní [shard mapování správy](sql-database-elastic-scale-shard-map-management.md) a [směrování závislé dat](sql-database-elastic-scale-data-dependent-routing.md) pomocí Entity Framework **Kód první** přístup. [Kód první – nové databáze](http://msdn.microsoft.com/data/jj193542.aspx) kurz EF slouží jako náš pracovního příklad v tomto dokumentu. Ukázkový kód doprovodným tento dokument je součástí pružná databázové nástroje nastavte vzorků ve Visual Studiu ukázek kódu.
  
## <a name="downloading-and-running-the-sample-code"></a>Stažení a spuštění ukázkový kód
Pokud si chcete stáhnout kód v tomto článku:

* Vyžaduje se Visual Studio 2012 nebo novější. 
* Spusťte aplikaci Visual Studio. 
* Ve Visual Studiu-vyberte soubor > Nový projekt. 
* V dialogovém okně "Nový projekt" přejděte **Online vzorků** pro **Visual Basic** a zadejte "pružná db" do vyhledávacího pole v pravém horním rohu.
    
    ![Framework entita a pružná databáze ukázkové aplikace][1] 

    Vyberte ukázkový nazývá **Pružná DB nástroje pro SQL Azure – Entity Framework integrace**. Po přijetí licence, načte vzorku. 

Spuštění vzorku, je potřeba vytvořit tři prázdná databáze v databázi SQL Azure:

* Shard správce mapy databáze
* Databáze shard 1
* Databáze shard 2

Po vytvoření těchto databází vyplňte zástupného v **Program.cs** s název svého serveru databáze SQL Azure, názvy databází a svoje přihlašovací údaje pro připojení k databází. Vytvoření řešení ve Visual Studiu. Visual Studio stáhne požadované NuGet balíčky pro knihovnu klienta pružná databáze Entity Framework a přechodná odolnost zpracování jako součást procesu Tvůrce dotazů. Ujistěte se, že obnovení NuGet balíčků aktivované řešení řešení. Toto nastavení můžete povolit kliknutím pravým tlačítkem myši na soubor řešení v okně Průzkumník řešení Visual Studio. 

## <a name="entity-framework-workflows"></a>Entita rámec pracovních postupů 

Entita Framework vývojáři závisí na jednu z následujících čtyř pracovních postupů k vytváření aplikací a k zajištění trvalé pro objekty aplikace: 

* **Kód první (nové databáze)**: EF vývojářem modelu v kódu aplikace a potom EF generuje databáze z něho. 
* **Kód první (existující databáze)**: vývojář umožňuje EF generovat kód aplikace pro model z existující databáze.
* **První model**: vývojář vytvoří model v Návrháři EF a potom EF vytvoří databázi z modelu.
* **První databáze**: vývojář používá EF nástroje odvodit modelu z existující databáze. 

Těchto postupů přes v předmětu DbContext transparentně Správa připojení k databázi a schématu databáze aplikace. Jak se probereme Tato podrobnější dál v dokumentu, jiné konstruktory na základní třídy DbContext povolit pro různé úrovně publikum nemůže ovládat vytvoření připojení databáze vytváření zavádění a schématu. Problémy při vzniknou primárně tomu, že správu připojení databáze poskytovanou EF protíná s možnostmi správy připojení dat závislá směrování rozhraní podle klienta knihovnou pružná databáze. 

## <a name="elastic-database-tools-assumptions"></a>Klávesové zkratky nástroje předpoklady pružné databáze 

Definice termínů najdete v článku [Glosář nástroje pružná databáze](sql-database-elastic-scale-glossary.md).

Oddíly aplikace dat s názvem shardlets lze definovat s knihovnou klienta pružná databáze. Shardlets jsou určeny klíčem sharding a jsou namapované na konkrétní databáze. Aplikace může mít tolik databáze podle potřeby a distribuce shardlets poskytují dostatek kapacita nebo výkonu zadaných aktuální obchodní požadavky. Mapování hodnoty klíče sharding k databázím uložený ve shard mapy poskytované klienta pružná databáze rozhraní API. Název této možnosti **Správy mapy Shard**nebo SMM pro krátký. Mapa shard taky slouží jako zprostředkovatel připojení k databázi pro požadavky, které přenášejí sharding klíče. Budeme v nápovědě k této možnosti jako **směrování závislé na data**. 
 
Správce mapy shard zabraňuje uživatelé nekonzistentní zobrazení do shardlet dat, která mohou vyskytnout, když se děje operace správy souběžné shardlet (například přemístění dat z jedné shard do jiného). Postup mapy shard spravuje zprostředkovatele knihovny klienta připojení databáze aplikace. Díky funkci shard map automaticky při operace správy shard můžou mít vliv na shardlet vytvořenou připojení pro ukončení připojení k databázi. Tento přístup je třeba integrace s některých funkcí EF společnosti, například vytvářet nové připojení z existující Zkontrolovat existenci databáze. Obecně naše pozorování zjištění, že funguje jenom pracovní problémy se spolehlivým pro připojení skrytých databáze, která můžete bezpečně klonovat pro EF standardní DbContext konstruktory. Princip návrh pružná databáze místo toho je pouze zprostředkovatele otevřené připojení. Jednu myslíte, že zavření připojení brokered knihovnou klienta před předání k EF DbContext může vyřešit tento problém. Však zavřením připojení a může v EF ho znovu otevřít, jednu foregoes ověření a konzistence kontroly prováděné knihovnu. Funkci migrací v EF, ale používá tato připojení ke správě podkladového schématu databáze tak, že je průhledné do aplikace. V ideálním případě byste rádi uchovávání a kombinování všechny tyto funkce z knihovny klienta pružná databáze a EF ve stejné aplikaci. V následující části popisuje tyto vlastnosti a požadavky podrobněji. 


## <a name="requirements"></a>Požadavky 

Když pracujete s pružná databáze klienta knihovny a rozhraní API Framework Entity, chceme zachovat následující vlastnosti: 

* **Škálování**: Přidání nebo odebrání databází od osy dat sharded aplikace podle potřeby pro požadavky kapacity rohu aplikace. Znamená to, že ovládací prvek nad vytváření a odstraňování databází a pomocí shard pružné databáze mapování API Správce pro správu databáze a mapování shardlets. 

* **Soulad**: aplikace využívá sharding a používá závislá směrování možnosti dat podokně úloh Knihovna klienta. Chcete-li předejít poškození nebo výsledky nepovedlo dotazu, jsou připojení brokered přes správce shard mapy. Zachová také ověřování a konzistence.
 
* **Kód prvního**: uchovávání výhodou první paradigma společnosti EF kód. V první kód tříd v aplikaci jsou namapované transparentně základní struktura databáze. Kód aplikace spolupracuje s DbSets, které skryty většina aspektů v podkladovém zpracování databáze.
 
* **Schéma**: Entity Framework má na starosti vytvoření schématu počáteční databáze a následné schématu vývoj prostřednictvím migrace. Přizpůsobení aplikace je snadno zachováním tyto funkce jako vývoj data. 

Následující pokyny pokyn postup splňovat tyto požadavky pro kód první aplikací pomocí nástrojů pružná databáze. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Závislé směrování použití EF DbContext dat 

Připojení databáze s Entity Framework jsou obvykle spravovány prostřednictvím dílčí **DbContext**. Vytvořte tyto podřízené odvozené od **DbContext**. Je to, kde můžete definovat vaší **DbSets** , které implementovat kolekce záložní databáze CLR objektů aplikace. V rámci dat závislá směrování budou popsána několik užitečné vlastností, které nutně nemají další EF kód první aplikace scénáře: 

* Databáze již existuje a registrované v mapě shard pružná databáze. 
* Schéma aplikace již byly nasazeny k databázi (popsáno níže). 
* Závislý na datech směrování připojení k databázi jsou brokered tak, že shard mapa. 

Integrovat **DbContexts** s závislý na datech směrování škálování:

1. Vytváření fyzické databáze připojení prostřednictvím rozhraní klienta pružná databáze správce shard mapy 
2. Zalomit připojení s podtřídy **DbContext**
3. Předáte připojení základní třídy **DbContext** zajistit, aby že se všechny zpracování na straně EF stane taky. 

Následující příklad ukazuje tento přístup. (Tento kód je také v doprovodnou Visual Studio projektu)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Hlavní body
* Nový konstruktor nahradí výchozí konstruktor v podtřídě DbContext 
* Nový konstruktor má argumenty, které jsou potřeba pro závislá směrování dat prostřednictvím pružná databáze klienta knihovny:
    * Mapa shard pro přístup k rozhraní směrování závislé na data
    * klíč sharding k identifikaci shardlet,
    * připojovací řetězec s přihlašovací údaje pro připojení směrování závislý na datech shard. 
 
* Volání konstruktoru základní třídy, která bude detour do statické metodu, která provádí všechny potřebné kroky potřebné za účelem směrování závislé na data. 
   * Použije volání OpenConnectionForKey rozhraní klienta pružná databáze na mapě shard připojení otevřít.
   * Mapování shard vytvoří připojit ke shard obsahující shardlet dané sharding klíče.
   * Otevřít připojení předána zpět konstruktoru základní třídy DbContext, který označuje, že toto připojení pro použití v EF místo nechat EF vytvořit nové připojení automaticky. Tímto způsobem připojení byl označil klientem pružné databáze API tak, aby ho zaručit konzistence ve skupinovém rámečku operace správy shard mapu.
 
  
Použijte nový konstruktor podtřídy DbContext místo výchozí konstruktor v kódu. Tady je příklad: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Nový konstruktor otevře připojení k shard, která obsahuje data pro shardlet označenými přínosu **tenantid1**. Kód v bloku **pomocí** zůstane pro přístup k **DbSet** pro blogy používat EF na shard pro **tenantid1**beze změny. Tím se změní sémantiku pro kód do pomocí blokovat tak, aby se všechny operace databáze jsou teď s rozsahem jeden shard kde bude k dispozici **tenantid1** . Například LINQ dotaz přes blogy **DbSet** vrátí pouze blogy uložené na aktuální shard, ale ne těch, které jsou uložené v jiných shards.  

#### <a name="transient-faults-handling"></a>Přechodné chyby zpracování
Tým Microsoft Patterns a postupy publikovat [Přechodná poruch zpracování aplikace blokovat](https://msdn.microsoft.com/library/dn440719.aspx). Pružná měřítko klienta knihovny v kombinaci s EF se používá knihovnu. Ale zajistit, aby všechny přechodná výjimky vrátí na místo, kde jsme můžete zajistit, že konstruktoru new je použit po přechodná poruch tak, aby všechny nové připojení pokusu pomocí konstruktorů, kterou jsme tweaked. V opačném připojení k správné shard není zaručené a neexistují žádné záruky, které připojení je zachována dochází ke změnám shard mapu. 

Následující příklad kódu ukazuje použití zásad opakovat SQL okolo nové konstruktorům podtřídy **DbContext** : 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** kódu výše je definována jako **SqlDatabaseTransientErrorDetectionStrategy** s počet opakování 10 a 5 sekund prodleva mezi opakování. Tento postup je podobný návodu EF a inicializovaný uživatelem transakce (najdete v článku [omezení s opakování spuštění strategie (roku EF6)](http://msdn.microsoft.com/data/dn307226). Obě situace vyžadují program aplikace řídí obor do kterého přechodná výjimce vrací: znovu otevřete transakce nebo (jak je znázorněno) znovu kontextu správné konstruktoru, který využívá knihovnu klienta pružná databáze.

Potřebujete řídit, kdy přechodná výjimky trvat zpět v rozsahu taky neumožňuje použití předdefinované **SqlAzureExecutionStrategy** , které jsou součástí EF. **SqlAzureExecutionStrategy** by znovu otevřete připojení, ale pomocí **OpenConnectionForKey** a tedy obejít ověření se provádí v rámci hovoru **OpenConnectionForKey** . Ukázka kódu použije předdefinované **DefaultExecutionStrategy** , které jsou také součástí EF. Na rozdíl od **SqlAzureExecutionStrategy**funguje správně v kombinaci s zásady opakovat z přechodná zpracování chyby. Ve třídě **ElasticScaleDbConfiguration** je nastavená zásada spuštění. Všimněte si, že jsme nepoužije **DefaultSqlExecutionStrategy** od navrhne použít **SqlAzureExecutionStrategy** v případě vzniku přechodná výjimky - která by vedla ke nepovedlo chování, jak je popsáno. Další informace o různých opakovat zásady a EF najdete v článku [Připojení odolnost proti chybám v EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor přepíše
Výše uvedené příklady ilustrují výchozí konstruktor znovu zapíše při použití dat závislá směrování s Entity Framework vyžaduje aplikace. V následující tabulce zobecňuje tento přístup k další konstruktory. 


Aktuální konstruktor  | Přepsat konstruktor dat | Základní konstruktor | Poznámky
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, logická hodnota) |Připojení musí být funkci shard mapy a směrování klávesu závislé na data. Potřebujete k vytvoření automatického připojení obtokem tak, že EF a místo toho pomocí mapy shard broker připojení. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, logická hodnota) |Připojení je funkci shard mapy a směrování klávesu závislé na data. Řetězec jméno nebo připojení pevné databáze nebude fungovat se jich obtokem ověřením shard mapa. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logická hodnota) |Připojení k získání vytvoří dané shard mapy a sharding klíč s modelem k dispozici. Zkompilované modelu bude předávat základní c'tor.
MyContext (DbConnection, logická hodnota) |ElasticScaleContext (ShardMap TKey, logická hodnota) |DbContext (DbConnection, logická hodnota) |Připojení, musí být odvozeny od shard mapy a klíčem. Nemůže být zadané jako vstup (Pokud tento vstup používal už shard mapy a klávesu). Bude předán logická hodnota. 
MyContext (řetězec, DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logická hodnota) |Připojení, musí být odvozeny od shard mapy a klíčem. Nemůže být zadané jako vstup (Pokud tento vstup používal shard mapy a klíče). Zkompilované modelu bude předán. 
MyContext (ObjectContext, logická hodnota) |ElasticScaleContext (ShardMap, TKey ObjectContext, logická hodnota) |DbContext (ObjectContext, logická hodnota) |Nový konstruktor musí zajistěte, aby byl jakékoli připojení v ObjectContext předané jako vstup přesměrován připojení spravuje pružná měřítko. Podrobný popis ObjectContexts je předmětem tohoto dokumentu.
MyContext (DbConnection, DbCompiledModel, logická hodnota) |ElasticScaleContext (ShardMap, TKey DbCompiledModel, logická hodnota)| DbContext (DbConnection, DbCompiledModel, logická hodnota); |Připojení, musí být odvozeny od shard mapy a klíčem. Připojení nelze zadat jako vstup (Pokud tento vstup používal už shard mapy a klávesu). Model a logická hodnota předávají se základní třídy konstruktoru. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard schémat nasazení prostřednictvím EF migrace 

Správa schématu automatické je usnadnění poskytované Entity Framework. V souvislosti s aplikací pomocí pružná databázové nástroje chceme zachovat tuto možnost automaticky zřízení schématu nově vytvořený shards při databáze se přidají do sharded aplikace. Případu primární použití, je lepší alternativou v osy dat pro sharded aplikací pomocí EF. Sharded aplikaci založená na EF může na jeho EF funkcí pro správu schémat snižuje úsilí Správa databáze. 

Schéma nasazení prostřednictvím EF migrací funguje nejlíp u **Neotevřená připojení**. To je rozdíl od scénáře pro závisí na datech směrování, závisí na otevřené připojení poskytovanou pružná databáze klientského rozhraní API. Další rozdíl je požadovaného konzistence: při žádoucí zajistit soulad pro všechna připojení směrování závislý na datech a chráněny před souběžné shard mapy manipulace, není problém na počáteční schémat nasazení novou databázi obsahující dosud registrované v mapě shard a dosud byla přiděleno shardlets. Proto jsme můžete spolehnout standardní databázi připojení pro tento scénáře, namísto směrování závislé na data.  

To vede k přístupu k kde schémat nasazení prostřednictvím EF migrace je pevně svázáno s registrace nové databáze jako shard v mapě shard aplikace. To závisí na následující požadavky: 

* Databáze již vytvořila. 
* Databáze je prázdná – obsahuje žádné schéma uživatele a žádná uživatelská data.
* Databázi nelze ještě k nim získat přístup přes klienta pružná databáze rozhraní API za účelem směrování závislé na data. 

S těmito požadavky na místě můžete vytvořit běžná zrušením otevřené **SqlConnection** a spusťte tak EF migrací schémat nasazení. Následující příklad kódu znázorňuje tento přístup. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Tento příklad ukazuje metodu **RegisterNewShard** zaregistruje shard v mapě shard, nasadí schématu pomocí EF migrace, který ukládá mapování sharding klávesy shard. Závisí na konstruktoru podtřídu **DbContext** (**ElasticScaleContext** ve výběru), která trvá připojovací řetězec SQL předávat na vstupu. Kód tento konstruktor je vážný jejich předávání dál, jako na následujícím příkladu: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Jednu použili jste verzi konstruktoru dědí od základní třídy. Ale kód musí zajistit, že je výchozí inicializace EF využívá při připojení. Proto krátká detour do statickou metodu před voláte do konstruktoru základní třídy pomocí připojovacího řetězce. Všimněte si, že registrace shards by měla běžet v doméně jiné aplikace nebo proces, který zajišťuje, že nastavení inicializační EF není v konfliktu. 


## <a name="limitations"></a>Omezení 

Postupy uvedené v tomto dokumentu za následek pár omezení: 

* EF aplikace, které používají **LocalDb** nejdřív migrovat do standardní databázi SQL serveru před použitím knihovny klienta pružná databáze. Rozšiřování aplikaci prostřednictvím sharding pomocí pružná škály není možné s **LocalDb**. Všimněte si, že vývoj můžete dál používat **LocalDb**. 

* Všechny změny aplikace, které změny schématu databáze za následek potřeba nastavit EF migrací na všechny shards. Ukázkový kód pro tento dokument není ukazují, jak se to dělá. Zvažte možnost aktualizace databáze s parametrem ConnectionString iteraci všechny shards; nebo extrahování skript T-SQL na zjištění stavu úkolů migrace pomocí aktualizace databáze pomocí – skriptu možnost a jeho aplikování vaší shards skript T-SQL.  

* Možnost žádost, předpokládá se, že všechny své zpracování databáze je součástí jedné shard určeno klíčem sharding poskytovanou žádost. Však tento předpoklad není vždycky stiskněte a podržte PRAVDA. Například, pokud není možné zpřístupnit sharding klíče. Při řešení to knihovna klienta poskytuje **MultiShardQuery** třídy implementující odběru připojení pro dotazování přes několik shards. Používání **MultiShardQuery** v kombinaci s EF je nad rámec tohoto dokumentu

## <a name="conclusion"></a>Uzavření

Pomocí kroků uvedených v tomto dokumentu EF aplikace pomocí funkce knihovnu klienta pružná databáze pro data závislá směrování refaktoring konstruktory její podřízené **DbContext** použitého v aplikaci EF. Toto omezení změn muset tyto místa, kde již existují **DbContext** třídy. Kromě toho EF aplikace můžete nadále využívat automatické schémat nasazení kombinací kroky, které vyvolat potřebné EF migrace se registrace nového shards a mapování shard. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 