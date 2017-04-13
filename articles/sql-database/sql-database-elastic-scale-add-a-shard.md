<properties 
    pageTitle="Přidání shard pomocí pružná databázové nástroje | Microsoft Azure" 
    description="Použití rozhraní API pružná měřítko nové shards přidáte shard nastavení." 
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

# <a name="adding-a-shard-using-elastic-database-tools"></a>Přidání shard pomocí pružná databázové nástroje

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Chcete-li přidat shard pro novou oblast nebo klíč  

Aplikace se často nutné jednoduše přidat nové shards zpracování dat, kterou očekává z nových klíčů nebo klíčové oblasti, shard mapování, které už existuje. Například aplikace sharded podle ID klienta asi muset vytvořit nové shard u nového klienta nebo sharded měsíční dat může být nutné nové shard zřízení před začátkem každý nový měsíc. 

Pokud i nový rozsahu klíčových hodnot není součástí existující mapování, je velmi jednoduché a přidejte nové shard přidružit nový klíč nebo oblasti, které shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Příklad: Přidání shard a jeho rozsah dat do existující mapování shard
Tento příklad používá [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx) [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) metody a vytváří instanci třídy [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . V následující ukázce databázi s názvem **sample_shard_2** a všechny potřebné schématu objekty do něj vytvořené pro uložení oblast [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Jako alternativu můžete pomocí prostředí Powershell vytvořte nové mapování odpovědi Shard. Příklad je k dispozici [v tomto poli](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Chcete-li přidat shard pro prázdnou část stávající rozsah  

V některých případech můžete už namapované oblasti shard a částečně vyplněný s daty, ale teď chcete nadcházející data přesměrováni na různých shard. Například se shard podle rozsahu den muset shard už přidělenou 50 dnů, ale dnem 24 chcete má v různých shard budoucí data. Pružná databáze [Nástroje rozděleného sloučení](sql-database-elastic-scale-overview-split-and-merge.md) můžete tuto operaci, ale pokud přesun dat není nutné zadávat (například data pro oblasti dnů [25, 50), tedy, den 25 včetně 50 výhradní dosud neexistuje) můžete provádět úplně přímo pomocí rozhraní API Shard mapy správy.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Příklad: rozdělení oblasti a přiřadit shard nově přidaný do prázdné části

Databáze s názvem "sample_shard_2" a všechny potřebné schématu objekty do něj byly vytvořeny.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Důležité**: použít tento postup, jen pokud si nejste jistí, že rozsah aktualizované mapování je prázdná.  Výše uvedené metody nerezervujte data pro oblasti, která je přesouvána, tak je vhodné zahrnout kontroly v kódu.  Pokud řádky jsou v oblasti, která je přesouvána, a vlastních dat rozdělení neodpovídají aktualizované shard mapy. Pomocí [Nástroje rozdělit sloučení](sql-database-elastic-scale-overview-split-and-merge.md) k provedení operace místo v těchto případech.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
