<properties
   pageTitle="Migrace databáze SQL serveru k SQL databázi pomocí nasazení databáze Microsoft Azure databázi Průvodce | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze, Průvodce databáze Microsoft Azure"
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
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Migrace databáze SQL serveru k SQL databázi pomocí nasazení databázi Průvodce databáze Microsoft Azure


> [AZURE.SELECTOR]
- [Průvodce migrací SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Export do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importovat ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Nasazení databázi Průvodce databázi Microsoft Azure SQL Server Management Studio migruje [kompatibilní databáze SQL serveru](sql-database-cloud-migrate.md) přímo do vaší databáze SQL Azure serveru.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Použít databázi nasazení průvodce databáze Microsoft Azure

> [AZURE.NOTE] Následující postup předpokládá, že máte [zřízení databáze SQL serveru](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Zkontrolujte, jestli máte nejnovější verzi SQL Server Management Studio. Nové verze Management Studio se automaticky aktualizují měsíční zůstat synchronizace s aktualizacemi portálu Azure.

    > [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otevřete Management Studio a připojení k databázi SQL serveru k poštovním v prohlížeči objektů.
3. Klikněte pravým tlačítkem myši na databázi v Průzkumníku objektu, přejděte k **úkolům**a klikněte na **Nasazení databáze Microsoft Azure SQL databáze...**

    ![Nasazení Azure v nabídce úkoly](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  V Průvodci nasazení klikněte na tlačítko **Další**a potom klikněte na **Připojit** k připojení k databázi SQL serveru.

    ![Nasazení Azure v nabídce úkoly](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. V části připojit k serveru dialogovým oknem zadejte informace o připojení k připojení k databázi SQL serveru.

    ![Nasazení Azure v nabídce úkoly](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Zadejte následující [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru, který tímto průvodcem během procesu migrace:

 - **Nový název databáze** 
 - **Z Microsoft Azure SQL databázi Edition** ([vrstvy služeb](sql-database-service-tiers.md))
 - **Maximální velikost databáze**
 - **Cíl služby** (úroveň výkonnosti)
 - **Dočasný název souboru**  

    ![Export nastavení](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Ukončete průvodce. V závislosti na velikosti a složitosti databáze nasazení až může trvat několik minut až počet hodin. Pokud tento průvodce detekuje problémy s kompatibilitou, zobrazení chybových zpráv na obrazovku a migrace neobjevuje. Pokyny k řešení problémů s kompatibilitou databáze přejděte na [řešení problémů s kompatibilitou databáze](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Pomocí Průzkumníka objektu, připojte k migrované databáze na serveru databáze SQL Azure.
8.  Na portálu Azure, zobrazení databáze a jeho vlastnosti.

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
