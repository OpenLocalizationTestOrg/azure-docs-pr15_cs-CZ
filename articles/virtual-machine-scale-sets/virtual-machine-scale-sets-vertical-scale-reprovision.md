<properties
    pageTitle="Svisle měřítko Azure virtuálního počítače měřítko sady | Microsoft Azure"
    description="Jak svisle měřítko virtuálního počítače v odpovědi na sledování oznámení s automatizaci Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Nastaví automatické svislé měřítko s měřítkem virtuálního počítače

Tento článek popisuje, jak zobrazit svisle Azure [Sady měřítko virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/) pomocí hesla nebo bez reprovisioning. Svislé měřítko VMs, které nejsou v sadě měřítko, najdete [ve svislém směru měřítko Azure virtuálního počítače s Azure automatizaci](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Svislé měřítko, nazývaný také _škálování_ a _Neomezovat_znamená, že zvětšení nebo zmenšení velikosti virtuálního počítače (OM) v odpovědi úlohu. Porovnejte toto s [vodorovnou měřítko](./virtual-machine-scale-sets-autoscale-overview.md), bývá označovaná taky jako _Měřítko_ a _velkém měřítku v_, kde počet VMs měnit v závislosti na pracovní zátěž.

Reprovisioning znamená, že odstranění existující OM a nahrazení novou mapou. Když zvětšit nebo zmenšit velikost VMs v sadě měřítko OM, v některých případech chcete změnit velikost existující VMs a zachovat data, zatímco v ostatních případech je třeba nasadit nové VMs novou velikost. Tento dokument obsahuje obou případech.

Změna měřítka svislé může být užitečné při:

- Služba založená na virtuálních počítačích je v části využít (například na víkendy). Zmenšení velikosti OM můžete snížit náklady na měsíční.
- Zvětšení velikosti OM čelit větší služba nevytvářet další VMs.

Můžete nastavit svislé měřítko jako vyžádané na základě metrických výstrahy ze sady OM měřítko. Upozornění při aktivaci aktivuje webhook této aktivačních událostí, které postupu runbook, které můžete změnit velikost vaší měřítko nastavit nahoru nebo dolů. Změna měřítka svislé lze nastavit pomocí následujících kroků:

1. Vytvořte účet Azure automatizaci s možností spuštění jako.
2. Naimportujte runbooks Azure automatizaci svislé měřítko pro OM měřítko sady předplatného.
3. Přidejte webhook vaše postupu runbook.
4. Dodejte OM měřítko sady pomocí oznámení webhook upozornění.

> [AZURE.NOTE] Svislý neobsahovaly text lze provést pouze v určitém rozsahu OM velikostí. Porovnání specifikace každou velikost před rozhodnutím o měřítko mezi sebou (vyšší čísla vždy neznamená OM většími). Můžete zobrazit mezi následující páry velikosti:

>| Změna měřítka pár velikosti OM |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Vytvořte účet Azure automatizaci s možností spuštění jako

První věc, kterou musíte udělat je vytvořit účet Azure automatizaci, který bude hostitelem runbooks slouží k velikosti nastavte měřítko OM instancí. [Automatizace Azure](https://azure.microsoft.com/services/automation/) naposledy zavádí funkci "Spustit jako účet", která umožňuje nastavení nahoru jistinu služby pro automatické spuštění runbooks jménem uživatele velmi snadno. Další informace v níže uvedených článků:

* [Ověření Runbooks s Azure spustit jako účet](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Import Azure automatizaci svislé měřítko runbooks do vašeho předplatného

Runbooks potřeba svisle zobrazit sady měřítko OM jsou publikované v galerii Azure automatizaci postupu Runbook. Import do vašeho předplatného postupujte podle pokynů v tomto článku:

* [Galerie postupu Runbook a modulu pro automatizaci Azure](../automation/automation-runbook-gallery.md)

Zvolte možnost galerie procházet z nabídky Runbooks:

![Runbooks k importu][runbooks]

Zobrazují se runbooks, které mají být importovány. Vyberte postupu runbook založené na tom, jestli chcete, aby svislé měřítko pomocí hesla nebo bez reprovisioning:

![Galerie Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Přidat webhook do svého postupu runbook

Po importu runbooks, které budete muset přidat webhook postupu runbook tak můžete spouštěný kliknutím upozornění z OM měřítko sady. Podrobnosti o vytváření webhook pro vaše postupu Runbook jsou popsané v tomto článku:

* [Azure webhooks automatizaci](../automation/automation-webhooks.md)

> [AZURE.NOTE] Zkontrolujte, že zkopírujete webhook URI před zavřením okna webhook jako budete potřebovat v další části.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Přidání upozornění nastavit měřítko OM

Níže je skript Powershellu, který ukazuje, jak přidat upozornění nastavovaná měřítko OM. Podívejte se na následující článek zjištění názvu metriky aktivováno upozornění na: [běžné metriky Azure Monitor neobsahovaly text](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Doporučuje se konfigurace rozumné časového intervalu pro upozornění a zabránit tak spouštění svislé měřítko a všechny přidružené přerušení poskytování služeb, příliš často. Zvažte okno nejméně nejnižších 20 až 30 minut. Zvažte vodorovné měřítka v případě potřeby zabránit přerušení.

Další informace o tom, jak vytvořit upozornění naleznete v následujících článcích:

* [Azure Monitor Powershellu úvodní vzorky](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure ukázky úvodní Monitor a platformy rozhraní příkazového řádku](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Souhrn

Tento článek ukázal jednoduché svislé měřítka příklady. Pomocí těchto stavebních bloků – automatizaci účtu, runbooks, webhooks, upozornění – můžete se připojit bohaté řadu událostí s vlastní sady akcí.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
