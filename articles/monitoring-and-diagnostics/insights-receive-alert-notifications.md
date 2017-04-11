<properties
    pageTitle="Přijímání oznámení služby Azure | Microsoft Azure"
    description="Potvrzení o pravidla výstrah podmínky."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Přijímání oznámení

Obdržíte upozornění na základě sledování metriky pro nebo události na služby Azure.

Pravidlo výstrahy na hodnotu metriky když hodnota zadaná metriky ve výkresu protínají mezní přiřazen, pravidlo výstrahy aktivuje a můžete odeslat oznámení. Pravidlo oznámení o událostech pravidlo můžete odeslat oznámení na *všech* událostí nebo, pouze v případě, že určitý počet události dojít.

Když vytvoříte pravidlo výstrahy, můžete vybírat možnosti Odeslat e-mailové oznámení, Správce služeb a dalších správců nebo jiného správce, které můžete zadat. Při aktivaci pravidla a upozornění podmínka vyřeší se odešle e-mail oznámení.

[Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) nebo [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) slouží ke konfiguraci a získat informace o pravidlech výstrah programově.

## <a name="create-an-alert-rule"></a>Vytvoření pravidla výstrahy

1. Na [portálu](https://portal.azure.com/)klikněte na **Procházet**a pak zdroje máte zájem sledování.

2. Klikněte na dlaždici **pravidla výstrah** ve lens **operace** .

3. Klikněte na příkaz **Přidat upozornění** .

    ![Přidání oznámení](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Název pravidla výstrahy a zvolte popis, který se zobrazí v e-mailových oznámení.

5. Když vyberete **metriky** zvolím podmínku a mezní hodnota metriky. Toto je po dobu používající Azure ke sledování a zobrazované upozornění aktivity.

    ![Podmínky a prahové hodnoty](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Můžete taky zvolit **události**a najděte oznámení důvody chování určitých událost nastavit jako.

    ![Události](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Nakonec můžete poslat e-mailového oznámení zodpovědné správcům.

Po klepnutí na tlačítko **Uložit**objevit během pár minut si budete moct informovaní kdykoli míru, které zvolíte větší než mezní hodnota.

## <a name="managing-your-alert-rules"></a>Správa pravidel upozornění

Po vytvoření pravidla výstrahy, můžete zobrazit náhled oznámení mezní porovnání míru z předchozí den.

![Události](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Samozřejmě můžete upravit toto pravidlo výstrahy a **Zakázání** nebo **Povolení** ho Pokud chcete dočasně oznámení přestala chodit k němu.

## <a name="next-steps"></a>Další kroky

* [Konfigurace webhooks na upozornění](insights-webhooks-alerts.md) na směrování oznámení různých kanálů
* [Sledování služby metriky](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
* [Povolit sledování a diagnostice](insights-how-to-use-diagnostics.md) shromažďovat podrobné metriky vysokou četnost na služby.
* [Dostupnost monitor a rychlostí reakce webové stránky](../application-insights/app-insights-monitor-web-app-availability.md) s přehledy aplikace, kterou můžete zjistit, zda stránku je vypnutý.
* [Sledování aplikací výkonu](../application-insights/app-insights-azure-web-apps.md) Pokud chcete porozumět přesně jak funguje kódu v cloudu.
* [Zobrazit události a protokolů auditování](insights-debugging-with-events.md) další všechno uskutečněných v služby.
* [Sledování stavu služeb](insights-service-health.md) zjistit, kdy došlo Azure výkonu snížení úrovně nebo službu vyrušování.
