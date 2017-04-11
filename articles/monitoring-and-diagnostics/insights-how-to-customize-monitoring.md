<properties
    pageTitle="Základní informace o metriky v Microsoft Azure | Microsoft Azure"
    description="Zjistěte, jak přizpůsobit sledování grafy v Azure."
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

# <a name="overview-of-metrics-in-microsoft-azure"></a>Základní informace o metriky v Microsoft Azure

Všechny služby Azure sledování klíčových metrik, pomocí kterých budete ke sledování stavu, výkonu, dostupnost a použití te000126961 svých služeb. Můžete zobrazit tyto metriky Azure portálu a taky můžete [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) pro přístup k úplné sady metriky programově.

U některých služeb budete muset zapnout diagnostiky Chcete-li zobrazit všechny metriky. Pro ostatní, například virtuálních počítačích získáte základní množiny metriky, ale potřeba povolit úplné nastavení velmi častých metriky. V tématu [Povolení sledování a diagnostice](insights-how-to-use-diagnostics.md) Další informace.

## <a name="using-monitoring-charts"></a>Pomocí sledování grafů

Můžete vytvořit graf jednotlivých metriky je vyberete všechny časové období.

1. Na [Portálu Azure](https://portal.azure.com/)klikněte na **Procházet**a pak zdroje máte zájem sledování.

2. V části **Sledování** obsahuje nejdůležitější metriky pro jednotlivé Azure zdroje. Například do webových aplikací má **požadavky a chyby**, kde jsou virtuálního počítače by měla **Procento využití procesoru** a **disku čtení a zápis**:  ![lens sledování](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Po kliknutí na libovolný grafu se zobrazí zásuvné **míru** . Na zásuvné kromě grafu je tabulka zobrazující agregace metriky (například průměr, minimální a maximální přes časový rozsah, který jste se rozhodli). V následující tabulce, která jsou upozornění pravidla pro daný zdroj.
    ![Metriky zásuvné](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Přizpůsobení řádky, které se zobrazí, klikněte na tlačítko **Upravit** v diagramu nebo na příkaz **Upravit graf** v metrických zásuvné.

5. Tři věci budete dělat na zásuvné upravit dotaz:
    - Změna časového rozsahu
    - Přepínání mezi vzhled pruhový graf a řádek
    - Zvolte jiné metics ![upravit dotaz](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Změna časového rozsahu je stejně snadná jako výběrem jiné oblasti (třeba **Poslední hodinu**) a kliknutím na **Uložit** v dolní části zásuvné. Můžete taky zvolit **vlastní**, které si můžete zvolit doba za poslední dva týdny. Například zobrazí se na celé dva týdny nebo jenom 1 hodinu Včerejší. Zadejte do textového pole a zadejte jiné hodinu.
    ![Vlastní časového rozsahu](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Pod časový rozsah vyberte kanál je libovolný počet metriky zobrazíte v diagramu.

8. Když kliknete na Uložit změny uloží k danému zdroji. Například pokud máte dvě virtuálních počítačích a změna grafu na jednom, nebude ovlivňovat druhé.

## <a name="creating-side-by-side-charts"></a>Vytváření grafů vedle sebe

Pomocí výkonných úprav na portálu můžete přidat tolik grafů, jak chcete.

1. V nabídce **...** v horní části zásuvné klikněte na **Add dlaždice**:  
    ![Přidání nabídky](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Pak můžete vybrat graf můžete vybrat z **Galerie** na pravé straně obrazovky:  ![Galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Pokud nevidíte míru, které chcete, můžete vždy přidat jeden z přednastavených metriky a **Úprava** grafu zobrazte míru, které potřebujete.

## <a name="monitoring-usage-quotas"></a>Sledování kvót použití

Většina metriky vám ukazují trendů určitou dobu, ale určité údaje, jako je využití kvóty jsou informace v okamžiku s prahovou hodnotu.

Najdete v článku použití kvóty na zásuvné zdrojů, jejichž kvóty:

![Použití](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Jako metriky, můžete pomocí [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) nebo [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) pro přístup k úplné sady využití kvóty programově.

## <a name="next-steps"></a>Další kroky

* [Přijímání oznámení](insights-receive-alert-notifications.md) kdykoli metriky protíná prahovou hodnotu.
* [Povolit sledování a diagnostice](insights-how-to-use-diagnostics.md) shromažďovat podrobné metriky vysokou četnost na služby.
* Abyste měli jistotu, že vaše služba je k dispozici a citlivé [Zobrazit počet instancí automaticky](insights-how-to-scale.md) .
* [Sledování aplikací výkonu](../application-insights/app-insights-azure-web-apps.md) Pokud chcete porozumět přesně jak funguje kódu v cloudu.
* Pomocí [Aplikace přehledy JavaScript aplikace a webových stránek](../application-insights/app-insights-web-track-usage.md) analýzy klienta o prohlížečích najdete na webovou stránku.
* [Dostupnost monitor a rychlostí reakce webové stránky](../application-insights/app-insights-monitor-web-app-availability.md) s přehledy aplikace, kterou můžete zjistit, zda stránku je vypnutý.
