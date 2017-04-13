<properties
   pageTitle="Migrace databáze SQL serveru k databázi SQL Azure | Microsoft Azure"
   description="Databázi Microsoft Azure SQL databáze nasadit, migrace databáze, import z databáze, export databáze, Průvodce přenesením"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Import z BACPAC k SQL databázi pomocí SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portálu](sql-database-import.md)
- [Prostředí PowerShell](sql-database-import-powershell.md)

Tento článek ukazuje, jak importovat ze souboru [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) k databázi SQL pomocí Průvodce aplikace exportovat osy dat v SQL Server Management Studio.

> [AZURE.NOTE] Následující postup předpokládá, máte už zřízení logické instanci aplikace Azure SQL a mít informace o připojení k dispozici.

1. Zkontrolujte, jestli máte nejnovější verzi SQL Server Management Studio. Nové verze Management Studio se automaticky aktualizují měsíční zůstat synchronizace s aktualizacemi portálu Azure.

     > [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Připojení k databázi SQL Azure serveru, klikněte pravým tlačítkem myši na složku **databáze** a klikněte na **Import dat osy Application...**

    ![Import položka nabídky aplikace osy dat](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Vytvářet databáze v databázi SQL Azure, import souboru BACPAC z místního disku a vyberte účet Azure úložiště a kontejner, do které jste nahráli BACPAC soubor.

    ![Nastavení importu](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Při importu BACPAC z úložiště objektů blob Azure pomocí standardní úložiště. Import BACPAC z úložiště premium není podporován.

4.  Zadejte **Nový název databáze** pro databáze na databáze SQL Azure, nastavte **Edition z databázi Microsoft Azure SQL** (vrstvy služeb), **maximální velikost databáze**a **Služby cíle** (úroveň výkonnosti).

    ![Nastavení databáze](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Klikněte na tlačítko **Další** a potom klikněte na **Dokončit** naimportujte BACPAC souboru nové databáze na serveru databáze SQL Azure.

6. Pomocí Průzkumníka objektu, připojte k migrované databáze na serveru databáze SQL Azure.

6.  Na portálu Azure, zobrazení databáze a jeho vlastnosti.

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
