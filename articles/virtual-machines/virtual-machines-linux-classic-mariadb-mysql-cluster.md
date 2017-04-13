<properties
    pageTitle="Spuštění clusteru MariaDB (MySQL) na Azure"
    description="Vytvoření MariaDB + Galera MySQL clusteru na virtuálních počítačích Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>Cluster MariaDB (MySQL) – kurz Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  Organizace MariaDB clusteru je teď k dispozici v Azure Marketplace.  Nová nabídka automaticky nasadit MariaDB Galera obrázku v cloudu. Měli byste použít nové nabídky z https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

Vytváření jsme více hlavních clusteru [Galera](http://galeracluster.com/products/) [MariaDBs](https://mariadb.org/en/about/), robustní scalable a spolehlivé drop-in náhrady samolepicích MySQL pro práci v prostředí vysoce dostupné na virtuálních počítačích Azure.

## <a name="architecture-overview"></a>Přehled

Toto téma provede následující kroky:

1. Vytvoření clusteru 3 uzel
2. Samostatných disků Data z disku OS
3. Vytvoření disků dat v RAID-0/rozložené nastavení zvětšíte procesorů
4. Pomocí služby Vyrovnávání zatížení Azure Vyrovnávání zatížení 3 uzlů
5. Minimalizovat opakujících práce, vytvořte OM obrázek obsahující MariaDB + Galera a jeho použití k vytvoření druhého clusteru VMs.

![Architektura](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  Toto téma používá nástroje [Azure rozhraní příkazového řádku](../xplat-cli-install.md) , takže nezapomeňte stáhnete a připojte k předplatnému Azure podle pokynů. Pokud potřebujete odkaz na příkazy k dispozici v Azure rozhraní příkazového řádku, podívejte se na tento odkaz pro [rozhraní příkazového řádku Azure informace o příkazech](../virtual-machines-command-line-tools.md). Bude taky potřeba [vytvořit SSH klíč pro ověřování] a poznamenejte si **.pem umístění souboru**.


## <a name="creating-the-template"></a>Vytvoření šablony

### <a name="infrastructure"></a>Infrastruktura

1. Vytvoření skupiny spřažení pro uložení zdroje společně

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Vytvořit virtuální sítě

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Vytvoření účtu úložiště hostovat naše discích. Poznámku, že jste neměli uvedení více než 40 často používá disků na stejný účet úložiště chcete-li předejít zasažení 20 000 procesorů účtu limit velikosti. V tomto případě jsme mimo z toto číslo tak jsme všechno ukládat na stejný účet pro zjednodušení

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Jak najdu název obrázek CentOS 7 virtuálního počítače

        azure vm image list | findstr CentOS
to bude výstupní nějak `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Použijte název v následujícím kroku.

4. Vytvoření šablony OM **/path/to/key.pem** nahraďte cestu umístění klávesu SSH vygenerovaných .pem

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. Připojení dat disků 4 x 500GB OM pro použití v konfiguraci RAID

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH do šablony OM, který jste vytvořili v **mariadbhatemplate.cloudapp.net:22** a připojit pomocí privátním klíčem.

### <a name="software"></a>Software

1. Získání kořenové

        sudo su

2. Nainstalujte RAID podpory:

     - Instalace mdadm

                yum install mdadm

     - Vytvoření konfigurace 0/pruh s systému souborů EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Vytvoření připojení adresář

                mkdir /mnt/data

     - Načtení UUID nově vytvořený RAID zařízení

                blkid | grep /dev/md0

     - Úprava /etc/fstab

                vi /etc/fstab

     - Přidání zařízení v tam povolení automatického mouting restartování nahrazení UUID získanou od příkazu **blkid** před hodnotu

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Připojit nový oddíl

                mount /mnt/data

3. Nainstalujte MariaDB:

     - Vytvoření souboru MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Vyplňte ho s pod obsahu

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Odebrání stávajícího přípona a mariadb knihoven abyste se vyhnuli konfliktům

            yum remove postfix mariadb-libs-*

     - Nainstalovat MariaDB Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Přesunutí datového adresáře MySQL RAID blokové zařízení

     - Zkopírujte aktuálnímu adresáři MySQL do nového umístění a odebrání původního adresáře

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Podle toho nastavit oprávnění pro nové adresář

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Vytvoření symlink přejdete na nové místo v oddílu RAID původního adresáře

            ln -s /mnt/data/mysql /var/lib/mysql

5. Protože [že selinux bude rušit operace clusteru](http://galeracluster.com/documentation-webpages/configuration.html#selinux), je nutné ho zakázat pro aktuální relaci (zobrazila kompatibilní verzi). Úprava `/etc/selinux/config` zakázat pro následné restartování:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Ověření se spustí MySQL

    - Zahájení MySQL

            service mysql start

    - Zabezpečené instalace MySQL, dialogové okno nastavit heslo kořenové, odebrat anonymní uživatelé zakázání přihlášení vzdálené kořenové a odebrání testovací databáze

            mysql_secure_installation

    - Vytvoření uživatele v databázi aplikace operace clusteru a volitelně aplikace

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - Ukončení MySQL

            service mysql stop

7. Vytvoření konfigurace zástupného symbolu

    - Úprava konfigurace MySQL vytvořit zástupný symbol pro nastavení obrázku. Nezastavíte **`<Vairables>`** nebo zrušte komentář nyní. Která se stane po vytvoříme virtuálního počítače pomocí této šablony.

            vi /etc/my.cnf.d/server.cnf

    - Úpravy části **[galera]** a vymažte ho

    - Úprava části **[mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Otevřete požadované porty v bráně firewall (pomocí FirewallD na CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Nové načtení bránu firewall:`firewall-cmd --reload`

9.  Optimalizace výkonu systému. Podívejte se na tento článek o [strategie ladění výkonu](virtual-machines-linux-classic-optimize-mysql.md) pro další informace

    - Znova upravit konfiguračního souboru MySQL

            vi /etc/my.cnf.d/server.cnf

    - Úprava části **[mariadb]** a připojte pod

    > [AZURE.NOTE] Doporučujeme **innodb\_vyrovnávací paměť\_pool_size** 70 % vaší OM paměti. Je nastavený na zde 2.45GB OM Azure médium s 3,5 GB paměti RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Zastavit MySQL, zakázat MySQL služba spuštěná spouštění chcete-li předejít zásahu clusteru při přidávání nové uzel a deprovision počítač.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Zachycení OM prostřednictvím portálu. (V současnosti [problém #1268 v rozhraní příkazového řádku Azure] nástroje Popisujte faktů obrázky zachycených nástroje rozhraní příkazového řádku Azure není zachytit disků připojená data)

    - Vypnutí počítače prostřednictvím portálu
    - Klikněte na snímku a zadejte název obrázek jako **mariadb galera obrázek** a zadat popis a kontrola "Můžu spuštění waagent".
    ![Zachycení virtuální počítač](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![zachytit virtuálního počítače](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Vytvoření clusteru

Vytvoření 3 VMs mimo šabloně jste právě vytvořili a potom konfiguraci a spuštění clusteru.

1. Vytvoření první OM 7 CentOS z obrázku **mariadb galera obrázek** jste vytvořili, poskytující název virtuální sítě **mariadbvnet** a podsítě **mariadb**počítače velikost **Střední**, předávání ve službě Cloud název je **mariadbha** (nebo libovolný jiný název, který chcete k nim získat přístup prostřednictvím mariadbha.cloudapp.net) nastavení na název tohoto počítače **mariadb1** a uživatelské jméno je **azureuser**a povolení přístupu SSH a předávání SSH certifikátů .pem souboru a nahrazování **/path/to/key.pem** s cestou místo, kam jste uložené klávesu SSH vygenerovaných .pem.

    > [AZURE.NOTE] Příkazy dole se dělí na více řádků pro projekty, ale měli zadat každého jako jeden řádek.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. Vytvoření 2 Další virtuálních počítačích _připojením_ do právě vytvořených **mariadbha** cloudové služby, změňte **Název OM** , jakož i **SSH port** na jedinečné není v konfliktu s jiných VMs ve stejném cloudové služby.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
a MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Budete potřebovat zobrazíte interní IP adresu každého 3 VMs pro další krok:

    ![Získání IP adresa](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH do 3 VMs a a úpravy konfiguračního souboru na každém

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** a **`wsrep_cluster_address`** odebráním **#** na začátku a ověření jsou skutečně co potřebujete.
    Kromě toho nahradit **`<ServerIP>`** v **`wsrep_node_address`** a **`<NodeName>`** v **`wsrep_node_name`** OM IP adres pojmenujte respektive a zrušte komentář tyto řádky stejně.

5. Zahájení clusteru na MariaDB1 a nechejte ji při spuštění

        sudo service mysql bootstrap
        chkconfig mysql on

6. Spuštění MySQL MariaDB2 a MariaDB3 a nechejte ji při spuštění

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Clusteru vyrovnávání zatížení
Po vytvoření skupinový VMs byly přidány do dostupnosti sady s názvem **clusteravset** zajistěte, aby že se umístění na jiné poruch a aktualizovat domény a že Azure nikdy znamená údržby na všech počítačích najednou. Konfigurace splňuje požadavky se plánuje podpora tak, že tento Azure služby úroveň dohodu (SLA).

Teď můžete umožňuje Vyrovnávání zatížení Azure zůstatek požadavky mezi naše 3 uzlů.

Spustit pod příkazy na vašem počítači pomocí rozhraní příkazového řádku Azure.
Struktura parametry příkaz je:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Nakonec protože rozhraní příkazového řádku nastaví interval zkušební Vyrovnávání zatížení 15 sekund, (které může být trochu příliš dlouho.), ho změníte v portálu v části **koncové body** některého z VMs

![Úprava koncový bod](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

potom klikněte na nastavení Reconfigure The Load-Balanced a přejděte další

![Změňte konfiguraci načíst rovnováha sadu](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

pak můžete změnit Probe Interval 5 sekund a uložit

![Změna intervalu zkušební](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Ověřování clusteru

Pevné práci. Clusteru by měl být teď přístupné na `mariadbha.cloudapp.net:3306` které bude přístupů Vyrovnávání zatížení a směrování požadavky mezi 3 VMs hladce a efektivně.

Pomocí Oblíbené klienta MySQL připojit nebo jenom připojit z jednoho z VMs k ověření, že tento cluster pracuje.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Vytvořte novou databázi a doplníte některá data

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Zobrazí výsledek v následující tabulce

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

V tomto článku jste vytvořili 3 uzel MariaDB + Galera vysoce dostupné obrázku na virtuálních počítačích Azure systém CentOS 7. VMs rozloženy s služby Vyrovnávání zatížení Azure.

Je vhodné podívejte se na [Další způsob, jak clusteru MySQL na Linux](virtual-machines-linux-classic-mysql-cluster.md) a způsoby, jak [optimalizovat a otestujte MySQL výkon Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Vytvoření SSH klíč pro ověřování]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[problém #1268 v rozhraní příkazového řádku Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268
