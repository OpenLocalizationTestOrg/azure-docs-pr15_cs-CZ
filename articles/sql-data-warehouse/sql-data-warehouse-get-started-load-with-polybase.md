<properties
   pageTitle="PolyBase v SQL datový sklad kurzu | Microsoft Azure"
   description="Zjistěte, co je PolyBase a jak se používá pro data skladování scénáře."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Načtení dat s PolyBase v SQL datový sklad

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Tento kurz ukazuje, jak načíst data do SQL datový sklad pomocí AzCopy a PolyBase. Až budete hotovi, budete vědět, jak:

- Použití AzCopy ke kopírování dat k úložišti objektů blob Azure
- Vytvoření databázových objektů k definování dat
- Spuštění dotazu T-SQL načtení dat

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud chcete přecházet mezi tohoto kurzu, musíte

- Databáze SQL datový sklad.
- Účet Azure úložiště typu standardní místně nadbytečné úložiště (standardní-LRS), Standard Geo přebytečné úložiště (standardní GRS) nebo standardní úložiště Geo přebytečné přístup pro čtení (standardní RAGRS).
- Nástroj AzCopy příkazového řádku. Stáhněte a nainstalujte [nejnovější verzi AzCopy][] , které se instalují spolu s nástroji Microsoft Azure úložiště.

    ![Nástroje Azure úložiště](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Krok 1: Přidání ukázková data k úložišti objektů blob Azure

K načtení dat, potřebujeme umístění ukázkových dat do úložišti objektů blob Azure. V tomto kroku jsme naplnění Azure úložiště objektů blob s ukázkovými daty. Později použijeme PolyBase načtení ukázkových dat do databáze SQL datový sklad.

### <a name="a-prepare-a-sample-text-file"></a>ODPOVĚĎ: Příprava ukázkových textového souboru

Příprava ukázkových textového souboru:

1. Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového souboru. Uložte do adresáře vaší místní temp jako % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Najít koncový bod služby objektů blob

Chcete-li najít koncový bod služby objektů blob:

1. Z portálu Microsoft Azure vyberte **Procházet** > **Úložiště účty**.
2. Klikněte na úložiště účet, který chcete použít.
3. V zásuvné účtu úložiště klikněte na objekty BLOB

    ![Klikněte na objekty BLOB](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Adresa URL koncového bodu služby objektů blob uložte pro pozdější použití.

    ![Koncový bod služby objektů BLOB](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Najděte kód Azure úložiště

Vyhledání vašeho kódu Azure úložiště key:

1. Z portálu Microsoft Azure, vyberte možnost **Vyhledat** > **Úložiště účty**.
2. Klikněte na úložiště účet, který chcete použít.
3. Vyberte **všechna nastavení** > **přístupových kláves z verze**.
4. Klepněte do pole kopie jednu přístupových kláves zkopírovat do schránky.

    ![Zkopírujte klíč Azure úložiště](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Zkopírujte ukázkový soubor k úložišti objektů blob Azure

Ke kopírování dat k úložišti objektů blob Azure:

1. Otevřete příkazový řádek a změňte adresářů k adresáři AzCopy instalace. Tento příkaz se změní na výchozí instalační adresář v klientském počítači 64bitová verze Windows.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Spusťte tento příkaz nahrát soubor. Zadejte svůj objektů blob koncový bod adresy URL služby pro <blob service endpoint URL> a svůj klíč účtu Azure úložiště pro < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Viz taky [začínáte pracovat s nástroj AzCopy příkazového řádku][].

### <a name="e-explore-your-blob-storage-container"></a>E. Prozkoumání kontejner úložiště objektů blob

Chcete-li zobrazit soubor nahrát k úložišti objektů blob:

1. Přejděte zpět do svého zásuvné objektů Blob služby.
2. V části kontejnerů poklikejte na **datacontainer**.
3. Které můžete prozkoumat cestu k datům, klikněte na složku **datedimension** a zobrazí se uložený soubor **DimDate2.txt**.
4. Chcete-li zobrazit vlastnosti, klikněte na **DimDate2.txt**.
5. Všimněte si, že ve vlastnosti zásuvné objektů Blob, můžete stáhnout nebo odstranění souboru.

    ![Zobrazení objektů blob Azure úložiště](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Krok 2: Vytvoření externího tabulky pro ukázková data

V této části vytvoříme externí tabulky, který definuje ukázková data.

PolyBase používá externí tabulky pro přístup k datům v úložišti objektů blob Azure. Od data nejsou uložena v SQL datový sklad, zpracuje PolyBase ověřování k externím datům pomocí pověření omezené databáze.

Příklad v tomto kroku používá k vytvoření externí tabulku tyto příkazy jazyce Transact-SQL.

- [Vytvořte hlavní klíč (Transact-SQL)][] k šifrování tajná databázi omezené přihlašovacích údajů.
- [Vytvoření databáze omezené přihlašovacích údajů (Transact-SQL)][] , chcete-li zadat ověřovací informace pro váš účet Azure úložiště.
- [Vytvořit externí zdroj dat (Transact-SQL)][] určit umístění úložišti objektů blob Azure.
- [Vytvořit externí formát souboru (Transact-SQL)][] zadejte formát data.
- [Vytvořit externí tabulku (Transact-SQL)][] zadejte definice tabulky a umístění data.

Spusťte dotaz SQL datový sklad databázi. Vytvoří externí tabulku s názvem DimDate2External ve schématu dbo odkazující na DimDate2.txt ukázková data v úložišti objektů blob Azure.


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


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


V Průzkumníku objektu SQL serveru ve Visual Studiu zobrazí se formátu externí zdroje externích dat a tabulce DimDate2External.

![Zobrazení externí tabulka](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Krok 3: Načtení dat do SQL datový sklad

Po vytvoření externí tabulka můžete data načítáte do nové tabulky nebo vložení do existující tabulky.

- Načtení dat do nové tabulky, spusťte příkaz [Vytvořit tabulku jako vyberte jazyce Transact-SQL ()][] . Nová tabulka bude mít sloupce s názvem v dotazu. Datové typy sloupců se budou shodovat datových typů v externí tabulce definice.
- Načtení dat do existující tabulky, použijte [vložení... Vyberte (Transact-SQL)][] údajů.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Vytvoření statistiky na nově načtení dat

SQL datový sklad automaticky vytvářet ani automaticky aktualizován statistiky. Proto dosáhnout vysoké dotazu výkonu, je důležité na každý sloupec oblasti jednotlivými tabulkami vytvořit statistiky po první načíst. Je také důležité aktualizace statistiky po podstatné změny data.

Tento příklad vytvoří statistiky jedním sloupcem s novou tabulkou DimDate2.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Další informace najdete v tématu [statistiky][].  


## <a name="next-steps"></a>Další kroky
V tématu [Průvodce PolyBase][] Další informace, které byste měli vědět, jak vyvíjet řešení, které využívá PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Průvodce PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Začínáme s nástroj AzCopy příkazového řádku]: ../storage/storage-use-azcopy.md
[nejnovější verzi AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Vytvoření EXTERNÍHO zdroje dat (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[VYTVOŘIT externí formátu (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[VYTVOŘIT externí tabulku (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Vytvoření tabulky jako vyberte (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[VLOŽENÍ... Vyberte (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Vytvořte hlavní klíč (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Vytvoření databáze OBOR pověření (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
