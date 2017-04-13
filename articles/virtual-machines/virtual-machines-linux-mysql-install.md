<properties
    pageTitle="Nastavení MySQL na Linux OM | Microsoft Azure "
    description="Zjistěte, jak nainstalovat na počítač virtuální Linux (se systémem Ubuntu RedHat řady nebo OS) v Azure zásobníku MySQL"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Jak nainstalovat MySQL na Azure


V tomto článku se dozvíte, jak můžete nainstalovat a nakonfigurovat MySQL v Azure virtuální počítači spuštěná Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>Na počítači virtuální nainstalovat MySQL

> [AZURE.NOTE] Už musíte mít Microsoft Azure virtuální počítač Linux dokončení tohoto kurzu. Přečtěte si téma [Azure Linux OM kurz](virtual-machines-linux-quick-create-cli.md) vytvoření a nastavení Linux OM s `mysqlnode` jako název OM a `azureuser` jako uživatel před pokračováním.

V tomto případě použití 3306 portu jako MySQL port.  

Připojení k Linux OM vytvořený prostřednictvím nátěrové. Pokud při prvním použití Azure Linux OM, přečtěte si, jak používat nátěrové připojení k Linux OM [tady](virtual-machines-linux-mac-create-ssh-keys.md).

Použijeme úložiště balíčku nainstalovat MySQL5.6 jako příklad v tomto článku. MySQL5.6 má ve skutečnosti, další zlepšení výkonu než MySQL5.5.  Další informace [v tomto poli](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Jak nainstalovat MySQL5.6 systémem Ubuntu
Linux OM se systémem Ubuntu z Azure použijeme tady.

- Krok 1: Instalace MySQL serveru 5.6 přepnutí `root` uživatele:

            #[azureuser@mysqlnode:~]sudo su -

    Nainstalujte mysql server 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Během instalace zobrazí se dialogovém okně okno poping až do zeptejte se, že je možné nastavit MySQL kořenové heslo a potřebujete tady si můžete nastavit heslo.

    ![Obrázek](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Zadejte heslo potvrďte.

    ![Obrázek](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Krok 2: Přihlášení na Server MySQL

    Po dokončení instalace server MySQL, bude automaticky spuštěná služba MySQL. Přihlášení MySQL Server s `root` uživatele.
    Použití pod příkaz pro přihlášení a zadání hesla.

             #[root@mysqlnode ~]# mysql -uroot -p

- Krok 3: Správa pracovního služby MySQL

    a získat stav služby MySQL

             #[root@mysqlnode ~]# service mysql status

    (b) spusťte službu MySQL

             #[root@mysqlnode ~]# service mysql start

    (c) zastavení služby MySQL

             #[root@mysqlnode ~]# service mysql stop

    (d) restartovat službu MySQL

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Jak nainstalovat MySQL na červené klobouk operačních systémů jako CentOS, Oracle Linux
Linux OM s CentOS nebo Oracle Linux použijeme tady.

- Krok 1: Přidání úložiště MySQL Yum přepnout do `root` uživatele:

            #[azureuser@mysqlnode:~]sudo su -

    Stažení a instalace balíček MySQL vydaná verze:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Krok 2: Upravte pod soubor, který chcete povolit úložiště MySQL pro její stažení balíčku MySQL5.6.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Aktualizace každou hodnotu tohoto souboru níže:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Krok 3: Instalace MySQL z úložiště MySQL instalace MySQL:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL ot zabalit a všech souvisejících balíčků nainstaluje.

- Krok 4: Správa pracovního služby MySQL

    (a) zkontrolujte stav služby MySQL server:

           #[root@mysqlnode ~]#service mysqld status

    (b) zkontrolujte, jestli je spuštěný výchozí port MySQL server:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) spusťte MySQL server:

           #[root@mysqlnode ~]#service mysqld start

    (d) přestat MySQL server:

           #[root@mysqlnode ~]#service mysqld stop

    (e) nastavení MySQL při spuštění systému spouštěcí zdola nahoru:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Jak nainstalovat MySQL SUSE Linux
Linux OM s OpenSUSE použijeme tady.

- Krok 1: Stažení a instalace MySQL Server

    Přepnutí do `root` uživatelů prostřednictvím pod příkaz:  

           #sudo su -

    Stažení a instalace MySQL balíčku:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Krok 2: Správa pracovního služby MySQL

    (a) zkontrolujte stav MySQL server:

           #[root@mysqlnode ~]# rcmysql status

    (b) zaškrtněte zda výchozí port MySQL server:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) spusťte MySQL server:

           #[root@mysqlnode ~]# rcmysql start

    (d) přestat MySQL server:

           #[root@mysqlnode ~]# rcmysql stop

    (e) nastavení MySQL při spuštění systému spouštěcí zdola nahoru:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Další krok
Hledání dalších použití a informace o MySQL [tady](https://www.mysql.com/).
