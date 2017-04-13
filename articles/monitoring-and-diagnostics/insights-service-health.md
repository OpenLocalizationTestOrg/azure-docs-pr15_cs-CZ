<properties
    pageTitle="Sledovat stav služby Azure pomocí Azure Monitor aktivity protokoly | Microsoft Azure"
    description="Zjistíte, kdy došlo Azure výkonu snížení úrovně nebo službu vyrušování. "
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

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Sledovat stav služby Azure pomocí Azure Monitor aktivity protokoly

Azure publicizes pokaždé, když je přerušení nebo výkonu snížení úrovně služeb. Můžete procházet tyto události na portálu Azure a taky můžete [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) pro přístup k úplné sady události programově.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Procházet protokoly stavu služby pro vaše předplatné

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).

2. Na **Domů** byste měli vidět dlaždice s názvem **Stav služby**. Klikněte na ni.

    ![Domů](./media/insights-service-health/Insights_Home.png)

3. Zobrazí se seznam všech oblastí v Azure. Klikněte na všechny oblasti vyvoláte protokolu činnosti dotaz, který ukazuje servisním incidentem, které mají vliv na některou z předplatných za posledních 24 hodin.

    ![Stav služby předplatného protokolu aktivity](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Můžete zobrazit podrobnosti o incident jednotlivé po kliknutí na dané události v tabulce.

5. Změna **časového rozpětí** zobrazíte delší časový rámec.

## <a name="next-steps"></a>Další kroky

* [Dostupnost monitor a rychlostí reakce webové stránky](../application-insights/app-insights-monitor-web-app-availability.md) s přehledy aplikace, kterou můžete zjistit, zda stránku je vypnutý.
