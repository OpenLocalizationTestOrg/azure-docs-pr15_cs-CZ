<properties
    pageTitle="Průzkumník DocumentDB dotaz: SQL editoru dotazů | Microsoft Azure"
    description="Informace o podokně dotaz DocumentDB editoru dotazů SQL Azure portálu pro psaní dotazy SQL a systémem proti kolekce NoSQL DocumentDB."
    keywords="psaní dotazy sql editoru dotazů sql"
    services="documentdb"
    authors="kirillg"
    manager="jhubbard"
    editor="monicar"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="kirillg"/>

# <a name="write-edit-and-run-sql-queries-for-documentdb-using-query-explorer"></a>Psaní, úpravy a spouštění dotazů SQL pro DocumentDB pomocí Průzkumníka dotazu 

Tento článek obsahuje přehled o podokně dotaz [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Azure portálu nástroj, který umožňuje psaní, úpravy a kontrolovat dotazy SQL [DocumentDB kolekce](documentdb-create-collection.md).

1. Na portálu Azure v Jumpbar klikněte na **DocumentDB (NoSQL)**. Pokud **DocumentDB (NoSQL)** není zobrazená, klikněte na **Další služby** a potom klikněte na **DocumentDB (NoSQL)**.

2. V nabídce zdrojů klikněte na položku **Průzkumník dotazu**. 

    ![Snímek obrazovky s portálu Azure v Průzkumníkovi dotazu zvýrazněným](./media/documentdb-query-collections-query-explorer/queryexplorercommand.png)

3. V zásuvné **Explorer dotazu** vyberte **databází** a **kolekcí** dotazu z rozevíracích seznamů a zadejte spustit dotaz. 

    Rozevírací seznamy **databází** a **kolekcí** již předem vyplněna v závislosti na kontextu, ve kterém spuštění Průzkumníka dotazu. 

    Výchozí dotaz `SELECT TOP 100 * FROM c` je k dispozici.  Můžete přijmout výchozího dotazu nebo vytvoření vlastního dotazu pomocí jazyka SQL dotazu podle [dotaz SQL zobrazený cheaty listu](documentdb-sql-query-cheat-sheet.md) nebo v článku [dotaz SQL zobrazený a syntaxe jazyka SQL](documentdb-sql-query.md) .

    Klepnutím na tlačítko Zobrazit výsledky **spuštění dotazu** .

    ![Snímek obrazovky s psát dotazy SQL v dotazu Průzkumníku editoru dotazů SQL](./media/documentdb-query-collections-query-explorer/queryexplorerinitial.png)

4. **Výsledky** zásuvné zobrazí výstup dotazu. 

    ![Snímek obrazovky s výsledky psát dotazy SQL v dotazu Průzkumníka](./media/documentdb-query-collections-query-explorer/queryresults1.png)

## <a name="work-with-results"></a>Práce s výsledky

Ve výchozím nastavení Průzkumníka dotaz vrátí výsledky v sady 100.  Pokud váš dotaz vrací víc než 100 výsledky, jednoduše příkazy **Další stránka** a **Předchozí stránka** procházení sadu výsledků.

![Snímek obrazovky s Průzkumníkem dotazu stránkování podpory](./media/documentdb-query-collections-query-explorer/queryresultspagination.png)

Pro úspěšné dotazy v **informačním** podokně obsahuje metriky třeba požadavek poplatků číslo zaokrouhlit cesty dotazu, sadu výsledků aktuálně zobrazovaly, a jestli jsou další výsledky, které lze přistupovat prostřednictvím příkaz **Další stránky** , jako jsme zmínili dříve.

![Snímek obrazovky s Průzkumníkem dotazu dotazu informace](./media/documentdb-query-collections-query-explorer/queryinformation.png)

## <a name="use-multiple-queries"></a>Použití víc dotazů

Pokud používáte víc dotazů a chcete rychle přepnout mezi nimi, zadejte všechny dotazy v textovém poli dotazu zásuvné **Explorer dotaz** a pak vyberte tu, kterou chcete spustit a klepnutím na tlačítko Zobrazit výsledky **spuštění dotazu** .

![Snímek obrazovky s psaní víc dotazů SQL v Průzkumníku dotaz (query SQL editor) a zvýraznění a systémem jednotlivé dotazy](./media/documentdb-query-collections-query-explorer/queryexplorerhighlightandrun.png)

## <a name="add-queries-from-a-file-into-the-sql-query-editor"></a>Přidání dotazů ze souboru do editoru dotazů SQL

Můžete načíst obsah existujícího souboru pomocí příkazu **Načtení souboru** .

![Snímek obrazovky ukazující, jak načíst dotazy SQL ze souboru do dotazu Průzkumník Windows načtení souboru](./media/documentdb-query-collections-query-explorer/loadqueryfile.png)

## <a name="troubleshoot"></a>Poradce při potížích s

Pokud dokončení dotazu s chybami dotazu Průzkumník zobrazuje seznam chyb, které vám mohou pomoci při řešení potíží úsilí.

![Snímek obrazovky s Průzkumníkem dotazu dotazu chyby](./media/documentdb-query-collections-query-explorer/queryerror.png)

## <a name="run-documentdb-sql-queries-outside-the-portal"></a>Spouštění dotazů mimo portál DocumentDB SQL

Průzkumník dotazu na portálu Azure je právě způsobů kontrolovat DocumentDB dotazy SQL. Můžete taky spustit dotazy SQL pomocí [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo [klienta SDK](documentdb-sdk-dotnet.md). Další informace o použití těchto postupů najdete v článku [dotazů spouštěním SQL](documentdb-sql-query.md#executing-sql-queries)

## <a name="next-steps"></a>Další kroky

Další informace o DocumentDB SQL gramatiky podporované v Průzkumníku dotazu, najdete v článku [dotaz SQL zobrazený a syntaxe jazyka SQL](documentdb-sql-query.md) nebo vytisknout [dotaz SQL zobrazený cheaty listu](documentdb-sql-query-cheat-sheet.md).
Může taky líbit experimentování s [Hřišť dotazu](https://www.documentdb.com/sql/demo) , kde můžete otestovat, dotazy online pomocí datovou sadu vzorku.
