
<properties
   pageTitle="Export databáze SQL serveru do souboru BACPAC pomocí SQL Server Management Studio | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze export databáze, export do souboru BACPAC, Průvodce exportem aplikace osy dat"
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
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Export databáze SQL serveru do souboru BACPAC pomocí SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Tento článek ukazuje, jak exportovat do [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) soubor pomocí aplikace Průvodce exportem dat osy v SQL Server Management Studio v databázi SQL serveru. 

1. Zkontrolujte, jestli máte nejnovější verzi SQL Server Management Studio. Nové verze Management Studio se automaticky aktualizují měsíční zůstat synchronizace s aktualizacemi portálu Azure.

     > [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otevřete Management Studio a připojení k databázi zdroje v prohlížeči objektů.

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Klikněte pravým tlačítkem myši zdrojové databáze v prohlížeči objektů, přejděte k **úkolům**a klikněte na **Exportovat Data osy Application...**

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. V Průvodci exportem konfigurace exportovat do souboru BACPAC buď umístění na místním disku nebo do Azure objektů blob. Exportovaná BACPAC vždy obsahuje schématu dokončení databáze a ve výchozím nastavení, data ze všech tabulek. Pokud chcete, aby se vyloučila data z některých nebo všech tabulek, použijte kartu Upřesnit. Můžete třeba exportovat jen tu část dat pro referenční tabulky a ne ze všech tabulek.

***Důležité*** Pokud exportujete BACPAC k úložišti objektů blob Azure, použijte standardní úložiště. Import BACPAC z úložiště premium není podporován.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Import BACPAC k databázi SQL Azure pomocí SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Import BACPAC SqlPackage databáze Azure SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Import BACPAC do portálu Azure databáze SQL Azure](sql-database-import.md)
- [Import BACPAC prostředí PowerShell databáze Azure SQL](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
