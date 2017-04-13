<properties
   pageTitle="Načtení dat z úložiště objektů blob Azure do SQL datový sklad (PolyBase) | Microsoft Azure"
   description="Naučte se používat PolyBase k načtení dat z úložiště objektů blob Azure do SQL datový sklad. Načtení několik tabulek z ve veřejných datech do schématu Contoso maloobchodní datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Načtení dat z úložiště objektů blob Azure do SQL datový sklad (PolyBase)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Načtení dat z úložiště objektů blob Azure do datový sklad SQL Azure pomocí příkazů PolyBase a T-SQL. 

Tento kurz k v jednoduchosti je krása, načte dvou tabulek z veřejné objektů Blob Azure úložiště do schématu Contoso maloobchodní datový sklad. Načtení celou sadu dat, spusťte příklad [načíst celou Contoso maloobchodní datový sklad][] z úložiště Microsoft SQL Server vzorků.

V tomto kurzu udělejte toto:

1. Konfigurace PolyBase načíst z úložiště objektů blob Azure
2. Načtení veřejných dat do databáze
3. Optimalizace proveďte po dokončení načíst.


## <a name="before-you-begin"></a>Než začnete
Spuštění tohoto kurzu, musíte mít účet Azure obsahující datový sklad SQL databáze. Pokud ještě nemáte to, přečtěte si článek [vytvořit datový sklad SQL][].

## <a name="1-configure-the-data-source"></a>1. nakonfigurovat zdroj dat

PolyBase používá k definování umístění a atributy externích dat externí objekty T-SQL. Definice externího objektu jsou uloženy v SQL datový sklad. Vlastní data uložena externě.

### <a name="11-create-a-credential"></a>1.1. Vytvoření pověření

**Přeskočit tento krok** Pokud načítáte veřejných datech Contoso. Protože je už přístupný pro kohokoli, nemusíte zabezpečený přístup k ve veřejných datech.

**Není Přeskočit tento krok** : Jestliže používáte tento kurz jako šablonu pro načtení vlastních datech. Přístup k datům až pověření, použijte tento skript k vytvoření databáze omezené pověření a potom ji použijte při definování umístění zdroje dat.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Přejděte ke kroku 2.

### <a name="12-create-the-external-data-source"></a>1.2. Vytvoření externího zdroje dat.

Tímto příkazem [Vytvořit externí zdroje dat][] ukládat umístění data a zadejte data. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Pokud se rozhodnete sdělit zveřejnění objektů blob azure kontejnery úložiště, mějte na paměti, že jakožto vlastníka dat můžete strhne příslušná data výstupní poplatky při dat ponechá datovém centru. 

## <a name="2-configure-data-format"></a>2. formát dat ve sloupcích konfigurace

Data se ukládají v textových souborů v úložišti objektů blob Azure a každé pole oddělené pomocí oddělovače. Spusťte tento příkaz [Vytvořit externí formátu][] určit formát data v textových souborů. Datech Contoso nekomprimované a kanálu středníkem.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. vytvořit externí tabulky

Teď, když jste určili vybraného datového zdroje a formátu souboru, jste připraveni k vytvoření externího tabulek. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Vytvoření schématu pro data. 

Pokud chcete vytvořit místo pro uložení dat Contoso v databázi, schéma vytvořte.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. Vytvořte externí tabulky. 

Spusťte tento skript k vytvoření externího tabulky DimProduct a FactOnlineSales. Všechny jsme dělají tady je definování názvů sloupců a datových typů a vazba na umístění a formátu souborů úložiště objektů blob Azure. Definice uložený v SQL datový sklad a data jsou ještě ve objektů Blob Azure úložiště.

Parametr **umístění** je složka v kořenové složce v objektů Blob Azure úložiště. Každá tabulka je do jiné složky.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. data načítáte
Existuje způsobů, jak pracovat s externími daty.  Můžete načítat data přímo z v externí tabulce, data načítáte do nových databázových tabulek nebo přidání externích dat do existující tabulky databáze.  


### <a name="41-create-a-new-schema"></a>4.1. Vytvoření nového schématu

CTAS vytvoří novou tabulku, která obsahuje data.  Nejprve vytvořte schématu pro datech contoso.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Načtení dat do nové tabulky

Načtení dat z úložiště objektů blob Azure a jejich ukládání v tabulce uvnitř vaší databáze, použijte příkaz [Vytvořit tabulku jako vyberte jazyce Transact-SQL ()][] . Načtení s CTAS využívá silného typu externí tabulky, kterou jste právě vytvořili. Načíst data do nové tabulky, použijte příkaz jeden [CTAS][] každou tabulku. 

CTAS vytvoří nová tabulka a naplní výsledky příkazu select. CTAS definuje novou tabulku mají stejné sloupce a datové typy jako výsledky příkazu select. Pokud vyberete všechny sloupce z externí tabulky, budou do nové tabulky otevřené sloupce a datových typů v externí tabulce.

V tomto příkladu vytvoříme dimenzi a tabulce faktů jako hash distribuované tabulek. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 sledování průběhu načítání

Můžete sledovat průběh vašeho zatížení použití Správa dynamických zobrazení (DMVs). 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. optimalizace columnstore komprese

Ve výchozím nastavení ukládá SQL datový sklad tabulky jako skupinový columnstore index. Po dokončení zatížení některé řádky dat nemusí komprimovány do columnstore.  Existuje různých důvodů, proč k tomu může dojít. Další informace najdete v tématu [Správa columnstore indexy][].

Pro optimalizaci výkonu dotazu a komprese columnstore po zatížení, znovu vytvořte tabulku vynutit indexu columnstore komprimovat všechny řádky. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Další informace o zachování columnstore indexy naleznete v článku [Správa columnstore indexy][] .

## <a name="6-optimize-statistics"></a>6. statistiky optimalizace

Je vhodné vytvořit jednosloupcovou statistiky bezprostředně za zatížení. Existuje několik možností, jak statistiky. Například pokud vytvořte jednosloupcovou statistiky na každý sloupec může trvat dlouho znovu vytvořit všechny statistiky. Pokud víte, že některé sloupce neprojdou být predikáty dotazu, můžete přejít vytváření statistiky na tyto sloupce.

Pokud se rozhodnete vytvořte jednosloupcovou statistiky na každý sloupec každé tabulky, můžete použít vzorek kód uložená procedura `prc_sqldw_create_stats` v článku [statistiky][] .

V následujícím příkladu je dobré výchozí bod pro vytváření statistiky. Vytvoří jednosloupcovou statistiky na každý sloupec v tabulce dimenze a na každý spojování sloupec v tabulce faktů. Můžete vždy přidat jeden nebo více sloupci statistiky do jiných sloupců v tabulce faktů později.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Dosažení odemknout!

Můžete mít úspěšně načtená ve veřejných datech datový sklad SQL Azure. Skvělá práce!

Můžete začít dotazování tabulek pomocí dotazů takto:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Další kroky
Načtení úplná Contoso maloobchodní datový sklad, pomocí skriptu v další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].

<!--Image references-->

<!--Article references-->
[Vytvořit datový sklad SQL]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Přehled vývoje SQL datový sklad]: sql-data-warehouse-overview-develop.md
[Správa columnstore indexy]: sql-data-warehouse-tables-index.md
[Statistiky]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[VYTVOŘENÍ EXTERNÍHO ZDROJE DAT]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[VYTVOŘIT EXTERNÍ FORMÁTU]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Vytvoření tabulky jako vyberte (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Načtení úplné datový sklad maloobchodní Contoso]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
