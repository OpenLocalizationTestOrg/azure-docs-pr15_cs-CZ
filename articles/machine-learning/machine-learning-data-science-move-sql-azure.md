<properties 
    pageTitle="Přesunutí dat do databáze SQL Azure pro Azure počítače výukové | Azure" 
    description="Vytvoření tabulky SQL a načtení dat k tabulce SQL" 
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
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Přesunutí dat do databáze SQL Azure pro vzdělávání počítače Azure

Toto téma popisuje možnosti pro přesunutí dat z textových souborů (formátování souboru CSV nebo TSV) nebo z dat uložených na SQL serveru místní databázi Azure SQL. Tyto úkoly přesunutí dat do cloudu jsou součástí procesu týmu dat pro výzkum.

Téma, které popisuje možnosti pro přesunutí dat na serveru SQL místní pro počítač výukové najdete v článku [přesunutí dat na serveru SQL Server Azure virtuální počítače](machine-learning-data-science-move-sql-server-virtual-machine.md).

Následující **nabídky** odkazy na témata, která popisují, jak jedí data do cílové prostředí, kde můžete data uložená a zpracování během týmu dat pro výzkum obrázku (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Následující tabulka shrnuje možnosti pro přesunutí dat do databáze SQL Azure.

<b>ZDROJE</b> |<b>Cíl: Azure SQL databáze</b> |
-------------- |--------------------------------|
<b>Plochému souboru (CSV nebo TSV formátované)</b> |<a href="#bulk-insert-sql-query">Dotaz SQL zobrazený hromadné vložení |
<b>Místnímu serveru SQL</b> | 1. <a href="#export-flat-file">export k plochému souboru<br> 2. <a href="#insert-tables-bcp">Průvodce přenesením databáze SQL<br> 3. <a href="#db-migration">databáze back nahoru a obnovení<br> 4. <a href="#adf">azure Data Factory |


## <a name="prereqs"></a>Zjistit předpoklady pro
Postupy uvedené v tomto poli nutné, abyste měli:

* **Azure předplatného**. Pokud nemáte předplatné, můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure úložiště**. Používáte účet Azure úložiště pro ukládání data v tomto kurzu. Pokud nemáte účet Azure úložiště, podívejte se na článek [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) . Po vytvoření účtu úložiště, musíte získat klíč účtu použitých při přístupu k úložišti. V tématu [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Přístup k **databázi Azure SQL**. Pokud musíte nastavit databáze SQL Azure, [Začínáme s Microsoft Azure SQL databáze](../sql-database/sql-database-get-started.md) obsahuje informace o tom, jak vytvořit nové instance databáze SQL Azure.
* Nainstalovanou a nakonfigurovanou **Azure PowerShell** místně. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

**Data**: procesu migrace jsou prokázáno pomocí [NYC Taxi datovou sadu](http://chriswhong.com/open-data/foil_nyc_taxi/). Datová sada NYC Taxi obsahuje informace o data ze služební cesty a veletrhy a neexistuje, jak je uvedeno tento příspěvek v úložišti objektů blob Azure: [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/). Výběr a popis tyto soubory jsou uvedeny v [NYC Taxi cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Můžete přizpůsobit popsaných do sady vlastních datech procedur nebo postupujte podle pokynů pomocí NYC Taxi datovou sadu. K nahrání datovou sadu NYC Taxi do vaší místní databáze SQL serveru, podle pokynů uvedených v [Hromadné Import dat do databáze systému SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Tyto pokyny jsou určené pro systém SQL Server na Azure virtuálního počítače, ale postup pro odeslání do místní SQL Server je stejný.


## <a name="file-to-azure-sql-database"></a>Přesunutí dat ze zdroje plochému souboru k databázi Azure SQL

K databázi Azure SQL pomocí hromadného vložit dotaz SQL přesouvat data v textových souborů (CSV nebo TSV naformátované).

### <a name="bulk-insert-sql-query"></a>Dotaz SQL zobrazený hromadné vložení

Kroky pro postup pomocí hromadného vložit SQL dotazu se podobají uvedených v částech přesunutí dat ze zdroje plochému souboru na serveru SQL Server Azure OM. Podrobnosti najdete v tématu [Vložení dotaz SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Přesunutí dat z místních SQL serveru k databázi Azure SQL

Pokud se zdrojovými daty je uložený na místním SQL Server, existují různé možnosti pro přesunutí dat do databáze Azure SQL:

1. [Export k plochému souboru](#export-flat-file) 
2. [Průvodce migrací databáze SQL](#insert-tables-bcp)
3. [Databáze back nahoru a obnovení](#db-migration)
4. [Azure Data Factory](#adf)

Kroky pro první tři jsou velmi podobné [Přesunutí](machine-learning-data-science-move-sql-server-virtual-machine.md) dat na serveru SQL Server Azure počítače virtuální oddíly, které průvodní stejných kroků. Odkazy na příslušné oddíly v tomto tématu, jsou uvedeny v následující pokyny.

###<a name="export-flat-file"></a>Export k plochému souboru

Postup při exportu k plochému souboru podobají uvedených ve [Exportovat k plochému souboru](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Průvodce migrací databáze SQL

Kroky pro pomocí Průvodce přenesením databáze SQL jsou podobné těm, které jsou součástí [Průvodce přenesením databáze SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Databáze back nahoru a obnovení

Kroky pro použití databáze zálohování a obnovení podobají uvedených v [databázi, zálohování a obnovení](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure Data Factory

Postup přesunutí dat do databáze Azure SQL pomocí Azure dat Factory (ADF) je k dispozici v tématu [přesunutí dat ze serveru SQL místní na SQL Azure pomocí Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md). Toto téma ukazuje, jak chcete přesunout data z databáze SQL serveru místní k databázi Azure SQL pomocí úložiště objektů Blob Azure pomocí ADF. 

Zvažte použití ADF je třeba neustále migrovat ve scénáři hybridní, který má přístup k místním i cloudové zdroje dat a pokud, data je zpracováván jako transakce nebo musí změnit nebo máte obchodní logiky do něj přidali při migrací. ADF umožňuje plánování a sledování úlohy pomocí jednoduché skripty JSON Správa pohybu dat v pravidelných intervalech. ADF má i další funkce, například podpora složité operace.




