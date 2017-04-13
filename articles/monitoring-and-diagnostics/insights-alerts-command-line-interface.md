<properties
    pageTitle="Vytvoření upozornění pro služby Azure pomocí různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku) | Microsoft Azure"
    description="Umožňuje vytvořit Azure upozornění, které mohou být příčinou oznámení nebo automatizaci Pokud jsou splněny zadané podmínky rozhraní příkazového řádku."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Vytvoření upozornění pro služby Azure pomocí různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku)

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak nastavit Azure upozornění pomocí rozhraní příkazového rozhraní příkazového řádku (řádku).

>[AZURE.NOTE] Azure Monitor je nový název pro co označovalo jako "Azure přehledy" až do 25 Sept 2016. Však obory názvů a tím následující příkazy stále obsahovat "přehledy".

Obdržíte upozornění na základě sledování metriky pro nebo události na služby Azure.

- **Metrické hodnoty** – upozornění aktivačních událostí, když hodnota zadaná metrické ve výkresu protínají mezní hodnoty, které můžete přiřadit v obou směrech. To znamená, spustí jak kdy nejdřív splnění podmínky a pak navíc při, podmínka už probíhá došlo ke splnění.    
- **Aktivity události protokolu** – upozornění můžete spustit na *všech* událostí nebo, pouze v případě, že určitý počet události dojít.

Můžete nakonfigurovat upozornění při aktivaci následujícím způsobem:

- odeslání e-mailová oznámení správce služby a dalších správců
- odeslání e-mailu k další e-mailů, které zadáte.
- volání webhook
- spuštění Azure postupu runbook (pouze z Azure portál v současné době)

Konfigurace a získat informace o oznámení pomocí pravidla

- [Azure portálu](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [rozhraní příkazového řádku (rozhraní příkazového řádku)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Vždy získáte nápovědu k příkazům zadáním příkazu a - Nápověda na konci. Příklad:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Vytváření pravidel výstrah pomocí rozhraní příkazového řádku

1. Umožňuje proveďte požadavky a přihlaste se k Azure. Prohlédněte [Azure Monitor rozhraní příkazového řádku](insights-cli-samples.md). Stručně řečeno nainstalujte rozhraní příkazového řádku a spusťte tyto příkazy. Vám přihlášeni, zobrazit jaké předplatné a připravit se spusťte Azure Monitor příkazy.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Existující pravidla pro skupinu zdrojů, použijte následující formulář **azure přehledy, že upozornění pravidlo seznamu** *[možnosti] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Pokud chcete vytvořit pravidlo, musíte mít několik důležité údaje nejdřív.
    - **Pole číslo ID zdroje** pro zdroj, který chcete nastavit upozornění pro
    - **Metrické definice** k dispozici pro daný zdroj

    Je možné získat číslo ID zdroje je používat portál Azure. Za předpokladu, že už vytvořili zdroje, vyberte na portálu. V dalším zásuvné vyberte příkaz *Vlastnosti* v části *Nastavení* . *Pole číslo ID zdroje* je pole v dalším zásuvné. Dalším způsobem je pomocí [Průzkumníka Azure zdroje](https://resources.azure.com/).

    Se příklad pole číslo id zdroje pro web app

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Pokud chcete získat seznam dostupných metriky a jednotky pro tyto metriky předchozí příklad zdrojů, zadejte následující příkaz rozhraní příkazového řádku:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ je k dispozici měření (1-minute intervalů). Použití různých granularity poskytuje metrických různé možnosti.


4. Chcete-li vytvořit pravidlo založené na míru výstrahy, použijte příkaz následující formulář:

    **Azure přehledy upozornění pravidlo míru nastavení** *[možnosti] &lt;Název_pravidla&gt; &lt;umístění&gt; &lt;resourceGroup&gt; &lt;velikost_okna&gt; &lt;operátor&gt; &lt;prahové hodnoty&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Následující příklad nastaví upozornění na zdroj webu. Upozornění aktivace kdykoli konzistentní přijímá všechny přenosy pro 5 minut a znovu při přijetí bez přenosy 5 minut.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Pokud chcete vytvořit webhook nebo odeslání e-mailu, když se aktivuje upozornění, je nutné nejprve vytvořte e-mailu a/nebo webhooks. Vytvořte pravidlo okamžitě později. Nelze propojit webhook nebo e-mailů s vytvořen pravidla pomocí rozhraní příkazového řádku.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Pokud chcete vytvořit upozornění, že se aktivuje, na určité podmínky v protokolu činnosti, formulář:

    **přehledy upozornění pravidlo protokolování sady** *[možnosti] &lt;Název_pravidla&gt; &lt;umístění&gt; &lt;resourceGroup&gt; &lt;název operace&gt; *

    Příklad

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Název operace odpovídá typu událostí pro příslušnou položku v protokolu činnosti. Jako příklad lze uvést *Microsoft.Compute/virtualMachines/delete* a *microsoft.insights/diagnosticSettings/write*.

    Použití příkazu Powershellu [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) získat seznam možných operationNames. Případně můžete portálu Azure dotazu protokol aktivity a najít konkrétní dřívější operacích, které chcete vytvořit upozornění pro. Operace v zobrazení obrázku protokolu popisných názvů. Najděte ve formátu JSON položky a vysunout hodnoty název operace.   

7. Můžete ověřit, že upozornění vytvořil správně prohlížíte jednotlivá pravidla.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Pokud chcete odstranit pravidla, použijte příkaz formuláře:

    **Odstranění pravidla přehledy upozornění** možnosti] &lt;resourceGroup&gt; &lt;Název_pravidla&gt;

    Tyto příkazy odstraňte pravidla dřív vytvořili v tomto článku.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Další kroky

* [Získejte přehled o sledování Azure](monitoring-overview.md) včetně typy informací, můžete shromažďovat a sledovat.
* Další informace o [konfiguraci webhooks v upozornění](insights-webhooks-alerts.md).
* Další informace o [Runbooks automatizaci Azure](..\automation\automation-starting-a-runbook.md).
* Získáte [základní informace o shromažďování protokoly pro diagnostiku](monitoring-overview-of-diagnostic-logs.md) shromažďovat podrobné metriky vysokou četnost na služby.
* Získáte [základní informace o metriky kolekce](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
