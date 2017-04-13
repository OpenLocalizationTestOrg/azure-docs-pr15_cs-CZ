<properties
    pageTitle="Změna úrovně osy a výkonu služby databáze Azure SQL | Microsoft Azure"
    description="Změna vrstvy služeb a úroveň výkonu databáze Azure SQL ukazuje, jak změnit velikost databáze SQL nahoru nebo dolů. Změna ceny osy databáze Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Změna služby osy a výkonu úrovně (cena za osy) databáze SQL Azure portálu


> [AZURE.SELECTOR]
- [**Azure portálu**](sql-database-scale-up.md)
- [Prostředí PowerShell](sql-database-scale-up-powershell.md)


Služba úrovní výkonu úrovně popisuje funkce a prostředky k dispozici pro databázi SQL a můžete aktualizovat podle potřeby změnit aplikace. Podrobnosti najdete v tématu [– Úrovně služeb](sql-database-service-tiers.md).

Všimněte si, že změna vrstvy služeb a/nebo úroveň výkonu databáze vytvoří kopii původní databáze na nové úrovni výkonu a pak se přepne připojení k otevřené. Během tohoto procesu dojde ke ztrátě žádná data, ale během stručný momentový při jsme přepnutím na otevřené připojení k databázi zakázány, aby některé transakce letu může vrátit zpět. Toto okno se liší, ale je průměrně o ve skupinovém rámečku 4 sekundy a ve více než 99 % případech je menší než 30 sekund. Velmi málo zejména pokud existují velké počtu transakce v letech v okamžiku, kdy připojení jsou zakázány, toto okno může být delší.  

Doba trvání celého procesu měřítko zdola nahoru závisí na velikosti a service osy databázi před a po změně. 250 GB databázi, která se mění do z nebo v rámci standardní služba osy, by například Dokončit 6 hodin. Pro danou databázi velikosti změny výkonu úrovních vrstvy služeb Premium měli dokončit 3 hodin.


Pomocí informací v [Azure SQL databáze služby úrovní a výkonu úrovně](sql-database-service-tiers.md) k určení úrovně osy a výkonu příslušné služby databáze SQL Azure.

- Snížit verzi databáze, musí být menší než maximální povolenou velikost vrstvy služeb cílovou databázi. 
- Při upgradu databáze s [Geo replikace](sql-database-geo-replication-overview.md) povoleno, je třeba nejprve upgradovat sekundární databází na osy požadovanou výkonu před upgradem primárního databáze.
- Při přechodu vrstvy služeb, musíte nejdřív ukončit všechny relace Geo replikace. 
- Nabízené služby obnovení se liší pro různé úrovně služby. Pokud jsou přechodu se může dojít ke ztrátě možnost obnovit bodu v čase nebo nižší období záložní uchovávání informací. Další informace najdete v článku [zálohy databáze SQL Azure a obnovení](sql-database-business-continuity.md).
- Změna databáze ceny osy nezmění velikost maximální databáze. Databáze maximální velikosti můžete změnit [Jazyce Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) nebo [Powershellu](https://msdn.microsoft.com/library/mt619433.aspx).
- Nové vlastnosti databáze nejsou použity až do dokončení změny.



**Tento článek je potřeba k provedení těchto věcí:**

- Databáze Azure SQL. Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Změna úrovně služby osy a výkonu databáze


Otevřete zásuvné databáze SQL databáze, kterou chcete změnit měřítko nahoru nebo dolů:

1.  V [Azure portál](https://portal.azure.com), klikněte na **Další služby** > **SQL databáze**.
2.  Klikněte na databázi, kterou chcete změnit.
3.  Na zásuvné **SQL databáze** klikněte na **ceny osy (měřítko DTUs)**:

    ![ceny osy][1]

1.  Zvolte nové osy a klikněte na **Výběr**:

    Kliknutím **Vyberte** odešle měřítko požadavek na změnu ceny osy. V závislosti na velikosti databáze operaci měřítko chvíli trvat k dokončení (viz informace v horní části tohoto článku).

    > [AZURE.NOTE] Změna databáze ceny osy nezmění velikost maximální databáze. Databáze maximální velikosti můžete změnit [Jazyce Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) nebo [Powershellu](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Vyberte ceny osy][2]

3.  Klikněte na ikonu upozornění (zvonu) v pravém horním rohu:

    ![oznámení][3]

    Klikněte na oznámení text otevřete podokno podrobností kde navíc přehledně uvidíte stav.




## <a name="next-steps"></a>Další kroky

- Změňte maximální velikost databáze pomocí [Jazyce Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) nebo [Powershellu](https://msdn.microsoft.com/library/mt619433.aspx).
- [Změna velikosti a](sql-database-elastic-scale-get-started.md)
- [Připojení a dotaz SQL databáze pomocí SSMS](sql-database-connect-query-ssms.md)
- [Export databáze Azure SQL](sql-database-export.md)

## <a name="additional-resources"></a>Další zdroje informací

- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
