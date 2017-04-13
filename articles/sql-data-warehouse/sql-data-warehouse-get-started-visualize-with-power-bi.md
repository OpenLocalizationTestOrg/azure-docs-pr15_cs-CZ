<properties
   pageTitle="Vizualizace dat s Power BI Microsoft Azure SQL datový sklad"
   description="Vizualizace SQL datový sklad dat s Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Vizualizace dat s Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Výukové Azure počítače](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SqlCmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tento kurz se dozvíte, jak používat Power BI pro připojení k SQL datový sklad a vytvářet několika základní vizualizace.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Zjistit předpoklady pro

Můžete přecházet mezi tohoto kurzu, budete potřebovat:

- Datový sklad SQL předem s databází AdventureWorksDW načíst. Zřízení to, najdete v tématu [Vytvoření datový sklad SQL][] a zvolte načtení ukázkových dat. Pokud už máte datový sklad, ale nebudete mít ukázková data, můžete [načtení ukázkových dat ručně][].


## <a name="1-connect-to-your-database"></a>1. připojení k databázi

Otevřete Power BI a připojení k databázi AdventureWorksDW:

1. Přihlaste se k [portálu Azure][].
2. Klikněte na **SQL databáze** a vyberte databázi AdventureWorks SQL datový sklad.

    ![Vyhledání databáze][1]

3. Kliknutím na tlačítko "Otevřený v Power BI".

    ![Tlačítko Power BI][2]

4. Teď byste měli vidět stránce připojení SQL datový sklad zobrazení webovou adresu vaší databáze. Klikněte na další.

    ![Připojení Power BI][3]

6. Zadejte svoje uživatelské jméno Azure SQL serveru a heslo a bude úplně propojeno databáze SQL datový sklad.

    ![Přihlašovací Power BI][4]

7. Jakmile jste se přihlásili k Power BI, klikněte na datovou sadu AdventureWorksDW na levém zásuvné. Otevře se databáze.

    ![Otevřít AdventureWorksDW Power BI][5]



## <a name="2-create-a-report"></a>2. vytvoření sestavy

Teď jste připraveni analýza AdventureWorksDW ukázkových dat pomocí Power BI. Abyste mohli provést analýzu, má AdventureWorksDW zobrazení s názvem AggregateSales. Toto zobrazení obsahuje několik klíčových metrik pro analýzu prodejem společnosti.

1. Vytvoření mapy částka prodeje podle PSČ, v podokně pravé pole, klikněte na zobrazení AggregateSales rozbalte. Klikněte na PSČ a SalesAmount je vyberte.

    ![Power BI vyberte AggregateSales][6]

    Power BI automaticky rozpozná to je zeměpisné údaje a v mapě za vás.

    ![Power BI mapu][7]

2. Tento krok vytvoří sloupcový graf, který zobrazuje množství prodeje za příjem zákazníka. Pokud chcete vytvořit přejděte do zobrazení rozbaleného AggregateSales. Klikněte na SalesAmount pole. Přetáhněte pole zákazníka příjem vlevo a vložíte do osy.

    ![Power BI vyberte osy][8]

    Pruhový graf jsme přesunuli nad vlevo.

    ![Power BI panelem][9]

3. Tento krok vytvoří spojnicový graf zobrazující prodej-částka za datum objednávky. Pokud chcete vytvořit přejděte do zobrazení rozbaleného AggregateSales. Klikněte na SalesAmount a DatumObjednávky. Vizualizace sloupce klikněte na ikonu spojnicový graf; Toto je na první ikonu v druhém řádku pod položkou vizualizace.

    ![Power BI vyberte spojnicový graf][10]

    Sestavy zobrazující tři různé vizualizace dat Teď máte.

    ![Power BI řádku][11]

Kdykoli můžete uložit průběhu kliknutím na **soubor** a vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
Teď můžeme můžete udělíte chvíli, teplé s ukázkovými daty, najdete v článku Jak [vývoj][], [načtení][]nebo [migrace][]. Nebo se podívejte na [webu Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrace]: sql-data-warehouse-overview-migrate.md
[Můžete vyvíjet]: sql-data-warehouse-overview-develop.md
[načtení]: sql-data-warehouse-overview-load.md
[načtení ukázkových dat ručně]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Vytvořit datový sklad SQL]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portálu]: https://portal.azure.com/
[Web Power BI]: http://www.powerbi.com/
