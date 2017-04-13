<properties
   pageTitle="Migrace databáze SQL serveru k SQL databázi | Microsoft Azure"
   description="Zjistěte, Co takhle místního serveru SQL Server migrace databáze k databázi SQL Azure v cloudu. Otestovat funkce pro kompatibilitu před migrace databáze pomocí nástroje pro migraci z databáze."
   keywords="databáze migrace, migrace databáze sql serveru, nástroje pro migraci z databáze, migrovat databázi, migrovat databázi sql"
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

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Migrace databáze SQL serveru k SQL databázi v cloudu

V tomto článku se naučíte, jak migrovat místní SQL Server 2005 nebo novější databáze do databáze SQL Azure. V tomto procesu migrace databázi nejdřív migrujete schéma a dat z databáze SQL serveru ve vašem aktuální prostředí do SQL databáze. Pro úspěšné, existující databázi projít nejdřív testu kompatibility. Pomocí [V12 databáze SQL](sql-database-v12-whats-new.md)máte několik zbývající problémy s kompatibilitou, než problém týkající se operace úrovni serveru a křížově databáze. Databáze a aplikace, které jsou závislé na [částečně nebo nepodporované funkce](sql-database-transact-sql-information.md) potřebujete některé znovu konstrukce oprava těchto kompatibilitou před můžete migrovat databázi SQL serveru.

Migrace, jsou následující kroky lze provést:

- **Test pro kompatibilitu**: ověření kompatibility s [V12 databáze SQL](sql-database-v12-whats-new.md)databáze. 
- **Řešení problémů s kompatibilitou, případně**: Pokud neúspěšné ověření musí vyřešit chyby ověření.  
- **Provedení migrace** Jakmile je kompatibilní databáze, můžete k provedení migrace jednoho nebo několika způsoby. 

SQL Server nabízí několik způsobů provádění všech kroků. Tento článek obsahuje přehled o dostupných metod pro každý úkol. Následující obrázek znázorňuje kroky a metody.

  ![Diagram VSSSDT migrace](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Migrace databáze SQL serveru, včetně aplikace Microsoft Access, Sybase, MySQL Oracle a DB2 k databázi SQL Azure, najdete v článku [SQL Server migrace Assistant](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Nástroje pro migraci z databáze testování kompatibilitu databáze SQL serveru s databáze SQL

K testování SQL databáze problémů s kompatibilitou před spuštění migračního procesu databáze, použijte jeden z těchto způsobů:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Poradce při upgradu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT používá nejnovější funkce pro kompatibilitu pravidla ke zjištění kompatibilitou V12 databáze SQL. Pokud je zjištěno kompatibilitou, můžete opravit zjištěny problémy přímo v tomto nástroji. Tento způsob doporučuje se otestujte a vyřešit problémy s kompatibilitou V12 databáze SQL. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage je nástroj příkazového řádku, která testuje pro problémy kompatibility a vygeneruje sestavu obsahující problémy s kompatibilitou zjištěné. Pokud tento nástroj použít, zkontrolujte, jestli že používáte nejnovější verzi použít nejnovější funkce pro kompatibilitu pravidla. Zjištění chyb pomocí jiného nástroje musí opravit některé problémy s kompatibilitou zjištěné - SSDT doporučujeme.  
- [Průvodce aplikací pro Export dat osy v SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): Tento průvodce zjistí a ohlásí chyby na obrazovku. Pokud jsou zjištěny chyby, můžete pokračovat a dokončete migrace k SQL databázi. Zjištění chyb pomocí jiného nástroje musí opravit některé problémy s kompatibilitou zjištěné - SSDT doporučujeme.
- [Microsoft SQL Server 2016 Upgrade Advisor Preview](http://www.microsoft.com/download/details.aspx?id=48119): Tento nástroj samostatně, právě v náhledu, rozpozná a vytváří sestavu statistických s kompatibilitou V12 databáze SQL. Tento nástroj ještě nemá nejnovější funkce pro kompatibilitu pravidla. Pokud jsou zjištěny žádné chyby, můžete pokračovat a provedení migrace k SQL databázi. Zjištění chyb pomocí jiného nástroje musí opravit některé problémy s kompatibilitou zjištěné - SSDT doporučujeme. 
- [Průvodce migrací SQL Azure ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW je codeplex nástroj, který používá funkce pro kompatibilitu pravidla V11 databáze SQL Azure zjišťování kompatibilitou V12 databáze SQL Azure. Pokud je zjištěno kompatibilitou, můžete přímo v tomto nástroji pevných některé problémy. Tento nástroj zjistit nekompatibilita, není nutné opravit. Byla první databázi SQL Azure pomoc nástroje pro migraci k dispozici a aktivně komunity SQL Server nepodporuje. Tento nástroj můžete také dokončení migrace z v rámci nástroj sám. 

## <a name="fix-database-migration-compatibility-issues"></a>Řešení problémů s kompatibilitou migrace databáze

Pokud jsou zjištěny problémy s kompatibilitou, je nutné opravit před pokračováním migrace databáze SQL serveru. Existuje širokou škálu problémy s kompatibilitou, ke kterým může dojít, obojí v závislosti na verzi systému SQL Server ve zdrojové databáze a složitosti databáze migrujete. Starší verze SQL serveru mít více problémů s kompatibilitou. Použijte v následujících zdrojích, kromě cílových Internet vyhledávání pomocí vyhledávacího webu voleb:

- [Funkce databáze SQL serveru nepodporované v databázi SQL Azure](sql-database-transact-sql-information.md)
- [Funkce Engine ukončené databáze na serveru SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Funkce Engine ukončené databáze na serveru SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Funkce Engine ukončené databáze na serveru SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Funkce Engine ukončené databáze SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Funkce Engine ukončené databáze na serveru SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Kromě hledání na Internetu a používat tyto materiály, použijte [fóra komunity MSDN SQL Server](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) nebo [StackOverflow](http://stackoverflow.com/).

Použijte jeden z následujících databázové nástroje Migrace kvůli odstraňování problémů detekovaný:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Použití [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): můžete SSDT importu schématu databáze do SQL Server Data Tools for Visual Studio "SSDT") a vytvoření projektu pro nasazení V12 databáze SQL. Potom odstraňte všechny problémy s kompatibilitou zjištěné v SSDT. Až budete hotovi, synchronizovat změny zpět zdrojové databáze (nebo kopírovat zdrojové databáze. SSDT doporučuje se aktuálně otestujte a vyřešit problémy s kompatibilitou V12 databáze SQL. Kliknutím na odkaz pro [walk-through pomocí SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Použití [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): můžete SSMS provádět jazyce Transact-SQL příkazy oprava chyb detekovaný nástrojem pro jiné. Tento způsob je určen pro pokročilé uživatele k úpravám schématu databáze přímo v zdrojové databáze. 
- Pomocí [Průvodce přenesením SQL Azure ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): Pokud chcete použít SAMW, generovat skriptu jazyce Transact-SQL ze zdrojové databáze. Průvodce transformací skript, kdykoli je to možné, aby schématu kompatibilní s V12 databáze SQL. Až budete hotovi, SAMW můžete připojit k SQL databázi V12 spustit skript. Tento nástroj také analyzuje sledování soubory k určení problémy s kompatibilitou. Skript můžete vytvořit s pouze schéma nebo mohou obsahovat data ve formátu BCP.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migrace kompatibilní databáze SQL serveru k SQL databázi

Migrace kompatibilní databáze SQL serveru, Microsoft nabízí několik způsobů migrace různých scénářích. Metody, kterou zvolíte, závisí na vaší odchylky výpadek služeb, velikosti a složitosti databázi SQL serveru a připojení ke cloudové Microsoft Azure.  

> [AZURE.SELECTOR]
- [Průvodce migrací SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Export do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importovat ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Zvolte metodu migrace, první otázky pokládat je-li si můžete dovolit databázi mimo provozní během migrace. Migrace databáze během aktivní transakcí můžete mít za následek nekonzistence databáze a poškození možné databáze. Způsoby mnoho uvedení do nečinnosti databázi z zakázání připojení klientů k vytvoření [databáze snímek](https://msdn.microsoft.com/library/ms175876.aspx).

Migrace s minimálními prostoje, použijte, pokud splňuje požadavky na transakční replikace databáze [SQL serveru transakce replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md) . Případně můžete dovolit některé prostoje provádíte testovací migraci produkční databáze na pozdější přenesení, zvažte jednu z těchto tří způsobů:

- [Průvodce migrací SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): pro malé a střední databází migrace databázi kompatibilní SQL Server 2005 nebo vyšší je jednoduchá – stačí [Nasadit databáze Microsoft Azure databázi Průvodce](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) aplikaci SQL Server Management Studio.
- [Export do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) a [Importovat ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Pokud máte připojení výzvy (bez připojení, úzké šířce pásma nebo problémy vypršení časového limitu) a středně velké databází, použití [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru. Tímto způsobem export schéma SQL serveru a data do souboru BACPAC. Potom import souboru BACPAC do SQL databáze pomocí Data osy aplikace Průvodce exportem v SQL Server Management Studio nebo nástroje [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) příkazový řádek.
- Používat BACPAC BCP společně: [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souborů a [BCP](https://msdn.microsoft.com/library/ms162802.aspx) pro větší databáze dosáhnout pomocí větší paralelního zpracování pro zvýšení výkonu, i když se větší složitost. Tímto způsobem migrujte schématu a data samostatně.
 - [Export schématu jenom do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Import schéma jenom ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) do SQL databáze.
 - Extrahování dat do textových souborů a potom [paralelní zatížení](https://technet.microsoft.com/library/dd425070.aspx) tyto soubory do databáze SQL Azure pomocí [BCP](https://msdn.microsoft.com/library/ms162802.aspx) .

     ![SQL Server databáze migrace - migrovat databázi SQL do cloudu.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Další kroky

- [Microsoft SQL serveru 2016 Preview Poradce pro Upgrade](http://www.microsoft.com/download/details.aspx?id=48119)
- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Další zdroje informací

- [SQL databáze V12](sql-database-v12-whats-new.md)
[jazyce Transact-SQL částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
