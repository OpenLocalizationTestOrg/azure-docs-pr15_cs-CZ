<properties
    pageTitle="Konfigurace stráž dat Oracle v VMs | Microsoft Azure"
    description="Návod pro nastavení a implementaci stráž dat Oracle pro Azure virtuálních počítačích pro vysokou dostupnost a havárie obnovení projít."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Konfigurace stráž dat Oracle pro Azure


Tento kurz ukazuje, jak se dají vytvořit a implementaci stráž dat Oracle v Azure virtuálních počítačích prostředí pro dostupnost a obnovení. Kurz se zaměřuje na jednosměrné replikace databáze Oracle certifikátů.

Ochrana dat Oracle podporuje ochrana dat a obnovení databáze Oracle. Je jednoduché, výkonné a drop-in řešení pro využití havárie, ochrana dat a dostupnost pro celou databázi Oracle.

Tento kurz předpokládá, že už máte zkušenosti teoretický a praktické na databáze Oracle dostupnost a obnovení koncepty. Informace najdete v tématu [Oracle webu](http://www.oracle.com/technetwork/database/features/availability/index.html) a také [Koncepty stráž dat Oracle a příručka pro správu](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Kromě toho kurzu se předpokládá, že jste již provedly následující požadavky:

- V části dostupnost a havárie obnovení informace v tématu [Oracle virtuálního počítače obrázky - různé aspekty](virtual-machines-windows-classic-oracle-considerations.md) už neprojdete. Azure aktuálně podporuje samostatného databáze Oracle instance, ale ne Oracle reálnou aplikace clusterů (Oracle certifikátů).


- Vytvoření dvou virtuálních počítačích (VMs) v Azure pomocí platformu stejný obrázek Oracle Enterprise Edition k dispozici. Zkontrolujte, jestli virtuálních počítačích ve [stejné Cloudová služba](virtual-machines-windows-load-balance.md) a ve stejné síti virtuální zajistit, že se můžou dostat k sobě přes trvalý soukromé IP adresu. Kromě toho se doporučuje VMs umístit stejný [Nastavte dostupnost](virtual-machines-windows-manage-availability.md) umožňují Azure, že je umístíte do samostatných poruch domén a upgrade domény. Ochrana dat Oracle je dostupná jenom s Enterprise Edition databáze Oracle. Každý počítač musí mít aspoň 2 GB paměti a 5 místa na disku. Nejaktuálnější informace o platformu podle velikosti OM najdete v článku [Virtuálního počítače velikosti Azure](virtual-machines-windows-sizes.md). Pokud potřebujete další disk hlasitost pro vaše VMs můžete připojit další disk. Informace najdete v tématu [jak připojit Disk dat do virtuálního počítače](virtual-machines-windows-classic-attach-disk.md).



- Virtuální názvy jako "Počítač1" jste nastavili pro primární OM a "Počítač2" pro úsporném OM na portálu Azure klasické.

- Jste nastavili proměnnou **ORACLE_HOME** prostředí tak, aby ukazovaly na stejnou oracle kořenovou cestu k instalaci primární a úsporném režimu virtuálních počítačích, jako například `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Přihlášení k systému Windows server jako člen skupiny **Administrators** nebo členem skupiny **ORA_DBA** .

V tomto kurzu udělejte toto:

Implementace fyzických úsporném databáze prostředí

1. Vytvořte hlavní databází

2. Příprava primární databáze pro vytvoření úsporném databáze

    1. Povolit vynuceného protokolování

    2. Vytvoření souboru hesla

    3. Konfigurace protokol úsporném znovu

    4. Povolení archivace

    5. Nastavení parametrů inicializace primární databáze

Vytvoření fyzické úsporném databáze

1. Příprava inicializačního souboru parametr úsporném databáze

2. Konfigurace posluchače a tnsnames podporuje na počítačích primární a záložní databáze

    1. Konfigurace listener.ora na serverech k ukládání položek pro obě databáze

    2. Pokud chcete přidržet položky pro primární a záložní databáze, nakonfigurujte tnsnames.ora na virtuálních počítačích primární a záložní. 

    3. Spuštění nástroje pro sledování a zaškrtněte políčko tnsping na obou virtuálních počítačích obou služeb.

3. Spouštění úsporném instance v nomount stavu

4. Použití RMAN vytvořit kopii databáze a vytvořte úsporném databázi

5. Spuštění fyzické úsporném databáze v režimu spravovaných obnovení

6. Ověření fyzické úsporném databázi

> [AZURE.IMPORTANT] Tento kurz byl nastavení a testování proti následující hardware a software konfigurace:
>
>|                      | **Hlavní databází**                      | **Úsporném databáze**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Databáze Oracle**   | Verze Enterprise Oracle11g (11.2.0.4.0) | Verze Enterprise Oracle11g (11.2.0.4.0) |
>| **Název počítače**     | POCITAC1                                  | POČÍTAČ2                                  |
>| **Operační systém** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle ID zabezpečení**       | TEST                                      | TEST\_STBY                                |
>| **Paměti**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Místo na disku**       | Min 5 GB                                  | Min 5 GB                                  |

Pro pozdější verze databáze Oracle a stráž dat Oracle může být některé další změny, které potřebujete k provedení. Nejaktuálnější informace specifické pro verze dokumentaci [Stráž dat](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) a [Databáze Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) na webový server Oracle.

##<a name="implement-the-physical-standby-database-environment"></a>Implementace fyzických úsporném databáze prostředí
### <a name="1-create-a-primary-database"></a>1. vytvořit hlavní databází

- Vytvořte hlavní databází "Testování" v primární virtuálního počítače. Informace najdete v tématu Vytvoření a konfigurace databáze Oracle.
- Pokud chcete zobrazit název databáze, připojit k databázi jako uživatel systémových s rolí SYSDBA v SQL * Plus příkazový řádek a spusťte následující příkaz:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Pak dotazu názvy souborů databáze ze zobrazení pro systém dba_data_files:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. připravte primární databáze pro vytvoření úsporném databáze

Před vytvořením úsporném databáze, je vhodné zajistit, že je správně nastavený primární databáze. Následuje seznam kroků, které potřebujete provést:

1. Povolit vynuceného protokolování
2. Vytvoření souboru hesla
3. Konfigurace protokol úsporném znovu
4. Povolení archivace
5. Nastavení parametrů inicializace primární databáze

#### <a name="enable-forced-logging"></a>Povolit vynuceného protokolování

Implementace databázi úsporném režimu, potřebujeme povolit vynucená protokolování v hlavní databází. Tuto možnost zajistí, že i když se provádí operace "bez protokolování", platnost protokolování přednost a všechny operace přihlášeni protokoly znovu. Proto jsme zkontrolujte, že všechno v databázi primární zaznamenané a replikace v úsporném režimu zahrnuje všechny operace primární databáze. Povolení protokolování platnost, spusťte příkaz alter databáze:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Vytvoření souboru hesla

Aby mohla dodat a jeho aplikování archivované protokoly z primárního serveru server úsporném režimu, systémových heslo se musí shodovat na serverech primárních a úsporném režimu. Proto vytvoříte soubor heslo na primární databáze a jeho zkopírování do záložní server.

>[AZURE.IMPORTANT] Pokud chcete použít databáze Oracle 12c, je nový uživatel, **SYSDG**, které slouží ke správě stráž dat Oracle. Další informace najdete v článku [změny databáze Oracle 12 c vydání](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Kromě toho, ujistěte se, že ORACLE\_prostředí domů je definován v pocitac1. Pokud ne, je definujte jako proměnná prostředí pomocí dialogového okna proměnné. Toto dialogové okno aplikace access, spusťte nástroj **systém** dvojitým kliknutím na ikonu systém v **Ovládacích panelech**. potom klikněte na kartu **Upřesnit** a vyberte **Proměnné**. Pokud chcete nastavit proměnné prostředí, klikněte na tlačítko **Nový** v části **Systém proměnné**. Po nastavení proměnné prostředí, zavřete příkazového řádku existující Windows a otevřete novou.

Spusťte následující příkaz Přepnout do Oracle\_Domů adresáře, například C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\databáze.

    cd %ORACLE_HOME%\database

Vytvořte soubor heslo pomocí hesla soubor vytváření nástroj [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). Ve stejném Windows příkazového řádku v POCITAC1 spusťte tento příkaz nastavením hodnotu heslo jako heslo **systémových**:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Tento příkaz vytvoří soubor heslo stejný název jako PWDTEST.ora ve verzi databáze ORACLE\_HOME\\adresáře databáze. Měli byste zkopírovat tento soubor % ORACLE\_Domů %\\databáze adresáře počítač2 ručně.

#### <a name="configure-a-standby-redo-log"></a>Konfigurace protokol úsporném znovu

Potom musíte nakonfigurovat úsporném protokol znovu tak, aby primární můžete správně upozorněni znovu z něj stal úsporném režimu. Předem je zde vytváření umožňuje, aby protokoly úsporném znovu automaticky vytvořit v úsporném režimu. Je důležité úsporném režimu zopakování protokoly (SRL) nakonfigurovat stejně velká jako online zopakování protokoly. Velikost aktuální protokoly úsporném znovu musí přesně shodovat velikost souboru protokolu online znovu aktuální primární databáze.

Spusťte následující příkaz SQL\*PLUS příkazového řádku v pocitac1. Soubor protokolu $ v je zobrazení systému, který obsahuje informace o znovu protokoly.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Všimněte si, že 52428800 50 megabajtů.

Pak v SQL\*Plus okno, spusťte následující příkazy přidat novou skupinu souborů protokolu úsporném znovu úsporném databáze a zadejte číslo označující skupiny pomocí klauzule skupiny. Správa skupiny souborů protokolu úsporném znovu jednodušší můžete provést pomocí skupiny čísel:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Další spusťte následující zobrazení systémové zobrazení informací o znovu protokoly. Tuto operaci také ověří, že byly vytvořeny skupiny souborů protokolu úsporném znovu:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Povolení archivace

Povolení pak archivace spuštěním následující příkazy pro umístění hlavní databází v režimu ARCHIVELOG a povolení automatické archivace. Povolit režim protokolu archivu připojení databáze a poté spuštění příkazu archivelog.

Nejdřív se přihlaste jako sysdba. Ve Windows příkazovém řádku spusťte:

    sqlplus /nolog

    connect / as sysdba

Potom vypnutí databáze v SQL\*Plus příkazového řádku:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Potom spusťte příkaz připojení spuštění připojení databáze. Zajistíte tím, že Oracle přidruží instance zadané databázi.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Spusťte:

    SQL> alter database archivelog;
    Database altered.

Spusťte Alter příkazu database s otevřít klauzulí být k dispozici pro běžné používání databáze:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Nastavení parametrů inicializace primární databáze

Abyste mohli nakonfigurovat stráž dat, musíte vytvořit a nakonfigurovat úsporném parametry na běžná pfile (inicializace parametr souboru) první. Pokud pfile hotovou, budete muset převést do souboru parametr serveru (soubor SPFILE).

Můžete určit Data stráž prostředí pomocí parametrů v Inicializace. ORA soubor. Až po tomto kurzu se budete muset aktualizovat primární databázi Inicializace. ORA tak, že se můžou obsahovat obě role: primární nebo úsporném režimu.

    SQL> create pfile from spfile;
    File created.

Pak budete muset upravit pfile přidáte úsporném parametry. K tomuto účelu otevřete INITTEST. ORA změnit umístění % ORACLE\_Domů %\\databáze. Potom připojte následující příkazy k souboru INITTEST.ora. Konvence pro vaše Inicializace. Soubor ORA je Inicializace\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Předchozího časového výkazu zahrnuje tři důležité nastavení položky:
-   **LOG_ARCHIVE_CONFIG...:** Můžete definovat jedinečnou databázi ID, pomocí tohoto příkazu.
-   **LOG_ARCHIVE_DEST_1...:** Zadejte umístění složky místního archivu pomocí tohoto příkazu. Doporučujeme vytvořit nový adresář archivace potřebám vaší databáze a určení umístění místního archivu pomocí tohoto příkazu explicitně spíše než Oracle výchozí složku % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... ... ASYNCHRONNÍ LGWR:** definování adresy asynchronní protokolu Redaktor proces (LGWR) shromažďování dat a znovu transakce a předávat na úsporném míst. Tady DB_UNIQUE_NAME Určuje jedinečný název databáze v cílovém úsporném režimu serveru.

Jakmile nový soubor parametr je připravená, je potřeba vytvořit soubor spfile z něho.

Nejdřív vypnutí databáze:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Potom příkaz spuštění nomount následujícím způsobem:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Dále vytvořte soubor spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Potom vypnutí databáze:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Spusťte instance, použijte příkaz při spuštění:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Vytvoření fyzické úsporném databáze
Tato část se zaměřuje na kroky, které je třeba provést v počítač2 Příprava fyzické úsporném databázi.

Nejdřív budete muset Vzdálená plocha počítač2 prostřednictvím portálu Azure klasické.

V úsporném režimu serveru (počítač2) vytvořte všechny potřebné složky pro úsporném databázi, například C:\\\<YourLocalFolder\>\\testu. Při sledování tohoto kurzu, zkontrolujte, že struktura složek odpovídá struktura složek na POCITAC1 zachovat všechny potřebné soubory, jako jsou soubory controlfile, datafiles, redologfiles, udump, bdump a cdump. Kromě toho definovat ORACLE\_pro studenty a ORACLE\_základní proměnné v POČÍTAČ2. Pokud ne, je definujte jako proměnná prostředí pomocí dialogového okna proměnné. Toto dialogové okno aplikace access, spusťte nástroj **systém** dvojitým kliknutím na ikonu systém v **Ovládacích panelech**. potom klikněte na kartu **Upřesnit** a vyberte **Proměnné**. Pokud chcete nastavit proměnné prostředí, klikněte na tlačítko **Nový** ve skupinovém rámečku **Systémové proměnné**. Po nastavení proměnné prostředí, musíte zavřít příkazového řádku existující Windows a otevřete novou tyto změny neuvidíte.

Pak postupujte takto:

1. Příprava inicializačního souboru parametr úsporném databáze

2. Konfigurace posluchače a tnsnames podporuje na počítačích primární a záložní databáze

    1. Konfigurace listener.ora na serverech k ukládání položek pro obě databáze

    2. Konfigurace tnsnames.ora na virtuálních počítačích primární a záložní k ukládání položek pro primární a záložní databáze

    3. Spuštění nástroje pro sledování a zaškrtněte políčko tnsping na obou virtuálních počítačích obou služeb.

3. Spouštění úsporném instance v nomount stavu

4. Použití RMAN vytvořit kopii databáze a vytvořte úsporném databázi

5. Spuštění fyzické úsporném databáze v režimu spravovaných obnovení

6. Ověření fyzické úsporném databázi

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. připravte soubor inicializace parametr úsporném databáze

Tato část ukazuje, jak připravit inicializačního souboru parametr úsporném databáze. K tomuto účelu zkopírujte INITTEST. ORA souboru z počítače 1 počítač2 ručně. Můžete by měl být k dispozici INITTEST. Soubor je ORA % ORACLE\_Domů %\\složka pro databáze v oba počítače. Změňte INITTEST.ora soubor počítač2 ho nastavit roli úsporném uvedené níže:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Předchozího časového výkazu obsahuje dvě důležité nastavení položky:

-   **\*. LOG_ARCHIVE_DEST_1:** je potřeba vytvořit složku c:\OracleDatabase\TEST_STBY\archives v počítač2 ručně.
-   **\*. LOG_ARCHIVE_DEST_2:** Toto je nepovinný krok. Toto nastavíte v případě potřeby ji může být při primární počítače probíhá údržbu a úsporném počítač bude hlavní databází.

Pak budete muset spuštěním úsporném instance. Na úsporném databázovém serveru zadejte následující příkaz příkazového řádku systému Windows pro vytvoření Oracle instance vytvořením služba Windows:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Příkaz **Oradim** vytváří instanci Oracle ale se nespustí. Můžete najít v C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\KOŠ adresář.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Konfigurace posluchače a tnsnames podporuje na počítačích primární a záložní databáze
Než vytvoříte úsporném databáze, budete muset Ujistěte se, že primární a záložní databáze v konfiguraci můžete vzájemné komunikaci. K tomuto účelu musíte nakonfigurovat posluchače a TNSNames ručně nebo pomocí nástroje Konfigurace sítě NETCA. Při je to povinné úkolu použijte nástroj obnovení správce (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Konfigurace listener.ora na serverech k ukládání položek pro obě databáze

Vzdálená plocha POCITAC1 a úpravy souboru listener.ora jako uvedeny níže. Při úpravě souboru listener.ora vždy zkontrolujte, že levou a pravou kulatou závorku zarovnání ve stejném sloupci. Soubor listener.ora najdete v následující složce: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\sítě\\správce\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Další, Vzdálená plocha počítač2 upravit listener.ora souborů takto: # listener.ora soubor konfigurace sítě: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfigurace tnsnames.ora na virtuálních počítačích primární a záložní k ukládání položek pro primární a záložní databáze

Vzdálená plocha POCITAC1 a úpravy souboru tnsnames.ora jako uvedeny níže. Soubor tnsnames.ora najdete v následující složce: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\sítě\\správce\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Vzdálená plocha POČÍTAČ2 a úpravy tnsnames.ora souborů takto:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Spuštění nástroje pro sledování a zaškrtněte políčko tnsping na obou virtuálních počítačích obou služeb.

Otevřete nový Windows příkazovém řádku na primární a záložní virtuálních počítačích a spusťte následující příkazy:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Spouštění úsporném instance v nomount stavu
K nastavení prostředí pro podporu úsporném databáze na úsporném režimu virtuálního počítače (počítač2).

Nejdřív zkopírujte soubor heslo z primárního počítače (POCITAC1) do úsporném počítače (počítač2) ručně. Je to nutné jako heslo **systémových** se musí shodovat oba počítače.

Potom otevřete okno příkazového řádku Windows v POČÍTAČ2 a nastavení proměnné tak, aby ukazovaly na databázi úsporném režimu následujícím způsobem:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Potom spuštění databáze úsporném režimu nomount stav a pak vytvořte soubor spfile.

Spuštění databáze:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Použití RMAN vytvořit kopii databáze a vytvořte úsporném databázi
Obnovení správce systému (RMAN) umožňuje trvat záložní kopii primární databáze, kterou chcete vytvořit fyzické úsporném databázi.

Vzdálená plocha úsporném režimu virtuálního počítače (počítač2) a spustit nástroj RMAN zadáním úplného připojení řetězce pro obě cílem (primární databázi POCITAC1) a pomocné (úsporném databázi počítač2) instance.

>[AZURE.IMPORTANT] Nepoužívejte ověření operačního systému, protože tam ještě žádné databáze v počítači úsporném režimu serveru.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Spuštění fyzické úsporném databáze v režimu spravovaných obnovení
Tento kurz ukazuje, jak vytvořit databázi fyzické úsporném režimu. Informace o vytvoření logické úsporném databáze najdete v dokumentaci Oracle.

Otevřete SQL\*Plus příkazového řádku a povolení stráž dat v úsporném režimu virtuálního počítače nebo na serveru (počítač2) takto:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Při otevření databázi úsporném režimu **Připojit** , archivovat protokol pokračuje dodávky a proces spravovaných obnovení pokračuje protokolu použití úsporném databázi. Zajistíte tím, že úsporném databáze zůstane aktuální s hlavní databází. Nezapomeňte, že databázi úsporném nemůže být přístupné pro účely vykazování během tohoto období.

Při otevření úsporném databáze v režimu **Čtení jen** archivu protokolu dodávky budou dál problémy. Ale proces spravovaných obnovení se zastaví. To způsobí, že databázi úsporném osvobozením stále aktuální, dokud spravovaných obnovení po dokončení opravy. Máte přístup k úsporném databázi pro účely vykazování během této doby, ale data nesmí obsahovat nejnovější změny.

Obecně doporučujeme, abyste databázi úsporném režimu **Připojit** aktualizovat data v databázi úsporném Pokud dojde k selhání primární databáze. Však si můžete nechat úsporném databáze v režimu **Čtení pouze** pro účely vykazování v závislosti na požadavky aplikace. Podle těchto kroků ukazují, jak povolit stráž dat v režimu jen pro čtení pomocí jazyka SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Ověření fyzické úsporném databázi
V této části demonstruje ověřte konfiguraci dostupnost jako správce.

Otevřete SQL\*Plus okno příkazového řádku a zaškrtněte archivovány zopakování protokolu v úsporném režimu virtuálního počítače (počítač2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Otevřete SQL * Plus příkazového okno a zaměňte logfiles primární počítače (POCITAC1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

V protokolu archivované znovu v úsporném režimu virtuálního počítače (počítač2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Vyhledat všechny mezery v úsporném režimu virtuálního počítače (počítač2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Jiné metody ověřování může být převezme úsporném databáze a otestujte Pokud je možné navrácení primární databáze. Aktivace úsporném databáze jako primární databáze, použijte následující příkazy:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Pokud nepovolíte flashback na původní primární databáze, doporučujeme uvolnění původní primární databáze a znovu vytvořte jako úsporném databáze.

Doporučujeme povolit flashback databáze na primárním a úsporném databází. Důvody selhání chování, primární databáze můžete blikal zpět na čas před záložní a rychle se převedou na úsporném databáze.

##<a name="additional-resources"></a>Další zdroje informací
[Oracle virtuálního počítače obrázky Azure](virtual-machines-windows-classic-oracle-images.md)
