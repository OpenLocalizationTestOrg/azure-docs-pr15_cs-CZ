<properties
    pageTitle="Dotaz v cloudu databáze pomocí různých schématu | Microsoft Azure"
    description="jak nastavit křížově databázových dotazů přes svislé oddíly"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Dotaz na cloud databáze s různými schématy (verze preview)

![Dotaz na tabulky do jiné databáze][1]

Svisle oddíly databáze používají různé sady tabulek na jiné databáze. Která znamená, že schéma různých na jiné databáze. Například jsou všechny tabulky zásob na jednu databázi a všechny související účetní tabulky ve druhém databázi. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Uživatel musí mít oprávnění upravit jakékoli externí zdroj dat. Toto oprávnění je součástí oprávnění vlastnosti databáze.
* Odkázat na podkladového zdroje dat jsou nutná oprávnění upravit jakékoli externí zdroj dat.

## <a name="overview"></a>Základní informace

**Poznámka**: na rozdíl od s vodorovné rozdělení, výkazy DDL nezávisí na definování osy dat s mapou shard přes klienta knihovnu pružná databáze.

1. [VYTVOŘTE HLAVNÍ KLÍČ](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VYTVOŘENÍ DATABÁZE OMEZENÉ PŘIHLAŠOVACÍCH ÚDAJŮ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VYTVOŘENÍ EXTERNÍHO ZDROJE DAT](https://msdn.microsoft.com/library/dn935022.aspx)
4. [VYTVOŘIT EXTERNÍ TABULKA](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Vytvoření databáze omezené hlavní klíč a přihlašovací údaje 

Pověření umožňuje pružná dotazem připojení ke vzdálené databáze.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Poznámka:**    Zkontrolujte, že je *<username>* neobsahuje žádné *"@servername"* přípona. 

## <a name="create-external-data-sources"></a>Vytvořit externí zdroje dat

Syntaxe:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Důležité**   Parametr TYPE musíte nastavit **RDBMS**. 

### <a name="example"></a>Příklad 

Následující příklad ukazuje použití příkazu vytvořit pro externí zdroje dat. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Načtení seznamu aktuální externím zdrojům dat: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externí tabulky 

Syntaxe:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Příklad  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Následující příklad ukazuje, jak načíst seznam externích tabulek z aktuální databáze: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Poznámky:

Pružná query rozšíří existující syntaxe externí tabulka definovat externí tabulky, které používají externím zdrojům dat typu RDBMS. Externí tabulka definice pro svislé rozdělení zahrnuje následující aspekty: 

* **Schéma**: Externí tabulka DDL definuje schéma, které můžete použít svoje dotazy. Uvedenou v externí tabulce definice schématu musí odpovídat schématu tabulek v Vzdálená databáze, kde je uložena vlastních dat. 

* **Vzdálená databáze odkaz**: Externí tabulka DDL se vztahuje k externímu zdroji dat. Externí zdroj dat Určuje název logické serveru a název databáze Vzdálená databáze, kde je uložena data skutečného tabulky. 

Pomocí externího zdroje dat, jak je uvedeno v předchozí části, přesvědčte se, vytvořit externí tabulky vypadá takto: 

Klauzule DATA_SOURCE definuje zdroji externích dat (tedy Vzdálená databáze pro nečekané svislé oddílů), který se používá pro externí tabulka.  

Klauzule SCHEMA_NAME a název_objektu – umožňují mapování definici externí tabulky do tabulky v jiné schéma vzdálené databázi a tabulku s jiným názvem, respektive. Toto je užitečné, když chcete definovat externí tabulku do katalogu zobrazení nebo DMV na vzdálené databázi – nebo jiné situaci, kde název vzdáleného tabulky, se už považuje místně.  

Následující příkaz DDL vynechává definici stávající externí tabulky z místní katalogu. Vzdálená databáze nemá vliv. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Oprávnění pro externí tabulka vytvořit/rozevírací**: oprávnění upravit jakékoli externí zdroj dat jsou potřebné pro externí tabulka DDL, které je taky potřeba odkázat na podkladovém zdroji dat.  

## <a name="security-considerations"></a>Otázky bezpečnosti pro
Uživatelé s přístupem k tabulce externí automaticky získat přístup k vzdálené podkladové tabulky v části přihlašovací údaje uvedený v tématu definice zdroje externích dat. Přístup k tabulce externí by měl pečlivě spravovat a zabránit tak nežádoucí zvýšení oprávnění pomocí přihlašovacích údajů zdroje externích dat. Běžná SQL oprávnění lze udělit nebo ODVOLAT přístup k tabulce externí stejně, jako by byly normální tabulka.  


## <a name="example-querying-vertically-partitioned-databases"></a>Příklad: dotazování svisle oddíly databází 

Následující dotaz provádí tři způsob spojení mezi dvěma tabulkami místní objednávky a řádky a vzdálené tabulky pro zákazníky. Toto je příklad případu použití dat odkaz pružná dotazu: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Uložená procedura pro vzdálené spuštění T-SQL: sp\_execute_remote

Pružná dotazu zavádí uložené procedury, která obsahuje přímý přístup k shards. Uložená procedura nazývá [sp\_spustit \_vzdálené](https://msdn.microsoft.com/library/mt703714) a lze použít k provedení vzdálené uložené procedury nebo kód T SQL Vzdálená databáze. Trvá následujících parametrů: 

* Název zdroje dat (nvarchar): název externí zdroj dat typu RDBMS. 
* Dotaz (nvarchar): provedení na každé shard dotazu T-SQL. 
* Deklarace parametru (nvarchar) – volitelné: řetězec s definice typů dat pro parametry použité v parametru dotazu (třeba sp_executesql). 
* Seznam hodnot parametr – volitelné: čárkou oddělený seznam hodnot parametrů (třeba sp_executesql).

Sp\_spustit\_vzdáleného použití externího zdroje dat podle Parametry vyvolání na vzdálená databáze provést příkaz dané T-SQL. Použije pověření externího zdroje dat pro připojení k databázi shardmap správce a Vzdálená databáze.  

Příklad: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 


  
## <a name="connectivity-for-tools"></a>Připojení pro nástroje

Běžná řetězce připojení SQL serveru můžete použít pro připojení vaší nástroje pro integraci BI a dat do databáze na serveru SQL databáze, který má pružná dotazu povolené a externí tabulky definované. Ujistěte se, že SQL Server je podporované jako zdroje dat pro nástroj. Pak v nápovědě k databázi pružná dotazu a její externí tabulky stejně jako jakékoli jiné databáze SQL serveru, ke kterému chcete připojit pomocí svého nástroje. 

## <a name="best-practices"></a>Doporučené postupy 
 
* Ujistěte se, že databázi koncový bod pružná dotazu přidělil přístup ke vzdálené databázi povolení přístupu služby Azure v jeho konfigurace brány firewall SQL databáze. Zajistěte také pověření podle definice zdroje externích dat můžete úspěšně přihlásit Vzdálená databáze a nemá oprávnění pro přístup k vzdálené tabulky.  

* Pružná dotazu je nejvhodnější pro dotazy kde většina výpočtu se teď dá Vzdálená databáze. Se obvykle zobrazí optimálního výkonu dotazu s predikáty výběrové filtru, které může být vyhodnocen jako na vzdálená databáze nebo spojení, které můžete provádět úplně vzdálené databázi. Další vzorce dotazu může být nutné načítání velkých objemů dat z Vzdálená databáze a může provádět špatně. 


## <a name="next-steps"></a>Další kroky

K vytvoření dotazu ve vodorovném směru rozdělený databáze (označovaná taky jako sharded databáze), najdete v článku [dotazů přes sharded cloudu databáze (vodorovně oddíly)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
