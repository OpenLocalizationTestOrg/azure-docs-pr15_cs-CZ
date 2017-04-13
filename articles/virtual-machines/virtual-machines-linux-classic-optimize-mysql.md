<properties
    pageTitle="Optimalizace výkonu MySQL na Linux VMs | Microsoft Azure"
    description="Zjistěte, jak optimalizovat MySQL spuštěna Azure virtuální počítač (OM) Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimalizace výkonu MySQL na Azure Linux VMs

Existuje spousta faktorů, které mít vliv na výkon MySQL na Azure, v konfiguraci výběru a software virtuální hardwaru. Tento článek se zaměřuje na optimalizace výkonu až úložiště, systému a konfigurace databáze.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Využití RAID na Azure virtuálního počítače
Úložiště je faktor klíčové ovlivňuje výkon databáze v cloudu prostředí.  Srovnání jeden disk, RAID umožňují rychlejší přístup prostřednictvím souběžné.  Podrobnější informace v nápovědě k [RAID úroveň](http://en.wikipedia.org/wiki/Standard_RAID_levels) .   

Disk vstupu a výstupu výkonu a vstupu a výstupu dobu odezvy v Azure lze výrazně zvýšit prostřednictvím RAID. Náš laboratorní testů vstupu disku a výstupu může být dvojitá výkon a doba odezvy v/v operací můžete sníží o polovinu průměrně při dvojitý počet disků RAID (2 až 4, 4 až 8 atd.). Další informace naleznete v [dodatku A](#AppendixA) .  

Kromě disku vstupu a výstupu zlepšuje výkon MySQL při zvýšit úroveň RAID.  Další informace naleznete v [Dodatku B](#AppendixB) .  

Můžete také zvažte velikost bloku. Obecně když budete mít větší velikost bloku, zobrazí se vám nižší nároky zvlášť pro velké zápisy. Pokud velikost bloku je příliš velký, může přidat další režijních a nelze využít RAID. Aktuální výchozí velikost je 512KB, což je ověřené být optimální pro většinu provozním prostředí. Další informace najdete v článku [Dodatku C](#AppendixC) .   

Všimněte si, že jsou na kolik discích můžete přidat různé virtuálního počítače typy omezení. Tyto limity jsou uvedené v [virtuální počítač a velikosti cloudové služby Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Přestože můžete nastavit RAID méně disků budete potřebovat 4 disků připojená data v našem příkladu RAID v tomto článku.  

V tomto článku se předpokládá jste již vytvořili Linux virtuálního počítače a MYSQL nainstalovali a nakonfigurovali. Další informace o Začínáme získáte v tom, jak nainstalovat MySQL na Azure.  

###<a name="setting-up-raid-on-azure"></a>Nastavení RAID na Azure
Následující postup vytvoření RAID na Azure na portálu Azure klasické. Můžete taky nastavit RAID pomocí skriptů Windows Powershellu.
V tomto příkladu jsme nastaví její konfiguraci raid0 4 discích.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Krok 1: Přidání disku dat do virtuálního počítače  

Na stránce virtuálních počítačích portálu Azure klasické na virtuálního počítače, ke kterému chcete přidat data disku. V tomto příkladu je virtuální počítač mysqlnode1.  

![][1]

Na stránce virtuálního počítače klikněte na **řídicí panel**.  

![][2]


V hlavním panelu klikněte na tlačítko **Připojit**.

![][3]

A potom klikněte na **prázdný disk připojit**.  

![][4]

Data disků **Hostitele mezipaměti předvoleb** by měl nastavit **žádný**.  

Tím přidáte jeden prázdný disk do virtuálního počítače. Opakujte tento krok tři krát tak, aby měli 4 dat disků RAID.  

Přidané jednotek ve počítače virtuální zobrazíte tak, že prohlížíte protokol jádra zpráv. Například se toto na systémem Ubuntu, můžete tento příkaz:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Krok 2: Vytvoření RAID s další
Postupujte podle tohoto článku podrobnosti RAID instalační program:  

[Konfigurace software RAID na Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Pokud používáte systém souborů XFS, postupujte následujícím způsobem po vytvoření RAID.

Pokud chcete nainstalovat XFS Debian, se systémem Ubuntu nebo Linux máta, použijte tento příkaz:  

    apt-get -y install xfsprogs  

Pokud chcete nainstalovat XFS na Fedora, CentOS nebo RHEL, použijte tento příkaz:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Krok 3: Nastavení nové cesty úložiště
Použijte tento příkaz:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Krok 4: Zkopírování původní data do nové cesty úložiště
Použijte tento příkaz:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Krok 5: Oprávnění k úpravám, můžete získat přístup MySQL (čtení a zápis) disku dat
Použijte tento příkaz:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Úprava algoritmu plánování vstupu a výstupu disku
Linux implementuje čtyři typy vstupu a výstupu plánování algoritmů:  

-   Algoritmus nedostupný (NoOp) (bez operace)
-   Algoritmus konečný termín (termínu)
-   Úplně veletrhu fronty algoritmus (CFQ)
-   Rozpočet období algoritmus (Anticipatory)  

Můžete vybrat jiné plánovače vstupu a výstupu v různých scénářích pro optimalizaci výkonu. V prostředí úplně RAM není velký rozdíl mezi CFQ a konečného termínu algoritmů výkon. Obecně doporučujeme nastavit prostředí databáze MySQL konečného termínu stability. Pokud je hodně sekvenční vstupu a výstupu, CFQ snižte výkon vstupu a výstupu disku.   

SSD a jiné zařízení můžete pomocí nedostupný (NoOp) nebo termín dosažení vyšší výkon než výchozí plánovač.   

Výchozí plánování algoritmus vstupu a výstupu není z jádra 2,5 konečného termínu. Počínaje jádra 2.6.18 CFQ stal výchozí plánování algoritmus vstupu a výstupu.  Můžete určit toto nastavení používají při spouštění jádra nebo dynamicky toto nastavení změnit, když běží systém.  

Následující příklad ukazuje, jak zaškrtněte políčko a nastavte výchozí plánovač do algoritmu nedostupný (NoOp).  

Pro Debian distribuční skupině:

###<a name="step-1view-the-current-io-scheduler"></a>Krok 1 zobrazení aktuální vstupu/výstupu Plánovač
Použijte tento příkaz:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Zobrazí se následující výstup, který označuje aktuální plánovač.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Krok 2. Změna na aktuální zařízení (/ vývojáře/sda) vstupu a výstupu plánování algoritmus
Pomocí následujících příkazů:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Toto nastavení pro /dev/sda samotný není užitečné. Je třeba nastavit na všech dat discích kde jsou uložená v databázi.  

Měli byste vidět následující výstup označovat tak, že tento grub.cfg byl úspěšně znovu a že výchozí plánovač aktualizovaný nedostupný (NoOp).  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Pro rodinu rozdělení Redhat stačí tento příkaz:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Konfigurace nastavení operace systému souborů
Jeden doporučený postup je zakázání funkce protokolování atime v systému souborů. Atime je posledního přístup k souboru. Pokaždé, když souboru pracuje, systém souborů časového razítka záznamů v protokolu. Tyto informace se však jen zřídka používá. Pokud nepotřebujete, které omezí celkovou dobu přístupu na disk můžete ho zakázat.  

Jak zakázat atime protokolování, potřebujete upravit soubor systému konfigurační soubor/etc / fstab a používalo možnost **noatime** .  

Například upravte soubor /etc/fstab vim přidáním noatime, jak je ukázáno v následujícím příkladu.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Znovu připojte systém souborů pomocí následujícího příkazu:  

    mount -o remount /RAID0

Otestujte změněné výsledek. Všimněte si, že při úpravě testovací soubor čas přístupu neaktualizuje.  

Před příklad:     

![][5]

Po příklad:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Zvětšení maximální počet systému pomocí úchytů pro vysokou souběžné
MySQL je vysoké souběžné databáze. Výchozí počet souběžné úchyty je 1024 Linux, které nestačí vždy. **Pomocí následujících kroků můžete zvýšit maximální souběžné úchyty systém podporuje vysoké souběžné MySQL**.

###<a name="step-1-modify-the-limitsconf-file"></a>Krok 1: Upravit soubor limits.conf
Přidejte následující čtyři řádky v souboru /etc/security/limits.conf zvětšíte maximální povolený souběžné úchyty. Všimněte si, že 65536 největší číslo, které podporují systému.   

    * měkké nofile 65536
    * pevné nofile 65536
    * měkké nproc 65536
    * pevné nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Krok 2: Aktualizace systému pro nové limity
Spusťte následující příkazy:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Krok 3: Ujistěte se, omezení se aktualizují při spuštění
Následující příkazy při spuštění zobrazovat v souboru /etc/rc.local, takže ho se projeví při každém spuštění.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optimalizace databáze MySQL
Můžete stejné strategie ladění výkonu pro konfiguraci MySQL Azure jako v místním počítači.  

Hlavní pravidla optimalizace vstupu a výstupu jsou:   

-   Zvětšete velikost mezipaměti.
-   Zkrácení doby odezvy vstupu a výstupu.  

Optimalizovat nastavení server MySQL, můžete aktualizovat soubor my.cnf, což je výchozí soubor konfigurace serveru a klientských počítačích.  

Následující položky konfigurace jsou hlavní faktory, které ovlivňují MySQL výkon:  

-   **innodb_buffer_pool_size**: fondu vyrovnávací paměť obsahuje pamětí data a index. Obvykle to nastavenou 70 % fyzické paměti.
-   **innodb_log_file_size**: Toto je velikost protokolu znovu. Protokoly znovu použít k zajištění zápisu rychlé spolehlivý a obnovit po zhroucení. Tato možnost o velikosti 512MB, které vám nabídne dostatek místa pro protokolování zápisu.
-   **max_connections**: někdy aplikací nezavírejte připojení správně. Větší hodnotu vám umožní serveru delší dobu odpadkového nečinný připojení. Maximální počet připojení 10000, ale doporučené maximum 5000.
-   **Innodb_file_per_table**: Toto nastavení povolit nebo zakázat možnost InnoDB uložení tabulky v samostatných souborech. Zapněte možnost zajistí, že několik operace rozšířené správy se dají použít efektivně. Z výkonu ho dosažení vyššího přenosu místa tabulky a optimalizovat výkon správy suť. Doporučené nastavení pro to je vlastně zapnuto.</br>
    Z 5.6 MySQL je ve výchozím nastavení zapnuto. Proto není nutná žádná akce. Výchozí nastavení ve všech verzích, která není starší než 5.6, je vypnuto. Zapnutí této dál je potřeba. A má použít ji před načtením, protože se týká jenom nově vytvořené tabulky.
-   **innodb_flush_log_at_trx_commit**: výchozí hodnota 1, s rozsahem nastavena na hodnotu 0 ~ 2. Výchozí hodnota je nejvhodnější možnost pro samostatnou databáze MySQL. Nastavení 2 umožňuje většina integrity dat a je vhodné pro předlohy v MySQL obrázku. Nastavení 0 umožňuje ztráty dat, což může ovlivnit spolehlivost: v některých případech se lepší výkon a je vhodné pro podřízený MySQL clusteru.
-   **Innodb_log_buffer_size**: vyrovnávací paměť protokolu umožňuje transakce, které chcete spustit, aniž byste museli vyprázdnění protokol disk před transakcí potvrzení. Pokud existuje velké binární objektu nebo textové pole, mezipaměti bude velmi rychle spotřebované a vstupu časté disku a výstupu bude aktivován. Je lepší zvětšení vyrovnávací paměť nejsou-li proměnná stavu Innodb_log_waits 0.
-   **query_cache_size**: nejlepší možností je zakázat od začátku. Query_cache_size na hodnotu 0 (Toto je teď výchozí nastavení v MySQL 5.6) a použít jiné metody účelem dosažení vyššího dotazů.  

Naleznete v [Dodatku D](#AppendixD) pro porovnání po optimalizaci výkonu.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Zapnutí protokolu pomalá dotazu MySQL analýza kritické výkonu
Protokolu pomalá dotazu MySQL vám může pomoct zjistit pomalé dotazů pro MySQL. Po povolení protokolu pomalá dotazu MySQL, můžete pomocí nástrojů MySQL jako **mysqldumpslow** k identifikaci kritický výkonu.  

Upozorňujeme, že ve výchozím nastavení to není povolené. Zapnutí protokolu pomalá dotazu může zabrat několik procesoru zdrojů. Proto doporučujeme povolit to dočasné řešení kritické.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Krok 1: Úprava my.cnf soubor přidáním následující řádky do konce   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Krok 2: Restartujte mysql server
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Krok 3: Zkontrolujte, zda nastavení se neprojeví pomocí příkazu "Zobrazit"

![][7]   

![][8]

V tomto příkladu uvidíte, že funkce pomalé dotazu má zapnutý. Pak můžete nástroj **mysqldumpslow** určit kritické a optimalizovat výkon, například přidání indexy.





##<a name="appendix"></a>Dodatek

Následující ukázkový výkonu testovací data na cílových testovacím prostředí, poskytují obecné na trendu výkonu dat jiné přístupy, ladění výkonu však výsledky lišit podle různých verzí prostředí nebo produktu.

<a name="AppendixA"></a>Dodatek A:  
**Výkon disku (procesorů) s jinou RAID úrovně**


![][9]

**Test příkazy:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Poznámka: Pracovní zátěž tento test používá 64 vláknech snaží kontaktovat horní mez RAID.

<a name="AppendixB"></a>Dodatek B:  
**Porovnání výkon (výkon) MySQL s jinou RAID úrovně**   
XFS systému souborů)


![][10]  
![][11]

**Test příkazy:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Porovnání výkon (OLTP) MySQL s jinou RAID úrovně**  
![][12]

**Test příkazy:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Dodatek C:   
**Porovnání výkon (procesorů) disku pro různé bloku velikosti**  
XFS systému souborů)


![][13]

**Test příkazy:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Poznámka: velikost souboru pro tento testování respektive je 30GB a 1, s RAID 0(4 disks) XFS souboru systému.


<a name="AppendixD"></a>Dodatek D:  
**Porovnání výkon (výkon) MySQL v dřívější a novější optimalizace**  
(XFS File System)


![][14]

**Test příkazy:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Konfigurace nastavení pro výchozí a optimalizaci vypadá takto:**

|Parametry |Výchozí    |optimalizaci
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Žádná   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Více podrobné parametry konfigurace optimalizace získáte úřední pokyny mysql.  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testovacím prostředí**  

|Hardwarová   |Podrobnosti
|-----------|-------
|Procesor    |AMD Opteron(tm) procesor 4171 HE / 4 vzorky
|Paměti |14G
|disk   |10G či disketa
|operační systém |Systémem Ubuntu 14.04.1 l



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
