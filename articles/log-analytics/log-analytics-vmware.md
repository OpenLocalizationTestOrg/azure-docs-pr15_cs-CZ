<properties
    pageTitle="Sledování VMware řešení v protokolu analýzy | Microsoft Azure"
    description="Informace o tom, jak můžete řešení VMware sledování pomoct sledovat ESXi tabulkami hosts a spravovat protokoly."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Řešení v protokolu analýzy VMware sledování (verze Preview)

Sledování VMware řešení v protokolu analýzy je řešení, které vám pomůže vytvořit centralizované protokolování a monitorování přístup pro velké VMware protokoly. Tento článek popisuje, jak můžete Poradce při potížích s, zachycení a správa ESXi tabulkami hosts na jednom místě pomocí řešení. S řešení zobrazí se podrobná data pro všechny vaše ESXi tabulkami hosts na jednom místě. Uvidíte počty horní události, stav a trendů OM a ESXi tabulkami hosts poskytovanou protokoly ESXi Host (hostitel). Můžete vyřešit tak, že zobrazení a hledání centralizované ESXi hostitele protokoly. A můžou vytvářet výstrahy podle protokolu vyhledávacích dotazů.

Řešení používá funkce nativního syslog hostitele ESXi vložit data do cílové OM, který má OMS Agent. Řešení však není napište soubory do syslog v cílové OM. OMS agent otevře port 1514 a sleduje takto. Jakmile obdrží data, OMS agent posune data do OMS.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení

Instalace a konfigurace řešení použijte následující informace.

- Přidání sledování VMware řešení OMS pracovního prostoru pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Podporované VMware ESXi tabulkami hosts
vSphere ESXi hostitele 5.5 a 6.0

#### <a name="prepare-a-linux-server"></a>Příprava Linux server
Vytvoření operační systém OM přijme všechna data syslog od ESXi tabulkami hosts Linux. [OMS Linux Agent](log-analytics-linux-agents.md) představuje bod kolekce pro všechna data syslog ESXi Host (hostitel). Více ESXi tabulkami hosts umožňuje přeposlat protokoly k jednomu Linux serveru, jako v následujícím příkladu.  

   ![Syslog toku](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfigurace syslog kolekce

1. Nastavení přesměrování syslog pro VSphere. Podrobné informace o tom, nastavte přesměrování syslog najdete v článku [Konfigurace syslog na ESXi 5.x a 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Přejděte na **Konfigurace hostitele ESXi** > **Software** > **Upřesňující nastavení** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. V poli *Syslog.global.logHost* přidáte Linux server a číslo portu *1514*. Například `tcp://hostname:1514` nebo`tcp://123.456.789.101:1514`

3. Otevřete hostitelské brány firewall ESXi pro syslog. **Konfigurace hostitele ESXi** > **Software** > **Zabezpečovací profil** > **bránu Firewall** a otevřít **Vlastnosti**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Zaškrtněte políčko vSphere konzoly pro ověření, že tento syslog správně nastavená. Potvrďte na hostiteli ESXI přístavu **1514** nakonfigurovaný.

5. Test připojení mezi Linux server a hostiteli ESXi pomocí `nc` příkazu na hostiteli ESXi. Příklad:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Stažení a instalace agenta OMS Linux na serveru Linux. Další informace najdete v článku [si přečtěte následující dokumentaci pro OMS Agent Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Po instalaci agenta OMS Linux přejděte do adresáře /etc/opt/microsoft/omsagent/sysconf/omsagent.d a zkopírujte soubor vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d adresář a změna vlastníka nebo skupinu a oprávnění pro tento soubor. Příklad:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Restartujte agenta OMS Linux spuštěním `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Na portálu OMS vyhledejte protokolu `Type=VMware_CL`. Když OMS shromáždí syslog data, zachovává formát syslog. Na portálu některé konkrétních polí ukládány, jako je *název hostitele* a *název_procesu*.  

    ![Typ](./media/log-analytics-vmware/type.png)  

    -Li zobrazit výsledky hledání protokolu vidíte na obrázku nahoře, máte nastavené můžete na řídicím panelu OMS VMware sledování řešení.  

## <a name="vmware-data-collection-details"></a>VMware podrobnosti shromažďování dat

Řešení VMware sledování shromažďuje různých metriky a protokolu údaje o výkonu ESXi tabulkami hosts používání agentů OMS Linux, které jste zapnuli automatický přístup.

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se shromažďují data.

| platformy | Agent OMS Linux | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Linux|![Ano](./media/log-analytics-vmware/oms-bullet-green.png)|![Ne](./media/log-analytics-vmware/oms-bullet-red.png)|![Ne](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Ne](./media/log-analytics-containers/oms-bullet-red.png)|![Ne](./media/log-analytics-vmware/oms-bullet-red.png)| Každý 3 minuty|


V následující tabulce zobrazit příklady datových polí shromážděná řešení VMware sledování:

| Název pole | Popis |
| --- | --- |
| Device_s| VMware úložiště zařízení |
| ESXIFailure_s | Chyba při typy |
| EventTime_t | čas výskytu události |
| HostName_s | Název hostitele ESXi |
| Operation_s | Vytvoření OM nebo odstranění OM |
| ProcessName_s | Název události |
| ResourceId_s | název hostitele VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | Stav VMware SCSI |
| SyslogMessage_s | Syslog dat |
| UserName_s | uživatele, kterému vytvoření nebo odstranění OM |
| VMName_s | Název OM |
| Počítač | hostitelském počítači |
| TimeGenerated | čas, kdy byl vytvořen data |
| DataCenter_s | VMware datacentru |
| StorageLatency_s | latence úložiště ([ms) |

## <a name="vmware-monitoring-solution-overview"></a>Přehled sledování VMware řešení

VMware dlaždice na portálu OMS. Poskytuje souhrnné zobrazení všechny chyby. Po kliknutí na dlaždici přejdete do zobrazení řídicího panelu.

![dlaždice](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Přejděte na řídicí panel zobrazení

V zobrazení řídicího panelu **VMware** listy seřazené podle:

- Počet stav chyb
- Vrátí začátek hostitele události
- Vrátí začátek události
- Virtuální počítač aktivity
- Host (hostitel) ESXi disku události


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klikněte na libovolnou zásuvné otevřete podokno hledání protokolu analýzy zobrazující podrobné informace specifické pro zásuvné.

Tady můžete upravit vyhledávacího dotazu k úpravám pro určitý objekt. Návod na základní informace o hledání OMS, najdete v článku [OMS protokolu hledání kurz.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Najít ESXi hostitele události

Jednoho hostitele ESXi vygeneruje více protokoly, na základě jejich procesů. Řešení VMware sledování centralizuje je a shrnuje počty události. Toto zobrazení centralizované pomůže porozumět hostitele ESXi, který obsahuje velké množství událostí a jaké událostem modus-nejčastěji ve vašem prostředí.

![události](./media/log-analytics-vmware/events.png)

Umožňuje další po kliknutí na hostiteli ESXi nebo typ události.

Když kliknete na název hostitele ESXi, zobrazit informace z této ESXi hostitele. Pokud chcete zúžit výsledky s typem události, přidejte `“ProcessName_s=EVENT TYPE”` vyhledávacího dotazu. Vyberte **název_procesu** ve vyhledávacím filtru. Informace, který zúží.

![procházení](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Vyhledání činnosti s vysokým OM

Virtuální počítač lze vytvořit a odstranit na libovolného ESXi hostitele. Je užitečné pro správce k identifikaci kolik VMs hostiteli ESXi vytvoří. Tento v zapnout, pomůže pochopit plánování výkonu a kapacity. Zachování přehledu o OM aktivity události je důležité, pokud spravujete prostředí.

![procházení](./media/log-analytics-vmware/vmactivities1.png)

Pokud chcete zobrazit další ESXi hostitele OM dat pro vytváření, klikněte na název hostitele ESXi.

![procházení](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Běžné vyhledávacích dotazů

Řešení obsahuje jiné užitečné dotazy, které vám usnadní správu ESXi hosts, například vysoké úložného prostoru úložiště latence a cesta selhání.

![dotazy](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Uložit dotazy

Uložení vyhledávacích dotazů je standardní funkcí v OMS a vám pomůže uchovávat všechny dotazy, které najdete užitečné. Po vytvoření dotazu, která je užitečná uložte kliknutím na **Oblíbené položky**. Uložený dotaz umožňuje snadno jej znovu použít na stránce [Moje řídicího panelu](log-analytics-dashboards.md) které můžete vytvořit vlastní řídicího panelu.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Vytvořit upozornění dotazů

Po vytvoření dotazů můžete pomocí dotazů upozorňující zvláštní události dojít. Informace o tom, jak vytvořit upozornění naleznete v tématu [upozornění v protokolu analýzy](log-analytics-alerts.md) . Příklady výstrahy dotazů a další příklady dotazu najdete v článku příspěvek blogu [Monitor VMware pomocí OMS protokolu analýzy](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) .

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Co je potřeba v ESXi hostovat nastavení? Jaký vliv bude mít na svůj aktuální prostředí?
Řešení používá nativní Syslog hostitele ESXi přesměrování mechanismus. Nemusíte jakékoli další software společnosti Microsoft na hostiteli ESXi k zaznamenání protokoly. Měl by mít zhoršeným vliv na existující prostředí. Přesto musíte nastavit přesměrování syslog, což je funkce ESXI.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Je potřeba restartovat hostitel osobních ESXi?
Ne. Tento proces nevyžaduje restartovat počítač. V některých případech vSphere správně neaktualizuje syslog. V takovém případě přihlaste k hostiteli ESXi a načíst syslog. Nemáte znovu, restartujte hostiteli, tak tento proces není skutečnosti prostředí.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Můžete zvýšit nebo snížit množství dat protokolu posílat OMS?
Ano, můžete. Úroveň protokolování hostitele ESXi nastavení můžete použít v vSphere. Protokol kolekce je založena na úrovni *informace* . Ano Pokud mají být auditovány OM vytvoření nebo odstranění musíte zachovat úroveň *informace* na Hostd. Další informace najdete v tématu [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Proč není Hostd poskytuje data OMS? Moje protokolu nastavena na informace.
Došlo ESXi hostitele chyb pro časové razítko syslog. Další informace najdete v tématu [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Po použití řešení Hostd by měl normálně.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Můžete mít víc ESXi tabulkami hosts přesměrování syslog data do jednoho OM s omsagent?
Ano. Můžete mít více ESXi tabulkami hosts přesměrování do jednoho OM s omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Proč nevidím toku do OMS dat?

Může být z několika důvodů:

- Hostiteli ESXi není správně předání dat do OM systém omsagent. Chcete-li otestovat, proveďte následující kroky:
    1. Abyste si ověřili, přihlaste se k hostiteli ESXi pomocí ssh a spusťte tento příkaz:`nc -z ipaddressofVM 1514`

        Pokud to není úspěšné, vSphere nastavení v konfiguraci rozšířeného mívají neodstraní. Přečtěte si článek [Konfigurace syslog kolekce](#configure-syslog-collection) informace o tom, jak nastavit ESXi hostitele syslog přesměrování.

    2. Pokud je úspěšně syslog port připojení, ale pořád nevidíte všechna data, pak znovu načtěte syslog na hostiteli ESXi pomocí ssh spusťte tento příkaz:` esxcli system syslog reload`

- OM s OMS Agent není správně nastavená. Chcete-li otestovat, proveďte následující kroky:
    1. OMS přijímá port 1514 a zapisuje data do OMS. Pokud chcete ověřit, zda je otevřen, spusťte tento příkaz:`netstat -a | grep 1514`
    2. Měli byste vidět port `1514/tcp` otevřete. Když uděláte není správně instalaci omsagent ověřte. Pokud nevidíte informace o portu, není syslog port otevřený v OM.
        1. Zkontrolujte, jestli je spuštěný agenta OMS pomocí `ps -ef | grep oms`. Pokud není spuštěná, spusťte proces pomocí příkazu` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Otevřít `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` soubor.

            Zkontrolujte, že správné uživatelů a nastavení skupiny platné, podobně jako:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Pokud soubor neexistuje nebo je špatně na uživatele a nastavení skupiny, akce korekční tak, že [Příprava serveru Linux](#prepare-a-linux-server).

## <a name="next-steps"></a>Další kroky

- Pomocí [Protokolu vyhledávání](log-analytics-log-searches.md) v protokolu analýzy zobrazíte podrobná data VMware Host (hostitel).
- [Vytvářet své vlastní řídicí panely](log-analytics-dashboards.md) zobrazující VMware hostitele data.
- [Vytvořit upozornění](log-analytics-alerts.md) při určitých VMware hostitele událostem.
