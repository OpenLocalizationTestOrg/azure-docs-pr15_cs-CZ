<properties
    pageTitle="Vytvoření upozornění pro služby Azure pomocí prostředí PowerShell | Microsoft Azure"
    description="Použití Powershellu ke vytvořit Azure upozornění, které mohou být příčinou oznámení nebo automatizaci Pokud jsou splněny zadané podmínky."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Vytvoření upozornění pro služby Azure pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak nastavit Azure upozornění pomocí Powershellu.  

Obdržíte upozornění na základě sledování metriky pro nebo události na služby Azure.

- **Metriky hodnoty** – upozornění aktivační události při hodnota zadaná metriky protíná prahové hodnoty, které můžete přiřadit v obou směrech. To znamená, spustí jak kdy nejdřív splnění podmínky a pak navíc při, podmínka už probíhá došlo ke splnění.    
- **Aktivity události protokolu** – upozornění můžete spustit na *všech* událostí nebo, pouze v případě, že určitý počet události dojít.

Můžete nakonfigurovat upozornění při aktivaci následujícím způsobem:

- odeslání e-mailová oznámení správce služby a dalších správců
- odeslání e-mailu k další e-mailů, které zadáte.
- volání webhook
- spuštění Azure postupu runbook (pouze z portálu Microsoft Azure)

Konfigurace a získat informace o oznámení pomocí pravidla

- [Azure portálu](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [rozhraní příkazového řádku (rozhraní příkazového řádku)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Další informace, vždy napsat ```get-help``` a potom příkazu Powershellu chcete zobrazit nápovědu.

## <a name="create-alert-rules-in-powershell"></a>Vytváření pravidel výstrah v prostředí PowerShell

1. Přihlaste se k Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Zobrazte seznam z předplatných máte k dispozici. Ověřte, že pracujete s správné předplatného. Pokud ne, nastavte ten správný pomocí výstup `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Seznam existující pravidla pro skupinu zdrojů, použijte tento příkaz:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Pokud chcete vytvořit pravidlo, musíte mít několik důležité údaje nejdřív. 
    - **Pole číslo ID zdroje** pro zdroj, který chcete nastavit upozornění pro
    - **Metriky definice** k dispozici pro daný zdroj

    Je možné získat číslo ID zdroje je používat portál Azure. Za předpokladu, že už vytvořili zdroje, vyberte na portálu. V dalším zásuvné vyberte příkaz *Vlastnosti* v části *Nastavení* . Číslo ID zdroje je pole v dalším zásuvné. Dalším způsobem je pomocí [Průzkumníka Azure zdroje](https://resources.azure.com/).

    Se příklad pole číslo id zdroje pro web app

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Můžete použít `Get-AzureRmMetricDefinition` zobrazíte seznam všech metrické definic určitého zdroje.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Následující příklad vytvoří tabulku s metrické název a jednotky pro tuto metrické.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Úplný seznam dostupných možností pro získání AzureRmMetricDefinition neexistuje spuštěním Get-MetricDefinitions.


5. Následující příklad nastaví upozornění na zdroj webu. Upozornění aktivace kdykoli konzistentní přijímá všechny přenosy pro 5 minut a znovu při přijetí bez přenosy 5 minut.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Pokud chcete vytvořit webhook nebo odeslání e-mailu při aktivaci upozornění, je nutné nejprve vytvořte e-mailu a/nebo webhooks. Okamžitě vytvořte pravidlo navíc se značkou - akce a jak je uvedeno v následujícím příkladu. Nelze propojit webhook nebo e-mailů s vytvořen pravidla prostřednictvím Powershellu.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Chcete-li vytvořit upozornění, která aktivuje na určitou podmínku v protokolu činnosti, použijte příkazy následující formulář

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    Název operace - odpovídá typu události v protokolu činnosti. Jako příklad lze uvést *Microsoft.Compute/virtualMachines/delete* a *microsoft.insights/diagnosticSettings/write*.

    Použití příkazu Powershellu [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) získat seznam možných operationNames. Případně můžete portálu Azure dotaz protokolu činnosti a vyhledání určitých dřívější operacích, které chcete vytvořit upozornění pro. Operace v zobrazení obrázku protokolu popisných názvů. Najděte ve formátu JSON položky a vysunout hodnoty název operace.   

8. Ověřte, že nastavení upozornění vytvořil správně prohlížíte jednotlivá pravidla.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Odstraňte nastavení upozornění. Tyto příkazy odstraňte pravidla vytvořili dříve v tomto článku.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Další kroky

* [Získejte přehled o sledování Azure](monitoring-overview.md) včetně typy informací, můžete shromažďovat a sledovat.
* Další informace o [konfiguraci webhooks v upozornění](insights-webhooks-alerts.md).
* Další informace o [Runbooks automatizaci Azure](..\automation\automation-starting-a-runbook.md).
* Získáte [základní informace o shromažďování protokoly pro diagnostiku](monitoring-overview-of-diagnostic-logs.md) shromažďovat podrobné metriky vysokou četnost na služby.
* Získáte [základní informace o metriky kolekce](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
