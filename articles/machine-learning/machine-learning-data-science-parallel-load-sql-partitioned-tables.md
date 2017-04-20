<properties 
    pageTitle="Paralelní hromadný Import dat pomocí oddílů tabulky SQL | Microsoft Azure" 
    description="Paralelní hromadný Import dat pomocí oddílů tabulky SQL" 
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
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Paralelní hromadný Import dat pomocí oddílů tabulky SQL

Tento dokument popisuje způsob vytvoření oddílů tabulky pro rychlé paralelní hromadný import dat do databáze serveru SQL Server. Pro velké zatížení/přenos dat do databáze SQL import dat do SQL databáze a následné dotazy lze zvýšit pomocí _oddílů tabulky a zobrazení_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Vytvořit novou databázi a sadu skupin souborů

- [Vytvořit novou databázi](https://technet.microsoft.com/library/ms176061.aspx) (pokud neexistuje)
- Přidání skupin souborů databáze na databázi, která bude obsahovat oddíly fyzické soubory

  Poznámka: Stačí [Vytvořit databázi](https://technet.microsoft.com/library/ms176061.aspx) , pokud nový nebo [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) Pokud databáze již existuje.

- Přidejte jeden nebo více souborů pro každý skupina souborů databáze (podle potřeby)

 > [AZURE.NOTE] Určete cílovou skupinu souborů, které obsahuje data pro tento oddíl a názvy souborů databáze fyzického uložení data skupiny souborů.
 
Následující příklad vytvoří novou databázi pomocí tří skupin souborů než primární a skupiny protokolu obsahující jeden fyzický soubor v každé. Databázové soubory jsou vytvářeny ve složce Data serveru SQL Server výchozí podle konfigurace v instanci serveru SQL Server. Další informace o výchozí umístění souborů naleznete v tématu [Umístění souborů pro výchozí a pojmenované instance SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Vytvoření oddílů tabulky

Vytvoření oddílů tabulky podle schéma dat namapovat na skupin souborů databáze, vytvořené v předchozím kroku. Po hromadného importu více oddíly tabulky dat se záznamy rozdělena mezi skupin souborů podle schéma oddílu, jak je popsáno níže.

**Chcete-li vytvořit tabulku oddílů, je třeba:**

- [Vytvoření oddílu funkce](https://msdn.microsoft.com/library/ms187802.aspx) , která definuje rozsah hodnot/hranice mají být zahrnuty v jednotlivých tabulkách jednotlivých oddílů, například, chcete-li omezit oddíly podle měsíce (některé\_datetime\_pole) v roce 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Vytvořit schéma oddílu](https://msdn.microsoft.com/library/ms179854.aspx) mapuje každý oddíl rozsah funkce oddílu fyzického skupina souborů, např:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Tip: Ověřte rozsah platných v každém oddílu podle požadované funkce schéma, spusťte následující dotaz:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Vytvoření oddílů tabulky](https://msdn.microsoft.com/library/ms174979.aspx) (s) podle data pro vaše datové schéma a určit oddíl schématu a omezení pole použít oddíl tabulky, např:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Další informace naleznete v tématu [Vytvoření oddílů tabulky a indexy](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Hromadný import dat pro každou tabulku jednotlivých oddílů

- Můžete použít BCP, HROMADNÉ vložení nebo jinými metodami, jako je například [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/). V příkladu je použita metoda BCP.

- [Provádění změn v databázi](https://msdn.microsoft.com/library/bb522682.aspx) změnit režim protokolování transakcí na BULK_LOGGED pro minimalizaci režie protokolování, např:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Chcete-li urychlit načítání dat, spuštění hromadné operace importu paralelně. Tipy na urychlení hromadného importu big dat do databáze serveru SQL Server naleznete v tématu [Načtení 1 TB za méně než 1 hodinu](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Následující skript prostředí PowerShell je příkladem paralelního dat načítání pomocí BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Vytvoření indexů pro optimalizaci spojení a výkonu dotazu

- Pokud bude extrahovat data pro modelování z více tabulek, vytvořte indexy klíčů spojení ke zlepšení výkonu spojení.

- [Vytvoření indexů](https://technet.microsoft.com/library/ms188783.aspx) (clusteru nebo bez clusterů) zaměření na stejné skupina souborů pro každý oddíl pro např:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
nebo,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Můžete vytvořit indexy před hromadného importu dat. Vytvoření indexu před hromadným importem zpomalí načítání dat.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Advanced Analytics procesy a technologii v příkladu akce

Příklad návod end-to-end proces Cortana Analytics prostřednictvím veřejné datové sady naleznete v tématu [procesu Cortana Analytics v akci: použití SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
