<properties 
   pageTitle="Syslog zpráv v protokolu analýzy | Microsoft Azure"
   description="Syslog je protokol protokolování událostí, který je běžné Linux.   Tento článek popisuje postup při konfiguraci kolekci zpráv Syslog v protokolu technologie pro analýzu a podrobnosti o záznamy, které by vytvářeli v úložišti OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Zdroje dat Syslog v protokolu analýzy

Syslog je protokol protokolování událostí, který je běžné Linux.  Aplikace se odesílat zprávy, které mohou být uloženy v místním počítači nebo Doručená Syslog kolekcí.  Po instalaci agenta OMS Linux nakonfiguruje démona místní Syslog předávání zpráv agenta.  Agent pošle zprávu do protokolu technologie pro analýzu tam, kde je v úložišti OMS vytvořili odpovídající záznam.  

> [AZURE.NOTE]Protokol analýzy podporuje kolekci zpráv odeslaných rsyslog nebo syslog ng. Démona syslog výchozí verze 5 verze Enterprise Linux klobouk červené, CentOS a Oracle Linux (sysklog) není podporované pro shromažďování syslog událostí. Sběr dat syslog v této verzi tyto distribuce, by měl [rsyslog démon](http://rsyslog.com) nainstalovali a nakonfigurovali nahrazení sysklog.

![Kolekce Syslog](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Konfigurace Syslog
Agent OMS Linux pouze shromáždí událostí pomocí zařízení a severities určených v konfiguraci.  Syslog můžete nakonfigurovat pomocí portálu OMS nebo Správa konfigurace souborů na agentů Linux.


### <a name="configure-syslog-in-the-oms-portal"></a>Konfigurace Syslog OMS portálu

Konfigurace Syslog v [nabídce Data v dialogovém okně nastavení technologie pro analýzu protokolu](log-analytics-data-sources.md#configuring-data-sources).  Konfigurace dostane konfiguračního souboru na každý agent Linux.

Přidat nové zařízení tak, že zadáte jeho název a kliknutím na **+**.  Pro každý zařízení budou shromážděny jenom na zprávy s vybranou severities.  Zaškrtněte políčko severities pro konkrétní zařízení, která chcete shromažďovat.  Nelze zadat další kritéria pro filtrování zpráv.

![Konfigurace Syslog](media/log-analytics-data-sources-syslog/configure.png)


Ve výchozím nastavení se všechny změny konfigurace automaticky posunou všechny agentů.  Pokud chcete ručně nakonfigurovat Syslog každý Linux agent, potom zrušte zaškrtnutí políčka *použít pod konfigurace Moje počítačům Linux*.


### <a name="configure-syslog-on-linux-agent"></a>Konfigurace Syslog Linux agenta

Když [OMS agent nainstalovaný v klientském počítači Linux](log-analytics-linux-agents.md), nainstaluje výchozí syslog konfiguračního souboru, který definuje zařízení a závažnosti zprávy, které byly shromážděny.  Můžete upravit tento soubor a změnit konfiguraci.  Konfigurační soubor se liší v závislosti na démona Syslog nainstalovaného klienta.

> [AZURE.NOTE] Pokud upravíte syslog konfigurace, je nutné restartovat démona syslog změny se projeví.

#### <a name="rsyslog"></a>rsyslog

Konfigurační soubor rsyslog je umístěn na **/etc/rsyslog.d/95-omsagent.conf**.  Výchozí obsah jsou uvedeny níže.  To shromažďuje syslog zprávám odesílaným z místní agent pro všechny zařízení s úrovní upozornění nebo vyšší.

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

Zařízení můžete odebrat tím, že odeberete jejich část konfiguračního souboru.  Můžete nastavit omezení severities, které byly shromážděny pro konkrétní zařízení změnou položky, které zařízení.  Například omezit zařízení uživatele ke zprávám s závažnosti chyby nebo vyšší by upravte tento řádek konfiguračního souboru následujícím způsobem:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog ng

Konfigurační soubor rsyslog je umístění **/etc/syslog-ng/syslog-ng.conf**.  Výchozí obsah jsou uvedeny níže.  To shromažďuje syslog zprávám odesílaným z místní agent pro všechny zařízení a všechny severities.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Zařízení můžete odebrat tím, že odeberete jejich část konfiguračního souboru.  Můžete nastavit omezení severities, které byly shromážděny pro konkrétní zařízení odstraněním ze seznamu.  Například autora zařízení uživatele k jenom upozornění a důležité zprávy, by změníte danou část konfiguračního souboru následujícím způsobem:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Změna Syslog port

OMS agent přijímají Syslog zprávy místního klienta přístavu 25224.  Tento port můžete změnit přidáním v následující části OMS agent konfiguračního souboru c:\ **/etc/opt/microsoft/omsagent/conf/omsagent.conf**.  Nahraďte 25224 v položce **port** číslo portu, které chcete.  Nezapomeňte, že můžete taky musí změnit konfiguračního souboru pro démona Syslog k odesílání zpráv na tento port.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Shromažďování dat

OMS agent přijímají Syslog zprávy místního klienta přístavu 25224. Konfigurační soubor démona Syslog předá Syslog zprávám odesílaným z aplikace k tomuto portu, kdy byly shromážděny podle protokolu analýzy.


## <a name="syslog-record-properties"></a>Syslog dialogovém okně Vlastnosti záznamu

Záznamy Syslog mít typ **Syslog** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Počítač | Počítač, který byl odebrané události. |
| Zařízení | Definuje součást systému, které vygenerovalo zprávu. |
| HostIP | IP adresy systému odeslání zprávy.  |
| Název hostitele | Název systému odeslání zprávy. |
| SeverityLevel | Úroveň závažnosti události. |
| SyslogMessage | Text zprávy. |
| ID procesu | ID procesu, které vygenerovalo zprávu. |
| EventTime | Datum a čas, které vygenerovalo události.



## <a name="log-queries-with-syslog-records"></a>Protokol dotazy se Syslog záznamy

V následující tabulce jsou uvedeny různých příklady protokolu dotazy, které načíst Syslog záznamy.

| Dotaz | Popis |
|:--|:--|
| Typ = Syslog | Všechny Syslogs. |
| Typ = Syslog SeverityLevel = chyba | Všechny záznamy Syslog s závažnosti chyby. |
| Typ = Syslog & #124; míra count() ve počítače | Počet Syslog záznamy ve počítače. |
| Typ = Syslog & #124; míra count() tak, že zařízení | Počet Syslog záznamy tak, že zařízení. |

## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení. 
- Použijte [Vlastní pole](log-analytics-custom-fields.md) analyzovat data ze záznamů syslog do jednotlivých polí.
- [Konfigurace Linux agentů](log-analytics-linux-agents.md) získat informace o jiných typů dat. 
