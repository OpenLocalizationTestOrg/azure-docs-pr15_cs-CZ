<properties
    pageTitle="Kopírování dat z úložiště objektů Blob k SQL databázi | Microsoft Azure"
    description="Tento kurz se dozvíte, jak můžete kopírovat aktivity v Azure Data Factory kanálu ke kopírování dat z úložiště objektů Blob k SQL databázi."
    Keywords="kulatý sql, úložiště objektů blob kopírovat data"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Kopírování dat z úložiště objektů Blob k SQL databázi pomocí Data Factory 
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopírování Průvodce](data-factory-copy-data-wizard-tutorial.md)
- [Azure portálu](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure správce prostředků šablony](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [ROZHRANÍ REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [ROZHRANÍ API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


V tomto kurzu vytvoříte factory dat s kanálu ke kopírování dat z úložiště objektů Blob k SQL databázi.

Aktivity kopírovat provede přesun dat v Azure Data Factory. Je technologii globálně dostupná služba, která můžete přesouvat data mezi různých úložiště dat zabezpečené spolehlivý a scalable způsobem. Článek [Aktivity přesun dat](data-factory-data-movement-activities.md) najdete v článku podrobné informace o aktivitě kopírovat.  

> [AZURE.NOTE] Podrobné informace o službu Data Factory najdete v článku [Úvod k Azure Data Factory](data-factory-introduction.md) .

##<a name="prerequisites-for-the-tutorial"></a>Požadavky pro kurz
Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**.  Pokud nemáte předplatné, můžete vytvořit bezplatný účet zkušební jenom několika minut. Naleznete v článku [Bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/) podrobnosti.
- **Účet azure úložiště**. Úložiště objektů blob použijete jako **zdroj** dat úložiště v tomto kurzu. Pokud nemáte účet Azure úložiště, najdete v článku [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) kroky k jeho vytvoření.
- **Databáze azure SQL**. Databáze Azure SQL použijete jako **cílové** datový úložiště v tomto kurzu. Pokud nemáte databáze Azure SQL, využívající v tomto kurzu, přečtěte si, [jak vytvořit a nakonfigurovat databázi SQL Azure](../sql-database/sql-database-get-started.md) a vytvořte si ho.
- **SQL Server 2012/2014 nebo Visual Studio 2013**. SQL Server Management Studio nebo Visual Studio slouží k vytvoření databáze ukázkové a k zobrazení Výsledná data v databázi.  

## <a name="collect-blob-storage-account-name-and-key"></a>Shromáždit název účtu úložiště objektů blob a klíčem 
Potřebujete název účtu a účtu klíč účtu Azure úložiště k tomuto kurzu. Poznamenejte si **název účtu** a **klíč účtu** pro váš účet Azure úložiště.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Další služby** v levé nabídce a vyberte **Účty úložiště**.

    ![Procházet - úložiště účty](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. V zásuvné **Úložiště účty** vyberte **účet Azure úložiště** , který chcete použít v tomto kurzu.
4. Vyberte odkaz **přístupové klávesy** klikněte v části **Nastavení**.
5.  Klikněte na tlačítko **Kopírovat** (obraz) vedle textového pole **název účtu úložiště** a uložit a vložit ho jinam, postupujte (například: z textového souboru).
6. Opakujte předchozí krok kopírovat nebo Poznámka dolů **klíč1**.
    
    ![Úložiště přístupová klávesa](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Zavřete všechny listy kliknutím na **X**.

## <a name="collect-sql-server-database-user-names"></a>Shromáždit SQL serveru, databázi, uživatelská jména
Názvy Azure SQL server databáze a uživatele k tomuto kurzu potřebujete. Poznamenejte si názvy **serveru**, **databázi**a **uživatele** pro databázi Azure SQL.

1. V **Azure portál**klikněte na **Další služby** na levé straně a vyberte **SQL databáze**.
2. V **SQL databáze zásuvné**vyberte **databázi** , kterou chcete použít v tomto kurzu. Poznamenejte si **název databáze**.  
3. V **SQL databázi** zásuvné klikněte v části **Nastavení** **Vlastnosti** .
4. Poznámka: hodnoty **Název serveru** a **PŘIHLAŠOVACÍ správce serveru**.
5. Zavřete všechny listy kliknutím na **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Povolit přístup k serveru SQL server Azure služby 
Zkontrolujte, **že nastavení **Povolit přístup ke službám Azure** zapnuto serveru Azure SQL tak, aby služby Data Factory přístup k serveru Azure SQL** . Ověřit a zapněte tuto možnost, proveďte následující kroky:

1. Klikněte na **Další služby** centrální na levé straně a klikněte na položku **SQL servery**.
2. Vyberte server a v části **Nastavení**klikněte na **Brána Firewall** . 
4. V **nastavení brány Firewall** zásuvné klikněte **na** **Povolit přístup ke službám Azure**.
5. Zavřete všechny listy kliknutím na **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Příprava úložiště objektů Blob a databáze SQL 
Teď Příprava úložišti objektů blob Azure a databáze Azure SQL kurzu provedením následujících kroků:  

1. Spusťte Poznámkový blok, vložte následující text a uložit jako **emp.txt** **C:\ADFGetStarted** složky na pevném disku.

        John, Doe
        Jane, Doe

2. Pomocí nástrojů, jako jsou [Průzkumníka úložišť Azure](https://azurestorageexplorer.codeplex.com/) vytvořit kontejner **adftutorial** a nahrajte soubor **emp.txt** do kontejneru.

    ![Průzkumník Azure úložiště. Kopírování dat z úložiště objektů Blob k SQL databázi](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Použijte tento skript SQL k vytvoření **emp** tabulky v databázi SQL Azure.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Pokud máte SQL Server 2012/2014 na počítači nainstalovaný:** postupujte podle pokynů z [Krok 2: připojení k databázi SQL Správa Azure SQL databáze pomocí SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) článek připojení k serveru Azure SQL a spouštění skript SQL. Tento článek používá [klasický Azure portál](http://manage.windowsazure.com), ne na [novém portálu Azure](https://portal.azure.com)konfigurace brány firewall pro server Azure SQL.

    Pokud váš klient není povoleno pro přístup k serveru Azure SQL, musíte konfigurace brány firewall pro server Azure SQL umožníte přístup z počítače (IP adresa). Podívejte se [v tomto článku](../sql-database/sql-database-configure-firewall-settings.md) postup konfigurace brány firewall pro server Azure SQL.

Jste dokončili požadavky. Můžete vytvořit factory dat pomocí jedné z následujících způsobů. Klikněte na jednu z karty začátku nebo na následující odkazy provádět kurzu.     

- [Azure portálu](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [ROZHRANÍ REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [ROZHRANÍ API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kopírování Průvodce](data-factory-copy-data-wizard-tutorial.md)
