<properties
   pageTitle="Načíst data do SQL datový sklad pomocí společnosti Redgate dat platformy Studio | Microsoft Azure"
   description="Naučte se používat na Redgate dat platformy Studio dat skladovými scénářích."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Načtení dat s Redgate dat platformy Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Tento kurz se dozvíte, jak používat [Na Redgate dat platformy Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) k přesunutí dat z místních SQL Server Azure SQL datový sklad. Data platformy Studio slouží k použití nejvhodnější opravy kompatibility a optimalizace, tak, aby byl nejrychlejším způsobem, jak začít pracovat s SQL datový sklad.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) je dlouhodobými partnera společnosti Microsoft, který nabízí různé nástroje systému SQL Server. Tato funkce ve studiu platformy dat byly provedeny k dispozici volně obchodní a jiných obchodních.

## <a name="before-you-begin"></a>Než začnete
### <a name="create-or-identify-resources"></a>Vytvoření nebo identifikaci zdroje

Před zahájením tohoto kurzu, potřebujete:

- **Místní databáze systému SQL Server**: požadovaná data importovat do SQL datový sklad musí pocházet z místních SQL serveru (verze 2008 R2 nebo novější). Data platformy Studio nelze importovat data přímo z databáze SQL Azure nebo soubory ve formátu RTF.
- **Účet Azure úložiště**: Data platformy Studio uspořádání dat v úložišti objektů Blob Azure před načtením do SQL datový sklad. Účet úložiště nutné použít místo "Klasické" nasazení modelu nasazení modelu "Správce prostředků" (výchozí). Pokud nemáte účet úložiště, přečtěte si, jak vytvořit účet úložiště. 
- **Datový sklad SQL**: Tento kurz přesune data z místních SQL serveru SQL datový sklad, musíte mít datový sklad online. Pokud už nemáte datový sklad, přečtěte si, jak vytvořit datový sklad SQL Azure.

> [AZURE.NOTE] Pokud účet úložiště a datový sklad vytvářejí ve stejné oblasti lepší výkon.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Krok 1: Přihlášení k aplikaci Studio platformy dat s účtem Azure
Otevřete webový prohlížeč a přejděte na web [Studio platformy Data](https://www.dataplatformstudio.com/) . Přihlaste se pomocí stejného Azure účet, který jste použili k vytvoření úložiště účet a datový sklad. Pokud e-mailové adresy přidružené k pracovní nebo školní účet a účet Microsoft, třeba zvolit účet, který má oprávnění k přístupu k prostředkům.

> [AZURE.NOTE] Pokud je to poprvé pomocí Studio platformy dat, budete vyzváni k aplikace oprávnění ke správě Azure prostředků.

## <a name="step-2-start-the-import-wizard"></a>Krok 2: Spuštění Průvodce importem
Na hlavní obrazovce DPS vyberte importu Azure SQL datový sklad odkaz spusťte Průvodce importem.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Krok 3: Instalace brána Studio platformy dat
Připojení k databázi SQL serveru na pracovišti, budete potřebovat k instalaci DPS brány. Brána je agent klienta, který umožňuje přístup k místním prostředí, extrahuje data a odešle ke svému účtu úložiště. Data nikdy prochází Redgate na serverech. Chcete-li nainstalovat bránu:

1.  Klikněte na odkaz **Vytvoření brány**
2. Stažení a instalace brány pomocí ujednaných instalačního programu

![][2]

> [AZURE.NOTE] Brány možné nainstalovat na zařízení s přístupem k síti zdrojové databáze SQL serveru. Má přístup k databázi serveru SQL Server pomocí ověřování Windows pomocí přihlašovacích údajů aktuálního uživatele.

Po instalaci stav brány se změní na připojit a vyberte další.

## <a name="step-4-identify-the-source-database"></a>Krok 4: Určení zdrojové databáze
Do textového pole *Zadejte název serveru* zadejte název serveru, který je hostitelem vaší databáze a vyberte **Další**. V rozevírací nabídce vyberte databázi, který chcete importovat data z.

![][3]

DPS vyhodnotí databázi vybrané pro tabulky, které chcete importovat. Ve výchozím nastavení importuje DPS všech tabulek v databázi. Můžete zaškrtněte nebo zrušte tabulek rozbalením spojení všech tabulek. Vyberte Přechod vpřed na tlačítko Další.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Krok 5: Zvolte účet úložiště připravení data
DPS zobrazí výzvu k zadání umístění fáze data. Zvolte existujícího účtu úložiště u předplatného a vyberte **Další**.

> [AZURE.NOTE] DPS vytvoříte nové kontejneru objektů blob ve zvoleném úložiště účtu a použití různých složky pro každého importu.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Krok 6: Vyberte datový sklad
Potom vyberte online databázi [SQL Azure datový sklad](http://aka.ms/sqldw) k importu dat do. Po výběru databáze, budete muset zadat pověření připojení k databázi a potom vyberte **Další**.

![][5]

> [AZURE.NOTE] DPS sloučí zdrojové tabulky dat datový sklad. Pokud název tabulky vyžadují přepsat existující tabulky v datový sklad upozorní DPS. Můžete volitelně odstranit existující objekty v datový sklad zaškrtnutím odstranit všechny existující objekty před importem.

## <a name="step-7-import-the-data"></a>Krok 7: Import dat
DPS potvrzuje, že chcete importovat data. Stačí klikněte na tlačítko Spustit import zahájíte importu dat.

![][6]

DPS zobrazí vizualizace, která zobrazuje průběh extrahování a uložení dat ze serveru SQL Server na pracovišti a průběhu importovat do SQL datový sklad.

![][7]

Po dokončení importu DPS zobrazuje přehled importu dat a změnu sestavy kompatibility problémů, které byly provedeny.

![][8]

## <a name="next-steps"></a>Další kroky
Zkoumání dat v SQL datový sklad, začněte tím, zobrazení:

- [Dotaz Azure SQL datový sklad (Visual Studio)][]
- [Vizualizace dat s Power BI][]

Další informace o společnosti Redgate dat platformy Studio:

- [Přejděte na domovskou stránku DPS](http://www.dataplatformstudio.com/)
- [Podívejte se na ukázku DPS na Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Základní informace o dalších způsobech migrace a načtení dat v SQL datový sklad najdete tady:

- [Migrace řešení pro systém SQL Data Warehouse][]
- [Načtení dat do datový sklad SQL Azure](./sql-data-warehouse-overview-load.md)

Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Dotaz Azure SQL datový sklad (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Vizualizace dat s Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrace řešení pro systém SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md