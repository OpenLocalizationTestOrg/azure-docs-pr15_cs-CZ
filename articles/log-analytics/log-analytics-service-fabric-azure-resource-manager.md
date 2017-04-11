<properties
    pageTitle="Optimalizace vaše prostředí řešení služby struktury v protokolu analýzy | Microsoft Azure"
    description="Řešení služby struktury můžete použít k posouzení rizika a stavu služby struktury aplikace, micro služeb, uzly a clusterů."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Řešení struktury služby v protokolu analýzy

> [AZURE.SELECTOR]
- [Správce prostředků](log-analytics-service-fabric-azure-resource-manager.md)
- [Prostředí PowerShell](log-analytics-service-fabric.md)

Tento článek popisuje, jak pomocí služby struktury řešení v protokolu analýzy identifikovat a Poradce při potížích s přes svůj cluster struktury služby.

Řešení služby struktury používá Azure diagnostiky data ze struktury vaše služby VMs tak, že shromažďování dat z Azure WAD tabulkami. Protokol analýzy potom čte služby struktury framework události, včetně **Spolehlivé události služby**, **Actor**, **Provozní události**a **událostech vlastní trasování událostí pro Windows**. S řídicím panelu řešení se ve vašem prostředí služby struktury zobrazit důležitá problémy a důležité události.

Začít s řešení, budou muset připojit svůj cluster struktury služby do pracovního prostoru protokolu analýzy. Tady jsou tři scénářů k zvážení:

1. Nasazený svůj cluster služby struktury postupem ***nasazení clusteru struktury služby připojení do pracovního prostoru protokolu analýzy*** nasadit nového obrázku a nakonfigurovanou sestava protokolu analýzy.

2. Pokud potřebujete výkonnosti shromažďovat hostitelů pro použití dalších řešení OMS například zabezpečení na svůj Cluster struktury služby, postupujte podle kroků v ***nasazení clusteru struktury služby připojení do pracovního prostoru OMS s příponou OM nainstalovaný.***

3. Pokud jste již nainstalovali clusteru služby struktury a chcete připojit k protokolu analýzy, postupujte podle kroků v tématu ***Přidání existujícího účtu úložiště do protokolu Analytics.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Nasazení clusteru struktury služby připojení do pracovního prostoru protokolu analýzy.
Tato šablona dělá toto:


1. Nasadí clusteru služby Azure služby struktury připojeni do pracovního prostoru protokolu analýzy. Máte možnost vytvořit nový pracovní prostor při nasazení šablony, nebo zadat název existujícího pracovního prostoru protokolu analýzy.
2. Přidá diagnostiky úložiště účet do pracovního prostoru protokolu analýzy.
3. Umožňuje řešení služby struktury v pracovním prostoru protokolu analýzy.

[![Nasazení Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Jakmile vyberete tlačítko nasazení, budou přicházet na portálu Azure s parametry pro úpravy. Je potřeba k vytvoření nové skupiny prostředků, pokud je zadat nový název pracovního prostoru protokolu analýzy: ![struktury služby](./media/log-analytics-service-fabric/2.png)

![Služba struktury](./media/log-analytics-service-fabric/3.png)

Přijměte podmínky pro právnické a klepněte na možnost "Vytvořit" spusťte nasazení. Po dokončení nasazení byste měli vidět nový pracovní prostor a clusteru vytvořili a WADServiceFabric * událost, WADWindowsEventLogs a WADETWEvent tabulkami přidali:

![Služba struktury](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Nasazení clusteru struktury služby připojení do prostoru OMS s příponou OM nainstalovaný.
Tato šablona dělá toto:

1. Nasadí clusteru služby Azure služby struktury připojeni do pracovního prostoru protokolu analýzy. Můžete vytvořit nový pracovní prostor nebo použít existující úrovně.
2. Účty diagnostiky úložiště přidáte do pracovního prostoru protokolu analýzy.
3. Umožňuje řešení služby struktury v pracovním prostoru protokolu analýzy.
4. Nainstaluje rozšíření agenta MMA v jednotlivých OM měřítko nastavení v svůj cluster struktury služby. Instalaci agenta MMA se zobrazíte měřítka o uzly.


[![Nasazení Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Po stejné výše uvedených kroků potřebných parametrů při zadávání a zahájit nasazení. Ještě jednou byste měli vidět nový pracovní prostor, clusteru a všech vytvořený WAD tabulkami:

![Služba struktury](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Zobrazení Data o výkonu

Zobrazení dat výkonu z uzly:
</br>
- Spuštění analýzy protokolu pracovního prostoru z portálu Microsoft Azure.

![Služba struktury](./media/log-analytics-service-fabric/6.png)

- Přejděte na nastavení v levém podokně a vyberte Data >> Windows výkonnosti >> "Přidat vybrané výkonnosti": ![struktury služby](./media/log-analytics-service-fabric/7.png)

- Do pole hledání protokolu delve do klíčových metrik o uzly pomocí následující dotazů:
</br>

    na. Porovnání průměr využití procesoru na všech uzlech v poslední hodinu zobrazíte uzlů, které jsou potíže a na jaké časový interval uzel využili zásobníku:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Služba struktury](./media/log-analytics-service-fabric/10.png)


    b. Zobrazení podobné spojnicové grafy pro dostupnou pamětí v jednotlivých uzlech s Tento dotaz:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Chcete-li zobrazit seznam všech uzlech, zobrazující přesné průměrnou hodnotu pro počet MB k dispozici pro každý uzel, použijte tento dotaz:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Služba struktury](./media/log-analytics-service-fabric/11.png)


    c. V případě, že chcete přecházet na podrobnější určitého uzlu porovnáním hodinové průměr, minimální, maximální Statistika a percentily 75 využití procesoru budete moct pomocí tohoto dotazu (nahradit pole počítače):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Služba struktury](./media/log-analytics-service-fabric/12.png)

    Přečtěte si další informace o výkonu metriky v protokolu technologie pro analýzu [tady]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Přidání existujícího účtu úložiště do protokolu analýzy

Tato šablona jednoduše přidá stávajících účtů úložiště do nového nebo existujícího protokolu analýzy pracovního prostoru.
</br>

[![Nasazení Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] Při výběru skupinu zdrojů, vyberte Pokud pracujete s již existujícího pracovního prostoru protokolu analýzy, "Použít existující" a vyhledejte skupinu prostředků obsahující OMS pracovního prostoru. Vytvoření nového jeden if jinak.
![Služba struktury](./media/log-analytics-service-fabric/8.png)

Po nasazení šabloně budete moct zobrazit úložiště účtů připojených k protokolu analýzy pracovního prostoru. V této instanci přidám do pracovního prostoru Exchange, který jsem vytvořil(a) nad více účtů úložiště.
![Služba struktury](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Zobrazit služby struktury události

Po dokončení nasazením a povolil řešení služby struktury v pracovním prostoru, vyberte dlaždici **Struktury služby** v portálu protokolu Analytics pro spuštění řídicího panelu služeb struktury. Řídicí panel obsahuje sloupce v následující tabulce. Každý sloupec oblasti seznamy podle tohoto sloupce kritéria pro zadaný časový rozsah přiřazování počet horních deseti události. Můžete spustit hledání protokolu poskytující celého seznamu kliknutím na **Zobrazit všechny** v pravé dolní části každý sloupec oblasti nebo kliknutím na záhlaví sloupce.

| **Služba struktury události** | **Popis** |
| --- | --- |
| Důležitá problémy | Zobrazení problémů například RunAsyncFailures RunAsynCancellations a uzel nabídky. |
| Funkční události | Důležitá provozní události, jako je třeba upgradovat aplikaci a nasazení. |
| Spolehlivé služby události | Důležitá spolehlivé služby události těchto Runasyncinvocations. |
| Objekt actor události | Důležitá actor události generované micro služby, například výjimky vyvolané metodu actor, actor aktivací a deaktivací a tak dál. |
| Události aplikace | Všechny vlastní trasování událostí pro Windows události generované aplikace. |

![Řídicí panel služeb struktury](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel služeb struktury](./media/log-analytics-service-fabric/sf4.png)


V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje pro službu struktury.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ne](./media/log-analytics-malware/oms-bullet-red.png)|![Ne](./media/log-analytics-malware/oms-bullet-red.png)| ![Ano](./media/log-analytics-malware/oms-bullet-green.png)|            ![Ne](./media/log-analytics-malware/oms-bullet-red.png)|![Ne](./media/log-analytics-malware/oms-bullet-red.png)|10 minut |


>[AZURE.NOTE] Změna oboru těchto událostí v řešení služby struktury kliknutím na **Data podle posledních 7 dnů** v horní části řídicího panelu. Můžete taky zobrazit události generované během posledních 7 dnů, 1 den nebo 6 hodin. Nebo můžete zvolit **vlastní** chcete zadat rozsah vlastní datum.


## <a name="next-steps"></a>Další kroky

- Pomocí [Protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data události struktury služby.
