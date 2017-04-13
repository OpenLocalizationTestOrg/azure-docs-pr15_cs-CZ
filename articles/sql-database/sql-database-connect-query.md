<properties
    pageTitle="Připojení k databázi SQL pomocí dotazu C# | Microsoft Azure"
    description="Napište program v jazyce C# k vytváření dotazů a připojení k databázi SQL. Informace o IP adresy připojovací řetězec, zabezpečené přihlášení a bezplatné aplikace Visual Studio."
    services="sql-database"
    keywords="c# databázových dotazů, c# dotazu, připojení k databázi SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Připojení k databázi SQL pomocí aplikace Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Aplikace Excel](sql-database-connect-excel.md)

Informace o připojení k databázi Azure SQL ve Visual Studiu. 

## <a name="prerequisites"></a>Zjistit předpoklady pro


Připojení k databázi SQL pomocí aplikace Visual Studio, musíte: 


- Databáze SQL pro připojení k. Tento článek používá ukázkovou databázi **AdventureWorks** . Ukázkové databázi AdventureWorks získáte v tématu [Vytvoření ukázkovou databázi](sql-database-get-started.md).


- Visual Studio 2013 aktualizace 4 (nebo novější). Microsoft poskytuje teď Visual Studio komunity *zdarma*.
 - [Stažení aplikace Visual Studio komunity](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Další možnosti pro uvolnit Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Otevřete aplikaci Visual Studio z portálu Microsoft Azure


1. Přihlaste se k [portálu Azure](https://portal.azure.com/).

2. Klikněte na **Další služby** > **SQL databáze**
3. Otevřete zásuvné databázi **AdventureWorks** vyhledáním a kliknete na databázi *AdventureWorks* .

6. Klikněte na tlačítko **Nástroje** v horní části zásuvné databáze:

    ![Nový dotaz. Připojení k databázi SQL serveru: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klikněte na tlačítko **Otevřít v aplikaci Visual Studio** (v případě potřeby Visual Studiu, klikněte na odkaz ke stažení):

    ![Nový dotaz. Připojení k databázi SQL serveru: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio otevře se okno **připojit k serveru** už nastavení pro připojení k serveru a databázi, kterou jste vybrali v portálu.  (Klikněte na **Možnosti** můžete ověřit, že je nastavený připojení k databázi správné.) Zadejte heslo správce serveru a klikněte na **Připojit**.


    ![Nový dotaz. Připojení k databázi SQL serveru: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Pokud nemáte sadu pravidel brány firewall pro IP adresu počítače, se zobrazí zpráva *se nemůže připojit* tady. Pokud chcete vytvořit pravidlo brány firewall, přečtěte si téma [Konfigurace pravidlo brány firewall na úrovni serveru databáze SQL Azure](sql-database-configure-firewall-settings.md).


9. Po úspěšném připojení, otevře se okno **Průzkumníka objektu SQL serveru** s připojení k databázi.

    ![Nový dotaz. Připojení k databázi SQL serveru: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Spuštění dotazu ukázka

Teď můžeme jste připojení k databázi, následující postup k tomu jednoduchého dotazu:

2. Klikněte pravým tlačítkem myši databázi a potom vyberte **Nový dotaz**.

    ![Nový dotaz. Připojení k databázi SQL serveru: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Okna aplikace Microsoft query zkopírujte a vložte následující kód.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klikněte na tlačítko **Spustit** spusťte dotaz:

    ![Úspěšné. Připojení k databázi SQL serveru: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Další kroky

- Otevření SQL databáze ve Visual Studiu používá SQL Server Data Tools. Další informace najdete v tématu [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Připojení k databázi SQL pomocí kódu, najdete v článku [připojení k databázi SQL pomocí .NET (C#)](sql-database-develop-dotnet-simple.md).



