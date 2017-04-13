<properties
   pageTitle="Konfigurace aplikace závislost monitoru (ADM) sady správy operace (OMS) | Microsoft Azure"
   description="Aplikace závislost monitoru (ADM) je operace správy sady (OMS) řešení, které automaticky zjistí součástí aplikace v systémech Windows a Linux a mapy komunikace mezi službami.  Tento článek obsahuje podrobnosti o nasazení ADM ve vašem prostředí a práce v různých scénářích."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Konfigurace aplikace závislost Monitor řešení v sadě správy operace (OMS)
![Ikona upozornění správy](media/operations-management-suite-application-dependency-monitor/icon.png) Aplikace závislost monitoru (ADM) automaticky zjistí součástí aplikace v systémech Windows a Linux a mapy komunikace mezi službami. Umožňuje zobrazit serverech tak, jak by podle vás z nich – jako propojené systémy splnit kritické služby.  Sledování závislost aplikace zobrazí připojení mezi servery, procesy, a porty přes všechny připojení TCP architektura s žádnou konfiguraci vyžadované jedině instalace agent.

Tento článek popisuje podrobnosti o konfiguraci aplikace závislost monitoru i rychlého připojení agentů.  Informace o použití ADM najdete v tématu [sledování aplikací závislost řešení v sadě správy operace (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Sledování závislost aplikace momentálně v soukromé náhledu.  Můžete požádat o přístup k soukromé náhled ADM v [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Během soukromé zobrazení náhledu, máte všechny účty OMS neomezený přístup k ADM.  Uzly ADM jsou zadarmo, ale protokolu analýzy dat pro typy AdmComputer_CL a AdmProcess_CL bude podle objemu dat jako jakékoli jiné řešení.
>
>Po ADM zadá veřejné náhled, bude k dispozici pouze zákazníkům bezplatných i placených přehled & analýzy v OMS ceny plánu.  Bezplatné osy účty budou omezeny na 5 ADM uzlů.  Pokud se účastníte soukromé náhled a nejsou zaregistrované v OMS ceny plánu po ADM vložení do veřejné náhled, přestane ADM v té době. 



## <a name="connected-sources"></a>Propojených zdrojů
Následující tabulka popisuje připojení zdroje, které jsou podporovány ADM řešení.

| Připojení zdroje | Podporované | Popis |
|:--|:--|:--|
| [Windows agentů](../log-analytics/log-analytics-windows-agents.md) | Ano | ADM analyzuje a shromažďuje údaje o Windows agent počítačů.  <br><br>Poznámka: kromě agenta OMS Windows agentů požadování Microsoft Agent závislost.  Přečtěte si téma [podporované operační systémy](#supported-operating-systems) úplný seznam verzí operačního systému. |
| [Linux agentů](../log-analytics/log-analytics-linux-agents.md) | Ano | ADM analyzuje a shromažďuje údaje o počítačů agent Linux.  <br><br>Poznámka: kromě agenta OMS Linux agentů požadování Microsoft Agent závislost.  Přečtěte si téma [podporované operační systémy](#supported-operating-systems) úplný seznam verzí operačního systému. |
| [Skupina Správa SCOM](../log-analytics/log-analytics-om-agents.md) | Ano | ADM analyzuje a shromáždí data z Windows a Linux agentů ve skupině Správa připojení SCOM. <br><br>Přímé připojení z počítače agent SCOM k OMS požaduje. Data jsou odeslány přímo z přepošlou ze skupiny správy OMS úložiště|
| [Účet Azure úložiště](../log-analytics/log-analytics-azure-storage.md) | Ne | ADM shromažďuje údaje o agent počítačů, tedy žádná data z něho shromažďovat z Azure úložiště. |

Všimněte si, že ADM podporuje pouze 64bitová verze platformy.

Ve Windows, Microsoft sledování Agent (MMA) používá SCOM a OMS shromáždění a odeslat monitorování dat.  (Tento agent se nazývá SCOM Agent, OMS Agent, MMA nebo přímé Agent, v závislosti na kontextu.)  SCOM a OMS poskytují různé mimo pole verzích MMA, ale tyto verze lze každou zprávu do SCOM OMS nebo obojí.  Na Linux, Agent OMS Linux shromažďuje a odešle sledování data, která chcete OMS.  Můžete ADM na serverech s přímé agentů OMS nebo na serverech, které jsou připojené k OMS pomocí SCOM Správa skupin.  Pro účely tohoto si přečtěte následující dokumentaci, budeme odkazovat na všechny agenti – podle Linux nebo Windows, zda připojen k SCOM g nebo přímo do OMS – jako "OMS agenta", pokud názvu konkrétními Agent není potřeba kontextu.

ADM agent neodesílal samotná data a nevyžaduje změn brány firewall nebo porty.  Na ADM data vždy přenášena agentem OMS k OMS, přímo nebo prostřednictvím OMS brány.

![ADM agentů](media/operations-management-suite-application-dependency-monitor/agents.png)

Pokud jste zákazníkem SCOM se skupinou Správa připojení k OMS:

- Pokud SCOM agentů získat přístup k Internetu se připojit k OMS, je potřeba nevyžaduje žádnou další konfiguraci.  
- Pokud SCOM agentů přístup OMS přes internet, musíte se ke konfiguraci brány OMS pro práci s SCOM.
  
Pokud používáte přímé Agent OMS, budete muset konfigurace agenta OMS pro připojení k OMS nebo ho bráně OMS.  Brána OMS si můžete stáhnout z [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Zamezení duplicitních dat

Pokud jste zákazníkem SCOM, neměli byste konfigurovat SCOM agentů sdělovat přímo OMS nebo dat bude vykázat dvakrát.  V ADM výsledkem bude počítačů dvakrát v seznamu počítače.

Konfigurace OMS má proběhnout pouze v jednom z následujících umístění:

- Panel SCOM konzoly operace správy sady pro spravovaných počítačů
- Azure provozní přehledy konfigurace ve vlastnostech MMA

Obě konfigurace pomocí stejného pracovního prostoru v každém způsobí dat. duplikace. Jak u různých pracovních prostorů pomocí může způsobit konfliktní konfigurace (jednu s ADM řešení povolené a druhý bez), který brání pokračuje ADM úplně data.  

Všimněte si, že i když vlastní počítač není zadaný v konzole SCOM OMS konfiguraci Pokud skupinu Instance například "Skupiny pro Windows Server instance" je aktivní, pořád udělování přijímání OMS konfiguraci počítače prostřednictvím SCOM.



## <a name="management-packs"></a>Správa sad
Po aktivaci ADM v pracovním prostoru OMS 300KB Management Pack se pošle všem Microsoft sledování agentů v tomto pracovním prostoru.  Pokud používáte SCOM agentů v [připojené Správa skupiny](../log-analytics/log-analytics-om-agents.md), ADM Management Pack nasazeny z SCOM.  Pokud agentů přímo připojení, MP budete dostávat OMS.

MP jmenuje Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  To je došlo k zápisu *%Programfiles%\Microsoft sledování Agent\Agent\Health State\Management aktualizace\*.  Zdroj dat použít tak, že management pack je *% Program files%\Microsoft sledování Agent\Agent\Health služby State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfigurace
Kromě Windows a Linux počítače, máte agent nainstalovaný a připojení k OMS, instalační program závislost Agent musí být stažené ze ADM řešení a na každý spravovaných server nainstalovaný jako kořenovou nebo správce.  Po instalaci agenta ADM na serveru sestav OMS ADM závislost map zobrazí 10 minut.  Pokud máte problémy, e-mailem [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migrace z BlueStripe FactFinder
Sledování závislost aplikace dodá BlueStripe technologii do OMS v fáze. FactFinder je pořád podporované pro stávající zákazníky, ale už není dostupná pro jednotlivé nákup.  Tato verze preview agenta závislost pouze komunikovat s OMS.  Pokud jste zákazníkem aktuální FactFinder, uveďte sadu servery test pro ADM, které nejsou spravovány FactFinder. 

### <a name="download-the-dependency-agent"></a>Stažení agenta závislostí
Kromě správy Microsoft Agent (MMA) a OMS Linux Agent, které umožňují připojení mezi počítačem a OMS musí mít počítače, analyzovat aplikace závislost sledování instalaci agenta závislost.  Na Linux musí být nainstalovaný před agenta závislost agenta OMS Linux. 

![Dlaždice aplikace závislost monitoru](media/operations-management-suite-application-dependency-monitor/tile.png)

Chcete-li stáhnout agenta závislosti, klikněte na **Konfigurovat řešení** v dlaždici **Aplikace závislost Monitor** otevřete zásuvné **Závislost Agent** .  Závislost typu Agent zásuvné obsahuje odkazy pro Windows a agentů Linux. Klikněte na příslušný odkaz pro stažení každý agent. Naleznete v následujících částech Podrobné informace o instalaci agent v různých sítě.

### <a name="install-the-dependency-agent"></a>Instalace agenta závislostí

#### <a name="microsoft-windows"></a>Microsoft Windows
Instalace a odinstalace agent je třeba oprávnění správce.

Závislost typu Agent nainstalovaný v počítačích Windows s ADM Agent Windows.exe. Pokud spustíte tento spustitelný soubor bez všechny možnosti, potom bude spuštěn průvodce, který sledujete instalace interaktivně.  

Pomocí následujících kroků instalace agenta závislost na každém počítači Windows.

1.  Ujistěte se, že je nainstalovaný agenta OMS pomocí pokynů u počítačů připojit přímo do OMS.
2.  Stáhnout agent systému Windows a spustit pomocí následujícího příkazu.<br>*ADM Agent Windows.exe*
3.  Postupujte podle pokynů průvodce nainstalovat agent.
4.  Pokud Agent závislost nespustí, v protokolech podrobné informace o chybě. Adresář s protokolem zvýrazněná na Windows agenti – *C:\Program Files\BlueStripe\Collector\logs*. 

Závislost Agent pro systém Windows můžete odinstalovat správcem v Ovládacích panelech.


#### <a name="linux"></a>Linux
Přístup kořenové potřebných k instalace a konfigurace agenta.

Agent závislost nainstalovaný na počítačích s ADM-Agent-Linux64.bin skriptu prostředí s extrahování binární Linux. Můžete spustit soubor s sh nebo můžete přidat oprávnění pro samotný soubor.
 
Pomocí následujících kroků instalace agenta závislost na každém počítači Linux.

1.  Zkontrolujte, zda je OMS agent nainstalována pomocí pokynů uvedených v [Shromáždit a spravovat data z počítače Linux.  To je potřeba nainstalovat před Agent závislost Linux](https://technet.microsoft.com/library/mt622052.aspx).
2.  Instalace agenta Linux závislost jako kořenového pomocí následujícího příkazu.<br>*sh ADM Agent Linux64.bin*.
3.  Pokud Agent závislost nespustí, v protokolech podrobné informace o chybě. Adresář s protokolem zvýrazněná na Linux agenti – */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Odinstalace agenta závislost na Linux
Úplně odinstalovat agenta závislost z Linux, musíte odebrat agent sebe sama a proxy server, který se instaluje automaticky s agentem.  Můžete i pomocí následujícího příkazu jednoho odinstalovat.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Instalace z příkazového řádku
V předchozí části obsahuje pokyny k instalaci agent závislost sledování pomocí výchozích možností.  V následujících částech obsahují pokyny k instalaci agent z příkazového řádku pomocí vlastních možností.

#### <a name="windows"></a>Windows
Pomocí možností v tabulce níže instalace z příkazového řádku. Chcete-li zobrazit seznam instalace příznaků spusťte instalační program s /? označení takto.

    ADM-Agent-Windows.exe /?

| Příznak | Popis |
|:--|:--|
| /S | Provedení používaného bez zobrazování výzev uživatele. |

Soubory pro Agent závislost Windows jsou umístěny v *C:\Program Files\BlueStripe\Collector* ve výchozím nastavení.


#### <a name="linux"></a>Linux
Pomocí možností v tabulce níže provést instalaci. Zobrazení seznamu požadovaný typ instalace příznaků spuštění instalace aplikace pomocí - příznak následujícím způsobem.

    ADM-Agent-Linux64.bin -help

| Popis příznaku
|:--|:--|
| – Ne | Provedení používaného bez zobrazování výzev uživatele. |
| – Zkontrolujte | Zkontroluje, oprávnění a operační systém, ale nenainstaluje agent. |

Soubory pro agenta závislost jsou umístěny v následujících adresářů.

| Soubory | Umístění |
|:--|:--|
| Základní soubory | /usr/lib/bluestripe-Collector |
| Protokoly | /var/OPT/Microsoft/Dependency-Agent/log |
| Konfigurace souborů | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Služba spustitelných | /sbin/bluestripe-Collector<br>/sbin/bluestripe-Collector-Manager |
| Binární úložiště souborů | /var/OPT/Microsoft/Dependency-Agent/Storage |



## <a name="troubleshooting"></a>Řešení potíží
Pokud jste docházet k problémům s aplikací závislost Monitor, můžete získat informace o řešení potíží z více komponent pomocí následujících informací.

### <a name="windows-agents"></a>Windows agentů

#### <a name="microsoft-dependency-agent"></a>Závislost typu Microsoft Agent
Při generování Poradce při potížích dat z agenta závislosti, otevřete okno příkazového řádku jako správce a spusťte skript CollectBluestripeData.vbs pomocí následujícího příkazu.  Můžete přidat – nápovědy příznak zobrazíte další možnosti.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Sady dat podpory se uloží do adresáře % uživatelského profilu % aktuálního uživatele.  Můžete použít – souboru <filename> možnost uložit do jiného umístění.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Závislost typu Microsoft Agent Management Pack pro MMA
Závislost typu Agent Management Pack spuštěn uvnitř Microsoft Agent pro správu.  Načte data z agenta závislost a předá ho cloudovou službu ADM.
  
Ověřte, že se management pack stahuje provedením následujících kroků.

1.  Najděte soubor s názvem Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp v C:\Program Files\Microsoft sledování Agent\Agent\Health State\Management aktualizace.  
2.  Pokud soubor neobsahuje data a agent je připojený do skupiny Správa SCOM, ověřte, že byl importované do SCOM kontrolou Management Pack v pracovním prostoru správy konzoly operace.

ADM MP zapíše do protokolu událostí systému Windows Operations Manager události.  Protokol může být [Prohledávat v OMS](../log-analytics/log-analytics-log-searches.md) prostřednictvím protokolu řešení systému, kde lze konfigurovat které protokoly nahrát.  Pokud jsou povolené ladění události, že jsou došlo k zápisu protokolu událostí, ke zdroji události *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Sledování programu Microsoft Agent
Získat informace o diagnostické trasování, otevřete okno příkazového řádku jako správce a následující příkazy: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Aby došlo k c:\Windows\Logs\OpsMgrTrace jsou zápisu trasování.  Můžete zastavit sledování s StopTracing.cmd.


### <a name="linux-agents"></a>Linux agentů

#### <a name="microsoft-dependency-agent"></a>Závislost typu Microsoft Agent
Generovat Poradce při potížích data z agenta závislost přihlášení pomocí účtu, který má oprávnění sudo nebo kořenového adresáře a spusťte tento příkaz.  Můžete přidat na – příznak Nápověda zobrazíte další možnosti.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Sady dat podpory se uloží do /var/opt/microsoft/dependency-agent/log (Pokud kořenové) v části agenta instalační adresář nebo k adresáři domácí uživatel, který spustil skript (je-li jiné než kořenové).  Můžete použít – souboru <filename> možnost uložit do jiného umístění.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Závislost Microsoft Agent Fluentd Linux modulu Plug-in
Modul Plug-in Fluentd Agent závislost spuštěn uvnitř agenta OMS Linux.  Načte data z agenta závislost a předá ho cloudovou službu ADM.  

Aby došlo k následující dva soubory jsou zápisu protokolů.

- /var/OPT/Microsoft/omsagent/log/omsagent.log
- /var/log/Messages

#### <a name="oms-agent-for-linux"></a>Agent OMS Linux
Poradce při potížích zdroje pro připojení Linux servery k OMS najdete tady: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Protokoly pro agenta OMS Linux jsou umístěny v */var a určovat, jestli / / omsagent/protokolu microsoft/*.  

Protokoly pro omsconfig (Konfigurace agenta) jsou umístěny v */var a určovat, jestli / / omsconfig/protokolu microsoft/*.
 
Log (OMI) a SCX součástí, které poskytují data o výkonu metriky jsou umístěny v */var a určovat, jestli/režimu omi/protokol/* a */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Shromažďování dat
Každý agent pro přenos přibližně 25MB denně, podle toho, jak složitý závislosti systém jsou můžete očekávat.  ADM závislost odesílaly každý agentem každých 15 sekund.  

ADM Agent obvykle spotřebovává 0,1 paměti systému a 0,1 % systému procesoru.


## <a name="supported-operating-systems"></a>Podporované operační systémy
Následující části uvádějí podporované operační systémy pro agenta závislost.   32bitové architektury nejsou podporované pro žádný operační systém.

### <a name="windows-server"></a>Windows Server
- Windows serveru 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Počítač s Windows
- Poznámka: Windows 10 ještě nepodporuje
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Červené klobouk Enterprise Linux, CentOS Linux a Linux Oracle (plus pár RHEL jádra)
- Jsou podporované jenom výchozí a vydáních jádra SMP Linux.
- Nestandardní jádra vydání, například rozšíření fyzické adresy a Xen, nejsou podporované pro všechny rozdělení Linux. Například systém vydání řetězcem "2.6.16.21-0.8-xen" není podporován.
- Vlastní jádra, včetně překompilování standardní jádra, nejsou podporované
- Centos Plus jádra nepodporuje.
- Oracle nerozbitného jádra (UEK) jsou obsaženy v různých oddíl níž.


#### <a name="red-hat-linux-7"></a>Červené klobouk Linux 7
| Operační systém verze | Verze jádra |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Červené klobouk Linux 6
| Operační systém verze | Verze jádra |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Červené klobouk Linux 5
| Operační systém verze | Verze jádra |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux s nerozbitného jádra (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Operační systém verze | Verze jádra
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Operační systém verze | Verze jádra
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Operační systém verze | Verze jádra
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Operační systém verze | Verze jádra
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnostika a použití dat
Microsoft se automaticky shromažďují použití a výkonu dat prostřednictvím používání služby aplikace závislost Monitor. Společnost Microsoft použije tyto údaje poskytovat a zlepšovat kvalitu, zabezpečení a integrity služby aplikace závislost Monitor. Data informace o konfiguraci software jako verze operačního systému a toho obsahuje a IP adresu, DNS název a název Workstation za účelem zajištění přesné a efektivně Poradce při potížích možnosti. Nebudeme zjišťovat jména, adresy nebo jiné kontaktní informace.

Další informace o shromažďování dat a použití najdete v článku [Microsoft Online Services zásady ochrany osobních údajů](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Další kroky
- Zjistěte, jak jednou [pomocí aplikace závislost sledování](operations-management-suite-application-dependency-monitor.md) bylo nasazeném a nakonfigurovali.
