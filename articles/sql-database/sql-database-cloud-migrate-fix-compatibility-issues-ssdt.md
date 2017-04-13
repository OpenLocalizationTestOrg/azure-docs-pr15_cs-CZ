<properties
   pageTitle="Řešení problémů s kompatibilitou databáze SQL serveru před migrací k SQL databázi | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze, kompatibilita, SQL Azure Průvodce přenesením SSDT"
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

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Migrovat databázi SQL serveru k databázi Azure SQL pomocí služby SQL Server Data Tools for Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Poradce při upgradu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

V tomto článku se naučíte rozpoznávání a oprava problémů kompatibility databáze SQL serveru pomocí SQL Server Data Tools for Visual Studio před migrací k databázi SQL Azure.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Použití SQL Server Data Tools for Visual Studio

SQL Server Data Tools for Visual Studio ("SSDT") importujete pomocí schématu databáze do projekt aplikace Visual Studio databáze pro analýzu. A analyzujte data, zadat platformu pro projekt jako V12 SQL databáze a pak vytvoříte projektu. Pokud sestavení není úspěšné, databáze je kompatibilní. Pokud sestavení nezdaří, můžete vyřešit chyby ve SSDT (nebo na některou z dalších nástrojů popisované v tomto tématu). Po úspěšném sestavení projektu, můžete publikovat zpět jako kopii databáze zdroje. Pak můžete porovnat funkce dat v SSDT zkopírujte data ze zdrojové databáze k databázi SQL Azure V12 kompatibilní. Pak můžete migrovat aktualizované databázi. Tuto možnost použít, stáhněte si [nejnovější verzi SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![Diagram VSSSDT migrace](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Pokud jen schématu migrace se požaduje, schématu publikováním přímo z aplikace Visual Studio přímo k databázi SQL Azure. Tento způsob použijte, když schématu databáze vyžaduje další změny než můžete přistupovat pomocí Průvodce přenesením samostatně.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Zjišťování problémů s kompatibilitou pomocí SQL Server Data Tools for Visual Studio
   
1.  Otevřete **Průzkumníka objektu SQL serveru** ve Visual Studiu. Připojení k instanci systému SQL Server obsahující Probíhá migrace databáze pomocí **Přidat SQL Server** . Databáze v prohlížeči objektů, klikněte pravým tlačítkem myši databázi a vyberte **Vytvořit nový projekt...**     
    
    ![Nový projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Konfigurace nastavení importu k **importu s rozsahem aplikace pouze objekty**. Zrušte zaškrtnutí možnosti importu následující: uváděný přihlášení, oprávnění a nastavení databáze.    

    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Klikněte na tlačítko **Spustit** import databáze a vytvoření projektu se souborem T-SQL skriptu pro jednotlivé objekty v databázi. Soubor skriptu vnořené ve složkách v rámci projektu.    

    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  V okně Průzkumník Visual Studio klikněte pravým tlačítkem myši projektu databáze a vyberte vlastnosti. Na stránce **Nastavení projektu** konfigurace cílové platformy na Microsoft Azure SQL databáze V12.    
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Klikněte pravým tlačítkem myši projektu a vyberte **vytvořit** pro vytváření projektu.    
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  **Seznam chyb** se zobrazí každý nekompatibilita. V tomto případě uživatelské jméno NT AUTHORITY\NETWORK služeb není kompatibilní. Takové alternativní řešení je kompatibilní, můžete ji komentář nebo ho odebrat (a adresa důsledky tento přihlášení a roli odebráním řešení databáze).     
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Řešení problémů s kompatibilitou pomocí SQL Server Data Tools for Visual Studio

1.  Poklepejte na první skript, který chcete otevřít skript v okně dotazu a komentáře se skript a potom spusťte skript.     
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Tento postup opakujte pro každou skript s kompatibilitou dokud zůstat žádná chybová zpráva.    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Po bez chyb databázi projektu klikněte pravým tlačítkem myši a vyberte **Publikovat**. Vytvoření a publikování kopii zdrojové databáze (důrazně doporučujeme používat kopii, alespoň původně).     
 - Před publikováním, v závislosti serveru SQL Server verze zdroje (starší než SQL Server 2014), budete muset obnovit projektu platformu povolit nasazení.     
 - Při migraci starší databáze SQL serveru, ne pro nové všechny funkce projektu, které nejsou podporovány ve zdroji serveru SQL Server do migrovat databázi novější verzi serveru SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  V Průzkumníku objektu SQL serveru klikněte pravým tlačítkem myši zdrojové databáze a klikněte na **Porovnání datových**. Porovnání projektu na původní databáze pomůže pochopit, jaké někdo udělal nějaké změny pomocí průvodce. Vyberte si Azure SQL V12 verzi databáze a klikněte na **Dokončit**.    
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Zkontrolovat rozdíly zjišťování a potom klikněte na **Aktualizovat cílové** k migraci dat ze zdrojové databáze do databáze Azure SQL V12.     
    
    ![alternativní text](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Zvolte způsob nasazení. V tématu [migrace kompatibilní databáze SQL serveru k SQL databázi.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
