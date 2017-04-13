<properties
    pageTitle="Umožňuje vytvořit upozornění pro Azure služby Azure portál | Microsoft Azure"
    description="Pomocí portálu Azure vytvořit Azure upozornění, které mohou být příčinou oznámení nebo automatizaci Pokud jsou splněny zadané podmínky."
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Umožňuje vytvořit upozornění pro Azure služby Azure portálu

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak nastavit Azure upozornění na Azure portálu.   

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


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Vytvoření pravidla výstrahy na metrické pomocí portálu Azure

1. Na [portálu](https://portal.azure.com/)vyhledejte zdroje zajímají sledování a vyberte ho.

2. Vyberte **upozornění** nebo **oznámení pravidla** v části sledování. Text a ikony pro různé zdroje trochu lišit.  

    ![Sledování](./media/insights-alerts-portal/AlertRulesButton.png)


3. Vyberte příkaz **Přidat upozornění** a do polí.

    ![Přidání oznámení](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Název** oznámení pravidlo a zvolte, **Popis**, který se zobrazí také oznámení e-mailů.
5. Vyberte **míru** , chcete-li sledovat a pak vyberte **podmínku** a **mezní** hodnota metriky. Také se rozhodnout, **období** , která metrických pravidla musí být splněny před upozornění aktivačních událostí. Tak například pokud používáte období "PT5M" a oznámení vyhledá procesoru vyšší než 80 %, spustí upozornění, když procesoru byl konzistentní výše 80 % 5 minut. Když dojde k první aktivační událost, ji znovu spustí, když procesoru zůstane pod 80 % 5 minut. Měření procesoru probíhá každou 1 minutu.   

6. **E-mailu vlastníci...** zaškrtněte, pokud chcete správci a dalších správců mailem při upozornění.

7. Pokud budete potřebovat další e-mailové zprávy zobrazilo oznámení, pokud se aktivuje, oznámení, přidejte je do pole **email(s) další správce** . Oddělte středníky - víc e-mailů*email@contoso.com;email2@contoso.com*

8. Umístěte platný identifikátor URI v poli **Webhook** požadovaná říká při upozornění.

9. Pokud používáte automatické Azure, můžete vybrat postupu Runbook proběhnout při upozornění.

10. Kliknutím na **OK** po dokončení vytvořit upozornění.   

Upozornění objevit během pár minut, je aktivní a spustí jak bylo popsáno dříve.

## <a name="managing-your-alerts"></a>Správa nastavení upozornění

Po vytvoření upozornění můžete vybrat ho a:

- Zobrazte graf zobrazující metrické mezní hodnota a skutečné hodnoty z předchozí den.
- Upravit nebo odstranit.
- **Zakázání** nebo **Povolení** ho Pokud chcete dočasně zastavit nebo životopis doručování oznámení pro oznámení.



## <a name="next-steps"></a>Další kroky

* [Získejte přehled o sledování Azure](monitoring-overview.md) včetně typy informací, můžete shromažďovat a sledovat.
* Další informace o [konfiguraci webhooks v upozornění](insights-webhooks-alerts.md).
* Další informace o [Runbooks automatizaci Azure](..\automation\automation-starting-a-runbook.md).
* Vytvořit [Přehled protokoly pro diagnostiku](monitoring-overview-of-diagnostic-logs.md) a Automatický sběr podrobné metriky vysokou četnost na služby.
* Získáte [základní informace o metriky kolekce](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
