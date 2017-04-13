<properties
    pageTitle="Připojit Linux počítače k protokolu analýzy | Microsoft Azure"
    description="Použití protokolu analýzy, můžete shromažďovat a chovalo na informace shromážděné z počítače Linux."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="connect-linux-computers-to-log-analytics"></a>Připojení k protokolu analýzy Linux počítačů

Použití protokolu analýzy, můžete shromažďovat a chovalo na informace shromážděné z počítače Linux. Přidání dat shromážděných z Linux OMS umožňuje spravovat Linux systémy a řešení kontejneru třeba Docker bez ohledu na to, kde se nacházejí počítače – prakticky odkudkoli. Ano těmto zdrojům dat mohou být umístěné v místním datacentra jako fyzických serverů, virtuálních počítačích v hostované cloudové služby, jako je Amazon webové služby AWS nebo Microsoft Azure nebo dokonce přenosný počítač na svůj pracovní stůl. Kromě toho OMS také shromažďuje data z počítače se systémem Windows podobně tak podporuje hybridním opravdu prostředí.

Můžete zobrazit a spravovat data ze všech těchto zdrojů pomocí protokolu analýzy v OMS pomocí portálu Správa jednoho. Sníží potřeba ho z mnoha různých systémech umožňuje snadno používat a můžete exportovat data, která chcete do jakéhokoliv obchodního řešení analýzy nebo systému, který už máte sledovat.

Tento článek je rychlého spusťte průvodce, který vám pomůže shromažďovat a správa dat pro počítače Linux pomocí agenta OMS Linux. Další technické podrobnosti například konfigurací proxy server, informace o CollectD metriky a vlastní zdroje dat JSON najdete tyto informace na [OMS Agent Linux přehledem](https://github.com/Microsoft/OMS-Agent-for-Linux) a [OMS Agent úplné dokumentaci Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Github.


V současné době získáte následující typy dat na Linux počítače:

- Měřítka
- Syslog události
- Upozornění od Nagios a Zabbix
- Docker kontejneru měřítka, zásob a protokolů

## <a name="supported-linux-versions"></a>Podporované Linux verze

Verze x86 a x64 úředně podporuje na různých distribuce Linux. Agent OMS Linux mohou také spustit na jiných distribuce není uvedená.

- Amazon Linux 2012.09 prostřednictvím 2015.09
- CentOS Linux 5, 6 a 7
- Oracle Linux 5, 6 a 7
- Červené klobouk Enterprise Linux Server 5, 6 a 7
- Debian GNU/Linux 6, 7 a 8
- Systémem Ubuntu 12.04 l, 14.04 l 15.04, 15.10
- SUSE Linux Enterprise Server 11 a 12

## <a name="oms-agent-for-linux"></a>Agent OMS Linux
Agent sadu správy operace Linux zahrnuje více balíčků. Vydaná verze souboru obsahuje následující balíčky dostupné spuštěním příruček prostředí s `--extract`.

**Balíčku** | **Verze** | **Popis**
----------- | ----------- | --------------
omsagent | 1.1.0 | Agent sadu správy operace Linux
omsconfig | 1.1.1 | Konfigurace agent OMS agenta
(OMI) | 1.0.8.3 | Otevřít správu infrastruktury (režimu OMI) – lightweight CIM serveru
scx | 1.6.2 | Poskytovatelé CIM (OMI) pro operační systém měřítka
Apache cimprov | 1.0.0 | Apache HTTP Server výkonu poskytovatele (OMI). Pouze pokud je nainstalovaný nerozpoznal Apache HTTP serveru.
MySQL cimprov | 1.0.0 | Sledování poskytovatele režimu OMI MySQL Server výkonu. Pouze pokud je nainstalovaný zjištěn server MySQL/MariaDB.
docker cimprov | 0.1.0 | Zprostředkovatel docker pro (OMI). Nainstalovat pouze pokud je zjištěno Docker.

### <a name="additional-installation-artifacts"></a>Další instalace artefakty
Po instalaci agenta OMS Linux balíčků, použijí se tyto změny další systémové konfiguraci. Při odinstalaci balíček omsagent budou odebrány tyto artefakty.
- Uživatel bez oprávnění s názvem: `omsagent` se vytvoří. Spuštění démona omsagent jako účet
- Vytvoření souboru "zahrnout" sudoers na /etc/sudoers.d/omsagent to povoluje omsagent restartujte procesy daemon syslog a omsagent. Pokud sudo "zahrnout" směrnice nejsou podporovány v nainstalované verzi sudo, tyto položky bude mít došlo k zápisu /etc/sudoers.
- Konfigurace syslog je upraven předat agent podmnožinu události. Další informace najdete v tématu **Konfigurace shromažďování dat** v části

### <a name="linux-data-collection-details"></a>Linux podrobnosti shromažďování dat

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se shromažďují data.

| zdroje | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Zabbix|![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|1 minuta|
|Nagios|![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|Při příchodu|
|Syslog|![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|z Azure úložiště: 10 minut; z agenta: při příchodu|
|Linux výkonnosti|![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|Podle plánu, minimální 10 sekund|
|sledování změn|![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|každou hodinu|



### <a name="package-requirements"></a>Požadavky na balíček
| **Povinný balíčku**  | **Popis**   | **Minimální verze**|
|--------------------- | --------------------- | -------------------|
|Glibc |    Knihovna GNU C   | 2,5 12|
|OpenSSL    | OpenSSL knihoven | 0.9.8E nebo 1.0|
|Otočení | Otočení webový klient | 7.15.5
|Python ctypes |Funkce knihovny | není k dispozici|
|PAM | Připojitelné ověřování modulů  |není k dispozici |

>[AZURE.NOTE] Rsyslog nebo syslog ng nutné shromáždit syslog zprávy. Démona syslog výchozí verze 5 verze Enterprise Linux klobouk červené, CentOS a Oracle Linux (sysklog) není podporované pro shromažďování syslog událostí. Sběr dat syslog v této verzi tyto distribuce, by měl démona rsyslog nainstalovali a nakonfigurovali nahrazení sysklog.

## <a name="quick-install"></a>Představení instalace

Následující příkazy pro stažení omsagent, ověřte kontrolní součet a potom nainstalovat a integrovaný agent. Příkazy jsou pro 64bitovou verzi. Pracovní prostor ID a primární klíč se nacházejí v portálu OMS klikněte v části **Nastavení** na kartě **Připojení zdroje** .

![Podrobnosti o pracovního prostoru](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.1.0-28/omsagent-1.1.0-28.universal.x64.sh
sha256sum ./omsagent-1.1.0-28.universal.x64.sh
sudo sh ./omsagent-1.1.0-28.universal.x64.sh --upgrade -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Existují různé jiné metody nainstalovat agent a upgrade ho. Další informace o jejich na [pokynů k instalaci agenta OMS Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Můžete taky zobrazit [Azure videa návodu](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>Volba způsobu Linux dat kolekce

Jak se rozhodnete typy dat, které chcete shromáždit závisí na zda chcete používat portál OMS nebo pokud chcete upravovat různých konfigurace soubory přímo v klientech Linux. Pokud se rozhodnete sdělit nám portálu, konfiguraci odeslaný do všechny svoje klienty Linux automaticky. Pokud budete potřebovat různé konfigurace pro různé Linux klienty, musíte jednotlivě – upravit soubory klienta nebo pomocí alternativy jako DSC Powershellu, Chef nebo Puppet.

Můžete určit syslog událostí a výkonnosti, které chcete získat informace o používání souborů konfigurace počítačů Linux. *Pokud jste se rozhodli konfigurace shromažďování dat úpravou agent konfigurační soubory, měli byste zakázat centralizované konfigurace.*  Pokyny jsou uvedeny níže konfigurace shromažďování dat v souborech konfigurace agenta i zakázání centrální konfigurace pro všechny OMS agentů Linux nebo jednotlivé počítače.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Zakázání OMS správy pro jednotlivé počítače Linux

Centralizované shromažďování dat pro data konfigurace není k dispozici pro jednotlivé počítače Linux se skriptem OMS_MetaConfigHelper.py. To může být užitečné, když podmnožinu počítačů by měla být specializované konfigurace.

Jak zakázat centralizované konfigurace:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Pokud chcete povolit centralizované konfigurace:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Linux výkonnosti

Linux výkonnosti se podobají výkonnosti Windows – jak pracovat podobně. Přidat a nakonfigurovat je můžete použít následující postupy. Po přidání do OMS se údaje pro ně každých 30 sekund.

### <a name="to-add-a-linux-performance-counter-in-oms"></a>Chcete-li přidat čítače výkonu Linux v OMS

1. Konfigurace OMS agentů Linux pomocí portálu OMS, můžete přidat Linux výkonnosti na stránce nastavení, klikněte na **Data**.  
2. Na stránce **Nastavení** v části **dat** klikněte na **Linux výkonnosti** a vyberte nebo zadejte název čítač, který chcete přidat.  
    ![data](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Pokud neznáte celé jméno čítači, můžete začít psát část jména a zobrazí se seznam dostupných čítačů. Jakmile hledaný čítač, který chcete přidat, klikněte na název v seznamu a potom klikněte na ikonu se znaménkem plus přidejte čítač.
4. Po přidání čítači se zobrazí v seznamu čítače zvýrazněny barevného pruhu.
5. Ve výchozím nastavení je vybraná možnost **použít pod konfigurace Moje počítačích** . Pokud chcete zakázat odesílání dat konfigurace, zrušte výběr.
6. Po dokončení výkonnosti změny, klikněte v dolní části stránky na dokončení změny **Uložit** . Změny konfigurace, které jste provedli potom posílají všechny agentů OMS Linux, které jsou registrované s OMS, obvykle do 5 minut.

### <a name="configure-linux-performance-counters-in-oms"></a>Konfigurace Linux výkonnosti v OMS

Pro Windows výkonnosti můžete vybrat konkrétní instanci pro každou čítače. Však pro Linux výkonnosti, jakéhokoliv výskyt čítač, který se rozhodnete se týká peaks podřízené čítače nadřazené. Následující tabulka zobrazuje běžné případy k dispozici pro výkonnosti Linux a Windows.

| **Název instance** | **Význam** |
| --- | --- |
| \_Součet | Součet všech instancí |
| \* | Všechny instance |
| (/ & #124; / var) | Shoduje s názvem instance: / nebo /var |


Pozastaveno, které zvolíte nadřazené čítače Podobně platí pro všechny její podřízené čítače. Jinými slovy je všechny podřízené čítač ukázkové intervalů a instance stejným společně.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Přidat a nakonfigurovat výkonu metriky s Linux

Měřítka shromažďovat jsou řízeny konfigurací v /etc/opt/microsoft/omsagent/conf/omsagent.conf. Naleznete v tématu [dostupné měřítka](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) dostupných tříd a metriky agenta OMS Linux.

Každý objekt nebo kategorie měřítka shromažďovat stanovit v souboru konfigurace jako jediný `<source>` prvek. Syntaxe následující dole.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Konfigurovatelné parametry: Tento element je:

- **Objekt\_název**: název objektu kolekce.
- **Instance\_regex**: *regulárních výrazů* , které instance získat informace o definování. Hodnota: `.*` Určuje všechny instance. Shromažďování metriky procesor pro jenom \_celkové instance, mohli byste určit, `_Total`. Získat informace o metriky procesů pro pouze crond nebo sshd instance, mohli byste určit: `(crond|sshd)`.
- **Čítač\_název\_regex**: *regulárních výrazů* definující, které čítačů (objekt) shromažďovat. Získat informace o peaks objektu, zadejte: `.*`. Získat informace o pouze vyměňovat místo čítače paměti objektu, můžete napsat:`.+Swap.+`
- **Intervalu:**: četnost kdy byly shromážděny čítače objektu.

Výchozí konfigurace měřítka je:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Povolení MySQL výkonnosti pomocí příkazů Linux

Pokud MySQL Server nebo MariaDB Server je zjištěno v počítači nainstalovaná příruček omsagent, je automaticky instalován sledování poskytovatele pro MySQL Server výkonu. Zprostředkovatel se připojí k místní server MySQL/MariaDB vystavit údaje o výkonu. Budete muset nastavit přihlašovací údaje uživatele pro MySQL, takže zprostředkovatele dostanete MySQL Server.

Výchozí uživatelský účet pro MySQL server na localhost, zadejte následující příkaz například.

>[AZURE.NOTE] Musí být účet omsagent soubor přihlašovací údaje. Spuštění příkazu mycimprovauth jako omsgent se doporučuje.


```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'

sudo service omiserverd restart
```


Můžete taky určit požadovaných pověření MySQL v souboru, tak, že vytvoříte soubor: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Další informace o správě MySQL přihlašovací údaje pro sledování prostřednictvím mysql ověření souboru najdete v článku [Správa MySQL sledování pověření v souboru ověřování](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Podrobné informace o oprávněních objekt muset uživatelem MySQL shromažďovat data o výkonu MySQL Server najdete v článku [oprávnění k databázi potřebných pro MySQL výkonnosti](#database-permissions-required-for-mysql-performance-counters) .

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Povolení výkonnosti Apache HTTP serveru pomocí příkazů Linux

Pokud Apache HTTP Server je zjištěno v počítači nainstalovaná příruček omsagent, je automaticky instalován sledování poskytovatele pro Apache HTTP Server výkonu. Zprostředkovatel závisí na Apache "moduly", které je nutné načíst do Apache HTTP Server pro přístup k data o výkonu.

Můžete načíst modul pomocí následujícího příkazu:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Odinstalace modulu Sledování Apache, spusťte tento příkaz:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="to-view-performance-data-with-log-analytics"></a>Chcete-li zobrazit data o výkonu s protokolu analýzy

1. Na portálu operace správy sady klikněte na dlaždici protokolu vyhledávání.
2. Do panelu hledání zadejte `* (Type=Perf)` zobrazíte všechny výkonnosti.


Protože OMS taky shromáždí data čítačů Windows, můžete má obor dolů hledání tak, aby data specifická Linux. Ano v následujícím příkladu by zobrazit data o výkonu specifické pro server Linux příkladu s názvem Chorizo21.

```
Type=Perf Computer=chorizo*
```

![Příklad serveru vidět ve výsledcích hledání](./media/log-analytics-linux-agents/oms-perfsearch01.png)

Ve výsledcích kliknete **metriky** zobrazíte čítačů, které údaje pro. Data v reálném čase je zobrazen jako grafy pro každý čítač.

![metriky](./media/log-analytics-linux-agents/oms-perfmetrics01.png)


## <a name="syslog"></a>Syslog

Syslog je podobný protokoly událostí Windows protokol protokolování událostí – obě podobně pracovat při zobrazení v OMS.

### <a name="to-add-a-new-linux-syslog-facility-in-oms"></a>Chcete-li přidat nové zařízení syslog Linux v OMS

1. Na stránce **Nastavení** v části **dat** klikněte na **Syslog** a potom vlevo ikonu se znaménkem plus, zadejte název syslog zařízení, které chcete přidat.
    ![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2.  Pokud si nejste jisti celé jméno zařízení, můžete začít psát část jména a zobrazí se seznam dostupných syslog střediska. Jakmile hledaný syslog zařízení, které chcete přidat, klikněte na název v seznamu a potom klikněte na ikonu plus pro přidání místa konání syslog.
3.  Po přidání zařízení, se zobrazí v seznamu zvýrazněny barevného pruhu. Pak vyberte severities (kategoriemi syslog zařízení informací), která chcete shromažďovat.
4.  V dolní části stránky klikněte na **Uložit** do dokončení změny. Změny konfigurace, které jste provedli potom posílají všechny agentů OMS Linux, které jsou registrované s OMS, obvykle do 5 minut.


### <a name="configure-linux-syslog-facilities-in-linux"></a>Konfigurace Linux syslog zařízení v Linux

Události Syslog jsou odeslány z démona syslog, například rsyslog nebo syslog ng, místní port, který je agent přijímá na. Ve výchozím nastavení port 25224. Po instalaci agent výchozí konfigurace syslog se použije. Nachází se na:


Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog ng: /etc/syslog-ng/syslog-ng.conf


Konfigurace výchozího OMS agenta syslog odešle syslog události ze všech zařízeních s závažnosti upozornění nebo vyšší.

>[AZURE.NOTE] Pokud upravíte syslog konfigurace, je nutné restartovat démona syslog změny se projeví.

Výchozí syslog konfigurace agenta OMS Linux pro OMS je:

#### <a name="rsyslog"></a>Rsyslog

```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a>Syslog ng

```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="to-view-all-syslog-events-with-log-analytics"></a>Chcete-li zobrazit všechny události Syslog s protokolu analýzy

1. Na portálu operace správy sady klikněte na dlaždici **Protokolu vyhledávání** .
2. V **Protokolu správy** seskupení, klikněte na předdefinované syslog vyhledávání a pak vyberte jednu ho spusťte.

Tento příklad uvádí všechny Syslog události.

![Zobrazené v hledání protokolu událostí Syslog](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Teď můžete přejít do výsledků hledání.

## <a name="linux-alerts"></a>Linux upozornění

Pokud používáte ke správě počítače Linux Nagios nebo Zabbix, OMS dostávat upozornění generovaných těmito nástroje. Ale momentálně není metody konfigurace příchozích upozornění dat na portálu OMS. Místo toho bude potřebujete upravit konfiguračního souboru začít odesílat upozornění na OMS.



### <a name="collect-alerts-from-nagios"></a>Shromáždit upozornění od Nagios

Získat informace o upozornění ze serveru Nagios, musíte provést následující změny konfigurace.

1. Udělení přístupu k souboru protokolu Nagios (tedy /var/log/nagios/nagios.log/var/log/nagios/nagios.log) pro čtení **omsagent** uživateli. Za předpokladu, že soubor nagios.log vlastní skupiny **nagios** uživatele **omsagent** můžete přidat do skupiny **nagios** .

    ```
    sudo usermod –a -G nagios omsagent
    ```

2. Upravit soubor omsagent.confconfiguration (/ etc/opt/microsoft/omsagent/conf/omsagent.conf). Zajistěte, aby že se tyto záznamy prezentovat a ne se skrytými v komentářích:

    ```
    <source>
    type tail
    #Update path to point to your nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```

3. Restartujte démona omsagent:

    ```
    sudo service omsagent restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Shromáždit upozornění od Zabbix

Získat oznámení ze serveru Zabbix provedením kroků podobné těm pro výše uvedená Nagios s tím rozdílem, budete muset zadat uživatele a heslo v *prostý text*. Je ideální, ale bude pravděpodobně měnit brzy bude k dispozici. Tento problém vyřešit, doporučujeme vám vytvoření uživatele a udělit oprávnění ke sledování pouze.

Příklad část konfiguračního souboru omsagent.conf (/ etc/opt/microsoft/omsagent/conf/omsagent.conf) pro Zabbix by měl vypadat takto:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>Zobrazit upozornění v protokolu analýzy hledání

Po konfiguraci počítače Linux odešlete OMS upozornění můžete zobrazit upozornění na pár jednoduchých protokolu vyhledávacích dotazů. Následující příklad dotazu vyhledávání vrátí zaznamenané oznámení, které byly vytvořeny. Například v případě určitý druh problém v infrastrukturu pak výsledků pro následující dotaz může určují, kde může pocházet problém. A můžete snadno přejít pro upozornění na systémem zdroje pro zpřesnění vaší vyšetřování. Výhody je, že nemusíte nutně přejděte na různých systémy řízení od začátku – za předpokladu, že nastavení upozornění se odesílají OMS, se můžete pustit tam.

```
Type=Alert
```

#### <a name="to-view-all-nagios-alerts-with-log-analytics"></a>Chcete-li zobrazit všechna upozornění Nagios s protokolu analýzy
1. Na portálu operace správy sady klikněte na dlaždici **Protokolu vyhledávání** .
2. Do řádku dotazu zadejte následující vyhledávacího dotazu

    ```
    Type=Alert SourceSystem=Nagios
    ```
![Upozornění Nagios zobrazené v hledání protokolu](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Až uvidíte výsledky hledání, můžete přejít na další podrobnosti, například *AlertState*.

### <a name="to-view-all-zabbix-alerts-with-log-analytics"></a>Chcete-li zobrazit všechna upozornění Zabbix s protokolu analýzy
1. Na portálu operace správy sady klikněte na dlaždici **Protokolu vyhledávání** .
2. Do řádku dotazu zadejte následující vyhledávacího dotazu

    ```
    Type=Alert SourceSystem=Zabbix
    ```
![Upozornění Zabbix zobrazené v hledání protokolu](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Až uvidíte výsledky hledání, můžete přejít na další podrobnosti, například *AlertName*.


## <a name="compatibility-with-system-center-operations-manager"></a>Funkce pro kompatibilitu s System Center Operations Manager

Agent OMS Linux sdílí agent binární s agentem System Center Operations Manager. Instalace agenta OMS Linux v systému aktuálně spravuje Operations Manager aktualizuje balíčků (OMI) a SCX v počítači na novější verzi. Agent OMS Linux a System Center 2012 R2 jsou kompatibilní. Však **System Center 2012 SP1 a dřívějších verzích jsou momentálně není kompatibilní nebo podporované s agentem OMS Linux.**

>[AZURE.NOTE] Pokud agenta OMS Linux nainstalovaný na počítači, který není aktuálně spravuje Operations Manager a chcete později spravovat počítač s Operations Manager, je třeba změnit konfigurace režimu OMI před objevit v počítači. **Tento krok není potřeba nainstalovaného agenta Operations Manager je před agenta OMS Linux.**

### <a name="to-enable-the-oms-agent-for-linux-to-communicate-with-operations-manager"></a>Chcete-li povolit agenta OMS Linux komunikovat s Operations Manager

1. Úprava /etc/opt/omi/conf/omiserver.conf souboru
2. Zkontrolujte, že je řádek začínající **httpsport =** definuje port 1270. Jako`httpsport=1270`
3. Restartujte server (OMI):

    ```
    service omiserver restart or systemctl restart omiserver
    ```




## <a name="database-permissions-required-for-mysql-performance-counters"></a>Databáze informace o oprávněních požadovaných pro výkonnosti MySQL

Pokud chcete udělit oprávnění pro uživatele sledování MySQL, poskytnutí uživatel musí mít oprávnění UDĚLTE možnost, jakož i udělení oprávnění.

Aby mohl uživatel MySQL vrátit data o výkonu uživatele, musí přístup k následujícím dotazů:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Kromě tyto dotazy MySQL vyžaduje uživatele vyberte přístup k v následujících tabulkách výchozí:

- zobrazení INFORMATION_SCHEMA
- MySQL

Tato oprávnění lze přidělit spuštěním následující příkazy grant (udělit).

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-the-authentication-file"></a>Správa MySQL sledování přihlašovacích údajů v souboru ověřování

Následující části vám pomohou spravovat pověření MySQL.

### <a name="configure-the-mysql-omi-provider"></a>Konfigurace režimu MySQL OMI poskytovatele

Zprostředkovatele režimu OMI MySQL vyžaduje předkonfigurované MySQL uživatele a nainstalovanou dotazu výkonu/stavu informace z MySQL instance knihoven klienta MySQL.

### <a name="mysql-omi-authentication-file"></a>Režimu MySQL OMI ověření souboru

Zprostředkovatele režimu MySQL OMI používá soubor ověřování a zjistit, jakou adresu vazbu port MySQL instance listening na a co pověření umožňuje shromáždit metriky. Během instalace režimu OMI MySQL poskytovatele vyhledá MySQL my.cnf konfigurace souborů (výchozí umístění) pro vazbu adresa a port a částečně nastavení režimu OMI MySQL ověření souboru.

K provedení sledování instance server MySQL, přidejte do předem vygenerovaných režimu MySQL OMI ověřování soubor ve správném adresáři.

### <a name="authentication-file-format"></a>Ověření formátu

Textový soubor, který obsahuje informace o je soubor ověřování (OMI MySQL):

- Port
- Vazba adresu
- Uživatelské jméno MySQL
- Zakódovaný ve formátu Base 64 heslo

Soubor režimu MySQL OMI ověřování pouze uděluje oprávnění pro čtení i zápis Linux uživatele, které vygenerovalo ho.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Výchozí režimu MySQL OMI ověření souboru obsahuje výchozí instanci a číslo portu podle toho, jaké informace jsou k dispozici a analyzované z konfiguračního souboru nalezených MySQL.

Výchozí instanci prostředkem k Usnadněte si správu několika instancích spuštěných MySQL na jednoho hostitele Linux jednodušší a je označený instancí s portem 0. Všechny další instance zdědí v výchozí instanci můžete nastavit vlastnosti. Třeba když je přidán MySQL instance přijímá požadavky na port "3308", vazbu adresa, uživatelské jméno a heslo ve formátu Base 64 zakódovaný výchozí instanci se použijí k akci a sledovat instance listening na 3308. Když instance na 3308 binded na jinou adresu a používá stejnou MySQL uživatelské jméno a heslo dvojici není potřeba pouze respecification adresu vazby a bude dědí vlastnosti.

Příklady ověřovací soubor vypadat takto.

Výchozí instanci a instance s portem 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Výchozí instanci a instance s portem 3308 + různých Base 64 zakódovaný heslo:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Vlastnost** | **Popis** |
| --- | --- |
| Port | Port představuje aktuální port, který MySQL instance listening na.  Port 0 znamená, že následující vlastnosti se používají pro výchozí instanci. |
| Vazba adresu | Svázat adresa je aktuální MySQL vazbu- |
| uživatelské jméno | Tento uživatelské jméno uživatele MySQL chcete použít ke sledování instanci server MySQL. |
| Kódováním Base 64 heslo | To je heslo uživatele sledování MySQL zakódovaný v ve formátu Base 64. |
| AutoUpdate | Při upgradu na poskytovatele režimu OMI MySQL zprostředkovatel prohledat změny v souboru my.cnf a soubor MySQL režimu OMI ověřování přepsat. Nastavte tento příznak na hodnotu true nebo false v závislosti na aktualizace nutné k souboru ověřování (OMI MySQL). |

#### <a name="authentication-file-location"></a>Umístění souboru ověřování

Soubor MySQL režimu OMI ověřování by měl umístěn v následujícím umístění a s názvem "mysql auth":

/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth

Soubor (a auth/omsagent adresář) musí být ve vlastnictví omsagent uživatele.

## <a name="agent-logs"></a>Protokoly agenta

Protokoly pro agenta OMS Linux je:

/ var a určovat, jestli / / omsagent/protokolu microsoft /

Protokoly pro agenta OMS Linux programu omsconfig (Konfigurace agenta) je:

/ var a určovat, jestli / / omsconfig/protokolu microsoft /

Protokoly pro (OMI) a SCX (které poskytují data o výkonu metriky) je:

/ var a určovat, jestli/režimu omi/protokol/a /var/opt/microsoft/scx/log

## <a name="troubleshooting-the-oms-agent-for-linux"></a>Poradce při potížích s agenta OMS Linux

Použijte tyto informace diagnostikovat a řešení běžných problémů.

Pokud žádné informace o řešení potíží v této části umožňuje, můžete také v následujících zdrojích vyřešit váš problém.

- Zákazníkům s Prémiová podpora protokolu případ podpory prostřednictvím [Premier](https://premier.microsoft.com/)
- Zákazníkům s Azure podpory smlouvami můžete Přihlaste se podpora případech [Azure portálu](https://manage.windowsazure.com/?getsupport=true)
- Soubor [GitHub problém](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
- Fórum pro názory pro nápady a k vytváření chybu vykazování [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Umístění důležité protokolu

Soubor | Cesta
---- | -----
Agent OMS souboru protokolu Linux | `/var/opt/microsoft/omsagent/log/omsagent.log `
Soubor protokolu konfigurace OMS Agent | `/var/opt/microsoft/omsconfig/omsconfig.log`

### <a name="important-configuration-files"></a>Důležité konfigurační soubory

Catergory | Umístění souboru
----- | -----
Syslog | `/etc/syslog-ng/syslog-ng.conf`nebo `/etc/rsyslog.conf` nebo`/etc/rsyslog.d/95-omsagent.conf`
Výkon, Nagios, Zabbix OMS výstup a obecné agent | `/etc/opt/microsoft/omsagent/conf/omsagent.conf`
Další konfigurace | `/etc/opt/microsoft/omsagent/conf.d/*.conf`

>[AZURE.NOTE] Pokud je povoleno nastavení portálu OMS jsou přepsání úpravy souborů konfigurace výkonnosti a syslog. Zakázání konfigurace na portálu OMS (pro všechny uzly) nebo jednotlivých uzlů spuštěním následujícího:

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Povolení protokolování ladění

Chcete-li povolit protokolování ladění, můžete OMS výstup modul plug-in a podrobný výstup.

#### <a name="oms-output-plugin"></a>Modul plug-in OMS výstup

FluentD umožňuje modul plug-in k určení úrovně protokolování pro jiný protokol úrovně pro vstupů a výstupů. Můžete určit úroveň jiný protokol pro výstup OMS, upravit konfigurace obecné agenta v `/etc/opt/microsoft/omsagent/conf/omsagent.conf` soubor.

V dolní části konfiguračního souboru, změna `log_level` vlastnost z `info` k `debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Protokolování ladění vám umožní zobrazit dávkový nahrávání ke službě OMS odděleni typ počtu položek dat a doba k odeslání.

*Příklad ladění podporou protokolu:*
```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Podrobný výstup
Místo použití modulu plug-in výstup OMS, můžete taky exportujete data položky přímo do `stdout`, který se zobrazí v OMS Agent pro soubor protokolu Linux.

V souboru konfigurace obecné agent OMS na `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, komentáře a rezervovat OMS výstup modul plug-in přidáním `#` před každý řádek.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Pod modul plug-in výstup odebrat komentář v následující části odebráním `#` symbol na začátku každého řádku.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-the-log"></a>Předávání zprávy dál Syslog se nezobrazují v protokolu

#### <a name="probable-causes"></a>Možné příčiny

- Konfigurace serveru Linux použita neumožňuje kolekce odeslané zařízení a/nebo úrovně protokolu
- Syslog není předávané správně Linux server
- Řada zprávy předávané sekundu je příliš velký pro základní konfigurace agenta OMS Linux pro zpracování

#### <a name="resolutions"></a>Řešení

- Zkontrolujte, že konfiguraci na portálu OMS Syslog všechny zařízení a úrovní správné protokolu
  - **Portál OMS > Nastavení > Data > Syslog**
-  Ověřte, že nativní syslog zpráv procesy daemon (`rsyslog`, `syslog-ng`) budou moct přijímat zprávy předané dál
- Zkontrolujte nastavení brány firewall na serveru Syslog ověřit, zda nejsou blokovány zprávy
-  Simulovat Syslog zprávu pomocí OMS `logger` command – například:
  - `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-to-oms-when-using-a-proxy"></a>Problémy s připojením k OMS při použití proxy serveru

#### <a name="probable-causes"></a>Možné příčiny

- Proxy server zadat, když je nesprávné instalace a konfigurace agenta
- Koncové body OMS služby nejsou whitelistested ve vaší datacentru

#### <a name="resolutions"></a>Řešení

- Přeinstalace agenta OMS Linux pomocí následujícího příkazu s možností `-v` povolené. Díky podrobný výstup agent připojení prostřednictvím proxy ke službě OMS.
  - `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  - V dokumentaci ke [konfiguraci agent pro použití s serveru proxy HTTP](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server) proxy OMS
- Ověřte, jestli jsou následující koncové body OMS služby povolené

Agent zdroje | Porty
---- | ----
& #42;. Ods.opinsights.Azure.com | Port 443
& #42;. OMS.opinsights.Azure.com | Port 443
Ods.systemcenteradvisor.com | Port 443
& #42;.blob.core.windows.net/ | Port 443

### <a name="a-403-error-is-displayed-when-onboarding"></a>403 chyba se zobrazí při rychlého připojení

#### <a name="probable-causes"></a>Možné příčiny

- Data a času nejsou správné na Linux Server
- Pracovní prostor ID a pracovní prostor klíče nejsou správné

#### <a name="resolution"></a>Rozlišení

- Ověřte čas na serveru Linux s `date` příkaz. Pokud data je větší nebo menší než 15 minut od aktuálního času, rychlého připojení se nezdaří. Tento problém můžete vyřešit, aktualizujte datum nebo časové pásmo serveru Linux.
- Nejnovější verzi agenta OMS Linux upozorní časový rozdíl je příčinou selhání rychlého připojení
- Nové integrovaný pomocí správného pracovního prostoru ID a klíč pracovního prostoru. Další informace najdete v tématu [rychlého připojení pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .

### <a name="a-500-error-or-404-error-appears-in-the-log-file-after-onboarding"></a>500 Chyba nebo Chyba 404 se zobrazí v souboru protokolu za rychlého připojení

Toto je známý problém, ke kterým dochází při prvním uložení dat Linux do pracovního prostoru OMS. To neovlivní odesílaného dat nebo jiných problémů. Můžete ignorovat chyby při původně rychlého připojení.

### <a name="nagios-data-does-not-appear-in-the-oms-portal"></a>Nagios dat na portálu OMS nezobrazí.

#### <a name="probable-causes"></a>Možné příčiny
- Omsagent uživatel nemá oprávnění ke čtení ze souboru protokolu Nagios
- Oddíly Nagios zdroje a filtr se pořád změněna na komentář v souboru omsagent.conf

#### <a name="resolutions"></a>Řešení

- Přidání uživatele omsagent pro čtení souborů Nagios. Další informace najdete v tématu [Nagios upozornění](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) .
- Ve službě OMS Agent pro Linux obecné konfiguračního souboru v `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, zajistit, aby tento **jak** zdroji Nagios a filtr oddíly mít komentáře odebírat, podobně jako v následujícím příkladu.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-the-oms-portal"></a>Na portálu OMS nezobrazí Linux data

#### <a name="probable-causes"></a>Možné příčiny

- Rychlého připojení ke službě OMS se nezdařila.
- Připojení ke službě OMS blokované
- Agent OMS Linux dat je zálohované

#### <a name="resolutions"></a>Řešení

- Ověření tohoto rychlého připojení ke službě OMS byl úspěšný ověřením, která `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` existuje.
- Nové integrovaný pomocí příkazového řádku omsadmin.sh. Další informace najdete v tématu [rychlého připojení pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- Pokud používáte proxy server, použijte proxy Poradce při potížích s výše uvedené kroky
- V některých případech při agenta OMS Linux nemůžete komunikovat ani ke službě OMS údaje o Agent je zálohované velikosti celé rezervy 50 MB. Restartujte agenta OMS Linux spuštěním buď `service omsagent restart` nebo `systemctl restart omsagent` příkazy.
  >[AZURE.NOTE] Tento problém bude pevně Agent verze 1.1.0-28 nebo novější.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-the-oms-portal"></a>Konfigurace čítač výkonu Syslog Linux se nepoužil na portálu OMS

#### <a name="probable-causes"></a>Možné příčiny

- Konfigurace agent agenta OMS Linux nebyla načtená nejnovější konfigurace z portálu OMS.
- Nebyly použity upravené nastavení na portálu

#### <a name="resolutions"></a>Řešení

`omsconfig`je agent konfigurace agenta OMS Linux, která načte změny konfigurace portálu OMS každých 5 minut. Tuto konfiguraci se použije agenta OMS pro soubory konfigurace Linux c:\ `/etc/opt/microsoft/omsagent/conf/omsagent.conf`.

- V některých případech agenta OMS Linux konfigurace agenta nebudete moci komunikovat s službu portálu konfigurace výsledkem nejnovější konfigurace není použitý.
- Ověřte, že `omsconfig` agent nainstalovaný následující:
  - `dpkg --list omsconfig`nebo`rpm -qi omsconfig`
  - Pokud není nainstalovaná znovu nainstalujte nejnovější verzi agenta OMS Linux

- Ověřte, že `omsconfig` agent mohli komunikovat s OMS služby
  - Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
    - Příkaz výše vrátí konfigurace této agent načte z portálu Microsoft, včetně nastavení Syslog, Linux výkonnosti a vlastní protokoly
    - Pokud příkaz výše nepovede, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz vynutí agenta omsconfig komunikovat s službu OMS k načtení nejnovější konfigurace.


### <a name="custom-linux-log-data-does-not-appear-in-the-oms-portal"></a>Vlastní data protokolu Linux nezobrazující v portálu OMS

#### <a name="probable-causes"></a>Možné příčiny

- Rychlého připojení ke službě OMS se nezdařila.
- Nastavení **použít následující konfigurace servery Linux** není zaškrtnuto políčko
- omsconfig nebyla převzít protokol nejnovější vlastní z portálu
- `omsagent` Používá nelze získat přístup k protokolu vlastní kvůli potížím oprávnění nebo `omsagent` nebyl nalezen. V tomto případě se zobrazí následující výstup:
  - `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  - `[DATETIME] [error]: file not accessible by omsagent.`
- Toto je známý problém, který byl pevně v OMS Agent pro Linux verze 1.1.0-217 časování

#### <a name="resolutions"></a>Řešení
- Ověřte, že jste úspěšně onboarded, tak, že zjištění, jestli `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` soubor existuje.
  - Pokud potřebné, integrovaný znovu pomocí příkazového řádku omsadmin.sh. Další informace najdete v tématu [rychlého připojení pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- Na portálu OMS klikněte v části **Nastavení** na kartě **Data** zajistit, že je zaškrtnuté toto nastavení **použít následující konfiguraci servery Linux**  
  ![použití konfigurace](./media/log-analytics-linux-agents/customloglinuxenabled.png)

- Ověřte, že `omsconfig` agent mohli komunikovat s OMS služby
  - Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
  - Příkaz výše vrátí konfigurace této agent načte z portálu Microsoft, včetně nastavení Syslog, Linux výkonnosti a vlastní protokoly
  - Pokud příkaz výše nepovede, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz vynutí agenta omsconfig ke komunikaci s OMS služby a získat nejnovější konfigurace.


Místo OMS Agent pro uživatele Linux spuštěný jako privilegovaných uživatelů `root`, agenta OMS Linux běží jako `omsagent` uživatele. Ve většině případů explicitní musí být povoleno uživateli za účelem čtení některé soubory.

Chcete udělit oprávnění k `omsagent` uživatele, spusťte následující příkazy:

1. Přidejte `omsagent` uživatele do určité skupiny s`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Udělte univerzální přístup ke čtení požadovaný soubor s`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Je známý problém časování, který byl pevně v OMS Agent pro 1.1.0-217 verze Linux. Po aktualizaci na nejnovější agent, spusťte tento příkaz nejnovější verzi modulu plug-in výstup:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/conf/omsagent.conf
```

## <a name="known-limitations"></a>Známé omezení
Přečtěte si následující oddíly se naučit používat aktuální omezení agenta OMS Linux.

### <a name="azure-diagnostics"></a>Azure diagnostiky

Pro Linux virtuálních počítačích spuštěné v Azure dalších kroků může být nutné povolit shromažďování dat Azure Diagnostika a operace správy sadu. **Verze 2.2** koncovku diagnostiky Linux je nutný pro funkce pro kompatibilitu s agentem OMS Linux.

Další informace o instalaci a konfiguraci diagnostiky rozšíření Linux najdete v článku [použití příkazu rozhraní příkazového řádku Azure povolit diagnostické rozšíření Linux](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Upgrade koncovku Linux diagnostiky z 2.0 2.2 ASM Azure rozhraní příkazového řádku:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Tyto příklady příkaz odkaz do souboru nazvaného PrivateConfig.json. Formát souboru by měla vypadat podobně jako v následujícím příkladu.

```
    {
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog nepodporuje

Rsyslog nebo syslog ng nutné shromáždit syslog zprávy. Démona syslog výchozí verze 5 verze Enterprise Linux klobouk červené, CentOS a Oracle Linux (sysklog) není podporované pro shromažďování syslog událostí. Sběr dat syslog v této verzi tyto distribuce, by měl démona rsyslog nainstalovali a nakonfigurovali nahrazení sysklog. Další informace o nahrazení sysklog rsyslog najdete v tématu [instalace nově vytvořené rsyslog ot](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Další kroky

- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- Seznámení s [protokolu hledání](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
- Uložení a zobrazení vlastní hledání pomocí [řídicích panelů](log-analytics-dashboards.md) .
