<properties
   pageTitle="Migrace existující databáze do škálování | Microsoft Azure"
   description="Převedení sharded databází používat pružná databázové nástroje vytvořením shard správce mapy"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migrace existující databáze do škálování

Snadná správa stávajících sharded databází rozšířených pomocí databáze SQL Azure databázové nástroje (například národní [Knihovna klienta pružná databáze](sql-database-elastic-database-client-library.md)). Je nutné nejprve převést existující sady databází používat [shard mapování správce](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Základní informace
Migrace existující sharded databáze: 

1. Příprava [shard mapy správce databáze](sql-database-elastic-scale-shard-map-management.md).
2. Vytvoření mapy shard.
3. Připravte se na jednotlivé shards.  
2. Přidání mapování shard mapy.

Tyto postupy lze provést pomocí [.NET Framework klienta knihovny](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)nebo skriptů Powershellu vyhledaných [Databáze SQL Azure - pružná databázové nástroje skripty](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). V příkladech pomocí skriptů Powershellu.

Další informace o ShardMapManager najdete v článku [Shard mapování správy](sql-database-elastic-scale-shard-map-management.md). Základní informace o pružná databázové nástroje najdete v článku [Přehled funkcí pružná databáze](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Příprava správce databáze shard mapy

Správce mapy shard je speciální databáze, která obsahuje data, která chcete spravovat rozšířených databáze. Můžete použít existující databázi, nebo vytvořte novou databázi. Nezapomeňte, že budou sloužit jako správce mapy shard databáze by neměly být stejné databázi jako shard. Navíc nezapomeňte, že skript Powershellu nedojde k vytvoření databáze za vás. 

## <a name="step-1-create-a-shard-map-manager"></a>Krok 1: vytvoření shard správce mapy

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>K načtení správce shard mapy

Po vytvoření můžete získat správce shard mapy s tuto rutinu. Tento krok je nutný pokaždé, když je potřeba použít ShardMapManager objektu.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Krok 2: vytvoření mapování shard

Je nutné vybrat typ shard mapování a vytvořte. Volba závisí na architektura databáze: 

1. Jednoho klienta na databázi (podmínky, najdete v článku [Glosář](sql-database-elastic-scale-glossary.md).) 
2. Víc klientů na databázi (dva typy):
    3. Mapování seznamu
    4. Oblast mapování
 

Pro jednoho klienta model vytvoření mapy shard **mapování seznamu** . Model jednoho klienta přiřadí jednu databázi na klienta. Toto je efektivní model pro vývojáře SaaS zjednodušit správy.

![Mapování seznamu][1]

Model více klienta přiřadí několika klientů do jedné databáze (a distribuce skupiny Klienti přes více databázím). Tento model používejte, když je očekávaná každého klienta mít malé datových potřeb. V tomto modelu jsme přiřadit oblasti klienti databáze pomocí **mapování oblast**. 
 

![Oblast mapování][2]

Nebo můžete implementovat modelu databáze více klientovi pomocí *mapování seznamu* přiřadit víc klientů jednu databázi. Například ZR1 slouží k uložení informací o id klienta 1 a 5 a DB2 obsahuje data pro klienta 7 a klientovi 10. 

![Klienti vnořením na jedné databáze][3] 

**Podle svého výběru, zvolte jednu z těchto možností:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Možnost 1: vytvoření mapování shard pro mapování seznamu
Vytvoření mapy shard pomocí ShardMapManager objektu. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Možnost 2: vytvoření mapování shard pro mapování oblast

Všimněte si, že pokud chcete využít tento mapování vzorku, id klienta musí být souvislé oblasti hodnoty a je přijatelná mezer v oblasti tak, že jednoduše vynechání oblast při vytváření databází.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Možnost 3: Seznam mapování na jednu databázi
Nastavte si tento způsob je třeba vytvoření mapy seznamu uvedeno v kroku 2, možnost 1.

## <a name="step-3-prepare-individual-shards"></a>Krok 3: Příprava jednotlivé shards

Přidání jednotlivých shard (databáze) Správce shard mapy. Jednotlivé databáze se připraví pro uložení informací o mapování. Na každé shard proveďte tento postup.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Krok 4: Přidání mapování

Přidání mapování závisí na druhu shard mapu, kterou jste vytvořili. Pokud jste vytvořili seznam mapu, přidejte seznam mapování. Pokud jste vytvořili mapě oblast, přidejte oblast mapování.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Možnost 1: mapování dat pro mapování seznamu

Namapujte data přidáním mapování seznamu pro každého klienta.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Možnost 2: mapování dat pro mapování oblast

Přidání mapování oblast pro všechny klienta id oblast – přidružení databáze:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Krok 4 možnost 3: namapování dat pro víc klientů na jednu databázi

U každého klienta, spusťte přidat ListMapping (možnost 1, výše). 


## <a name="checking-the-mappings"></a>Kontrola mapování

Informace o existující shards a mapování spojené s nimi můžete zjistit pomocí následující příkazy:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Souhrn

Po dokončení instalace, můžete začít používat knihovnu klienta pružná databáze. Můžete taky [dat závislá směrování](sql-database-elastic-scale-data-dependent-routing.md) a [více shard dotazu](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Další kroky


Získání skriptů Powershellu z [sripts nástroje DB ohebné databáze SQL Azure](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

V sekci nástroje GitHub přihlášeni také: [Azure/ohebné db nástroje](https://github.com/Azure/elastic-db-tools).

Pomocí nástroje Rozdělit sloučení přesunout data do modelu jednoho klienta nebo z modelu více klienta. V tématu [nástroj sloučení rozdělení](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Další zdroje informací

Další informace o běžných vzorků dat architektura více klienta software jako služby (SaaS) databázové aplikace najdete v článku [Provedeních více klienta SaaS aplikací se databáze SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Dotazy a poslat žádost o funkci

Dotazy přejděte prosím kontaktujte nás na [Fórum komunity databáze SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a poslat žádost o funkci, přidejte je do [fórum pro názory databáze SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
