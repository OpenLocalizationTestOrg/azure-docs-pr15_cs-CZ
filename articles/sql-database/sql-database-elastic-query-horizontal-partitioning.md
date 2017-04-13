<properties
    pageTitle="Vytváření sestav v cloudu rozšířených databází | Microsoft Azure"
    description="jak nastavit pružná dotazů přes vodorovné oddíly"    
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

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Vytváření sestav v cloudu rozšířených databáze (verze preview)

![Dotaz na shards][1]

Sharded databází distribuování řádků v škálovanou se data osy. Schéma je u všech zúčastněných databází, označovaná jako vodorovné rozdělení shodné. Použití pružná dotazu, můžete vytvářet sestavy, které zahrnují všechny databáze sharded databáze.

Rychlý start najdete v článku [zasílání zpráv o chybách v cloudu rozšířených databází](sql-database-elastic-query-getting-started.md).

Sharded databází najdete v článku [dotazu v cloudu databáze s různými schématy](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Vytvoření mapy shard pomocí klienta knihovny pružná databáze. v tématu [Shard mapování správy](sql-database-elastic-scale-shard-map-management.md). Nebo je používat aplikaci ukázkové [začít pracovat s pružná databázové nástroje](sql-database-elastic-scale-get-started.md).
* Můžete taky v tématu [migrace existující databáze do diagramů s měřítky mimo databází](sql-database-elastic-convert-to-use-elastic-tools.md).
* Uživatel musí mít oprávnění upravit jakékoli externí zdroj dat. Toto oprávnění je součástí oprávnění vlastnosti databáze.
* Odkázat na podkladového zdroje dat jsou nutná oprávnění upravit jakékoli externí zdroj dat.

## <a name="overview"></a>Základní informace

Tyto příkazy vytvořit metadat znázornění osy sharded dat v databázi pružná dotazu. 


1. [VYTVOŘTE HLAVNÍ KLÍČ](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VYTVOŘENÍ DATABÁZE OMEZENÉ PŘIHLAŠOVACÍCH ÚDAJŮ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VYTVOŘENÍ EXTERNÍHO ZDROJE DAT](https://msdn.microsoft.com/library/dn935022.aspx)
4. [VYTVOŘIT EXTERNÍ TABULKA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 vytvořit databázi omezené hlavní klíč a přihlašovací údaje 

Pověření umožňuje pružná dotazem připojení ke vzdálené databáze.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Ujistěte se, že *"\<uživatelské jméno\>"* neobsahuje žádné *"@servername"* přípona. 

## <a name="12-create-external-data-sources"></a>1.2 vytvořit externí zdroje dat

Syntaxe:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Příklad 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Načtení seznamu aktuální externím zdrojům dat: 

    select * from sys.external_data_sources; 

Externí zdroj dat odkazuje shard mapu. Pružná dotazu pomocí externího zdroje dat a základní mapy shard umožňuje zobrazit výčet databází, které jsou součástí osy dat. Stejné přihlašovací údaje se používají k mapu shard číst a přístup k datům v shards během zpracování pružná dotazu. 

## <a name="13-create-external-tables"></a>1.3 vytvořit externí tabulky 
 
Syntaxe:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Příklad**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Načtení seznamu externích tabulek z aktuální databáze: 

    SELECT * from sys.external_tables; 

Chcete-li zrušit externí tabulky:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Poznámky:

DATA\_zdroj klauzule definuje zdroje externích dat (shard mapa), který se používá pro externí tabulka.  

SCHÉMA\_název a OBJEKT\_název klauzule mapování definici externí tabulky do tabulky v jiné schéma. Pokud není zadáno, schématu vzdálený objekt považován "dbo" a dosadí odpovídá názvu externí tabulka definovaného názvu. Toto je užitečné, když název vzdáleného tabulky, se už považuje v databázi místo, kam chcete vytvořit externí tabulka. Například chcete definovat externí tabulka získat agregované zobrazení z katalogu zobrazení nebo DMVs na datech škálovanou mimo úroveň. Protože katalogu zobrazení a DMVs již existuje místně, nelze použít názvy pro externí definice. Místo toho použít jiný název a použijte zobrazení katalogu nebo DMV název ve schématu\_název a/nebo OBJEKT\_klauzule název. (Viz Příklad dole). 

Klauzule rozdělení určuje distribuce dat použít v této tabulce. Zpracování dotazů využívá údaje v klauzuli rozdělení vytvářet nejefektivnější plány dotazů.  

1. **SHARDED** znamená, že data ve vodorovném směru rozdělený napříč databází. Rozdělení klíč pro distribuci dat je parametr **< sharding_column_name >** .
2. **REPLIKOVANÁ** znamená, že jsou stejné kopií tabulky prezentovat na každou databázi. Je odpovědností zajistit replik stejná napříč databází.
3. **ZAOKROUHLIT\_Petra** znamená, že v tabulce ve vodorovném směru rozdělena metodou rozdělení závislé na aplikace. 

**Data úroveň odkaz**: Externí tabulka DDL se vztahuje k externímu zdroji dat. Externí zdroj dat určuje shard mapu, která nabízí informace potřebné k najděte všechny databáze ve vrstvě data v externí tabulce. 


### <a name="security-considerations"></a>Otázky bezpečnosti pro 

Uživatelé s přístupem k tabulce externí automaticky získat přístup k vzdálené podkladové tabulky v části přihlašovací údaje uvedený v tématu definice zdroje externích dat. Vyhněte se nežádoucí zvýšení oprávnění pomocí přihlašovacích údajů zdroje externích dat. Použití udělit nebo ODVOLAT pro externí tabulku stejně, jako by byly normální tabulka.  

Po definování externího zdroje dat a externí tabulkách teď můžete celou T-SQL přes externí tabulkách.

## <a name="example-querying-horizontal-partitioned-databases"></a>Příklad: vodorovné rozdělený databázových dotazů 

Následující dotaz provádí tři způsob spojení mezi sklady, objednávky a řádky a používá několik agregace a výběrové filtru. Předpokládá (1) vodorovné rozdělení (sharding) a (2), že sklady, objednávky a řádky jsou sharded ve sloupci id sklad a, pružná dotazu můžete společně vyhledejte spojení v shards a zpracovat drahé část dotaz na shards paralelně. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
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

Běžná řetězce připojení SQL serveru použít pro připojení aplikace, vaše BI a datové nástroje pro integraci k databázi s definicemi externí tabulka. Ujistěte se, že SQL Server je podporované jako zdroje dat pro nástroj. Potom odkaz databázi pružná dotazu jako jakékoli jiné databáze SQL serveru připojení k nástroji a používání externích tabulek z nástroje nebo aplikace jako kdyby to byl místní tabulky. 

## <a name="best-practices"></a>Doporučené postupy 

* Ujistěte se, že databázi koncový bod pružná dotazu byl udělen přístup k databázi shardmap a všechny shards bran SQL databáze.  

* Ověření nebo vynutit distribuce dat definované v externí tabulce. Pokud rozdělení vlastních dat se liší od rozdělení podle definice tabulky, dotazy výnos neočekávané výsledky. 

* Pružná dotazu aktuálně neprovádí shard odstranění při predikáty myši klávesu sharding by mohla bezpečně vyloučíte určité shards z zpracování.

* Pružná dotazu je nejvhodnější pro dotazy kde většina výpočtu lze provést v shards. Obvykle optimálního výkonu dotazu s predikáty výběrové filtru, které může být vyhodnocen jako na získáte shards nebo spojení přes rozdělení klávesy, pomocí kterých lze provést způsobem oddíl zarovnaný na všechny shards. Další vzorce dotazu může být nutné načtení velké objemy dat z shards do hlavního uzlu a může provádět špatně

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
