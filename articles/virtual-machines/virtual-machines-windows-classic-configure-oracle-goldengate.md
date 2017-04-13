<properties
    pageTitle="Konfigurace Oracle GoldenGate v VMs | Microsoft Azure"
    description="Návod pro nastavení a implementaci Oracle GoldenGate na VMs serveru Windows Azure pro vysokou dostupnost a havárie obnovení projít."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Konfigurace Oracle GoldenGate pro Azure


Tento kurz ukazuje, jak nastavit Oracle GoldenGate pro Azure virtuálních počítačích prostředí pro dostupnost a obnovení. Kurz se zaměřuje na [obousměrné replikace](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) databáze Oracle certifikátů a vyžaduje, aby byly obě lokality aktivní.

Oracle GoldenGate podporuje distribuce dat a integrace dat. Umožňuje nastavit distribuce dat a datové řešení synchronizace prostřednictvím konfigurace replikace Oracle Oracle a nabízí flexibilní dostupnost řešení. Oracle GoldenGate doplňuje stráž dat Oracle s jeho možnosti replikace povolit inovace rozdělení a nula prostoje informace nevyžádaného organizace a migrace. Podrobné informace najdete v tématu [Použití Oracle GoldenGate s stráž dat Oracle](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate obsahuje následující hlavní součásti: extrahování stopa čerpadlo, Replicat, Data nebo extrahujte soubory, kontrolních bodů a správci kolekcí. Mít obousměrné replikace mezi dvěma weby, potřebujete k vytvoření a spuštění všechny komponenty na obou webech. Podrobné informace o Oracle GoldenGate architektura příručce [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Tento kurz se předpokládá, že už máte teoretický a praktické znalostní databáze Oracle dostupnost a obnovení koncepty, jakož i [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Další informace najdete v tématu [Oracle webu](http://www.oracle.com/technetwork/database/features/availability/index.html).

Kromě toho kurzu se předpokládá, že jste již provedly následující požadavky:

- V části dostupnost a havárie obnovení informace v tématu [Oracle virtuálního počítače obrázky - různé aspekty](virtual-machines-windows-classic-oracle-considerations.md) už neprojdete. Všimněte si, že Azure podporuje aktuálně samostatného databáze Oracle instance, ale ne Oracle skutečné aplikace clusterů (Oracle certifikátů).

- Stažení softwaru Oracle GoldenGate z webu [Stažení Oracle](http://www.oracle.com/us/downloads/index.html) . Vyberete Middleware fúze produktu Pack Oracle – integrace dat. Poté co jste vybrali Oracle GoldenGate na Oracle v11.2.1 Media Pack systému Microsoft Windows x64 (64bitový) pro databáze Oracle 11 g. Další stáhněte si Oracle GoldenGate V11.2.1.0.3 pro Oracle 11g 64-bit v systému Windows 2008 (64bitová).

- Vytvoření dvou virtuálních počítačích (VMs) v Azure pomocí Oracle Enterprise Edition v systému Windows Server. Ujistěte se, že virtuálních počítačích ve [stejné Cloudová služba](virtual-machines-linux-load-balance.md) a ve stejné [Virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) zajistit, že se můžou dostat k sobě přes trvalý soukromé IP adresu.

- Na portálu Azure klasické jste nastavili virtuální názvy jako "MachineGG1" pro web A a "MachineGG2" pro web B.

- Jste vytvořili testovací databází "TestGG1" na webu A a "TestGG2" na webu B.

- Přihlášení k systému Windows server jako člen skupiny Administrators nebo členem skupiny **ORA_DBA** .

V tomto kurzu udělejte toto:

1. Nastavení databáze na webu A a B webu  

    1. Načíst počátečního data

2. Příprava webu A a B webu replikace databáze:

3. Vytvoření všechny potřebné objekty na podporu replikace DDL

4. Nastavení správce GoldenGate sítěmi A a B

5. Vytvořit skupinu extrahovat a Data pumpa procesy na webu A a B webu

    1. Vytváření procesů extrahovat a pumpa dat na web

    2. Vytvoření tabulky kontrolní bod GoldenGate na webu B

    3. Přidání REPLICAT na webu B

    4. Vytváření procesů extrahovat a pumpa dat na webu B

    5. Vytvoření tabulky kontrolní bod GoldenGate na web

    6. Přidání REPLICAT na web

    7. Přidání trandata na webu A a B webu

    8. Zahájení procesy extrahovat a pumpa dat na web

    9. Zahájení procesy extrahovat a pumpa dat na webu B

    10. Spustit proces REPLICAT na web

    11. Spustit proces REPLICAT na webu B

6. Ověření obousměrné replikační proces

>[AZURE.IMPORTANT] Tento kurz byl nastavení a testování proti následující software konfigurace:
>
>|                        | **Web databáze**              | **Databáze webu B**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Databáze Oracle**     | Oracle11g verze 2 – (11.2.0.1) | Oracle11g verze 2 – (11.2.0.1) |
>| **Název počítače**       | MachineGG1                       | MachineGG2                       |
>| **Operační systém**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle ID zabezpečení**         | TESTGG1                          | TESTGG2                          |
>| **Replikace schématu** | JIŘÍ                            | JIŘÍ                            |

Pro pozdější verze databáze Oracle a Oracle GoldenGate může být některé další změny, které potřebujete k provedení. Aktuální verze konkrétní informace najdete v tématu [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) a [Databáze Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) přečtěte následující dokumentaci Oracle webu. Například, vydání 11.2.0.4 zdrojové databáze a novějších verzích zachycení DDL provádí serveru logmining asynchronní a vyžaduje žádné zvláštní aktivačních událostí, tabulek a dalších objektů databáze je třeba nainstalovat. Oracle GoldenGate upgrady lze provést bez ukončení aplikace uživatele. Použití aktivační DDL a podpůrné objekty po extrahovat v integrovaném režimu s databáze Oracle 11g zdroj, který předchází verze 11.2.0.4 požaduje. Podrobné pokyny naleznete v tématu [instalace a konfigurace Oracle GoldenGate pro databáze Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. nastavení databáze na webu A a B webu
Tato část popisuje, jak provádět požadavky databáze na webu A a B. webu Musíte provedení všech kroků v této části na obou webech: sítě A a B. webu

První, vzdálené ploše pro web A a B webu prostřednictvím portálu Azure klasické. Otevřete příkazový Windows a vytvořte domácí adresář Oracle GoldenGate instalačních souborů:

    mkdir C:\OracleGG

Potom unzip a nainstalujte software Oracle GoldenGate v této složce. Po dokončení tohoto kroku se spuštěním následujícího příkazu začnete video Interpreter příkaz Software GoldenGate (GGSCI):

    C:\OracleGG\.\ggsci

Můžete použít [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) provádět několik příkazů, které slouží ke konfiguraci, řídit a sledovat Oracle GoldenGate.

Další spusťte tento příkaz Vytvořit všechny podřízené složky z instalační balíček:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Spusťte tento příkaz ukončíte GGSCI příkazového řádku:

    GGSCI (Hostname) 1> EXIT

Potom je potřeba vytvořit databázi uživateli, která se použije procesy Oracle GoldenGate správce, extrahovat a replikace. Všimněte si, že můžete vytvářet jednotlivé uživatele pro každý proces nebo konfigurace jedinou běžné uživatele. V tomto kurzu vytvoříme jednoho uživatele, který se označuje jako ggate. Potom jsme uživateli udělit potřebná oprávnění. Všimněte si, že je nutné provést tyto operace na webu A a B. webu

Otevřete SQL\*Plus okna příkazového na webu A a B webu s oprávněními správce databáze pomocí **SYSDBA**, například:

    Enter username: / as sysdba

A spuštění:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Pak najděte Inicializace\<DatabaseSID\>. Soubor je ORA % ORACLE\_Domů %\\databázi složek na webu A a B webu a přidat následujících parametrů databáze do INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Úplný seznam všechny příkazy Oracle GoldenGate GGSCI najdete v článku Principy [pro Oracle GoldenGate pro Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Načíst počátečního data

Provedením několika postupů můžete provádět načtení počáteční dat v databázi. Například můžete [Oracle GoldenGate přímé načtení](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) nebo běžná nástroje pro Export a Import exportu dat tabulky z webu A na webu B.

Prokázat procesu replikace Oracle GoldenGate ukazuje tohoto kurzu Vytvoření tabulky na webu A a B webu pomocí následujících příkazů.

Nejdřív otevřete SQL\*Plus příkaz okno a spusťte tento příkaz vytvoříte tabulku zásob na webu A a B webu databází:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Dále přidejte omezení do nově vytvořené tabulky na webu A a B webu databází:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Udělte všechna oprávnění v nové tabulce skladové ggate uživatele na webu A a b webu

    grant all on scott.inventory to ggate;

Dále vytvořte a povolte databáze aktivační událost, INVENTORY_CDR_TRG, v tabulce nově vytvořený a ujistěte se, zaznamenání všech transakcí do nové tabulky nejsou-li uživateli ggate. Na webu A a B. webu tuto operaci

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. připravte webu A a B webu replikace databáze:
Tato část popisuje jak připravit webu A a B webu replikace databáze. Musíte provedení všech kroků v této části na obou webech: sítě A a B. webu

První, vzdálené ploše pro web A a B webu prostřednictvím portálu Azure klasické. Databáze, přepněte do režimu archivelog pomocí příkazu SQL * Plus okno příkazového řádku:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Zapnutím minimální [doplňkové protokolování](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) následujícím způsobem:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Pak Příprava databáze pro podporu replikace DDL (data definition language):

    SQL> alter system set recyclebin=off scope=spfile;

Potom vypnutí a restartování databáze:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. všechny potřebné objekty na podporu replikace DDL vytvořte
Tato část uvádí, které potřebujete, abyste mohli vytvářet všechny potřebné objekty na podporu replikace DDL skriptů. Je potřeba spustit skripty uvedené v této části na webu A a B. webu

Otevřete příkazový Windows a přejděte do složky Oracle GoldenGate, například C:\\OracleGG. Zahájení SQL\*Plus příkazového řádku s oprávněními správce databáze, například pomocí **SYSDBA** na webu A a B. webu

Spusťte následující skripty:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Nástroj GoldenGate Oracle vyžaduje přihlašovací úrovně tabulky podporu DDL (data definition language). Je to proto, povolit doplňkové protokolování na úrovni tabulky změníte pomocí příkazu TRANDATA přidat. Otevřete okno Video interpreter Oracle GoldenGate příkaz, přihlaste se k databázi a potom na příkaz Přidat TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. konfiguraci GoldenGate správce sítěmi A a B
Správce GoldenGate Oracle provádí řadu funkcí jako od jiných GoldenGate procesy, Správa souborů protokolu záznam a vytváření sestav.

Potřebujete konfigurovat proces Oracle GoldenGate správce na webu A a B. webu K tomuto účelu proveďte následující kroky na webu A a B. webu

Otevřených oken příkaz okno a spustit video interpreter Oracle GoldenGate příkaz:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Přihlásí relace GGSCI databázi tak, aby bylo možné provést příkazy, které ovlivňují databáze:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Zobrazení stavu a prodlevy (případně) pro všechny správce, extrahovat a Replicat procesy v systému:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Zobrazení stavu a prodlevy (případně) pro všechny správce, extrahovat a Replicat procesy v systému:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Přihlásí relace GGSCI databázi tak, aby bylo možné provést příkazy, které ovlivňují databáze:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Spustíte proces správce:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. vytvořit extrahovat procesy skupiny a pumpa dat na webu A a B webu

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Vytváření procesů extrahovat a pumpa dat na web

Je potřeba vytvořit procesy extrahovat a pumpa dat na web a webu B. Vzdálená plocha na webu A a B webu prostřednictvím portálu Azure klasické. Otevřete okno Video interpreter příkaz GGSCI. Spusťte následující příkazy pro web:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace: GGSCI (MachineGG1) 18 > Upravit parametry ext1 EXTRAHOVAT ext1 ID ggate heslo ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate tabulky scott.inventory GETBEFORECOLS (dál aktualizace KEYINCLUDING (prod_category, qty_in_stock, last_dml), KEYINCLUDING odstranit dál (prod_category, qty_in_stock, last_dml))

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Vytvoření tabulky kontrolní bod GoldenGate na webu B

Pak budete muset přidat tabulku kontrolních bodů na webu B. K tomuto účelu potřebujete otevřete okno Video interpreter příkaz GoldenGate a spuštění:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

A, přidejte tabulce kontrolní bod k databázi, kde je ggate vlastníka:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Přidejte název tabulky čárky zaškrtnutí GLOBALS souboru na server cílové, což je webu B v tomto kroku. Upravte soubor GLOBALS B: webu

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Potom připojíte parametr CHECKPOINTTABLE do existujícího souboru GLOBALS:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Jako poslední krok uložte a zavřete soubor GLOBALS parametrů.


###<a name="add-replicat-on-site-b"></a>Přidání REPLICAT na webu B
Tato část popisuje, jak přidat proces REPLICAT "REP2" na webu B.

Příkazem přidat REPLICAT Replicat skupina vytvořit na webu B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Vytváření procesů extrahovat a pumpa dat na webu B

Tato část popisuje jak vytvořit nový proces extrahovat "EXT2" a nový proces čerpadlo dat "DPUMP2" na webu B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Vytvoření tabulky kontrolní bod GoldenGate na web

Otevřete okno Video interpreter příkaz Oracle GoldenGate a vytvořte tabulku kontrolní bod:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Bude potřeba přidat název tabulky čárky zaškrtnutí GLOBALS soubor na webu A.

Otevřete okno Video interpreter příkaz Oracle GoldenGate a úpravy souboru GLOBALS na webu A:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Uložte a zavřete soubor GLOBALS parametrů.

###<a name="add-replicat-on-site-a"></a>Přidání REPLICAT na web

Tato část popisuje, jak přidat proces REPLICAT "REP1" na webu A.

Následující příkaz vytvoří skupinu rep1 Replicat s názvem stopy a související checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Otevřete soubor parametrů pomocí příkazu upravit parametry a připojte následující informace:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Přidání trandata na webu A a B webu

Protokolování doplňkové na úrovni tabulky změníte pomocí příkazu TRANDATA přidat. Otevřete okno Video interpreter Oracle GoldenGate příkaz, přihlaste se k databázi a potom na příkaz Přidat TRANDATA.

Vzdálená plocha MachineGG1, otevřete video interpreter příkaz Oracle GoldenGate a spuštění:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Vzdálená plocha MachineGG2, otevřete video interpreter příkaz Oracle GoldenGate a spuštění:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Zobrazení informací o stavu úrovni tabulky doplňkové protokolování:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Přidání trandata na webu A a B webu

Protokolování doplňkové na úrovni tabulky změníte pomocí příkazu TRANDATA přidat. Otevřete okno Video interpreter Oracle GoldenGate příkaz, přihlaste se k databázi a potom na příkaz Přidat TRANDATA.

Vzdálená plocha MachineGG1, otevřete video interpreter příkaz Oracle GoldenGate a spuštění:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Vzdálená plocha MachineGG2, otevřete video interpreter příkaz Oracle GoldenGate a spuštění:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Zahájení procesy extrahovat a pumpa dat na web

Spustit proces ext1 extrahovat pro web:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Zahájení dpump1 proces čerpadlo dat na webu A:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Zobrazení informací o skupině ext1 extrahovat: GGSCI (MachineGG1) 32 > informace o extrahování ext1 EXTRAHOVAT EXT1 poslední začít 2013-11 – 25 08:03 stav SPUŠTĚNÝ kontrolní bod prodlevy 00:00:00 (aktualizované 00:00:02 před) protokolu čtení kontrolní bod Oracle znovu protokoly 2013-11 – 25 08:03:18 Seqno, 6, RBA 3230720 oznámení změny stavu 0.1074371 (1074371)

Zobrazení stavu a prodlevy (případně) pro všechny správce, extrahovat a Replicat procesy v systému:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Zahájení procesy extrahovat a pumpa dat na webu B

Spustit proces ext2 extrahovat na webu B:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Zahájení dpump2 proces čerpadlo dat na webu B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Zobrazení stavu a prodlevy (případně) pro všechny správce, extrahovat a Replicat procesy v systému:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Spustit proces REPLICAT na web

Tato část popisuje, jak zahájení procesu REPLICAT "REP1" na webu A.

Spustit proces Replicat pro web:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Zobrazte stav Replicat skupiny:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Spustit proces REPLICAT na webu B

Tato část popisuje, jak zahájení procesu REPLICAT "REP2" na webu B.

Spustit proces Replicat na webu B:

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Zobrazte stav Replicat skupiny:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. obousměrné replikační proces ověření

Ověřte konfiguraci Oracle GoldenGate, vložení řádku do databáze na webu a Vzdálená plocha webu a otevřete SQL * Plus příkazový řádek a spustit: SQL > Vybrat název z $ v databázi.

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Zkontrolujte, pokud tento řádek replikovat na webu B. K tomuto účelu Vzdálená plocha na webu B. otevřeno nahoru SQL Plus okno a spuštění:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Vložit nový záznam na web B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Vzdálená plocha na web a zkontrolujte, zda replikace došlo:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Další zdroje informací
[Oracle virtuálního počítače obrázky Azure](virtual-machines-linux-classic-oracle-images.md)
