<properties
    pageTitle="Konfigurace Geo replikace databáze Azure SQL pomocí Transact-SQL | Microsoft Azure"
    description="Konfigurace Geo replikace databáze SQL Azure pomocí jazyce Transact-SQL"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurace Geo replikace databáze Azure SQL pomocí Transact-SQL

> [AZURE.SELECTOR]
- [Základní informace](sql-database-geo-replication-overview.md)
- [Azure portálu](sql-database-geo-replication-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tento článek popisuje, jak nakonfigurovat aktivní Geo replikace databáze SQL Azure pomocí jazyce Transact-SQL.

Zahajte převzetí pomocí jazyce Transact-SQL, najdete v článku [Zahájit plánované nebo neplánované přepojení pro databáze SQL Azure pomocí jazyce Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktivní Geo replikace (čitelné druhotné) je teď k dispozici pro všechny databáze ve všech vrstvách služby. V dubnovou 2017-čitelné sekundární typ ukončení a existující-čitelné databáze upgradují automaticky k čitelné druhotné.

Konfigurace aktivní Geo replikace pomocí jazyce Transact-SQL, budete potřebovat následující:

- Předplatné Azure.
- Logické serveru databáze SQL Azure <MyLocalServer> a databázi SQL <MyDB> -primární databáze, kterou chcete replikovat.
- Jeden nebo více logické databáze SQL Azure serverů < MySecondaryServer(n) > - logické servery, které budou servery partnera, ve kterých se vytváří sekundární databáze.
- Přihlášení, které je DBManager na primárním, máte db_ownership místní databázi bude geo replikace a DBManager servery partnera, ke kterému se konfigurace Geo replikace.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Přidání sekundární databáze

K vytvoření replikovat geo sekundární databáze na serveru partner můžete použít příkaz **Vlastnosti databáze** . Spusťte tento příkaz na hlavní databáze serveru obsahujícího databázi replikovat. Geo replikovanou databázi ("primární databázi") bude mít stejný název jako replikace databáze a ve výchozím nastavení mají stejné úrovni služeb jako primární databáze. Sekundární databáze může být číst nebo čitelné a může být jednu databázi nebo pružná databbase. Další informace najdete v tématu [Vlastnosti databáze (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [Úrovní služby](sql-database-service-tiers.md).
Po sekundárním databáze vytvořené a nasadí, začne se data replikace asynchronní z primární databáze. Následující kroky popisují, jak nakonfigurovat Geo replikace pomocí Management Studio. Postup vytvoření-čitelnější a čitelný druhotné, ať už jednu databázi nebo databázi pružná jsou k dispozici.

> [AZURE.NOTE] Příkaz se nezdaří, pokud existuje databáze na serveru zadaný partnera se stejným názvem jako primární databáze.


### <a name="add-non-readable-secondary-single-database"></a>Přidání-čitelné sekundární (jedné databáze)

Pomocí následujících kroků k vytvoření-čitelné sekundární jako jednu databázi.

1. Pomocí verze 13.0.600.65 nebo novější SQL Server Management Studio.

     > [AZURE.IMPORTANT] Stáhněte si [nejnovější](https://msdn.microsoft.com/library/mt238290.aspx) verzi SQL Server Management Studio. Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizace s aktualizacemi portálu Azure.


2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Pokud chcete vytvořit místní databázi Geo replikace primárního s-čitelné sekundárním databází na MySecondaryServer1 kde MySecondaryServer1 je název vašeho serveru popisný použijte příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klikněte na tlačítko **Spustit** spusťte dotaz.


### <a name="add-readable-secondary-single-database"></a>Přidání čitelné sekundární (jedné databáze)
Pomocí následujících kroků k vytvoření čitelné sekundární jako jednu databázi.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Pokud chcete vytvořit místní databázi Geo replikace primární s čitelné sekundární databáze na serveru sekundární pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klikněte na tlačítko **Spustit** spusťte dotaz.



### <a name="add-non-readable-secondary-elastic-database"></a>Přidání-čitelné sekundární (pružná databáze)

Umožňuje vytvořit – čitelné sekundární jako pružná databázi podle těchto kroků.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Pokud chcete vytvořit místní databázi Geo replikace primární s-čitelné sekundární databáze na vedlejší serveru ve fondu pružná pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klikněte na tlačítko **Spustit** spusťte dotaz.



### <a name="add-readable-secondary-elastic-database"></a>Přidání čitelné sekundární (pružná databáze)
Umožňuje vytvořit čitelné sekundární jako pružná databázi podle těchto kroků.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Pokud chcete vytvořit místní databázi Geo replikace primární s čitelné sekundární databáze na vedlejší serveru ve fondu pružná pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klikněte na tlačítko **Spustit** spusťte dotaz.



## <a name="remove-secondary-database"></a>Odebrat sekundární databázi

Trvale ukončit replikace partnerství jeho primární a sekundární databáze můžete použít příkaz **Vlastnosti databáze** . Tento příkaz je proveden na hlavní databáze, ve kterém se nachází primární databáze. Po ukončení relace sekundárním databáze se změní na standardní databázi pro čtení i zápis. Při přerušení propojení sekundární databáze příkazu povede, ale sekundární stane po obnovení připojení pro čtení i zápis. Další informace najdete v tématu [Vlastnosti databáze (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [Úrovní služby](sql-database-service-tiers.md).

Pomocí následujících kroků replikovat geo sekundární odebrání partnerství Geo replikace.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Odebrání sekundárních replikovat geo pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klikněte na tlačítko **Spustit** spusťte dotaz.

## <a name="monitor-geo-replication-configuration-and-health"></a>Sledování stavu a konfigurace Geo replikace

Sledování úkolů zahrnují sledování konfiguraci Geo replikace a sledování stavu replikace data.  Správa dynamických zobrazení **sys.dm_geo_replication_links** v hlavní databáze slouží k vrácení informací o všechny existující replikační odkazy na každou databázi na serveru logické databáze SQL Azure. Toto zobrazení obsahuje řádek pro každou replikace propojení mezi primárních a sekundárních databází. Správa dynamických zobrazení **sys.dm_replication_link_status** umožňuje vrátit řádku na každou databázi SQL Azure, která je aktuálně zabývají replikace replikace odkaz. Tato volba zahrnuje primární a sekundární databází. Pokud existuje více než jeden odkaz nepřetržitý replikace pro danou databázi primárního, tato tabulka obsahuje řádek pro každou vztahy. Zobrazení je vytvořit v všechny databáze, včetně logické předlohy. Dotazování toto zobrazení v předloze logické však vrátí prázdnou sadu. Správa dynamických zobrazení **sys.dm_operation_status** slouží k zobrazení stavu pro všechny operace databáze včetně stavu replikační odkazy. Další informace najdete v tématu [sys.geo_replication_links (databáze SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (databáze SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)a [sys.dm_operation_status (databáze SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx).

Sledování partnerství Geo replikace pomocí následujících kroků.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Umožňuje znázornit všechny databáze s odkazy Geo replikace následující příkaz.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klikněte na tlačítko **Spustit** spusťte dotaz.
5. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **MyDB**a potom klikněte na **Nový dotaz**.
6. Umožňuje znázornit následující příkaz replikace pomalou a poslední replikace čas Můj sekundární databází MyDB.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klikněte na tlačítko **Spustit** spusťte dotaz.
8. Umožňuje znázornit posledních geo replikace operace spojené s databází MyDB následující příkaz.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klikněte na tlačítko **Spustit** spusťte dotaz.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Upgrade-čitelné sekundární pro čtení

V dubnovou 2017-čitelné sekundární typ ukončení a existující-čitelné databáze upgradují automaticky k čitelné druhotné. Pokud používáte-čitelné druhotné dnes a byste měli upgradovat, aby byly čitelné, můžete pro každou sekundární následující jednoduchých kroků.

> [AZURE.IMPORTANT] Je bez samoobslužné metoda místní upgrade z-čitelné sekundární k čitelný. Pokud přetáhnete jenom sekundárním, pak primární databáze zůstává Odemknutý až do nového sekundární bude kopmletně sesynchronizovaná. Pokud vaše aplikace SLA vyžaduje, že hlavní vždy zamčený, byste měli zvážit vytvoření paralelní sekundární v jiném serveru před použitím výše uvedené kroky probereme. Poznámka: každý primární můžete mít až 4 sekundární databází.


1. Nejdřív připojit k serveru daného *sekundární* a uvolnění-čitelné sekundární databáze:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Nyní připojit k serveru daného *primární* a přidejte nové čitelné sekundární

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Další kroky

- Další informace o aktivní Geo replikace najdete v tématu - [Aktivní Geo replikace](sql-database-geo-replication-overview.md)
- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
