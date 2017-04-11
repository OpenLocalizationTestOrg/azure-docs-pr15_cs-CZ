<properties 
   pageTitle="Upozornění v protokolu analýzy | Microsoft Azure"
   description="Upozornění v protokolu analýzy identifikovat důležitých informací v úložišti OMS a můžete včasným upozorňovat na problémy nebo vyvolat akce pokoušet opravte.  Tento článek popisuje, jak vytvořit pravidlo výstrahy a podrobnosti různé kroky, které můžou používat."
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
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Upozornění v protokolu analýzy

Upozornění v protokolu analýzy popsána důležitých informací v úložišti OMS.  Upozornění pravidla automaticky pomocí protokolu vyhledávání podle plánu a vytvářet výstrahy záznam pokud výsledky podle určitého kritéria.  Pravidlo můžete automaticky spusťte jedné nebo víc akcí včasným zobrazil upozornění při upozornění nebo vyvolat jiným procesem.   

![Protokol analýzy upozornění](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Vytvoření pravidla výstrahy
Pokud chcete vytvořit pravidlo výstrahy, začněte vytvořením protokolu vyhledejte záznamy, které by měl vyvolat upozornění.  Tlačítko **upozornění** pak bude k dispozici, můžete vytvořit a nakonfigurovat pravidlo výstrahy.

1.  Na stránce Přehled OMS klikněte na **Hledat protokolu**.
2.  Vytvoří nový dotaz hledání protokolu nebo vyberte Hledat uložený protokol. 
3.  Klikněte na **upozornění** v horní části stránky otevřete obrazovce **Přidat pravidlo výstrahy** .
4. Podívejte se do tabulky pod podrobné informace o možnostech konfigurace upozornění.
5. Při zadání časového intervalu pro pravidlo výstrahy, zobrazí se počet stávajících záznamů, které splňují daná kritéria vyhledávání pro tuto časového intervalu.  Můžete určit požadovanou četnost získáte počet výsledků, které jste očekávali.
4.  Klikněte na tlačítko **Uložit** pravidlo výstrahy.  Bude spuštěn spuštěn okamžitě.


![Přidání pravidla výstrahy](media/log-analytics-alerts/add-alert-rule.png)

| Vlastnost | Popis |
|:--|:--|
| **Informace o oznámení** | |
| Jméno |  Jedinečný název pro identifikaci pravidlo výstrahy. |
| Závažnosti | Závažnosti upozornění, která se vytvoří pomocí tohoto pravidla. |
| Vyhledávání | Zaškrtněte políčko **Použít aktuální vyhledávací dotaz** a použít aktuální dotaz ze seznamu vyberte existující uložené hledání.  Syntaxe dotazu je k dispozici v textovém poli, kde jste ho upravit v případě potřeby.  |
| Časového intervalu | Určuje časový rozsah pro dotaz.  Dotaz vrátí jenom záznamy, které byly vytvořené v rozmezí od aktuálního času.  Může to být libovolná hodnota 5 minut až 24 hodin.  Musíte být větší než nebo rovno četnost oznámení.  <br><br> Například pokud časového intervalu nastavena na hodnotu 60 minut a spuštění dotazu na 1:15 PM, bude vrácena pouze záznamy o vytvořené 12:15 PM až 1:15 PM. |
| **Plán** |     
| Mezní hodnota | Kritéria pro vytvoření upozornění.  Počet záznamů vracená dotazem odpovídá tato kritéria, vytvoří se upozornění. |
| Četnost oznámení | Určuje, jak často spustí dotaz.  Může být libovolná hodnota 5 minut až 24 hodin.  Musí být rovna nebo menší než časového intervalu. |
| Potlačení upozornění | Po zapnutí potlačení upozornění pravidla akce pravidla budou zakázány definovaný dobu, po vytvoření nového upozornění.  Spuštění a vytvoří upozornění záznamy, pokud dojde ke splnění kritéria pravidla.  Toto je povolit, že čas a k odstranění problému bez spuštění duplicitní akce. |
| **Akce** | |
| E-mailového oznámení | Pokud chcete e-mailu k odeslání, když je výstraha, zadejte **Ano** . |
| Předmět    | Předmět: v e-mailu.  Do textu e-mailu, nelze změnit. |
| Příjemci | Adresy všech příjemců e-mailu.  Pokud zadáte víc adres, pak adresy oddělte středníkem (;). |
| Webhook | Pokud chcete zavolat webhook, když je výstraha, zadejte **Ano** . |
| Adresa URL Webhook | Adresa URL webhook. |
| Zahrnout vlastní datové JSON | Tuto možnost vyberte, pokud chcete nahradit datové výchozí vlastní datové části. |
| Zadejte vlastní datové části JSON | Vlastní datové webhook.  Zobrazit podrobnosti. |
| Postupu Runbook | Pokud chcete začít postupu runbook Azure automatizaci, když je výstraha, zadejte **Ano** . |
| Vyberte postupu runbook | Vyberte postupu runbook spuštění ze runbooks v automatizaci účet nakonfigurovaný ve vašem automatizaci řešení. |
| Spustit | Vyberte **Azure** spuštění postupu runbook Azure cloudu.  Vyberte **Hybridní pracovního** postupu runbook spuštění [Pracovního postupu Runbook hybridní](..\automation\automation-hybrid-runbook-worker.md) v místním prostředí. |


## <a name="manage-alert-rules"></a>Správa pravidel upozornění
Dostaňte seznam všech pravidel upozornění v nabídce **upozornění** v protokolu analýzy **Nastavení**.  

![Správa upozornění](./media/log-analytics-alerts/configure.png)

1. V konzole OMS vyberte dlaždici **Nastavení** .
2. Vyberte **upozornění**.

Provést více akcí v tomto zobrazení.

- Zakažte pravidlo tak, že vyberete **vypnout** vedle ní.
- Upravte pravidlo výstrahy, klikněte na ikonu tužky vedle ní.
- Odebrání pravidlo výstrahy po kliknutí na ikonu **X** vedle ní. 


## <a name="setting-time-windows"></a>Nastavení systému windows 

### <a name="event-alerts"></a>Výstrahy pro události

Události zahrnují zdrojům dat, jako jsou protokoly událostí Windows, Syslog, a vlastní protokoly.  Je vhodné vytvořit upozornění při získá vytvořit událost konkrétní chyby nebo při vytváření více událostí chyby během určitého časového intervalu.

Upozornění na jedné události, nastavte počet výsledků na pozdější než 0 a jak počet_plateb a časového intervalu na 5 minut.  Spustí dotaz každých 5 minut a zkontrolujte výskyt jedné události, který byl vytvořený od posledního spuštění dotazu.  Frekvenci delší může zpoždění času mezi událostí odebraná a upozornění vzniku.

Některé aplikace mohou protokolu občas chyby, které by neměly být nutně vyvolat upozornění.  Aplikace může například opakujte proces vytvořenou událost chyby a potom úspěšný při příštím.  V tomto případě nemusí chcete vytvořit upozornění nevytváříte více události během určitého časového intervalu.  

V některých případech je vhodné vytvořit oznámení při nepřítomnosti události.  Proces může například protokolovat běžná události označíte, že funguje zařízení správně.  Pokud nezaznamenává jednu z těchto události během určitého časového intervalu, je třeba vytvořit upozornění.  V tomto případě je mezní hodnota nastavíte *menší než*1.

### <a name="performance-alerts"></a>Upozornění na výkon

[Data o výkonu](log-analytics-data-sources-performance-counters.md) uložený jako záznamy v úložišti OMS podobně jako události.  Každý záznam hodnotu průměr měří přes předchozí 30 minut.  Pokud chcete upozornit při čítače výkonu překročí určitou prahovou hodnotu, pak tuto mezní měl by být součástí dotaz.

Například, pokud jste chtěli Upozornit při spuštění procesor víc než 90 % 30 minut by použití dotazu jako *Typ = výkon název_objektu = procesor název_čítače = "% času procesoru" přepočtené > 90* a prahové hodnoty pro pravidlo výstrahy *větší než 0*.  

 Protože [výsledků](log-analytics-data-sources-performance-counters.md) agregován každých 30 minut bez ohledu na to, počet_plateb shromáždit každý čítač, může vrátit časového intervalu menší než 30 minut žádné záznamy.  Nastavení časového intervalu až 30 minut zajistí získat jeden záznam pro každé připojení zdroje, představující průměr v té době.

## <a name="alert-actions"></a>Oznámení akce

Kromě vytváření upozornění záznamu, můžete nakonfigurovat pravidlo výstrahy automaticky spustit jednu nebo více akcí.  Akce můžete včasným zobrazil upozornění při upozornění nebo vyvolat některé proces pokusí k odstranění problému, který byl zjištěn.  Následující oddíly popisují akce, které jsou aktuálně k dispozici.

### <a name="email-actions"></a>Akce e-mailu
E-mailu akce Odeslat e-mail s podrobnostmi upozornění jednoho nebo více příjemců.  Zadejte předmět e-mailu, ale jeho obsah je standardní formát vytvořen podle protokolu analýzy.  Obsahuje souhrnné informace, jako je název upozornění kromě podrobnosti maximálně deseti záznamů protokolu nalezené.  Obsahuje taky odkazu můžete vyhledat protokolu v protokolu analýzy, který vrátí celou sadu záznamů z tohoto dotazu.   Je odesílatel e-mailu *týmu služeb Microsoft operace správy sadu &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook akce

Akce Webhook umožňují vyvolat externí proces až jednu žádost HTTP příspěvek.  Služba volání by měl podporují webhooks a určete, jak ho používat všechny datové obdrží.  Může taky volání rozhraní REST API, která nepodporuje konkrétně webhooks, pokud je žádost ve formátu, který rozumí rozhraní API.  Příklady použití webhook v odpovědi na upozornění na při odeslání zprávy s podrobnostmi upozornění nebo vytváření incident v služby, jako je [PagerDuty](http://pagerduty.com/)pomocí služby, jako je [časová rezerva](http://slack.com) .  

Kompletní návod vytvoříte pravidlo výstrahy s webhook volání služby vzorku je k dispozici [Webhooks v protokolu analýzy upozornění](log-analytics-alerts-webhooks.md).

Webhooks obsahovat adresy URL a datové části formátu JSON, které jsou odeslané ke službě externí data.  Ve výchozím nastavení bude obsahovat datové hodnoty v následující tabulce.  Můžete nahradit tato data vlastní prezentaci.  V takovém případě můžete proměnných v tabulce pro každý parametr vlastní datové části zahrnout jejich hodnoty.


| Parametr | Proměnná | Popis |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Název pravidla výstrahy. |
| AlertThresholdOperator | #thresholdoperator | Mezní hodnota operátor upozornění pravidla.  *Větší* nebo *menší než*. |
| AlertThresholdValue | #thresholdvalue | Mezní hodnota upozornění pravidla. |
| LinkToSearchResults | #linktosearchresults | Odkaz na protokolu analýzy protokolu vyhledávání, která vrací záznamy z dotazu pro vytvořenou upozornění. |
| ResultCount  | #searchresultcount | Počet záznamů v seznamu výsledků hledání. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Koncové datum dotazu ve formátu UTC. |
| SearchIntervalInSeconds | #searchinterval | Časového intervalu upozornění pravidla. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Spuštění dotazu ve formátu UTC. |
| SearchQuery | #searchquery | Protokol vyhledávací dotaz používaný pravidlo výstrahy. |
| SearchResults | Viz níže | Záznamy vracená dotazem ve formátu JSON.  Omezené nejprve 5 000 záznamů. |
| WorkspaceID | #workspaceid | ID OMS pracovního prostoru. |


Například můžete určit následující vlastní datové, který obsahuje jeden parametr nazývá *text*.  Služba, která volá tento webhook by očekává tento parametr.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Tento příklad datové by překládal přibližně po odeslání webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Zahrnout vlastní datové části výsledky hledání, přidejte následující řádek jako vlastnost nejvyšší úrovně v datové json.  

    "IncludeSearchResults":true

Například pokud chcete vytvořit vlastní datové obsahující pouze název upozornění a výsledky hledání, můžete použít následující. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Můžete projděte si kompletní příklad vytvoříte pravidlo výstrahy s webhook spuštění externí služby v [protokolu analýzy upozornit webhook vzorku](log-analytics-alerts-webhooks.md).

### <a name="runbook-actions"></a>Postupu Runbook akce

Akce postupu Runbook start postupu runbook v Azure automatizaci.  Abyste mohli použít tento typ akce, musíte mít [automatizaci řešení](log-analytics-add-solutions.md) nainstalovali a nakonfigurovali v pracovním prostoru OMS.  Pokud nemáte nainstalovaný při vytváření nové pravidlo výstrahy, zobrazí se odkaz na jeho nainstalovat.  Můžete vybírat z runbooks v okně automatizaci účet, které jste nakonfigurovali ve automatizaci řešení.

Akce postupu Runbook začněte pomocí [webhook](../automation/automation-webhooks.md)postupu runbook.  Při vytváření pravidla výstrahy se automaticky vytvoří nový webhook pro postupu runbook s názvem **OMS upozornění Remediation** následovaný identifikátor GUID.  

Nelze přímo naplnění všechny parametry postupu runbook, ale [$WebhookData parametrů](../automation/automation-webhooks.md) bude obsahovat podrobnosti o oznámení, včetně výsledků hledání protokol, který byl vytvořen.  Postupu runbook muset definovat **$WebhookData** jako parametr ho pro přístup k vlastnosti na upozornění.  Upozornění data je k dispozici ve formátu json v jedné vlastnosti s názvem **SearchResults** ve vlastnosti **RequestBody** **$WebhookData**.  To bude mít s vlastnostmi v následující tabulce.


| Uzel | Popis |
|:--|:--|
| ID         | Cesta a GUID hledání. |
| __metadata | Informace o upozornění, včetně počtu zobrazených záznamů a stavu výsledků hledání. |
|  Hodnota     |  Samostatnou položku pro každý záznam ve výsledcích hledání.  Podrobnosti o položka se budou shodovat vlastnosti a hodnoty záznamu.   |

Například následující postupu runbook by extrahovat záznamů protokolu nalezené a přiřadit různé vlastností závislosti na použitém typu každý záznam.  Všimněte si, že postupu runbook spustí převodem **RequestBody** z json, takže je možné pracovat s jako objekt v prostředí PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Upozornění záznamů

Upozornění záznamy vytvořil upozornění pravidel v protokolu analýzy obsahují **typu** **Upozornění** a **SourceSystem** **OMS**.  V následující tabulce mají vlastnosti.

| Vlastnost | Popis |
|:--|:--|
| Typ          | *Upozornit* |
| SourceSystem  | *OMS* |
| AlertSeverity | Úroveň závažnosti upozornění. |
| AlertName     | Název upozornění. |
| Dotaz         | Text dotazu, který byl spuštěn.  |
| QueryExecutionEndTime   | Konec časový rozsah pro dotaz. |
| QueryExecutionStartTime | Začátek časového rozsahu dotazu.  |
| TimeGenerated | Datum a čas vytvoření upozornění. |

Existují jiné druhy upozornění záznamy jsou vytvářeny [řešení pro správu upozornění](log-analytics-solution-alert-management.md) a [Exportuje Power BI](log-analytics-powerbi.md).  Tyto všechny mít **Typ** **upozornění** , ale jsou odlišeny jejich **SourceSystem**.




## <a name="next-steps"></a>Další kroky

- Nainstalujte [řešení upozornění správy](log-analytics-solution-alert-management.md) a analyzujte data upozornění vytvořené v protokolu analýzy spolu s upozornění shromážděné z System Center operace správce (SCOM).
- Další informace o [protokolu hledání](log-analytics-log-searches.md) , která můžou vytvářet výstrahy.
- Dokončete postup při [konfiguraci webook](log-analytics-alerts-webhooks.md) pomocí pravidla výstrahy.  
- Zjistěte, jak psát [runbooks v Azure automatizaci](https://azure.microsoft.com/documentation/services/automation) k nápravě problémy označenými upozornění.