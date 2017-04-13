<properties
   pageTitle="Načtení dat ze serveru SQL Server do Azure datový sklad SSIS (SQL) | Microsoft Azure"
   description="Se dozvíte, jak vytvořit balíček SQL Server Integration Services (SSIS) přesuňte data z nejrůznějších zdrojů dat do SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Načtení dat ze serveru SQL Server do Azure datový sklad SSIS (SQL)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Vytvoření balíčku SQL Server Integration Services (SSIS) k načtení dat ze serveru SQL Server do datový sklad SQL Azure. Volitelně můžete restrukturalizace, transformace a čistí data jako prochází toku dat SSIS.

V tomto kurzu udělejte toto:

- Ve Visual Studiu vytvořte nový projekt integračních služeb.
- Připojení ke zdrojům dat, včetně SQL Server (jako zdroj) a SQL datový sklad (jako cíle).
- Navrhněte balíčku SSIS, která načte data ze zdroje na požadované místo.
- Spuštění balíčku SSIS načtěte data.

Tento kurz používá jako zdroje dat SQL Server. SQL Server může běžet místně nebo v Azure virtuálního počítače.

## <a name="basic-concepts"></a>Základní koncepty

Balíček je jednotka práce v SSIS. Související balíčků seskupeny do projektů. Vytvoření projektů a návrh balíčků ve Visual Studiu s SQL Server Data Tools. Proces návrhu je vizuální proces, ve kterém můžete přetahování součásti z panelu nástrojů na návrhovou plochu, je připojit a nastavte jejich vlastnosti. Po dokončení balíček, můžete volitelně nasadit ho SQL serveru pro úplné řízení, sledování a zabezpečení.

## <a name="options-for-loading-data-with-ssis"></a>Možnosti pro načítání dat pomocí SSIS

Integrace služby SSIS (SQL Server) je flexibilní sada nástrojů, které obsahuje řadu možností pro připojení k a načítání dat do SQL datový sklad.

1. Určení čisté ADO umožňuje připojení k SQL datový sklad. Tento kurz používá cíl čistého ADO, protože obsahuje nejmenší počet možnosti konfigurace.
2. Připojení k SQL datový sklad pomocí zprostředkovatele OLE DB cíl. Tuto možnost stanovit mírně vyšší výkon než určení čisté ADO.
3. Pomocí úkolů nahrát objektů Blob Azure fáze data v úložišti objektů Blob Azure. Spuštění Polybase skript, který načítá data do SQL datový sklad pomocí úkolu SSIS provést SQL. Tato možnost nabízí optimálního výkonu zde uvedené tři možnosti. Nahrání objektů Blob Azure úkolu stáhnete [Microsoft SQL Server 2016 Integration Services Feature Pack pro Azure][]. Další informace o Polybase najdete v příručce [PolyBase][].

## <a name="before-you-start"></a>Než začnete

Můžete přecházet mezi tohoto kurzu, budete potřebovat:

1. **SQL Server Integration Services (služby SSIS)**. SSIS je součástí serveru SQL Server a vyžaduje zkušební verzi nebo licencovanou verzi serveru SQL Server. Získání zkušební verze SQL serveru 2016 Preview, naleznete v článku [Hodnocení SQL serveru][].
2. **Visual studia**. Bezplatné Visual Studio 2015 komunity Edition, získáte [Visual Studio komunity][].
3. **SQL Server Data Tools for Visual Studio (SSDT)**. SQL Server Data Tools Visual Studio 2015 získáte v tématu [Stáhnout SQL Server Data nástroje (SSDT)][].
4. **Ukázková data**. Tento kurz používá k načtení do SQL datový sklad ukázkových dat uložených na SQL serveru v ukázkové databázi AdventureWorks jako zdrojová data. Ukázkové databázi AdventureWorks získáte v tématu [AdventureWorks 2014 ukázkové databáze][].
5. **Databáze A SQL datový sklad a oprávnění**. Tento kurz se připojí k instanci SQL datový sklad a načte data do ní. Musíte mít oprávnění k vytvoření tabulky a načíst data.
6. **Pravidla brány firewall**. Budete muset vytvořit pravidlo bránu firewall na SQL datový sklad s IP adresu místního počítače před odeslat data do SQL datový sklad.

## <a name="step-1-create-a-new-integration-services-project"></a>Krok 1: Vytvořte nový projekt integračních služeb

1. Spuštění aplikace Visual Studio 2015.
2. V nabídce **soubor** , vyberte **nové | Projekt**.
3. Přejděte **nainstalovaný | Šablony | Funkce Business Intelligence | Integrace služby** typů projektů.
4. Vyberte položku **Integration Services Project**. Zadat hodnoty **název** a **umístění**a pak vyberte **OK**.

Visual Studio otevře a vytvoří nový projekt Integration Services (SSIS). Potom Visual Studio otevře tvůrce pro jeden nový balíček SSIS (Package.dtsx) v projektu. Zobrazí se následující oblasti obrazovky:

- V levé části **nástrojů** součástí SSIS.
- Uprostřed návrhovou plochu s více listech. Obvykle používáte aspoň **Control Flow** a karty **Data Flow** .
- Napravo **Průzkumník řešení** a podokna **Vlastnosti** .

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Krok 2: Vytvoření základního datového toku

1. Přetáhněte úkol toku dat z panelu nástrojů na střed návrhovou plochu (na kartě **Control Flow** ).

    ![][02]

2. Poklikejte na úkol toku dat přejděte na kartu Data Flow.
3. Ze seznamu jiných zdrojů v panelu nástrojů přetáhněte zdroj ADO.NET na návrhovou plochu. S adaptér zdroje stále vybrán změňte jeho název na **zdroje systému SQL Server** v podokně **Vlastnosti** .
4. Z jiných seznamu na panelu nástrojů a přetáhněte na návrhovou plochu ve skupinovém rámečku zdroj ADO.NET cíl ADO.NET. Určení adaptér stále vybrán změňte jeho název na **cíl SQL DW** v podokně **Vlastnosti** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Krok 3: Konfigurace adaptér zdroje

1. Poklikejte na adaptér zdroje otevřete **Editor zdrojového ADO.NET**.

    ![][03]

2. Na kartě **Connection Manager** **ADO.NET Editor zdrojového kódu**klikněte na tlačítko **Nový** vedle seznamu **ADO.NET připojení správce** a otevřete dialogové okno **Konfigurovat ADO.NET Connection Manager** a namapovat připojení databáze SQL serveru, ze které tento kurz načte data.

    ![][04]

3. V dialogovém okně **Konfigurovat ADO.NET Connection Manager** klikněte na tlačítko **Nový** a otevřete dialogové okno **Správce připojení** a vytvořte nové datové připojení.

    ![][05]

4. V dialogovém okně **Connection Manager** provedete následující kroky.

    1. U **poskytovatele**vyberte poskytovatele SqlClient Data.
    2. **Název serveru**zadejte název serveru SQL Server.
    3. Vybrat nebo zadat ověřovací informace v části **přihlášení na server** .
    4. V části **připojení k databázi** vyberte ukázkové databázi AdventureWorks.
    5. Klikněte na **Testovat připojení**.
    
        ![][06]
    
    6. V dialogovém okně, které zprávy výsledky test připojení klikněte na **OK** se vraťte do dialogového okna **Správce připojení** .
    7. V dialogovém okně **Správce připojení** klikněte na **OK** se vraťte do dialogového okna **Správce konfigurace připojení ADO.NET** .
 
5. V dialogovém okně **Konfigurovat ADO.NET Connection Manager** klikněte na **OK** se vraťte do **ADO.NET Editor zdrojového kódu**.
6. V dialogovém okně **ADO.NET Editor zdrojového kódu**v seznamu **název tabulky nebo zobrazení** vyberte tabulku **Sales.SalesOrderDetail** .

    ![][07]

7. Klikněte na **Náhled** zobrazíte prvních 200 řádků dat v tabulce zdroj v dialogovém okně **Náhled výsledků dotazu** .

    ![][08]

8. V dialogovém okně **Náhled výsledků dotazu** klikněte na **Zavřít** se vraťte do **ADO.NET Editor zdrojového kódu**.
9. **Editor zdrojového ADO.NET**klikněte na **OK** dokončete konfiguraci zdroje dat.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Krok 4: Připojení k určení adaptér adaptér zdroje

1. Vyberte adaptér zdroje na návrhovou plochu.
2. Vyberte na modrou šipku, která slouží k rozšíření z adaptér zdroje a přetáhněte ji do cílového editoru ho přichytí.

    ![][10]

    Typické balíček SSIS můžete použít číslo jiné součásti z panelu nástrojů SSIS mezi zdroji a cíle struktury, transformace a čistí dat jako prochází toku dat SSIS. Chcete-li zachovat v tomto příkladu co nejjednodušší, jsme připojujete zdroji přímo do cíle.

## <a name="step-5-configure-the-destination-adapter"></a>Krok 5: Konfigurace adaptér cíl

1. Poklepejte na cíl adaptér otevřete **Editor cíl ADO.NET**.

    ![][11]

2. Na kartě **Connection Manager** **ADO.NET cíl editoru**klikněte na tlačítko **Nový** vedle seznamu **připojení správce** a otevřete dialogové okno **Nastavení správce připojení ADO.NET** a vytvořte nastavení připojení pro Azure SQL datový sklad databázi, do které tento kurz načte data.
3. V dialogovém okně **Konfigurovat ADO.NET Connection Manager** klikněte na tlačítko **Nový** a otevřete dialogové okno **Správce připojení** a vytvořte nové datové připojení.
4. V dialogovém okně **Connection Manager** provedete následující kroky.
    1. U **poskytovatele**vyberte poskytovatele SqlClient Data.
    2. **Název serveru**zadejte název SQL datový sklad.
    3. V části **přihlásit k serveru** zaškrtněte políčko **použít SQL Server ověřování** a zadejte ověřovací údaje.
    4. V části **připojení k databázi** vyberte existující databázi SQL datový sklad.
    5. Klikněte na **Testovat připojení**.
    6. V dialogovém okně, které zprávy výsledky test připojení klikněte na **OK** se vraťte do dialogového okna **Správce připojení** .
    7. V dialogovém okně **Správce připojení** klikněte na **OK** se vraťte do dialogového okna **Správce konfigurace připojení ADO.NET** .
5. V dialogovém okně **Konfigurovat ADO.NET Connection Manager** klikněte na **OK** se vraťte do **Editoru cíl ADO.NET**.
6. V **Editoru cíl ADO.NET**klikněte na **Nový** vedle **použití tabulky nebo zobrazení** seznamu a otevře se dialogové okno **Vytvořit tabulku** , ve kterém lze vytvořit nové cílové tabulce pomocí seznamu sloupce, který odpovídá zdrojové tabulky.

    ![][12a]

7. V dialogovém okně **Vytvořit tabulku** provedete následující kroky.

    1. Změňte název cílové tabulce na **Podrobnosti prodejní objednávky**.
    2. Odeberte sloupec **rowguid** . Datový typ **uniqueidentifier** nepodporuje SQL datový sklad.
    3. Změna datového typu sloupce **LineTotal** **peněz**. Datový typ **decimal** nepodporuje SQL datový sklad. Informace o podporovaných datových typů najdete v tématu [CREATE TABLE (datový sklad SQL Azure, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Kliknutím na **OK** vytvořte v tabulce a vraťte se do **Editoru cíl ADO.NET**.

8. V **Editoru cíl ADO.NET**vyberte kartu **mapování** se chcete podívat, jak jsou namapované sloupců ve zdroji sloupce v cílovém.

    ![][13]

9. Klikněte na **OK** dokončete konfiguraci zdroje dat.

## <a name="step-6-run-the-package-to-load-the-data"></a>Krok 6: Spuštění balíčku načtení dat

Spuštění balíčku kliknutím na tlačítko **Start** na panelu nástrojů nebo tak, že vyberete některou z možností **Spustit** v nabídce **ladění** .

Jako balíček začne spustit, se zobrazí žlutá kola otáčející označíte činnosti, stejně jako počet řádků zpracování zatím.

![][14]

Pokud balení dokončeno, uvidíte zelené značky zaškrtnutí označíte úspěch, jakož i celkový počet řádků dat ze zdroje načíst do cíle.

![][15]

Blahopřejeme! SQL Server Integration Services úspěšně vypotřebovali k načtení dat do datový sklad SQL Azure.

## <a name="next-steps"></a>Další kroky

- Další informace o toku dat SSIS. Začněte zde: [Data Flow][].
- Naučte se ladění a řešení potíží s balíčků přímo na návrhového prostředí. Začněte zde: [Poradce při potížích s nástroje pro vývoj balíčku][].
- Naučte se nasadit SSIS projekty a balíčků Integration Services Server nebo do jiného umístění úložiště. Začněte zde: [projekty nasazení a balíčků][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Průvodce PolyBase]: https://msdn.microsoft.com/library/mt143171.aspx
[Stažení SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[Vytvoření tabulky (datový sklad Azure SQL, – paralelní datový sklad)]: https://msdn.microsoft.com/library/mt203953.aspx
[Toku dat]: https://msdn.microsoft.com/library/ms140080.aspx
[Poradce při potížích nástroje pro vývoj balíčku]: https://msdn.microsoft.com/library/ms137625.aspx
[Projekty a balíčků nasazení]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack pro Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server hodnocení]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Komunita aplikace Visual Studio]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[Databáze ukázkové databázi AdventureWorks 2014]: https://msftdbprodsamples.codeplex.com/releases/view/125550
