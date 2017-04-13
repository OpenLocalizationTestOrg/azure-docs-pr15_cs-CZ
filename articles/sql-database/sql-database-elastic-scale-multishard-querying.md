<properties 
    pageTitle="Dotazování více shard | Microsoft Azure" 
    description="Spouštění dotazů přes shards pomocí klienta knihovny pružná databáze." 
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
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Dotazování více shard

## <a name="overview"></a>Základní informace

[Pružná databázové nástroje](sql-database-elastic-scale-introduction.md)můžete vytvořit sharded databázová řešení. **Dotazování více shard** se používá pro úkoly například kolekce a vykazování dat, které vyžadují spuštění dotazu, který umístěna napříč několika shards. (Kontrastu to na [směrování závislé na data](sql-database-elastic-scale-data-dependent-routing.md), která provádí veškerá práce na jeden shard.) 

## <a name="overview"></a>Základní informace

1. Pokud potřebujete [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)nebo metody [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) . V tématu [**vytváření ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) a [**získat RangeShardMap nebo ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Vytvoření objektu **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** .
2. Vytvoření **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Nastavte **[Vlastnost CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** na příkaz T-SQL.
3. Provedení příkazu voláním **[ExecuteReader metody](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Zobrazení výsledků pomocí **[MultiShardDataReader předmětu](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Příklad

Následující kód ukazuje použití více shard dotazování pomocí zadaného **ShardMap** s názvem *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Klíčové rozdíl je konstrukce více shard připojení. Pokud **SqlConnection** pracuje na jednu databázi, trvá **MultiShardConnection** ***kolekce shards*** jeho předávat na vstupu. Vyplnění kolekce shards z shard mapy. Dotaz se spustí klikněte na kolekce shards pomocí **UNION ALL** sémantiku získat výsledkem je jedna celková hodnota. Název shard, ze kterého řádek pochází z v případě potřeby lze přidat do výstupu pomocí vlastnosti **ExecutionOptions** příkaz. 

Poznámka: volání **myShardMap.GetShards()**. Tento způsob načte všechny shards z shard mapy a poskytují snadný způsob, jak ke spuštění dotazu přes všechny relevantní databáze. Kolekce shards pro více shard dotazu může být kontrast další provedením LINQ dotaz přes kolekci vrácených volání **myShardMap.GetShards()**. V kombinaci s zásad částečné výsledky aktuální funkce v několika shard dotazování byl navržen tak, aby vhodný pro desítky až stovky shards.

Omezení u více shard dotazování momentálně chybí ověření pro shards a shardlets, které jsou pomocí dotazu. Během směrování závislý na datech ověří, že dané shard je součástí shard mapy v době dotazování, neprovádějte více shard dotazů kontrola. Může dojít k více shard dotazy na databáze, které mohly být odebrány z mapy shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Více shard dotazů a operace sloučení rozdělení

Více shard dotazů není ověřte, zda shardlets dotazovaném databázi se účastní probíhající operace sloučení rozdělení. (Viz [Měřítko nástrojem pružná databáze korespondence rozdělené](sql-database-elastic-scale-overview-split-and-merge.md).) To může vést k nekonzistence kde řádků ze stejného shardlet zobrazení pro více databází ve stejném více shard dotazu. Uvědomte si tato omezení a zvažte vyprázdnění probíhající operace sloučení rozdělení a změny mapu shard při provádění dotazů více shard.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Viz taky
Třídy **[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** a metody.


Správa shards [pružná databáze klienta knihovny](sql-database-elastic-database-client-library.md). Obsahuje obor názvů s názvem [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , která poskytuje možnost více shards pomocí jednoho dotazu a výsledků dotazu. Poskytuje dotazu odběru přes kolekce shards. Je také alternativní provádění zásady, zejména částečné výsledky řešení chyb při dotazování na mnoha shards.  

 