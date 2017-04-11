<properties 
    pageTitle="Zkoumání dat v SQL Server virtuálního počítače na Azure | Microsoft Azure" 
    description="Jak zkoumat data, která je uložena v OM Server SQL na Azure." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Zkoumání dat v SQL Server virtuálního počítače na Azure


Tento dokument obsahuje informace, které můžete prozkoumat data, která je uložena v OM Server SQL na Azure. Stačí dat wrangling pomocí jazyka SQL nebo pomocí programovací jazyk jako Python.

Následující **nabídky** odkazy na témata, která popisují, jak používat nástroje, které můžete prozkoumat data z různých prostředích úložiště. Tento úkol je kroku v procesu Cortana analýzy (Zakončení).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Příkazy SQL vzorku v tomto dokumentu Předpokládejme, že data je na serveru SQL Server. Pokud není, v nápovědě k mapování cloudu dat pro výzkum proces se dozvíte, jak přesunout data SQL serveru.



## <a name="sql-dataexploration"></a>Zkoumání dat SQL pomocí skriptů SQL

Tady je několik ukázky skriptů SQL, které lze použít k prohlížení úložiště dat v SQL Server.

1. Zjištění počtu pozorování denně

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Získání úrovně do kategorií sloupce

    `select  distinct <column_name> from <databasename>`

3. Počet úrovní vstoupit spojení dvou sloupců kategorií 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Získání rozdělení pro číselné sloupce

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Praktické například můžete použít [sadu NYC Taxi](http://www.andresmh.com/nyctaxitrips/) a v nápovědě k IPNB s názvem [NYC Data wrangling pomocí IPython Poznámkový blok a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro walk-through začátku do konce.

##<a name="python"></a>Zkoumání dat SQL s Python

Zkoumání dat a generovat funkce když data jsou na serveru SQL Server pomocí Python je podobný zpracování dat v objektů blob Azure pomocí Python, jak je uvedeno v [objektů Blob Azure proces dat ve vašem prostředí pro výzkum data](machine-learning-data-science-process-data-blob.md). Data musí být načte z databáze do pandas DataFrame a pak můžete další zpracování. Proces připojování k databázi a načítání dat do DataFrame v této části uvádíme.

Následující formát připojovacího řetězce mohou sloužit k připojení k databázi SQL serveru z Python pomocí pyodbc (webu název serveru nahradit dbname, uživatelské jméno a heslo specifické hodnoty):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Knihovna Pandas](http://pandas.pydata.org/) v Python poskytuje celá řada datové struktury a nástrojů pro analýzu dat pro manipulaci s daty programování Python. Následující kód přečte výsledky vrácených z databáze SQL serveru do rámečku Pandas dat:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teď můžete pracovat s Pandas DataFrame podle pokynů v tématu [objektů Blob Azure proces dat ve vašem prostředí pro výzkum data](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Proces analýzy Cortanu v příkladu akce

Příklad začátku do konce návodu proces analýzy Cortana pomocí veřejných datovou sadu, najdete v článku [týmu dat pro výzkum proces v akci: pomocí serveru SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
