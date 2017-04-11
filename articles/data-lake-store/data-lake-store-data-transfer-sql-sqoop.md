<properties 
   pageTitle="Přesouvat data mezi úložiště jezera dat a databáze Azure SQL pomocí Sqoop | Microsoft Azure"
   description="Použití Sqoop přesouvat data mezi databáze SQL Azure a jezera úložiště dat" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Přesouvat data mezi úložiště jezera dat a databáze Azure SQL pomocí Sqoop

Naučte se používat Apache Sqoop k importu a exportu dat mezi databáze SQL Azure a jezera úložiště.
 

## <a name="what-is-sqoop"></a>Co je Sqoop?

Velký dat aplikace jsou přírodní volba pro zpracování částečně strukturovaná a Nestrukturovaná data, třeba protokoly a soubory. Však také pravděpodobně nutné zpracovat strukturovaná data, která je uložena v relační databáze.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) je nástroj určený pro přenos dat mezi relační databáze a velký datový úložiště, například jezera úložiště. Můžete ji používat k importu dat ze systému správy relační databáze (RDBMS), jako jsou databáze SQL Azure do úložiště jezera Data. Můžete pak transformace a analýza dat pomocí pracovního vytížení velký dat a pak exportovat data zpět do RDBMS. V tomto kurzu umožňuje databáze SQL Azure jako relační databáze import a export mezi.
 

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Povolení předplatného Azure** jezera úložiště veřejného (verze Preview). V tématu [pokyny](data-lake-store-get-started-portal.md#signup). 
- **Azure HDInsight obrázku** s přístupem k úložišti jezera účtu. V tématu [Vytvoření HDInsight obrázku s úložištěm jezera Data](data-lake-store-hdinsight-hadoop-use-portal.md). V tomto článku se předpokládá, že máte HDInsight Linux obrázku s přístupem jezera úložiště.
- **Databáze azure SQL**. Další informace o tom, jak ho vytvořit najdete v článku [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Zjistěte jak rychle se videa?

[Podívejte se na toto video](https://mix.office.com/watch/1butcdjxmu114) o tom, jak přesouvat data mezi objekty BLOB Azure úložiště a jezera úložiště dat pomocí DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Vytvoření ukázkové tabulky v databázi SQL Azure

1. Abyste mohli začít vytvořte dva ukázkové tabulky v databázi SQL Azure. Umožňuje [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio připojení k databázi SQL Azure a spusťte následující dotazů.

    **Vytvoření tabulky Table1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Vytvoření tabulka2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Do **tabulky Table1**přidejte ukázkovými daty. Nechte **tabulka2** prázdné. From **tabulka1** jsme bude import dat do úložiště jezera dat. Potom jsme budou exportovat data z úložiště jezera dat do **tabulky Table2**. Spusťte následující fragment kódu.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Použití Sqoop z Hdinsightu obrázku s přístupem k úložišti jezera dat.

HDInsight clusteru už má k dispozici Sqoop balíčků. Pokud jste nakonfigurovali clusteru HDInsight používat úložiště jezera dat jako dalšího úložiště, můžete použít Sqoop (bez změny konfigurace) pro import nebo export dat mezi relační databáze (v tomto příkladu databáze SQL Azure) a jezera úložišti účtu. 

1. Pro účely tohoto návodu jsme předpokládá, že jste vytvořili Linux obrázku tak, aby SSH byste měli použít pro připojení k clusteru. Přečtěte si článek [připojení k obrázku na základě Linux HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Ověřte, zda můžete získat přístup k účtu úložiště jezera dat z clusteru. Z příkazového řádku SSH spusťte tento příkaz:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    To by měl obsahovat seznamu souborů a složek v úložišti jezera účtu.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Import dat z databáze SQL Azure do úložiště jezera dat

3. Přejděte v adresáři, kde jsou k dispozici Sqoop balíčků. Obvykle to bude `/usr/hdp/<version>/sqoop/bin`. 

4. Importujte data z **tabulky Table1** zohledňovala jezera úložiště. Použijte tuto syntaxi:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Poznámka: Tento zástupný symbol **sql databáze serveru názvů** představuje název serveru, na kterém běží databáze Azure SQL. **název databáze SQL** zastupuje název aktuální databáze.

    Například

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Ověřte, že data přenesete do účtu úložiště jezera dat. Spusťte tento příkaz:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Měli byste vidět následující výstup.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Každý **část-m -** * soubor odpovídá jeden řádek v tabulce zdrojové * *tabulky Table1**. Zobrazení obsahu na část-m -* souborů, které chcete ověřit.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Export dat z úložiště jezera dat do databáze SQL Azure

6. Exportujte data z účtu jezera úložiště pro prázdnou tabulku, **tabulka2**v databázi SQL Azure. Pomocí následující syntaxe.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Například

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Ověřte, že byl odeslán data do tabulky databáze SQL. Umožňuje [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio připojení k databázi SQL Azure a spuštěním následujícího dotazu.

        
        SELECT * FROM TABLE2

    Musí mít takto výstupu.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Viz taky

- [Kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat](data-lake-store-copy-data-azure-storage-blob.md)
- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
