<properties
    pageTitle="Připojení k databázi SQL pomocí Python | Microsoft Azure"
    description="Představuje vzorek Python kód, který používáte pro připojení k databázi SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Připojení k databázi SQL pomocí Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Toto téma ukazuje, jak se můžete připojit a dotaz databázi SQL Azure pomocí Python. Tento příklad lze spustit z Windows, se systémem Ubuntu Linux nebo Mac platformách.


## <a name="step-1-create-a-sql-database"></a>Krok 1: Vytvoření databáze SQL

Najdete v článku [Stránka Začínáme](sql-database-get-started.md) se dozvíte, jak vytvořit ukázkovou databázi.  Je důležité, že které sledujete průvodce k vytvoření **šablony databáze AdventureWorks**. Ukázky ukázáno v následujícím příkladu fungují jen **AdventureWorks schéma**. Jakmile vytvoříte uděláte databáze se povolíte přístup k IP adrese povolíte pravidla brány firewall podle popisu v [Stránka Začínáme](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Krok 2: Nakonfigurujte vývojové prostředí:

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Instalace požadovaných moduly
Otevřete svůj terminál a instalace

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (se systémem Ubuntu)**

Otevřete svůj terminál a přejděte v adresáři, ve které se chystáte týkající se vytváření vašeho python skriptu. Zadejte následující příkazy k instalaci **FreeTDS** a **pymssql**. pymssql FreeTDS používá pro připojení k databázím SQL.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Nainstalujte pymssql z [**tady**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Zkontrolujte, že zvolíte soubor správné whl. Příklad: Pokud používáte Python 2.7 na počítač 64-bit zvolte: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Po stažení souboru .whl jeho umístění ve složce C:/Python27.

Teď nainstalujte pymssql pomocí pip z příkazového řádku. disk CD do C:/Python27 a spusťte následující
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Pokyny, jak povolit použití pip najdete [tady](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Krok 3: Spustit ukázkový kód

Vytvoření souboru s názvem **sql_sample.py** kód a vložte následující uvnitř. Lze ji spustit pomocí příkazového řádku:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Připojení k databázi SQL

Funkce [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) se používá pro připojení k databázi SQL.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Provedení příkazu SQL SELECT

Funkci [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) lze použít k načítání výsledků nastavte z dotazu v databázi SQL. Tato funkce v podstatě přijme dotazy a vrátí výsledek nastavení, které můžete vstupní přes s volbou [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Vložení řádku, předat parametry a načíst vygenerovaných primárního klíče

V SQL databázi vlastnost [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx) a [pořadí](https://msdn.microsoft.com/library/ff878058.aspx) objektů mohou sloužit k automaticky generované hodnotami [primárního klíče](https://msdn.microsoft.com/library/ms179610.aspx) . 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transakce


Tento příklad ukazuje použití transakce ve kterém můžete:

* Začněte transakce
* Vložení řádku dat
* Vrácení transakce zpět vložit 

Vložte tento kód uvnitř sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Další kroky

* Projděte si [Přehled vývoje databáze SQL](sql-database-develop-overview.md)
* Další informace o [Ovladač Python společnosti Microsoft pro systém SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Navštivte [Středisko pro vývojáře Python](/develop/python/).

## <a name="additional-resources"></a>Další zdroje informací 

* [Provedeních více klienta SaaS aplikací se databáze Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Prozkoumat všechny [Možnosti SQL databáze](https://azure.microsoft.com/services/sql-database/)
