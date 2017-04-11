<properties
    pageTitle="Kontejnery řešení v protokolu analýzy | Microsoft Azure"
    description="Kontejnery řešení v protokolu analýzy umožňuje zobrazovat a spravovat svůj Docker kontejneru hosts na jednom místě."
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



# <a name="containers-preview-solution-log-analytics"></a>Kontejnery (verze Preview) řešení protokolu analýzy

Tento článek popisuje, jak nastavit a používat řešení kontejnery v protokolu technologie pro analýzu, která slouží k zobrazení a správa vašeho hostitele kontejneru Docker na jednom místě. Docker je systém virtualizace software použitá k vytvoření kontejnery, které automatizují software nasazení jejich IT infrastrukturu.

S řešení uvidíte, jaký kontejnery spuštěných pro kontejner hosts a co obrázky běží v kontejnerech. Můžete zobrazit podrobnosti o auditování zobrazující příkazů s kontejnery. A kontejnery můžete vyřešit tak, že zobrazení a prohledávání centralizované protokoly aniž byste museli vzdáleně zobrazit Docker tabulkami hosts. Můžete najít kontejnery, které mohou být vysokou úroveň šumu a náročné nadbytečné zdroje na hostitele. A můžete zobrazit centralizované procesoru, paměti, ukládání a informace použití a výkonu sítě pro kontejnery.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení

Instalace a konfigurace řešení použijte následující informace.

Přidání kontejnerů řešení do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).

Existují dva způsoby, jak nainstalovat a používat Docker s OMS:

- Podporované Linux operačních systémech instalace a spuštění Docker a pak nainstalujte a nakonfigurujte OMS Agent Linux
- Na jádro operačního systému nainstalujte a spusťte Docker a potom i konfiguraci OMSAgent spuštění uvnitř kontejneru

Hostitele kontejneru na [GitHub](https://github.com/Microsoft/OMS-docker)naleznete v tématu podporované operační systém verze Docker a Linux.

>[AZURE.IMPORTANT] Docker musí být pracovního **před** instalace [OMS Agent Linux](log-analytics-linux-agents.md) v kontejneru hosts. Pokud jste si už nainstalovali agent před instalací Docker, musíte k přeinstalaci agenta OMS Linux. Další informace o Docker najdete na [webu Docker](https://www.docker.com).

Potřebujete na kontejner hosts nakonfigurovat před můžete sledovat kontejnery následující nastavení.

## <a name="configure-settings-for-the-linux-container-host"></a>Konfigurace nastavení pro hostiteli Linux kontejneru

Po instalaci Docker, použijte následující nastavení vašeho hostitele kontejneru nakonfigurovat agent pro použití s Docker. Jádro operačního systému, které nejsou podporovány tento způsob konfigurace.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Konfigurace nastavení pro hostiteli kontejneru - systemd (SUSE, openSUSE, CentOS 7.x RHEL 7.x a systémem Ubuntu 15.x nebo novější)

1. Úprava docker.service přidáte takto:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Přidání $DOCKER\_ROZHODNE v &quot;ExecStart = / usr/Koš/docker démon&quot; v souboru docker.service. V následujícím příkladu.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Restartujte Docker. Příklad:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Konfigurace nastavení pro hostiteli kontejneru - Upstart (se systémem Ubuntu 14.x)

1. Úprava /etc/default/docker a přidejte tyto informace:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Uložte soubor a potom restartujte Docker a OMS služby.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Konfigurace nastavení pro hostiteli kontejneru - Amazon Linux

1. Úprava /etc/sysconfig/docker a přidejte tyto informace:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Uložte soubor a znovu spusťte službu Docker.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Konfigurace nastavení pro kontejnery jádro operačního systému

Po instalaci Docker, použijte následující nastavení pro jádro operačního systému a spustit Docker vytvořit kontejner. Můžete použít jakoukoli podporovanou verzi Linux – včetně jádro operačního systému, se tento způsob konfigurace. Budete potřebovat svoje [ID pracovního prostoru OMS a klíčem](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>Můžete OMS u všech kontejnerů s jádro operačního systému

- Spusťte kontejneru OMS, který chcete sledovat. Úprava a použijte v následujícím příkladu.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Přechod z pomocí nainstalované agenta do jedné v kontejneru

Pokud dříve použili agent přímo nainstalovaný a chcete místo toho použít agent spuštěné v kontejneru, je třeba nejprve odebrat OMSAgent. Najdete v článku [Pokyny k instalaci agenta OMS Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Kontejnery podrobnosti shromažďování dat

Řešení kontejnery shromažďuje různých metriky a protokolu data o výkonu od kontejneru hosts a kontejnery používání agentů OMS Linux, které jste zapnuli automatický přístup a od OMSAgent spuštěné v kontejnerech.

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje kontejnerů.

| platformy | Agent OMS Linux | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Linux|![Ano](./media/log-analytics-containers/oms-bullet-green.png)|![Ne](./media/log-analytics-containers/oms-bullet-red.png)|![Ne](./media/log-analytics-containers/oms-bullet-red.png)|            ![Ne](./media/log-analytics-containers/oms-bullet-red.png)|![Ne](./media/log-analytics-containers/oms-bullet-red.png)| Každý 3 minuty|


V následující tabulce zobrazit příklady typů data shromážděná řešení kontejnery:

| Datový typ | Pole |
| --- | --- |
| Výkon tabulkami hosts a kontejnery | Počítač, název_objektu, název_čítače & #40; % Procesor času disku přečte MB, disku zapíše MB, MB využití paměti sítě bajty, sítě odesláno bajty, procesor sec použití sítě & #41; přepočtené, TimeGenerated, Cesta_k_čítači, SourceSystem |
| Kontejner zásob | TimeGenerated, počítače, kontejneru název, ContainerHostname, obrázek, ImageTag, ContinerState, ExitCode, EnvironmentVar, příkaz, CreatedTime, StartedTime, FinishedTime, SourceSystem, Id_kontejneru, ID obrázku |
| Obrázek zásob kontejneru | TimeGenerated počítače, obrázek, ImageTag, ImageSize, VirtualSize, spuštění, pozastavit, zastavení selže, SourceSystem, ID obrázku, TotalContainer |
| Kontejner protokolu | TimeGenerated počítače, ID obrázku, jméno container LogEntrySource LogEntry, SourceSystem, Id_kontejneru |
| Protokol služby kontejneru | TimeGenerated počítače, TimeOfCommand, obrázek, příkaz, SourceSystem, Id_kontejneru |

## <a name="monitor-containers"></a>Sledování kontejnery

Až budete mít povolený na portálu OMS řešení, zobrazí se dlaždici **kontejnery** zobrazující souhrnné informace o kontejneru hosts a kontejnery aplikaci hosts.

![Kontejnerech dlaždic](./media/log-analytics-containers/containers-title.png)

Na dlaždici ukazuje přehled o kolik kontejnery máte v prostředí a jestli máte selže, pracovního zastaveno.

### <a name="using-the-containers-dashboard"></a>Na řídicím panelu kontejnery

Klikněte na dlaždici **kontejnery** . Tady uvidíte zobrazení uspořádaný podle:

- Kontejner události
- Chyby
- Stav kontejnery
- Obrázek zásob kontejneru
- Vytížení procesoru a paměti výkonu

Jednotlivých podoken v řídicím panelu je vizuální znázornění hledání, které se spustí na údaje.

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash01.png)

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash02.png)

V **Kontejneru stav** zásuvné kliknutím horní oblasti, jak je ukázáno v následujícím příkladu.

![Stav kontejnery](./media/log-analytics-containers/containers-status.png)

Hledání protokolu otevře informace o tabulkami hosts a kontejnery spuštěné v nich.

![Hledání protokolu pro kontejnery](./media/log-analytics-containers/containers-log-search.png)

Tady můžete upravit vyhledávacího dotazu ho upravit a vyhledání určitých informací, že vás zajímá. Další informace o protokolu hledání najdete v článku [hledání protokolu v protokolu analýzy](log-analytics-log-searches.md).

Můžete třeba upravit dotaz hledání tak, že ukazovat přestal kontejnery místo pracovního kontejnery změnou **systém** do **ukončení uživatelem** vyhledávacího dotazu.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Poradce při potížích s vyhledáním kontejneru nezdařeném uložení

Pokud byl ukončen s kódem nenulového OMS označí kontejneru jako **poškozený** . V tématu Přehled chyby a k chybám v prostředí v zásuvné **Kontejnery se nezdařila** .

### <a name="to-find-failed-containers"></a>K vyhledání selhalo kontejnery

1. Klikněte na zásuvné **Kontejneru události** .  
  ![kontejnery události](./media/log-analytics-containers/containers-events.png)
2. Otevře se protokolu hledání zobrazující stav kontejnerů, podobně jako tento.  
  ![kontejnery stavu](./media/log-analytics-containers/containers-container-state.png)
3. Pak klikněte na hodnotu selhalo zobrazíte další informace, například velikost obrázku nebo počet přestal a nezdařeném uložení obrázků. Rozbalte položku **Zobrazit další** ID obrázek.  
  ![selhalo kontejnery](./media/log-analytics-containers/containers-state-failed.png)
4. Pak najděte kontejneru, na kterém běží tento obrázek. Napište do vyhledávacího dotazu.
  `Type=ContainerInventory <ImageID>`Zobrazí se protokoly. Se budou posunovat zobrazíte kontejneru selhalo.  
  ![selhalo kontejnery](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Hledání v protokolech kontejneru dat

Když máte řešení problémů s konkrétní chyby, může pomoci zobrazíte, kde se vyskytuje ve vašem prostředí. Následující typy protokolu vám pomůže vytvořit dotazů pro vrácení informací, že hledáte.

- **ContainerInventory** – použití tohoto typu, pokud chcete získat informace o umístění kontejneru, jaké jsou jejich jména a co obrázky používáte systém.
- **ContainerImageInventory** – použití tohoto typu, pokud chcete najít informace uspořádané podle obrázku a zobrazíte informace obrázek například obrázky, ID nebo velikost.
- **ContainerLog** – použití tohoto typu, pokud chcete najít informace v protokolu konkrétní chyby a položky.
- **ContainerServiceLog** – použití tohoto typu, pokud chcete najít informace o protokolu auditování pro démona Docker například spustit, zastavit, odstranit nebo příkazy vložit.

### <a name="to-search-logs-for-container-data"></a>Pokud chcete hledat protokoly pro kontejner data

- Vyberte obrázek, který znáte naposledy selže a vyhledat protokolů chyb. Začněte tím, že zjistíte název kontejneru, na kterém běží tento obrázek **ContainerInventory** vyhledávání. Příklad hledání`Type=ContainerInventory ubuntu Failed`  
    ![Hledání se systémem Ubuntu kontejnery](./media/log-analytics-containers/search-ubuntu.png)

  Podívejte se na jméno container vedle **názvu**a vyhledejte jsou protokoly. V tomto příkladu je `Type=ContainerLog adoring_meitner`.

**Zobrazení informací o výkonu**

Pokud začínáte vytvářet dotazy, může pomoci najdete v článku co je možné nejdřív. Pokud chcete zobrazit všechna data o výkonu, zkuste například obecných dotazu zadáním následujícího vyhledávacího dotazu.

```
Type=Perf
```

![kontejnery výkonu](./media/log-analytics-containers/containers-perf01.png)



Zobrazí se tato ve formuláři další grafické po kliknutí na slovo **metriky** ve výsledcích.

![kontejnery výkonu](./media/log-analytics-containers/containers-perf02.png)



Můžete omezit data o výkonu, které se zobrazuje v určitém kontejneru tak, že zadáte název je napravo od dotazu.

```
Type=Perf <containerName>
```

Který obsahuje seznam metriky výkonu, které byly shromážděny pro jednotlivé kontejner.

![kontejnery výkonu](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Příklad protokolu vyhledávacích dotazů

Je často užitečné k vytvoření dotazů počínaje příklad nebo dva jste a upravili, aby odpovídal prostředí. Jako výchozí bod můžete vyzkoušet zásuvné **Důležitá dotazů** můžete vytvořit rozšířené dotazy.

![Kontejnery dotazů](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Uložení protokolů vyhledávacích dotazů

Ukládání dotazů je standardní funkcí v protokolu analýzy. Uložením, budete mít k dispozici ty, které najdete užitečné užitečný pro budoucí použití.

Po vytvoření dotazu, která je užitečná uložte kliknutím na **Oblíbené položky** v horní části stránky protokolu hledání. Potom snadno dostanete ho později ze stránky **Moje řídicího panelu** .

## <a name="next-steps"></a>Další kroky

- [Hledání protokoly](log-analytics-log-searches.md) zobrazíte podrobné kontejneru záznamy.
