<properties 
    pageTitle="Pružná měřítko Azure SQL nejčastější dotazy týkající se | Microsoft Azure" 
    description="Nejčastější dotazy k pružná měřítko databáze Azure SQL." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Pružná databázové nástroje časté otázky 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Jak se pokud máme jednoho tenanta za shard a bez sharding klíč, naplnění klávesu sharding schématu informace?

Objekt informace schématu pouze slouží k rozdělení scénáře hromadné korespondence. Pokud podstatě jednoho klienta je aplikace, nevyžaduje Nástroj rozdělit sloučit a tedy není potřeba k naplnění objektu schématu informace.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Jste zřízení databáze a už mám odpovědi mapy Shard, jak zaregistrovat novou databázi jako shard?

Přečtěte si téma **[Přidání shard aplikace pomocí klienta knihovny pružná databáze](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Kolik pružná databázové nástroje stát?

Pomocí klienta knihovny pružná databázi není vynakládá žádné. Jenom pro databáze Azure SQL, které používáte pro shards a správce mapy Shard, jakož i webové či pracovníka role, které zřizujete pro nástroj sloučení rozděleného nabíhání nákladů.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Proč moje přihlašovací údaje nefunguje po přidání shard z jiného serveru?
Nepoužívejte přihlašovací údaje ve formě "uživatelské ID=username@servername”, jednoduše použijte" ID uživatele = uživatelské jméno ".  Taky Ujistěte se, že "uživatelské jméno" má oprávnění na shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Je potřeba vytvořit správce Shard mapy a naplnění shards při každém spuštění aplikace?

Ne – vytvoření správce mapy Shard (například **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) je jednorázovou operaci.  Aplikace používejte volání **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** v době po spuštění aplikace.  Třeba jenom jeden takové volání na doménu aplikace.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Můžu mít dotazy týkající se používání pružná databázové nástroje, jak se dostanu odpoví? 

Prosím kontaktujte nás na [Fórum komunity databáze SQL Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Když se mi zobrazí připojení k databázi pomocí sharding, můžu můžete pořád dotaz na data pro další klávesy sharding na stejné shard.  Je to záměrné?

Pružná rozhraní API měřítko umožňují připojení k databázi správné sharding klíče, ale nenabízejí sharding klíčové filtrování.  Přidáte **místo, KAM** věty do dotazu omezíte obor klávesu ujednaných sharding v případě potřeby.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Můžu používat jinou verzi Azure databáze pro každou shard v sadě shard?

Ano, shard je jednotlivé databáze a tedy jeden shard může být Premium edition, zatímco jiné standardní vydání. Kromě toho edition shard můžete změnit velikost nahoru nebo dolů tisknutím po dobu platnosti shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Znamená poskytování nástrojů korespondence rozdělení (nebo odstranění) databázi během operace sloučení nebo rozdělení? 

Ne. Pro **rozdělení** operace cílové databáze musí existovat s příslušnou schématu a registrovaný u správce Shard mapy.  Pro operace **sloučení** musí shard zabránění správce shard mapy a potom odstraňte databázi.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 