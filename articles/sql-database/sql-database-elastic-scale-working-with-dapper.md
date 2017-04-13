<properties 
    pageTitle="Pružná databáze klienta knihovny pomocí Dapper | Microsoft Azure" 
    description="Pružná databáze klienta knihovny pomocí Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Pružná databáze klienta knihovny pomocí Dapper 

Tento dokument je pro vývojáře, které jsou závislé na Dapper k vytváření aplikací, ale budete chtít podpořit [pružná databázové nástroje](sql-database-elastic-scale-introduction.md) pro vytváření aplikacemi implementující sharding škálování jejich osy dat.  Tento dokument ukazuje změny v Dapper aplikacím, které jsou potřebné pro integraci s pružná databázové nástroje. Náš fokus na psaní Správa shard pružná databáze a závisí na datech směruje s Dapper. 

**Ukázkový kód**: [pružná databázové nástroje pro databázi SQL Azure - Dapper integrace](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integrace **Dapper** a **DapperExtensions** s knihovnou klienta pružná databáze pro databázi SQL Azure je snadné. Aplikace můžete použít data závislá směrování změnou vytváření a otevírání nových objektů [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) používat volání [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) z [knihovny klienta](http://msdn.microsoft.com/library/azure/dn765902.aspx). Toto omezení změny v aplikaci pouze pokud vytváří a otevření nové připojení. 

## <a name="dapper-overview"></a>Dapper přehled
**Dapper** je objekt relační mapování. Mapy .NET objekty z aplikace na relační databáze (nebo naopak). První část kódu ukázka znázorňuje, jak integrovat knihovnu klienta pružná databáze pomocí aplikace založené na Dapper. Druhá část ukázkového kódu ukazuje, jak integrovat při použití Dapper a DapperExtensions.  

Funkce mapování v Dapper obsahuje rozšíření metody připojení databáze, které usnadňují odeslání příkazy T SQL pro spuštění nebo dotazu v databázi. Například Dapper usnadňuje mapování mezi .NET objekty a parametrech příkazy SQL **spouštění** volání nebo používání výsledky dotazech SQL na objekty .NET pomocí **dotazu** hovory z Dapper. 

Pokud chcete použít DapperExtensions, musíte už poskytují příkazy SQL. Rozšíření metody ATP **GetList** **Vložit** přes připojení k databázi vytvořit příkazů SQL na pozadí.
 
Další výhodou Dapper i DapperExtensions je, že aplikace určuje vytvoření připojení k databázi. Díky práce s knihovnou klienta pružná databáze, která zprostředkovatelů databáze připojení na základě mapování shardlets databází.

Dapper sestavení získáte v tématu [Dapper tečka síť](http://www.nuget.org/packages/Dapper/). Pole Dapper příponu najdete v článku [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Rychlý přehled knihovnu klienta pružná databáze

S knihovnou klienta pružná databáze definovat oddíly aplikace dat s názvem *shardlets* , namapujte je na databáze a přehlednost tak, že *sharding klíče*. Máte tolik databází potřebujete a distribuování váš shardlets v těchto databází. Mapování hodnoty klíče sharding k databázím uložený ve shard mapy poskytované rozhraní API knihovny. Tato možnost nazývá **shard mapování správy**. Mapa shard taky slouží jako zprostředkovatel připojení k databázi pro požadavky, které přenášejí sharding klíče. Tato možnost nazývá **závislá směrování data**.

![Shard mapy a závislá směrování dat][1]

Správce mapy shard zabraňuje uživatelé nekonzistentní zobrazení do shardlet dat, která mohou vyskytnout, když operace správy souběžné shardlet se děje na databází. Postup mapování shard broker připojení databáze vytvořené pomocí knihovnu aplikace. Operace správy shard můžou mít vliv shardlet, díky funkci shard map automaticky ukončení připojení k databázi. 

Místo použití tradiční způsob, jak vytvořit připojení pro Dapper, potřebujeme použijte [metodu OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn824099.aspx). Zajistíte, že všechny ověřování probíhá a připojení se spravuje správně přesun dat mezi shards.

### <a name="requirements-for-dapper-integration"></a>Požadavky pro Dapper integrace

Když pracujete s knihovnu klienta pružná databáze a Dapper rozhraní API, chceme zachovat následující vlastnosti:

* **Scaleout**: chceme přidání nebo odebrání databází osy dat sharded aplikace podle potřeby pro požadavky kapacity rohu aplikace. 

-    **Soulad**: vzhledem k tomu, že naše aplikace měřítko pomocí sharding, potřebujeme provádět závislá směrování data. Chcete používat Data závislá směrování funkce knihovny k tomu nevyzve. Zejména chceme zachovat ověření a konzistence zaručuje poskytovanou připojení, která jsou brokered přes správce shard mapy a zabránit tak poškození nebo výsledky nepovedlo dotazu. Zajistíte, že jsou připojení k dané shardlet odmítnuto nebo zastaveno (například) shardlet aktuálně přesunete do různých shard pomocí rozhraní API rozdělit/korespondence.

-    **Mapování objektu**: chceme zachovat výhodou mapování poskytovanou Dapper překládat mezi tříd v aplikaci a základní struktura databáze. 

Následující části jsou uvedeny pokyny pro tyto požadavky pro aplikace založené na **Dapper** a **DapperExtensions**.

## <a name="technical-guidance"></a>Příručku
### <a name="data-dependent-routing-with-dapper"></a>Závislé směrování s Dapper dat 

S Dapper obvykle pro vytváření a otevírání připojení k databázi podkladového odpovídá aplikaci. Možnost typu T aplikací, Dapper vrátí výsledků dotazu jako .NET kolekce typu T. Dapper provádí mapování z řádků výsledek T-SQL k objektům typ T. Podobně Dapper mapy .NET objekty do SQL hodnoty nebo parametrů pro příkazy data pro manipulaci s language (DML). Dapper nabízí tuto funkci prostřednictvím přípony metody běžná [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektu z knihoven ADO .NET SQL klienta. Připojení SQL vrácené pružná rozhraní API měřítko pro DDR jsou také běžná [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekty. Díky nám přímo přes Dapper rozšíření typ vrácený rozhraní API DDR knihovnu klienta, jako je také jednoduchého připojení klienta SQL.

Tyto poznámky zjednodušují pomocí připojení brokered klienta knihovnou pružná databáze pro Dapper.

Tento kód (z doprovodnou vzorek) ukazuje přístup, kde klávesu sharding poskytované aplikace do knihovny broker připojení k správné shard.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Volání [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) rozhraní API nahrazuje výchozí vytváření a otevírání připojení klienta SQL. Volání [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) má argumenty, které jsou potřeba pro směrování závislá dat: 

-    Mapa shard pro přístup k rozhraní závislá směrování dat
-    Klíč sharding k identifikaci shardlet
-    Přihlašovací údaje (uživatelské jméno a heslo) připojení k shard

Objekt mapy shard vytvoří připojení k shard obsahující shardlet dané sharding klíče. Pružná databáze klientského rozhraní API také označení připojení k provedení jeho konzistence záruky. Od volání [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) vrátí běžná objekt připojení klienta SQL, spočívá v následujících volání metody rozšíření **spouštět** z Dapper standardní Dapper praktické cvičení.

Dotazy pracují velmi velmi podobným způsobem – nejdřív otevřít připojení s protokolem [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) z klientského rozhraní API. Pak můžete pomocí metody běžná Dapper rozšíření mapovat výsledků dotazu SQL na objekty .NET:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Všimněte si, že **pomocí** bloku připojení DDR obory všechny operace databáze v rámci bloku do jednoho shard, kde bude k dispozici tenantId1. Dotaz vrátí jenom blogy uložené na aktuální shard, ale ne těch, které jsou uložené v jiných shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Závislé směrování s Dapper a DapperExtensions dat

Dapper získáváte ekosystém další rozšíření, které můžete přidat další pohodlí a odběru z databáze při vytváření databázové aplikace. DapperExtensions je znázorněn příklad. 

Použití DapperExtensions v aplikaci nezmění jak vytváří a spravuje připojení k databázi. Je pořád aplikace zodpovědnost otevřete připojení a běžná objekty připojení klienta SQL očekává se, že metodami přípony. Podle pokynů uvedených výše jsme můžete spolehnout [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) . Následující kód ukázky ilustrují, pouze změnit je, že už máme psát příkazy T-SQL:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

A tady je ukázka kódu pro dotaz: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Zpracování přechodná chyb

Tým Microsoft Patterns a postupy publikovat [Přechodná poruch zpracování blok aplikace](http://msdn.microsoft.com/library/hh680934.aspx) aby vývojáři aplikace zmírnění společné podmínky přechodná poruch narazili při spuštění v cloudu. Další informace najdete v tématu [Perseverance, tajná všechny vítězství: pomocí blok aplikace zpracování přechodná poruch](http://msdn.microsoft.com/library/dn440719.aspx).

Ukázka kódu závisí na knihovnu přechodná poruch a chráněny před přechodná chyby. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** kódu výše je definována jako **SqlDatabaseTransientErrorDetectionStrategy** s počet opakování 10 a 5 sekund prodleva mezi opakování. Pokud používáte transakce, ujistěte se, rozsah opakování návrat na začátek transakce v případě přechodná poruch.

## <a name="limitations"></a>Omezení

Postupy uvedené v tomto dokumentu za následek pár omezení:

* Ukázkový kód pro tento dokument není ukazují, jak spravovat schéma přes shards.
* Možnost žádost, jsme se předpokládá, že jeho zpracování databáze je obsažená v jedné shard určeno klíčem sharding poskytovanou žádost. Však tento předpoklad není vždy podržte, například když není možné zpřístupnit sharding klíče. Při řešení to, obsahuje knihovnu klienta pružná databáze [MultiShardQuery předmětu](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Třídy používá odběru připojení pro dotazování přes několik shards. Použití MultiShardQuery v kombinaci s Dapper je předmětem tohoto dokumentu.

## <a name="conclusion"></a>Uzavření

Aplikace pomocí Dapper a DapperExtensions můžete snadno využít pružná databázové nástroje pro databázi SQL Azure. Pomocí kroků uvedených v tomto dokumentu tyto aplikace můžete pomocí funkce na nástroj pro data závislá směrování změnou vytváření a otevírání nových objektů [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) využívat volání [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) knihovny klienta pružná databáze. Toto omezení změny aplikace muset tyto místa, kde vytváří a otevření nové připojení. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 