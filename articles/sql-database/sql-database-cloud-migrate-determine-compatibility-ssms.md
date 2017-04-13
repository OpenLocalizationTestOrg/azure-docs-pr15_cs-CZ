<properties
   pageTitle="Umožňuje určit databáze SQL kompatibility před migrací k databázi SQL Azure SQL Server Management Studio | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze, databáze SQL kompatibilita dat osy aplikace exportem"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Umožňuje určit databáze SQL kompatibility před migrací k databázi SQL Azure SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Poradce při upgradu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
V tomto článku, který jste zjistěte, jak zjistit, zda databáze SQL serveru je kompatibilní migrovat do databáze SQL pomocí Průvodce aplikace exportovat osy dat v SQL Server Management Studio.

## <a name="using-sql-server-management-studio"></a>Použití SQL Server Management Studio

1. Zkontrolujte, jestli máte nejnovější verzi SQL Server Management Studio. Nové verze Management Studio se automaticky aktualizují měsíční zůstat synchronizace s aktualizacemi portálu Azure.

     > [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otevřete Management Studio a připojení k databázi zdroje v prohlížeči objektů.
3. Klikněte pravým tlačítkem myši zdrojové databáze v prohlížeči objektů, přejděte k **úkolům**a klikněte na **Exportovat Data osy Application...**

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. V Průvodci exportem klikněte na tlačítko **Další**a potom na kartě **Nastavení** konfigurace exportovat do BACPAC soubor uložit do umístění na místním disku nebo objektů blob Azure. Pokud máte problémy s kompatibilitou žádné databáze, je BACPAC soubor uložený. Pokud jsou problémy s kompatibilitou, se zobrazí na konzole.

    ![Export nastavení](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Pokud chcete přeskočit, export dat, klikněte na **kartu Upřesnit** a zrušte zaškrtnutí políčka **Vybrat vše** . Naším cílem je v tomto okamžiku pouze k testování kompatibility.

    ![Export nastavení](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Klikněte na tlačítko **Další** a potom klikněte na **Dokončit**. Problémy s kompatibilitou databáze, případně se zobrazí po Průvodce ověří schéma.

    ![Export nastavení](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Pokud bez chyb se zobrazí, je kompatibilní databáze a budete připravení k migraci. Pokud máte chyby, budete muset vyřešit. Chyby zobrazíte kliknutím na **chyby** pro **Validating schéma**. 
    ![Export nastavení](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Pokud *. Vygeneruje se úspěšně BACPAC soubor, pak je kompatibilní se službou databáze SQL databáze a budete připravení k migraci.

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Řešení problémů s kompatibilitou migrace databáze](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrace kompatibilní databáze SQL serveru k SQL databázi](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
