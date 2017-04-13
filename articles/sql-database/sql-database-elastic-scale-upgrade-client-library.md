< vlastnosti
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Upgrade aplikace pro použití nejnovější knihovny klienta pružná databáze

Nová [Knihovna klienta pružná databáze](sql-database-elastic-database-client-library.md) jsou k dispozici prostřednictvím NuGetand rozhraní NuGetPackage správce ve Visual Studiu. Upgrade obsahují opravy chyb a podpory pro nové funkce podokně úloh Knihovna klienta.

**Nejnovější verzi:** Přejděte na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Aplikaci s novou knihovnu, jakož i změnit existující metadata správce mapy Shard uložené v databázích SQL Azure nové funkce.

Provedením následujících kroků v pořadí zajišťuje starší verze knihovnu klienta už prezentovat ve vašem prostředí k aktualizaci objekty metadat, což znamená, že objekty starší verzi metadat nevytvoří se po upgradu.   

## <a name="upgrade-steps"></a>Kroky probereme

**1. upgrade aplikace.** Ve Visual Studiu stáhněte si a odkazovat na nejnovější verzi klienta knihovny do všech projektů vývoj, které využívají knihovnu; potom opětovné vytvoření a nasazení. 

 * Ve Visual Studiu řešení, vyberte **Nástroje** --> **Správce balíčků NuGet** -->  **Správa NuGet balíčků řešení**. 
 * (Visual Studio 2013) V levém panelu vyberte **aktualizací**a potom klikněte na tlačítko **Aktualizovat** na balíček **Azure SQL databáze pružná měřítko klienta knihovny** , která se zobrazí v okně.
 * (Visual Studio 2015) Nastavte pole Filtr upgradovat **k dispozici**. Vyberte balíček aktualizovat a klikněte na tlačítko **Aktualizovat** .
    
 
 * Vytvořte a nasaďte. 

**2. upgrade skriptů.** Pokud používáte skriptů **Powershellu** ke správě shards [stáhnout novou verzi knihovny](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) a zkopírujte ho do adresáře, ze kterého spustit skripty. 

**3. upgrade služby rozdělit hromadné korespondence.** Pokud používáte nástroj rozdělit sloučení pružná databáze pro změnu uspořádání sharded data, [Stáhnout a nainstalovat nejnovější verzi nástroje](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Podrobné kroky probereme pro službu najdete [tady](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. upgradovat databáze správce mapy Shard**. Upgradujte metadata podpora Shard mapy v databázi SQL Azure.  Existují dva způsoby, jak se dají dělat, pomocí prostředí PowerShell nebo C#. Obě možnosti jsou uvedeny níže.

***Možnost 1: Upgrade metadat pomocí prostředí PowerShell***

1. Stáhněte si nejnovější nástroj příkazového řádku pro NuGet z [tady](http://nuget.org/nuget.exe) a uložte do složky. 

2. Otevřete příkazový řádek a přejděte do stejné složky, příkaz:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Přejděte do podsložky obsahující novou verzi DLL klienta, který jste právě stáhli, například:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Stahování [Centrum skriptů](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)pružná databáze skriptletu upgradu klienta a uložit, abyste ji do složky obsahující DLL.

5. Z této složky spusťte "Powershellu.\upgrade.ps1" z příkazového řádku a postupujte podle pokynů.
 
***Možnost 2: Aktualizace metadat pomocí C#***

Můžete taky vytvořte aplikaci Visual Studio, která otevře vaší ShardMapManager, iterace přes všechny shards a provede upgrade metadat voláním metody [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) a [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) jako v následujícím příkladu: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Tyto postupy upgradech metadat lze použít tisknutím bez poškození. Například pokud starší verzi klienta omylem vytvoří shard, když už jste aktualizovali, můžete spustit upgrade znovu přes všechny shards a ujistěte se, že nejnovější verze metadat je prezentovat v celém infrastrukturu. 

**Poznámka:**  Novou verzi klienta knihovny publikování ke dnešnímu dni pokračovat v práci v předchozích verzích metadata správce Shard mapy na databáze SQL Azure a naopak.   Však využít některé praktickým novým funkcím v klientovi nejnovější, třeba upgradovat metadata.   Všimněte si, že aktualizuje metadata neovlivní všechna uživatelská data nebo specifická data, pouze objekty vytvořené a používá správce Shard mapy.  A aplikace nadále pracovat prostřednictvím při upgradu jsme je popsali výše. 

## <a name="elastic-database-client-version-history"></a>Historie verzí klienta pružná databáze 

Historie verzí přejděte na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 