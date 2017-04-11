<properties
    pageTitle="Scénáře pro pokročilé technologie pro analýzu procesy a technologii Azure počítač přečíst | Microsoft Azure"
    description="Vyberte vhodné scénáře to Upřesnit prediktivní analýzy s procesem vědy dat týmu."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scénáře pro pokročilé technologie pro analýzu Azure počítač přečíst

V tomto článku najdete spoustu ukázkové zdroje dat a cílové scénáře, které můžete uskutečněných jednotlivými [Týmu dat pro výzkum obrázku (TDSP)](data-science-process-overview.md). TDSP poskytuje systematický postup pro týmy ke spolupráci na vytváření inteligentní aplikací. Scénáře zde uvedené ilustrují možností v pracovním postupu zpracování dat, které jsou závislé na ukazatele, umístění zdroje a cílového úložiště v Azure.

**Rozhodovací strom** pro výběr ukázkové situace určený pro vaše data a cíle jsou uvedeny v části poslední.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Každá z těchto částí představuje scénáři výběru. Pro každý scénář vědy dat nebo pokročilé technologie pro analýzu toku a referenční materiály Azure jsou uvedeny.

>[AZURE.NOTE]**Pro všechny následující scénáře, budete muset:**
><br/>
>* [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md)
><br/>
>* [Vytvoření pracovního prostoru výukové počítače Azure](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Scénář \#1: malé a střední tabulkových datovou sadu místní souborů se změnami

![Malé a střední místní soubory][1]

#### <a name="additional-azure-resources-none"></a>Další zdroje informací Azure: žádné

1.  Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

2.  Nahrajte datovou sadu.

3.  Vytvoření tok experiment Azure počítače výukové počínaje nahraný publikovaných.

## <a name="smalllocalprocess"></a>Scénář \#2: malé a střední datovou sadu místní souborů, které vyžadují zpracování

![Malé a střední místní soubory s zpracování][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (Poznámkový blok IPython server)

1.  Vytvoření Azure virtuální počítač IPython Poznámkový blok.

2.  Odeslání dat do kontejneru Azure úložiště.

3.  Předem zpracovat a vymazání data v poznámkovém bloku IPython přístup k datům z Azure úložiště kontejner.

4.  Transformace dat vyčistit, formátu tabulky.

5.  Kromě objektů BLOB Azure Transformovaná data.

6.  Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

7.  Číst z Azure objektů BLOB používat [Importovat Data] data[ import-data] modul.

8. Vytvoření toku experiment Azure počítače výukové počínaje požití publikovaných.

## <a name="largelocal"></a>Scénář \#3: velké datovou sadu místní souborů zacílení objektů BLOB Azure

![Velké místní soubory][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (Poznámkový blok IPython server)

1.  Vytvoření Azure virtuální počítač IPython Poznámkový blok.

2.  Odeslání dat do kontejneru Azure úložiště.

3.  Předem zpracovat a vymazání data v poznámkovém bloku IPython přístup k datům z objektů BLOB Azure.

4.  Transformace dat vyčistit, formátu tabulky podle potřeby.

5.  Zkoumání dat a podle potřeby vytvářet funkce.

6.  Extrahujte vzorek malé a střední data.

7.  Uložení vybraných dat v objektů BLOB Azure.

8. Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

9. Číst z Azure objektů BLOB používat [Importovat Data] data[ import-data] modul.

10. Vytvoření toku experiment Azure počítače výukové počínaje požití publikovaných.


## <a name="smalllocaltodb"></a>Scénář \#4: malé a střední datovou sadu místní souborů zacílení SQL Server ve počítače Virtal Azure

![Malé a střední místní soubory do SQL databáze v Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (SQL Server nebo Poznámkový blok IPython serveru)

1.  Vytvoření Azure virtuální počítač serveru SQL Server + IPython Poznámkový blok.

2.  Odeslání dat do kontejneru Azure úložiště.

3.  Předem zpracovat a vymazání data v kontejneru Azure úložiště IPython poznámkového bloku.

4.  Transformace dat vyčistit, formátu tabulky podle potřeby.

5.  Uložení dat OM místní soubory (Poznámkový blok IPython běží na OM, místní jednotky odkažte OM jednotky).

6.  Načtení dat do databáze serveru SQL Server Azure OM se systémem.

    Možnost \#1: použití SQL Server Management Studio.

    - Přihlaste se k serveru SQL Server OM
    - Spusťte SQL Server Management Studio.
    - Vytvoření databáze a cílovou tabulku.
    - Použijte jednu hromadné importovat metod k načtení dat z OM místní soubory.

    Možnost \#2: použití IPython Poznámkový blok – není vhodné pro středních a větší datových sad<!-- -->    
    - Umožňuje přístup k SQL serveru OM ODBC připojovací řetězec.
    - Vytvoření databáze a cílovou tabulku.
    - Použijte jednu hromadné importovat metod k načtení dat z OM místní soubory.

7.  Zkoumání dat, vytvořit funkce podle potřeby. Všimněte si, že funkce nemusí být nastala v databázových tabulkách. Poznámka: pouze potřebné dotaz tak, aby byla vytvořená.

8. Rozhodnete o velikosti ukázkových dat, pokud potřeby a/nebo žádoucí.

9. Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

10. Číst data přímo z SQL serveru pomocí [Importovat Data] [ import-data] modul. Vložte potřebné dotaz, který extrahuje pole vytvoří funkcí a vzorky dat v případě potřeby přímo v okně [Importovat Data] [ import-data] dotazu.

11. Vytvoření toku experiment Azure počítače výukové počínaje požití publikovaných.

## <a name="largelocaltodb"></a>Scénář \#5: velké datové sady v místní soubory obrázku SQL Server ve OM Azure

![Velké místní soubory do SQL databáze v Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (SQL Server nebo Poznámkový blok IPython serveru)

1.  Vytvoření Azure virtuální počítač SQL Server a server IPython Poznámkový blok.

2.  Odeslání dat do kontejneru Azure úložiště.

3.  (Volitelné) Předem zpracovat a vymazání data.

    na.  Předem zpracovat a vymazání data v poznámkovém bloku IPython přístup k datům z objektů BLOB Azure.

    b.  Transformace dat vyčistit, formátu tabulky podle potřeby.

    c.  Uložení dat OM místní soubory (Poznámkový blok IPython běží na OM, místní jednotky odkažte OM jednotky).

4.  Načtení dat do databáze serveru SQL Server Azure OM se systémem.

    na.  Přihlaste se k serveru SQL Server OM.

    b.  Pokud data nejsou uložena už, stáhněte si datové soubory z Azure úložiště kontejneru místní OM složky.

    c.  Spusťte SQL Server Management Studio.

    d.  Vytvoření databáze a cílovou tabulku.

    e.  Použijte některou z hromadné importovat metody načtěte data.

    f.  Pokud spojení tabulky jsou potřeba, vytvořte indexy pro urychlení spojení.

     > [AZURE.NOTE] Rychlejší načítání velkých dat velikostí se doporučuje vytvořit rozdělený tabulky a hromadného importu dat souběžně. Další informace najdete v tématu [Paralelní Import dat do tabulky oddíly SQL](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Zkoumání dat, vytvořit funkce podle potřeby. Všimněte si, že funkce nemusí být nastala v databázových tabulkách. Poznámka: pouze potřebné dotaz tak, aby byla vytvořená.

6.  Rozhodnete o velikosti ukázkových dat, pokud potřeby a/nebo žádoucí.

7.  Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

8. Číst data přímo z SQL serveru pomocí [Importovat Data] [ import-data] modul. Vložte potřebné dotaz, který extrahuje pole vytvoří funkcí a vzorky dat v případě potřeby přímo v okně [Importovat Data] [ import-data] dotazu.

9. Jednoduchý toku experiment Azure počítače výukové počínaje nahraný datovou sadu

## <a name="largedbtodb"></a>Scénář \#6: velké datové sady v serveru SQL databáze místní, zaměření na serveru SQL Server ve Azure virtuálního počítače

![Velké SQL databáze místní do SQL databáze v Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (SQL Server nebo Poznámkový blok IPython serveru)

1.  Vytvoření Azure virtuální počítač SQL Server a server IPython Poznámkový blok.

2.  Použijte některou z data exportovat metody export požadovaných dat ze serveru SQL Server na výpisu soubory.

    > [AZURE.NOTE] Pokud se rozhodnete přesunutí všech dat z databáze místní alternativním (rychlejší) způsobem přejdete celou databázi k instanci systému SQL Server v Azure. Kroky exportu dat, vytvořit databázi a načítání a import dat do cílové databáze a jejich sledování alternativní metody přeskočte.

3.  Nahrajte soubory výpisu na kontejner Azure úložiště.

4.  Načtení dat do databáze SQL serveru spuštěný na Azure virtuálního počítače.

    na.  Přihlaste se k serveru SQL Server OM.

    b.  Složky local OM stahování kontejneru Azure úložiště datové soubory.

    c.  Spusťte SQL Server Management Studio.

    d.  Vytvoření databáze a cílovou tabulku.

    e.  Použijte některou z hromadné importovat metody načtěte data.

    f.  Pokud spojení tabulky jsou potřeba, vytvořte indexy pro urychlení spojení.

    > [AZURE.NOTE] Rychlejší načítání velkých dat rozměrů, vytvořit rozdělený tabulek a hromadně importujte data souběžně. Další informace najdete v tématu [Paralelní Import dat do tabulky oddíly SQL](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Zkoumání dat, vytvořit funkce podle potřeby. Všimněte si, že funkce nemusí být nastala v databázových tabulkách. Poznámka: pouze potřebné dotaz tak, aby byla vytvořená.

6.  Rozhodnete o velikosti ukázkových dat, pokud potřeby a/nebo žádoucí.

7.  Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

8. Číst data přímo z SQL serveru pomocí [Importovat Data] [ import-data] modul. Vložte potřebné dotaz, který extrahuje pole vytvoří funkcí a vzorky dat v případě potřeby přímo v okně [Importovat Data] [ import-data] dotazu.

9. Jednoduchý výukové počítače Azure experimentovat toku počínaje nahraný datovou sadu.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternativní metody zkopírujte úplnou databáze z místního SQL serveru k databázi SQL Azure

![Odpojení místní databáze a připojení k SQL databáze v Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další zdroje informací Azure: Azure virtuálního počítače (SQL Server nebo Poznámkový blok IPython serveru)

Replikace celou databázi SQL serveru ve vaší OM SQL Server, měli byste zkopírovat databáze z jednoho umístění/serveru do jiné, za předpokladu, že databázi můžete zpřístupnit dočasně offline. Můžete to udělat v SQL Server Management Studio Object Explorer nebo příkazy odpovídající jazyce Transact-SQL.

1. Odpojení databází v umístění zdroje. Další informace najdete v tématu [Odpojit databáze](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. V okně Průzkumníka Windows nebo Windows příkazového řádku zkopírujte oddělí databázový soubor nebo soubory a soubor protokolu nebo soubory cílové umístění na OM SQL serveru v Azure.
3. Připojte k němu zkopírovaného soubory cílové instance serveru SQL Server. Další informace najdete v tématu [připojení databáze](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Přesunutí databáze pomocí odpojení a připojit (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Scénář \#7: velký dat v místní soubory, používat databázi podregistru v Azure HDInsight Hadoop clusterů

![Velký data v místním cílové podregistru][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Další zdroje informací Azure: Azure HDInsight Hadoop obrázku a Azure virtuálního počítače (Poznámkový blok IPython server)

1.  Vytvoření Azure virtuálního počítače serverem IPython Poznámkový blok.

2.  Vytvoření Azure HDInsight Hadoop clusteru.

3.  (Volitelné) Předem zpracovat a vymazání data.

    na.  Předem zpracovat a vymazání data v poznámkovém bloku IPython přístup k datům z objektů BLOB Azure.

    b.  Transformace dat vyčistit, formátu tabulky podle potřeby.

    c.  Uložení dat OM místní soubory (Poznámkový blok IPython běží na OM, místní jednotky odkažte OM jednotky).

4.  Odeslání dat do kontejneru výchozí Hadoop clusteru vybrané v kroku 2.

5.  Načtení dat do databáze podregistru v Azure HDInsight Hadoop obrázku.

    na.  Přihlaste se k hlavního uzlu Hadoop obrázku

    b.  Otevřete Hadoop příkazového řádku.

    c.  Zadejte kořenovém adresáři podregistru příkazem `cd %hive_home%\bin` v Hadoop příkazového řádku.

    d.  Spouštění dotazů podregistru vytvořit databázi a tabulky a načtení dat z úložiště objektů blob podregistru tabulek.

    > [AZURE.NOTE] Pokud jsou data velký, mohou uživatelé vytvářet tabulku podregistru oddílů. Potom uživatelé můžou použít `for` smyčka v Hadoop příkazového řádku na uzel hlavy k načtení dat do tabulku podregistru oddíly oddíl.

6.  Zkoumání dat a vytváření funkcí v případě potřeby v Hadoop příkazového řádku. Všimněte si, že funkce nemusí být nastala v databázových tabulkách. Poznámka: pouze potřebné dotaz tak, aby byla vytvořená.

    na.  Přihlaste se k hlavního uzlu Hadoop obrázku

    b.  Otevřete Hadoop příkazového řádku.

    c.  Zadejte kořenovém adresáři podregistru příkazem `cd %hive_home%\bin` v Hadoop příkazového řádku.

    d.  Spouštění dotazů podregistru v Hadoop příkazového řádku na uzel hlavy clusteru Hadoop zkoumat data a podle potřeby vytvářet funkce.

7.  Pokud potřeby a/nebo žádoucí, ukázková data v Azure počítače výukové Studio na šířku.

8.  Přihlaste se k [Azure počítače výukové Studio](https://studio.azureml.net/).

9. Načtení dat přímo z `Hive Queries` používat [Importovat Data] [ import-data] modul. Vložte potřebné dotaz, který extrahuje pole vytvoří funkcí a vzorky dat v případě potřeby přímo v okně [Importovat Data] [ import-data] dotazu.

10. Jednoduchý výukové počítače Azure experimentovat toku počínaje nahraný datovou sadu.

## <a name="decisiontree"></a>Rozhodovací strom scénáři výběru
------------------------

Následující schéma shrnuje scénáře jsme je popsali výše a Upřesnit proces analýzy a technologie volba, která vás zavedou ke každé rozepsané scénáře. Všimněte si, že zpracování dat, průzkum, inženýrské funkce a odběr může trvat umístit do jedné nebo více metoda/prostředí – na straně zdroje, intermediate, a/nebo cílové prostředí – a může ovládací opakované podle potřeby. Diagram pouze slouží jako obrázek některé z možných toků a neposkytuje vyčerpávající výčet.

![Ukázka DS proces návodu scénáře][8]

### <a name="advanced-analytics-in-action-examples"></a>Pokročilé technologie pro analýzu v akci příklady

Návody Azure počítače výukové začátku do konce, které využívají pokročilé technologie pro analýzu procesy a technologii pomocí veřejných datových sadách účely najdete v článku:


* [Týmu dat pro výzkum obrázku v akci: pomocí serveru SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Týmu dat pro výzkum obrázku v akci: použití HDInsight Hadoop clusterů](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
