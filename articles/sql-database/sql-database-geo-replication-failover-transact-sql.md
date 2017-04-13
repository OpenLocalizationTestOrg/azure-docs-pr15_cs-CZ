<properties 
    pageTitle="Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí jazyce Transact-SQL | Microsoft Azure" 
    description="Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí jazyce Transact-SQL" 
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
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí jazyce Transact-SQL


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-geo-replication-failover-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Tento článek ukazuje, jak zahajte převezme sekundární databáze SQL pomocí jazyce Transact-SQL. Abyste mohli nakonfigurovat Geo replikace, najdete v článku [Konfigurace Geo replikace databáze SQL Azure](sql-database-geo-replication-transact-sql.md).



Zahajte překlopení, musíte:

- Přihlášení, které je DBManager na primárním, máte db_ownership místní databázi bude geo replikace a DBManager servery partnera, ke kterému se konfigurace Geo replikace.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Zahájení konverzace plánované přepojení podporu sekundární databáze osvobozením od nové primární

K povýšení sekundární databáze osvobozením od novou databázi primární plánované způsobem, převedení existujícího primární osvobozením od sekundární můžete použít příkaz **Vlastnosti databáze** . Tento příkaz je proveden na hlavní databáze na serveru logické databáze SQL Azure, ve kterém je umístěn replikovat geo sekundární databázi, která je propagované. Tato funkce slouží k plánované překlopení, jako je třeba během cvičení DR a vyžaduje, aby databázi primární k dispozici.

Příkaz provádí následující pracovního postupu:

1. Dočasně kombinace kláves vymění replikace synchronní režim příčinou všechny zbývající transakce na vyprázdnit sekundární a všechny nové transakce; blokování

2. Kombinace kláves vymění role dvou databází v partnerství Geo replikace.  

Tato řada zaručuje, že dvě databáze jsou synchronizovány před přepněte role a tedy dojde k žádné ztrátě dat. Je krátké období, po kterou obě databáze nedostupné (pořadí 0 až 25 sekund) během rolím se přechodu na jiný. Pokud primární databázi používá více sekundárním databází, příkaz automaticky znovu jiných druhotné se připojit k nové primární.  Celou operaci lze provést méně než jednu minutu dokončete za normálních okolností. Další informace najdete v tématu [Vlastnosti databáze (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [Úrovní služby](sql-database-service-tiers.md).


Pomocí následujících kroků zahajte plánované přepojení.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru ve kterém je umístěn replikovanou geo sekundárním databázi.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Přepnutí sekundární databáze na primární roli pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB> FAILOVER;

4. Klikněte na tlačítko **Spustit** spusťte dotaz.

>[AZURE.NOTE] V některých případech je možné, že operaci nelze dokončit, může se zobrazit zablokované. V tomto případě a uživatel může provést příkaz převzetí platnost přijmout ztrátou dat.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Zahájení neplánované převezme z primární databáze sekundární databáze

Můžete použít příkaz **Vlastnosti databáze** na podporu sekundární databáze osvobozením od novou databázi primárního neplánované způsobem vynucení snížení úrovně existující primární osvobozením sekundární v době, kdy primárního databáze už není dostupná. Tento příkaz je proveden na hlavní databáze serveru logické databáze SQL Azure, ve kterém je umístěn replikovat geo sekundární databázi, která je propagované.

Tato funkce slouží k obnovení havárie při obnovení dostupnost databáze kritické a ztrátu dat přijatelné. Při vyvolání vynuceného převzetí zadaná sekundární databáze okamžitě změní primární databáze a přijímat transakce zapsat, nebude zahájen. Hned, jak je moct připojit se toto nové databáze primární původní primární databáze, zálohování, se považuje na původní primární databáze a staré primární databáze je volání do vedlejší databáze pro novou databázi primární. následně je pouze synchronizace otevřené nové primární.

Však protože čárky obnovit v době není podporován sekundární databází, pokud chce uživatel obnovení dat potvrzeného starou primární databázi, které nebyly byla replikovat na novou databázi primární před došlo k vynucený překlopení, uživatel bude potřebovat provozovat podpory k obnovení to ztracena data.

Pokud primární databázi používá více sekundární databází, příkaz automaticky znovu jiných druhotné se připojit k nové primární.

Zahájení neplánované převzetí pomocí následujících kroků.

1. V Management Studiuo připojení k databázi SQL Azure logické serveru ve kterém je umístěn replikovanou geo sekundární databázi.

2. Otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

3. Přepnutí sekundární databáze na primární roli pomocí následující příkaz **Vlastnosti databáze** .

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klikněte na tlačítko **Spustit** spusťte dotaz.

>[AZURE.NOTE] Pokud je příkaz vydán při online jsou primárních a sekundárních staré primární nepůjde nové sekundární okamžitě bez dat synchronizace. Spouštění transakce, pokud je příkaz vydán ztrátu dat může dojít, pokud je primární.



## <a name="next-steps"></a>Další kroky   

- Po selhání zajistěte, aby že požadavky ověřování pro server a databázi nastavené na nové primární. Další informace najdete v tématu [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md).
- Další obnovení po selhání pomocí aktivní Geo replikace včetně pre a publikovat obnovení kroky a obnovení podrobností havárie naleznete v tématu [Obnovení havárie](sql-database-disaster-recovery.md)
- Sasha Nosov příspěvek na blogu o aktivní Geo replikace najdete v článku [zaměření na nové funkce Geo replikace](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informace o navrhování cloudové aplikace používat aktivní Geo replikace najdete v tématu [navrhování cloudu žádosti o nepřerušený pomocí Geo replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informace o používání aktivní Geo replikace fondy pružná databáze najdete v článku [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Přehled firmy continurity naleznete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
