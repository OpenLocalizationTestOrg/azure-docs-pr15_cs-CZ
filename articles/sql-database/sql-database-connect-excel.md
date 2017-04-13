<properties
    pageTitle="Připojit Excel k SQL databázi | Microsoft Azure"
    description="Zjistěte, jak se připojit k databázi Azure SQL v cloudu aplikaci Microsoft Excel. Import dat do Excelu pro vytváření sestav a zkoumání dat."
    services="sql-database"
    keywords="připojit excel k sql, import dat do aplikace excel"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Databáze SQL kurz: připojení k databázi Azure SQL aplikace Excel a vytvářet sestavy

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Aplikace Excel](sql-database-connect-excel.md)

Zjistěte, jak připojit Excel k databázi SQL v cloudu, abyste import dat a vytváření tabulek a grafů na základě hodnot v databázi. V tomto kurzu, který chcete nastavit připojení mezi Excelem a databázovou tabulku uložte soubor, který ukládá data a informace o připojení pro Excel a vytvořte kontingenční graf z hodnoty v databázi.

Než začnete, musíte databázi SQL Azure. Pokud nemáte jeden, najdete v článku [vytvoření první databáze SQL](sql-database-get-started.md) získat databáze s ukázkovými daty nahoru a spuštění za několik minut. V tomto článku od tohoto článku budete importovat ukázková data do Excelu, ale na vlastních datech, postupujte podobným způsobem.

Taky musíte kopii Excelu. Tento článek používá [Microsoft Excelu 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Připojení k databázi SQL aplikace Excel a vytvořte soubor odc

1.  Připojit Excel k SQL databázi, spusťte aplikaci Excel a vytvořte nový sešit nebo otevřete existující sešit aplikace Excel.

2.  V řádku nabídek v horní části stránky klikněte na **Data**, klikněte na **Z jiných zdrojů**a potom klikněte na **Ze serveru SQL Server**.

    ![Výběr zdroje dat: připojit Excel k SQL databázi.](./media/sql-database-connect-excel/excel_data_source.png)

    Otevře se Průvodce datovým připojením.

3.  V dialogovém okně **připojení k databázovému serveru** zadejte SQL databáze **název serveru** se chcete připojit ve formuláři <*webu název serveru*>**. database.windows.net**. Například **adworkserver.database.windows.net**.

4.  V části **přihlašovací pověření**klikněte na **použít následující uživatelské jméno a heslo**, zadejte **Uživatelské jméno** a **heslo** nastavení pro databáze SQL serveru, když jste ho vytvořili a klikněte na tlačítko **Další**.

    ![Zadejte název serveru a přihlašovací údaje](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] V závislosti na prostředí sítě stát, že nebudete moct připojit nebo kterou může dojít ke ztrátě připojení Pokud databázi SQL serveru nedovoluje adres IP adresa klienta. Přejděte na [portál Azure](https://portal.azure.com/), klikněte na položku SQL servery, klikněte na serveru, klikněte v části nastavení brány firewall a přidejte IP adresa klienta. Podrobnosti najdete v článku [jak nakonfigurovat nastavení brány firewall](sql-database-configure-firewall-settings.md) .

5. V dialogovém okně **Vybrat databázi a tabulku** vyberte databázi, kterým chcete pracovat s ze seznamu a potom klikněte na tabulky nebo zobrazení chcete pracovat s (zvolili jsme **vGetAllCategories**) a potom na tlačítko **Další**.

    ![Vyberte databázi a tabulku.](./media/sql-database-connect-excel/select-database-and-table.png)

    Otevře se dialogové okno **Uložit soubor datového připojení a dokončit** místo, kam zadáte informace o souboru Office databáze připojení (*.odc), který používá Excel. Můžete nechat výchozí hodnoty nebo upravit nastavení.

6. Ponechejte výchozí hodnoty, ale konkrétně zapište si **Název souboru** . **Popis**, **Popisný název**a **Klíčová slova pro hledání** pomoct vám i ostatní uživatelé nezapomeňte připojujete k a najděte připojení. Pokud chcete připojení informace uložené v soubor odc, takže můžete aktualizovat, když se k ní připojit a klikněte na tlačítko **Dokončit**, klikněte na **vždy se snaží použít tento soubor aktualizovat data** .

    ![Ukládání souboru sady odc](./media/sql-database-connect-excel/save-odc-file.png)

    Zobrazí se dialogové okno **importovat data** .

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Import dat do Excelu a vytvořte kontingenční graf
Teď, když jste připojili a vytvoření souboru s data a informace o připojení, kterou právě čtete importovat data.

1. V dialogovém okně **Importovat Data** vyberte možnost, kterou chcete pro prezentaci dat v listu a potom klikněte na **OK**. Zvolili jsme **kontingenčního grafu**. Můžete také vytvořit **Nový list** nebo **Přidat tahle data do datového modelu**. Další informace o datových modelů najdete v tématu [Vytvoření datového modelu v Excelu](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klikněte na **Vlastnosti** , které můžete prozkoumat informace o soubor odc, kterou jste vytvořili v předchozím kroku a zvolte možnosti pro aktualizaci dat.

    ![Výběr formátu data v Excelu](./media/sql-database-connect-excel/import-data.png)

    Listu se prázdný kontingenční tabulka a graf.

8. V seznamu **Pole kontingenční tabulky**vyberte všechny-políčka u polí, která chcete zobrazit.

    ![Konfigurace databáze sestavy.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Pokud chcete připojit k databázi jiných Excelových sešitů a listů, klikněte na **Data**, klikněte na **připojení**, klikněte na **Přidat**, vyberte připojení, který jste vytvořili v seznamu a klepněte na tlačítko **Otevřít**.
> ![Připojit z jiného sešitu](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak se [připojit k databázi SQL s SQL Server Management Studio](sql-database-connect-query-ssms.md) rozšířené dotazy a analýzy.
- Informace o výhodách [pružná fondů](sql-database-elastic-pool.md).
- Zjistěte, jak [vytvořit webovou aplikaci, ke kterému je připojen k SQL databázi na back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
