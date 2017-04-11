<properties 
    pageTitle="Ukázková Data na serveru SQL Server na Azure | Microsoft Azure" 
    description="Ukázková Data v SQL Server na Azure" 
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
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Ukázková data v SQL Server na Azure


Tento dokument znázorňuje vzorová data uložená na serveru SQL Server na Azure SQL nebo Python programovacího jazyka. Také ukazuje, jak přesunout vybraných dat do Azure počítače výukové uložíte do souboru, odeslání do objektů blob Azure a čtení ho do Azure počítače výukové Studio.

Analytický nástroj vzorkování Python používá knihovnu ODBC [pyodbc](https://code.google.com/p/pyodbc/) připojit k serveru SQL Azure a knihovně [Pandas](http://pandas.pydata.org/) pro odběr.

>[AZURE.NOTE] Ukázkový kód SQL v tomto dokumentu předpokládá, že data je v serverem SQL Azure. Pokud není, získáte [přesouvat data SQL serveru na Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) téma návod k přesunutí dat systému SQL Server na Azure.

**Proč ukázková data?**
Pokud datovou sadu, které chcete analyzovat velký, není vhodné dolů ukázková data, která chcete zmenšit velikost menší, ale zástupce a lépe. To usnadňuje Principy dat, průzkum a inženýrské funkce. Jeho role v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je to povolíte rychlé vytváření prototypů z funkcí zpracování dat a automatické učení modely.

**Nabídka** pod odkazů na témata, která popisují, jak vzorová data z různých prostředích úložiště. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Analytický nástroj vzorkování úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Pomocí jazyka SQL

Tato část popisuje těmito způsoby provádění jednoduchých výběrová proti data v databázi pomocí SQL. Zvolte způsob založený na velikost dat a jejich distribuce.

Dvě položky dole předvedení použití newid na serveru SQL Server provádět odběr. Metodu zvolíte závisí na tom, jak náhodné chcete vzorku, který má být (pk_id v ukázkovém kódu dole se dosadí automaticky generované primárního klíče).

1. Méně striktně výběrová

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Další výběrová 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample může být použita pro odběr i znázorněn. Je-li vaše data velikost velké (za předpokladu, že není vazba dat na různých stránkách) a na dokončení v časového dotazu může být lepší přístup.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Zkoumání a generovat funkce z vybraných dat uložením do nové tabulky


###<a name="sql-aml"></a>Připojení k výukové Azure počítače

Ukázka dotazy nad lze použít přímo v Azure počítače výukové [Importovat Data] [ import-data] modul dolů ukázková data rychlé úpravy a přenést do pokus výukové počítače Azure. Snímek obrazovky s používání modulu čtečka ke čtení vybraných dat jsou uvedeny níže:
   
![čtečky sql][1]

##<a name="python"></a>Použití Python programovacího jazyka 

Tato část ukazuje, jak pomocí [knihovny pyodbc](https://code.google.com/p/pyodbc/) stanovit ODBC připojení k databázi SQL serveru v Python. Připojovací řetězec databáze je takto: (nahraďte webu název serveru, dbname, uživatelské jméno a heslo konfiguraci):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Knihovna [Pandas](http://pandas.pydata.org/) v Python poskytuje celá řada datové struktury a nástrojů pro analýzu dat pro manipulaci s daty programování Python. Následující kód přečte 0,1 % ukázková data z tabulky databáze Azure SQL do Pandas dat:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nyní můžete pracovat s vybraných dat v rámci Pandas data. 

###<a name="python-aml"></a>Připojení k výukové Azure počítače

Následující ukázkový kód umožňuje uložit i data odběr dolů do souboru a nahrajte ho do objektů blob Azure. Data v objektů blob lze přímo přečíst do pokus Azure počítače učení pomocí [Importovat Data] [ import-data] modul. Tyto kroky jsou následujícím způsobem: 

1. Psaní rámečku pandas dat do místního souboru

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Odeslání místní souboru do objektů blob Azure

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Načtení dat z objektů blob Azure pomocí Azure počítače výukové [Importovat Data] [ import-data] modul jak je znázorněno níže obrazovky drapákem:
 
![kulatý Readeru][2]

## <a name="the-team-data-science-process-in-action-example"></a>Proces týmu dat pro výzkum v příkladu akce

Příklad začátku do konce návodu procesu týmu dat pro výzkum pomocí veřejných datovou sadu, najdete v článku [týmu dat pro výzkum obrázku v akci: pomocí serveru SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
