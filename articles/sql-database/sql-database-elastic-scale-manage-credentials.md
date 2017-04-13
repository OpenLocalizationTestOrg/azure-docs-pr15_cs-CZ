<properties 
    pageTitle="Správa přihlašovacích údajů v knihovně klienta pružná databáze | Microsoft Azure" 
    description="Jak nastavit správnou úroveň v části přihlašovací údaje správce jen pro čtení, pružná databáze aplikací" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Přihlašovací údaje pro přístup k knihovnu klienta pružná databáze

[Pružná databáze klienta knihovny](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) používá tři různé druhy přihlašovací údaje pro přístup k [shard mapy správce](sql-database-elastic-scale-shard-map-management.md). Podle potřeby pomocí nejnižší úrovně přístupu možné pověření.

* **Správa přihlašovacích údajů**: pro vytváření nebo k manipulaci s vedoucím shard mapy. (Viz [Glosář](sql-database-elastic-scale-glossary.md).) 
* **Přístupová pověření**: přístup k existující správce mapy shard k získání informací o shards.
* **Připojení pověření**: připojení k shards. 

[Správa databází a přihlášení v databázi SQL Azure](sql-database-manage-logins.md), viz také. 
 
## <a name="about-management-credentials"></a>O řízení přihlašovací údaje

Správa přihlašovacích údajů slouží k vytvoření [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objektu pro aplikace, které zpracovávají shard mapy. (Například, najdete v článku [Přidání shard pomocí pružná databázové nástroje](sql-database-elastic-scale-add-a-shard.md) a [závislá směrování dat](sql-database-elastic-scale-data-dependent-routing.md)) Uživatel knihovnu klienta pružná měřítko vytvoří SQL uživatele a SQL přihlášení a zajišťuje, aby že každý je oprávnění pro čtení i zápis na databáze globální shard mapy a taky všechny shard databází. Těchto přihlašovacích údajů se používají k Udržovat globální shard mapy a mapy místní shard po provedení změn mapy shard budou. Například pověření správy umožňuje vytvořit objekt shard mapy správce (pomocí [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Proměnná **smmAdminConnectionString** je připojovací řetězec, který obsahuje pověření správy. Uživatelské ID a heslo k dispozici pro čtení i zápis databáze shard mapy a jednotlivé shards. Správa připojovací řetězec obsahuje taky název serveru a název databáze k identifikaci databáze globální shard mapy. Tady je typické připojovací řetězec k tomuto účelu:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Nepoužívejte hodnoty ve formě "username@server"—instead budete moct používat jenom hodnotu "uživatelské jméno".  Je to proto pověření musí fungovat proti shard mapy správce databáze a jednotlivé shards, které mohou být na jiných serverů.

## <a name="access-credentials"></a>Access přihlašovací údaje
  
Při vytváření shard správce mapy v aplikaci, která není spravovat shard mapy, použijte přihlašovací údaje s oprávněními jen pro čtení na mapě globální shard. Informace načtené z globální shard mapy v části těchto přihlašovacích údajů se používají pro [směrování závislý na datech](sql-database-elastic-scale-data-dependent-routing.md) a k naplnění mezipaměť shard mapy na straně klienta. Přihlašovací údaje jsou k dispozici prostřednictvím stejné vzorek volání do **GetSqlShardMapManager** uvedené výše: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Všimněte si **smmReadOnlyConnectionString** tak, aby odrážely použití různých přihlašovacích údajů pro tento přístup jménem **jiných správu** uživatelů: těchto přihlašovacích údajů by neměly poskytovat oprávnění k zápisu na mapě globální shard. 

## <a name="connection-credentials"></a>Připojení přihlašovací údaje 

Při použití metodu [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) pro přístup k shard související s klávesou sharding jsou přístupové další údaje. Tyto přihlašovací údaje potřebovat oprávnění pro přístup k tabulkám mapy místní shard umístěných v shard jen pro čtení. To je potřeba k ověření připojení závislý na datech směrování u shard. Tento fragment kódu umožňuje přístup k datům v kontextu závislá směrování dat: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

V tomto příkladu obsahuje **smmUserConnectionString** připojovací řetězec pro přihlašovací údaje uživatele. Databáze SQL Azure tady je typické připojovací řetězec pro přihlašovací údaje uživatele: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Pomocí přihlašovacích údajů správce není hodnoty ve formě "username@server". Místo toho používejte jenom "uživatelské jméno".  Navíc nezapomeňte, že připojovací řetězec neobsahuje název serveru a název databáze. Je proto, že volání **OpenConnectionForKey** bude automaticky přímé připojení k správné shard na základě klíče. Proto název databáze a název serveru nezobrazují. 

## <a name="see-also"></a>Viz taky
[Správa databází a přihlášení v databázi SQL Azure](sql-database-manage-logins.md)

[Zabezpečení databáze SQL](sql-database-security.md)

[Začínáme s úloh pružná databází](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 