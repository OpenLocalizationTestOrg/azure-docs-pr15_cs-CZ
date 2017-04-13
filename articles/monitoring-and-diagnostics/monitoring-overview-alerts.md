<properties
    pageTitle="Přehled upozornění v Microsoft Azure | Microsoft Azure"
    description="Upozornění umožňují sledovat metriky Azure zdroje, událostí nebo protokoly a upozorněni splněny zadané podmínky."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Přehled upozornění v Microsoft Azure


Tento článek popisuje co upozornění se jejich výhody a jak začít s jejich použití.  

## <a name="what-are-alerts"></a>Co jsou oznámení?
Upozornění byla způsob, jak sledování metriky Azure zdroje, událostí, nebo protokoly a potom oznámení při podmínka zadaná došlo ke splnění.

Můžete jim zobrazovat upozornění na základě:

- **Hodnoty metriky**: Toto oznámení spustí, když hodnota zadaná metriky protíná prahové hodnoty, kterou jste přiřadili v obou směrech. To znamená, spustí jak při prvním splnění podmínky a pak později při podmínky, které už je došlo ke splnění.
- **Aktivity protokolu událostí**: Toto oznámení můžete spustit na všech událostí nebo jenom určitý počet události dojít.

## <a name="alerts-in-different-azure-services"></a>Upozornění v různých službách Azure

Upozornění jsou k dispozici v různých službách, včetně:

- **Přehledy aplikace**: umožňuje webu test a metrické upozornění. V tématu [Nastavení upozornění v aplikaci přehledy](../application-insights/app-insights-alerts.md) a [sledování dostupnosti a citlivosti jakýkoli web](../application-insights/app-insights-monitor-web-app-availability.md).
- **Protokol analýzy (operace správy sadu)**: Povoluje směrování diagnostické protokoly, aby protokolu analýzy. Operace správy sadu umožňuje metrické protokolu a ostatní typy oznámení. Další informace najdete v tématu [upozornění v protokolu analýzy](../log-analytics/log-analytics-alerts.md).  
- **Sledování Azure**: umožňuje upozornění na základě metrické hodnoty a aktivity události protokolu. Azure Monitor obsahuje [Rozhraní REST API Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx).  Další informace najdete v tématu [použití portálu Azure, Powershellu nebo rozhraní příkazového řádku pro vytvoření upozornění](insights-alerts-portal.md).

## <a name="alert-actions"></a>Oznámení akce
Můžete nakonfigurovat upozornění, postupujte takto:

- Odeslání e-mailová oznámení má správce služby, dalších správců nebo další e-mailové adresy, které zadáte.
- Zavolejte na linku webhook, která umožňuje spuštění automatizaci další akce.

 ![Vysvětlení upozornění](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Další kroky

Získáte informace o pravidlech výstrah a konfiguraci pomocí:

- [Azure portálu](insights-alerts-portal.md)
- [Prostředí PowerShell](insights-alerts-powershell.md)
- [Rozhraní příkazového řádku (rozhraní příkazového řádku)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
