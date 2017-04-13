<properties 
    pageTitle="Zahájení přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure | Microsoft Azure" 
    description="Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Zahájení přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-geo-replication-failover-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


V tomto článku se dozvíte, jak zahajte převzetí k databázi SQL sekundárním s [Azure portálu](http://portal.azure.com). Abyste mohli nakonfigurovat Geo replikace, najdete v článku [Konfigurace Geo replikace databáze SQL Azure](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Zahájení konverzace selhání

Sekundární databáze můžete přepínat osvobozením od primární.  

1. V [Azure portál](http://portal.azure.com) Procházet a hlavní databází ve partnerství Geo replikace.
2. Na zásuvné SQL databáze vyberte **všechna nastavení** > **Geo replikace**.
3. V seznamu **druhotné** vyberte databázi, kterou chcete nové primární a klepněte na příkaz **překlopení**.

    ![překlopení][2]

4. Klikněte na **Ano** zahájíte záložní.

Příkaz okamžitě přepne sekundární databáze do primární roli. 

Je krátké období, po kterou obě databáze nedostupné (pořadí 0 až 25 sekund) během rolím se přechodu na jiný. Pokud primární databázi používá více sekundární databází, příkaz automaticky znovu jiných druhotné se připojit k nové primární. Celou operaci lze provést méně než jednu minutu dokončete za normálních okolností. 

>[AZURE.NOTE] Pokud primární je online a potvrzování transakcí, pokud je příkaz vydán ztrátu dat může dojít.


## <a name="next-steps"></a>Další kroky   

- Po selhání zajistěte, aby že požadavky ověřování pro server a databázi nastavené na nové primární. Další informace najdete v tématu [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md).
- Další obnovení po selhání pomocí aktivní Geo replikace včetně pre a publikovat obnovení kroky a obnovení podrobností havárie naleznete v tématu [Obnovení cvičení havárie](sql-database-disaster-recovery.md)
- Sasha Nosov příspěvek na blogu o aktivní Geo replikace najdete v článku [zaměření na nové funkce Geo replikace](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informace o navrhování cloudové aplikace používat aktivní Geo replikace najdete v tématu [navrhování cloudu žádosti o nepřerušený pomocí Geo replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informace o používání aktivní Geo replikace fondy pružná databáze najdete v článku [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Přehled firmy continurity naleznete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
