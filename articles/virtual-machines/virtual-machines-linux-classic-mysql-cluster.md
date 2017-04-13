<properties
    pageTitle="Clusterize MySQL sadami Vyrovnávání zatížení | Microsoft Azure"
    description="Nastavení Vyrovnávání zatížení, vysoké dostupnost Linux MySQL clusteru vytvořená pomocí klasické nasazení modelu v Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Pomocí vyrovnávání zatížení sady clusterize MySQL na Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Účelem v tomto článku je zkoumání a znázornění jiné přístupy k dispozici pro nasazení vysoce na základě Linux službám na Microsoft Azure prozkoumání MySQL Server dostupnost jako základy. Video znázorňující tento přístup je dostupná na [kanál 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Uvádíme sdílené není nic dvěma uzly jednoduchým předlohy MySQL dostupnost řešení založené na DRBD, Corosync a kardiostimulátor. Pouze jeden uzel běží MySQL najednou. Čtení a zápis ze zdroje DRBD je také omezena pouze jeden uzel najednou.

Protože sady Vyrovnávání zatížení Microsoft Azure použijeme k poskytování funkci kruhového a koncový bod zjišťování, odebrání i úspěšné obnovení VIP není potřeba řešení VIP jako LVS. VIP je globálně směrovatelné adresy IPv4 přidělil Microsoft Azure při prvním vytvoření cloudovou službu.

Nejsou k dispozici jako OM na [OM sklad](http://vmdepot.msopentech.com)další možné architektury MySQL včetně NBD clusteru, Percona a Galera, jakož i několik middleware řešení, včetně alespoň jedno. Dokud tato řešení replikovat na unicast porovnání vícesměrového nebo vysílání a nepoužívejte sdílené úložiště nebo více síťových rozhraní, třeba scénáře snadno pro nasazení Microsoft Azure.

Samozřejmě tyto clusterů architektury lze rozšířit o další produkty, jako třeba PostgreSQL a OpenLDAP na podobně. Například tato vyrovnávání postup s sdílených není nic zatížení byla úspěšně testováno s více předlohy OpenLDAP a můžete se podívat se na našeho blogu Channel 9.

## <a name="getting-ready"></a>Příprava

Budete potřebovat účet Microsoft Azure platné předplatné moct vytvářet nejméně dvě (2) VMs (Křížky byla v tomto příkladě použita), v síti a podsítě skupinu spřažení nastavit a dostupné, i možnost vytvořit nový VHD ve stejné oblasti jako cloudovou službu a připojte k Linux VMs.

### <a name="tested-environment"></a>Testované prostředí

- Se systémem Ubuntu 13.10
  - DRBD
  - MySQL Server
  - Corosync a kardiostimulátor

### <a name="affinity-group"></a>Skupina spřažení

Skupina spřažení řešení se vytvořil protokolování do Azure klasické portálu posouvání dolů na nastavení a vytvoření nové skupiny spřažení. Vytvoření později přidělené zdrojů přiřadíte k této skupině spřažení.

### <a name="networks"></a>Sítě

Vytvoří novou síť a podsítě vznikne uvnitř sítě. Zvolili jsme 10.10.10.0/24 sítě s jedinou /24 podsítí uvnitř.

### <a name="virtual-machines"></a>Virtuálních počítačích

První 13.10 OM systémem Ubuntu je vytvořen pomocí Galerie Endorsed systémem Ubuntu a s názvem `hadb01`. Vytvoří se nový Cloudová služba ve proces se nazývá hadb. Jsme zavolejte ho tímto způsobem ke znázornění povahy sdílené, Vyrovnávání zatížení, že služba bude mít při jsme přidávat další materiály. Vytvoření `hadb01` je bezproblémové a dokončené na portálu. Koncový bod pro SSH se automaticky vytvoří a naší síti vytvořené zaškrtnuté. Jsme také rozhodnout pro vytvoření nového dostupnost pro VMs.

Po prvním OM je vytvořen (doslova, při cloudovou službu) jsme přejít k vytváření druhý OM `hadb02`. Pro druhý OM taky použijeme systémem Ubuntu 13.10 OM z Galerie na portálu, ale nemůžeme se rozhodnete sdělit nám existující cloudové služby, `hadb.cloudapp.net`, místo abyste vytvářeli novou. Nastavení sítě a dostupnosti by měla být automaticky vybraná us. Koncový bod SSH se vytvoří, příliš.

Po vytvoření obou VMs jsme vezme SSH port pro `hadb01` (TCP 22) a `hadb02` (automaticky přiřazené tak, že Azure)

### <a name="attached-storage"></a>Připojené úložiště

Připojení nového disku k oběma VMs jsme vytvoření nových disků 5 GB v procesu. Disků bude hostované v kontejneru virtuální pevný disk se používá pro naše disků hlavní operační systém. Po vytvoření a přiřazení disků je bez nutnosti nám restartujte Linux jako jádra uvidí na nové zařízení (obvykle `/dev/sdc`, můžete zkontrolovat `dmesg` pro výstup)

Na každé OM jsme přejít k vytvoření nového oddílu `cfdisk` (primární, Linux oddíl) a zapisovat do nové tabulky oddíl. **Nevytvářejte systému souborů v tomto oddílu** .

## <a name="setting-up-the-cluster"></a>Nastavení clusteru

V obou VMs systémem Ubuntu potřebujeme umožňuje instalaci Corosync, kardiostimulátor a DRBD byt č. Použití `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Neinstalovat MySQL v současné době** . Debian a systémem Ubuntu instalační skripty spustí datového adresáře MySQL na `/var/lib/mysql`, ale od v adresáři se nahrazen příkazem DRBD systému souborů, potřebujeme to provést později.

V tomto okamžiku jsme také ověřte (pomocí `/sbin/ifconfig`), jak VMs používáte adres podsítí 10.10.10.0/24 a že jsou navzájem ping podle názvu. Podle potřeby můžete taky použít `ssh-keygen` a `ssh-copy-id` a ujistěte se, obě VMs můžete komunikovat SSH bez nutnosti hesla.

### <a name="setting-up-drbd"></a>Nastavení DRBD

Vytvoříme DRBD prostředek, který používá základní `/dev/sdc1` oddílu, který chcete vytvořit `/dev/drbd1` zdroje mohou být naformátován pomocí ext3 a použít v primárních a sekundárních uzlů. K tomuto účelu otevřete `/etc/drbd.d/r0.res` a zkopírujte následující definice zdroje. V obou VMs udělejte toto:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Po tak Inicializace zdroje pomocí `drbdadm` v obou VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

A nakonec na primárním (`hadb01`) vynutit vlastnictví (primární) DRBD zdroje:

    sudo drbdadm primary --force r0

Pokud si prohlédnete obsah/procedury/drbd (`sudo cat /proc/drbd`) na obou VMs byste měli vidět `Primary/Secondary` na `hadb01` a `Secondary/Primary` na `hadb02`a konzistentní s řešením v tomto okamžiku. 5 GB disku bude se synchronizovat přes síť 10.10.10.0/24 zdarma zákazníkům.

Po synchronizaci disku můžete vytvořit souborů na `hadb01`. Pro účely testování jsme použili ext2 ale vytvoří systému souborů ext3 následujících pokynů:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Připojení zdroje DRBD

Na `hadb01` můžeme začít nyní připojit DRBD zdroje. Použití debian a deriváty `/var/lib/mysql` jako společnosti MySQL datového adresáře. Protože jsme nenainstalovali MySQL, jsme vytvoříte adresář a připojit DRBD zdroje. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>Nastavení MySQL

Teď si budete chtít nainstalovat MySQL `hadb01`:

    sudo apt-get install mysql-server

Pro `hadb02`, máte dvě možnosti. Můžete nainstalovat server mysql nyní, který bude vytvářet /var/lib/mysql vyplnit nového datového adresáře a potom pokračujte odebrat obsah. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Druhou možností je převezme `hadb02` a nainstalujte server mysql tam (instalační skripty všimnete stávající instalaci a nebude dotykového ovládání)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Pokud nemáte v plánu převzetí DRBD nyní, je jednodušší sice pravděpodobně nižší elegantní první možnost. Po nastavení to, můžete začít pracovat na databáze MySQL. Na `hadb02` (nebo libovolný serverů je aktivní, podle DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Upozornění**: Tento posledního příkazu efektivně zakáže ověřování pro uživatele root v této tabulce. To by měl být nahrazen výrobní testu udělit příkazy a je zahrnut pouze pro účely příkladů.

Bude potřeba povolit sítě pro MySQL Pokud mají být dotazy z mimo VMs, což je účelu této příručce. V obou VMs otevřete `/etc/mysql/my.cnf` a přejděte k `bind-address`, změňte z 127.0.0.1 na 0.0.0.0. Po uložení souboru, problémů `sudo service mysql restart` na aktuální primární.

### <a name="creating-the-mysql-load-balanced-set"></a>Vytváření načíst MySQL rovnováha nastavení

Jsme se vraťte se do portálu a přejděte `hadb01` OM a potom koncové body. Jsme se vytvořit nový koncový bod, zvolte MySQL (TCP 3306) z rozevíracího seznamu a značky v okně *vytvořit nový zatížení rovnováha nastavení* . Název koncového bodu naše rozloženy `lb-mysql`. Jsme ponechá většina možností samotný s výjimkou čas, které jsme budete snížit až 5 (v sekundách, minimální)

Po vytvoření koncového bodu jsme přejděte na `hadb02`, koncové body a vytvořit nové endpoint ale doporučujeme zvolit `lb-mysql`, v rozevírací nabídce vyberte MySQL. Můžete taky Azure rozhraní příkazového řádku pro tento krok.

V tomto okamžiku máme všechno potřebné pro ruční operace clusteru.

### <a name="testing-the-load-balanced-set"></a>Testování načíst rovnováha nastavení

Testuje lze provést z jiné než počítače, pomocí libovolného MySQL klienta, jakož i aplikace (například phpMyAdmin spuštěný jako webu Azure) v tomto případě jsme použili nástroj příkazového řádku na MySQL na jiné pole Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Ruční nedaří myši

Převzetí služeb při selhání můžete simulovat teď vypnutí MySQL, přechod na DRBD primární a znovu spustíte MySQL.

Na hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Pak klikněte na hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Jakmile je převzetí ručně opakujte vzdálené dotazu a by měl funguje dokonale.

## <a name="setting-up-corosync"></a>Nastavení Corosync

Corosync je základní infrastruktura obrázku požadované pro kardiostimulátor pracovat. Pro prezenční signál uživatelé v1 a v2 (a dalších metodologie jako Ultramonkey) Corosync je rozdělení CRM funkcí, zatímco kardiostimulátor zůstane podobné předávání funkčnosti.

Hlavní omezení pro Corosync Azure je Corosync přednost multicast přes vysílání přes unicast komunikace, že sítí Microsoft Azure podporuje pouze unicast.

Naštěstí Corosync má pracovní jednosměrovém režimu a pouze skutečné omezení, protože všechny uzly nejsou komunikaci mezi sebou *automagically*, potřebujete definovat uzlů v souborech konfigurace, včetně jejich adres IP. Soubory příklad Corosync můžete použít pro jednosměrového a jenom změnit svázat adresa, uzel seznamy a adresář protokolování (používá se systémem Ubuntu `/var/log/corosync` během příkladu soubory použití `/var/log/cluster`) a povolení kvora nástroje.

**Poznámku `transport: udpu` směrnice níže a ručně definovaných IP adres pro uzly**.

Na `/etc/corosync/corosync.conf` pro oba uzly:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Zkopírujte tento soubor konfigurace v obou VMs jsme zahájení Corosync v obou uzly:

    sudo service start corosync

Brzy po spuštění služby clusteru stanovit v aktuálním vyzvánět a kvora by měla být tvořena. Jsme kontrolou prvek pro tuhle funkci Kontrola protokoly nebo:

    sudo corosync-quorumtool –l

Postupujte podle výstup podobně jako na následujícím obrázku:

![corosync quorumtool - l ukázkový výstup](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Nastavení kardiostimulátor

Kardiostimulátor používá clusteru sledovat pro zdroje, definovat při základní barvy přejděte a přejděte druhotné zdroje. Zdroje lze definovat některý ze sady dostupných skriptů nebo ze skriptů LSB (inicializace like), mezi další možnosti.

Chceme kardiostimulátor "vlastní" DRBD zdroji BodPřipojení a službě MySQL. Pokud kardiostimulátor můžete zapnout nebo vypnout DRBD, připojte ho / dokončení umount a spuštění nebo zastavení MySQL ve správném pořadí při něco chybná se stane po instalaci primární naše nastavení.

Při první instalaci kardiostimulátor konfiguraci by měl být jednoduchý dostatečně, něco jako:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Zkontrolujte, jestli je spuštěním `sudo crm configure show`. Dále vytvořte souboru (například `/tmp/cluster.conf`) s v následujících zdrojích:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

A teď načíst do konfigurace (potřebujete jenom v jednom uzlu):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Nezapomeňte, že kardiostimulátor spustí při spuštění v obou uzlů:

    sudo update-rc.d pacemaker defaults

Po pár sekund a práce s nimi `sudo crm_mon –L`, ověřte, zda nějaká uzly stal předlohy pro cluster a běží všechny zdroje. Připojení a ps umožňuje zkontrolujte, jestli je spuštěný zdroje.

Následující obrazovka ukazuje `crm_mon` s uzlů zastaveno (konec pomocí ovládacího prvku-C)

![Přerušili crm_mon uzel](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

A tento snímek obrazovky znázorňuje uzlech s jednu předlohu a jeden podřízeného:

![podřízený provozní předlohy crm_mon](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testování

Mrzí jste připraveni na automatické převzetí simulace. Existují dva způsoby, jak to: dočasná a pevná. Měkké tak, jak používá funkce vypnutí clusteru: ``crm_standby -U `uname -n` -v on``. Tímto způsobem v předloze bude podřízeného převzít řízení. Nezapomeňte, nastavte zpět vypnuto (crm_mon se dozvíte, jeden uzel je v úsporném režimu)

Pevné způsobem je vypnutí primární OM (hadb01) prostřednictvím portálu nebo změna runlevel na OM (tedy zastavení, vypnutí) a pak nám pomáháte Corosync a kardiostimulátor tak, že signalizace předlohy přechod dolů. Jsme to vyzkoušet (užitečné pro zachování windows), ale nemůžeme můžete vynutit scénáře jenom ukotvením OM.

## <a name="stonith"></a>STONITH

Je třeba umožnit o vystavení OM vypnutí prostřednictvím rozhraní příkazového řádku Azure namísto STONITH skript, který určuje fyzické zařízení. Můžete použít `/usr/lib/stonith/plugins/external/ssh` jako základní a povolit STONITH v konfiguraci clusteru. Globálně nainstalovaný Azure rozhraní příkazového řádku a publikovat nastavení/profil by měl načíst clusteru uživateli.

Ukázka kód pro daný zdroj k dispozici na [GitHub](https://github.com/bureado/aztonith). Potřebujete změnit clusteru konfigurace přidáním podle následujících pokynů `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Poznámka:** skript nechová nahoru nebo dolů kontroly. Původní zdroj SSH vám to 15 kontroly ping ale obnovení dobu Azure OM může být více proměnné.

## <a name="limitations"></a>Omezení

Platí následující omezení:

- Skript linbit DRBD prostředku, který má na starosti DRBD jako zdroj kardiostimulátor použití `drbdadm down` vypnutí uzel, i když na uzel má dělat jenom úsporném režimu. Toto je ideální od na podřízený nesmí být synchronizace zdroje DRBD během předloze získá zápisy. Pokud na předlohu zdvořile nezdaří, můžete na podřízený převzít řízení starší stavu systému souborů. Existují dva způsoby možných řešení takto:
  - Vynucení `drbdadm up r0` ve všech uzlech prostřednictvím místní sledovacích (ne clusterized), případně
  - Úpravy linbit DRBD skriptu a ověřte, zda `down` není jen v `/usr/lib/ocf/resource.d/linbit/drbd`.
- Vyrovnávání zatížení potřebuje alespoň na úrovni 5 sekund, než se reagovat, aby aplikace by měl být vědom obrázku a být příjemnější časového limitu; jiným architekturám plochy můžete taky Nápověda, například v aplikaci fronty middlewares dotazu, atd.
- Optimalizace MySQL je nezbytné k zajištění psaní probíhá sane tempem a jsou na disk se často abyste minimalizovali ztráty paměti vyprázdnění mezipaměti
- Psaní výkon budou závislý v OM propojení v přepínačem virtuální je to mechanismus používaný DRBD replikovat zařízení
