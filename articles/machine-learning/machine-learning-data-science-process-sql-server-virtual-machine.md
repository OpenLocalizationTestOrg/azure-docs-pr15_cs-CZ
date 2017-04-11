<properties 
    pageTitle="Zpracování dat v databázi SQL Azure | Microsoft Azure" 
    description="Proces dat z databáze SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Proces dat v SQL Server virtuálního počítače na Azure

Tento dokument obsahuje informace o zkoumání dat a generovat funkcí pro data uložená v OM Server SQL na Azure. Stačí dat wrangling pomocí jazyka SQL nebo pomocí programovací jazyk jako Python.


> [AZURE.NOTE] Příkazy SQL vzorku v tomto dokumentu Předpokládejme, že data je na serveru SQL Server. Pokud není, získáte cloudu pro výzkum proces mapování se dozvíte, jak přesunout data SQL serveru.

##<a name="SQL"></a>Pomocí jazyka SQL

Jsme popisují následující data wrangling úkoly v této části pomocí jazyka SQL:

1. [Průzkum dat](#sql-dataexploration)
2. [Funkce generování](#sql-featuregen)

###<a name="sql-dataexploration"></a>Průzkum dat
Tady je několik ukázky skriptů SQL, které lze použít k prohlížení úložiště dat v SQL Server.


> [AZURE.NOTE] Praktické například můžete použít [sadu NYC Taxi](http://www.andresmh.com/nyctaxitrips/) a v nápovědě k IPNB s názvem [NYC Data wrangling pomocí IPython Poznámkový blok a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro walk-through začátku do konce.

1. Zjištění počtu pozorování denně

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Získání úrovně do kategorií sloupce

    `select  distinct <column_name> from <databasename>`

3. Počet úrovní vstoupit spojení dvou sloupců kategorií 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Získání rozdělení pro číselné sloupce

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Funkce generování

V této části Popis možností, jak vytvářet funkce pomocí jazyka SQL:  

1. [Určit počet výskytů funkce generování](#sql-countfeature)
2. [Analytický nástroj Generátor binning funkce](#sql-binningfeature)
3. [Zavádění funkce z jednoho sloupce](#sql-featurerollout)


> [AZURE.NOTE] Po generovat další funkce můžete přidat ve sloupcovém grafu do existující tabulky nebo vytvořit novou tabulku s dalšími funkcemi a primárního klíče, který může být připojen s původní tabulky. 

###<a name="sql-countfeature"></a>Určit počet výskytů funkce generování

Tento dokument znázorňuje dva způsoby vytváření funkce count. První metoda používá podmíněným součtem a druhý způsob klauzule where". Tyto lze potom připojit původní tabulku (pomocí sloupce primárního klíče) mít funkce count společně s původní data.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Analytický nástroj Generátor binning funkce

Následující příklad ukazuje, jak generovat binned funkce podle binning (pomocí 5 intervalů) číselné sloupce, který slouží jako funkce místo:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Zavádění funkce z jednoho sloupce

V této části jsme ukazují, jak zavádění jednoho sloupce v tabulce generovat další funkce. V příkladu předpokládá, že je v tabulce, ze kterého se pokoušíte generovat funkce šířky nebo šířky sloupců.

Tady je stručný základy zeměpisná šířka a délka umístění dat (zdroji z stackoverflow [jak měřit přesnost šířky a délky?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). To je užitečné znát před featurizing pole umístění:

- Znaménko víme, zda jsme severní nebo jih a východ západní na světě.
- Nenulová stovky číslice víme Pracujeme s programem délky, ne šířky!
- Desítky číslice nabízí pozice na o produktu 1 000 km. Je nám užitečné informace o tom, jaké kontinentu nebo prezentace s mořskými jsme v.
- Jednotky číslici (jedno desetinné místo míru) vám bude radit pozici 111 km (60 námořní míle, asi 69 mil). Ho můžete přesně určit nám zhruba co velké stavy nebo země, kterou jsme v.
- Na jedno desetinné místo stojí až 11.1 km: můžete rozlišit pozici jeden velký města z sousedních velké město.
- Dvě desetinná místa stojí až 1.1 km: ho oddělili jeden vesnice od dalšího.
- Můžete určit velké zemědělské pole nebo institucionální areálu jmění až 110 m je na tři desetinná místa.
- Můžete určit balení Nevyžádaná pošta je přesunuta jmění až 11 m je čtvrtý desetinné místo. Je srovnatelná s typické přesnost neopravených jednotku GPS s žádná.
- Pátý desetinné místo stojí až 1.1 m je rozlišení stromy od sebe. Přesnost k této úrovni s komerční GPS jednotek lze dosáhnout pouze s rozdílné oprav.
- Šestým desetinné místo stojí až 0,11 m, že které lze použít toto rozložení struktury podrobně pro navrhování krajiny, vytváření cest. By měl být více než to nestačí ke sledování pohybu glaciers a řeky. Lze dosáhnout tak, že painstaking opatření s GPS, například differentially opravený GPS.

Informace o umístění lze featurized následujícím způsobem oddělení oblast, umístění a nejsou informace. Všimněte si, že můžete taky volat ZBÝVAJÍCÍ koncový bod například API mapy Bing k dispozici na [najít místo čárky](https://msdn.microsoft.com/library/ff701710.aspx) získat informace o oblasti/oblasti.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Funkce výše uvedených na základě umístění další lze generovat další počet funkce jak bylo popsáno dříve. 


> [AZURE.TIP] Můžete vložit programově záznamy pomocí jazyka podle výběru. Budete muset vložte data v blocích pro zvýšení efektivity zápis (například udělat pomocí pyodbc naleznete v tématu [Hello World A ukázkové pro přístup k SQL Server s python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Další možností je chcete vložit data v databázi pomocí [nástroje BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Připojení k výukové Azure počítače

Funkce nově generovaného lze přidat jako sloupec do existující tabulky nebo uložené v nové tabulce a spojené s původní tabulka výukové počítače. Funkce můžete generovaného nebo k nim získat přístup Pokud už vytvořili, pomocí [Importovat Data] [ import-data] modul Azure počítače dozvědět, jak je ukázáno v následujícím příkladu:

![azureml Čtenář][1] 

##<a name="python"></a>Použití programovací jazyk jako Python

Zkoumání dat a generovat funkce když data jsou na serveru SQL Server pomocí Python je podobný zpracování dat v objektů blob Azure pomocí Python, jak je uvedeno v [objektů Blob Azure proces data v můžete data pro výzkum prostředí](machine-learning-data-science-process-data-blob.md). Data načíst z databáze do rámečku pandas dat je potřeba a pak můžete další zpracování. Proces připojování k databázi a načítání dat do rámečku data v této části uvádíme.

Následující formát připojovacího řetězce mohou sloužit k připojení k databázi SQL serveru z Python pomocí pyodbc (nahradit webu název serveru, dbname, uživatelské jméno a heslo specifické hodnoty):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Knihovna Pandas](http://pandas.pydata.org/) v Python poskytuje celá řada datové struktury a nástrojů pro analýzu dat pro manipulaci s daty programování Python. Následující kód přečte výsledky vrácených z databáze SQL serveru do rámečku Pandas dat:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teď můžete pracovat s rámečku Pandas data podle pokynů v článku [objektů Blob Azure proces data v můžete data pro výzkum prostředí](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Výzkum Azure dat v příkladu akce

Příklad začátku do konce návodu proces pro výzkum dat Azure pomocí veřejných datovou sadu najdete v článku [Proces pro výzkum dat Azure v akci](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
