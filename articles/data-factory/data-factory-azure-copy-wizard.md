<properties
    pageTitle="Průvodce Azure kopírovat data Factory | Microsoft Azure"
    description="Informace o tom, jak pomocí Průvodce dat Azure Factory Kopírovat zkopírujte data z podporovaných zdrojů dat do propadů."
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
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure Data Factory – Průvodce kopírováním
Průvodce kopírováním Factory dat Azure je usnadnění procesu ingesting data, která bývá prvním krokem ve scénáři integrace dat začátku do konce. Při filtrované pomocí Průvodce kopírováním Factory dat Azure, není nutné pochopit všechny JSON definice propojené služeb, datových sad a potrubí. Však po provedení všech kroků v průvodci Průvodce automaticky vytvoří kanálu ke kopírování dat z vybraný zdroj dat do vybraných cíle. Kromě toho kopírovat Průvodce vám pomůže ověřit data požití při vytváření, které se uloží moc času, zejména pokud se jsou ingesting dat první ze zdroje dat. Spusťte Průvodce kopírovat, klikněte na dlaždici **zkopírujte data** na domovské stránce výrobce data.

![Kopírování Průvodce](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Intuitivní průvodce ke kopírování dat
Tento průvodce umožňuje snadno přesuňte data z nejrůznějších zdrojů do cíle v minutách. Po přechodu procházet všemi kroky průvodce, kanálu aktivitu Kopírovat se automaticky vytvoří spolu s závislých Data Factory položek (propojené services a datových sad). Žádné další kroky nutné vytvořit kanálu.   

![Vyberte zdroj dat](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Článek [Průvodce kopírováním kurz](data-factory-copy-data-wizard-tutorial.md) najdete v článku podrobné pokyny k vytvoření ukázkové kanálu zkopírujte data z Azure objektů blob do tabulky databáze SQL Azure. 

Průvodce slouží velký daty v paměti od začátku. Je jednoduchý a efektivně vytvářet Data Factory kanály, které přesunout stovky různých složek, soubory nebo tabulek pomocí Průvodce datovým kopírovat. Průvodce podporuje následující tři funkce: náhled automatické dat schématu zachycení a mapování a filtrování dat. 

## <a name="automatic-data-preview"></a>Náhled automatické dat 
Průvodce kopírováním umožňuje zkontrolujete část data z vybraný zdroj dat můžete k ověření, zda data je správná data, které chcete kopírovat. Kromě toho při změně zdrojových dat z textového souboru Průvodce kopírováním analyzuje textový soubor další řádek a sloupec oddělovače a schématu automaticky. 

![Nastavení formátu souboru](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Zachycení schématu a mapování 
Schéma vstupní data neodpovídají schématu výstupní data v některých případech. V tomto scénáři budete muset namapování sloupců z schématu zdroje na sloupce ze cíle schéma. 

Průvodce kopírováním automaticky mapuje sloupce ve schématu zdrojového sloupce v cíle schéma. Můžete změnit mapování pomocí rozevíracích seznamů (nebo) určit, zda sloupec je nutné při kopírování data.   

![Mapování schématu](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrování dat  
Průvodce vám umožní filtrovat zdroje dat vyberte jen tu část dat, která potřebuje budou zkopírovány do cílové/jímky úložiště. Filtrování snižuje objemu data, která chcete zkopírovat do úložiště dat jímky a tedy zlepšuje výkon operace kopírování. Poskytuje flexibilní způsob, jak filtrovat data v relační databázi pomocí SQL dotazu jazyk (nebo) soubory ve složce aplikace objektů blob Azure [Data Factory funkcí a proměnných](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrování dat v databázi  
V příkladu, dotaz SQL zobrazený pomocí `Text.Format` (funkce) a `WindowStart` proměnné. 

![Ověření výrazů](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrování dat ve složce aplikace objektů blob Azure
Proměnné v cestu ke složce slouží ke kopírování dat ze složky určené za běhu na základě [systém proměnných](data-factory-functions-variables.md#data-factory-system-variables). Jsou podporované proměnné: **{rok}**, **{měsíc}**, **{den}**, **{hodinu}**, **{minutu}**a **{vlastní}**. Příklad: inputfolder / {rok} / {měsíc} / {den}.

Předpokládejme, že máte zadané složky ve formátu následující:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klikněte na tlačítko **Procházet** pro **soubor nebo složku**, přejděte na jednu z těchto složek (například 2016 -> 03 -> 01 -> 02) a klikněte na **Vybrat**. Měli byste vidět `2016/03/01/02` do textového pole. Teď, nahraďte **2016** **{rok}**, **03** s **{měsíc}**, **01** s **{den}**a **02** s **{hodinu}**a stisknutím klávesy Tab. Měli byste vidět rozevíracích seznamech vyberte formát těchto čtyř proměnných:

![Pomocí proměnných systému](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Jak ukazuje následující obrázek, můžete také **vlastní** proměnné a všechny [podporované formátovací řetězce](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Chcete-li vybrat jinou složku s tuto strukturu, tlačítko **Procházet** nejdřív. Potom nahraďte hodnotu **{vlastní}**a stisknutím klávesy Tab zobrazíte textové pole, kam lze zadat formátovací řetězec.     

![Použití vlastní proměnná](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Podpora pro různorodého dat a typy objektů
Pomocí Průvodce kopírováním můžete efektivně přesunete stovky tabulkami, soubory nebo složky.

![Vyberte tabulky, ze které chcete zkopírovat data](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Možnosti plánování
Zkopírování můžete spustit jednou nebo při plánování (hodinové denně, a tak dál). Obě tyto možnosti lze šířka spojnice mezi místním prostředím, cloudu a místní kopie plochy.

Jednorázové kopírování umožňuje přesun dat ze zdroje s cílem jenom jednou. Platí pro data libovolnou velikost a podporovaný formát. Plánované kopírovat umožňuje zkopírujte data v předepsaném opakování. Formátovaný nastavení (například opakovat časový limit a oznámení) umožňuje konfigurace plánované kopírovat.

![Plánování vlastností](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Další kroky
Stručný návod pomocí Průvodce datovým Factory kopírovat a vytvořte příležitost s kopírovat aktivitu naleznete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).
