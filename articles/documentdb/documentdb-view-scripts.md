<properties
    pageTitle="DocumentDB skript Průzkumníku editoru jazyka JavaScript | Microsoft Azure"
    description="Informace o podokně skript DocumentDB nástroj Azure portálu pro správu DocumentDB serverovou programovací artefakty včetně uložené procedury, aktivačních událostí a funkcí definovaných uživatelem."
    keywords="editor jazyka JavaScript"
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

# <a name="create-and-run-stored-procedures-triggers-and-user-defined-functions-using-the-documentdb-script-explorer"></a>Vytvoření a spuštění uložené procedury aktivačních událostí a pomocí Průzkumníka skript DocumentDB funkcí definovaných uživatelem

Tento článek obsahuje přehled [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Průzkumník skript, který je editor jazyka JavaScript v portálu Azure, který umožňuje zobrazit a spustit DocumentDB serverovou programovací artefakty včetně uložené procedury, aktivačních událostí a funkcí definovaných uživatelem. Přečtěte si další informace o DocumentDB serverovou programování v článku [aktivačních událostí databáze, uložené procedury a funkce definované uživatelem](documentdb-programming.md) .

## <a name="launch-script-explorer"></a>Spuštění Průzkumník skriptů

1. Na portálu Azure v Jumpbar klikněte na **DocumentDB (NoSQL)**. Pokud **DocumentDB účty** není zobrazená, klikněte na **Další služby** a klikněte na **DocumentDB (NoSQL)**.

2. V nabídce zdrojů klikněte na položku **Průzkumník skriptu**.

    ![Snímek obrazovky s příkazem Průzkumník skriptů](./media/documentdb-view-scripts/scriptexplorercommand.png)
 
    **Shromažďování** a **databáze** rozevírací seznamy jsou předem vyplněné v závislosti na kontextu, ve kterém spuštění Průzkumník skriptů.  Například pokud spuštění z databáze zásuvné aktuální databáze je předem vyplněna.  Pokud spuštění z kolekce zásuvné je aktuální kolekci předem vyplněna.

4.  Umožňuje snadno změnit kolekci odkud skripty jsou aktuálně zobrazujete aniž byste museli zavřete a znovu spusťte Průzkumníka skript **databáze** a **kolekce** rozevírací seznamy.  

5. Průzkumník skriptů podporuje také filtrování aktuálně načtené sadu skripty podle jejich id vlastnosti.  Jednoduše napište do pole Filtr a výsledky v seznamu Průzkumník skriptů jsou filtrovány podle zadaných kritérií.

    ![Snímek obrazovky s skript Průzkumníkem s filtrovaných výsledků](./media/documentdb-view-scripts/scriptexplorerfilterresults.png)


    > [AZURE.IMPORTANT] Průzkumník skriptů filtrování funkce pouze filtry ze sady ***aktuálně*** načíst skripty a automaticky neaktualizuje aktuálně vybranou kolekci.

5. Abyste mohli aktualizovat seznam skriptů načíst Průzkumníkem skript, jednoduše klikněte na příkaz **Aktualizovat** v horní části zásuvné.

    ![Příkaz Aktualizovat snímek obrazovky s Průzkumníkem skriptu](./media/documentdb-view-scripts/scriptexplorerrefresh.png)


## <a name="create-view-and-edit-stored-procedures-triggers-and-user-defined-functions"></a>Vytvářet, prohlížet a upravovat uložené procedury, aktivačních událostí a funkcí definovaných uživatelem

Průzkumník skriptů umožňuje snadno provádět operace CRUD DocumentDB serverovou programovací artefakty.  

- Vytvořit skript, jednoduše klikněte na příslušné vytvořit příkaz Průzkumník skriptů zadejte id, zadejte obsah skript a klikněte na **Uložit**.

    ![Snímek obrazovky s Průzkumníkem skript možnost vytvoření, zobrazení editoru jazyka JavaScript](./media/documentdb-view-scripts/scriptexplorercreatecommand.png)

- Při vytváření aktivační událost, musíte taky zadáte operaci typ signálu a aktivační signál aktivační událost

    ![Snímek obrazovky s Průzkumníkem skript vytvořit možnost aktivační událost](./media/documentdb-view-scripts/scriptexplorercreatetrigger.png)

- Chcete-li zobrazit skript, jednoduše klikněte skript, které vás zajímají.

    ![Snímek obrazovky s Průzkumníkem skript zobrazení skriptu prostředí](./media/documentdb-view-scripts/scriptexplorerviewscript.png)

- Úprava skriptu, jednoduše proveďte požadované změny v JavaScriptu editor a klikněte na **Uložit**.

    ![Snímek obrazovky s Průzkumníkem skript zobrazení skriptu prostředí](./media/documentdb-view-scripts/scriptexplorereditscript.png)

- Zrušit změny čekající na vyřízení skript, jednoduše klikněte na příkaz **Zrušit** .

    ![Snímek obrazovky s Průzkumníkem skript kliknutím na zahodit změny prostředí](./media/documentdb-view-scripts/scriptexplorerdiscardchanges.png)

- Průzkumník skriptů umožňuje snadno zobrazit vlastnosti systému aktuálně načtené skriptu po kliknutí na příkaz **Vlastnosti** .

    ![Zobrazení vlastností skript snímek obrazovky s Průzkumníkem skriptu](./media/documentdb-view-scripts/scriptproperties.png)

    > [AZURE.NOTE] Vlastnost časové razítko (_ts) interně tvaru epocha čas, ale Průzkumník skriptů zobrazí hodnotu v GMT formátu čitelného.

- Pokud chcete odstranit skript, vyberte je v Průzkumník skriptů a klikněte na příkaz **Odstranit** .

    ![Příkaz Odstranit snímek obrazovky s Průzkumníkem skriptu](./media/documentdb-view-scripts/scriptexplorerdeletescript1.png)

- Potvrďte odstranění kliknutím na tlačítko **Ano** nebo -li akci odstranění po kliknutí na **žádný**.

    ![Příkaz Odstranit snímek obrazovky s Průzkumníkem skriptu](./media/documentdb-view-scripts/scriptexplorerdeletescript2.png)

## <a name="execute-a-stored-procedure"></a>Spuštění uložené procedury

> [AZURE.WARNING] Spuštění uložené procedury v Průzkumníku skript ještě nepodporuje Collections straně oddíly serveru. Další informace Navštěvujte blog o [Partitioning a měřítko DocumentDB](documentdb-partition-data.md).

Průzkumník skriptů umožňuje provádět uložené procedury serverovou z portálu Microsoft Azure.

- Při otevírání nového zásuvné postup vytvoření uložené, výchozí skript (*Předpona*) předtím zadali. Chcete-li spustit *předponu* nebo vlastní skript, přidejte *id* a *vstupů*. Všechny vstupy uložené procedury, které podporují více parametrů, musí být v rámci určené (například *["foo", "panel"]*).

    ![Snímek obrazovky s skript Explorer uložené procedury zásuvné spuštění uložené procedury a přidejte zadání vstupních hodnot](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-input.png)

- Spuštění uložené procedury, jednoduše klikněte na příkaz **Uložit a spouštět** podokna script editor.

    > [AZURE.NOTE] Příkaz **Uložit a spouštět** uloží uloženou procedurou před spuštěním, což znamená, že dojde k přepsání dříve uložené verzi uložená procedura.

- Spuštění úspěšné uložená procedura budou mít stav *úspěšně uloží a spouštět uložené procedury* a vrácených výsledků se zobrazí v podokně *výsledků* .

    ![Snímek obrazovky s skript Explorer uložené procedury zásuvné provést uložená procedura](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure.png)

- Pokud spuštění dojde k chybě, chyba se zobrazí v podokně *výsledků* .

    ![Snímek obrazovky s Průzkumníkem skript skript vlastnosti listu. Spuštění uložené procedury s chybami](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-error.png)

## <a name="work-with-scripts-outside-the-portal"></a>Práce s skripty mimo portálu

Průzkumník skriptů Azure portálu je jediným způsob, jak pracovat s uložené procedury aktivačních událostí a funkcí definovaných uživatelem v DocumentDB. Můžete taky spolupracovat s skriptů pomocí rozhraní REST API a [klienta SDK](documentdb-sdk-dotnet.md). Dokumentace rozhraní REST API obsahuje ukázky pro práci s [uložené procedury pomocí ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/mt489092.aspx) [pomocí ZBÝVAJÍCÍ funkce definované uživatelem](https://msdn.microsoft.com/library/azure/dn781481.aspx)a [ZBÝVAJÍCÍ pomocí aktivačních událostí](https://msdn.microsoft.com/library/azure/mt489116.aspx). Ukázky jsou rovněž k dispozici zobrazující jak [pracovat s skriptů pomocí C#](documentdb-dotnet-samples.md#server-side-programming-examples) a [práce s skriptů pomocí Node.js](documentdb-nodejs-samples.md#server-side-programming-examples).

## <a name="next-steps"></a>Další kroky

Další informace o DocumentDB serverovou programování v článku [uložené procedury, databáze aktivačními událostmi a funkce definované uživatelem](documentdb-programming.md) .

[Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/documentdb/) je také užitečné zdroje pro vás jak Další informace o DocumentDB.  
