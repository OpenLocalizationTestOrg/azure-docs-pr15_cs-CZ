<properties
   pageTitle="Dotaz Azure SQL datový sklad (Visual Studio) | Microsoft Azure"
   description="Dotaz SQL datový sklad s Visual Studia."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Dotaz Azure SQL datový sklad (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Výukové Azure počítače](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SqlCmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Pomocí aplikace Visual Studio dotazu Azure SQL datový sklad za několik minut. Tento způsob používá příponu SQL Server Data nástroje (SSDT) ve Visual Studiu. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli používat tohoto kurzu, potřebujete:

+ Existující SQL datový sklad. Chcete-li vytvořit průzkum, v tématu [Vytvoření datový sklad SQL][].
+ SSDT for Visual Studio. Pokud máte Visual Studiu, pravděpodobně už máte to. Pokyny k instalaci a možnosti najdete v tématu [Instalace Visual Studia a SSDT][].
+ Plně kvalifikovaný název serveru SQL. To najdete v tématu [připojení k SQL datový sklad][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. připojení k SQL datový sklad

1. Otevřete aplikaci Visual Studio 2013 nebo 2015.
2. Otevřete Průzkumníka objektu SQL serveru. K tomuto účelu vyberte **zobrazení** > **Průzkumník objektu SQL serveru**.

    ![Průzkumník objektu SQL serveru][1]

3. Klikněte na ikonu **Přidat SQL Server** .

    ![Přidání SQL serveru][2]

4. Vyplňte pole v části připojit k serveru okna.

    ![Připojení k serveru][3]

    - **Název serveru**. Zadejte **název serveru** předchozím kroku.
    - **Ověřování**. Vyberte **ověřování serveru SQL Server** nebo **integrované ověřování služby Active Directory**.
    - **Uživatelské jméno** a **heslo**. Pokud při výběru ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.
    - Klikněte na **Připojit**.

5. Pokud chcete prozkoumat, rozbalte Azure SQL serveru. Můžete zobrazit databází přiřazené k serveru. Rozbalte AdventureWorksDW zobrazíte tabulek v ukázkové databázi.

    ![Prozkoumání AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. ukázkový dotaz spustit

Teď vytvořit připojení k databázi Pojďme vytvořit dotaz.

1. Klikněte pravým tlačítkem myši databáze v Průzkumníku objektu SQL serveru.

2. Vyberte **Nový dotaz**. Otevře se nové okno dotazu.

    ![Nový dotaz][5]

3. Zkopírujte tento dotaz TSQL do okna dotazu:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Spusťte dotaz. K tomuto účelu kliknutím na zelenou šipku nebo použijte následující zkratku: `CTRL` + `SHIFT` + `E`.

    ![Spuštění dotazu][6]

5. Podívejte se na výsledky dotazu. V tomto příkladu jsou v tabulce FactInternetSales 60398 řádky.

    ![Výsledky dotazu][7]

## <a name="next-steps"></a>Další kroky

Teď se můžete připojit a dotaz, zkuste [vizualizace dat s PowerBI][].

Abyste mohli nakonfigurovat prostředí pro ověřování služby Azure Active Directory, najdete v článku [Ověřit SQL datový sklad][].

<!--Arcticles-->
[Připojení k SQL datový sklad]: sql-data-warehouse-connect-overview.md
[Vytvořit datový sklad SQL]: sql-data-warehouse-get-started-provision.md
[Instalace aplikace Visual Studio a SSDT]: sql-data-warehouse-install-visual-studio.md
[Ověření SQL datový sklad]: sql-data-warehouse-authentication.md
[vizualizace dat s PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
