<properties 
    pageTitle="Přesuňte data do SQL Server Azure počítače virtuální | Azure" 
    description="Přesuňte data z textových souborů nebo z místního SQL serveru SQL Server na OM Azure." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Přesunutí dat na serveru SQL Server Azure virtuální počítače

Toto téma popisuje možnosti pro přesunutí dat z textových souborů (formátování souboru CSV nebo TSV) nebo z místních SQL serveru SQL Server na Azure virtuálního počítače. Tyto úkoly přesunutí dat do cloudu jsou součástí procesu týmu dat pro výzkum.

Téma, které popisuje možnosti pro přesunutí dat do databáze SQL Azure pro vzdělávání počítače najdete v článku [přesunutí dat do databáze SQL Azure pro vzdělávání počítače Azure](machine-learning-data-science-move-sql-azure.md).

**Nabídka** pod odkazů na témata, která popisují, jak jedí dat do jiných cílové prostředí, kde data můžete být uloženy zpracování během týmu dat pro výzkum obrázku (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


Následující tabulka shrnuje možnosti pro přesunutí dat na serveru SQL Server Azure virtuální počítače.

<b>ZDROJE</b> |<b>Cíl: SQL Server na Azure OM</b> |
------------------ |-------------------- |
<b>Plochému souboru</b> |1. <a href="#insert-tables-bcp">Nástroj Kopírovat příkazového řádku hromadné (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">hromadně vložení dotazu SQL</a><br> 3. <a href="#sql-builtin-utilities">grafické předdefinované nástroje SQL serveru</a>
<b>Místní SQL serveru</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">nasazení databázi SQL serveru k Microsoft Azure OM Průvodce</a><br> 2. <a href="#export-flat-file">Exportovat k plochému souboru</a><br> 3. <a href="#sql-migration">Průvodce přenesením databáze SQL</a> <br> 4. <a href="#sql-backup">databáze back nahoru a obnovení</a><br>

Všimněte si, že tento dokument předpokládá, že příkazy SQL budou provedeny z SQL Server Management Studio nebo Visual Studio Průzkumník databáze.

> [AZURE.TIP] Jako alternativu slouží k vytvoření a naplánování kanálu, který bude přesouvat data angličtině Server SQL na Azure [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) . Další informace najdete v tématu [kopírování dat s Azure Data Factory (kopírovat aktivity)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Zjistit předpoklady pro
Tento kurz se předpokládá, že máte:

* **Azure předplatného**. Pokud nemáte předplatné, můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure úložiště**. Účet Azure úložiště bude používat pro ukládání data v tomto kurzu. Pokud nemáte účet Azure úložiště, podívejte se na článek [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) . Po vytvoření účtu úložiště, musíte získat klíč účtu použitých při přístupu k úložišti. V tématu [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Zřizování **serveru SQL Server na Azure OM**. Pokyny najdete v tématu [Nastavení Azure SQL Server virtuálního počítače jako server IPython Poznámkový blok pro pokročilé technologie pro analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Nainstalovanou a nakonfigurovanou **Azure PowerShell** místně. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Přesunutí dat ze zdroje plochému souboru na serveru SQL Server Azure OM

-Li data v plochému souboru (uspořádaných do formátu řádků nebo sloupců), ho můžete přesunout do SQL Server OM na Azure pomocí následujících metod:

1. [Nástroj Kopírovat příkazového řádku hromadné (BCP)](#insert-tables-bcp) 
2. [Dotaz SQL zobrazený hromadné vložení](#insert-tables-bulkquery)
3. [Grafické vestavěných nástrojů v SQL Server (Import nebo Export, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Nástroj Kopírovat příkazového řádku hromadné (BCP)

BCP nástroj nainstalovaný se serverem SQL Server a je nejrychlejší způsob chcete přesunout data. Funguje napříč všechny tři varianty SQL serveru (místní SQL Server, SQL Azure nebo SQL Server OM na Azure). 

> [AZURE.NOTE]**Kde Moje data třeba pro BCP?**  
> I když není povinný, máte soubory obsahující zdrojová data nachází na stejném počítači jako cílový SQL server umožňuje rychlejší přenosy (síť rychlosti a místní disk vstupu a výstupu rychlost). Můžete přesunout textových souborů obsahujících data k počítači, kde je nainstalovaný SQL Server pomocí kopírování nástrojů, jako jsou [AZCopy](../storage/storage-use-azcopy.md)různých souborů, [Exploreru úložiště Azure](http://storageexplorer.com/) nebo windows zkopírovat a vložit pomocí vzdálené plochy RDP (Protocol).

1. Zajistit, aby databázi a tabulky jsou vytvořené v cílové databázi SQL serveru. Tady je příklad toho, jak tuto pomocí `Create Database` a `Create Table` příkazy:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Formát souboru, který popisuje schématu pro tabulku tak, že zadáním následujícího příkazu z příkazového řádku počítači nainstalovanou bcp generovat.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Vložte data do databáze pomocí příkazu bcp takto. To měli spolupracovat z příkazového řádku za předpokladu, že je na stejném počítači nainstalovaný SQL Server:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimalizace BCP vloží** Další informace naleznete v následujícím článku ["Pokyny pro optimalizaci hromadného importu"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimalizovat takové vloží.


### <a name="insert-tables-bulkquery-parallel"></a>Paralelním prováděním vloží pro rychlejší přesun dat

Pokud jsou data, který přesouváte velké, můžete urychlit věci současně spuštěním více příkazů BCP paralelně skript Powershellu.

> [AZURE.NOTE]**Velký dat požití** Pokud chcete optimalizovat načítání velkých a velmi velkých datových sad dat, oddílů logické a fyzické databázových tabulek pomocí více skupin souborů a oddíl tabulek. Další informace o vytváření a načtení dat do tabulky oddílů najdete v článku [Paralelní tabulky oddílů SQL načíst](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Ukázkový skript Powershellu dole prokázat paralelní vloží pomocí bcp:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Dotaz SQL zobrazený hromadné vložení

[Dotaz SQL vložte hromadné](https://msdn.microsoft.com/library/ms188365) mohou sloužit k importu dat do databáze z řádek či sloupec na základě souborů (podporované typy jsou uvedena v[Příprava dat pro hromadnou Export nebo Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) tématu. 

Tady jsou některé ukázkové příkazy pro hromadné vložení jsou jako níže:  

1. Analýza dat a nastavit vlastní možnosti před importem abyste měli jistotu, že databázi SQL serveru předpokládá stejný formát pro speciální pole například kalendářní data. Tady je příklad toho, jak můžete nastavit formát data jako rok měsíc a den (Pokud data obsahují datum ve formátu rok měsíc a den):

        SET DATEFORMAT ymd; 
    
2. Import dat pomocí hromadného importu příkazy:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Integrované nástroje SQL serveru

Integrace služby SSIS (SQL Server) umožňuje importovat data do SQL Server OM na Azure z plochému souboru. SSIS je k dispozici v každém z obou studio prostředí. Podrobnosti najdete v tématu [Studio prostředí a Integration Services (SSIS)](https://technet.microsoft.com/library/ms140028.aspx):

- Podrobnosti o SQL Server Data Tools najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Podrobnosti o Průvodce importem a exportem najdete v tématu [SQL Server importem a exportem](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Přesunutí dat z místního serveru SQL Server SQL Azure OM

Můžete taky použít následující strategie migrace:

1. [Nasazení databázi SQL serveru Microsoft Azure OM Průvodce](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Export k plochému souboru](#export-flat-file) 
3. [Průvodce migrací databáze SQL](#sql-migration)
4. [Databáze back nahoru a obnovení](#sql-backup)

Popis každou z nich níže:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Nasazení databázi SQL serveru Microsoft Azure OM Průvodce

**Nasazení databázi SQL serveru k Microsoft Azure OM Průvodce** je jednoduchý a doporučené způsob, jak přesuňte data z místního instance serveru SQL Server SQL Server na OM Azure. Podrobný postup i diskuse další možnosti najdete v článku [Migrace databáze SQL Server na OM Azure](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Export k plochému souboru

Různé způsoby mohou sloužit k hromadně exportovat data ze serveru SQL na pracovišti, jak je uvedeno v tématu [Hromadný Import a Export z dat (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) . Tento dokument převezme hromadné kopírovat Program (BCP) jako příklad. Po exportu dat do plochému souboru můžete importovat do jiného serveru SQL server pomocí hromadného importu. 

1. Exportovat data z místního SQL serveru k souboru pomocí nástroje bcp takto

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Vytvořit databázi a tabulky na SQL Server OM na Azure pomocí `create database` a `create table` u schématu tabulky exportovány v kroku 1.

3. Vytvoření souboru ve formátu pro popis schématu tabulky je exportovat a importovat data. Podrobnosti o formátu souborů jsou popsané v tématu [Vytvoření souboru ve formátu (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Formát souboru generování při spuštění BCP z počítače, SQL Server 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Formát souboru generování při spuštění BCP vzdáleně SQL serveru 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Použijte libovolný metod popsaných v části [Přesunutí dat ze souboru zdroje](#filesource_to_sqlonazurevm) a přesunutí dat v textových souborů k serveru SQL Server.

### <a name="sql-migration"></a>Průvodce migrací databáze SQL

[Průvodce migrací databáze SQL serveru](http://sqlazuremw.codeplex.com/) umožňuje uživatelsky přesun dat mezi dvě instance serveru SQL. Umožňuje uživateli mapování schéma dat mezi zdroji a tabulek, zvolte typy sloupců a různých dalších funkcí. Použití hromadné kopírování (BCP) na pozadí. Snímek obrazovky s úvodní obrazovku pro Průvodce přenesením SQL databáze jsou uvedeny níže.  

![Průvodce migrací SQL Server][2]

### <a name="sql-backup"></a>Databáze back nahoru a obnovení

SQL Server podporuje: 

1. [Databáze back nahoru a obnovení funkce](https://msdn.microsoft.com/library/ms187048.aspx) (jak místní soubor nebo bacpac exportovat objektů blob) a [Aplikací osy dat](https://msdn.microsoft.com/library/ee210546.aspx) (pomocí bacpac). 
2. Možnost přímo na vytvořit za SQL Server VMs Azure zkopírovaný databáze nebo kopírovat do existující databáze SQL Azure. Další informace najdete v tématu [použití Průvodce kopírováním databáze](https://msdn.microsoft.com/library/ms188664.aspx). 

Snímek obrazovky s zpět databáze nebo obnovit možnosti systému SQL Server Management Studio jsou uvedeny níže.

![Nástroj pro Import SQL serveru][1]

## <a name="resources"></a>Zdroje informací

[Migrace databáze SQL Server na Azure OM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server na virtuálních počítačích Azure přehled](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
