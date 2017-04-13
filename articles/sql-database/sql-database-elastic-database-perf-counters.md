<properties
    pageTitle="Výkonnosti pro správce shard mapy"
    description="ShardMapManager třídy a data závislá směrování výkonnosti"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Výkonnosti pro správce shard mapy

Shromáždíte výkonu [správce mapy shard](sql-database-elastic-scale-shard-map-management.md), zvlášť když použití [směrování závislá data](sql-database-elastic-scale-data-dependent-routing.md). Čítače vytvořené pomocí metody třídy Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Čítače slouží ke sledování výkonu operací [závislá směrování data](sql-database-elastic-scale-data-dependent-routing.md) . Tyto čítače jsou dostupné v nástroji Sledování výkonu ve skupinovém rámečku kategorie "Pružná: Shard Správa databáze".

**Nejnovější verzi:** Přejděte na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Viz taky [Upgrade aplikace pro použití nejnovější knihovny klienta pružná databáze](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Vytvoření kategorie výkonu a čítačů, uživatel musí být členem skupiny místních **správců** v počítači hostitelské služby aplikace.  

* Vytvoření instanci čítače výkonu a aktualizovat čítače, uživatel musí být členem skupiny **Administrators** nebo **Performance Monitor Users** . 

## <a name="create-performance-category-and-counters"></a>Vytvoření kategorie výkonu a čítačů 

Vytvoření čítače, zavolejte metodu CreatePeformanceCategoryAndCounters [ShardMapManagmentFactory předmětu](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Pouze správce můžete spustit metodu: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Taky můžete [Tento](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) skript Powershellu provádět požadovanou metodu. Metoda vytvoří následující výkonnosti:  

* **Mapování mezipaměť**: počet mapování mezipaměti pro danou mapu shard.
*  **DDR operace/sec**: sazbu závislá směrování operace s daty pro danou mapu shard. Čítač se aktualizuje při volání [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) za následek úspěšné připojení k určení shard. 
*  **Mapování přístupy do mezipaměti vyhledávacího/s**: míra vyhledávání operací úspěšné mezipaměti pro mapování shard. 
*  **Mapování vyhledávacího mezipaměti chybějící položky za sekundu**: míra vyhledávání operací selhalo mezipaměti pro mapování shard.
*  **Mapování přidali nebo aktualizovala v mezipaměti/sec**: rychlost se vás někdo přidá které mapování nebo aktualizované v mezipaměti pro danou mapu shard. 
*  **Mapování odebrány z mezipaměti/sec**: sazba niž mapování jsou odebraná z mezipaměti pro danou mapu shard. 

Výkonnosti vytvořené pro každou režim cached shard mapu na obrázku.  


## <a name="notes"></a>Poznámky
Následující události aktivace vystavením výkonnosti:  

* Inicializace [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) s [přes načítání](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), pokud ShardMapManager obsahuje všechny shard mapy. Jedná se o [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) a [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) metody.
* Úspěšné vyhledávání shard mapy (pomocí [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) nebo [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Úspěšné vytvoření mapy shard pomocí CreateShardMap().

Výkonnosti bude aktualizován operacemi mezipaměti provádějí shard mapy a mapování. Odinstalace shard mapě pomocí DeleteShardMap () reults odstranění instance čítače výkonu.  

## <a name="best-practices"></a>Doporučené postupy

* Vytvoření kategorie výkonu a čítače je třeba provést jenom jednou před vytvořením ShardMapManager objektu. Každé spuštění příkazu CreatePerformanceCategoryAndCounters() vymaže předchozí čítače (ztráty údajích všechny instance) a vytvoří nové.  

* Instance čítačů výkonu vzniká jednoho obrázku. Odstraňování instancí čítače výkonu způsobí zhroucení aplikace nebo odebrání map shard z mezipaměti.  

### <a name="see-also"></a>Viz taky

[Přehled funkcí pružná databáze](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

